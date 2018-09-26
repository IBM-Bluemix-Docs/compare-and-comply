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

# Instalación, configuración y utilización de los paneles de control de supervisión
{: #monitor}

Puede supervisar el estado de {{site.data.keyword.cnc_short}} utilizando los paneles de control de supervisión de IBM Cloud Private. Los paneles de control de supervisión utilizan Grafana, Kibana y Prometheus para visualizar información detallada y personalizada acerca de la instancia de {{site.data.keyword.cnc_short}}.

Para obtener información acerca de la supervisión del clúster de {{site.data.keyword.BluOpenStackDed}}, consulte [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window}.

Antes de utilizar los paneles de control, debe generarlos e instalarlos como se describe en los pasos siguientes.

## Paso 1: Descargar, extraer y representar las plantillas del panel de control
{: #render}

Realice los pasos siguientes para preparar las plantillas del panel de control para la instalación.

1. Descargue la imagen de {{site.data.keyword.cnc_short}} en Passport Advantage (PPA). El archivo es un archivo tar comprimido con un nombre similar a `ibm-watson-compare-comply-prod-1.0.5.tar.gz`. El archivo incluye las plantillas del panel de control de registro y un script `bash` para representar los paneles de control desde las plantillas.

1. Descomprima y expanda el archivo tar:
  ```bash
  mkdir ibm-watson-compare-comply-prod-1.0.5 && tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tar.gz -C ibm-watson-compare-comply-prod-1.0.5
  ``` 
  {: pre}

1. Cambie al directorio `charts` en el directorio extraído:
  ```bash
  cd ibm-watson-compare-comply-prod-1.0.5/charts
  ```
  {: pre}

1. Descomprima y expanda el archivo tar comprimido en el directorio `charts`:
  ```bash
  tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tgz
  ```
  {: pre}

1. Cambie al directorio `panel de control`. Incluye plantillas para alertas, registros y métricas además de un script `bash` para generar paneles de control a partir de plantillas.
  ```
  $ cd ibm-watson-compare-comply-prod/dashboard
  
  $ tree
    .
  ├── alerts.json.tpl
  ├── external-process-logging.json.tpl
  ├── frontend-logging.json.tpl
  ├── metrics.json.tpl
  └── render-dashboards.sh

  0 directorios, 5 archivos
  ```

1. Ejecute el script `render-dashboards.sh` para representar las plantillas. Las opciones para el script incluyen:
  
    - `-v`, `--version {chart_version}`: La versión del gráfico; por ejemplo, `1.0.5`.
    - `-h`, `--help`: Ayuda y salida del mandato print.
    - `-r`, `--release {release_name}`: El nombre de release de Helm.
    - `-n`, `--namespace {namespace}`: El espacio de nombre del despliegue. El espacio de nombres predeterminado es `default`.

  ```bash
  ./render-dashboards.sh -v 1.0.5 -r my-test-release -n default
  ```
  {: pre}

Los archivos JSON del panel de control se generan en `ibm-watson-compare-comply-prod-1.0.5/charts/ibm-watson-compare-comply-prod/dashboard`.

  ```
  $ tree
    .
  ├── alerts.json
  ├── alerts.json.tpl
  ├── external-process-logging.json
  ├── external-process-logging.json.tpl
  ├── frontend-logging.json
  ├── frontend-logging.json.tpl
  ├── metrics.json
  ├── metrics.json.tpl
  └── render-dashboards.sh

  0 directorios, 9 archivos
  ```

## Paso 2: Importar las plantillas
{: #import-tpls}

Después de representar los paneles de control, importe uno o varios como se describe en las secciones siguientes:

  - [Importación del panel de control de métricas](metrics.html#import)
  - [Importación de los paneles de control de registro](logging.html#import)
  - [Importación del panel de control de alertas](alerts.html#import)

## Paso 3: Visualizar y personalizar paneles de control
{: #use-dboards}

Después de importar los paneles de control seleccionados, visualícelos y personalícelos como se describe en las secciones siguientes:

  - [Visualización del panel de control de métricas](metrics.html#view)
  - [Visualización de los paneles de control de registro](logging.html#view)
  - [Adición de reglas de alerta](alerts.html#add)
