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

# Informationen zum Ausgabeschema
{: #output_schema}

Nachdem ein Dokument durch den Service syntaktisch analysiert und aufbereitet wurde, stellt der Service JSON-Ausgabe nach folgendem Schema bereit.

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

Das Schema ist wie folgt angeordnet. 

 - `document_text`: Der vollständige Text des syntaktisch analysierten Dokuments in HTML-Format.
 - `document_title`: Der Titel des syntaktisch analysierten Dokuments. Wenn der Service keinen Titel erkannt hat, ist der Wert dieses Elements `null`.
 - `elements`: Ein Array mit den Dokumentelementen, die vom Service erkannt wurden.
    - `sentence`: Die Position des Satzes, die durch den Anfangs- und den Endindex definiert ist.
    - `sentence_text`: Der Text des Satzes.
    - `types`: Ein Array, das beschreibt, was das Element ist und worauf es sich auswirkt. Es besteht aus einem oder mehreren Sätzen von `nature`-Schlüsseln (d. h. der Auswirkung des Satzes auf die angegebene `party`) und `party`-Schlüsseln (d. h. den vom Satz Betroffenen).
      - `label`: Ein Array mit `nature`- und `party`-Elementen.
        - `nature`: Die Art von Aktion, die der Satz erfordert. Gültige Werte sind `Definition`, `Disclaimer`, `Exclusion`, `Obligation` und `Right`.
        - `party`: Eine Zeichenfolge, die die Partei angibt, für die der Satz gilt.
      - `assurance`: Die Konfidenz, dass der erkannte Typ gültig ist. Mögliche Werte sind `High` und `Low`.
      - `provenance`: Jedes Objekt in den `types`- und `categories`-Arrays enthält ein Objekt vom Typ `provenance`. Das Objekt verfügt über einen oder mehrere Schlüssel vom Typ `id`.
        - `id`: Ein Hashwert, den Sie an IBM senden können, um Feedback zu liefern oder Unterstützung zu erhalten.
    - `categories`: Ein Array, das die funktionalen Kategorien auflistet, in die der Satz fällt, anders ausgedrückt also das Thema oder der Gegenstand des Satzes.
      - `label`: Die Kategorie, in die der Satz fällt. Eine Liste der Kategorien finden Sie unter [Categories at Understanding contract parsing](/docs/services/compare-and-comply/parsing.html#contract_categories).
      - `assurance`: Die Konfidenz, dass die erkannte Kategorie gültig ist. Mögliche Werte sind `High` und `Low`.
      - `provenance`: Jedes Objekt in den `types`- und `categories`-Arrays enthält ein Objekt vom Typ `provenance`, das einen oder mehrere `id`-Schlüssel enthält.
        - `id`: Ein Hashwert, den Sie an IBM senden können, um Feedback zu liefern oder Unterstützung zu erhalten.
    - `attributes`: Ein Array mit `type`-, `text`- und `attribute`-Elementen.
      - `type`: Der Typ von Attribut. Mögliche Werte sind `Location`, `DateTime` und `Currency`.
      - `text`: Der Text, der dem Attribut zugeordnet ist.
      - `attribute`: Die Position des Attributs, die durch die `begin`- und `end`-Indizes definiert ist.


  - `tables`: Ein Array, der die vom Service angegebenen Tabellen definiert.
    - `table`: Die Position der aktuellen Tabelle, die durch die `begin`- und `end`-Offsets im Eingabedokument definiert ist.
    - `table_text`: Der Textinhalt der aktuellen Tabelle aus dem Eingabedokument ohne zugehörigen Markup-Inhalt.
    - `column_headers`: Ein Array von Zellen der aktuellen Tabelle auf Spaltenebene, die in derselben Spalte jeweils als Überschrift für andere Zellen anwendbar sind. Jede Spaltenüberschrift ist als Sammlung der folgenden Elemente definiert:
      - `id`: Ein Zeichenfolgewert im Format `columnHeader-x-y`, wobei `x` und `y` die 'begin'- bzw. 'end'-Offsets dieser Spaltenüberschriftszelle im Eingabedokument sind.
      - `cell`: Die Position dieser Zelle in der aktuellen Tabelle, die durch die `begin`- und `end`-Offsets im Eingabedokument definiert ist.
      - `cell_text`: Der Textinhalt dieser Zelle aus dem Eingabedokument ohne zugehörigen Markup-Inhalt. 
      - `row_index_min`: Der Anfangsindex (begin) der `row`-Position dieser Zelle in der aktuellen Tabelle.
      - `row_index_max`: Der Endindex (end) der `row`-Position dieser Zelle in der aktuellen Tabelle.
      - `column_index_min`: Der Anfangsindex (begin) der `column`-Position dieser Zelle in der aktuellen Tabelle.
      - `column_index_max`: Der Endindex (end) der `column`-Position dieser Zelle in der aktuellen Tabelle.
    - `row_headers`: Ein Array von Zellen der aktuellen Tabelle auf Zeilenebene, die in derselben Zeile jeweils als Überschrift für andere Zellen anwendbar sind. Jede Zeilenüberschrift ist als Sammlung der folgenden Elemente definiert:
      - `id`: Ein Zeichenfolgewert im Format `rowHeader-x-y`, wobei `x` und `y` die 'begin'- bzw. 'end'-Offsets dieser Zeilenüberschriftszelle im Eingabedokument sind.
      - `cell`: Die Position dieser Zelle in der aktuellen Tabelle, die durch die `begin`- und `end`-Offsets im Eingabedokument definiert ist.
      - `cell_text`: Der Textinhalt dieser Zelle aus dem Eingabedokument ohne zugehörigen Markup-Inhalt. 
      - `row_index_min`: Der Anfangsindex (begin) der `row`-Position dieser Zelle in der aktuellen Tabelle.
      - `row_index_max`: Der Endindex (end) der `row`-Position dieser Zelle in der aktuellen Tabelle.
      - `column_index_min`: Der Anfangsindex (begin) der `column`-Position dieser Zelle in der aktuellen Tabelle.
      - `column_index_max`: Der Endindex (end) der `column`-Position dieser Zelle in der aktuellen Tabelle.
    - `body_cells`: Ein Array von Zellen der aktuellen Tabelle, die keine Tabellenüberschrifts-, Spaltenüberschrifts- oder Zeilenüberschriftszellen sind, mit entsprechenden Zeilen- und Spaltenüberschriftszuordnungen. Jede Hauptteilzelle ist als Sammlung der folgenden Elemente definiert:
      - `id`: Ein Zeichenfolgewert im Format `bodyCell-x-y`, wobei `x` und `y` die 'begin' bzw. 'end'-Offsets dieser Hauptteilzelle im Eingabedokument sind.
      - `cell`: Die Position dieser Zelle in der aktuellen Tabelle, die durch die `begin`- und `end`-Offsets im Eingabedokument definiert ist.
      - `cell_text`: Der Textinhalt dieser Zelle aus dem Eingabedokument ohne zugehörigen Markup-Inhalt. 
      - `row_index_min`: Der Anfangsindex (begin) der `row`-Position dieser Zelle in der aktuellen Tabelle.
      - `row_index_max`: Der Endindex (end) der `row`-Position dieser Zelle in der aktuellen Tabelle.
      - `column_index_min`: Der Anfangsindex (begin) der `column`-Position dieser Zelle in der aktuellen Tabelle.
      - `column_index_max`: Der Endindex (end) der `column`-Position dieser Zelle in der aktuellen Tabelle.
      - `row_header_ids`: Ein Array von Werten, wobei jeder Wert der `id`-Wert einer Zeilenüberschrift ist, die auf diese Hauptteilzelle anwendbar ist.
      - `row_header_texts`: Ein Array von Werten, wobei jeder Wert der `cell_text`-Wert einer Zeilenüberschrift ist, die auf diese Hauptteilzelle anwendbar ist.
      - `column_header_ids`: Ein Array von Werten, wobei jeder Wert der `id`-Wert einer Spaltenüberschrift ist, die auf diese Hauptteilzelle anwendbar ist.
      - `column_header_texts`: Ein Array von Werten, wobei jeder Wert der `cell_text`-Wert einer Spaltenüberschrift ist, die auf diese Hauptteilzelle anwendbar ist.
    - `section_title`: Falls angegeben, ist dies die Position eines Abschnittstitels, der die aktuelle Tabelle enthält; sie ist durch die 'begin'- und 'end'-Offsets im ursprünglichen Eingabedokument definiert. Wenn kein Abschnittstitel angegeben ist, ist dieses Element leer.
    - `section_title_text`: Falls angegeben, ist dies der Textinhalt des Abschnittstitels, der die aktuelle Tabelle enthält, ohne zugeordneten Markup-Inhalt. Wenn kein Abschnittstitel angegeben ist, ist dieses Element leer.
    - `table_headers`: Ein Array von Zellen auf Tabellenebene, die als Überschriften für alle anderen Zellen der aktuellen Tabelle anwendbar sind. Jede Tabellenüberschrift ist als Sammlung der folgenden Elemente definiert:
      - `id`: Zeichenfolgewert im Format `tableHeader-x-y`, wobei `x` und `y` die 'begin'- und 'end'-Offsets des Zellenwerts im ursprünglichen Eingabedokument sind.
      - `cell`: Die Position der Tabellenüberschriftszelle in der aktuellen Tabelle, die durch die 'begin'- und 'end'-Offsets im ursprünglichen Eingabedokument definiert sind.
      - `cell_text`: Der Textinhalt dieser Zelle aus dem Eingabedokument ohne zugehörigen Markup-Inhalt. 
      - `row_index_min`: Der Anfangsindex (begin) der Zeilenposition dieser Zelle in der aktuellen Tabelle.
      - `row_index_max`: Der Endindex (end) der Zeilenposition dieser Zelle in der aktuellen Tabelle.
      - `column_index_min`: Der Anfangsindex (begin) der `column`-Position dieser Zelle in der aktuellen Tabelle.
      - `column_index_max`: Der Endindex (end) der Spaltenposition dieser Zelle in der aktuellen Tabelle.
      
**Anmerkung:**
  - Zeilen- und Spaltenindexwerte der einzelnen Zellen sind null-basiert und beginnen daher mit `0`.
  - Mehrere Werte in Arrays von `row_header_ids`- und `row_header_texts`-Elementen geben eine Hierarchie von Zeilenüberschriften an.
  - Mehrere Werte in Arrays von `column_header_ids`- und `column_header_texts`-Elementen geben eine Hierarchie von Spaltenüberschriften an.
