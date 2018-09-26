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

# 使用監視警示
{: #alerts}

匯入警示儀表板之後，您可以為 {{site.data.keyword.cnc_short}} 實例設定 Prometheus 警示，如下節所述。

## 匯入警示儀表板及新增警示規則
{: #import}

若要匯入警示儀表板並將警示規則新增至儀表板，請執行下列步驟。

  1. 確定您已擷取並產生警示儀表板，如[步驟 1：下載、擷取及呈現儀表板範本](/docs/services/compare-and-comply/monitor.html#monitor)中所述。

  1. 登入您的 ICP 叢集。

  1. 從左上角的「功能表」圖示中，選取**配置 -> ConfigMap**。
      ![「IBM Cloud Private 功能表」圖示](images/icp-menu.png) <br />
      ![「配置 -> ConfigMap」功能表](images/configmaps.png)

  1. **ConfigMap** 頁面即會開啟，以顯示 ConfigMap 的表格。在表格中，找出標示 `alert-rules` 的列。在 `alert-rules` 列的**動作**直欄中，按一下功能表圖示，並選取**編輯**。
     ![alert-rules 編輯](images/configmaps-page.png)

  1. 在文字編輯器中開啟 `.../ibm-watson-compare-comply-prod-1.0.0/charts/ibm-watson-compare-comply-prod/dashboard/alerts.json` 檔，並複製開頭為 `cnc.rules` 的行。

  1. **編輯 ConfigMap** 視窗即會開啟。在 `data` 物件中，於物件的最後一行尾端新增一個逗點，然後貼入您在前一個步驟中複製的 `cnc.rules` 行。<br />
     ![編輯 ConfigMap](images/edit-configmap.png)

  1. 按一下**編輯 ConfigMap** 視窗中的**提交**。

## 檢視警示規則

若要檢視警示規則的清單，請執行下列步驟。

  1. 在 IBM Cloud Private 叢集上導覽至 Prometheus 儀表板。Prometheus 儀表板位於 `https://{ICP_cluster_IP_address}:{ICP_cluster_port}/prometheus`。

  1. 按一下**警示**標籤。Prometheus 儀表板會顯示所有警示規則的清單，以及每一個警示規則的作用中警示數目。<br />
    ![Prometheus 警示](images/prometheus-dboard.png)

## 新增警示通知

您可以針對眾多分頁系統新增警示通知，包括 Slack、PagerDuty、HipChat、電子郵件，以及其他。Prometheus 提供下列網站所記載之通知的支援：

 - [Prometheus 警示配置文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://prometheus.io/docs/alerting/configuration/){: new_window}
 - [Prometheus 通知範例文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://prometheus.io/docs/alerting/notification_examples/){: new_window}

若要在 IBM Cloud Private 上建立 {{site.data.keyword.cnc_short}} 的通知接收端，請執行下列步驟。
{: #create-notification-receiver}

  1. 登入您的 ICP 叢集。

  1. 從左上角的「功能表」圖示中，選取**配置 -> ConfigMap**。<br />
      ![「IBM Cloud Private 功能表」圖示](images/icp-menu.png) <br />
      ![「配置 -> ConfigMap」功能表](images/configmaps.png)

  1. **ConfigMap** 頁面即會開啟，以顯示 ConfigMap 的表格。在此表格中，找出標示 `monitoring-prometheus-alertmanager` 的列。在 `monitoring-prometheus-alertmanager` 列的**動作**直欄中，按一下功能表圖示，並選取**編輯**。

  1. **編輯 ConfigMap** 視窗即會開啟。在 `data` 物件中，輸入新的接收端配置。
     ![編輯 ConfigMap](images/prom-alert-edit.png)

  1. 按一下**編輯 ConfigMap** 視窗中的**提交**。

### 範例

若要建立 Slack 通知，請執行下列步驟。

  1. 驗證目標 Slack 頻道存在。如果不存在，請建立一個。如需詳細資料，請參閱[說明如何建立頻道的 Slack 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://get.slack.help/hc/en-us/articles/201402297-Create-a-channel){: new_window}。

  1. 取得或建立 Slack 頻道的 WebHook。如需詳細資料，請參閱 [WebHook 的 Slack 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://get.slack.help/hc/en-us/articles/115005265063-Incoming-WebHooks-for-Slack){: new_window}。

  1. 如[新增警示通知](#create-notification-receiver)所述，在 ConfigMap 編輯器中開啟 `monitoring-prometheus-alertmanager` ConfigMap。

  1. 更新 ConfigMap 中的 `data` 物件，如下所示：
    ```
    "data": {
      "alertmanager.yml": "global: \n  slack_api_url: '{WebHook_URL_for_Slack_channel}' \nreceivers: \n  - name: default-receiver \n    slack_configs: \n    - channel: '#{Slack_channel}' \n      send_resolved: true \nroute: \n  receiver: default-receiver \n  routes: \n  - match: \n    severity: critical \n   receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. 在**編輯 ConfigMap** 視窗中，按一下**提交**。

若要建立 PagerDuty 通知，請執行下列步驟。

  1. 驗證 PagerDuty 服務存在。如果不存在，請建立一個。如需詳細資料，請參閱 [PagerDuty 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://v2.developer.pagerduty.com/docs){: new_window}。

  1. 新增 Prometheus 整合，以取得 PagerDuty 整合金鑰。如需詳細資料，請參閱 [PagerDuty API 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://v2.developer.pagerduty.com/docs/events-api){: new_window}。

  1. 如[新增警示通知](#create-notification-receiver)所述，在 ConfigMap 編輯器中開啟 `monitoring-prometheus-alertmanager` ConfigMap。

  1. 更新 ConfigMap 中的 `data` 物件，如下所示：
    ```
    "data": {
      "alertmanager.yml": "global:\nreceivers:\n  - name: default-receiver\n    pagerduty_configs:\n    - service_key: ' {PagerDuty_integration_key}'\nroute:\n  receiver: default-receiver\n  routes:\n  - match:\n      severity: critical\n    receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. 在**編輯 ConfigMap** 視窗中，按一下**提交**。
  
