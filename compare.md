---

copyright:
years: 2018
lastupdated: "2018-07-12"

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

# Comparing two documents
{: #compare}

You can use the service to compare two documents. For example, you can compare a new, unsigned contract with a signed contract from the previous year. 

The `POST /v1/comparison` method enables you to compare two documents. Specifically, the method finds and reports semantically aligned elements from the documents. It also reports elements from each document that do not semantically align with any other element.

## Step 1: Identify two comparable documents
{: #step1}

Identify two documents to compare. See [Step 1 in the Getting started](/docs/services/compare-and-comply/getting-started.html)  for information on document requirements. For best results, choose two documents that are related to each other; for example, an original contract and a revised version of the same contract.

## Step 2: Compare two documents
{: #step2}

In a `bash` shell or equivalent environment such as Cygwin, issue the following command to compare the documents, with values as follows:
  - Replace `{file1}` and `{file2}` with the path to the PDF or JSON files you want to compare.
  - Optionally specify labels for `file1_label` and `file2_label` to identify files 1 and 2, respectively. If you do not specify labels, the method uses the default label values `file1` and `file2`.
  - Optionally specify the value contract for the subdomain parameter.
    **Note:** The only subdomain value accepted by the `POST /v1/comparison` method is `contract`.

```bash
curl -X POST -k -H "Content-Type: multipart/form-data" -F "file1=@/Users/Downloads/{file1}.pdf" -F "file2=@/Users/Downloads/{file2}.pdf" -F file1_label="document_1" -F file2_label="document_2" https://{ICP_IP_address}/compare-and-comply/api/v1/compare?version=2018-09-28&subdomain=contract
```
{: codeblock}

If you are submitting JSON files instead of PDF files for comparison, specify the media type for the JSON files as follows:

```bash
curl -X POST -k -H "Content-Type: multipart/form-data" -F "file1=@/Users/Downloads/{file1}.json;type=application/json" -F "file2=@/Users/Downloads/{file2}.json;type=application/json" -F file1_label="document_1" -F file2_label="document_2" https://{ICP_IP_address}/compare-and-comply/api/v1/compare?version=2018-09-28
```
{: codeblock}

## Step 3: Review the comparison
{: #step3}

The method returns a JSON object that contains the aligned and unaligned elements. The JSON object uses the following schema:

```
{
  "documents": [
    {
      "document_label": string,
      "document_text": string,
      "document_title": string
    },
    {
      "document_label": string,
      "document_text": string,
      "document_title": string
    }
  ],
  "aligned_elements": [
    {
      "element_pair": [
        {
          "document_label": string,
          "sentence": { "begin": int, "end": int },
          "sentence_text": string,
          "types": [
            {
              "label": { "nature": string, "party": string }
            },
            ...
                ],
                "categories": [
                    {
                        "label": string
                    },
                    ...
                ]
            }, 
            {
              "document_label": string,
              "sentence": { "begin": int, "end": int },
              "sentence_text": string,
              "types": [
                {
                   "label": { "nature": string, "party": string }
                },
                ...
                ],
                "categories": [
                  {
                    "label": string
                  },
                  ...
                ]
            }
        ],
        "identical_text": boolean
        },
        ...
  ],
  "unaligned_elements": [
    {
      "document_label": string,
      "sentence": { "begin": int, "end": int },
      "sentence_text": string,
      "types": [
        {
          "label": { "nature": string, "party": string }
        },
        ...
      ],
      "categories": [
        {
          "label": string
        },
        ...
      ]
    },
    ...
  ] 
}
```
{ :pre}

The JSON output includes three arrays:
  - `documents`: Provides the following information about the documents being compared:
    - The `document_title` key inside the `documents` array lists the filenames of the input documents.
    - The `document_text` key contains the HTML version of the input files. It represents the HTML over which element spans are applied.
    - The `document_label` keys that appear inside the arrays contain the values passed as `file1_label` or `file2_label`, depending on which input document the entry represents.
  - `aligned_elements`: Lists pairs of elements that semantically align in the compared documents.
  - `unaligned_elements`: Lists elements that do not semantically align between the compared documents.
  
A sample output file resembles the following:

```
{
  "documents": [
    {
      "document_title": "31235_000156459017003570_re-ex1013_296.pdf",
      "document_text": "<html><head>...",
      "document_label": "file1"
    },
    {
      "document_title": "31235_000156459017003570_re-ex1013_297.pdf",
      "document_text": "<html><head>...",
      "document_label": "file2"
    }
  ]

  "aligned_elements": [
    {
      "element_pair": [
        {
          "document_label": "file1",
          "sentence": {
            "begin": 5690,
            "end": 5865
          },
          "sentence_text": "(g) The Committee shall determine whether an event has occurred resulting in the forfeiture of the Shares, in accordance with this Agreement, and all determinations of the Committee shall be final and conclusive.",
          "types": [
            {
              "label": {
                "nature": "Obligation",
                "party": "Committee"
              }
            }
          ],
          "categories": []
        },
        {
          "document_label": "file2",
          "sentence": {
            "begin": 5772,
            "end": 5947
          },
          "sentence_text": "(g) The Committee shall determine whether an event has occurred resulting in the forfeiture of the RSUs and any Shares issuable thereunder in accordance with this Agreement and all determinations of the Committee shall be final and conclusive.",
          "types": [
            {
              "label": {
                "nature": "Obligation",
                "party": "Committee"
              }
            }
          ],
          "categories": []
        }
      ],
      "identical_text": false
    },
  ...
  ]
  "unaligned_elements": [
    {
      "document_label": "file1",
      "sentence": {
        "begin": 6590,
        "end": 6684
      },
      "sentence_text": "The RSUs (at the time of vesting or otherwise) will be includible as compensation for pension.",
      "types": [],
      "categories": []
    },
    ...
  ]
}
```
{: pre}
