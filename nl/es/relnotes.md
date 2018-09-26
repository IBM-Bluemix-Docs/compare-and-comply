
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

# Notas del release
{: #release_notes}

Las notas del release proporcionan información sobre los cambios en el release de servicio de {{site.data.keyword.cnc_long}} en {{site.data.keyword.BluOpenStackDed}}.

**Importante:** Este conjunto de documentación se aplica únicamente al servicio de {{site.data.keyword.cnc_short}} en {{site.data.keyword.BluOpenStackDed}}. No se aplica a otros servicios de Watson que estén disponibles en IBM Cloud público.

**Nota:** Para obtener más información sobre la nota que afecta a todos los servicios de {{site.data.keyword.BluOpenStackDed}}, consulte [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/known_issues.html#issues_nlv ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/known_issues.html#issues_nlv){: new_window}.

## Versiones de la API del servicio
{: #api_versioning}

Las solicitudes de API requieren un parámetro de versión que toma una fecha en el formato `version=AAAA-MM-DD`. Siempre que cambiamos la API de forma que sea incompatible con versiones anteriores, ponemos a su disposición una versión inferior de la API.

Envíe el parámetro version con cada solicitud de API. El servicio utiliza la versión de la API correspondiente a la fecha que especifique, o la versión más reciente anterior a dicha fecha. No tiene como valor predeterminado la fecha actual. En su lugar, especifique una fecha que coincida con una versión que sea compatible con la aplicación, y no la cambie hasta que la aplicación esté lista para una versión posterior.

La versión actual es `2018-03-23`.

## Cambios
{: #changes}

Están disponibles las siguientes nuevas características y cambios en el servicio.

**Importante**: El número de versión al que se hace referencia en las secciones siguientes es la versión del diagrama Helm {{site.data.keyword.cnc_long}} que ha desplegado en el clúster de {{site.data.keyword.BluOpenStackDed}}.

### 1.0.5, 2 de agosto de 2018
{: #105}

  - La adición del análisis de tablas como se describe en [Comprensión del esquema de salida](/docs/services/compare-and-comply/schema.html#output_schema) y [Comprensión del análisis de tablas](/docs/services/compare-and-comply/tables.html#understanding_tables).


### 1.0.4, 5 de julio de 2018
{: #ingress}

**Importante**: Debido a los cambios descritos en esta sección, no puede actualizar a la versión 1.0.4 desde versiones anteriores de {{site.data.keyword.cnc_short}} utilizando los procedimientos de actualización estándares de {{site.data.keyword.BluOpenStackDed}}. Debe volver a desplegar {{site.data.keyword.cnc_short}} como se indica a continuación:

1.  Elimine el despliegue de {{site.data.keyword.cnc_short}} existente tal como se describe en [Eliminación de un despliegue ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_applications/remove_app.html){: new_window}.

1.  Despliegue {{site.data.keyword.cnc_short}} versión 1.0.4 o posterior tal como se describe en el archivo `README.md` del diagrama Helm de {{site.data.keyword.cnc_short}} y en [Despliegue del servicio](/docs/services/compare-and-comply/deploy.html).

**Importante:** El redespliegue del servicio tiene las consecuencias siguientes:

- El servicio no está disponible entre la eliminación de la instancia anterior y el despliegue de la instancia actual.
- Si tiene código que realiza llamadas de API que incluyen números de puerto en los URL de llamadas, debe eliminar los números de puerto. Si realiza una llamada de API que incluye un número de puerto en {{site.data.keyword.cnc_short}} versión 1.0.4 o posterior, la llamada falla con un error similar al siguiente:
  ```
  curl: (7) Failed to connect to 10.19.74.45 port 8443: Connection refused
  ```
  {: pre}

**Importante:** Si está ejecutando una versión de {{site.data.keyword.cnc_short}} anterior a 1.0.4, debe incluir el especificador `:{port_number}` con la dirección IP del clúster {{site.data.keyword.BluOpenStackDed}} al realizar llamadas al servicio, como en el ejemplo siguiente:
```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

#### Notas adicionales

-   Ahora se proporcionan instrucciones de despliegue adicionales en el apartado [Despliegue del servicio](/docs/services/compare-and-comply/deploy.html).
-   El servicio {{site.data.keyword.cnc_short}} ahora está integrado con el [controlador de Ingress ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/components.html){: new_window} de {{site.data.keyword.BluOpenStackDed}}. Este cambio le permite llamar a los métodos de la API del servicio sin especificar un número de puerto en los URL de llamadas. A continuación se indica una llamada de API típica antes de la integración de **Ingress**:

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
    ```
    {: pre}

  La llamada equivalente en la versión 1.0.4 o posterior es:

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45/api/v1/parse?version=2018-03-23
    ```
    {: pre}

- Al desplegar el release 1.0.4, es posible que vea un error similar al siguiente:

    ```
    Unexpected response code 500 from request: GET https://icp-management-ingress:8443/helm-api/api/v1/releases/weblib?locale=en-US HTTP/1.1 Accept: application/json Content-Type: application/json Content-Type: application/json Cookie: *** {} HTTP/1.1 500 Server: openresty/1.11.2.4 Date: Thu, 21 Jun 2018 23:09:05 GMT Content-Type: application/json; charset=utf-8 Content-Length: 93 Connection: close X-Powered-By: Express Cache-Control: private, max-age=0, no-cache Etag: W/"5d-6htOwRqeFWIdvat6vK/kTg" {"statusCode":500,"message":"Internal service error : Cannot read property '0' of undefined"}
    ```
    {: codeblock}

    Puede no tenerlo en cuenta y cerrarlo.

### 1.0.3, 25 de mayo de 2018

- La salida del método `analizar` ahora incluye la matriz `atributos`. Consulte [Revisar el análisis](/docs/services/compare-and-comply/getting-started.html#review_analysis) y [Atributos](/docs/services/compare-and-comply/parsing.html#attributes) para obtener información. Puede utilizar la información de la matriz `atributos` para buscar elementos de documentos que hacen referencia a ubicaciones específicas; horas, fechas, rango de horas o rango de fechas; y valor y unidades monetarias.
- Cada objeto de las matrices `tipos` y `categorías` incluye un objeto `procedencia`. El objeto `procedencia` tiene una o varias claves `id`. Cada clave `id` tiene un valor de hash que puede enviar a IBM para proporcionar comentarios o recibir soporte.
- Mejoras en la precisión del análisis de PDF y en el rendimiento del análisis de archivos PDF de gran tamaño.
- Este release está disponible en {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 o superior. Consulte la entrada de catálogo para el servicio para obtener detalles sobre los requisitos de {{site.data.keyword.BluOpenStackDed}}.
- Los valores posibles para la clave `seguros` ya no incluyen `Bajo`.

### 1.0.2, 19 de abril de 2018

- Categorías soportadas adicionales. Consulte la [documentación de Categorías](/docs/services/compare-and-comply/parsing.html#contract_categories) para obtener la lista más reciente de categorías soportadas.
    - `Uso de activos`
    - `Comunicación`
    - `Protección y seguridad`
-  Actualizaciones de la documentación, incluidas correcciones en los ejemplos en [Iniciación](/docs/services/compare-and-comply/getting-started.html).

### 1.0.1 (release de disponibilidad general), 23 de marzo de 2018

Las siguientes notas se aplican al release de disponibilidad general (GA) del servicio {{site.data.keyword.cnc_long}}.

- El release de GA solo está disponible en {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 o superior. Consulte la entrada de catálogo para el servicio para obtener detalles sobre los requisitos de {{site.data.keyword.BluOpenStackDed}}.

## Notas generales
{: #general_notes}

- La API de {{site.data.keyword.cnc_short}} en {{site.data.keyword.BluOpenStackDed}} no requiere autorización, ya que las API en IBM Cloud público sí.
 - Los PDF deben estar en formato de texto para analizarse. Los documentos que se han escaneado no se pueden analizar, incluso aunque los escaneos se hayan procesado mediante un lector de caracteres óptico (OCR).

## Problemas conocidos
{: #known_issues}

- El tamaño máximo de un archivo PDF que se puede cargar es 50 Mb.
- Los PDF con la seguridad habilitada no se pueden analizar.
- Los documentos con diseños de página no estándares (como 2 o 3 columnas por página) no se analizan correctamente.
