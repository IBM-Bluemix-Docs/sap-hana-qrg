---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, bring your own license, BYOL, VLAN, application server, database server, three-tier, SAP certified servers

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. 192 GB- und 32 GB-Server für eine Konfiguration mit drei Ebenen bestellen
{: #install_three_tier}

## Server bestellen
{: #order_servers}

Führen Sie zum Bestellen des SAP NetWeaver-Anwendungsservers die Schritte unter [32 GB-Server bestellen](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-install_32GB#order_32GB) aus. Die folgenden Schritte führen Sie durch den Bestellprozess für den Datenbankserver.

## Datenbankserver bestellen
{: #order-db-server}

1. Melden Sie sich beim [Kundenportal der {{site.data.keyword.cloud_notm}}-Infrastruktur ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com){: new_window} mit Ihren eindeutigen Berechtigungsnachweisen an.
2. Klicken Sie auf **Konto** > **Bestellen** auf der Kontozusammenfassungsseite.
3. Klicken Sie auf der Seite 'Geräte' unter **{{site.data.keyword.baremetal_long}}** auf den Link **Monatlich**. Die Liste der Server wird angezeigt; die von SAP zertifizierten Server stehen am Listenanfang.
4. Klicken Sie auf den Hyperlink unter **Startpreis pro Monat**, um den Server **BI.S3.NW192 (Betriebssystemoptionen)** auszuwählen.

Der BI.S3.NW32-Server (Betriebssystemoptionen) ist ebenfalls für die Abrechnung auf **Stundenbasis** verfügbar.
{: note}

## Datenbankserver konfigurieren
{: #options_192GB}

1. Belassen Sie den Wert **1** im Feld **Menge**.
2. Wählen Sie **TOR01** als **Rechenzentrum** aus.
3. Das Feld **Server** nimmt basierend auf der Serverauswahl standardmäßig einen Wert ein, der nicht geändert werden kann.
4. Klicken Sie auf **192 GB RAM**, obwohl die Auswahl für **RAM** basierend auf der Serverauswahl standardmäßig einen Wert einnimmt, der nicht geändert werden kann.
5. Klicken Sie auf **Red Hat** und wählen Sie **Red Hat Enterprise Linux for SAP Business Application 7.X (64-Bit)** als **Betriebssystem** aus. **Hinweis**: Wenn Sie eine eigene Lizenz für Ihr Betriebssystem verwenden (BYOL, Bring Your Own License), wählen Sie **Sonstige** > **Kein Betriebssystem** aus. Weitere Informationen finden Sie in [BYOL (Bring Your Own License)](#byol).
6. Fügen Sie ein zweites SATA-Laufwerk mit 2 TB hinzu, indem Sie auf das Dropdown-Menü **Plattencontroller 1** klicken und **SATA (2 TB)** auswählen. Klicken Sie auf **Platte hinzufügen**.
7. Klicken Sie auf **Alle Platten auswählen** und dann auf **RAID-Speichergruppe erstellen**.
8. Klicken Sie auf **Typ** und wählen Sie **RAID 1** aus. Geben Sie einen Wert für die **Größe** ein, der die gesamte benötigte Speichermenge abdeckt.
9. Wählen Sie **LVM** nicht aus und akzeptieren Sie den Standardwert für die **Partitionsvorlage**: **Linux (Basis)**.
10. Klicken Sie auf **Fertig**.

## Zusätzliche Datenbankserveroptionen auswählen
{: #addl-server-options}

1. Wählen Sie **500 GB** als **Öffentliche Bandbreite** aus.
2. Wählen Sie **1 Gb/s redundante öffentliche & private Netzuplinks** für **Uplink-Port-Geschwindigkeit** aus.
3. Belassen Sie für dieses Beispiel die Standardwerte für alle anderen Felder. Detaillierte Beschreibungen der Optionen finden Sie in [Angepassten Bare-Metal-Server erstellen](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#addl-server-options).
4.	Klicken Sie unten auf der Seite auf **Zur Bestellung hinzufügen**. Nachdem Ihre Bestellung geprüft wurde, werden Sie an die Kassenseite weitergeleitet.

## Erweitere Systemkonfigurationen einrichten
{: #adv_config}

1. Verwenden Sie die Werte in Tabelle 1 für die Felder für die erweiterte Systemkonfiguration. Weitere Informationen finden Sie in den Richtlinien zur [erweiterten Systemkonfiguration](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#adv-system-config).

|              Feld               |      Wert                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|Back-End-VLAN                      | Auswahl in Dropdown-Liste, z. B. `tor01.bcr01a.1241`     |
|Teilnetz                            | Auswahl in Dropdown-Liste, z. B. `10.114.63.64/26`       |
|Front-End-VLAN                     | Auswahl in Dropdown-Liste, z. B. `tor01.fcr01a.851`      |
|Teilnetz                            | Auswahl in Dropdown-Liste, z. B. `158.85.65.224/28`      |
|Bereitstellungsscripts                 | Keine Angabe machen                                                          |
|SSH-Schlüssel                          | Standardwert `Add`, d. h. kein SSH-Schlüssel                            |
|Hostname                          | Beispiel: `sdb192`                                                |
|Domäne                            | Beispiel: `saptest.com`                                           |
{: caption="Tabelle 1. Werte für erweiterte 192 GB-Konfiguration" caption-side="top"}  

## Auswahl bestätigen
{: #confirm_selections}

1. Bestätigen Sie Ihre Auswahl auf der Kassenseite und klicken Sie rechts auf der Seite auf **Bedingungen für den Cloud-Service** und **Vereinbarung zu Drittanbietersoftware**.
2. Klicken Sie auf **Bestellung abschicken**. Sie werden an einen Bildschirm mit Ihrer Bestellnummer weitergeleitet. Sie können diese Information ausdrucken, da es sich hierbei auch um den Bestelleingang für Ihre Unterlagen handelt.

Nachdem die Bestellung zur Verarbeitung übergeben wurde, kann der Server je nach Ihrer Bestellung in einer bis vier Stunden verwendet werden. Den Status der Bereitstellungsschritte finden Sie in der Anzeige mit den Gerätedetails auf der Hauptseite des {{site.data.keyword.slportal}}s (**Geräte > Geräteliste**). Klicken Sie auf den **Gerätenamen**, der dem angegebenen Hostnamen und der Domäne entspricht, um den Status des Geräts anzuzeigen.

## BYOL (Bring Your Own License)
{: #byol}

Wenn Sie über eine eigene Betriebssystemlizenz verfügen, installieren Sie sie auf dem {{site.data.keyword.baremetal_short}} anhand der Herstelleranweisungen. Weitere Informationen finden Sie in [Option 'Kein Betriebssystem'](/docs/bare-metal?topic=bare-metal-bm-no-os#bm-no-os).

## Nächste Schritte

  [2. Server für die SAP-Installation vorbereiten](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-prepare_256GB)

  [3. Partitionierung und Dateisysteme](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-3-partitioning-and-file-systems)

  [4. Netz vorbereiten](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-network#network)
