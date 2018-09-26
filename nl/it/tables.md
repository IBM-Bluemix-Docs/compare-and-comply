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

# Comprensione dell'analisi della tabella 
{: #understanding_tables}

Dopo che un documento è stato arricchito dal servizio, tale documento contiene un array `tables`. Ogni oggetto nell'array descrive una tabella identificata nel documento di input dal servizio. Consulta [Comprensione dello schema di output](/docs/services/compare-and-comply/schema.html#output_schema) per informazione sul formato di analisi della tabella.

La seguente è una tabella di esempio da un documento di input.
 ![Tabella di esempio](images/example-table.png)

La tabella è composta nel seguente modo:
 ![Composizione tabella](images/table-comp.png)
 
dove:

<ul>
  <li><strong><em>Testo in corsivo in grassetto</em></strong> indica un'intestazione della tabella</li>
  <li><strong>Testo in grassetto</strong> indica un'intestazione della colonna</li>
  <li><em>Testo in corsivo</em> indica un'intestazione della riga</li>
  <li>Il testo senza stile indica una cella del corpo</li>
</ul>
  
L'output dal servizio rappresenta la prima cella del corpo di esempio (cioè, la prima cella nella riga 3 con un valore di `35.0%`) come di seguito.

```
"tables": [ {
    "table": {
      "begin": 872,
      "end": 5879
    },
    "table_text": "...",
    "row_headers": [ {
      "id": "rowHeader-2244-2262",
      "cell": {
        "begin": 2244,
        "end": 2263
      },
      "cell_text": "Statutory tax rate",
      "row_index_min": 2,
      "row_index_max": 2,
      "column_index_min": 0,
      "column_index_max": 0
    },
    ... 
    ],
    "column_headers": [ {
      "id": "colHeader-1050-1082",
      "cell": {
        "begin": 1050,
        "end": 1083
      },
      "cell_text": "Three months ended September 30,",
      "row_index_min": 0,
      "row_index_max": 0,
      "column_index_min": 1,
      "column_index_max": 2
      }, {
      "id": "colHeader-1544-1548",
      "cell": {
        "begin": 1544,
        "end": 1549
      },
      "cell_text": "2005",
      "row_index_min": 1,
      "row_index_max": 1,
      "column_index_min": 1,
      "column_index_max": 1
    },
    ...
    ],
    "body_cells": [ {
      "id": "bodyCell-2450-2455",
      "cell": {
        "begin": 2450,
        "end": 2456
      },
      "cell_text": "35.0%",
      "row_index_min": 2,
      "row_index_max": 2,
      "column_index_min": 1,
      "column_index_max": 1,
      "row_header_ids": [ "rowHeader-2244-2262"],
      "row_header_texts": [ "Statutory tax rate"],
      "column_header_ids": [ "colHeader-1050-1082", "colHeader-1544-1548"],
      "column_header_texts": [ "Three months ended September 30,", "2005"]
      },
    ...
    ],
      "section_title": { },
      "section_title_text": "",
      "table_headers" : [ ]
    } 
]
```
    
