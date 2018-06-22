---
title: Přidat model do aplikace ASP.NET MVC základní pomocí sady Visual Studio pro Mac
author: rick-anderson
description: Přidáte model do jednoduchou aplikaci ASP.NET Core.
ms.author: riande
ms.date: 09/22/2017
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 53d63cd554f6a3ec958f27ed35b0a30b1833f84c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276110"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app-with-visual-studio-for-mac"></a>Přidat model do aplikace ASP.NET MVC základní pomocí sady Visual Studio pro Mac

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model1.md)]

* Klikněte pravým tlačítkem myši *modely* složku a potom vyberte **přidat** > **nový soubor**. 
* V **nový soubor** dialogové okno:

  * Vyberte **Obecné** v levém podokně.
  * Vyberte **prázdné třídy** v problémové center.
  * Název třídy **film** a vyberte **nový**.

Přidejte následující vlastnosti pro `Movie` třídy:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Pole je vyžadováno pro databázi pro primární klíč.

Sestavte projekt a ověřte, že nemáte žádné chyby. Nyní máte **M**odelu ve vaší **M**VC aplikace.

## <a name="prepare-the-project-for-scaffolding"></a>Příprava projektu pro generování uživatelského rozhraní

- Klikněte pravým tlačítkem na soubor projektu a pak vyberte **nástroje > Upravit soubor**.

  ![zobrazení výše krok](adding-model/_static/1.png)

- Přidejte následující zvýrazněnou balíčků NuGet *MvcMovie.csproj* souboru:
             
  [!code-csharp[](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Uložte soubor.

- Vytvoření *Models/MvcMovieContext.cs* souboru a přidejte následující `MvcMovieContext` třídy:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Otevřete *Startup.cs* souboru a přidejte dva direktiv Using:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Přidat kontext databáze, který má *Startup.cs* souboru:

   [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Tato hodnota informuje Entity Framework, které třídy modelu jsou zahrnuty v datovém modelu. Definujete jeden *sady entit* film objektů, které bude možné vyjádřit v databázi jako film tabulku.

- Sestavte projekt a ověřte, zda že nejsou žádné chyby.

## <a name="scaffold-the-moviecontroller"></a>Vygenerované uživatelské rozhraní MovieController

Otevřete okno terminálu ve složce projektu a spusťte následující příkazy:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
Pokud dojde k chybě `No executable found matching command "dotnet-aspnet-codegenerator", verify`:

 * Pokud jste v adresáři projektu. Má adresáři projektu *Program.cs*, *Startup.cs* a *.csproj* soubory.
 * Je vaše dotnet verze 1.1 nebo vyšší. Spustit `dotnet` získat verzi.
 * Jste přidali `<DotNetCliToolReference>` elementu, který chcete [MvcMovie.csproj soubor](#prepare-the-project-for-scaffolding).
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

Modul generování uživatelského rozhraní vytvoří následující:

* Řadič filmy (*Controllers/MoviesController.cs*)
* Zobrazení syntaxe Razor soubory vytvořit, odstranit, podrobnosti, úpravy a Index stránky (*zobrazení nebo filmy nebo\*.cshtml*)

Automatické vytváření [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení se označuje jako *generování uživatelského rozhraní*. Brzy budete mít funkční webovou aplikaci, která vám umožní spravovat film databáze.

### <a name="add-the-files-to-visual-studio"></a>Přidání souborů do sady Visual Studio

* Přidat *MovieController.cs* souboru projektu sady Visual Studio:

  * Klikněte pravým tlačítkem na *řadiče* složky a vyberte **Přidat > Přidat soubory**.
  * Vyberte *MovieController.cs* souboru.

* Přidat *filmy* složek a zobrazení:

  * Klikněte pravým tlačítkem na *zobrazení* složky a vyberte **Přidat > Přidat existující složku**.
  * Přejděte na *zobrazení* složky, vyberte *Views\Movies*a potom vyberte **otevřete**.
  * V **vyberte soubory, které chcete přidat z filmy** vyberte **zahrnují všechny**a potom **OK**.

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

Nyní máte databázi a stránkami zobrazit, upravit, aktualizovat a odstranit data. V dalším kurzu jsme budete pracovat s databází.

## <a name="additional-resources"></a>Další zdroje

* [Pomocné rutiny značek](xref:mvc/views/tag-helpers/intro)
* [Globalizace a lokalizace](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Předchozí přidávání zobrazení](adding-view.md)
> [další práci s SQL](working-with-sql.md)  
