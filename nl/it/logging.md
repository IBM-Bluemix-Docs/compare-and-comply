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

# Utilizzo della registrazione
{: #using-logging}

## Installazione ed esecuzione dei dashboard di registrazione

Per installare il dashboard di registrazione per {{site.data.keyword.cnc_short}}, completa la seguente procedura.

  1. Scarica il file PPA (Passport Advantage) per {{site.data.keyword.cnc_short}}. Il file è di tipo tar compresso con un nome simile a `ibm-watson-compare-comply-prod-1.0.0.tar.gz`. Il file include i template del dashboard di registrazione e uno script `bash` per la rappresentazione dei dashboard dai template.

  1. Decomprimi ed espandi il file PPA:
    ```bash
    $ mkdir ibm-watson-compare-comply-prod-1.0.0 && tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tar.gz -C ibm-watson-compare-comply-prod-1.0.0
    ```
    {: codeblock}

  1. Passa alla directory `charts` nella directory estratta:
    ```bash
    $ cd ibm-watson-compare-comply-prod-1.0.0/charts
    ```

  1. Decomprimi ed espandi il file tar compresso nella directory `charts`:
    ```bash
    $ tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tgz
    ```

  1. Passa alla directory `dashboard`. Questa include i template per le metriche e la registrazione e uno script bash per generare i dashboard
dai template.

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

  1. Esegui lo script `render-dashboards.sh` per eseguire il rendering dei template. Le opzioni per lo script includono:
  
    -  `-v, --version {chart_version}`: la versione del grafico; ad esempio, `1.0.0`.
    -  `-h, --help`: guida e uscita del comando di stampa.
    -  `-r, --release {release_name}`: il nome della release Helm.
    -  `-n, --namespace {namespace}`: lo spazio dei nomi della distribuzione. Lo spazio dei nomi predefinito è `default`.

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

## Importazione dei dashboard di registrazione

Per importare i dashboard di registrazione per {{site.data.keyword.cnc_short}} in IBM Cloud Private, completa la seguente procedura.

  1. Accedi al tuo cluster IBM Cloud Private.

  1. Dall'icona Menu nell'angolo superiore a sinistra, seleziona **Platform -> Logging**. <br />
    ![Icona Menu di IBM Cloud Private](images/icp-menu.png) <br />
    ![Menu Platform -> Logging](images/icp-logging.png)

  1. Fai clic su **Management** sul lato sinistro dell'interfaccia Kibana. <br />
    ![Interfaccia Kibana](images/kibana.png)

  1. Seleziona la scheda **Saved Objects**.
    ![Scheda Saved Objects](images/saved-obj.png)

  1. Seleziona la scheda **Searches** e fai clic su **Import**.
    ![Import dalla scheda Searches](images/searches-import.png)

  1. Importa individualmente i file `frontend-logging.json` e `external-process-logging.json` che sono stati generati nel passo 6 della procedura precedente. Quando richiesto, fai clic su **Yes, overwrite all**.
     ![Prompt Yes, overwrite all](images/overwrite-all.png)

  1. I dashboard vengono visualizzati nella scheda **Searches**.
     ![Dashboard nella scheda Searches](images/searches-tab.png)

## Visualizzazione dei dashboard di registrazione

Per visualizzare i dashboard di registrazione, completa la seguente procedura.

  1. Passa alla scheda **Discover**.

  1. Fai clic su **Open** accanto al lato superiore destro dell'interfaccia Kibana.

  1. Seleziona il dashboard che vuoi visualizzare. Ci sono due dashboard di registrazione, uno per il log di servizio e l'altro per il log del processo esterno.
    ![Visualizza i dashboard di registrazione](images/kibana-dboards.png)

Puoi facilmente modificare l'intervallo di tempo e la frequenza dell'aggiornamento automatico:
  ![Modifica l'intervallo di tempo e la frequenza di aggiornamento](images/log-dboard-change.png)

