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

# 메트릭 사용
{: #metrics}

IBM Cloud Private의 모니터링 대시보드를 사용하여 {{site.data.keyword.cnc_short}} 상태를 모니터할 수 있습니다. 모니터링 대시보드는 메트릭에 Grafana, 경보에 Prometheus 및 로깅에 Kibana를 사용하여 {{site.data.keyword.cnc_short}} 인스턴스에 대한 자세한 정보를 제시합니다. 

## 메트릭 대시보드 가져오기

{{site.data.keyword.cnc_short}}에 대한 메트릭 대시보드를 IBM Cloud Private으로 가져오려면 다음 단계를 수행하십시오.

  1. [1단계: 대시보드 템플리트의 다운로드, 추출 및 렌더링](/docs/services/compare-and-comply/monitor.html#monitor)에서 설명한 대로 메트릭 대시보드를 추출하고 생성했는지 확인하십시오. 

  1. IBM Cloud Private 클러스터에 로그인하십시오.

  1. 왼쪽 상단 모서리에 있는 메뉴 아이콘에서 **플랫폼 -> 모니터링**을 선택하십시오. <br />
      ![IBM Cloud Private 메뉴 아이콘](images/icp-menu.png) <br />
      ![플랫폼 -> 모니터링 메뉴](images/icp-monitoring.png)

  1. Grafana 인터페이스의 왼쪽 상단에 있는 **홈**을 클릭하십시오. <br />
      ![홈 아이콘](images/icp-home.png)

  1. **대시보드 가져오기**를 클릭하십시오.
      ![대시보드 가져오기 아이콘](images/import-dboard.png)

  1. 이전 프로시저의 6단계에서 생성된 `metrics.json` 파일을 선택한 다음 **.json 파일 업로드**를 클릭하십시오. <br />
      ![metrics.json 파일 업로드](images/metrics-json.png)

  1. **Prometheus**를 데이터 소스로 선택한 다음 **가져오기**를 클릭하십시오.
       ![Prometheus 선택](images/prometheus.png)

## 메트릭 대시보드 보기
{: #view}

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
        
