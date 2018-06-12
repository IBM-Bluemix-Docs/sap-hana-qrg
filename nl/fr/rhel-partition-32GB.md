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

# 3. Partitionnement et systèmes de fichiers
{: #partition_32GB}

Dans l'exemple de configuration à noeud unique, vous avez commandé un serveur avec un disque logique (sur RAID1). Un disque en miroir apparaît dans le système d'exploitation avec un grand système de fichiers racine qui est égal à la taille totale du disque (avec un espace utilisé pour `/boot`). L'agencement du système de fichiers décrit ci-dessous est seulement une approche possible. Pour une utilisation dans un environnement de production, vous pouvez suivre les informations de dimensionnement de votre système, car d'autres structures risquent de mieux répondre à vos besoins ou aux conditions requises par SAP. 

Les trois répertoires requis pour l'installation SAP, `/usr/sap`, `/sapmnt` et `/db2` ont été créés :
```
[root@e2e1270 ~]# mkdir /sapmnt
[root@e2e1270 ~]# mkdir /usr/sap
[root@e2e1270 ~]# mkdir /db2
```
Votre serveur IBM Bare Metal est maintenant prêt pour l'ajout d'une mémoire externe et pour l'installation de vos logiciels et applications SAP.

## Etapes suivantes

  * [Ajout d'une mémoire externe à vos serveurs Bare Metal](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [Installation de vos logiciels et applications SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
