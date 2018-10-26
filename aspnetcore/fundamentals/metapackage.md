---
title: Metabalíček Microsoft.aspnetcore.all pro ASP.NET Core 2.0
author: Rick-Anderson
description: Metabalíček Microsoft.aspnetcore.all se nedoporučuje pro ASP.NET Core 2.1 nebo novější.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/metapackage
ms.openlocfilehash: f78684cb31976f976aec5e1773bcc728dfecc82e
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090703"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="eca82-103">Metabalíček Microsoft.aspnetcore.all pro ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="eca82-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="eca82-104">Doporučujeme aplikace pro ASP.NET Core 2.1 a později použít [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) namísto tohoto balíčku.</span><span class="sxs-lookup"><span data-stu-id="eca82-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="eca82-105">Zobrazit [migrace z metabalíček na Microsoft.AspNetCore.App](#migrate) v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="eca82-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="eca82-106">Tato funkce vyžaduje ASP.NET Core 2.x cílení na .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="eca82-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="eca82-107">[Metabalíček](https://www.nuget.org/packages/Microsoft.AspNetCore.All) Microsoft.AspNetCore.All pro ASP.NET Core zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="eca82-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="eca82-108">Všechny podporované balíčky vytvořené týmem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eca82-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="eca82-109">Všechny podporované balíčky pomocí Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="eca82-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="eca82-110">Interní závislosti a závislosti třetích stran používané v rámci ASP.NET Core a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="eca82-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="eca82-111">Všechny funkce aplikace ASP.NET Core 2.x a Entity Framework Core 2.x jsou součástí `Microsoft.AspNetCore.All` balíčku.</span><span class="sxs-lookup"><span data-stu-id="eca82-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="eca82-112">Tento balíček použít výchozí šablony projektu cílení ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="eca82-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="eca82-113">Číslo verze `Microsoft.AspNetCore.All` Microsoft.aspnetcore.all představuje verzi technologie ASP.NET Core a Entity Framework Core verze.</span><span class="sxs-lookup"><span data-stu-id="eca82-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="eca82-114">Aplikace, které používají `Microsoft.AspNetCore.All` Microsoft.aspnetcore.all automaticky využijí [.NET Core Runtime Store](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="eca82-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="eca82-115">Modul Runtime Store obsahuje všechny prostředky modulu runtime potřebné ke spouštění ASP.NET Core 2.x aplikací.</span><span class="sxs-lookup"><span data-stu-id="eca82-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="eca82-116">Při použití `Microsoft.AspNetCore.All` Microsoft.aspnetcore.all, **žádné** nasazení se aplikace používají prostředky z balíčků odkazovaných ASP.NET Core NuGet &mdash; Store .NET Core Runtime obsahuje tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="eca82-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="eca82-117">Prostředky v modulu Runtime Store předkompilovány zlepšit dobu spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="eca82-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="eca82-118">Proces oříznutí balíčku můžete použít k odebrání balíčků, které nepoužíváte.</span><span class="sxs-lookup"><span data-stu-id="eca82-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="eca82-119">Oříznutý balíčky jsou vyloučeny ve výstupu publikovanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eca82-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="eca82-120">Následující *.csproj* souboru odkazy `Microsoft.AspNetCore.All` Microsoft.aspnetcore.all pro ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="eca82-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="eca82-121">Migrace z metabalíček na Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="eca82-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="eca82-122">Následující balíčky jsou součástí `Microsoft.AspNetCore.All` , ale ne `Microsoft.AspNetCore.App` balíčku.</span><span class="sxs-lookup"><span data-stu-id="eca82-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

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

<span data-ttu-id="eca82-123">K přesunutí z `Microsoft.AspNetCore.All` k `Microsoft.AspNetCore.App`, když vaše aplikace používá libovolné rozhraní API z výše uvedených balíčků, nebo v režimu balíčky těchto balíčků, přidejte odkazy na tyto balíčky v projektu.</span><span class="sxs-lookup"><span data-stu-id="eca82-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="eca82-124">Všechny závislosti z předchozí balíčky, které jinak nejsou závislosti `Microsoft.AspNetCore.App` implicitně nejsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="eca82-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="eca82-125">Příklad:</span><span class="sxs-lookup"><span data-stu-id="eca82-125">For example:</span></span>

* <span data-ttu-id="eca82-126">`StackExchange.Redis` jako závislost `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="eca82-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="eca82-127">`Microsoft.ApplicationInsights` jako závislost `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="eca82-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

## <a name="update-aspnet-core-21"></a><span data-ttu-id="eca82-128">Aktualizace ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="eca82-128">Update ASP.NET Core 2.1</span></span>

<span data-ttu-id="eca82-129">Doporučujeme migrovat na `Microsoft.AspNetCore.App` Microsoft.aspnetcore.all 2.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="eca82-129">We recommend migrating to the `Microsoft.AspNetCore.App` metapackage for 2.1 and later.</span></span> <span data-ttu-id="eca82-130">Pokud chcete dál používat `Microsoft.AspNetCore.All` Microsoft.aspnetcore.all a ověřte nasazení nejnovější verze opravy:</span><span class="sxs-lookup"><span data-stu-id="eca82-130">To keep using the `Microsoft.AspNetCore.All` metapackage and ensure the latest patch version is deployed:</span></span>

* <span data-ttu-id="eca82-131">Na počítačích vývojářů a buildovací servery: nainstalujte nejnovější [.NET Core SDK](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="eca82-131">On development machines and build servers: Install the latest [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="eca82-132">Na serverech nasazení: nainstalujte nejnovější [.NET Core runtime](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="eca82-132">On deployment servers: Install the latest [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>
 <span data-ttu-id="eca82-133">Vaše aplikace se posunout vpřed byla nejnovější nainstalovaná verze na restartování aplikace.</span><span class="sxs-lookup"><span data-stu-id="eca82-133">Your app will roll forward to the latest installed version on an application restart.</span></span>
