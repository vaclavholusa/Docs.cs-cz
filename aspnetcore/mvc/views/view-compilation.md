---
title: Kompilace zobrazení syntaxe Razor a předkompilaci v ASP.NET Core
author: rick-anderson
description: Informace o povolení zobrazení kompilace MVC Razor a předkompilaci v aplikacích ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a><span data-ttu-id="dc000-103">Syntaxe Razor (cshtml) souboru kompilace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc000-103">Razor file (.cshtml) compilation in ASP.NET Core</span></span>

<span data-ttu-id="dc000-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dc000-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dc000-105">Při vyvolání zobrazení zobrazení syntaxe Razor kompilované za běhu.</span><span class="sxs-lookup"><span data-stu-id="dc000-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="dc000-106">ASP.NET Core 2.1.0 nebo novější kompilace zobrazení v sestavení a publikování současně pomocí [Razor Sdk](/aspnetcore/mvc/razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="dc000-106">ASP.NET Core 2.1.0 or later compile views at build and publish time using [Razor Sdk](/aspnetcore/mvc/razor-pages/sdk).</span></span> <span data-ttu-id="dc000-107">V ASP.NET Core 1.1 a ASP.NET Core 2.0, můžete volitelně mělo být zobrazení kompilovat při publikování a nasazen s aplikací &mdash; pomocí tool předkompilace.</span><span class="sxs-lookup"><span data-stu-id="dc000-107">In ASP.NET Core 1.1, and ASP.NET Core 2.0, views can optionally be compiled at publish and deployed with the app &mdash; using the precompilation tool.</span></span> 



<span data-ttu-id="dc000-108">Předkompilace aspekty:</span><span class="sxs-lookup"><span data-stu-id="dc000-108">Precompilation considerations:</span></span>

* <span data-ttu-id="dc000-109">Předkompilace zobrazení následek menší publikované sady a rychlejší čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="dc000-109">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="dc000-110">Soubory Razor předkompilovat zobrazení nelze upravit.</span><span class="sxs-lookup"><span data-stu-id="dc000-110">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="dc000-111">Upravená zobrazení nebudou existovat v publikované sady.</span><span class="sxs-lookup"><span data-stu-id="dc000-111">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="dc000-112">Nasazení předkompilovaných zobrazení:</span><span class="sxs-lookup"><span data-stu-id="dc000-112">To deploy precompiled views:</span></span>

# <a name="aspnet-core-21tabaspnetcore21"></a>[<span data-ttu-id="dc000-113">ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="dc000-113">ASP.NET Core 2.1</span></span>](#tab/aspnetcore21/)
<span data-ttu-id="dc000-114">Sestavení a publikování v době kompilace Razor souborů je standardně SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="dc000-114">Build and publish time compilation of Razor files is enabled by default by the Razor Sdk.</span></span> <span data-ttu-id="dc000-115">Úprava souborů Razor po jejich aktualizací je podporován v čase vytvoření buildu.</span><span class="sxs-lookup"><span data-stu-id="dc000-115">Editing Razor files after they are updated is supported at build time.</span></span> <span data-ttu-id="dc000-116">Ve výchozím nastavení pouze zkompilovaný *Views.dll* a žádné soubory cshtml nasazených s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="dc000-116">By default, only the compiled *Views.dll* and no cshtml files are deployed with your application.</span></span> 
    
> [!IMPORTANT]
> <span data-ttu-id="dc000-117">Sadu Sdk Razor je platná, pouze pokud žádné předkompilace konkrétní vlastnosti, které jsou nastavené v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="dc000-117">The Razor Sdk is only effective when no precompilation specific properties are set in your project file.</span></span> <span data-ttu-id="dc000-118">Například nastavení `MvcRazorCompileOnPublish` ve vaší *.csproj* souboru vypne Razor Sdk.</span><span class="sxs-lookup"><span data-stu-id="dc000-118">For instance, setting `MvcRazorCompileOnPublish` in your *.csproj* file will disable the Razor Sdk.</span></span>

# <a name="aspnet-core-20tabaspnetcore20"></a>[<span data-ttu-id="dc000-119">Jádro ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="dc000-119">ASP.NET Core 2.0</span></span>](#tab/aspnetcore20/)

<span data-ttu-id="dc000-120">Pokud je cílem vašeho projektu je .NET Framework, obsahovat odkaz na balíček [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="dc000-120">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="dc000-121">Pokud je cílem vašeho projektu je .NET Core, jsou nezbytné žádné změny.</span><span class="sxs-lookup"><span data-stu-id="dc000-121">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="dc000-122">Šablony projektů ASP.NET Core 2.x implicitně nastaví `MvcRazorCompileOnPublish` k `true` ve výchozím nastavení, což znamená, můžete z bezpečně odebrat tento uzel *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="dc000-122">The ASP.NET Core 2.x project templates implicitly sets `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span>
    
> [!IMPORTANT]
> <span data-ttu-id="dc000-123">Předkompilace zobrazení syntaxe Razor v není k dispozici při provádění [samostatná nasazení (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="dc000-123">Razor view precompilation in not available when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> 

<span data-ttu-id="dc000-124">Příprava aplikace pro [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) s [publikování .NET Core rozhraní příkazového řádku příkaz](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="dc000-124">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="dc000-125">Například spusťte následující příkaz v kořenovém adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="dc000-125">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="dc000-126">A *< název_projektu >. PrecompiledViews.dll* obsahující kompilované zobrazení syntaxe Razor vytváří při předkompilaci úspěšné.</span><span class="sxs-lookup"><span data-stu-id="dc000-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="dc000-127">Například následující snímek obrazovky znázorňuje obsah *Index.cshtml* uvnitř *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="dc000-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Zobrazení syntaxe Razor uvnitř knihovny DLL](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc000-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc000-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="dc000-130">Nastavit `MvcRazorCompileOnPublish` k `true` a obsahovat odkaz na balíček `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="dc000-130">Set `MvcRazorCompileOnPublish` to `true` and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="dc000-131">Následující *.csproj* ukázka označuje tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="dc000-131">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

