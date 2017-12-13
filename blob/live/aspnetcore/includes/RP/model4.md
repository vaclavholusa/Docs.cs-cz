<span data-ttu-id="24f9b-101">Následující tabulka Podrobnosti ASP.NET Core kódu generátory parametry:</span><span class="sxs-lookup"><span data-stu-id="24f9b-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="24f9b-102">Parametr</span><span class="sxs-lookup"><span data-stu-id="24f9b-102">Parameter</span></span>               | <span data-ttu-id="24f9b-103">Popis</span><span class="sxs-lookup"><span data-stu-id="24f9b-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="24f9b-104">-m</span><span class="sxs-lookup"><span data-stu-id="24f9b-104">-m</span></span>  | <span data-ttu-id="24f9b-105">Název modelu.</span><span class="sxs-lookup"><span data-stu-id="24f9b-105">The name of the model.</span></span> |
| <span data-ttu-id="24f9b-106">-řadiče domény</span><span class="sxs-lookup"><span data-stu-id="24f9b-106">-dc</span></span>  | <span data-ttu-id="24f9b-107">Data kontextu.</span><span class="sxs-lookup"><span data-stu-id="24f9b-107">The data context.</span></span> |
| <span data-ttu-id="24f9b-108">-udl</span><span class="sxs-lookup"><span data-stu-id="24f9b-108">-udl</span></span> | <span data-ttu-id="24f9b-109">Použijte výchozí rozložení.</span><span class="sxs-lookup"><span data-stu-id="24f9b-109">Use the default layout.</span></span> |
| <span data-ttu-id="24f9b-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="24f9b-110">-outDir</span></span> | <span data-ttu-id="24f9b-111">Relativní výstupní cesta ke složce vytvoření zobrazení.</span><span class="sxs-lookup"><span data-stu-id="24f9b-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="24f9b-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="24f9b-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="24f9b-113">Přidá `_ValidationScriptsPartial` upravovat a vytvářet stránky</span><span class="sxs-lookup"><span data-stu-id="24f9b-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="24f9b-114">Použití `h` přepínač tak, aby získejte pomoc na `aspnet-codegenerator razorpage` příkaz:</span><span class="sxs-lookup"><span data-stu-id="24f9b-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="24f9b-115">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="24f9b-115">Test the app</span></span>

* <span data-ttu-id="24f9b-116">Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="24f9b-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="24f9b-117">Testovací **vytvořit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="24f9b-117">Test the **Create** link.</span></span>

 ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="24f9b-119">Testovací **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="24f9b-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="24f9b-120">Pokud se zobrazí podobná následující chyby, ověřte, jestli mají spuštění migrace a aktualizovat databázi:</span><span class="sxs-lookup"><span data-stu-id="24f9b-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
