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

# 3. 分区和文件系统

对于三层设置的示例，订购了带有一个逻辑磁盘（在 RAID10 上）的 256 GB 服务器（数据库服务器），以及带有一个逻辑磁盘（在 RAID1 上）的 32 GB 服务器（应用程序服务器）。两个服务器都随附了等于磁盘总大小的一个大型根文件系统（其中一些空间用于 `/boot`）。

对于 32 GB 服务器，请执行“准备服务器以用于 SAP 安装（32 GB）”中的步骤 1-10；然后，继续执行步骤 11-14，但将 `db2` 替换为 `/usr/sap/`。将创建 `sapmnt1` 和 `/usr/sap/trans`，并从数据库服务器导出网络文件系统 (NFS)，数据库服务器上还保存了 Advanced Business Application Programming (ABAP) SAP Central Service [(A)SCS] 实例。

以下文件系统布局是一种可能的方法。对于生产用途，您可能希望遵循您系统的大小设置信息，因为其他布局可能会更好地满足您的需要或 SAP 需求，或者您可能希望使用配额。
需要使用以下命令来创建安装 SAP 软件时必需的目录 `/sapmnt`、`/usr/sap` 和 `/db2`：
```
[root@ e2e2690 ~]# mkdir /sapmnt
[root@ e2e2690 ~]# mkdir /usr/sap
[root@ e2e2690 ~]# mkdir /db2
```
 
## 后续步骤

[4. 准备网络](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
