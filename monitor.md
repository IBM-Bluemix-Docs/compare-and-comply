---

copyright:
  years: 2017, 2021
lastupdated: "2021-05-25"

subcollection: compare-and-comply

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

# Installing, configuring, and using the monitoring dashboards
{: #monitor}

You can monitor the status of Compare and Comply by using IBM Cloud Private's monitoring dashboards. The monitoring dashboards use Grafana, Kibana, and Prometheus to display detailed, customizable information about your Compare and Comply instance.
{: shortdesc}

For information about monitoring your IBM Cloud Private cluster, see [https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/manage_metrics/monitoring_service.html ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/manage_metrics/monitoring_service.html){: new_window}.

Before using the dashboards, you must generate and install them as described in the following steps.

## Step 1: Download, extract, and render the dashboard templates
{: #render}

Perform the following steps to prepare the dashboard templates for installation.

1. Download the Compare and Comply image from Passport Advantage (PPA). The file is a zipped tar file with a name similar to `ibm-watson-compare-comply-prod-1.1.4.tar.gz`. The file includes the dashboard templates and a `bash` script to render the dashboards from the templates.

1. Uncompress and expand the tar file:
  ```bash
  mkdir ibm-watson-compare-comply-prod-1.1.4 && tar -xvzf ibm-watson-compare-comply-prod-1.1.4.tar.gz -C ibm-watson-compare-comply-prod-1.1.4
  ```
  {: pre}

1. Change to the `charts` directory in the extracted directory:
  ```bash
  cd ibm-watson-compare-comply-prod-1.1.1/charts
  ```
  {: pre}

1. Uncompress and expand the zipped tar file in the `charts` directory:
  ```bash
  tar -xvzf ibm-watson-compare-comply-prod-1.1.1.tgz
  ```
  {: pre}

1. Change to the `dashboard` directory. It includes templates for alerts, logging, and metrics, as well as a `bash` script to generate dashboards
from templates.
  ```
  $ cd ibm-watson-compare-comply-prod/dashboard

  $ tree
  .
  ├── alerts.json.tpl
  ├── external-process-logging.json.tpl
  ├── frontend-logging.json.tpl
  ├── metrics.json.tpl
  └── render-dashboards.sh

  0 directories, 5 files
  ```

1. Run the `render-dashboards.sh` script to render the templates. Options for the script include:

    - `-v`, `--version {chart_version}`: The chart version; for example, `1.1.1`.
    - `-h`, `--help`: Print command help and exit.
    - `-r`, `--release {release_name}`: The Helm release name.
    - `-n`, `--namespace {namespace}`: The namespace of the deployment. The default namespace is `default`.

  ```bash
  ./render-dashboards.sh -v 1.1.1 -r my-test-release -n default
  ```
  {: pre}

The dashboard JSON files are generated under `ibm-watson-compare-comply-prod-1.1.0/charts/ibm-watson-compare-comply-prod/dashboard`.

  ```
  $ tree
  .
  ├── alerts.json
  ├── alerts.json.tpl
  ├── external-process-logging.json
  ├── external-process-logging.json.tpl
  ├── frontend-logging.json
  ├── frontend-logging.json.tpl
  ├── metrics.json
  ├── metrics.json.tpl
  └── render-dashboards.sh

  0 directories, 9 files
  ```

## Step 2: Import the templates
{: #import-tpls}

After you render the dashboards, import one or more of them as described in the following sections:

  - [Importing the metrics dashboard](/docs/compare-and-comply?topic-compare-and-comply-metrics#import-metrics)
  - [Importing the logging dashboards](/docs/compare-and-comply?topic-compare-and-comply-logging#import-logging-dashboards)
  - [Importing the alerts dashboard](/docs/compare-and-comply?topic=compare-and-comply-alerts#import-alert-dashboards)

## Step 3: View and customize dashboards
{: #use-dboards}

After you import your selected dashboards, view and customize them as described in the following sections:

  - [Viewing the metrics dashboard](/docs/compare-and-comply?topic-compare-and-comply-metrics#view-metrics)
  - [Viewing the logging dashboards](/docs/compare-and-comply?topic-compare-and-comply-logging#view-logging)
  - [Adding alert notifications](/docs/compare-and-comply?topic=compare-and-comply-alerts#add-alert-notifications)
