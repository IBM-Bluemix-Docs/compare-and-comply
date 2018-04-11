---

copyright:
  years: 2017, 2018
lastupdated: "2018-04-10"

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

## Service API Versioning
{: #api_versioning}

API requests require a version parameter that takes a date in the format `version=YYYY-MM-DD`. Whenever we change the API in a backwards-incompatible way, we release a new minor version of the API.

Send the version parameter with every API request. The service uses the API version for the date you specify, or the most recent version before that date. Do not default to the current date. Instead, specify a date that matches a version that is compatible with your application, and do not change it until your application is ready for a later version.

The current version is `2018-03-23`.

## Changes
{: #changes}

The following new features and changes to the service are available.

**Important**: The version number referred to in the following sections is the version of the {{site.data.keyword.cnc_long}} Helm chart that you have deployed on your IBM Cloud Private instance. 

### 1.0.4, 10 April 2018

Additional supported `categories`: `Asset Use` and `Communication`.

See the [Categories documentation](parsing.md#contract_categories) for the most recent list of supported categories.

### 1.0.3, 6 April 2018

 - Updates to the descriptions of supported `categories`. See the [Categories documentation](parsing.md#contract_categories) for the most recent list of supported categories.
 - Documentation updates, including corrections to the examples in [Getting Started](getting-started.md).

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

