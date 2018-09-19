---



copyright:
  years: 2017, 2018
lastupdated: "2017-08-13"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. Partitionnement et systèmes de fichiers

Dans l'exemple de configuration à trois niveaux, un serveur 192 Go (serveur de base de données) avec un disque logique (sur RAID10) et un serveur 32 Go (serveur d'applications) avec un disque logique (sur RAID 1) ont été commandés. Les deux serveurs sont fournis avec un grand système de fichiers racine qui est égal à la taille totale du disque (avec un espace utilisé pour `/boot`).

Pour le serveur 32 Go, créez le système de fichiers `/usr/sap/`. Notez que `sapmnt1` et `/usr/sap/trans` sont créés sur le serveur de base de données. Le système de fichiers réseau (NFS) est exporté du serveur de base de données, qui héberge également l'instance SCS (SAP Central Service) ABAP (Advanced Business Application Programming) [(A)SCS].

L'agencement du système de fichiers ci-dessous est une approche possible. Pour une utilisation dans un environnement de production, vous pouvez suivre les informations de dimensionnement de votre système, car d'autres structures risquent de mieux répondre à vos besoins ou aux conditions requises par SAP. Vous pouvez également utiliser des quotas.

Les répertoires requis pour installer le logiciel SAP, `/sapmnt`, `/usr/sap` et `/db2`, doivent être créés à l'aide des commandes suivantes :
```
[root@ sdb192 ~]# mkdir /sapmnt
[root@ sdb192 ~]# mkdir /usr/sap
[root@ sdb192 ~]# mkdir /db2
```

## Etapes suivantes

[4. Préparation de votre réseau](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
