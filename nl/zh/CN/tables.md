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

# 了解表解析
{: #understanding_tables}

当文档经过服务扩充之后，其中会包含一个 `tables` 数组。该数组中的每个对象都会描述一个表，它们是服务在输入文档中所识别的表。有关表解析格式的信息，请参阅[了解输出模式](/docs/services/compare-and-comply/schema.html#output_schema)。

下面是输入文档中某个表的示例。
 ![示例表](images/example-table.png)

该表的构成如下所示：
 ![表的构成](images/table-comp.png)
 
其中：

<ul>
  <li><strong><em>加粗斜体文本</em></strong>表示表头</li>
  <li><strong>粗体文本</strong>表示列标题</li>
  <li><em>斜体文本</em>表示行标题</li>
  <li>无格式文本表示正文单元格</li>
</ul>
  
服务的输出表示该示例的第一个正文单元格（也就是说，第 3 行中的第一个单元格的值为 `35.0%`），如下所示。

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
    
