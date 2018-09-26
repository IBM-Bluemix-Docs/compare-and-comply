---

copyright:
years: 2018
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

# 部署服务
{: #install}

安装 {{site.data.keyword.cnc_long}} Helm 图表之前（如 Helm 图表的 `README.md` 文件中所述），您可能需要执行其他部署步骤。如果部署的是 {{site.data.keyword.cnc_long}} V1.0.3 或更低版本，那么需要执行以下步骤。如果部署的是 {{site.data.keyword.cnc_long}} V1.0.4 或更高版本，那么以下步骤不适用。

## 缺省系统值
{: #sys_vals}

这些指示信息使用以下缺省系统值。系统的值可能会有所不同。请咨询 IBM Cloud Private 管理员。

 - IBM Cloud Private 集群名称和端口号：
      `mycluster.icp:8500`

 - IBM Cloud Private 名称空间：
      `default`

## 完成部署
{: #deploy}

执行以下步骤以完成 {{site.data.keyword.cnc_short}} 部署：

### 步骤 1：授予 Docker 访问权
{: #step1}

  1. 授予对 IBM Cloud Private 注册表的 Docker 访问权。
  
    - 如果运行的是 Microsoft Windows 或者 Apple macOS 或 OS X，请执行以下步骤：

      1. 单击 **Docker** 图标。
      
      1. 打开 Docker **首选项**菜单。
    
      1. 单击**守护程序**选项卡。
    
      1. 在**不安全注册表**字段中，添加 `mycluster.icp:8500` 值或 IBM Cloud Private 安装的值。

      ![配置 Docker](images/docker.png)

    - 如果运行的是 Linux，请执行以下步骤：

      1. 找到 `daemon.json` 文件。缺省情况下，此文件的位置是 `/etc/docker/daemon.json`。如果该文件不存在，请在缺省位置创建该文件。在文本编辑器中打开该文件，然后验证或添加以下 JSON。

      ```
      {
         "insecure-registries": ["mycluster.icp:8500"]
 	  }
 	  ```
      {: pre}
      
      1. 运行以下命令：
      
      ```bash
systemctl daemon reload
 	  ```
      {: codeblock}
      
      ```bash
systemctl restart docker
 	  ```
      {: codeblock} 

### 步骤 2：下载 IBM Cloud Private 和 IBM Cloud CLI
{: #step2}

  1. 针对使用的每个 IBM Cloud Private 版本下载相应的 Helm CLI 并进行安装。有关详细信息，请参阅 [IBM Cloud Private 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/icp_cli.html){: new_window}。
  
  1. 下载并安装 IBM Cloud CLI。有关详细信息，请参阅 [IBM Cloud 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html#download_install){: new_window}。
  
  1. 下载并安装 IBM Cloud CLI `bx` 插件。有关详细信息，请参阅 [IBM Cloud 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/extend_cli.html#plug-ins){: new_window}。

### 步骤 3：下载包文件
{: #step3}

将 `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` 包文件从 [Passport Advantage ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www-01.ibm.com/software/passportadvantage/){: new_window} 下载到本地工作站。此文件包含整个 {{site.data.keyword.cnc_short}} 服务映像。
  
#### 根据需要，编辑该包文件
{: edit-pkg-file}

**重要信息**：*仅当*满足以下条件时，才可执行此步骤中的过程。如果不满足其中一个条件或两个条件都不满足，请继续执行[步骤 4](#step4)。

 - 此步骤仅适用于 {{site.data.keyword.cnc_short}} V1.0.3 和更低版本。如果要安装 {{site.data.keyword.cnc_short}} V1.0.4 或更高版本，请勿执行此步骤中的过程。

 - 仅当 IBM Cloud Private 集群未连接到外部因特网时，才需要执行此步骤中的过程。如果 IBM Cloud Private 集群对 [Passport Advantage ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www-01.ibm.com/software/passportadvantage/){: new_window} 站点具有网络访问权，那么不用对包文件进行任何修改，就可以安装并部署 {{site.data.keyword.cnc_short}} V1.0.3。

**注**：如果执行此步骤中的过程，可能会遇到 {{site.data.keyword.cnc_short}} V1.0.3 升级和回滚问题。 

  1. 在 `bash` shell 或类似环境（如 Cygwin）中，解压缩 `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` 文件：
  
  ```bash
  cd {path_to_file}
  ```
  {: codeblock}

  ```bash
tar -xvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz
 	```
  {: codeblock}

 解压缩的文件包含一个名为 `charts` 的目录，其中包含名为 `ibm-watson-compare-comply-prod-1.0.3.tgz` 的文件，该文件是 {{site.data.keyword.cnc_short}} 的 Helm 图表的压缩版本。

  1. 切换到 `charts` 目录并解压缩 `ibm-watson-compare-comply-prod-1.0.3.tgz` 文件：
  
  ```bash
  cd charts
  ```
  {: codeblock}
 
  ```bash
tar -xvzf ibm-watson-compare-comply-prod-1.0.3.tgz
 	```
  {: codeblock}
 
  解压缩的文件包含多个目录和文件，其中包括含有 `templates` 目录的 `ibm-watson-compare-comply-prod` 顶级目录。

  1. 切换到 `ibm-watson-compare-comply-prod/templates` 目录：
  
  ```bash
  cd ibm-watson-compare-comply-prod/templates
  ```
  {: codeblock}
 
  1. 在 `ibm-watson-compare-comply-prod/templates` 目录中找到 `deployment.yaml` 文件，并在文本编辑器中将其打开。
 
  1. 如下所示编辑 `deployment.yaml` 文件：
  
  **原始内容**
  
  ```
    – name: {{ .Chart.Name }}
        image: "hyc-icpcontent-docker-local.artifactory.swg-devops.com/cncdev/cnc-frontend:v2.3.12"
        imagePullPolicy: Always
  ```
  {: codeblock}
 
  **更新内容**
  
  ```
    – name: {{ .Chart.Name }}
        image: "mycluster.icp:8500/default/cnc-frontend:v2.3.12"
        imagePullPolicy: Always
  ```
  {: pre}
  
  保存并关闭 `deployment.yaml` 文件。

  1. 返回到 `charts` 目录并重新打包 `ibm-watson-compare-comply-prod-1.0.3.tgz` 文件：
  
    ```bash
cd ../..
    ```
    {: codeblock}
  
    ```bash
tar -cvzf ibm-watson-compare-comply-prod-1.0.3.tgz ibm-watson-compare-comply-prod
 	```
    {: codeblock}
   
  1. 返回到顶级目录，然后重新打包 `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` 文件：
  
    ```bash
cd ..
 	```
    {: codeblock}
 
   ```bash
tar -cvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz charts images manifest.json manifest.yaml
 	```
  {: codeblock}
  
  下面是解压缩后“IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz”文件的内容“tree”视图，供您参考。此过程中涉及的文件由右侧列的“<-----”指示。

  ```bash
  cd {path_to_archive_file}
  ```
  {: codeblock}
   
  ```
  $ tree
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
   
### 步骤 4：准备终端
{: #step4}

通过在 shell 中运行以下命令，准备“bash”shell 或等效程序，以用于 IBM Cloud Private 安装：

  ```bash
  bx pr login -a https://mycluster.icp:8443 --skip-ssl-validation
  ```
  {: codeblock}
    
  ```bash
  bx pr cluster-config mycluster
  ```
  {: codeblock}
  
  ```bash
  docker login mycluster.icp:8500
  ```
  {: codeblock}

  **注**：在上面列表中的第一个命令中，URL“https://mycluster.icp:8443”中的端口号“8443”表示用于与 IBM Cloud Private 上的 {{site.data.keyword.cnc_short}} 服务进行通信的缺省值。

### 步骤 5：创建映像注册表私钥和补丁服务帐户
{: #step5}

运行以下命令，以使所有必需组件都能访问 IBM Cloud Private 内部注册表：

```bash
kubectl create secret docker-registry $secret_name --docker-server=mycluster.icp:8500 --docker-username=$username --docker-password=$password --docker-email=admin@admin.com --namespace=default
```
{: codeblock}

```bash
kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "$secret_name"}]}' --namespace=default
```
{: codeblock}

其中：
  - “$username”和“$password”因集群而异。请询问管理员。
  - “$secret_name”是您所指定的字符串，用于 [实现 Docker 容器的安全通信 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.docker.com/engine/swarm/secrets/){: new_window}。

### 步骤 6：将包文件上传到 IBM Cloud Private 内部映像注册表
{: #step6}

在 [步骤 4](#step4) 准备的终端中，运行以下命令来上传打包的映像文件：

```bash
bx pr load-ppa-archive --archive {path_to}/IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz --clustername mycluster.icp --namespace default 
```
{: codeblock}
### 步骤 7：完成安装

返回到 IBM Cloud Private 集群上的 **目录** 页面，找到并选择“ibm-watson-compare-comply-prod”条目，然后单击 **配置** 以继续安装。
