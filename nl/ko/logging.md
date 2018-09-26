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

# 로깅 사용
{: #logging}

## 로깅 대시보드 가져오기

{{site.data.keyword.cnc_short}}에 대한 로깅 대시보드를 IBM Cloud Private으로 가져오려면 다음 단계를 수행하십시오.

  1. [1단계: 대시보드 템플리트의 다운로드, 추출 및 렌더링](/docs/services/compare-and-comply/monitor.html#monitor)에서 설명한 대로 로깅 대시보드를 추출하고 생성했는지 확인하십시오.  

  1. IBM Cloud Private 클러스터에 로그인하십시오.

  1. 왼쪽 상단 모서리에 있는 메뉴 아이콘에서 **플랫폼 -> 로깅**을 선택하십시오. <br />
    ![IBM Cloud Private 메뉴 아이콘](images/icp-menu.png) <br />
    ![플랫폼 -> 로깅 메뉴](images/icp-logging.png)

  1. Kibana 인터페이스의 왼쪽에 있는 **관리**를 클릭하십시오. <br />
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
{: #view}

로깅 대시보드를 보려면 다음 단계를 수행하십시오.

  1. **검색** 탭으로 이동하십시오.

  1. Kibana 인터페이스의 맨 위 오른쪽에 있는 **열기**를 클릭하십시오.

  1. 보려는 대시보드를 선택하십시오. 서비스 로그 및 외부 프로세스 로그에 대한 두 가지 로깅 대시보드가 있습니다.
    ![로깅 대시보드 보기](images/kibana-dboards.png)

시간 범위 및 자동 새로 고치기 빈도를 쉽게 변경할 수 있습니다.
  ![시간 범위 및 새로 고치기 간격 변경](images/log-dboard-change.png)

