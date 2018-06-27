---
title: Přidejte nové pole do aplikace ASP.NET MVC jádra
author: rick-anderson
description: Další informace o použití migrace Entity Framework Code First přidávají nové pole do modelu a migrovat tato změna k databázi.
ms.author: riande
ms.date: 10/14/2016
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: eb98ebcde1086ad605127dddc055a18d4874c722
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961032"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>Přidejte nové pole do aplikace ASP.NET MVC jádra

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V této části budete používat [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migrace Code First a přidat nové pole do modelu migraci, které změnit na databázi.

Při použití EF Code First automaticky vytvořit databázi, Code First přidá tabulku k databázi chcete-li sledovat, jestli je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze. Pokud nejsou synchronizované, EF vyvolá výjimku. Díky tomu je snazší najít problémy nekonzistentní databáze nebo kódu.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Přidání vlastnosti hodnocení filmu modelu

Otevřete *Models/Movie.cs* souboru a přidejte `Rating` vlastnost:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]
::: moniker-end

Sestavení aplikace (Ctrl + Shift + B).

Protože jste přidali nové pole do `Movie` třídy, je také potřeba aktualizovat seznamu povolených vazby, tato vlastnost budou zahrnuty. V *MoviesController.cs*, aktualizovat `[Bind]` atribut pro obě `Create` a `Edit` akce metody pro zahrnutí `Rating` vlastnost:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Je také potřeba aktualizovat zobrazit šablony, aby bylo možné zobrazit, vytvořit a upravit nové `Rating` vlastnost v okně prohlížeče.

Upravit */Views/Movies/Index.cshtml* souboru a přidejte `Rating` pole:

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Aktualizace */Views/Movies/Create.cshtml* s `Rating` pole. Můžete zkopírujte a vložte předchozí "formuláře skupinu" a pomohou intelliSense aktualizovat pole. IntelliSense pracuje s [značky Pomocníci](xref:mvc/views/tag-helpers/intro). Poznámka: V verze RTM nástroje Visual Studio 2017 musíte nainstalovat [služby jazyk Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) technologie intelliSense pro Razor. Tento problém bude vyřešený v příští verzi.

![Vývojář zadal písmeno R hodnoty atributu ASP-pro v druhé elementu label zobrazení. Kontextové nabídky Intellisense ukazuje se zobrazuje dostupná pole, včetně hodnocení, které je v seznamu je zvýrazněna automaticky. Když vývojář klikne na pole nebo stiskne klávesu Enter na klávesnici, nastaví se hodnota k hodnocení.](new-field/_static/cr.png)

Aplikace nebude fungovat, dokud aktualizujeme DB zahrnout nové pole. Pokud spustíte ji nyní, získáte následující `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Tato chyba se zobrazuje, protože aktualizované třídy modelu film se liší od schématu tabulky film existující databáze. (V tabulce databáze neexistuje žádný sloupec hodnocení.)

Řešení chyby několika způsoby:

1. Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi na základě nové třídy schématu modelu. Tento přístup je velmi praktické již v rané fázi v cyklu vývoje, pokud byste active vývoj pro testovací databázi. umožňuje rychle společně momentální schéma modelu a databáze. Nevýhodou, ale je, že přijdete o stávající data v databázi – tak, že nechcete, aby pro tento postup u provozní databáze! Pomocí inicializátoru automaticky počáteční hodnoty databázi s daty test je často produktivní způsob, jak vyvíjet aplikace.

2. Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu. Výhodou tohoto přístupu je, že zachováte data. Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.

3. Použijte migrace Code First k aktualizaci schématu databáze.

V tomto kurzu použijeme migrace Code First.

Aktualizace `SeedData` třídy tak, aby poskytuje hodnotu pro nový sloupec. Ukázka změnu jsou uvedeny níže, ale budete chtít tuto změnu provést pro každý `new Movie`.

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Sestavte řešení.

Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.

  ![Pomocí PMC nabídky](adding-model/_static/pmc.png)

Pomocí PMC zadejte následující příkazy:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Příkaz zjistí rozhraní migrace prozkoumat aktuální `Movie` modelu s aktuálním `Movie` schéma databáze a vytvořit nezbytného kódu migrace databáze do nového modelu. Název "Hodnocení" libovolný a slouží k názvu souboru migrace. Je vhodné použít smysluplný název souboru migrace.

Pokud odstraníte všechny záznamy v databázi, bude inicializovat počáteční hodnoty databáze a zahrnout `Rating` pole. Můžete provést s odstranit odkazy v prohlížeči nebo z SSOX.

Spusťte aplikaci a ověřte, můžete vytvořit, upravit nebo zobrazení filmy s `Rating` pole. Měli byste také přidat `Rating` do `Edit`, `Details`, a `Delete` zobrazí šablony.

> [!div class="step-by-step"]
> [Předchozí](search.md)
> [další](validation.md)  
