---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-22"

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

# 시작하기
{: #getting_started}

이 짧은 튜토리얼에서는 IBM Cloud Private의 {{site.data.keyword.cnc_long}}을 소개하고 계약을 구문 분석하여 컴포넌트 조각, 해당 네이처, 영향을 받는 당사자 및 식별되는 모든 카테고리를 식별하는 프로세스를 살펴봅니다.

## 시작하기 전에
{: #before-you-begin}

{{site.data.keyword.cnc_short}} 서비스를 사용하려면 먼저 [IBM Cloud Private CLI 설치](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/install_cli.html)에 설명된 대로 IBM Cloud Private CLI를 설치하고 IBM Cloud Private 클러스터에 로그인해야 합니다.
 
## 1단계: 컨텐츠 식별
{: #identify_content}

분석할 해당 문서를 식별하십시오. {{site.data.keyword.cnc_short}}은 다음 조건에 맞는 계약 <!-- and regulatory -->문서를 분석하도록 설계되어 있습니다.

- 분석할 파일이 PDF 형식입니다.
- PDF 컨텐츠는 텍스트 양식입니다. 스캔이 광학 문자 인식(OCR)으로 처리된 경우에도 스캔된 문서를 구문 분석할 수 없습니다.

  **참고:** PDF 뷰어에서 문서를 열고 **텍스트 선택** 도구로 하나의 단어를 선택하여 텍스트로 된 PDF를 식별할 수 있습니다. 문서에서 하나의 단어를 선택할 수 없는 경우 파일을 구문 분석할 수 없습니다.

- 파일 크기가 50MB를 넘지 않습니다.
- 보안 PDF(열기 위해 비밀번호 사용) 및 편집 제한 PDF(편집을 위해 비밀번호 사용)를 구문 분석할 수 없습니다.

## 2단계: 계약 구문 분석
{: #parse_contract}

`bash` 쉘 또는 이와 동등한 환경(예: Cygwin)에서 `POST /v1/parse` 메소드를 사용하여 계약을 구문 분석하십시오. `{ICP_IP_address}`를 IBM Cloud Private 클러스터의 IP 주소로 대체하십시오. `{PDF_file}`을 구문 분석할 PDF에 대한 경로로 대체하십시오.

```bash
curl -k -X POST -F 'file=@{PDF_file};type=application/pdf' https://{ICP_IP_address}/api/v1/parse?version=2018-03-23
```
{: pre}

**중요:** {{site.data.keyword.cnc_short}} 1.0.4 전의 버전을 실행하는 경우, 서비스에 대한 호출을 작성할 때 다음과 같이 IBM Cloud Private 클러스터의 IP 주소와 함께 `:{port_number}` 지정자를 포함해야 합니다.

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://{ICP_IP_address}:{port_number}/api/v1/parse?version=2018-03-23
```
{: pre}

예를 들어, 다음과 같습니다.

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

세부사항은 [**ingress**의 릴리스 정보](/docs/services/compare-and-comply/relnotes.html#ingress)를 참조하십시오.

메소드는 다음이 포함된 JSON 오브젝트를 리턴합니다.

 - 소스 PDF 파일의 HTML 변환.
 - 압축을 푼 문서 제목.
 - 계약의 컴포넌트 파트를 정의하는 `elements`의 배열.
 - 당사자로 식별된 엔티티를 정의하는 `parties`의 배열.
 
**중요**: IBM Cloud Private의 {{site.data.keyword.cnc_short}} API에는 공용 IBM Cloud의 API와 같이 권한이 필요하지 않습니다.

## 3단계: 분석 검토
{: #review_analysis}

`elements` 배열의 각 오브젝트는 {{site.data.keyword.cnc_short}}이 식별한 계약의 요소를 설명합니다. 다음 코드는 일반 요소를 나타냅니다.

```
{
  "sentence_text": "If the parties are unable to agree on the License Price within thirty (30) days after Richmond Enterprises provides the written notice of its exercise of the Optional Patent License to Savage Narwhal Studios in Section 2.3(a), such FMV Dispute shall, at either party's request, be resolved solely and exclusively by final, binding and confidential arbitration to be filed and the decision rendered in New York, New York (with hearings at the request of either party to be held in San Francisco, California or other mutually agreeable place convenient for the parties) in accordance with the Commercial Arbitration Rules of the American Arbitration Association (\" AAA \"), including as supplemented by the Procedures for Large, Complex Commercial Disputes.",
  "attributes": [
	{
		"type": "Location",
		"text": "New York",
		"attribute": {
			"begin": 58372,
			"end": 58380
		}
	},
	{
		"type": "Location",
		"text": "New York",
		"attribute": {
			"begin": 58382,
			"end": 58390
		}
	},
	{
		"type": "Location",
		"text": "San Francisco",
		"attribute": {
			"begin": 58451,
			"end": 58464
		}
	},
	{
		"type": "Location",
		"text": "California",
		"attribute": {
			"begin": 58466,
			"end": 58476
		}
	}
],
"categories": [
	{
		"label": "Communication",
		"assurance": "High",
		"provenance": [
			{
				"id": "C7xhbsepUodh09zmJdUXSvYZCdixx00wFyCZuAnTujok="
			}
		]
	},
	{
		"label": "Dispute Resolution",
		"assurance": "High",
		"provenance": [
			{
				"id": "Ck8vgUWOj41OutOOLJ38b2Q7jOj3F30ABGaGLKKxppFA="
			},
			{
				"id": "CdPeg8mAxM5YIsdpzzaluDg7jOj3F30ABGaGLKKxppFA="
			}
		]
	},
	{
		"label": "Intellectual Property",
		"assurance": "High",
		"provenance": [
			{
				"id": "Cor/mgcf1UE/zmsKm68M6+a9LSRCpcKe8EWCUdwsjrgs="
			}
		]
	}
],
"types": [
	{
		"label": {
			"nature": "Obligation",
			"party": "All Parties"
		},
		"assurance": "High",
		"provenance": [
			{
				"id": "NNpSqaNkY2zHtxI6Zh78NaZccVNtutrQxISkzdnaVjx0="
			},
			{
				"id": "PlyERkjg5is36RpFjVUFXp69eDmGmCxLCXRs1sDMDUCo="
			}
		]
	}
],
"sentence": {
	"begin": 57998,
	"end": 58952
	}
}
```

요소에는 다음과 같은 다섯 가지 중요한 섹션이 있습니다.
 - `sentence_text`: 분석된 텍스트.
 - `attributes`: 요소의 속성을 하나 이상 나열하는 배열. `attributes` 배열에 현재 지원되는 오브젝트에는 `Location`(요소에서 참조되는 지리적 위치 또는 지역), `DateTime`(요소에서 지정된 날짜, 시간, 날짜 범위 또는 시간 범위) 및 `Currency`(화폐 가치 및 단위)가 포함됩니다. 
 - `categories`: 식별된 문장이 속하는 기능 카테고리(즉, 문장의 주제)가 나열된 배열.
 - `types`: 요소의 개념 및 이 요소의 영향을 받는 사용자를 설명하는 배열. 이는 하나 이상의 `nature` 키 세트(식별된 `party`에 대한 문자의 영향) 및 `party` 키(문장의 영향을 받는 사용자)로 구성됩니다.
 - `sentence`: 변환된 HTML에서 요소가 있는 위치를 설명하는 오브젝트. 여기에는 `start` 문자 값과 `end` 문자 값이 포함됩니다.

**참고**: 일부 문장은 어떤 유형이나 카테고리에도 속하지 않으며, 이 경우 서비스는 `types` 및 `categories` 배열을 빈 오브젝트로 리턴합니다.

**참고:** 일부 문장은 다중 주제를 포함하며, 이 경우 서비스는 여러 `types` 및 `categories` 오브젝트 세트를 리턴합니다.

**참고**: 일부 문장에는 식별 가능한 속성이 없으며, 이 경우 서비스는 `attributes` 배열을 빈 오브젝트로 리턴합니다.

또한 식별된 당사자는 `parties` 배열에 정의됩니다. JSON 출력에서 `parties` 배열은 `elements` 배열 뒤에 위치합니다.

```
  "parties" : [ {
    "party" : "Customer",
    "role" : "Buyer"
  } ]
```

`parties` 배열에는 다음과 같은 두 가지 중요한 섹션이 있습니다.

 - `party`: 문서 내에서 당사자로 식별된 텍스트.
 - `role`: 식별된 당사자의 역할. 하위 도메인에 따라 역할이 변경됩니다. [가능한 역할 목록에 지정된 하위 도메인에 대한 문서](/docs/services/compare-and-comply/parsing.html#contract_parties)를 참조하십시오. 특정 역할을 가진 것으로 식별할 수 없는 당사자는 `unknown` 값으로 나열됩니다.

## 다음 단계
{: #next_steps}

계약을 구문 분석하여 문서의 컴포넌트 파트에 대한 네이처, 당사자 및 카테고리를 식별했습니다. 분석을 사용하여 구문 분석된 계약을 빠르게 이해하고 적용할 수 있습니다. 다음 단계는 다음과 같습니다.

 - 유형 및 카테고리를 이해합니다.
 - 구문 분석 옵션을 검토합니다.


