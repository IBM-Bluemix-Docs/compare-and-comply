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

# メトリックの使用
{: #metrics}

IBM Cloud Private のモニタリング・ダッシュボードを使用して、{{site.data.keyword.cnc_short}} の状況をモニターできます。 モニタリング・ダッシュボードは、Grafana (メトリック用)、Prometheus (アラート用)、Kibana (ロギング用) を使用して {{site.data.keyword.cnc_short}} インスタンスに関する詳細情報を表示します。

## メトリック・ダッシュボードのインポート

{{site.data.keyword.cnc_short}} のメトリック・ダッシュボードを IBM Cloud Private にインポートするには、以下のステップを実行します。

  1. [ステップ 1: ダッシュボード・テンプレートのダウンロード、抽出、レンダリング](/docs/services/compare-and-comply/monitor.html#monitor)の説明に従ってメトリック・ダッシュボードを抽出し生成してあることを確認します。

  1. IBM Cloud Private クラスターにログインします。

  1. 左上隅のメニュー・アイコンから、**「プラットフォーム」->「モニタリング」**を選択します。 <br />
      ![IBM Cloud Private のメニュー・アイコン](images/icp-menu.png) <br />
      ![「プラットフォーム」->「モニタリング」メニュー](images/icp-monitoring.png)

  1. Grafana インターフェースの左上付近の**「Home」**をクリックします。 <br />
      ![「Home」アイコン](images/icp-home.png)

  1. **「Import Dashboard」**をクリックします。
      ![「Import Dashboard」アイコン](images/import-dboard.png)

  1. 前の手順のステップ 6 で生成した `metrics.json` ファイルを選択し、**「Upload .json File」**をクリックします。 <br />
      ![metrics.json ファイルのアップロード](images/metrics-json.png)

  1. データ・ソースとして**「Prometheus」**を選択してから、**「Import」**をクリックします。
       ![Prometheus の選択](images/prometheus.png)

## メトリック・ダッシュボードの表示
{: #view}

メトリック・ダッシュボードは以下のようなものです。
![メトリック・ダッシュボード](images/metrics-dboard.png)

時刻範囲と自動最新表示の頻度は簡単に変更できます。
![時刻範囲と最新表示頻度の変更](images/dboard-change.png)

## メトリック・ダッシュボードの編集

メトリック・ダッシュボードの編集や新規ダッシュボードの作成を行うには、以下のステップを実行します。

  1. 左上隅のメニュー・アイコンから、**「プラットフォーム」->「モニタリング」**を選択し、Grafana UI にアクセスします。

  1. Grafana インターフェースの左上付近の**「Home」**をクリックしてから、**「+ New Dashboard」**をクリックします。

  1. **「Graph」**や**「Table」**など、追加するパネルのタイプを選択します。

  1. パネル・タイトルをクリックしてから、**「Edit」**をクリックします。 デフォルトのパネル・タイトルは`「Panel title」`です。

  1. **「General」**タブを使用してパネルのタイトル、説明、大きさを設定します。 12 ユニットでブラウザー・ウィンドウの全幅になります。

  1. **「Metrics」**タブを使用して、Prometheus からのデータを表示する照会を作成します。

    1. 照会言語に詳しければ照会を直接記述できます。**「Metric lookup」**フィールドを使用して、現在 Prometheus に報告されているメトリックから選択することもできます。

    1. 照会結果が新しいダッシュボード・パネルにリアルタイムで表示されます。

    1. 複数の照会を 1 つのパネルに追加することができます。 例えば、読み取り操作と書き込み操作を同じグラフに表示したり、合計アクセス数と合計訪問者数を同じ表に表示したりできます。
        
