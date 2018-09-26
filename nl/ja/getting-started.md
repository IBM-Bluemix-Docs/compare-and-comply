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

# はじめに
{: #getting_started}

このショート・チュートリアルでは、IBM Cloud Private 上の {{site.data.keyword.cnc_long}} を紹介するとともに、契約を解析するプロセスの手順を追いながら、構成要素断片、それらの性質、影響を受けるパーティー、識別されるカテゴリーを識別します。

## 始めに
{: #before-you-begin}

{{site.data.keyword.cnc_short}} サービスを使用するには、その前に [IBM Cloud Private CLI のインストール](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/install_cli.html)の説明に従って、IBM Cloud Private CLI をインストールして IBM Cloud Private クラスターにログインする必要があります。
 
## ステップ 1: 内容の識別
{: #identify_content}

分析する適切な文書を識別します。 {{site.data.keyword.cnc_short}} は、以下の基準を満たす契約<!-- and regulatory -->書を分析するように設計されています。

- 分析されるファイルは PDF 形式である。
- PDF コンテンツはテキスト形式である。 スキャンされた文書は解析できません。スキャンが光学式文字読取装置 (OCR) で処理されたとしても同様です。

  **注:** テキスト形式の PDF を識別するには、文書を PDF ビューアーで開き、**テキスト選択**ツールを使用して単語を 1 つ選択します。 文書内で 1 つの単語を選択できない場合、そのファイルは解析できません。

- ファイルのサイズは 50 MB 以下です。
- 保護された PDF (開くためにパスワードが必要) と編集制限付きの PDF (編集するためにパスワードが必要) は解析できません。

## ステップ 2: 契約の解析
{: #parse_contract}

`bash` シェルまたはそれと同等の Cygwin などの環境で、`POST /v1/parse` メソッドを使用して契約を解析します。 `{ICP_IP_address}` は、IBM Cloud Private クラスターの IP アドレスに置き換えます。 `{PDF_file}` は、解析する PDF のパスに置き換えます。

```bash
curl -k -X POST -F 'file=@{PDF_file};type=application/pdf' https://{ICP_IP_address}/api/v1/parse?version=2018-03-23
```
{: pre}

**重要:** バージョン 1.0.4 より前の {{site.data.keyword.cnc_short}} を実行している場合は、サービスの呼び出しを行うときに、次のように IBM Cloud Private クラスターの IP アドレスとともに `:{port_number}` 指定子を含める必要があります。

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://{ICP_IP_address}:{port_number}/api/v1/parse?version=2018-03-23
```
{: pre}

以下に例を示します。

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

詳しくは、[**ingress** のリリース・ノート](/docs/services/compare-and-comply/relnotes.html#ingress)を参照してください。

このメソッドで返される JSON オブジェクトには以下のものが含まれます。

 - ソース PDF ファイルを HTML 変換した文書。
 - 抽出された文書タイトル。
 - 契約の構成要素パーツを定義する `elements` の配列。
 - パーティーとして識別されたエンティティーを定義する `parties` の配列。
 
**重要**: IBM Cloud Private 上の {{site.data.keyword.cnc_short}} API では、パブリック IBM Cloud 上の API の場合と異なり、許可は不要です。

## ステップ 3: 分析のレビュー
{: #review_analysis}

`elements` 配列内の各オブジェクトは、契約の中で {{site.data.keyword.cnc_short}} が識別したエレメントを記述します。 以下のコードは、標準的なエレメントを表しています。

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

このエレメントには、以下の 5 つの重要なセクションがあります。
  - `sentence_text`: 分析されたテキスト。
  - `attributes`: エレメントの 1 つ以上の属性をリストした配列。 現在サポートされている `attributes` 配列内のオブジェクトには、`Location` (エレメントによって参照されている地理的位置または地域)、`DateTime` (エレメントによって明示されている日付、時刻、日付範囲、または時刻範囲)、`Currency` (貨幣価値と通貨単位) が含まれます。 
  - `categories`: 識別されたセンテンスが分類される機能カテゴリー、つまりセンテンスの主題をリストした配列。
  - `types`: エレメントの種類とその影響を受ける対象者を記述した配列。 `nature` キー (識別された `party` へのセンテンスの影響) と `party` キー (センテンスの影響が及ぶ対象者) のセットが 1 つ以上あります。
  - `sentence`: 変換された HTML の中でエレメントが見つかった場所を記述するオブジェクト。 `start` 文字値と `end` 文字値が含まれます。

**注**: センテンスによっては、どのようなタイプにもカテゴリーにも分類されないものもあります。その場合、このサービスは、`types` 配列と `categories` 配列を空オブジェクトとして返します。

**注:** センテンスによっては、複数のトピックを扱っているものもあります。その場合、このサービスは、`types` オブジェクトと `categories` オブジェクトのセットを複数返します。

**注**: センテンスによっては、識別可能な属性を含んでいないものもあります。その場合、このサービスは、`attributes` 配列を空オブジェクトとして返します。

さらに、識別されたパーティーは `parties` 配列で定義されます。 `parties` 配列は、JSON 出力内の `elements` 配列の後にあります。

```json
  "parties" : [ {
    "party" : "Customer",
    "role" : "Buyer"
  } ]
```

`parties` 配列には、次の 2 つの重要なセクションが含まれます。

  - `party`: 文書内でパーティーとして識別されたテキスト。
  - `role`: 識別されたパーティーの役割。 役割はサブドメイン・ベースで扱われるように変更されました。候補となる役割のリストについては、[示されるサブドメインに関する資料](/docs/services/compare-and-comply/parsing.html#contract_parties)を参照してください。 特定の役割を持っているパーティーとして識別できないパーティーの場合、この値は `unknown` になります。

## 次のステップ
{: #next_steps}

契約を解析して、文書の構成要素パーツの性質、パーティー、カテゴリーを識別することに成功しました。 この分析を使用することにより、解析された契約を迅速に理解して施行することができます。 次のステップは以下のとおりです。

 - [契約解析について](/docs/services/compare-and-comply/parsing.html#contract_parsing)
 - [出力スキーマについて](/docs/services/compare-and-comply/schema.html#output_schema)および[表解析について](/docs/services/compare-and-comply/tables.html#understanding_tables)。


