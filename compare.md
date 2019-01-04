---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-04"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Comparing documents
{: #compare}

You can use the service to compare two documents. For example, you can compare a new, unsigned contract with a signed contract from the previous year.
{: shortdesc}

The `POST /v1/comparison` method enables you to compare two documents. Specifically, the method finds and reports semantically aligned elements from the documents. It also reports elements from each document that do not semantically align with any other element.

## Step 1: Identify two comparable documents
{: #step1}

Identify two documents to compare. See [Step 1 in Getting started](/docs/services/compare-and-comply/getting-started.html) and [Supported input formats](/docs/services/compare-and-comply/formats.html#formats) for information on document requirements. For best results, choose two documents that are related to each other; for example, an original contract and a revised version of the same contract.

## Step 2: Compare two documents
{: #step2}

In a `bash` shell or equivalent environment such as Cygwin, issue the following command to compare the documents, with values as follows:
  - Replace `{apikey_value}` with the API key you copied in [Before you begin in Getting Started](/docs/services/compare-and-comply/getting-started.html#before-you-begin).
  - Replace `{file_1}` and `{file_2}` with the path to the PDF or JSON files you want to compare.
  - Optionally specify values for `file_1_label` and `file_2_label` to identify files 1 and 2, respectively. If you do not specify labels, the method uses the default label values `file_1` and `file_2`.
  - Optionally specify the value `contracts` for the `model` parameter. The only model value accepted by the `POST /v1/comparison` method is `contracts`.

```bash
curl -X POST -u "apikey":"{apikey_value}" -H "Content-Type: multipart/form-data" -F "file_1=@/Users/Downloads/{file_1}" -F "file_2=@/Users/Downloads/{file_2}" -F file_1_label="document_1" -F file_2_label="document_2" https://{cluster_CA_domain}/{deployment_name}/compare-and-comply/api/v1/comparison?version=2018-10-15&model=contracts
```
{: pre}

If you are submitting JSON files for comparison, specify the media type for the JSON files as follows:

```bash
curl -X POST -u "apikey":"{apikey_value}" -H "Content-Type: multipart/form-data" -F "file_1=@/Users/Downloads/{file_1}.json;type=application/json" -F "file_2=@/Users/Downloads/{file_2}.json;type=application/json" -F file_1_label="document_1" -F file_2_label="document_2" https://{cluster_CA_domain}/{deployment_name}/compare-and-comply/api/v1/comparison?version=2018-10-15
```
{: pre}

## Step 3: Review the comparison
{: #step3}

The method returns a JSON object that contains the aligned and unaligned elements. The JSON object uses the following schema:

```
{
  "model_id" : string,
  "model_version" : string,
  "documents": [
    {
      "label": string,
      "html": string,
      "hash": string,
      "title": string
    },
    {
      "label": string,
      "html": string,
      "hash": string,
      "title": string
    }
  ],
  "aligned_elements": [
    {
      "element_pair": [
        {
          "document_label": string,
          "location": { "begin": int, "end": int },
          "text": string,
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
          ],
          "attributes": [
            {
              "type": string,
              "text": string,
              "location": { "begin": int, "end", int }
            },
            ...
          ]
        },
        {
          "document_label": string,
          "location": { "begin": int, "end", int },
          "text": string,
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
          ],
          "attributes": [
            {
              "type": string,
              "text": string,
              "location": { "begin": int, "end", int }
            },
            ...
          ]
        }
      ],
      "identical_text": boolean,
      "significant_elements": boolean,
      "provenance_ids": [ string, ... ]
    },
    ...
  ],
  "unaligned_elements": [
    {
      "document_label": string,
      "location": { "begin": int, "end", int },
      "text": string,
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
      ],
      "attributes": [
        {
          "type": string,
          "text": string,
          "location": { "begin": int, "end", int }
        },
        ...
      ]
    },
    ...
  ]
}
```
{: screen}

The JSON output includes five objects, three of which are arrays:
  - `model_id`: The model or subdomain used for the comparison. Currently, the only supported value is `contracts`.
  - `model_version`: The version number of the model or subdomain used for the comparison.
  - `documents`: An array that provides the following information about the documents being compared:
    - The `title` key inside the `documents` array lists the filename of an input document.
    - The `html` key contains the HTML version of the input file. It represents the HTML over which element spans are applied.
    - The `hash` key contains the MD5 hash value of the input file.
    - The `label` keys that appear inside the arrays contain the values passed as `file_1_label` or `file_2_label`, depending on which input document the entry represents.
  - `aligned_elements`: An array that lists pairs of elements that semantically align in the compared documents.
    - The `identical_text` key is a boolean that indicates if the aligned text is identical.
    - The `significant_elements` key is a boolean that indicates if the aligned text contains contractual clauses of significance. An _insignificant_ element is one that has no `types` or `categories`, has text that is shorter in length, and is not a structural element such as a section title, a leading sentence, or a list item.
  - `unaligned_elements`: An array that lists elements that do not semantically align between the compared documents.

  The `provenance_ids` field is useful when applying feedback to compared documents.
  {: tip}

A sample output file resembles the following:

```
{
 "model_id" : "contracts",
 "model_version" : "1.0.0",
 "documents": [
    {
      "title": "31235_000156459017003570_kodk-ex1013_296.pdf",
      "html": "<html><head>...",
      "hash": "abdfds",
      "label": "file_1"
    },
    {
      "title": "31235_000156459017003570_kodk-ex1013_297.pdf",
      "html": "<html><head>...",
      "hash": "abdfds",
      "label": "file_2"
    }
  ]
"aligned_elements": [
    {
      "element_pair": [
        {
          "document_label": "file_1",
          "location": {
            "begin": 5690,
            "end": 5865
          },
          "text": "You will not have the rights of a Kodak shareholder with respect to the shares issued to you in payment of your RSUs until the shares are actually issued and delivered to you.",
          "types": [
            {
              "label": {
                "nature": "Exclusion",
                "party": "You"
              }
            }
          ],
          "categories": [],
          "attributes": []
        },
        {
          "document_label": "file_2",
          "location": {
            "begin": 5772,
            "end": 5947
          },
          "text": "You will not have the rights of a Kodak shareholder with respect to the shares issued to you in payment of your RSUs until the shares are actually issued and delivered to you.",
          "types": [
            {
              "label": {
                "nature": "Exclusion",
                "party": "You"
              }
            }
          ],
          "categories": [],
          "attributes": []
        }
      ],
      "identical_text": true,
      "significant_elements": true,
      "provenance_ids": [ "R1432223" ]
    },
    ...
  ]
"unaligned_elements": [
    {
      "document_label": "file_1",
      "location": {
        "begin": 6590,
        "end": 6684
      },
      "text": "The RSUs (at the time of vesting or otherwise) will be includible as compensation for pension.",
      "types": [],
      "categories": [],
      "attributes": []
    },
    ...
  ]
}
```
{: screen}
