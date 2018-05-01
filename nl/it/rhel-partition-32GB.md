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

# 3. Partizionamento e filesystem
{: #partition_32GB}

Per l'esempio a singolo nodo, hai ordinato un server con un disco logico (su RAID1). C'è un disco con mirroring presente nel sistema operativo, con un filesystem root di grandi dimensioni, uguale alla dimensione totale del disco (con dello spazio utilizzato per `/boot`). Il seguente layout di filesystem è solo un approccio possibile. Per l'utilizzo in produzione, magari attieniti alle informazioni relative alle dimensioni per il tuo sistema poiché altri layout potrebbero rispondere meglio alle tue esigenze o ai requisiti SAP. 

Le tre directory necessarie per l'installazione SAP, `/usr/sap`, `/sapmnt` e `/db2` sono state create:
```
[root@e2e1270 ~]# mkdir /sapmnt
[root@e2e1270 ~]# mkdir /usr/sap
[root@e2e1270 ~]# mkdir /db2
```
Il tuo {{site.data.keyword.baremetal_long}} è ora pronto per l'archiviazione esterna e l'installazione delle tue applicazioni e del tuo software SAP.

## Passi successivi

  * [Aggiunta di archiviazione esterna al tuo {{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [Installazione di applicazioni e software SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
