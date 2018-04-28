---
title: Publikování ASP.NET Core SignalR aplikace do webové aplikace Azure
author: rachelappel
description: Publikování ASP.NET Core SignalR aplikace do webové aplikace Azure
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: fd7d38ad47d9004db2ae7b5858dc22609943f601
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/28/2018
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Publikování ASP.NET Core SignalR aplikace do webové aplikace Azure

[Webové aplikace Azure](/azure/app-service/app-service-web-overview) je [Microsoft cloud computing](https://azure.microsoft.com/) služby platformu pro hostování webových aplikací, včetně ASP.NET Core.

## <a name="publish-the-app"></a>Publikování aplikace

Visual Studio poskytuje integrované nástroje pro publikování do webové aplikace Azure. Visual Studio Code uživatel může použít [rozhraní příkazového řádku Azure](/cli/azure) příkazy k publikování aplikace do Azure. Tento článek se zabývá publikování pomocí nástrojů v sadě Visual Studio. Publikování aplikace pomocí příkazového řádku Azure CLI, najdete v části [publikovat aplikaci ASP.NET Core do Azure pomocí nástroje příkazového řádku](xref:tutorials/publish-to-azure-webapp-using-cli).

Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **publikovat**. Potvrďte, že **vytvořit nový** se změnami **vyberte cíl publikování** dialogové okno a vyberte **publikovat**.

![Vybrat publikovat cíl](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Zadejte následující informace v **vytvořit službu App Service** dialogové okno a vyberte **vytvořit**.

| Položka | Popis |
| ---- | ----------- |
| **Název aplikace.** | Jedinečný název aplikace. |
| **Předplatné** | Předplatné Azure, který používá aplikace. |
| **Skupiny prostředků** | Skupina souvisejících prostředků, do kterých patří aplikace.  |
| **Hostování plán** | Cenový plán pro webovou aplikaci. |

![Vytvoření služby app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio dokončí následující úkoly:

* Vytvoří profil publikování obsahující nastavení publikování.
* Vytvoří nebo použije existující *webové aplikace Azure* s zadané podrobnosti.
* Publikuje aplikace.
* Spustí prohlížeč, se publikované webové aplikace načíst.

Všimněte si formát adresy URL pro aplikaci je *{aplikace} .azurewebsites .net*. Například aplikaci s názvem `SignalRChattR` má adresu URL, která vypadá jako `https://signalrchattr.azurewebsites.net`.

Pokud dojde k chybě HTTP 502.2, přečtěte si téma [verze preview nasazení základní technologie ASP.NET do služby Azure App Service](xref:host-and-deploy/azure-apps/index) jeho řešení.

## <a name="configure-signalr-web-app"></a>Konfigurace funkce SignalR webové aplikace

Aplikace ASP.NET Core SignalR, které jsou publikovány jako webové aplikace Azure musí mít [směrování žádostí na aplikace spřažení](https://en.wikipedia.org/wiki/Application_Request_Routing) povolena. [Technologie WebSockets](xref:fundamentals/websockets) by měla povoleno, umožňuje přenos Websocket funkce.

Na portálu Azure přejděte do **nastavení aplikace** pro vaši webovou aplikaci. Nastavit **Websocket** k **na**a ověřte **směrování žádostí na aplikace spřažení** je **na**.

![Azure nastavení webové aplikace na portálu Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 Technologie WebSockets a ostatní přenosy [jsou omezeny na plán aplikační služby na základě](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Související informační zdroje

* [Publikování aplikace ASP.NET Core do Azure pomocí nástroje příkazového řádku](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [Publikování aplikace ASP.NET Core do Azure pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Hostování a nasazení aplikací ASP.NET Core Preview v Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
