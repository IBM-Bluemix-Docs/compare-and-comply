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

# 表解析について
{: #understanding_tables}

サービスによって文書が拡充されると、拡充されたその文書には`表`配列が含まれます。その配列内の各オブジェクトは、サービスによって入力文書内で識別された表を示しています。表解析フォーマットについては、[出力スキーマについて](/docs/services/compare-and-comply/schema.html#output_schema)を参照してください。

入力文書内の表の例を以下に示します。
 ![表の例](images/example-table.png)

この表は以下のように構成されています。
 ![表の構成](images/table-comp.png)
 
各部の意味は、次のとおりです。

<ul>
  <li><strong><em>太字イタリック・テキスト</em></strong> は表ヘッダーを示しています</li>
  <li><strong>太字テキスト</strong>は列ヘッダーを示しています</li>
  <li><em>イタリック・テキスト</em> は行ヘッダーを示しています</li>
  <li>スタイル設定なしのテキストは、本文セルを示しています</li>
</ul>
  
サービスからの出力では、この例の 1 番目の本文セル (つまり、値が `35.0%` である、行 3 の第 1 セル) が以下のように表されます。

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
    
