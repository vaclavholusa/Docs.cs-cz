podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V této části přidáte třídy pro správu filmy v databázi. Použití těchto tříd s [Entity Framework Core](/ef/core) (EF jader) pro práci s databází. Základní EF je představuje rozhraní objektu relační mapování (ORM), které zjednodušuje data přístupový kód, který máte k zápisu.

Třídy modelu, který vytvoříte se označují jako třídy objektů POCO (z "prostý starý CLR objektů"), protože nemají některé závislé na základní EF. Že definují vlastnosti data, která jsou uložená v databázi.

V tomto kurzu nejprve napsat třídy modelu a EF základní vytvoří databázi. Alternativní přístup nejsou zahrnuté v tomto poli je [vygenerovat třídy modelu z existující databáze](/ef/core/get-started/aspnetcore/existing-db).

[Zobrazení nebo stažení](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ukázka.