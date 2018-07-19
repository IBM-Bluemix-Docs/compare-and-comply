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

# Utilización de alertas
{: #using-alerts}

Puede configurar alertas para el clúster de {{site.data.keyword.cnc_short}}.

## Instalación del panel de control de alertas

Para instalar el panel de control de alertas para {{site.data.keyword.cnc_short}}, efectúe los pasos siguientes.

 1. Descargue el archivo PPA (Passport Advantage) para {{site.data.keyword.cnc_short}}. El archivo es un archivo tar comprimido con un nombre similar a `ibm-watson-compare-comply-prod-1.0.0.tar.gz`. El archivo incluye la plantilla del panel de control de alertas y un script `bash` para representar el panel de control desde la plantilla.

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

## Adición de las reglas de alerta

Para añadir las reglas de alerta al panel de control, efectúe los pasos siguientes.

  1. Inicie sesión en el clúster de ICP.

  1. En el icono Menú de la esquina superior izquierda, seleccione **Configuración -> ConfigMaps**.
      ![Icono de Menú de IBM Cloud Private](images/icp-menu.png) <br />
      ![Configuración -> Menú ConfigMaps](images/configmaps.png)

  1. La página **ConfigMaps** se abre para mostrar una tabla de configmaps. En la tabla, localice la fila con la etiqueta `alert-rules`. En la columna **Acción** de la fila `alert-rules`, pulse el icono de menú y seleccione **Editar**.
     ![Editar alert-rules](images/configmaps-page.png)

  1. Abra el archivo `.../ibm-watson-compare-comply-prod-1.0.0/charts/ibm-watson-compare-comply-prod/dashboard/alerts.json` en un editor de texto y copie la línea que empieza por `cnc.rules`.

  1. Se abre la ventana **Editar ConfigMap**. En el objeto `data`, añada una coma al final de la última línea del objeto y, a continuación, pegue en la línea `cnc.rules` que ha copiado en el paso anterior. <br />
     ![Editar ConfigMap](images/edit-configmap.png)

  1. Pulse **Enviar** en la ventana **Editar ConfigMap**.

## Visualización de reglas de alerta

Para ver la lista de reglas de alerta, efectúe los pasos siguientes.

  1. Navegue al panel de control de Prometheus en el clúster de IBM Cloud Private. El panel de control de Prometheus se encuentra en `https://{ICP_cluster_IP_address}:{ICP_cluster_port}/prometheus`.

  1. Pulse el separador **Alertas**. El panel de control Prometheus muestra una lista de todas las reglas de alerta y el número de alertas activas para cada uno. <br />
    ![Alertas de Prometheus](images/prometheus-dboard.png)

## Adición de notificación de alerta

Puede añadir notificaciones de alerta para varios sistemas de paginación, incluidos Slack, PagerDuty, HipChat, correo electrónico y otros. Prometheus proporciona soporte para notificaciones tal como se documenta en los sitios siguientes:

 - [Documentación de configuración de alertas de Prometheus ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://prometheus.io/docs/alerting/configuration/){: new_window}
 - [Documentación de ejemplos de notificación de Prometheus ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://prometheus.io/docs/alerting/notification_examples/){: new_window}

Para crear un receptor de notificaciones para {{site.data.keyword.cnc_short}} en IBM Cloud Private, efectúe los pasos siguientes.
{: #create-notification-receiver}

  1. Inicie sesión en el clúster de ICP.

  1. En el icono Menú de la esquina superior izquierda, seleccione **Configuración -> ConfigMaps**. <br />
      ![Icono de menú de IBM Cloud Private](images/icp-menu.png) <br />
      ![Configuración -> Menú ConfigMaps](images/configmaps.png)

  1. La página **ConfigMaps** se abre para mostrar una tabla de configmaps. En la tabla, localice la fila con la etiqueta `monitoring-prometheus-alertmanager`. En la columna **Acción** de la fila `monitoring-prometheus-alertmanager`, pulse el icono de menú y seleccione **Editar**.

  1. Se abre la ventana **Editar ConfigMap**. En el objeto `data`, especifique las nuevas configuraciones de receptor.
     ![Editar ConfigMap](images/prom-alert-edit.png)

  1. Pulse **Enviar** en la ventana **Editar ConfigMap**.

### Ejemplos

Para crear una notificación Slack, efectúe los pasos siguientes.

  1. Verifique que el canal Slack de destino exista. Si no existe, créelo. Consulte la [documentación de Slack para crear un canal ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://get.slack.help/hc/en-us/articles/201402297-Create-a-channel){: new_window} para obtener detalles.

  1. Obtenga o cree el WebHook para el canal Slack. Consulte la [documentación de Slack para WebHooks ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://get.slack.help/hc/en-us/articles/115005265063-Incoming-WebHooks-for-Slack){: new_window} para obtener detalles.

  1. Abra el ConfigMap de `monitoring-prometheus-alertmanager` en el editor de ConfigMap como se describe en [Adición de notificaciones de alerta](#create-notification-receiver).

  1. Actualice el objeto `data` en el ConfigMap como se indica a continuación:
    ```
    "data": {
      "alertmanager.yml": "global: \n  slack_api_url: '{WebHook_URL_for_Slack_channel}' \nreceivers: \n  - name: default-receiver \n    slack_configs: \n    - channel: '#{Slack_channel}' \n      send_resolved: true \nroute: \n  receiver: default-receiver \n  routes: \n  - match: \n    severity: critical \n   receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. En la ventana **Editar ConfigMap**, pulse **Enviar**.

Para crear una notificación de PagerDuty, efectúe los pasos siguientes.

  1. Verifique que exista el servicio de PagerDuty. Si no existe, créelo. Consulte la [documentación de PagerDuty ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://v2.developer.pagerduty.com/docs){: new_window} para obtener detalles.

  1. Obtenga la clave de integración de PagerDuty añadiendo la integración de Prometheus. Consulte la [documentación de la API de PagerDuty ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://v2.developer.pagerduty.com/docs/events-api){: new_window} para obtener detalles.

  1. Abra el ConfigMap de `monitoring-prometheus-alertmanager` en el editor de ConfigMap como se describe en [Adición de notificaciones de alerta](#create-notification-receiver).

  1. Actualice el objeto `data` en el ConfigMap como se indica a continuación:
    ```
    "data": {
      "alertmanager.yml": "global:\nreceivers:\n  - name: default-receiver\n    pagerduty_configs:\n    - service_key: ' {PagerDuty_integration_key}'\nroute:\n  receiver: default-receiver\n  routes:\n  - match:\n      severity: critical\n    receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. En la ventana **Editar ConfigMap**, pulse **Enviar**.
  
