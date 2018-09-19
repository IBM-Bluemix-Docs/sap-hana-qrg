---



copyright:
  years: 2017, 2018
lastupdated: "2018-08-14"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. 訂購用於三層設定的 192 GB 及 32 GB 伺服器
{: #install_three_tier}

## 訂購伺服器
{: #order_servers}

遵循[訂購 32 GB 伺服器](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-set-up-infrastructure-32GB.html#order_32GB)中的步驟來訂購 SAP NetWeaver 應用程式伺服器。下列步驟引導您完成資料庫伺服器的訂購。

## 訂購資料庫伺服器
{: #order-db-server}

1. 使用您的唯一認證登入 [{{site.data.keyword.cloud_notm}} 基礎架構客戶入口網站](https://control.softlayer.com)。
2. 按一下「帳戶摘要」頁面上的**帳戶** > **下訂單**。
3. 按一下「裝置」頁面上 **{{site.data.keyword.baremetal_long}}** 下的**每月**。會出現伺服器清單；SAP 認證伺服器在清單的頂端。
4. 按一下 **STARTING PRICE PER MONTH** 下的超鏈結，以選取伺服器 **BI.S3.NW192（OS 選項）。**

## 配置資料庫伺服器
{: #options_192GB}

1. 保留**數量**欄位中的 **1**。
2. 對於**資料中心**選取 **TOR01**。
3. **伺服器**預設為根據您選擇的伺服器的預定值，且無法變更。
4. 按一下 **192 GB RAM**，即使 **RAM** 選擇是預設為根據您選擇的伺服器的預定值，且無法變更。
5. 按一下 **Red Hat Enterprise Linux for SAP Business Application 7.X（64 位元）**作為您的**作業系統**。**附註**：如果您自帶作業系統的授權 (BYOL)，請選取**其他** > **無作業系統**。如需相關資訊，請參閱[自帶授權](#byol)。
6. 新增第二個 2 TB 的 SATA 磁碟機，方法是按一下**磁碟控制器 1** 下拉功能表，然後選取 **2 TB SATA**。按一下**新增磁碟**。
7. 按一下**選取所有磁碟**，然後按一下**建立 RAID 儲存空間群組**。
8. 按一下**類型**，然後選取 **RAID 1**。輸入涵蓋您需要之儲存空間總量的**大小**。
9. 維持不勾選 **LVM**，然後接受預設的**分割區範本**－**Linux Basic**。
10. 按一下**完成**。

## 選取其他資料庫伺服器選項
{: #addl-server-options}

1. 對於**公用頻寬**選取 **500 GB**。
2. 對於**上行鏈路埠速度**選取 **1 Gbps 備援公用及專用網路上行鏈路**。
3. 以此範例而言，請保留所有其他欄位的預設值。您可以參考[建置自訂裸機伺服器](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#addl-server-options)，以取得選項的詳細說明。
4.	按一下頁面底端的**新增至訂單**。當您的訂單通過驗證之後，會將您重新導向「結帳」頁面。

## 設定進階系統配置
{: #adv_config}

1. 對於「進階系統配置」下的欄位，請使用「表格 1」中的值。[進階系統配置](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#adv-system-config)準則有提供相關資訊。

|欄位                |值                                                              |
| -------------------------------- | -------------------------------------------------------------------- |
|後端 VLAN                         |從下拉清單中選取，例如，`tor01.bcr01a.1241`                          |
|子網路                            |從下拉清單中選取，例如，`10.114.63.64/26`                            |
|前端 VLAN                         |從下拉清單中選取，例如，`tor01.fcr01a.851`                          |
|子網路                            |從下拉清單中選取，例如，`158.85.65.224/28`                            |
|佈建 Script                       |空白                                                                 |
|SSH 金鑰                          |預設為 `Add`，意指沒有任何 SSH 金鑰                            |
|主機名稱                          |例如，`sdb192`                                                      |
|網域                              |例如，`saptest.com`                                                  |
{: caption="表 1. 192 GB 進階配置值" caption-side="top"}  

## 確認選擇
{: #confirm_selections}

1. 確認「結帳」頁面上的選擇，並按一下頁面右邊的**雲端服務條款**及**協力軟體合約**。
2. 按一下**送出訂單**。會將您重新導向有您的訂單號碼的畫面。您可以列印此畫面，因為它也是您的訂單收據。

送出訂單之後，視您的訂單而定，該伺服器將在一到四小時內可供使用。您可以檢查主要 {{site.data.keyword.slportal}} 頁面上的「裝置詳細資料」畫面（**裝置 > 裝置清單**），以查看佈建步驟的狀態。按一下符合您給定的主機名稱和網域的**裝置名稱**，以查看其狀態。

## 自帶授權
{: #byol}

當您有自己的作業系統授權時，請根據供應商的指示將它安裝在 {{site.data.keyword.baremetal_short}} 上。如需相關資訊，請參閱[無 OS 選項](https://console.bluemix.net/docs/bare-metal/introduction-no-os.html#how-to-install-an-operating-system-on-a-no-os-server-)。

## 後續步驟

  [2. 為 SAP 安裝準備伺服器](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-256GB.html)

  [3. 分割及檔案系統](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-256GB.html)

  [4. 準備網路](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
