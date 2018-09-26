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

# 모니터링 대시보드 설치, 구성 및 사용
{: #monitor}

IBM Cloud Private의 모니터링 대시보드를 사용하여 {{site.data.keyword.cnc_short}}의 상태를 모니터할 수 있습니다. 모니터링 대시보드는 Grafana, Kibana 및 Prometheus를 사용하여 {{site.data.keyword.cnc_short}} 인스턴스에 대한 자세하고 사용자 정의할 수 있는 정보를 표시합니다. 

{{site.data.keyword.BluOpenStackDed}} 클러스터 모니터링에 대한 정보는 [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window}을 참조하십시오. 

대시보드를 사용하려면 우선 다음 단계에서 설명하는 대로 이를 생성하고 설치해야 합니다. 

## 1단계: 대시보드 템플리트의 다운로드, 추출 및 렌더링
{: #render}

다음 단계를 수행하여 설치를 위한 대시보드 템플리트를 준비하십시오. 

1. Passport Advantage(PPA)에서 {{site.data.keyword.cnc_short}} 이미지를 다운로드하십시오. 파일은 `ibm-watson-compare-comply-prod-1.0.5.tar.gz`와 유사한 이름을 지닌 압축된 tar 파일입니다. 파일에는 템플리트에서 대시보드를 렌더링하기 위한 `bash` 스크립트 및 대시보드 템플리트가 포함됩니다. 

1. tar 파일의 압축을 풀고 이를 펼치십시오. 
  ```bash
  mkdir ibm-watson-compare-comply-prod-1.0.5 && tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tar.gz -C ibm-watson-compare-comply-prod-1.0.5
  ``` 
  {: pre}

1. 압축을 푼 디렉토리에서 `charts` 디렉토리로 변경하십시오.
  ```bash
  cd ibm-watson-compare-comply-prod-1.0.5/charts
  ```
  {: pre}

1. 압축된 tar 파일의 압축을 풀고 이를 `charts` 디렉토리에 펼치십시오.
  ```bash
  tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tgz
  ```
  {: pre}

1. `dashboard` 디렉토리로 변경하십시오. 여기에는 경보, 로깅 및 메트릭에 대한 템플리트는 물론 템플리트에서 대시보드를 생성하기 위한 `bash` 스크립트도 포함됩니다. 
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

1. `render-dashboards.sh` 스크립트를 실행하여 템플리트를 렌더링하십시오. 스크립트에 대한 옵션은 다음과 같습니다.
  
    - `-v`, `--version {chart_version}`: 차트 버전(예: `1.0.5`)입니다. 
    - `-h`, `--help`: 명령 도움말을 인쇄하고 종료합니다.
    - `-r`, `--release {release_name}`: Helm 릴리스 이름입니다. 
    - `-n`, `--namespace {namespace}`: 배치의 네임스페이스입니다. 기본 네임스페이스는 `default`입니다.

  ```bash
  ./render-dashboards.sh -v 1.0.5 -r my-test-release -n default
  ```
  {: pre}

대시보드 JSON 파일은 `ibm-watson-compare-comply-prod-1.0.5/charts/ibm-watson-compare-comply-prod/dashboard` 아래에 생성됩니다. 

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

## 2단계: 템플리트 가져오기
{: #import-tpls}

대시보드를 렌더링한 후에는 다음 절에서 설명하는 대로 이 중에서 하나 이상을 가져오십시오. 

  - [메트릭 대시보드 가져오기](metrics.html#import)
  - [로깅 대시보드 가져오기](logging.html#import)
  - [경보 대시보드 가져오기](alerts.html#import)

## 3단계: 대시보드 보기 및 사용자 정의
{: #use-dboards}

선택된 대시보드를 가져온 후에는 다음 절에서 설명하는 대로 이를 보고 사용자 정의하십시오. 

  - [메트릭 대시보드 보기](metrics.html#view)
  - [로깅 대시보드 보기](logging.html#view)
  - [경보 규칙 추가](alerts.html#add)
