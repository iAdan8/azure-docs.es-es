---
title: Matriz de compatibilidad para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: Aquí se resumen los sistemas operativos y los componentes admitidos para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure mediante Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: raynew
ms.openlocfilehash: 8115065afcbd81da1527e09c07ca89ce89100d7d
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56236998"
---
# <a name="support-matrix-for-disaster-recovery--of-vmware-vms-and-physical-servers-to-azure"></a>Matriz de compatibilidad para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure.

En este artículo se resumen los ajustes y los componentes admitidos para la recuperación ante desastres de máquinas virtuales de VMware en Azure mediante [Azure Site Recovery](site-recovery-overview.md).

Para empezar a usar Azure Site Recovery con el escenario de implementación más sencillo, visite nuestros [tutoriales](tutorial-prepare-azure.md). Puede aprender más sobre la arquitectura de Azure Site Recovery [aquí](vmware-azure-architecture.md).

## <a name="replication-scenario"></a>Escenario de replicación

**Escenario** | **Detalles**
--- | ---
Máquinas virtuales de VMware | Replicación de máquinas virtuales de VMware locales en Azure. Puede implementar este escenario en Azure Portal o mediante [PowerShell](vmware-azure-disaster-recovery-powershell.md).
Servidores físicos | Replicación de servidores Windows/Linux físicos locales en Azure. Puede implementar este escenario en Azure Portal.

## <a name="on-premises-virtualization-servers"></a>Servidores de virtualización locales

**Servidor** | **Requisitos** | **Detalles**
--- | --- | ---
VMware | vCenter Server 6.7, 6.5, 6.0 o 5.5, o vSphere 6.7, 6.5, 6.0 o 5.5 | Se recomienda usar vCenter Server.<br/><br/> Se recomienda que los hosts de vSphere y los servidores vCenter se encuentren en la misma red que el servidor de procesos. De forma predeterminada, los componentes del servidor de procesos se ejecutan en el servidor de configuración, por lo esta será la red en la que se configurará el servidor de configuración, a menos que elija un servidor de procesos especializado.
Física | N/D

## <a name="site-recovery-configuration-server"></a>Servidor de configuración de Site Recovery

El servidor de configuración es una máquina local que ejecuta componentes de Site Recovery locales, incluido el servidor de configuración, el servidor de procesos y el servidor de destino maestro. Para la replicación de VMware se configura el servidor de configuración con todos los requisitos, mediante una plantilla OVF para crear una máquina virtual de VMware. Para la replicación de un servidor físico, la máquina del servidor de configuración se configura manualmente.

