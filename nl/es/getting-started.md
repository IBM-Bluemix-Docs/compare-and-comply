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

# Iniciación
{: #getting_started}

En esta breve guía de aprendizaje, introducimos {{site.data.keyword.cnc_long}} en IBM Cloud Private y analizamos el proceso de análisis de un contrato para identificar partes de componentes, su naturaleza, las partes afectadas y cualquier categoría identificada.

## Antes de empezar
{: #before-you-begin}

Antes de poder utilizar el servicio de {{site.data.keyword.cnc_short}}, debe instalar la CLI de IBM Cloud Private e iniciar sesión en el clúster de IBM Cloud Private tal como se describe en [Instalación de la CLI de IBM Cloud Private](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/install_cli.html).
 
## Paso 1: Identificar contenido
{: #identify_content}

Identificar los documentos adecuados para analizar. {{site.data.keyword.cnc_short}} se ha diseñado para analizar los documentos de contrato <!-- and regulatory --> que cumplen los siguientes criterios:

- Los archivos a analizar deben estar en formato PDF.
- El contenido de PDF está en formato de texto. Los documentos que se han escaneado no se pueden analizar, incluso aunque los escaneos se hayan procesado mediante un lector de caracteres óptico (OCR).

  **Nota:** Puede identificar un PDF que se encuentra en un texto abriendo el documento en un visor de PDF y utilizando la herramienta **Selección de texto** para seleccionar una sola palabra. Si no puede seleccionar una única palabra en el documento, el archivo no se puede analizar.

- Los archivos no pueden tener un tamaño superior a 50 Mb.
- No es posible analizar PDF protegidos (con una contraseña para poder abrirlos) y cuya edición esté restringida (con una contraseña para poder editarlos).

## Paso 2: Analizar un contrato
{: #parse_contract}

En un shell `bash` o en un entorno equivalente como Cygwin, utilice el método `POST /v1/parse` para analizar el contrato. Sustituya `{ICP_IP_address}` por la dirección IP para el clúster de IBM Cloud Private. Sustituya `{PDF_file}` por la vía de acceso al PDF que se va a analizar.

```bash
curl -k -X POST -F 'file=@{PDF_file};type=application/pdf' https://{ICP_IP_address}/api/v1/parse?version=2018-03-23
```
{: pre}

**Importante:** Si está ejecutando una versión de {{site.data.keyword.cnc_short}} anterior a la 1.0.4, debe incluir el especificador `:{port_number}` con la dirección IP del clúster de IBM Cloud Private al realizar llamadas al servicio, tal como se indica a continuación:

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://{ICP_IP_address}:{port_number}/api/v1/parse?version=2018-03-23
```
{: pre}

Por ejemplo:

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

Consulte la [nota del release en **ingress**](/docs/services/compare-and-comply/relnotes.html#ingress) para obtener detalles.

El método devuelve un objeto JSON que contiene:

 - Una conversión de HTML del archivo PDF de origen.
 - El título del documento extraído.
 - Una matriz de `elementos` que definen las partes del componente del contrato.
 - Una matriz de `partes` que definen las entidades que se han identificado como partes.
 
**Importante**: La API de {{site.data.keyword.cnc_short}} en IBM Cloud Private no requiere autorización, ya que las API en IBM Cloud público sí.

## Paso 3: Revisar el análisis
{: #review_analysis}

Cada objeto de la matriz `elementos` describe un elemento del contrato que {{site.data.keyword.cnc_short}} ha identificado. El código siguiente representa un elemento típico:

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

El elemento tiene cinco secciones importantes:
  - `sentence_text`: El texto que se ha analizado.
  - `atributos`: Una matriz que enumera uno o más atributos del elemento. Los objetos soportados actualmente en la matriz `atributos` incluyen `Ubicación` (ubicación geográfica o región a la que hace referencia el elemento), `DateTime` (fecha, hora, rango de fechas o rango de hora especificados por el elemento) y `Moneda` (valores y unidades monetarias). 
  - `categorías`: Una matriz que enumera las categorías funcionales en las que está la sentencia identificada; en otras palabras, el asunto de la frase.
  - `tipos`: Una matriz que describe lo que es el elemento y a quién afecta. Consta de uno o varios conjuntos de claves `naturaleza` (el efecto de la frase en la `parte` identificada) y claves de `parte` (a quién afecta la frase).
  - `frase`: Un objeto que describe dónde se ha encontrado el elemento en el HTML convertido. Contiene un valor de carácter `inicio` y un valor de carácter `final`.

**Nota**: Algunas frases no están bajo ningún tipo o categoría, en cuyo caso el servicio devuelve las matrices `tipos` y `categorías` como objetos vacíos.

**Nota:** Algunas frases cubren varios temas, en cuyo caso el servicio devuelve varios conjuntos de objetos `tipos` y `categorías`.

**Nota**: Algunas frases no contienen ningún atributo identificable, en cuyo caso el servicio devuelve la matriz `atributos` como objetos vacíos.

Además, cualquier parte identificada se define en la matriz `partes`. La matriz `partes` está ubicada después de la matriz `elementos` en la salida JSON.

```json
  "parties" : [ {
    "party" : "Customer",
    "role" : "Buyer"
  } ]
```

La matriz `partes` incluye dos secciones importantes:

  - `parte`: El texto identificado como parte dentro del documento.
  - `rol`: El rol de la parte identificada. Los roles se han modificado en función del subdominio; consulte [la documentación en el subdominio especificado para obtener una lista de roles posibles](/docs/services/compare-and-comply/parsing.html#contract_parties). Las partes que no se pueden identificar como que tienen un rol específico se listan con el valor `desconocido`.

## Pasos siguientes
{: #next_steps}

Ha analizado correctamente un contrato para identificar la naturaleza, las partes y las categorías de las partes del componente del documento. Puede utilizar el análisis para comprender e imponer rápidamente el contrato analizado. Los siguientes pasos son:

 - [Comprensión del análisis del contrato](/docs/services/compare-and-comply/parsing.html#contract_parsing)
 - [Comprensión del esquema de salida](/docs/services/compare-and-comply/schema.html#output_schema) y del [análisis de tabla](/docs/services/compare-and-comply/tables.html#understanding_tables).


