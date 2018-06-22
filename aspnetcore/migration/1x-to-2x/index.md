---
title: Migrace z ASP.NET Core 1.x na 2.0
author: scottaddie
description: Tento článek popisuje požadavky a nejběžnější kroky migrace projektu ASP.NET Core 1.x na technologii ASP.NET 2.0 jádra.
ms.author: scaddie
ms.date: 10/03/2017
uid: migration/1x-to-2x/index
ms.openlocfilehash: 1052b17b433f06162325db340cd53ee61b76a184
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272501"
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a>Migrace z ASP.NET Core 1.x na 2.0

Podle [Scott Addie](https://github.com/scottaddie)

V tomto článku jsme vás provede procesem aktualizace existujícího projektu ASP.NET Core 1.x na technologii ASP.NET 2.0 jádra. Migrace vaší aplikace na technologii ASP.NET Core 2.0 umožňuje využít výhod [mnoho nových funkcí a vylepšení výkonu](xref:aspnetcore-2.0).

Stávající aplikace ASP.NET Core 1.x vycházejí z šablony projektů specifické pro verzi. Jako rozhraní ASP.NET Core zpracovaní, takže udělat šablony projektů a počáteční kód v nich obsažené. Kromě aktualizace rozhraní ASP.NET Core, budete muset aktualizovat kód pro vaši aplikaci.

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Požadavky
V tématu [Začínáme s ASP.NET Core](xref:getting-started).

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>Aktualizujte cílový Framework Přezdívka (TFM)
Projektech zacílených na .NET Core měli používat [TFM](/dotnet/standard/frameworks#referring-to-frameworks) verze větší než nebo rovna hodnotě .NET Core 2.0. Vyhledejte `<TargetFramework>` uzlu *.csproj* souboru a nahraďte jeho vnitřní text s `netcoreapp2.0`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

Projekty cílení na rozhraní .NET Framework by měli používat TFM verze větší než nebo rovna hodnotě rozhraní .NET Framework 4.6.1. Vyhledejte `<TargetFramework>` uzlu *.csproj* souboru a nahraďte jeho vnitřní text s `net461`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> Rozhraní .NET 2.0 základní nabízí mnohem větší oblasti prostor než .NET Core 1.x. Pokud jste možnosti cílení na rozhraní .NET Framework výhradně z důvodu chybějících rozhraní API v .NET Core 1.x, cílení na rozhraní .NET Core 2.0 je pravděpodobně fungovat.

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>Verze rozhraní .NET Core SDK aktualizace v global.json
Pokud vaše řešení závisí na [ *global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) souboru chcete zacílit na konkrétní verzi rozhraní .NET Core SDK, aktualizujte jeho `version` vlastnost na používání verze 2.0, který je nainstalovaný na počítači:

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>Odkazy na balíčku aktualizace
*.Csproj* souboru v projektu 1.x uvádí každý balíček NuGet používané v projektu.

V projektu ASP.NET 2.0 základní cílení na rozhraní .NET 2.0 jádra, jeden [metapackage](xref:fundamentals/metapackage) odkaz v *.csproj* souboru nahrazuje kolekce balíčků:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

Všechny funkce technologie ASP.NET 2.0 jádra a Entity Framework Core 2.0 jsou součástí metapackage.

Projekty ASP.NET Core 2.0 cílení na rozhraní .NET Framework by měly být nadále odkazovat na jednotlivých balíčků NuGet. Aktualizace `Version` atribut jednotlivých `<PackageReference />` uzlu 2.0.0.

Tady je seznam například `<PackageReference />` použitých v typickém cílení na rozhraní .NET Framework projektu ASP.NET 2.0 základní uzlů:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>Aktualizace nástrojů rozhraní .NET Core rozhraní příkazového řádku
V *.csproj* souboru, aktualizovat `Version` atribut jednotlivých `<DotNetCliToolReference />` uzlu 2.0.0.

Zde je například seznam nástrojů příkazového řádku používá v typickém projektu ASP.NET 2.0 základní cílení na rozhraní .NET 2.0 jádra:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>Přejmenujte vlastnost záložní cíl balíčku
*.Csproj* souboru projektu 1.x používá `PackageTargetFallback` uzlu a proměnnou:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

Přejmenovat na uzel a proměnnou `AssetTargetFallback`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>Aktualizujte metodu Main v souboru Program.cs
V projektech 1.x `Main` metodu *Program.cs* hledá takto:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

V projektech 2.0 `Main` metodu *Program.cs* je jednodušší:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

Přijetí této nové 2.0 vzor se důrazně doporučuje a je nutné na funkce produktu jako [migrace základní Entity Framework (EF)](xref:data/ef-mvc/migrations) pracovat. Například běžet `Update-Database` z okna konzoly Správce balíčků nebo `dotnet ef database update` z příkazu řádku (na projekty převést na technologii ASP.NET 2.0 základní) generuje následující chybu:

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a>Přidejte poskytovatele konfigurace
V projektech 1.x poskytovatelé konfigurace přidání do aplikace byla provést pomocí `Startup` konstruktor. Potřebný postup vytvoření instance `ConfigurationBuilder`, načítání použít poskytovatelé (proměnné prostředí, nastavení aplikace atd.) a inicializace členem `IConfigurationRoot`.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

Načte v předchozím příkladu `Configuration` člena s nastavením konfigurace z *appSettings.JSON určený* a také všechny *appsettings.\< EnvironmentName\>.json* odpovídající soubor `IHostingEnvironment.EnvironmentName` vlastnost. Umístění těchto souborů se stejnou cestou jako *Startup.cs*.

V projektech 2.0 spustí servisní často používaný kód konfigurace vyplývajících na 1.x projekty. Proměnné prostředí a nastavení aplikace jsou načtena při spuštění. Ekvivalent *Startup.cs* kód zkrátila `IConfiguration` inicializace s vloženého instancí:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

Odebrání výchozí zprostředkovatele přidal `WebHostBuilder.CreateDefaultBuilder`, vyvolání `Clear` metodu `IConfigurationBuilder.Sources` vlastnost uvnitř `ConfigureAppConfiguration`. Chcete-li přidat zprostředkovatele zpět, použít `ConfigureAppConfiguration` metoda v *Program.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

Konfigurace používané `CreateDefaultBuilder` metoda v předchozím fragmentu kódu můžete vidět [zde](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).

Další informace najdete v tématu [konfigurace v ASP.NET Core](xref:fundamentals/configuration/index).

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a>Přesunutí databáze inicializace kódu
V projektech 1.x pomocí EF základní 1.x, příkazy jako `dotnet ef migrations add` provede následující akce:
1. Vytvoří `Startup` instance
2. Vyvolá `ConfigureServices` metody pro registraci všech služeb pomocí vkládání závislostí (včetně `DbContext` typy)
3. Provádí jeho požadavků úlohy

V 2.0 projektů pomocí EF základní 2.0 `Program.BuildWebHost` je vyvolána k získání aplikačních služeb. Na rozdíl od 1.x, tato akce nemá další vedlejším účinkem vyvolání `Startup.Configure`. Pokud je vaše aplikace 1.x vyvolána kód inicializace databáze v jeho `Configure` metoda, může dojít neočekávané problémy. Například pokud databáze ještě neexistuje, kód synchronizace replik indexů se spustí před spuštěním příkazu EF základní migrace. Tento problém způsobí, že `dotnet ef migrations list` příkaz selže, pokud databáze ještě neexistuje.

Vezměte v úvahu následující kód inicializace 1.x počáteční hodnoty v `Configure` metodu *Startup.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

V projektech 2.0, přesuňte `SeedData.Initialize` volat na `Main` metodu *Program.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

Od verze 2.0, je chybný postup udělat něco `BuildWebHost` s výjimkou sestavení a konfiguraci webového hostitele. Všechno, co je o spuštění aplikace by měly být zpracovány mimo `BuildWebHost` &mdash; obvykle v `Main` metodu *Program.cs*.

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a>Zkontrolujte nastavení kompilace zobrazení syntaxe Razor
Rychlejší doba spuštění aplikace a menší publikované sady mají velký důraz na vás. Z těchto důvodů [kompilace zobrazení syntaxe Razor](xref:mvc/views/view-compilation) je povoleno ve výchozím nastavení v technologii ASP.NET 2.0 jádra.

Nastavení `MvcRazorCompileOnPublish` vlastnost na hodnotu true se už nevyžaduje. Pokud se zakázání kompilace zobrazení, vlastnost mohl být odebrán z *.csproj* souboru.

Při cílení na rozhraní .NET Framework, stále je třeba explicitně odkazovat [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) balíček NuGet ve vaší *.csproj* souboru:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>Funkce "light-up" Application Insights
Snadné nastavení výkonu instrumentace aplikací je důležité. Nyní můžete spoléhat na novém [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" funkce dostupné v nástroji Visual Studio 2017.

Projekty ASP.NET Core 1.1 vytvořené v aplikaci Visual Studio 2017 přidat Application Insights ve výchozím nastavení. Pokud nepoužíváte Application Insights SDK přímo, mimo *Program.cs* a *Startup.cs*, postupujte takto:

1. Pokud cílení na .NET Core, odeberte následující `<PackageReference />` uzlu z *.csproj* souboru:
    
    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. Pokud cílení na .NET Core, odeberte `UseApplicationInsights` volání metody rozšíření z *Program.cs*:

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. Odeberte volání rozhraní API klienta služby Application Insights z *_Layout.cshtml*. Obsahuje následující dva řádky kódu:

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

Pokud používáte Application Insights SDK přímo, nadále používat. Rozhraní 2.0 [metapackage](xref:fundamentals/metapackage) obsahuje nejnovější verzi Application Insights, tak k chybě přechod na starší verzi balíčku se zobrazí v případě, že jste odkazující na starší verze.

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a>Přijmout nebo identitu ověřování vylepšení
Jádro ASP.NET 2.0 obsahuje nový model ověřování a několik významných změn ASP.NET Core Identity. Pokud jste vytvořili projekt s jednotlivé uživatelské účty, které jsou povolené, nebo pokud jste ručně přidali ověřování nebo Identity, najdete v části [migrovat ověřování a identita na technologii ASP.NET 2.0 základní](xref:migration/1x-to-2x/identity-2x).

## <a name="additional-resources"></a>Další zdroje

* [Nejnovější změny ve jádro ASP.NET 2.0](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