**Componente** | **Requisitos**
--- |---
Núcleos de CPU | 8
RAM | 16 GB
Número de discos | 3 discos<br/><br/> Los discos incluyen el disco del sistema operativo, el disco de memoria caché del servidor de procesos y la unidad de retención para la conmutación por recuperación.
Espacio libre en el disco | 600 GB de espacio necesarios para la memoria caché del servidor de procesos.
Espacio libre en el disco | 600 GB de espacio necesarios para la unidad de retención.
Sistema operativo  | Windows Server 2012 R2 o Windows Server 2016 |
Configuración regional del sistema operativo | Español (es-es)
PowerCLI | Se debe instalar [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0").
Roles de Windows Server | No habilite: <br/> - Active Directory Domain Services <br/>- Internet Information Services <br/> - Hyper-V |
Directivas de grupo| No habilite: <br/> - Impedir el acceso al símbolo del sistema. <br/> - Impedir el acceso a herramientas de edición del Registro. <br/> - Confiar en la lógica de datos adjuntos de archivos. <br/> - Activar la ejecución de scripts. <br/> [Más información](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
IIS | Asegúrese de lo siguiente:<br/><br/> - No hay ningún sitio web preexistente predeterminado <br/> - Habilitar la [autenticación anónima](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br/> - Habilitar la configuración de [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx)  <br/> - No hay ningún sitio web o aplicación preexistente que escuche en el puerto 443<br/>
Tipo de NIC | VMXNET3 (cuando se implementa como una máquina virtual VMware)
Tipo de dirección IP | estática
Puertos | 443 se usa para la orquestación del canal de control<br/>9443 se usa para el transporte de datos

## <a name="replicated-machines"></a>Máquinas replicadas

Site Recovery admite la replicación de cualquier carga de trabajo que se ejecute en una máquina compatible.

**Componente** | **Detalles**
--- | ---
Configuración del equipo | Las máquinas que se replican en Azure deben cumplir con los [requisitos de Azure](#azure-vm-requirements).
Carga de trabajo del equipo | Site Recovery admite la replicación de cualquier carga de trabajo (por ejemplo, Active Directory, SQL Server, etc.) que se ejecute en una máquina compatible. Para más información, haga clic [aquí](https://aka.ms/asr_workload)
Sistema operativo Windows | Windows Server 2019 de 64 bits, Windows Server 2016 de 64 bits (Server Core, Server con Experiencia de escritorio), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 con al menos SP1. </br></br>  [Windows Server 2008 con al menos SP2: 32 y 64 bits](migrate-tutorial-windows-server-2008.md) (solo en la migración). </br></br> No se admite Windows 2016 Nano Server.
Sistema operativo Linux | Red Hat Enterprise Linux: Desde 5.2 hasta 5.11<b>\*\*</b>, desde 6.1 hasta 6.10<b>\*\*</b> y desde 7.0 hasta 7.6 <br/><br/>CentOS: Desde 5.2 hasta 5.11<b>\*\*</b>, desde 6.1 hasta 6.10<b>\*\*</b> y desde 7.0 hasta 7.6 <br/><br/>Servidor Ubuntu 14.04 LTS[ (versiones de kernel admitidas)](#ubuntu-kernel-versions)<br/><br/>Servidor Ubuntu 16.04 LTS[ (versiones de kernel admitidas)](#ubuntu-kernel-versions)<br/><br/>Debian 7/Debian 8[ (versiones de kernel admitidas)](#debian-kernel-versions)<br/><br/>SUSE Linux Enterprise Server 12 SP1, SP2, SP3 [ (versiones de kernel admitidas)](#suse-linux-enterprise-server-12-supported-kernel-versions)<br/><br/>SUSE Linux Enterprise Server 11 SP3<b>\*\*</b>, SUSE Linux Enterprise Server 11 SP4 * </br></br>Oracle Linux 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5 que ejecuten el kernel compatible de Red Hat o Unbreakable Enterprise Kernel Release 3 (UEK3) <br/><br/></br>- No se admite la actualización de máquinas replicadas de SUSE Linux Enterprise Server 11 SP3 a SP4. Para actualizar, deshabilite la replicación y habilítela de nuevo después de la actualización.</br></br> - [Obtenga más información](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) sobre la compatibilidad con Linux y la tecnología de código abierto en Azure. Site Recovery organiza la conmutación por error para ejecutar servidores Linux en Azure. Sin embargo, los proveedores de Linux podrían limitar la compatibilidad a solo las versiones de distribución que no hayan alcanzado el final del ciclo de vida.<br/><br/> - En las distribuciones de Linux, solo se admiten kernels de stock que forman parte del lanzamiento o la actualización de la versión secundaria de la distribución.<br/><br/> - La actualización de máquinas protegidas entre distribuciones de versiones principales de Linux no se admite. Para actualizar, deshabilite la replicación, actualice el sistema operativo y, a continuación, habilite la replicación de nuevo.<br/><br/> - Los servidores con Red Hat Enterprise Linux 5.2-5.11 o CentOS 5.2-5.11 deben tener los [componentes de Linux Integration Services (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) instalados para que las máquinas puedan iniciar Azure.

### <a name="ubuntu-kernel-versions"></a>Versiones de kernel de Ubuntu


**Versión compatible** | **Versión de Azure Site Recovery Mobility Service** | **Versión de kernel** |
--- | --- | --- |
14.04 LTS | [9.22][9.22 UR] | 3.13.0-24-generic a 3.13.0-164-generic,<br/>3.16.0-25-generic a 3.16.0-77-generic,<br/>3.19.0-18-generic a 3.19.0-80-generic,<br/>4.2.0-18-generic a 4.2.0-42-generic,<br/>4.4.0-21-generic a 4.4.0-140-generic,<br/>4.15.0-1023-azure a 4.15.0-1036-azure |
14.04 LTS | [9.21][9.21 UR] | 3.13.0-24-generic a 3.13.0-163-generic,<br/>3.16.0-25-generic a 3.16.0-77-generic,<br/>3.19.0-18-generic a 3.19.0-80-generic,<br/>4.2.0-18-generic a 4.2.0-42-generic,<br/>4.4.0-21-generic a 4.4.0-140-generic,<br/>4.15.0-1023-azure a 4.15.0-1035-azure |
14.04 LTS | [9.20][9.20 UR] | 3.13.0-24-generic a 3.13.0-153-generic,<br/>3.16.0-25-generic a 3.16.0-77-generic,<br/>3.19.0-18-generic a 3.19.0-80-generic,<br/>4.2.0-18-generic a 4.2.0-42-generic,<br/>4.4.0-21-generic a 4.4.0-138-generic,<br/>4.15.0-1023-azure a 4.15.0-1025-azure |
14.04 LTS | [9.19][9.19 UR] | 3.13.0-24-generic a 3.13.0-153-generic,<br/>3.16.0-25-generic a 3.16.0-77-generic,<br/>3.19.0-18-generic a 3.19.0-80-generic,<br/>4.2.0-18-generic a 4.2.0-42-generic,<br/>4.4.0-21-generic a 4.4.0-131-generic |
|||
16.04 LTS | [9.22][9.22 UR] | 4.4.0-21-generic a 4.4.0-140-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-43-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1036-azure|
16.04 LTS | [9.21][9.21 UR] | 4.4.0-21-generic a 4.4.0-140-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-42-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1035-azure|
16.04 LTS | [9.20][9.20 UR] | 4.4.0-21-generic a 4.4.0-138-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-38-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1025-azure|
16.04 LTS | [9.19][9.19 UR] | 4.4.0-21-generic a 4.4.0-131-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-30-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1019-azure|

### <a name="debian-kernel-versions"></a>Versiones de kernel de Debian


**Versión compatible** | **Versión de Azure Site Recovery Mobility Service** | **Versión de kernel** |
--- | --- | --- |
Debian 7 | [9.19][9.19 UR],[9.20][9.20 UR],[9.21][9.21 UR], [9.22][9.22 UR]| 3.2.0-4-amd64 a 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | [9.20][9.20 UR],[9.21][9.21 UR],[9.22][9.22 UR] | 3.16.0-4-amd64 a 3.16.0-7-amd64, 4.9.0-0.bpo.4-amd64 a 4.9.0-0.bpo.8-amd64 |
Debian 8 | [9.19][9.19 UR] | 3.16.0-4-amd64 a 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 a 4.9.0-0.bpo.7-amd64 |


### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>Versiones de kernel admitidas de SUSE Linux Enterprise Server 12

**Versión** | **Versión de Mobility service** | **Versión de kernel** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | [9.22][9.22 UR] | SP1 3.12.49-11-default a 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default a 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default a 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default a 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default a 4.4.162-94.72-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | [9.21][9.21 UR] | SP1 3.12.49-11-default a 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default a 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default a 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default a 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default a 4.4.156-94.72-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | [9.20][9.20 UR] | SP1 3.12.49-11-default a 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default a 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default a 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default a 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default a 4.4.156-94.64-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | [9.19][9.19 UR] | SP1 3.12.49-11-default a 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default a 3.12.74-60.64.96-default</br></br> SP2 4.4.21-69-default a 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default a 4.4.121-92.85-default</br></br>SP3 4.4.73-5-default a 4.4.140-94.42-default |

## <a name="linux-file-systemsguest-storage"></a>Sistemas de archivos Linux/almacenamiento de invitados

**Componente** | **Compatible**
--- | ---
Sistemas de archivos | ext3, ext4, XFS
Administrador de volúmenes | Antes de la [versión 9.20](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), <br/> 1. LVM2 es compatible. <br/> 2. Se admite LVM solo para discos de datos. <br/> 3. Las máquinas virtuales de Azure tienen un único disco de sistema operativo.<br/><br/>A partir de la [versión 9.20](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), LVM y LVM2 son compatibles.
Dispositivos de almacenamiento paravirtualizados | No se admiten dispositivos que se hayan exportado con controladores paravirtualizados.
Dispositivos de E/S de bloque multicola | No compatible.
Servidores físicos con controlador de almacenamiento HP CCISS | No compatible.
Convención de nomenclatura de punto de montaje/dispositivo | El nombre de dispositivo o el nombre de punto de montaje debe ser único. Asegúrese de que los nombres de dos dispositivos o puntos de montaje no solo se diferencien en las mayúsculas y minúsculas. </br> Ejemplo: no puede asignarle a dos dispositivos de la misma máquina virtual el nombre *device1* y *Device1*.
Directorios | Antes de la [versión 9.20](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), <br/> 1. Los siguientes directorios (si están configurados como sistemas de archivos o particiones independientes) deben estar en el mismo disco de sistema operativo del servidor de origen: /(root), /boot, /usr, /usr/local, /var, /etc.</br>2. /boot debe estar en una partición de disco y no en un volumen LVM.<br/><br/> A partir de la [versión 9.20](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), las restricciones mencionadas no son aplicables. No se admite /boot en el volumen LVM en más de un disco.
Directorio raíz | No se admiten varios discos de arranque en una máquina virtual. <br/><br/> Una máquina sin disco de arranque no se puede proteger.

Requisitos de espacio libre | 2 GB en la partición /root <br/><br/> 250 MB en la carpeta de instalación XFSv5 | Las características de XFSv5 en sistemas de archivos XFS, como la suma de comprobación de metadatos, se admiten a partir de la versión 9.10 de Mobility Service. Puede usar la utilidad xfs_info para comprobar el superbloque XFS de la partición. Si ftype está establecido en 1, entonces se usan las características de XFSv5.

## <a name="vmdisk-management"></a>Administración de máquinas virtuales o discos

**Acción** | **Detalles**
--- | ---
Cambiar el tamaño de disco en una máquina virtual replicada |  Se admite.
Agregar disco a una máquina virtual replicada | Deshabilite la replicación para la máquina virtual, agregue el disco y vuelva a habilitar la replicación. Agregar un disco en una máquina virtual de replicación no se admite en la actualidad.

## <a name="network"></a>Red

**Componente** | **Compatible**
--- | ---
Formación de equipos de adaptador de red de la red de host | Se admite en máquinas virtuales de VMware. <br/><br/>No se admite en la replicación de máquinas físicas.
VLAN de la red de host | Sí.
IPv4 de la red de host | Sí.
IPv6 de la red de host |  No.
Formación de equipos de adaptador de red de la red de invitado o de servidor |  No.
IPv4 de la red de invitado o de servidor | Sí.
IPv6 de la red de invitado o de servidor |  No.
IP estática de la red de invitado o de servidor (Windows) | Sí.
IP estática de la red de invitado o de servidor (Linux) | Sí. <br/><br/>Las máquinas virtuales se configuran para usar DHCP en la conmutación por recuperación.
Varios adaptadores de red de la red de invitado o de servidor | Sí.


## <a name="azure-vm-network-after-failover"></a>Red de máquinas virtuales de Azure (después de la conmutación por error)

**Componente** | **Compatible**
--- | ---
Azure ExpressRoute | Sí
ILB | Sí
ELB | Sí
Administrador de tráfico de Azure | Sí
Varias NIC | Sí
Dirección IP reservada | Sí
IPv4 | Sí
Conservar la dirección IP de origen | Sí
Puntos de conexión del servicio Azure Virtual Network<br/> (sin firewalls de Azure Storage) | Sí
Redes aceleradas | Sin 

## <a name="storage"></a>Almacenamiento
**Componente** | **Compatible**
--- | ---
Disco dinámico | El disco del sistema operativo debe ser un disco básico. <br/><br/>Los discos de datos pueden ser discos dinámicos.
Configuración del disco de Docker | Sin 
NFS de host | Sí para VMware<br/><br/> No para servidores físicos
SAN de host (ISCSI/FC) | Sí
vSAN de host | Sí para VMware<br/><br/> N/D para servidores físicos
Varias rutas de host (MPIO) | Sí, probado con DSM de Microsoft, EMC PowerPath 5.7 SP4, EMC PowerPath DSM para CLARiiON.
Hospedaje de volúmenes virtuales (VVols) | Sí para VMware<br/><br/> N/D para servidores físicos
VMDK de invitado/servidor | Sí
EFI/UEFI de invitado/servidor| Parcial (migración a Azure para Windows Server 2012 y versiones posteriores) <br/><br/> Consulte la nota al final de la tabla.
Disco de clúster compartido de invitado/servidor | Sin 
Disco cifrado de invitado/servidor | Sin 
NFS de invitado/servidor | Sin 
SMB 3.0 de invitado/servidor | Sin 
RDM de invitado/servidor | Sí<br/><br/> N/D para servidores físicos
Disco de invitado/servidor > 1 TB | Sí<br/><br/>Hasta 4095 GB<br/><br/> El disco debe ser de un tamaño superior a 1024 MB.
Disco de invitado/servidor con tamaño de sector físico de 4K y lógico de 4K | Sí
Disco de invitado/servidor con tamaño de sector físico de 512 bytes y lógico de 4K | Sí
Volumen de invitado/servidor con disco seccionado > 4 TB <br/><br/>Administración de volúmenes lógicos (LVM)| Sí
Invitado/servidor: espacios de almacenamiento | Sin 
Invitado/servidor: adición/eliminación de disco en caliente | Sin 
Invitado/servidor: disco de exclusión | Sí
Varias rutas (MPIO) de invitado/servidor | Sin 

> [!NOTE]
> Es posible migrar a Azure las máquinas virtuales de VMware con arranque UEFI que ejecuten Windows Server 2012 o versiones posteriores. Se aplican las restricciones que se indican a continuación:

> - Se admite solo la migración a Azure. No se admite la conmutación por recuperación a sitios de VMware locales.
> - El servidor no debe tener más de cuatro particiones en el disco del sistema operativo.
> - Requiere la versión de Mobility Service 9.13 o una posterior.

## <a name="azure-storage"></a>Almacenamiento de Azure

**Componente** | **Compatible**
--- | ---
Almacenamiento con redundancia local | Sí
Almacenamiento con redundancia geográfica | Sí
Almacenamiento con redundancia geográfica con acceso de lectura | Sí
Almacenamiento de acceso esporádico | Sin 
Almacenamiento de acceso frecuente| Sin 
Blobs en bloques | Sin 
Cifrado en reposo (Cifrado del servicio Storage)| Sí
Premium Storage | Sí
Servicio Import/Export | Sin 
Los firewalls de Azure Storage para redes virtuales se configuran en la cuenta de almacenamiento o la cuenta de almacenamiento en caché de destino (se usa para almacenar datos de replicación) | Sin 
Cuentas de almacenamiento de uso general v2 (niveles de acceso frecuente y esporádico) | Sin 

## <a name="azure-compute"></a>Azure Compute

**Característica** | **Compatible**
--- | ---
Conjuntos de disponibilidad | Sí
CONCENTRADOR | Sí
Discos administrados | Sí

## <a name="azure-vm-requirements"></a>Requisitos de VM de Azure

Las máquinas virtuales locales que se replican en Azure deben cumplir con los requisitos de VM de Azure que se resumen en esta tabla. Cuando Site Recovery ejecuta una comprobación de requisitos previos, se producirá un error si no se cumplen algunos de los requisitos.

**Componente** | **Requisitos** | **Detalles**
--- | --- | ---
Sistema operativo invitado | Compruebe los [sistemas operativos compatibles](#replicated-machines) con máquinas replicadas. | Se produce un error en la comprobación si no es compatible.
Arquitectura del sistema operativo invitado | 64 bits | Se produce un error en la comprobación si no es compatible.
Tamaño del disco del sistema operativo | Hasta 2048 GB | Se produce un error en la comprobación si no es compatible.
Número de discos del sistema operativo | 1 | Se produce un error en la comprobación si no es compatible.
Número de discos de datos | 64 o menos | Se produce un error en la comprobación si no es compatible.
Tamaño del disco de datos | Hasta 4095 GB | Se produce un error en la comprobación si no es compatible.
Adaptadores de red | Se admiten varios adaptadores. |
VHD compartido | No compatible. | Se produce un error en la comprobación si no es compatible.
Disco FC | No compatible. | Se produce un error en la comprobación si no es compatible.
BitLocker | No compatible. | Debe deshabilitar BitLocker antes de habilitar la replicación de una máquina. |
Nombre de la máquina virtual | Entre 1 y 63 caracteres.<br/><br/> Restringido a letras, números y guiones.<br/><br/> El nombre de la máquina debe empezar y terminar con una letra o un número. |  Actualice el valor de las propiedades de la máquina en Site Recovery.


## <a name="vault-tasks"></a>Tareas de almacén

**Acción** | **Compatible**
--- | ---
Mover el almacén entre grupos de recursos<br/><br/> Entre las suscripciones | Sin 
Mover el almacenamiento, la red y las máquinas virtuales de Azure entre grupos de recursos<br/><br/> Entre las suscripciones | Sin 


## <a name="download-latest-azure-site-recovery-components"></a>Descargar los componentes más recientes de Azure Site Recovery

**Nombre** | **Descripción** | **Instrucciones de descarga de la versión más reciente**
--- | --- | --- | --- | ---
Servidor de configuración | Coordina las comunicaciones entre servidores de VMware locales y Azure  <br/><br/>  Se instala en servidores de VMware locales | Si es una instalación nueva, haga clic [aquí](vmware-azure-deploy-configuration-server.md). Para actualizar un componente existente a la versión más reciente, haga clic [aquí](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server).
Servidor de proceso|Se instala de forma predeterminada en el servidor de configuración. Recibe los datos de la replicación; los optimiza mediante almacenamiento en caché, compresión y cifrado, y los envía a Azure Storage. A medida que crece la implementación, puede agregar más servidores de procesos independientes para controlar mayores volúmenes de tráfico de replicación.| Si es una instalación nueva, haga clic [aquí](vmware-azure-set-up-process-server-scale.md). Para actualizar un componente existente a la versión más reciente, haga clic [aquí](vmware-azure-manage-process-server.md#upgrade-a-process-server).
Mobility Service | Coordina la replicación entre servidores de VMware locales o servidores físicos y el sitio secundario o Azure<br/><br/> Se instala en cada máquina virtual de VMware o servidor físico que se va a replicar | Si es una instalación nueva, haga clic [aquí](vmware-azure-install-mobility-service.md). Para actualizar un componente existente a la versión más reciente, haga clic [aquí](vmware-physical-mobility-service-overview.md#update-the-mobility-service).

Para conocer las características y correcciones más recientes, haga clic [aquí](https://aka.ms/ASR_latest_release_notes).


## <a name="next-steps"></a>Pasos siguientes
[Aprenda](tutorial-prepare-azure.md) a preparar Azure para la recuperación ante desastres de las máquinas virtuales de VMware.

[9.22 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.21 UR]: https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery
[9.20 UR]: https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery
[9.19 UR]: https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30
[9.18 UR]: https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery
