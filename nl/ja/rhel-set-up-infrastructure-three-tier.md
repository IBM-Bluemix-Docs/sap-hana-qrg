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

# 1. 3 層セットアップ用の 256 GB サーバーと 32 GB サーバーの注文
{: #install_three_tier}

## サーバーの注文
{: #order_servers}

[32 GB サーバーの注文](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-set-up-infrastructure-32GB.html#order_32GB)のステップに従って、SAP NetWeaver アプリケーション・サーバーを注文します。以下のステップは、データベース・サーバーを注文する際の手順を説明しています。

1. お客様固有の資格情報を使用して、[{{site.data.keyword.cloud_notm}}インフラストラクチャーのカスタマー・ポータル](https://control.softlayer.com)にログインします。
2. 「アカウント要約」ページで**「デバイス」**アイコンをクリックします。
3. 「デバイス」ページの**「{{site.data.keyword.baremetal_long}}」**で**「月単位」**をクリックします。「サーバー・リスト」ダイアログ・ボックスが表示されます。
4. **「月単位の開始価格 (STARTING PRICE PER MONTH)」**の下のハイパーリンクをクリックして、サーバー **「BI.S1.NW256 (OS Options)」**を選択します。

## サーバー・オプションの選択
{: #options_32GB}

1. **「数量」**フィールドは**「1」**のままにしておきます。
2. **「データ・センター」**には**「TOR01」**を選択します。
3. **「サーバー」**は、デフォルトでお客様の選択に基づいた事前定義値になるため、変更することはできません。
4. **「RAM」**の選択項目は、デフォルトでお客様のサーバーの選択に基づいた事前定義値になり、変更できませんが、**「32 GB RAM」**をクリックします。
5. 「オペレーティング・システム」として、**「Red Hat Enterprise Linux for SAP Business Application 6.X」**を選択します。
6. **「ハード・ディスク」**では、2 番目の 2 TB SATA ディスクを選択し、両方のディスクから、合計ストレージ量をカバーする RAID ストレージ・グループ RAID1 を作成して、**「パーティション・テンプレート」**として**「Linux 基本」**を選択します。**「LVM」**はチェック・マークを外したままにします。
7. **「パブリック処理能力」**には**「500 GB」**を選択します。
8. **「アップリンク・ポート速度」**には**「1 Gbps 冗長パブリックおよびプライベート・ネットワーク・アップリンク」**を選択します。
9. この例の場合、他のフィールドはすべて、デフォルト値のままにします。オプションの詳しい説明については、[ベア・メタル・サーバーの構成](https://console.bluemix.net/docs/bare-metal/configuring.html#setting-up-your-bare-metal-servers)を参照することができます。
10.	ページ下部の**「注文に追加」**をクリックします。注文を確認すると、「ご精算」ページにリダイレクトされます。

## 「拡張システム構成」のセットアップ
{: #adv_config}

1. 「拡張システム構成」の各フィールドには、表 1 の値を使用します。詳細は、[拡張システム構成](https://console.bluemix.net/docs/bare-metal/configuring.html#advanced-system-configuration)のガイドラインに記載されています。

|              フィールド          |      値                                                              |
| -------------------------------- | -------------------------------------------------------------------- |
|バックエンド VLAN                 | ドロップダウン・リストから選択。例: `tor01.bcr01a.1241`|
|サブネット                        | ドロップダウン・リストから選択。例: `10.114.63.64/26`  |
|フロントエンド VLAN               | ドロップダウン・リストから選択。例: `tor01.fcr01a.1199`|
|サブネット                        | ドロップダウン・リストから選択。例: `169.55.137.160/27`|
|プロビジョン・スクリプト          | デフォルトは「なし」                                                 |
|SSH 鍵                            | デフォルトは「なし」                                                 |
|ホスト名                          | 例: `e2e2690`                                       |
|ドメイン                          | 例: `saptest.com`                                   |
{: caption="表 1. 拡張構成の値" caption-side="top"}  

## 選択内容の確認
{: #confirm_selections}

1. 「ご精算」ページの選択内容を確認して、ページの右側の**「クラウド・サービスのご利用条件」**および**「サード・パーティーのソフトウェアのご使用条件」**をクリックします。
2. **「注文の送信」**をクリックします。注文番号を示す画面にリダイレクトされます。この画面は注文の受領証でもあるので、印刷できます。

ご注文の送信後、サーバーは、ご注文に応じて 1 時間から 4 時間のうちに使用できるようになります。プロビジョニング・ステップの状況については、{{site.data.keyword.slportal}}のメイン・ページ (**「デバイス」>「デバイス・リスト」**) の「デバイスの詳細」画面で確認できます。お客様の所定のホスト名とドメインに一致する**「デバイス名」**をクリックして、デバイスの状況を確認してください。

## 次のステップ

  [2. SAP インストール用のサーバーの準備](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-256GB.html)
  
  [3. パーティショニングとファイル・システム](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-256GB.html)
  
  [4. ネットワークの準備](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
