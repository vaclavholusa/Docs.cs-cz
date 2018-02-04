# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="7b65e-101">Vygenerované Razor stránky v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7b65e-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="7b65e-102">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7b65e-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7b65e-103">V tomto kurzu prověří stránky Razor vytvořené generování uživatelského rozhraní v předchozí kurzu.</span><span class="sxs-lookup"><span data-stu-id="7b65e-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="7b65e-104">[Zobrazení nebo stažení](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ukázka.</span><span class="sxs-lookup"><span data-stu-id="7b65e-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="7b65e-105">Vytvořit, odstranit, podrobnosti a upravit stránky.</span><span class="sxs-lookup"><span data-stu-id="7b65e-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="7b65e-106">Zkontrolujte *Pages/Movies/Index.cshtml.cs* Model stránky:[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="7b65e-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model: [!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>

<span data-ttu-id="7b65e-107">Stránky Razor jsou odvozeny od `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="7b65e-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="7b65e-108">Podle konvence `PageModel`-odvozené třídy se nazývá `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="7b65e-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="7b65e-109">Používá konstruktoru [vkládání závislostí](xref:fundamentals/dependency-injection) přidat `MovieContext` na stránku.</span><span class="sxs-lookup"><span data-stu-id="7b65e-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="7b65e-110">Všechny vygenerované stránky postupujte podle tohoto vzoru.</span><span class="sxs-lookup"><span data-stu-id="7b65e-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="7b65e-111">V tématu [asynchronní kód](xref:data/ef-rp/intro#asynchronous-code) Další informace o asynchronní programing s platformou Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7b65e-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="7b65e-112">Po odeslání žádosti pro stránku, `OnGetAsync` metoda vrátí seznam hodnot filmy na stránku Razor.</span><span class="sxs-lookup"><span data-stu-id="7b65e-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="7b65e-113">`OnGetAsync`nebo `OnGet` nazývá na stránce Razor k chybě při inicializaci stavu pro stránku.</span><span class="sxs-lookup"><span data-stu-id="7b65e-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="7b65e-114">V takovém případě `OnGetAsync` získá seznam filmy a zobrazí je.</span><span class="sxs-lookup"><span data-stu-id="7b65e-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span> 

<span data-ttu-id="7b65e-115">Když `OnGet` vrátí `void` nebo `OnGetAsync` vrátí`Task`, žádný návratový metoda se používá.</span><span class="sxs-lookup"><span data-stu-id="7b65e-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="7b65e-116">Pokud je návratový typ `IActionResult` nebo `Task<IActionResult>`, je třeba zadat příkaz return.</span><span class="sxs-lookup"><span data-stu-id="7b65e-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="7b65e-117">Například *Pages/Movies/Create.cshtml.cs* `OnPostAsync` metoda:</span><span class="sxs-lookup"><span data-stu-id="7b65e-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

