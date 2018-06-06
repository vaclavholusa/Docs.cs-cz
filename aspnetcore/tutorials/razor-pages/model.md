---
title: Přidat model do aplikace pro stránky Razor v ASP.NET Core
author: rick-anderson
description: Zjistit, jak přidat třídy pro správu filmy v databázi pomocí Entity Framework Core (EF jader).
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 50b1b01448ad870a2889db7dfe8367ab9e661840
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729960"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Přidat model do aplikace pro stránky Razor v ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Přidat datový model

V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**. Název složky *modely*.

Klikněte pravým tlačítkem *modely* složky. Vyberte **přidat** > **třída**. Název třídy **film** a přidejte následující vlastnosti:

Nahraďte obsah `Movie` třídy následujícím kódem:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>Vygenerované uživatelské rozhraní film modelu

V této části se vygeneroval film modelu. To znamená vytvoří nástroj pro generování uživatelského rozhraní stránky pro operace vytvoření, čtení, aktualizace a odstranění (CRUD) pro model film.

Vytvoření *stránkách nebo filmy* složky:

* V **Průzkumníku řešení**, klikněte pravým tlačítkem na *stránky* složky > **přidat** > **novou složku**.
* Název složky *filmy*

V **Průzkumníku řešení**, klikněte pravým tlačítkem na *stránkách nebo filmy* složky > **přidat** > **novou vygenerovanou položku**.

![Bitovou kopii z předchozích pokynů.](model/_static/sca.png)

V **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **stránky Razor pomocí Entity Framework (CRUD)** > **přidat**.

![Bitovou kopii z předchozích pokynů.](model/_static/add_scaffold.png)

Dokončení **přidat stránky Razor pomocí Entity Framework (CRUD)** dialogové okno:

* V **třída modelu** rozevírací nabídku, vyberte **Movie (RazorPagesMovie.Models)**.
* V **třída kontextu dat** řádek, vyberte **+** (a), přihlášení a přijměte vygenerovaný název **RazorPagesMovie.Models.RazorPagesMovieContext**.
* V **třída kontextu dat** rozevírací nabídku, vyberte **RazorPagesMovie.Models.RazorPagesMovieContext**
* Vyberte **přidat**.

![Bitovou kopii z předchozích pokynů.](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>Provedení počáteční migrace

V této části použijte konzoly Správce balíčků (PMC) na:

* Přidáte počáteční migrace.
* Aktualizace databáze s počáteční migrace.

Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.

  ![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

Pomocí PMC zadejte následující příkazy:

```PMC
Add-Migration Initial
Update-Database
```

Alternativně můžete použít následující příkazy .NET Core rozhraní příkazového řádku:

```console
dotnet ef migrations add Initial
dotnet ef database update
```

Následující zpráva upozornění ignorovat, kterou vyřešíte v dalším kurzu:

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

`Add-Migration` Příkaz generuje kód pro vytvoření schématu počáteční databáze. Schéma je založena na zadaný ve model `DbContext` (v *Models/MovieContext.cs* souboru). `Initial` Argument se používá k pojmenování byla migrace. Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci. V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.

`Update-Database` Příkaz spustí `Up` metoda v *migrace / {časové razítko} _InitialCreate.cs* souboru, který vytvoří databázi.

Pokud dojde k chybě:

SqlException: Nelze otevřít databázi "RazorPagesMovieContext identifikátor GUID" požadovaný v přihlášení. Přihlášení se nezdařilo.
Přihlášení se nezdařilo pro uživatele, uživatelského jména'.

Je provedena [krok migrace](#pmc).
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Přidat datový model

V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**. Název složky *modely*.

Klikněte pravým tlačítkem *modely* složky. Vyberte **přidat** > **třída**. Název třídy **film** a přidejte následující vlastnosti:

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Přidat připojovací řetězec databáze

Přidat připojovací řetězec k *appSettings.JSON určený* souboru.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Zaregistrovat kontext databáze

Zaregistrovat kontext databáze s [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru [ConfigureServices metodu třída při spuštění](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Sestavte projekt a ověřte, že nemáte žádné chyby.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Přidat vygenerované uživatelské rozhraní nástroje a provedení počáteční migrace

V této části použijte konzoly Správce balíčků (PMC) na:

* Přidejte balíček generování kódu webové sady Visual Studio. Tento balíček je potřeba spustit modul generování uživatelského rozhraní.
* Přidáte počáteční migrace.
* Aktualizace databáze s počáteční migrace.

Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.

  ![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

Pomocí PMC zadejte následující příkazy:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

Alternativně můžete použít následující příkazy .NET Core rozhraní příkazového řádku:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

Ignorujte se následující zpráva:

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

To opravíme v dalším kurzu.

`Install-Package` Příkaz nainstaluje nástroje potřebné ke spuštění modulu generování uživatelského rozhraní.

`Add-Migration` Příkaz generuje kód pro vytvoření schématu počáteční databáze. Schéma je založena na zadaný ve model `DbContext` (v *Models/MovieContext.cs* souboru). `Initial` Argument se používá k pojmenování byla migrace. Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci. V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.

`Update-Database` Příkaz spustí `Up` metoda v *migrace / {časové razítko} _InitialCreate.cs* souboru, který vytvoří databázi.

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>Testování aplikace

* Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/movies`).
* Testovací **vytvořit** odkaz.

  ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Testovací **upravit**, **podrobnosti**, a **odstranit** odkazy.

Pokud dojde k výjimce SQL, ověřte, jestli mají spuštění migrace a aktualizovat databázi.

Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.

> [!div class="step-by-step"]
> [Předchozí: Začínáme](xref:tutorials/razor-pages/razor-pages-start)
> [Další: vygeneroval stránky Razor](xref:tutorials/razor-pages/page)    
