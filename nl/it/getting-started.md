---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-22"

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

# Introduzione
{: #getting_started}

In questa breve esercitazione, introduciamo {{site.data.keyword.cnc_long}} su IBM Cloud Private e illustriamo il processo di analisi di un contratto per identificare le parti componenti, la loro natura, le parti interessate e qualsiasi categoria identificata.

## Prima di iniziare
{: #before-you-begin}

Prima di poter utilizzare il servizio {{site.data.keyword.cnc_short}}, devi installare la CLI di IBM Cloud Private e accedere al tuo cluster IBM Cloud Private come descritto in [Installazione della CLI IBM Cloud Private](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/install_cli.html).
 
## Passo 1: identifica il contenuto
{: #identify_content}

Identifica i documenti appropriati da analizzare. {{site.data.keyword.cnc_short}} è stato progettato per analizzare i documenti <!-- and regulatory -->contrattuali che soddisfano i seguenti criteri:

- I file da analizzare sono in formato PDF.
- I contenuti del PDF sono in formato testo. I documenti che sono stati scansionati non possono essere analizzati, anche se le scansioni sono state elaborate da un lettore di caratteri ottici (OCR).

  **Nota:** puoi identificare un PDF in formato testo aprendo il documento in un programma di visualizzazione PDF e utilizzando lo strumento di **selezione testo** per selezionare una singola parola. Se non puoi selezionare una singola parola nel documento, il file non può essere analizzato.

- I file non hanno una dimensione superiore a 50 Mb.
- I PDF protetti (con una password per l'apertura) e i PDF con restrizioni di modifica (con una password per la modifica) non possono essere analizzati.

## Passo 2: analizza un contratto
{: #parse_contract}

In una shell `bash` o in un ambiente equivalente come Cygwin, utilizza il metodo `POST /v1/parse` per analizzare il tuo contratto. Sostituisci `{ICP_IP_address}` con l'indirizzo IP del tuo cluster IBM Cloud Private. Sostituisci `{PDF_file}` con il percorso del PDF da analizzare.

```bash
curl -k -X POST -F 'file=@{PDF_file};type=application/pdf' https://{ICP_IP_address}/api/v1/parse?version=2018-03-23
```
{: pre}

**Importante:** se stai utilizzando una versione di {{site.data.keyword.cnc_short}} precedente alla 1.0.4, devi includere lo specificatore `:{port_number}` con l'indirizzo IP del cluster IBM Cloud Private quando effettui chiamate al servizio, come nel seguente esempio:

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://{ICP_IP_address}:{port_number}/api/v1/parse?version=2018-03-23
```
{: pre}

Ad esempio:

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

Per i dettagli, vedi la [nota sulla release relativa a **Ingress**](/docs/services/compare-and-comply/relnotes.html#ingress).

Il metodo restituisce un oggetto JSON che contiene:

 - Una conversione HTML del file PDF di origine.
 - Il titolo del documento estratto.
 - Un array di `elements` che definiscono le parti componenti del contratto.
 - Un array di `parties` che definiscono le entità identificate come parti.
 
**Importante**: l'API {{site.data.keyword.cnc_short}} su IBM Cloud Private non richiede l'autorizzazione, mentre le API su IBM Cloud la richiedono.

## Passo 3: rivedi l'analisi
{: #review_analysis}

Ogni oggetto nell'array `elements` descrive un elemento del contratto identificato da {{site.data.keyword.cnc_short}}. Il seguente codice rappresenta un elemento tipico:

```
{
  "sentence_text": "If the parties are unable to agree on the License Price within thirty (30) days after Richmond Enterprises provides the written notice of its exercise of the Optional Patent License to Savage Narwhal Studios in Section 2.3(a), such FMV Dispute shall, at either party's request, be resolved solely and exclusively by final, binding and confidential arbitration to be filed and the decision rendered in New York, New York (with hearings at the request of either party to be held in San Francisco, California or other mutually agreeable place convenient for the parties) in accordance with the Commercial Arbitration Rules of the American Arbitration Association (\" AAA \"), including as supplemented by the Procedures for Large, Complex Commercial Disputes.",
  "attributes": [
	{
		"type": "Location",
		"text": "New York",
		"attribute": {
			"begin": 58372,
			"end": 58380
		}
	},
	{
		"type": "Location",
		"text": "New York",
		"attribute": {
			"begin": 58382,
			"end": 58390
		}
	},
	{
		"type": "Location",
		"text": "San Francisco",
		"attribute": {
			"begin": 58451,
			"end": 58464
		}
	},
	{
		"type": "Location",
		"text": "California",
		"attribute": {
			"begin": 58466,
			"end": 58476
		}
	}
],
"categories": [
	{
		"label": "Communication",
		"assurance": "High",
		"provenance": [
			{
				"id": "C7xhbsepUodh09zmJdUXSvYZCdixx00wFyCZuAnTujok="
			}
		]
	},
	{
		"label": "Dispute Resolution",
		"assurance": "High",
		"provenance": [
			{
				"id": "Ck8vgUWOj41OutOOLJ38b2Q7jOj3F30ABGaGLKKxppFA="
			},
			{
				"id": "CdPeg8mAxM5YIsdpzzaluDg7jOj3F30ABGaGLKKxppFA="
			}
		]
	},
	{
		"label": "Intellectual Property",
		"assurance": "High",
		"provenance": [
			{
				"id": "Cor/mgcf1UE/zmsKm68M6+a9LSRCpcKe8EWCUdwsjrgs="
			}
		]
	}
],
"types": [
	{
		"label": {
			"nature": "Obligation",
			"party": "All Parties"
		},
		"assurance": "High",
		"provenance": [
			{
				"id": "NNpSqaNkY2zHtxI6Zh78NaZccVNtutrQxISkzdnaVjx0="
			},
			{
				"id": "PlyERkjg5is36RpFjVUFXp69eDmGmCxLCXRs1sDMDUCo="
			}
		]
	}
],
"sentence": {
	"begin": 57998,
	"end": 58952
	}
}
```

L'elemento ha cinque sezioni importanti:
 - `sentence_text`: il testo che è stato analizzato.
 - `attributes`: un array che elenca uno o più attributi dell'elemento. Gli oggetti attualmente supportati nell'array `attributes` includono `Location` (posizione geografica o regione a cui fa riferimento l'elemento), `DateTime` (data, ora, intervallo di date o intervallo di ore specificati dall'elemento) e `Currency` (valori monetari e unità). 
 - `categories`: un array che elenca le categorie funzionali in cui rientra la frase identificata; in altre parole, l'oggetto della frase.
 - `types`: un array che descrive cos'è l'elemento e a chi interessa. È costituito da una o più serie di chiavi `nature` (l'effetto della frase sulla `parte` identificata) e chiavi `party` (a chi interessa la frase).
 - `sentence`: un oggetto che descrive dove è stato trovato l'elemento nell'HTML convertito. Contiene un valore di carattere `start` e un valore di carattere `end`.

**Nota**: alcune frasi non rientrano in alcun tipo o categoria, nel qual caso il servizio restituisce gli array `types` e `categories` come oggetti vuoti.

**Nota:** alcune frasi ricoprono più argomenti, nel qual caso il servizio restituisce più serie di oggetti `types` e `categories`.

**Nota**: alcune frasi non contengono alcun attributo identificabile, nel qual caso il servizio restituisce l'array `attributes` come oggetto vuoto.

In aggiunta, tutte le parti identificate sono definite nell'array `parties`. L'array `parties` si trova dopo l'array `elements` nell'output JSON.

```
  "parties" : [ {
    "party" : "Customer",
    "role" : "Buyer"
  } ]
```

L'array `parties` include due sezioni importanti:

 - `party`: il testo che è stato identificato come parte all'interno del documento.
 - `role`: il ruolo della parte identificata. I ruoli cambiano in base al sottodominio; consulta la [documentazione sul sottodominio specificato per un elenco di possibili ruoli](/docs/services/compare-and-comply/parsing.html#contract_parties). Le parti che non possono essere identificate come aventi un ruolo specifico sono elencate con il valore `unknown`.

## Passi successivi
{: #next_steps}

Hai analizzato correttamente un contratto per identificare la natura, le parti e le categorie delle parti componenti del documento. Puoi utilizzare l'analisi per comprendere e applicare rapidamente il contratto analizzato. I passi successivi consentono di:

 - Comprendere i tipi e le categorie.
 - Esaminare le opzioni di analisi.


