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

# 1. 为三层设置订购 192 GB 和 32 GB 服务器
{: #install_three_tier}

## 订购服务器
{: #order_servers}

执行[订购 32 GB 服务器](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-install_32GB#order_32GB)中的步骤以订购 SAP NetWeaver 应用程序服务器。以下步骤将指导您执行订购数据库服务器的过程。

## 订购数据库服务器
{: #order-db-server}

1. 使用您的唯一凭证登录到 [{{site.data.keyword.cloud_notm}} 基础架构客户门户网站 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com){: new_window}。
2. 单击“帐户汇总”页面上的**帐户** > **下订单**。
3. 在“设备”页面上的 **{{site.data.keyword.baremetal_long}}** 下单击**每月**。此时将显示“服务器列表”；SAP 认证的服务器位于列表顶部。
4. 单击**每月的起始价格**下的超链接，以选择服务器 **BI.S3.NW192（操作系统选项）**。

BI.S3.NW32（操作系统选项）服务器也可用于**按小时**计费。
{: note}

## 配置数据库服务器
{: #options_192GB}

1. 在**数量**字段中保留 **1** 不变。
2. 选择 **TOR01** 作为**数据中心**。
3. **服务器**缺省为基于所选服务器的预定义值，并且无法更改。
4. 单击 **192 GB RAM**，尽管 **RAM** 选择缺省为基于所选服务器的预定义值并且无法更改。
5. 单击 **Redhat** 并选择 **Red Hat Enterprise Linux for SAP Business Application 7.X（64 位）**作为**操作系统**。**注**：如果您自带许可证 (BYOL) 用于操作系统，请选择**其他** > **无操作系统**。有关更多信息，请参阅[自带许可证](#byol)。
6. 通过单击**磁盘控制器 1** 下拉菜单，然后选择 **2 TB SATA**，添加第二个 2 TB SATA 驱动器。单击**添加磁盘**。
7. 单击**选择所有磁盘**，然后单击**创建 RAID 存储器组**。
8. 单击**类型**，然后选择 **RAID 1**。输入满足所需存储器总量的**大小**。
9. 保留 **LVM** 为未选中状态，并接受缺省**分区模板** > **Linux Basic**。
10. 单击**完成**。

## 选择其他数据库服务器选项
{: #addl-server-options}

1. 为**公用带宽**选择 **500 GB**。
2. 为**上行端口速度**选择 **1 Gbps 冗余公用和专用网络上行链路**。
3. 对于本示例，将其他所有字段保持缺省值。您可以参阅[构建定制裸机服务器](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#addl-server-options)以获取各个选项的详细描述。
4.	单击页面底部的**添加到订单**。在验证订单后，您将重定向到“结帐”页面。

## 设置高级系统配置
{: #adv_config}

1. 将表 1 中的值用于“高级系统配置”下的字段。[高级系统配置](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#adv-system-config)准则中提供了更多信息。

|字段                |值                                                                    |
| -------------------------------- | -------------------------------------------------------------------- |
|后端 VLAN                         |从下拉列表中进行选择，例如，`tor01.bcr01a.1241`      |
|子网                              |从下拉列表中进行选择，例如，`10.114.63.64/26`        |
|前端 VLAN                         |从下拉列表中进行选择，例如，`tor01.fcr01a.851`       |
|子网                              |从下拉列表中进行选择，例如，`158.85.65.224/28`       |
|供应脚本                          |保持为空                                                              |
|SSH 密钥                          |缺省为 `Add`，这意味着没有 SSH 密钥                  |
|主机名                            |例如，`sdb192`                                       |
|域                                |例如，`saptest.com`                                  |
{: caption="表 1. 192 GB 高级配置值" caption-side="top"}  

## 确认您的选择
{: #confirm_selections}

1. 在“结帐”页面上确认您的选择，然后单击页面右侧的**云服务条款**和**第三方软件协议**。
2. 单击**提交订单**。您将重定向到带有您的订单号的屏幕。可以打印此屏幕，因为这也是您的订单回执。

提交订单后，服务器将在一到四个小时（具体取决于您的订单）内可供使用。您可以在 {{site.data.keyword.slportal}} 主页面上的“设备详细信息”屏幕（**设备 > 设备列表**）中检查供应步骤的状态。单击与您的给定“主机名”和“域”匹配的**设备名称**以查看其状态。

## 自带许可证
{: #byol}

如果有自己的操作系统许可证，请按照供应商的指示信息将其安装在 {{site.data.keyword.baremetal_short}} 上。有关更多信息，请参阅[无操作系统选项](/docs/bare-metal?topic=bare-metal-bm-no-os#bm-no-os)。

## 后续步骤

  [2. 准备服务器以用于 SAP 安装](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-prepare_256GB)

  [3. 分区和文件系统](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-3-partitioning-and-file-systems)

  [4. 准备网络](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-network#network)
