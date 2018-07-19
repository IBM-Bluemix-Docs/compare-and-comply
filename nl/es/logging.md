---

copyright:
years: 2017, 2018
lastupdated: "2018-03-23"

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

# Uso de los registros
{: #using-logging}

## Instalación y ejecución de los paneles de control de registro

Para instalar el panel de control de registro para {{site.data.keyword.cnc_short}}, efectúe los pasos siguientes.

  1. Descargue el archivo PPA (Passport Advantage) para {{site.data.keyword.cnc_short}}. El archivo es un archivo tar comprimido con un nombre similar a `ibm-watson-compare-comply-prod-1.0.0.tar.gz`. El archivo incluye las plantillas del panel de control de registro y un script `bash` para representar los paneles de control desde las plantillas.

  1. Descomprima y expanda el archivo PPA:
    ```bash
    $ mkdir ibm-watson-compare-comply-prod-1.0.0 && tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tar.gz -C ibm-watson-compare-comply-prod-1.0.0
    ```
    {: codeblock}

  1. Cambie al directorio `charts` en el directorio extraído:
    ```bash
    $ cd ibm-watson-compare-comply-prod-1.0.0/charts
    ```

  1. Descomprima y expanda el archivo tar comprimido en el directorio `charts`:
    ```bash
    $ tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tgz
    ```

  1. Cambie al directorio `panel de control`. Incluye plantillas para métricas y registro, y un script bash para generar paneles de control
a partir de plantillas.

    ```bash
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
  
    -  `-v, --version {chart_version}`: La versión del gráfico; por ejemplo, `1.0.0`.
    -  `-h, --help`: Ayuda y salida del mandato print.
    -  `-r, --release {release_name}`: El nombre de release de Helm.
    -  `-n, --namespace {namespace}`: El espacio de nombres del despliegue. El espacio de nombres predeterminado es `default`.

    ```bash
    $ ./render-dashboards.sh -v 1.0.0 -r my-test-release -n default
    The dashboard JSON files are generated under /Users/{user}/Downloads/ibm-watson-compare-comply-prod-1.0.0/charts/ibm-watson-compare-comply-prod/dashboard.

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

## Importación de los paneles de control de registro

Para importar los paneles de control de registro para {{site.data.keyword.cnc_short}} en IBM Cloud Private, efectúe los pasos siguientes.

  1. Inicie sesión en el clúster de IBM Cloud Private.

  1. En el icono Menú de la esquina superior izquierda, seleccione **Plataforma -> Registro**. <br />
    ![Icono de Menú de IBM Cloud Private](images/icp-menu.png) <br />
    ![Plataforma -> Menú de registro](images/icp-logging.png)

  1. Pulse **Gestión** en el lado izquierdo de la interfaz de Kibana. <br />
    ![Interfaz de Kibana](images/kibana.png)

  1. Seleccione el separador **Objetos guardados**.
    ![Separador Objetos guardados](images/saved-obj.png)

  1. Seleccione el separador **Búsquedas** y pulse **Importar**.
    ![Separador Importar desde Búsquedas](images/searches-import.png)

  1. Importe de forma individual los archivos `frontend-logging.json` y `external-process-logging.json` generados en el Paso 6 del procedimiento anterior. Cuando se le solicite, pulse **Sí, sobrescribir todo**.
     ![Solicitud Sí, sobrescribir todo](images/overwrite-all.png)

  1. Los paneles de control aparecen en el separador **Búsquedas**.
     ![Separador Paneles de control en las Búsquedas](images/searches-tab.png)

## Visualización de los paneles de control de registro

Para ver los paneles de control de registro, efectúe los pasos siguientes.

  1. Navegue hasta el separador **Descubrir**.

  1. Pulse **Abrir** junto al lado superior derecho de la interfaz de Kibana.

  1. Seleccione el panel de control que desee ver. Hay dos paneles de control de registro, para el registro de servicio y el registro de proceso externo.
    ![Ver paneles de control de registro](images/kibana-dboards.png)

Puede cambiar fácilmente el rango de tiempo y la frecuencia de la renovación automática:
  ![Cambiar el rango de tiempo y la frecuencia de renovación](images/log-dboard-change.png)

