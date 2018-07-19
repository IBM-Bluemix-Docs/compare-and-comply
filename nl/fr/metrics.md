---

copyright:
  years: 2017, 2018
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

# Utilisation de métriques
{: #using-metrics}

Vous pouvez surveiller l'état de {{site.data.keyword.cnc_short}} à l'aide du tableau de bord de surveillance d'IBM Cloud Private. Le tableau de bord de surveillance utilise Grafana, Prometheus et Kibana pour présenter des informations détaillées sur votre instance {{site.data.keyword.cnc_short}}. 

Pour plus d'informations sur le tableau de bord de surveillance, voir[https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window}.

## Installation et exécution du tableau de bord des métriques

Afin d'installer le tableau de bord des métriques pour {{site.data.keyword.cnc_short}}, procédez comme suit :

 1. Téléchargez le fichier PPA (Passport Advantage) pour {{site.data.keyword.cnc_short}}. Il s'agit d'un fichier tar compressé portant un nom tel que `ibm-watson-compare-comply-prod-1.0.0.tar.gz`. Il inclut le modèle de tableau de bord des métriques et un script `bash` permettant d'afficher le rendu du tableau de bord à partir du modèle. 

 1. Décompressez et développez le fichier PPA :
  ```bash
  $ mkdir ibm-watson-compare-comply-prod-1.0.0 && tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tar.gz -C ibm-watson-compare-comply-prod-1.0.0
  ```
  {: codeblock}

 1. Accédez au répertoire `charts` dans le répertoire ayant fait l'objet d'une extraction :
   ```bash
   $ cd ibm-watson-compare-comply-prod-1.0.0/charts
   ```

 1. Décompressez et développez le fichier tar compressé dans le répertoire `charts` :
   ```bash
   $ tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tgz
   ```

 1. Accédez au répertoire `dashboard`. Il comprend des modèles pour les métriques et la journalisation, ainsi qu'un script bash permettant de générer des tableaux de bord à partir des modèles.


   ```bash
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
  
    -  `-v, --version {chart_version}` : version du graphique, par exemple, `1.0.0`.
    -  `-h, --help` : aide et sortie de la commande d'impression. 
    -  `-r, --release {release_name}` : nom de l'édition Helm. 
    -  `-n, --namespace {namespace}` : espace de nom du déploiement. L'espace de nom par défaut est `default`.

   ```bash
   $ ./render-dashboards.sh -v 1.0.0 -r my-test-release -n default
   The dashboard JSON files are generated under /Users/{user}/Downloads/ibm-watson-compare-comply-prod-1.0.0/charts/ibm-watson-compare-comply-prod/dashboard.

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

## Importation du tableau de bord des métriques

Afin d'importer le tableau de bord des métriques pour {{site.data.keyword.cnc_short}} dans IBM Cloud Private, procédez comme suit :

  1. Connectez-vous à votre cluster IBM Cloud Private. 

  1. A partir de l'icône de menu située dans l'angle supérieur gauche, sélectionnez **Plateforme -> Surveillance**. <br />
      ![Icône de menu IBM Cloud Private](images/icp-menu.png) <br />
      ![Menu Plateforme -> Surveillance](images/icp-monitoring.png)

  1. Cliquez sur **Home** dans l'angle supérieur gauche de l'interface Grafana. <br />
      ![Icône Home](images/icp-home.png)

  1. Cliquez sur **Import Dashboard**.
      ![Icône Import Dashboard](images/import-dboard.png)

  1. Sélectionnez le fichier `metrics.json` qui a été généré à l'étape 6 de la procédure précédente, puis cliquez sur **Upload .json File**. <br />
      ![Téléchargement du fichier metrics.json](images/metrics-json.png)

  1. Sélectionnez **Prometheus** comme source de données, puis cliquez sur **Import**.
       ![Sélection de Prometheus](images/prometheus.png)

## Affichage du tableau de bord des métriques

Le tableau de bord des métriques se présente comme suit :
![Tableau de bord des métriques](images/metrics-dboard.png)

Vous pouvez facilement modifier l'intervalle et la fréquence d'actualisation automatique :
  ![Modification de l'intervalle et de la fréquence d'actualisation](images/dboard-change.png)

## Edition du tableau de bord des métriques

Vous pouvez éditer le tableau de bord des métriques ou créer un nouveau tableau de bord en procédant comme suit :

  1. A partir de l'icône de menu située dans l'angle supérieur gauche, sélectionnez **Plateforme -> Surveillance** pour accéder à l'interface utilisateur Grafana. 

  1. Cliquez sur **Home** dans l'angle supérieur gauche de l'interface Grafana, puis cliquez sur **+ New Dashboard**.

  1. Sélectionnez le type de panneau que vous souhaitez ajouter, par exemple, **Graph** ou **Table**.

  1. Cliquez sur le titre de panneau, puis sur **Edit**. Le titre de panneau par défaut est `Panel title`.

  1. Utilisez l'onglet **General** pour définir le titre, la description et les dimensions du panneau. Notez que la pleine largeur d'une fenêtre de navigateur correspond à 12 unités. 

  1. Utilisez l'onglet **Metrics** pour créer des requêtes permettant d'afficher des données à partir de Prometheus.

        1. Vous pouvez écrire la requête directement si vous connaissez le langage de requête ou vous pouvez utiliser la zone **Metric lookup** pour effectuer une sélection parmi les métriques actuellement signalées à Prometheus.

        1. Les résultats des requêtes sont affichés en temps réel dans le nouveau panneau de tableau de bord. 

        1. Plusieurs requêtes peuvent être ajoutées à un panneau. Par exemple, vous pouvez afficher des opérations de lecture et d'écriture sur le même diagramme ou le nombre total de visites et le nombre total de visiteurs dans le même tableau. 
        
