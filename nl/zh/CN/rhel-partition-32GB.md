---



copyright:
  years: 2017, 2018
lastupdated: "2018-08-13"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. 分区和文件系统
{: #partition_32GB}

对于单节点示例，您订购了带有一个逻辑磁盘（在 RAID 1 上）的服务器。操作系统 (OS) 中显示了一个镜像磁盘，带有等于磁盘总大小的一个大型根文件系统（其中一些空间用于 `/boot`）。以下文件系统布局只是一种可能的方法。对于生产用途，您可能希望遵循您系统的大小设置信息，因为其他布局可能会更好地满足您的需要或 SAP 需求。


1. 创建 SAP 安装所需的三个目录：`/sapmnt`、`usr/sap` 和 `/db2`。
```
[root@e2e1270 ~]# mkdir /sapmnt
[root@e2e1270 ~]# mkdir /usr/sap
[root@e2e1270 ~]# mkdir /db2
```
您的 {{site.data.keyword.baremetal_long}} 现在已准备好用于外部存储器以及用于安装 SAP 应用程序和软件。

## 后续步骤

  * [向 {{site.data.keyword.baremetal_short}} 添加外部存储器](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [安装 SAP 应用程序和软件](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
