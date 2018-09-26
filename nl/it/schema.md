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

# Comprensione dello schema di output 
{: #output_schema}

Dopo che un documento è stato analizzato e arricchito dal servizio, il servizio fornisce l'output JSON nel seguente schema.

```
{
    "document_text": string,
    "document_title": string,
    "elements": [
        {
          "sentence": { "begin": int, "end": int },
          "sentence_text": string,
          "types": [
              {
                "label": { "nature": string, "party": string },
                "assurance": "High" or "Low",
                "provenance": [
                    {
                      "id": string
                    }
                    ...
                ]
              }
              ...
          ],
"categories": [
	{
                "label": string,
                "assurance": "High" or "Low",
                "provenance": [
                   {
                     "id": string
                   }
                ]
              }
              ...
          ]
	  "attributes": [
              {
              	"type": string,
              	"text": string,
              	"attribute": {"begin": int, "end": int}
              }
          ]
        }
        ...
    ], 
    "tables": [
      {
        "table": {
          "begin": int,
          "end": int
        },
        "table_text": string,
        "section_title": {
          "begin": int,
          "end": int
        },
        "section_title_text": string,
        "table_headers" : [
          {
            "id": string,
            "cell": {
              "begin": int,
              "end": int
            },
            "cell_text": string,
            "row_index_min": int,
            "row_index_max": int,
            "column_index_min"  int,
            "column_index_max": int
          },
          ...
        ],
        "column_headers": [
         {
          "id": string,
          "cell": {
            "begin": int,
            "end": int
          },
          "cell_text": string,
          "row_index_min": int,
          "row_index_max": int,
          "column_index_min": int,
          "column_index_max": int
        },
        ...
      ],
      "row_headers": [
        {
            "id": string,	
            "cell": {
              "begin": int,
              "end": int
            },
            "cell_text": string,
            "row_index_min": int,
            "row_index_max": int,
            "column_index_min": int,
            "column_index_max": int
            },
            ...
          ],
          "body_cells": [
          {
            "id": string,
            "cell": {
              "begin": int,
              "end": int
            },
            "cell_text": string,
            "row_index_min": int,
            "row_index_max": int,
            "column_index_min": int,
            "column_index_max": int,
            "row_header_ids": [string],
            "row_header_texts": [string],
            "column_header_ids": [string],
          "column_header_texts": [string]
          },
        ...  
        ]
      },
      ...
    ]
}
```

Lo schema è organizzato come segue. 

 - `document_text`: il testo completo del documento analizzato in formato HTML.
 - `document_title`: il titolo del documento analizzato. Se il servizio non ha rilevato un titolo, il valore di questo elemento è `null`.
 - `elements`: un array degli elementi del documento rilevati dal servizio.
    - `sentence`: l'ubicazione della frase come definita dai suoi indici begin e end.
    - `sentence_text`: il testo della frase.
    - `types`: un array che descrive cos'è l'elemento e a chi interessa. È costituito da una o più serie di chiavi `nature` (l'effetto della frase sulla `parte` identificata) e chiavi `party` (a chi interessa la frase).
      - `label`: un array degli elementi `nature` e `party`.
        - `nature`: il tipo di azione che la frase richiede. I valori possibili sono `Definition`, `Disclaimer`, `Exclusion`, `Obligation` e `Right`.
        - `party`: una stringa che identifica la parte a cui si applica la frase.
      - `assurance`: la fiducia applicabile al tipo rilevato. I valori possibili sono `High` e `Low`.
      - `provenance`: ogni oggetto presente negli array `types` e `categories` include un oggetto `provenance`. L'oggetto ha una o più chiavi `id`. 
        - `id`: un valore di hash che puoi inviare a IBM per fornire un feedback o ricevere supporto.
    - `categories`: un array che elenca le categorie funzionali in cui rientra la frase; in altre parole, l'oggetto della frase.
      - `label`: la categoria in cui cade la frase. Consulta [Categorie per la comprensione dell'analisi del contratto](/docs/services/compare-and-comply/parsing.html#contract_categories) per un elenco di categorie.
      - `assurance`: la fiducia applicabile alla categoria rilevata. I valori possibili sono `High` e `Low`.
      - `provenance`: ogni oggetto presente negli array `types` e `categories` include un oggetto `provenance` che include una o più chiavi `id`. 
        - `id`: un valore di hash che puoi inviare a IBM per fornire un feedback o ricevere supporto.
    - `attributes`: un array degli elementi `type`, `text` e `attribute`.
      - `type`: il tipo di attributo. I valori possibili sono `Location`, `DateTime` e `Currency`.
      - `text`: il testo associato all'attributo.
      - `attribute`: l'ubicazione dell'attributo come definito dai relativi indici `begin` e `end`.


  - `tables`: un array che definisce le tabelle identificate dal servizio.
    - `table`: l'ubicazione della tabella corrente come definito dai relativi offset `begin` e `end` nel documento di input.
    - `table_text`: il contenuto testuale della tabella corrente dal documento di input senza il contenuto di markup associato.
    - `column_headers`: un array di celle al livello della colonna, ognuna applicabile come un'intestazione di altre celle nella stessa colonna, della tabella corrente. Ogni intestazione della colonna viene definita come una raccolta di quanto segue:
      - `id`: un valore di stringa nel formato `columnHeader-x-y`, dove `x` e `y` sono gli offset begin e end di questa cella di intestazione della colonna nel documento di input.
      - `cell`: l'ubicazione di questa cella nella tabella corrente come definito dai relativi offset `begin` e `end` nel documento di input.
      - `cell_text`: il contenuto testuale di questa cella dal documento di input senza il contenuto di markup associato. 
      - `row_index_min`: l'indice begin dell'ubicazione `row` di questa cella nella tabella corrente.
      - `row_index_max`: l'indice end dell'ubicazione `row` di questa cella nella tabella corrente.
      - `column_index_min`: l'indice begin dell'ubicazione `column` di questa cella nella tabella corrente.
      - `column_index_max`: l'indice end dell'ubicazione `column` di questa cella nella tabella corrente.
    - `row_headers`: un array di celle al livello della riga, ognuna applicabile come un'intestazione di altre celle nella stessa riga, della tabella corrente. Ogni intestazione della riga viene definita come una raccolta di quanto segue:
      - `id`: un valore di stringa nel formato `rowHeader-x-y`, dove `x` e `y` sono gli offset begin e end di questa cella di intestazione della riga nel documento di input.
      - `cell`: l'ubicazione di questa cella nella tabella corrente come definito dai relativi offset `begin` e `end` nel documento di input.
      - `cell_text`: il contenuto testuale di questa cella dal documento di input senza il contenuto di markup associato. 
      - `row_index_min`: l'indice begin dell'ubicazione `row` di questa cella nella tabella corrente.
      - `row_index_max`: l'indice end dell'ubicazione `row` di questa cella nella tabella corrente.
      - `column_index_min`: l'indice begin dell'ubicazione `column` di questa cella nella tabella corrente.
      - `column_index_max`: l'indice end dell'ubicazione `column` di questa cella nella tabella corrente.
    - `body_cells`: un array di celle che non sono intestazioni di tabella, colonna o riga, della tabella corrente con le associazioni di intestazione di colonna e riga corrispondenti. Ogni cella del corpo viene definita come una raccolta di quanto segue:
      - `id`: un valore di stringa nel formato `bodyCell-x-y`, dove `x` e `y` sono gli offset begin e end di questa cella del corpo nel documento di input.
      - `cell`: l'ubicazione di questa cella nella tabella corrente come definito dai relativi offset `begin` e `end` nel documento di input.
      - `cell_text`: il contenuto testuale di questa cella dal documento di input senza il contenuto di markup associato. 
      - `row_index_min`: l'indice begin dell'ubicazione `row` di questa cella nella tabella corrente.
      - `row_index_max`: l'indice end dell'ubicazione `row` di questa cella nella tabella corrente.
      - `column_index_min`: l'indice begin dell'ubicazione `column` di questa cella nella tabella corrente.
      - `column_index_max`: l'indice end dell'ubicazione `column` di questa cella nella tabella corrente.
      - `row_header_ids`: un array di valori, ognuno che inizia con il valore `id` di un'intestazione della riga applicabile a questa cella del corpo.
      - `row_header_texts`: un array di valori, ognuno che inizia con il valore `cell_text` di un'intestazione della riga applicabile a questa cella del corpo.
      - `column_header_ids`: un array di valori, ognuno che inizia con il valore `id` di un'intestazione della colonna applicabile a questa cella del corpo.
      - `column_header_texts`: un array di valori, ognuno che inizia con il valore `cell_text` di un'intestazione della colonna applicabile a questa cella del corpo.
    - `section_title`: se identificato, l'ubicazione di un titolo della sezione che contiene la tabella corrente, come definito dai relativi offset begin e end nel documento di input originale. Vuoto se non viene identificato alcun titolo di sezione.
    - `section_title_text`: se identificato, il contenuto testuale del titolo della sezione che contiene la tabella corrente, senza il contenuto di markup associato. Vuoto se non viene identificato alcun titolo di sezione.
    - `table_headers`: un array di celle al livello della tabella applicabili come intestazioni per tutte le altre celle della tabella corrente. Ogni intestazione della tabella viene definita come una raccolta dei seguenti elementi:
      - `id`: un valore di stringa nel formato `tableHeader-x-y` dove `x` e `y` sono gli offset begin e end del valore della cella nel documento di input originale.
      - `cell`: l'ubicazione della cella di intestazione della tabella nella tabella corrente come definito dai relativi offset begin e end nel documento di input originale.
      - `cell_text`: il contenuto testuale di questa cella dal documento di input senza il contenuto di markup associato. 
      - `row_index_min`: l'indice begin dell'ubicazione della riga di questa cella nella tabella corrente.
      - `row_index_max`: l'indice end dell'ubicazione della riga di questa cella nella tabella corrente.
      - `column_index_min`: l'indice begin dell'ubicazione `column` di questa cella nella tabella corrente.
      - `column_index_max`: l'indice end dell'ubicazione della colonna di questa cella nella tabella corrente.
      
**Nota:**
  - I valori di indice riga e colonna per la cella sono basati su zero e iniziano quindi con `0`.
  - Più valori negli array degli elementi `row_header_ids` e `row_header_texts` indicano una gerarchia di intestazioni della riga.
  - Più valori negli array degli elementi `column_header_ids` e `column_header_texts` indicano una gerarchia di intestazioni della colonna.
