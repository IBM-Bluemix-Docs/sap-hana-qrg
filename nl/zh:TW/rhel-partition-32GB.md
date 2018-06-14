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

# 3. 分割及檔案系統
{: #partition_32GB}

以單一節點範例而言，您已訂購一個具有一個邏輯磁碟（在 RAID1 上）的伺服器。有一個鏡映磁碟出現在作業系統 (OS) 中 - 具有一個等於總磁碟大小的大型 root 檔案系統（其中有些空間用於 `/boot`）。下列檔案系統佈置只是一個可能的方式。在正式作業用途上，您可能想要追蹤您系統的調整大小資訊，因為其他佈置可能更能夠符合您的需求或 SAP 需求。 

已建立 SAP 安裝所需要的 `/usr/sap`、`/sapmnt` 及 `/db2` 這三個目錄：
```
[root@e2e1270 ~]# mkdir /sapmnt
[root@e2e1270 ~]# mkdir /usr/sap
[root@e2e1270 ~]# mkdir /db2
```
您的 {{site.data.keyword.baremetal_long}} 現在可以用於外部儲存空間及 SAP 應用程式和軟體的安裝。

## 後續步驟

  * [將外部儲存空間新增至 {{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [安裝 SAP 應用程式及軟體](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
