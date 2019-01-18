---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-14"

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

# Supported input formats
{: #formats}

IBM Watson&reg; Compare and Comply supports a variety of input formats, including PDF, JSON, Microsoft Word, images, and text.
{: shortdesc}

## General support
{: #general}

Observe the following notes regarding files submitted to Compare and Comply.

  - The maximum size of a file you can submit to the service is 50 MB. However, if you submit multiple files simultaneously or in close succession, you can experience relatively long processing times, depending on the capacity of your IBM Cloud Private cluster.
  - Documents with non-standard page layouts (such as 2 or 3 columns per page) do not parse correctly.

## PDF support
{: #pdfs}

Compare and Comply can process PDF files. Note the following.

  - The MIME type is `application/pdf`.
  - Both programmatic and scanned PDF files are supported. Files that have been scanned and processed by an optical character reader (OCR) are also supported.
  - Secure PDFs, which require a password to open, and restricted PDFs, which require a password to edit, cannot be processed.

## Microsoft Word support
{: #word}

Compare and Comply can process Microsoft Word files in the following formats.
  - DOC (`application/msword`)
  - DOCX (`application/vnd.openxmlformats-officedocument.wordprocessingml.document`)

## Image support
{: #images}

  - BMP (`image/bmp`)
  - GIF (`image/gif`)
  - JPEG (`image/jpeg`)
  - JPEG2000 (`image/jpeg`)
  - PNG (`image/png`)
  - RAW (no specific MIME type)
  - TIFF (`image/tiff`)
  
For best results, use image files with a resolution of 300 DPI or higher. Using image files with a resolution under 300 DPI can result in non-optimal output.
{: note}

## Text support
{: #text}

The service can process "plain" text (ASCII) files that use a monospaced font and page breaks. Richer text formats that include non-monospaced fonts and style attributes such as bold and italics are not yet supported. If you need to process an enriched text file, convert it to PDF before submitting it to the service.

The MIME type is `text/plain`.

## Support by method
{: #methods}

The service's methods can accept different types of files as specified in the following table.

| Method           |PDF/Word support    |Image support             |Text support        
|------------------|-----------------|-----------------------------------------|
|`/v1/html_conversion`| Supported | All supported image formats | Supported |
|`/v1/element_classification`| Supported | All supported image formats | **Not** supported|
|`/v1/tables`      | Supported | All supported image formats | Supported |
|`/v1/comparison`*  | Supported | All supported image formats | **Not** supported |

\*The `/v1/comparison` method also accepts JSON files from the output of the `/v1/element_classification` method.
{: note}
