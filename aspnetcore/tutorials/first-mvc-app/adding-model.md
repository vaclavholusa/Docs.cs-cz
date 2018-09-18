---
title: Přidání modelu pro aplikace ASP.NET Core MVC
author: rick-anderson
description: Přidání modelu pro jednoduchou aplikaci ASP.NET Core.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 5a820789ee3a761025d09aa78f3c42e59fc5fa38
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011375"
---
[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

Klikněte pravým tlačítkem myši *modely* složky > **přidat** > **třídy**. Název třídy **film** a přidejte následující vlastnosti:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Pole vyžaduje databázi pro primární klíč. 

Sestavte projekt a ověřte, že nemáte žádné chyby. Teď máte **M**odelu ve vaší **M**VC aplikace.

## <a name="scaffolding-a-controller"></a>Generování uživatelského rozhraní kontroleru

::: moniker range=">= aspnetcore-2.1"

V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *řadiče* složky **> Přidat > novou vygenerovanou položku**.

![Přehled nad krok](adding-model/_static/add_controller21.png)

V **přidat vygenerované uživatelské rozhraní** dialogového okna, klepněte na **kontroler MVC se zobrazeními, s použitím Entity Framework > Přidat**.

![Přidat vygenerované uživatelské rozhraní – dialogové okno](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složky **> Přidat > Kontroleru**.

![Přehled nad krok](adding-model/_static/add_controller.png)

Pokud **přidat závislosti MVC** se zobrazí dialogové okno:

* [Aktualizace na nejnovější verzi sady Visual Studio](https://www.visualstudio.com/downloads/). Visual Studio verze starší než 15.5 zobrazí tento dialog.
* Pokud nelze aktualizovat, vyberte **přidat**a pak postupujte podle kroků přidat kontroler.

V **přidat vygenerované uživatelské rozhraní** dialogového okna, klepněte na **kontroler MVC se zobrazeními, s použitím Entity Framework > Přidat**.

![Přidat vygenerované uživatelské rozhraní – dialogové okno](adding-model/_static/add_scaffold2.png)

::: moniker-end

Dokončení **přidat kontroler** dialogové okno:

* **Třída modelu:** *Movie (MvcMovie.Models)*
* **Třída kontextu dat:** vyberte **+** ikonu a přidejte výchozí **MvcMovie.Models.MvcMovieContext**

![Přidat kontext dat](adding-model/_static/dc.png)

* **Zobrazení:** zachovat výchozí jednotlivé možnosti zaškrtnuto
* **Název kontroleru:** ponechte výchozí *MoviesController*
* Klepněte na **přidat**

![Přidat dialog Kontroleru](adding-model/_static/add_controller2.png)

Visual Studio vytvoří:

* Entity Framework Core [třídy kontextu databáze](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* Kontroler filmy (*Controllers/MoviesController.cs*)
* Zobrazení Razor soubory vytvořit, odstranit, podrobností, úpravy a Index stránky (<em>zobrazení/filmy/&ast;.cshtml</em>)

Automatické vytváření kontext databáze a [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvoření, čtení, aktualizace a odstranění) metody akce a zobrazení se označuje jako *generování uživatelského rozhraní*. Brzy budete mít funkční webovou aplikaci, která vám umožní spravovat databáze filmů.

Pokud spustíte aplikaci a klikněte na **Mvc Movie** odkaz, dojde k chybě podobný následujícímu:

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

Je potřeba vytvořit databázi a budete používat jádro EF [migrace](xref:data/ef-mvc/migrations) funkce, které provedete. Migrace vám umožní vytvořit databázi, která odpovídá datového modelu a aktualizovat schéma databáze, pokud datový model změny.

## <a name="add-ef-tooling-and-perform-initial-migration"></a>Přidat EF nástroje a provádět počáteční migraci

V této části použijete konzoly Správce balíčků (PMC) na:

* Přidání balíčku Entity Framework Core Tools. Tento balíček je potřeba přidat migrace a aktualizaci databáze.
* Přidáte počáteční migraci.
* Aktualizujte počáteční migraci databáze.

Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.

<!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC nabídky](adding-model/_static/pmc.png)

V konzole PMC zadejte následující příkazy:

::: moniker range=">= aspnetcore-2.1"

``` PMC
Add-Migration Initial
Update-Database
```

Ignorovat tuto chybovou, opravíme v dalším kurzu:

*Microsoft.EntityFrameworkCore.Model.Validation[30000]*  
      *Pro desetinných sloupec "Price" na typ entity "Video" nebyl zadán žádný typ. To způsobí, že hodnoty bylo tiché zkrácení, pokud to není ve výchozí přesnost a měřítko. Explicitně zadejte typ sloupce serveru SQL, který zvládne všech hodnot pomocí "ForHasColumnType()".*

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Poznámka:** Pokud obdržíte chybu s `Install-Package` příkaz, otevřete Správce balíčků NuGet, vyhledejte `Microsoft.EntityFrameworkCore.Tools` balíčku. To umožňuje nainstalovat balíček nebo zkontrolujte, jestli je už nainstalovaná. Alternativně si zobrazte [rozhraní příkazového řádku přístup](#cli) Pokud máte problémy s konzolu PMC.

::: moniker-end

`Add-Migration` Příkaz vytvoří kód, který vytvoří schéma počáteční databáze. Schéma je založen na zadaném v modelu `DbContext`(v *Data/MvcMovieContext.cs* souboru). `Initial` Argument se používá k pojmenování migrace. Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci. Zobrazit [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.

`Update-Database` Příkaz spustí `Up` metodu *migrace /\<časové razítko > _Initial.cs* soubor, který vytvoří databázi.

<a name="cli"></a> Místo konzolu PMC provedením předcházejících krocích pomocí rozhraní příkazového řádku (CLI):

* Přidat [nástroje EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) k *.csproj* souboru.
* Spuštěním následujících příkazů z konzoly (v adresáři projektu):

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  Pokud spustíte aplikaci a zobrazí chybová zpráva:

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

Zřejmě nespustily `dotnet ef database update`.

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Technologie IntelliSense místní nabídku pro položku modelu seznam dostupných vlastností pro ID, ceny, datum vydání a název](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Další zdroje

* [Pomocné rutiny značek](xref:mvc/views/tag-helpers/intro)
* [Globalizace a lokalizace](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Předchozím přidávání zobrazení](adding-view.md)
> [další práce s SQL](working-with-sql.md)  
