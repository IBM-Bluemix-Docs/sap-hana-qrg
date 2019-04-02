---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, ABAP, ASCS Instance, Database Instance, ABAP SAP Central Services, SWPM, application server, database server

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Installazione del tuo ambiente SAP
{: #install_landscape}

## Installazione di pacchetti RPM (prerequisito)
{: #RPM}

Un'istallazione SAP richiede specifici prerequisiti per i pacchetti installati sul sistema operativo e i daemon del sistema operativo in esecuzione. Fai riferimento alle più recenti [guide all'installazione ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://support.sap.com/software/installations.html)}: new_window (è necessario un [ID S-user SAP](/docs/infrastructure/sap-netweaver?topic=sap-netweaver-getting-started#getting-started) e le [note di supporto ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://support.sap.com/notes){: new_window} (richiede un ID S-user SAP) da SAP per un elenco aggiornato di questi prerequisiti. Ci sono altri due pacchetti che devono essere installati:
* compat-sap-c++: normalmente ottiene la compatibilità del runtime C++ con i compilatori utilizzati da SAP. Poiché è stato selezionato Red Hat Enterprise Linux per SAP Business Application 7.X come sistema operativo per il server delle applicazioni da 32 GB e il server di database da 192 GB, utilizzerai `compat-sap-c++-7`.
* uuidd: mantiene il supporto del sistema operativo per la creazione di UUID.

### Verifica dell'installazione di uuidd

1. Controlla se `uuid daemon (uuidd)` è installato. Se non è installato, installalo e avvialo.
```
[root@sdb192 ~]# rpm -qa | grep uuidd
[root@sdb192 ~]# yum install uuidd
[root@sdb192 ~]# chkconfig uuidd on
[root@sdb192 ~]# service uuidd start
```

### Installazione del pacchetto compat-sap-c++-7

1. Attieniti alla [nota SAP 2195019 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://launchpad.support.sap.com/#/notes/2195019){: new_window}, installa il pacchetto compat-sap-c++-7 e crea un soft-link specifico, che è richiesto dai file binari SAP. Controlla le note SAP specifiche della release per il prodotto che stai installando per determinare se la libreria è obbligatoria o meno.
```
[root@sdb192 ~]# yum install compat-sap-c++-7-7.2.1-2.e17_4.x86_64.rpm
....
[root@sdb192 ~]# mkdir -p /usr/sap/lib
[root@sdb192 ~]# ln -s /opt/rh/SAP/lib64/compat-sap-c++.so /usr/sap/lib/libstdc++.so.6
```

## Download del tuo software SAP
{: download_software}

Accedi a [SAP Support Portal ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://support.sap.com/en/index.html){: new_window}, fai clic su **Download Software** e scarica i DVD richiesti su un'unità condivisa locale. Trasferisci i file al tuo server di cui è stato eseguito il provisioning. Un'altra opzione consiste nello scaricare [SAP Software Download Manager ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://support.sap.com/en/my-support/software-downloads.html#section_995042677){: new_window}, installarlo sul tuo server di destinazione e scaricare direttamente le immagini DVD sul server.

## Preparazione della GUI SWPM di SAP
{: #prepare_for_GUI}

A seconda della tua larghezza di banda di rete e della latenza, puoi eseguire la GUI (graphical user interface) SWPM (SAP Software Provisioning Manager) in remoto in una sessione VNC (virtual network computing). Un'altra opzione consiste nell'eseguire la GUI in locale e nello stabilire una connessione a SWPM sulla macchina di destinazione. Consulta la [documentazione di SWPM ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://wiki.scn.sap.com/wiki/display/SL/Software+Provisioning+Manager+1.0+and+2.0){: new_window} se decidi di eseguire la GUI in locale.

La seguente procedura descrive l'esecuzione della GUI SWPM in remoto in una sessione VNC. Questa opzione installa un server VNC, che potrebbe non essere conforme con la protezione avanzata del tuo sistema operativo; assicurati che stai rispettando le tue misure di sicurezza. Fai riferimento alla [documentazione di VNC ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://searchnetworking.techtarget.com/definition/virtual-network-computing){: new_window} per ottenere una panoramica delle sue funzioni, se non hai dimestichezza con esso.

1. Utilizza i seguenti comandi per installare un server VNC.
```
  [root@sdb192 ~]# yum install tigervnc-server

  ...
```

2. Utilizza il seguente comando per installare l'X11 window manager, `twm`, che è incluso nella distribuzione Linux.

`[root@sdb192 ~]# yum install twm`

3. Installa un'emulazione di terminale, ad esempio `xterm`.

 `[root@sdb192 ~]# yum install xterm`

4. Avvia il server VNC dalla riga di comando.

 `[root@sdb192 ~]# vncserver`

Ora ti serve un programma client VNC; sono disponibili molteplici implementazioni per tutti i sistemi operativi e senza alcun costo aggiuntivo. Di norma, hai bisogno che la porta `590X` (dove X è il numero dei server in esecuzione, iniziando da 1) sia accessibile dal tuo client.

Potresti dover avviare una `xterm` dal menu di background di `twm`. Puoi avviare `SWPM` (`sapinst`) dalla `xterm`.

## Installazione del software SAP
{: #install_software}

Dopo aver scaricato il supporto di installazione, attieniti alla procedura di installazione standard di SAP che è documentata nella guida all'installazione di SAP per la versione e i componenti da te utilizzati e le corrispondenti note correlate.

Puoi avviare SAP SWPM dalla `xterm` ed eseguire la procedura di installazione.

### Installazione del software SAP in un ambiente a tre livelli

Attieniti alla procedura in SAP SWPM per una configurazione a tre livelli.

1. Seleziona **Distributed System** ed esegui l'installazione di ASCS e l'installazione del database sul server di database.
2. Installa AS (Application Server) ABAP sul server delle applicazioni. Assicurati di utilizzare gli indirizzi privati per ASCS e i nomi host di database durante l'installazione del server delle applicazioni.

L'utilizzo degli indirizzi privati e dei nomi host garantisce che il traffico di rete tra il server delle applicazioni e ASCS o il database transiti per la rete privata e non per la rete pubblica.
