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

# 3. 分区和文件系统

对于三层示例，订购了带有一个逻辑磁盘（在 RAID10 上）的 192 GB 服务器（数据库服务器），以及带有一个逻辑磁盘（在 RAID1 上）的 32 GB 服务器（应用程序服务器）。两个服务器都随附了等于磁盘总大小的一个大型根文件系统（其中一些空间用于 `/boot`）。

对于 32 GB 服务器，创建 `/usr/sap/` 文件系统。请注意，`sapmnt1` 和 `/usr/sap/trans` 也要在数据库服务器上创建。从数据库服务器导出网络文件系统 (NFS)，其上还托管 Advanced Business Application Programming (ABAP) SAP Central Service [(A)SCS] 实例。

以下文件系统布局是一种可能的方法。对于生产用途，您可能希望遵循您系统的大小设置信息，因为其他布局可能会更好地满足您的需要或 SAP 需求，或者您可能希望使用配额。


需要使用以下命令来创建安装 SAP 软件时必需的目录 `/sapmnt`、`/usr/sap` 和 `/db2`：
```
[root@ sdb192 ~]# mkdir /sapmnt
[root@ sdb192 ~]# mkdir /usr/sap
[root@ sdb192 ~]# mkdir /db2
```

## 后续步骤

[4. 准备网络](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
