---



copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-14"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. Partitioning and file systems

For the three-tier example, a 192 GB server (database server) with one logical disk (on RAID10) was ordered and a 32 GB server (application server) with one logical disk (on RAID 1). Both servers come with one large root file system that is equal to the total size of disk (with some space that is used for `/boot`).

For the 32 GB server, create the `/usr/sap/` file system. Note that `sapmnt1` and `/usr/sap/trans` are created on the database server. The network file system (NFS) is exported from the database server, which also hosts the Advanced Business Application Programming (ABAP) SAP Central Service [(A)SCS] instance.

The following file system layout is one possible approach. For production use, you might want to follow the sizing information for your system as other layouts might better cater to your needs or SAP requirements, or you might want to use quotas.

The required directories for installing the SAP software, `/sapmnt`, `/usr/sap`, and `/db2`, need to be created by using the following commands:
```
[root@ sdb192 ~]# mkdir /sapmnt
[root@ sdb192 ~]# mkdir /usr/sap
[root@ sdb192 ~]# mkdir /db2
```

## Next Steps

[4. Preparing your network](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-network#network)
