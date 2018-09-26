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

# 배치 구성 이해
{: #configs}

{{site.data.keyword.cnc_short}}에 대한 기본 배치 구성은 `2Cores 10G 1 concurrent document (2 VPC)`입니다. 이 구성은 시간당 약 1500페이지를 처리할 수 있습니다. 처리량 요구사항에 따라 IBM Cloud Private의 {{site.data.keyword.cnc_short}} 배치를 여러 레벨로 확장할 수 있습니다. IBM Cloud Private의 {{site.data.keyword.cnc_short}} 용량 늘리기에 대한 자세한 정보는 IBM 영업 담당자에게 문의하십시오.

{{site.data.keyword.cnc_short}}은 두 가지 차원, 즉 _수직_과 _수평_으로 확장할 수 있습니다.

 - _수직적_ 확장은 문서가 구문 분석되는 처리량을 늘립니다.
 - _수평적_ 확장은 구문 분석되는 동시 문서의 수를 늘립니다.

두 확장 옵션에서는 {{site.data.keyword.cnc_short}}에서 사용되는 VPC(Virtual Processor Cores)의 수를 늘려야 합니다. VPC에 대한 정보는 IBM Cloud Private [라이센싱 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_cluster/licensing.html){: new_window}를 참조하십시오.

수직적 확장 옵션은 다음과 같습니다.

| 구성                             |대략적 처리량*         |
|:-----------------------------------------:|--------------------------------|
|`2Cores 10G 1 concurrent document (2 VPC)` |시간당 1500페이지(기본값)   |
|`4Cores 20G 1 concurrent document (4 VPC)` |시간당 2500페이지             |
|`8Cores 40G 1 concurrent document (8 VPC)` |시간당 3000페이지             |

수평적 확장 옵션은 다음과 같습니다.

| 구성                               |대략적 처리량*         |
|:-------------------------------------------:|--------------------------------|
|`2Cores 10G 2 concurrent documents (4 VPC)`  |시간당 3000페이지             |
|`4Cores 20G 2 concurrent documents (8 VPC)`  |시간당 5000페이지             |
|`8Cores 40G 2 concurrent documents (16 VPC)` |시간당 6000페이지             |

\***참고**: 처리량 추정은 내부 테스트를 기반으로 합니다. 프로덕션 환경의 처리량은 동일한 IBM Cloud Private 클러스터의 다른 워크로드, 클라이언트 애플리케이션과 IBM Cloud Private 클라이언트 간 대기 시간 및 다른 환경 조건이 포함된 여러 요인에 따라 다릅니다.

{{site.data.keyword.cnc_short}} 클러스터를 추가하거나 제거하여 배치 구성을 변경하려면 다음 단계를 수행하십시오.

**참고**: 사용자가 {{site.data.keyword.cnc_short}} 배치를 축소하면 IBM Cloud Private이 즉시 해제된 리소스를 재확보하므로 배치의 처리 용량을 줄일 수 있습니다. 특히 배치가 프로덕션 환경에 있는 경우 사용 중인 배치를 축소하지 마십시오.
	
이와 유사하게 사용자가 {{site.data.keyword.cnc_short}} 배치를 확장하면 IBM Cloud Private이 즉시 요청된 리소스를 배치에 적용하므로 리소스가 사용 가능하다고 추정됩니다. IBM Cloud Private 설치에서 제공하는 리소스보다 많은 리소스가 필요한 경우 IBM Cloud Private 설치 용량 늘리기에 대해 IBM 영업 담당자에게 문의하십시오.

  1. IBM Cloud Private 클러스터에 로그인하십시오.

  1. 왼쪽 상단 모서리에 있는 메뉴 아이콘에서 **워크로드 -> 배치**를 선택하십시오.
  
  1. 콘솔에 **배치** 페이지가 표시됩니다. 배치 테이블에서 스케일링할 배치를 찾으십시오.
  
  1. 배치가 나열된 테이블 행에서 테이블의 마지막 열에 있는 **조치** 아이콘을 클릭한 다음 **스케일링**을 클릭하십시오.
  
  1. 콘솔에 **배치 스케일링** 창이 표시됩니다. **인스턴스** 필드에 원하는 인스턴스 수를 입력하거나 위로 및 아래로 화살표를 사용하여 원하는 값을 선택하십시오.
  
  1. **배치 스케일링**을 클릭하십시오.
  
  1. **배치 스케일링** 창이 닫히고 콘솔이 **배치** 페이지 표시로 돌아갑니다. 배치를 찾고 **원하는** 열 및 **현재** 열을 사용하여 요청된 인스턴스 수가 배치에 할당되었는지 확인하십시오.
  
  1. 선택적으로 **배치** 페이지에서 배치 이름을 클릭하여 배치 세부사항을 보십시오.
  
  

