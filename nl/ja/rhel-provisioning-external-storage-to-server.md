---



copyright:
  years: 2017, 2018
lastupdated: "2018-02-26"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# サーバーへの外部ストレージの追加
{: #storage}

## 外部ストレージのセットアップ
{: #set_up_storage}

外部ストレージをバックアップ・デバイスとして使用する場合、またはテスト環境でスナップショットを使用してデータベースを素早くリストアする場合は、プロビジョン済みサーバー (複数可) に外部ストレージを追加することができます。3 層の例の場合は、データベースのログ・ファイルの保存と、データベースのオンライン・バックアップおよびオフライン・バックアップの両方にブロック・ストレージが使用されます。最小のバックアップ時間を保証するために役立つよう、最高速のブロック・ストレージ (GB 当たり 4 IOPS) が選択されました。それより低速のブロック・ストレージでもお客様のニーズに十分な場合があります。{{site.data.keyword.blockstoragefull}} について詳しくは、[ブロック・ストレージの概要](https://console.bluemix.net/docs/infrastructure/BlockStorage/index.html#getting-started-with-block-storage)に関する資料を参照してください。


1. お客様固有の資格情報を使用して、[{{site.data.keyword.cloud_notm}}インフラストラクチャーのカスタマー・ポータル](https://control.softlayer.com/)にログインします。
2. **「ストレージ」>「ブロック・ストレージ」**を選択します。
3. 「ブロック・ストレージ」ページの右上隅にある**「ブロック・ストレージを注文」**をクリックします。
4. ストレージ必要量の明細を選択します。表 1 に、一般的なデータベース・ワークロードに対する 4 IOPS/GB も含め、推奨値を示します。

|              フィールド          |      値                                           |
| -------------------------------- | ------------------------------------------------- |
|ストレージ・タイプの選択          | エンデュランス (デフォルト)                       |
|ロケーション                      | TOR01                                             |
|請求方式                          | 月単位 (デフォルト)                               |
|ストレージ・パッケージの選択      | 4 IOPS/GB                                         |
|ストレージ・サイズの選択          | 1000 GB                                           |
|スナップショット・スペース・サイズの指定| 0 GB                                              |
|OS タイプの選択                 | Linux (デフォルト)                                |
{: caption="表 1. ブロック・ストレージの推奨値" caption-side="top"}

5. **「続行」**をクリックします。
6. Select **「マスター・サービス契約を読み...」**を選択して、**「注文する」**をクリックします。
7. ご使用の LUN の右側にある**「アクション」**をクリックして**「ホストの許可」**を選択し、プロビジョン済みストレージにアクセスします。
8. **「デバイス」**を選択します。**「デバイス・タイプ」**はデフォルトで「ベア・メタル・サーバー」になります。**「ハードウェア」**をクリックして、デバイスのホスト名を選択します。
9. **「送信」**ボタンをクリックします。
10. **「デバイス」**> (デバイスを選択) >**「ストレージ」タブ**でプロビジョン済みストレージの状況を確認します。
11. サーバー (iSCSI イニシエーター) の**ターゲット・アドレス**および iSCSI 修飾名 (**IQN**) と、iSCSI サーバーによる許可のための **username** と **password** をメモしておきます。以下のステップで、それらの情報が必要です。
12. [Linuxでの MPIO iSCSI LUN への接続](https://console.bluemix.net/docs/infrastructure/BlockStorage/accessing_block_storage_linux.html#connecting-to-mpio-iscsi-luns-on-linux)のステップに従い、プロビジョン済みサーバーからストレージにアクセスできるようにします。

## ストレージをマルチパスにする方法
{: #multipath}

サンプル・デプロイメントでは、**「ストレージ」**タブから以下のデータを取得しました。
  * ターゲット IP: `10.2.62.78`
  * IQN: `iqn.2005-05.com.softlayer:SL01SU276540-H896345`
  * ユーザー: `SL01SU276540-H896345`
  * パスワード: `EtJ79F4RA33dXm2q`

1. 取得した情報に基づいて、以下を入力します。
```
[root@e2e2690 ~]# cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.2005-05.come.softlayer:SL01SU276540-H986345
``` 
   場合によっては、`/etc/iscsi/initiatorname.iscsi` で既存の項目を置き換える必要があります。

2. `/etc/iscsi/iscsid.conf` の下部に以下の行を追加します。
```
[root@e2e2690 ~]# tail /etc/iscsi/iscsid.conf
# it continue to respond to R2Ts. To enable this, uncomment this line
# node.session.iscsi.FastAbort = No
node.session.auth.authmethod = CHAP
node.session.auth.username = SL01SU276540-H896345
node.session.auth.password = EtJ79F4RA33dXm2q
discovery.sendtargets.auth.authmethod = CHAP
discovery.sendtargets.auth.username = SL01SU276540-H896345
discovery.sendtargets.auth.password = EtJ79F4RA33dXm2q
```

3. ステップ 2 の `username` 値と `password` 値を、外部ストレージのセットアップのステップ 11 でメモした値に置き換えます。

4. 以下の行を入力して、iSCSI ターゲットをディスカバーします。
```
[root@e2e2690 ~]# iscsiadm -m discovery -t sendtargets -p "10.2.62.78"
10.2.62.78:3260,1031 iqn.1992-08.com.netapp:tor0101
10.2.62.87:3260,1032 iqn.1992-08.com.netapp:tor0101
```

5. iSCSI アレイに自動的にログインするよう、ホストを設定します。

      `[root@e2e2690 ~]# iscsiadm -m node -L automatic`

6. マルチパス・デーモンをインストールし、開始します。
```
[root@e2e2690 ~]# yum install device-mapper-multipath
…
[root@e2e2690 ~]# chkconfig multipathd on
[root@e2e2690 ~]# service multipathd start
```

7. 別の LUN がマルチパス出力に表示されるよう、[Linux でのブロック・ストレージ・ボリュームのマウント](https://console.bluemix.net/docs/infrastructure/BlockStorage/accessing_block_storage_linux.html#mounting-block-storage-volumes)のすべてのコマンドを完了します。
```
[root@e2e2690 ~]# multipath -ll
…
3600a098038303452543f464142755a42 dm-9 NETAPP,LUN C-Mode
size=500G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
|-+- policy='round-robin 0' prio=50 status=active
| `- 10:0:0:169 sde 8:64 active ready running
`-+- policy='round-robin 0' prio=10 status=enabled
`- 9:0:0:169  sdf 8:80 active ready running
…`
```

これで、マルチパス・デバイスをすべてのディスク・デバイスと同じように使用できるようになりました。デバイス・パスは、`/dev/mapper/3600a098038303452543f464142755a42` の下に表示されます。

サンプルの `/etc/multipath.conf` を [`multipath.conf` の例](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-sample.html#sample)から取得し、ご使用のサーバー上に作成します。コピーされた特殊文字、DOS のような復帰文字、改行入力によって、予期しない動作が発生する可能性があることに注意してください。内容をコピーした後に、必ず ASCII UNIX ファイルを使用していることを確認してください。

`1/dev/mapper/mpath1` にあるデバイスにアクセスするために、`/etc/multipath.conf` のマルチパス・ブロックを適宜に変更して、別名を作成します。

      multipaths {
	       multipath {
		         wwid 3600a098038303452543f464142755a42
		         alias mpath1
	       }
     }

8. `multipathd` を再始動します。これで、`/backup filesystem` を作成し、ブロック・デバイスにマウントできるようになりました。
        
      [root@e2e2690 ~]# service multipathd restart
      [root@e2e2690 ~]# mkfs.ext4 /dev/mapper/mpath1
      [root@e2e2690 ~]# mkdir  /backup

9. 両方のサーバーのファイル・システムを確認します。出力は、以下のようになります。

        [root@e2e1270 ~]# df -h
        Filesystem		    Size  Used Avail Use% Mounted on
        /dev/sda3             879G  1,5G  833G   1% /
        tmpfs                  16G     0   16G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/sdb2             849G  201M  805G   1% /usr/sap
        db2690-priv:/usr/sap/trans
                      165G   59M  157G   1% /usr/sap/trans
                      db2690-priv:/sapmnt/C10
                      165G   59M  157G   1% /sapmnt/C10

        [root@e2e2690 ~]# df -h
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

SAP NetWeaver ベースの SAP アプリケーションを {{site.data.keyword.Db2_on_Cloud_long}} にインストールする場合、完全なバックアップとアーカイブ・ログ・ファイルを実現するためには、データベース管理者ユーザー (`db2SID`) が所有する `/backup` の下にサブディレクトリーを作成する必要があります。ログ・ファイルの自動アーカイブのためには、ご使用の {{site.data.keyword.Db2_on_Cloud_short}} データベースに `LOGMETH1` を設定する必要があります。詳しくは、[{{site.data.keyword.Db2_on_Cloud_short}} の資料](http://www.ibm.com/support/knowledgecenter/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0051344.html)を参照してください。
