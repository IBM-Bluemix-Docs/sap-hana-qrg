---



copyright:
  years: 2017, 2018
lastupdated: "2018-08-13"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 4. Préparation de votre réseau pour une configuration à trois niveaux
{: #network}

Si vous prévoyez d'installer une configuration à trois niveaux, le réseau doit être configuré correctement. Dans l'exemple, un serveur de base de données de 192 Go (nommé `sdb192`) et un serveur d'applications de 32 Go (nommé `e2e1270`) ont été déployés. Le serveur de base de données héberge également l'instance (A)SCS. L'ajout des adresses IP sur le réseau privé à votre répertoire `/etc/hosts` est utile pour les étapes suivantes et assure que le trafic de réseau interne SAP passe par le bon réseau.

![Figure 1. Exemple de configuration à trois niveaux](/images/network-01.png "Exemple de configuration à trois niveaux")

Exécutez les étapes suivantes pour établir votre réseau.

1. Connectez-vous aux serveurs et recherchez leur configuration de réseau privé.

        [root@sdb192 ~]# ifconfig bond0
            bond0	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A8
                    inet addr:10.17.139.35  Bcast:10.17.139.63 Mask:255.255.255.192
                    inet6 addr: fe80::ec4:7aff:fe66:2da8/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:128080 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:25491 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:19137850 (18.2 MiB)  TX bytes:3426015 (3.2 MiB)

        [root@sdb192 ~]# ifconfig bond1
            bond1	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A9
                    inet addr:208.43.211.118  Bcast:208.43.211.127 Mask:255.255.255.224
                    inet6 addr: fe80::ec4:7aff:fe66:2da9/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:289610 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:109371 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:31216160 (29.7 MiB)  TX bytes:18591947 (17.7 MiB)

Dans l'exemple à trois niveaux, 10.17.139.35 est l'adresse IP privée du serveur de base de données ; il s'agit de l'une des plages d'adresses IP de RFC 1597. Vous pouvez également déterminer l'adresse IP du serveur d'applications et ajouter les deux adresses IP aux répertoires `/etc/hosts` des deux serveurs.

        [root@sdb192 ~]# cat /etc/hosts
        127.0.0.1 localhost.localdomain localhost
        208.43.211.118 e2e2690.saptest.com e2e2690

        10.17.139.35    sdb192-priv
        10.17.139.46    e2e1270-priv

Ajoutez également les deux dernières lignes sur `e2e1270`.

## Installation du logiciel NFS

1. Installez le logiciel NFS `nfs-utils` **sur les deux serveurs**.

      `[root@sdb192 ~]# yum install nfs-utils`

Assurez-vous de démarrer et d'enregistrer le `rpcbind` et le service NFS sur le serveur de base de données.

        [root@sdb192 ~]# service rpcbind start
        [root@sdb192 ~]# chkconfig nfs on
        [root@sdb192 ~]# service nfs start

## Utilisation de NFS pour l'exportation

1. Utilisez NFS pour exporter `/sapmnt` et `/usr/sap/trans` du serveur de base de données vers le serveur d'applications en ajoutant l'entrée requise au répertoire `/etc/exports` du serveur de base de données.

        /sapmnt/C10		10.17.139.46(rw,no_root_squash,sync,no_subtree_check)
        /usr/sap/trans	10.17.139.46(rw,no_root_squash,sync,no_subtree_check)

L'exemple de valeur `C10` doit être remplacé par l'ID de votre système SAP. Vous devez créer le répertoire avant de l'exporter. Exécutez les commandes suivantes à partir de la ligne de commande du serveur de base de données :

        [root@sdb192 ~]# mkdir /sapmnt/C10
        [root@sdb192 ~]# mkdir -p /usr/sap/trans
        [root@sdb192 ~]# exportfs -a

## Montage du partage NFS

1. Montez le partage NFS sur le serveur d'applications en ajoutant l'entrée suivante à son répertoire `/etc/fstab` et montez-le à partir de la ligne de commande.

        sdb192-priv:/sapmnt/C10 /sapmnt/C10 nfs defaults 0 0
        sdb192-priv:/usr/sap/trans /usr/sap/trans nfs defaults 0 0

2. Créez les répertoires cibles sur le serveur d'applications et montez-les.

        [root@e2e1270 ~]# mkdir -p /sapmnt/C10
        [root@e2e1270 ~]# mkdir /usr/sap/trans
        [root@e2e1270 ~]# mount /sapmnt/C10
        [root@e2e1270 ~]# mount /usr/sap/trans

Vos serveurs sont maintenant préparés pour héberger les composants d'une installation SAP distribuée.

## Etapes suivantes

  * [Ajout d'une mémoire externe à vos serveurs Bare Metal](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [Installation de vos logiciels et applications SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
