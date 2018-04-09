---
title: Práce s LocalDB serveru SQL v ASP.NET Core
author: rick-anderson
description: Další informace o použití SQL serveru LocalDB v jednoduchou aplikaci ASP.NET MVC jádra.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 3f69657cb21e163bdf00fb1faea98889046e9b45
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a>Práce s LocalDB serveru SQL v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` Objekt zpracovává úlohu s připojením k databázi a mapování `Movie` objekty záznamy v databázi. Kontext databáze není zaregistrována [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru v `ConfigureServices` metoda v *Startup.cs* souboru:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

ASP.NET Core [konfigurace](xref:fundamentals/configuration/index) systému čtení `ConnectionString`. Pro místní vývoj, získá připojovací řetězec z *appSettings.JSON určený* souboru:

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

Když nasadíte aplikaci k testu nebo produkčním serveru, můžete použít proměnné prostředí nebo jiný přístup k nastavení připojovacího řetězce k skutečné systému SQL Server. V tématu [konfigurace](xref:fundamentals/configuration/index) Další informace.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB je Odlehčená verze SQL serveru Express databázového stroje je cílová pro vývoj programu. LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není žádná komplexní konfigurace. Ve výchozím nastavení, vytvoří databáze LocalDB "\*.mdf" soubory *C: či uživatelů nebo\<uživatele\>*  adresáře.

* Z **zobrazení** nabídce otevřete **Průzkumník objektů systému SQL Server** (SSOX).

  ![Nabídka Zobrazit](working-with-sql/_static/ssox.png)

* Klikněte pravým tlačítkem na `Movie` tabulky **> Návrhář zobrazení**

  ![Otevřete v tabulce film kontextové nabídky](working-with-sql/_static/design.png)

  ![Tabulka film otevřít v Návrháři](working-with-sql/_static/dv.png)

Poznámka: na ikonu klíče do `ID`. Ve výchozím nastavení, bude EF vlastnost s názvem `ID` primární klíč.

* Klikněte pravým tlačítkem na `Movie` tabulky **> zobrazení dat**

  ![Otevřete v tabulce film kontextové nabídky](working-with-sql/_static/ssox2.png)

  ![Tabulka film otevřete zobrazení dat v tabulce](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a>Počáteční hodnoty databáze

Vytvořte novou třídu s názvem `SeedData` v *modely* složky. Generovaného kódu nahraďte následujícím textem:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Pokud jsou všechny filmy v databázi, vrátí inicializátoru počáteční hodnoty a jsou přidány žádné filmy.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Přidat inicializátoru počáteční hodnoty

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)
Přidat inicializátoru počáteční hodnoty do `Main` metoda v *Program.cs* souboru:

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x/)
Přidejte na konec inicializátoru počáteční hodnoty `Configure` metoda v *Startup.cs* souboru.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

* * *
Testování aplikace

* Odstraňte všechny záznamy v databázi. Můžete provést s odstranit odkazy v prohlížeči nebo z SSOX.
* Vynutit aplikace k chybě při inicializaci (volání metody v `Startup` třída), spustí metodu počáteční hodnoty. Pokud chcete vynutit inicializace, IIS Express musí být zastavena a restartována. Můžete to provést pomocí některé z následujících postupů:

  * Klikněte pravým tlačítkem na hlavním panelu ikonu IIS Express systému v oznamovací oblasti a klepněte na **ukončení** nebo **zastavit lokality**

    ![Služba IIS Express ikonu na hlavním panelu](working-with-sql/_static/iisExIcon.png)

    ![Kontextové nabídky](working-with-sql/_static/stopIIS.png)

    * Pokud VS byly spuštěny v režimu bez ladění, stisknutím klávesy F5 spusťte v režimu ladění
    * Pokud VS byly spuštěny v režimu ladění, zastavení ladicího programu a stisknutím klávesy F5

Aplikace zobrazuje dosazená data.

![Aplikace MVC film otevřete v Microsoft Edge znázorňující film data](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [Předchozí](adding-model.md)
> [další](controller-methods-views.md)  
