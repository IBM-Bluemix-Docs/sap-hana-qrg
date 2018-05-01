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

# 3. Partizionamento e filesystem

Per l'esempio a tre livelli, sono stati ordinati un server da 256 GB (server di database) con un disco logico (su RAID10) e un server da 32 GB (server delle applicazioni) con un disco logico (su RAID 1). Entrambi i server sono dotati di un filesystem root di grandi dimensioni che è uguale alla dimensione totale del disco (con dello spazio che viene utilizzato per `/boot`).

Per il server a 32 GB, attieniti ai passi da 1 a 10 in Preparazione del suo server per l'installazione SAP (32 GB); continua quindi con i passi da 11 a 14, sostituendo però /`db2` con `/usr/sap/`. Vengono creati `sapmnt1` e `/usr/sap/trans` e viene esportato l'NFS (network file system) dal server di database, che mantiene anche l'istanza (A)SCS [ABAP (Advanced Business Application Programming) SAP Central Service].

Il seguente layout di filesystem è un approccio possibile. Per l'utilizzo in produzione, magari attieniti alle informazioni relative alle dimensioni per il tuo sistema poiché altri layout potrebbero rispondere meglio alle tue esigenze o ai requisiti SAP oppure utilizza le quote.
Le directory richieste per installare il software SAP, `/sapmnt`, `/usr/sap` e `/db2`, devono essere create utilizzando i seguenti comandi:
```
[root@ e2e2690 ~]# mkdir /sapmnt
[root@ e2e2690 ~]# mkdir /usr/sap
[root@ e2e2690 ~]# mkdir /db2
```
 
## Passi successivi

[4. Preparazione della tua rete](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
