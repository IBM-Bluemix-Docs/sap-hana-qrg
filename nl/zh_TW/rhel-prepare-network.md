---



copyright:
  years: 2017, 2018
lastupdated: "2018-02-21"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 4. 為三層設定準備網路
{: #network}

如果您規劃要安裝三層設定，則需要正確設定網路。在此範例中，已部署 256 GB 資料庫伺服器（名稱為 `e2e2690`）及 32 GB 應用程式伺服器（名稱為 `e2e1270`）。資料庫伺服器也管理 (A)SCS 實例。將專用網路上的 IP 位址新增至 `/etc/hosts` 有助於接下來的步驟，並確保 SAP 內部網路資料流量通過正確網路。

![圖 1。三層設定的範例](/images/network-01.png "三層設定的範例")

使用下列步驟來建立網路。

1. 登入伺服器，並尋找其專用網路配置。

        [root@e2e2690 ~]# ifconfig bond0
            bond0	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A8
                    inet addr:10.17.139.35  Bcast:10.17.139.63 Mask:255.255.255.192
                    inet6 addr: fe80::ec4:7aff:fe66:2da8/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:128080 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:25491 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:19137850 (18.2 MiB)  TX bytes:3426015 (3.2 MiB)

        [root@e2e2690 ~]# ifconfig bond1
            bond1	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A9
                    inet addr:208.43.211.118  Bcast:208.43.211.127 Mask:255.255.255.224
                    inet6 addr: fe80::ec4:7aff:fe66:2da9/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:289610 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:109371 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:31216160 (29.7 MiB)  TX bytes:18591947 (17.7 MiB)

在三層範例中，10.17.139.35 是資料庫伺服器的專用 IP 位址；它來自 RFC 1597 的其中一個 IP 位址範圍。您也可以決定應用程式伺服器的 IP 位址，並將兩個 IP 位址新增至兩個伺服器的 `/etc/hosts`。

        [root@e2e2690 ~]# cat /etc/hosts
        127.0.0.1 localhost.localdomain localhost
        208.43.211.118 e2e2690.saptest.com e2e2690
        
        10.17.139.35    e2e2690-priv
        10.17.139.46    e2e1270-priv

同時在 `e2e1270` 新增最後兩行。

## 安裝 NFS 軟體

1. **在兩個伺服器上**安裝 NFS 軟體 `nfs-utils`。

      `[root@e2e2690a ~]# yum install nfs-utils`

請確定您在資料庫伺服器上啟動及登錄 `rpcbind` 及 NFS 服務。

        [root@e2e2690 ~]# service rpcbind start
        [root@e2e2690 ~]# chkconfig nfs on
        [root@e2e2690 ~]# service nfs start

## 使用 NFS 來匯出

1. 使用 NFS 將必要項目新增至資料庫伺服器的 `/etc/exports`，藉此將 `/sapmnt` 及 `/usr/sap/trans` 從資料庫伺服器匯出至應用程式伺服器。

        /sapmnt/C10		10.17.139.46(rw,no_root_squash,sync,no_subtree_check)
        /usr/sap/trans	10.17.139.46(rw,no_root_squash,sync,no_subtree_check)

範例值 `C10` 需要取代為 SAP 系統的 SAP 系統 ID。您必須在匯出目錄之前先建立目錄。從資料庫伺服器的指令行執行下列指令：

        [root@e2e2690 ~]# mkdir /sapmnt/C10
        [root@e2e2690 ~]# mkdir -p /usr/sap/trans
        [root@e2e2690 ~]# exportfs -a

## 裝載 NFS 共用

1. 將下列項目新增至 NFS 共用的 `/etc/fstab`，並從指令行裝載它，藉此在應用程式伺服器上裝載 NFS 共用。

        e2e2690-priv:/sapmnt/C10 /sapmnt/C10 nfs defaults 0 0
        e2e2690-priv:/usr/sap/trans /usr/sap/trans nfs defaults 0 0

2. 在應用程式伺服器上建立目標目錄，並裝載它們。

        [root@e2e1270 ~]# mkdir -p /sapmnt/C10
        [root@e2e1270 ~]# mkdir /usr/sap/trans
        [root@e2e1270 ~]# mount /sapmnt/C10
        [root@e2e1270 ~]# mount /usr/sap/trans

您的伺服器現在已備妥，可管理分散式 SAP 安裝的元件。 

## 後續步驟

  * [將外部儲存空間新增至 {{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [安裝 SAP 應用程式及軟體](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
