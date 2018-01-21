---
title: "Přidání modelu do aplikace ASP.NET MVC jádra."
author: rick-anderson
description: "Přidáte model do jednoduchou aplikaci ASP.NET Core."
ms.author: riande
ms.date: 09/18/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
manager: wpickett
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 4c09225c925c326da7e815b39f176325a04fc17b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* Přidání třídy k *modely* složku s názvem *Movie.cs*.
* Přidejte následující kód, který *Models/Movie.cs* souboru:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Pole je vyžadováno pro databázi pro primární klíč. 

Vytvořit aplikaci pro ověření nemáte žádné chyby a nakonec jste přidali **M**odelu k vaší **M**VC aplikace.

## <a name="prepare-the-project-for-scaffolding"></a>Příprava projektu pro generování uživatelského rozhraní

- Přidejte následující zvýrazněnou balíčků NuGet *MvcMovie.csproj* souboru:
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Uložte tento soubor a vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".
- Vytvoření *Models/MvcMovieContext.cs* souboru a přidejte následující `MvcMovieContext` třídy:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Otevřete *Startup.cs* souboru a přidejte dva direktiv Using:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Přidat kontext databáze, který má *Startup.cs* souboru:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Tato hodnota informuje Entity Framework, které třídy modelu jsou zahrnuty v datovém modelu. Definujete jeden *sady entit* film objektů, které bude možné vyjádřit v databázi jako film tabulku.

- Sestavte projekt a ověřte, zda že nejsou žádné chyby.

## <a name="scaffold-the-moviecontroller"></a>Vygenerované uživatelské rozhraní MovieController

Otevřete okno terminálu ve složce projektu a spusťte následující příkazy:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> Pokud dojde k chybě při spuštění příkazu generování uživatelského rozhraní, přečtěte si téma [vydání 444 v úložišti generování uživatelského rozhraní](https://github.com/aspnet/scaffolding/issues/444) alternativní řešení.

Modul generování uživatelského rozhraní vytvoří následující:

* Řadič filmy (*Controllers/MoviesController.cs*)
* Zobrazení syntaxe Razor soubory vytvořit, odstranit, podrobnosti, úpravy a Index stránky (*zobrazení nebo filmy nebo\*.cshtml*)

Automatické vytváření [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení se označuje jako *generování uživatelského rozhraní*. Brzy budete mít funkční webovou aplikaci, která vám umožní spravovat film databáze.

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

Nyní máte databázi a stránkami zobrazit, upravit, aktualizovat a odstranit data. V dalším kurzu jsme budete pracovat s databází.

### <a name="additional-resources"></a>Další zdroje

* [Pomocné rutiny značek](xref:mvc/views/tag-helpers/intro)
* [Globalizace a lokalizace](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Předchozí - přidat zobrazení](adding-view.md)
[další – práce s SQLite](working-with-sql.md)
