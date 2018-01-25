---
title: "Začínáme s ASP.NET Core a Entity Framework 6"
author: tdykstra
description: "Tento článek ukazuje, jak používat Entity Framework 6 v aplikaci ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 7f3c1f28c1e0b3a68db7f6f84c56b18643b56cc8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a>Začínáme s ASP.NET Core a Entity Framework 6

Podle [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), a [tní Dykstra](https://github.com/tdykstra)

Tento článek ukazuje, jak používat Entity Framework 6 v aplikaci ASP.NET Core.

## <a name="overview"></a>Přehled

Pokud chcete používat Entity Framework 6, váš projekt má zkompilovat pro rozhraní .NET Framework jako Entity Framework 6 nepodporuje .NET Core. Pokud potřebujete více platforem funkce budete muset upgradovat na [Entity Framework Core](https://docs.microsoft.com/ef/).

Doporučeným způsobem, jak používat Entity Framework 6 v aplikaci ASP.NET Core je uvést kontext EF6 a třídy modelu v knihovně tříd rozhraní úplné projektu s cílem. Přidáte odkaz na knihovnu tříd z projektu ASP.NET Core. Viz ukázka [řešení sady Visual Studio s projekty EF6 a ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).

Nelze blokovat kontextu EF6 v projektu ASP.NET Core, protože .NET Core projekty nepodporují všechny funkce, která EF6 příkazy, jako *Enable-Migrations* vyžadují.

Bez ohledu na typ projektu, ve kterém můžete najít váš kontext EF6 s kontextu EF6 pracovat pouze EF6 nástroje příkazového řádku. Například `Scaffold-DbContext` je dostupný jenom v Entity Framework Core. Pokud potřebujete zpětná analýza databáze do EF6 model, přečtěte si téma [Code First k existující databázi](https://msdn.microsoft.com/jj200620).

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>Odkaz na úplné framework a EF6 v projektu ASP.NET Core

Projekt ASP.NET Core musí odkazovat na rozhraní .NET framework a EF6. Například *.csproj* souboru projektu ASP.NET Core bude vypadat podobně jako v následujícím příkladu (jsou zobrazeny pouze odpovídající části souboru).

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

Pokud vytváříte nový projekt, použijte **webové aplikace ASP.NET Core (rozhraní .NET Framework)** šablony.

## <a name="handle-connection-strings"></a>Popisovač připojovací řetězce

EF6 nástroje příkazového řádku, které budete používat v projektu knihovny tříd EF6 vyžadují výchozí konstruktor, takže se můžete vytvořit instanci kontextu. Ale budete pravděpodobně chtít zadejte připojovací řetězec k použití v projektu ASP.NET Core, v takovém případě váš kontext konstruktor musí mít parametr, který umožňuje předat v připojovacím řetězci. Tady je příklad.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

Vzhledem k tomu, že váš kontext EF6 nemá konstruktor bez parametrů, musí poskytnout implementaci projektu EF6 [IDbContextFactory](https://msdn.microsoft.com/library/hh506876). Nástroje příkazového řádku EF6 vyhledá a použít tuto implementaci, takže se můžete vytvořit instanci kontextu. Tady je příklad.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

V tento ukázkový kód `IDbContextFactory` implementace předá v pevně připojovací řetězec. Toto je připojovací řetězec, který bude používat nástroje příkazového řádku. Budete chtít provádět strategii zajistit, že knihovna tříd používá stejný připojovací řetězec, který používá volající aplikace. Například může získat hodnotu z proměnné prostředí v obou projektů.

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>Nastavit vkládání závislostí v projektu ASP.NET Core

V projektu základní *Startup.cs* souboru, nastavte kontext EF6 pro vkládání závislostí (DI) v `ConfigureServices`. Objektů kontextu EF by měl určené pro jednotlivé požadavky životnost.

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

Pak můžete získat instance kontextu v řadičích pomocí DI. Kód je podobná by zápisu pro kontextu EF jádra:

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>Ukázkové aplikace

Ukázkovou aplikaci, práce, najdete v článku [ukázkové řešení sady Visual Studio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) doprovodný v tomto článku.

Tato ukázka lze vytvořit od začátku pomocí následujících kroků v sadě Visual Studio:

* Vytvoření řešení.

* **Přidat nový projekt > Web > ASP.NET Core webové aplikace (rozhraní .NET Framework)**

* **Přidat nový projekt > klasický desktopový Windows > třídy knihovny (rozhraní .NET Framework)**

* V **Konzola správce balíčků** (pomocí PMC) pro oba projekty, spusťte příkaz `Install-Package Entityframework`.

* V projektu knihovny tříd vytvořit datové třídy modelu a třídu kontextu a implementaci `IDbContextFactory`.

* Pomocí PMC pro projektu knihovny tříd, spusťte příkazy `Enable-Migrations` a `Add-Migration Initial`. Pokud jste nastavili ASP.NET Core projekt jako spouštěný projekt, přidejte `-StartupProjectName EF6` tyto příkazy.

* V projektu základní přidáte odkaz na projekt na projekt knihovny tříd.

* V projektu jádra v *Startup.cs*, zaregistrovat kontext pro DI.

* V projektu jádra v *appSettings.JSON určený*, přidejte připojovací řetězec.

* V projektu jádra přidejte řadiče a zobrazení, chcete-li ověřit, že můžete číst a zapisovat data. (Všimněte si, že generování uživatelského rozhraní ASP.NET MVC základní nebudou fungovat s kontextem EF6 na něj odkazovat z knihovny tříd.)

## <a name="summary"></a>Souhrn

Tento článek poskytl základní pokyny pro používání Entity Framework 6 v aplikaci ASP.NET Core.

## <a name="additional-resources"></a>Další prostředky

* [Rozhraní Entity Framework - konfigurace založené na kódu](https://msdn.microsoft.com/data/jj680699.aspx)
