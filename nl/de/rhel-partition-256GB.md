---



copyright:
  years: 2017, 2018
lastupdated: "2017-02-21"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. Partitionierung und Dateisysteme

Für das dreischichtige Beispiel wurde ein 256 GB-Server (Datenbankserver) mit einer logischen Platte (auf einem RAID 10) und ein 32 GB-Server (Anwendungsserver) mit einer logischen Platte (auf einem RAID 1) bestellt. Beide Server verfügen über ein großes Stammdateisystem, das der Gesamtplattengröße (mit etwas Speicherplatz, der für `/boot` verwendet wird) entspricht.

Führen Sie für den 32 GB-Server die Schritte 1-10 unter 'Server für die SAP-Installation vorbereiten (32 GB)' aus. Fahren Sie dann mit den Schritten 11-14 fort; ersetzen Sie dabei jedoch /`db2` durch `/usr/sap/`. `sapmnt1` und `/usr/sap/trans` werden erstellt und das Netzdateisystem (NFS) vom Datenbankserver exportiert, das auch die Advanced Business Application Programming (ABAP) SAP Central Service [(A)SCS]-Instanz umfasst.

Bei dem folgenden Dateisystemlayout handelt es sich nur um einen möglichen Ansatz. Für Produktionszwecke sollten Sie jedoch eher die Dimensionierungsinformationen für Ihr System verwenden, da andere Layouts möglicherweise besser Ihren Bedürfnissen oder SAP-Anforderungen entsprechen, oder aber mit Kontingenten arbeiten.
Die erforderlichen Verzeichnisse `/sapmnt`, `/usr/sap` und `/db2` zum Installieren der SAP-Software müssen mithilfe der folgenden Befehle erstellt werden:
```
[root@ e2e2690 ~]# mkdir /sapmnt
[root@ e2e2690 ~]# mkdir /usr/sap
[root@ e2e2690 ~]# mkdir /db2
```
 
## Nächste Schritte

[4. Netz vorbereiten](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
