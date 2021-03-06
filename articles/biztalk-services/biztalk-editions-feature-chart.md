---
title: Información acerca de las características de las ediciones de BizTalk Services | Microsoft Docs
description: 'Compare las capacidades de las ediciones de BizTalk Services: Free, Developer, Basic, Standard y Premium. MABS, WABS.'
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: c589629f-06b1-44bb-b8ca-1db71826ea59
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 8e25f98f1189f71943ece4a7877a86e29698689a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240518"
---
# <a name="biztalk-services-editions-chart"></a>BizTalk Services: gráfico de ediciones

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services ofrece varias ediciones. Use este artículo para determinar qué edición es la adecuada para sus escenario y necesidades empresariales.

## <a name="compare-the-editions"></a>Comparar las ediciones
**Free (Vista previa)**

Permite crear y administrar conexiones híbridas. Una conexión híbrida es una manera sencilla de conectar un sitio web de Azure a un recurso local, como SQL Server.

**Developer**

Incluye conexiones híbridas y procesamiento de mensajes de EAI y EDI con un portal de administración fácil de utilizar para socios comerciales, además de compatibilidad con esquemas EDI y procesamiento enriquecido EDI sobre X12 y AS2. Puede crear escenarios comunes de EAI que conectan servicios en la nube con cualquier protocolo HTTP/S, REST, FTP, WCF y SFTP para leer y escribir mensajes.  Utilice la conectividad en sistemas locales de LOB con adaptadores SAP, Oracle eBusiness, Oracle DB, Siebel y SQL Server listos para utilizar. Utilice un entorno centrado en el desarrollador con herramientas de Visual Studio para desarrollo e implementación simples. Limitado a fines de desarrollo y prueba solo sin Contrato de nivel de servicio (SLA).

**Básico**

Incluye la mayoría de las capacidades de la edición Developer con aumentos en conexiones híbridas, puentes EAI, contratos EDI y conexiones del BizTalk Adapter Pack. También ofrece alta disponibilidad y la opción para escalar con un Contrato de nivel de servicio (SLA).

**Estándar**

Incluye la mayoría de las capacidades de la edición Basic con aumentos en conexiones híbridas, puentes EAI, contratos EDI y conexiones del BizTalk Adapter Pack. También ofrece alta disponibilidad y la opción para escalar con un Contrato de nivel de servicio (SLA).

**Premium**

Incluye la mayoría de las capacidades de la edición Standard con aumentos en conexiones híbridas, puentes EAI, contratos EDI y conexiones del BizTalk Adapter Pack. También incluye el archivado, la alta disponibilidad y la opción para escalar con un Contrato de nivel de servicio (SLA).

## <a name="editions-chart"></a>Gráfico de ediciones
En la tabla siguiente se muestran las diferencias.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Free (Vista previa)</th>
        <th>Developer</th>
        <th>Básica</th>
        <th>Estándar</th>
        <th>Premium</th>
</tr>

