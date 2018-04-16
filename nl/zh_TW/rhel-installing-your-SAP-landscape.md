---



copyright:
  years: 2017, 2018
lastupdated: "2018-02-22"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 安裝 SAP 環境
{: #install_landscape}

## 安裝 RPM 套件（必備項目）
{: #RPM}

對於安裝在 OS 及執行的 OS 常駐程式上的套件，SAP 的安裝需要特定必備項目。請參閱 SAP 的最新[安裝手冊](https://support.sap.com/software/installations.html)（需要 [SAP S-user ID](/docs/infrastructure/sap-netweaver/sap-index.html#getting-started)）及[支援注意事項](https://support.sap.com/notes)（需要SAP S-user ID），以取得這些必備項目的最新清單。還有兩個套件需要安裝：
* compat-sap-c++：達到 C++ 運行環境與 SAP 使用的編譯器的相容性。
* uuidd：維護用於建立 UUID 的 OS 支援。

### 檢查是否已安裝 uuidd

1. 檢查是否已安裝 `uuid daemon (uuidd)`。如果沒有，請安裝並啟動它。
```
[root@e2e2690 ~]# rpm -qa | grep uuidd
[root@e2e2690 ~]# yum install uuidd
[root@e2e2690 ~]# chkconfig uuidd on
[root@e2e2690 ~]# service uuidd start
```

### 安裝套件 compat-sap-c++

1. 遵循 [SAP Note 2195019](https://launchpad.support.sap.com/#/notes/2195019)，並安裝套件 compat-sap-c++，以及建立 SAP 二進位檔所需的特定軟鏈結。
```
[root@e2e2690 ~]# yum install compat-sap-c++
....
[root@e2e2690 ~]# mkdir -p /usr/sap/lib
[root@e2e2690 ~]# ln -s /opt/rh/SAP/lib64/compat-sap-c++.so /usr/sap/lib/libstdc++.so.6
```

## 下載 SAP 軟體
{: download_software}

登入 [SAP Service Marketplace](https://websmp201.sap-ag.de/)，將必要的 DVD 下載至本端共用磁碟機。將檔案傳送至您佈建的伺服器。另一個選項是下載 [SAP Software Download Manager](https://support.sap.com/en/my-support/software-downloads.html#section_995042677)，將它安裝在目標伺服器上，然後直接將 DVD 映像檔下載至該伺服器。 

## 為 SAP 的 SWPM GUI 做準備
{: #prepare_for_GUI}

視您的網路頻寬和延遲而定，您也許想在虛擬網路運算 (VNC) 階段作業中遠端地執行 SAP Software Provisioning Manager (SWPM) 圖形使用者介面。另一個選項是在本端執行 GUI，然後在目標機器上連接至 SWPM。如果您決定在本端執行 GUI，請參閱 [SWPM 文件](https://wiki.scn.sap.com/wiki/display/SL/Software+Provisioning+Manager+1.0)。 

下列步驟概述在 VNC 階段作業中遠端執行 SWPM GUI。此選項安裝的 VNC 伺服器未必與強化您的作業系統一致；請確定您符合安全措施。如果您對其功能不太熟悉，請參閱 [VNC 文件](http://searchnetworking.techtarget.com/definition/virtual-network-computing)，以取得其功能的概觀。

1. 使用下列指令安裝 VNC 伺服器。
```
  [root@e2e2690 ~]# yum install tigervnc-server

  ...
```

2. 使用下列指令安裝 X11 視窗管理程式 `twm`，它包含在 Linux 發行套件中。

`[root@e2e2690 ~]# yum install twm`

3. 安裝終端機模擬器，例如 `xterm`。
 
 `[root@e2e2690 ~]# yum install xterm`

4. 從指令行啟動 VNC 伺服器。
 
 `[root@e2e2690 ~]# vncserver`

現在您需要 VNC 用戶端程式；有多個實作可免費使用於所有作業系統。一般而言，您需要能夠從用戶端存取埠 `590X`（其中 X 是執行的伺服器數目，從 1 開始）。

您可能必須從 `twm` 的背景功能表啟動 `xterm`。您可以從 `xterm` 啟動 `SWPM` (`sapinst`)。

## 安裝 SAP 軟體
{: #install_software}

下載安裝媒體之後，請遵循標準 SAP 安裝程序，它記載在您的 SAP 版本和元件的 SAP 安裝手冊中，以及對應的 SAP Notes 中。

您可以從 `xterm` 啟動 SAP SWPM，並執行安裝步驟。 

### 在三層環境中安裝 SAP 軟體。

遵循 SAP 的 SWPM 中對於三層設定的步驟。 

1. 選取**分散式系統**，並在資料庫伺服器上執行 ASCS 安裝及資料庫安裝。 
2. 在應用程式伺服器上安裝 Application Server ABAP。在應用程式伺服器的安裝期間，務必使用 ASCS 的專用位址和資料庫主機名稱。 

使用專用位址及主機名稱可確保應用程式伺服器與 ASCS 或資料庫之間的網路資料流量是通過專用網路，而不是通過公用網路。
