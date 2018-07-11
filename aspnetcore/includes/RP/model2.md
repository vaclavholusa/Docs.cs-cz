Přidejte následující vlastnosti pro `Movie` třídy:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

`ID` Pole vyžaduje databázi pro primární klíč.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Přidání třídy kontextu databáze

Přidejte následující *MovieContext.cs* třídu *modely* složky:  

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

Předchozí kód vytvoří `DbSet` vlastnost sady entit. V terminologii Entity Framework obvykle sadu entit odpovídá databázové tabulky a entity odpovídající řádek v tabulce.
