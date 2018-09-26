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

# 테이블 구문 분석 이해
{: #understanding_tables}

문서가 서비스에 의해 강화되면 강화된 문서에 `tables` 배열이 포함됩니다. 배열의 각 오브젝트는 서비스에 의해 입력 문서에서 식별된 테이블을 기술합니다. 테이블 구문 분석 형식에 대한 정보는 [출력 스키마 이해](/docs/services/compare-and-comply/schema.html#output_schema)를 참조하십시오. 

다음은 입력 문서의 예제 테이블입니다.
 ![예제 테이블](images/example-table.png)

테이블은 다음으로 구성되어 있습니다.
 ![테이블 구성](images/table-comp.png)
 
여기서

<ul>
  <li><strong><em>굵은 기울임체 텍스트</em></strong>는 테이블 헤더를 표시함</li>
  <li><strong>굵은체 텍스트</strong>는 열 헤더를 표시함</li>
  <li><em>기울임체 텍스트</em>는 행 헤더를 표시함</li>
  <li>스타일이 없는 텍스트는 본문 셀을 표시함</li>
</ul>
  
서비스의 출력은 다음과 같이 예제의 첫 번째 본문 셀을 표시합니다(즉, `35.0%` 값의 행 3의 첫 번째 셀).

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
    
