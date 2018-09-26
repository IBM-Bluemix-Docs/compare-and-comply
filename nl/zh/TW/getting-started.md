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

# 開始使用
{: #getting_started}

在本簡短指導教學中，我們會介紹 IBM Cloud Private 上的 {{site.data.keyword.cnc_long}}，並透過剖析合約的處理程序，來識別元件片段、其本質、受影響的當事人，以及任何已識別的種類。

## 開始之前
{: #before-you-begin}

在可以使用 {{site.data.keyword.cnc_short}} 服務之前，您必須如[安裝 IBM Cloud Private CLI](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/install_cli.html) 所述，安裝 IBM Cloud Private CLI，並登入 IBM Cloud Private 叢集。
 
## 步驟 1：識別內容
{: #identify_content}

識別要分析的適當文件。{{site.data.keyword.cnc_short}} 已設計為分析符合下列準則的合約<!-- and regulatory -->文件：

- 要分析的檔案為 PDF 格式。
- PDF 內容是以文字形式表示。無法剖析已掃描的文件，即使掃描已經過光學字元讀取器 (OCR) 處理亦然。

  **附註：**您可以識別以文字表示的 PDF，方法為在 PDF 檢視器中開啟文件，然後使用**文字選取**工具來選取一個字。如果您無法在文件中選取一個字，則無法剖析檔案。

- 檔案的大小不超過 50Mb。
- 無法剖析安全 PDF（需要密碼才能開啟）和編輯受限 PDF（需要密碼才能編輯）。

## 步驟 2：剖析合約
{: #parse_contract}

在 `bash` Shell 或對等環境（例如 Cygwin）中，使用 `POST /v1/parse` 方法來剖析您的合約。將 `{ICP_IP_address}` 取代為 IBM Cloud Private 叢集的 IP 位址。將 `{PDF_file}` 取代為要剖析之 PDF 的路徑。

```bash
curl -k -X POST -F 'file=@{PDF_file};type=application/pdf' https://{ICP_IP_address}/api/v1/parse?version=2018-03-23
```
{: pre}

**重要事項：**如果您執行 1.0.4 之前的 {{site.data.keyword.cnc_short}} 版本，則必須在呼叫服務時，包含 `:{port_number}` 指定元與 IBM Cloud Private 叢集的 IP 位址，如下所示：

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://{ICP_IP_address}:{port_number}/api/v1/parse?version=2018-03-23
```
{: pre}

例如：

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

如需詳細資料，請參閱 [**ingress** 上的版本注意事項](/docs/services/compare-and-comply/relnotes.html#ingress)。

此方法會傳回 JSON 物件，其中包含：

 - 來源 PDF 檔案的 HTML 轉換。
 - 擷取的文件標題。
 - 定義合約之元件部分的 `elements` 陣列。
 - 定義已被識別為當事人之實體的 `parties` 陣列。
 
**重要事項**：與公用 IBM Cloud 上的 API 一樣，IBM Cloud Private 上的 {{site.data.keyword.cnc_short}} API 不需要授權。

## 步驟 3：檢閱分析
{: #review_analysis}

`elements` 陣列中的每一個物件說明 {{site.data.keyword.cnc_short}} 已識別之合約的元素。下列程式碼代表一般元素：

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

元素有五個重要區段：
  - `sentence_text`：已分析的文字。
  - `attributes`：列出一個以上元素屬性的陣列。`attributes` 陣列中目前支援的物件包括 `Location`（元素所參照的地理位置或地區）、`DateTime`（元素所指定的日期、時間、日期範圍或時間範圍），以及 `Currency`（必要的貨幣值與單位）。 
  - `categories`：列出識別句子所屬之功能種類的陣列；換言之，句子的主題。
  - `types`：說明何謂元素及其影響對象的陣列。它由一組以上的 `nature` 索引鍵（句子對所識別 `party` 的影響）及 `party` 索引鍵（句子影響對象）組成。
  - `sentence`：說明在已轉換 HTML 中找到元素之位置的物件。它包含 `start` 字元值及 `end` 字元值。

**附註**：部分句子不屬於任何類型或種類，在此情況下，服務會傳回 `types` 和 `categories` 陣列作為空物件。

**附註：**部分句子涵蓋多個主題，在此情況下，服務會傳回多組 `types` 和 `categories` 物件。

**附註**：部分句子不包含任何可識別的屬性，在此情況下，服務會傳回 `attributes` 陣列作為空物件。

此外，任何識別的當事人都會定義在 `parties` 陣列中。`parties` 陣列位於 JSON 輸出中的 `elements` 陣列後面。

```json
  "parties" : [ {
    "party" : "Customer",
    "role" : "Buyer"
  } ]
```

`parties` 陣列包括兩個重要區段：

  - `party`：在文件內被識別為當事人的文字。
  - `role`：所識別當事人的角色。角色根據子網域而變更；請參閱[所指定子網域的相關文件來取得可能角色的清單](/docs/services/compare-and-comply/parsing.html#contract_parties)。無法識別為具有特定角色的當事人會搭配 `unknown` 值列出。

## 後續步驟
{: #next_steps}

您已順利剖析合約，用來識別文件之元件部分的本質、當事人和種類。您可以使用分析，快速瞭解及施行已剖析的合約。後續步驟為：

 - [瞭解合約剖析](/docs/services/compare-and-comply/parsing.html#contract_parsing)
 - [瞭解輸出綱目](/docs/services/compare-and-comply/schema.html#output_schema)及[表格剖析](/docs/services/compare-and-comply/tables.html#understanding_tables)。


