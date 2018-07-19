---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-28"

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

# Utilización de métricas
{: #using-metrics}

Puede supervisar el estado de {{site.data.keyword.cnc_short}} utilizando el panel de control de supervisión de IBM Cloud Private. El panel de control de supervisión utiliza Grafana, Prometheus y Kibana para presentar información detallada sobre la instancia de {{site.data.keyword.cnc_short}}.

Para obtener más información sobre el panel de control de supervisión, consulte [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window}.

## Instalación y ejecución del panel de control de métricas

Para instalar el panel de control de métricas para {{site.data.keyword.cnc_short}}, efectúe los pasos siguientes.

 1. Descargue el archivo PPA (Passport Advantage) para {{site.data.keyword.cnc_short}}. El archivo es un archivo tar comprimido con un nombre similar a `ibm-watson-compare-comply-prod-1.0.0.tar.gz`. El archivo incluye la plantilla del panel de control de métricas y un script `bash` para representar el panel de control desde la plantilla.

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

## Importación del panel de control de métricas

Para importar el panel de control de métricas para {{site.data.keyword.cnc_short}} en IBM Cloud Private, efectúe los pasos siguientes.

  1. Inicie sesión en el clúster de IBM Cloud Private.

  1. En el icono Menú de la esquina superior izquierda, seleccione **Plataforma -> Supervisión**. <br />
      ![Icono de Menú de IBM Cloud Private](images/icp-menu.png) <br />
      ![Plataforma -> Menú de supervisión](images/icp-monitoring.png)

  1. Pulse **Inicio** cerca de la parte superior izquierda de la interfaz de Grafana. <br />
      ![Icono de inicio](images/icp-home.png)

  1. Pulse **Importar panel de control**.
      ![Icono Importar panel de control](images/import-dboard.png)

  1. Seleccione el archivo `metrics.json` que se ha generado en el paso 6 del procedimiento anterior y, a continuación, pulse **Cargar archivo .json**. <br />
      ![Cargar el archivo metrics.json](images/metrics-json.png)

  1. Seleccione **Prometheus** como origen de datos y, a continuación, pulse **Importar**.
       ![Seleccionar Prometheus](images/prometheus.png)

## Visualización del panel de control de métricas

El panel de control de métricas se parece a lo siguiente:
![El panel de control de métricas](images/metrics-dboard.png)

Puede cambiar fácilmente el rango de tiempo y la frecuencia de la renovación automática:
![Cambiar el rango de tiempo y la frecuencia de renovación](images/dboard-change.png)

## Edición del panel de control de métricas

Puede editar el panel de control de métricas o crear un panel de control nuevo efectuando los pasos siguientes.

  1. En el icono Menú de la esquina superior izquierda, seleccione **Plataforma -> Supervisión** para acceder a la IU de Grafana.

  1. Pulse **Inicio** cerca de la parte superior izquierda de la interfaz de Grafana y, a continuación, pulse **+ Panel de control nuevo**.

  1. Seleccione el tipo de panel que desee añadir, como por ejemplo **Gráfico** o **Tabla**.

  1. Pulse el título de panel y, a continuación, pulse **Editar**. El título de panel predeterminado es `Título de panel`.

  1. Utilice el separador **General** para establecer el título, la descripción y las dimensiones del panel. Tenga en cuenta que 12 unidades es la anchura completa de una ventana de navegador.

  1. Utilice el separador **Métricas** para crear consultas que muestren datos de Prometheus.

        1. Puede escribir la consulta directamente si está familiarizado con el lenguaje de consulta, o puede utilizar el campo **Búsqueda de métrica** para elegir entre las métricas que se están notificando actualmente a Prometheus.

        1. Los resultados de las consultas se muestran en tiempo real en el panel de control nuevo.

        1. Se pueden añadir varias consultas a un único panel. Por ejemplo, puede visualizar operaciones de lectura y de escritura en el mismo gráfico, o las visitas y los visitantes totales en la misma tabla.
        
