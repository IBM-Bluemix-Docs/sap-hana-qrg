---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, {{site.data.keyword.cloud_notm}}, deployment, BYOL

subcollection: sap-netweaver

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# 2. Configuration de votre infrastructure
{: #set_up_infrastructure}

Vous trouverez dans cette section tous les conseils nécessaires à la configuration de vos serveurs {{site.data.keyword.baremetal_long}} sur SAP NetWeaver, que ce soit en matière de réseau, de connectivité, de sécurité, de stockage ou de système d'exploitation. Prenez contact avec le [support {{site.data.keyword.cloud_notm}} ](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support) pour toute question.

## Commande de votre serveur
{: order-server}

Suivez les étapes suivantes pour commander vos serveurs {{site.data.keyword.baremetal_short}}. Vous trouverez des informations supplémentaires dans la section [Génération d'un serveur bare metal personnalisé](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server).

1. Connectez-vous au [portail client de l'infrastructure {{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com){: new_window} à l'aide de vos données d'identification uniques.
2. Cliquez sur **Compte** > **Passer une commande** sur la page Récapitulatif du compte.
3. Cliquez sur le lien **Au mois** sous {{site.data.keyword.baremetal_short}}, sur la page Unités. La boîte de dialogue Liste des serveurs s'affiche avec les serveurs certifiés SAP en haut de la liste.
4. Cliquez sur le lien hypertexte situé sous **Prix par mois** pour sélectionner le serveur approprié et le déplacer vers la page de configuration/commande.

Le serveur BI.S3.NW32 (options du système d'exploitation) peut également être soumis à une facturation **horaire**.
{: note}

   Les serveurs certifiés SAP NetWeaver sont facilement identifiables par la mention **-NW** sous Modèle CPU. La configuration du serveur Red Hat est décrite à l'étape 7 et à l'étape 1 de la section [Sélection de vos options de serveur](#select_options). La procédure est la même sous Microsoft Windows. Notez que pour VMware, le choix est plus limité, mais la procédure reste la même.

   Voici un exemple de la manière dont vous pouvez déchiffrer les noms des serveurs SAP NetWeaver.

| Nom de serveur | Composant de la convention de dénomination | Signification |
| --- | --- | --- |
| BI.S3.NW768 | BI | Interface Bluemix |
| | S3 | Série 2 (génération de processeur) |
| | | S1 est Ivy Bridge/Haswell |
| | | S2 est Broadwell |
| | | S3 est Skylake/Kaby Lake |
| | NW | Serveur certifié NetWeaver |
| | 768 | Quantité de mémoire RAM |

5. Entrez le nombre de serveurs que vous commandez dans la zone **Qualité** et sélectionnez votre **centre de données**.
6. Les options relatives au **serveur**, à la **mémoire vive** et au stockage privé prennent la valeur par défaut du serveur sélectionné et ne sont pas modifiables. {{site.data.keyword.IBM_notm}} {{site.data.keyword.blockstorageshort}} for {{site.data.keyword.cloud_notm}} ou {{site.data.keyword.filestorage_full_notm}} sont commandés une fois que vous avez commandé votre serveur.
7. Sélectionnez votre système d'exploitation dans **Système d'exploitation** (Microsoft, Red Hat ou SUSE), puis sélectionnez le système d'exploitation ou l'hyperviseur VMware correspondant à votre serveur. 

Si vous apportez votre propre licence (BYOL) pour votre système d'exploitation, sélectionnez **Autre** > **Sans système d'exploitation**. Pour plus d'informations, voir [Apport de votre propre licence](#byol).
{: note}

## Sélection de vos options de serveur
{: #select_options}

Au cours de l'étape suivante, vous allez sélectionner le type et le nombre de disques à ajouter à votre configuration. Vous pouvez aussi sélectionner des options différentes pour les groupes de stockage RAID (Redundant Array of Independent Disks) et les agencements de partition sur les groupes de stockage RAID. Pour plus d'informations, consultez la section [About RAID](/docs/bare-metal?topic=bare-metal-about-raid#about-raid) ou contactez le [support {{site.data.keyword.cloud_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).

1. Sous **Serveurs certifiés SAP**, effectuez votre sélection en fonction de la façon dont vous prévoyez d'utiliser votre serveur. Consultez l'outil [Design Decision Tool ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-cloud-architecture/infrastructure-design-decision-tool){: new_window} (faire défiler la page jusqu'à l'outil) ou consultez le [support {{site.data.keyword.cloud_notm}} ](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support) pour plus d'informations.
2. Cliquez sur le bouton **Ajouter à la commande** au bas de la page.

## Définition de configurations système avancées
{: #adv_config}

1. Suivez les instructions de la rubrique [Configuration système avancée](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server) pour obtenir de l'aide sur les valeurs figurant dans la fenêtre **Configuration système avancée**.

Les noms d'hôte SAP doivent être composés de 13 caractères alphanumériques maximum. Voir les [notes SAP 611361 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://launchpad.support.sap.com/#/611361){: new_window} et [129997 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://launchpad.support.sap.com/#/129997){: new_window} pour obtenir plus de détails sur les noms d'hôte SAP.

Si vous avez décidé de configurer votre environnement en mode public/privé et que vous envisagez
  * d'installer plusieurs systèmes SAP ayant besoin de communiquer entre eux *ou*
  * de choisir une configuration SAP à trois niveaux (la base de données et les instances d'application étant installées sur du matériel différent)

Assurez-vous que la résolution de nom reflète l'adresse externe et les adresses internes. L'adresse externe correspond au nom d'hôte du serveur qui a été résolu par son nom de domaine complet (FQDN).

Les adresses internes n'apparaissent pas dans le système de résolution de nom (DNS). Sachant que les adresses IP internes peuvent être utilisées pour les communications entre les serveurs, assurez-vous d'étendre votre fichier `/etc/hosts` ou votre fichier "host" Microsoft Windows en conséquence. Ces informations s'appliquent également au système d'exploitation invité de vos déploiements VMware ESXi.

## Confirmation de vos sélections
{: #confirm_selections}

1. Validez vos choix sur la page de paiement et cochez les cases **Conditions des services Cloud** et **Contrat de licence logiciel tiers**.
2. Faites défiler la page vers le bas jusqu'à la partie Créer un compte. Entrez les informations de facturation et renseignez les zones **Type de paiement, Nom, Carte** et **Expiration** (date) relatives à la carte de paiement à utiliser pour la facturation.
3. Cliquez sur le bouton **Soumettre commande**. Vous êtes redirigé vers un écran affichant votre numéro de commande. Vous pouvez imprimer cette page, qui vous servira également d'accusé de réception de commande.

Un courrier électronique intitulé _Votre commande {{site.data.keyword.cloud_notm}} numéro ## a été approuvée_ est alors envoyé à l'adresse que vous avez renseignée dans votre profil. Il indique que votre serveur a été approuvé et qu'il est sur le point d'être déployé. Une fois déployé, un autre avis est envoyé vous informant que le serveur est disponible et peut être géré via le [portail client de l'infrastructure {{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com){: new_window}.

## Apport de votre propre licence
{: #byol}

Si vous possédez votre propre licence de système d'exploitation, installez celui-ci sur votre serveur {site.data.keyword.baremetal_short} en suivant les instructions du fournisseur. Pour plus d'informations, voir [Option Sans système d'exploitation](/docs/bare-metal?topic=bare-metal-bm-no-os#bm-no-os).

## Etapes suivantes

Vous pouvez maintenant commencer à gérer vos serveurs {{site.data.keyword.baremetal_short}}. Pour connaître la procédure correspondante, voir [Gestion de votre environnement SAP NetWeaver](/docs/infrastructure/sap-netweaver?topic=sap-netweaver-manage_environment#manage_environment).
