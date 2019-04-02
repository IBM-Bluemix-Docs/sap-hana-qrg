---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, ABAP, ASCS Instance, Database Instance, ABAP SAP Central Services, SWPM, application server, database server

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# SAP ランドスケープのインストール
{: #install_landscape}

## RPM パッケージのインストール (前提アプリケーション)
{: #RPM}

SAP のインストールには、稼働する OS および OS デーモンにインストールされるパッケージ用に特定の前提アプリケーションが必要です。 それらの前提アプリケーションの最新のリストについては、SAP の[インストール・ガイド ![外部リンクのアイコン](../icons/launch-glyph.svg "外部リンクのアイコン")](https://support.sap.com/software/installations.html)}: new_window ([SAP S-user ID](/docs/infrastructure/sap-netweaver?topic=sap-netweaver-getting-started#getting-started) が必要) および [サポート・ノート ![外部リンクのアイコン](../icons/launch-glyph.svg "外部リンクのアイコン")](https://support.sap.com/notes){: new_window} (SAP S-user ID が必要) を参照してください。以下の 2 つのパッケージをさらにインストールする必要があります。
* compat-sap-c++: SAP で使用されているコンパイラーとの C++ ランタイムの互換性は、通常はこれで実現されます。 32 GB アプリケーション・サーバーと 192 GB データベース・サーバーのいずれでも OS には Red Hat Enterprise Linux for SAP Business Application 7.X が選択されたため、`compat-sap-c++-7` を使用してください。
* uuidd: UUID の作成のための OS サポートを保守します。

### UUIDD がインストールされているかどうかの確認

1. `uuid daemon (uuidd)` がインストールされているかどうかを確認します。 インストールされていない場合は、インストールして開始します。
```
[root@sdb192 ~]# rpm -qa | grep uuidd
[root@sdb192 ~]# yum install uuidd
[root@sdb192 ~]# chkconfig uuidd on
[root@sdb192 ~]# service uuidd start
```

### compat-sap-c++-7 パッケージのインストール

1. [SAP Note 2195019 ![外部リンクのアイコン](../icons/launch-glyph.svg "外部リンクのアイコン")](https://launchpad.support.sap.com/#/notes/2195019){: new_window} に従い、compat-sap-c++-7 パッケージをインストールして、特定のソフト・リンクを作成します。SAP のバイナリーでは、このようにする必要があります。このライブラリーが必要かどうかは、インストールする製品向けのリリース固有 SAP Notes を調べて判断してください。
```
[root@sdb192 ~]# yum install compat-sap-c++-7-7.2.1-2.e17_4.x86_64.rpm
....
[root@sdb192 ~]# mkdir -p /usr/sap/lib
[root@sdb192 ~]# ln -s /opt/rh/SAP/lib64/compat-sap-c++.so /usr/sap/lib/libstdc++.so.6
```

## SAP ソフトウェアのダウンロード
{: download_software}

[SAP サポート・ポータル ![外部リンクのアイコン](../icons/launch-glyph.svg "外部リンクのアイコン")にログイン](https://support.sap.com/en/index.html){: new_window}し、**「Download Software」**をクリックして、必要な DVD をローカル共有ドライブにダウンロードします。
ご使用のプロビジョン済みサーバーにファイルを転送します。 もう 1 つのオプションは、[SAP Software Download Manager ![外部リンクのアイコン](../icons/launch-glyph.svg "外部リンクのアイコン")](https://support.sap.com/en/my-support/software-downloads.html#section_995042677){: new_window} をダウンロードし、それをターゲット・サーバーにインストールし、DVD イメージをサーバーに直接ダウンロードすることです。

## SAP の SWPM GUI の準備
{: #prepare_for_GUI}

ご使用のネットワーク帯域幅および待ち時間によっては、SAP Software Provisioning Manager (SWPM) グラフィカル・ユーザー・インターフェース (GUI) をリモート側で仮想ネットワーク・コンピューティング (VNC) セッションによって実行することもできます。 もう 1 つのオプションは、この GUI をローカル側で実行し、ターゲット・マシン上の SWPM に接続することです。 GUI をローカル側で実行する場合、[SWPM の資料![外部リンクのアイコン](../icons/launch-glyph.svg "外部リンクのアイコン")](https://wiki.scn.sap.com/wiki/display/SL/Software+Provisioning+Manager+1.0+and+2.0){: new_window}を参照してください。

以下のステップは、SWPM GUI をリモート側で VNC セッションによって実行する概略を示しています。 このオプションは VNC サーバーをインストールしますが、その VNC サーバーがオペレーティング・システムの堅牢化にそぐわない可能性もあるので、セキュリティーのための方策が十分であることを確認してください。 VNC の機能の概要に精通していない場合は、[VNC の資料![外部リンクのアイコン](../icons/launch-glyph.svg "外部リンクのアイコン")](http://searchnetworking.techtarget.com/definition/virtual-network-computing){: new_window}を参照してください。

1. 以下のコマンドを使用して、VNC サーバーをインストールします。
```
  [root@sdb192 ~]# yum install tigervnc-server

  ...
```

2. 次のコマンドを使用して、X11 ウィンドウ・マネージャー `twm` をインストールします。これは、Linux ディストリビューションに含まれています。

`[root@sdb192 ~]# yum install twm`

3. 端末エミュレーター (例えば、`xterm`) をインストールします。

 `[root@sdb192 ~]# yum install xterm`

4. コマンド・ラインから VNC サーバーを始動します。

 `[root@sdb192 ~]# vncserver`

この時点で VNC クライアント・プログラムが必要になります。無償ですべてのオペレーティング・システムに使用できる複数の実装が存在します。 一般に、ポート `590X` (ここで、X は稼働しているサーバーの番号で、1 から始まります) がクライアントからアクセス可能であることが必要です。

`twm` のバックグラウンド・メニューから `xterm` を開始することが必要な場合もあります。 `xterm` から `SWPM` (`sapinst`) を開始することができます。

## SAP ソフトウェアのインストール
{: #install_software}

インストール・メディアをダウンロードした後、標準的な SAP インストール手順に従います。この手順は、ご使用の SAP バージョンとコンポーネントのための SAP インストール・ガイド、および対応する SAP ノートに文書化されています。

`xterm` から SAP SWPM を開始し、インストール・ステップを開始することができます。

### 3 層環境での SAP ソフトウェアのインストール

3 層セットアップ用の SAP の SWPM のステップに従います。

1. **「分散システム」**を選択して、データベース・サーバーへの ASCS インストールとデータベース・インストールを実行します。
2. アプリケーション・サーバーに Application Server ABAP をインストールします。 アプリケーション・サーバーのインストール中は、必ず ASCS のプライベート・アドレスとデータベース・ホスト名を使用してください。

プライベート・アドレスとホスト名を使用すると、アプリケーション・サーバーと ASCS またはデータベースとの間のネットワーク・トラフィックが必ず、パブリック・ネットワークでなくプライベート・ネットワークをパス・スルーすることが保証されます。
