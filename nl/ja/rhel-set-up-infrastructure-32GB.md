---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, bring your own license, BYOL, VLAN

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. 32 GB サーバーの注文
{: #install_32GB}

## サーバーの注文
{: #order_32GB}

1. ユーザー固有の資格情報を使用して、[{{site.data.keyword.cloud_notm}} インフラストラクチャーのカスタマー・ポータル ![外部リンクのアイコン](../../icons/launch-glyph.svg "外部リンクのアイコン")](https://control.softlayer.com){: new_window} にログインします。
2. 「アカウント要約」ページで、**「アカウント」** > **「注文 (Place an Order)」**をクリックします。
3. 「デバイス」ページの「{{site.data.keyword.baremetal_short}}」で**「月単位」**をクリックします。 サーバー・リストが表示されます。SAP 認定サーバーはリストの先頭に載っています。
4. **「月単位の開始価格 (STARTING PRICE PER MONTH)」**の下のハイパーリンクをクリックして、サーバー**「BI.S3.NW32 (OS Options)」**を選択します。

BI.S3.NW32 (OS オプション) サーバーは**「毎時」**の請求でも利用できます。
{: note}

## サーバーの構成
{: #configure_server}

1. **「数量」**フィールドは**「1」**のままにしておきます。
2. **「データ・センター」**には**「TOR01」**を選択します。 データ・センターのリストは、特定のデータ・センターにおける製品の可用性によって異なります。
3. **「サーバー」**は、デフォルトでお客様の選択に基づいた事前定義値になるため、変更することはできません。
4. **「RAM」**の選択項目は、デフォルトでお客様のサーバーの選択に基づいた事前定義値になり、変更できませんが、**「32 GB RAM」**をクリックします。
5. **「Redhat」**をクリックし、**オペレーティング・システム**として**「Red Hat Enterprise Linux for SAP Business Application 7.X (64 ビット)」**を選択します。 **注**: オペレーティング・システムに対して所有ライセンスの導入 (BYOL) を行う場合は、**「その他」** > **「オペレーティング・システムなし (No Operating System)」**を選択してください。 詳しくは、[『所有ライセンスの導入』](#byol)を参照してください。
6. **「ディスク・コントローラー 1 (Disk Controller 1)」**ドロップダウン・メニューをクリックし、**「2 TB SATA」**を選択して、もう 1 台、2 TB SATA ドライブも追加します。 **「ディスクの追加 (Add Disk)」**をクリックします。
7. **「すべてのディスクを選択 (Select All Disks)」**をクリックし、**「RAID ストレージ・グループの作成 (Create RAID storage group)」**をクリックします。
8. **「タイプ」**をクリックして**「RAID 1」**を選択し、必要なストレージ総量が賄える**「サイズ」 **を入力します。
9. **「LVM」**はチェック・マークを外したままにし、デフォルトの**「パーティション・テンプレート (Partition Template)」**として**「Linux 基本 (Linux Basic)」**を受け入れます。
10. **「完了」**をクリックします。

## 追加のサーバー・オプションの選択
{: #options_32GB}

1. **「パブリック処理能力」**には**「500 GB」**を選択します。
2.	**「アップリンク・ポート速度」**には**「1 Gbps 冗長パブリックおよびプライベート・ネットワーク・アップリンク」**を選択します。
3. 他のフィールドはすべて、デフォルト値のままにします。 オプションの詳しい説明については、[「カスタム・ベアメタル・サーバーのビルド (Building a custom bare metal server)」](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#addl-server-options)を参照してください。
4.	ページ下部の**「注文に追加」**をクリックします。 注文を確認すると、「ご精算」ページにリダイレクトされます。

## 「拡張システム構成」のセットアップ
{: #adv_config}

「拡張システム構成」の各フィールドには、表 1 の値を使用します。 詳細は、[『拡張サーバー構成オプション (Advanced server configuration options)」](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#adv-system-config)ガイドラインに記載されています。

1. スクロールダウンして、**「拡張システム構成」**で表 1 の値を入力します。

|              フィールド               |      値                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|バックエンド VLAN                      | ドロップダウン・リストから選択。例: `tor01.bcr01a.1241`     |
|サブネット                            | ドロップダウン・リストから選択。例: `10.114.63.64/26`       |
|フロントエンド VLAN                     | ドロップダウン・リストから選択。例: `tor01.fcr01a.851`      |
|サブネット                            | ドロップダウン・リストから選択。例: `158.85.65.224/28`      |
|プロビジョン・スクリプト                 | 強制ブランク                                                          |
|SSH 鍵                          | `追加` (デフォルト)。つまり SSH 鍵なし                            |
|ホスト名                          | 例: `e2e1270`                                               |
|ドメイン                            | 例: `saptest.com`                                           |
{: caption="表 1. 32 GB 拡張構成の値" caption-side="top"}  

## 選択内容の確認
{: #confirm_selections}

1. 「ご精算」ページの選択内容を確認して、ページの右側の**「クラウド・サービスのご利用条件」**および**「サード・パーティーのソフトウェアのご使用条件」**をクリックします。
2. フォームの右側の**「注文の送信」**をクリックします。 注文番号が載ったページにリダイレクトされます。そのページは、受領証でもあるので印刷できます。 さらに、件名が*「お客様の IBM Cloud 注文 ## は承認されました」*という確認の E メールも受け取ります。## は、お客様のご注文番号です。

ご注文の送信後、お客様のサーバーは、ご注文に応じて 1 時間から 4 時間のうちに使用できるようになります。 プロビジョニング・ステップの状況については、「メイン・インフラストラクチャー・カスタマー・ポータル (main infrastructure customer portal)」ページの「デバイスの詳細 (Device Details)」画面 (**「デバイス」 > 「デバイス・リスト (Device List)」**) で確認できます。 お客様の所定のホスト名とドメインに一致する**「デバイス名」**をクリックして、デバイスの状況を確認してください。

## 所有ライセンスの導入
{: #byol}

オペレーティング・システムのライセンスを自分で所有しているときは、ベンダーの指示に基づいて、{{site.data.keyword.baremetal_short}} にそのライセンスをインストールします。 詳しくは、[『OS なしオプション』](/docs/bare-metal?topic=bare-metal-the-no-os-option#how-to-install-an-operating-system-on-a-no-os-server-)を参照してください。

## 次のステップ

  [2. SAP インストール用のサーバーの準備](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-prepare_32GB)

  [3. パーティショニングとファイル・システム](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-partition_32GB)
