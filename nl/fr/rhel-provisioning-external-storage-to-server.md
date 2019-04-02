---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, database server, deployment

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Ajout d'une mémoire externe à votre serveur
{: #storage}

## Configuration de la mémoire externe
{: #set_up_storage}

Une mémoire externe peut être ajoutée aux serveurs mis à disposition si vous souhaitez l'utiliser comme unité de sauvegarde ou utiliser une image instantanée pour restaurer rapidement votre base de données dans un environnement de test. Dans l'exemple de configuration à trois niveaux, le stockage par blocs est utilisé à la fois pour archiver les fichiers journaux de la base de données et pour les sauvegardes en ligne et hors ligne de la base de données. Le stockage par blocs le plus rapide (4 IOPS par Go) a été sélectionné pour garantir un temps de sauvegarde minimal. Un stockage par blocs plus lent peut être suffisant pour vos besoins. Pour en savoir plus sur {{site.data.keyword.blockstoragefull}}, voir [Initiation au stockage par blocs](/docs/infrastructure/BlockStorage?topic=BlockStorage-GettingStarted#getting-started-with-block-storage).


1. Connectez-vous au [portail client de l'infrastructure {{site.data.keyword.cloud_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){: new_window} avec vos données d'identification uniques.
2. Sélectionnez **Stockage** > **Stockage par blocs.**
3. Cliquez sur **Commander du stockage par blocs** dans le coin supérieur droit de la page Stockage par blocs.
4. Sélectionnez vos besoins de stockage spécifiques. Le tableau 1 contient les valeurs recommandées, notamment 4 IOPS/Go pour une charge de travail de base de données typique.

|              Zone               |      Valeur                                        |
| -------------------------------- | ------------------------------------------------- |
|Emplacement                          | TOR01                                             |
|Facturation                    | Au mois (valeur par défaut)                                 |
|Nouvelle taille de stockage                  | 1000 Go                                           |
|Options d'E-S/s de stockage              | Endurance (E-S/s échelonnées) (par défaut)                 |
|E-S/s échelonnées d'endurance             | 10 Go                                             |
|Taille de l'espace d'instantané               | 0 Go                                              |
|Type de système d'exploitation                           | Linux par défaut                                 |
{: caption="Tableau 1. Valeurs recommandées pour le stockage par blocs" caption-side="top"}

5. Cochez les deux cases et cliquez sur **Valider la commande**.

## Autorisation d'hôtes
{: authorize-host}

1. Cliquez sur **Actions** à la droite de votre numéro d'unité logique et sélectionnez **Hôte autorisé** pour accéder au stockage mis à disposition.
2. Sélectionnez **Unités**. Le **Type d'unité** prend la valeur par défaut Serveur physique. Cliquez sur **Matériel** et sélectionnez les noms d'hôte de vos unités.
3. Cliquez sur le bouton **Soumettre**.
4. Vérifiez l'état de la mémoire mise à disposition sous **Unités** > (sélectionnez votre unité) > onglet **Stockage**.
5. Notez l'**adresse cible** et le nom qualifié iSCSI (**IQN**) de votre serveur (initiateur iSCSI) ainsi que le **nom d'utilisateur** et le **mot de passe** pour l'accès au serveur iSCSI. Vous aurez besoin de ces informations au cours des prochaines étapes.
6. Suivez les instructions décrites dans la section [Connexion à des numéros d'unité logique (LUN) iSCSI sous Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#connecting-to-mpio-iscsi-luns-on-linux) pour rendre votre mémoire accessible depuis votre serveur mis à disposition.

## Accès multiple à la mémoire
{: #multipath}

Dans l'exemple de déploiement, vous avez extrait les données suivantes de l'onglet **Stockage** :
  * IP cible : `10.2.62.78`
  * Nom qualifié iSCSI : `iqn.2005-05.com.softlayer:SL01SU276540-H896345`
  * Utilisateur : `SL01SU276540-H896345`
  * Mot de passe : `EtJ79F4RA33dXm2q`

1. Entrez les données suivantes sur la base des informations extraites :
```
[root@sdb192 ~]# cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.2005-05.come.softlayer:SL01SU276540-H986345
```
   Il est possible qu'une entrée existante ait besoin d'être remplacée dans `/etc/iscsi/initiatorname.iscsi`.

2. Ajoutez les lignes suivantes au bas de `/etc/iscsi/iscsid.conf` :
```
[root@sdb192 ~]# tail /etc/iscsi/iscsid.conf
# it continue to respond to R2Ts. To enable this, uncomment this line
# node.session.iscsi.FastAbort = No
node.session.auth.authmethod = CHAP
node.session.auth.username = SL01SU276540-H896345
node.session.auth.password = EtJ79F4RA33dXm2q
discovery.sendtargets.auth.authmethod = CHAP
discovery.sendtargets.auth.username = SL01SU276540-H896345
discovery.sendtargets.auth.password = EtJ79F4RA33dXm2q
```

3. Remplacez les valeurs `username` et `password` de l'étape 2 par les valeurs annotées à l'étape 5 de la section *Autorisation d'hôtes*.

4. Découvrez la cible iSCSI en entrant les lignes suivantes.
```
[root@sdb192 ~]# iscsiadm -m discovery -t sendtargets -p "10.2.62.78"
10.2.62.78:3260,1031 iqn.1992-08.com.netapp:tor0101
10.2.62.87:3260,1032 iqn.1992-08.com.netapp:tor0101
```

5. Définissez l'hôte pour qu'il se connecte automatiquement à la grappe iSCSI.

      `[root@sdb192 ~]# iscsiadm -m node -L automatic`

6. Installez et démarrez le démon multi-accès.
```
[root@sdb192 ~]# yum install device-mapper-multipath
…
[root@sdb192 ~]# chkconfig multipathd on
[root@sdb192 ~]# service multipathd start
```

7. Suivez toutes les commandes de la section [Connexion à des numéros d'unité logique (LUN) iSCSI sous Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux) pour qu'un autre numéro d'unité logique apparaisse dans la sortie multi-accès.
```
[root@sdb192 ~]# multipath -ll
…
3600a098038303452543f464142755a42 dm-9 NETAPP,LUN C-Mode
size=500G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
|-+- policy='round-robin 0' prio=50 status=active
| `- 10:0:0:169 sde 8:64 active ready running
`-+- policy='round-robin 0' prio=10 status=enabled
`- 9:0:0:169  sdf 8:80 active ready running
…`
```

Vous pouvez maintenant utiliser l'unité multi-accès comme vous utiliseriez n'importe quelle unité de disque. Un chemin d'accès d'unité apparaît sous `/dev/mapper/3600a098038303452543f464142755a42`.

Prenez l'exemple de chemin `/etc/multipath.conf` du fichier exemple [ `multipath.conf`](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-sample) et créez-le sur votre serveur. Notez que les caractères spéciaux, retours chariot de type DOS et sauts de ligne copiés peuvent entraîner des comportements inattendus. Assurez-vous que vous avez un fichier Unix ASCII après avoir copié le contenu.

Adaptez le bloc 'multipath' de `/etc/multipath.conf` pour créer un alias du chemin d'accès pour accéder à l'unité sous `1/dev/mapper/mpath1`.

      multipaths {
	       multipath {
		         wwid 3600a098038303452543f464142755a42
		         alias mpath1
	       }
     }

8. Redémarrez `multipathd`. Vous pouvez maintenant créer le répertoire `/backup filesystem` et procéder au montage sur l'unité par blocs.

      [root@sdb192 ~]# service multipathd restart
      [root@sdb192 ~]# mkfs.ext4 /dev/mapper/mpath1
      [root@sdb192 ~]# mkdir  /backup

9. Vérifiez les systèmes de fichiers sur les deux serveurs. Vous devez obtenir un résultat similaire à la sortie ci-dessous.

        [root@e2e1270 ~]# df -h
        Filesystem		    Size  Used Avail Use% Mounted on
        /dev/sda3             879G  1,5G  833G   1% /
        tmpfs                  16G     0   16G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/sdb2             849G  201M  805G   1% /usr/sap
        db192-priv:/usr/sap/trans
                      165G   59M  157G   1% /usr/sap/trans
                      db192-priv:/sapmnt/C10
                      165G   59M  157G   1% /sapmnt/C10

        [root@sdb192 ~]# df -h
        Filesystem      	    Size  Used Avail Use% Mounted on
        /dev/sda3             549G  2,3G  519G   1% /
        tmpfs                 127G     0  127G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/mapper/mpath1    493G   70M  468G   1% /backup
        /dev/mapper/datavg-datalv
                      1,2T   71M  1,1T   1% /db2
        /dev/mapper/datavg-saplv
                      165G   60M  157G   1% /usr/sap
        /dev/mapper/datavg-sapmntlv
                      165G   60M  157G   1% /sapmnt

Si vous installez une application SAP basée sur NetWeaver sur {{site.data.keyword.Db2_on_Cloud_long}}, vous devez créer des sous-répertoires sous `/backup` appartenant à l'administrateur de base de données (`db2SID`) pour les sauvegardes intégrales et les fichiers journaux archivés. Pour archiver les fichiers journaux de manière automatique, vous devez définir `LOGMETH1` dans votre base de données {{site.data.keyword.Db2_on_Cloud_short}}. Veuillez vous référer à la documentation de [{{site.data.keyword.Db2_on_Cloud_short}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://www.ibm.com/support/knowledgecenter/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0051344.html){: new_window} pour obtenir des détails.
