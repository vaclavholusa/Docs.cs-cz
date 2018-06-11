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
ms.openlocfilehash: 7f8ccff7e3da93d6e617505ac93fafc3a82ed880
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252006"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="7b837-103">Hostitel v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7b837-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="7b837-104">Konfigurace aplikace .NET a spusťte *hostitele*.</span><span class="sxs-lookup"><span data-stu-id="7b837-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="7b837-105">Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="7b837-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="7b837-106">Dva hostitele rozhraní API jsou k dispozici pro použití:</span><span class="sxs-lookup"><span data-stu-id="7b837-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="7b837-107">[Webové hostitele](xref:fundamentals/host/web-host) &ndash; vhodná pro hostování webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7b837-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="7b837-108">[Obecné hostitele](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 nebo vyšší) &ndash; vhodná pro hostování jiných webových aplikací (například aplikace, které běží úlohy na pozadí).</span><span class="sxs-lookup"><span data-stu-id="7b837-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="7b837-109">V budoucí verzi bude obecné hostitele vhodný pro hostování jakékoliv aplikace, včetně webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7b837-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="7b837-110">Obecné hostitel nakonec nahradí webového hostitele.</span><span class="sxs-lookup"><span data-stu-id="7b837-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="7b837-111">Pro hostování ASP.NET Core *webové aplikace*, vývojáři využít webového hostitele na základě [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="7b837-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="7b837-112">Pro hostování *jiných webových aplikací*, vývojáři měli použít obecné hostitele na základě [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span><span class="sxs-lookup"><span data-stu-id="7b837-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
