---
title: Hostitel v ASP.NET Core
author: guardrex
description: Další informace o ASP.NET Core webového hostitele a obecné hostitele .NET, které jsou zodpovědní za správu spuštění a životního cyklu aplikací.
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276614"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="f155c-103">Hostitel v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f155c-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="f155c-104">Konfigurace aplikace .NET a spusťte *hostitele*.</span><span class="sxs-lookup"><span data-stu-id="f155c-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="f155c-105">Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="f155c-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="f155c-106">Dva hostitele rozhraní API jsou k dispozici pro použití:</span><span class="sxs-lookup"><span data-stu-id="f155c-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="f155c-107">[Webové hostitele](xref:fundamentals/host/web-host) &ndash; vhodná pro hostování webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="f155c-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="f155c-108">[Obecné hostitele](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 nebo vyšší) &ndash; vhodná pro hostování jiných webových aplikací (například aplikace, které běží úlohy na pozadí).</span><span class="sxs-lookup"><span data-stu-id="f155c-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="f155c-109">V budoucí verzi bude obecné hostitele vhodný pro hostování jakékoliv aplikace, včetně webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="f155c-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="f155c-110">Obecné hostitel nakonec nahradí webového hostitele.</span><span class="sxs-lookup"><span data-stu-id="f155c-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="f155c-111">Pro hostování ASP.NET Core *webové aplikace*, vývojáři využít webového hostitele na základě [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="f155c-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="f155c-112">Pro hostování *jiných webových aplikací*, vývojáři měli použít obecné hostitele na základě [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span><span class="sxs-lookup"><span data-stu-id="f155c-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
