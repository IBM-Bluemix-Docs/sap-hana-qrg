---



copyright:
  years: 2018
lastupdated: "2018-02-26"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. 32 GB-Server bestellen
{: #install_32GB}

## Server bestellen
{: #order_32GB}

1. Melden Sie sich beim [Kundenportal der {{site.data.keyword.cloud_notm}}-Infrastruktur](https://control.softlayer.com) mit Ihren eindeutigen Berechtigungsnachweisen an.
2. Klicken Sie auf der Seite mit der Kontozusammenfassung auf **Geräte**.
3. Klicken Sie auf der Seite 'Geräte' unter {{site.data.keyword.baremetal_short}} auf den Link **Monatlich**. Das Dialogfeld 'Serverliste' wird angezeigt.
4. Klicken Sie unter **Startpreis pro Monat** auf den Hyperlink, um den Server **BI.S1.NW32 (OS Options)** auszuwählen.

## Serveroptionen auswählen
{: #options_32GB}

1. Belassen Sie den Wert **1** im Feld **Menge**.
2. Wählen Sie **TOR01** als **Rechenzentrum** aus. Die Liste der Rechenzentren hängt von der Produktverfügbarkeit in einem bestimmten Rechenzentrum ab.
3. Das Feld **Server** nimmt basierend auf der Serverauswahl standardmäßig einen Wert ein, der nicht geändert werden kann.
4. Klicken Sie auf **32 GB RAM**, obwohl die Auswahl für **RAM** basierend auf der Serverauswahl standardmäßig einen Wert einnimmt, der nicht geändert werden kann.
5. Wählen Sie **Red Hat Enterprise Linux for SAP Business Application 6.X** als Ihr **Betriebssystem** aus.
6. Wählen Sie unter **Festplatten** eine zweite 2 TB-SATA-Platte aus, erstellen Sie die RAID-Speichergruppe 'RAID1' von beiden Platten, die den gesamten Speicherplatz abdeckt, und wählen Sie **Linux Basic** als **Partitionsvorlage** aus. Belassen Sie die Einstellung des Kontrollkästchens **LVM** (inaktiviert).
7. Wählen Sie **500 GB** als **Öffentliche Bandbreite** aus.
8.	Wählen Sie **1 Gb/s redundante öffentliche & private Netzuplinks** für **Uplink-Port-Geschwindigkeit** aus.
9. Belassen Sie die Standardwerte für alle anderen Felder. Eine detaillierte Beschreibung der Optionen finden Sie unter [Bare-Metal-Server einrichten](https://console.bluemix.net/docs/bare-metal/configuring.html#setting-up-your-bare-metal-servers).
10.	Klicken Sie unten auf der Seite auf **Zur Bestellung hinzufügen**. Nachdem Ihre Bestellung geprüft wurde, werden Sie an die Kassenseite weitergeleitet.

## Erweitere Systemkonfigurationen einrichten
{: #adv_config}

Verwenden Sie die Werte in Tabelle 1 für die Felder für die erweiterte Systemkonfiguration. Weitere Informationen finden Sie in den Richtlinien zur [erweiterten Systemkonfiguration](https://console.bluemix.net/docs/bare-metal/configuring.html#advanced-system-configuration).

1. Blättern Sie nach unten und geben Sie die Werte in Tabelle 1 unter **Erweiterte Systemkonfiguration** ein.

|              Feld               |      Wert                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|Back-End-VLAN                     | Auswahl aus der Dropdown-Liste, beispielsweise `tor01.bcr01a.1241`     |
|Teilnetz                          | Auswahl aus der Dropdown-Liste, beispielsweise `10.114.63.64/26`       |
|Front-End-VLAN                    | Auswahl aus der Dropdown-Liste, beispielsweise `tor01.fcr01a.1199`     |
|Teilnetz                          | Auswahl aus der Dropdown-Liste, beispielsweise `169.55.137.160/27`     |
|Bereitstellungsscripts            | Nimmt standardmäßig den Wert 'Keine' an                                                     |
|SSH-Schlüssel                     | Hinzufügen                                                                  |
|Hostname                          | Beispiel: `e2e1270`                                               |
|Domäne                            | Beispiel: `saptest.com`                                           |
{: caption="Tabelle 1. Werte für erweiterte 32 GB-Konfiguration" caption-side="top"}  

## Auswahl bestätigen
{: #confirm_selections}

1. Bestätigen Sie Ihre Auswahl auf der Kassenseite und klicken Sie rechts auf der Seite auf **Bedingungen für den Cloud-Service** und **Vereinbarung zu Drittanbietersoftware**.
2. Klicken Sie rechts im Formular auf **Bestellung abschicken**. Sie werden an eine Seite mit Ihrer Bestellnummer weitergeleitet. Sie können diese Information ausdrucken, da es sich hierbei auch um den Bestelleingang für Ihre Unterlagen handelt. Außerdem erhalten Sie eine Bestätigungs-E-Mail mit dem Betreff '*Ihre IBM Cloud-Bestellung ## wurde genehmigt*, wobei ## für Ihre Bestellnummer steht.

Nachdem die Bestellung zur Verarbeitung übergeben wurde, kann der Server je nach Ihrer Bestellung in einer bis vier Stunden verwendet werden. Den Status der Bereitstellungsschritte finden Sie in der Anzeige mit den Gerätedetails auf der Hauptseite des Kundenportals (**Geräte > Geräteliste**). Klicken Sie auf den **Gerätenamen**, der dem angegebenen Hostnamen und der Domäne entspricht, um den Status des Geräts anzuzeigen.

## Nächste Schritte
 
  [2. Server für die SAP-Installation vorbereiten](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-32GB.html)
  
  [3. Partitionierung und Dateisysteme](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-32GB.html)
