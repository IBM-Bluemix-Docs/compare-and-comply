---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-25"

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

# 使用监视警报
{: #alerts}

在导入警报仪表板后，您可以为 {{site.data.keyword.cnc_short}} 实例设置 Prometheus 警报，如以下各部分所述。

## 导入警报仪表板和添加警报规则
{: #import}

要导入警报仪表板并将警报规则添加到仪表板中，请执行以下步骤。

  1. 确保已抽取并生成了警报仪表板，如[步骤 1：下载、解压缩和呈现仪表板模板](/docs/services/compare-and-comply/monitor.html#monitor)中所述。

  1. 登录到 ICP 集群。

  1. 从左上角的“菜单”图标中，选择**配置 -> 配置映射**。
      ![IBM Cloud Private“菜单”图标](images/icp-menu.png)<br />
      ![配置 >“配置映射”菜单](images/configmaps.png)

  1. 这将打开**配置映射**页面，以显示配置映射表。在该表中，找到标记为 `alert-rules` 的行。在 `alert-rules` 行的**操作**列中，单击“菜单”图标，并选择**编辑**。
     ![alert-rules 编辑](images/configmaps-page.png)

  1. 在文本编辑器中打开 `.../ibm-watson-compare-comply-prod-1.0.0/charts/ibm-watson-compare-comply-prod/dashboard/alerts.json` 文件，然后复制以 `cnc.rules` 开头的行。

  1. 这将打开**编辑配置映射**窗口。在 `data` 对象中，在该对象的最后一行末尾添加一个逗号，然后粘贴上一步中复制的 `cnc.rules` 行。<br />
     ![编辑配置映射](images/edit-configmap.png)

  1. 在**编辑配置映射**窗口中，单击**提交**。

## 查看警报规则

要查看警报规则列表，请执行以下步骤。

  1. 浏览到 IBM Cloud Private 集群上的 Prometheus 仪表板。Prometheus 仪表板位于 `https://{ICP_cluster_IP_address}:{ICP_cluster_port}/prometheus`。

  1. 单击**警报**选项卡。Prometheus 仪表板将显示所有警报规则以及每个警报规则的活动警报数的列表。<br />
    ![Prometheus 警报](images/prometheus-dboard.png)

## 添加警报通知

可以为许多页面调度系统（包括 Slack、PagerDuty、HipChat、电子邮件等）添加警报通知。Prometheus 提供对通知的支持，如以下站点所述：

 - [Prometheus Alerting Configuration 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://prometheus.io/docs/alerting/configuration/){: new_window}
 - [Prometheus Notification Examples 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://prometheus.io/docs/alerting/notification_examples/){: new_window}

要为 IBM Cloud Private 上的 {{site.data.keyword.cnc_short}} 创建通知接收方，请执行以下步骤。
{: #create-notification-receiver}

  1. 登录到 ICP 集群。

  1. 从左上角的“菜单”图标中，选择**配置 -> 配置映射**。<br />
      ![IBM Cloud Private“菜单”图标](images/icp-menu.png) <br />
      ![配置 >“配置映射”菜单](images/configmaps.png)

  1. 这将打开**配置映射**页面，以显示配置映射表。在该表中，找到标记为 `monitoring-prometheus-alertmanager` 的行。在 `monitoring-prometheus-alertmanager` 行的**操作**列中，单击“菜单”图标，并选择**编辑**。

  1. 这将打开**编辑配置映射**窗口。在 `data` 对象中，输入新的接收方配置。
     ![编辑配置映射](images/prom-alert-edit.png)

  1. 在**编辑配置映射**窗口中，单击**提交**。

### 示例

要创建 Slack 通知，请执行以下步骤。

  1. 验证目标 Slack 通道是否存在。如果不存在，请进行创建。有关详细信息，请参阅 [Slack 文档中有关创建通道的内容 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://get.slack.help/hc/en-us/articles/201402297-Create-a-channel){: new_window}。

  1. 获取或创建 Slack 通道的 WebHook。有关详细信息，请参阅 [Slack 文档中有关 WebHook 的内容 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://get.slack.help/hc/en-us/articles/115005265063-Incoming-WebHooks-for-Slack){: new_window}。

  1. 在配置映射编辑器中打开 `monitoring-prometheus-alertmanager` 配置映射，如[添加警报通知](#create-notification-receiver)中所述。

  1. 如下所示更新该配置映射中的 `data` 对象：
    ```
    "data": {
      "alertmanager.yml": "global: \n  slack_api_url: '{WebHook_URL_for_Slack_channel}' \nreceivers: \n  - name: default-receiver \n    slack_configs: \n    - channel: '#{Slack_channel}' \n      send_resolved: true \nroute: \n  receiver: default-receiver \n  routes: \n  - match: \n    severity: critical \n   receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. 在**编辑配置映射**窗口中，单击**提交**。

要创建 PagerDuty 通知，请执行以下步骤。

  1. 验证 PagerDuty 服务是否存在。如果不存在，请进行创建。有关详细信息，请参阅 [PagerDuty 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://v2.developer.pagerduty.com/docs){: new_window}。

  1. 通过添加 Prometheus 集成来获取 PagerDuty 集成密钥。有关详细信息，请参阅 [PagerDuty API 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://v2.developer.pagerduty.com/docs/events-api){: new_window}。

  1. 在配置映射编辑器中打开 `monitoring-prometheus-alertmanager` 配置映射，如[添加警报通知](#create-notification-receiver)中所述。

  1. 如下所示更新该配置映射中的 `data` 对象：
    ```
    "data": {
      "alertmanager.yml": "global:\nreceivers:\n  - name: default-receiver\n    pagerduty_configs:\n    - service_key: ' {PagerDuty_integration_key}'\nroute:\n  receiver: default-receiver\n  routes:\n  - match:\n      severity: critical\n    receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. 在**编辑配置映射**窗口中，单击**提交**。
  
