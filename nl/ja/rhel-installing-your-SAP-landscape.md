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

# SAP ランドスケープのインストール
{: #install_landscape}

## RPM パッケージのインストール (前提アプリケーション)
{: #RPM}

SAP のインストールには、稼働する OS および OS デーモンにインストールされるパッケージ用に特定の前提アプリケーションが必要です。それらの前提アプリケーションの最新のリストについては、SAP の最新の[インストール・ガイド](https://support.sap.com/software/installations.html) ([SAP S ユーザー ID](/docs/infrastructure/sap-netweaver/sap-index.html#getting-started) が必要) および[サポート・ノート](https://support.sap.com/notes) (SAP S ユーザー ID が必要) を参照してください。以下の 2 つのパッケージをさらにインストールする必要があります。
* compat-sap-c++: C++ ランタイムと SAP が使用するコンパイラーとの互換性を実現します。
* uuidd: UUID の作成のための OS サポートを保守します。

### UUIDD がインストールされているかどうかの確認

1. `uuid daemon (uuidd)` がインストールされているかどうかを確認します。インストールされていない場合は、インストールして開始します。
```
[root@e2e2690 ~]# rpm -qa | grep uuidd
[root@e2e2690 ~]# yum install uuidd
[root@e2e2690 ~]# chkconfig uuidd on
[root@e2e2690 ~]# service uuidd start
```

### compat-sap-c++ パッケージのインストール

1. [SAP Note 2195019](https://launchpad.support.sap.com/#/notes/2195019) に従い、compat-sap-c++ パッケージをインストールして、SAP バイナリーに必要な特定のソフト・リンクを作成します。
```
[root@e2e2690 ~]# yum install compat-sap-c++
....
[root@e2e2690 ~]# mkdir -p /usr/sap/lib
[root@e2e2690 ~]# ln -s /opt/rh/SAP/lib64/compat-sap-c++.so /usr/sap/lib/libstdc++.so.6
```

## SAP ソフトウェアのダウンロード
{: download_software}

[SAP Service Marketplace](https://websmp201.sap-ag.de/) にログインし、必要な DVD をローカル共有ドライブにダウンロードします。ご使用のプロビジョン済みサーバーにファイルを転送します。別のオプションとして、[SAP Software Download Manager](https://support.sap.com/en/my-support/software-downloads.html#section_995042677) をダウンロードすることができます。これをターゲット・サーバーにインストールし、DVD イメージをサーバーに直接、ダウンロードします。 

## SAP の SWPM GUI の準備
{: #prepare_for_GUI}

ご使用のネットワーク帯域幅および待ち時間によっては、SAP Software Provisioning Manager (SWPM) グラフィカル・ユーザー・インターフェース (GUI) をリモート側で仮想ネットワーク・コンピューティング (VNC) セッションによって実行することもできます。もう 1 つのオプションは、この GUI をローカル側で実行し、ターゲット・マシン上の SWPM に接続することです。GUI をローカル側で実行する場合は、[SWPM の資料](https://wiki.scn.sap.com/wiki/display/SL/Software+Provisioning+Manager+1.0)を参照してください。 

以下のステップは、SWPM GUI をリモート側で VNC セッションによって実行する概略を示しています。このオプションは VNC サーバーをインストールしますが、その VNC サーバーがオペレーティング・システムの堅牢化にそぐわない可能性もあるので、セキュリティーのための方策が十分であることを確認してください。VNC の機能の概要に精通していない場合は、[VNC の資料](http://searchnetworking.techtarget.com/definition/virtual-network-computing)を参照してください。

1. 以下のコマンドを使用して、VNC サーバーをインストールします。
```
  [root@e2e2690 ~]# yum install tigervnc-server

  ...
```

2. 次のコマンドを使用して、X11 ウィンドウ・マネージャー `twm` をインストールします。これは、Linux ディストリビューションに含まれています。

`[root@e2e2690 ~]# yum install twm`

3. 端末エミュレーター (例えば、`xterm`) をインストールします。
 
 `[root@e2e2690 ~]# yum install xterm`

4. コマンド・ラインから VNC サーバーを始動します。
 
 `[root@e2e2690 ~]# vncserver`

この時点で VNC クライアント・プログラムが必要になります。無償ですべてのオペレーティング・システムに使用できる複数の実装が存在します。一般に、ポート `590X` (ここで、X は稼働しているサーバーの番号で、1 から始まります) がクライアントからアクセス可能であることが必要です。

`twm` のバックグラウンド・メニューから `xterm` を開始することが必要な場合もあります。`xterm` から `SWPM` (`sapinst`) を開始することができます。

## SAP ソフトウェアのインストール
{: #install_software}

インストール・メディアをダウンロードした後、標準的な SAP インストール手順に従います。この手順は、ご使用の SAP バージョンとコンポーネントのための SAP インストール・ガイド、および対応する SAP ノートに文書化されています。

`xterm` から SAP SWPM を開始し、インストール・ステップを開始することができます。 

### 3 層環境での SAP ソフトウェアのインストール

3 層セットアップ用の SAP の SWPM のステップに従います。 

1. **「分散システム」**を選択して、データベース・サーバーへの ASCS インストールとデータベース・インストールを実行します。 
2. アプリケーション・サーバーに Application Server ABAP をインストールします。アプリケーション・サーバーのインストール中は、必ず ASCS のプライベート・アドレスとデータベース・ホスト名を使用してください。 

プライベート・アドレスとホスト名を使用すると、アプリケーション・サーバーと ASCS またはデータベースとの間のネットワーク・トラフィックが必ず、パブリック・ネットワークでなくプライベート・ネットワークをパス・スルーすることが保証されます。
