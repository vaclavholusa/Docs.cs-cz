---
title: Metabalíček Microsoft.aspnetcore.all pro ASP.NET Core 2.0
author: Rick-Anderson
description: Metabalíček Microsoft.aspnetcore.all se nedoporučuje pro ASP.NET Core 2.1 nebo novější.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: d95bafd412969bb8db38499bd2ff01af510d872c
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148847"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Metabalíček Microsoft.aspnetcore.all pro ASP.NET Core 2.0

> [!NOTE]
> Doporučujeme aplikace pro ASP.NET Core 2.1 a později použít [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) namísto tohoto balíčku. Zobrazit [migrace z metabalíček na Microsoft.AspNetCore.App](#migrate) v tomto článku.

Tato funkce vyžaduje ASP.NET Core 2.x cílení na .NET Core 2.x.

[Metabalíček](https://www.nuget.org/packages/Microsoft.AspNetCore.All) Microsoft.AspNetCore.All pro ASP.NET Core zahrnuje:

* Všechny podporované balíčky vytvořené týmem ASP.NET Core.
* Všechny podporované balíčky pomocí Entity Framework Core.
* Interní závislosti a závislosti třetích stran používané v rámci ASP.NET Core a Entity Framework Core.

Všechny funkce aplikace ASP.NET Core 2.x a Entity Framework Core 2.x jsou součástí `Microsoft.AspNetCore.All` balíčku. Tento balíček použít výchozí šablony projektu cílení ASP.NET Core 2.0.

Číslo verze `Microsoft.AspNetCore.All` Microsoft.aspnetcore.all představuje verzi technologie ASP.NET Core a Entity Framework Core verze.

Aplikace, které používají `Microsoft.AspNetCore.All` Microsoft.aspnetcore.all automaticky využijí [.NET Core Runtime Store](/dotnet/core/deploying/runtime-store). Modul Runtime Store obsahuje všechny prostředky modulu runtime potřebné ke spouštění ASP.NET Core 2.x aplikací. Při použití `Microsoft.AspNetCore.All` Microsoft.aspnetcore.all, **žádné** nasazení se aplikace používají prostředky z balíčků odkazovaných ASP.NET Core NuGet &mdash; Store .NET Core Runtime obsahuje tyto prostředky. Prostředky v modulu Runtime Store předkompilovány zlepšit dobu spuštění aplikace.

Proces oříznutí balíčku můžete použít k odebrání balíčků, které nepoužíváte. Oříznutý balíčky jsou vyloučeny ve výstupu publikovanou aplikaci.

Následující *.csproj* souboru odkazy `Microsoft.AspNetCore.All` Microsoft.aspnetcore.all pro ASP.NET Core:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a>Implicitní vytváření verzí

V ASP.NET Core 2.1 nebo novější, můžete zadat `Microsoft.AspNetCore.All` odkaz bez verze balíčku. Pokud není zadána verze, je určen implicitní verze sady SDK (`Microsoft.NET.Sdk.Web`). Doporučujeme, abyste spoléhat na implicitní verze určené sady SDK a nastavení nejsou explicitně číslo verze na odkaz na balíček. Pokud máte dotazy týkající se tento přístup, ponechte v Githubu komentář [diskuse implicitní verze Microsoft.AspNetCore.App](https://github.com/aspnet/Docs/issues/6430).

Implicitní verze je nastaveno na `major.minor.0` pro přenosné aplikace. Mechanismus vpřed sdílené architektuře aplikace spouští nejnovější kompatibilní verzi mezi nainstalovaných sdílené architektury. Chcete-li zaručit, že při vývoji se používá stejnou verzi, testovacím i produkčním prostředí, ujistěte se, že stejná verze modulu sdílené framework je nainstalována ve všech prostředích. Samostatná aplikace, číslo verze implicitní je nastavena na `major.minor.patch` sdílené Framework dodávat v nainstalované sady SDK.

Zadáním čísla verze na `Microsoft.AspNetCore.All` nemá odkaz na balíček **není** zaručit, že tato verze sdíleného framework je vybrán. Předpokládejme například, ale není určená verze "2.1.1", ale je nainstalovaná "2.1.3". V takovém případě bude aplikace používat "2.1.3". Však není doporučena, můžete zakázat Posunutí vpřed (opravy a/nebo menší). Další informace týkající se příkaz dotnet hostitele vpřed a jak nakonfigurovat své chování najdete v tématu [dotnet hostitele dopředné posunutí](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

Sady SDK projektu musí být nastaveno na `Microsoft.NET.Sdk.Web` v souboru projektu použije implicitní verzi `Microsoft.AspNetCore.All`. Když `Microsoft.NET.Sdk` SDK je zadána (`<Project Sdk="Microsoft.NET.Sdk">` v horní části souboru projektu), následující upozornění:

*NU1604 upozornění: Závislost projektu metabalíček neobsahuje inkluzivní dolní mez. Dolní mez zahrňte do verze závislosti zajistit výsledky obnovení budou konzistentní.*

Jedná se o známý problém s .NET Core 2.1 SDK a bude opravený v .NET Core 2.2 SDK.

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Migrace z metabalíček na Microsoft.AspNetCore.App

Následující balíčky jsou součástí `Microsoft.AspNetCore.All` , ale ne `Microsoft.AspNetCore.App` balíčku.

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

K přesunutí z `Microsoft.AspNetCore.All` k `Microsoft.AspNetCore.App`, když vaše aplikace používá libovolné rozhraní API z výše uvedených balíčků, nebo v režimu balíčky těchto balíčků, přidejte odkazy na tyto balíčky v projektu.

Všechny závislosti z předchozí balíčky, které jinak nejsou závislosti `Microsoft.AspNetCore.App` implicitně nejsou zahrnuty. Příklad:

* `StackExchange.Redis` jako závislost `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` jako závislost `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`

## <a name="update-aspnet-core-21"></a>Aktualizace ASP.NET Core 2.1

Doporučujeme migrovat na `Microsoft.AspNetCore.App` Microsoft.aspnetcore.all 2.1 nebo novější. Pokud chcete dál používat `Microsoft.AspNetCore.All` Microsoft.aspnetcore.all a ověřte nasazení nejnovější verze opravy:

* Na počítačích vývojářů a buildovací servery: nainstalujte nejnovější [.NET Core SDK](https://www.microsoft.com/net/download).
* Na serverech nasazení: nainstalujte nejnovější [.NET Core runtime](https://www.microsoft.com/net/download).
 Vaše aplikace se posunout vpřed byla nejnovější nainstalovaná verze na restartování aplikace.
