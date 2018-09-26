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

# Comprensión del esquema de salida
{: #output_schema}

Cuando el servicio haya analizado y enriquecido un documento, este proporcionará la salida JSON en el esquema siguiente.

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

El esquema se organiza de la manera siguiente. 

 - `document_text`: El texto completo del documento analizado en formato HTML.
 - `document_title`: El título del documento analizado. Si el servicio no ha detectado un título, el valor del elemento es `null`.
 - `elements`: Una matriz de los elementos del documento detectados por el servicio.
    - `sentence`: La ubicación de la oración como la definen los índices de inicio y fin.
    - `sentence_text`: El texto de la oración.
    - `types`: Una matriz que describe lo que es el elemento y a quién afecta. Consta de uno o varios conjuntos de claves de `nature` (naturaleza, el efecto de la frase en la parte identificada, `party`) y claves de `party` (a quién afecta la frase).
      - `label`: Una matriz de los elementos `nature` y `party`.
        - `nature`: El tipo de acción que requiere la oración. Los valores posibles son `Definition` (Definición), `Disclaimer` (Definición), `Exclusion` (Exclusión), `Obligation` (Obligación) y `Right` (Derecho).
        - `party`: Una cadena que identifica la parte a la que se aplica la oración.
      - `assurance`: La seguridad de que el tipo detectado es aplicable. Los valores posibles son `High` (Alta) y `Low` (Baja).
      - `provenance`: Cada objeto de las matrices `types` y `categories` incluye un objeto `provenance` (procedencia). El objeto tiene una o varias claves `id`.
        - `id`: Un valor hash que puede enviar a IBM para proporcionar comentarios o recibir soporte.
    - `categories`: Una matriz que enumera las categorías funcionales en las que se encuentra la oración identificada; en otras palabras, el tema de la frase.
      - `label`: La categoría en la que se encuentra la oración. Consulte [Categorías en Comprensión del análisis de contrato](/docs/services/compare-and-comply/parsing.html#contract_categories) para obtener una lista de categorías.
      - `assurance`: La seguridad de que la categoría detectada es aplicable. Los valores posibles son `High` (Alta) y `Low` (Baja).
      - `provenance`: Cada objeto en las matrices `types` y `categories` incluye un objeto `provenance` (procedencia) que contiene una o varias claves `id`.
        - `id`: Un valor hash que puede enviar a IBM para proporcionar comentarios o recibir soporte.
    - `attributes`: Una matriz de los elementos `type` (tipo), `text` (text) y `attribute` (atributo).
      - `type`: El tipo de atributo. Los valores posibles son `Location` (Ubicación), `DateTime` (Fecha y hora) y `Currency` (Moneda).
      - `text`: El texto que está asociado con el atributo.
      - `attribute`: La ubicación del atributo como la definen los índices `begin` (inicio) y `end` (fin).


  - `tables`: Una matriz que define las tablas identificadas por el servicio.
    - `table`: La ubicación de la tabla actual como la definen los desplazamientos `begin` (inicio) y `end` (fin) en el documento de entrada.
    - `table_text`: Los contenidos textuales de la tabla actual del documento de entrada sin contenido de marcación asociado.
    - `column_headers`: Una matriz de las celdas a nivel de columna, cada una aplicable como cabecera a otras celdas en la misma columna de la tabla actual. Cada cabecera de columna se define como una colección de los valores siguientes:
      - `id`: Una valor de serie en el formato `columnHeader-x-y` donde `x` e `y` son los desplazamientos de inicio y fin de la celda de cabecera de columna en el documento de entrada.
      - `cell`: La ubicación de la celda en la tabla actual tal y como la definen los desplazamientos `begin` (inicio) y `end` (fin) en el documento de entrada.
      - `cell_text`: Los contenidos textuales de la celda del documento de entrada sin contenido de marcación asociado. 
      - `row_index_min`: El índice de inicio de la ubicación `row` (fila) de la celda en la tabla actual.
      - `row_index_max`: El índice de fin de la ubicación `row` (fila) de la celda en la tabla actual.
      - `column_index_min`: El índice de inicio de la ubicación `column` (columna) de la celda en la tabla actual.
      - `column_index_max`: El índice de fin de la ubicación `column` (columna) de la celda en la tabla actual.
    - `row_headers`: Una matriz de las celdas a nivel de fila, cada una aplicable como cabecera a otras celdas de la misma fila de la tabla actual. Cada cabecera de fila se define como una colección de los valores siguientes:
      - `id`: Una valor de serie en el formato `rowHeader-x-y` donde `x` e `y` son los desplazamientos de inicio y fin de la celda de cabecera de fila en el documento de entrada.
      - `cell`: La ubicación de la celda en la tabla actual como la definen los desplazamientos `begin` (inicio) y `end` (fin) en el documento de entrada.
      - `cell_text`: Los contenidos textuales de la celda del documento de entrada sin contenido de marcación asociado. 
      - `row_index_min`: El índice de inicio de la ubicación `row` (fila) de la celda en la tabla actual.
      - `row_index_max`: El índice de fin de la ubicación `row` (fila) de la celda en la tabla actual.
      - `column_index_min`: El índice de inicio de la ubicación `column` (columna) de la celda en la tabla actual.
      - `column_index_max`: El índice de fin de la ubicación `column` (columna) de la celda en la tabla actual.
    - `body_cells`: Una matriz de las celdas que no son de cabecera de tabla, cabecera de columna ni cabecera de fila de la tabla actual con las asociaciones de cabecera de columna y fila correspondientes. Cada celda del cuerpo se define como una colección de los valores siguientes:
      - `id`: Una valor de serie en el formato `bodyCell-x-y` donde `x` e `y` son los desplazamientos de inicio y fin de la celda del cuerpo en el documento de entrada.
      - `cell`: La ubicación de la celda en la tabla actual como la definen los desplazamientos `begin` (inicio) y `end` (fin) en el documento de entrada.
      - `cell_text`: Los contenidos textuales de la celda del documento de entrada sin contenido de marcación asociado. 
      - `row_index_min`: El índice de inicio de la ubicación `row` (fila) de la celda en la tabla actual.
      - `row_index_max`: El índice de fin de la ubicación `row` (fila) de la celda en la tabla actual.
      - `column_index_min`: El índice de inicio de la ubicación `column` (columna) de la celda en la tabla actual.
      - `column_index_max`: El índice de fin de la ubicación `column` (columna) de la celda en la tabla actual.
      - `row_header_ids`: Una matriz de valores, que son el valor `id` de una cabecera de fila aplicable a la celda del cuerpo.
      - `row_header_texts`: Una matriz de valores, que son el valor `cell_text` (texto_celda) de una cabecera de fila aplicable a la celda del cuerpo.
      - `column_header_ids`: Una matriz de valores, que son el valor `id` de una cabecera de columna aplicable a la celda del cuerpo.
      - `column_header_texts`: Una matriz de valores, que son el valor `cell_text` (texto_celda) de una cabecera de columna aplicable a la celda del cuerpo.
    - `section_title`: Si se identifica, la ubicación de un título de sección que contiene la tabla actual, tal y como lo definen los desplazamientos de inicio y fin en el documento de entrada original. Aparece vacío si no se identifica ningún título de sección.
    - `section_title_text`: Si se identifica, los contenidos textuales del título de sección que contienen la tabla actual sin contenido de marcación asociado. Aparece vacío si no se identifica ningún título de sección.
    - `table_headers`: Una matriz de las celdas a nivel de tabla aplicables como cabeceras al resto de celdas de la tabla actual. Cada cabecera de tabla se define como una colección de los elementos siguientes:
      - `id`: Valor de serie en el formato `tableHeader-x-y` donde `x` e `y` son los desplazamientos de inicio y fin del valor de celda en el documento de entrada.
      - `cell`: La ubicación de la celda de la cabecera de tabla en la tabla actual, tal y como la definen los desplazamientos de inicio y fin en el documento de entrada original.
      - `cell_text`: Los contenidos textuales de la celda del documento de entrada sin contenido de marcación asociado. 
      - `row_index_min`: El índice de inicio de la ubicación de la fila de la celda en la tabla actual.
      - `row_index_max`: El índice de fin de la ubicación fila de la celda en la tabla actual.
      - `column_index_min`: El índice de inicio de la ubicación `column` (columna) de la celda en la tabla actual.
      - `column_index_max`: El índice de fin de la ubicación columna de la celda en la tabla actual.
      
**Nota:**
  - Los valores de índice de columna y fila por celda están basados en cero y empiezan por `0`.
  - Los valores múltiples en las matrices de los elementos `row_header_ids` y `row_header_texts` indican una jerarquía de las cabeceras de fila.
  - Los valores múltiples en las matrices de los elementos `column_header_ids` y `column_header_texts` indican una jerarquía de cabeceras de columna.
