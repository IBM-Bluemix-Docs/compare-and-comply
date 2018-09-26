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

# Entendendo o esquema de saída
{: #output_schema}

Depois que um documento tiver sido analisado e enriquecido pelo serviço, o serviço fornecerá a saída JSON no esquema a seguir.

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
          ], "categorias": [ {
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

O esquema é organizado conforme a seguir. 

 - `document_text`: o texto completo do documento analisado no formato HTML.
 - `document_title`: o título do documento analisado. Se o serviço não detectar um título, o valor desse elemento será `null`.
 - `elements`: uma matriz dos elementos de documento detectados pelo serviço.
    - `sentence`: o local da sentença, conforme definido por seus índices de início e de término.
    - ` sentence_text `: O texto da sentença.
    - `types`: uma matriz que descreve qual é o elemento e quem ele afeta. Ela consiste
em um ou mais conjuntos de chaves `nature` (o efeito da sentença na `party`
identificada) e de chaves `party` (quem a sentença afeta).
      - `label`: uma matriz dos elementos `nature` e `party`.
        - `nature`: o tipo de ação que a sentença requer. Os valores possíveis são
`Definition`, `Disclaimer`, `Exclusion`, `Obligation` e `Right`.
        - `party`: uma sequência que identifica a parte à qual a sentença se aplica.
      - `assurance`: a confiança de que o tipo detectado é aplicável. Os valores possíveis são  ` Alta `  e  ` Baixa `.
      - `provenance`: cada objeto nas matrizes `types` e `categories`
inclui um objeto `provenance`. O objeto tem uma ou mais chaves `id`.
        - `id`: um valor em hash que pode ser enviado para a IBM para fornecer feedback ou receber suporte.
    - `categories`: uma matriz que lista as categorias funcionais nas quais a sentença se enquadra; em outras palavras, o assunto da sentença.
      - `label`: a categoria na qual a sentença se enquadra. Consulte
[Categorias em Entendendo a análise sintática do
contrato](/docs/services/compare-and-comply/parsing.html#contract_categories) para obter uma lista de categorias.
      - `assurance`: a confiança de que a categoria detectada é aplicável. Os valores possíveis são  ` Alta `  e  ` Baixa `.
      - `provenance`: cada objeto nas matrizes `types` e `categories`
inclui um objeto `provenance` que inclui uma ou mais chaves `id`.
        - `id`: um valor em hash que pode ser enviado para a IBM para fornecer feedback ou receber suporte.
    - `attributes`: uma matriz dos elementos `type`, `text` e
`attribute`.
      - ` type `: o tipo de atributo. Os valores possíveis são `Location`, `DateTime` e `Currency`.
      - `text`: o texto associado ao atributo.
      - `attribute`: o local do atributo, conforme definido por seus índices `begin` e `end`.


  - `tables`: uma matriz que define as tabelas identificadas pelo serviço.
    - `table`: o local da tabela atual, conforme definido por seus deslocamentos `begin` e `end` no documento de entrada.
    - `table_text`: os conteúdos textuais da tabela atual do documento de entrada sem o conteúdo de marcação
associado.
    - `column_headers`: uma matriz de células de nível de coluna da tabela atual, cada uma delas aplicável como
um cabeçalho para as outras células na própria coluna. Cada cabeçalho da coluna é definido como uma coleção do seguinte:
      - `id`: um valor de sequência no formato `columnHeader-x-y`, em que
`x` e `y` são os deslocamentos inicial e final da célula desse cabeçalho da coluna no
documento de entrada.
      - `cell`: o local dessa célula na tabela atual, conforme definido por seus deslocamentos `begin` e `end` no documento de entrada.
      - `cell_text`: os conteúdos textuais dessa célula do documento de entrada sem o conteúdo de marcação
associado. 
      - `row_index_min`: o índice de início do local de `row` dessa célula na tabela atual.
      - `row_index_max`: o índice de término do local de `row` dessa célula na tabela
atual.
      - `column_index_min`: o índice de início do local de `column` dessa célula na tabela atual.
      - `column_index_max`: o índice de término do local de `column` dessa célula na tabela
atual.
    - `row_headers`: uma matriz de células de nível de linha da tabela atual, cada uma delas aplicável como
um cabeçalho para as outras células na própria linha. Cada cabeçalho de linha é definido como uma coleção do seguinte:
      - `id`: um valor de sequência no formato `rowHeader-x-y`, em que `x`
e `y` são os deslocamentos inicial e final da célula desse cabeçalho da linha no documento de
entrada.
      - `cell`: o local dessa célula na tabela atual, conforme definido por seus deslocamentos `begin` e `end` no documento de entrada.
      - `cell_text`: os conteúdos textuais dessa célula do documento de entrada sem o conteúdo de marcação associado. 
      - `row_index_min`: o índice de início do local de `row` dessa célula na tabela atual.
      - `row_index_max`: o índice de término do local de `row` dessa célula na tabela atual.
      - `column_index_min`: o índice de início do local de `column` dessa célula na tabela atual.
      - `column_index_max`: o índice de término do local de `column` dessa célula na tabela
atual.
    - `body_cells`: uma matriz de células da tabela atual que não são cabeçalho da tabela, cabeçalho da coluna nem
células do cabeçalho da linha com as associações de cabeçalho de linha e de coluna correspondentes. Cada célula do
corpo é definida como uma coleção do seguinte:
      - `id`: um valor de sequência no formato `bodyCell-x-y`, em que `x`
e `y` são os deslocamentos inicial e final da célula desse corpo no documento de entrada.
      - `cell`: o local dessa célula na tabela atual, conforme definido por seus deslocamentos `begin` e `end` no documento de entrada.
      - `cell_text`: os conteúdos textuais dessa célula do documento de entrada sem o conteúdo de marcação associado. 
      - `row_index_min`: o índice de início do local de `row` dessa célula na tabela atual.
      - `row_index_max`: o índice de término do local de `row` dessa célula na tabela atual.
      - `column_index_min`: o índice de início do local de `column` dessa célula na tabela atual.
      - `column_index_max`: o índice de término do local de `column` dessa célula na tabela
atual.
      - `row_header_ids`: uma matriz de valores, cada um deles sendo o valor `id` de um
cabeçalho da linha aplicável à célula desse corpo.
      - `row_header_texts`: uma matriz de valores, cada um deles sendo o valor `cell_text`
de um cabeçalho da linha aplicável à célula desse corpo.
      - `column_header_ids`: uma matriz de valores, cada um deles sendo o valor `id` de um
cabeçalho da coluna aplicável à célula desse corpo.
      - `column_header_texts`: uma matriz de valores, cada um deles sendo o valor
`cell_text` de um cabeçalho da coluna aplicável à célula desse corpo.
    - `section_title`: se identificado, o local de um título da seção que contém a tabela atual, conforme
definido por seus deslocamentos inicial e final no documento de entrada original. Vazio se nenhum título da seção for identificado.
    - `section_title_text`: se identificado, os conteúdos textuais do título da seção que contém a tabela
atual, sem o conteúdo de marcação associado. Vazio se nenhum título da seção for identificado.
    - `table_headers`: uma matriz de células de nível de tabela aplicáveis como cabeçalhos para todas as
outras células da tabela atual. Cada cabeçalho da tabela é definido como uma coleção dos elementos a seguir:
      - `id`: valor de sequência no formato `tableHeader-x-y` em que `x` e
`y` são os deslocamentos inicial e final do valor da célula no documento de entrada original.
      - `cell`: o local da célula do cabeçalho da tabela na tabela atual, conforme definido por seus
deslocamentos inicial e final no documento de entrada original.
      - `cell_text`: os conteúdos textuais dessa célula do documento de entrada sem o conteúdo de marcação associado. 
      - `row_index_min`: o índice de início do local da linha dessa célula na tabela atual.
      - `row_index_max`: o índice de término do local da linha dessa célula na tabela atual.
      - `column_index_min`: o índice de início do local de `column` dessa célula na tabela atual.
      - `column_index_max`: o índice de término do local da coluna dessa célula na tabela atual.
      
** Nota: **
  - Os valores de índice da linha e da coluna por célula são baseados em zero e, portanto, iniciam com `0`.
  - Múltiplos valores em matrizes de elementos `row_header_ids` e `row_header_texts`
indicam uma hierarquia de cabeçalhos de linha.
  - Múltiplos valores em matrizes de elementos `column_header_ids` e `column_header_texts` indicam uma hierarquia de cabeçalhos de coluna.
