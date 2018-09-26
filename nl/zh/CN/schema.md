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

# 了解输出模式
{: #output_schema}

在对文档进行解析和扩充之后，服务会使用以下模式来提供 JSON 输出。

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

该模式按如下方式排列。 

 - `document_text`：所解析的文档的全文，格式为 HTML。
 - `document_title`：所解析的文档的标题。如果服务未检测到标题，此元素的值为 `null`。
 - `elements`：服务检测到的文档元素的数组。
    - `sentence`：语句的位置，由其 begin 和 end 索引来定义。
    - `sentence_text`：语句的文本。
    - `types`：一个数组，用于描述元素的定义及元素影响的对象。它由一组或多组 `nature` 键（语句对识别到的 `party` 的影响）和 `party` 键（语句影响的对象）组成。
      - `label`：`nature` 和 `party` 元素的数组。
        - `nature`：语句所要求的操作的类型。可能的值为 `Definition`、`Disclaimer`、`Exclusion`、`Obligation` 和 `Right`。
        - `party`：用于标识语句所适用的当事方的字符串。
      - `assurance`：检测到的类型适用的置信度。可能的值为 `High` 和 `Low`。
      - `provenance`：`types` 和 `categories` 数组中的每个对象都包含一个 `provenance` 对象。该对象具有一个或多个 `id` 键。
        - `id`：一个散列值，您可以将其发送给 IBM 来提供反馈或获取支持。
    - `categories`：一个数组，用于列出语句所属的功能类别；换句话说，就是语句的主题。
      - `label`：语句所属的类别。有关类别的列表，请参阅[了解合同解析的类别](/docs/services/compare-and-comply/parsing.html#contract_categories)。
      - `assurance`：检测到的类别适用的置信度。可能的值为 `High` 和 `Low`。
      - `provenance`：`types` 和 `categories` 数组中的每个对象都包含一个 `provenance` 对象，其具有一个或多个 `id` 键。
        - `id`：一个散列值，您可以将其发送给 IBM 来提供反馈或获取支持。
    - `attributes`：`type`、`text` 和 `attribute` 元素的数组。
      - `type`：属性的类型。可能的值为 `Location`、`DateTime` 和 `Currency`。
      - `text`：与属性相关联的文本。
      - `attribute`：属性的位置，由其 `begin` 和 `end` 索引来定义。


  - `tables`：一个数组，用于定义由服务识别的表。
    - `table`：当前表的位置，由其在输入文档中的 `begin` 和 `end` 偏移量来定义。
    - `table_text`：输入文档中当前表的文本内容，没有关联的标记内容。
    - `column_headers`：当前表中列级单元格的数组，其中每个单元格都可用作同一列中其他单元格的标题。每个列标题都由以下内容的集合来定义：
      - `id`：格式为 `columnHeader-x-y` 的字符串值，其中 `x` 和 `y` 是输入文档中此列标题单元格的 begin 和 end 偏移量。
      - `cell`：此单元格在当前表中的位置，由其在输入文档中的 `begin` 和 `end` 偏移量来定义。
      - `cell_text`：输入文档中此单元格的文本内容，没有关联的标记内容。 
      - `row_index_min`：当前表中此单元格的 `row` 位置的 begin 索引。
      - `row_index_max`：当前表中此单元格的 `row` 位置的 end 索引。
      - `column_index_min`：当前表中此单元格的 `column` 位置的 begin 索引。
      - `column_index_max`：当前表中此单元格的 `column` 位置的 end 索引。
    - `row_headers`：当前表中行级单元格的数组，其中每个单元格都可用作同一行中其他单元格的标题。每个行标题都由以下内容的集合来定义：
      - `id`：格式为 `rowHeader-x-y` 的字符串值，其中 `x` 和 `y` 是输入文档中此行标题单元格的 begin 和 end 偏移量。
      - `cell`：此单元格在当前表中的位置，由其在输入文档中的 `begin` 和 `end` 偏移量来定义。
      - `cell_text`：输入文档中此单元格的文本内容，没有关联的标记内容。 
      - `row_index_min`：当前表中此单元格的 `row` 位置的 begin 索引。
      - `row_index_max`：当前表中此单元格的 `row` 位置的 end 索引。
      - `column_index_min`：当前表中此单元格的 `column` 位置的 begin 索引。
      - `column_index_max`：当前表中此单元格的 `column` 位置的 end 索引。
    - `body_cells`：一个单元格数组，这些单元格既不是当前表的表头，也不是列标题，也不是行标题，但与行标题和列标题具有相应的关联。每个正文单元格都由以下内容的集合来定义：
      - `id`：格式为 `bodyCell-x-y` 的字符串值，其中 `x` 和 `y` 是输入文档中此正文单元格的 begin 和 end 偏移量。
      - `cell`：此单元格在当前表中的位置，由其在输入文档中的 `begin` 和 `end` 偏移量来定义。
      - `cell_text`：输入文档中此单元格的文本内容，没有关联的标记内容。 
      - `row_index_min`：当前表中此单元格的 `row` 位置的 begin 索引。
      - `row_index_max`：当前表中此单元格的 `row` 位置的 end 索引。
      - `column_index_min`：当前表中此单元格的 `column` 位置的 begin 索引。
      - `column_index_max`：当前表中此单元格的 `column` 位置的 end 索引。
      - `row_header_ids`：一个值数组，其中每个值都是适用于此正文单元格的行标题的 `id` 值。
      - `row_header_texts`：一个值数组，其中每个值都是适用于此正文单元格的行标题的 `cell_text` 值。
      - `column_header_ids`：一个值数组，其中每个值都是适用于此正文单元格的列标题的 `id` 值。
      - `column_header_texts`：一个值数组，其中每个值都是适用于此正文单元格的列标题的 `cell_text` 值。
    - `section_title`：如果识别出节标题，那么为包含当前表的节标题的位置，由其在原始输入文档中的 begin 和 end 偏移量来定义。如果未识别出节标题，那么为空。
    - `section_title_text`：如果识别出节标题，那么为包含当前表的节标题的文本内容，但没有关联的标记内容。如果未识别出节标题，那么为空。
    - `table_headers`：一个表级单元格数组，其中每个单元格可用作当前表的所有其他单元格的标题。每个表头都由以下内容的集合来定义：
      - `id`：格式为 `tableHeader-x-y` 的字符串值，其中 `x` 和 `y` 是原始输入文档中单元格值的 begin 和 end 偏移量。
      - `cell`：当前表中表头单元格的位置，由其在原始输入文档中的 begin 和 end 偏移量来定义。
      - `cell_text`：输入文档中此单元格的文本内容，没有关联的标记内容。 
      - `row_index_min`：当前表中此单元格的行位置的 begin 索引。
      - `row_index_max`：当前表中此单元格的行位置的 end 索引。
      - `column_index_min`：当前表中此单元格的 `column` 位置的 begin 索引。
      - `column_index_max`：当前表中此单元格的列位置的 end 索引。
      
**注：**
  - 每个单元格的行索引值和列索引值都是从零开始的，因此起始值为 `0`。
  - `row_header_ids` 和 `row_header_texts` 元素数组中的多个值表示行标题的层次结构。
  - `column_header_ids` 和 `column_header_texts` 元素数组中的多个值表示列标题的层次结构。
