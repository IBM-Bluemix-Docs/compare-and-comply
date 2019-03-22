---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-22"

subcollection: compare-and-comply

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Release notes
{: #release_notes}

The release notes provide information about changes to the IBM Watson&trade; Compare and Comply service release on IBM Cloud Private.
{: shortdesc}

This documentation set applies only to the Compare and Comply service on IBM Cloud Private. It does not apply to other Watson services that are available on the public IBM Cloud, including the public version of Compare and Comply.
{: important}

For release note information that affects all IBM Cloud Private services, see [https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/getting_started/known_issues.html ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/getting_started/known_issues.html){: new_window}.
{: note}

## Service API Versioning
{: #api_versioning}

API requests require a version parameter that takes a date in the format `version=YYYY-MM-DD`. Whenever we change the API in a backwards-incompatible way, we release a new minor version of the API.

Send the version parameter with every API request. The service uses the API version for the date you specify, or the most recent version before that date. Do not default to the current date. Instead, specify a date that matches a version that is compatible with your application, and do not change it until your application is ready for a later version.

The current version is `2018-10-15`.

## Changes
{: #changes}

The following new features and changes to the service are available.

The version number referred to in the following sections is the version of the IBM Watson Compare and Comply Helm chart that you have deployed on your IBM Cloud Private cluster.
{: important}

### 1.1.4, 22 March 2019
{: #114}

Version 1.1.4 includes the following updates:

  - Updated analysis models for [Element Classification](/docs/services/compare-and-comply?topic=compare-and-comply-getting_started#parse_contract) and [Table Understanding](/docs/services/compare-and-comply?topic-compare-and-comply-understanding_tables).
  - Updated PDF parser.
  - Updated IBM Cloud Private components for authentication and provisioning.

### 1.1.3, 18 January 2019
{: #113}

Version 1.1.3 includes the following changes and updates:

  - Support for IBM Cloud Private for Data.
  - The Compare and Comply service on IBM Cloud Private is no longer supported on IBM Private Cloud 2.1.0.3 or earlier.
  - The output of the `/v1/tables` method now includes an `attributes` array for each body cell. For more information, see [Classifying elements](/docs/services/compare-and-comply?topic=compare-and-comply-output_schema) and [Attributes](/docs/services/compare-and-comply?topic=compare-and-comply-output_schemal#attributes).
  - The output of the `/v1/element_classification` method now includes the following:
    - The `parties` array now includes an `importance` field that indicates whether the party is a `Primary` party or an `Unknown` (non-primary) party.
    - The `effective_dates`, `contract_amounts`, and `termination_dates` arrays now each include a `confidence_level` field that indicates a value of `High`, `Medium`, or `Low`.
    For more information, see [Classifying elements](/docs/services/compare-and-comply?topic=compare-and-comply-output_schema) and [Understanding element classification](/docs/services/compare-and-comply?topic=compare-and-comply-contract_parsing).


### 1.1.2, 27 December 2018
{: #112}

Version 1.1.2 includes the following changes and updates:

#### Method paths

The API method paths have been changed to more closely match the public API. Previously the URL was `{ICP-cluster}/{deployment-name}-api/`. The new URL is `{ICP-cluster}/{deployment-name}/compare-and-comply/api`. Both paths can be used with this release. In future releases only the new path will be valid.

#### API Authentication

-  Added standard Watson authentication to service instances. An API key is created with each service instance. This API key must be used to authenticate to the service when making calls.

#### Support for Microsoft Word documents

- Microsoft Word documents can now be used with the service in the same way that PDFs are.


### 1.1.1, 16 November 2018
{: #111}

Version 1.1.1 includes the following changes and updates:

#### API and schema updates

  - A new API version date (`2018-10-15`). If you specify an API version date earlier than `2018-10-15`, you call an older API that most likely has different method names and parameters than those documented for the current release.
  - Changes to the output schema for the `/v1/element_classification` method. See [Getting started](/docs/services/compare-and-comply?topic=compare-and-comply-getting_started) and [Understanding the output schema](/docs/services/compare-and-comply?topic=compare-and-comply-output_schema) for details.
  - Changes to the `/v1/tables` method's output schema. See [Understanding the output schema](/docs/services/compare-and-comply?topic=compare-and-comply-output_schema) and [Classifying tables](/docs/services/compare-and-comply?topic=compare-and-comply-understanding_tables) for information about the table parsing format.

#### New input formats

Compare and Comply now has the ability to process certain image files and text files as listed at [Supported input formats](/docs/services/compare-and comply?topic=compare-and-comply-formats).

The service can process "plain" text (ASCII) files that use a monospaced font and page breaks. Richer text formats that include non-monospaced fonts and style attributes such as bold and italics are not yet supported. If you need to process an enriched text file, convert it to PDF before submitting it to the service.

Supported image formats currently include the following. Scanned image files must have a resolution of at least 300 DPI.
  - BMP
  - GIF
  - JPEG
  - JPEG2000
  - PNG
  - RAW
  - TIFF

The service's methods can accept different types of files as specified in the following table.

| Method           |Image support    |Text support                             |
|------------------|-----------------|-----------------------------------------|
|`/v1/html_conversion`| All supported image formats | Supported |
|`/v1/element_classification`|  All supported image formats | **Not** supported|
|`/v1/tables`      | All supported image formats | Supported |
|`/v1/comparison`*  | All supported image formats | **Not** supported|

\*The `/v1/comparison` method still accepts JSON files.
{: note}


### 1.1.0, 28 September 2018
{: #110}

Version 1.1.0 includes the following changes and updates:

#### Significant enhancements and changes to the API
{: #110_api_updates}

Note the following changes:

  - The version date on all calls is updated to `2018-09-28`.
  - The `POST /v1/parse` method has been deprecated and superseded by several more specific methods:
    - The `POST /v1/html_conversion` method converts a PDF input document to HTML without performing any enhancements or analysis. See [Converting an input document into HTML](/docs/services/compare-and-comply?topic=compare-and-comply-html_conversion) for more information.
    - The `POST /v1/element_classification` method analyzes the semantics and structure of a PDF input document. The output of the method has several enhancements. See [Getting started](/docs/services/compare-and-comply?topic=compare-and-comply-getting_started) and [Understanding the output schema](/docs/services/compare-and-comply?topic=compare-and-comply-output_schema) for more information.
    - The `POST /v1/tables` method analyzes the contents of tables in a PDF input document. The method does not analyze content that is not in tables. See [Classifying tables](/docs/services/compare-and-comply?topic=compare-and-comply-understanding_tables) and [Understanding the output schema](/docs/services/compare-and-comply?topic=compare-and-comply-output_schema) for more information.
    - The `POST /v1/comparison` method analyzes the difference between two PDF or JSON input documents. See [Comparing two documents](/docs/services/compare-and-comply?topic=compare-and-comply-compare) for more information.
  - The `v1/subdomain` methods have been deprecated.

#### Other updates and issues
{: #110_other}

You cannot upgrade from a previous version of Compare and Comply to version 1.1.0. You must deploy version 1.1.0 and, if necessary, delete previous versions of the service. You can delete previous versions either by following the instructions at [Removing a deployment](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/manage_applications/remove_app.html) or by using the `helm delete {old_release} --purge` command, where `{old_release}` is the previous version of the service.
{: important}

  - You can now analyze scanned PDF files that have been processed by an optical character reader (OCR).
  - The maximum size of a file you can submit to the service is 50 MB. However, if you submit multiple files simultaneously or in close succession, you can experience relatively long processing times, depending on the capacity of your IBM Cloud Private cluster.

### 1.0.5, 2 August 2018
{: #105}

  - The addition of table parsing as described at [Understanding the output schema](/docs/services/compare-and-comply?topic=compare-and-comply-output_schema) and [Understanding table parsing](/docs/services/compare-and-comply?topic=compare-and-comply-understanding_tables).

### 1.0.4, 5 July 2018
{: #ingress}

Because of the changes described in this section, you cannot upgrade to version 1.0.4 from previous versions of Compare and Comply by using the standard IBM Cloud Private upgrade procedures. 
{: important}

You must redeploy Compare and Comply as follows:

1.  Remove the existing Compare and Comply deployment as described in [Removing a deployment ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_applications/remove_app.html){: new_window}.

1.  Deploy Compare and Comply version 1.0.4 or later as described in the Compare and Comply Helm chart's `README.md` file.

Observe the following if you need to redeploy the service.
{: important}

Redeploying the service has the following consequences:

- The service is unavailable between the removal of the previous instance and the deployment of the current instance.
- If you have code that makes API calls that include port numbers in the calls' URLs, you must remove the port numbers. If you make an API call that includes a port number to Compare and Comply version 1.0.4 or later, the call fails with an error similar to the following:
  ```
  curl: (7) Failed to connect to 10.19.74.45 port 8443: Connection refused
  ```
  {: pre}

If you are running a version of Compare and Comply earlier than 1.0.4, you must include the `:{port_number}` specifier with the IP address of the ICP Cloud Private cluster when making calls to the service, as in the following example:
```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

#### Additional notes

-   The Compare and Comply service is now integrated with IBM Cloud Private's [ingress controller ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/components.html){: new_window}. This change enables you to call the service's API methods without specifying a port number in the calls' URLs. The following is a typical API call before the **ingress** integration:

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
    ```
    {: pre}

  The equivalent call in version 1.0.4 or later is:

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45/api/v1/parse?version=2018-03-23
    ```
    {: pre}

- When you deploy the 1.0.4 release, you might see an error similar to the following:

    ```
    Unexpected response code 500 from request: GET https://icp-management-ingress:8443/helm-api/api/v1/releases/weblib?locale=en-US HTTP/1.1 Accept: application/json Content-Type: application/json Content-Type: application/json Cookie: *** {} HTTP/1.1 500 Server: openresty/1.11.2.4 Date: Thu, 21 Jun 2018 23:09:05 GMT Content-Type: application/json; charset=utf-8 Content-Length: 93 Connection: close X-Powered-By: Express Cache-Control: private, max-age=0, no-cache Etag: W/"5d-6htOwRqeFWIdvat6vK/kTg" {"statusCode":500,"message":"Internal service error : Cannot read property '0' of undefined"}
    ```
    {: codeblock}

    You can safely disregard and close the error.

### 1.0.3, 25 May 2018

- The output of the `parse` method now includes the `attributes` array. See [Review the analysis](/docs/services/compare-and-comply?topic=compare-and-comply-getting_started#review_analysis) and [Attributes](/docs/services/compare-and-comply?topic=compare-and-comply-contract_parsing_#attributes) for information. You can use the information in the `attributes` array to search for document elements that refer to specific locations; times, dates, time range, or date range; and monetary value and units.
- Each object in the `types` and `categories` arrays includes a `provenance` object. The `provenance` object has one or more `id` keys. Each `id` key has a hashed value that you can send to IBM to provide feedback or receive support.
- Improvements to the accuracy of PDF parsing and to the performance of parsing large PDF files.
- This release is available on IBM Cloud Private 2.1.0.2 or higher. See the catalog entry for the service for details on IBM Cloud Private requirements.
- Possible values for the `assurance` key no longer include `Low`.

### 1.0.2, 19 April 2018

- Additional supported categories. See the [Categories documentation](/docs/services/compare-and-comply?topic=compare-and-comply-contract_parsing#contract_categories) for the most recent list of supported categories.
    - `Asset Use`
    - `Communication`
    - `Safety and Security`
-  Documentation updates, including corrections to the examples in [Getting Started](/docs/services/compare-and-comply?topic=compare-and-comply-getting_started).

### 1.0.1 (General Availability release), 23 March 2018

The following notes apply to the General Availability (GA) release of the IBM Watson Compare and Comply service.

- The GA release is available only on IBM Cloud Private 2.1.0.2 or higher. See the catalog entry for the service for details on IBM Cloud Private requirements.

## General notes
{: #general_notes}

- The Compare and Comply API on IBM Cloud Private does not require authorization as APIs on the public IBM Cloud do.
 - PDFs must be in text format to be parsed. Documents that have been scanned cannot be parsed, even if the scans have been processed by an optical character reader (OCR).

## Known issues
{: #known_issues}

- PDFs with security enabled cannot be parsed.
- Documents with non-standard page layouts (such as 2 or 3 columns per page) do not parse correctly.
