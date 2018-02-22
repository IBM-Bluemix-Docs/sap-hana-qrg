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

# 3. Partitioning and file systems
{: #partition_32GB}

For the single-node example, you ordered a server with one logical disk (on RAID1). There is one mirrored disk appearing in the operating system (OS)—with one large root file system equal to the total size of the disk (with some space used for `/boot`). The following file system layout is just one possible approach. For production use, you might want to follow the sizing information for your system as other layouts might better meet your needs or SAP requirements. 

The three directories required for the SAP installation, `/usr/sap`, `/sapmnt`, and `/db2` have been created:
```
[root@e2e1270 ~]# mkdir /sapmnt
[root@e2e1270 ~]# mkdir /usr/sap
[root@e2e1270 ~]# mkdir /db2
```
Your {{site.data.keyword.baremetal_long}} is now ready for external storage and the installation of your SAP applications and software.

## Next Steps

  * [Adding external storage to your {{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [Installing your SAP applications and software](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
