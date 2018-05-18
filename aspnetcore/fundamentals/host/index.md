---
title: Hostitel v ASP.NET Core
author: guardrex
description: Další informace o ASP.NET Core webového hostitele a obecné hostitele .NET, které jsou zodpovědní za správu spuštění a životního cyklu aplikací.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="7a5b4-103">Hostitel v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7a5b4-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="7a5b4-104">Konfigurace aplikace .NET a spusťte *hostitele*.</span><span class="sxs-lookup"><span data-stu-id="7a5b4-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="7a5b4-105">Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="7a5b4-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="7a5b4-106">Dva hostitele rozhraní API jsou k dispozici pro použití:</span><span class="sxs-lookup"><span data-stu-id="7a5b4-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="7a5b4-107">[Webové hostitele](xref:fundamentals/host/web-host) &ndash; vhodná pro hostování webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7a5b4-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="7a5b4-108">[Obecné hostitele](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 nebo vyšší) &ndash; vhodná pro hostování jiných webových aplikací (například aplikace, které běží úlohy na pozadí).</span><span class="sxs-lookup"><span data-stu-id="7a5b4-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="7a5b4-109">V budoucí verzi bude obecné hostitele vhodný pro hostování jakékoliv aplikace, včetně webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7a5b4-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="7a5b4-110">Obecné hostitel nakonec nahradí webového hostitele.</span><span class="sxs-lookup"><span data-stu-id="7a5b4-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="7a5b4-111">V tomto okamžiku by vývojáři použít [webového hostitele](xref:fundamentals/host/web-host) na základě [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) pro hostování aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a5b4-111">At this time, developers should use the [Web Host](xref:fundamentals/host/web-host) based on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) for hosting ASP.NET Core apps.</span></span>
