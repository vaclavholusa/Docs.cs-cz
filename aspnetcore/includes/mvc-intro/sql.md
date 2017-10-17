# <a name="working-with-sqlite-in-an-aspnet-core-mvc-project"></a><span data-ttu-id="3db22-101">Práce s SQLite v projektu ASP.NET MVC jádra</span><span class="sxs-lookup"><span data-stu-id="3db22-101">Working with SQLite in an ASP.NET Core MVC project</span></span>

<span data-ttu-id="3db22-102">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3db22-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3db22-103">`MvcMovieContext` Objekt zpracovává úlohu s připojením k databázi a mapování `Movie` objekty záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="3db22-103">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="3db22-104">Kontext databáze není zaregistrována [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru v `ConfigureServices` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="3db22-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="3db22-105">[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]</span><span class="sxs-lookup"><span data-stu-id="3db22-105">[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]</span></span>

## <a name="sqlite"></a><span data-ttu-id="3db22-106">SQLite</span><span class="sxs-lookup"><span data-stu-id="3db22-106">SQLite</span></span>

<span data-ttu-id="3db22-107">[SQLite](https://www.sqlite.org/) webu stavy:</span><span class="sxs-lookup"><span data-stu-id="3db22-107">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="3db22-108">SQLite je samostatná, vysoká spolehlivost, embedded, plně funkční, veřejné domény, databázový stroj SQL.</span><span class="sxs-lookup"><span data-stu-id="3db22-108">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="3db22-109">SQLite je nejpoužívanější databázový stroj na světě.</span><span class="sxs-lookup"><span data-stu-id="3db22-109">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="3db22-110">Existuje mnoho nástroje třetích stran, si můžete stáhnout spravovat a zobrazovat databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="3db22-110">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="3db22-111">Následující obrázek je z [DB prohlížeče pro SQLite](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="3db22-111">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="3db22-112">Pokud máte nástroj Oblíbené SQLite, co se vám líbí o něm ponechte komentář.</span><span class="sxs-lookup"><span data-stu-id="3db22-112">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![DB prohlížeče pro SQLite zobrazující film db](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="3db22-114">Počáteční hodnoty databáze</span><span class="sxs-lookup"><span data-stu-id="3db22-114">Seed the database</span></span>

<span data-ttu-id="3db22-115">Vytvořte novou třídu s názvem `SeedData` v *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="3db22-115">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="3db22-116">Generovaného kódu nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="3db22-116">Replace the generated code with the following:</span></span>

<span data-ttu-id="3db22-117">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="3db22-117">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]</span></span>

<span data-ttu-id="3db22-118">Pokud jsou všechny filmy v databázi, vrátí inicializátoru počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3db22-118">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="3db22-119">Přidat inicializátoru počáteční hodnoty</span><span class="sxs-lookup"><span data-stu-id="3db22-119">Add the seed initializer</span></span>

<span data-ttu-id="3db22-120">Přidat inicializátoru počáteční hodnoty do `Main` metoda v *Program.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="3db22-120">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

<span data-ttu-id="3db22-121">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]</span><span class="sxs-lookup"><span data-stu-id="3db22-121">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]</span></span>

### <a name="test-the-app"></a><span data-ttu-id="3db22-122">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="3db22-122">Test the app</span></span>

<span data-ttu-id="3db22-123">Odstraňte všechny záznamy v databázi (tak, aby metoda počáteční hodnoty spuštěno).</span><span class="sxs-lookup"><span data-stu-id="3db22-123">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="3db22-124">Zastavení a spuštění aplikace počáteční hodnoty databáze.</span><span class="sxs-lookup"><span data-stu-id="3db22-124">Stop and start the app to seed the database.</span></span>
   
<span data-ttu-id="3db22-125">Aplikace zobrazuje dosazená data.</span><span class="sxs-lookup"><span data-stu-id="3db22-125">The app shows the seeded data.</span></span>

![Film MVC aplikace otevřete prohlížeč zobrazující film dat](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)
