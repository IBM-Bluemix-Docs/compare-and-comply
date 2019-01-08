---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Converting a PDF, Word, image, or text document into HTML
{: #html_conversion}

You can transform an [input document](/docs/services/compare-and-comply/formats.html#formats)(PDF, Word, image, text) into HTML by using the `POST /v1/html_conversion` method.

In a `bash` shell or equivalent environment such as Cygwin, use the `POST /v1/html_conversion` method to convert an input document into HTML. The method takes the following input parameters:
  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
  - `file` (**required** `file`): The input file that is to be converted into HTML.
  - `model` (optional `string`): If this parameter is specified, the service runs the specified type of element classification. Currently, the only supported value is `contracts`.

You can specify the response content type to return the converted HTML in either JSON (the default) or raw HTML. See the examples following the command example for the different formats.
  - To return JSON explicitly, specify the header `-H "Accept: application/json"`. This is the default.
  - To return raw HTML, specify the header `-H "Accept: text/html"`.

Replace `{apikey_value}` with the API key you copied earlier and `{input_file}` with the path to the input file to convert.

```bash
curl -X POST -u "apikey":"{apikey_value}" -H "Accept: application/json"
-F "file=@{input_file}" https://{cluster_CA_domain}/{deployment_name}/compare-and-comply/api/v1/html_conversion?version=2018-10-15
```
{: pre}

In this example, the header `-H "Accept: application/json"` returns output in the following format:

```
{
  "num_pages": "18",
  "author": "SOME_USER",
  "publication_date": "2017-04-04",
  "title": "no title",
  "html": "<?xml version='1.0' encoding='UTF-8' standalone='yes'?><html>...</html>"
}
```
{: screen}

If you specify `-H "Accept: text/html"`, the service returns only the contents of the `"html"` field from the previous output example:

```
<?xml version='1.0' encoding='UTF-8' standalone='yes'?><html>...</html>
```
{: screen}
