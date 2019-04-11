---

copyright:
years: 2017, 2019
lastupdated: "2019-04-10"

keywords: SAP NetWeaver, application server, database server, three-tier

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. 分割及檔案系統
{: #3-partitioning-and-file-systems}

以三層範例而言，其訂購具有一個邏輯磁碟的 192 GB 伺服器（資料庫伺服器），以及具有一個邏輯磁碟（在 RAID 1 上）的 32 GB 伺服器（資料庫伺服器）。這兩個伺服器皆配備一個等於總磁碟大小的大型 root 檔案系統（其中有些空間用於 `/boot`）。

若為 32 GB 伺服器，請建立 `/usr/sap/` 檔案系統。請注意，`sapmnt1` 和 `/usr/sap/trans` 會建立在資料庫伺服器。網路檔案系統 (NFS) 會從資料庫伺服器匯出，資料庫伺服器也管理了 Advanced Business Application Programming (ABAP) SAP Central Service [(A)SCS] 實例。

下列檔案系統佈置是一個可能的方式。在正式作業用途上，您可能想要追蹤您系統的調整大小資訊，因為其他佈置可能更能夠滿足您的需求或 SAP 需求，或您可能想要使用配額。


需要使用下列指令來建立安裝 SAP 軟體的必要目錄 `/sapmnt`、`/usr/sap` 及 `/db2`：
```
[root@ sdb192 ~]# mkdir /sapmnt
[root@ sdb192 ~]# mkdir /usr/sap
[root@ sdb192 ~]# mkdir /db2
```

## 後續步驟

[4. 準備網路](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-network#network)