<tr>
<td><strong>Precio de salida</strong></td>
<td colspan="5"><a HREF="https://go.microsoft.com/fwlink/p/?LinkID=304011">Precios de Azure BizTalk Services</a> <br/><br/> <a HREF="https://azure.microsoft.com/pricing/calculator/?scenario=full">Calculadora de precios de Azure</a></td>
</tr>
<tr>
<td><strong>Configuración mínima predeterminada</strong></td>
<td>1 unidad gratis</td>
<td>1 unidad Developer</td>
<td>1 unidad Basic</td>
<td>1 unidad Standard</td>
<td>1 unidad Premium</td>
</tr>
<tr>
<td><strong>Escala</strong></td>
<td>Sin escala</td>
<td>Sin escala</td>
<td>Sí, en incrementos de 1 unidad Basic</td>
<td>Sí, en incrementos de una unidad Standard</td>
<td>Sí, en incrementos de una unidad Premium</td>
</tr>
<tr>
<td><strong>Escalado horizontal máximo permitido</strong></td>
<td>Sin escala</td>
<td>Sin escala</td>
<td>Hasta ocho unidades</td>
<td>Hasta ocho unidades</td>
<td>Hasta ocho unidades</td>
</tr>
<tr>
<td><strong>Puentes EAI por unidad</strong></td>
<td>No se incluye</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>EDI, AS2</strong>
<br/><br/>
Incluye contratos TPM</td>
<td>No se incluye</td>
<td>Se incluye. 10 contratos por unidad.</td>
<td>Se incluye. 50 contratos por unidad.</td>
<td>Se incluye. 250 contratos por unidad.</td>
<td>Se incluye. 1000 contratos por unidad.</td>
</tr>
<tr>
<td><strong>Conexiones híbridas por unidad</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Transferencia de datos de conexiones híbridas (GB) por unidad</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>Conexiones del servicio BizTalk Adapter a sistemas LOB locales</strong></td>
<td>No se incluye</td>
<td>1 conexión</td>
<td>2 conexiones</td>
<td>5 conexiones</td>
<td>25 conexiones</td>
</tr>
<tr>
<td align="left"><strong>Sistemas/protocolos compatibles:</strong>
<ul>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Service Bus</li>
<li>Blob de Azure</li>
<li>API de REST</li>
</ul>
</td>
<td>No se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
</tr>
<tr>
<td><strong>Alta disponibilidad</strong>
<br/><br/>
Para ver el Acuerdo de Nivel de Servicio, consulte <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=304011">Detalles de precios de Azure BizTalk Services</a>.
</td>
<td>No se incluye</td>
<td>No se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
</tr>
<tr>
<td><strong>Copia de seguridad y restauración</strong></td>
<td>No se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
</tr>
<tr>
<td><strong>Seguimiento</strong></td>
<td>No se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
</tr>
<tr>
<td><strong>Archivado</strong><br/><br/>
Incluye la recepción sin rechazo (NRR) y la descarga de mensajes controlados</td>
<td>No se incluye</td>
<td>Se incluye</td>
<td>No se incluye</td>
<td>No se incluye</td>
<td>Se incluye</td>
</tr>
<tr>
<td><strong>Uso de código personalizado</strong></td>
<td>No se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
</tr>
<tr>
<td><strong>Uso de transformaciones, lo que incluye XSLT personalizado</strong></td>
<td>No se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
<td>Se incluye</td>
</tr>
</table>

> [!NOTE]
> Para la resistencia frente a los errores de hardware, la Alta disponibilidad implica disponer de varias máquinas virtuales en una sola unidad BizTalk.
> 
> 

## <a name="faqs"></a>Preguntas más frecuentes
#### <a name="what-is-a-biztalk-unit"></a>¿Qué es una unidad BizTalk?
Una "unidad" es el nivel atómico de una implementación de Azure BizTalk Services. Cada edición incluye una unidad con distinta memoria y capacidad de proceso. Por ejemplo, una unidad Basic tiene más proceso que Developer, Standard tiene más proceso que Basic, etc. Cuando escala un servicio de BizTalk, se escala en términos de unidades.

#### <a name="what-is-the-difference-between-biztalk-services-and-azure-biztalk-vm"></a>¿Qué diferencia hay entre BizTalk Services y una máquina virtual de Azure BizTalk?
BizTalk Services proporciona una verdadera arquitectura de plataforma como servicio (PaaS) para la creación de soluciones de integración en la nube. Con el modelo de PaaS, se puede centrar por completo en la lógica de la aplicación y dejar toda la administración de la infraestructura a Microsoft, incluyendo lo siguiente:

* No hay necesidad de administrar o revisar máquinas virtuales.
* Microsoft garantiza la disponibilidad.
* Puede controlar la escala a petición simplemente solicitando más o menos capacidad a través del portal de Azure.

BizTalk Server en Azure Virtual Machines proporciona una arquitectura de infraestructura como servicio (IaaS). Puede crear máquinas virtuales y configurarlas exactamente como el entorno local, con lo que se facilita la ejecución de aplicaciones existentes en la nube sin cambiar el código. Con IaaS, sigue siendo su responsabilidad la configuración de las máquinas virtuales, la administración de las máquinas virtuales (por ejemplo, la instalación de software y revisiones de sistema operativo) y el diseño de la aplicación para una alta disponibilidad.

Si busca crear nuevas soluciones de integración que minimicen su esfuerzo de administración de la infraestructura, utilice BizTalk Services. Si busca migrar rápidamente sus soluciones existentes de BizTalk o desea un entorno a petición para desarrollar y probar las aplicaciones de BizTalk Server, utilice BizTalk Server en la Máquina virtual de Azure.

