---
title: Microsoft.AspNetCore.All metapackage pro technologii ASP.NET 2.0 jádra a novější
author: Rick-Anderson
description: Microsoft.AspNetCore.All metapackage zahrnuje všechny podporované balíčků ASP.NET Core a Entity Framework Core, spolu s jejich závislosti.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: fbb76f41f3178ddc4e51faa14edece1869a30cd0
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729074"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Microsoft.AspNetCore.All metapackage pro technologii ASP.NET 2.0 jádra

> [!NOTE]
> Doporučujeme aplikací pro ASP.NET Core 2.1 a později [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) místo tohoto balíčku. V tématu [migrace z Microsoft.AspNetCore.All na Microsoft.AspNetCore.App](#migrate) v tomto článku.

Tato funkce vyžaduje rozhraní .NET zaměřená na aplikace ASP.NET Core 2.x základní 2.x.

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage pro ASP.NET Core zahrnuje:

* Všechny podporované balíčky tým ASP.NET Core.
* Všechny podporované balíčků základní Entity Framework. 
* Interní a 3. stran závislosti používané ASP.NET Core a Entity Framework Core. 

Všechny funkce ASP.NET Core 2.x a Entity Framework Core 2.x jsou součástí `Microsoft.AspNetCore.All` balíčku. Tento balíček použít výchozí šablony projektů cílení na technologii ASP.NET 2.0 jádra.

Číslo verze `Microsoft.AspNetCore.All` metapackage představuje ASP.NET Core verze a verze Entity Framework Core.

Aplikace, které používají `Microsoft.AspNetCore.All` metapackage automaticky využívat výhod [.NET Core Runtime úložiště](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). Modul Runtime úložiště obsahuje všechny prostředky potřebné ke spuštění 2.x aplikace ASP.NET Core runtime. Při použití `Microsoft.AspNetCore.All` metapackage, **žádné** prostředky z odkazované balíčky ASP.NET Core NuGet nasazených aplikací &mdash; úložiště .NET Core Runtime obsahuje tyto prostředky. Prostředky v úložišti Runtime jsou předkompilovaných ke zlepšení času spuštění aplikace.

Proces oříznutí balíčku můžete odebrat balíčky, které nepoužíváte. Oříznutý balíčky jsou vyloučeny ve výstupu publikované aplikace.

Následující *.csproj* souboru odkazy `Microsoft.AspNetCore.All` metapackage pro ASP.NET Core:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Migrace z Microsoft.AspNetCore.All na Microsoft.AspNetCore.App

Následující balíčky jsou součástí `Microsoft.AspNetCore.All` ale ne `Microsoft.AspNetCore.App` balíčku. 

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

Pro přesun z `Microsoft.AspNetCore.All` k `Microsoft.AspNetCore.App`, pokud vaše aplikace používá žádné rozhraní API z výše uvedených balíčky nebo balíčky nebude těchto balíčků, přidejte odkazy na tyto balíčky v projektu.

Všechny závislosti z předchozí balíčky, které jinak nejsou závislostí `Microsoft.AspNetCore.App` implicitně nejsou zahrnuty. Příklad:

* `StackExchange.Redis` jako závislosti `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` jako závislosti `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
