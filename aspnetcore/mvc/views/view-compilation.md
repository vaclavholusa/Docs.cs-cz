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
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a>Syntaxe Razor (cshtml) souboru kompilace v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Při vyvolání zobrazení zobrazení syntaxe Razor kompilované za běhu. ASP.NET Core 2.1.0 nebo novější kompilace zobrazení v sestavení a publikování současně pomocí [Razor Sdk](/aspnetcore/mvc/razor-pages/sdk). V ASP.NET Core 1.1 a ASP.NET Core 2.0, můžete volitelně mělo být zobrazení kompilovat při publikování a nasazen s aplikací &mdash; pomocí tool předkompilace. 



Předkompilace aspekty:

* Předkompilace zobrazení následek menší publikované sady a rychlejší čas spuštění.
* Soubory Razor předkompilovat zobrazení nelze upravit. Upravená zobrazení nebudou existovat v publikované sady. 

Nasazení předkompilovaných zobrazení:

# <a name="aspnet-core-21tabaspnetcore21"></a>[ASP.NET Core 2.1](#tab/aspnetcore21/)
Sestavení a publikování v době kompilace Razor souborů je standardně SDK Razor. Úprava souborů Razor po jejich aktualizací je podporován v čase vytvoření buildu. Ve výchozím nastavení pouze zkompilovaný *Views.dll* a žádné soubory cshtml nasazených s vaší aplikací. 
    
> [!IMPORTANT]
> Sadu Sdk Razor je platná, pouze pokud žádné předkompilace konkrétní vlastnosti, které jsou nastavené v souboru projektu. Například nastavení `MvcRazorCompileOnPublish` ve vaší *.csproj* souboru vypne Razor Sdk.

# <a name="aspnet-core-20tabaspnetcore20"></a>[Jádro ASP.NET 2.0](#tab/aspnetcore20/)

Pokud je cílem vašeho projektu je .NET Framework, obsahovat odkaz na balíček [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Pokud je cílem vašeho projektu je .NET Core, jsou nezbytné žádné změny.

Šablony projektů ASP.NET Core 2.x implicitně nastaví `MvcRazorCompileOnPublish` k `true` ve výchozím nastavení, což znamená, můžete z bezpečně odebrat tento uzel *.csproj* souboru.
    
> [!IMPORTANT]
> Předkompilace zobrazení syntaxe Razor v není k dispozici při provádění [samostatná nasazení (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) v technologii ASP.NET 2.0 jádra. 

Příprava aplikace pro [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) s [publikování .NET Core rozhraní příkazového řádku příkaz](/dotnet/core/tools/dotnet-publish). Například spusťte následující příkaz v kořenovém adresáři projektu:

```console
dotnet publish -c Release
```

A *< název_projektu >. PrecompiledViews.dll* obsahující kompilované zobrazení syntaxe Razor vytváří při předkompilaci úspěšné. Například následující snímek obrazovky znázorňuje obsah *Index.cshtml* uvnitř *WebApplication1.PrecompiledViews.dll*:

![Zobrazení syntaxe Razor uvnitř knihovny DLL](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Nastavit `MvcRazorCompileOnPublish` k `true` a obsahovat odkaz na balíček `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Následující *.csproj* ukázka označuje tato nastavení:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

