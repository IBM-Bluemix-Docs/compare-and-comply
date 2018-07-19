
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

# 版本注意事項
{: #release_notes}

版本注意事項提供 {{site.data.keyword.BluOpenStackDed}} 上 {{site.data.keyword.cnc_long}} 服務版本之變更的相關資訊。

**重要事項：**此文件集僅適用於 {{site.data.keyword.BluOpenStackDed}} 上的 {{site.data.keyword.cnc_short}} 服務。不適用於公用 IBM Cloud 上所提供的其他 Watson 服務。

## 服務 API 版本化
{: #api_versioning}

API 要求需要版本參數接受 `version=YYYY-MM-DD` 格式的日期。每當我們以舊版不相容方式變更 API，就會發行 API 的新次要版本。

每個 API 要求都會傳送版本參數。服務會使用所指定日期的 API 版本，或該日期之前的最新版本。不要預設為現行日期。反之，指定一個日期，符合與您應用程式相容的版本，而且不要變更它，直到您的應用程式備妥，可供更新版本使用為止。

現行版本為 `2018-03-23`。

## 變更
{: #changes}

提供服務的下列新特性及變更。

**重要事項**：在下列各節中提到的版本號碼是您已部署在 {{site.data.keyword.BluOpenStackDed}} 叢集上之 {{site.data.keyword.cnc_long}} Helm 圖表的版本。

### 1.0.4，2018 年 7 月 5 日
{: #ingress}

**重要事項**：因為本節中所說明的變更，所以您無法使用標準 {{site.data.keyword.BluOpenStackDed}} 升級程序，從舊版 {{site.data.keyword.cnc_short}} 升級至 1.0.4 版。您必須重新部署 {{site.data.keyword.cnc_short}}，如下所示：

1.  如[移除部署 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_applications/remove_app.html){: new_window} 所述，移除現有的 {{site.data.keyword.cnc_short}} 部署。

1.  如 {{site.data.keyword.cnc_short}} Helm 圖表的 `README.md` 檔案及如[部署服務](/docs/services/compare-and-comply/deploy.html)所述，部署 {{site.data.keyword.cnc_short}} 1.0.4 版或更新版本。

**重要事項：**重新部署服務會有下列結果：

- 在移除前一個實例與部署現行實例之間無法使用服務。
- 如果您有程式碼發出 API 呼叫，其中包括呼叫 URL 中的埠號，則必須移除埠號。如果您對 {{site.data.keyword.cnc_short}} 1.0.4 版或更新版本發出包括埠號的 API 呼叫，則呼叫會失敗，因為發生類似下列的錯誤：
  ```
  curl: (7) Failed to connect to 10.19.74.45 port 8443: Connection refused
  ```
  {: pre}

**重要事項：**如果您執行的是 1.0.4 之前的 {{site.data.keyword.cnc_short}} 版本，則必須在呼叫服務時，包括 `:{port_number}` 指定元與 {{site.data.keyword.BluOpenStackDed}} 叢集的 IP 位址，如下列範例所示：
```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

#### 其他附註

-   現在，已在[部署服務](/docs/services/compare-and-comply/deploy.html)時提供其他部署指示。
-   {{site.data.keyword.cnc_short}} 服務現在與 {{site.data.keyword.BluOpenStackDed}} 的 [Ingress 控制器 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/components.html){: new_window} 整合。此變更可讓您呼叫服務的 API 方法，而不必在呼叫的 URL 中指定埠號。以下是在 **Ingress** 整合之前的一般 API 呼叫：

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
    ```
    {: pre}

  1.0.4 版或更新版本中的對等呼叫如下：

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45/api/v1/parse?version=2018-03-23
    ```
    {: pre}

- 當您部署 1.0.4 版時，可能會看到與下列類似的錯誤：

    ```
    Unexpected response code 500 from request: GET https://icp-management-ingress:8443/helm-api/api/v1/releases/weblib?locale=en-US HTTP/1.1 Accept: application/json Content-Type: application/json Content-Type: application/json Cookie: *** {} HTTP/1.1 500 Server: openresty/1.11.2.4 Date: Thu, 21 Jun 2018 23:09:05 GMT Content-Type: application/json; charset=utf-8 Content-Length: 93 Connection: close X-Powered-By: Express Cache-Control: private, max-age=0, no-cache Etag: W/"5d-6htOwRqeFWIdvat6vK/kTg" {"statusCode":500,"message":"Internal service error : Cannot read property '0' of undefined"}
    ```
    {: codeblock}

    您可以放心地不處理並關閉錯誤。

### 1.0.3，2018 年 5 月 25 日

- `parse` 方法的輸出現在包括 `attributes` 陣列。如需相關資訊，請參閱[檢閱分析](/docs/services/compare-and-comply/getting-started.html#review_analysis)及[屬性](/docs/services/compare-and-comply/parsing.html#attributes)。您可以使用 `attributes` 陣列中的資訊，來搜尋參照特定位置的文件元素；時間、日期、時間範圍或日期範圍；以及貨幣值和單位。
- `types` 及 `categories` 陣列中的每一個物件都包括一個 `provenance` 物件。`provenance` 物件具有一個以上的 `id` 索引鍵。每一個 `id` 索引鍵都有一個雜湊值，您可以將其傳送給 IBM，以提供意見或接收支援。
- 改善 PDF 剖析的精確度，以及剖析大型 PDF 檔案的效能。
- 此版本可在 {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 或更新版本上使用。如需 {{site.data.keyword.BluOpenStackDed}} 需求的詳細資料，請參閱服務的型錄項目。
- `assurance` 索引鍵的可能值不再包括 `Low`。

### 1.0.2，2018 年 4 月 19 日

- 其他支援的種類。如需受支援種類的最新清單，請參閱[種類文件](/docs/services/compare-and-comply/parsing.html#contract_categories)。
    - `Asset Use`
    - `Communication`
    - `Safety and Security`
-  文件更新，包括[開始使用](/docs/services/compare-and-comply/getting-started.html)中範例的更正。

### 1.0.1（通用版），2018 年 3 月 23 日

下列注意事項適用於 {{site.data.keyword.cnc_long}} 服務的「通用版 (GA)」。

- GA 版只能在 {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 或更新版本上使用。如需 {{site.data.keyword.BluOpenStackDed}} 需求的詳細資料，請參閱服務的型錄項目。

## 一般注意事項
{: #general_notes}

- 與公用 IBM Cloud 上的 API 一樣，{{site.data.keyword.BluOpenStackDed}} 上的 {{site.data.keyword.cnc_short}} API 不需要授權。
 - PDF 必須是文字格式才能進行剖析。無法剖析已掃描的文件，即使光學字元讀取器 (OCR) 已處理掃描。

## 已知問題
{: #known_issues}

- 可以上傳的 PDF 檔案大小上限是 50Mb。
- 無法剖析已啟用安全的 PDF。
- 具有非標準頁面佈置（例如每頁 2 或 3 個直欄）的文件無法正確剖析。
