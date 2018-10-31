<span data-ttu-id="ce57c-101">Následující tabulka obsahuje podrobnosti o parametry generátor kódu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="ce57c-101">The following table details the ASP.NET Core code generator parameters:</span></span>

| <span data-ttu-id="ce57c-102">Parametr</span><span class="sxs-lookup"><span data-stu-id="ce57c-102">Parameter</span></span>               | <span data-ttu-id="ce57c-103">Popis</span><span class="sxs-lookup"><span data-stu-id="ce57c-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="ce57c-104">-m</span><span class="sxs-lookup"><span data-stu-id="ce57c-104">-m</span></span>  | <span data-ttu-id="ce57c-105">Název modelu.</span><span class="sxs-lookup"><span data-stu-id="ce57c-105">The name of the model.</span></span> |
| <span data-ttu-id="ce57c-106">-dc</span><span class="sxs-lookup"><span data-stu-id="ce57c-106">-dc</span></span>  | <span data-ttu-id="ce57c-107">Data kontextu.</span><span class="sxs-lookup"><span data-stu-id="ce57c-107">The data context.</span></span> |
| <span data-ttu-id="ce57c-108">-udl</span><span class="sxs-lookup"><span data-stu-id="ce57c-108">-udl</span></span> | <span data-ttu-id="ce57c-109">Použijte výchozí rozložení.</span><span class="sxs-lookup"><span data-stu-id="ce57c-109">Use the default layout.</span></span> |
| <span data-ttu-id="ce57c-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="ce57c-110">-outDir</span></span> | <span data-ttu-id="ce57c-111">Relativní cesta k výstupní složce vytvořte zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ce57c-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="ce57c-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="ce57c-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="ce57c-113">Přidá `_ValidationScriptsPartial` upravovat a vytvářet stránky</span><span class="sxs-lookup"><span data-stu-id="ce57c-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="ce57c-114">Použití `h` přepínače můžete zobrazit nápovědu pro `aspnet-codegenerator razorpage` příkaz:</span><span class="sxs-lookup"><span data-stu-id="ce57c-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="ce57c-115">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="ce57c-115">Test the app</span></span>

* <span data-ttu-id="ce57c-116">Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/Movies`).</span><span class="sxs-lookup"><span data-stu-id="ce57c-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/Movies`).</span></span>
* <span data-ttu-id="ce57c-117">Test **vytvořit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="ce57c-117">Test the **Create** link.</span></span>

  ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="ce57c-119">Test **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="ce57c-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="ce57c-120">Pokud se zobrazí chyba podobná následující, ověřte máte spusťte migrace a aktualizovat databázi:</span><span class="sxs-lookup"><span data-stu-id="ce57c-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

`An unhandled exception occurred while processing the request. 'no such table: Movie'.`
