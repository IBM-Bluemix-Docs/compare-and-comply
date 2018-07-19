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

# 로깅 사용
{: #using-logging}

## 로깅 대시보드 설치 및 실행

{{site.data.keyword.cnc_short}}에 대한 로깅 대시보드를 설치하려면 다음 단계를 수행하십시오.

  1. {{site.data.keyword.cnc_short}}의 PPA(Passport Advantage) 파일을 다운로드하십시오. 파일은 이름이 `ibm-watson-compare-comply-prod-1.0.0.tar.gz`와 유사한 압축된 tar 파일입니다. 파일에는 템플리트에서 대시보드를 렌더링하기 위한 `bash` 스크립트 및 로깅 대시보드 템플리트가 포함됩니다.

  1. PPA 파일의 압축을 풀고 이를 펼치십시오.
    ```bash
    $ mkdir ibm-watson-compare-comply-prod-1.0.0 && tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tar.gz -C ibm-watson-compare-comply-prod-1.0.0
    ```
    {: codeblock}

  1. 압축을 푼 디렉토리에서 `charts` 디렉토리로 변경하십시오.
    ```bash
    $ cd ibm-watson-compare-comply-prod-1.0.0/charts
    ```

  1. 압축된 tar 파일의 압축을 풀고 이를 `charts` 디렉토리에 펼치십시오.
    ```bash
    $ tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tgz
    ```

  1. `dashboard` 디렉토리로 변경하십시오. 이 파일에는 메트릭 및 로깅을 위한 템플리트와 템플리트에서 대시보드를 생성하기 위한 bash 스크립트가 포함됩니다.

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

  1. `render-dashboards.sh` 스크립트를 실행하여 템플리트를 렌더링하십시오. 스크립트에 대한 옵션은 다음과 같습니다.
  
    -  `-v, --version {chart_version}`: 차트 버전입니다(예: `1.0.0`).
    -  `-h, --help`: 명령 도움말을 인쇄하고 종료합니다.
    -  `-r, --release {release_name}`: Helm 릴리스 이름입니다.
    -  `-n, --namespace {namespace}`: 배치의 네임스페이스입니다. 기본 네임스페이스는 `default`입니다.

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

## 로깅 대시보드 가져오기

{{site.data.keyword.cnc_short}}에 대한 로깅 대시보드를 IBM Cloud Private으로 가져오려면 다음 단계를 수행하십시오.

  1. IBM Cloud Private 클러스터에 로그인하십시오.

  1. 왼쪽 상단 모서리에 있는 메뉴 아이콘에서 **플랫폼 -> 로깅**을 선택하십시오.<br />
    ![IBM Cloud Private 메뉴 아이콘](images/icp-menu.png) <br />
    ![플랫폼 -> 로깅 메뉴](images/icp-logging.png)

  1. Kibana 인터페이스의 왼쪽에 있는 **관리**를 클릭하십시오.<br />
    ![Kibana 인터페이스](images/kibana.png)

  1. **저장된 오브젝트** 탭을 선택하십시오.
    ![저장된 오브젝트 탭](images/saved-obj.png)

  1. **검색** 탭을 선택하고 **가져오기**를 클릭하십시오.
    ![검색에서 가져오기 탭](images/searches-import.png)

  1. 이전 프로시저의 6단계에서 생성된 `frontend-logging.json` 및 `external-process-logging.json` 파일을 개별적으로 가져오십시오. 프롬프트가 표시되면 **예, 모두 겹쳐씁니다**를 클릭하십시오.
     ![예, 모든 프롬프트를 겹쳐씁니다](images/overwrite-all.png)

  1. 대시보드가 **검색** 탭에 나타납니다.
     ![검색 탭의 대시보드](images/searches-tab.png)

## 로깅 대시보드 보기

로깅 대시보드를 보려면 다음 단계를 수행하십시오.

  1. **검색** 탭으로 이동하십시오.

  1. Kibana 인터페이스의 맨 위 오른쪽에 있는 **열기**를 클릭하십시오.

  1. 보려는 대시보드를 선택하십시오. 서비스 로그 및 외부 프로세스 로그에 대한 두 가지 로깅 대시보드가 있습니다.
    ![로깅 대시보드 보기](images/kibana-dboards.png)

시간 범위 및 자동 새로 고치기 빈도를 쉽게 변경할 수 있습니다.
  ![시간 범위 및 새로 고치기 간격 변경](images/log-dboard-change.png)

