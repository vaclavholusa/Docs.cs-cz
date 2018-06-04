# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="519b1-101">Vygenerované Razor stránky v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="519b1-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="519b1-102">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="519b1-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="519b1-103">V tomto kurzu prověří stránky Razor vytvořené generování uživatelského rozhraní v předchozí kurzu.</span><span class="sxs-lookup"><span data-stu-id="519b1-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="519b1-104">[Zobrazení nebo stažení](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ukázka.</span><span class="sxs-lookup"><span data-stu-id="519b1-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="519b1-105">Vytvořit, odstranit, podrobnosti a upravit stránky.</span><span class="sxs-lookup"><span data-stu-id="519b1-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="519b1-106">Zkontrolujte *Pages/Movies/Index.cshtml.cs* Model stránky:</span><span class="sxs-lookup"><span data-stu-id="519b1-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="519b1-107">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="519b1-107">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="519b1-108">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="519b1-108">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]</span></span>

::: moniker-end

<span data-ttu-id="519b1-109">Stránky Razor jsou odvozeny od `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="519b1-109">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="519b1-110">Podle konvence `PageModel`-odvozené třídy se nazývá `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="519b1-110">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="519b1-111">Používá konstruktoru [vkládání závislostí](xref:fundamentals/dependency-injection) přidat `MovieContext` na stránku.</span><span class="sxs-lookup"><span data-stu-id="519b1-111">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="519b1-112">Všechny vygenerované stránky postupujte podle tohoto vzoru.</span><span class="sxs-lookup"><span data-stu-id="519b1-112">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="519b1-113">V tématu [asynchronní kód](xref:data/ef-rp/intro#asynchronous-code) Další informace o asynchronní programing s platformou Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="519b1-113">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="519b1-114">Po odeslání žádosti pro stránku, `OnGetAsync` metoda vrátí seznam hodnot filmy na stránku Razor.</span><span class="sxs-lookup"><span data-stu-id="519b1-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="519b1-115">`OnGetAsync` nebo `OnGet` nazývá na stránce Razor k chybě při inicializaci stavu pro stránku.</span><span class="sxs-lookup"><span data-stu-id="519b1-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="519b1-116">V takovém případě `OnGetAsync` získá seznam filmy a zobrazí je.</span><span class="sxs-lookup"><span data-stu-id="519b1-116">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span> 

<span data-ttu-id="519b1-117">Když `OnGet` vrátí `void` nebo `OnGetAsync` vrátí`Task`, žádný návratový metoda se používá.</span><span class="sxs-lookup"><span data-stu-id="519b1-117">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="519b1-118">Pokud je návratový typ `IActionResult` nebo `Task<IActionResult>`, je třeba zadat příkaz return.</span><span class="sxs-lookup"><span data-stu-id="519b1-118">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="519b1-119">Například *Pages/Movies/Create.cshtml.cs* `OnPostAsync` metoda:</span><span class="sxs-lookup"><span data-stu-id="519b1-119">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

<!-- TODO - replace with snippet
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
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

<span data-ttu-id="519b1-120">Zkontrolujte *Pages/Movies/Index.cshtml* Razor stránky:</span><span class="sxs-lookup"><span data-stu-id="519b1-120">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="519b1-121">Syntaxe Razor můžete přejít z kódu HTML do jazyka C# nebo do značky specifické pro syntaxi Razor.</span><span class="sxs-lookup"><span data-stu-id="519b1-121">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="519b1-122">Když `@` následuje symbol [Razor vyhrazené – klíčové slovo](xref:mvc/views/razor#razor-reserved-keywords)přechází do konkrétní Razor značek, jinak přejde do jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="519b1-122">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="519b1-123">`@page` Direktivu Razor vytvoří soubor do akce MVC &mdash; to znamená, že může zpracovávat požadavky.</span><span class="sxs-lookup"><span data-stu-id="519b1-123">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="519b1-124">`@page` musí být první direktivu Razor na stránce.</span><span class="sxs-lookup"><span data-stu-id="519b1-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="519b1-125">`@page` je příkladem přechod do kódu specifické pro syntaxi Razor.</span><span class="sxs-lookup"><span data-stu-id="519b1-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="519b1-126">V tématu [syntaxe Razor](xref:mvc/views/razor#razor-syntax) Další informace.</span><span class="sxs-lookup"><span data-stu-id="519b1-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="519b1-127">Zkontrolujte výrazu lambda použít v následujících pomocné rutiny HTML:</span><span class="sxs-lookup"><span data-stu-id="519b1-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="519b1-128">`DisplayNameFor` Zkontroluje pomocné rutiny HTML `Title` odkazovaným v tomto výrazu lambda k určení zobrazovaný název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="519b1-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="519b1-129">Výraz lambda prověřovány místo bude vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="519b1-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="519b1-130">To znamená, že neexistuje žádná porušení přístupu při `model`, `model.Movie`, nebo `model.Movie[0]` jsou `null` nebo je prázdný.</span><span class="sxs-lookup"><span data-stu-id="519b1-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="519b1-131">Při vyhodnocení výrazu lambda (například s `@Html.DisplayFor(modelItem => item.Title)`), se vyhodnocují hodnoty vlastností modelu.</span><span class="sxs-lookup"><span data-stu-id="519b1-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="519b1-132">@model – Direktiva</span><span class="sxs-lookup"><span data-stu-id="519b1-132">The @model directive</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="519b1-133">`@model` – Direktiva Určuje typ modelu předána na stránku Razor.</span><span class="sxs-lookup"><span data-stu-id="519b1-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="519b1-134">V předchozím příkladu `@model` řádek díky `PageModel`-odvozené třídy, které jsou k dispozici pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="519b1-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="519b1-135">Model se používá v `@Html.DisplayNameFor` a `@Html.DisplayName` [pomocné objekty HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stránce.</span><span class="sxs-lookup"><span data-stu-id="519b1-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="519b1-136"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData a rozložení</span><span class="sxs-lookup"><span data-stu-id="519b1-136"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="519b1-137">Vezměte v úvahu následující kód:</span><span class="sxs-lookup"><span data-stu-id="519b1-137">Consider the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="519b1-138">Předchozí zvýrazněný kód je příkladem Razor přechod do jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="519b1-138">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="519b1-139">`{` a `}` znaky, uzavřete blok kódu jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="519b1-139">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="519b1-140">`PageModel` Základní třída má `ViewData` slovníku vlastnost, která můžete použít k přidání data, která chcete předat do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="519b1-140">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="519b1-141">Přidání objektů do `ViewData` slovník pomocí vzoru klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="519b1-141">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="519b1-142">V předchozím příkladu je vlastnost "Title" přidat do `ViewData` slovníku.</span><span class="sxs-lookup"><span data-stu-id="519b1-142">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="519b1-143">Vlastnost "Title" se používá v *Pages/_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="519b1-143">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="519b1-144">Následující kód ukazuje několik prvních řádků *Pages/_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="519b1-144">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="519b1-145">Vlastnost "Title" se používá v *Pages/Shared/_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="519b1-145">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="519b1-146">Následující kód ukazuje několik prvních řádků *_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="519b1-146">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="519b1-147">Na řádku `@*Markup removed for brevity.*@` je komentáře syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="519b1-147">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="519b1-148">Na rozdíl od komentáře HTML (`<!-- -->`), komentáře syntaxe Razor neodešlou do klienta.</span><span class="sxs-lookup"><span data-stu-id="519b1-148">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="519b1-149">Spusťte aplikaci a otestovat odkazů v projektu (**Domů**, **o**, **kontaktujte**, **vytvořit**, **upravit**, a **odstranit**).</span><span class="sxs-lookup"><span data-stu-id="519b1-149">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="519b1-150">Každé stránce nastaví název, který se zobrazí na záložce prohlížeče. Když vytvoříte záložku na stránce, název se používá pro záložky.</span><span class="sxs-lookup"><span data-stu-id="519b1-150">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="519b1-151">*Pages/Index.cshtml* a *Pages/Movies/Index.cshtml* aktuálně mají stejný název, ale můžete je do mají různé hodnoty upravit.</span><span class="sxs-lookup"><span data-stu-id="519b1-151">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="519b1-152">Nemusí být možné je zadat desetinné čárky ve `Price` pole.</span><span class="sxs-lookup"><span data-stu-id="519b1-152">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="519b1-153">Pro podporu [k ověřování jQuery](https://jqueryvalidation.org/) pro neanglická národní prostředí, které používají čárkou (",") pro desetinné čárky a formát data neanglických USA, musíte provést kroky globalizace aplikace.</span><span class="sxs-lookup"><span data-stu-id="519b1-153">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="519b1-154">To [potíže Githubu 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) postup pro přidání desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="519b1-154">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="519b1-155">`Layout` Je nastavena *Pages/_ViewStart.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="519b1-155">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="519b1-156">Předchozí kód nastaví rozložení souboru *Pages/_Layout.cshtml* pro všechny soubory Razor pod *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="519b1-156">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="519b1-157">V tématu [rozložení](xref:mvc/razor-pages/index#layout) Další informace.</span><span class="sxs-lookup"><span data-stu-id="519b1-157">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="519b1-158">Aktualizace rozložení</span><span class="sxs-lookup"><span data-stu-id="519b1-158">Update the layout</span></span>

<span data-ttu-id="519b1-159">Změna `<title>` element v *Pages/_Layout.cshtml* soubor, použijte kratší řetězec.</span><span class="sxs-lookup"><span data-stu-id="519b1-159">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="519b1-160">Najít následující element anchor v *Pages/_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="519b1-160">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="519b1-161">Nahraďte element předchozí následující kód.</span><span class="sxs-lookup"><span data-stu-id="519b1-161">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="519b1-162">Předchozí element anchor je [značky pomocná](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="519b1-162">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="519b1-163">V takovém případě má [pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="519b1-163">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="519b1-164">`asp-page="/Movies/Index"` Značky pomocný atribut a hodnotu vytvoří odkaz `/Movies/Index` stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="519b1-164">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="519b1-165">Uložte změny a aplikaci otestovat a kliknutím na **RpMovie** odkaz.</span><span class="sxs-lookup"><span data-stu-id="519b1-165">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="519b1-166">Najdete v článku [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) souboru na Githubu.</span><span class="sxs-lookup"><span data-stu-id="519b1-166">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-modelthe-create-page-model"></a><span data-ttu-id="519b1-167">Vytvoření stránky modelThe vytvořit stránku model</span><span class="sxs-lookup"><span data-stu-id="519b1-167">The Create page modelThe Create page model</span></span>

<span data-ttu-id="519b1-168">Zkontrolujte *Pages/Movies/Create.cshtml.cs* model stránky:</span><span class="sxs-lookup"><span data-stu-id="519b1-168">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="519b1-169">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]</span><span class="sxs-lookup"><span data-stu-id="519b1-169">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="519b1-170">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]</span><span class="sxs-lookup"><span data-stu-id="519b1-170">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]</span></span>
::: moniker-end


