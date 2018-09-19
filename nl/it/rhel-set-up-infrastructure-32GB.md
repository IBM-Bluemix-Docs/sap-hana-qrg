---



copyright:
  years: 2018
lastupdated: "2018-08-14"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. Ordinazione del tuo server da 32 GB
{: #install_32GB}

## Ordinazione del tuo server
{: #order_32GB}

1. Accedi al [Portale del client dell'infrastruttura {{site.data.keyword.cloud_notm}}](https://control.softlayer.com) con le tue credenziali univoche.
2. Fai clic su **Account** > **Place an Order** nella pagina Account Summary.
3. Fai clic su **Monthly** in {{site.data.keyword.baremetal_short}} nella pagina Devices. Viene visualizzato l'elenco dei server; i server certificati SAP sono all'inizio dell'elenco.
4. Fai clic sul collegamento ipertestuale sotto **STARTING PRICE PER MONTH** per selezionare il server **BI.S3.NW32 (OS Options)**

## Configurazione del tuo server
{: #configure_server}

1. Lascia **1** nel campo **Quantity**.
2. Seleziona **TOR01** per **Data Center**. L'elenco dei data center dipende dalla disponibilità del prodotto in uno specifico data center.
3. **Server** viene automaticamente impostato su un valore predefinito basato sulla tua selezione del server e non è possibile modificarlo.
4. Fai clic su **32 GB RAM** anche se la selezione di **RAM** viene automaticamente impostata su un valore predefinito basato sulla tua selezione del server che non è possibile modificare.
5. Fai clic su **Redhat** e seleziona **Red Hat Enterprise Linux for SAP Business Application 7.X (64 bit)** come tuo **Operating System**. **Nota**: se stai utilizzando BYOL (Bring Your Own License) per il tuo sistema operativo, seleziona **Other** > **No Operating System**. Per ulteriori informazioni, vedi [BYOL (Bring Your Own License)](#byol).
6. Aggiungi una seconda unità SATA da 2 TB facendo clic sul menu a discesa **Disk Controller 1** e selezionando **2 TB SATA**. Fai clic su **Add Disk**.
7. Fai clic su **Select All Disks** e su **Create RAID storage group**.
8. Fai clic su **Type** e seleziona **RAID 1**. Immetti una **Size** che include la quantità totale di archiviazione di cui hai bisogno.
9. Lascia **LVM** non selezionato e accetta il valore predefinito di **Partition Template**, **Linux Basic**.
10. Fai clic su **Done**.

## Selezione delle tue opzioni server aggiuntive
{: #options_32GB}

1. Seleziona **500 GB** per **Public Bandwidth**.
2.	Seleziona **1 Gbps Redundant Public & Private Network Uplinks** per **Uplink Port Speed**.
3. Lascia i valori predefiniti per tutti gli altri campi. Per delle descrizioni delle opzioni dettagliate, consulta [Creazione di un server bare metal personalizzato](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#addl-server-options).
4.	Fai clic su **Add to Order** in fondo alla pagina. Dopo che il tuo ordine è stato verificato, vieni reindirizzato alla pagina Checkout.

## Impostazione di configurazioni di sistema avanzate
{: #adv_config}

Utilizza i valori nella Tabella 1 per i campi in Advanced System Configuration. Ulteriori informazioni sono disponibili nelle linee guida [Opzioni di configurazione del server avanzate](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#adv-system-config).

1. Scorri verso il basso e immetti i valori nella Tabella 1 in **Advanced System Configuration**.

|              Campo               |      Valore                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|Backend VLAN                      | Seleziona dall'elenco a discesa, ad esempio `tor01.bcr01a.1241`     |
|Subnet                            | Seleziona dall'elenco a discesa, ad esempio `10.114.63.64/26`       |
|Frontend VLAN                     | Seleziona dall'elenco a discesa, ad esempio `tor01.fcr01a.851`      |
|Subnet                            | Seleziona dall'elenco a discesa, ad esempio `158.85.65.224/28`      |
|Provision Scripts                 | Lascia vuoto                                                          |
|SSH Keys                          | Il valore predefinito è `Add`, che significa nessuna chiave SSH                            |
|Hostname                          | Ad esempio, `e2e1270`                                               |
|Domain                            | Ad esempio, `saptest.com`                                           |
{: caption="Tabella 1. Valori della configurazione avanzata a 32 GB" caption-side="top"}  

## Conferma delle tue selezioni
{: #confirm_selections}

1. Conferma le tue selezioni nella pagina Checkout e fai clic su **Cloud Service terms** e **3rd Party Software Agreement** sul lato destro della pagina.
2. Fai clic su **Submit Order** sul lato destro del modulo. Vieni reindirizzato a una pagina con il tuo numero di ordine; puoi stampare la pagina perché è anche la tua ricevuta dell'ordine. Inoltre, ricevi una email di conferma con l'oggetto *Your IBM Cloud Order ## has been approved*, dove ## è il tuo numero di ordine.

Dopo che l'ordine è stato inoltrato, il tuo server è, a seconda del tuo ordine, disponibile per l'uso in 1-4 ore. Puoi controllare la schermata Device Details nella pagina del portale dell'infrastruttura principale di (**Devices > Device List**) per uno stato dei passi di provisioning. Fai clic sul **Device Name** che corrisponde ai tuoi specifici nome host e dominio per visualizzarne lo stato.

## BYOL (Bring your own license)
{: #byol}

Quando hai la tua licenza del sistema operativo, installala sul tuo {{site.data.keyword.baremetal_short}} in base alle istruzioni del fornitore. Per ulteriori informazioni, vedi [L'opzione no SO](https://console.bluemix.net/docs/bare-metal/introduction-no-os.html#how-to-install-an-operating-system-on-a-no-os-server-).

## Passi successivi

  [2. Preparazione del tuo server per la tua installazione SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-32GB.html)

  [3. Partizionamento e file system](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-32GB.html)
