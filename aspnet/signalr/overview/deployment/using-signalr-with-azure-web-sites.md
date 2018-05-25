---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Pomocí funkce SignalR webové aplikace v Azure App Service | Microsoft Docs
author: pfletcher
description: Tento dokument popisuje, jak nakonfigurovat aplikaci SignalR, která běží na Microsoft Azure. V tomto kurzu použili verze softwaru, Visual Studio 2013 nebo Vis....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="90008-104">Pomocí funkce SignalR webové aplikace v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="90008-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="90008-105">podle [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="90008-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="90008-106">Tento dokument popisuje, jak nakonfigurovat aplikaci SignalR, která běží na Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="90008-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="90008-107">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="90008-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="90008-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) nebo Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="90008-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="90008-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="90008-109">.NET 4.5</span></span>
> - <span data-ttu-id="90008-110">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="90008-110">SignalR version 2</span></span>
> - <span data-ttu-id="90008-111">2.3 Azure SDK pro Visual Studio 2013 nebo 2012</span><span class="sxs-lookup"><span data-stu-id="90008-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="90008-112">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="90008-112">Questions and comments</span></span>
> 
> <span data-ttu-id="90008-113">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="90008-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="90008-114">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), nebo [fóra Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="90008-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="90008-115">Obsah</span><span class="sxs-lookup"><span data-stu-id="90008-115">Table of Contents</span></span>

- [<span data-ttu-id="90008-116">Úvod</span><span class="sxs-lookup"><span data-stu-id="90008-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="90008-117">Nasazení funkce SignalR webové aplikace do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="90008-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="90008-118">Povolení technologie WebSockets v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="90008-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="90008-119">Pomocí propojovacího rozhraní mezipaměť Redis systému Azure</span><span class="sxs-lookup"><span data-stu-id="90008-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="90008-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90008-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="90008-121">Úvod</span><span class="sxs-lookup"><span data-stu-id="90008-121">Introduction</span></span>

<span data-ttu-id="90008-122">Funkce SignalR technologie ASP.NET umožňuje uvést novou úroveň interaktivity mezi servery a webové nebo klientů .NET.</span><span class="sxs-lookup"><span data-stu-id="90008-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="90008-123">Když jsou hostované v Azure, SignalR můžou aplikace využít vysoce dostupných, škálovatelných a původce prostředí, který běží v cloudu poskytuje.</span><span class="sxs-lookup"><span data-stu-id="90008-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="90008-124">Nasazení funkce SignalR webové aplikace do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="90008-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="90008-125">SignalR nepřidá žádné konkrétní komplikace k nasazení aplikace do Azure a nasazení na místní server.</span><span class="sxs-lookup"><span data-stu-id="90008-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="90008-126">Aplikace, která používá funkci SignalR může být hostovaný v Azure bez jakékoli změny v konfiguraci a další nastavení (přestože objekty WebSockets podporu najdete na webu [povolení technologie WebSockets v Azure App Service](#websocket) níže.) V tomto kurzu budete nasazovat aplikace vytvořené v [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md) do Azure.</span><span class="sxs-lookup"><span data-stu-id="90008-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="90008-127">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="90008-127">**Prerequisites**</span></span>

- <span data-ttu-id="90008-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="90008-128">Visual Studio 2013.</span></span> <span data-ttu-id="90008-129">Pokud nemáte Visual Studio, Visual Studio 2013 Express pro Web je součástí instalace sady Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="90008-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="90008-130">[2.3 Azure SDK pro Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) nebo [2.3 Azure SDK pro sadu Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="90008-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="90008-131">K dokončení tohoto kurzu, potřebujete předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="90008-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="90008-132">Můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), nebo [si zaregistrovat zkušební předplatné](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="90008-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="90008-133">Nasazení funkce SignalR webové aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="90008-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="90008-134">Dokončení [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md), nebo stáhnout dokončení projektu ze [galerie kódů](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="90008-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="90008-135">V sadě Visual Studio, vyberte **sestavení**, **publikování SignalR Chat**.</span><span class="sxs-lookup"><span data-stu-id="90008-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="90008-136">V dialogovém okně "Publikovat Web" Vyberte "webů systému Windows Azure".</span><span class="sxs-lookup"><span data-stu-id="90008-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Vyberte weby Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="90008-138">Pokud nejste přihlášení ke svému účtu Microsoft, klikněte na tlačítko **přihlásit...**  v dialogovém okně "Vyberte existující webový server" a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="90008-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Vyberte existující webovou stránku](using-signalr-with-azure-web-sites/_static/image2.png)    ![Přihlaste se k Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="90008-141">V dialogovém okně "Vyberte existující webový server", klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="90008-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Nový web](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="90008-143">V dialogovém okně "Při vytváření webu v systému Windows Azure" Zadejte jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="90008-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="90008-144">Vyberte oblast, která je nejblíže k vám v rozevírací nabídce oblast.</span><span class="sxs-lookup"><span data-stu-id="90008-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="90008-145">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="90008-145">Click **Create**.</span></span>

    ![Vytvořit web v Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="90008-147">V dialogovém okně "Publikovat Web", klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="90008-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![publikování webu](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="90008-149">Po dokončení publikování aplikace SignalR chatovací aplikace hostované v Azure App Service Web Apps se otevře v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="90008-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Otevření v prohlížeči lokality](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="90008-151">Povolení technologie WebSockets ve službě Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="90008-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="90008-152">Technologie WebSockets musí být explicitně povolená ve vaší webové aplikaci mají být použity v SignalR aplikace; jinak, bude třeba použít jiné protokoly (viz [přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="90008-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="90008-153">Chcete-li použít objekty WebSockets v Azure App Service Web Apps, povolte v části Konfigurace webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="90008-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="90008-154">K tomu, otevřete webovou aplikaci v [portálu pro správu Azure](https://manage.windowsazure.com/)a vyberte položku konfigurace.</span><span class="sxs-lookup"><span data-stu-id="90008-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Karta Konfigurace](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="90008-156">V horní části stránky konfigurace Ujistěte se, .NET 4.5 slouží pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="90008-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Nastavení rozhraní .NET framework verze 4.5](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="90008-158">Na stránce konfigurace v **Websocket** nastavení, vyberte **na**.</span><span class="sxs-lookup"><span data-stu-id="90008-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Technologie WebSockets nastavení: na](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="90008-160">V dolní části stránky konfigurace, vyberte **Uložit** uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="90008-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Uložit nastavení](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="90008-162">Pomocí propojovacího rozhraní mezipaměť Redis systému Azure</span><span class="sxs-lookup"><span data-stu-id="90008-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="90008-163">Pokud používáte více instancí pro vaši webovou aplikaci a uživatele těchto instancí muset vzájemné interakce (tak, aby, například chat zpráv vytvořených za jednu instanci dosáhnout uživatelů připojených k další instance), [Azure Redis Cache Propojovací rozhraní](../performance/scaleout-with-redis.md) musí být implementován v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="90008-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="90008-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90008-164">Next Steps</span></span>

<span data-ttu-id="90008-165">Další informace o webových aplikací ve službě Azure App Service naleznete v tématu [přehled Web Apps](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="90008-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
