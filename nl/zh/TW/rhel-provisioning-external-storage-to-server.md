---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, database server, deployment

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 將外部儲存空間新增至伺服器
{: #storage}

## 設定外部儲存空間
{: #set_up_storage}

如果您想要使用外部儲存空間作為備份裝置，或想要在測試環境中使用 Snapshot 快速還原您的資料庫，您可以將外部儲存空間新增至您佈建的伺服器。以三層範例而言，區塊儲存空間可同時用來保存資料庫的日誌檔，以及資料庫的線上和離線備份。已選取最快速區塊儲存空間（每 GB 4 IOPS）來幫助確保最短備份時間。慢速區塊儲存空間可能足夠滿足您的需求。如需 {{site.data.keyword.blockstoragefull}} 的相關資訊，請參閱[開始使用區塊儲存空間](/docs/infrastructure/BlockStorage?topic=BlockStorage-GettingStarted#getting-started-with-block-storage)。


1. 使用您的唯一認證登入 [{{site.data.keyword.cloud_notm}} 基礎架構客戶入口網站 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){: new_window}。
2. 選取**儲存空間** > **Block Storage**。
3. 按一下「區塊儲存空間」頁面右上角的**訂購區塊儲存空間**。
4. 選取儲存空間需求的特性。「表格 1」包含建議值，包括用於一般資料庫工作負載的 4 IOPS/GB。

|欄位                |值                                                              |
| -------------------------------- | ------------------------------------------------- |
|位置                              |TOR01                                             |
|計費方法                          |每月（預設值）                                    |
|新的儲存空間大小                  |1000 GB                                           |
|儲存空間 IOPS 選項              | 耐久性（分層 IOPS）（預設值）|
|耐久性分層 IOPS             | 10 GB                                             |
|Snapshot 空間大小|0 GB                                              |
|OS 類型                           |預設為 Linux|
{: caption="表格 1。區塊儲存空間的建議值" caption-side="top"}

5. 按一下兩個勾選框，然後按一下**下訂單**。

## 授權主機
{: authorize-host}

1. 按一下 LUN 右邊的**動作**，並選取**授權主機**以存取佈建的儲存空間。
2. 選取**裝置**；**裝置類型**預設為「裸機伺服器」。按一下**硬體**，並選取裝置的主機名稱。
3. 按一下**送出**按鈕。
4. 在**裝置** >（選取裝置）> **儲存空間**標籤下，檢查已佈建儲存空間的狀態。
5. 請記下**目標位址**及您伺服器（iSCSI 起始器）的 iSCSI 完整名稱 (**IQN**)，以及用於 iSCSI 伺服器之授權的**使用者名稱**和**密碼**。在下列步驟中您會需要此資訊。
6. 請遵循[連接至 Linux 上的 iSCSI LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#connecting-to-mpio-iscsi-luns-on-linux) 中的步驟，讓您的儲存空間能夠從您佈建的伺服器進行存取。

## 使儲存空間成為多路徑
{: #multipath}

在範例部署中，您從**儲存空間**標籤擷取下列資料：
  * 目標 IP：`10.2.62.78`
  * IQN：`iqn.2005-05.com.softlayer:SL01SU276540-H896345`
  * 使用者：`SL01SU276540-H896345`
  * 密碼：`EtJ79F4RA33dXm2q`

1. 根據所擷取的資訊輸入下列資訊：
```
[root@sdb192 ~]# cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.2005-05.come.softlayer:SL01SU276540-H986345
```
   可能必須在 `/etc/iscsi/initiatorname.iscsi` 中取代現有的項目。

2. 將下列幾行新增至 `/etc/iscsi/iscsid.conf` 底端：
```
[root@sdb192 ~]# tail /etc/iscsi/iscsid.conf
# it continue to respond to R2Ts. To enable this, uncomment this line
# node.session.iscsi.FastAbort = No
node.session.auth.authmethod = CHAP
node.session.auth.username = SL01SU276540-H896345
node.session.auth.password = EtJ79F4RA33dXm2q
discovery.sendtargets.auth.authmethod = CHAP
discovery.sendtargets.auth.username = SL01SU276540-H896345
discovery.sendtargets.auth.password = EtJ79F4RA33dXm2q
```

3. 將步驟 2 的 `username` 及 `password` 值取代為您在*授權主機* 的步驟 5 期間記下的那些資訊。

4. 輸入下列幾行以探索 iSCSI 目標。
```
[root@sdb192 ~]# iscsiadm -m discovery -t sendtargets -p "10.2.62.78"
10.2.62.78:3260,1031 iqn.1992-08.com.netapp:tor0101
10.2.62.87:3260,1032 iqn.1992-08.com.netapp:tor0101
```

5. 設定主機以自動登入 iSCSI 陣列。

      `[root@sdb192 ~]# iscsiadm -m node -L automatic`

6. 安裝及啟動多路徑常駐程式。
```
[root@sdb192 ~]# yum install device-mapper-multipath
…
[root@sdb192 ~]# chkconfig multipathd on
[root@sdb192 ~]# service multipathd start
```

7. 完成[在 Linux 上連接至 iSCSI LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux) 中的所有指令，使另一個 LUN 出現在多路徑輸出中。
```
[root@sdb192 ~]# multipath -ll
…
3600a098038303452543f464142755a42 dm-9 NETAPP,LUN C-Mode
size=500G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
|-+- policy='round-robin 0' prio=50 status=active
| `- 10:0:0:169 sde 8:64 active ready running
`-+- policy='round-robin 0' prio=10 status=enabled
`- 9:0:0:169  sdf 8:80 active ready running
…`
```

您現在可以像使用任何磁碟裝置一樣來使用多路徑裝置。裝置路徑出現在 `/dev/mapper/3600a098038303452543f464142755a42` 之下。

使用[範例 `multipath.conf`](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-sample) 中的範例 `/etc/multipath.conf`，並在伺服器上建立它。注意，任何複製的特殊字元、DOS-like 換行、換行項目可能導致非預期的行為。在複製內容之後，請確定您有 ASCII Unix 檔案。

調整 `/etc/multipath.conf` 的多路徑區塊，以建立路徑的別名，來存取 `1/dev/mapper/mpath1` 之下的裝置。

      multipaths {
	       multipath {
		         wwid 3600a098038303452543f464142755a42
		         alias mpath1
	       }
     }

8. 重新啟動 `multipathd`。您現在可以建立 `/backup filesystem`，並將它裝載於區塊裝置。

      [root@sdb192 ~]# service multipathd restart
      [root@sdb192 ~]# mkfs.ext4 /dev/mapper/mpath1
      [root@sdb192 ~]# mkdir  /backup

9. 檢查兩個伺服器上的檔案系統。您的輸出應類似下列輸出。

        [root@e2e1270 ~]# df -h
        Filesystem		    Size  Used Avail Use% Mounted on
        /dev/sda3             879G  1,5G  833G   1% /
        tmpfs                  16G     0   16G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/sdb2             849G  201M  805G   1% /usr/sap
        db192-priv:/usr/sap/trans
                      165G   59M  157G   1% /usr/sap/trans
                      db192-priv:/sapmnt/C10
                      165G   59M  157G   1% /sapmnt/C10

        [root@sdb192 ~]# df -h
        Filesystem      	    Size  Used Avail Use% Mounted on
        /dev/sda3             549G  2,3G  519G   1% /
        tmpfs                 127G     0  127G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/mapper/mpath1    493G   70M  468G   1% /backup
        /dev/mapper/datavg-datalv
                      1,2T   71M  1,1T   1% /db2
        /dev/mapper/datavg-saplv
                      165G   60M  157G   1% /usr/sap
        /dev/mapper/datavg-sapmntlv
                      165G   60M  157G   1% /sapmnt

如果您在 {{site.data.keyword.Db2_on_Cloud_long}} 上安裝以 SAP NetWeaver 為基礎的 SAP 應用程式，您必須在具有管理權的資料庫使用者 (`db2SID`) 所擁有的 `/backup` 之下建立子目錄，以存放完整備份及保存日誌檔。如果要自動保存日誌檔，您應該在 {{site.data.keyword.Db2_on_Cloud_short}} 資料庫中設定 `LOGMETH1`。如需詳細資料，請參閱 [{{site.data.keyword.Db2_on_Cloud_short}} 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://www.ibm.com/support/knowledgecenter/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0051344.html){: new_window}。
