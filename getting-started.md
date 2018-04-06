---

copyright:
  years: 2017, 2018
lastupdated: "2018-04-05"

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

# Getting started
{: #getting_started}

In this short tutorial, we introduce {{site.data.keyword.cnc_long}} on IBM Cloud Private and go through the process of parsing a contract to identify component pieces, their nature, the parties affected, and any identified categories.

## Step 1: Identify content
{: #identify_content}

Identify appropriate documents to analyze. {{site.data.keyword.cnc_short}} has been designed to analyze contract <!-- and regulatory -->documents that meet the following criteria:

- Files to be analyzed are in PDF format.
- The PDF contents are in text form. Documents that have been scanned cannot be parsed, even if they have been OCRed.

  **Note:** You can identify a PDF that is in text by opening the document in a PDF viewer and using the  **Text select**  tool to select a single word. If you cannot select a single word in the document, the file cannot be parsed.

- Files are no larger than 50Mb in size.
- Secure PDFs (with a password to open) and editing restricted PDFs (with a password to edit) cannot be parsed.

## Step 2: Parse a contract
{: #parse_contract}

In a `bash` shell or equivalent environment such as Cygwin, use the `POST /v1/parse` method to parse your contract. Replace `{IP_address}:{port_number}` with the IP address and port number for your IBM Cloud Private installation. Replace `{PDF_file}` with the path to the PDF to parse.

```
curl -k -X POST -F 'file=@{PDF_file};type=application/pdf' https://{IP_address}:{port_number}/api/v1/parse?version=2018-03-23
```
{: codeblock}

The method returns a JSON object that contains:

 - An HTML conversion of the source PDF file.
 - The extracted document title.
 - An array of `elements` that define the component parts of the contract.
 - An array of `parties` that define entities that have been identified as parties.
 
**Important**: The {{site.data.keyword.cnc_short}} API on IBM Cloud Private does not require authorization as APIs on the public IBM Cloud do.

## Step 3: Review the analysis
{: #review_analysis}

Each object in the `elements` array describes an element of the contract that {{site.data.keyword.cnc_short}} has identified. The following code represents a typical element:

```
  {
    "sentence" : {
      "begin" : 6185,
      "end" : 6331
    },
    "sentence_text" : "(ii) the parties wish to grant and receive certain other licenses (or options with respect to licenses), in each case as further described herein.",
    "types" : [ {
      "label" : {
        "nature" : "Obligation",
        "party" : "Supplier"
      },
      "assurance" : "Low"
    } ],
    "categories" : [ {
      "label" : "Intellectual Property",
      "assurance" : "High"
    } ]
  }
```

The element has four important sections:
 - `sentence_text`: The text that was analyzed.
 - `sentence`: An object that describes where the element was found in the converted HTML. It contains a `start` character value and an `end` character value.
 - `types`: An array that describes what the element is and whom it affects. It consists of one or more sets of `nature` keys (the effect of the sentence on the identified `party`) and `party` keys (whom the sentence affects).
 - `categories` (_optional_): An array that lists the functional categories into which the identified sentence falls; in other words, the subject matter of the sentence.

**Note**: Some sentences do not fall under any type or category, in which case the service returns the `types` and `categories` arrays as empty objects.

**Note:** Some sentences cover multiple topics, in which case the service returns multiple sets of `types` and `categories` objects.

Additionally, any identified parties are defined in the `parties` array. The `parties` array is located after the `elements` array in the JSON output.

```
  "parties" : [ {
    "party" : "Customer",
    "role" : "Buyer"
  } ]
```

The `parties` array includes two important sections:

 - `party`: The text that was identified as a party within the document.
 - `role`: The role of the identified party. Roles changed based on subdomain; see [the documentation on the specified subdomain for a list of possible roles](parsing.md#contract_parties). Parties that cannot be identified as having a specific role are listed with the `unknown` value.

## Next steps
{: #next_steps}

You have successfully parsed a contract to identify the nature, parties, and categories of the component parts of the document. You can use the analysis to quickly understand and enforce the parsed contract. The next steps are to:

 - Understand types and categories.
 - Review parsing options.
 
An accessible version of this page is available at: [https://www.ibm.com/watson/developercloud/compare-and-comply/doc/getting-started.html](https://www.ibm.com/watson/developercloud/compare-and-comply/doc/getting-started.html)

