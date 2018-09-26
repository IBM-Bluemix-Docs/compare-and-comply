---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-25"

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

# Utilizzo delle metriche
{: #metrics}

Puoi monitorare lo stato di {{site.data.keyword.cnc_short}} utilizzando il dashboard di monitoraggio di IBM Cloud Private. Il dashboard di monitoraggio utilizza Grafana per le metriche, Prometheus per gli avvisi e Kibana per la registrazione per presentare informazioni dettagliate relative alla tua istanza {{site.data.keyword.cnc_short}}.

## Importazione del dashboard delle metriche

Per importare il dashboard delle metriche per {{site.data.keyword.cnc_short}} in IBM Cloud Private, completa la seguente procedura.

  1. Assicurarti di aver estratto e generato i dashboard delle metriche come descritto in [Passo 1: scarica, estrai e rendering dei template del dashboard](/docs/services/compare-and-comply/monitor.html#monitor).

  1. Accedi al tuo cluster IBM Cloud Private.

  1. Dall'icona Menu nell'angolo superiore a sinistra, seleziona **Platform -> Monitoring**. <br />
      ![Icona Menu di IBM Cloud Private](images/icp-menu.png) <br />
      ![Menu Platform -> Monitoring](images/icp-monitoring.png)

  1. Fai clic su **Home** accanto alla parte superiore sinistra dell'interfaccia Grafana. <br />
      ![Icona Home](images/icp-home.png)

  1. Fai clic su **Import Dashboard**.
      ![Icona Import Dashboard](images/import-dboard.png)

  1. Seleziona il file `metrics.json` che è stato generato nel passo 6 della procedura precedente e fai clic su **Upload .json File**. <br />
      ![Carica il file metrics.json](images/metrics-json.png)

  1. Seleziona **Prometheus** come origine dati e fai clic su **Import**.
       ![Seleziona Prometheus](images/prometheus.png)

## Visualizzazione del dashboard delle metriche
{: #view}

Il dashboard delle metriche è simile al seguente:
![Dashboard delle metriche](images/metrics-dboard.png)

Puoi facilmente modificare l'intervallo di tempo e la frequenza dell'aggiornamento automatico:
  ![Modifica l'intervallo di tempo e la frequenza di aggiornamento](images/dboard-change.png)

## Modifica del dashboard delle metriche

Puoi modificare il dashboard delle metriche o creare un nuovo dashboard effettuando le seguenti operazioni.

  1. Dall'icona Menu nell'angolo superiore a sinistra, seleziona **Platform -> Monitoring** per accedere all'interfaccia utente Grafana.

  1. Fai clic su **Home** accanto alla parte superiore sinistra dell'interfaccia Grafana, quindi seleziona **+ New Dashboard**.

  1. Seleziona il tipo di pannello che vuoi aggiungere, ad esempio **Graph** o **Table**.

  1. Fai clic sul titolo del pannello e quindi su **Edit**. Il titolo del pannello predefinito è `Panel title`.

  1. Utilizza la scheda **General** per impostare il titolo, la descrizione e le dimensioni del pannello. Nota che 12 unità è l'intera larghezza di una finestra del browser.

  1. Utilizza la scheda **Metrics** per creare query che visualizzano i dati da Prometheus.

    1. Puoi scrivere direttamente la query se hai familiarità con il linguaggio di query oppure puoi utilizzare il campo **Metric lookup** per scegliere tra le metriche attualmente segnalate a Prometheus.

    1. I risultati delle query vengono visualizzati in tempo reale nel nuovo pannello del dashboard.

    1. È possibile aggiungere più query a un singolo pannello. Ad esempio, puoi visualizzare le operazioni di lettura e scrittura nello stesso grafico o le visite totali e i visitatori totali nella stessa tabella.
        
