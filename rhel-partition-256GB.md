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

# 3. Partitioning and file systems

For the three-tier example, a 256 GB server (database server) with one logical disk (on RAID10) was ordered and a 32 GB server (application server) with one logical disk (on RAID 1). Both servers come with one large root file system that is equal to the total size of disk (with some space that is used for `/boot`).

For the 32 GB server, follow the steps 1-10 in Preparing your server for the SAP installation (32 GB); then, continue with steps 11-14, but replacing /`db2` with `/usr/sap/`. `sapmnt1` and `/usr/sap/trans` is created, and the network file system (NFS) exported from the database server, which also keeps the Advanced Business Application Programming (ABAP) SAP Central Service [(A)SCS] instance.

The following file system layout is one possible approach. For production use, you might want to follow the sizing information for your system as other layouts might better cater to your needs or SAP requirements, or you might want to use quotas.
The required directories for installing the SAP software, `/sapmnt`, `/usr/sap`, and `/db2`, need to be created by using the following commands:
```
[root@ e2e2690 ~]# mkdir /sapmnt
[root@ e2e2690 ~]# mkdir /usr/sap
[root@ e2e2690 ~]# mkdir /db2
```
 
## Next Steps

[4. Preparing your network](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
