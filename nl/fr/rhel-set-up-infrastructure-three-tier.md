---



copyright:
  years: 2017, 2018
lastupdated: "2018-08-14"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. Commande des serveurs 192 Go et 32 Go dans une configuration à trois niveaux
{: #install_three_tier}

## Commande de vos serveurs
{: #order_servers}

Suivez les étapes de la section [Commande de votre serveur 32 Go](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-set-up-infrastructure-32GB.html#order_32GB) pour commander le serveur d'applications SAP NetWeaver. Les étapes qui suivent vous guident à travers la commande du serveur de base de données.

## Commande de votre serveur de base de données
{: #order-db-server}

1. Connectez-vous au portail [{{site.data.keyword.cloud_notm}} Infrastructure Customer Portal](https://control.softlayer.com) avec vos données d'identification uniques.
2. Cliquez sur **Compte** > **Passer une commande** sur la page Récapitulatif du compte.
3. Cliquez sur **Au mois** sous Serveurs IBM Bare Metal sur la page Unités. La liste des serveurs s'affiche ; les serveurs certifiés SAP figurent en haut de la liste. 
4. Cliquez sur le lien hypertexte situé sous **PRIX PAR MOIS** pour sélectionner le serveur **BI.S3.NW192 (options du système d'exploitation).**

## Configuration de votre serveur de base de données
{: #options_192GB}

1. Laissez **1** dans la zone **Quantité**.
2. Sélectionnez **TOR01** pour **Centre de données.**
3. Par défaut, la zone **Serveur** affiche une valeur prédéfinie basée sur votre sélection de serveur qui ne peut pas être modifiée.
4. Cliquez sur **192 Go RAM** même si la sélection de **RAM** affiche par défaut une valeur prédéfinie basée sur votre sélection de serveur qui ne peut pas être modifiée.
5. Cliquez sur **Redhat** et sélectionnez **Red Hat Enterprise Linux for SAP Business Application 7.X (64 bits)** comme **Système d'exploitation**. **Remarque** : Si vous apportez votre propre licence (BYOL) pour votre système d'exploitation, sélectionnez **Autre** > **Sans système d'exploitation**. Pour plus d'informations, voir [Apportez votre propre licence](#byol).
6. Ajoutez une deuxième unité SATA de 2 To en cliquant sur le menu déroulant **Contrôleur de disques 1** et en sélectionnant **Disque SATA de 2 To**. Cliquez sur **Ajouter un disque**.
7. Cliquez sur **Sélectionner tous les disques** et cliquez sur **Créer un groupe de stockage RAID**.
8. Cliquez sur **Type** et sélectionnez **RAID 1**. Saisissez une **Taille** couvrant la quantité totale de stockage dont vous avez besoin. 
9. Ne cochez pas la case **LVM** et acceptez les valeurs par défaut **Modèle de partition**, **Linux de base**.
10. Cliquez sur **Terminé**.

## Sélection de vos options de serveur de base de données supplémentaires
{: #addl-server-options}

1. Sélectionnez **500 Go** sous **Bande passante publique.**
2. Sélectionnez **Liaisons montantes publiques redondantes de 1 Gbps** sous **Débits des ports de liaison montante.**
3. Dans cet exemple, laissez les valeurs par défaut pour toutes les autres zones. Vous pouvez consulter [Building a custom bare metal server](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#addl-server-options) pour obtenir une description détaillée des options.
4.	Cliquez sur **Ajouter à la commande** au bas de la page. Une fois votre commande vérifiée, vous serez redirigé vers la page de paiement.

## Définition de configurations système avancées
{: #adv_config}

1. Utilisez les valeurs du tableau 1 dans les zones de Configuration système avancée. Vous trouverez des informations supplémentaires dans les directives fournies sous [Configuration système avancée](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#adv-system-config).

|              Zone               |      Valeur                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|VLAN back end                      | Sélectionnez dans la liste déroulante, par exemple `tor01.bcr01a.1241`     |
|Sous-réseau                            | Sélectionnez dans la liste déroulante, par exemple `10.114.63.64/26`       |
|VLAN front end                     |Sélectionnez dans la liste déroulante, par exemple `tor01.fcr01a.851`      |
|Sous-réseau                            |Sélectionnez dans la liste déroulante, par exemple `158.85.65.224/28`      |
|Scripts de provisionnement                 | Laissez à blanc                                                              |
|Clés SSH                          | Valeur par défaut `Ajouter`, qui correspond à aucune clé SSH                           |
|Nom d'hôte                          |Par exemple, `sdb192`                                                |
|Domaine                            | Par exemple, `saptest.com`                                           |
{: caption="Tableau 1. Valeurs de configuration avancée 192 Go" caption-side="top"}  

## Confirmation de vos sélections
{: #confirm_selections}

1. Confirmez vos sélections sur la page de paiement et cliquez sur **Conditions de services Cloud** et **Contrat de logiciel tiers** sur le côté droit de la page.
2. Cliquez sur **Soumettre commande**. Vous êtes redirigé vers un écran affichant votre numéro de commande. Vous pouvez imprimer cette page, qui vous servira également d'accusé de réception de commande.

Une fois votre commande soumise, le serveur peut être utilisé pendant une durée allant de 1 à 4 heures, suivant votre commande. Vous pouvez vérifier l'écran Détails de l'unité sur la page principale du portail {{site.data.keyword.slportal}} (**Unités > Liste des unités**) pour obtenir l'état des étapes de mise à disposition. Cliquez sur le **Nom de l'unité** correspondant à votre nom d'hôte donné et à votre domaine pour afficher son état.

## Apportez votre propre licence
{: #byol}

Lorsque vous disposez de votre propre licence de système d'exploitation, vous l'installez sur vos serveurs bare metal en suivant les instructions du fournisseur. Pour plus d'informations, voir [The no OS option](https://console.bluemix.net/docs/bare-metal/introduction-no-os.html#how-to-install-an-operating-system-on-a-no-os-server-).

## Etapes suivantes

  [2. Préparation de votre serveur pour votre installation SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-256GB.html)

  [3. Partitionnement et systèmes de fichiers](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-256GB.html)

  [4. Préparation de votre réseau](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
