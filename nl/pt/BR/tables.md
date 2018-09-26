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

# Entendendo a análise de tabela
{: #understanding_tables}

Depois que um documento tiver sido enriquecido pelo serviço, o documento enriquecido conterá uma matriz `tables`. Cada objeto na matriz descreve uma tabela identificada no documento de entrada pelo serviço. Consulte [Entendendo o esquema de saída](/docs/services/compare-and-comply/schema.html#output_schema) para obter informações sobre o formato da análise sintática da tabela.

A seguir está uma tabela de exemplo de um documento de entrada.
![Tabela de exemplo](images/example-table.png)

A tabela é composta conforme a seguir:
 ![Composição da tabela](images/table-comp.png)
 
em que:

<ul>
  <li><strong><em>Texto em itálico e negrito</em></strong> indica um cabeçalho da tabela</li>
  <li><strong> Bold text </strong>  indica um cabeçalho da coluna</li>
  <li><em> Texto em itálico </em>  indica um cabeçalho de linha</li>
  <li>O texto não estilizado indica uma célula do corpo</li>
</ul>
  
A saída do serviço representa a primeira célula do corpo do exemplo (ou seja, a primeira célula na linha três com um valor de `35,0% `) conforme a seguir.

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
      "cell_text": "Taxa de imposto estabelecida por lei",
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
      "cell_text": "Três meses terminados em 30 de setembro,",
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
      "row_header_texts": [ "Taxa de imposto estabelecida por lei"],
      "column_header_ids": [ "colHeader-1050-1082", "colHeader-1544-1548"],
      "column_header_texts": [ "Três meses terminados em 30 de setembro,", "2005"]
      },
    ...
    ],
      "section_title": { },
      "section_title_text": "",
      "table_headers" : [ ]
    } 
]
```
    
