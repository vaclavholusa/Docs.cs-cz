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
ms.locfileid: "34336275"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Kompilace souboru Razor v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"
Při vyvolání přidružené zobrazení MVC v době běhu kompiluje souboru nástroje Razor. Publikování souboru Razor čase vytvoření buildu není podporováno. Soubory Razor můžete volitelně zkompilovat na publikování čas a nasazen s aplikací&mdash;pomocí tool předkompilace.
::: moniker-end
::: moniker range="= aspnetcore-2.0"
Při vyvolání přidružené zobrazení stránky Razor nebo MVC v době běhu kompiluje souboru nástroje Razor. Publikování souboru Razor čase vytvoření buildu není podporováno. Soubory Razor můžete volitelně zkompilovat na publikování čas a nasazen s aplikací&mdash;pomocí tool předkompilace.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Při vyvolání přidružené zobrazení stránky Razor nebo MVC v době běhu kompiluje souboru nástroje Razor. Syntaxe Razor soubory kompilované za obě sestavení a publikování současně pomocí [Razor SDK](xref:mvc/razor-pages/sdk).
::: moniker-end

## <a name="precompilation-considerations"></a>Předkompilace aspekty

Tady jsou vedlejší účinky předkompilace Razor soubory:

* Menší publikované sady
* Rychlejší čas spuštění
* Nelze upravit soubory Razor&mdash;chybí související obsah ze sady publikované.

## <a name="deploy-precompiled-files"></a>Nasazení předkompilovaných souborů

::: moniker range=">= aspnetcore-2.1"
Kompilace sestavení a publikování běhu Razor souborů je povoleno ve výchozím nastavení SDK Razor. Úprava souborů Razor po jejich jste aktualizaci je podporována v čase vytvoření buildu. Ve výchozím nastavení pouze zkompilovaný *Views.dll* a žádné *.cshtml* soubory jsou nasazeny s vaší aplikací.

> [!IMPORTANT]
> Razor SDK je platná pouze v případě, že žádné vlastnosti specifické pro předkompilaci jsou nastavené v souboru projektu. Například nastavení *.csproj* souboru `MvcRazorCompileOnPublish` vlastnost `true` zakáže Razor SDK.
::: moniker-end

::: moniker range="= aspnetcore-2.0"
Pokud je cílem vašeho projektu je .NET Framework, nainstalujte [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) balíček NuGet:

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Pokud je cílem vašeho projektu je .NET Core, jsou nezbytné žádné změny.

Šablony projektů ASP.NET Core 2.x implicitně nastavit `MvcRazorCompileOnPublish` vlastnost `true` ve výchozím nastavení. V důsledku toho tento element může bezpečně odebrat z *.csproj* souboru.

> [!IMPORTANT]
> Předkompilace Razor soubor není k dispozici při provádění [samostatná nasazení (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) v technologii ASP.NET 2.0 jádra.
::: moniker-end

::: moniker range="= aspnetcore-1.1"
Nastavte `MvcRazorCompileOnPublish` vlastnost `true`a nainstalujte [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) balíček NuGet. Následující *.csproj* ukázka označuje tato nastavení:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
Příprava aplikace pro [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) s [publikování .NET Core rozhraní příkazového řádku příkaz](/dotnet/core/tools/dotnet-publish). Například spusťte následující příkaz v kořenovém adresáři projektu:

```console
dotnet publish -c Release
```

A *< název_projektu >. PrecompiledViews.dll* obsahující zkompilované soubory Razor vytváří při předkompilaci úspěšné. Například následující snímek obrazovky znázorňuje obsah *Index.cshtml* v rámci *WebApplication1.PrecompiledViews.dll*:

![Zobrazení syntaxe Razor uvnitř knihovny DLL](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a>Další zdroje

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