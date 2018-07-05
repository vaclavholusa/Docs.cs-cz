---
uid: webhooks/diagnostics/logging
title: ASP.NET – Webhooky protokolování | Dokumentace Microsoftu
author: rick-anderson
description: Postup přihlášení ASP.NET – Webhooky.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: ''
ms.openlocfilehash: 5501d250ea6dd86c0cfddec8d9941ec16b4f57ba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364103"
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="fd846-103">ASP.NET – Webhooky protokolování</span><span class="sxs-lookup"><span data-stu-id="fd846-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="fd846-104">Microsoft ASP.NET WebHooks používá protokolování jako způsob hlášení chyb a problémů.</span><span class="sxs-lookup"><span data-stu-id="fd846-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="fd846-105">Ve výchozím nastavení se protokoly zapisují pomocí [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) kde může být spravovaných pomocí [naslouchacích procesů trasování](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) jako libovolný jiný datový proud protokolu.</span><span class="sxs-lookup"><span data-stu-id="fd846-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="fd846-106">Při nasazení webové aplikace jako webové aplikace Azure, protokoly se automaticky rozpoznat a lze je spravovat společně s jinými [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) protokolování.</span><span class="sxs-lookup"><span data-stu-id="fd846-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="fd846-107">Podrobnosti najdete v tématu [povolení protokolování diagnostiky pro webové aplikace ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="fd846-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="fd846-108">Kromě toho protokoly můžete získat přímo v prostředí Visual Studio, jak je popsáno v [řešení problémů s webovou aplikací ve službě Azure App Service pomocí sady Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="fd846-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="fd846-109">Přesměrování protokolů</span><span class="sxs-lookup"><span data-stu-id="fd846-109">Redirecting Logs</span></span>

<span data-ttu-id="fd846-110">Místo psaní protokoly [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), je možné poskytnout alternativní protokolování implementace, která můžete připojit přímo k log manager například [Log4Net](http://logging.apache.org/log4net/) a [NLog ](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="fd846-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="fd846-111">Stačí zadat implementace [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) a zaregistrovat modul injektáž závislostí podle vašeho výběru a jeho se získat vyzvednou WebHooks Microsoft ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fd846-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="fd846-112">Podrobnosti najdete na [injektáž závislostí v ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="fd846-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
