---
title: Microsoft.AspNetCore.All metapackage pro technologii ASP.NET 2.0 jádra a novější
author: Rick-Anderson
description: Microsoft.AspNetCore.All metapackage zahrnuje všechny podporované balíčků ASP.NET Core a Entity Framework Core, spolu s jejich závislosti.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: fbb76f41f3178ddc4e51faa14edece1869a30cd0
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729074"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="5da50-103">Microsoft.AspNetCore.All metapackage pro technologii ASP.NET 2.0 jádra</span><span class="sxs-lookup"><span data-stu-id="5da50-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="5da50-104">Doporučujeme aplikací pro ASP.NET Core 2.1 a později [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) místo tohoto balíčku.</span><span class="sxs-lookup"><span data-stu-id="5da50-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="5da50-105">V tématu [migrace z Microsoft.AspNetCore.All na Microsoft.AspNetCore.App](#migrate) v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5da50-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="5da50-106">Tato funkce vyžaduje rozhraní .NET zaměřená na aplikace ASP.NET Core 2.x základní 2.x.</span><span class="sxs-lookup"><span data-stu-id="5da50-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="5da50-107">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage pro ASP.NET Core zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="5da50-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="5da50-108">Všechny podporované balíčky tým ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5da50-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="5da50-109">Všechny podporované balíčků základní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5da50-109">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="5da50-110">Interní a 3. stran závislosti používané ASP.NET Core a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5da50-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="5da50-111">Všechny funkce ASP.NET Core 2.x a Entity Framework Core 2.x jsou součástí `Microsoft.AspNetCore.All` balíčku.</span><span class="sxs-lookup"><span data-stu-id="5da50-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="5da50-112">Tento balíček použít výchozí šablony projektů cílení na technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="5da50-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="5da50-113">Číslo verze `Microsoft.AspNetCore.All` metapackage představuje ASP.NET Core verze a verze Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5da50-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="5da50-114">Aplikace, které používají `Microsoft.AspNetCore.All` metapackage automaticky využívat výhod [.NET Core Runtime úložiště](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="5da50-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="5da50-115">Modul Runtime úložiště obsahuje všechny prostředky potřebné ke spuštění 2.x aplikace ASP.NET Core runtime.</span><span class="sxs-lookup"><span data-stu-id="5da50-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="5da50-116">Při použití `Microsoft.AspNetCore.All` metapackage, **žádné** prostředky z odkazované balíčky ASP.NET Core NuGet nasazených aplikací &mdash; úložiště .NET Core Runtime obsahuje tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="5da50-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="5da50-117">Prostředky v úložišti Runtime jsou předkompilovaných ke zlepšení času spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="5da50-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="5da50-118">Proces oříznutí balíčku můžete odebrat balíčky, které nepoužíváte.</span><span class="sxs-lookup"><span data-stu-id="5da50-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="5da50-119">Oříznutý balíčky jsou vyloučeny ve výstupu publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="5da50-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="5da50-120">Následující *.csproj* souboru odkazy `Microsoft.AspNetCore.All` metapackage pro ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="5da50-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="5da50-121">Migrace z Microsoft.AspNetCore.All na Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="5da50-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="5da50-122">Následující balíčky jsou součástí `Microsoft.AspNetCore.All` ale ne `Microsoft.AspNetCore.App` balíčku.</span><span class="sxs-lookup"><span data-stu-id="5da50-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

<span data-ttu-id="5da50-123">Pro přesun z `Microsoft.AspNetCore.All` k `Microsoft.AspNetCore.App`, pokud vaše aplikace používá žádné rozhraní API z výše uvedených balíčky nebo balíčky nebude těchto balíčků, přidejte odkazy na tyto balíčky v projektu.</span><span class="sxs-lookup"><span data-stu-id="5da50-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="5da50-124">Všechny závislosti z předchozí balíčky, které jinak nejsou závislostí `Microsoft.AspNetCore.App` implicitně nejsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="5da50-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="5da50-125">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5da50-125">For example:</span></span>

* <span data-ttu-id="5da50-126">`StackExchange.Redis` jako závislosti `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="5da50-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="5da50-127">`Microsoft.ApplicationInsights` jako závislosti `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="5da50-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>
