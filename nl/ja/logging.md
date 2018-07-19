---

copyright:
years: 2017, 2018
lastupdated: "2018-03-23"

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

# ロギングの使用
{: #using-logging}

## ロギング・ダッシュボードのインストールと実行

{{site.data.keyword.cnc_short}} のロギング・ダッシュボードをインストールするには、以下のステップを実行します。

  1. {{site.data.keyword.cnc_short}} のパスポート・アドバンテージ (PPA) ファイルをダウンロードします。このファイルは、`ibm-watson-compare-comply-prod-1.0.0.tar.gz` のような名前の zip 圧縮された tar ファイルです。このファイルには、ロギング・ダッシュボード・テンプレートと、このテンプレートからダッシュボードをレンダリングするための `bash` スクリプトが含まれています。

  1. PPA ファイルを圧縮解除して展開します。
    ```bash
    $ mkdir ibm-watson-compare-comply-prod-1.0.0 && tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tar.gz -C ibm-watson-compare-comply-prod-1.0.0
    ```
    {: codeblock}

  1. 解凍されたディレクトリー内で `charts` ディレクトリーに移動します。
    ```bash
    $ cd ibm-watson-compare-comply-prod-1.0.0/charts
    ```

  1. `charts` ディレクトリー内の zip 圧縮された tar ファイルを圧縮解除して展開します。
    ```bash
    $ tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tgz
    ```

  1. `dashboard` ディレクトリーに移動します。そこには、メトリックとロギングに関するテンプレートと、そのテンプレートからダッシュボードを生成するための bash スクリプトが含まれています。


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

  1. `render-dashboards.sh` スクリプトを実行して、テンプレートをレンダリングします。スクリプトのオプションは以下のとおりです。
  
    -  `-v, --version {chart_version}`: 図表のバージョン。`1.0.0` など。
    -  `-h, --help`: コマンド・ヘルプを出力して終了します。
    -  `-r, --release {release_name}`: Helm のリリース名。
    -  `-n, --namespace {namespace}`: デプロイメントの名前空間。デフォルトの名前空間は `default` です。

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

## ロギング・ダッシュボードのインポート

{{site.data.keyword.cnc_short}} のロギング・ダッシュボードを IBM Cloud Private にインポートするには、以下のステップを実行します。

  1. IBM Cloud Private クラスターにログインします。

  1. 左上隅のメニュー・アイコンから、**「プラットフォーム」->「ロギング」**を選択します。<br />
    ![IBM Cloud Private のメニュー・アイコン](images/icp-menu.png) <br />
    ![「プラットフォーム」->「ロギング」メニュー](images/icp-logging.png)

  1. Kibana インターフェースの左側の**「Management」**をクリックします。<br />
    ![Kibana インターフェース](images/kibana.png)

  1. **「Saved Objects」**タブを選択します。
    ![「Saved Objects」タブ](images/saved-obj.png)

  1. **「Searches」**タブを選択し、**「Import」**をクリックします。
    ![「Searches」タブから「Import」](images/searches-import.png)

  1. 前の手順のステップ 6 で生成した `frontend-logging.json` ファイルと `external-process-logging.json` ファイルを個別にインポートします。 プロンプトが出されたら、**「Yes, overwrite all」**をクリックします。
     ![「Yes, overwrite all」プロンプト](images/overwrite-all.png)

  1. **「Searches」**タブにダッシュボードが表示されます。
     ![「Searches」タブのダッシュボード](images/searches-tab.png)

## ロギング・ダッシュボードの表示

ロギング・ダッシュボードを表示するには、以下のステップを実行します。

  1. **「Discover」**タブにナビゲートします。

  1. Kibana インターフェースの右上付近の**「Open」**をクリックします。

  1. 表示するダッシュボードを選択します。保守ログ用と外部プロセス・ログ用の 2 つのロギング・ダッシュボードがあります。
    ![ロギング・ダッシュボードの表示](images/kibana-dboards.png)

時刻範囲と自動最新表示の頻度は簡単に変更できます。
  ![時刻範囲と最新表示頻度の変更](images/log-dboard-change.png)

