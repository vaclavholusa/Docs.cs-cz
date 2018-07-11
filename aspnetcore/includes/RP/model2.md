<span data-ttu-id="d17c1-101">Přidejte následující vlastnosti pro `Movie` třídy:</span><span class="sxs-lookup"><span data-stu-id="d17c1-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="d17c1-102">`ID` Pole vyžaduje databázi pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="d17c1-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="d17c1-103">Přidání třídy kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="d17c1-103">Add a database context class</span></span>

<span data-ttu-id="d17c1-104">Přidejte následující *MovieContext.cs* třídu *modely* složky:</span><span class="sxs-lookup"><span data-stu-id="d17c1-104">Add the following *MovieContext.cs* class to the *Models* folder:</span></span>  

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

<span data-ttu-id="d17c1-105">Předchozí kód vytvoří `DbSet` vlastnost sady entit.</span><span class="sxs-lookup"><span data-stu-id="d17c1-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="d17c1-106">V terminologii Entity Framework obvykle sadu entit odpovídá databázové tabulky a entity odpovídající řádek v tabulce.</span><span class="sxs-lookup"><span data-stu-id="d17c1-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
