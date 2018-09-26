
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

# Note sulla release
{: #release_notes}

Le note sulla release forniscono informazioni sulle modifiche apportate alla release del servizio {{site.data.keyword.cnc_long}} su {{site.data.keyword.BluOpenStackDed}}.

**Importante:** questa serie di documentazioni si applica solo al servizio {{site.data.keyword.cnc_short}} su {{site.data.keyword.BluOpenStackDed}}. Non si applica ad altri servizi Watson disponibili su IBM Cloud pubblico.

**Nota:** per informazioni sulle note della release che riguardano tutti i servizi {{site.data.keyword.BluOpenStackDed}}, consulta [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/known_issues.html#issues_nlv ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/known_issues.html#issues_nlv){: new_window}.

## Controllo delle versioni dell'API del servizio
{: #api_versioning}

Le richieste API richiedono un parametro di versione che utilizza una data nel formato `version=YYYY-MM-DD`. Ogni volta che modifichiamo l'API in modo incompatibile con le versioni precedenti, rilasciamo una nuova versione secondaria dell'API.

Invia il parametro di versione con ogni richiesta API. Il servizio utilizza la versione API per la data da te specificata o la versione più recente prima di tale data. Non impostare come valore predefinito la data corrente. Specifica invece una data che corrisponde a una versione compatibile con la tua applicazione e non modificarla finché l'applicazione non è pronta per una versione successiva.

La versione corrente è `2018-03-23`.

## Modifiche
{: #changes}

Sono disponibili le seguenti nuove funzioni e modifiche per il servizio.

**Importante**: il numero di versione a cui si fa riferimento nelle seguenti sezioni è la versione del grafico Helm {{site.data.keyword.cnc_long}} che hai distribuito sul tuo cluster {{site.data.keyword.BluOpenStackDed}}.

### 1.0.5, 2 agosto 2018
{: #105}

  - L'aggiunta di una analisi della tabella come descritto in [Comprensione dello schema di output](/docs/services/compare-and-comply/schema.html#output_schema) e [Comprensione dell'analisi della tabella](/docs/services/compare-and-comply/tables.html#understanding_tables).


### 1.0.4, 5 luglio 2018
{: #ingress}

**Importante**: a causa delle modifiche descritte in questa sezione, non puoi eseguire l'aggiornamento alla versione 1.0.4 dalle precedenti versioni di {{site.data.keyword.cnc_short}} utilizzando le procedure di aggiornamento standard di {{site.data.keyword.BluOpenStackDed}}. Devi ridistribuire {{site.data.keyword.cnc_short}} nel seguente modo:

1.  Rimuovi la distribuzione di {{site.data.keyword.cnc_short}} esistente come descritto in [Removing a deployment ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_applications/remove_app.html){: new_window}.

1.  Distribuisci {{site.data.keyword.cnc_short}} versione 1.0.4 o successiva come descritto nel file `README.md` del grafico Helm {{site.data.keyword.cnc_short}} e in [Distribuzione del servizio](/docs/services/compare-and-comply/deploy.html).

**Importante:** la ridistribuzione del servizio ha le seguenti conseguenze:

- Il servizio non è disponibile tra la rimozione dell'istanza precedente e la distribuzione dell'istanza corrente.
- Se disponi di codice che effettua chiamate API che includono numeri di porta negli URL delle chiamate, devi rimuovere i numeri di porta. Se effettui una chiamata API che include un numero di porta a {{site.data.keyword.cnc_short}} versione 1.0.4 o successiva, la chiamata non riesce e produce un errore simile al seguente:
  ```
  curl: (7) Failed to connect to 10.19.74.45 port 8443: Connection refused
  ```
  {: pre}

**Importante:** se stai utilizzando una versione di {{site.data.keyword.cnc_short}} precedente alla 1.0.4, devi includere lo specificatore `:{port_number}` con l'indirizzo IP del cluster {{site.data.keyword.BluOpenStackDed}} quando effettui chiamate al servizio, come nel seguente esempio:
```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

#### Note aggiuntive

-   Ulteriori istruzioni di distribuzione sono ora fornite in [Distribuzione del servizio](/docs/services/compare-and-comply/deploy.html).
-   Il servizio {{site.data.keyword.cnc_short}} è ora integrato con il [controller Ingress ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/components.html){: new_window} di {{site.data.keyword.BluOpenStackDed}}. Questa modifica ti consente di chiamare i metodi API del servizio senza specificare un numero di porta negli URL delle chiamate. Di seguito è riportata una tipica chiamata API prima dell'integrazione con **ingress**:

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
    ```
    {: pre}

  La chiamata equivalente nella versione 1.0.4 o successiva è:

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45/api/v1/parse?version=2018-03-23
    ```
    {: pre}

- Quando distribuisci la release 1.0.4, potresti visualizzare un errore simile al seguente:

    ```
    Unexpected response code 500 from request: GET https://icp-management-ingress:8443/helm-api/api/v1/releases/weblib?locale=en-US HTTP/1.1 Accept: application/json Content-Type: application/json Content-Type: application/json Cookie: *** {} HTTP/1.1 500 Server: openresty/1.11.2.4 Date: Thu, 21 Jun 2018 23:09:05 GMT Content-Type: application/json; charset=utf-8 Content-Length: 93 Connection: close X-Powered-By: Express Cache-Control: private, max-age=0, no-cache Etag: W/"5d-6htOwRqeFWIdvat6vK/kTg" {"statusCode":500,"message":"Internal service error : Cannot read property '0' of undefined"}
    ```
    {: codeblock}

    Puoi tranquillamente ignorare e chiudere l'errore.

### 1.0.3, 25 maggio 2018

- L'output del metodo `parse` include ora l'array `attributes`. Per informazioni, consulta [Rivedi l'analisi](/docs/services/compare-and-comply/getting-started.html#review_analysis) e [Attributi](/docs/services/compare-and-comply/parsing.html#attributes). Puoi utilizzare le informazioni contenute nell'array `attributes` per cercare elementi del documento che si riferiscono a specifiche posizioni, ore, date, intervallo di ore o intervallo di date e valore monetario e unità.
- Ogni oggetto presente negli array `types` e `categories` include un oggetto `provenance`. L'oggetto `provenance` ha una o più chiavi `id`. Ogni chiave `id` ha un valore di hash che puoi inviare a IBM per fornire un feedback o ricevere supporto.
- Miglioramento dell'accuratezza dell'analisi PDF e delle prestazioni dell'analisi di file PDF di grandi dimensioni.
- Questa release è disponibile su {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 o superiore. Consulta la voce di catalogo relativa al servizio per i dettagli sui requisiti di {{site.data.keyword.BluOpenStackDed}}.
- I valori possibili per la chiave `assurance` non includono più `Low`.

### 1.0.2, 19 aprile 2018

- Ulteriori categorie supportate. Consulta la [documentazione sulle Categorie](/docs/services/compare-and-comply/parsing.html#contract_categories) per l'elenco più recente di categorie supportate.
    - `Asset Use`
    - `Communication`
    - `Safety and Security`
-  Aggiornamenti della documentazione, comprese correzioni agli esempi descritti in [Introduzione](/docs/services/compare-and-comply/getting-started.html).

### 1.0.1 (Release General Availability), 23 marzo 2018

Le seguenti note si applicano alla release GA (General Availability) del servizio {{site.data.keyword.cnc_long}}.

- La release GA è disponibile solo su {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 o superiore. Consulta la voce di catalogo relativa al servizio per i dettagli sui requisiti di {{site.data.keyword.BluOpenStackDed}}.

## Note generali
{: #general_notes}

- L'API {{site.data.keyword.cnc_short}} su {{site.data.keyword.BluOpenStackDed}} non richiede l'autorizzazione, mentre le API su IBM Cloud la richiedono.
 - I PDF devono essere in formato testo per poter essere analizzati. I documenti che sono stati scansionati non possono essere analizzati, anche se le scansioni sono state elaborate da un lettore di caratteri ottici (OCR).

## Problemi noti
{: #known_issues}

- La dimensione massima di un file PDF che è possibile caricare è 50 Mb.
- I file PDF con la sicurezza abilitata non possono essere analizzati.
- I documenti con layout di pagina non standard (ad esempio, 2 o 3 colonne per pagina) non vengono analizzati correttamente.
