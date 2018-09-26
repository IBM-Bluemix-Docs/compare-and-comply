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

# 安装、配置和使用监视仪表板
{: #monitor}

您可以使用 IBM Cloud Private 的监视仪表板来监视 {{site.data.keyword.cnc_short}} 的状态。监视仪表板使用 Grafana、Kibana 和 Prometheus 来显示有关 {{site.data.keyword.cnc_short}} 实例的详细可定制信息。

有关监视 {{site.data.keyword.BluOpenStackDed}} 集群的信息，请参阅 [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window}。

在使用仪表板之前，必须按照以下步骤中的说明生成并安装仪表板。

## 步骤 1：下载、解压缩和呈现仪表板模板
{: #render}

要准备仪表板模板来进行安装，请执行以下步骤。

1. 从 Passport Advantage (PPA) 下载 {{site.data.keyword.cnc_short}} 映像。该文件是压缩的 tar 文件，文件名类似于 `ibm-watson-compare-comply-prod-1.0.5.tar.gz`。该文件包含仪表板模板以及用于基于模板呈现仪表板的 `bash` 脚本。

1. 解压缩 tar 文件：
  ```bash
  mkdir ibm-watson-compare-comply-prod-1.0.5 && tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tar.gz -C ibm-watson-compare-comply-prod-1.0.5
  ``` 
  {: pre}

1. 切换到解压缩目录中的 `charts` 目录：
  ```bash
  cd ibm-watson-compare-comply-prod-1.0.5/charts
  ```
  {: pre}

1. 解压缩 `charts` 目录中的压缩 tar 文件：
  ```bash
  tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tgz
  ```
  {: pre}

1. 切换到 `dashboard` 目录。此目录包含警报、日志记录和度量值的模板，以及用于基于模板生成仪表板的 `bash` 脚本。
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

1. 运行 `render-dashboards.sh` 脚本以呈现模板。用于脚本的选项包括：
  
    - `-v`, `--version {chart_version}`：图表版本；例如，`1.0.5`。
    - `-h`, `--help`：打印命令帮助并退出。
    - `-r`, `--release {release_name}`：Helm 发行版名称。
    - `-n`, `--namespace {namespace}`：部署的名称空间。缺省名称空间为 `default`。

  ```bash
  ./render-dashboards.sh -v 1.0.5 -r my-test-release -n default
  ```
  {: pre}

所生成的仪表板 JSON 文件位于 `ibm-watson-compare-comply-prod-1.0.5/charts/ibm-watson-compare-comply-prod/dashboard` 之下。

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

## 步骤 2：导入模板
{: #import-tpls}

呈现仪表板后，请按照以下各部分中的说明导入一个或多个仪表板：

  - [导入度量值仪表板](metrics.html#import)
  - [导入日志记录仪表板](logging.html#import)
  - [导入警报仪表板](alerts.html#import)

## 步骤 3：查看和定制仪表板
{: #use-dboards}

导入所选仪表板后，请按照以下各部分中的说明查看和定制仪表板：

  - [查看度量值仪表板](metrics.html#view)
  - [查看日志记录仪表板](logging.html#view)
  - [添加警报规则](alerts.html#add)
