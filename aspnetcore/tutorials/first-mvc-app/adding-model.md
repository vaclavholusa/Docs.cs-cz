---
title: "Přidání modelu do aplikace ASP.NET MVC jádra"
author: rick-anderson
description: "Přidáte model do jednoduchou aplikaci ASP.NET Core."
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 7469546494ec54bfe36bc5bd2f5f9702889ddf4a
ms.sourcegitcommit: 2e61e287e220eddd5f3f4cd9147aa6417cfd9236
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

Poznámka: Šablony ASP.NET 2.0 základní obsahují *modely* složky.

V Průzkumníku řešení klikněte pravým tlačítkem **MvcMovie** Projekt > **přidat** > **novou složku**. Název složky *modely*.

Klikněte pravým tlačítkem *modely* složky > **přidat** > **třída**. Název třídy **film** a přidejte následující vlastnosti:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Pole je vyžadováno pro databázi pro primární klíč. 

Sestavte projekt a ověřte, že nemáte žádné chyby. Nyní máte **M**odelu ve vaší **M**VC aplikace.

## <a name="scaffolding-a-controller"></a>Řadič generování uživatelského rozhraní

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složky **> Přidat > řadiče**.

![zobrazení výše krok](adding-model/_static/add_controller.png)

V **přidat závislosti MVC** dialogovém okně, vyberte **minimální závislosti**a vyberte **přidat**.

![zobrazení výše krok](adding-model/_static/add_depend.png)

Visual Studio přidává závislosti nutné chcete vygenerovat řadič, ale samotný řadič není vytvořena. Další vyvolat z **> Přidat > řadiče** vytvoří kontroler. 

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složky **> Přidat > řadiče**.

![zobrazení výše krok](adding-model/_static/add_controller.png)

V **přidat vygenerované uživatelské rozhraní** dialogové okno, klepněte na **kontroler MVC se zobrazeními s využitím nástroje Entity Framework > Přidat**.

![Přidat vygenerované uživatelské rozhraní – dialogové okno](adding-model/_static/add_scaffold2.png)

Dokončení **přidat kontroler** dialogové okno:

* **Třída modelu:** *Movie (MvcMovie.Models)*
* **Třída kontextu dat:** vyberte  **+**  ikonu a přidejte výchozí **MvcMovie.Models.MvcMovieContext**

![Přidat kontextu dat](adding-model/_static/dc.png)

* **Zobrazení:** zachovat ve výchozím nastavení mají jednotlivé možnosti zaškrtnuto
* **Název řadiče:** ponechat výchozí *MoviesController*
* Klepněte na **přidat**

![Dialogové okno řadiče přidání](adding-model/_static/add_controller2.png)

Visual Studio vytvoří:

* Entity Framework Core [databáze třída kontextu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* Řadič filmy (*Controllers/MoviesController.cs*)
* Zobrazení syntaxe Razor soubory vytvořit, odstranit, podrobnosti, úpravy a Index stránky (*zobrazení nebo filmy nebo&ast;.cshtml*)

Automatické vytváření kontext databáze a [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení se označuje jako *generování uživatelského rozhraní*. Brzy budete mít funkční webovou aplikaci, která vám umožní spravovat film databáze.

Pokud spustíte aplikaci a klikněte na **Mvc film** odkazu, který budete dojde k chybě podobný následujícímu:

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

Musíte vytvořit databázi, a budete používat jádro EF [migrace](xref:data/ef-mvc/migrations) funkci tak, aby to udělat. Migrace umožňuje vytvořit databázi, která odpovídá datový model a aktualizovat schéma databáze, když datový model změny.

## <a name="add-ef-tooling-and-perform-initial-migration"></a>Přidejte EF nástrojů a provedení počáteční migrace

V této části pomocí konzoly Správce balíčků (PMC) na:

* Přidejte balíček základní nástroje Entity Framework. Tento balíček je potřebné k přidání migrace a aktualizaci databáze.
* Přidáte počáteční migrace.
* Aktualizace databáze s počáteční migrace.

Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Pomocí PMC nabídky](adding-model/_static/pmc.png)

Pomocí PMC zadejte následující příkazy:

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Poznámka:** Pokud obdržíte chybu s `Install-Package` příkaz, otevřete Správce balíčků NuGet a vyhledejte `Microsoft.EntityFrameworkCore.Tools` balíčku. To vám umožňuje nainstalovat balíček nebo zkontrolujte, zda je již nainstalován. Můžete si také přečíst [rozhraní příkazového řádku přístup](#cli) Pokud máte problémy s pomocí PMC.

`Add-Migration` Příkaz vytvoří kód k vytvoření schématu počáteční databáze. Schéma je založena na zadaný ve model `DbContext`(v *Data/MvcMovieContext.cs* souboru). `Initial` Argument se používá k pojmenování byla migrace. Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci. V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.

`Update-Database` Příkaz spustí `Up` metoda v *migrace nebo\<časové razítko > _InitialCreate.cs* souboru, který vytvoří databázi.

<a name="cli"></a>Místo pomocí PMC můžete provést kroky předcházející pomocí rozhraní příkazového řádku (CLI):

* Přidat [EF základní nástrojů](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) k *.csproj* souboru.
* Spusťte následující příkazy z konzoly (v adresáři projektu):

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Kontextové nabídky IntelliSense pro položku modelu, seznam dostupných vlastností pro ID, ceny, datum vydání a název](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Další zdroje

* [Pomocníci značky](xref:mvc/views/tag-helpers/intro)
* [Globalizace a lokalizace](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Předchozí přidávání zobrazení](adding-view.md)
[další práci s SQL](working-with-sql.md)  