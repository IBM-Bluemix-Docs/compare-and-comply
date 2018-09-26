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

# 출력 스키마 이해
{: #output_schema}

서비스에 의해 문서가 구문 분석되고 강화된 후에 서비스는 다음 스키마의 JSON 출력을 제공합니다. 

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

스키마는 다음과 같이 배열됩니다.  

 - `document_text`: HTML 형식의 구문 분석된 문서의 전체 텍스트입니다. 
 - `document_title`: 구문 분석된 문서의 제목입니다. 서비스가 제목을 발견하지 않은 경우, 이 요소의 값은 `null`입니다. 
 - `elements`: 서비스가 발견한 문서 요소의 배열입니다. 
    - `sentence`: 시작 및 끝 색인에 의해 정의된 문장의 위치입니다. 
    - `sentence_text`: 문장의 텍스트입니다. 
    - `types`: 요소의 개념 및 이 요소의 영향을 받는 사용자를 설명하는 배열. 이는 하나 이상의 `nature` 키 세트(식별된 `party`에 대한 문자의 영향) 및 `party` 키(문장의 영향을 받는 사용자)로 구성됩니다.
      - `label`: `nature` 및 `party` 요소의 배열입니다. 
        - `nature`: 문장에 필요한 조치의 유형입니다. 가능한 값은 `Definition`, `Disclaimer`, `Exclusion`, `Obligation` 및 `Right`입니다. 
        - `party`: 문장이 적용되는 당사자를 식별하는 문자열입니다. 
      - `assurance`: 발견된 유형이 적용될 수 있는 신뢰도입니다. 가능한 값은 `High` 및 `Low`입니다. 
      - `provenance`: `types` 및 `categories` 배열의 각 오브젝트에 `provenance` 오브젝트가 포함됩니다. 오브젝트에 하나 이상의 `id` 키가 있습니다. 
        - `id`: 피드백을 제공하거나 지원을 받기 위해 IBM에 보낼 수 있는 해시 값입니다. 
    - `categories`: 문장이 속하는 기능 카테고리(즉, 문장의 주제)를 나열하는 배열입니다. 
      - `label`: 문장이 속하는 카테고리입니다. 카테고리의 목록은 [계약 구문 분석 이해의 카테고리](/docs/services/compare-and-comply/parsing.html#contract_categories)를 참조하십시오. 
      - `assurance`: 발견된 카테고리가 적용될 수 있는 신뢰도입니다. 가능한 값은 `High` 및 `Low`입니다. 
      - `provenance`: `types` 및 `categories` 배열의 각 오브젝트에 하나 이상의 `id` 키가 포함된 `provenance` 오브젝트가 포함됩니다. 
        - `id`: 피드백을 제공하거나 지원을 받기 위해 IBM에 보낼 수 있는 해시 값입니다. 
    - `attributes`: `type`, `text` 및 `attribute` 요소의 배열입니다. 
      - `type`: 속성의 유형입니다. 가능한 값은 `Location`, `DateTime` 및 `Currency`입니다. 
      - `text`: 속성과 연관된 텍스트입니다. 
      - `attribute`: `begin` 및 `end` 색인에 의해 정의된 속성의 위치입니다. 


  - `tables`: 서비스에 의해 식별된 테이블을 정의하는 배열입니다. 
    - `table`: 입력 문서에서 `begin` 및 `end` 오프셋으로 정의된 현재 테이블의 위치입니다. 
    - `table_text`: 연관된 마크업 컨텐츠가 없는 이 문서의 현재 테이블의 텍스트 컨텐츠입니다. 
    - `column_headers`: 각각 자신과 동일한 열의 기타 셀에 헤더로서 적용 가능한 현재 테이블의 열 레벨 셀의 배열입니다. 각각의 열 헤더는 다음의 콜렉션으로 정의됩니다. 
      - `id`: `columnHeader-x-y` 형식의 문자열 값입니다. 여기서 `x` 및 `y`는 입력 문서에서 이 열 헤더 셀의 시작 및 끝 오프셋입니다. 
      - `cell`: 입력 문서에서 `begin` 및 `end` 오프셋으로 정의된 현재 테이블의 이 셀의 위치입니다. 
      - `cell_text`: 연관된 마크업 컨텐츠가 없는 입력 문서의 이 셀의 텍스트 컨텐츠입니다.  
      - `row_index_min`: 현재 테이블에서 이 셀의 `row` 위치의 시작 색인입니다. 
      - `row_index_max`: 현재 테이블에서 이 셀의 `row` 위치의 끝 색인입니다. 
      - `column_index_min`: 현재 테이블에서 이 셀의 `column` 위치의 시작 색인입니다. 
      - `column_index_max`: 현재 테이블에서 이 셀의 `column` 위치의 끝 색인입니다. 
    - `row_headers`: 각각 자신과 동일한 행의 기타 셀에 헤더로서 적용 가능한 현재 테이블의 행 레벨 셀의 배열입니다. 각각의 행 헤더는 다음의 콜렉션으로 정의됩니다. 
      - `id`: `rowHeader-x-y` 형식의 문자열 값입니다. 여기서 `x` 및 `y`는 입력 문서에서 이 행 헤더 셀의 시작 및 끝 오프셋입니다. 
      - `cell`: 입력 문서에서 `begin` 및 `end` 오프셋으로 정의된 현재 테이블의 이 셀의 위치입니다. 
      - `cell_text`: 연관된 마크업 컨텐츠가 없는 입력 문서의 이 셀의 텍스트 컨텐츠입니다.  
      - `row_index_min`: 현재 테이블에서 이 셀의 `row` 위치의 시작 색인입니다. 
      - `row_index_max`: 현재 테이블에서 이 셀의 `row` 위치의 끝 색인입니다. 
      - `column_index_min`: 현재 테이블에서 이 셀의 `column` 위치의 시작 색인입니다. 
      - `column_index_max`: 현재 테이블에서 이 셀의 `column` 위치의 끝 색인입니다. 
    - `body_cells`: 대응되는 행 및 열 헤더 연관이 있는 현재 테이블의 테이블 헤더, 열 헤더, 행 헤더 셀 중 어느 것도 아닌 셀의 배열입니다. 각각의 본문 셀은 다음의 콜렉션으로 정의됩니다. 
      - `id`: `bodyCell-x-y` 형식의 문자열 값입니다. 여기서 `x` 및 `y`는 입력 문서에서 이 본문 셀의 시작 및 끝 오프셋입니다. 
      - `cell`: 입력 문서에서 `begin` 및 `end` 오프셋으로 정의된 현재 테이블의 이 셀의 위치입니다. 
      - `cell_text`: 연관된 마크업 컨텐츠가 없는 입력 문서의 이 셀의 텍스트 컨텐츠입니다.  
      - `row_index_min`: 현재 테이블에서 이 셀의 `row` 위치의 시작 색인입니다. 
      - `row_index_max`: 현재 테이블에서 이 셀의 `row` 위치의 끝 색인입니다. 
      - `column_index_min`: 현재 테이블에서 이 셀의 `column` 위치의 시작 색인입니다. 
      - `column_index_max`: 현재 테이블에서 이 셀의 `column` 위치의 끝 색인입니다. 
      - `row_header_ids`: 각각 이 본문 셀에 적용 가능한 행 헤더의 `id` 값인 값의 배열입니다. 
      - `row_header_texts`: 각각 이 본문 셀에 적용 가능한 행 헤더의 `cell_text` 값인 값의 배열입니다. 
      - `column_header_ids`: 각각 이 본문 셀에 적용 가능한 열 헤더의 `id` 값인 값의 배열입니다. 
      - `column_header_texts`: 각각 이 본문 셀에 적용 가능한 열 헤더의 `cell_text` 값인 값의 배열입니다. 
    - `section_title`: 식별된 경우, 원래 입력 문서에서 시작 및 끝 오프셋으로 정의된 현재 테이블이 포함된 섹션 제목의 위치입니다. 섹션 제목이 식별되지 않으면 비어 있습니다. 
    - `section_title_text`: 식별된 경우, 연관된 마크업 컨텐츠가 없는 현재 테이블이 포함된 섹션 제목의 텍스트 컨텐츠입니다. 섹션 제목이 식별되지 않으면 비어 있습니다. 
    - `table_headers`: 현재 테이블의 기타 모든 셀에 헤더로서 적용 가능한 테이블 레벨 셀의 배열입니다. 각 테이블 헤더는 다음 요소의 콜렉션으로 정의됩니다. 
      - `id`: `tableHeader-x-y` 형식의 문자열 값입니다. 여기서 `x` 및 `y`는 원래 입력 문서에서 셀 값의 시작 및 끝 오프셋입니다. 
      - `cell`: 원래 입력 문서에서 시작 및 끝 오프셋으로 정의된 현재 테이블의 테이블 헤더 셀의 위치입니다. 
      - `cell_text`: 연관된 마크업 컨텐츠가 없는 입력 문서의 이 셀의 텍스트 컨텐츠입니다.  
      - `row_index_min`: 현재 테이블에서 이 셀의 행 위치의 시작 색인입니다. 
      - `row_index_max`: 현재 테이블에서 이 셀의 행 위치의 끝 색인입니다. 
      - `column_index_min`: 현재 테이블에서 이 셀의 `column` 위치의 시작 색인입니다. 
      - `column_index_max`: 현재 테이블에서 이 셀의 열 위치의 끝 색인입니다. 
      
**참고:**
  - 셀당 행 및 열 색인 값은 제로(0) 기반이므로 `0`에서 시작합니다. 
  - `row_header_ids` 및 `row_header_texts` 요소의 배열의 다중 값은 행 헤더의 계층 구조를 표시합니다. 
  - `column_header_ids` 및 `column_header_texts` 요소의 배열의 다중 값은 열 헤더의 계층 구조를 표시합니다. 
