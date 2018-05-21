---
title: Kompilace souboru Razor a předkompilaci v ASP.NET Core
author: rick-anderson
description: Další informace o výhodách předkompilace soubory Razor a o způsobech Razor předkompilaci souboru v aplikaci ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 03b11116a15c291452acd878e32cd015dc553dcc
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/20/2018
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="9c492-103">Kompilace souboru Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9c492-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="9c492-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9c492-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="9c492-105">Při vyvolání přidružené zobrazení MVC v době běhu kompiluje souboru nástroje Razor.</span><span class="sxs-lookup"><span data-stu-id="9c492-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="9c492-106">Publikování souboru Razor čase vytvoření buildu není podporováno.</span><span class="sxs-lookup"><span data-stu-id="9c492-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="9c492-107">Soubory Razor můžete volitelně zkompilovat na publikování čas a nasazen s aplikací&mdash;pomocí tool předkompilace.</span><span class="sxs-lookup"><span data-stu-id="9c492-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="9c492-108">Při vyvolání přidružené zobrazení stránky Razor nebo MVC v době běhu kompiluje souboru nástroje Razor.</span><span class="sxs-lookup"><span data-stu-id="9c492-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="9c492-109">Publikování souboru Razor čase vytvoření buildu není podporováno.</span><span class="sxs-lookup"><span data-stu-id="9c492-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="9c492-110">Soubory Razor můžete volitelně zkompilovat na publikování čas a nasazen s aplikací&mdash;pomocí tool předkompilace.</span><span class="sxs-lookup"><span data-stu-id="9c492-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="9c492-111">Při vyvolání přidružené zobrazení stránky Razor nebo MVC v době běhu kompiluje souboru nástroje Razor.</span><span class="sxs-lookup"><span data-stu-id="9c492-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="9c492-112">Syntaxe Razor soubory kompilované za obě sestavení a publikování současně pomocí [Razor SDK](xref:mvc/razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="9c492-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:mvc/razor-pages/sdk).</span></span>
::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="9c492-113">Předkompilace aspekty</span><span class="sxs-lookup"><span data-stu-id="9c492-113">Precompilation considerations</span></span>

<span data-ttu-id="9c492-114">Tady jsou vedlejší účinky předkompilace Razor soubory:</span><span class="sxs-lookup"><span data-stu-id="9c492-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="9c492-115">Menší publikované sady</span><span class="sxs-lookup"><span data-stu-id="9c492-115">A smaller published bundle</span></span>
* <span data-ttu-id="9c492-116">Rychlejší čas spuštění</span><span class="sxs-lookup"><span data-stu-id="9c492-116">A faster startup time</span></span>
* <span data-ttu-id="9c492-117">Nelze upravit soubory Razor&mdash;chybí související obsah ze sady publikované.</span><span class="sxs-lookup"><span data-stu-id="9c492-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="9c492-118">Nasazení předkompilovaných souborů</span><span class="sxs-lookup"><span data-stu-id="9c492-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="9c492-119">Kompilace sestavení a publikování běhu Razor souborů je povoleno ve výchozím nastavení SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="9c492-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="9c492-120">Úprava souborů Razor po jejich jste aktualizaci je podporována v čase vytvoření buildu.</span><span class="sxs-lookup"><span data-stu-id="9c492-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="9c492-121">Ve výchozím nastavení pouze zkompilovaný *Views.dll* a žádné *.cshtml* soubory jsou nasazeny s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="9c492-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c492-122">Razor SDK je platná pouze v případě, že žádné vlastnosti specifické pro předkompilaci jsou nastavené v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="9c492-122">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="9c492-123">Například nastavení *.csproj* souboru `MvcRazorCompileOnPublish` vlastnost `true` zakáže Razor SDK.</span><span class="sxs-lookup"><span data-stu-id="9c492-123">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="9c492-124">Pokud je cílem vašeho projektu je .NET Framework, nainstalujte [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) balíček NuGet:</span><span class="sxs-lookup"><span data-stu-id="9c492-124">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

<span data-ttu-id="9c492-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span><span class="sxs-lookup"><span data-stu-id="9c492-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span></span>

<span data-ttu-id="9c492-126">Pokud je cílem vašeho projektu je .NET Core, jsou nezbytné žádné změny.</span><span class="sxs-lookup"><span data-stu-id="9c492-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="9c492-127">Šablony projektů ASP.NET Core 2.x implicitně nastavit `MvcRazorCompileOnPublish` vlastnost `true` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9c492-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="9c492-128">V důsledku toho tento element může bezpečně odebrat z *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="9c492-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c492-129">Předkompilace Razor soubor není k dispozici při provádění [samostatná nasazení (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="9c492-129">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="9c492-130">Nastavte `MvcRazorCompileOnPublish` vlastnost `true`a nainstalujte [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="9c492-130">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="9c492-131">Následující *.csproj* ukázka označuje tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="9c492-131">The following *.csproj* sample highlights these settings:</span></span>

<span data-ttu-id="9c492-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span><span class="sxs-lookup"><span data-stu-id="9c492-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span></span>
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="9c492-133">Příprava aplikace pro [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) s [publikování .NET Core rozhraní příkazového řádku příkaz](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="9c492-133">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="9c492-134">Například spusťte následující příkaz v kořenovém adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="9c492-134">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="9c492-135">A *< název_projektu >. PrecompiledViews.dll* obsahující zkompilované soubory Razor vytváří při předkompilaci úspěšné.</span><span class="sxs-lookup"><span data-stu-id="9c492-135">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="9c492-136">Například následující snímek obrazovky znázorňuje obsah *Index.cshtml* v rámci *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="9c492-136">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Zobrazení syntaxe Razor uvnitř knihovny DLL](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9c492-138">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9c492-138">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
* <xref:mvc/razor-pages/index>
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
* <xref:mvc/razor-pages/index>
* <xref:mvc/views/overview>
* <xref:mvc/razor-pages/sdk>
::: moniker-end