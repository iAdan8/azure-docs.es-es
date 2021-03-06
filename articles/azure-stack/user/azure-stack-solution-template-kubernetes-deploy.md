---
title: Implementación de Kubernetes en Azure Stack | Microsoft Docs
description: Aprenda a implementar Kubernetes en Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: c96fdbfb56fe5274de941c0f9ab1a12cbcc241b6
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55658426"
---
# <a name="deploy-kubernetes-to-azure-stack"></a>Implementación de Kubernetes en Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

> [!Note]  
> Kubernetes en Azure Stack está en versión preliminar. El escenario sin conexión de Azure Stack no es compatible actualmente con la versión preliminar.

Puede seguir los pasos descritos en este artículo para implementar y configurar los recursos de Kubernetes en una operación única y coordinada. Los siguientes pasos usan una plantilla de solución de Azure Resource Manager. Tendrá que recopilar la información necesaria sobre la instalación de Azure Stack, generar la plantilla y, a continuación, implementarla en la nube. La plantilla de Azure Stack no usa el mismo servicio AKS administrado que se ofrece en Azure en general.

## <a name="kubernetes-and-containers"></a>Kubernetes y contenedores

Puede instalar Kubernetes mediante plantillas de Azure Resource Manager generadas por el motor de ACS en Azure Stack. [Kubernetes](https://kubernetes.io) es un sistema de código abierto para automatizar la implementación, el escalado y la administración de aplicaciones en contenedores. Un [contenedor](https://www.docker.com/what-container) está en una imagen. La imagen de contenedor es parecida a una VM aunque, a diferencia de una VM, el contenedor tan solo incluye los recursos necesarios para ejecutar una aplicación, como el código, el tiempo de ejecución para ejecutar el código, bibliotecas específicas y la configuración.

Puede utilizar Kubernetes para:

- Desarrollar aplicaciones escalables de forma masiva y actualizables que se pueden implementar en segundos. 
- Simplificar el diseño de la aplicación y mejorar la fiabilidad mediante distintas aplicaciones de Helm. [Helm](https://github.com/kubernetes/helm) es una herramienta de empaquetado de código abierto que ayuda a instalar y administrar el ciclo de vida de las aplicaciones de Kubernetes.
- Supervisar y diagnosticar fácilmente el estado de las aplicaciones con una funcionalidad de escalado y actualización.

Solo se le cobrará por el uso de proceso requerido por los nodos que forman el clúster. Para más información, consulte [Uso y facturación en Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-billing-and-chargeback).

## <a name="deploy-kubernetes"></a>Implementación de Kubernetes

Los pasos para implementar un clúster de Kubernetes en Azure Stack dependerán de su servicio de administración de identidad. Compruebe la solución de administración de identidades utilizada por la instalación de Azure Stack. Póngase en contacto con el administrador de Azure Stack para comprobar el servicio de administración de identidad.

- **Azure Active Directory (Azure AD)**  
Para obtener instrucciones acerca de cómo instalar el clúster al usar Azure AD, consulte [Implementación de Kubernetes en Azure Stack con Azure Active Directory (Azure AD)](azure-stack-solution-template-kubernetes-azuread.md).

- **Servicios de federación de Active Directory (AD FS)**.  
Para obtener instrucciones acerca de cómo instalar el clúster al usar AD FS, consulte [Deploy Kubernetes to Azure Stack using Active Directory Federated Services (AD FS)](azure-stack-solution-template-kubernetes-adfs.md) [Implementación de Kubernetes en Azure Stack con los Servicios de federación de Active Directory (Azure AD)].

## <a name="connect-to-your-cluster"></a>Conexión al clúster

Ahora está listo para conectarse al clúster. La máquina virtual maestra se puede encontrar en el grupo de recursos del clúster y se denomina `k8s-master-<sequence-of-numbers>`. Use un cliente de SSH para conectarse a la máquina virtual maestra. En ella, puede usar **kubectl**, el cliente de línea de comandos de Kubernetes, para administrar el clúster. Para obtener instrucciones, consulte [Kubernetes.io](https://kubernetes.io/docs/reference/kubectl/overview).

Puede que también le resulte útil el administrador de paquetes **Helm** para instalar e implementar aplicaciones en el clúster. Para más instrucciones sobre cómo instalar y usar Helm con el clúster, consulte [helm.sh](https://helm.sh/).

## <a name="next-steps"></a>Pasos siguientes

[Agregar un clúster de Kubernetes a Marketplace (para el operador de Azure Stack)](../azure-stack-solution-template-kubernetes-cluster-add.md)

[Implementación de Kubernetes en Azure Stack con Azure Active Directory (Azure AD)](azure-stack-solution-template-kubernetes-azuread.md)

[Deploy Kubernetes to Azure Stack using Active Directory Federated Services (AD FS)](azure-stack-solution-template-kubernetes-adfs.md) [Implementación de Kubernetes en Azure Stack con los Servicios de federación de Active Directory (AD FS)]

[Kubernetes en Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
