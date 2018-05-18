---
title: Microsoft.AspNetCore.App metapackage pro ASP.NET Core 2.1 a novější
author: Rick-Anderson
description: Microsoft.AspNetCore.App metapackage zahrnuje všechny podporované balíčků ASP.NET Core a Entity Framework Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage-app
ms.openlocfilehash: ac402bf8542560267ae179a1f41212380435dbfa
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>Microsoft.AspNetCore.App metapackage pro ASP.NET Core 2.1

Tato funkce vyžaduje ASP.NET Core, 2,1 a novější cílení na rozhraní .NET Core 2.1 nebo novější.

[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) metapackage pro ASP.NET Core:

* Nezahrnuje závislosti třetích stran s výjimkou [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), a [IX asynchronní](https://www.nuget.org/packages/System.Interactive.Async/). Tyto závislosti 3. stran se považují za nezbytné pro zajištění architektury hlavní funkce funkce.
* Zahrnuje všechny podporované balíčky tým ASP.NET Core kromě těch, které obsahují závislosti třetích stran (jiné než uvedené výše).
* Zahrnuje všechny podporované balíčky tým Entity Framework Core kromě těch, které obsahují závislosti třetích stran (jiné než uvedené výše).

Všechny funkce ASP.NET Core 2.1 a novější a Entity Framework Core 2.1 a vyšší jsou součástí `Microsoft.AspNetCore.App` balíčku. Výchozí šablony cílení na ASP.NET Core 2.1 projektu a později tento balíček použít. Doporučujeme aplikace cílené na ASP.NET Core, 2,1 a novější a Entity Framework Core 2.1 a později `Microsoft.AspNetCore.App` balíčku.

Číslo verze `Microsoft.AspNetCore.App` metapackage představuje ASP.NET Core verze a verze Entity Framework Core.

Pomocí `Microsoft.AspNetCore.App` metapackage poskytuje verze omezení, které chrání aplikace:

* Pokud je balíček zahrnuté, který má přenositelné (ne direct) závislost na balíček v `Microsoft.AspNetCore.App`a tato čísla verze se liší, NuGet dojde k chybě.
* Další balíčky přidat do vaší aplikace nelze změnit verzi balíčků, které jsou součástí `Microsoft.AspNetCore.App`.
* Verze konzistence zajišťuje spolehlivé prostředí. `Microsoft.AspNetCore.App` byla navržená tak, aby se zabránilo kombinace netestované verze související služby bits se použít společně ve stejné aplikaci.

Aplikace, které používají `Microsoft.AspNetCore.App` metapackage automaticky využít sdílené rozhraní ASP.NET Core. Při použití `Microsoft.AspNetCore.App` metapackage, **žádné** prostředky z odkazované balíčky ASP.NET Core NuGet nasazených aplikací &mdash; sdílené rozhraní ASP.NET Core obsahuje tyto prostředky. Prostředky v rámci sdílené jsou předkompilovaných ke zlepšení času spuštění aplikace. Další informace najdete v tématu "sdílený framework" v [.NET Core distribuční balení](/dotnet/core/build/distribution-packaging).

Následující *.csproj* odkazů na soubor projektu `Microsoft.AspNetCore.App` metapackage pro ASP.NET Core:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>

```

Předchozí kód představuje typické ASP.NET Core 2.1 a novější šablony. Neurčuje číslo verze `Microsoft.AspNetCore.App` balíček odkaz. Není-li verze, je implicitní verze zadána sadou SDK, tedy `Microsoft.NET.Sdk.Web`. Doporučujeme, abyste spoléhat na implicitní verze určeného sady SDK a není explicitně nastavení číslo verze na odkaz na balíček. Můžete nechat v Githubu komentář [diskusní pro verzi implicitní Microsoft.AspNetCore.App](https://github.com/aspnet/Docs/issues/6430).

Implicitní verze je nastaveno na `major.minor.0` pro přenosné aplikace. Mechanismus úplné dopředné sdílený framework se spustí aplikace na nejnovější verzi kompatibilní mezi nainstalovaných sdílené architektury. Pokud chcete zajistit, že se stejnou verzí se používá v vývoj, testování a provozním, zkontrolujte, jestli že je stejnou verzi nástroje sdílený framework nainstalované ve všech prostředích. Pro vlastní aplikace, číslo verze implicitní nastavena na `major.minor.patch` sdílený Framework dodávat v sadě SDK nainstalované.

Zadání číslo verze na `Microsoft.AspNetCore.App` nemá odkaz na **není** zaručit, že tato verze sdílený framework nastavení bude vybrána. Předpokládejme například, je určená verze "2.1.1", ale je nainstalována "2.1.3". V takovém případě aplikace bude používat "2.1.3". I když není doporučeno, můžete zakázat dopředné posunutí (opravy a/nebo menší). Další informace o hostiteli dotnet úplné dopředné a postup konfigurace své chování najdete v tématu [dotnet hostitele dopředné posunutí](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

Pokud vaše aplikace už dřív použili `Microsoft.AspNetCore.All`, najdete v části [migrace z Microsoft.AspNetCore.All na Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).