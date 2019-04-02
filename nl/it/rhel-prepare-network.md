---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, ABAP, ASCS instance, ABAP SAP Central Services, application server, database server, three-tier

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 4. Preparazione della tua rete per una configurazione a tre livelli
{: #network}

Se stai pianificando l'installazione di una configurazione a tre livelli, la rete deve essere configurata correttamente. Nell'esempio, sono stati distribuiti un server di database da 192 GB (denominato `sdb192`) e un server delle applicazioni da 32 GB (denominato `e2e1270`). Il server di database ospita anche l'istanza (A)SCS. L'aggiunta di indirizzi IP sulla rete privata al tuo `/etc/hosts` è utile nei prossimi passi e garantisce che il traffico di rete interno SAP transiti per la rete corretta.

![Figura 1. Esempio di configurazione a tre livelli](/images/network-01.png "Esempio di configurazione a tre livelli")

Per stabilire la tua rete, attieniti alla seguente procedura.

1. Accedi ai server e trova la loro configurazione di rete privata.

        [root@sdb192 ~]# ifconfig bond0
            bond0	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A8
                    inet addr:10.17.139.35  Bcast:10.17.139.63 Mask:255.255.255.192
                    inet6 addr: fe80::ec4:7aff:fe66:2da8/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:128080 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:25491 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:19137850 (18.2 MiB)  TX bytes:3426015 (3.2 MiB)

        [root@sdb192 ~]# ifconfig bond1
            bond1	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A9
                    inet addr:208.43.211.118  Bcast:208.43.211.127 Mask:255.255.255.224
                    inet6 addr: fe80::ec4:7aff:fe66:2da9/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:289610 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:109371 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:31216160 (29.7 MiB)  TX bytes:18591947 (17.7 MiB)

Nell'esempio a tre livelli, 10.17.139.35 è l'indirizzo IP privato del server di database; proviene da uno degli intervalli di indirizzi IP da RFC 1597. Puoi anche determinare l'indirizzo IP del server delle applicazioni e aggiungere entrambi gli indirizzi IP a entrambi i `/etc/hosts` dei server.

        [root@sdb192 ~]# cat /etc/hosts
        127.0.0.1 localhost.localdomain localhost
        208.43.211.118 e2e2690.saptest.com e2e2690

        10.17.139.35    sdb192-priv
        10.17.139.46    e2e1270-priv

Aggiungi anche le ultime due righe su `e2e1270`.

## Installazione del software NFS

1. Installa il software NFS `nfs-utils` **su entrambi i server**.

      `[root@sdb192 ~]# yum install nfs-utils`

Assicurati di avviare e registrare `rpcbind` e il servizio NFS sul server di database.

        [root@sdb192 ~]# service rpcbind start
        [root@sdb192 ~]# chkconfig nfs on
        [root@sdb192 ~]# service nfs start

## Utilizzo di NFS per l'esportazione

1. Utilizza NFS per esportare `/sapmnt` e `/usr/sap/trans` dal server di database al server delle applicazioni aggiungendo la voce richiesta al `/etc/exports` del server di database.

        /sapmnt/C10		10.17.139.46(rw,no_root_squash,sync,no_subtree_check)
        /usr/sap/trans	10.17.139.46(rw,no_root_squash,sync,no_subtree_check)

Il valore di esempio `C10` deve essere sostituito con l'ID di sistema SAP per il tuo sistema SAP. Devi creare la directory prima di esportarla. Esegui i seguenti comandi dalla riga di comando del server di database:

        [root@sdb192 ~]# mkdir /sapmnt/C10
        [root@sdb192 ~]# mkdir -p /usr/sap/trans
        [root@sdb192 ~]# exportfs -a

## Montaggio della condivisione NFS

1. Monta la condivisione NFS sul server delle applicazioni aggiungendo la seguente voce al suo `/etc/fstab` e montala dalla riga di comando.

        sdb192-priv:/sapmnt/C10 /sapmnt/C10 nfs defaults 0 0
        sdb192-priv:/usr/sap/trans /usr/sap/trans nfs defaults 0 0

2. Crea le directory di destinazione sul server delle applicazioni e montale.

        [root@e2e1270 ~]# mkdir -p /sapmnt/C10
        [root@e2e1270 ~]# mkdir /usr/sap/trans
        [root@e2e1270 ~]# mount /sapmnt/C10
        [root@e2e1270 ~]# mount /usr/sap/trans

I tuoi server sono ora pronti ad ospitare i componenti di un'installazione SAP distribuita.

## Passi successivi

  * [Aggiunta di archiviazione esterna al tuo {{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-storage)
  * [Installazione del tuo ambiente SAP (applicazioni e software)](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-install_landscape)
