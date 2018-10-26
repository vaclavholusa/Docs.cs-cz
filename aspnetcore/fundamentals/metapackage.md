---
title: Metabalíček Microsoft.aspnetcore.all pro ASP.NET Core 2.0
author: Rick-Anderson
description: Metabalíček Microsoft.aspnetcore.all se nedoporučuje pro ASP.NET Core 2.1 nebo novější.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/metapackage
ms.openlocfilehash: f78684cb31976f976aec5e1773bcc728dfecc82e
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090703"
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

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

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
