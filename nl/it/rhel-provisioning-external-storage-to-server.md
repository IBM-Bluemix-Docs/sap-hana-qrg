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

# Aggiunta di archiviazione esterna al tuo server
{: #storage}

## Configurazione dell'archiviazione esterna
{: #set_up_storage}

L'archiviazione esterna può essere aggiunta al tuo server o ai tuoi server di cui è stato eseguito il provisioning se vuoi farne uso come un dispositivo di backup oppure utilizzare un'istantanea per ripristinare rapidamente il tuo database in un ambiente di test. Per l'esempio a tre livelli, l'archiviazione a blocchi viene utilizzata sia per archiviare i file di log del database che per i backup online e offline per il database. È stata selezionata l'archiviazione a blocchi più veloce (4 IOPS per GB) per contribuire a garantire un tempo di backup minimo. Un'archiviazione a blocchi più lenta potrebbe essere sufficiente per le tue esigenze. Per ulteriori informazioni su {{site.data.keyword.blockstoragefull}}, consulta [Introduzione all'archiviazione a blocchi](https://console.bluemix.net/docs/infrastructure/BlockStorage/index.html#getting-started-with-block-storage).


1. Accedi al [Portale del cliente dell'infrastruttura {{site.data.keyword.cloud_notm}}](https://control.softlayer.com/) con le tue credenziali univoche.
2. Seleziona **Storage** > **Block Storage.**
3. Fai clic su **Order Block Storage** nell'angolo superiore destro della pagina Block Storage.
4. Seleziona le specifiche per le tue esigenze di archiviazione. La Tabella 1 contiene i valori consigliati, tra cui 4 IOPS/GB per un tipico carico di lavoro del database.

|              Campo               |      Valore                                        |
| -------------------------------- | ------------------------------------------------- |
|Location                          | TOR01                                             |
|Billing Method                    | Monthly (valore predefinito)                                 |
|New Storage Size                  | 1000 GB                                           |
|Storage IOPS Options              | Endurance (Tiered IOPS) (default)                 |
|Endurance Tiered IOPS             | 10 GB                                             |
|Snapshot Space Size               | 0 GB                                              |
|OS Type                           | Defaults to Linux                                 |
{: caption="Tabella 1. Valori consigliati per l'archiviazione a blocchi" caption-side="top"}

5. Fai clic sulle due caselle di spunta e su **Place Order**.

## Autorizzazione host
{: authorize-host}

1. Fai clic su **Actions** a destra della tua LUN e seleziona **Authorize Host** per accedere all'archiviazione di cui è stato eseguito il provisioning.
2. Seleziona **Devices**; il **Device Type** assume il valore predefinito di Bare Metal Server. Fai clic su **Hardware** e seleziona i nomi host dei tuoi dispositivi.
3. Fai clic sul pulsante **Submit**.
4. Controlla lo stato della tua archiviazione di cui è stato eseguito il provisioning nella scheda **Devices** > (seleziona il tuo dispositivo) > **Storage**.
5. Annota il **Target Address** e il nome completo iSCSI (**IQN**) per il tuo server (iniziatore iSCSI) e **username** e **password** per l'autorizzazione presso il server iSCS. Hai bisogno di queste informazioni nei passi successivi.
6. Attieniti alla procedura indicata in [Connessione alle LUN MPIO iSCSI su Linux](https://console.bluemix.net/docs/infrastructure/BlockStorage/accessing_block_storage_linux.html#connecting-to-mpio-iscsi-luns-on-linux) per rendere la tua archiviazione accessibile dal tuo server di cui è stato eseguito il provisioning.

## Come rendere a più percorsi l'archiviazione
{: #multipath}

Nella distribuzione di esempio, hai richiamato i seguenti dati dalla scheda **Storage**:
  * Target IP: `10.2.62.78`
  * IQN: `iqn.2005-05.com.softlayer:SL01SU276540-H896345`
  * User: `SL01SU276540-H896345`
  * Password: `EtJ79F4RA33dXm2q`

1. Immetti quanto segue, sulla base delle informazioni richiamate:
```
[root@sdb192 ~]# cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.2005-05.come.softlayer:SL01SU276540-H986345
```
   Potrebbe essere necessario sostituire una voce esistente in `/etc/iscsi/initiatorname.iscsi`.

2. Aggiungi le seguenti righe in fondo a `/etc/iscsi/iscsid.conf`:
```
[root@sdb192 ~]# tail /etc/iscsi/iscsid.conf
# it continue to respond to R2Ts. To enable this, uncomment this line
# node.session.iscsi.FastAbort = No
node.session.auth.authmethod = CHAP
node.session.auth.username = SL01SU276540-H896345
node.session.auth.password = EtJ79F4RA33dXm2q
discovery.sendtargets.auth.authmethod = CHAP
discovery.sendtargets.auth.username = SL01SU276540-H896345
discovery.sendtargets.auth.password = EtJ79F4RA33dXm2q
```

3. Sostituisci i valori `username` e `password` nel passo 2 con quelli che hai annotato durante il passo 5 di *Autorizzazione host*.

4. Rileva la destinazione iSCSI immettendo le seguenti righe.
```
[root@sdb192 ~]# iscsiadm -m discovery -t sendtargets -p "10.2.62.78"
10.2.62.78:3260,1031 iqn.1992-08.com.netapp:tor0101
10.2.62.87:3260,1032 iqn.1992-08.com.netapp:tor0101
```

5. Imposta l'host per eseguire automaticamente l'accesso all'array iSCSI.

      `[root@sdb192 ~]# iscsiadm -m node -L automatic`

6. Installa e avvia il daemon a più percorsi.
```
[root@sdb192 ~]# yum install device-mapper-multipath
…
[root@sdb192 ~]# chkconfig multipathd on
[root@sdb192 ~]# service multipathd start
```

7. Completa tutti i comandi nel documento relativo al [montaggio dei volumi di Block Storage su Linux](https://console.bluemix.net/docs/infrastructure/BlockStorage/accessing_block_storage_linux.html#mounting-block-storage-volumes) in modo che nell'output a più percorsi sia presente un'altra LUN.
```
[root@sdb192 ~]# multipath -ll
…
3600a098038303452543f464142755a42 dm-9 NETAPP,LUN C-Mode
size=500G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
|-+- policy='round-robin 0' prio=50 status=active
| `- 10:0:0:169 sde 8:64 active ready running
`-+- policy='round-robin 0' prio=10 status=enabled
`- 9:0:0:169  sdf 8:80 active ready running
…`
```

Puoi ora utilizzare il dispositivo a più percorsi come useresti qualsiasi dispositivo disco. Un percorso dispositivo viene visualizzato sotto `/dev/mapper/3600a098038303452543f464142755a42`.

Prendi il `/etc/multipath.conf` di esempio dal [ `multipath.conf`](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-sample.html#sample) di esempio e crealo sul tuo server. Tieni presente che eventuali caratteri speciali, ritorni a capo tipo DOS e voci di avanzamento riga che copi potrebbero causare una modalità di funzionamento imprevista. Assicurati di avere un file Unix ASCII dopo che hai copiato il contenuto.

Adatta il blocco a più percorsi da `/etc/multipath.conf` per creare un alias del percorso per accedere al dispositivo in `1/dev/mapper/mpath1`.

      multipaths {
	       multipath {
		         wwid 3600a098038303452543f464142755a42
		         alias mpath1
	       }
     }

8. Riavvia `multipathd`. Puoi ora creare `/backup filesystem` ed eseguire il montaggio sul dispositivo a blocchi.

      [root@sdb192 ~]# service multipathd restart
      [root@sdb192 ~]# mkfs.ext4 /dev/mapper/mpath1
      [root@sdb192 ~]# mkdir  /backup

9. Controlla i file system su entrambi i server. Il tuo output dovrebbe essere simile a quello di seguito riportato.

        [root@e2e1270 ~]# df -h
        Filesystem		    Size  Used Avail Use% Mounted on
        /dev/sda3             879G  1,5G  833G   1% /
        tmpfs                  16G     0   16G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/sdb2             849G  201M  805G   1% /usr/sap
        db192-priv:/usr/sap/trans
                      165G   59M  157G   1% /usr/sap/trans
                      db192-priv:/sapmnt/C10
                      165G   59M  157G   1% /sapmnt/C10

        [root@sdb192 ~]# df -h
        Filesystem      	    Size  Used Avail Use% Mounted on
        /dev/sda3             549G  2,3G  519G   1% /
        tmpfs                 127G     0  127G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/mapper/mpath1    493G   70M  468G   1% /backup
        /dev/mapper/datavg-datalv
                      1,2T   71M  1,1T   1% /db2
        /dev/mapper/datavg-saplv
                      165G   60M  157G   1% /usr/sap
        /dev/mapper/datavg-sapmntlv
                      165G   60M  157G   1% /sapmnt

Se installi un'applicazione SAP basata su SAP NetWeaver su {{site.data.keyword.Db2_on_Cloud_long}}, devi creare delle sottodirectory in `/backup` che appartengono all'utente amministratore del database (`db2SID`) per i backup completi e i file di log archiviati. Per l'archiviazione automatica dei file di log, devi impostare `LOGMETH1` nel tuo database {{site.data.keyword.Db2_on_Cloud_short}}. Per i dettagli, fai riferimento alla [documentazione di {{site.data.keyword.Db2_on_Cloud_short}}](http://www.ibm.com/support/knowledgecenter/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0051344.html).
