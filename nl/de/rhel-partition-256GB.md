---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, application server, database server, three-tier

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. Partitionierung und Dateisysteme

Für das Beispiel mit drei Ebenen wurde ein 192 GB-Server (Datenbankserver) mit einer logischen Platte (auf einem RAID 10) und ein 32 GB-Server (Anwendungsserver) mit einer logischen Platte (auf einem RAID 1) bestellt. Beide Server verfügen über ein großes Stammdateisystem, das der Gesamtplattengröße (mit etwas Speicherplatz, der für `/boot` verwendet wird) entspricht.

Erstellen Sie das `/usr/sap/`-Dateisystem für den 32 GB-Server. Beachten Sie, dass `sapmnt1` und `/usr/sap/trans` auf dem Datenbankserver erstellt werden. Das Netzdateisystem (NFS) wird vom Datenbankserver exportiert, auf dem auch die Instanz von Advanced Business Application Programming (ABAP) SAP Central Service [(A)SCS] bereitgestellt wird.

Bei dem folgenden Dateisystemlayout handelt es sich nur um einen möglichen Ansatz. Für Produktionszwecke sollten Sie jedoch eher die Dimensionierungsinformationen für Ihr System verwenden, da andere Layouts möglicherweise besser Ihren Bedürfnissen oder SAP-Anforderungen entsprechen, oder aber mit Kontingenten arbeiten.

Die erforderlichen Verzeichnisse `/sapmnt`, `/usr/sap` und `/db2` zum Installieren der SAP-Software müssen mithilfe der folgenden Befehle erstellt werden:
```
[root@ sdb192 ~]# mkdir /sapmnt
[root@ sdb192 ~]# mkdir /usr/sap
[root@ sdb192 ~]# mkdir /db2
```

## Nächste Schritte

[4. Netz vorbereiten](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-network#network)
