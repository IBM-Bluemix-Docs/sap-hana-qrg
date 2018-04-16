---



copyright:
  years: 2018
lastupdated: "2018-02-26"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. 訂購 32 GB 伺服器
{: #install_32GB}

## 訂購伺服器
{: #order_32GB}

1. 使用您的唯一認證登入 [{{site.data.keyword.cloud_notm}} 基礎架構客戶入口網站](https://control.softlayer.com)。
2. 按一下「帳戶摘要」頁面上的**裝置**。
3. 按一下「裝置」頁面上 {{site.data.keyword.baremetal_short}} 下的**每月**。會出現「伺服器清單」對話框。
4. 按一下 **STARTING PRICE PER MONTH** 下的超鏈結，以選取伺服器 **BI.S1.NW32（OS 選項）。**

## 選取伺服器選項
{: #options_32GB}

1. 保留**數量**欄位中的 **1**。
2. 對於**資料中心**選取 **TOR01**。資料中心清單視特定資料中心內的產品可用性而定。
3. **伺服器**預設為根據您選擇的伺服器的預定值，且無法變更。
4. 按一下 **32 GB RAM**，即使 **RAM** 選擇是預設為根據您選擇的伺服器的預定值，且無法變更。
5. 選取 **Red Hat Enterprise Linux for SAP Business Application 6.X** 作為您的**作業系統**。
6. 在**硬碟**下選取第二個 2 TB SATA 磁碟，從涵蓋總儲存空間的兩個磁碟中建立 RAID1 的 RAID 儲存空間群組，並選擇 **Linux Basic** 作為**分割區範本**。不勾選 **LVM**。
7. 對於**公用頻寬**選取 **500 GB**。
8.	對於**上行鏈路埠速度**選取 **1 Gbps 備援公用及專用網路上行鏈路**。
9. 請保留所有其他欄位的預設值。如需詳細選項說明，請參閱[設定裸機伺服器](https://console.bluemix.net/docs/bare-metal/configuring.html#setting-up-your-bare-metal-servers)。
10.	按一下頁面底端的**新增至訂單**。當您的訂單通過驗證之後，會將您重新導向「結帳」頁面。

## 設定進階系統配置
{: #adv_config}

對於「進階系統配置」下的欄位，請使用「表格 1」中的值。[進階系統配置](https://console.bluemix.net/docs/bare-metal/configuring.html#advanced-system-configuration)準則有提供相關資訊。

1. 向下捲動，並在**進階系統配置**下輸入「表格 1」中的值。

|              欄位                |      值                                                              |
| -------------------------------- | -------------------------------------------------------------------- |
|後端 VLAN                         | 從下拉清單中選取，例如，`tor01.bcr01a.1241`                          |
|子網路                            | 從下拉清單中選取，例如，`10.114.63.64/26`                            |
|前端 VLAN                         | 從下拉清單中選取，例如，`tor01.fcr01a.1199`                          |
|子網路                            | 從下拉清單中選取，例如，`169.55.137.160/27`                          |
|佈建 Script                       | 預設為 None                                                          |
|SSH 金鑰                          | 新增                                                                 |
|主機名稱                          | 例如，`e2e1270`                                                      |
|網域                              | 例如，`saptest.com`                                                  |
{: caption="表格 1。32GB 進階配置值" caption-side="top"}  

## 確認選擇
{: #confirm_selections}

1. 確認「結帳」頁面上的選擇，並按一下頁面右邊的**雲端服務條款**及**協力軟體合約**。
2. 按一下表單右邊的**送出訂單**。會將您重新導向有您的訂單號碼的頁面。您可以列印此頁面，因為它也是您的訂單收據。此外，您會收到一封確認電子郵件，其主旨為*已核准您的 IBM Cloud 訂單 ##*，其中 ## 是您的訂單號碼。

送出訂單之後，視您的訂單而定，您的伺服器將在一到四小時內可供使用。您可以檢查主要「客戶入口網站」頁面上的「裝置詳細資料」畫面（**裝置 > 裝置清單**），以查看佈建步驟的狀態。按一下符合您給定的主機名稱和網域的**裝置名稱**，以查看其狀態。

## 後續步驟
 
  [2. 為 SAP 安裝準備伺服器](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-32GB.html)
  
  [3. 分割及檔案系統](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-32GB.html)
