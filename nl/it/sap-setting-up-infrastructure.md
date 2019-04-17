---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, {{site.data.keyword.cloud_notm}}, deployment, BYOL

subcollection: sap-netweaver

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# 2. Configurazione della tua infrastruttura
{: #set_up_infrastructure}

La seguente sezione contiene le indicazioni per configurare i tuoi {{site.data.keyword.baremetal_long}}, rete, sicurezza, archiviazione e sistema operativo di SAP NetWeaver. Per eventuali domande, rivolgiti al [supporto di {{site.data.keyword.cloud_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).

## Ordinazione del tuo server
{: order-server}

Utilizza la seguente procedura per ordinare il tuo {{site.data.keyword.baremetal_short}}. Ulteriori informazioni sono disponibili in [Creazione di un server bare metal personalizzato](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server).

1. Accedi al [Portale del client dell'infrastruttura {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com){: new_window} con le tue credenziali univoche.
2. Fai clic su **Account** > **Place an Order** nella pagina Account Summary.
3. Fai clic sul link **Monthly** sotto {{site.data.keyword.baremetal_short}} nella pagina Devices. Viene visualizzata la casella di dialogo dell'elenco dei server; i server certificati SAP sono all'inizio dell'elenco.
4. Fai clic sul collegamento ipertestuale sotto **STARTING PRICE PER MONTH** per selezionare il server appropriato e andare alla pagina Configure/Order.

Il server BI.S3.NW32 (OS Options) è disponibile anche per la fatturazione **Hourly**.
{: note}

   I server con certificazione SAP NetWeaver sono identificati con **-NW** sotto CPU Model. La configurazione server basata su Red Hat è descritta nel passo 7 e nel passo 1 in [Selezione delle tue opzioni server](#select_options); la procedura è la stessa per Microsoft Windows. Nota: per VMWare, la tua scelta è limitata; tuttavia, la procedura è la stessa.

   Questo è un esempio di come decifrare i nomi server SAP NetWeaver.

| Nome server | Convenzione di denominazione del componente | Significato |
| --- | --- | --- |
| BI.S3.NW768 | BI | Interfaccia Bluemix |
| | S3 | Series 2 (generazione processore) |
| | | S1 è Ivy Bridge/Haswell |
| | | S2 è Broadwell |
| | | S3 è Skylake/Kaby Lake |
| | NW | Server certificato NetWeaver |
| | 768 | Quantità di RAM |

5. Immetti il numero di server che stai ordinando nel campo **Quality** e seleziona il tuo **Data Center**.
6. **Server**, **RAM** e la tua opzione di archiviazione privata sono impostati su valori predefiniti in base alla tua selezione del server e non possono essere modificati. {{site.data.keyword.IBM_notm}} {{site.data.keyword.blockstorageshort}} per {{site.data.keyword.cloud_notm}} o {{site.data.keyword.filestorage_full_notm}} vengono ordinati dopo che hai ordinato il server.
7. Seleziona il tuo sistema operativo (**Operating System**) da Microsoft, Red Hat o SUSE e seleziona il sistema operativo specifico, oppure l'hypervisor VMWare per il tuo server. 

Se stai utilizzando BYOL (Bring Your Own License) per il tuo sistema operativo, seleziona **Other** > **No Operating System**. Per ulteriori informazioni, vedi [BYOL (Bring Your Own License)](#byol).
{: note}

## Selezione delle tue opzioni server
{: #select_options}

Nel passo successivo, selezionerai il tipo e il numero di dischi che vuoi aggiungere alla tua configurazione. Puoi inoltre selezionare opzioni differenti per i gruppi di archiviazione RAID (Redundant Array of Independent Disks) e per i layout di partizionamento con i gruppi di archiviazione RAID. Per ulteriori informazioni, consulta [Informazioni su RAID](/docs/bare-metal?topic=bare-metal-about-raid#about-raid) o [Supporto di {{site.data.keyword.cloud_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).

1. In **SAP Certified Servers**, effettua la tua selezione in base a come pianifichi di utilizzare il tuo server. Per ulteriori informazioni, vedi [Design Decision Tool ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-cloud-architecture/infrastructure-design-decision-tool){: new_window} (scorri in basso nello strumento) o il [Supporto di {{site.data.keyword.cloud_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support) 
2. Fai clic sul pulsante **Add to order** in fondo alla pagina.

## Impostazione di configurazioni di sistema avanzate
{: #adv_config}

1. Attieniti alle linee guida di [Configurazione di sistema avanzata](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server) per un aiuto con i valori nella finestra **Advanced System Configuration**.

I nomi host SAP devono essere formati da un massimo di 13 caratteri alfanumerici. Vedi le [note SAP 611361 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://launchpad.support.sap.com/#/611361){: new_window} e [129997 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://launchpad.support.sap.com/#/129997){: new_window} per ulteriori dettagli sui nomi host SAP.

Se hai deciso in merito a una configurazione pubblica/privata per il tuo ambiente e intendi
  * Installare più sistemi SAP che devono comunicare tra di loro *oppure*
  * Scegliere una configurazione SAP a tre livelli (istanze database e applicazione su hardware differente)

Assicurati che la tua risoluzione dei nomi rifletta gli indirizzi interni ed esterni. L'indirizzo esterno corrisponde al nome host del server risolto in base al suo nome dominio completo (FDQN, fully-qualified domain name).

Gli indirizzi interni non appariranno nel DNS (domain name system). Poiché si devono utilizzare gli IP interni per le comunicazioni tra i server, assicurati di estendere il tuo file `/etc/hosts` oppure "host" di Microsoft Windows ci conseguenza. Queste informazioni possono essere valide anche per il sistema operativo guest delle tue distribuzioni basate su VMware ESXi.

## Conferma delle tue selezioni
{: #confirm_selections}

1. Conferma le tue selezioni nella pagina Checkout e fai clic sulle caselle di spunta **Cloud Service terms** e **3rd Party Software Agreement**.
2. Scorri verso il basso fino a Create Account; accedi a Billing e immetti i valori per **Payment Type, Name, Card** e **Expiration** (data) per la carta di credito da utilizzare per la fatturazione.
3. Fai clic sul pulsante **Submit Order**. Vieni reindirizzato a una schermata con il tuo numero di ordine. Puoi stampare la schermata perché è anche la tua ricevuta dell'ordine.

Una email di conferma con l'oggetto Your _{{site.data.keyword.cloud_notm}} Order ## has been approved_ viene inviata all'indirizzo email nel tuo profilo. Questa email è l'avviso che il tuo server è stato approvato ed è in fase di distribuzione. Dopo che è stato distribuito, viene inviato un altro avviso che ti avverte che il server è disponibile e può essere gestito tramite il [portale del cliente dell'infrastruttura {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com){: new_window}.

## BYOL (Bring your own license)
{: #byol}

Quando hai la tua licenza del sistema operativo, installala sul tuo {site.data.keyword.baremetal_short} in base alle istruzioni del fornitore. Per ulteriori informazioni, vedi [L'opzione no SO](/docs/bare-metal?topic=bare-metal-bm-no-os#bm-no-os).

## Passi successivi

Sei ora pronto per iniziare a gestire il tuo {{site.data.keyword.baremetal_short}}. Vedi [Gestione del tuo landscape SAP NetWeaver](/docs/infrastructure/sap-netweaver?topic=sap-netweaver-manage_environment#manage_environment) per i tuoi passi successivi.
