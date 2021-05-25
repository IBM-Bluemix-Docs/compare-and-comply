---

copyright:
  years: 2018, 2021
lastupdated: "2021-05-25"

subcollection: compare-and-comply

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

# Getting started
{: #getting_started}

This short tutorial introduces IBM Watson&reg; Compare and Comply on IBM Cloud Private and goes through the process of classifying a contract to identify component pieces, their nature, the parties affected, and any identified categories.
{: shortdesc}

## Before you begin
{: #gs-before-you-begin}

Before you can use the Compare and Comply service, the service must be deployed and configured on your IBM Cloud Private cluster. On your workstation, you must also install the IBM Cloud Private CLI and log in to your IBM Cloud Private cluster as described in [Installing the IBM Cloud Private CLI](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.1/manage_cluster/install_cli.html).

This tutorial uses an API key to authenticate. For production uses, make sure that you review the [API key management APIs](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.1/apis/cfc_api_files/api_keys.html).
{: tip}

## Step 1: Identify content
{: #identify_content}

Identify appropriate documents to analyze. Compare and Comply can analyze contractual documents that meet the criteria listed in [Supported input formats](/docs/compare-and-comply?topic=compare-and-comply-formats).

For the example in this tutorial, the file must be in PDF or a supported image format.

  You can submit PDF files that have been scanned and processed by an optical character reader (OCR).
  {: tip}

- Files can be up to 1.5 MB when submitted to the service with individual methods. If you submit files through the [`/v1/batches` interface](/docs/compare-and-comply?topic=compare-and-comply-batching), files can be up to 50 MB.
- Secure PDFs (with a password to open) and restricted PDFs (with a password to edit) cannot be processed.

## Step 2: Classify a contract's elements
{: #parse_contract}

In a `bash` shell or equivalent environment such as Cygwin, use the `POST /v1/element_classification` method to classify your contract. The method takes the following input parameters:
  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
  - `file` (**required** `file`): The input file that is to be classified.
  - `model` (optional `string`): If this parameter is specified, the service runs the specified type of element classification. Currently, the only supported value is `contracts`.

Replace the following values:
  - `{cluster_CA_domain}/{deployment_name}` with the name or address of your IBM Cloud Private cluster's domain and the name of your Compare and Comply instance's deployment, respectively.
  - `{input_file}` with the path to the file that is to be classified.
  - `{apikey}` with the API key you copied earlier.

```bash
curl -X POST -u "apikey:{apikey}" -F "file=@{input_file}" https://{cluster_CA_domain}/{deployment_name}/compare-and comply/api/v1/element_classification?version=2018-10-15
```
{: pre}

The method returns a JSON object that contains:

  - [A `documents` object](#documents) that includes the input document's title,  an HTML version of the input document, and the MD5 hash of the input document.
  - Information about the model used to classify the input document.  
  - [An `elements` array](#elements) that details semantic elements identified in the input document.
  - [A `tables` array](#tables) that breaks down the tables identified in the input document.
  - [A `document_structure` object](#doc_struct) that lists section titles and leading sentences identified in the input document.
  - [A `parties` array](#parties) that lists the parties, roles, addresses, and contacts of parties identified in the input document.
  - [Arrays defining `effective_dates`, `contract_amounts`, and `termination_dates`.](#other_arrays)

## Step 3: Review the analysis
{: #review_analysis}

This section provides a high-level overview of the output of the `POST /v1/element_classification` method, focusing on the major sections. See [Understanding the output schema](/docs/compare-and-comply?topic=compare-and-comply-output_schema) for a detailed discussion of the method's output.

### Documents
{: #documents}

The `documents` object provides basic information about the input document.

### Elements
{: #elements}

Each object in the `elements` array describes an element of the contract that Compare and Comply has identified. The following code represents a typical element:

```
{
    "location" : {
      "begin" : 134323,
      "end" : 135109
    },
    "text" : "9. In the event that the Participant's total vested account balance is determined to be less than or equal to $2,000.00 as of the date that the Order is received, the parties will be informed in writing that the QDRO determination fee may potentially liquidate the account.",
    "types" : [ {
      "label" : {
        "nature" : "Obligation",
        "party" : "All Parties"
      },
      "provenance_ids" : ["Nlu0ogWAEGms4vjhhzpMv3iXhm8b8fBqMBNtT/bXH8JI=", "PlyERkjg5is36RpFjVUFXp69eDmGmCxLCXRs1sDMDUCo="
      ]
    } ],
    "categories" : [ {
      "label" : "Communication",
      "provenance_ids" : [ "Cs38YyU6VBFtJK1/bgtEJBlqqWmX5F6OYUciRxQXf7HrN5TOCPuI7QXbkbj4LRXoxVuB3/i9H15q5TU+vFxorhUBeWFfF998OYQiPYViD2yI="
      ]
    } ],
    "attributes" : [ {
      "type" : "Currency",
      "text" : "$2,000.00",
      "attribute" : {
        "begin" : 134780,
        "end" : 134789
      }
    } ]
}
```
{: screen}

Each element has five important sections:
  - `location`: The `begin` and `end` indexes indicating the location of the element in the input document.
  - `text`: The text of the classified element.
  - `types`: An array that includes zero or more `label` objects. Each `label` object includes a `nature` field that lists the effect of the element on the identified party (for example, `Right` or `Exclusion`) and a `party` field that identifies the party or parties affected by the element. See [Types](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing#contract_types) in [Understanding contract parsing](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing) for additional information.
  - `categories`: An array that contains zero or more `label` objects. The value of each `label` object lists a functional category into which the identified element falls. See [Categories](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing#contract_categories) in [Understanding contract parsing](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing) for additional information.
  - `attributes`: An array that lists zero or more objects that define attributes of the element. Currently supported attribute types include `Address`, `Currency`, `DateTime`, `Location`, `Organization`, and `Person`. Each object in the `attributes` array also includes the identified element's text and location; location is defined by the `begin` and `end` indexes of the text in the input document. See [Attributes](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing#attributes) in [Understanding contract parsing](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing) for additional information.

Additionally, each object in the `types` and `categories` arrays includes a `provenance_ids` array. The values listed in the `provenance_ids` array are hashed values that you can send to IBM to provide feedback or receive support about the part of the analysis associated with the element.

Some sentences do not fall under any type or category, in which case the service returns the `types` and `categories` arrays as empty objects.
{: note}

Some sentences cover multiple topics, in which case the service returns multiple sets of `types` and `categories` objects.
{: note}

Some sentences do not contain any identifiable attributes, in which case the service returns the `attributes` array as empty objects.
{: note}


### Tables
{: tables}

The `tables` array details the structure and content of any tables found in the input document. See [Classifying tables](/docs/compare-and-comply?topic=compare-and-comply-understanding_tables) and [Understanding the output schema](/docs/compare-and-comply?topic=compare-and-comply-output_schema) for details.

### Document structure
{: #gs_doc_struct}

The `document_structure` object identifies the section titles and leading sentences of the input document. See [Understanding document structure](/docs/compare-and-comply?topic=compare-and-comply-doc_struct) for details.

### Parties
{: #parties}

The `parties` array lists available information about parties affected by the input document, including the party's name, role, address or addresses, and contacts. See [Parties](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing#contract_parties) in [Understanding contract parsing](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing) for additional information.

```
  "parties": [
      {
      "party": "Wolfbone Investments, LLC",
      "role": "Supplier",
      "importance": "Primary",
      "addresses": [],
      "contacts": [
        {
         "name": "Will Smith",
         "role": "business contact"
        },
        ...
      ]
    },
    {
      "party": "Torchlight Energy, Inc.",
      "role": "Buyer",
      "importance": "Primary",
      "addresses": [
       {
       "text": "5700 W. Plano Pkwy., Ste. 3600, Plano, Texas 75093",
       "location": {
          "begin": 150,
          "end": 200
        }
       },
       ...
      ],
      "contacts": [ ]
    }
  ]
```
{: screen}

### Other arrays
{: #other_arrays}

The following arrays provide useful information about the input document. Each of the arrays contains zero or more objects that list the `text` in which the information was identified, the confidence level of the identification (`High`, `Medium`, or `Low`), and the `location` of that text as defined by the text's `begin` and `end` indexes.

  - The `effective_dates` array lists any effective dates identified in the input document.
  - The `contract_amounts` array lists monetary amounts specified by the input document.
  - The `termination_dates` array lists the input document's termination dates.

## Next steps
{: #next_steps}

You have successfully classified a contract to identify its elements, tables, structure, parties, and other information. You can use the analysis to quickly understand and enforce the classified contract. The next steps are:

 - [Understanding contract parsing](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing)
 - [Understanding the output schema](/docs/compare-and-comply?topic=compare-and-comply-output_schema) and [classifying tables](/docs/compare-and-comply?topic=compare-and-comply-understanding_tables)
