---



copyright:
  years: 2017, 2018
lastupdated: "2018-08-14"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. Ordinazione di server da 192 GB e 32 GB per una configurazione a tre livelli
{: #install_three_tier}

## Ordinazione dei tuoi server
{: #order_servers}

Attieniti alla procedura indicata in [Ordinazione del tuo server da 32 GB](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-set-up-infrastructure-32GB.html#order_32GB) per ordinare il tuo server delle applicazioni SAP NetWeaver. La seguente procedura ti assiste nell'ordinazione del server di database

## Ordinazione del tuo server database
{: #order-db-server}

1. Accedi al [Portale del client dell'infrastruttura {{site.data.keyword.cloud_notm}}](https://control.softlayer.com) con le tue credenziali univoche.
2. Fai clic su **Account** > **Place an Order** nella pagina Account Summary.
3. Fai clic su **Monthly** in **{{site.data.keyword.baremetal_long}}** nella pagina Devices. Viene visualizzato l'elenco dei server; i server certificati SAP sono all'inizio dell'elenco.
4. Fai clic sul collegamento ipertestuale sotto **STARTING PRICE PER MONTH** per selezionare il server **BI.S3.NW192 (OS Options)**

## Configurazione del tuo server database
{: #options_192GB}

1. Lascia **1** nel campo **Quantity**.
2. Seleziona **TOR01** per **Data Center**.
3. **Server** viene automaticamente impostato su un valore predefinito basato sulla tua selezione del server e non è possibile modificarlo.
4. Fai clic su **192 GB RAM** anche se la selezione di **RAM** viene automaticamente impostata su un valore predefinito basato sulla tua selezione del server che non è possibile modificare.
5. Fai clic su **Redhat** e seleziona **Red Hat Enterprise Linux for SAP Business Application 7.X (64 bit)** come tuo **Operating System**. **Nota**: se stai utilizzando BYOL (Bring Your Own License) per il tuo sistema operativo, seleziona **Other** > **No Operating System**. Per ulteriori informazioni, vedi [BYOL (Bring Your Own License)](#byol).
6. Aggiungi una seconda unità SATA da 2 TB facendo clic sul menu a discesa **Disk Controller 1** e selezionando **2 TB SATA**. Fai clic su **Add Disk**.
7. Fai clic su **Select All Disks** e su **Create RAID storage group**.
8. Fai clic su **Type** e seleziona **RAID 1**. Immetti una **Size** che include la quantità totale di archiviazione di cui hai bisogno.
9. Lascia **LVM** non selezionato e accetta il valore predefinito di **Partition Template.**, **Linux Basic**.
10. Fai clic su **Done**.

## Selezione delle tue opzioni server database aggiuntive
{: #addl-server-options}

1. Seleziona **500 GB** per **Public Bandwidth**.
2. Seleziona **1 Gbps Redundant Public & Private Network Uplinks** per **Uplink Port Speed**.
3. Per questo esempio, lascia i valori predefiniti per tutti gli altri campi. Puoi consultare [Creazione di un server bare metal personalizzato](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#addl-server-options) per delle descrizioni dettagliate delle opzioni.
4.	Fai clic su **Add to Order** in fondo alla pagina. Dopo che il tuo ordine è stato verificato, vieni reindirizzato alla pagina Checkout.

## Impostazione di configurazioni di sistema avanzate
{: #adv_config}

1. Utilizza i valori nella Tabella 1 per i campi in Advanced System Configuration. Ulteriori informazioni sono disponibili nelle linee guida [Configurazione di sistema avanzata](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#adv-system-config).

|              Campo               |      Valore                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|Backend VLAN                      | Seleziona dall'elenco a discesa, ad esempio `tor01.bcr01a.1241`     |
|Subnet                            | Seleziona dall'elenco a discesa, ad esempio `10.114.63.64/26`       |
|Frontend VLAN                     | Seleziona dall'elenco a discesa, ad esempio `tor01.fcr01a.851`      |
|Subnet                            | Seleziona dall'elenco a discesa, ad esempio `158.85.65.224/28`      |
|Provision Scripts                 | Lascia vuoto                                                          |
|SSH Keys                          | Il valore predefinito è `Add`, che significa nessuna chiave SSH                            |
|Hostname                          | Ad esempio, `sdb192`                                                |
|Domain                            | Ad esempio, `saptest.com`                                           |
{: caption="Tabella 1. Valori della configurazione avanzata a 192 GB" caption-side="top"}  

## Conferma delle tue selezioni
{: #confirm_selections}

1. Conferma le tue selezioni nella pagina Checkout e fai clic su **Cloud Service terms** e **3rd Party Software Agreement** sul lato destro della pagina.
2. Fai clic su **Submit Order**. Vieni reindirizzato a una schermata con il tuo numero di ordine. Puoi stampare la schermata perché è anche la tua ricevuta dell'ordine.

Una volta inoltrato l'ordine, il server, a seconda del tuo ordine, è disponibile per l'uso entro 1-4 ore. Puoi controllare la schermata Device Details nella pagina principale di {{site.data.keyword.slportal}} (**Devices > Device List**) per uno stato dei passi di provisioning. Fai clic sul **Device Name** che corrisponde ai tuoi specifici nome host e dominio per visualizzarne lo stato.

## BYOL (Bring your own license)
{: #byol}

Quando hai la tua licenza del sistema operativo, installala sul tuo {{site.data.keyword.baremetal_short}} in base alle istruzioni del fornitore. Per ulteriori informazioni, vedi [L'opzione no SO](https://console.bluemix.net/docs/bare-metal/introduction-no-os.html#how-to-install-an-operating-system-on-a-no-os-server-).

## Passi successivi

  [2. Preparazione del tuo server per la tua installazione SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-256GB.html)

  [3. Partizionamento e file system](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-256GB.html)

  [4. Preparazione della tua rete](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
