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

# 3. Partizionamento e file system

Per l'esempio a tre livelli, sono stati ordinati un server da 192 GB (server di database) con un disco logico (su RAID10) e un server da 32 GB (server delle applicazioni) con un disco logico (su RAID 1). Entrambi i server sono dotati di un file system root di grandi dimensioni che è uguale alla dimensione totale del disco (con dello spazio che viene utilizzato per `/boot`).

Per il server da 32 GB, crea il file system `/usr/sap/`. Nota che `sapmnt1` e `/usr/sap/trans` vengono creati nel server database. L'NFS (network file system) viene esportato dal server di database, che ospita anche l'istanza (A)SCS [ABAP (Advanced Business Application Programming) SAP Central Service].

Il seguente layout di file system è un approccio possibile. Per l'utilizzo in produzione, magari attieniti alle informazioni relative alle dimensioni per il tuo sistema poiché altri layout potrebbero rispondere meglio alle tue esigenze o ai requisiti SAP oppure utilizza le quote.

Le directory richieste per installare il software SAP, `/sapmnt`, `/usr/sap` e `/db2`, devono essere create utilizzando i seguenti comandi:
```
[root@ sdb192 ~]# mkdir /sapmnt
[root@ sdb192 ~]# mkdir /usr/sap
[root@ sdb192 ~]# mkdir /db2
```

## Passi successivi

[4. Preparazione della tua rete](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
