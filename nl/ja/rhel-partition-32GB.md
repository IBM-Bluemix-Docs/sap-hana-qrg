---



copyright:
  years: 2017, 2018
lastupdated: "2018-02-21"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. パーティショニングとファイル・システム
{: #partition_32GB}

単一ノードの例では、1 つの論理ディスク (RAID1) を備えたサーバーを注文しました。オペレーティング・システム (OS) に表示される 1 つのミラー保護されたディスクが存在し、これには、合計ディスク・サイズ (および `/boot` に使用されるいくらかのスペース) に相当する 1 つの大きなルート・ファイル・システムがあります。次のファイル・システム・レイアウトは、単純に 1 つの方法として考えられるものです。実動に使用する場合は、他のレイアウトの方がお客様のニーズや SAP 要件をより的確に満たす可能性もあるので、お客様のシステムのサイジング情報に従ってもかまいません。 

SAP インストールに必要な 3 つのディレクトリー `/usr/sap`、`/sapmnt`、および `/db2` が作成されました。
```
[root@e2e1270 ~]# mkdir /sapmnt
[root@e2e1270 ~]# mkdir /usr/sap
[root@e2e1270 ~]# mkdir /db2
```
これで、ご使用の {{site.data.keyword.baremetal_long}} を外部ストレージと SAP アプリケーションおよびソフトウェアのインストールに使用する準備ができました。

## 次のステップ

  * [{{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)への外部ストレージの追加
  * [SAP アプリケーションおよびソフトウェアのインストール](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
