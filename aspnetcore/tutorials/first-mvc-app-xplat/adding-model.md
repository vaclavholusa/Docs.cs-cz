---
title: Přidat model do aplikace ASP.NET MVC jádra
author: rick-anderson
description: Přidáte model do jednoduchou aplikaci ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 77750ba0df7775d6a0e4744811848bfe9782d995
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Přidat model do aplikace ASP.NET MVC jádra

[!INCLUDE [adding-model1](../../includes/mvc-intro/adding-model1.md)]

* Přidání třídy k *modely* složku s názvem *Movie.cs*.
* Přidejte následující kód, který *Models/Movie.cs* souboru:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Pole je vyžadováno pro databázi pro primární klíč. 

Vytvořit aplikaci pro ověření nemáte žádné chyby a nakonec jste přidali **M**odelu k vaší **M**VC aplikace.

## <a name="prepare-the-project-for-scaffolding"></a>Příprava projektu pro generování uživatelského rozhraní

- Přidejte následující zvýrazněnou balíčků NuGet *MvcMovie.csproj* souboru:
             
   [!code-csharp[](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Uložte tento soubor a vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".
- Vytvoření *Models/MvcMovieContext.cs* souboru a přidejte následující `MvcMovieContext` třídy:

   [!code-csharp[](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Otevřete *Startup.cs* souboru a přidejte dva direktiv Using:

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Přidat kontext databáze, který má *Startup.cs* souboru:

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Tato hodnota informuje Entity Framework, které třídy modelu jsou zahrnuty v datovém modelu. Definujete jeden *sady entit* film objektů, které bude možné vyjádřit v databázi jako film tabulku.

- Sestavte projekt a ověřte, zda že nejsou žádné chyby.

## <a name="scaffold-the-moviecontroller"></a>Vygenerované uživatelské rozhraní MovieController

Otevřete okno terminálu ve složce projektu a spusťte následující příkazy:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
Modul generování uživatelského rozhraní vytvoří následující:

* Řadič filmy (*Controllers/MoviesController.cs*)
* Zobrazení syntaxe Razor soubory vytvořit, odstranit, podrobnosti, úpravy a Index stránky (*zobrazení nebo filmy nebo\*.cshtml*)

Automatické vytváření [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení se označuje jako *generování uživatelského rozhraní*. Brzy budete mít funkční webovou aplikaci, která vám umožní spravovat film databáze.

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

Nyní máte databázi a stránkami zobrazit, upravit, aktualizovat a odstranit data. V dalším kurzu jsme budete pracovat s databází.

### <a name="additional-resources"></a>Další zdroje

* [Pomocné rutiny značek](xref:mvc/views/tag-helpers/intro)
* [Globalizace a lokalizace](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Předchozí - přidat zobrazení](adding-view.md)
> [další – práce s SQLite](working-with-sql.md)
