---
uid: webhooks/diagnostics/logging
title: "Protokolování Webhooky ASP.NET | Microsoft Docs"
author: rick-anderson
description: "Postup přihlášení Webhooky ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 5691d5c394b4e606282ba302953e8a06e7d73860
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="72478-103">ASP.NET Webhooky protokolování</span><span class="sxs-lookup"><span data-stu-id="72478-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="72478-104">Microsoft ASP.NET WebHooks používá protokolování jako způsob hlášení problémů a problémů.</span><span class="sxs-lookup"><span data-stu-id="72478-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="72478-105">Ve výchozím nastavení se protokoly zapisují pomocí [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) kde je lze spravovat pomocí [trasování – moduly naslouchání](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) jako jakékoli jiné datový proud protokolu.</span><span class="sxs-lookup"><span data-stu-id="72478-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="72478-106">Při nasazení webové aplikace jako webové aplikace Azure, protokoly jsou automaticky převzata a lze je spravovat společně s jakýmikoli jiným [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) protokolování.</span><span class="sxs-lookup"><span data-stu-id="72478-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="72478-107">Podrobnosti najdete v tématu [povolit protokolování diagnostiky pro webové aplikace v Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="72478-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="72478-108">Kromě toho protokoly můžete získat přímo v prostředí Visual Studio podle popisu v [řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="72478-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="72478-109">Přesměrování protokoly</span><span class="sxs-lookup"><span data-stu-id="72478-109">Redirecting Logs</span></span>

<span data-ttu-id="72478-110">Místo psaní protokoly [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), je možné poskytnout alternativní protokolování implementace, která můžete připojit přímo k správce protokolu, jako například [Log4Net](http://logging.apache.org/log4net/) a [NLog ](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="72478-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="72478-111">Jednoduše zadejte implementaci [objektu ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) a zaregistrovat ji pomocí modul vkládání závislostí podle svého výběru a bude získat převzata podle WebHooks Microsoft ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="72478-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="72478-112">Najdete v tématu [vkládání závislostí ve webovém rozhraní API 2 ASP.NET](https://www.asp.net/web-api/overview/advanced/dependency-injection) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="72478-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
