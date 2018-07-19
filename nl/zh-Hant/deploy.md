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

# 部署服務
{: #install}

在如 Helm 圖表的 `README.md` 檔案所述安裝 {{site.data.keyword.cnc_long}} Helm 圖表之前，您可能需要執行其他部署步驟。

## 預設系統值
{: #sys_vals}

這些指示會使用下列預設系統值。您的系統值可能不同。請洽詢 IBM Cloud Private 管理者。

 - IBM Cloud Private 叢集名稱及埠號：
 	`mycluster.icp:8500`
 	
 - IBM Cloud Private 名稱空間：
 	`default`

## 完成部署
{: #deploy}

請執行下列步驟來完成 {{site.data.keyword.cnc_short}} 部署：

### 步驟 1：授與 Docker 存取權
{: #step1}

  1. 授與 Docker 對 IBM Cloud Private 登錄的存取權。
  
  	如果您執行的是 Microsoft Windows 或 Apple macOS 或 OS X，請執行下列步驟：

 	 1. 按一下 **Docker** 圖示。
 	 1. 開啟 Docker **喜好設定**功能表。
 	 1. 按一下**常駐程式**標籤。
 	 1. 在**不安全登錄**欄位中，新增 `mycluster.icp:8500` 值或 IBM Cloud Private 安裝的值。
 	 
 	 ![配置 Docker](images/docker.png)
 	 
 	如果您執行 Linux，請執行下列步驟：
 	
 	1. 找出 `daemon.json` 檔案。依預設，此檔案位於 `/etc/docker/daemon.json`。如果檔案不存在，請在預設位置建立它。在文字編輯器中開啟檔案，並驗證或新增下列 JSON。
 	
 	  ```json
 	  {
 	    "insecure-registries": ["mycluster.icp:8500"]
 	  }
 	  ```
 	  
 	1. 執行下列指令：
 	  ```bash
 	  systemctl daemon reload
 	  ```
 	  {: pre}
 	  
 	  ```bash
 	  systemctl restart docker
 	  ``` 
 	  {: pre}

### 步驟 2：下載 IBM Cloud Private 和 IBM Cloud CLI
{: #step2}

  1. 針對您所使用之 IBM Cloud Private 的每一個版本，下載並安裝 Helm CLI。如需詳細資料，請參閱 [IBM Cloud Private 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/icp_cli.html){: new_window}。
  
  1. 下載並安裝 IBM Cloud CLI。如需詳細資料，請參閱 [IBM Cloud 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html#download_install){: new_window}。
  
  1. 下載並安裝 IBM Cloud CLI `bx` 外掛程式。如需詳細資料，請參閱 [IBM Cloud 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/extend_cli.html#plug-ins){: new_window}。
 	
### 步驟 3：下載套件檔
{: #step3}
 	
將 `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` 套件檔從 [Passport Advantage ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www-01.ibm.com/software/passportadvantage/){: new_window} 下載至本端工作站。此檔案包含整個 {{site.data.keyword.cnc_short}} 服務映像檔。
  
#### 編輯套件檔（必要的話）
{: edit-pkg-file}

**重要事項**：*僅* 在下列條件為 true 時，才執行此步驟中的程序。如果任一個條件或兩個條件都為 false，請繼續進行[步驟 4](#step4)。

 - 此步驟僅適用於 {{site.data.keyword.cnc_short}} 1.0.3 版及更舊版本。如果您安裝 {{site.data.keyword.cnc_short}} 1.0.4 版或更新版本，請不要執行此步驟中的程序。

 - 只有在 IBM Cloud Private 叢集未連接至外部網際網路時，才需要遵循此步驟中的程序。如果 IBM Cloud Private 叢集可透過網路存取 [Passport Advantage ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www-01.ibm.com/software/passportadvantage/){: new_window} 網站，則您可以安裝並部署 {{site.data.keyword.cnc_short}} 1.0.3 版，而無需對套件檔進行任何修改。

**附註**：如果您執行此步驟中的程序，則可能會在升級和回復 {{site.data.keyword.cnc_short}} 1.0.3 版時遇到問題。 
 
  1. 在 `bash` Shell 或類似環境（例如 Cygwin）中，解壓縮 `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` 檔案：
  
 	```bash
 	cd {path_to_file}
 	```
 	{: pre}
 	
 	```bash
 	tar -xvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz
 	```
 	{: pre}
 	
 	解壓縮的檔案包括名為 `charts` 的目錄，其中包含名為 `ibm-watson-compare-comply-prod-1.0.3.tgz` 的檔案，此檔案為 {{site.data.keyword.cnc_short}} 之 Helm 圖表的壓縮版本。
 	
  1. 切換至 `charts` 目錄，並解壓縮 `ibm-watson-compare-comply-prod-1.0.3.tgz` 檔案：
  
 	```bash
 	cd charts
 	```
 	{: pre}
 	
 	```
 	tar -xvzf ibm-watson-compare-comply-prod-1.0.3.tgz
 	```
 	{: pre}
 	
 	解壓縮的檔案包含一些目錄和檔案，其中包括名為 `ibm-watson-compare-comply-prod` 的最上層目錄，其中包含名為 `templates` 的目錄。
 	
  1. 切換至 `ibm-watson-compare-comply-prod/templates` 目錄：
  
 	```bash
 	cd ibm-watson-compare-comply-prod/templates
 	```
 	{: pre}

  1. 在 `ibm-watson-compare-compati-prod/templates` 目錄中找出 `deployment.yaml` 檔案，並在文字編輯器中開啟它。
 
  1. 編輯 `deployment.yaml` 檔案，如下所示：
  
 	**原始**
 	```
 	- name: {{ .Chart.Name }}
 		image: "hyc-icpcontent-docker-local.artifactory.swg-devops.com/cncdev/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	
 	**已更新**
 	```
 	- name: {{ .Chart.Name }}
 		image: "mycluster.icp:8500/default/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	儲存並關閉 `deployment.yaml` 檔案。
 	
  1. 回到 `charts` 目錄，並重新包裝 `ibm-watson-compare-comply-prod-1.0.3.tgz` 檔案：
  
 	```bash
 	cd ../..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf ibm-watson-compare-comply-prod-1.0.3.tgz ibm-watson-compare-comply-prod
 	```
 	{: pre}
 	
  1. 回到最上層目錄，並重新包裝 `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` 檔案：
  
 	```bash
 	cd ..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz charts images manifest.json manifest.yaml
 	```
 	{: pre}
 	
 	 如需參照，未壓縮之 `IBM_WTN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` 檔案內容的 `tree` 視圖如下所示。此程序涉及的檔案會在右側直欄中標示 `<-----`。
 	 
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
   
### 步驟 4：準備終端機
{: #step4}
 	
在 Shell 中執行下列指令，準備要與 IBM Cloud Private 安裝搭配使用的 `bash` Shell 或對等項目：
  
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

  **附註**：在上述清單的第一個指令中，URL `https://mycluster.icp:8443` 中的埠號 `8443` 代表與 IBM Cloud Private 上的 {{site.data.keyword.cnc_short}} 服務進行通訊的預設值。

### 步驟 5：建立映像檔登錄密碼及修補程式服務帳戶
{: #step5}

執行下列指令，以啟用所有必要元件來存取 IBM Cloud Private 的內部登錄：

```bash
kubectl create secret docker-registry $secret_name --docker-server=mycluster.icp:8500 --docker-username=$username --docker-password=$password --docker-email=admin@admin.com --namespace=default
```
{: pre}

```bash
kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "$secret_name"}]}' --namespace=default
```
{: pre}

其中：
  - `$username` 和 `$password` 依叢集而有所不同。請詢問管理者。
  - `$secret_name` 是您指定的字串，[可讓 Docker 儲存器安全地通訊 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.docker.com/engine/swarm/secrets/){: new_window}。

### 步驟 6：將套件檔上傳至 IBM Cloud Private 內部映像檔登錄
{: #step6}

在您於[步驟 4](#step4) 準備的終端機中，執行下列指令來上傳 Passport Advantage 套件檔：

```bash
bx pr load-ppa-archive --archive {path_to}/IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz --clustername mycluster.icp --namespace default 
```
{: pre}

### 步驟 7：完成安裝

回到 IBM Cloud Private 叢集上的**型錄**頁面、找出並選取 `ibm-watson-compare-comply-prod` 項目，然後按一下**配置**來繼續安裝。
