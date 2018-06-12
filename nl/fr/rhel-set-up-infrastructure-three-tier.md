---



copyright:
  years: 2017, 2018
lastupdated: "2018-02-26"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. Commande des serveurs 256 Go et 32 Go dans une configuration à trois niveaux
{: #install_three_tier}

## Commande de vos serveurs
{: #order_servers}

Suivez les étapes de la section [Commande de votre serveur 32 Go](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-set-up-infrastructure-32GB.html#order_32GB) pour commander le serveur d'applications SAP NetWeaver. Les étapes qui suivent vous guident à travers la commande du serveur de base de données.

1. Connectez-vous au portail [{{site.data.keyword.cloud_notm}} Infrastructure Customer Portal](https://control.softlayer.com) avec vos données d'identification uniques.
2. Cliquez sur l'icône **Unités** sur la page Récapitulatif du compte.
3. Cliquez sur **Au mois** sous Serveurs IBM Bare Metal sur la page Unités. La boîte de dialogue présentant la liste des serveurs s'ouvre.
4. Cliquez sur le lien hypertexte situé sous **PRIX PAR MOIS** pour sélectionner le serveur **BI.S1.NW256 (options du système d'exploitaiton).**

## Sélection de vos options de serveur
{: #options_32GB}

1. Laissez **1** dans la zone **Quantité**.
2. Sélectionnez **TOR01** pour **Centre de données.**
3. Par défaut, la zone **Serveur** affiche une valeur prédéfinie basée sur votre sélection de serveur qui ne peut pas être modifiée.
4. Cliquez sur **32 Go RAM** même si la sélection de **RAM** affiche par défaut une valeur prédéfinie basée sur votre sélection de serveur qui ne peut pas être modifiée.
5. Sélectionnez **Red Hat Enterprise Linux for SAP Business Application 6.X** comme système d'exploitation.
6. Sous **Disques durs**, sélectionnez un second disque SATA 2 To, créez un groupe de stockage RAID de RAID1 à partir des deux disques, couvrant la capacité de stockage totale et sélectionnez **Linux Basic** sous **Modèle de partition.** Laissez l'option **LVM** non cochée.
7. Sélectionnez **500 Go** sous **Bande passante publique.**
8. Sélectionnez **Liaisons montantes publiques redondantes de 1 Gbps** sous **Débits des ports de liaison montante.**
9. Dans cet exemple, laissez les valeurs par défaut pour toutes les autres zones. Vous pouvez consulter [Configuration de vos serveurs physiques](https://console.bluemix.net/docs/bare-metal/configuring.html#setting-up-your-bare-metal-servers) pour obtenir une description détaillée des options.
10.	Cliquez sur **Ajouter à la commande** au bas de la page. Une fois votre commande vérifiée, vous serez redirigé vers la page de paiement.

## Définition de configurations système avancées
{: #adv_config}

1. Utilisez les valeurs du tableau 1 dans les zones de Configuration système avancée. Vous trouverez des informations supplémentaires dans les directives fournies sous [Configuration système avancée](https://console.bluemix.net/docs/bare-metal/configuring.html#advanced-system-configuration).

|              Zone               |      Valeur                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|VLAN back end                      | Sélectionnez dans la liste déroulante, par exemple `tor01.bcr01a.1241`     |
|Sous-réseau                            | Sélectionnez dans la liste déroulante, par exemple `10.114.63.64/26`       |
|VLAN front end                     | Sélectionnez dans la liste déroulante, par exemple `tor01.fcr01a.1199`     |
|Sous-réseau                            | Sélectionnez dans la liste déroulante, par exemple `169.55.137.160/27`     |
|Scripts de provisionnement                 | Défini sur Aucun par défaut                                                     |
|Clés SSH                          | Défini sur Aucun par défaut                                                     |
|Nom d'hôte                          | Par exemple, `e2e2690`                                               |
|Domaine                            | Par exemple, `saptest.com`                                           |
{: caption="Tableau 1. Valeurs de configuration avancées" caption-side="top"}  

## Confirmation de vos sélections
{: #confirm_selections}

1. Confirmez vos sélections sur la page de paiement et cliquez sur **Conditions de services Cloud** et **Contrat de logiciel tiers** sur le côté droit de la page.
2. Cliquez sur **Soumettre commande**. Vous êtes redirigé vers un écran affichant votre numéro de commande. Vous pouvez imprimer cette page, qui vous servira également d'accusé de réception de commande.

Une fois votre commande soumise, le serveur peut être utilisé pendant une durée allant de 1 à 4 heures, suivant votre commande. Vous pouvez vérifier l'écran Détails de l'unité sur la page principale du portail {{site.data.keyword.slportal}} (**Unités > Liste des unités**) pour obtenir l'état des étapes de mise à disposition. Cliquez sur le **Nom de l'unité** correspondant à votre nom d'hôte donné et à votre domaine pour afficher son état.

## Etapes suivantes

  [2. Préparation de votre serveur pour votre installation SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-256GB.html)
  
  [3. Partitionnement et systèmes de fichiers](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-256GB.html)
  
  [4. Préparation de votre réseau](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
