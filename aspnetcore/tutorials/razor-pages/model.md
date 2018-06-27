---
title: Přidat model do aplikace pro stránky Razor v ASP.NET Core
author: rick-anderson
description: Zjistit, jak přidat třídy pro správu filmy v databázi pomocí Entity Framework Core (EF jader).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: ed8faf8b3049adc7bcc7953d63ad805b0a836bd9
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961172"
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

Proces vygenerované uživatelské rozhraní vytvořené a změněné následující soubory:

### <a name="files-created"></a>Vytvořené soubory

* *Stránky/filmy* vytvořit, odstranit, podrobnosti, úpravy, Index. Tyto stránky jsou podrobně popsané v dalším kurzu.
* *Data/RazorPagesMovieContext.cs*

### <a name="files-updates"></a>Soubory aktualizací

* *Startup.cs*: změny tohoto souboru v jsou podrobně popsané v další části.
* *appSettings.JSON určený*: Přidání připojovací řetězec použitý pro připojení k místní databázi.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Zkontrolujte kontext zaregistrována vkládání závislostí

ASP.NET Core je vytvořené s [vkládání závislostí](xref:fundamentals/dependency-injection). Služby (například kontext databáze základní EF) jsou registrovány vkládání závislostí při spuštění aplikace. Součásti, které vyžadují tyto služby (například stránky Razor) jsou k dispozici tyto služby prostřednictvím konstruktor parametry. Později v tomto kurzu se zobrazí kód konstruktor, který získá kontext instanci databáze.

Nástroj pro generování uživatelského rozhraní automaticky vytvořen kontext databáze a zaregistrována kontejneru pro vkládání závislostí.

Zkontrolujte `Startup.ConfigureServices` metoda. Zvýrazněný řádek byl přidán modulem scaffolder:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Hlavní třída, která koordinuje EF základní funkce pro daný datový model je třídy kontextu databáze. Data kontextu je odvozený od [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Data kontextu určuje entit, které jsou zahrnuty v datovém modelu. V tomto projektu je třída s názvem `RazorPagesMovieContext`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

Předchozí kód vytvoří [DbSet\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) vlastnost pro sadu entit. V terminologii rozhraní Entity Framework obvykle sadu entit odpovídá do databázové tabulky. Entity odpovídá na řádek v tabulce.

Název připojovacího řetězce, je předaná do kontextu voláním metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) objektu. Pro místní vývoj [ASP.NET Core konfigurační systém](xref:fundamentals/configuration/index) čte připojovací řetězec z *appSettings.JSON určený* souboru.

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

`Add-Migration` Příkaz generuje kód pro vytvoření schématu počáteční databáze. Schéma je založena na zadaný ve model `RazorPagesMovieContext` (v *Data/RazorPagesMovieContext.cs* souboru). `Initial` Argument se používá k pojmenování byla migrace. Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci. V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.

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
