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

# Comprensión de las configuraciones de despliegue
{: #configs}

La configuración de despliegue predeterminada para {{site.data.keyword.cnc_short}} es `2 núcleos de 10 G y 1 documento simultáneo (2 VPC)`. Esta configuración puede procesar aproximadamente 1500 páginas por hora. Puede escalar el despliegue de {{site.data.keyword.cnc_short}} en IBM Cloud Private a distintos niveles en función de los requisitos de rendimiento. Póngase en contacto con el representante de ventas de IBM para obtener más información sobre el aumento de la capacidad de {{site.data.keyword.cnc_short}} en IBM Cloud Private.

{{site.data.keyword.cnc_short}} puede escalarse en dos dimensiones: _vertical_ y _horizontal_.

 - El escalado _Vertical_ aumenta el rendimiento en el que se analiza un documento.
 - El escalado _Horizontal_ aumenta el número de documentos simultáneos analizados.

Ambas opciones de escalado necesitan aumentar el número de Núcleos de procesador virtual (VPC) utilizados por {{site.data.keyword.cnc_short}}. Para obtener información sobre los VPC, consulte la [documentación de licencia de IBM Cloud Private ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_cluster/licensing.html){: new_window}.

Las opciones de escalado vertical incluyen lo siguiente:

| Configuración                             |Rendimiento aproximado*         |
|:-----------------------------------------:|--------------------------------|
|`2 núcleos de 10 G y 1 documento simultáneo (2 VPC)` |1500 páginas por hora (predeterminado)   |
|`4 núcleos de 20 G y 1 documento simultáneo (4 VPC)` |2500 páginas por hora             |
|`8 núcleos de 40 G y 1 documento simultáneo (8 VPC)` |3000 páginas por hora             |

Las opciones de escalado horizontal incluyen lo siguiente:

| Configuración                               |Rendimiento aproximado*         |
|:-------------------------------------------:|--------------------------------|
|`2 núcleos de 10 G y 2 documentos simultáneos (4 VPC)`  |3000 páginas por hora             |
|`4 núcleos de 20 G y 2 documentos simultáneos (8 VPC)`  |5000 páginas por hora             |
|`8 núcleos de 40 G y 2 documentos simultáneos (16 VPC)` |6000 páginas por hora             |

\***Nota**: Las estimaciones de rendimiento se basan en pruebas internas. El rendimiento en un entorno de producción depende de varios factores, incluidas otras cargas de trabajo en el mismo clúster de IBM Cloud Private, las latencias entre la aplicación cliente y el clúster de IBM Cloud Private, y otras condiciones ambientales.

Para cambiar la configuración de despliegue añadiendo o eliminando clústeres de {{site.data.keyword.cnc_short}}, realice los pasos siguientes.

**Nota**: Si reduce un despliegue de {{site.data.keyword.cnc_short}}, IBM Cloud Private reclama inmediatamente los recursos liberados, reduciendo así la capacidad de proceso del despliegue. No reduzca un despliegue que se esté utilizando, especialmente si el despliegue se encuentra en un entorno de producción.
	
De forma similar, si aumenta un despliegue de {{site.data.keyword.cnc_short}}, IBM Cloud Private aplica inmediatamente los recursos solicitados al despliegue, suponiendo que los recursos estén disponibles. Si necesita más recursos de los que puede proporcionar la instalación de IBM Cloud Private, consulte con el representante de ventas de IBM acerca del aumento de la capacidad de la instalación de IBM Cloud Private.

  1. Inicie sesión en el clúster de IBM Cloud Private.

  1. En el icono de menú junto a la esquina superior izquierda, seleccione **Cargas de trabajo -> Despliegues**.
  
  1. La consola muestra la página **Despliegues**. Localice el despliegue que desea escalar en la tabla de despliegues.
  
  1. En la fila de tabla que enumera el despliegue, pulse el icono **Acción** en la última columna de la tabla y, a continuación, pulse **Escalar**.
  
  1. La consola muestra la ventana **Despliegue de escalado**. En el campo **Instancias**, especifique el número de instancias que desee, o utilice las flechas hacia arriba y hacia abajo para seleccionar el valor deseado.
  
  1. Pulse **Despliegue de escalado**.
  
  1. La ventana **Despliegue de escalado** se cerrará y la consola vuelve a mostrar la página **Despliegues**. Busque el despliegue y utilice las columnas **Deseado** y **Actual** para verificar que el número de instancias solicitado se haya asignado al despliegue.
  
  1. Opcionalmente, pulse el nombre del despliegue en la página **Despliegues** para ver detalles del despliegue.
  
  

