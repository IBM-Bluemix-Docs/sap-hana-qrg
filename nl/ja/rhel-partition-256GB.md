---



copyright:
  years: 2017, 2018
lastupdated: "2017-08-13"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. パーティショニングとファイル・システム

3 層のサンプルでは、RAID10 の論理ディスクを 1 つ搭載した 192 GB サーバー (データベース・サーバー) と、RAID 1 の論理ディスクを 1 つ搭載した 32 GB サーバー (アプリケーション・サーバー) を注文しました。どちらのサーバーにも、合計ディスク・サイズ (および `/boot` に使用されるいくらかのスペース) に相当する 1 つの大きなルート・ファイル・システムが付属しています。

32 GB サーバーには、`/usr/sap/` ファイル・システムを作成します。ただし、`sapmnt1` と `/usr/sap/trans` はデータベース・サーバー上に作成されます。ネットワーク・ファイル・システム (NFS) はデータベース・サーバーからエクスポートされます。このファイル・システムには、Advanced Business Application Programming (ABAP) SAP Central Service [(A)SCS] インスタンスもホストされます。

次のファイル・システム・レイアウトは、1 つの方法として考えられるものです。 実動に使用する場合は、ご使用のシステムのサイジング情報に従った方がいいでしょう。他のレイアウトの方がお客様のニーズや SAP の要件をより的確に満たす可能性があるためです。あるいは割り当て量を使用してもよいでしょう。


以下のコマンドを使用して、SAP ソフトウェアのインストールに必要なディレクトリー `/sapmnt`、`/usr/sap`、および `/db2` を作成する必要があります。
```
[root@ sdb192 ~]# mkdir /sapmnt
[root@ sdb192 ~]# mkdir /usr/sap
[root@ sdb192 ~]# mkdir /db2
```

## 次のステップ

[4. ネットワークの準備](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
