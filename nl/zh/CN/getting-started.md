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

# 入门
{: #getting_started}

在此简短教程中，我们将介绍 IBM Cloud Private 上的 {{site.data.keyword.cnc_long}}，并了解解析合同的整个过程，以识别组成部分、其性质、受影响的当事方以及任何识别到的类别。

## 开始之前
{: #before-you-begin}

要能够使用 {{site.data.keyword.cnc_short}} 服务，必须先安装 IBM Cloud Private CLI 并登录到 IBM Cloud Private 集群，如[安装 IBM Cloud Private CLI](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/install_cli.html) 中所述。
 
## 步骤 1：确定内容
{: #identify_content}

确定要分析的相应文档。{{site.data.keyword.cnc_short}} 旨在分析符合以下条件的合同<!-- and regulatory -->文档：

- 要分析的文件采用 PDF 格式。
- PDF 内容以文本格式显示。无法解析扫描的文档，即使扫描内容已由光学字符阅读器 (OCR) 进行处理。

  **注：**您可以通过在 PDF 查看器中打开文档，并使用**文本选择**工具来选择单个词，从而确定 PDF 是否为文本。如果无法在文档中选择单个词，那么无法解析该文件。

- 文件大小不超过 50 MB。
- 无法解析安全 PDF（使用密码才能打开）和编辑受限的 PDF（使用密码才能编辑）。

## 步骤 2：解析合同
{: #parse_contract}

在 `bash` shell 或等效环境（如 Cygwin）中，使用 `POST /v1/parse` 方法来解析合同。将 `{ICP_IP_address}` 替换为 IBM Cloud Private 集群的 IP 地址。将 `{PDF_ file}` 替换为要解析的 PDF 的路径。

```bash
curl -k -X POST -F 'file=@{PDF_file};type=application/pdf' https://{ICP_IP_address}/api/v1/parse?version=2018-03-23
```
{: pre}

**重要信息：**如果运行的 {{site.data.keyword.cnc_short}} 版本早于 1.0.4，那么在调用该服务时，必须在 IBM Cloud Private 集群的 IP 地址中包含 `:{port_number}` 说明符，如下所示：

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://{ICP_IP_address}:{port_number}/api/v1/parse?version=2018-03-23
```
{: pre}

例如：


```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

有关详细信息，请参阅 [**Ingress** 发行说明](/docs/services/compare-and-comply/relnotes.html#ingress)。

此方法会返回包含以下内容的 JSON 对象：

 - 源 PDF 文件的 HTML 转换。
 - 抽取的文档标题。
 - `elements` 数组，用于定义合同的组成部分。
 - `parties` 数组，用于定义识别为当事方的实体。
 
**重要信息**：IBM Cloud Private 上的 {{site.data.keyword.cnc_short}} API 不用像公共 IBM Cloud 上的 API 那样需要授权。

## 步骤 3：查看分析
{: #review_analysis}

`elements` 数组中的每个对象描述了 {{site.data.keyword.cnc_short}} 识别到的一个合同元素。以下代码表示典型元素：

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

该元素具有五个重要部分：
 - `sentence_text`：分析的文本。
 - `attributes`：一个数组，用于列出元素的一个或多个属性。`attributes` 数组中当前支持的对象包括 `Location`（元素引用的地理位置或区域）、`DateTime`（元素指定的日期、时间、日期范围或时间范围）和 `Currency`（货币值和单位）。 
 - `categories`：一个数组，用于列出识别到的语句所属的功能类别；换言之，这是语句的主题。
 - `types`：一个数组，用于描述元素的定义及元素影响的对象。它由一组或多组 `nature` 键（语句对识别到的 `party` 的影响）和 `party` 键（语句影响的对象）组成。
 - `sentence`：一个对象，用于描述在已转换 HTML 中找到元素的位置。它包含 `start` 字符值和 `end` 字符值。

**注**：某些语句不属于任何类型或类别，在这种情况下，服务会将 `types` 和 `categories` 数组作为空对象返回。

**注**：某些语句涵盖多个主题，在这种情况下，服务会返回多组 `types` 和 `categories` 对象。

**注**：某些语句不包含任何可识别的属性，在这种情况下，服务会将 `attributes` 数组作为空对象返回。

此外，在 `parties` 数组中定义了任何识别到的当事方。在 JSON 输出中，`parties` 数组位于 `elements` 数组后面。

```
  "parties" : [ {
    "party" : "Customer",
    "role" : "Buyer"
  } ]
```

`parties` 数组包含两个重要部分：

 - `party`：在文档中被识别为当事方的文本。
 - `role`：识别到的当事方的角色。角色已根据子域进行更改；请参阅[有关指定子域的文档，以获取可能角色的列表](/docs/services/compare-and-comply/parsing.html#contract_parties)。无法识别为具有特定角色的当事方在列出时会带有 `unknown` 值。

## 后续步骤
{: #next_steps}

您已成功解析合同来识别文档各组成部分的性质、当事方和类别。您可以使用分析来快速了解和强制实施已解析的合同。后续步骤包括：

 - 了解类型和类别。
 - 查看解析选项。


