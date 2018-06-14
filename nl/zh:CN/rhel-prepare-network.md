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

# 4. 为三层设置准备网络
{: #network}

如果您计划安装三层设置，需要正确设置网络。在示例中，已部署 256 GB 数据库服务器（名为 `e2e2690`）和 32 GB 应用程序服务器（名为 `e2e1270`）。数据库服务器还托管了 (A)SCS 实例。将专用网络上的 IP 地址添加到 `/etc/hosts` 有助于执行后续步骤，并可确保 SAP 内部网络流量流经正确的网络。

![图 1. 三层设置的样本](/images/network-01.png "三层设置的样本")

使用以下步骤来建立网络。

1. 登录到这些服务器并查找其专用网络配置。

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

在三层示例中，10.17.139.35 是数据库服务器的 IP 地址；该地址取自 RFC 1597 的某个 IP 地址范围。您也可以确定应用程序服务器的 IP 地址，并将这两个 IP 地址添加到这两个服务器的 `/etc/hosts`。

        [root@e2e2690 ~]# cat /etc/hosts
        127.0.0.1 localhost.localdomain localhost
        208.43.211.118 e2e2690.saptest.com e2e2690
        
        10.17.139.35    e2e2690-priv
        10.17.139.46    e2e1270-priv

在 `e2e1270` 上也添加最后两行。

## 安装 NFS 软件

1. **在两个服务器上**安装 NFS 软件 `nfs-utils`。

      `[root@e2e2690a ~]# yum install nfs-utils`

确保在数据库服务器上启动并注册 `rpcbind` 和 NFS 服务。

        [root@e2e2690 ~]# service rpcbind start
        [root@e2e2690 ~]# chkconfig nfs on
        [root@e2e2690 ~]# service nfs start

## 使用 NFS 来导出

1. 使用 NFS 将 `/sapmnt` 和 `/usr/sap/trans` 从数据库服务器导出到应用程序服务器，方法是将所需的条目添加到数据库服务器的 `/etc/exports`。

        /sapmnt/C10		10.17.139.46(rw,no_root_squash,sync,no_subtree_check)
        /usr/sap/trans	10.17.139.46(rw,no_root_squash,sync,no_subtree_check)

样本值 `C10` 需要替换为 SAP 系统的 SAP 系统标识。必须先创建目录，然后再进行导出。从数据库服务器的命令行运行以下命令：

        [root@e2e2690 ~]# mkdir /sapmnt/C10
        [root@e2e2690 ~]# mkdir -p /usr/sap/trans
        [root@e2e2690 ~]# exportfs -a

## 安装 NFS 共享

1. 在应用程序服务器上安装 NFS 共享，方法是将以下条目添加到其 `/etc/fstab` 并从命令行进行安装。

        e2e2690-priv:/sapmnt/C10 /sapmnt/C10 nfs defaults 0 0
        e2e2690-priv:/usr/sap/trans /usr/sap/trans nfs defaults 0 0

2. 在应用程序服务器上创建目标目录并安装这些目录。

        [root@e2e1270 ~]# mkdir -p /sapmnt/C10
        [root@e2e1270 ~]# mkdir /usr/sap/trans
        [root@e2e1270 ~]# mount /sapmnt/C10
        [root@e2e1270 ~]# mount /usr/sap/trans

您的服务器现在已准备好托管分布式 SAP 安装的组件。 

## 后续步骤

  * [向 {{site.data.keyword.baremetal_short}} 添加外部存储器](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [安装 SAP 应用程序和软件](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
