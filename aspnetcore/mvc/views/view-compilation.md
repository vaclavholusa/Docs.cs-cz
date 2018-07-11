---
title: Kompilace souboru Razor a předkompilace v ASP.NET Core
author: rick-anderson
description: Přečtěte si o výhodách předkompilace Razor soubory a jak provádět Razor předkompilace souboru v aplikaci ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: 9355d467ca819ea8c6292963b31367ad5ca36d55
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938534"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="7ec4e-103">Kompilace souboru Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ec4e-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="7ec4e-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7ec4e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="7ec4e-105">Soubor Razor je zkompilován za běhu, když uživatel vyvolá přidružené zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="7ec4e-106">Publikování souborů Razor čas sestavení není podporováno.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="7ec4e-107">Volitelně mohou být zkompilovány soubory Razor na čas publikování a nasazen s aplikací&mdash;nástrojem pro předkompilaci.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="7ec4e-108">Souboru Razor je zkompilován za běhu, když uživatel vyvolá přidružené zobrazení stránky Razor nebo MVC.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="7ec4e-109">Publikování souborů Razor čas sestavení není podporováno.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="7ec4e-110">Volitelně mohou být zkompilovány soubory Razor na čas publikování a nasazen s aplikací&mdash;nástrojem pro předkompilaci.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="7ec4e-111">Souboru Razor je zkompilován za běhu, když uživatel vyvolá přidružené zobrazení stránky Razor nebo MVC.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="7ec4e-112">Razor soubory kompilované za obě sestavení a publikování pomocí [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="7ec4e-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>
::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="7ec4e-113">Předkompilace důležité informace</span><span class="sxs-lookup"><span data-stu-id="7ec4e-113">Precompilation considerations</span></span>

<span data-ttu-id="7ec4e-114">Tady jsou vedlejší účinky předkompilace souborech Razor:</span><span class="sxs-lookup"><span data-stu-id="7ec4e-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="7ec4e-115">Menší publikované sady</span><span class="sxs-lookup"><span data-stu-id="7ec4e-115">A smaller published bundle</span></span>
* <span data-ttu-id="7ec4e-116">Rychlejší spuštění</span><span class="sxs-lookup"><span data-stu-id="7ec4e-116">A faster startup time</span></span>
* <span data-ttu-id="7ec4e-117">Nelze upravovat soubory Razor&mdash;chybí související obsah ze sady publikované.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="7ec4e-118">Předkompilované soubory nasazení</span><span class="sxs-lookup"><span data-stu-id="7ec4e-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="7ec4e-119">Kompilace sestavení a publikování běhu Razor souborů je povolené ve výchozím nastavení sada Razor SDK.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="7ec4e-120">Úprava souborů Razor, až se aktualizují je podporována v okamžiku sestavení.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="7ec4e-121">Ve výchozím nastavení pouze kompilované *Views.dll* a ne *.cshtml* jsou soubory nasazeny s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ec4e-122">Sada Razor SDK nabídka platí jenom v případě, že žádné vlastnosti specifické pro předkompilaci jsou nastaveny v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-122">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="7ec4e-123">Například nastavení *.csproj* souboru `MvcRazorCompileOnPublish` vlastnost `true` zakáže Razor SDK.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-123">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="7ec4e-124">Pokud váš projekt cílí na .NET Framework, nainstalujte [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) balíček NuGet:</span><span class="sxs-lookup"><span data-stu-id="7ec4e-124">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="7ec4e-125">Pokud váš projekt cílí na .NET Core, nejsou nutné žádné změny.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-125">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="7ec4e-126">Šablony projektů ASP.NET Core 2.x implicitně nastavena `MvcRazorCompileOnPublish` vlastnost `true` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-126">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="7ec4e-127">V důsledku toho tento prvek můžete ho bezpečně odebrat z *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-127">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ec4e-128">Předkompilace Razor soubor není k dispozici při provádění [samostatná nasazení (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) v ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-128">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="7ec4e-129">Nastavte `MvcRazorCompileOnPublish` vlastnost `true`a nainstalujte [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-129">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="7ec4e-130">Následující *.csproj* ukázka zvýrazní tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="7ec4e-130">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="7ec4e-131">Příprava aplikace pro [nasazení závisí na architektuře](/dotnet/core/deploying/#framework-dependent-deployments-fdd) s [příkazu rozhraní příkazového řádku .NET Core pro publikování](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="7ec4e-131">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="7ec4e-132">Můžete třeba spustíte následující příkaz v kořenovém adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="7ec4e-132">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="7ec4e-133">A *< project_name >. PrecompiledViews.dll* soubor obsahující kompilovaných souborech Razor, je vytvořen při předkompilaci proběhne úspěšně.</span><span class="sxs-lookup"><span data-stu-id="7ec4e-133">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="7ec4e-134">Například následující snímek obrazovky znázorňuje obsah *Index.cshtml* v rámci *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="7ec4e-134">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Zobrazení syntaxe Razor uvnitř knihovny DLL](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="7ec4e-136">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7ec4e-136">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>
::: moniker-end
