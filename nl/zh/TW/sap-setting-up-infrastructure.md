---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, {{site.data.keyword.cloud_notm}}, deployment, BYOL

subcollection: sap-netweaver

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# 2. 設定基礎架構
{: #set_up_infrastructure}

在下一節，有關於設定 SAP NetWeaver {{site.data.keyword.baremetal_long}}、網路、安全、儲存空間及作業系統 (OS) 的指引。如有任何問題，請聯絡 [{{site.data.keyword.cloud_notm}} 支援中心](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)。

## 訂購伺服器
{: order-server}

請使用下列步驟來訂購 {{site.data.keyword.baremetal_short}}。您可以在[建置自訂裸機伺服器](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server)中找到其他資訊。

1. 使用您的唯一認證登入 [{{site.data.keyword.cloud_notm}} 基礎架構客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com){: new_window}。
2. 按一下「帳戶摘要」頁面上的**帳戶** > **下訂單**。
3. 在「裝置」頁面上，按一下 {{site.data.keyword.baremetal_short}} 下的**每月**鏈結。會出現「伺服器清單」對話框；SAP 認證伺服器在清單的頂端。
4. 按一下**每個月的起始價格**下的超鏈結，以選取適當的伺服器並進入「配置/訂購」頁面。

BI.S3.NW32（OS 選項）伺服器也適用於**按小時**計費。
{: note}

   在「CPU 模型」下，會以 **-NW** 識別 SAP NetWeaver 認證的伺服器。Red Hat 型伺服器配置會在[選取伺服器選項](#select_options)下的步驟 7 和步驟 1 加以說明；其步驟與 Microsoft Windows 的步驟相同。請注意，對於 VMWare，您的選擇有限，但步驟相同。

   以下是如何解密 SAP NetWeaver 伺服器名稱的範例。

|伺服器名稱|命名慣例元件|代表意義|
| --- | --- | --- |
| BI.S3.NW768 | BI | Bluemix 介面|
| | S3 | 系列 2（處理器世代）|
| | | S1 是 Ivy Bridge/Haswell |
| | | S2 是 Broadwell |
| | | S3 是 Skylake/Kaby Lake |
| | NW | NetWeaver 認證伺服器|
| | 768 |RAM 數量|

5. 在**品質**欄位中輸入您要訂購的伺服器數目，然後選取**資料中心**。
6. **伺服器**、**RAM** 及專用儲存空間選項的預設值是根據您的伺服器選擇，無法加以變更。在您訂購伺服器之後，會訂購 {{site.data.keyword.IBM_notm}} {{site.data.keyword.blockstorageshort}} for {{site.data.keyword.cloud_notm}} 或 {{site.data.keyword.filestorage_full_notm}}。
7. 從 Microsoft、Red Hat 或 SUSE 中選取**作業系統**，並選取特定的作業系統，或為您的伺服器選取 VMware Hypervisor。

如果您自帶作業系統的授權 (BYOL)，請選取**其他** > **無作業系統**。如需相關資訊，請參閱[自帶授權](#byol)。
{: note}

## 選取伺服器選項
{: #select_options}

下一步，您將選取要新增至配置的磁碟類型和數目。您也可以為「獨立磁碟的備用陣列 (RAID)」儲存空間群組以及 RAID 儲存空間群組上的分割佈置選取不同的選項。如需相關資訊，請參閱[關於 RAID](/docs/bare-metal?topic=bare-metal-about-raid#about-raid) 或 [{{site.data.keyword.cloud_notm}} 支援中心](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)。

1. 在 **SAP 認證伺服器**下，根據如何規劃伺服器的使用進行選取。如需相關資訊，請參閱 [Design Decision Tool ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-cloud-architecture/infrastructure-design-decision-tool){: new_window}（向下捲動至工具）或 [{{site.data.keyword.cloud_notm}} 支援中心](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)。
2. 按一下頁面底端的**新增至訂單**按鈕。

## 設定進階系統配置
{: #adv_config}

1. 遵循[進階系統配置](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server)準則，以協助輸入**進階系統配置**視窗中的值。

SAP 主機名稱最多由 13 個英數字元組成。如需 SAP 主機名稱詳細資料，請參閱 [SAP 附註 611361 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://launchpad.support.sap.com/#/611361){: new_window} 及 [129997 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://launchpad.support.sap.com/#/129997){: new_window}。

如果您已決定您環境的公用/專用設定，並且打算
  * 安裝多個需要彼此通訊的 SAP 系統，*或*
  * 選擇三層 SAP 設定（不同硬體上的資料庫及應用程式實例）

請確定您的名稱解析反映了內部和外部位址。外部位址符合由伺服器完整網域名稱 (FQDN) 所解析的伺服器主機名稱。

內部位址不會出現在網域名稱系統 (DNS) 中。因為內部 IP 應該用於伺服器之間的通訊，所以請確保您有相應地延伸 `/etc/hosts` 或 Microsoft Windows "host" 檔案。此資訊也可能適用於 VMware ESXi 型部署的來賓作業系統。

## 確認選擇
{: #confirm_selections}

1. 在「結帳」頁面上確認您的選取項目，然後按一下**雲端服務條款**及**協力廠商軟體合約**勾選框。
2. 向下捲動至「建立帳戶：輸入計費」，然後輸入**付款類型、名稱、卡片**及**到期**（日期），以使用信用卡付費。
3. 按一下**送出訂單**按鈕。會將您重新導向有您的訂單號碼的畫面。您可以列印此畫面，因為它也是您的訂單收據。

主旨為_已核准 {{site.data.keyword.cloud_notm}} 訂單 ##_ 的確認電子郵件會傳送至您設定檔中的電子郵件位址。此電子郵件通知您的伺服器已核准且正在進行部署。部署之後，會傳送另一則通知，通知您伺服器已經可用，並且可透過 [{{site.data.keyword.cloud_notm}} 基礎架構客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com){: new_window} 管理。

## 自帶授權
{: #byol}

當您有自己的作業系統授權時，請根據供應商的指示將它安裝在 {{site.data.keyword.baremetal_short}} 上。如需相關資訊，請參閱[無 OS 選項](/docs/bare-metal?topic=bare-metal-how-to-install-an-operating-system-on-a-no-os-server-#how-to-install-an-operating-system-on-a-no-os-server-)。

## 後續步驟

現在，您已準備好開始管理您的 {{site.data.keyword.baremetal_short}}。關於後續步驟，請參閱[管理 SAP NetWeaver 環境](/docs/infrastructure/sap-netweaver?topic=sap-netweaver-manage_environment#manage_environment)。
