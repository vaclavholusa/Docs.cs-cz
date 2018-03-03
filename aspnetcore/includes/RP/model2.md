Přidejte následující vlastnosti pro `Movie` třídy:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

`ID` Pole je vyžadováno pro databázi pro primární klíč.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Přidání třídy kontextu databáze

Přidejte následující `DbContext` odvozené třídy s názvem *MovieContext.cs* k *modely* složky: [!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

Předchozí kód vytvoří `DbSet` vlastnost pro sadu entit. V terminologii rozhraní Entity Framework obvykle sadu entit, odpovídá do databázové tabulky a entity odpovídá na řádek v tabulce.
