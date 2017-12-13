
## <a name="test-the-app"></a><span data-ttu-id="d0451-101">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="d0451-101">Test the app</span></span>

* <span data-ttu-id="d0451-102">Spusťte aplikaci a klepněte **Mvc film** odkaz.</span><span class="sxs-lookup"><span data-stu-id="d0451-102">Run the app and tap the **Mvc Movie** link.</span></span>
* <span data-ttu-id="d0451-103">Klepněte **vytvořit nový** propojit a vytvořit film.</span><span class="sxs-lookup"><span data-stu-id="d0451-103">Tap the **Create New** link and create a movie.</span></span>

  ![Vytvoření zobrazení v polích pro genre, ceny, datum vydání a název](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* <span data-ttu-id="d0451-105">Nelze zadat desetinných míst nebo čárkami v `Price` pole.</span><span class="sxs-lookup"><span data-stu-id="d0451-105">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="d0451-106">Pro podporu [k ověřování jQuery](https://jqueryvalidation.org/) pro neanglická národní prostředí, které používají čárkou (",") pro desetinné čárky a formát data neanglických USA, musíte provést kroky globalizace aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0451-106">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="d0451-107">V tématu https://github.com/aspnet/Docs/issues/4076 a [další prostředky](#additional-resources) Další informace.</span><span class="sxs-lookup"><span data-stu-id="d0451-107">See https://github.com/aspnet/Docs/issues/4076 and [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="d0451-108">Teď zadejte jenom celá čísla, jako je 10.</span><span class="sxs-lookup"><span data-stu-id="d0451-108">For now, just enter whole numbers like 10.</span></span>

<a name="displayformatdatelocal"></a>

* <span data-ttu-id="d0451-109">V některých národní prostředí budete muset zadat formát data.</span><span class="sxs-lookup"><span data-stu-id="d0451-109">In some locales you need to specify the date format.</span></span> <span data-ttu-id="d0451-110">Viz následující zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="d0451-110">See the highlighted code below.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

<span data-ttu-id="d0451-111">Budeme mluvit o `DataAnnotations` dál v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d0451-111">We'll talk about `DataAnnotations` later in the tutorial.</span></span>

<span data-ttu-id="d0451-112">Klepnutím **vytvořit** způsobí, že formulář odeslat na server, kde film informace se ukládají do databáze.</span><span class="sxs-lookup"><span data-stu-id="d0451-112">Tapping **Create** causes the form to be posted to the server, where the movie information is saved in a database.</span></span> <span data-ttu-id="d0451-113">Aplikace provede přesměrování */Movies* adresu URL, kde se zobrazují informace nově vytvořený film.</span><span class="sxs-lookup"><span data-stu-id="d0451-113">The app redirects to the */Movies* URL, where the newly created movie information is displayed.</span></span>

![Výpis film zobrazení zobrazující nově vytvořený filmy](../../tutorials/first-mvc-app/adding-model/_static/h.png)

<span data-ttu-id="d0451-115">Vytvořte několik další film položky.</span><span class="sxs-lookup"><span data-stu-id="d0451-115">Create a couple more movie entries.</span></span> <span data-ttu-id="d0451-116">Zkuste **upravit**, **podrobnosti**, a **odstranit** odkazy, které jsou všechny funkční.</span><span class="sxs-lookup"><span data-stu-id="d0451-116">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>
