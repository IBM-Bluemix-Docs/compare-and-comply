---

copyright:
  years: 2017, 2018
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

# 메트릭 사용
{: #using-metrics}

IBM Cloud Private의 모니터링 대시보드를 사용하여 {{site.data.keyword.cnc_short}} 상태를 모니터할 수 있습니다. 모니터링 대시보드는 Grafana, Prometheus 및 Kibana를 사용하여 {{site.data.keyword.cnc_short}} 인스턴스에 대한 자세한 정보를 제공합니다.

모니터링 대시보드에 대한 자세한 정보는 [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window}을 참조하십시오.

## 메트릭 대시보드 설치 및 실행

{{site.data.keyword.cnc_short}}의 메트릭 대시보드를 설치하려면 다음 단계를 수행하십시오.

 1. {{site.data.keyword.cnc_short}}의 PPA(Passport Advantage) 파일을 다운로드하십시오. 파일은 이름이 `ibm-watson-compare-comply-prod-1.0.0.tar.gz`와 유사한 압축된 tar 파일입니다. 파일에는 템플리트에서 대시보드를 렌더링하기 위한 `bash` 스크립트 및 메트릭 대시보드 템플리트가 포함됩니다.

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

 1. `dashboard` 디렉토리로 변경하십시오. 이 파일에는 메트릭 및 로깅을 위한 템플리트와
템플리트에서 대시보드를 생성하기 위한 bash 스크립트가 포함됩니다.

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

## 메트릭 대시보드 가져오기

{{site.data.keyword.cnc_short}}에 대한 메트릭 대시보드를 IBM Cloud Private으로 가져오려면 다음 단계를 수행하십시오.

  1. IBM Cloud Private 클러스터에 로그인하십시오.

  1. 왼쪽 상단 모서리에 있는 메뉴 아이콘에서 **플랫폼 -> 모니터링**을 선택하십시오.<br />
      ![IBM Cloud Private 메뉴 아이콘](images/icp-menu.png) <br />
      ![플랫폼 -> 모니터링 메뉴](images/icp-monitoring.png)

  1. Grafana 인터페이스의 왼쪽 상단에 있는 **홈**을 클릭하십시오.<br />
      ![홈 아이콘](images/icp-home.png)

  1. **대시보드 가져오기**를 클릭하십시오.
      ![대시보드 가져오기 아이콘](images/import-dboard.png)

  1. 이전 프로시저의 6단계에서 생성된 `metrics.json` 파일을 선택한 다음 **.json 파일 업로드**를 클릭하십시오. <br />
      ![metrics.json 파일 업로드](images/metrics-json.png)

  1. **Prometheus**를 데이터 소스로 선택한 다음 **가져오기**를 클릭하십시오.
       ![Prometheus 선택](images/prometheus.png)

## 메트릭 대시보드 보기

메트릭 대시보드는 다음과 비슷합니다.
![메트릭 대시보드](images/metrics-dboard.png)

시간 범위 및 자동 새로 고치기 빈도를 쉽게 변경할 수 있습니다.
![시간 범위 및 새로 고치기 간격 변경](images/dboard-change.png)

## 메트릭 대시보드 편집

다음 단계를 수행하여 메트릭 대시보드를 편집하거나 새 대시보드를 작성할 수 있습니다.

  1. 왼쪽 상단 모서리에 있는 메뉴 아이콘에서 **플랫폼 -> 모니터링**을 선택하여 Grafana UI에 액세스하십시오.

  1. Grafana 인터페이스의 왼쪽 상단에 있는 **홈**을 클릭한 다음 **+ 새 대시보드**를 클릭하십시오.

  1. 추가할 패널의 유형을 선택하십시오(예: **그래프** 또는 **테이블**).

  1. 패널 제목을 클릭한 다음 **편집**을 클릭하십시오. 기본 패널 제목은 `Panel title`입니다.

  1. **일반** 탭을 사용하여 패널 제목, 설명 및 차원을 설정하십시오. 12단위는 브라우저 창의 전체 너비입니다.

  1. **메트릭** 탭을 사용하여 Prometheus에서 데이터를 표시하는 조회를 작성하십시오.

        1. 조회 언어에 익숙한 경우 조회를 직접 작성할 수 있거나, **메트릭 검색** 필드를 사용하여 현재 보고되는 메트릭부터 Prometheus까지 선택할 수 있습니다.

        1. 새 대시보드 패널에 실시간으로 조회 결과가 표시됩니다.

        1. 여러 조회를 단일 패널에 추가할 수 있습니다. 예를 들어, 동일한 그래프에 읽기 및 쓰기 오퍼레이션을 표시하거나 동일한 테이블에 총 방문 수와 총 방문자 수를 표시할 수 있습니다.
        
