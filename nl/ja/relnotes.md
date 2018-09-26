
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

# リリース・ノート
{: #release_notes}

このリリース・ノートでは、{{site.data.keyword.BluOpenStackDed}} 上の {{site.data.keyword.cnc_long}} サービス・リリースへの変更点について説明します。

**重要:** この資料セットは、{{site.data.keyword.BluOpenStackDed}} 上の {{site.data.keyword.cnc_short}} サービスにのみ適用されます。 パブリック IBM Cloud 上で使用できるその他の Watson サービスには適用されません。

**注:** すべての {{site.data.keyword.BluOpenStackDed}} サービスに関係するリリース・ノート情報については、[https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/known_issues.html#issues_nlv ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/known_issues.html#issues_nlv){: new_window} を参照してください。

## サービス API のバージョン管理
{: #api_versioning}

API 要求には、`version=YYYY-MM-DD` という形式で日付を設定する version パラメーターが必要です。 後方互換性のない方法で API が変更されると、API の新規マイナー・バージョンがリリースされます。

API 要求のたびに version パラメーターを送信してください。 本サービスでは、指定された日付の API バージョン、またはその日付より前の最新のバージョンが使用されます。 現在日付をデフォルトに設定しないでください。 代わりに、ご使用のアプリケーションと互換性のあるバージョンに一致する日付を指定し、より新しいバージョン用にアプリケーションの準備ができるまでその日付を変更しないでください。

現在のバージョンは `2018-03-23` です。

## 変更内容
{: #changes}

本サービスに対する以下の新機能および変更点があります。

**重要**: 以下のセクションで言及されているバージョン番号は、{{site.data.keyword.BluOpenStackDed}} クラスターにデプロイした {{site.data.keyword.cnc_long}} Helm chart のバージョンです。

### 1.0.5、2018 年 8 月 2 日
{: #105}

  - [出力スキーマについて](/docs/services/compare-and-comply/schema.html#output_schema)および[表解析について](/docs/services/compare-and-comply/tables.html#understanding_tables)で説明されている、表解析の追加。


### 1.0.4、2018 年 7 月 5 日
{: #ingress}

**重要**: このセクションで説明されている変更により、{{site.data.keyword.BluOpenStackDed}} の標準的なアップグレード手順では、以前のバージョンの {{site.data.keyword.cnc_short}} からバージョン 1.0.4 にアップグレードすることはできません。 以下のようにして {{site.data.keyword.cnc_short}} を再デプロイする必要があります。

1.  [デプロイメントの削除 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_applications/remove_app.html){: new_window} の説明に従って既存の {{site.data.keyword.cnc_short}} デプロイメントを削除します。

1.  {{site.data.keyword.cnc_short}} Helm chart の `README.md` ファイルおよび[サービスのデプロイ](/docs/services/compare-and-comply/deploy.html)の説明に従って、{{site.data.keyword.cnc_short}} バージョン 1.0.4 以降をデプロイします。

**重要:** サービスの再デプロイによって以下のような影響があります。

- 以前のインスタンスの削除から現在のインスタンスのデプロイメントまでの間は、このサービスを利用できません。
- 呼び出しの URL にポート番号が含まれている API 呼び出しを行うコードがある場合は、ポート番号を削除する必要があります。 ポート番号が含まれている API 呼び出しを {{site.data.keyword.cnc_short}} バージョン 1.0.4 以降に対して行うと、呼び出しは以下のようなエラーで失敗します。
  ```
  curl: (7) Failed to connect to 10.19.74.45 port 8443: Connection refused
  ```
  {: pre}

**重要:** バージョン 1.0.4 より前の {{site.data.keyword.cnc_short}} を実行している場合は、サービスの呼び出しを行うときに、次の例のように {{site.data.keyword.BluOpenStackDed}} クラスターの IP アドレスとともに `:{port_number}` 指定子を含める必要があります。
```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

#### 追加のメモ

-   [サービスのデプロイ](/docs/services/compare-and-comply/deploy.html)に、追加のデプロイメントの説明があります。
-   {{site.data.keyword.cnc_short}} サービスは {{site.data.keyword.BluOpenStackDed}} の [ingress コントローラー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/components.html){: new_window} に統合されました。 この変更によって、呼び出しの URL でポート番号を指定しなくても、サービスの API メソッドを呼び出せるようになりました。 **ingress** 統合前の典型的な API 呼び出しは以下のようなものです。

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
    ```
    {: pre}

  バージョン 1.0.4 以降では、同等の呼び出しは以下のようになります。

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45/api/v1/parse?version=2018-03-23
    ```
    {: pre}

- 1.0.4 リリースをデプロイすると、次のようなエラーが表示される可能性があります。

    ```
    Unexpected response code 500 from request: GET https://icp-management-ingress:8443/helm-api/api/v1/releases/weblib?locale=en-US HTTP/1.1 Accept: application/json Content-Type: application/json Content-Type: application/json Cookie: *** {} HTTP/1.1 500 Server: openresty/1.11.2.4 Date: Thu, 21 Jun 2018 23:09:05 GMT Content-Type: application/json; charset=utf-8 Content-Length: 93 Connection: close X-Powered-By: Express Cache-Control: private, max-age=0, no-cache Etag: W/"5d-6htOwRqeFWIdvat6vK/kTg" {"statusCode":500,"message":"Internal service error : Cannot read property '0' of undefined"}
    ```
    {: codeblock}

    このエラーは無視しても構いません。

### 1.0.3、2018 年 5 月 25 日

- `parse` メソッドの出力に `attributes` 配列が含まれるようになりました。 詳しくは、[分析のレビュー](/docs/services/compare-and-comply/getting-started.html#review_analysis)と[属性](/docs/services/compare-and-comply/parsing.html#attributes)を参照してください。 `attributes` 配列にある情報を利用して、特定のロケーションに言及する文書エレメント、時刻、日付、時刻範囲、または日付範囲に言及する文書エレメント、貨幣価値と通貨単位に言及する文書エレメントを検索できます。
- `types` 配列および `categories` 配列内の各オブジェクトには `provenance` オブジェクトが含まれます。 `provenance` オブジェクトには 1 つ以上の `id` キーがあります。 それぞれの `id` キーにはハッシュ値があり、これを IBM に送信して、フィードバックを送信したりサポートを受けたりすることができます。
- PDF 解析の正確度、および大容量 PDF ファイルの解析パフォーマンスが改善されました。
- このリリースは {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 以上で使用できます。 {{site.data.keyword.BluOpenStackDed}} の要件について詳しくは、本サービスのカタログ項目を参照してください。
- `assurance` キーの候補となる値に `Low` が含まれなくなりました。

### 1.0.2、2018 年 4 月 19 日

- 追加のサポート対象カテゴリー。 最新のサポート対象カテゴリーのリストについては、[カテゴリー文書](/docs/services/compare-and-comply/parsing.html#contract_categories)を参照してください。
    - `Asset Use`
    - `Communication`
    - `Safety and Security`
-  [はじめに](/docs/services/compare-and-comply/getting-started.html)にある例の修正を含む文書の更新。

### 1.0.1 (一般出荷版リリース)、2018 年 3 月 23 日

以下の注意事項は、{{site.data.keyword.cnc_long}} サービスの一般出荷版 (GA) リリースに当てはまります。

- GA リリースは {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 以上でのみ使用できます。 {{site.data.keyword.BluOpenStackDed}} の要件について詳しくは、本サービスのカタログ項目を参照してください。

## 一般的な注意事項
{: #general_notes}

- {{site.data.keyword.BluOpenStackDed}} 上の {{site.data.keyword.cnc_short}} API では、パブリック IBM Cloud 上の API の場合と異なり、許可は不要です。
 - PDF を解析するにはテキスト・フォーマットにする必要があります。 スキャンされた文書は解析できません。スキャンが光学式文字読取装置 (OCR) で処理されたとしても同様です。

## 既知の問題
{: #known_issues}

- アップロード可能な PDF ファイルの最大サイズは 50Mb です。
- セキュリティー機能が有効になっている PDF は解析できません。
- 標準的なページ・レイアウトになっていない文書 (各ページに 2 列から 3 列あるような文書) は、正しく解析できません。
