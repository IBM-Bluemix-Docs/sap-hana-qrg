---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, {{site.data.keyword.cloud_notm}}, deployment, BYOL

subcollection: sap-netweaver

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# 2. インフラストラクチャーのセットアップ
{: #set_up_infrastructure}

SAP NetWeaver {{site.data.keyword.baremetal_long}}、ネットワーク、セキュリティー、ストレージ、オペレーティング・システム (OS) をセットアップする場合の指針を以下のセクションに記します。ご質問があれば、[{{site.data.keyword.cloud_notm}} サポート](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)にご連絡ください。

## サーバーの注文
{: order-server}

以下の手順で {{site.data.keyword.baremetal_short}}を注文します。追加情報については、[カスタム・ベアメタル・サーバーの作成](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server)を参照してください。

1.  ユーザー固有の資格情報を使用して、[{{site.data.keyword.cloud_notm}} インフラストラクチャーのカスタマー・ポータル ![外部リンクのアイコン](../../icons/launch-glyph.svg "外部リンクのアイコン")](https://control.softlayer.com){: new_window} にログインします。
2. 「アカウント要約」ページで、**「アカウント」** > **「注文 (Place an Order)」**をクリックします。
3. 「デバイス」ページの {{site.data.keyword.baremetal_short}} の下にある**「毎月」**リンクをクリックします。「サーバー・リスト (Server List)」ダイアログ・ボックスが表示されます。SAP 認定サーバーは、リストの一番上にあります。
4. **「毎月の開始価格 (STARTING PRICE PER MONTH)」**の下のハイパーリンクをクリックして、該当するサーバーを選択し、「構成/オーダー (Configure/Order)」ページに移動します。

BI.S3.NW32 (OS オプション) サーバーは**「毎時」**の請求でも利用できます。
{: note}

   SAP NetWeaver 認定サーバーは、「CPU モデル」の下の **-NW** で見分けることができます。Red Hat ベースのサーバー構成については、ステップ 7 と[サーバー・オプションの選択](#select_options)のステップ 1 で説明します。これらの手順は、Microsoft Windows の場合と同じです。VMware の場合は、選択肢が限られることに注意してください。しかし、ステップは同じです。

   SAP NetWeaver サーバー名の復号方法の例を以下に示します。

| サーバー名 | 命名規則コンポーネント| 意味 |
| --- | --- | --- |
| BI.S3.NW768 | BI | Bluemix インターフェース |
| | S3 | シリーズ 2 (プロセッサー世代) |
| | | S1 は Ivy Bridge/Haswell |
| | | S2 は Broadwell |
| | | S3 は Skylake/Kaby Lake |
| | NW | NetWeaver 認定サーバー |
| | 768 | RAM の容量 |

5. 注文するサーバーの数を**「数量」**フィールドに入力し、**「データ・センター」**でデータ・センターを選択します。
6. **「サーバー」**、**「RAM」**、プライベート・ストレージ・オプションは、サーバーの選択内容に基づいてデフォルト設定が決まり、変更できません。サーバーの注文後に、{{site.data.keyword.IBM_notm}} {{site.data.keyword.blockstorageshort}} for {{site.data.keyword.cloud_notm}} または {{site.data.keyword.filestorage_full_notm}} を注文します。
7. **「オペレーティング・システム」**で、Microsoft、Red Hat、または SUSE を選択し、サーバー用の特定のオペレーティング・システムまたは VMware ハイパーバイザーを選択します。

オペレーティング・システムに関してライセンス持ち込み (BYOL) を行う場合は、**「その他」**>**「オペレーティング・システムなし」**を選択します。詳しくは、[『所有ライセンスの導入』](#byol)を参照してください。
{: note}

## サーバー・オプションの選択
{: #select_options}

次の手順では、構成に追加するディスクのタイプと数を選択します。RAID (Redundant Arrays of Independent Disks) ストレージ・グループと、RAID ストレージ・グループ上のパーティショニング・レイアウトに関して、複数の異なるオプションを選択することもできます。詳しくは、[RAID について](/docs/bare-metal?topic=bare-metal-about-raid#about-raid)か、[{{site.data.keyword.cloud_notm}} サポート](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)を参照してください。

1. **「SAP 認定サーバー (SAP Certified Servers)」**の下で、サーバーの使用計画に基づいて選択を行います。詳細については、[Design Decision Tool ![外部リンクのアイコン](../../icons/launch-glyph.svg "外部リンクのアイコン")](https://github.com/ibm-cloud-architecture/infrastructure-design-decision-tool){: new_window} (ツールまでスクロールダウンします) を参照するか、[{{site.data.keyword.cloud_notm}} サポート](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)にお問い合わせください
2. ページの下部にある**「注文に追加」**ボタンをクリックします。

## 「拡張システム構成」のセットアップ
{: #adv_config}

1. [拡張システム構成](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server)のガイドラインに沿って、**「拡張システム構成」**ウィンドウの値を設定します。

SAP ホスト名は、最大 13 文字の英数字で構成する必要があります。SAP ホスト名の詳細については、[SAP Notes 611361 ![外部リンクのアイコン](../../icons/launch-glyph.svg "外部リンクのアイコン")](https://launchpad.support.sap.com/#/611361){: new_window} および [129997 ![外部リンクのアイコン](../../icons/launch-glyph.svg "外部リンクのアイコン")](https://launchpad.support.sap.com/#/129997){: new_window} を参照してください。

ご使用の環境のパブリック/プライベート・セットアップについては決定済みで、今後は以下のことを計画しているとします。
  * 相互に通信する必要のある複数の SAP システムをインストールする。*または*
  * 3 層 SAP セットアップ (データベース・インスタンスとアプリケーション・インスタンスが別々のハードウェア上にある) を選択する。

この場合、内部アドレスと外部アドレスの設定を反映するように名前解決が行われることを確認してください。外部アドレスは、完全修飾ドメイン名 (FQDN) によって解決されるサーバーのホスト名と一致するようにします。

内部アドレスはドメイン・ネーム・システム (DNS) に表示されません。内部 IP をサーバー間の通信に使用する必要があるので、`/etc/hosts` のファイルまたは Microsoft Windows の「host」ファイルを適宜拡張するようにしてください。この情報は、VMware ESXi ベースのデプロイメントのゲスト・オペレーティング・システムにも適用される場合があります。

## 選択内容の確認
{: #confirm_selections}

1. 「チェックアウト (Checkout)」ページで選択内容を確認し、**「クラウド・サービスのご利用条件 (Cloud Service terms)」**と**「サード・パーティーのソフトウェアのご使用条件 (3rd Party Software Agreement)」**チェック・ボックスをクリックします。
2. 「アカウントの作成: 請求の入力 (Create Account: Enter Billing)」までスクロールダウンし、請求に使用するクレジット・カードの**「支払いタイプ」、「名前」、「カード」**と**「有効期限」**(日付) を入力します。
3. **「注文の送信」**ボタンをクリックします。注文番号を示す画面にリダイレクトされます。この画面は注文の受領証でもあるので、印刷できます。

_{{site.data.keyword.cloud_notm}} Order ## has been approved_ という件名の確認 E メールが、お客様のプロファイルにある E メール・アドレスに送信されます。この E メールは、サーバーが承認され、デプロイ中であることを示す通知です。デプロイ後、サーバーが利用可能であり、[{{site.data.keyword.cloud_notm}} インフラストラクチャーのカスタマー・ポータル ![外部リンクのアイコン](../../icons/launch-glyph.svg "外部リンクのアイコン")](https://control.softlayer.com){: new_window} を介して管理できることを知らせる別の通知が送信されます。

## 所有ライセンスの導入
{: #byol}

自分でオペレーティング・システム・ライセンスを所有している場合、ベンダーの指示に基づいて、それを {site.data.keyword.baremetal_short} にインストールします。詳しくは、[『OS なしオプション』](/docs/bare-metal?topic=bare-metal-how-to-install-an-operating-system-on-a-no-os-server-#how-to-install-an-operating-system-on-a-no-os-server-)を参照してください。

## 次のステップ

これで、{{site.data.keyword.baremetal_short}}の管理を開始する準備ができました。次の手順については、[SAP NetWeaver 環境の管理](/docs/infrastructure/sap-netweaver?topic=sap-netweaver-manage_environment#manage_environment)を参照してください。
