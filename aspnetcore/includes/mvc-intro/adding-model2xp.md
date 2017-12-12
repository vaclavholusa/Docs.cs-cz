<a name="cli"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="4f0ca-101">Provedení počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="4f0ca-101">Perform initial migration</span></span>

<span data-ttu-id="4f0ca-102">Z příkazového řádku spusťte následující příkazy .NET Core rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="4f0ca-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="4f0ca-103">`dotnet ef migrations add InitialCreate` Příkaz generuje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="4f0ca-103">The `dotnet ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="4f0ca-104">Schéma je založena na zadaný ve model `DbContext` (v *Models/MovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="4f0ca-104">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="4f0ca-105">`Initial` Argument se používá k pojmenování byla migrace.</span><span class="sxs-lookup"><span data-stu-id="4f0ca-105">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="4f0ca-106">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="4f0ca-106">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="4f0ca-107">V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4f0ca-107">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="4f0ca-108">`dotnet ef database update` Příkaz spustí `Up` metoda v *migrace nebo\<časové razítko > _InitialCreate.cs* souboru, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="4f0ca-108">The `dotnet ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>