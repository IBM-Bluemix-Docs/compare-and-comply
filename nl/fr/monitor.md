---

copyright:
years: 2017, 2018
lastupdated: "2018-08-02"

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

# Installation, configuration et utilisation des tableaux de bord de surveillance
{: #monitor}

Vous pouvez surveiller l'état de {{site.data.keyword.cnc_short}} à l'aide des tableaux de bord de surveillance d'IBM Cloud Private. Les tableaux de bord de surveillance utilisent Grafana, Kibana et Prometheus pour afficher des informations détaillées personnalisables concernant votre instance {{site.data.keyword.cnc_short}}.

Pour plus d'informations sur la surveillance de votre cluster {{site.data.keyword.BluOpenStackDed}}, voir [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window}.

Avant d'utiliser les tableaux de bord, vous devez les créer et les installer comme indiqué dans la procédure suivante.

## Etape 1: Télécharger, extraire et afficher le rendu des modèles de tableau de bord
{: #render}

Procédez comme suit pour préparer les modèles de tableau de bord pour installation.

1. Téléchargez l'image {{site.data.keyword.cnc_short}} depuis Passport Advantage (PPA). Il s'agit d'un fichier tar compressé portant un nom tel que `ibm-watson-compare-comply-prod-1.0.5.tar.gz`. Il inclut les modèles de tableau de bord et un script `bash` permettant d'afficher le rendu des tableaux de bord à partir des modèles.

1. Décompressez et développez le fichier tar :
  ```bash
  mkdir ibm-watson-compare-comply-prod-1.0.5 && tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tar.gz -C ibm-watson-compare-comply-prod-1.0.5
  ``` 
  {: pre}

1. Accédez au répertoire `charts` dans le répertoire ayant fait l'objet d'une extraction :
  ```bash
  cd ibm-watson-compare-comply-prod-1.0.5/charts
  ```
  {: pre}

1. Décompressez et développez le fichier tar compressé dans le répertoire `charts` :
  ```bash
  tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tgz
  ```
  {: pre}

1. Accédez au répertoire `dashboard`. Il contient des modèles pour les alertes, la journalisation et les métriques, ainsi qu'un script `bash` permettant de générer des tableaux de bord à partir des modèles.
  ```
  $ cd ibm-watson-compare-comply-prod/dashboard
  
  $ tree
   .
  ├── alerts.json.tpl
  ├── external-process-logging.json.tpl
  ├── frontend-logging.json.tpl
  ├── metrics.json.tpl
  └── render-dashboards.sh

  0 directories, 5 files
  ```

1. Exécutez le script `render-dashboards.sh` pour afficher le rendu des modèles. Les options relatives au script sont notamment les suivantes :
  
    - `-v`, `--version {chart_version}` : version du graphique, par exemple, `1.0.5`.
    - `-h`, `--help` : aide et sortie de la commande d'impression.
    - `-r`, `--release {release_name}` : nom de l'édition Helm.
    - `-n`, `--namespace {namespace}` : espace de nom du déploiement. L'espace de nom par défaut est `default`.

  ```bash
  ./render-dashboards.sh -v 1.0.5 -r my-test-release -n default
  ```
  {: pre}

Les fichiers JSON de tableau de bord sont générés sous `ibm-watson-compare-comply-prod-1.0.5/charts/ibm-watson-compare-comply-prod/dashboard`.

  ```
  $ tree
   .
  ├── alerts.json
  ├── alerts.json.tpl
  ├── external-process-logging.json
  ├── external-process-logging.json.tpl
  ├── frontend-logging.json
  ├── frontend-logging.json.tpl
  ├── metrics.json
  ├── metrics.json.tpl
  └── render-dashboards.sh

  0 directories, 9 files
  ```

## Etape 2 : Importer les modèles
{: #import-tpls}

Après avoir affiché le rendu des tableaux de bord, importez-en un ou plusieurs comme indiqué dans les sections suivantes :

  - [Importation du tableau de bord des métriques](metrics.html#import)
  - [Importation des tableaux de bord de journalisation](logging.html#import)
  - [Importation du tableau de bord des alertes](alerts.html#import)

## Etape 3 : Afficher et personnaliser les tableaux de bord
{: #use-dboards}

Après avoir importé les tableaux de bord sélectionnés, affichez et personnalisez-les comme indiqué dans les sections suivantes :

  - [Affichage du tableau de bord des métriques](metrics.html#view)
  - [Affichage des tableaux de bord de journalisation](logging.html#view)
  - [Ajout de règles d'alerte](alerts.html#add)
