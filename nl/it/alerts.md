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

# Utilizzo degli avvisi
{: #using-alerts}

Puoi configurare gli avvisi per il tuo cluster {{site.data.keyword.cnc_short}}.

## Installazione del dashboard degli avvisi

Per installare il dashboard degli avvisi per {{site.data.keyword.cnc_short}}, completa la seguente procedura.

 1. Scarica il file PPA (Passport Advantage) per {{site.data.keyword.cnc_short}}. Il file è di tipo tar compresso con un nome simile a `ibm-watson-compare-comply-prod-1.0.0.tar.gz`. Il file include il template del dashboard degli avvisi e uno script `bash` per la rappresentazione del dashboard dal template.

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

## Aggiunta delle regole di avviso

Per aggiungere le regole di avviso al dashboard, completa la seguente procedura.

  1. Accedi al tuo cluster ICP.

  1. Dall'icona Menu nell'angolo superiore a sinistra, seleziona **Configuration -> ConfigMaps**.
      ![Icona Menu di IBM Cloud Private](images/icp-menu.png) <br />
      ![Menu Configuration -> ConfigMaps](images/configmaps.png)

  1. Si apre la pagina **ConfigMaps** che visualizza una tabella di mappe di configurazione. Nella tabella, individua la riga etichettata `alert-rules`. Nella colonna **Action** della riga `alert-rules`, fai clic sull'icona di menu e seleziona **Edit**.
     ![Modifica di alert-rules](images/configmaps-page.png)

  1. Apri il file `.../ibm-watson-compare-comply-prod-1.0.0/charts/ibm-watson-compare-comply-prod/dashboard/alerts.json` in un editor di testo e copia la riga che inizia con `cnc.rules`.

  1. Viene visualizzata la finestra **Edit ConfigMap**. Nell'oggetto `data`, aggiungi una virgola alla fine dell'ultima riga dell'oggetto, quindi incolla la riga `cnc.rules` che hai copiato nel passo precedente. <br />
     ![Modifica della ConfigMap](images/edit-configmap.png)

  1. Fai clic su **Submit** nella finestra **Edit ConfigMap**.

## Visualizzazione delle regole di avviso

Per visualizzare l'elenco delle regole di avviso, completa la seguente procedura.

  1. Vai al dashboard Prometheus nel tuo cluster IBM Cloud Private. Il dashboard Prometheus si trova all'indirizzo `https://{ICP_cluster_IP_address}:{ICP_cluster_port}/prometheus`.

  1. Fai clic sulla scheda **Alerts**. Il dashboard Prometheus visualizza un elenco di tutte le regole di avviso e il numero di avvisi attivi per ciascuno. <br />
    ![Avvisi Prometheus](images/prometheus-dboard.png)

## Aggiunta di notifiche di avviso

Puoi aggiungere notifiche di avviso per numerosi sistemi di paging, tra cui Slack, PagerDuty, HipChat, e-mail e altri. Prometheus fornisce supporto per le notifiche come documentato nei seguenti siti:

 - [Documentazione della configurazione di avvisi Prometheus ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://prometheus.io/docs/alerting/configuration/){: new_window}
 - [Documentazione degli esempi di notifica Prometheus ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://prometheus.io/docs/alerting/notification_examples/){: new_window}

Per creare un ricevitore di notifiche per {{site.data.keyword.cnc_short}} su IBM Cloud Private, completa la seguente procedura.
{: #create-notification-receiver}

  1. Accedi al tuo cluster ICP.

  1. Dall'icona Menu nell'angolo superiore a sinistra, seleziona **Configuration -> ConfigMaps**. <br />
      ![Icona Menu di IBM Cloud Private](images/icp-menu.png) <br />
      ![Menu Configuration -> ConfigMaps](images/configmaps.png)

  1. Si apre la pagina **ConfigMaps** che visualizza una tabella di mappe di configurazione. Nella tabella, individua la riga etichettata `monitoring-prometheus-alertmanager`. Nella colonna **Action** della riga `monitoring-prometheus-alertmanager`, fai clic sull'icona di menu e seleziona **Edit**.

  1. Viene visualizzata la finestra **Edit ConfigMap**. Nell'oggetto `data`, immetti le nuove configurazioni del ricevitore.
     ![Modifica della ConfigMap](images/prom-alert-edit.png)

  1. Fai clic su **Submit** nella finestra **Edit ConfigMap**.

### Esempi

Per creare una notifica Slack, completa la seguente procedura.

  1. Verifica che il canale Slack di destinazione esista. Se non esiste, creane uno. Per i dettagli, consulta la [documentazione Slack per la creazione di un canale ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://get.slack.help/hc/en-us/articles/201402297-Create-a-channel){: new_window}.

  1. Ottieni o crea il WebHook per il canale Slack. Per i dettagli, consulta la [documentazione Slack per i WebHook ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://get.slack.help/hc/en-us/articles/115005265063-Incoming-WebHooks-for-Slack){: new_window}.

  1. Apri la mappa di configurazione `monitoring-prometheus-alertmanager` nell'editor ConfigMap come descritto in [Aggiunta di notifiche di avviso](#create-notification-receiver).

  1. Aggiorna l'oggetto `data` in ConfigMap nel seguente modo:
    ```
    "data": {
      "alertmanager.yml": "global: \n  slack_api_url: '{WebHook_URL_for_Slack_channel}' \nreceivers: \n  - name: default-receiver \n    slack_configs: \n    - channel: '#{Slack_channel}' \n      send_resolved: true \nroute: \n  receiver: default-receiver \n  routes: \n  - match: \n    severity: critical \n   receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. Nella finestra **Edit ConfigMap**, fai clic su **Submit**.

Per creare una notifica PagerDuty, completa la seguente procedura.

  1. Verifica che il servizio PagerDuty esista. Se non esiste, creane uno. Per i dettagli, consulta la [documentazione PagerDuty ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://v2.developer.pagerduty.com/docs){: new_window}.

  1. Ottieni la chiave di integrazione PagerDuty aggiungendo l'integrazione Prometheus. Per i dettagli, consulta la [documentazione API PagerDuty ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://v2.developer.pagerduty.com/docs/events-api){: new_window}.

  1. Apri la mappa di configurazione `monitoring-prometheus-alertmanager` nell'editor ConfigMap come descritto in [Aggiunta di notifiche di avviso](#create-notification-receiver).

  1. Aggiorna l'oggetto `data` in ConfigMap nel seguente modo:
    ```
    "data": {
      "alertmanager.yml": "global:\nreceivers:\n  - name: default-receiver\n    pagerduty_configs:\n    - service_key: ' {PagerDuty_integration_key}'\nroute:\n  receiver: default-receiver\n  routes:\n  - match:\n      severity: critical\n    receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. Nella finestra **Edit ConfigMap**, fai clic su **Submit**.
  
