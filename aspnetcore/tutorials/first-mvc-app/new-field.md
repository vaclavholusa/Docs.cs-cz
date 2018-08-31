---
title: Přidání nového pole do aplikace ASP.NET Core MVC
author: rick-anderson
description: Další informace o použití migrace Entity Framework Code First pro přidání nového pole do modelu a migrovat tuto změnu do databáze.
ms.author: riande
ms.date: 10/06/2017
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 74f7a98143c80504d534c5ee4fd06b3dd076a2f2
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312228"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>Přidání nového pole do aplikace ASP.NET Core MVC

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V této části použijete [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migrace Code First pro přidání nového pole do modelu a migraci, které změnit na databázi.

Při použití platforem EF Code First automaticky vytvořit databázi, Code First přidá tabulku do databáze pro sledování, zda je synchronizovaný s tříd modelu, které byly vygenerovány z schéma databáze. Pokud nejsou synchronizované, EF vyvolá výjimku. Díky tomu je snazší najít problémy s nekonzistentní databáze nebo kódu.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Přidání vlastnosti do hodnocení filmů modelu

Otevřít *Models/Movie.cs* a přidejte `Rating` vlastnost:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]
::: moniker-end

Vytvořte aplikaci (Ctrl + Shift + B).

Protože jsme přidali nové pole do `Movie` třídy, musíte také aktualizovat vazbu seznamu povolených, aby tuto novou vlastnost budou zahrnuty. V *MoviesController.cs*, aktualizovat `[Bind]` atribut pro obě `Create` a `Edit` metody akce, které chcete zahrnout `Rating` vlastnost:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Je také potřeba aktualizovat zobrazit šablony, aby bylo možné zobrazit, vytvořit a upravit novou `Rating` vlastností v okně prohlížeče.

Upravit */Views/Movies/Index.cshtml* a přidejte `Rating` pole:

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Aktualizace */Views/Movies/Create.cshtml* s `Rating` pole. Můžete kopírovat/vložit předchozí "formuláře skupinu" a aktualizujte pole pomohou technologie intelliSense. Technologie IntelliSense funguje s [pomocných rutin značek](xref:mvc/views/tag-helpers/intro). Poznámka: Ve verzi RTM sady Visual Studio 2017 je potřeba nainstalovat [jazykové služby Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) pro funkce Razor intelliSense. Tato chyba bude opravena v další vydané verzi.

![Vývojář napsal písmeno R pro hodnotu atributu ASP-pro druhý popisek prvku zobrazení. Jeho ikona místní nabídku technologie Intellisense zobrazí dostupná pole, včetně hodnocení, který je zvýrazněn v seznamu automaticky. Když vývojář klikne pole nebo stiskne klávesu Enter na klávesnici, hodnota se nastaví na hodnocení.](new-field/_static/cr.png)

Aplikace nebude fungovat, dokud aktualizujeme DB zahrnout nové pole. Pokud jste ji nyní spustit, zobrazí se následující `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Tato chyba se zobrazuje, protože aktualizované třídy modelu film se liší od schématu tabulky Movie existující databáze. (V tabulce databáze není žádný sloupec hodnocení.)

Řešení chyby několika způsoby:

1. Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi založené na nové schéma třídy modelu. Tento přístup je velmi vhodné v rané fázi vývojového cyklu při provádění testu databáze; s aktivním vývojem umožňuje rychlý rozvoj schématu modelu a databáze společně. Nevýhodou, je však dojít ke ztrátě existujících dat v databázi, takže nechcete tuto metodu použijte u provozní databáze. Použití inicializátoru automaticky naplnit databázi daty testu je často produktivní způsob, jak vyvíjet aplikace.

2. Explicitně upravte schéma stávající databázi tak, aby odpovídalo tříd modelu. Výhodou tohoto přístupu je, že zachováte vaše data. Můžete tuto změnu provést buď ručně, nebo tak, že vytvoříte databázi změnit skript.

3. Pomocí migrace Code First aktualizovat schéma databáze.

Pro účely tohoto kurzu používáme migrace Code First.

Aktualizace `SeedData` třídy tak, že poskytuje hodnoty pro nový sloupec. Ukázka změnu je uveden níže, ale budete chtít tuto změnu pro každou `new Movie`.

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Sestavte řešení.

Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.

  ![PMC nabídky](adding-model/_static/pmc.png)

V konzole PMC zadejte následující příkazy:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Příkaz říká rozhraní framework migrace ke kontrole aktuální `Movie` modelů s aktuálním `Movie` schématu databáze a vytvářet kód potřebné k migraci databáze na nový model. Název "Hodnocení" je volitelný a slouží k pojmenování souboru migrace. Je vhodné použít smysluplný název souboru migrace.

Při odstranění všech záznamů v databázi, bude inicializace naplnit databáze a zahrnout `Rating` pole. Můžete to provést s odstranit odkazy v prohlížeči nebo z SSOX.

Spusťte aplikaci a ověřit, je možné vytvořit/upravit/zobrazit videa s `Rating` pole. Měli byste také přidat `Rating` pole `Edit`, `Details`, a `Delete` zobrazení šablon.

> [!div class="step-by-step"]
> [Předchozí](search.md)
> [další](validation.md)  
