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

# 서비스 배치
{: #install}

Helm 차트의 `README.md` 파일에 설명된 대로 {{site.data.keyword.cnc_long}} Helm 차트를 설치하기 전에 추가 배치 단계를 수행해야 합니다.

## 기본 시스템값
{: #sys_vals}

이러한 지시사항에서는 다음 기본 시스템값을 사용합니다. 시스템의 값은 다를 수 있습니다. IBM Cloud Private 관리자와 확인하십시오.

 - IBM Cloud Private 클러스터 이름 및 포트 번호:
 	`mycluster.icp:8500`
 	
 - IBM Cloud Private 네임스페이스:
 	`default`

## 배치 완료
{: #deploy}

다음 단계를 수행하여 {{site.data.keyword.cnc_short}} 배치를 완료하십시오.

### 1단계: Docker 액세스 권한 부여
{: #step1}

  1. IBM Cloud Private 레지스트리에 Docker 액세스 권한을 부여하십시오.
  
  	Microsoft Windows 또는 Apple macOS 또는 OS X를 실행 중인 경우 다음 단계를 수행하십시오.

 	 1. **Docker** 아이콘을 클릭하십시오.
 	 1. Docker **환경 설정** 메뉴를 여십시오.
 	 1. **디먼** 탭을 클릭하십시오.
 	 1. **비보안 레지스트리** 필드에서 `mycluster.icp:8500` 값 또는 IBM Cloud Private 설치의 값을 추가하십시오.
 	 
 	 ![Docker 구성](images/docker.png)
 	 
 	Linux를 실행하는 경우 다음 단계를 수행하십시오.
 	
 	1. `daemon.json` 파일을 찾으십시오. 기본적으로 이 파일은 `/etc/docker/daemon.json`에 있습니다. 파일이 없는 경우 기본 위치에서 작성하십시오. 텍스트 편집기에서 파일을 열고 다음 JSON을 확인하거나 추가하십시오.
 	
 	  ```json
 	  {
 	    "insecure-registries": ["mycluster.icp:8500"]
 	  }
 	  ```
 	  
 	1. 다음 명령을 실행하십시오.
 	  ```bash
 	  systemctl daemon reload
 	  ```
 	  {: pre}
 	  
 	  ```bash
 	  systemctl restart docker
 	  ``` 
 	  {: pre}

### 2단계: IBM Cloud Private 및 IBM Cloud CLI 다운로드
{: #step2}

  1. 사용 중인 각 IBM Cloud Private 버전에 대한 Helm CLI를 다운로드하고 설치하십시오. 세부사항은 [IBM Cloud Private 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/icp_cli.html){: new_window}를 참조하십시오.
  
  1. IBM Cloud CLI를 다운로드하고 설치하십시오. 세부사항은 [IBM Cloud 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html#download_install){: new_window}를 참조하십시오.
  
  1. IBM Cloud CLI `bx` 플러그인을 다운로드하고 설치하십시오. 세부사항은 [IBM Cloud 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/extend_cli.html#plug-ins){: new_window}를 참조하십시오.
 	
### 3단계: 패키지 파일 다운로드
{: #step3}
 	
[Passport Advantage ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www-01.ibm.com/software/passportadvantage/){: new_window}의 `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` 패키지 파일을 로컬 워크스테이션으로 다운로드하십시오. 이 파일에는 전체 {{site.data.keyword.cnc_short}} 서비스 이미지가 포함되어 있습니다.
  
#### 필요한 경우 패키지 파일 편집
{: edit-pkg-file}

**중요**: 다음 조건이 true인 경우에*만* 이 단계의 프로시저를 수행하십시오. 조건 중 하나 또는 둘 다 false인 경우 [4단계](#step4)를 진행하십시오.

 - 이 단계는 {{site.data.keyword.cnc_short}} 버전 1.0.3 이하에만 적용됩니다. {{site.data.keyword.cnc_short}} 버전 1.0.4 이상을 설치하는 경우 이 단계에서 프로시저를 수행하지 마십시오.

 - IBM Cloud Private 클러스터가 외부 인터넷에 연결되지 않은 경우에만 이 단계의 프로시저를 따라야 합니다. IBM Cloud Private 클러스터에 [Passport Advantage ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www-01.ibm.com/software/passportadvantage/){: new_window} 사이트에 대한 네트워크 액세스 권한이 있는 경우 패키지 파일 수정 없이 {{site.data.keyword.cnc_short}} 버전 1.0.3을 설치하고 배치할 수 있습니다.

**참고**: 이 단계의 프로시저를 수행하는 경우 {{site.data.keyword.cnc_short}} 버전 1.0.3의 업그레이드 및 롤백에 대한 문제점이 발생할 수 있습니다. 
 
  1. `bash` 쉘 또는 유사 환경(예: Cygwin)에서 `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` 파일의 압축을 푸십시오.
  
 	```bash
 	cd {path_to_file}
 	```
 	{: pre}
 	
 	```bash
 	tar -xvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz
 	```
 	{: pre}
 	
 	압축을 푼 파일에는 파일 `ibm-watson-compare-comply-prod-1.0.3.tgz`이 포함된 디렉토리 `charts`가 포함되어 있으며, 이는 {{site.data.keyword.cnc_short}}에 대한 Helm 차트의 압축된 버전입니다.
 	
  1. `charts` 디렉토리로 변경하고 `ibm-watson-compare-comply-prod-1.0.3.tgz` 파일의 압축을 푸십시오.
  
 	```bash
 	cd charts
 	```
 	{: pre}
 	
 	```
 	tar -xvzf ibm-watson-compare-comply-prod-1.0.3.tgz
 	```
 	{: pre}
 	
 	압축을 푼 파일에는 여러 디렉토리와 파일이 포함되어 있으며, 디렉토리 `templates`가 포함된 최상위 레벨 디렉토리 `ibm-watson-compare-comply-prod`도 포함됩니다.
 	
  1. `ibm-watson-compare-comply-prod/templates` 디렉토리로 변경하십시오.
  
 	```bash
 	cd ibm-watson-compare-comply-prod/templates
 	```
 	{: pre}

  1. `ibm-watson-compare-comply-prod/templates` 디렉토리에서 `deployment.yaml` 파일을 찾아 텍스트 편집기에서 여십시오.
 
  1. 다음과 같이 `deployment.yaml` 파일을 편집하십시오.
  
 	**원본**
 	```
 	- name: {{ .Chart.Name }}
 		image: "hyc-icpcontent-docker-local.artifactory.swg-devops.com/cncdev/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	
 	**업데이트됨**
 	```
 	- name: {{ .Chart.Name }}
 		image: "mycluster.icp:8500/default/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	`deployment.yaml` 파일을 저장하고 닫으십시오.
 	
  1. `charts` 디렉토리로 돌아가 `ibm-watson-compare-comply-prod-1.0.3.tgz` 파일을 다시 패키징하십시오.
  
 	```bash
 	cd ../..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf ibm-watson-compare-comply-prod-1.0.3.tgz ibm-watson-compare-comply-prod
 	```
 	{: pre}
 	
  1. 최상위 레벨 디렉토리로 돌아가 `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` 파일을 다시 패키징하십시오.
  
 	```bash
 	cd ..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz charts images manifest.json manifest.yaml
 	```
 	{: pre}
 	
 	 참고로, 압축을 푼 `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` 파일의 컨텐츠에 대한 `tree` 보기는 다음과 같습니다. 이 프로시저와 연관된 파일은 오른쪽 열에서 `<-----`로 콜아웃됩니다.
 	 
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
   
### 4단계: 터미널 준비
{: #step4}
 	
쉘에서 다음 명령을 실행하여 IBM Cloud Private 설치에 사용할 `bash` 쉘 또는 이와 동등한 환경을 준비하십시오.
  
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

  **참고**: 이전 목록의 첫 번째 명령에서 URL `https://mycluster.icp:8443`의 포트 번호 `8443`은 IBM Cloud Private의 {{site.data.keyword.cnc_short}} 서비스와 통신하는 데 필요한 기본값을 나타냅니다.

### 5단계: 이미지 레지스트리 시크릿 및 패치 서비스 계정 작성
{: #step5}

다음 명령을 실행하여 모든 필수 컴포넌트가 IBM Cloud Private의 내부 레지스트리에 액세스할 수 있도록 하십시오.

```bash
kubectl create secret docker-registry $secret_name --docker-server=mycluster.icp:8500 --docker-username=$username --docker-password=$password --docker-email=admin@admin.com --namespace=default
```
{: pre}

```bash
kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "$secret_name"}]}' --namespace=default
```
{: pre}

여기서
  - `$username` 및 `$password`는 클러스터에 따라 다릅니다. 관리자에게 문의하십시오.
  - `$secret_name`은 [Docker 컨테이너가 안전하게 통신할 수 있게 하기 위해 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.docker.com/engine/swarm/secrets/){: new_window} 지정하는 문자열입니다.

### 6단계: IBM Cloud Private 내부 이미지 레지스트리에 패키지 파일 업로드
{: #step6}

[4단계](#step4)에서 준비한 터미널에서 다음 명령을 실행하여 Passport Advantage 패키지 파일을 업로드하십시오.

```bash
bx pr load-ppa-archive --archive {path_to}/IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz --clustername mycluster.icp --namespace default 
```
{: pre}

### 7단계: 설치 완료

IBM Cloud Private 클러스터의 **카탈로그** 페이지로 돌아가고 `ibm-watson-compare-comply-prod` 항목을 찾아 선택한 후 **구성**을 클릭하여 설치를 계속하십시오.
