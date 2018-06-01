<!-- This include not used by windows version -->
# <a name="adding-a-new-field"></a>Přidání nové pole

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Přidá nové pole do tohoto kurzu `Movies` tabulky. Jsme budete vyřaďte databázi a vytvořte novou, když nám změnit schéma (Přidat nové pole). Tento pracovní postup funguje dobře časná ve vývoj při nemáme žádná data produkční do opraveny.

Po nasazení vaší aplikace a máte data, která budete muset opraveny, nelze vyřadit vaší databáze, když potřebujete změnit schéma. Rozhraní Entity Framework [migrace Code First](/ef/core/get-started/aspnetcore/new-db) vám umožní aktualizovat schéma a migraci databáze bez ztráty dat. Migrace je oblíbených funkce při použití SQL serveru, ale SQLlite nepodporuje mnoho schématu operací migrace, takže jenom velmi jednoduše migrace se možná. V tématu [SQLite omezení](/ef/core/providers/sqlite/limitations) Další informace.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Přidání vlastnosti hodnocení filmu modelu

Otevřete *Models/Movie.cs* souboru a přidejte `Rating` vlastnost:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]
::: moniker-end

Protože jste přidali nové pole do `Movie` třídy, je také potřeba aktualizovat bílou vazby, tato vlastnost budou zahrnuty. V *MoviesController.cs*, aktualizovat `[Bind]` atribut pro obě `Create` a `Edit` akce metody pro zahrnutí `Rating` vlastnost:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Můžete také potřebovat k aktualizaci zobrazit šablony, aby bylo možné zobrazit, vytvářet a upravovat nové `Rating` vlastnost v okně prohlížeče.

Upravit */Views/Movies/Index.cshtml* souboru a přidejte `Rating` pole:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Aktualizace */Views/Movies/Create.cshtml* s `Rating` pole.

Aplikace nebude fungovat, dokud aktualizujeme DB zahrnout nové pole. Pokud spustíte ji nyní, získáte následující `SqliteException`:

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

Tato chyba se zobrazuje, protože aktualizované třídy modelu film se liší od schématu tabulky film existující databáze. (Není žádná `Rating` sloupec v tabulce databáze.)

Řešení chyby několika způsoby:

1. Vyřaďte databázi a mít automaticky znovu vytvořit databázi na základě nové třídy schématu modelu Entity Framework. S tímto přístupem, přijdete o stávající data v databázi –, nelze to provést pomocí provozní databáze! Pomocí inicializátoru automaticky počáteční hodnoty databázi s daty test je často produktivní způsob, jak vyvíjet aplikace.

2. Ručně upravte schéma z existující databáze tak, aby odpovídala třídy modelu. Výhodou tohoto přístupu je, že zachováte data. Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.

3. Použijte migrace Code First k aktualizaci schématu databáze.

V tomto kurzu jsme budete vyřadit a znovu vytvořit databázi, když se změní schéma. Spusťte následující příkaz z terminálu odpojení databáze:

`dotnet ef database drop`

Aktualizace `SeedData` třídy tak, aby poskytuje hodnotu pro nový sloupec. Ukázka změnu jsou uvedeny níže, ale budete chtít tuto změnu provést pro každý `new Movie`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Přidat `Rating` pole na `Edit`, `Details`, a `Delete` zobrazení.

Spusťte aplikaci a ověřte, můžete vytvořit, upravit nebo zobrazení filmy s `Rating` pole. šablony.
