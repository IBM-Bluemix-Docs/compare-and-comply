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

# 瞭解表格剖析
{: #understanding_tables}

在服務強化文件之後，強化過的文件會包含 `tables` 陣列。陣列中的每個物件會說明一個表格，該表格在輸入文件中以服務來識別。如需表格剖析格式的相關資訊，請參閱[瞭解輸出綱目](/docs/services/compare-and-comply/schema.html#output_schema)。

以下是來自輸入文件的範例表格。
 ![範例表格](images/example-table.png)

表格組合如下：
 ![表格組合](images/table-comp.png)
 
其中：

<ul>
  <li><strong><em>粗體斜體字</em></strong> 指出表格標頭</li>
  <li><strong>粗體字</strong> 指出直欄標頭</li>
  <li><em>斜體字</em> 指出列標頭</li>
  <li>無樣式的文字指出內文資料格</li>
</ul>
  
來自服務的輸出代表範例的第一個內文資料格（亦即，第 3 列第一個資料格，值為 `35.0%`），如下所述。

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
    