#### <a name="what-is-the-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>¿Cuál es la diferencia entre el Servicio de adaptador de BizTalk y las conexiones híbridas?
El Servicio de adaptador de BizTalk se usa por un Servicio de BizTalk de Azure. El servicio de adaptador de BizTalk usa el Pack de adaptador de BizTalk para establecer una conexión con un sistema de línea de negocio (LOB) local. Una conexión híbrida ofrece una manera sencilla y adecuada de conectar aplicaciones de Azure, como la característica Web Apps de Azure App Service y Azure Mobile Services, a un recurso local.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-the-limit-is-reached"></a>Significado de la transferencia de datos de conexiones híbridas (GB) por unidad ¿Es por minuto/hora/día/semana/mes? ¿Qué ocurre cuando se alcanza el límite?
El costo de conexión híbrida por unidad depende de la edición de BizTalk Services. En pocas palabras, los costos dependen de la cantidad de datos que transfiera. Por ejemplo, transferir 10 GB de datos al día cuesta menos que transferir 100 GB al día. Utilice la [Calculadora de precios](https://azure.microsoft.com/pricing/calculator/?scenario=full) para que BizTalk Services determine los costos específicos. Normalmente, los límites se aplican diariamente. Si supera el límite, cualquier cobertura se cargará a un precio de $1 por GB.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-the-number-of-bridges-go-up-by-two-instead-of-just-one"></a>Cuando creo un contrato en BizTalk Services, ¿por qué el número de puentes sube en incrementos de dos, en lugar de solo uno?
Cada contrato consta de dos puentes distintos: un puente de comunicación de envío y un puente de comunicación de recepción.

#### <a name="what-happens-when-i-hit-the-quota-limit-on-the-number-of-bridges-or-agreements"></a>¿Qué ocurre cuando se alcanza el límite en la cuota del número de puentes o contratos?
No podrá implementar ningún otro puente ni crear otro acuerdo. Si desea implementar más, deberá escalar verticalmente las unidades del servicio de BizTalk o actualizar a una edición superior.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-to-another"></a>¿Cómo realizo la migración de un nivel de BizTalk Services a otro?
La edición gratuita no se puede migrar ni "escalar verticalmente" a otro nivel y tampoco es posible realizar una copia de seguridad y restaurarla a otro nivel. Si necesita otro nivel, cree un nuevo servicio de BizTalk que use el nuevo nivel. Todos los artefactos creados con la edición gratuita, incluidas las conexiones híbridas, deben volver a crearse en el nuevo servicio de BizTalk. 

En las ediciones restantes, utilice la copia de seguridad y restauración para migrar los artefactos de un nivel a otro. Por ejemplo, realice una copia de seguridad de los artefactos en el nivel estándar y restáurelos en el nivel Premium. [BizTalk Services: copias de seguridad y restauración](biztalk-backup-restore.md) describe las rutas de acceso de migración compatibles y enumera los artefactos de los que se realiza copia de seguridad. Tenga en cuenta que no se realiza copia de seguridad de las conexiones híbridas. Después de realizar una copia de seguridad y restaurarla en un nuevo nivel, debe volver a crear las conexiones híbridas.  

#### <a name="is-the-biztalk-adapter-service-included-in-the-service-how-do-i-receive-the-software"></a>¿El servicio de adaptador de BizTalk está incluido en el servicio? ¿Cómo puedo recibir el software?
Sí, el servicio de adaptador de BizTalk con BizTalk Adapter Pack está incluido en la [descarga](https://www.microsoft.com/download/details.aspx?id=39087)del SDK de Azure BizTalk Services.

## <a name="next-steps"></a>Pasos siguientes
Para crear Azure BizTalk Services en Azure Portal, vaya a [Creación de BizTalk Services mediante Azure Portal](biztalk-provision-services.md). Para comenzar a crear aplicaciones, vaya a [Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>Recursos adicionales
* [Creación de BizTalk Services mediante Azure Portal](biztalk-provision-services.md)<br/>
* [BizTalk Services: gráfico del estado de aprovisionamiento](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: pestañas Panel, Monitor y Escala](biztalk-dashboard-monitor-scale-tabs.md)<br/>
* [BizTalk Services: Backup and restore](biztalk-backup-restore.md)<br/>
* [BizTalk Services: limitaciones](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: nombre del emisor y clave del emisor](biztalk-issuer-name-issuer-key.md)<br/>
* [¿Cómo puedo comenzar a utilizar el SDK de Azure BizTalk Services?](https://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>

