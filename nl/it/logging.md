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

# Utilizzo della registrazione
{: #logging}

## Importazione dei dashboard di registrazione

Per importare i dashboard di registrazione per {{site.data.keyword.cnc_short}} in IBM Cloud Private, completa la seguente procedura.

  1. Assicurarti di aver estratto e generato i dashboard di registrazione come descritto in [Passo 1: scarica, estrai e rendering dei template del dashboard](/docs/services/compare-and-comply/monitor.html#monitor).

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
{: #view}

Per visualizzare i dashboard di registrazione, completa la seguente procedura.

  1. Passa alla scheda **Discover**.

  1. Fai clic su **Open** accanto al lato superiore destro dell'interfaccia Kibana.

  1. Seleziona il dashboard che vuoi visualizzare. Ci sono due dashboard di registrazione, uno per il log di servizio e l'altro per il log del processo esterno.
    ![Visualizza i dashboard di registrazione](images/kibana-dboards.png)

Puoi facilmente modificare l'intervallo di tempo e la frequenza dell'aggiornamento automatico:
  ![Modifica l'intervallo di tempo e la frequenza di aggiornamento](images/log-dboard-change.png)

