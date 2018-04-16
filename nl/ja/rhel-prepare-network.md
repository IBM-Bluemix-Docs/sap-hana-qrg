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

# 4. 3 層セットアップ用のネットワークの準備
{: #network}

3 層セットアップのインストールを計画している場合は、ネットワークを正しくセットアップする必要があります。この例では、256 GB のデータベース・サーバー (名前は `e2e2690`) と 32 GB のアプリケーション・サーバー (名前は `e2e1270`) がデプロイされています。データベース・サーバーは、(A)SCS インスタンスもホストしています。プライベート・ネットワークの IP アドレスを `/etc/hosts` に追加すると、以後のステップに役立ち、SAP 内部のネットワーク・トラフィックが正しいネットワークを経由することが保証されます。

![図 1. 3 層セットアップのサンプル](/images/network-01.png "3 層セットアップのサンプル")

以下のステップを使用して、ネットワークを確立します。

1. サーバーにログインして、それらのサーバーのプライベート・ネットワーク構成を検出します。

        [root@e2e2690 ~]# ifconfig bond0
            bond0	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A8
                    inet addr:10.17.139.35  Bcast:10.17.139.63 Mask:255.255.255.192
                    inet6 addr: fe80::ec4:7aff:fe66:2da8/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:128080 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:25491 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:19137850 (18.2 MiB)  TX bytes:3426015 (3.2 MiB)

        [root@e2e2690 ~]# ifconfig bond1
            bond1	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A9
                    inet addr:208.43.211.118  Bcast:208.43.211.127 Mask:255.255.255.224
                    inet6 addr: fe80::ec4:7aff:fe66:2da9/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:289610 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:109371 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:31216160 (29.7 MiB)  TX bytes:18591947 (17.7 MiB)

この 3 層の例では、10.17.139.35 がデータベース・サーバーのプライベート IP アドレスです。これは RFC 1597 からの IP アドレス範囲の 1 つから取られています。アプリケーション・サーバーの IP アドレスを判別して、両方の IP アドレスを両方のサーバーの `/etc/hosts` に追加することもできます。

        [root@e2e2690 ~]# cat /etc/hosts
        127.0.0.1 localhost.localdomain localhost
        208.43.211.118 e2e2690.saptest.com e2e2690
        
        10.17.139.35    e2e2690-priv
        10.17.139.46    e2e1270-priv

最後の 2 行を `e2e1270` にも追加します。

## NFS ソフトウェアのインストール

1. NFS ソフトウェア `nfs-utils` を**両方のサーバーに**インストールします。

      `[root@e2e2690a ~]# yum install nfs-utils`

`rpcbind` および NFS サービスをデータベース・サーバーで開始し、登録します。

        [root@e2e2690 ~]# service rpcbind start
        [root@e2e2690 ~]# chkconfig nfs on
        [root@e2e2690 ~]# service nfs start

## NFS を使用したエクスポート

1. NFS を使用して、`/sapmnt` および `/usr/sap/trans` をデータベース・サーバーからアプリケーション・サーバーにエクスポートします。これは、必要な項目をデータベース・サーバーの `/etc/exports` に追加することによって行います。

        /sapmnt/C10		10.17.139.46(rw,no_root_squash,sync,no_subtree_check)
        /usr/sap/trans	10.17.139.46(rw,no_root_squash,sync,no_subtree_check)

サンプル値の `C10` は、ご使用の SAP システムの SAP システム ID に置き換える必要があります。ディレクトリーは、エクスポートする前に作成しておく必要があります。データベース・サーバーのコマンド・ラインから、以下のコマンドを実行します。

        [root@e2e2690 ~]# mkdir /sapmnt/C10
        [root@e2e2690 ~]# mkdir -p /usr/sap/trans
        [root@e2e2690 ~]# exportfs -a

## NFS 共有のマウント

1. アプリケーション・サーバーの `/etc/fstab` に次の項目を追加してコマンド・ラインからマウントすることにより、アプリケーション・サーバーに NFS 共有をマウントします。

        e2e2690-priv:/sapmnt/C10 /sapmnt/C10 nfs defaults 0 0
        e2e2690-priv:/usr/sap/trans /usr/sap/trans nfs defaults 0 0

2. アプリケーション・サーバー上にターゲット・ディレクトリーを作成して、マウントします。

        [root@e2e1270 ~]# mkdir -p /sapmnt/C10
        [root@e2e1270 ~]# mkdir /usr/sap/trans
        [root@e2e1270 ~]# mount /sapmnt/C10
        [root@e2e1270 ~]# mount /usr/sap/trans

これで、サーバーが分散 SAP インストールのコンポーネントをホストするための準備ができました。 

## 次のステップ

  * [{{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)への外部ストレージの追加  
  * [SAP アプリケーションおよびソフトウェアのインストール](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
