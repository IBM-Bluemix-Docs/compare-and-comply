---

copyright:
years: 2018
lastupdated: "2018-06-28"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Compréhension des configurations de déploiement
{: #configs}

La configuration de déploiement par défaut pour {{site.data.keyword.cnc_short}} est `2Cores 10G 1 concurrent document (2 VPC)`. Cette configuration peut traiter environ 1500 pages par heure. Vous pouvez augmenter votre déploiement {{site.data.keyword.cnc_short}} sur IBM Cloud Private à différents niveaux en fonction de vos besoins en matière de capacité de traitement. Pour en savoir plus sur l'augmentation de la capacité de traitement de {{site.data.keyword.cnc_short}} sur IBM Cloud Private, contacte votre ingénieur commercial IBM.

{{site.data.keyword.cnc_short}} peut être mis à l'échelle sur deux dimensions : _verticale_ et _horizontale_.

 - La mise à l'échelle _verticale_ augmente la capacité de traitement de l'analyse syntaxique d'un document.
 - La mise à l'échelle _horizontale_ augmente le nombre de documents faisant simultanément l'objet d'une analyse syntaxique.

Les options de mise à l'échelle nécessitent d'augmenter le nombre de coeurs de processeurs virtuels (VPC) utilisés par {{site.data.keyword.cnc_short}}. Pour plus d'informations sur les VPC, voir la [documentation sur l'octroi de licence ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_cluster/licensing.html){: new_window} pour IBM Cloud Private.

Les options de mise à l'échelle verticale sont notamment les suivantes :

| Configuration                             |Capacité de traitement approximative*         |
|:-----------------------------------------:|--------------------------------|
|`2Cores 10G 1 concurrent document (2 VPC)` |1500 pages par heure (option par défaut)   |
|`4Cores 20G 1 concurrent document (4 VPC)` |2500 pages par heure             |
|`8Cores 40G 1 concurrent document (8 VPC)` |3000 pages par heure             |

Les options de mise à l'échelle horizontale sont notamment les suivantes :

| Configuration                               |Capacité de traitement approximative*         |
|:-------------------------------------------:|--------------------------------|
|`2Cores 10G 2 concurrent documents (4 VPC)`  |3000 pages par heure             |
|`4Cores 20G 2 concurrent documents (8 VPC)`  |5000 pages par heure             |
|`8Cores 40G 2 concurrent documents (16 VPC)` |6000 pages par heure             |

\***Remarque** : ces estimations de capacité de traitement sont basées sur des tests réalisés en interne. La capacité de traitement dans un environnement de production dépend de plusieurs facteurs et notamment des autres charges de travail sur le même cluster IBM Cloud Private, des temps d'attente entre l'application client et le cluster IBM Cloud Private et d'autres conditions d'ordre environnemental.

Pour modifier la configuration de déploiement en ajoutant ou en retirant des clusters {{site.data.keyword.cnc_short}}, procédez comme suit :

**Remarque** : si vous réduisez la taille d'un déploiement {{site.data.keyword.cnc_short}}, IBM Cloud Private récupère immédiatement les ressources libérées, ce qui réduit la capacité de traitement du déploiement. Ne réduisez pas la taille d'un déploiement en cours d'utilisation, en particulier dans un environnement de production.
	
De même, si vous augmentez la taille d'un déploiement {{site.data.keyword.cnc_short}}, IBM Cloud Private applique immédiatement les ressources demandées au déploiement, en supposant que les ressources soient disponibles. Si vous avez besoin de plus de ressources que ce que votre installation IBM Cloud Private est capable de fournir, contactez votre ingénieur commercial IBM pour augmenter la capacité de traitement de votre installation IBM Cloud Private.

  1. Connectez-vous à votre cluster IBM Cloud Private.

  1. A partir de l'icône de menu située dans l'angle supérieur gauche, sélectionnez **Charges de travail -> Déploiements**.
  
  1. La console affiche la page **Déploiements**. Dans le tableau des déploiements, recherchez le déploiement que vous souhaitez mettre à l'échelle.
  
  1. Sur la ligne du tableau correspondant à votre déploiement, cliquez sur l'icône **Action** dans la dernière colonne du tableau, puis cliquez sur **Mettre à l'échelle**.
  
  1. La console affiche la fenêtre **Mettre à l'échelle Deployment**. Dans la zone **Instances**, entrez le nombre d'instances souhaité ou utilisez les flèches de déplacement vers le haut et vers le bas pour sélectionner la valeur souhaitée.
  
  1. Cliquez sur **Mettre à l'échelle Deployment**.
  
  1. La fenêtre **Mettre à l'échelle Deployment** se ferme et la console affiche de nouveau la page **Déploiements**. Recherchez votre déploiement et utilisez les colonnes **Souhaité** et **En cours** pour vérifier que le nombre d'instances que vous avez demandé a été alloué au déploiement.
  
  1. Vous pouvez éventuellement cliquer sur le nom du déploiement sur la page **Déploiement** pour afficher les détails du déploiement.
  
  

