---



copyright:
  years: 2017, 2018
lastupdated: "2018-02-26"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. Ordinazione di server da 256 GB e 32 GB per una configurazione a tre livelli
{: #install_three_tier}

## Ordinazione dei tuoi server
{: #order_servers}

Attieniti alla procedura indicata in [Ordinazione del tuo server da 32 GB](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-set-up-infrastructure-32GB.html#order_32GB) per ordinare il tuo server delle applicazioni SAP NetWeaver. La seguente procedura ti assiste nell'ordinazione del server di database

1. Accedi al [Portale del client dell'infrastruttura {{site.data.keyword.cloud_notm}}](https://control.softlayer.com) con le tue credenziali univoche.
2. Fai clic sull'icona **Devices** nella pagina Account Summary.
3. Fai clic su **Monthly** in **{{site.data.keyword.baremetal_long}}** nella pagina Devices. Viene visualizzata la casella di dialogo Server List.
4. Fai clic sul collegamento ipertestuale sotto **STARTING PRICE PER MONTH** per selezionare il server **BI.S1.NW256 (OS Options)**

## Selezione delle tue opzioni server
{: #options_32GB}

1. Lascia **1** nel campo **Quantity**.
2. Seleziona **TOR01** per **Data Center**.
3. **Server** viene automaticamente impostato su un valore predefinito basato sulla tua selezione del server e non è possibile modificarlo.
4. Fai clic su **32 GB RAM** anche se la selezione di **RAM** viene automaticamente impostata su un valore predefinito basato sulla tua selezione del server che non è possibile modificare.
5. Seleziona **Red Hat Enterprise Linux for SAP Business Application 6.X** come tuo Operating System.
6. Sotto **Hard Drives**, seleziona un secondo disco SATA da 2 TB, crea un gruppo di archiviazione RAID di RAID1 da entrambi i dischi che copre la quantità totale di archiviazione e seleziona **Linux Basic** come **Partition Template**. Lascia **LVM** deselezionato.
7. Seleziona **500 GB** per **Public Bandwidth**.
8. Seleziona **1 Gbps Redundant Public & Private Network Uplinks** per **Uplink Port Speed**.
9. Per questo esempio, lascia i valori predefiniti per tutti gli altri campi. Puoi consultare [Configurazione dei tuoi server bare metal](https://console.bluemix.net/docs/bare-metal/configuring.html#setting-up-your-bare-metal-servers) per delle descrizioni dettagliate delle opzioni.
10.	Fai clic su **Add to Order** in fondo alla pagina. Dopo che il tuo ordine è stato verificato, vieni reindirizzato alla pagina Checkout.

## Impostazione di configurazioni di sistema avanzate
{: #adv_config}

1. Utilizza i valori nella Tabella 1 per i campi in Advanced System Configuration. Ulteriori informazioni sono disponibili nelle linee guida [Configurazione di sistema avanzata](https://console.bluemix.net/docs/bare-metal/configuring.html#advanced-system-configuration).

|              Campo               |      Valore                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|Backend VLAN                      | Seleziona dall'elenco a discesa, ad esempio `tor01.bcr01a.1241`     |
|Subnet                            | Seleziona dall'elenco a discesa, ad esempio `10.114.63.64/26`       |
|Frontend VLAN                     | Seleziona dall'elenco a discesa, ad esempio `tor01.fcr01a.1199`     |
|Subnet                            | Seleziona dall'elenco a discesa, ad esempio `169.55.137.160/27`     |
|Provision Scripts                 | Il valore predefinito è None                                                         |
|SSH Keys                          | Il valore predefinito è None                                                         |
|Hostname                          | Ad esempio, `e2e2690`                                               |
|Domain                            | Ad esempio, `saptest.com`                                           |
{: caption="Tabella 1. Valori di configurazione avanzata" caption-side="top"}  

## Conferma delle tue selezioni
{: #confirm_selections}

1. Conferma le tue selezioni nella pagina Checkout e fai clic su **Cloud Service terms** e **3rd Party Software Agreement** sul lato destro della pagina.
2. Fai clic su **Submit Order**. Vieni reindirizzato a una schermata con il tuo numero di ordine. Puoi stampare la schermata perché è anche la tua ricevuta dell'ordine.

Una volta inoltrato l'ordine, il server, a seconda del tuo ordine, è disponibile per l'uso entro 1-4 ore. Puoi controllare la schermata Device Details nella pagina principale di {{site.data.keyword.slportal}} (**Devices > Device List**) per uno stato dei passi di provisioning. Fai clic sul **Device Name** che corrisponde ai tuoi specifici nome host e dominio per visualizzarne lo stato.

## Passi successivi

  [2. Preparazione del tuo server per la tua installazione SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-256GB.html)
  
  [3. Partizionamento e filesystem](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-256GB.html)
  
  [4. Preparazione della tua rete](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
