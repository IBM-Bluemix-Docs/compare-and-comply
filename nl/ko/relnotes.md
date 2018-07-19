
---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-03"

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

# 릴리스 정보
{: #release_notes}

릴리스 정보는 {{site.data.keyword.BluOpenStackDed}}의 {{site.data.keyword.cnc_long}} 서비스 릴리스 변경사항에 대한 정보를 제공합니다.

**중요:** 이 문서 세트는 {{site.data.keyword.BluOpenStackDed}}의 {{site.data.keyword.cnc_short}} 서비스에만 적용됩니다. 공용 IBM Cloud에서 사용 가능한 다른 Watson 서비스에는 적용되지 않습니다.

## 서비스 API 버전화
{: #api_versioning}

API 요청에는 `version=YYYY-MM-DD` 형식의 날짜를 사용하는 버전 매개변수가 필요합니다. 역호환되지 않는 방식으로 API를 변경할 때마다 새 API 부 버전을 릴리스합니다.

모든 API 요청에서 버전 매개변수를 전송하십시오. 서비스는 지정된 날짜의 API 버전을 사용하거나 이 날짜 전에 최신 버전을 사용합니다. 현재 날짜를 기본값으로 설정하지 마십시오. 대신 사용자 애플리케이션과 호환 가능한 버전과 일치하는 날짜를 지정하고 이후 버전에 대해 사용자 애플리케이션이 준비될 때까지 변경하지 마십시오.

현재 버전은 `2018-03-23`입니다.

## 변경
{: #changes}

서비스에 대한 다음 새 기능 및 변경이 사용 가능합니다.

**중요**: 다음 절에서 언급되는 버전 번호는 {{site.data.keyword.BluOpenStackDed}} 클러스터에 배치한 {{site.data.keyword.cnc_long}} Helm 차트의 버전입니다.

### 1.0.4, 2018년 7월 5일
{: #ingress}

**중요**: 이 절에 설명된 변경으로 인해 표준 {{site.data.keyword.BluOpenStackDed}} 업그레이드 프로시저를 사용하여 이전 버전의 {{site.data.keyword.cnc_short}}에서 버전 1.0.4로 업그레이드할 수 없습니다. 다음과 같이 {{site.data.keyword.cnc_short}}을 재배치해야 합니다.

1.  [배치 제거 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_applications/remove_app.html){: new_window}에 설명된 대로 기존 {{site.data.keyword.cnc_short}} 배치를 제거하십시오.

1.  {{site.data.keyword.cnc_short}} Helm 차트의 `README.md` 파일 및 [서비스 배치](/docs/services/compare-and-comply/deploy.html)에 설명된 대로 {{site.data.keyword.cnc_short}} 버전 1.0.4 이상을 배치하십시오.

**중요:** 서비스 재배치의 결과는 다음과 같습니다.

- 서비스는 이전 인스턴스 제거와 현재 인스턴스 배치 사이에 사용할 수 없습니다.
- 호출의 URL에 포트 번호가 포함된 API 호출을 작성하는 코드가 있는 경우 포트 번호를 제거해야 합니다. {{site.data.keyword.cnc_short}} 버전 1.0.4 이상에 대해 포트 번호가 포함된 API 호출을 작성하는 경우 호출이 실패하며 다음과 유사한 오류가 발생합니다.
  ```
  curl: (7) Failed to connect to 10.19.74.45 port 8443: Connection refused
  ```
  {: pre}

**중요:** {{site.data.keyword.cnc_short}} 1.0.4 전의 버전을 실행하는 경우, 서비스에 대한 호출을 작성할 때 다음 예제와 같이 {{site.data.keyword.BluOpenStackDed}} 클러스터의 IP 주소와 함께 `:{port_number}` 지정자를 포함해야 합니다.
```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

#### 추가 참고사항

-   추가 배치 지시사항은 이제 [서비스 배치](/docs/services/compare-and-comply/deploy.html)에 제공됩니다.
-   {{site.data.keyword.cnc_short}} 서비스는 이제 {{site.data.keyword.BluOpenStackDed}}의 [ingress 제어기 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/components.html){: new_window}와 통합됩니다. 이러한 변경을 통해 사용자는 호출의 URL에 포트 번호를 지정하지 않고 서비스의 API 메소드를 호출할 수 있습니다. 다음은 **ingress** 통합 전 일반 API 호출입니다.

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
    ```
    {: pre}

  버전 1.0.4 이상의 해당 호출은 다음과 같습니다.

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45/api/v1/parse?version=2018-03-23
    ```
    {: pre}

- 1.0.4 릴리스를 배치하면 다음과 유사한 오류가 표시됩니다.

    ```
    Unexpected response code 500 from request: GET https://icp-management-ingress:8443/helm-api/api/v1/releases/weblib?locale=en-US HTTP/1.1 Accept: application/json Content-Type: application/json Content-Type: application/json Cookie: *** {} HTTP/1.1 500 Server: openresty/1.11.2.4 Date: Thu, 21 Jun 2018 23:09:05 GMT Content-Type: application/json; charset=utf-8 Content-Length: 93 Connection: close X-Powered-By: Express Cache-Control: private, max-age=0, no-cache Etag: W/"5d-6htOwRqeFWIdvat6vK/kTg" {"statusCode":500,"message":"Internal service error : Cannot read property '0' of undefined"}
    ```
    {: codeblock}

    문제 없이 오류를 무시하고 닫을 수 있습니다.

### 1.0.3, 2018년 5월 25일

- 이제 `parse` 메소드의 출력에 `attributes` 배열이 포함됩니다. 정보는 [분석 검토](/docs/services/compare-and-comply/getting-started.html#review_analysis) 및 [속성](/docs/services/compare-and-comply/parsing.html#attributes)을 참조하십시오. `attributes` 배열의 정보를 사용하여 특정 위치, 시간, 날짜, 시간 범위 또는 날짜 범위 및 화폐 가치와 단위를 참조하는 문서 요소를 검색할 수 있습니다.
- `types` 및 `categories` 배열의 각 오브젝트에는 `provenance` 오브젝트가 있습니다. `provenance` 오브젝트에는 하나 이상의 `id` 키가 있습니다. 각 `id` 키에는 피드백을 제공하거나 지원을 받기 위해 IBM에 보낼 수 있는 해시 값이 있습니다.
- PDF 구문 분석의 정확도 및 대형 PDF 파일 구문 분석 성능에 대한 개선사항입니다.
- 이 릴리스는 {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 이상에서 사용 가능합니다. {{site.data.keyword.BluOpenStackDed}} 요구사항에 대한 세부사항은 서비스에 대한 카탈로그 항목을 참조하십시오.
- `assurance` 키의 가능한 값에 `Low`가 더 이상 포함되지 않습니다.

### 1.0.2, 2018년 4월 19일

- 추가 지원되는 카테고리입니다. 지원되는 카테고리의 최신 목록은 [카테고리 문서](/docs/services/compare-and-comply/parsing.html#contract_categories)를 참조하십시오.
    - `Asset Use`
    - `Communication`
    - `Safety and Security`
-  [시작하기](/docs/services/compare-and-comply/getting-started.html)의 예제에 대한 정정을 포함한 문서 업데이트입니다.

### 1.0.1(GA(General Availability) 릴리스), 2018년 3월 23일

다음 참고사항은 {{site.data.keyword.cnc_long}} 서비스의 GA(General Availability) 릴리스에 적용됩니다.

- GA 릴리스는 {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 이상에서만 사용 가능합니다. {{site.data.keyword.BluOpenStackDed}} 요구사항에 대한 세부사항은 서비스에 대한 카탈로그 항목을 참조하십시오.

## 일반 참고사항
{: #general_notes}

- {{site.data.keyword.BluOpenStackDed}}의 {{site.data.keyword.cnc_short}} API에는 공용 IBM Cloud의 API와 같이 권한이 필요하지 않습니다.
 - 구문 분석을 위해 PDF가 텍스트 형식이어야 합니다. 스캔이 광학 문자 인식(OCR)으로 처리된 경우에도 스캔된 문서를 구문 분석할 수 없습니다.

## 알려진 문제
{: #known_issues}

- 업로드할 수 있는 PDF 파일의 최대 크기는 50MB입니다.
- 보안이 사용되는 PDF는 구문 분석할 수 없습니다.
- 비표준 페이지 레이아웃(예: 페이지당 2 또는 3열)의 문서는 올바르게 구문 분석되지 않습니다.
