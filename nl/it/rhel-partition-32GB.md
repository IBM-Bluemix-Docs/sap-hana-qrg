---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. Partizionamento e file system
{: #partition_32GB}

Per l'esempio a singolo nodo, hai ordinato un server con un disco logico (su RAID 1). C'è un disco con mirroring presente nel sistema operativo, con un file system root di grandi dimensioni, uguale alla dimensione totale del disco (con dello spazio utilizzato per `/boot`). Il seguente layout di file system è solo un approccio possibile. Per l'utilizzo in produzione, magari attieniti alle informazioni relative alle dimensioni per il tuo sistema poiché altri layout potrebbero rispondere meglio alle tue esigenze o ai requisiti SAP.

1. Crea le tre directory necessarie per l'installazione SAP, `/sapmnt`, `usr\sap` e `/db2`.
```
[root@e2e1270 ~]# mkdir /sapmnt
[root@e2e1270 ~]# mkdir /usr/sap
[root@e2e1270 ~]# mkdir /db2
```
Il tuo {{site.data.keyword.baremetal_long}} è ora pronto per l'archiviazione esterna e l'installazione delle tue applicazioni e del tuo software SAP.

## Passi successivi

  * [Aggiunta di archiviazione esterna al tuo {{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-storage)
  * [Installazione del tuo ambiente SAP (applicazioni e software)](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-install_landscape)
