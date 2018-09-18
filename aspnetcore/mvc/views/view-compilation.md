---
title: Kompilace souboru Razor a předkompilace v ASP.NET Core
author: rick-anderson
description: Přečtěte si o výhodách předkompilace Razor soubory a jak provádět Razor předkompilace souboru v aplikaci ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: f5888cf43d8d8192acedaa33b3fa0f313737fc9b
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011284"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Kompilace souboru Razor v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"

Soubor Razor je zkompilován za běhu, když uživatel vyvolá přidružené zobrazení MVC. Publikování souborů Razor čas sestavení není podporováno. Volitelně mohou být zkompilovány soubory Razor na čas publikování a nasazen s aplikací&mdash;nástrojem pro předkompilaci.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Souboru Razor je zkompilován za běhu, když uživatel vyvolá přidružené zobrazení stránky Razor nebo MVC. Publikování souborů Razor čas sestavení není podporováno. Volitelně mohou být zkompilovány soubory Razor na čas publikování a nasazen s aplikací&mdash;nástrojem pro předkompilaci.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Souboru Razor je zkompilován za běhu, když uživatel vyvolá přidružené zobrazení stránky Razor nebo MVC. Razor soubory kompilované za obě sestavení a publikování pomocí [Razor SDK](xref:razor-pages/sdk).

::: moniker-end

## <a name="precompilation-considerations"></a>Předkompilace důležité informace

Tady jsou vedlejší účinky předkompilace souborech Razor:

* Menší publikované sady
* Rychlejší spuštění
* Nelze upravovat soubory Razor&mdash;chybí související obsah ze sady publikované.

## <a name="deploy-precompiled-files"></a>Předkompilované soubory nasazení

::: moniker range=">= aspnetcore-2.1"

Kompilace sestavení a publikování běhu Razor souborů je povolené ve výchozím nastavení sada Razor SDK. Úprava souborů Razor, až se aktualizují je podporována v okamžiku sestavení. Ve výchozím nastavení pouze kompilované *Views.dll* a ne *.cshtml* jsou soubory nasazeny s vaší aplikací.

> [!IMPORTANT]
> V ASP.NET Core 3.0 se odebere předkompilaci. Doporučujeme migrovat na [Razor Sdk](xref:razor-pages/sdk).
>
> Sada Razor SDK nabídka platí jenom v případě, že žádné vlastnosti specifické pro předkompilaci jsou nastaveny v souboru projektu. Například nastavení *.csproj* souboru `MvcRazorCompileOnPublish` vlastnost `true` zakáže Razor SDK.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Pokud váš projekt cílí na .NET Framework, nainstalujte [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) balíček NuGet:

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Pokud váš projekt cílí na .NET Core, nejsou nutné žádné změny.

Šablony projektů ASP.NET Core 2.x implicitně nastavena `MvcRazorCompileOnPublish` vlastnost `true` ve výchozím nastavení. V důsledku toho tento prvek můžete ho bezpečně odebrat z *.csproj* souboru.

> [!IMPORTANT]
> V ASP.NET Core 3.0 se odebere předkompilaci. Doporučujeme migrovat na [Razor Sdk](xref:razor-pages/sdk).
>
> Předkompilace Razor soubor není k dispozici při provádění [samostatná nasazení (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) v ASP.NET Core 2.0.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Nastavte `MvcRazorCompileOnPublish` vlastnost `true`a nainstalujte [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) balíček NuGet. Následující *.csproj* ukázka zvýrazní tato nastavení:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Příprava aplikace pro [nasazení závisí na architektuře](/dotnet/core/deploying/#framework-dependent-deployments-fdd) s [příkazu rozhraní příkazového řádku .NET Core pro publikování](/dotnet/core/tools/dotnet-publish). Můžete třeba spustíte následující příkaz v kořenovém adresáři projektu:

```console
dotnet publish -c Release
```

A *< project_name >. PrecompiledViews.dll* soubor obsahující kompilovaných souborech Razor, je vytvořen při předkompilaci proběhne úspěšně. Například následující snímek obrazovky znázorňuje obsah *Index.cshtml* v rámci *WebApplication1.PrecompiledViews.dll*:

![Zobrazení syntaxe Razor uvnitř knihovny DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="additional-resources"></a>Další zdroje

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
