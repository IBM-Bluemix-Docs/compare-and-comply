---

copyright:
  years: 2017, 2018
lastupdated: "2018-04-27"

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

The release notes provide information about changes to the {{site.data.keyword.cnc_long}} service release on IBM Private Cloud.

**Important:** This documentation set applies only to the {{site.data.keyword.cnc_short}} service on IBM Cloud Private. It does not apply to other Watson services that are available on the public IBM Cloud.

## Service API Versioning
{: #api_versioning}

API requests require a version parameter that takes a date in the format `version=YYYY-MM-DD`. Whenever we change the API in a backwards-incompatible way, we release a new minor version of the API.

Send the version parameter with every API request. The service uses the API version for the date you specify, or the most recent version before that date. Do not default to the current date. Instead, specify a date that matches a version that is compatible with your application, and do not change it until your application is ready for a later version.

The current version is `2018-03-23`.

## Changes
{: #changes}

The following new features and changes to the service are available.

**Important**: The version number referred to in the following sections is the version of the {{site.data.keyword.cnc_long}} Helm chart that you have deployed on your IBM Cloud Private instance. 

### 1.0.3, 28 April 2018

 - The output of the `parse` method now includes the `attributes` array. See [Review the analysis](/docs/services/compare-and-comply/getting-started.html#review_analysis) and [Attributes](/docs/services/compare-and-comply/parsing.html#attributes) for information. You can use the information in the `attributes` array to search for document elements that refer to specific locations; times, dates, time range, or date range; and monetary value and units.
 - Each object in the `types` and `categories` arrays includes a `provenance` object. The `provenance` object has one or more `id` keys. Each `id` key has a hashed value that you can send to IBM to provide feedback or receive support.

### 1.0.2, 19 April 2018

 - Additional supported categories. See the [Categories documentation](/docs/services/compare-and-comply/parsing.html#contract_categories) for the most recent list of supported categories.
   - `Asset Use`
   - `Communication`
   - `Safety and Security`
 -  Documentation updates, including corrections to the examples in [Getting Started](/docs/services/compare-and-comply/getting-started.html).

### 1.0.1 (General Availability release), 23 March 2018

The following notes apply to the General Availability (GA) release of the {{site.data.keyword.cnc_long}} service.

 - The GA release is available only on IBM Cloud Private 2.1.0.2 or higher. See the catalog entry for the service for details on IBM Cloud Private requirements.

## General notes
{: #general_notes}

 - The {{site.data.keyword.cnc_short}} API on IBM Cloud Private does not require authorization as APIs on the public IBM Cloud do.
 - PDFs must be in text format to be parsed. Scanned documents, even if they have been OCRed, cannot be parsed.
 
## Known issues
{: #known_issues}

 - The maximum size of PDF that can be uploaded is 50Mb.
 - PDFs with security enabled cannot be parsed.
 - Documents with non-standard page layouts (such as 2 or 3 columns per page) do not parse correctly.

