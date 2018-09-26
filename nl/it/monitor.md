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

# Installazione, configurazione e utilizzo dei dashboard di monitoraggio
{: #monitor}

Puoi monitorare lo stato di {{site.data.keyword.cnc_short}} utilizzando i dashboard di monitoraggio di IBM Cloud Private. I dashboard di monitoraggio utilizzano Grafana, Kibana e Prometheus per visualizzare informazioni dettagliate e personalizzabili relative alla tua istanza di {{site.data.keyword.cnc_short}}.

Per informazioni sul monitoraggio del tuo cluster {{site.data.keyword.BluOpenStackDed}}, consulta [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window}.

Prima di utilizzare i dashboard, devi generarli e installarli come descritto nella seguente procedura.

## Passo 1: scarica, estrai e rendering dei template del dashboard 
{: #render}

Effettua le seguenti operazioni per preparare i template del dashboard per l'installazione.

1. Scarica l'immagine {{site.data.keyword.cnc_short}} da PPA (Passport Advantage). Il file è di tipo tar compresso con un nome simile a `ibm-watson-compare-comply-prod-1.0.5.tar.gz`. Il file include i template del dashboard e uno script `bash` per la rappresentazione dei dashboard dai template.

1. Decomprimi ed espandi il file tar:
  ```bash
  mkdir ibm-watson-compare-comply-prod-1.0.5 && tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tar.gz -C ibm-watson-compare-comply-prod-1.0.5
  ``` 
  {: pre}

1. Passa alla directory `charts` nella directory estratta:
  ```bash
  cd ibm-watson-compare-comply-prod-1.0.5/charts
  ```
  {: pre}

1. Decomprimi ed espandi il file tar compresso nella directory `charts`:
  ```bash
  tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tgz
  ```
  {: pre}

1. Passa alla directory `dashboard`. Questa include i template per gli avvisi, la registrazione e le metriche così come uno script `bash` per generare i dashboard
dai template.
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

1. Esegui lo script `render-dashboards.sh` per eseguire il rendering dei template. Le opzioni per lo script includono:
  
    - `-v`, `--version {chart_version}`: la versione del grafico; ad esempio, `1.0.5`.
    - `-h`, `--help`: guida e uscita del comando di stampa.
    - `-r`, `--release {release_name}`: il nome della release Helm.
    - `-n`, `--namespace {namespace}`: lo spazio dei nomi della distribuzione. Lo spazio dei nomi predefinito è `default`.

  ```bash
  ./render-dashboards.sh -v 1.0.5 -r my-test-release -n default
  ```
  {: pre}

I file JSON del dashboard sono generati in `ibm-watson-compare-comply-prod-1.0.5/charts/ibm-watson-compare-comply-prod/dashboard`.

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

## Passo 2: importa i template
{: #import-tpls}

Dopo che hai eseguito il rendering dei dashboard, importa uno o più di essi come descritto nelle seguenti sezioni:

  - [Importazione del dashboard delle metriche](metrics.html#import)
  - [Importazione dei dashboard di registrazione](logging.html#import)
  - [Importazione del dashboard degli avvisi](alerts.html#import)

## Passo 3: visualizza e personalizza i dashboard
{: #use-dboards}

Dopo aver importato i dashboard selezionati, visualizzali e personalizzali come descritto nelle seguenti sezioni:

  - [Visualizzazione del dashboard delle metriche](metrics.html#view)
  - [Visualizzazione dei dashboard di registrazione](logging.html#view)
  - [Aggiunta delle regole di avviso](alerts.html#add)
