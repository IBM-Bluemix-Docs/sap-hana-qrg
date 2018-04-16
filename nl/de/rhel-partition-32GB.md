---



copyright:
  years: 2017, 2018
lastupdated: "2018-02-21"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. Partitionierung und Dateisysteme
{: #partition_32GB}

Für das Beispiel mit einem Knoten haben Sie einen Server mit einer logischen Platte (auf RAID 1) bestellt. Es ist eine gespiegelte Platte im Betriebssystem enthalten; dabei gibt es ein großes Stammdateisystem, das der Gesamtplattengröße (mit etwas Speicherplatz, der für `/boot`) entspricht. Bei dem folgenden Dateisystemlayout handelt es sich nur um einen möglichen Ansatz. Für Produktionszwecke sollten Sie jedoch eher die Dimensionierungsinformationen für Ihr System verwenden, da andere Layouts möglicherweise besser Ihren Bedürfnissen oder SAP-Anforderungen entsprechen. 

Die drei erforderlichen Verzeichnisse `/usr/sap`, `/sapmnt` und `/db2` für die SAP-Installation wurden erstellt:
```
[root@e2e1270 ~]# mkdir /sapmnt
[root@e2e1270 ~]# mkdir /usr/sap
[root@e2e1270 ~]# mkdir /db2
```
Die {{site.data.keyword.baremetal_long}}-Instanz ist nun für die Verwendung von externem Speicher und die Installation von SAP-Anwendungen und -Software bereit.

## Nächste Schritte

  * [Externen Speicher zur {{site.data.keyword.baremetal_short}}-Instanz hinzufügen](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [SAP-Anwendungen und -Software installieren](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
