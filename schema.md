---

copyright:
years: 2018
lastupdated: "2018-09-24"

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

# Understanding the output schema
{: #output_schema}

After a document has been processed by the `/v1/element_classification` method, the service provides JSON output in the following schema.

```
{
    "document_text": string,
    "document_title": string,
    "model_id": string,
    "model_version": string,
    "elements": [
        {
          "sentence": { "begin": int, "end": int },
          "sentence_text": string,
          "types": [
              {
                "label": { "nature": string, "party": string },
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
                "provenance": [
                   {
                     "id": string
                   }
                ]
              }
             ...
          ],
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
    "effective_dates": [
      {
        "text": string,
        "location": {"begin": int, "end": int}
      },
      ...
    ],
    "contract_amounts": [
      {
        "text": string,
        "location": {"begin": int, "end": int}
      },
      ...
    ],
    "parties": [
      {
        "party": string,
        "role": string
      },
      ...
    ],
    "document_structure": {
      "sections": [
        {
          "title_text": string,
          "title": {
            "begin": int,
            "end": int
          }
          "level": int
          "sentences": [
            {
              "begin": int,
              "end": int
            },
            ...
          ]
        },
        ...
      ],
      "leading_sentences": [
        {
          "sentence_text": string,
          "sentence": {
            "begin": int,
            "end": int
          },
          "sentences": [
            {
              "begin": int,
              "end": int
            },
            ...
          ]
        },
      ...
      ]
    },     
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
          "cell_text_normalized": string,
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
            "cell_text_normalized": string,
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
            "row_header_texts_normalized": [string],
            "column_header_ids": [string],
            "column_header_texts": [string],
            "column_header_texts_normalized": [string]
          },
        ...  
        ]
      },
      ...
    ]
}
```

The schema is arranged as follows. 

 - `document_text`: The full text of the parsed document in HTML format.
 - `document_title`: The title of the parsed document. If the service did not detect a title, the value of this element is `null`.
 - `model_id`: The analysis model to be used by the service. For the `/v1/element_classification` and `/v1/comparison` methods, the default is `contracts`. For the `/v1/tables` method, the default is `tables`. These defaults apply to the standalone methods as well as to the methods' use in batch-processing requests.
 - `model_version`: The version of the analysis model specified by the value of the `model_id` parameter.
 - `elements`: An array of the document elements detected by the service.
    - `sentence`: The location of the sentence as defined by its begin and end indexes.
    - `sentence_text`: The text of the sentence.
    - `types`: An array that describes what the element is and whom it affects. It consists of one or more sets of `nature` keys (the effect of the sentence on the identified `party`) and `party` keys (whom the sentence affects).
      - `label`: An array of `nature` and `party` elements.
        - `nature`: The type of action the sentence requires. Possible values are `Definition`, `Disclaimer`, `Exclusion`, `Obligation`, and `Right`.
        - `party`: A string identifying the party to whom the sentence applies.
      - `provenance`: Each object in the `types` and `categories` arrays includes a `provenance` object. The object has one or more `id` keys.
        - `id`: A hashed value that you can send to IBM to provide feedback or receive support.
    - `categories`: An array that lists the functional categories into which the sentence falls; in other words, the subject matter of the sentence.
      - `label`: The category into which the sentence falls. See [Categories at Understanding contract parsing](/docs/services/compare-comply/parsing.html#contract_categories) for a list of categories.
      - `provenance`: Each object in the `types` and `categories` arrays includes a `provenance` object that includes one or more `id` keys.
        - `id`: A hashed value that you can send to IBM to provide feedback or receive support.
    - `attributes`: An array of `type`, `text`, and `attribute` elements.
      - `type`: The type of attribute. Possible values are `Location`, `DateTime`, and `Currency`.
      - `text`: The text that is associated with the attribute.
      - `attribute`: The location of the attribute as defined by its `begin` and `end` indexes.

 - `effective_dates`: An array identifying the effective dates of the document.
   - `text`: The effective dates, listed as a string.
   - `location`: The location of the date or dates as defined by its `begin` and `end` indexes.
 - `contract_amounts`: An array identifying the monetary amounts identified in the document.
   - `text`: The contract amounts, listed as a string.
   - `location`: The location of the amount or amounts as defined by its `begin` and `end` indexes.

 - `parties`: An array defining the parties identified by the service.
   - `party`: A string value identifying the party.
   - `role`: A string value identifying the role of the party.
   <!--
     - `addresses`: An array of objects that identify addresses.
        - `text`: A string containing the address.
        - `location`: The location of the address as defined by its `begin` and `end` indexes.
     - `contacts`: An array defining the name and role of contacts identified in the input document.
        - `name`: A string listing the name of an identified contact.
        - `role`: A string listing the role of the identified contact.
        -->
    
 - `document_structure`: A JSON object describing the structure of the input document.
    - `sections`: A JSON array containing one object per section or subsection detected in the input document. Sections and subsections are not nested; instead, they are flattened out and can be placed back in order by using the `begin` and `end` values of the element and the `level` value of the section. Each object in the `sections` array includes a list of zero or more `sentences` that are directly under the title of the detected section.
      - `title_text`: A string listing the section title, if detected.
      - `title`: A JSON object listing the `begin` and `end` values of the section title.
      - `level`: An integer indicating the level at which the section is located in the input document. For example, `1` represents a top-level section, `2` represents a subsection within the level `1` section, and so forth.
      - `sentences`: A JSON array containing objects that specify the `begin` and `end` values of the sentences in the section.
    - `leading_sentences`: A JSON array containing one object per section or subsection, in parallel with the `sections` array, that details the leading sentences in the matching section or subsection. As in the `sections` array, the objects are not nested; instead, they are flattened out and can be placed back in order by using the `begin` and `end` values of the element or any level markers in the input document.
      - `sentence_text`: The text of the section's leading sentence.
      - `sentence`: A JSON object indicating the `begin` and `end` values of the leading sentence's location in the input document.
      - `sentences`: A JSON array containing objects that specify the `begin` and `end` values of the sentences that follow the leading sentence.
    
 - `tables`: An array defining the tables identified by the service.
    - `table`: The location of the current table as defined by its `begin` and `end` offsets in the input document.
    - `table_text`: The textual contents of the current table from the input document without associated markup content.
    - `section_title`: If identified, the location of a section title containing the current table, as defined by its begin and end offsets in the original input document. Empty if no section title is identified.
    - `section_title_text`: If identified, the textual contents of the section title containing the current table, without associated markup content. Empty if no section title is identified.
    - `table_headers`: An array of table-level cells applicable as headers to all the other cells of the current table. Each table header is defined as a collection of the following elements:
      - `id`: String value in the format `tableHeader-x-y` where `x` and `y` are the begin and end offsets of the cell value in the original input document.
      - `cell`: The location of the table header cell in the current table as defined by its begin and end offsets in the original input document.
      - `cell_text`: The textual contents of this cell from the input document without associated markup content. 
      - `row_index_min`: The begin index of this cell's row location in the current table.
      - `row_index_max`: The end index of this cell's row location in the current table.
      - `column_index_min`: The begin index of this cell's `column` location in the current table.
      - `column_index_max`: The end index of this cell's column location in the current table.
    - `column_headers`: An array of column-level cells, each applicable as a header to other cells in the same column as itself, of the current table. Each column header is defined as a collection of the following:
      - `id`: A string value in the format `columnHeader-x-y`, where `x` and `y` are the begin and end offsets of this column header cell in the input document.
      - `cell`: The location of this cell in the current table as defined by its `begin` and `end` offsets in the input document.
      - `cell_text`: The textual contents of this cell from the input document without associated markup content.
      - `cell_text_normalized`: If you provide customization input, the normalized version of the cell text according to the customization; otherwise, the same value as `cell_text`. 
      - `row_index_min`: The begin index of this cell's `row` location in the current table.
      - `row_index_max`: The end index of this cell's `row` location in the current table.
      - `column_index_min`: The begin index of this cell's `column` location in the current table.
      - `column_index_max`: The end index of this cell's `column` location in the current table.
    - `row_headers`: An array of row-level cells, each applicable as a header to other cells in the same row as itself, of the current table. Each row header is defined as a collection of the following:
      - `id`: A string value in the format `rowHeader-x-y`, where `x` and `y` are the begin and end offsets of this row header cell in the input document.
      - `cell`: The location of this cell in the current table as defined by its `begin` and `end` offsets in the input document.
      - `cell_text`: The textual contents of this cell from the input document without associated markup content.
      - `cell_text_normalized`: If you provide customization input, the normalized version of the cell text according to the customization; otherwise, the same value as `cell_text`.       
      - `row_index_min`: The begin index of this cell's `row` location in the current table.
      - `row_index_max`: The end index of this cell's `row` location in the current table.
      - `column_index_min`: The begin index of this cell's `column` location in the current table.
      - `column_index_max`: The end index of this cell's `column` location in the current table.
    - `body_cells`: An array of cells that are neither table header nor column header nor row header cells, of the current table with corresponding row and column header associations. Each body cell is defined as a collection of the following:
      - `id`: A string value in the format `bodyCell-x-y`, where `x` and `y` are the begin and end offsets of this body cell in the input document.
      - `cell`: The location of this cell in the current table as defined by its `begin` and `end` offsets in the input document.
      - `cell_text`: The textual contents of this cell from the input document without associated markup content. 
      - `row_index_min`: The begin index of this cell's `row` location in the current table.
      - `row_index_max`: The end index of this cell's `row` location in the current table.
      - `column_index_min`: The begin index of this cell's `column` location in the current table.
      - `column_index_max`: The end index of this cell's `column` location in the current table.
      - `row_header_ids`: An array of values, each being the `id` value of a row header that is applicable to this body cell.
      - `row_header_texts`: An array of values, each being the `cell_text` value of a row header that is applicable to this body cell.
      - `row_header_texts_normalized`: If you provide customization input, the normalized version of the row header texts according to the customization; otherwise, the same value as `row_header_texts`. 
      - `column_header_ids`: An array of values, each being the `id` value of a column header that is applicable to this body cell.
      - `column_header_texts`: An array of values, each being the `cell_text` value of a column header that is applicable to this body cell.
      - `column_header_texts_normalized`: If you provide customization input, the normalized version of the column header texts according to the customization; otherwise, the same value as `column_header_texts`.


**Note on tables:**
  - Row and column index values per cell are zero-based and so begin with `0`.
  - Multiple values in arrays of `row_header_ids` and `row_header_texts` elements indicate a hierarchy of row headers.
  - Multiple values in arrays of `column_header_ids` and `column_header_texts` elements indicate a hierarchy of column headers.
