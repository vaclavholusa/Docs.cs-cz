---
title: "Práce s SQL Server LocalDB a ASP.NET Core"
author: rick-anderson
description: "Vysvětluje práci s SQL Server LocalDB a ASP.NET Core."
keywords: "ASP.NET Core, stránky Razor, Razor, MVC, SQL, databáze LocalDB"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 1e6ea093317527eecd5909449ac1973ca13cfc32
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a>Práce s SQL Server LocalDB a ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Audette Jan](https://twitter.com/joeaudette) 

`MovieContext` Objekt zpracovává úlohu s připojením k databázi a mapování `Movie` objekty záznamy v databázi. Kontext databáze není zaregistrována [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru v `ConfigureServices` metoda v *Startup.cs* souboru:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

ASP.NET Core [konfigurace](xref:fundamentals/configuration/index) systému čtení `ConnectionString`. Pro místní vývoj, získá připojovací řetězec z *appSettings.JSON určený* souboru:

[!code-json[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

Když nasadíte aplikaci k testu nebo produkčním serveru, můžete použít proměnné prostředí nebo jiný přístup k nastavení připojovacího řetězce k skutečné systému SQL Server. V tématu [konfigurace](xref:fundamentals/configuration/index) Další informace.

## <a name="sql-server-express-localdb"></a>Databáze SQL Server Express LocalDB

LocalDB je Odlehčená verze SQL serveru Express databázového stroje je cílová pro vývoj programu. LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není žádná komplexní konfigurace. Ve výchozím nastavení, vytvoří databáze LocalDB "\*.mdf" soubory *C: či uživatelů nebo\<uživatele\>*  adresáře.

<a name="ssox"></a>
* Z **zobrazení** nabídce otevřete **Průzkumník objektů systému SQL Server** (SSOX).

  ![Nabídka Zobrazit](sql/_static/ssox.png)

* Klikněte pravým tlačítkem na `Movie` tabulky a vyberte **Návrhář zobrazení**:

  ![Otevřete v tabulce film kontextové nabídky](sql/_static/design.png)

  ![Tabulka film otevřít v Návrháři](sql/_static/dv.png)

Poznámka: na ikonu klíče do `ID`. Ve výchozím nastavení vytvoří EF vlastnost s názvem `ID` pro primární klíč.

* Klikněte pravým tlačítkem na `Movie` tabulky a vyberte **Data zobrazení**:

  ![Tabulka film otevřete zobrazení dat v tabulce](sql/_static/vd22.png)

## <a name="seed-the-database"></a>Počáteční hodnoty databáze

Vytvořte novou třídu s názvem `SeedData` v *modely* složky. Generovaného kódu nahraďte následujícím textem:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

Pokud jsou všechny filmy v databázi, vrátí inicializátoru počáteční hodnoty a jsou přidány žádné filmy.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Přidat inicializátoru počáteční hodnoty

Přidejte na konec inicializátoru počáteční hodnoty `Main` metoda v *Program.cs* souboru:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

Testování aplikace

* Odstraňte všechny záznamy v databázi. To lze provést pomocí odstranění odkazy v prohlížeči nebo z [SSOX](xref:tutorials/razor-pages/new-field#ssox)
* Vynutit aplikace k chybě při inicializaci (volání metody v `Startup` třída), spustí metodu počáteční hodnoty. Pokud chcete vynutit inicializace, IIS Express musí být zastavena a restartována. Můžete to provést pomocí některé z následujících postupů:

  * Klikněte pravým tlačítkem na hlavním panelu ikonu IIS Express systému v oznamovací oblasti a klepněte na **ukončení** nebo **zastavit lokality**:

    ![Služba IIS Express ikonu na hlavním panelu](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Kontextové nabídky](sql/_static/stopIIS.png)

   * Pokud VS byly spuštěny v režimu bez ladění, stisknutím klávesy F5 spusťte v režimu ladění.
   * Pokud VS byly spuštěny v režimu ladění, zastavení ladicího programu a stisknutím klávesy F5.
   
Aplikace zobrazuje dosazená data:

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](sql/_static/m55.png)

V dalším kurzu se vyčistit prezentaci dat.

>[!div class="step-by-step"]
[Předchozí: Vygeneroval stránky Razor](xref:tutorials/razor-pages/page)
[Další: aktualizace stránky](xref:tutorials/razor-pages/da1)
