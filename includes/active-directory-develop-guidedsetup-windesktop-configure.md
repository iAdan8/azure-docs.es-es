---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: b26b88d0e089217fa9915bdbdcb8f913731bcc67
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988215"
---
## <a name="register-your-application"></a>Registrar su aplicación

Puede registrar la aplicación de dos maneras.

### <a name="option-1-express-mode"></a>Opción 1: Modo rápido

Puede registrar rápidamente la aplicación mediante estos pasos:
1. Vaya al [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure).

2. Seleccione **Agregar una aplicación**.

3. En el cuadro**Application Name** (Nombre de la aplicación) escriba el nombre de su aplicación.

4. Asegúrese de que la casilla **Guided Setup** (Instalación guiada) esté activada y, luego, seleccione **Create** (Crear).

5. Siga las instrucciones para obtener el identificador de aplicación y péguelo en el código.

### <a name="option-2-advanced-mode"></a>Opción 2: Modo avanzado

Para registrar la aplicación y agregar la información de registro de aplicación a la solución, siga estos pasos:
1. Si aún no ha registrado la aplicación, vaya al [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/portal/register-app).

2. Seleccione **Agregar una aplicación**.

3. En el cuadro**Application Name** (Nombre de la aplicación) escriba el nombre de su aplicación.

4. Asegúrese de que la casilla **Guided Setup** (Instalación guiada) esté desactivada y, luego, seleccione **Create** (Crear).

5. Seleccione **Add Platform** (Agregar plataforma), seleccione **Native Application** (Aplicación nativa) y, luego, seleccione **Save** (Guardar).

6. En el cuadro **Application ID** (Id. de aplicación), copie el GUID.

7. Vaya a Visual Studio, abra el archivo *App.xaml.cs* y, luego, reemplace `your_client_id_here` por el identificador de aplicación que acaba de registrar y copiar.

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```