<!-- TODO - replace with snippet
[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```
<span data-ttu-id="7b65e-118">Zkontrolujte *Pages/Movies/Index.cshtml* Razor stránky:</span><span class="sxs-lookup"><span data-stu-id="7b65e-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="7b65e-119">Syntaxe Razor můžete přejít z kódu HTML do jazyka C# nebo do značky specifické pro syntaxi Razor.</span><span class="sxs-lookup"><span data-stu-id="7b65e-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="7b65e-120">Když `@` následuje symbol [Razor vyhrazené – klíčové slovo](xref:mvc/views/razor#razor-reserved-keywords)přechází do konkrétní Razor značek, jinak přejde do jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="7b65e-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="7b65e-121">`@page` Direktivu Razor vytvoří soubor do akce MVC &mdash; to znamená, že může zpracovávat požadavky.</span><span class="sxs-lookup"><span data-stu-id="7b65e-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="7b65e-122">`@page`musí být první direktivu Razor na stránce.</span><span class="sxs-lookup"><span data-stu-id="7b65e-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="7b65e-123">`@page`je příkladem přechod do kódu specifické pro syntaxi Razor.</span><span class="sxs-lookup"><span data-stu-id="7b65e-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="7b65e-124">V tématu [syntaxe Razor](xref:mvc/views/razor#razor-syntax) Další informace.</span><span class="sxs-lookup"><span data-stu-id="7b65e-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="7b65e-125">Zkontrolujte výrazu lambda použít v následujících pomocné rutiny HTML:</span><span class="sxs-lookup"><span data-stu-id="7b65e-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="7b65e-126">`DisplayNameFor` Zkontroluje pomocné rutiny HTML `Title` odkazovaným v tomto výrazu lambda k určení zobrazovaný název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7b65e-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="7b65e-127">Výraz lambda prověřovány místo bude vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="7b65e-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="7b65e-128">To znamená, že neexistuje žádná porušení přístupu při `model`, `model.Movie`, nebo `model.Movie[0]` jsou `null` nebo je prázdný.</span><span class="sxs-lookup"><span data-stu-id="7b65e-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="7b65e-129">Při vyhodnocení výrazu lambda (například s `@Html.DisplayFor(modelItem => item.Title)`), se vyhodnocují hodnoty vlastností modelu.</span><span class="sxs-lookup"><span data-stu-id="7b65e-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="7b65e-130">@model – Direktiva</span><span class="sxs-lookup"><span data-stu-id="7b65e-130">The @model directive</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="7b65e-131">`@model` – Direktiva Určuje typ modelu předána na stránku Razor.</span><span class="sxs-lookup"><span data-stu-id="7b65e-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="7b65e-132">V předchozím příkladu `@model` řádek díky `PageModel`-odvozené třídy, které jsou k dispozici pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="7b65e-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="7b65e-133">Model se používá v `@Html.DisplayNameFor` a `@Html.DisplayName` [pomocné objekty HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stránce.</span><span class="sxs-lookup"><span data-stu-id="7b65e-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="7b65e-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
###ViewData a rozložení</span><span class="sxs-lookup"><span data-stu-id="7b65e-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="7b65e-135">Vezměte v úvahu následující kód:</span><span class="sxs-lookup"><span data-stu-id="7b65e-135">Consider the following code:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="7b65e-136">Předchozí zvýrazněný kód je příkladem Razor přechod do jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="7b65e-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="7b65e-137">`{` a `}` znaky, uzavřete blok kódu jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="7b65e-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="7b65e-138">`PageModel` Základní třída má `ViewData` slovníku vlastnost, která můžete použít k přidání data, která chcete předat do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7b65e-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="7b65e-139">Přidání objektů do `ViewData` slovník pomocí vzoru klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="7b65e-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="7b65e-140">V předchozím příkladu je vlastnost "Title" přidat do `ViewData` slovníku.</span><span class="sxs-lookup"><span data-stu-id="7b65e-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="7b65e-141">Vlastnost "Title" se používá v *Pages/_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="7b65e-141">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="7b65e-142">Následující kód ukazuje několik prvních řádků *Pages/_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="7b65e-142">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="7b65e-143">Na řádku `@*Markup removed for brevity.*@` je komentáře syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="7b65e-143">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="7b65e-144">Na rozdíl od komentáře HTML (`<!-- -->`), komentáře syntaxe Razor neodešlou do klienta.</span><span class="sxs-lookup"><span data-stu-id="7b65e-144">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="7b65e-145">Spusťte aplikaci a otestovat odkazů v projektu (**Domů**, **o**, **kontaktujte**, **vytvořit**, **upravit**, a **odstranit**).</span><span class="sxs-lookup"><span data-stu-id="7b65e-145">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="7b65e-146">Každé stránce nastaví název, který se zobrazí na záložce prohlížeče. Když vytvoříte záložku na stránce, název se používá pro záložky.</span><span class="sxs-lookup"><span data-stu-id="7b65e-146">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="7b65e-147">*Pages/Index.cshtml* a *Pages/Movies/Index.cshtml* aktuálně mají stejný název, ale můžete je do mají různé hodnoty upravit.</span><span class="sxs-lookup"><span data-stu-id="7b65e-147">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="7b65e-148">`Layout` Je nastavena *Pages/_ViewStart.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="7b65e-148">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="7b65e-149">Předchozí kód nastaví rozložení souboru *Pages/_Layout.cshtml* pro všechny soubory Razor pod *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="7b65e-149">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="7b65e-150">V tématu [rozložení](xref:mvc/razor-pages/index#layout) Další informace.</span><span class="sxs-lookup"><span data-stu-id="7b65e-150">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="7b65e-151">Aktualizace rozložení</span><span class="sxs-lookup"><span data-stu-id="7b65e-151">Update the layout</span></span>

<span data-ttu-id="7b65e-152">Změna `<title>` element v *Pages/_Layout.cshtml* soubor, použijte kratší řetězec.</span><span class="sxs-lookup"><span data-stu-id="7b65e-152">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="7b65e-153">Najít následující element anchor v *Pages/_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="7b65e-153">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="7b65e-154">Nahraďte element předchozí následující kód.</span><span class="sxs-lookup"><span data-stu-id="7b65e-154">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="7b65e-155">Předchozí element anchor je [značky pomocná](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="7b65e-155">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="7b65e-156">V takovém případě má [pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="7b65e-156">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="7b65e-157">`asp-page="/Movies/Index"` Značky pomocný atribut a hodnotu vytvoří odkaz `/Movies/Index` stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="7b65e-157">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="7b65e-158">Uložte změny a aplikaci otestovat a kliknutím na **RpMovie** odkaz.</span><span class="sxs-lookup"><span data-stu-id="7b65e-158">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="7b65e-159">Najdete v článku [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) souboru na Githubu.</span><span class="sxs-lookup"><span data-stu-id="7b65e-159">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="7b65e-160">Vytvořit model stránky</span><span class="sxs-lookup"><span data-stu-id="7b65e-160">The Create page model</span></span>

<span data-ttu-id="7b65e-161">Zkontrolujte *Pages/Movies/Create.cshtml.cs* model stránky:</span><span class="sxs-lookup"><span data-stu-id="7b65e-161">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="7b65e-162">`OnGet` Metoda inicializuje jakýkoli stav potřebné pro stránku.</span><span class="sxs-lookup"><span data-stu-id="7b65e-162">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="7b65e-163">Stránka pro vytvoření nemá žádný stav k chybě při inicializaci.</span><span class="sxs-lookup"><span data-stu-id="7b65e-163">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="7b65e-164">`Page` Metoda vytvoří `PageResult` objekt, který vykreslí *Create.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="7b65e-164">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="7b65e-165">`Movie` Používá vlastnost `[BindProperty]` atribut zapojit [model vazby](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="7b65e-165">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="7b65e-166">Když vytvořit formulář provede hodnot formuláře, modul runtime ASP.NET Core váže odeslaných hodnoty, které mají `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="7b65e-166">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="7b65e-167">`OnPostAsync` Metoda je spustit, když je stránka odeslána data formuláře:</span><span class="sxs-lookup"><span data-stu-id="7b65e-167">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="7b65e-168">Pokud nejsou žádné chyby modelu, formulář se zobrazí znovu, spolu s daty formuláře odeslány.</span><span class="sxs-lookup"><span data-stu-id="7b65e-168">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="7b65e-169">Nejčastější chyby modelu můžete zachycena na straně klienta, před odesláním formuláře.</span><span class="sxs-lookup"><span data-stu-id="7b65e-169">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="7b65e-170">Příkladem chybu modelu je publikování hodnotu pole pro datum, kterou nelze převést na datum.</span><span class="sxs-lookup"><span data-stu-id="7b65e-170">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="7b65e-171">Budeme mluvit o další informace o ověřování na straně klienta a ověření modelu později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="7b65e-171">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="7b65e-172">Pokud nejsou žádné chyby modelu, uložení dat a prohlížeč je přesměrován na indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="7b65e-172">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="7b65e-173">Vytvoření stránky Razor</span><span class="sxs-lookup"><span data-stu-id="7b65e-173">The Create Razor Page</span></span>

<span data-ttu-id="7b65e-174">Zkontrolujte *Pages/Movies/Create.cshtml* Razor stránkovacího souboru:</span><span class="sxs-lookup"><span data-stu-id="7b65e-174">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