<span data-ttu-id="519b1-171">`OnGet` Metoda inicializuje jakýkoli stav potřebné pro stránku.</span><span class="sxs-lookup"><span data-stu-id="519b1-171">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="519b1-172">Stránka pro vytvoření nemá žádný stav k chybě při inicializaci, tak `Page` je vrácen.</span><span class="sxs-lookup"><span data-stu-id="519b1-172">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="519b1-173">Později v tomto kurzu se zobrazí `OnGet` metoda inicializovat stavu.</span><span class="sxs-lookup"><span data-stu-id="519b1-173">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="519b1-174">`Page` Metoda vytvoří `PageResult` objekt, který vykreslí *Create.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="519b1-174">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="519b1-175">`Movie` Používá vlastnost `[BindProperty]` atribut zapojit [model vazby](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="519b1-175">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="519b1-176">Když vytvořit formulář provede hodnot formuláře, modul runtime ASP.NET Core váže odeslaných hodnoty, které mají `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="519b1-176">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="519b1-177">`OnPostAsync` Metoda je spustit, když je stránka odeslána data formuláře:</span><span class="sxs-lookup"><span data-stu-id="519b1-177">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="519b1-178">Pokud nejsou žádné chyby modelu, formulář se zobrazí znovu, spolu s daty formuláře odeslány.</span><span class="sxs-lookup"><span data-stu-id="519b1-178">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="519b1-179">Nejčastější chyby modelu můžete zachycena na straně klienta, před odesláním formuláře.</span><span class="sxs-lookup"><span data-stu-id="519b1-179">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="519b1-180">Příkladem chybu modelu je publikování hodnotu pole pro datum, kterou nelze převést na datum.</span><span class="sxs-lookup"><span data-stu-id="519b1-180">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="519b1-181">Budeme mluvit o další informace o ověřování na straně klienta a ověření modelu později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="519b1-181">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="519b1-182">Pokud nejsou žádné chyby modelu, uložení dat a prohlížeč je přesměrován na indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="519b1-182">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="519b1-183">Vytvoření stránky Razor</span><span class="sxs-lookup"><span data-stu-id="519b1-183">The Create Razor Page</span></span>

<span data-ttu-id="519b1-184">Zkontrolujte *Pages/Movies/Create.cshtml* Razor stránkovacího souboru:</span><span class="sxs-lookup"><span data-stu-id="519b1-184">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
