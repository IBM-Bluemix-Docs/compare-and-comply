
---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-03"

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

# Release notes
{: #release_notes}

The release notes provide information about changes to the {{site.data.keyword.cnc_long}} service release on {{site.data.keyword.BluOpenStackDed}}.

**Important:** This documentation set applies only to the {{site.data.keyword.cnc_short}} service on {{site.data.keyword.BluOpenStackDed}}. It does not apply to other Watson services that are available on the public IBM Cloud.

## Service API Versioning
{: #api_versioning}

API requests require a version parameter that takes a date in the format `version=YYYY-MM-DD`. Whenever we change the API in a backwards-incompatible way, we release a new minor version of the API.

Send the version parameter with every API request. The service uses the API version for the date you specify, or the most recent version before that date. Do not default to the current date. Instead, specify a date that matches a version that is compatible with your application, and do not change it until your application is ready for a later version.

The current version is `2018-03-23`.

## Changes
{: #changes}

The following new features and changes to the service are available.

**Important**: The version number referred to in the following sections is the version of the {{site.data.keyword.cnc_long}} Helm chart that you have deployed on your {{site.data.keyword.BluOpenStackDed}} cluster.

### 1.0.4, 5 July 2018
{: #ingress}

**Important**: Because of the changes described in this section, you cannot upgrade to version 1.0.4 from previous versions of {{site.data.keyword.cnc_short}} by using the standard {{site.data.keyword.BluOpenStackDed}} upgrade procedures. You must redeploy {{site.data.keyword.cnc_short}} as follows:

1.  Remove the existing {{site.data.keyword.cnc_short}} deployment as described in [Removing a deployment ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_applications/remove_app.html){: new_window}.

1.  Deploy {{site.data.keyword.cnc_short}} version 1.0.4 or later as described in the {{site.data.keyword.cnc_short}} Helm chart's `README.md` file and in [Deploying the service](/docs/services/compare-and-comply/deploy.html).

**Important:** Redeploying the service has the following consequences:

- The service is unavailable between the removal of the previous instance and the deployment of the current instance.
- If you have code that makes API calls that include port numbers in the calls' URLs, you must remove the port numbers. If you make an API call that includes a port number to {{site.data.keyword.cnc_short}} version 1.0.4 or later, the call fails with an error similar to the following:
  ```
  curl: (7) Failed to connect to 10.19.74.45 port 8443: Connection refused
  ```
  {: pre}

**Important:** If you are running a version of {{site.data.keyword.cnc_short}} earlier than 1.0.4, you must include the `:{port_number}` specifier with the IP address of the {{site.data.keyword.BluOpenStackDed}} cluster when making calls to the service, as in the following example:
```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

#### Additional notes

-   Additional deployment instructions are now provided at [Deploying the service](/docs/services/compare-and-comply/deploy.html).
-   The {{site.data.keyword.cnc_short}} service is now integrated with {{site.data.keyword.BluOpenStackDed}}'s [ingress controller ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/components.html){: new_window}. This change enables you to call the service's API methods without specifying a port number in the calls' URLs. The following is a typical API call before the **ingress** integration:

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
    ```
    {: pre}

  The equivalent call in 1.0.4 or later is:

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

- The output of the `parse` method now includes the `attributes` array. See [Review the analysis](/docs/services/compare-and-comply/getting-started.html#review_analysis) and [Attributes](/docs/services/compare-and-comply/parsing.html#attributes) for information. You can use the information in the `attributes` array to search for document elements that refer to specific locations; times, dates, time range, or date range; and monetary value and units.
- Each object in the `types` and `categories` arrays includes a `provenance` object. The `provenance` object has one or more `id` keys. Each `id` key has a hashed value that you can send to IBM to provide feedback or receive support.
- Improvements to the accuracy of PDF parsing and to the performance of parsing large PDF files.
- This release is available on {{site.data.keyword.BluOpenStackDed}} 2.1.0.2, 2.1.0.3, or higher. See the catalog entry for the service for details on {{site.data.keyword.BluOpenStackDed}} requirements.
- Possible values for the `assurance` key no longer include `Low`.

### 1.0.2, 19 April 2018

- Additional supported categories. See the [Categories documentation](/docs/services/compare-and-comply/parsing.html#contract_categories) for the most recent list of supported categories.
    - `Asset Use`
    - `Communication`
    - `Safety and Security`
-  Documentation updates, including corrections to the examples in [Getting Started](/docs/services/compare-and-comply/getting-started.html).

### 1.0.1 (General Availability release), 23 March 2018

The following notes apply to the General Availability (GA) release of the {{site.data.keyword.cnc_long}} service.

- The GA release is available only on {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 or higher. See the catalog entry for the service for details on {{site.data.keyword.BluOpenStackDed}} requirements.

## General notes
{: #general_notes}

- The {{site.data.keyword.cnc_short}} API on {{site.data.keyword.BluOpenStackDed}} does not require authorization as APIs on the public IBM Cloud do.
 - PDFs must be in text format to be parsed. Scanned documents, even if they have been OCRed, cannot be parsed.

## Known issues
{: #known_issues}

- The maximum size of PDF that can be uploaded is 50Mb.
- PDFs with security enabled cannot be parsed.
- Documents with non-standard page layouts (such as 2 or 3 columns per page) do not parse correctly.
