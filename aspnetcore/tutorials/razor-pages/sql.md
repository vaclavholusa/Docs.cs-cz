---
title: Práce s SQL Server LocalDB a ASP.NET Core
author: rick-anderson
description: Vysvětluje, práci s SQL Server LocalDB a ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: ef4e1fb3bf1ac1b3695ff89d6692ac6fa1641e31
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/22/2018
ms.locfileid: "41820156"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a>Práce s SQL Server LocalDB a ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Joe Audette](https://twitter.com/joeaudette) 

`MovieContext` Objekt zpracovává úlohu s připojením k databázi a mapování `Movie` objekty se záznamy v databázi. Kontext databáze je zaregistrován [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru v `ConfigureServices` metoda ve *Startup.cs* souboru:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Další informace o metodách `ConfigureServices`, naleznete v tématu:

* [Podpora EU obecného Regulation (GDPR) v ASP.NET Core](xref:security/gdpr) pro `CookiePolicyOptions`.
* [SetCompatibilityVersion](xref:mvc/compatibility-version)

::: moniker-end

ASP.NET Core [konfigurace](xref:fundamentals/configuration/index) systému čtení `ConnectionString`. Pro místní vývoj, získá připojovací řetězec z *appsettings.json* souboru. Hodnota názvu databáze (`Database={Database name}`) se bude lišit pro vygenerovaný kód. Hodnota názvu je volitelný.

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

Když nasadíte aplikaci na testovacím nebo produkčním serveru, můžete použít proměnné prostředí nebo jiné přístup se nastavit připojovací řetězec na skutečný SQL Server. Zobrazit [konfigurace](xref:fundamentals/configuration/index) Další informace.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB je Odlehčená verze SQL serveru Express databázového stroje, která je určená pro vývoj v programu. LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není bez složité konfigurace. Ve výchozím nastavení, vytvoří databázi LocalDB "\*.mdf" soubory *C:/uživatele/\<uživatele\>*  adresáře.

<a name="ssox"></a>
* Z **zobrazení** nabídce otevřete **Průzkumník objektů systému SQL Server** (SSOX).

  ![Nabídka Zobrazit](sql/_static/ssox.png)

* Klikněte pravým tlačítkem na `Movie` tabulce a vybrat **Návrhář zobrazení**:

  ![Kontextová nabídka otevřít na tabulky Movie](sql/_static/design.png)

  ![Otevřít v Návrháři tabulky Movie](sql/_static/dv.png)

Poznámka: na ikonu klíče vedle `ID`. Ve výchozím nastavení, EF vytvoří vlastnost s názvem `ID` pro primární klíč.

* Klikněte pravým tlačítkem na `Movie` tabulce a vybrat **Data zobrazení**:

  ![Otevřít zobrazení tabulky dat tabulky Movie](sql/_static/vd22.png)

## <a name="seed-the-database"></a>Přidání dat do databáze

Vytvořte novou třídu s názvem `SeedData` v *modely* složky. Generovaného kódu nahraďte následujícím kódem:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

Pokud jsou všechny filmy v databázi, vrátí inicializátoru pro dosazení hodnot a jsou přidány žádné video.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Přidat inicializační výraz počáteční hodnoty

V *Program.cs*, změnit `Main` metoda můžete provádět následující:

* Instance kontextu databáze získáte z kontejneru pro vkládání závislostí.
* Volejte metodu počáteční hodnoty předání kontextu.
* Kontext Dispose po dokončení počáteční hodnoty metody.

Následující kód ukazuje aktualizovaný *Program.cs* souboru.

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

Produkční aplikace by volat `Database.Migrate`. Přidá se do předchozí kód, aby se zabránilo následující výjimku při `Update-Database` nebyl spuštěn:

SqlException: Databázi "RazorPagesMovieContext 21" požadovaný v přihlášení nelze otevřít. Přihlášení se nezdařilo.
Přihlašovací jméno uživatele 'jméno uživatele' se nezdařilo.

### <a name="test-the-app"></a>Testování aplikace

* Odstraníte všechny záznamy z databáze. To lze provést pomocí odstranit odkazy v prohlížeči nebo z [SSOX](xref:tutorials/razor-pages/new-field#ssox)
* Se aplikace inicializuje (volat metody ve `Startup` třídy), spustí seed – metoda. Pokud chcete vynutit inicializace, služba IIS Express musí zastavit, restartovat. Provést s některou z následujících postupů:

  * Klikněte pravým tlačítkem na službu IIS Express systému na hlavním panelu ikonu v oznamovací oblasti a klepněte na **ukončovací** nebo **zastavení webu**:

    ![Služba IIS Express ikonu na hlavním panelu](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Kontextové nabídky](sql/_static/stopIIS.png)

    * Pokud VS byly spuštěny v režimu bez ladění, stiskněte klávesu F5 ke spuštění v režimu ladění.
    * Pokud jste používali zastavení ladicího programu VS v režimu ladění a stisknutím klávesy F5.
   
Aplikace bude zobrazovat dosazená data:

![Otevřít v prohlížeči Chrome ukazující data o filmech aplikace Movie](sql/_static/m55.png)

V dalším kurzu se vyčistit prezentace data.

> [!div class="step-by-step"]
> [Předchozí: Generované uživatelské rozhraní pro stránky Razor](xref:tutorials/razor-pages/page)
> [Další: aktualizace stránek](xref:tutorials/razor-pages/da1)
