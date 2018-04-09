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
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Kompilace zobrazení syntaxe Razor a předkompilaci v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Při vyvolání zobrazení zobrazení syntaxe Razor kompilované za běhu. ASP.NET základní 1.1.0 a vyšší můžete volitelně zkompilovat zobrazení syntaxe Razor a nasadit je do aplikace&mdash;tento proces se označuje jako předkompilaci. Šablony projektů ASP.NET Core 2.x povolit předkompilaci ve výchozím nastavení.

> [!IMPORTANT]
> Předkompilace zobrazení syntaxe Razor není momentálně k dispozici při provádění [samostatná nasazení (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) v technologii ASP.NET 2.0 jádra. Tato funkce bude k dispozici pro SCDs po vydání 2.1. Další informace najdete v tématu [zobrazení kompilace selže, když mezi kompilace pro Linux v systému Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).

Předkompilace aspekty:

* Předkompilace zobrazení následek menší publikované sady a rychlejší čas spuštění.
* Soubory Razor předkompilovat zobrazení nelze upravit. Upravená zobrazení nebudou existovat v publikované sady. 

Nasazení předkompilovaných zobrazení:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)
Pokud je cílem vašeho projektu je .NET Framework, obsahovat odkaz na balíček [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Pokud je cílem vašeho projektu je .NET Core, jsou nezbytné žádné změny.

Šablony projektů ASP.NET Core 2.x implicitně nastavit `MvcRazorCompileOnPublish` k `true` ve výchozím nastavení, což znamená, můžete z bezpečně odebrat tento uzel *.csproj* souboru. Pokud dáváte přednost lze odvodit přímo, je není škodu v nastavení `MvcRazorCompileOnPublish` vlastnost `true`. Následující *.csproj* ukázka označuje toto nastavení:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x/)
Nastavit `MvcRazorCompileOnPublish` k `true`a obsahovat odkaz na balíček `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Následující *.csproj* ukázka označuje tato nastavení:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

Příprava aplikace pro [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) s [publikování .NET Core rozhraní příkazového řádku příkaz](/dotnet/core/tools/dotnet-publish). Například spusťte následující příkaz v kořenovém adresáři projektu:

```console
dotnet publish -c Release
```

A *< název_projektu >. PrecompiledViews.dll* obsahující kompilované zobrazení syntaxe Razor vytváří při předkompilaci úspěšné. Například následující snímek obrazovky znázorňuje obsah *Index.cshtml* uvnitř *WebApplication1.PrecompiledViews.dll*:

![Zobrazení syntaxe Razor uvnitř knihovny DLL](view-compilation/_static/razor-views-in-dll.png)
