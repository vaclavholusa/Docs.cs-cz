---
title: "Kompilace zobrazení syntaxe Razor a předkompilace"
author: rick-anderson
description: "Odkaz na dokument vysvětlením, jak povolit kompilace MVC Razor zobrazení a předkompilaci v aplikacích ASP.NET Core."
keywords: "ASP.NET Core, kompilace zobrazení syntaxe Razor, Razor pre kompilace, předkompilaci Razor"
ms.author: riande
manager: wpickett
ms.date: 12/05/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 873f6203f9e7b5bb14968dcec3f8d8e5548bd834
ms.sourcegitcommit: 282f69e8dd63c39bde97a6d72783af2970d92040
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="249df-104">Kompilace zobrazení syntaxe Razor a předkompilaci v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="249df-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="249df-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="249df-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="249df-106">Při vyvolání zobrazení zobrazení syntaxe Razor kompilované za běhu.</span><span class="sxs-lookup"><span data-stu-id="249df-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="249df-107">ASP.NET základní 1.1.0 a vyšší můžete volitelně zkompilovat zobrazení syntaxe Razor a nasadit je do aplikace&mdash;tento proces se označuje jako předkompilaci.</span><span class="sxs-lookup"><span data-stu-id="249df-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="249df-108">Šablony projektů ASP.NET Core 2.x povolit předkompilaci ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="249df-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!NOTE]
> <span data-ttu-id="249df-109">Předkompilace zobrazení syntaxe Razor není momentálně k dispozici při provádění [samostatná nasazení (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="249df-109">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="249df-110">Tato funkce bude k dispozici pro SCDs po vydání 2.1.</span><span class="sxs-lookup"><span data-stu-id="249df-110">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="249df-111">Další informace najdete v tématu [zobrazení kompilace selže, když mezi kompilace pro Linux v systému Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="249df-111">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="249df-112">Předkompilace aspekty:</span><span class="sxs-lookup"><span data-stu-id="249df-112">Precompilation considerations:</span></span>

* <span data-ttu-id="249df-113">Předkompilace zobrazení následek menší publikované sady a rychlejší čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="249df-113">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="249df-114">Soubory Razor předkompilovat zobrazení nelze upravit.</span><span class="sxs-lookup"><span data-stu-id="249df-114">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="249df-115">Upravená zobrazení nebudou existovat v publikované sady.</span><span class="sxs-lookup"><span data-stu-id="249df-115">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="249df-116">Nasazení předkompilovaných zobrazení:</span><span class="sxs-lookup"><span data-stu-id="249df-116">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="249df-117">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="249df-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="249df-118">Pokud je cílem vašeho projektu je .NET Framework, obsahovat odkaz na balíček [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="249df-118">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="249df-119">Pokud je cílem vašeho projektu je .NET Core, jsou nezbytné žádné změny.</span><span class="sxs-lookup"><span data-stu-id="249df-119">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="249df-120">Šablony projektů ASP.NET Core 2.x implicitně nastavit `MvcRazorCompileOnPublish` k `true` ve výchozím nastavení, což znamená, můžete z bezpečně odebrat tento uzel *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="249df-120">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="249df-121">Pokud dáváte přednost lze odvodit přímo, je není škodu v nastavení `MvcRazorCompileOnPublish` vlastnost `true`.</span><span class="sxs-lookup"><span data-stu-id="249df-121">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="249df-122">Následující *.csproj* ukázka označuje toto nastavení:</span><span class="sxs-lookup"><span data-stu-id="249df-122">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="249df-123">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="249df-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="249df-124">Nastavit `MvcRazorCompileOnPublish` k `true`a obsahovat odkaz na balíček `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="249df-124">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="249df-125">Následující *.csproj* ukázka označuje tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="249df-125">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="249df-126">A *< název_projektu >. PrecompiledViews.dll* obsahující kompilované zobrazení syntaxe Razor vytváří při předkompilaci úspěšné.</span><span class="sxs-lookup"><span data-stu-id="249df-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="249df-127">Například následující snímek obrazovky znázorňuje obsah *Index.cshtml* uvnitř *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="249df-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Zobrazení syntaxe Razor uvnitř knihovny DLL](view-compilation/_static/razor-views-in-dll.png)
