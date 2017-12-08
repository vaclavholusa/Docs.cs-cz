<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="a6488-101">Přidat připojovací řetězec databáze</span><span class="sxs-lookup"><span data-stu-id="a6488-101">Add a database connection string</span></span>

<span data-ttu-id="a6488-102">Přidat připojovací řetězec k *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="a6488-102">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="a6488-103">Zaregistrovat kontext databáze</span><span class="sxs-lookup"><span data-stu-id="a6488-103">Register the database context</span></span>

<span data-ttu-id="a6488-104">Zaregistrovat kontext databáze s [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="a6488-104">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>