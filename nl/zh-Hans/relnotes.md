
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

# 发行说明
{: #release_notes}

发行说明提供有关 {{site.data.keyword.BluOpenStackDed}} 上 {{site.data.keyword.cnc_long}} 服务发行版更改的信息。

**重要信息：**此文档集仅适用于 {{site.data.keyword.BluOpenStackDed}} 上的 {{site.data.keyword.cnc_short}} 服务。不适用于公共 IBM Cloud 上可用的其他 Watson 服务。

## 服务 API 版本控制
{: #api_versioning}

API 请求需要采用 `version=YYYY-MM-DD` 格式的日期的 version 参数。每当我们以向后不兼容方式更改 API 时，都会发布该 API 的新的次版本。

随每个 API 请求一起发送 version 参数。服务会使用您指定的日期的 API 版本，或使用该日期之前的最新版本。不要缺省为当前日期。请改为指定与应用程序兼容的版本的匹配日期，并且在应用程序准备好使用更高版本之前，不要对其进行更改。

当前版本为 `2018-03-23`。

## 更改
{: #changes}

为服务提供了以下新功能和更改。

**重要信息**：以下各部分中引用的版本号是您已在 {{site.data.keyword.BluOpenStackDed}} 集群上部署的 {{site.data.keyword.cnc_long}} Helm 图表的版本。

### 1.0.4（2018 年 7 月 5 日）
{: #ingress}

**重要信息**：由于本部分中描述的更改，无法使用标准 {{site.data.keyword.BluOpenStackDed}} 升级过程将 {{site.data.keyword.cnc_short}} 从先前版本升级到 V1.0.4。必须重新部署 {{site.data.keyword.cnc_short}}，如下所示：

1.  除去现有 {{site.data.keyword.cnc_short}} 部署，如[除去部署 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_applications/remove_app.html){: new_window} 中所述。

1.  部署 {{site.data.keyword.cnc_short}} V1.0.4 或更高版本，如 {{site.data.keyword.cnc_short}} Helm 图表的 `README.md` 文件中和[部署服务](/docs/services/compare-and-comply/deploy.html)中所述。

**重要信息**：重新部署服务会产生以下后果：

- 服务在除去先前实例与部署当前实例这两个操作之间不可用。
- 如果您有代码用于发出 API 调用，并且在调用的 URL 中包含端口号，那么必须除去这些端口号。如果向 {{site.data.keyword.cnc_short}} V1.0.4 或更高版本发出包含端口号的 API 调用，那么调用会失败，并生成类似于以下内容的错误：
  ```
  curl: (7) Failed to connect to 10.19.74.45 port 8443: Connection refused
  ```
  {: pre}

**重要信息**：如果运行的 {{site.data.keyword.cnc_short}} 版本早于 1.0.4，那么在调用该服务时，必须在 {{site.data.keyword.BluOpenStackDed}} 集群的 IP 地址中包含 `:{port_number}` 说明符，如以下示例中所示：
```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

#### 其他说明

-   现在，在[部署服务](/docs/services/compare-and-comply/deploy.html)上提供了更多部署指示信息。
-   现在，{{site.data.keyword.cnc_short}} 服务与 {{site.data.keyword.BluOpenStackDed}} 的 [Ingress 控制器 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/components.html){: new_window} 相集成。此更改支持在调用 URL 中不指定端口号的情况下调用服务的 API 方法。下面是 **Ingress** 集成之前的典型 API 调用：

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
    ```
    {: pre}

  在 V1.0.4 或更高版本中，等效调用为：

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45/api/v1/parse?version=2018-03-23
    ```
    {: pre}

- 部署 1.0.4 发行版时，可能会看到类似于以下内容的错误：

    ```
    Unexpected response code 500 from request: GET https://icp-management-ingress:8443/helm-api/api/v1/releases/weblib?locale=en-US HTTP/1.1 Accept: application/json Content-Type: application/json Content-Type: application/json Cookie: *** {} HTTP/1.1 500 Server: openresty/1.11.2.4 Date: Thu, 21 Jun 2018 23:09:05 GMT Content-Type: application/json; charset=utf-8 Content-Length: 93 Connection: close X-Powered-By: Express Cache-Control: private, max-age=0, no-cache Etag: W/"5d-6htOwRqeFWIdvat6vK/kTg" {"statusCode":500,"message":"Internal service error : Cannot read property '0' of undefined"}
    ```
    {: codeblock}

    您可以安全地忽略并关闭此错误。

### 1.0.3（2018 年 5 月 25 日）

- 现在，`parse` 方法的输出包含 `attributes` 数组。有关信息，请参阅[查看分析](/docs/services/compare-and-comply/getting-started.html#review_analysis)和 [Attributes](/docs/services/compare-and-comply/parsing.html#attributes)。可以使用 `attributes` 数组中的信息来搜索用于指以下信息的文档元素：特定位置；时间、日期、时间范围或日期范围；以及货币值和单位。
- `types` 和 `categories` 数组中的每个对象都包含一个 `provenance` 对象。`provenance` 对象有一个或多个 `id` 键。每个 `id` 键都有一个散列值，可以将该散列值发送给 IBM 以提供反馈或接收支持。
- 改进了 PDF 解析的准确性以及解析大型 PDF 文件的性能。
- 此发行版在 {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 或更高版本上可用。有关 {{site.data.keyword.BluOpenStackDed}} 需求的详细信息，请参阅该服务的目录条目。
- `assurance` 键的可能值不再包括 `Low`。

### 1.0.2（2018 年 4 月 19 日）

- 支持更多类别。有关支持的类别的最新列表，请参阅 [Categories 文档](/docs/services/compare-and-comply/parsing.html#contract_categories)。
    - `资产使用`
    - `通信`
    - `安全和保障`
-  文档更新，包括对[入门](/docs/services/compare-and-comply/getting-started.html)中示例的更正。

### 1.0.1（一般可用性发行版），2018 年 3 月 23 日

以下说明适用于 {{site.data.keyword.cnc_long}} 服务的一般可用性 (GA) 发行版。

- GA 发行版仅在 {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 或更高版本上可用。有关 {{site.data.keyword.BluOpenStackDed}} 需求的详细信息，请参阅该服务的目录条目。

## 一般说明
{: #general_notes}

- {{site.data.keyword.BluOpenStackDed}} 上的 {{site.data.keyword.cnc_short}} API 不用像公共 IBM Cloud 上的 API 那样需要授权。
 - PDF 必须为文本格式才能进行解析。无法解析扫描的文档，即使扫描内容已由光学字符阅读器 (OCR) 进行处理。

## 已知问题
{: #known_issues}

- 可上传的 PDF 文件的最大大小为 50 MB。
- 无法解析启用了安全性的 PDF。
- 无法正确解析采用非标准页面布局（例如，每页 2 列或 3 列）的文档。
