---

copyright:
years: 2017, 2018
lastupdated: "2018-03-23"

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

# Utilisation de la journalisation
{: #using-logging}

## Installation et exécution des tableaux de bord de journalisation

Afin d'installer le tableau de bord de journalisation pour {{site.data.keyword.cnc_short}}, procédez comme suit :

  1. Téléchargez le fichier PPA (Passport Advantage) pour {{site.data.keyword.cnc_short}}. Il s'agit d'un fichier tar compressé portant un nom tel que `ibm-watson-compare-comply-prod-1.0.0.tar.gz`. Il inclut les modèles de tableau de bord de journalisation et un script `bash` permettant d'afficher le rendu des tableaux de bord à partir des modèles. 

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

## Importation des tableaux de bord de journalisation

Afin d'importer les tableaux de bord de journalisation pour {{site.data.keyword.cnc_short}} dans IBM Cloud Private, procédez comme suit :

  1. Connectez-vous à votre cluster IBM Cloud Private. 

  1. A partir de l'icône de menu située dans l'angle supérieur gauche, sélectionnez **Plateforme -> Journalisation**. <br />
    ![Icône de menu IBM Cloud Private](images/icp-menu.png) <br />
    ![Menu Plateforme -> Journalisation](images/icp-logging.png)

  1. Cliquez sur **Management** dans la partie gauche de l'interface Kibana. <br />
    ![Interface Kibana](images/kibana.png)

  1. Sélectionnez l'onglet **Saved Objects**.
    ![Onglet Saved Objects](images/saved-obj.png)

  1. Sélectionnez l'onglet **Searches** et cliquez sur **Import**.
    ![Option Import sur l'onglet Searches](images/searches-import.png)

  1. Importez individuellement les fichiers `frontend-logging.json` et `external-process-logging.json` qui ont été générés à l'étape 6 de la procédure précédente. Lorsque vous y êtes invité, cliquez sur **Yes, overwrite all**.
     ![Invite Yes, overwrite all](images/overwrite-all.png)

  1. Les tableaux de bord s'affichent sur l'onglet **Searches**.
     ![Tableaux de bord affichés sur l'onglet Searches](images/searches-tab.png)

## Affichage des tableaux de bord de journalisation

Pour afficher les tableaux de bord de journalisation, procédez comme suit :

  1. Accédez à l'onglet **Discover**. 

  1. Cliquez sur **Open** en haut à droite de l'interface Kibana. 

  1. Sélectionnez le tableau de bord que vous souhaitez afficher. Il existe deux tableaux de bord de journalisation, pour le journal de maintenance et pour le journal de processus externe.
    ![Affichage des tableaux de bord de journalisation](images/kibana-dboards.png)

Vous pouvez facilement modifier l'intervalle et la fréquence d'actualisation automatique :
  ![Modification de l'intervalle et de la fréquence d'actualisation](images/log-dboard-change.png)

