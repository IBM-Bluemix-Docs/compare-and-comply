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

# モニタリング・アラートの使用
{: #alerts}

以下のセクションの説明に従ってアラート・ダッシュボードをインポートした後、{{site.data.keyword.cnc_short}} インスタンスのために Prometheus アラートをセットアップすることができます。

## アラート・ダッシュボードのインポートとアラート・ルールの追加
{: #import}

アラート・ダッシュボードをインポートし、アラート・ルールをダッシュボードに追加するには、以下のステップを実行します。

  1. [ステップ 1: ダッシュボード・テンプレートのダウンロード、抽出、レンダリング](/docs/services/compare-and-comply/monitor.html#monitor)の説明に従ってアラート・ダッシュボードを抽出し生成してあることを確認します。

  1. ICP クラスターにログインします。

  1. 左上隅のメニュー・アイコンから、**「構成」->「ConfigMaps」**を選択します。
      ![IBM Cloud Private のメニュー・アイコン](images/icp-menu.png) <br />
      ![「構成」->「ConfigMaps」メニュー](images/configmaps.png)

  1. **「ConfigMaps」**ページが開いて、ConfigMap のテーブルが表示されます。 テーブル内で、`alert-rules` というラベルのある行を見つけます。 `alert-rules` 行の**「アクション」**列で、メニュー・アイコンをクリックして**「編集」**を選択します。
     ![alert-rules の「編集」](images/configmaps-page.png)

  1. `.../ibm-watson-compare-comply-prod-1.0.0/charts/ibm-watson-compare-comply-prod/dashboard/alerts.json` ファイルをテキスト・エディターで開き、`cnc.rules` で始まる行をコピーします。

  1. **「ConfigMap の編集」**ウィンドウが開きます。 `data` オブジェクト内で、オブジェクトの最終行の末尾にコンマを追加してから、前のステップでコピーした `cnc.rules` 行を貼り付けます。 <br />
     ![ConfigMap の編集](images/edit-configmap.png)

  1. **「ConfigMap の編集」**ウィンドウで**「実行依頼」**をクリックします。

## アラート・ルールの表示

アラート・ルールのリストを表示するには、以下のステップを実行します。

  1. IBM Cloud Private クラスターの Prometheus ダッシュボードに移動します。 Prometheus ダッシュボードは `https://{ICP_cluster_IP_address}:{ICP_cluster_port}/prometheus` にあります。

  1. **「Alerts」**タブをクリックします。 Prometheus ダッシュボードには、すべてのアラート・ルールのリストと、それぞれのルールにおけるアクティブ・アラートの数が表示されます。 <br />
    ![Prometheus アラート](images/prometheus-dboard.png)

## アラート通知の追加

Slack、PagerDuty、HipChat、E メールなどのさまざまなページング・システム用にアラート通知を追加できます。 Prometheus には、以下のサイトで説明されているように、通知のためのサポートが用意されています。

 - [Prometheus Alerting Configuration の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://prometheus.io/docs/alerting/configuration/){: new_window}
 - [Prometheus Notification Examples の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://prometheus.io/docs/alerting/notification_examples/){: new_window}

IBM Cloud Private 上の {{site.data.keyword.cnc_short}} 用の通知レシーバーを作成するには、以下のステップを実行します。
{: #create-notification-receiver}

  1. ICP クラスターにログインします。

  1. 左上隅のメニュー・アイコンから、**「構成」->「ConfigMaps」**を選択します。 <br />
      ![IBM Cloud Private のメニュー・アイコン](images/icp-menu.png) <br />
      ![「構成」->「ConfigMaps」メニュー](images/configmaps.png)

  1. **「ConfigMaps」**ページが開いて、ConfigMap のテーブルが表示されます。 テーブル内で、`monitoring-prometheus-alertmanager` というラベルのある行を見つけます。 `monitoring-prometheus-alertmanager` 行の**「アクション」**列で、メニュー・アイコンをクリックして**「編集」**を選択します。

  1. **「ConfigMap の編集」**ウィンドウが開きます。 `data` オブジェクトで、新しいレシーバー構成を入力します。
     ![構成マップの編集](images/prom-alert-edit.png)

  1. **「ConfigMap の編集」**ウィンドウで**「実行依頼」**をクリックします。

### 例

Slack 通知を作成するには、以下のステップを実行します。

  1. ターゲットの Slack チャネルが存在することを確認します。 存在していない場合は、それを作成します。 詳しくは、[チャネルを作成するための Slack の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://get.slack.help/hc/en-us/articles/201402297-Create-a-channel){: new_window} を参照してください。

  1. Slack チャネルのための WebHook を取得または作成します。 詳しくは、[WebHook のための Slack の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://get.slack.help/hc/en-us/articles/115005265063-Incoming-WebHooks-for-Slack){: new_window} を参照してください。

  1. [アラート通知の追加](#create-notification-receiver)で説明されているように、ConfigMap エディターで `monitoring-prometheus-alertmanager` ConfigMap を開きます。

  1. 以下のようにして、ConfigMap 内の `data` オブジェクトを更新します。
    ```
    "data": {
      "alertmanager.yml": "global: \n  slack_api_url: '{WebHook_URL_for_Slack_channel}' \nreceivers: \n  - name: default-receiver \n    slack_configs: \n    - channel: '#{Slack_channel}' \n      send_resolved: true \nroute: \n  receiver: default-receiver \n  routes: \n  - match: \n    severity: critical \n   receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. **「ConfigMap の編集」**ウィンドウで、**「実行依頼」**をクリックします。

PagerDuty 通知を作成するには、以下のステップを実行します。

  1. PagerDuty サービスが存在することを確認します。 存在していない場合は、それを作成します。 詳しくは、[PagerDuty の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://v2.developer.pagerduty.com/docs){: new_window} を参照してください。

  1. Prometheus 統合を追加することによって PagerDuty 統合キーを取得します。 詳しくは、[PagerDuty API の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://v2.developer.pagerduty.com/docs/events-api){: new_window} を参照してください。

  1. [アラート通知の追加](#create-notification-receiver)で説明されているように、ConfigMap エディターで `monitoring-prometheus-alertmanager` ConfigMap を開きます。

  1. 以下のようにして、ConfigMap 内の `data` オブジェクトを更新します。
    ```
    "data": {
      "alertmanager.yml": "global:\nreceivers:\n  - name: default-receiver\n    pagerduty_configs:\n    - service_key: ' {PagerDuty_integration_key}'\nroute:\n  receiver: default-receiver\n  routes:\n  - match:\n      severity: critical\n    receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. **「ConfigMap の編集」**ウィンドウで、**「実行依頼」**をクリックします。
  
