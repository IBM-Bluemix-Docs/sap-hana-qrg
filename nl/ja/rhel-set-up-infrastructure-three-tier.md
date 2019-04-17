---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, bring your own license, BYOL, VLAN, application server, database server, three-tier, SAP certified servers

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. 3 層セットアップ用の 192 GB サーバーと 32 GB サーバーの注文
{: #install_three_tier}

## サーバーの注文
{: #order_servers}

[32 GB サーバーの注文](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-install_32GB#order_32GB)のステップに従って、SAP NetWeaver アプリケーション・サーバーを注文します。以下のステップは、データベース・サーバーを注文する際の手順を説明しています。

## データベース・サーバーの注文
{: #order-db-server}

1. ユーザー固有の資格情報を使用して、[{{site.data.keyword.cloud_notm}} インフラストラクチャーのカスタマー・ポータル ![外部リンクのアイコン](../icons/launch-glyph.svg "外部リンクのアイコン")](https://control.softlayer.com){: new_window}にログインします。
2. 「アカウント要約」ページで、**「アカウント」** > **「注文 (Place an Order)」**をクリックします。
3. 「デバイス」ページの**「{{site.data.keyword.baremetal_long}}」**で**「月単位」**をクリックします。 サーバー・リストが表示されます。SAP 認定サーバーはリストの先頭に載っています。
4. **「月単位の開始価格 (STARTING PRICE PER MONTH)」**の下のハイパーリンクをクリックして、サーバー**「BI.S3.NW192 (OS Options)」**を選択します。

BI.S3.NW32 (OS オプション) サーバーは**「毎時」**の請求でも利用できます。
{: note}

## データベース・サーバーの構成
{: #options_192GB}

1. **「数量」**フィールドは**「1」**のままにしておきます。
2. **「データ・センター」**には**「TOR01」**を選択します。
3. **「サーバー」**は、デフォルトでお客様の選択に基づいた事前定義値になるため、変更することはできません。
4. **「RAM」**は、デフォルトでお客様が選択したサーバーに基づく事前定義値が選択されるため変更できませんが、** 「192 GB RAM」**をクリックします。
5. **「Redhat」**をクリックし、**オペレーティング・システム**として**「Red Hat Enterprise Linux for SAP Business Application 7.X (64 ビット)」**を選択します。 **注**: オペレーティング・システムに対して所有ライセンスの導入 (BYOL) を行う場合は、**「その他」** > **「オペレーティング・システムなし (No Operating System)」**を選択してください。 詳しくは、[『所有ライセンスの導入』](#byol)を参照してください。
6. **「ディスク・コントローラー 1 (Disk Controller 1)」**ドロップダウン・メニューをクリックし、**「2 TB SATA」**を選択して、もう 1 台、2 TB SATA ドライブも追加します。 **「ディスクの追加 (Add Disk)」**をクリックします。
7. **「すべてのディスクを選択 (Select All Disks)」**をクリックし、**「RAID ストレージ・グループの作成 (Create RAID storage group)」**をクリックします。
8. **「タイプ」**をクリックして**「RAID 1」**を選択し、必要なストレージ総量が賄える**「サイズ」 **を入力します。
9. **「LVM」**はチェック・マークを外したままにし、デフォルトの**「パーティション・テンプレート (Partition Template)」**として**「Linux 基本 (Linux Basic)」**を受け入れます。
10. **「完了」**をクリックします。

## 追加のデータベース・サーバー・オプションの選択
{: #addl-server-options}

1. **「パブリック処理能力」**には**「500 GB」**を選択します。
2. **「アップリンク・ポート速度」**には**「1 Gbps 冗長パブリックおよびプライベート・ネットワーク・アップリンク」**を選択します。
3. この例の場合、他のフィールドはすべて、デフォルト値のままにします。 オプションの詳しい説明については、[『カスタム・ベアメタル・サーバーのビルド (Building a custom bare metal server)』](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#addl-server-options)を参照することができます。
4.	ページ下部の**「注文に追加」**をクリックします。 注文を確認すると、「ご精算」ページにリダイレクトされます。

## 「拡張システム構成」のセットアップ
{: #adv_config}

1. 「拡張システム構成」の各フィールドには、表 1 の値を使用します。 詳細は、[拡張システム構成](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#adv-system-config)のガイドラインに記載されています。

|              フィールド               |      値                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|バックエンド VLAN                      | ドロップダウン・リストから選択。例: `tor01.bcr01a.1241`     |
|サブネット                            | ドロップダウン・リストから選択。例: `10.114.63.64/26`       |
|フロントエンド VLAN                     | ドロップダウン・リストから選択。例: `tor01.fcr01a.851`      |
|サブネット                            | ドロップダウン・リストから選択。例: `158.85.65.224/28`      |
|プロビジョン・スクリプト                 | 強制ブランク                                                          |
|SSH 鍵                          | `追加` (デフォルト)。つまり SSH 鍵なし                            |
|ホスト名                          | 例: `sdb192`                                                |
|ドメイン                            | 例: `saptest.com`                                           |
{: caption="表 1. 192 GB 拡張構成の値" caption-side="top"}  

## 選択内容の確認
{: #confirm_selections}

1. 「ご精算」ページの選択内容を確認して、ページの右側の**「クラウド・サービスのご利用条件」**および**「サード・パーティーのソフトウェアのご使用条件」**をクリックします。
2. **「注文の送信」**をクリックします。 注文番号を示す画面にリダイレクトされます。 この画面は注文の受領証でもあるので、印刷できます。

ご注文の送信後、サーバーは、ご注文に応じて 1 時間から 4 時間のうちに使用できるようになります。 プロビジョニング・ステップの状況については、{{site.data.keyword.slportal}}のメイン・ページ (**「デバイス」>「デバイス・リスト」**) の「デバイスの詳細」画面で確認できます。 お客様の所定のホスト名とドメインに一致する**「デバイス名」**をクリックして、デバイスの状況を確認してください。

## 所有ライセンスの導入
{: #byol}

オペレーティング・システムのライセンスを自分で所有しているときは、ベンダーの指示に基づいて、{{site.data.keyword.baremetal_short}} にそのライセンスをインストールします。 詳しくは、[『OS なしオプション』](/docs/bare-metal?topic=bare-metal-bm-no-os#bm-no-os)を参照してください。

## 次のステップ

  [2. SAP インストール用のサーバーの準備](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-prepare_256GB)

  [3. パーティショニングとファイル・システム](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-3-partitioning-and-file-systems)

  [4. ネットワークの準備](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-network#network)
