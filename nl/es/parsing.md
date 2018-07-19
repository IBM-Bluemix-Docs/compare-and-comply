---

copyright:
  years: 2017, 2018
lastupdated: "2018-04-27"

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

# Comprensión del análisis del contrato
{: #contract_parsing}

{{site.data.keyword.cnc_short}} devuelve contratos analizados con un análisis de cada elemento identificado.

Las secciones siguientes describen cómo proporciona el análisis el JSON devuelto.

## Tipos
{: #contract_types}

La matriz `tipos` incluye varios objetos, cada uno de los cuales contiene las claves `naturaleza` y `parte` cuyos valores identifican una dupla para el elemento. Las tablas siguientes listan los valores posibles de las claves `naturaleza` y `parte`.

El valor `naturaleza` enumera los tipos de acción que requiere la frase.

| `naturaleza`         |Descripción                                                |
|:----------------:|-----------------------------------------------------------|
|`Definición`      |Este elemento añade claridad a un término, relación o similar. No es necesaria ninguna acción para cumplir el elemento, ni está afectada ninguna parte.|
|`Descargo de responsabilidad`      |La `parte` del elemento no está obligada a cumplir los términos especificados por el elemento, pero no tiene prohibido hacerlo.|
|`Exclusión`       |La `parte` del elemento no cumplirá los términos especificados por el elemento.|
|`Obligación`      |La `parte` del elemento necesita cumplir los términos especificados por el elemento.|
|`Derecho`           |La `parte` del elemento está garantizada para recibir los términos especificados por el elemento.|
|`Recomendación`  |La `parte` del elemento no está obligada, pero se le recomienda encarecidamente, a cumplir los términos especificados por el elemento.|

## Partes
{: #contract_parties}

La matriz `partes` especifica los participantes listados en el contrato. Cada objeto `parte` identificado enumera la parte identificada por nombre y se compara con un objeto `rol` que clasifica el rol del objeto `parte`. Los valores de `rol` que se pueden devolver para los contratos incluyen:

| `rol`           |Descripción                                                |
|:----------------:|-----------------------------------------------------------|
|`Comprador`           |La parte responsable de pagar por los productos o servicios enumerados en el contrato.|
|`Usuario final`        |La parte que interactuará con los productos o servicios proporcionados, distinguidos explícitamente del `Comprador`.|
|`Ninguno`            |No se ha identificado ninguna parte para el elemento. Este valor siempre está interconectado con la naturaleza `Definición`.|
|`Proveedor`        |La parte responsable de proporcionar los productos o servicios enumerados en el contrato.|

## Categorías
{: #contract_categories}

La matriz `categorías` define el tema principal de la frase. Las categorías soportadas actualmente incluyen:

| `categorías`     |Descripción                                                |
|:----------------:|-----------------------------------------------------------|
|`Enmiendas`      |Elementos que especifican cambios en el contrato una vez que se haya firmado o que se modifiquen en un contrato estándar. Los elementos que hacen referencia a las modificaciones de un contrato y a los cambios en los acuerdos también entran en esta categoría.|
|`Uso de activos`       |Elementos que hacen referencia a situaciones en las que una parte debe utilizar los activos (licencias o equipo) de la otra parte en la realización de sus funciones bajo el acuerdo, incluidos los permisos y las restricciones al respecto.|
|`Asignaciones`     |Elementos que describen la transferencia de derechos, obligaciones o ambos a un tercero.|
|`Auditorías`          |Esta categoría incluye los elementos que hacen referencia al mantenimiento de registros y al derecho de las partes a inspeccionar los libros o registros de la otra parte, y los elementos que hacen referencia al derecho de una parte para inspeccionar o revisar la conformidad, o los requisitos de que una parte esté disponible para su inspección o revisión de conformidad.|
|`Continuidad del negocio`|Una categoría reducida para los elementos que hacen referencia al efecto de una venta, fusión u otro cambio sustancial a una de las partes del acuerdo.|
|`Comunicación`  |Elementos que hacen referencia a los requisitos para notificar o proporcionar aviso, detalles sobre los métodos de comunicación (el acto o el proceso de intercambio de información), los representantes de contacto y la información de contacto. Además, los elementos que hacen referencia a los medios aceptables de intercambio de información entre las partes.|
|`Confidencialidad` |Esta categoría incluye elementos que describen cómo pueden utilizar las partes la información que obtienen al completar el contrato; elementos que tratan del mantenimiento de secretos comerciales; y elementos que describen la no divulgación de información sobre la empresa.|
|`Entregables`    |Esta categoría incluye elementos que especifican el elemento o elementos que una parte da a la otra parte a cambio de dinero, y elementos que describen la preparación de entregas.|
|`Entrega`        |Elementos que especifican los medios para transferir el elemento o los elementos de la categoría `Entregables` de una parte a otra. Por ejemplo, si una parte está vendiendo acceso o servicios en la nube, los medios para obtener ese acceso están dentro de esta categoría.|
|`Resolución de disputas`|Esta categoría incluye elementos que tratan de las disposiciones para resolver cualquier disputa que surja entre partes contratantes; por ejemplo, el acuerdo mediante un procedimiento definido como, por ejemplo, un panel de arbitraje. Las controversias pueden resolverse mediante distintos métodos que no sean el arbitraje; por ejemplo, el daño irreparable, la obtención de una orden judicial o una declaración en el efecto de "You may not pursue this as a class action". Entre los ejemplos de disputas se incluyen las disputas sobre la mano de obra y las disputas sobre facturas o facturación. La categoría también incluye elementos que especifican la ley de gobierno del contrato o la elección de la ley, como por ejemplo un país o jurisdicción en particular.|
|`Fuerza mayor`   |Elementos que hacen referencia a sucesos disruptivos fuera del control de una parte. Se trata de una limitación de responsabilidad específica.|
|`Indemnización` |Elementos que especifican la remediación de determinadas responsabilidades. La indemnización es cuando una parte del contrato se hace responsable de pagar por la responsabilidad de otra parte. El hecho de la indemnización implica que se ha producido una responsabilidad.|
|`Seguros`       |Elementos que hacen referencia a cualquier tipo de cobertura de seguro o de términos de cobertura proporcionados por una de las partes a la otra parte.|
|`Propiedad intelectual`|Los ejemplos tienden a estar dentro de dos categorías: las que utilizan definiciones formales de patentes, derechos de autor y marcas, normalmente extraídas de los estatutos; y las que utilizan conceptos más amplios de invenciones, auditoría y conocimientos. Las definiciones también se pueden proporcionar para derechos de propiedad intelectual, como por ejemplo el derecho a demandar y reclamar daños por la violación de tales derechos de propiedad. `Propiedad intelectual` incluye patentes, derechos de solicitud para patentes, marcas registradas, nombres comerciales, marcas de servicio, nombres de dominio, derechos de autor y todas las aplicaciones y registros de este tipo en todo el mundo, esquemas, modelos industriales, inventos, invenciones, secretos comerciales, programas de software de computación y otra información de propiedad intangible. Para el "Código de terceros", si alguna parte del código pertenece a otra persona, es necesario llamar a la IP.|
|`Responsabilidad`       |Elementos que describen el método para determinar cuándo y cómo se asignan los errores a cualquier parte.|
|`Condiciones de pago y facturación`|Una amplia categoría que incluye elementos que describen cómo se paga o se paga a una parte, incluidos los siguientes: elementos que hacen referencia al modo final de pago bajo el contrato o cómo se paga a una parte; elementos que detallan cómo y cuándo se va a pagar a la parte o los tipos de elementos que se pagarán o facturarán a las partes; elementos que hagan referencia a cualquier tipo de pago de tarifa, salvo aquellos que hagan referencia a precios o cifras específicos; y elementos que hagan referencia a la parte que recibe los pagos en el caso de que se pague bajo el contrato.|
|`Precios y tasas` |Esta categoría incluye elementos que describen lo que paga una parte o qué se paga. La categoría incluye elementos que hacen referencia a impuestos y cantidades específicas asociadas con los entregables individuales que se intercambian, incluidos detalles sobre los costes de un elemento concreto, y elementos que describen figuras o métodos específicos para calcular los precios.|
|`Privacidad`         | Elementos que especifican el tratamiento de la información personal; es decir, información privada sobre una persona o personas específicas que deben ser protegidas. La categoría es un subconjunto muy específico de la categoría más amplia de `Confidencialidad`.|
|`Responsabilidades`|Tareas accesorias del acuerdo, sobre las cuales sólo una de las partes tiene supervisión y control. Los tipos de elementos que entran en esta categoría difieren en función de la naturaleza del contrato.|
|`Protección y seguridad`|Elementos que hacen referencia a la seguridad física o a las protecciones de seguridad cibernética para datos y sistemas, así como a elementos que hacen referencia a la mejora de la seguridad del lugar o lugares de trabajo.|
|`Ámbito de trabajo`   |Esta categoría define lo que hay en el contrato en comparación con lo que no está en el contrato. Por lo general, los elementos que le dicen a las partes cómo se debe definir un orden concreto entran en esta categoría. Los ejemplos incluyen discusiones sobre las "declaraciones de trabajo" o las descripciones de los estándares de comunicaciones aplicables.|
|`Subcontratos`    |Elementos que hacen referencia a la contratación de terceros para realizar determinadas tareas bajo el contrato, y los permisos, derechos, restricciones y consecuencias relacionadas y que surjan de lo anterior.|
|`Término y terminación`|Elementos que hacen referencia a la duración del contrato, la planificación y los términos de la terminación del contrato, y las consecuencias de la terminación, incluidas las obligaciones que se aplican a la terminación o después de la misma.|
|`Garantías`      |Elementos que hacen referencia específicamente a asunciones de fondo y en segundo plano en las que se pueden basar las partes. Las consecuencias del incumplimiento de las garantías también están dentro de esta categoría.|

## Atributos
{: #attributes}

La matriz `atributos` especifica los atributos identificados en la frase. Cada objeto de la matriz incluye tres claves: `tipo` (el tipo de atributo de la tabla siguiente), `texto` (el texto aplicable) y `atributo` (los puntos de inicio y de final del atributo en el documento). Los atributos soportados actualmente incluyen:

| `atributos`     |Descripción                                                |
|:----------------:|-----------------------------------------------------------|
|`Ubicación`        |Una región o ubicación geográfica.                         |
|`DateTime`        |Una fecha, una hora, un rango de fechas o un rango de horas.                   |
|`Moneda`        |Valor monetario y unidades.                                  |

# Seguros
{: #assurance}

{{site.data.keyword.cnc_short}} proporciona una valoración de fiabilidad a cada elemento `tipo` o `categoría` que identifica. En este momento hay un valor de control, `Alto`, que indica que hay pruebas significativas de que la clasificación listada es representativa del contenido.

## Origen
{: #provenance}

Cada objeto de las matrices `tipos` y `categorías` incluye un objeto `procedencia`. El objeto `procedencia` tiene una o varias claves `id`. Cada clave `id` tiene un valor de hash que puede enviar a IBM para proporcionar comentarios o recibir soporte.

