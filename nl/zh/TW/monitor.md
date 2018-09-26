---

copyright:
years: 2017, 2018
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

# 安裝、配置及使用監視儀表板
{: #monitor}

您可以使用 IBM Cloud Private 的監視儀表板，來監視 {{site.data.keyword.cnc_short}} 的狀態。監視儀表板使用 Grafana、Kibana 及 Prometheus 來顯示詳細且可自訂的 {{site.data.keyword.cnc_short}} 實例相關資訊。

如需監視 {{site.data.keyword.BluOpenStackDed}} 叢集的相關資訊，請參閱 [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window}。

使用儀表板之前，您必須產生並安裝它們，如下列步驟中所述。

## 步驟 1：下載、擷取及呈現儀表板範本
{: #render}

請執行下列步驟來準備安裝用的儀表板範本。

1. 從 Passport Advantage (PPA) 下載 {{site.data.keyword.cnc_short}} 映像檔。此檔案是一個壓縮的 Tar 檔，其名稱類似於 `ibm-watson-compare-comply-prod-1.0.5.tar.gz`。此檔案包括儀表板範本，以及用來從範本呈現儀表板的 `bash` Script。

1. 解壓縮並展開 tar 檔案：
  ```bash
  mkdir ibm-watson-compare-comply-prod-1.0.5 && tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tar.gz -C ibm-watson-compare-comply-prod-1.0.5
  ``` 
  {: pre}

1. 切換至解壓縮目錄中的 `charts` 目錄：
  ```bash
  cd ibm-watson-compare-comply-prod-1.0.5/charts
  ```
  {: pre}

1. 在 `charts` 目錄中，解壓縮並展開壓縮的 Tar 檔：
  ```bash
  tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tgz
  ```
  {: pre}

1. 切換至 `dashboard` 目錄。它包括用於警示、記載及度量值的範本，以及用來從範本產生儀表板的 `bash` Script。
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

1. 執行 `render-dashboards.sh` Script 來呈現範本。Script 的選項包括：
  
    - `-v`, `--version {chart_version}`：圖表版本，例如 `1.0.5`。
    - `-h`, `--help`：列印指令說明並結束。
    - `-r`, `--release {release_name}`：Helm 版本名稱。
    - `-n`, `--namespace {namespace}`：部署的名稱空間。預設名稱空間為 `default`。

  ```bash
  ./render-dashboards.sh -v 1.0.5 -r my-test-release -n default
  ```
  {: pre}

儀表板 JSON 檔會產生在 `ibm-watson-compare-comply-prod-1.0.5/charts/ibm-watson-compare-comply-prod/dashboard` 下。

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

## 步驟 2：匯入範本
{: #import-tpls}

呈現儀表板之後，請匯入一個以上的儀表板，如下列各節所述：

  - [匯入度量儀表板](metrics.html#import)
  - [匯入記載儀表板](logging.html#import)
  - [匯入警示儀表板](alerts.html#import)

## 步驟 3：檢視及自訂儀表板
{: #use-dboards}

匯入所選取的儀表板之後，請檢視並自訂它們，如下列各節所述：

  - [檢視度量儀表板](metrics.html#view)
  - [檢視記載儀表板](logging.html#view)
  - [新增警示規則](alerts.html#add)
