<a name="cli"></a>

## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="a0f21-101">Přidat vygenerované uživatelské rozhraní nástroje a provádět počáteční migraci</span><span class="sxs-lookup"><span data-stu-id="a0f21-101">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="a0f21-102">Přidejte následující řádky do *RazorPagesMovie.csproj* souboru, těsně před uzavírací `</Project>` značky:</span><span class="sxs-lookup"><span data-stu-id="a0f21-102">Add the following lines to the *RazorPagesMovie.csproj* file, just before the closing `</Project>` tag:</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.1.0-preview1-final"/>
</ItemGroup>
```
  
<span data-ttu-id="a0f21-103">Z příkazového řádku spusťte následující příkazy rozhraní příkazového řádku .NET Core:</span><span class="sxs-lookup"><span data-stu-id="a0f21-103">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="a0f21-104">`DotNetCliToolReference` Elementu a `add package` příkaz nainstalovat nástroje, které jsou potřeba ke spouštění modulu generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a0f21-104">The `DotNetCliToolReference` element and the `add package` command install the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="a0f21-105">`ef migrations add InitialCreate` Příkaz vygeneruje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="a0f21-105">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="a0f21-106">Schéma je založen na zadaném v modelu `DbContext` (v *Models/MovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="a0f21-106">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="a0f21-107">`InitialCreate` Argument se používá k pojmenování migrace.</span><span class="sxs-lookup"><span data-stu-id="a0f21-107">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="a0f21-108">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="a0f21-108">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="a0f21-109">Zobrazit [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a0f21-109">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="a0f21-110">`ef database update` Příkaz spustí `Up` metodu *migrace /\<časové razítko > _InitialCreate.cs* soubor, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="a0f21-110">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
