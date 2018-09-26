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

# モニタリング・ダッシュボードのインストール、構成、および使用
{: #monitor}

IBM Cloud Private のモニタリング・ダッシュボードを使用して、{{site.data.keyword.cnc_short}} の状況をモニターできます。モニタリング・ダッシュボードは、Grafana、Kibana、および Prometheus を使用して、{{site.data.keyword.cnc_short}} インスタンスに関するカスタマイズ可能な詳細情報を表示します。

{{site.data.keyword.BluOpenStackDed}} クラスターのモニタリングについて詳しくは、[https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window} を参照してください。

ダッシュボードを使用する前に、次のステップに従って、ダッシュボードを生成してインストールする必要があります。

## ステップ 1: ダッシュボード・テンプレートのダウンロード、抽出、レンダリング
{: #render}

以下のステップを実行して、インストール用のダッシュボード・テンプレートを準備します。

1. パスポート・アドバンテージ (PPA) から {{site.data.keyword.cnc_short}} イメージをダウンロードします。このファイルは、`ibm-watson-compare-comply-prod-1.0.5.tar.gz` のような名前の zip 圧縮された tar ファイルです。このファイルには、ダッシュボード・テンプレートと、それらのテンプレートからダッシュボードをレンダリングするための `bash` スクリプトが含まれています。

1. tar ファイルを解凍して展開します。
  ```bash
  mkdir ibm-watson-compare-comply-prod-1.0.5 && tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tar.gz -C ibm-watson-compare-comply-prod-1.0.5
  ``` 
  {: pre}

1. 解凍されたディレクトリー内で `charts` ディレクトリーに移動します。
  ```bash
  cd ibm-watson-compare-comply-prod-1.0.5/charts
  ```
  {: pre}

1. `charts` ディレクトリー内の zip 圧縮された tar ファイルを圧縮解除して展開します。
  ```bash
  tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tgz
  ```
  {: pre}

1. `dashboard` ディレクトリーに移動します。 そこには、アラート、ロギング、およびメトリックのテンプレートと、テンプレートからダッシュボードを生成するための `bash` スクリプトが含まれています。

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

1. `render-dashboards.sh` スクリプトを実行して、テンプレートをレンダリングします。 スクリプトのオプションは以下のとおりです。
  
    - `-v`、`--version {chart_version}`: 図表のバージョン。`1.0.5` など。
    - `-h`、`--help`: コマンド・ヘルプを出力して終了します。
    - `-r`、`--release {release_name}`: Helm のリリース名。
    - `-n`、`--namespace {namespace}`: デプロイメントの名前空間。デフォルトの名前空間は `default` です。

  ```bash
  ./render-dashboards.sh -v 1.0.5 -r my-test-release -n default
  ```
  {: pre}

ダッシュボード JSON ファイルは、`ibm-watson-compare-comply-prod-1.0.5/charts/ibm-watson-compare-comply-prod/dashboard` の下に生成されます。

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

## ステップ 2: テンプレートのインポート
{: #import-tpls}

ダッシュボードをレンダリングした後、以下のセクションの説明に従って 1 つ以上のダッシュボードをインポートします。

  - [メトリック・ダッシュボードのインポート](metrics.html#import)
  - [ロギング・ダッシュボードのインポート](logging.html#import)
  - [アラート・ダッシュボードのインポート](alerts.html#import)

## ステップ 3: ダッシュボードの表示とカスタマイズ
{: #use-dboards}

選択したダッシュボードをインポートした後、以下のセクションの説明に従ってダッシュボードを表示してカスタマイズします。

  - [メトリック・ダッシュボードの表示](metrics.html#view)
  - [ロギング・ダッシュボードの表示](logging.html#view)
  - [アラート・ルールの追加](alerts.html#add)
