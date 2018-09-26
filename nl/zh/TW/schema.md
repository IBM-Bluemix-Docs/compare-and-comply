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

# 瞭解輸出綱目
{: #output_schema}

在服務剖析並強化文件之後，服務會在下列綱目提供 JSON 輸出。

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

綱目的安排如下所示。 

 - `document_text`：剖析文件的全文，HTML 格式。
 - `document_title`：剖析文件的標題。如果服務未偵測到標題，此元素的值為 `null`。
 - `elements`：服務所偵測到之文件元素的陣列。
    - `sentence`：句子的位置，如其 begin 及 end 索引所定義。
    - `sentence_text`：句子的文字。
    - `types`：說明何謂元素及其影響對象的陣列。它由一組以上的 `nature` 索引鍵（句子對所識別 `party` 的影響）及 `party` 索引鍵（句子影響對象）組成。
      - `label`：`nature` 及 `party` 元素的陣列。
        - `nature`：句子所需的動作類型。可能值為 `Definition`、`Disclaimer`、`Exclusion`、`Obligation` 及 `Right`。
        - `party`：識別句子所適用對象的字串。
      - `assurance`：所偵測到的類型為適用的信賴度。可能值為 `High` 及 `Low`。
      - `provenance`：`types` 及 `categories` 陣列中的每一個物件都包括一個 `provenance` 物件。物件具有一個以上的 `id` 索引鍵。
        - `id`：一個雜湊值，您可以將其傳送給 IBM，以提供意見或接收支援。
    - `categories`：列出句子所屬之功能種類的陣列；換言之，句子的主題。
      - `label`：句子所屬的種類。請參閱[「瞭解合約剖析」中的 Categories](/docs/services/compare-and-comply/parsing.html#contract_categories)，以取得種類的清單。
      - `assurance`：所偵測到的種類為適用的信賴度。可能值為 `High` 及 `Low`。
      - `provenance`：`types` 及 `categories` 陣列中的每一個物件都包括一個 `provenance` 物件，其包括一個以上的 `id` 索引鍵。
        - `id`：一個雜湊值，您可以將其傳送給 IBM，以提供意見或接收支援。
    - `attributes`：`type`、`text` 及 `attribute` 元素的陣列。
      - `type`：屬性的類型。可能值為 `Location`、`DateTime` 及 `Currency`。
      - `text`：與屬性相關聯的文字。
      - `attribute`：屬性的位置，如其 `begin` 及 `end` 索引所定義。


  - `tables`：定義服務所識別表格的陣列。
    - `table`：現行表格的位置，如輸入文件中的 `begin` 及 `end` 偏移所定義。
    - `table_text`：現行表格的文字內容，來自輸入文件且無相關聯的標記內容。
    - `column_headers`：現行表格之直欄層次資料格的陣列，每個適合作為與其本身相同直欄之其他資料格的標頭。每個直欄標頭定義為下列各項的集合：
      - `id`：格式為 `columnHeader-x-y` 的字串值，其中 `x` 和 `y` 是此直欄標頭資料格在輸入文件中的 begin 及 end 偏移。
      - `cell`：此資料格在現行表格的位置，如輸入文件中的 `begin` 及 `end` 偏移所定義。
      - `cell_text`：此資料格的文字內容，來自輸入文件且無相關聯的標記內容。 
      - `row_index_min`：此資料格 `row` 位置在現行表格中的 begin 索引。
      - `row_index_max`：此資料格 `row` 位置在現行表格中的 end 索引。
      - `column_index_min`：此資料格 `column` 位置在現行表格中的 begin 索引。
      - `column_index_max`：此資料格 `column` 位置在現行表格中的 end 索引。
    - `row_headers`：現行表格之列層次資料格的陣列，每個適合作為與其本身相同列之其他資料格的標頭。每個列標頭定義為下列各項的集合：
      - `id`：格式為 `rowHeader-x-y` 的字串值，其中 `x` 和 `y` 是此列標頭資料格在輸入文件中的 begin 及 end 偏移。
      - `cell`：此資料格在現行表格的位置，如輸入文件中的 `begin` 及 `end` 偏移所定義。
      - `cell_text`：此資料格的文字內容，來自輸入文件且無相關聯的標記內容。 
      - `row_index_min`：此資料格 `row` 位置在現行表格中的 begin 索引。
      - `row_index_max`：此資料格 `row` 位置在現行表格中的 end 索引。
      - `column_index_min`：此資料格 `column` 位置在現行表格中的 begin 索引。
      - `column_index_max`：此資料格 `column` 位置在現行表格中的 end 索引。
    - `body_cells`：現行表格中既不是表格標頭、不是直欄標頭也不是列標頭資料格的資料格陣列（具有對應的列和直欄關聯）。每個內文資料格定義為下列各項的集合：
      - `id`：格式為 `bodyCell-x-y` 的字串值，其中 `x` 和 `y` 是此內文資料格在輸入文件中的 begin 及 end 偏移。
      - `cell`：此資料格在現行表格的位置，如輸入文件中的 `begin` 及 `end` 偏移所定義。
      - `cell_text`：此資料格的文字內容，來自輸入文件且無相關聯的標記內容。 
      - `row_index_min`：此資料格 `row` 位置在現行表格中的 begin 索引。
      - `row_index_max`：此資料格 `row` 位置在現行表格中的 end 索引。
      - `column_index_min`：此資料格 `column` 位置在現行表格中的 begin 索引。
      - `column_index_max`：此資料格 `column` 位置在現行表格中的 end 索引。
      - `row_header_ids`：值的陣列，每個都是適用於此內文資料格之列標頭的 `id` 值。
      - `row_header_texts`：值的陣列，每個都是適用於此內文資料格之列標頭的 `cell_text` 值。
      - `column_header_ids`：值的陣列，每個都是適用於此內文資料格之直欄標頭的 `id` 值。
      - `column_header_texts`：值的陣列，每個都是適用於此內文資料格之直欄標頭的 `cell_text` 值。
    - `section_title`：（如果已識別）包含現行表格之區段標題的位置，如原始輸入文件中的 begin 及 end 偏移所定義。若未識別任何區段題頭，則為空白。
    - `section_title_text`：（如果已識別）包含現行表格之區段標題的文字內容，且無相關聯的標記內容。若未識別任何區段題頭，則為空白。
    - `table_headers`：表格層次資料格的陣列，這些資料格適合作為現行表格之所有其他資料格的標頭。每個表格標頭定義為下列元素的集合：
      - `id`：格式為 `tableHeader-x-y` 的字串值，其中 `x` 和 `y` 是資料格值在原始輸入文件中的 begin 及 end 偏移。
      - `cell`：表格標頭資料格在現行表格的位置，如原始輸入文件中的 begin 及 end 偏移所定義。
      - `cell_text`：此資料格的文字內容，來自輸入文件且無相關聯的標記內容。 
      - `row_index_min`：此資料格 `row` 位置在現行表格中的 begin 索引。
      - `row_index_max`：此資料格 `row` 位置在現行表格中的 end 索引。
      - `column_index_min`：此資料格 `column` 位置在現行表格中的 begin 索引。
      - `column_index_max`：此資料格 `column` 位置在現行表格中的 end 索引。
      
**附註：**
  - 每個資料格的列及直欄索引值都以零為基礎，因此從 `0` 開始。
  - `row_header_ids` 及 `row_header_texts` 元素陣列中的多個值指出列標頭的階層。
  - `column_header_ids` 及 `column_header_texts` 元素陣列中的多個值指出直欄標頭的階層。
