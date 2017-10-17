<span data-ttu-id="e6818-101">Následující tabulka Podrobnosti ASP.NET Core kódu generátory parametry:</span><span class="sxs-lookup"><span data-stu-id="e6818-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="e6818-102">Parametr</span><span class="sxs-lookup"><span data-stu-id="e6818-102">Parameter</span></span>               | <span data-ttu-id="e6818-103">Popis</span><span class="sxs-lookup"><span data-stu-id="e6818-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="e6818-104">-m</span><span class="sxs-lookup"><span data-stu-id="e6818-104">-m</span></span>  | <span data-ttu-id="e6818-105">Název modelu.</span><span class="sxs-lookup"><span data-stu-id="e6818-105">The name of the model.</span></span> |
| <span data-ttu-id="e6818-106">-řadiče domény</span><span class="sxs-lookup"><span data-stu-id="e6818-106">-dc</span></span>  | <span data-ttu-id="e6818-107">Data kontextu.</span><span class="sxs-lookup"><span data-stu-id="e6818-107">The data context.</span></span> |
| <span data-ttu-id="e6818-108">-udl</span><span class="sxs-lookup"><span data-stu-id="e6818-108">-udl</span></span> | <span data-ttu-id="e6818-109">Použijte výchozí rozložení.</span><span class="sxs-lookup"><span data-stu-id="e6818-109">Use the default layout.</span></span> |
| <span data-ttu-id="e6818-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="e6818-110">-outDir</span></span> | <span data-ttu-id="e6818-111">Relativní výstupní cesta ke složce vytvoření zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e6818-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="e6818-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="e6818-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="e6818-113">Přidá `_ValidationScriptsPartial` upravovat a vytvářet stránky</span><span class="sxs-lookup"><span data-stu-id="e6818-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="e6818-114">Použití `h` přepínač tak, aby získejte pomoc na `aspnet-codegenerator razorpage` příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6818-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="e6818-115">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="e6818-115">Test the app</span></span>

* <span data-ttu-id="e6818-116">Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="e6818-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="e6818-117">Testovací **vytvořit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="e6818-117">Test the **Create** link.</span></span>

 ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="e6818-119">Testovací **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="e6818-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="e6818-120">Pokud dojde k následující chybě, ověřte, jestli mají spuštění migrace a aktualizovat databázi:</span><span class="sxs-lookup"><span data-stu-id="e6818-120">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```