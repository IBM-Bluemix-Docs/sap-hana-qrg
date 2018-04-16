---



copyright:
  years: 2017, 2018
lastupdated: "2017-02-21"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. パーティショニングとファイル・システム

3 層の例では、1 つの論理ディスク (RAID10) を備えた 256 GB サーバー (データベース・サーバー) と、1 つの論理ディスク (RAID 1) を備えた 32 GB サーバー (アプリケーション・サーバー) が注文されました。どちらのサーバーにも、合計ディスク・サイズ (および `/boot` に使用されるいくらかのスペース) に相当する 1 つの大きなルート・ファイル・システムが付属しています。

32 GB サーバーの場合、SAP インストール用のサーバー (32 GB) の準備の 1 から 10 のステップを実行し、その後、ステップ 11 から 14 を続行しますが、/`db2` は `/usr/sap/` に置き換えます。`sapmnt1` および `/usr/sap/trans` が作成され、ネットワーク・ファイル・システム (NFS) がデータベース・サーバーからエクスポートされます。このサーバーは、Advanced Business Application Programming (ABAP) SAP Central Service [(A)SCS] インスタンスも保持します。

次のファイル・システム・レイアウトは、1 つの方法として考えられるものです。実動に使用する場合は、他のレイアウトの方がお客様のニーズや SAP 要件をより的確に満たす可能性もあるので、お客様のシステムのサイジング情報に従ってもかまいません。割り当て量を使用してもよいでしょう。
以下のコマンドを使用して、SAP ソフトウェアのインストールに必要なディレクトリー `/sapmnt`、`/usr/sap`、および `/db2` を作成する必要があります。
```
[root@ e2e2690 ~]# mkdir /sapmnt
[root@ e2e2690 ~]# mkdir /usr/sap
[root@ e2e2690 ~]# mkdir /db2
```
 
## 次のステップ

[4. ネットワークの準備](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
