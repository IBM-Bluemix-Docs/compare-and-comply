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

# Informationen zur Syntaxanalyse der Tabelle
{: #understanding_tables}

Nachdem ein Dokument durch den Service aufbereitet wurde, enthält das aufbereitete Dokument ein `tables`-Array. Jedes Objekt in dem Array beschreibt eine Tabelle, die im Eingabedokument durch den Service angegeben wird. Informationen zum Format der Syntaxanalyse der Tabelle finden Sie im Abschnitt [Informationen zum Ausgabeschema](/docs/services/compare-and-comply/schema.html#output_schema).

Im Folgenden sehen Sie eine Beispieltabelle aus einem Eingabedokument.
 ![Beispieltabelle](images/example-table.png)

Die Tabelle setzt sich wie folgt zusammen:
 ![Tabellenzusammensetzung](images/table-comp.png)
 
Hierbei gilt Folgendes:

<ul>
  <li><strong><em>Kursiver Text in Fettdruck</em></strong> zeigt eine Tabellenüberschrift an</li>
  <li><strong>Text in Fettdruck</strong> zeigt eine Spaltenüberschrift an</li>
  <li><em>Kursivtext</em> zeigt eine Zeilenüberschrift an</li>
  <li>Nicht formatierter Text zeigt eine Zelle des Hauptteils an</li>
</ul>
  
Die Ausgabe des Service stellt die erste Hauptteilzelle des Beispiels dar (das heißt die erste Zelle in Zeile 3 mit einem Wert von `35.0%`) wie folgt.

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
    
