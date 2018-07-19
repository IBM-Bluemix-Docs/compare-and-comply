---

copyright:
years: 2018
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

# サービスのデプロイ
{: #install}

Helm chart の `README.md` ファイルの説明に従って {{site.data.keyword.cnc_long}} Helm chart をインストールする前に、追加のデプロイメント・ステップを実行しなければならない場合があります。

## デフォルトのシステム値
{: #sys_vals}

この説明では、以下のデフォルト・システム値を使用します。ご使用のシステムでの値は異なる場合があります。IBM Cloud Private 管理者に確認してください。

 - IBM Cloud Private のクラスター名とポート番号:
 	`mycluster.icp:8500`
 	
 - IBM Cloud Private の名前空間:
 	`default`

## デプロイメントの実行
{: #deploy}

{{site.data.keyword.cnc_short}} デプロイメントを行うには、以下のステップを実行します。

### ステップ 1: Docker へのアクセス権限の付与
{: #step1}

  1. Docker へのアクセス権限を IBM Cloud Private レジストリーに付与します。
  
  	Microsoft Windows、あるいは Apple macOS または OS X で実行する場合は、以下のステップを実行します。

 	 1. **「Docker」**アイコンをクリックします。
 	 1. Docker の**「Preferences」**メニューを開きます。
 	 1. **「Daemon」**タブをクリックします。
 	 1. **「Insecure registries」**フィールドで、値 `mycluster.icp:8500`、または IBM Cloud Private インストール済み環境を表す値を追加します。
 	 
 	 ![Docker の構成](images/docker.png)
 	 
 	Linux で実行する場合は、以下のステップを実行します。
 	
 	1. `daemon.json` ファイルを見つけます。デフォルトでは、このファイルは `/etc/docker/daemon.json` にあります。ファイルが存在しない場合は、それをデフォルトの場所に作成してください。テキスト・エディターでファイルを開き、次の JSON を確認するか追加します。
 	
 	  ```json
 	  {
 	    "insecure-registries": ["mycluster.icp:8500"]
 	  }
 	  ```
 	  
 	1. 以下のコマンドを実行します。
 	  ```bash
 	  systemctl daemon reload
 	  ```
 	  {: pre}
 	  
 	  ```bash
 	  systemctl restart docker
 	  ``` 
 	  {: pre}

### ステップ 2: IBM Cloud Private 用 CLI と IBM Cloud CLI のダウンロード
{: #step2}

  1. ご使用の IBM Cloud Private のバージョンごとに、Helm CLI をダウンロードしてインストールします。詳しくは、[IBM Cloud Private の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/icp_cli.html){: new_window} を参照してください。
  
  1. IBM Cloud CLI をダウンロードしてインストールします。詳しくは、[IBM Cloud の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html#download_install){: new_window} を参照してください。
  
  1. IBM Cloud CLI `bx` プラグインをダウンロードしてインストールします。詳しくは、[IBM Cloud の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/extend_cli.html#plug-ins){: new_window} を参照してください。
 	
### ステップ 3: パッケージ・ファイルのダウンロード
{: #step3}
 	
[パスポート・アドバンテージ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-01.ibm.com/software/passportadvantage/){: new_window} で提供される `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` パッケージ・ファイルをローカル・ワークステーションにダウンロードします。このファイルには、{{site.data.keyword.cnc_short}} サービス・イメージ全体が含まれています。
  
#### パッケージ・ファイルの編集 (必要な場合)
{: edit-pkg-file}

**重要**: このステップの手順は、以下の条件が当てはまる場合*のみ* 実行してください。どちらか一方または両方の条件が当てはまらない場合は、[ステップ 4](#step4) に進んでください。

 - このステップは、{{site.data.keyword.cnc_short}} バージョン 1.0.3 以前にのみ適用されます。{{site.data.keyword.cnc_short}} バージョン 1.0.4 以降をインストールする場合は、このステップの手順を実行しないでください。

 - IBM Cloud Private クラスターが外部インターネットに接続されていない場合のみ、このステップの手順に従う必要があります。IBM Cloud Private クラスターが[パスポート・アドバンテージ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-01.ibm.com/software/passportadvantage/){: new_window} のサイトにネットワーク・アクセスできる場合は、{{site.data.keyword.cnc_short}} バージョン 1.0.3 をインストールして、パッケージ・ファイルに変更を加えることなくデプロイできます。

**注**: このステップの手順を実行すると、{{site.data.keyword.cnc_short}} バージョン 1.0.3 のアップグレードとロールバックで問題が発生する可能性があります。 
 
  1. `bash` シェルまたはそれに類似した Cygwin などの環境で、`IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` ファイルを解凍します。
  
 	```bash
 	cd {path_to_file}
 	```
 	{: pre}
 	
 	```bash
 	tar -xvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz
 	```
 	{: pre}
 	
 	解凍されたファイルには `charts` という名前のディレクトリーが含まれており、そこに `ibm-watson-compare-comply-prod-1.0.3.tgz` という名前のファイルが格納されています。このファイルは、{{site.data.keyword.cnc_short}} 用 Helm chart の圧縮版です。
 	
  1. `charts` ディレクトリーに移動して、`ibm-watson-compare-comply-prod-1.0.3.tgz` ファイルを解凍します。
  
 	```bash
 	cd charts
 	```
 	{: pre}
 	
 	```
 	tar -xvzf ibm-watson-compare-comply-prod-1.0.3.tgz
 	```
 	{: pre}
 	
 	解凍されたファイルには、いくつかのディレクトリーとファイルが含まれています。その中には `ibm-watson-compare-comply-prod` という名前の最上位ディレクトリーがあり、そこには `templates` という名前のディレクトリーが入っています。
 	
  1. `ibm-watson-compare-comply-prod/templates` ディレクトリーに移動します。
  
 	```bash
 	cd ibm-watson-compare-comply-prod/templates
 	```
 	{: pre}

  1. `ibm-watson-compare-comply-prod/templates` ディレクトリーで `deployment.yaml` ファイルを見つけて、テキスト・エディターで開きます。
 
  1. `deployment.yaml` ファイルを次のように編集します。
  
 	**オリジナル**
 	```
 	- name: {{ .Chart.Name }}
 		image: "hyc-icpcontent-docker-local.artifactory.swg-devops.com/cncdev/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	
 	**更新後**
 	```
 	- name: {{ .Chart.Name }}
 		image: "mycluster.icp:8500/default/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	`deployment.yaml` ファイルを保存して閉じます。
 	
  1. `charts` ディレクトリーに戻り、`ibm-watson-compare-comply-prod-1.0.3.tgz` ファイルを再パッケージします。
  
 	```bash
 	cd ../..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf ibm-watson-compare-comply-prod-1.0.3.tgz ibm-watson-compare-comply-prod
 	```
 	{: pre}
 	
  1. 最上位ディレクトリーに戻り、`IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` ファイルを再パッケージします。
  
 	```bash
 	cd ..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz charts images manifest.json manifest.yaml
 	```
 	{: pre}
 	
 	 参考までに、圧縮解除された `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` ファイルの内容の `tree` ビューは、次のようになります。この手順に関連するファイルは、右側の列に `<-----` で示しています。
 	 
 	 ```bash
 	 cd {path_to_archive_file}
 	 ```
 	 {: pre}
 	 
 	 ```bash
 	 tree
 	 
	 .
	 ├── IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz          <-----
     ├── charts                                            <-----
     │   ├── ibm-watson-compare-comply-prod
     │   │   ├── Chart.yaml
     │   │   ├── LICENSE
     │   │   ├── LICENSES
     │   │   │   └── LICENSE-Product
     │   │   ├── README.md
     │   │   ├── cnc-banner.png
     │   │   ├── dashboard
     │   │   │   ├── alerts.json.tpl
     │   │   │   ├── external-process-logging.json.tpl
     │   │   │   ├── frontend-logging.json.tpl
     │   │   │   ├── metrics.json.tpl
     │   │   │   └── render-dashboards.sh
     │   │   ├── templates                                 <-----
     │   │   │   ├── NOTES.txt
     │   │   │   ├── _helpers.tpl
     │   │   │   ├── _sch-chart-config.tpl
     │   │   │   ├── configmap.yaml
     │   │   │   ├── deployment.yaml                       <-----
     │   │   │   ├── ingress.yaml
     │   │   │   ├── metrics-service.yaml
     │   │   │   ├── sch-2.6.0
     │   │   │   │   ├── _config.tpl
     │   │   │   │   ├── _metadata.tpl
     │   │   │   │   ├── _names.tpl
     │   │   │   │   ├── _utils.tpl
     │   │   │   │   └── _version.tpl
     │   │   │   ├── service.yaml
     │   │   │   └── tests
     │   │   │       └── service-test.yaml
     │   │   ├── values-metadata.yaml
     │   │   └── values.yaml
     │   └── ibm-watson-compare-comply-prod-1.0.3.tgz      <-----
     ├── images
     │   ├── 304735eab4e4df55b236f9835104cb1058bc50cb5ff7f4ee7d719cb56d2a718b.tar.gz
     │   ├── af80995a4fcfcee364f4f72ba043bf9e73dcafe7fcac384e1cd06bf1c7a52c1f.tar.gz
     │   └── ccec10461710cd625cbbacc0f5d5d837a0cc5baf132e0fc39ed42d8264d25062.tar.gz
     ├── manifest.json
     └── manifest.yaml 
 	 ```
 	 {: pre}
   
### ステップ 4: 端末の準備
{: #step4}
 	
`bash` シェルまたは同等の環境で次のコマンドを実行して、IBM Cloud Private インストール済み環境で使用できるように準備しておきます。
  
  ```bash
  bx pr login -a https://mycluster.icp:8443 --skip-ssl-validation
  ```
  {: pre}
  
  ```bash
  bx pr cluster-config mycluster
  ```
  {: pre}
  
  ```bash
  docker login mycluster.icp:8500
  ```
  {: pre}

  **注**: 上記リストの最初のコマンドで、URL `https://mycluster.icp:8443` の中のポート番号 `8443` は、IBM Cloud Private 上の {{site.data.keyword.cnc_short}} サービスと通信するためのデフォルト値を表します。

### ステップ 5: イメージ・レジストリー・シークレットとパッチ・サービス・アカウントの作成
{: #step5}

次のコマンドを実行して、IBM Cloud Private の内部レジストリーにアクセスするために必要なすべてのコンポーネントを使用可能にします。

```bash
kubectl create secret docker-registry $secret_name --docker-server=mycluster.icp:8500 --docker-username=$username --docker-password=$password --docker-email=admin@admin.com --namespace=default
```
{: pre}

```bash
kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "$secret_name"}]}' --namespace=default
```
{: pre}

各部の意味は、次のとおりです。
  - `$username` と `$password` は、クラスターによって異なります。管理者に問い合わせてください。
  - `$secret_name` は、[Docker コンテナーが安全に通信できるようにする ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.docker.com/engine/swarm/secrets/){: new_window} ために指定するストリングです。

### ステップ 6: IBM Cloud Private 内部イメージ・レジストリーへのパッケージ・ファイルのアップロード
{: #step6}

[ステップ 4](#step4) で準備した端末で、次のコマンドを実行してパスポート・アドバンテージのパッケージ・ファイルをアップロードします。

```bash
bx pr load-ppa-archive --archive {path_to}/IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz --clustername mycluster.icp --namespace default 
```
{: pre}

### ステップ 7: インストールの完了

IBM Cloud Private クラスターの**「カタログ」**ページに戻り、`ibm-watson-compare-comply-prod` 項目を見つけて選択し、**「構成」**をクリックしてインストールを進めます。
