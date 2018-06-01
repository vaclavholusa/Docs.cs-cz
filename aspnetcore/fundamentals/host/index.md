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
ms.openlocfilehash: 37c527718433410eede8321dd7813f0ffd6473e5
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687441"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="afadb-103">Hostitel v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="afadb-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="afadb-104">Konfigurace aplikace .NET a spusťte *hostitele*.</span><span class="sxs-lookup"><span data-stu-id="afadb-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="afadb-105">Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="afadb-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="afadb-106">Dva hostitele rozhraní API jsou k dispozici pro použití:</span><span class="sxs-lookup"><span data-stu-id="afadb-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="afadb-107">[Webové hostitele](xref:fundamentals/host/web-host) &ndash; vhodná pro hostování webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="afadb-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="afadb-108">[Obecné hostitele](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 nebo vyšší) &ndash; vhodná pro hostování jiných webových aplikací (například aplikace, které běží úlohy na pozadí).</span><span class="sxs-lookup"><span data-stu-id="afadb-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="afadb-109">V budoucí verzi bude obecné hostitele vhodný pro hostování jakékoliv aplikace, včetně webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="afadb-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="afadb-110">Obecné hostitel nakonec nahradí webového hostitele.</span><span class="sxs-lookup"><span data-stu-id="afadb-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="afadb-111">V tomto okamžiku by vývojáři použít [webového hostitele](xref:fundamentals/host/web-host) na základě [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pro hostování aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="afadb-111">At this time, developers should use the [Web Host](xref:fundamentals/host/web-host) based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) for hosting ASP.NET Core apps.</span></span>
