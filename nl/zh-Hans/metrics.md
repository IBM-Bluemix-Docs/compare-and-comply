---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-28"

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

# 使用度量值
{: #using-metrics}

您可以使用 IBM Cloud Private 的监视仪表板来监视 {{site.data.keyword.cnc_short}} 的状态。监视仪表板使用 Grafana、Prometheus 和 Kibana 来提供有关 {{site.data.keyword.cnc_short}} 实例的详细信息。

有关监视仪表板的更多信息，请参阅 [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window}。

## 安装并运行度量值仪表板

要安装 {{site.data.keyword.cnc_short}} 的度量值仪表板，请执行以下步骤。

 1. 下载 {{site.data.keyword.cnc_short}} 的 Passport Advantage (PPA) 文件。该文件是压缩的 tar 文件，名称类似于 `ibm-watson-compare-comply-prod-1.0.0.tar.gz`。该文件包含度量值仪表板模板以及用于基于该模板呈现仪表板的 `bash` 脚本。

 1. 解压缩 PPA 文件：
  ```bash
  $ mkdir ibm-watson-compare-comply-prod-1.0.0 && tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tar.gz -C ibm-watson-compare-comply-prod-1.0.0
  ```
  {: codeblock}

 1. 切换到解压缩目录中的 `charts` 目录：
   ```bash
   $ cd ibm-watson-compare-comply-prod-1.0.0/charts    
   ```

 1. 解压缩 `charts` 目录中的压缩 tar 文件：
   ```bash
   $ tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tgz
   ```

 1. 切换到 `dashboard` 目录。此目录包括度量值和日志记录的模板，以及用于基于模板生成仪表板的 bash 脚本。

   ```bash
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
  
    -  `-v, --version {chart_version}`：图表版本；例如，`1.0.0`。
    -  `-h, --help`：打印命令帮助并退出。
    -  `-r, --release {release_name}`：Helm 发行版名称。
    -  `-n, --namespace {namespace}`：部署的名称空间。缺省名称空间为 `default`。

   ```bash
   $ ./render-dashboards.sh -v 1.0.0 -r my-test-release -n default
   The dashboard JSON files are generated under /Users/{user}/Downloads/ibm-watson-compare-comply-prod-1.0.0/charts/ibm-watson-compare-comply-prod/dashboard.

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

## 导入度量值仪表板

要将 {{site.data.keyword.cnc_short}} 的度量值仪表板导入到 IBM Cloud Private 中，请执行以下步骤。

  1. 登录到 IBM Cloud Private 集群。

  1. 从左上角的“菜单”图标中，选择**平台 -> 监视**。<br />
      ![IBM Cloud Private“菜单”图标](images/icp-menu.png) <br />
      ![平台 ->“监视”菜单](images/icp-monitoring.png)

  1. 单击 Grafana 界面左上方附近的**主页**。<br />
      ![“主页”图标](images/icp-home.png)

  1. 单击**导入仪表板**。
      ![“导入仪表板”图标](images/import-dboard.png)

  1. 选择先前过程的步骤 6 中生成的 `metrics.json` 文件，然后单击**上传 .json 文件**。<br />
      ![上传 metrics.json 文件](images/metrics-json.png)

  1. 选择 **Prometheus** 作为数据源，然后单击**导入**。
       ![选择 Prometheus](images/prometheus.png)

## 查看度量值仪表板

度量值仪表板类似于下图：
![度量值仪表板](images/metrics-dboard.png)

您可以轻松更改时间范围和自动刷新频率：
![更改时间范围和刷新率](images/dboard-change.png)

## 编辑度量值仪表板

可以通过执行以下步骤来编辑度量值仪表板或创建新的仪表板。

  1. 从左上角的“菜单”图标中，选择**平台 -> 监视**以访问 Grafana UI。

  1. 单击 Grafana 界面左上方附近的**主页**，然后单击 **+ 新建仪表板**。

  1. 选择要添加的面板的类型，例如**图形**或**表**。

  1. 单击面板标题，然后单击**编辑**。缺省面板标题为 `Panel title`。

  1. 使用**常规**选项卡来设置面板的标题、描述和维度。请注意，12 个单元即等于浏览器窗口全宽。

  1. 使用**度量值**选项卡来创建用于显示 Prometheus 中数据的查询。

        1. 如果您熟悉查询语言，那么可以直接编写查询，或者可以使用**度量值查找**字段从当前向 Prometheus 报告的度量值中进行选择。

        1. 查询的结果会实时显示在新的仪表板面板中。

        1. 可以将多个查询添加到一个面板。例如，可以在同一个图形上显示读写操作，或者在同一个表中显示访问总数和访问者总数。
        
