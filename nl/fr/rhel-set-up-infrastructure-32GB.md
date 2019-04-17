---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, bring your own license, BYOL, VLAN

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. Commande du serveur 32 Go
{: #install_32GB}

## Commande de votre serveur
{: #order_32GB}

1. Connectez-vous au [portail client de l'infrastructure {{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com){: new_window} avec vos données d'identification uniques.
2. Cliquez sur **Compte** > **Passer une commande** sur la page Récapitulatif du compte.
3. Cliquez sur **Au mois** sous Serveurs Bare Metal sur la page Unités. La liste des serveurs s'affiche ; les serveurs certifiés SAP figurent en haut de la liste.
4. Cliquez sur le lien hypertexte situé sous **PRIX PAR MOIS** pour sélectionner le serveur **BI.S3.NW32 (options du système d'exploitation).**

Le serveur BI.S3.NW32 (options du système d'exploitation) peut également être soumis à une facturation **horaire**.
{: note}

## Configuration de votre serveur
{: #configure_server}

1. Laissez **1** dans la zone **Quantité**.
2. Sélectionnez **TOR01** pour **Centre de données.** La liste des centres de données dépend des produits disponibles dans un centre de données spécifique.
3. Par défaut, la zone **Serveur** affiche une valeur prédéfinie basée sur votre sélection de serveur qui ne peut pas être modifiée.
4. Cliquez sur **32 Go RAM** même si la sélection de **RAM** affiche par défaut une valeur prédéfinie basée sur votre sélection de serveur qui ne peut pas être modifiée.
5. Cliquez sur **Redhat** et sélectionnez **Red Hat Enterprise Linux for SAP Business Application 7.X (64 bits)** comme **Système d'exploitation**. **Remarque** : Si vous apportez votre propre licence (BYOL) pour votre système d'exploitation, sélectionnez **Autre** > **Sans système d'exploitation**. Pour plus d'informations, voir [Apport de votre propre licence](#byol).
6. Ajoutez une deuxième unité SATA de 2 To en cliquant sur le menu déroulant **Contrôleur de disques 1** et en sélectionnant **Disque SATA de 2 To**. Cliquez sur **Ajouter un disque**.
7. Cliquez sur **Sélectionner tous les disques** et cliquez sur **Créer un groupe de stockage RAID**.
8. Cliquez sur **Type** et sélectionnez **RAID 1**. Saisissez une **Taille** couvrant la quantité totale de stockage dont vous avez besoin.
9. Ne cochez pas la case **LVM** et acceptez les valeurs par défaut **Modèle de partition**, **Linux de base**.
10. Cliquez sur **Terminé**.

## Sélection de vos options de serveur supplémentaires
{: #options_32GB}

1. Sélectionnez **500 Go** sous **Bande passante publique.**
2.	Sélectionnez **Liaisons montantes publiques redondantes de 1 Gbps** sous **Débits des ports de liaison montante.**
3. Laissez les valeurs par défaut de toutes les autres zones. Pour obtenir une description détaillée des options, voir [Génération d'un serveur bare metal personnalisé](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#addl-server-options).
4.	Cliquez sur **Ajouter à la commande** au bas de la page. Une fois votre commande vérifiée, vous serez redirigé vers la page de paiement.

## Définition de configurations système avancées
{: #adv_config}

Utilisez les valeurs du tableau 1 dans les zones de Configuration système avancée. Vous trouverez des informations supplémentaires dans les directives fournies sous [Advanced server configuration options](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#adv-system-config).

1. Faites défiler la liste et entrez les valeurs du tableau 1 sous **Configuration système avancée.**

|              Zone               |      Valeur                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|VLAN back end                      | Sélectionnez dans la liste déroulante, par exemple `tor01.bcr01a.1241`     |
|Sous-réseau                            | Sélectionnez dans la liste déroulante, par exemple `10.114.63.64/26`       |
|VLAN front end                     | Sélectionnez dans la liste déroulante, par exemple `tor01.fcr01a.851`      |
|Sous-réseau                            | Sélectionnez dans la liste déroulante, par exemple `158.85.65.224/28`      |
|Scripts de provisionnement                 | Laissez à blanc                                                          |
|Clés SSH                          | Valeur par défaut `Ajouter`, qui correspond à aucune clé SSH                            |
|Nom d'hôte                          | Par exemple, `e2e1270`                                               |
|Domaine                            | Par exemple, `saptest.com`                                           |
{: caption="Tableau 1. Valeurs de configuration avancée 32 Go" caption-side="top"}  

## Confirmation de vos sélections
{: #confirm_selections}

1. Confirmez vos sélections sur la page de paiement et cliquez sur **Conditions de services Cloud** et **Contrat de logiciel tiers** sur le côté droit de la page.
2. Cliquez sur **Soumettre commande** sur le côté droit du formulaire. Vous serez redirigé vers une page avec votre numéro de commande ; vous pouvez imprimer la page car il s'agit également de l'accusé de réception de votre commande. Vous recevrez en outre un courrier électronique de confirmation avec pour objet *Votre commande IBM Cloud numéro ## a été approuvée*, où ## correspond à votre numéro de commande.

Une fois votre commande soumise, votre serveur peut être utilisé pendant une durée allant de 1 à 4 heures, suivant votre commande. Vous pouvez vérifier l'écran Détails de l'unité sur la page principale du portail client de l'infrastructure (**Unités > Liste des unités**) pour afficher l'état des étapes de mise à disposition. Cliquez sur le **Nom de l'unité** correspondant à votre nom d'hôte donné et à votre domaine vous afficher son état.

## Apport de votre propre licence
{: #byol}

Lorsque vous disposez de votre propre licence de système d'exploitation, vous l'installez sur vos serveurs bare metal en suivant les instructions du fournisseur. Pour plus d'informations, voir [Option Sans système d'exploitation](/docs/bare-metal?topic=bare-metal-bm-no-os#bm-no-os).

## Etapes suivantes

  [2. Préparation de votre serveur pour votre installation SAP](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-prepare_32GB)

  [3. Partitionnement et systèmes de fichiers](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-partition_32GB)
