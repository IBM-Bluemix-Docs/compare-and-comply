---

copyright:
years: 2018
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

# Déploiement du service
{: #install}

Avant d'installer la charte Helm {{site.data.keyword.cnc_long}} comme indiqué dans le fichier `README.md` de la charte Helm, vous devrez peut-être effectuer des étapes de déploiement supplémentaires. Effectuez ces étapes uniquement si vous déployez {{site.data.keyword.cnc_long}} version 1.0.3 ou antérieure. Elles ne s'appliquent pas à un déploiement d'{{site.data.keyword.cnc_long}} version 1.0.4 ou ultérieure.

## Valeurs système par défaut
{: #sys_vals}

Ces instructions utilisent les valeurs système par défaut énumérées ci-après. Les valeurs de votre système peuvent être différentes. Renseignez-vous auprès de votre administrateur IBM Cloud Private.

 - Nom du cluster et numéro de port IBM Cloud Private :
 	`mycluster.icp:8500`

 - Espace de nom IBM Cloud Private :
 	`default`

## Exécution du déploiement
{: #deploy}

Procédez comme suit pour réaliser le déploiement de {{site.data.keyword.cnc_short}} :

### Etape 1 : Accorder un accès Docker
{: #step1}

  1. Accordez un accès Docker au registre IBM Cloud Private.
  
    - Si vous exécutez Microsoft Windows ou Apple macOS ou OS X, procédez comme suit :

      1. Cliquez sur l'icône **Docker**.
      
      1. Ouvrez le menu **Preferences**.
    
      1. Cliquez sur l'onglet **Daemon**.
    
      1. Dans la zone **Insecure registries**, ajoutez la valeur `mycluster.icp:8500` ou la valeur pour votre installation IBM Cloud Private.

      ![Configurer Docker](images/docker.png)

    - Si vous exécutez Linux, procédez comme suit :

      1. Repérez le fichier `daemon.json`. Par défaut, ce fichier se trouve dans `/etc/docker/daemon.json`. Si le fichier n'existe pas, créez-le à l'emplacement par défaut. Ouvrez le fichier dans un éditeur de texte et vérifiez ou ajoutez l'objet JSON suivant :

      ```
      {
         "insecure-registries": ["mycluster.icp:8500"]
 	  }
      ```
      {: pre}
      
      1. Exécutez les commandes suivantes :
      
      ```bash
      systemctl daemon reload
      ```
      {: codeblock}
      
      ```bash
      systemctl restart docker
      ```
      {: codeblock} 

### Etape 2 : Télécharger les interfaces de ligne de commande IBM Cloud Private et IBM Cloud
{: #step2}

  1. Téléchargez et installez l'interface de ligne de commande Helm pour chaque version d'IBM Cloud Private que vous utilisez. Pour plus d'informations, voir la [documentation IBM Cloud Private ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/icp_cli.html){: new_window}.
  
  1. Téléchargez et installez l'interface de ligne de commande IBM Cloud. Pour plus d'informations, voir la [documentation IBM Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html#download_install){: new_window}.
  
  1. Téléchargez et installez le plug-in `bx` de l'interface de ligne de commande IBM Cloud. Pour plus d'informations, voir la [documentation IBM Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/extend_cli.html#plug-ins){: new_window}.

### Etape 3 : Télécharger le fichier de package
{: #step3}

Téléchargez le fichier de package `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` depuis [Passport Advantage![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www-01.ibm.com/software/passportadvantage/){: new_window} sur votre poste de travail local. Ce fichier contient l'intégralité de l'image du service {{site.data.keyword.cnc_short}}.
  
#### Editez le fichier de package si nécessaire
{: edit-pkg-file}

**Important** : effectuez la procédure décrite dans cette étape *uniquement* si les conditions ci-après sont réunies. Si l'une ou les deux conditions ne sont pas réunies, passez à l'[étape 4](#step4).

 - Cette étape s'applique uniquement à {{site.data.keyword.cnc_short}} version 1.0.3 et antérieure. Si vous installez {{site.data.keyword.cnc_short}} version 1.0.4 ou ultérieure, n'effectuez pas la procédure décrite dans cette étape.

 - Vous devez suivre la procédure décrite dans cette étape uniquement si votre cluster IBM Cloud Private n'est pas connecté à l'Internet externe. Si votre cluster IBM Cloud Private dispose d'un accès réseau au site [Passport Advantage![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www-01.ibm.com/software/passportadvantage/){: new_window}, vous pouvez installer et déployer {{site.data.keyword.cnc_short}} version 1.0.3 sans apporter aucune modification au fichier de package.

**Remarque** : si vous suivez la procédure décrite dans cette étape, il se peut que vous rencontriez des problèmes avec les mises à niveau et les rétromigrations de {{site.data.keyword.cnc_short}} version 1.0.3. 

  1. Dans un interpréteur de commandes `bash` ou un environnement équivalent, tel que Cygwin, procédez à l'extraction du fichier `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` :
  
  ```bash
  cd {chemin_fichier}
  ```
  {: codeblock}

  ```bash
  tar -xvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz
  ```
  {: codeblock}

 Le fichier extrait comprend un répertoire nommé `charts` qui contient un fichier `ibm-watson-compare-comply-prod-1.0.3.tgz`, lequel correspond à la version compressée de la charte Helm pour {{site.data.keyword.cnc_short}}.

  1. Accédez au répertoire `charts` et procédez à l'extraction du fichier `ibm-watson-compare-comply-prod-1.0.3.tgz` :
  
  ```bash
  cd charts
  ```
  {: codeblock}
 
  ```bash
  tar -xvzf ibm-watson-compare-comply-prod-1.0.3.tgz
  ```
  {: codeblock}
 
  Le fichier extrait comprend un certain nombre de répertoires et de fichiers, y compris un répertoire de niveau supérieur nommé `ibm-watson-compare-comply-prod` qui contient un répertoire nommé `templates`.

  1. Accédez au répertoire `ibm-watson-compare-comply-prod/templates` :
  
  ```bash
  cd ibm-watson-compare-comply-prod/templates
  ```
  {: codeblock}
 
  1. Repérez le fichier `deployment.yaml` dans le répertoire `ibm-watson-compare-comply-prod/templates` et ouvrez-le dans un éditeur de texte.
 
  1. Editez le fichier `deployment.yaml` comme suit :
  
  **Original**
  
  ```
    – name: {{ .Chart.Name }}
        image: "hyc-icpcontent-docker-local.artifactory.swg-devops.com/cncdev/cnc-frontend:v2.3.12"
        imagePullPolicy: Always
  ```
  {: codeblock}
 
  **Mis à jour**
  
  ```
    – name: {{ .Chart.Name }}
        image: "mycluster.icp:8500/default/cnc-frontend:v2.3.12"
        imagePullPolicy: Always
  ```
  {: pre}
  
  Sauvegardez et fermez le fichier `deployment.yaml`.

  1. Revenez au répertoire `charts` et reconditionnez le fichier `ibm-watson-compare-comply-prod-1.0.3.tgz` :
  
    ```bash
    cd ../..
    ```
    {: codeblock}
  
    ```bash
    tar -cvzf ibm-watson-compare-comply-prod-1.0.3.tgz ibm-watson-compare-comply-prod
    ```
    {: codeblock}
   
  1. Revenez au répertoire de niveau supérieur et reconditionnez le fichier `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` :
  
    ```bash
    cd ..
    ```
    {: codeblock}
 
   ```bash
   tar -cvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz charts images manifest.json manifest.yaml
  ```
  {: codeblock}
  
  Pour référence, la vue "tree" du contenu du fichier "IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz" non compressé se présente comme illustré ci-après. Fichiers impliqués dans cette procédure sont appelés avec `<-----` dans la colonne de droite.

  ```bash
  cd {chemin_fichier_archive}
  ```
  {: codeblock}
   
  ```
  $ tree
   .
     ├── IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz          <-----
     ├── charts                                            <-----
     │   ├── ibm-watson-compare-comply-prod
     │   │   ├── Chart.yaml
     │   │   ├── LICENSE
     │   │   ├── LICENSES
     │   │   │   └── LICENSE-Product
     │   │   ├── README.md
     │   │   ├── cnc-banner.png
     │   │   ├── dashboard
     │   │   │   ├── alerts.json.tpl
     │   │   │   ├── external-process-logging.json.tpl
     │   │   │   ├── frontend-logging.json.tpl
     │   │   │   ├── metrics.json.tpl
     │   │   │   └── render-dashboards.sh
     │   │   ├── templates                                 <-----
     │   │   │   ├── NOTES.txt
     │   │   │   ├── _helpers.tpl
     │   │   │   ├── _sch-chart-config.tpl
     │   │   │   ├── configmap.yaml
     │   │   │   ├── deployment.yaml                       <-----
     │   │   │   ├── ingress.yaml
     │   │   │   ├── metrics-service.yaml
     │   │   │   ├── sch-2.6.0
     │   │   │   │   ├── _config.tpl
     │   │   │   │   ├── _metadata.tpl
     │   │   │   │   ├── _names.tpl
     │   │   │   │   ├── _utils.tpl
     │   │   │   │   └── _version.tpl
     │   │   │   ├── service.yaml
     │   │   │   └── tests
     │   │   │       └── service-test.yaml
     │   │   ├── values-metadata.yaml
     │   │   └── values.yaml
     │   └── ibm-watson-compare-comply-prod-1.0.3.tgz      <-----
     ├── images
     │   ├── 304735eab4e4df55b236f9835104cb1058bc50cb5ff7f4ee7d719cb56d2a718b.tar.gz
     │   ├── af80995a4fcfcee364f4f72ba043bf9e73dcafe7fcac384e1cd06bf1c7a52c1f.tar.gz
     │   └── ccec10461710cd625cbbacc0f5d5d837a0cc5baf132e0fc39ed42d8264d25062.tar.gz
     ├── manifest.json
     └── manifest.yaml 
  ```
  {: pre}
   
### Etape 4 : Préparer un terminal
{: #step4}

Préparez un interpréteur de commandes bash ou un environnement équivalent à utiliser avec votre installation IBM Cloud Private en exécutant les commandes suivantes dans l'interpréteur de commandes :

  ```bash
  bx pr login -a https://mycluster.icp:8443 --skip-ssl-validation
  ```
  {: codeblock}
    
  ```bash
  bx pr cluster-config mycluster
  ```
  {: codeblock}
  
  ```bash
  docker login mycluster.icp:8500
  ```
  {: codeblock}

  **Remarque** : dans la première commande de la liste précédente, le numéro de port 8443 dans l'URL https://mycluster.icp:8443 représente la valeur par défaut permettant de communiquer avec le service {{site.data.keyword.cnc_short}} sur IBM Cloud Private.

### Etape 5 : Créer un secret de registre d'images et un compte de service de correctif
{: #step5}

Exécutez les commandes suivantes pour permettre à tous les composants requis d'accéder au registre interne d'IBM Cloud Private :

```bash
kubectl create secret docker-registry $secret_name --docker-server=mycluster.icp:8500 --docker-username=$username --docker-password=$password --docker-email=admin@admin.com --namespace=default
```
{: codeblock}

```bash
kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "$secret_name"}]}' --namespace=default
```
{: codeblock}

où :
  - `$username` et `$password` varient d'un cluster à l'autre. Renseignez-vous auprès de votre administrateur.
  - `$secret_name` est une chaîne que vous spécifiez [pour permettre aux conteneurs Docker de communiquer en toute sécurité ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.docker.com/engine/swarm/secrets/){: new_window}.

### Etape 6 : Télécharger le fichier de package dans le registre d'images interne d'IBM Cloud Private
{: #step6}

Dans le terminal que vous avez préparé à l'[étape 4](#step4), exécutez la commande suivante pour télécharger package de fichier image :

```bash
bx pr load-ppa-archive --archive {path_to}/IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz --clustername mycluster.icp --namespace default 
```
{: codeblock}
### Etape 7 : Terminer l'installation

Revenez à la page **Catalogue** sur votre cluster IBM Cloud Private, repérez et sélectionnez l'entrée ibm-watson-compare-comply-prod, puis cliquez sur **Configurer** afin de poursuivre l'installation.
