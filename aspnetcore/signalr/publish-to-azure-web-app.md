---
title: Publikování ASP.NET Core SignalR aplikace do webové aplikace Azure
author: rachelappel
description: Publikování ASP.NET Core SignalR aplikace do webové aplikace Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 0d98c6b24b9695c0af0170173f13902bac5f55ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271915"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="00b2a-103">Publikování ASP.NET Core SignalR aplikace do webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="00b2a-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="00b2a-104">[Webové aplikace Azure](/azure/app-service/app-service-web-overview) je [Microsoft cloud computing](https://azure.microsoft.com/) služby platformu pro hostování webových aplikací, včetně ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="00b2a-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="00b2a-105">Tento článek se týká k publikování aplikace ASP.NET Core SignalR ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="00b2a-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="00b2a-106">Navštivte [SignalR služby pro Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) pro další informace o používání SignalR v Azure.</span><span class="sxs-lookup"><span data-stu-id="00b2a-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="00b2a-107">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="00b2a-107">Publish the app</span></span>

<span data-ttu-id="00b2a-108">Visual Studio poskytuje integrované nástroje pro publikování do webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="00b2a-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="00b2a-109">Visual Studio Code uživatel může použít [rozhraní příkazového řádku Azure](/cli/azure) příkazy k publikování aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="00b2a-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="00b2a-110">Tento článek se zabývá publikování pomocí nástrojů v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="00b2a-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="00b2a-111">Publikování aplikace pomocí příkazového řádku Azure CLI, najdete v části [publikovat aplikaci ASP.NET Core do Azure pomocí nástroje příkazového řádku](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="00b2a-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="00b2a-112">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="00b2a-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="00b2a-113">Potvrďte, že **vytvořit nový** se změnami **vyberte cíl publikování** dialogové okno a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="00b2a-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Vybrat publikovat cíl](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="00b2a-115">Zadejte následující informace v **vytvořit službu App Service** dialogové okno a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="00b2a-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="00b2a-116">Položka</span><span class="sxs-lookup"><span data-stu-id="00b2a-116">Item</span></span> | <span data-ttu-id="00b2a-117">Popis</span><span class="sxs-lookup"><span data-stu-id="00b2a-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="00b2a-118">**Název aplikace.**</span><span class="sxs-lookup"><span data-stu-id="00b2a-118">**App name**</span></span> | <span data-ttu-id="00b2a-119">Jedinečný název aplikace.</span><span class="sxs-lookup"><span data-stu-id="00b2a-119">A unique name of the app.</span></span> |
| <span data-ttu-id="00b2a-120">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="00b2a-120">**Subscription**</span></span> | <span data-ttu-id="00b2a-121">Předplatné Azure, který používá aplikace.</span><span class="sxs-lookup"><span data-stu-id="00b2a-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="00b2a-122">**Skupiny prostředků**</span><span class="sxs-lookup"><span data-stu-id="00b2a-122">**Resource Group**</span></span> | <span data-ttu-id="00b2a-123">Skupina souvisejících prostředků, do kterých patří aplikace.</span><span class="sxs-lookup"><span data-stu-id="00b2a-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="00b2a-124">**Hostování plán**</span><span class="sxs-lookup"><span data-stu-id="00b2a-124">**Hosting Plan**</span></span> | <span data-ttu-id="00b2a-125">Cenový plán pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="00b2a-125">The pricing plan for the web app.</span></span> |

![Vytvoření služby app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="00b2a-127">Visual Studio dokončí následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="00b2a-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="00b2a-128">Vytvoří profil publikování obsahující nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="00b2a-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="00b2a-129">Vytvoří nebo použije existující *webové aplikace Azure* s zadané podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="00b2a-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="00b2a-130">Publikuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="00b2a-130">Publishes the app.</span></span>
* <span data-ttu-id="00b2a-131">Spustí prohlížeč, se publikované webové aplikace načíst.</span><span class="sxs-lookup"><span data-stu-id="00b2a-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="00b2a-132">Všimněte si formát adresy URL pro aplikaci je *{aplikace} .azurewebsites .net*.</span><span class="sxs-lookup"><span data-stu-id="00b2a-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="00b2a-133">Například aplikaci s názvem `SignalRChattR` má adresu URL, která vypadá jako `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="00b2a-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="00b2a-134">Pokud dojde k chybě HTTP 502.2, přečtěte si téma [verze preview nasazení základní technologie ASP.NET do služby Azure App Service](xref:host-and-deploy/azure-apps/index) jeho řešení.</span><span class="sxs-lookup"><span data-stu-id="00b2a-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="00b2a-135">Konfigurace funkce SignalR webové aplikace</span><span class="sxs-lookup"><span data-stu-id="00b2a-135">Configure SignalR web app</span></span>

<span data-ttu-id="00b2a-136">Aplikace ASP.NET Core SignalR, které jsou publikovány jako webové aplikace Azure musí mít [směrování žádostí na aplikace spřažení](https://en.wikipedia.org/wiki/Application_Request_Routing) povolena.</span><span class="sxs-lookup"><span data-stu-id="00b2a-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="00b2a-137">[Technologie WebSockets](xref:fundamentals/websockets) by měla povoleno, umožňuje přenos Websocket funkce.</span><span class="sxs-lookup"><span data-stu-id="00b2a-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="00b2a-138">Na portálu Azure přejděte do **nastavení aplikace** pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="00b2a-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="00b2a-139">Nastavit **Websocket** k **na**a ověřte **směrování žádostí na aplikace spřažení** je **na**.</span><span class="sxs-lookup"><span data-stu-id="00b2a-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Azure nastavení webové aplikace na portálu Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="00b2a-141">Technologie WebSockets a ostatní přenosy [jsou omezeny na plán aplikační služby na základě](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="00b2a-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="00b2a-142">Související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="00b2a-142">Related resources</span></span>

* [<span data-ttu-id="00b2a-143">Publikování aplikace ASP.NET Core do Azure pomocí nástroje příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="00b2a-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="00b2a-144">Publikování aplikace ASP.NET Core do Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="00b2a-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="00b2a-145">Hostování a nasazení aplikací ASP.NET Core Preview v Azure</span><span class="sxs-lookup"><span data-stu-id="00b2a-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
