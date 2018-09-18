---
title: Přidat nové pole do stránky v ASP.NET Core Razor
author: rick-anderson
description: Ukazuje, jak přidat nové pole do stránky Razor pomocí Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: d6d59ff336095e2f1b8b2e9a0338b7791605ad7a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010894"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a>Přidat nové pole do stránky v ASP.NET Core Razor

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V této části použijete [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migrace Code First pro přidání nového pole do modelu a migraci, které změnit na databázi.

Při použití automaticky vytvořit databázi, Code First EF Code First:

* Přidá do databáze, kterou chcete sledovat, jestli je schéma databáze synchronizované s tříd modelu, které byly vygenerovány z tabulky.
* Pokud nejsou synchronizované s databáze třídy modelu, EF vyvolá výjimku. 

Automatické ověření schématu a model synchronizované usnadňuje vyhledání potíží nekonzistentní databáze nebo kódu.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Přidání vlastnosti do hodnocení filmů modelu

Otevřít *Models/Movie.cs* a přidejte `Rating` vlastnost:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

Vytvořte aplikaci (Ctrl + Shift + B).

Upravit *Pages/Movies/Index.cshtml*a přidejte `Rating` pole:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

Přidat `Rating` pole na stránkách Delete a podrobnosti.

Aktualizace *Create.cshtml* s `Rating` pole. Můžete zkopírovat a Vložit předchozí `<div>` aktualizujte pole elementu a umožňují nápovědy technologie intelliSense. Technologie IntelliSense funguje s [pomocných rutin značek](xref:mvc/views/tag-helpers/intro).

![Vývojář napsal písmeno R pro hodnotu atributu ASP-pro druhý popisek prvku zobrazení. Jeho ikona místní nabídku technologie Intellisense zobrazí dostupná pole, včetně hodnocení, který je zvýrazněn v seznamu automaticky. Když vývojář klikne pole nebo stiskne klávesu Enter na klávesnici, hodnota se nastaví na hodnocení.](new-field/_static/cr.png)

Následující kód ukazuje *Create.cshtml* s `Rating` pole:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

Přidat `Rating` pole na stránce Upravit.

Aplikace nebude fungovat, dokud databáze je aktualizováno, aby zahrnovalo nové pole. Je-li spustit, vyvolá aplikaci `SqlException`:

```
SqlException: Invalid column name 'Rating'.
```

Tato chyba je způsobena se liší od schématu tabulky Movie databáze třídy modelu aktualizované video. (Neexistuje žádný `Rating` sloupec v tabulce databáze.)

Řešení chyby několika způsoby:

1. Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi pomocí nové schéma třídy modelu. Tento přístup je vhodný v rané fázi vývojového cyklu; umožňuje rychlý rozvoj schématu modelu a databáze společně. Nevýhodou je, dojít ke ztrátě existujících dat v databázi. Nechcete tuto metodu použijte u provozní databáze. Vyřazení databáze na změny schématu a pomocí inicializátoru automaticky naplnit databázi daty testu je často produktivní způsob, jak vyvíjet aplikace.

2. Explicitně upravte schéma stávající databázi tak, aby odpovídalo tříd modelu. Výhodou tohoto přístupu je, že zachováte vaše data. Můžete tuto změnu provést buď ručně, nebo tak, že vytvoříte databázi změnit skript.

3. Pomocí migrace Code First aktualizovat schéma databáze.

V tomto kurzu pomocí migrace Code First.

Aktualizace `SeedData` třídy tak, že poskytuje hodnoty pro nový sloupec. Ukázka změnu je uveden níže, ale budete chtít tuto změnu pro každou `new Movie` bloku.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"

Zobrazit [dokončit soubor SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Zobrazit [dokončit soubor SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).

::: moniker-end

Sestavte řešení.

<a name="pmc"></a> Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.
V konzole PMC zadejte následující příkazy:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Příkaz říká rozhraní framework:

* Porovnání `Movie` modelů s `Movie` schématu databáze.
* Vytvoření kódu pro migraci schématu databáze na nový model.

Název "Hodnocení" je volitelný a slouží k pojmenování souboru migrace. Je vhodné použít smysluplný název souboru migrace.

<a name="ssox"></a> Při odstranění všech záznamů v databázi, bude inicializátoru naplnit databáze a zahrnout `Rating` pole. To lze provést pomocí odstranit odkazy v prohlížeči nebo z [Průzkumník objektů systému Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX). Pokud chcete odstranit databázi z SSOX:

* Vyberte databázi v SSOX.
* Klikněte pravým tlačítkem na databázi a vyberte *odstranit*.
* Zkontrolujte **ukončete stávající připojení**.
* Vyberte **OK**.
* V [PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizovat databázi:

  ```powershell
  Update-Database
  ```

Spusťte aplikaci a ověřit, je možné vytvořit/upravit/zobrazit videa s `Rating` pole. Pokud databáze není nasazený, zastavte službu IIS Express a pak spusťte aplikaci.

> [!div class="step-by-step"]
> [Předchozí: Přidání vyhledávací funkce](xref:tutorials/razor-pages/search)
> [Další: Přidání ověření](xref:tutorials/razor-pages/validation)
