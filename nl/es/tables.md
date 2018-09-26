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

# Comprensión del análisis de tablas
{: #understanding_tables}

Cuando el servicio haya enriquecido un documento, dicho documento contendrá una matriz de `tablas`. Cada objeto de la matriz describe una tabla que el servicio identificada en el documento de entrada. Consulte [Comprensión del esquema de salida](/docs/services/compare-and-comply/schema.html#output_schema) para obtener información acerca del formato de análisis de la tabla.

A continuación, se muestra una tabla de ejemplo de un documento de entrada. ![Tabla de ejemplo](images/example-table.png)

La tabla está compuesta de la siguiente forma:
 ![Composición de la tabla](images/table-comp.png)
 
donde:

<ul>
  <li><strong><em>Texto en cursiva y negrita</em></strong> indica una cabecera de tabla</li>
  <li><strong>Texto en negrita</strong> indica una cabecera de columna</li>
  <li><em>Texto en cursiva</em> indica una cabecera de fila</li>
  <li>Texto sin estilo indica una celda de cuerpo</li>
</ul>
  
La salida del servicio representa la primera celda del cuerpo del ejemplo (es decir, la primera celda en la fila 3 con un valor de `35.0%`) como se indica a continuación.

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
    
