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

# 2. Infrastruktur einrichten
{: #set_up_infrastructure}

Anweisungen zur Konfiguration von {{site.data.keyword.baremetal_long}}-Instanzen, Netz, Sicherheit, Speicher und Betriebssystem für SAP NetWeaver finden sich im folgenden Abschnitt. Bei Fragen wenden Sie sich an den [{{site.data.keyword.cloud_notm}}-Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).

## Server bestellen
{: order-server}

Führen Sie die folgenden Schritte für die Bestellung Ihrer {{site.data.keyword.baremetal_short}}-Instanz durch. Weitere Informationen finden Sie in [Angepassten Bare-Metal-Server erstellen](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server).

1. Melden Sie sich beim [Kundenportal der {{site.data.keyword.cloud_notm}}-Infrastruktur ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com){: new_window} mit Ihren eindeutigen Berechtigungsnachweisen an.
2. Klicken Sie auf **Konto** > **Bestellen** auf der Kontozusammenfassungsseite.
3. Klicken Sie auf der Seite 'Geräte' unter {{site.data.keyword.baremetal_short}} auf den Link **Monatlich**. Daraufhin wird das Dialogfenster mit der Liste der Server angezeigt; die von SAP zertifizierten Server stehen am Listenanfang.
4. Klicken Sie auf den Hyperlink unter **Startpreis pro Monat**, um den entsprechenden Server auszuwählen und zur Seite für Konfiguration und Bestellung weitergeleitet zu werden.

Der BI.S3.NW32-Server (Betriebssystemoptionen) ist ebenfalls für die Abrechnung auf **Stundenbasis** verfügbar.
{: note}

   SAP NetWeaver-zertifizierte Server sind mit einem **-NW** unter dem CPU-Modell gekennzeichnet. Die Konfiguration für Red Hat-basierte Server ist in Schritt 7 und Schritt 1 unter [Serveroptionen auswählen](#select_options); beschrieben; diese Schritte gelten auch für Microsoft Windows. Beachten Sie, dass für VMWare die Auswahl zwar begrenzt ist, die Schritte jedoch gleich sind.

   Im Folgenden sehen Sie ein Beispiel für die Entschlüsselung der SAP NetWeaver-Servernamen.

| Servername | Komponente zur Namenskonvention | Bedeutung |
| --- | --- | --- |
| BI.S3.NW768 | BI | Bluemix-Schnittstelle |
| | S3 | Series 2 (Prozessorgeneration) |
| | | S1 ist Ivy Bridge/Haswell |
| | | S2 ist Broadwell |
| | | S3 ist Skylake/Kaby Lake |
| | NW | NetWeaver-zertifizierter Server |
| | 768 | Menge an RAM |

5. Geben Sie im Feld **Qualität** die Anzahl zu bestellender Server ein und wählen Sie Ihr **Rechenzentrum** aus.
6. Die Standardwerte für **Server**, **RAM** und Ihre Option für privaten Speicher basieren auf Ihrer Serverauswahl und können nicht geändert werden. {{site.data.keyword.IBM_notm}} {{site.data.keyword.blockstorageshort}} für {{site.data.keyword.cloud_notm}} oder {{site.data.keyword.filestorage_full_notm}} werden nach der Bestellung Ihres Servers bestellt.
7. Wählen Sie Ihr **Betriebssystem** aus, entweder Microsoft, Red Hat oder SUSE, und wählen Sie das jeweilige Betriebssystem oder den VMware-Hypervisor für Ihren Server aus.

Wenn Sie eine eigene Lizenz für Ihr Betriebssystem verwenden (BYOL, Bring Your Own License), wählen Sie **Sonstige** > **Kein Betriebssystem** aus. Weitere Informationen finden Sie in [BYOL (Bring Your Own License)](#byol).
{: note}

## Serveroptionen auswählen
{: #select_options}

Im nächsten Schritt wählen Sie den Typ und die Anzahl der Platten aus, die zu Ihrer Konfiguration hinzugefügt werden sollen. Außerdem können Sie verschiedene Optionen für RAID-Speichergruppen (Redundant Array of Independent Disks) und Partitionslayouts über den RAID-Speichergruppen auswählen. Weitere Informationen erhalten Sie unter [Informationen zu RAID](/docs/bare-metal?topic=bare-metal-about-raid#about-raid) oder beim [{{site.data.keyword.cloud_notm}}-Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).

1. Treffen Sie unter **SAP-zertifizierte Server** Ihre Auswahl auf Grundlage des geplanten Verwendungszwecks für Ihren Server. Unterstützung erhalten Sie über das [Design Decision Tool ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-cloud-architecture/infrastructure-design-decision-tool){: new_window} (blättern Sie zu dem Tool nach unten) oder über den [{{site.data.keyword.cloud_notm}}-Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
2. Klicken Sie unten auf der Seite auf die Schaltfläche **Zur Bestellung hinzufügen**.

## Erweitere Systemkonfigurationen einrichten
{: #adv_config}

1. Befolgen Sie die Anweisungen unter [Erweitere Systemkonfiguration](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server) als Richtlinie für die Werte im Fenster **Erweiterte Systemkonfiguration**.

SAP-Hostnamen müssen aus mindestens 13 alphanumerischen Zeichen bestehen. Weitere Informationen zu SAP-Hostnamen finden Sie in den [SAP Notes 611361 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://launchpad.support.sap.com/#/611361){: new_window} und [129997 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://launchpad.support.sap.com/#/129997){: new_window}.

Wenn Sie sich für ein öffentliches/privates Setup für die Umgebung entschieden haben und Folgendes planen, müssen Sie sicherstellen, dass die Namensauflösung die internen und externen Adressen widerspiegelt:
  * Installation mehrerer SAP-Systeme, die miteinander kommunizieren müssen *oder*
  * Auswahl eines dreischichtigen SAP-Setups (Datenbank- und Anwendungsinstanzen auf unterschiedlicher Hardware)

Die externe Adresse entspricht dem Hostnamen des Servers, aufgelöst durch den vollständig qualifizierten Domänennamen (FQDN, Fully Qualified Domain Name).

Die internen Adressen erscheinen nicht im Domain Name System (DNS). Da interne IPs für die Kommunikation zwischen den Servern verwendet werden sollten, stellen Sie sicher, dass Sie die Datei `/etc/hosts` oder entsprechend die Microsoft Windows-Datei "host" erweitern. Diese Informationen können auch für das Gastbetriebssystem der VMware ESXi-basierten Bereitstellungen gelten.

## Auswahl bestätigen
{: #confirm_selections}

1. Bestätigen Sie Ihre Auswahl auf der Kassenseite und aktivieren Sie die Kontrollkästchen für **Bedingungen für den Cloud-Service** und **Vereinbarung zu Drittanbietersoftware**.
2. Blättern Sie nach unten zu 'Konto erstellen: Rechnungsdetails eingeben' und geben Sie **Zahlungsart, Name, Kartentyp** und das **Ablaufdatum** für die Kreditkarte an, die für die Rechnungsstellung verwendet werden soll.
3. Klicken Sie auf die Schaltfläche **Bestellung abschicken**. Sie werden an einen Bildschirm mit Ihrer Bestellnummer weitergeleitet. Sie können diese Information ausdrucken, da es sich hierbei auch um den Bestelleingang für Ihre Unterlagen handelt.

Es wird eine Bestätigungs-E-Mail mit dem Betreff 'Ihre _{{site.data.keyword.cloud_notm}}-Bestellung ## wurde genehmigt_' an die E-Mail-Adresse in Ihrem Profil gesendet. Diese E-Mail ist eine Benachrichtigung darüber, dass Ihr Server genehmigt wurde und demnächst bereitgestellt wird. Nach der Bereitstellung erhalten Sie eine weitere Benachrichtigung darüber, dass der Server verfügbar ist und über das [Kundenportal für die {{site.data.keyword.cloud_notm}}-Infrastruktur ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com){: new_window} verwaltet werden kann.

## BYOL (Bring Your Own License)
{: #byol}

Wenn Sie über eine eigene Betriebssystemlizenz verfügen, installieren Sie sie auf dem {{site.data.keyword.baremetal_short}} anhand der Herstelleranweisungen. Weitere Informationen finden Sie in [Option 'Kein Betriebssystem'](/docs/bare-metal?topic=bare-metal-bm-no-os#bm-no-os).

## Nächste Schritte

Sie können nun mit der Verwaltung der {{site.data.keyword.baremetal_short}}-Instanzen beginnen. Informationen zu den nächsten Schritten finden Sie unter [SAP NetWeaver-Umgebung verwalten](/docs/infrastructure/sap-netweaver?topic=sap-netweaver-manage_environment#manage_environment).
