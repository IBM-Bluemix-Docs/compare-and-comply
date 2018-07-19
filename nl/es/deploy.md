---

copyright:
years: 2018
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

# Despliegue del servicio
{: #install}

Antes de instalar el diagrama Helm de {{site.data.keyword.cnc_long}} tal como se describe en el archivo `README.md` del diagrama Helm, es posible que tenga que realizar pasos de despliegue adicionales.

## Valores del sistema predeterminados
{: #sys_vals}

Estas instrucciones utilizan los siguientes valores del sistema predeterminados. Los valores para el sistema pueden diferir. Consulte con el administrador de IBM Cloud Private.

 - Nombre de clúster y número de puerto de IBM Cloud Private:
 	`mycluster.icp:8500`
 	
 - Espacio de nombres de IBM Cloud Private:
 	`default`

## Completar el despliegue
{: #deploy}

Realice los pasos siguientes para completar el despliegue de {{site.data.keyword.cnc_short}}:

### Paso 1: Otorgar acceso a Docker
{: #step1}

  1. Otorgar acceso a Docker al registro de IBM Cloud Private.
  
  	Si está ejecutando Microsoft Windows o Apple macOS u OS X, efectúe los pasos siguientes:

 	 1. Pulse el icono **Docker**.
 	 1. Abra el menú **Preferencias** de Docker.
 	 1. Pulse el separador **Daemon**.
 	 1. En el campo **Registros no seguros**, añada el valor `mycluster.icp:8500` o el valor para la instalación de IBM Cloud Private.
 	 
 	 ![Configurar Docker](images/docker.png)
 	 
 	Si está ejecutando Linux, efectúe los pasos siguientes:
 	
 	1. Localice el archivo `daemon.json`. De forma predeterminada, este archivo se encuentra en `/etc/docker/daemon.json`. Si el archivo no existe, créelo en la ubicación predeterminada. Abra el archivo en un editor de texto y verifique o añada el JSON siguiente.
 	
 	  ```json
 	  {
 	    "insecure-registries": ["mycluster.icp:8500"]
 	  }
 	  ```
 	  
 	1. Ejecute los siguientes mandatos:
 	  ```bash
 	  systemctl daemon reload
 	  ```
 	  {: pre}
 	  
 	  ```bash
 	  systemctl restart docker
 	  ``` 
 	  {: pre}

### Paso 2: Descargar los CLI de IBM Cloud Private e IBM Cloud
{: #step2}

  1. Descargue e instale la CLI de Helm para cada versión de IBM Cloud Private que esté utilizando. Consulte la [documentación de IBM Cloud Private ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/icp_cli.html){: new_window} para obtener detalles.
  
  1. Descargue e instale la CLI de IBM Cloud. Consulte la [documentación de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html#download_install){: new_window} para obtener detalles.
  
  1. Descargue e instale el plug-in de `bx` de la CLI de IBM Cloud. Consulte la [documentación de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/extend_cli.html#plug-ins){: new_window} para obtener detalles.
 	
### Paso 3: Descargar el archivo de paquete
{: #step3}
 	
Descargue el archivo de paquete `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` desde [Passport Advantage ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-01.ibm.com/software/passportadvantage/){: new_window} a su estación de trabajo local. Este archivo contiene toda la imagen de servicio de {{site.data.keyword.cnc_short}}.
  
#### Editar el archivo de paquete si es necesario
{: edit-pkg-file}

**Importante**: Realice el procedimiento en este paso *solo* si las siguientes condiciones son verdaderas. Si alguna o ambas condiciones son falsas, continúe con el [Paso 4](#step4).

 - Este paso solo se aplica a {{site.data.keyword.cnc_short}} versión 1.0.3 y anterior. Si está instalando {{site.data.keyword.cnc_short}} versión 1.0.4 o posterior, no realice el procedimiento en este paso.

 - Debe seguir el procedimiento de este paso solo si el clúster de IBM Cloud Private no está conectado a la Internet externa. Si el clúster de IBM Cloud Private tiene acceso de red al sitio de [Passport Advantage ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-01.ibm.com/software/passportadvantage/){: new_window}, puede instalar y desplegar {{site.data.keyword.cnc_short}} versión 1.0.3 sin ninguna modificación al archivo de paquete.

**Nota**: Si realiza el procedimiento en este paso, puede experimentar problemas con actualizaciones y retrotracciones de {{site.data.keyword.cnc_short}} versión 1.0.3. 
 
  1. En un shell `bash` o en un entorno similar como Cygwin, extraiga el archivo `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz`:
  
 	```bash
 	cd {path_to_file}
 	```
 	{: pre}
 	
 	```bash
 	tar -xvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz
 	```
 	{: pre}
 	
 	El archivo extraído incluye un directorio denominado `charts` que contiene un archivo denominado `ibm-watson-compare-comply-prod-1.0.3.tgz`, que es una versión comprimida del diagrama Helm para {{site.data.keyword.cnc_short}}.
 	
  1. Vaya al directorio `charts` y extraiga el archivo `ibm-watson-compare-comply-prod-1.0.3.tgz`:
  
 	```bash
 	cd charts
 	```
 	{: pre}
 	
 	```
 	tar -xvzf ibm-watson-compare-comply-prod-1.0.3.tgz
 	```
 	{: pre}
 	
 	El archivo extraído incluye varios directorios y archivos, incluido un directorio de nivel superior denominado `ibm-watson-compare-comply-prod`, que contiene un directorio denominado `plantillas`.
 	
  1. Vaya al directorio `ibm-watson-compare-comply-prod/templates`:
  
 	```bash
 	cd ibm-watson-compare-comply-prod/templates
 	```
 	{: pre}

  1. Localice el archivo `deployment.yaml` en el directorio `ibm-watson-compare-comply-prod/templates` y ábralo en un editor de texto.
 
  1. Edite el archivo `deployment.yaml` tal como se indica a continuación:
  
 	**Original**
 	```
 	- name: {{ .Chart.Name }}
 		image: "hyc-icpcontent-docker-local.artifactory.swg-devops.com/cncdev/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	
 	**Actualizado**
 	```
 	- name: {{ .Chart.Name }}
 		image: "mycluster.icp:8500/default/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	Guarde y cierre el archivo `deployment.yaml`.
 	
  1. Vuelva al directorio `charts` y vuelva a empaquetar el archivo `ibm-watson-compare-comply-prod-1.0.3.tgz`:
  
 	```bash
 	cd ../..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf ibm-watson-compare-comply-prod-1.0.3.tgz ibm-watson-compare-comply-prod
 	```
 	{: pre}
 	
  1. Vuelva al directorio de nivel superior y vuelva a empaquetar el archivo `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz`:
  
 	```bash
 	cd ..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz charts images manifest.json manifest.yaml
 	```
 	{: pre}
 	
 	 Para su referencia, la vista de `árbol` del contenido del archivo `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` no comprimido es el siguiente. Los archivos implicados en este procedimiento se llaman con `<-----` en la columna de la derecha.
 	 
 	 ```bash
 	 cd {path_to_archive_file}
 	 ```
 	 {: pre}
 	 
 	 ```bash
 	 tree
 	 
	 .
	 ├── IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz          <-----
     ├── charts                                            <-----
     │   ├── ibm-watson-compare-comply-prod
     │   │   ├── Chart.yaml
     │   │   ├── LICENSE
     │   │   ├── LICENSES
     │   │   │   └── LICENSE-Product
     │   │   ├── README.md
     │   │   ├── cnc-banner.png
     │   │   ├── dashboard
     │   │   │   ├── alerts.json.tpl
     │   │   │   ├── external-process-logging.json.tpl
     │   │   │   ├── frontend-logging.json.tpl
     │   │   │   ├── metrics.json.tpl
     │   │   │   └── render-dashboards.sh
     │   │   ├── templates                                 <-----
     │   │   │   ├── NOTES.txt
     │   │   │   ├── _helpers.tpl
     │   │   │   ├── _sch-chart-config.tpl
     │   │   │   ├── configmap.yaml
     │   │   │   ├── deployment.yaml                       <-----
     │   │   │   ├── ingress.yaml
     │   │   │   ├── metrics-service.yaml
     │   │   │   ├── sch-2.6.0
     │   │   │   │   ├── _config.tpl
     │   │   │   │   ├── _metadata.tpl
     │   │   │   │   ├── _names.tpl
     │   │   │   │   ├── _utils.tpl
     │   │   │   │   └── _version.tpl
     │   │   │   ├── service.yaml
     │   │   │   └── tests
     │   │   │       └── service-test.yaml
     │   │   ├── values-metadata.yaml
     │   │   └── values.yaml
     │   └── ibm-watson-compare-comply-prod-1.0.3.tgz      <-----
     ├── images
     │   ├── 304735eab4e4df55b236f9835104cb1058bc50cb5ff7f4ee7d719cb56d2a718b.tar.gz
     │   ├── af80995a4fcfcee364f4f72ba043bf9e73dcafe7fcac384e1cd06bf1c7a52c1f.tar.gz
     │   └── ccec10461710cd625cbbacc0f5d5d837a0cc5baf132e0fc39ed42d8264d25062.tar.gz
     ├── manifest.json
     └── manifest.yaml 
 	 ```
 	 {: pre}
   
### Paso 4: Preparar un terminal
{: #step4}
 	
Prepare un shell `bash` o equivalente para utilizar con la instalación de IBM Cloud Private ejecutando los mandatos siguientes en el shell:
  
  ```bash
  bx pr login -a https://mycluster.icp:8443 --skip-ssl-validation
  ```
  {: pre}
  
  ```bash
  bx pr cluster-config mycluster
  ```
  {: pre}
  
  ```bash
  docker login mycluster.icp:8500
  ```
  {: pre}

  **Nota**: En el primer mandato de la lista anterior, el número de puerto `8443` del URL `https://mycluster.icp:8443` representa el valor predeterminado para comunicarse con el servicio de {{site.data.keyword.cnc_short}} en IBM Cloud Private.

### Paso 5: Crear un secreto de registro de imágenes y una cuenta de servicio de parches
{: #step5}

Ejecute los mandatos siguientes para permitir que todos los componentes necesarios accedan al registro interno de IBM Cloud Private:

```bash
kubectl create secret docker-registry $secret_name --docker-server=mycluster.icp:8500 --docker-username=$username --docker-password=$password --docker-email=admin@admin.com --namespace=default
```
{: pre}

```bash
kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "$secret_name"}]}' --namespace=default
```
{: pre}

donde:
  - `$username` y `$password` varían según el clúster. Pregunte al administrador.
  - `$secret_name` es una serie que especifica [para habilitar a los contenedores de Docker comunicarse de forma segura ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.docker.com/engine/swarm/secrets/){: new_window}.

### Paso 6: Cargar el archivo de paquete en el registro de imágenes interno de IBM Cloud Private
{: #step6}

En el terminal que ha preparado en el [Paso 4](#step4), ejecute el mandato siguiente para cargar el archivo de paquete de Passport Advantage:

```bash
bx pr load-ppa-archive --archive {path_to}/IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz --clustername mycluster.icp --namespace default 
```
{: pre}

### Paso 7: Finalizar la instalación

Vuelva a la página **Catálogo** en el clúster de IBM Cloud Private, localice y seleccione la entrada `ibm-watson-compare-comply-prod` y pulse **Configurar** para continuar con la instalación.
