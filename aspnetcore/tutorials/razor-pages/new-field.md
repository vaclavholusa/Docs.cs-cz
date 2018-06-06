---
title: Přidat nové pole na stránku Razor v ASP.NET Core
author: rick-anderson
description: Ukazuje, jak přidat nové pole na stránku Razor základní Entity Framework
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 926091a7643bbe584316677cbaae6574471c47f8
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582707"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a>Přidat nové pole na stránku Razor v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V této části můžete použít [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migrace Code First a přidat nové pole do modelu migraci, které změnit na databázi.

Při použití automaticky vytvořit databázi, Code First EF Code First:

* Přidá tabulku k databázi a sleduje, zda je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze.
* Pokud nejsou synchronizované s databáze třídy modelu, EF vyvolá výjimku. 

Automatické ověření schématu nebo modelu synchronizované usnadňuje vyhledání problémy nekonzistentní databáze nebo kódu.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Přidání vlastnosti hodnocení filmu modelu

Otevřete *Models/Movie.cs* souboru a přidejte `Rating` vlastnost:
::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

Sestavení aplikace (Ctrl + Shift + B).

Upravit *Pages/Movies/Index.cshtml*a přidejte `Rating` pole:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

Přidat `Rating` pole na stránky odstranit a podrobnosti.

Aktualizace *Create.cshtml* s `Rating` pole. Vám může zkopírujte a vložte předchozí `<div>` elementu a umožňují nápovědy intelliSense aktualizovat pole. IntelliSense pracuje s [značky Pomocníci](xref:mvc/views/tag-helpers/intro).

![Vývojář zadal písmeno R hodnoty atributu ASP-pro v druhé elementu label zobrazení. Kontextové nabídky Intellisense ukazuje se zobrazuje dostupná pole, včetně hodnocení, které je v seznamu je zvýrazněna automaticky. Když vývojář klikne na pole nebo stiskne klávesu Enter na klávesnici, nastaví se hodnota k hodnocení.](new-field/_static/cr.png)

Následující kód ukazuje *Create.cshtml* s `Rating` pole:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

Přidat `Rating` pole na stránku upravit.

Aplikace nebude fungovat, dokud nebude databáze je aktualizováno, aby zahrnovalo nové pole. Je-li spustit nyní, vyvolává aplikaci `SqlException`:

```
SqlException: Invalid column name 'Rating'.
```

Tato chyba je způsobená aktualizované třídy modelu film se liší od schématu film tabulky databáze. (Není žádná `Rating` sloupec v tabulce databáze.)

Řešení chyby několika způsoby:

1. Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi pomocí nové třídy schéma modelu. Tento přístup je vhodné již v rané fázi v cyklu vývoje; umožňuje rychle společně momentální schéma modelu a databáze. Nevýhodou je, že přijdete o stávající data v databázi. Nechcete, aby pro tento postup u provozní databáze! Vyřazení databáze na změny schématu a automaticky naplnit databázi daty test pomocí inicializátoru je často produktivní způsob, jak vyvíjet aplikace.

2. Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu. Výhodou tohoto přístupu je, že zachováte data. Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.

3. Použijte migrace Code First k aktualizaci schématu databáze.

V tomto kurzu používejte migrace Code First.

Aktualizace `SeedData` třídy tak, aby poskytuje hodnotu pro nový sloupec. Ukázka změnu jsou uvedeny níže, ale budete chtít tuto změnu provést pro každý `new Movie` bloku.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"
Najdete v článku [dokončit SeedData.cs souboru](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
Najdete v článku [dokončit SeedData.cs souboru](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).
::: moniker-end

Sestavte řešení.

<a name="pmc"></a> Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.
Pomocí PMC zadejte následující příkazy:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Příkaz zjistí rozhraní pro:

* Porovnání `Movie` modelu s `Movie` schéma databáze.
* Vytvoření kódu k migraci schéma databáze do nového modelu.

Název "Hodnocení" libovolný a slouží k názvu souboru migrace. Je vhodné použít smysluplný název souboru migrace.

<a name="ssox"></a> Pokud odstraníte všechny záznamy v databázi, bude inicializátoru počáteční hodnoty databáze a zahrnout `Rating` pole. To lze provést pomocí odstranění odkazy v prohlížeči nebo z [Průzkumník objektů systému Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX). Chcete-li odstranit databázi z SSOX:

* Vyberte databázi v SSOX.
* Klikněte pravým tlačítkem na databázi a vyberte *odstranit*.
* Zkontrolujte **ukončete stávající připojení**.
* Vyberte **OK**.
* V [pomocí PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizovat databázi:

  ```powershell
  Update-Database
  ```

Spusťte aplikaci a ověřte, můžete vytvořit, upravit nebo zobrazení filmy s `Rating` pole. Pokud není nasadí databázi, zastavte službu IIS Express a pak spusťte aplikaci.

> [!div class="step-by-step"]
> [Předchozí: Přidání vyhledávací](xref:tutorials/razor-pages/search)
> [Další: Přidání ověření](xref:tutorials/razor-pages/validation)
