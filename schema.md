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

# Understanding the output schema
{: #output_schema}

After a document has been parsed and enriched by the service, the service provides JSON output in the following schema.

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

The schema is arranged as follows. 

 - `document_text`: The full text of the parsed document in HTML format.
 - `document_title`: The title of the parsed document. If the service did not detect a title, the value of this element is `null`.
 - `elements`: An array of the document elements detected by the service.
    - `sentence`: The location of the sentence as defined by its begin and end indexes.
    - `sentence_text`: The text of the sentence.
    - `types`: An array that describes what the element is and whom it affects. It consists of one or more sets of `nature` keys (the effect of the sentence on the identified `party`) and `party` keys (whom the sentence affects).
      - `label`: An array of `nature` and `party` elements.
        - `nature`: The type of action the sentence requires. Possible values are `Definition`, `Disclaimer`, `Exclusion`, `Obligation`, and `Right`.
        - `party`: A string identifying the party to whom the sentence applies.
      - `assurance`: The confidence that the detected type is applicable. Possible values are `High` and `Low`.
      - `provenance`: Each object in the `types` and `categories` arrays includes a `provenance` object. The object has one or more `id` keys.
        - `id`: A hashed value that you can send to IBM to provide feedback or receive support.
    - `categories`: An array that lists the functional categories into which the sentence falls; in other words, the subject matter of the sentence.
      - `label`: The category into which the sentence falls. See [Categories at Understanding contract parsing](/docs/services/compare-and-comply/parsing.html#contract_categories) for a list of categories.
      - `assurance`: The confidence that the detected category is applicable. Possible values are `High` and `Low`.
      - `provenance`: Each object in the `types` and `categories` arrays includes a `provenance` object that includes one or more `id` keys.
        - `id`: A hashed value that you can send to IBM to provide feedback or receive support.
    - `attributes`: An array of `type`, `text`, and `attribute` elements.
      - `type`: The type of attribute. Possible values are `Location`, `DateTime`, and `Currency`.
      - `text`: The text that is associated with the attribute.
      - `attribute`: The location of the attribute as defined by its `begin` and `end` indexes.


  - `tables`: An array defining the tables identified by the service.
    - `table`: The location of the current table as defined by its `begin` and `end` offsets in the input document.
    - `table_text`: The textual contents of the current table from the input document without associated markup content.
    - `column_headers`: An array of column-level cells, each applicable as a header to other cells in the same column as itself, of the current table. Each column header is defined as a collection of the following:
      - `id`: A string value in the format `columnHeader-x-y`, where `x` and `y` are the begin and end offsets of this column header cell in the input document.
      - `cell`: The location of this cell in the current table as defined by its `begin` and `end` offsets in the input document.
      - `cell_text`: The textual contents of this cell from the input document without associated markup content. 
      - `row_index_min`: The begin index of this cell's `row` location in the current table.
      - `row_index_max`: The end index of this cell's `row` location in the current table.
      - `column_index_min`: The begin index of this cell's `column` location in the current table.
      - `column_index_max`: The end index of this cell's `column` location in the current table.
    - `row_headers`: An array of row-level cells, each applicable as a header to other cells in the same row as itself, of the current table. Each row header is defined as a collection of the following:
      - `id`: A string value in the format `rowHeader-x-y`, where `x` and `y` are the begin and end offsets of this row header cell in the input document.
      - `cell`: The location of this cell in the current table as defined by its `begin` and `end` offsets in the input document.
      - `cell_text`: The textual contents of this cell from the input document without associated markup content. 
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
      - `column_header_ids`: An array of values, each being the `id` value of a column header that is applicable to this body cell.
      - `column_header_texts`: An array of values, each being the `cell_text` value of a column header that is applicable to this body cell.
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
      
**Note:**
  - Row and column index values per cell are zero-based and so begin with `0`.
  - Multiple values in arrays of `row_header_ids` and `row_header_texts` elements indicate a hierarchy of row headers.
  - Multiple values in arrays of `column_header_ids` and `column_header_texts` elements indicate a hierarchy of column headers.
