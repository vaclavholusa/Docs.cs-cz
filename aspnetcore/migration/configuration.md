---
title: Migruje konfiguraci pro jádro ASP.NET
author: ardalis
description: Zjistěte, jak migrovat konfigurace z projektu aplikace ASP.NET MVC do projektu aplikace ASP.NET MVC jádra.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: ead4f96aa0041cd919caa972d3bb05bd94a857b3
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851719"
---
# <a name="migrate-configuration-to-aspnet-core"></a>Migruje konfiguraci pro jádro ASP.NET

Podle [Steve Smith](https://ardalis.com/) a [Scott Addie](https://scottaddie.com)

V předchozím článku jsme začali [migrovat projektu aplikace ASP.NET MVC do architektury ASP.NET MVC základní](xref:migration/mvc). V tomto článku jsme migruje konfiguraci.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Nastavení konfigurace

ASP.NET Core už používá *Global.asax* a *web.config* soubory, které jsou použity v předchozích verzích technologie ASP.NET. V předchozích verzích technologie ASP.NET byl uveden logika spuštění aplikace do `Application_StartUp` metoda v rámci *Global.asax*. Později v architektuře ASP.NET MVC *Startup.cs* souboru je zahrnutý v kořenovém adresáři projektu; a byla volána, když se aplikace spustí. ASP.NET Core přijal tento přístup zcela tím, že všechny logika spuštění v *Startup.cs* souboru.

*Web.config* souboru nahradila také v ASP.NET Core. Konfigurace samotného může být nyní nakonfigurována, jako součást spuštění aplikace postup popsaný v *Startup.cs*. Konfigurace může stále používat soubory XML, ale obvykle projektů ASP.NET Core umístí hodnoty konfigurace v souboru ve formátu JSON, jako *appSettings.JSON určený*. ASP.NET Core konfiguračního systému může také snadný přístup k proměnné prostředí, které můžete zadat [zabezpečení a odolnosti umístění](xref:security/app-secrets) pro konkrétní prostředí hodnoty. To platí hlavně pro tajné klíče jako připojovací řetězce a klíče rozhraní API, které by neměla být zaškrtnuta do správy zdrojového kódu. V tématu [konfigurace](xref:fundamentals/configuration/index) Další informace o konfiguraci v ASP.NET Core.

V tomto článku jsme začínáte s částečně migrované projektu ASP.NET Core z [předchozím článku](xref:migration/mvc). Chcete-li nastavit konfiguraci, přidejte následující konstruktor a vlastnost, která má *Startup.cs* soubor umístěný v kořenovém adresáři projektu:

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

Poznámka, která v tomto okamžiku *Startup.cs* soubor nebude kompilovat, protože musíme přidejte následující `using` příkaz:

```csharp
using Microsoft.Extensions.Configuration;
```

Přidat *appSettings.JSON určený* souboru do kořenového adresáře projektu pomocí šablony odpovídající položky:

![Přidat AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Migrace nastavení konfigurace ze souboru web.config

Naše projektu ASP.NET MVC zahrnout požadované databáze připojovací řetězec v *web.config*v `<connectionStrings>` elementu. V našem projektu ASP.NET Core přidáme k ukládání těchto informací v *appSettings.JSON určený* souboru. Otevřete *appSettings.JSON určený*a Všimněte si, že již zahrnuje následující:

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

Ve zvýrazněný řádek použité v ukázkách výše, změňte název databáze z **_CHANGE_ME** na název vaší databáze.

## <a name="summary"></a>Souhrn

ASP.NET Core umístí všechny logika spuštění aplikace v jednom souboru, ve kterém může být nezbytné služby a závislosti definované a nakonfigurovaná. Nahradí *web.config* soubor s flexibilní konfigurace funkcí, které můžete využít celou řadu formáty souborů, například JSON, jakož i proměnné prostředí.
