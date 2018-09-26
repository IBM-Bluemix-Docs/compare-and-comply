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

# 出力スキーマについて
{: #output_schema}

文書がサービスによって解析され、拡充された後に、サービスは次のスキーマの JSON 出力を提供します。

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

スキーマは次のように配置されます。 

 - `document_text`: HTML 形式の解析された文書のフルテキスト。
 - `document_title`: 解析された文書のタイトル。サービスがタイトルを検出しなかった場合、このエレメントの値は `null` です。
 - `elements`: サービスによって検出された文書エレメントの配列。
    - `sentence`: センテンスの位置 (その開始インデックスと終了インデックスによって定義される)。
    - `sentence_text`: センテンスのテキスト。
    - `types`: エレメントの種類とその影響を受ける対象者を記述した配列。 `nature` キー (識別された `party` へのセンテンスの影響) と `party` キー (センテンスの影響が及ぶ対象者) のセットが 1 つ以上あります。
      - `label`: `nature` エレメントおよび `party` エレメントの配列。
        - `nature`: センテンスに必要なアクションのタイプ。指定可能な値は `Definition`、`Disclaimer`、`Exclusion`、`Obligation`、および `Right` です。
        - `party`: センテンスが適用されるパーティーを識別する文字列。
      - `assurance`: 検出されたタイプが適用可能であることの確実性。指定可能な値は `High` および `Low` です。
      - `provenance`: `types` 配列と `categories` 配列内の各オブジェクトには `provenance` オブジェクトが含まれます。オブジェクトには 1 つ以上の `id` キーがあります。
        - `id`: IBM に送信して、フィードバックを送信したりサポートを受けたりすることができるハッシュ値。
    - `categories`: センテンスが分類される機能カテゴリー、つまりセンテンスの主題をリストした配列。
      - `label`: センテンスが分類されるカテゴリー。カテゴリーのリストについては、[『契約解析について』にあるカテゴリー](/docs/services/compare-and-comply/parsing.html#contract_categories)を参照してください。
      - `assurance`: 検出されたカテゴリーが適用可能であることの確実性。指定可能な値は `High` および `Low` です。
      - `provenance`: `types` 配列と `categories` 配列内の各オブジェクトには 1 つ以上の `id` キーを含む `provenance` オブジェクトが含まれます。
        - `id`: IBM に送信して、フィードバックを送信したりサポートを受けたりすることができるハッシュ値。
    - `attributes`: `type`、`text`、および `attribute` の各エレメントの配列。
      - `type`: 属性のタイプ。指定可能な値は `Location`、`DateTime`、および `Currency` です。
      - `text`: 属性と関連付けられているテキスト。
      - `attribute`: 属性の位置 (その`開始`インデックスと`終了`インデックスによって定義される)。


  - `tables`: サービスによって識別される表を定義する配列。
    - `table`: 現行表の位置 (入力文書内のその`開始`オフセットと`終了`オフセットによって定義される)。
    - `table_text`: 関連するマークアップ・コンテンツなしの、入力文書にある現行表のテキスト・コンテンツ。
    - `column_headers`: 現行表の、同じ列の他のセルのヘッダーとして適用される、列レベルのセルの配列。それぞれの列ヘッダーは次のエレメントの集合として定義されます。
      - `id`: `columnHeader-x-y` という形式のストリング値。ここで、`x` および `y` は、入力文書のこの列ヘッダー・セルの開始オフセットと終了オフセットです。
      - `cell`: 現行表におけるこのセルの位置 (入力文書内のその`開始`オフセットと`終了`オフセットによって定義される)。
      - `cell_text`: 関連するマークアップ・コンテンツなしの、入力文書にある現在のセルのテキスト・コンテンツ。 
      - `row_index_min`: 現行表におけるこのセルの`行`位置の開始インデックス。
      - `row_index_max`: 現行表におけるこのセルの`行`位置の終了インデックス。
      - `column_index_min`: 現行表におけるこのセルの`列`位置の開始インデックス。
      - `column_index_max`: 現行表におけるこのセルの`列`位置の終了インデックス。
    - `row_headers`: 現行表の、同じ行の他のセルのヘッダーとして適用される、行レベルのセルの配列。それぞれの行ヘッダーは次のエレメントの集合として定義されます。
      - `id`: 形式が `rowHeader-x-y` のストリング値。ここで `x`および `y` は入力文書のこの行のヘッダー・セルの開始オフセットと終了オフセットです。
      - `cell`: 現行表におけるこのセルの位置 (入力文書内のその`開始`オフセットと`終了`オフセットによって定義される)。
      - `cell_text`: 関連するマークアップ・コンテンツなしの、入力文書にある現在のセルのテキスト・コンテンツ。 
      - `row_index_min`: 現行表におけるこのセルの`行`位置の開始インデックス。
      - `row_index_max`: 現行表におけるこのセルの`行`位置の終了インデックス。
      - `column_index_min`: 現行表におけるこのセルの`列`位置の開始インデックス。
      - `column_index_max`: 現行表におけるこのセルの`列`位置の終了インデックス。
    - `body_cells`: 対応する行ヘッダーと列ヘッダーが関連付けられている現行表の、表ヘッダー・セルでも列ヘッダー・セルでも行ヘッダー・セルでもないセルの配列。それぞれの本文セルは、次のエレメントの集合として定義されます。
      - `id`: `bodyCell-x-y` という形式のストリング値。ここで `x` および `y` は入力文書内のこの本文セルの開始オフセットと終了オフセットです。
      - `cell`: 現行表におけるこのセルの位置 (入力文書内のその`開始`オフセットと`終了`オフセットによって定義される)。
      - `cell_text`: 関連するマークアップ・コンテンツなしの、入力文書にある現在のセルのテキスト・コンテンツ。 
      - `row_index_min`: 現行表におけるこのセルの`行`位置の開始インデックス。
      - `row_index_max`: 現行表におけるこのセルの`行`位置の終了インデックス。
      - `column_index_min`: 現行表におけるこのセルの`列`位置の開始インデックス。
      - `column_index_max`: 現行表におけるこのセルの`列`位置の終了インデックス。
      - `row_header_ids`: この本文セルに適用可能な行ヘッダーの `id` 値の配列。
      - `row_header_texts`: この本文セルに適用可能な行ヘッダーの `cell_text` 値の配列。
      - `column_header_ids`: この本文セルに適用可能な列ヘッダーの `id` 値の配列。
      - `column_header_texts`: この本文セルに適用可能な列ヘッダーの `cell_text` 値の配列。
    - `section_title`: セクション・タイトルが識別される場合は、現行表を含むセクション・タイトルの位置 (元の入力文書内のその開始オフセットと終了オフセットによって定義される)。セクション・タイトルが識別されない場合は空です。
    - `section_title_text`: セクション・タイトルが識別される場合は、関連するマークアップ・コンテンツなしの、現行表が含まれるセクション・タイトルのテキスト・コンテンツ。セクション・タイトルが識別されない場合は空です。
    - `table_headers`: 現行表の他のすべてのセルにヘッダーとして適用可能な表レベル・セルの配列。それぞれの表ヘッダーは次のエレメントの集合として定義されます。
      - `id`: `tableHeader-x-y` という形式のストリング値。ここで `x` および `y` は元の入力文書内のセル値の開始オフセットと終了オフセットです。
      - `cell`: 現行表における表ヘッダー・セルの位置 (元の入力文書内のその開始オフセットと終了オフセットによって定義される)。
      - `cell_text`: 関連するマークアップ・コンテンツなしの、入力文書にある現在のセルのテキスト・コンテンツ。 
      - `row_index_min`: 現行表におけるこのセルの行位置の開始インデックス。
      - `row_index_max`: 現行表におけるこのセルの行位置の終了インデックス。
      - `column_index_min`: 現行表におけるこのセルの`列`位置の開始インデックス。
      - `column_index_max`: 現行表におけるこのセルの列位置の終了インデックス。
      
**注:**
  - セルごとの行および列の索引値はゼロベースであり、`0` で始まります。
  - `row_header_ids` エレメントと `row_header_texts` エレメントの配列内の複数の値は、行ヘッダーの階層を示しています。
  - `column_header_ids` エレメントと `column_header_texts` エレメントの配列内の複数の値は、列ヘッダーの階層を示しています。
