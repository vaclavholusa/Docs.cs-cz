---
title: "Vygenerované Razor stránky v ASP.NET Core"
author: rick-anderson
description: "Vysvětluje stránky Razor generované generování uživatelského rozhraní."
keywords: "ASP.NET Core, stránky Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 09/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: 7ae83b9bdadf5ebf8846b0c09c585da406708d12
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="a49b1-104">Vygenerované Razor stránky v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a49b1-104">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="a49b1-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a49b1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a49b1-106">V tomto kurzu prozkoumá stránky Razor vytvořené generování uživatelského rozhraní v předchozí kurz tématu [přidání model](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span><span class="sxs-lookup"><span data-stu-id="a49b1-106">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial topic [Adding a model](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span></span> 

<span data-ttu-id="a49b1-107">[Zobrazení nebo stažení](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ukázka.</span><span class="sxs-lookup"><span data-stu-id="a49b1-107">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="a49b1-108">Vytvořit, odstranit, podrobnosti a upravit stránky.</span><span class="sxs-lookup"><span data-stu-id="a49b1-108">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="a49b1-109">Zkontrolujte *Pages/Movies/Index.cshtml.cs* souboru kódu na pozadí:[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="a49b1-109">Examine the *Pages/Movies/Index.cshtml.cs* code-behind file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>

<span data-ttu-id="a49b1-110">Stránky Razor jsou odvozeny od `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="a49b1-110">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="a49b1-111">Podle konvence `PageModel`-odvozené třídy se nazývá `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="a49b1-111">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="a49b1-112">Používá konstruktoru [vkládání závislostí](xref:fundamentals/dependency-injection) přidat `MovieContext` na stránku.</span><span class="sxs-lookup"><span data-stu-id="a49b1-112">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="a49b1-113">Všechny vygenerované stránky postupujte podle tohoto vzoru.</span><span class="sxs-lookup"><span data-stu-id="a49b1-113">All the scaffolded pages follow this pattern.</span></span>

<span data-ttu-id="a49b1-114">Po odeslání žádosti pro stránku, `OnGetAsync` metoda vrátí seznam hodnot filmy na stránku Razor.</span><span class="sxs-lookup"><span data-stu-id="a49b1-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="a49b1-115">`OnGetAsync`nebo `OnGet` nazývá na stránce Razor k chybě při inicializaci stavu pro stránku.</span><span class="sxs-lookup"><span data-stu-id="a49b1-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="a49b1-116">V takovém případě `OnGetAsync` získá seznam filmy k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a49b1-116">In this case, `OnGetAsync` gets a list of movies to display.</span></span>

<span data-ttu-id="a49b1-117">Zkontrolujte *Pages/Movies/Index.cshtml* Razor stránky:</span><span class="sxs-lookup"><span data-stu-id="a49b1-117">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="a49b1-118">Syntaxe Razor můžete přejít z kódu HTML do jazyka C# nebo do značky specifické pro syntaxi Razor.</span><span class="sxs-lookup"><span data-stu-id="a49b1-118">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="a49b1-119">Když `@` následuje symbol [Razor vyhrazené – klíčové slovo](xref:mvc/views/razor#razor-reserved-keywords)přechází do konkrétní Razor značek, jinak přejde do jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="a49b1-119">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="a49b1-120">`@page` Direktivu Razor vytvoří soubor do akce MVC &mdash; to znamená, že může zpracovávat požadavky.</span><span class="sxs-lookup"><span data-stu-id="a49b1-120">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="a49b1-121">`@page`musí být první direktivu Razor na stránce.</span><span class="sxs-lookup"><span data-stu-id="a49b1-121">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="a49b1-122">`@page`je příkladem přechod do kódu specifické pro syntaxi Razor.</span><span class="sxs-lookup"><span data-stu-id="a49b1-122">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="a49b1-123">V tématu [syntaxe Razor](xref:mvc/views/razor#razor-syntax) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a49b1-123">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="a49b1-124">Zkontrolujte výrazu lambda použít v následujících pomocné rutiny HTML:</span><span class="sxs-lookup"><span data-stu-id="a49b1-124">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="a49b1-125">`DisplayNameFor` Zkontroluje pomocné rutiny HTML `Title` odkazovaným v tomto výrazu lambda k určení zobrazovaný název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a49b1-125">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="a49b1-126">Výraz lambda prověřovány místo bude vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="a49b1-126">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="a49b1-127">To znamená, že neexistuje žádná porušení přístupu při `model`, `model.Movie`, nebo `model.Movie[0]` jsou `null` nebo je prázdný.</span><span class="sxs-lookup"><span data-stu-id="a49b1-127">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="a49b1-128">Při vyhodnocení výrazu lambda (například s `@Html.DisplayFor(modelItem => item.Title)`), se vyhodnocují hodnoty vlastností modelu.</span><span class="sxs-lookup"><span data-stu-id="a49b1-128">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="a49b1-129">@model – Direktiva</span><span class="sxs-lookup"><span data-stu-id="a49b1-129">The @model directive</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="a49b1-130">`@model` – Direktiva Určuje typ modelu předána na stránku Razor.</span><span class="sxs-lookup"><span data-stu-id="a49b1-130">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="a49b1-131">V předchozím příkladu `@model` řádek díky `PageModel`-odvozené třídy, které jsou k dispozici pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="a49b1-131">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="a49b1-132">Model se používá v `@Html.DisplayNameFor` a `@Html.DisplayName` [pomocné objekty HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stránce.</span><span class="sxs-lookup"><span data-stu-id="a49b1-132">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="a49b1-133"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
###ViewData a rozložení</span><span class="sxs-lookup"><span data-stu-id="a49b1-133"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="a49b1-134">Vezměte v úvahu následující kód:</span><span class="sxs-lookup"><span data-stu-id="a49b1-134">Consider the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

<span data-ttu-id="a49b1-135">Předchozí zvýrazněný kód je příkladem Razor přechod do jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="a49b1-135">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="a49b1-136">`{` a `}` znaky, uzavřete blok kódu jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="a49b1-136">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="a49b1-137">`PageModel` Základní třída má `ViewData` slovníku vlastnost, která můžete použít k přidání data, která chcete předat do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a49b1-137">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="a49b1-138">Přidání objektů do `ViewData` slovník pomocí vzoru klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="a49b1-138">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="a49b1-139">V předchozím příkladu je vlastnost "Title" přidat do `ViewData` slovníku.</span><span class="sxs-lookup"><span data-stu-id="a49b1-139">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="a49b1-140">Vlastnost "Title" se používá v *Pages/_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="a49b1-140">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="a49b1-141">Následující kód ukazuje několik prvních řádků *Pages/_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="a49b1-141">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

<span data-ttu-id="a49b1-142">Na řádku `@*Markup removed for brevity.*@` je komentáře syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="a49b1-142">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="a49b1-143">Na rozdíl od komentáře HTML (`<!-- -->`), komentáře syntaxe Razor neodešlou do klienta.</span><span class="sxs-lookup"><span data-stu-id="a49b1-143">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="a49b1-144">Spusťte aplikaci a otestovat odkazů v projektu (**Domů**, **o**, **kontaktujte**, **vytvořit**, **upravit**, a **odstranit**).</span><span class="sxs-lookup"><span data-stu-id="a49b1-144">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="a49b1-145">Každé stránce nastaví název, který se zobrazí na záložce prohlížeče. Když vytvoříte záložku na stránce, název se používá pro záložky.</span><span class="sxs-lookup"><span data-stu-id="a49b1-145">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="a49b1-146">*Pages/Index.cshtml* a *Pages/Movies/Index.cshtml* aktuálně mají stejný název, ale můžete je do mají různé hodnoty upravit.</span><span class="sxs-lookup"><span data-stu-id="a49b1-146">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="a49b1-147">`Layout` Je nastavena *Pages/_ViewStart.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="a49b1-147">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="a49b1-148">Předchozí kód nastaví rozložení souboru *Pages/_Layout.cshtml* pro všechny soubory Razor pod *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="a49b1-148">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="a49b1-149">V tématu [rozložení](xref:mvc/razor-pages/index#layout) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a49b1-149">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="a49b1-150">Aktualizace rozložení</span><span class="sxs-lookup"><span data-stu-id="a49b1-150">Update the layout</span></span>

<span data-ttu-id="a49b1-151">Změna `<title>` element v *Pages/_Layout.cshtml* soubor, použijte kratší řetězec.</span><span class="sxs-lookup"><span data-stu-id="a49b1-151">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="a49b1-152">Najít následující element anchor v *Pages/_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="a49b1-152">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="a49b1-153">Nahraďte element předchozí následující kód.</span><span class="sxs-lookup"><span data-stu-id="a49b1-153">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="a49b1-154">Předchozí element anchor je [značky pomocná](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="a49b1-154">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="a49b1-155">V takovém případě má [pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="a49b1-155">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="a49b1-156">`asp-page="/Movies/Index"` Značky pomocný atribut a hodnotu vytvoří odkaz `/Movies/Index` stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="a49b1-156">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="a49b1-157">Uložte změny a aplikaci otestovat a kliknutím na **RpMovie** odkaz.</span><span class="sxs-lookup"><span data-stu-id="a49b1-157">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="a49b1-158">Najdete v článku [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) souboru na Githubu.</span><span class="sxs-lookup"><span data-stu-id="a49b1-158">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-code-behind-page"></a><span data-ttu-id="a49b1-159">Vytvoření kódu stránky</span><span class="sxs-lookup"><span data-stu-id="a49b1-159">The Create code-behind page</span></span>

<span data-ttu-id="a49b1-160">Zkontrolujte *Pages/Movies/Create.cshtml.cs* souboru kódu na pozadí:</span><span class="sxs-lookup"><span data-stu-id="a49b1-160">Examine the *Pages/Movies/Create.cshtml.cs* code-behind file:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="a49b1-161">`OnGet` Metoda inicializuje jakýkoli stav potřebné pro stránku.</span><span class="sxs-lookup"><span data-stu-id="a49b1-161">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="a49b1-162">Stránka pro vytvoření nemá žádný stav k chybě při inicializaci.</span><span class="sxs-lookup"><span data-stu-id="a49b1-162">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="a49b1-163">`Page` Metoda vytvoří `PageResult` objekt, který vykreslí *Create.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="a49b1-163">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="a49b1-164">`Movie` Používá vlastnost `[BindProperty]` atribut zapojit [model vazby](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="a49b1-164">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="a49b1-165">Když vytvořit formulář provede hodnot formuláře, modul runtime ASP.NET Core váže odeslaných hodnoty, které mají `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="a49b1-165">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="a49b1-166">`OnPostAsync` Metoda je spustit, když je stránka odeslána data formuláře:</span><span class="sxs-lookup"><span data-stu-id="a49b1-166">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="a49b1-167">Pokud nejsou žádné chyby modelu, formulář se zobrazí znovu, spolu s daty formuláře odeslány.</span><span class="sxs-lookup"><span data-stu-id="a49b1-167">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="a49b1-168">Nejčastější chyby modelu můžete zachycena na straně klienta, před odesláním formuláře.</span><span class="sxs-lookup"><span data-stu-id="a49b1-168">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="a49b1-169">Příkladem chybu modelu je publikování hodnotu pole pro datum, kterou nelze převést na datum.</span><span class="sxs-lookup"><span data-stu-id="a49b1-169">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="a49b1-170">Budeme mluvit o další informace o ověřování na straně klienta a ověření modelu později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="a49b1-170">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="a49b1-171">Pokud nejsou žádné chyby modelu, uložení dat a prohlížeč je přesměrován na indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="a49b1-171">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="a49b1-172">Vytvoření stránky Razor</span><span class="sxs-lookup"><span data-stu-id="a49b1-172">The Create Razor Page</span></span>

<span data-ttu-id="a49b1-173">Zkontrolujte *Pages/Movies/Create.cshtml* Razor stránkovacího souboru:</span><span class="sxs-lookup"><span data-stu-id="a49b1-173">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<span data-ttu-id="a49b1-174">Visual Studio zobrazí `<form method="post">` značku rozlišovací písmo použité pro značku pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="a49b1-174">Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers.</span></span> <span data-ttu-id="a49b1-175">`<form method="post">` Element je [pomocná značku formuláře](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="a49b1-175">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="a49b1-176">Pomocník značku formuláře automaticky zahrne [antiforgery token](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="a49b1-176">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

![VS17 zobrazení Create.cshtml stránky](page/_static/th.png)

<span data-ttu-id="a49b1-178">Modul generování uživatelského rozhraní vytvoří kódu Razor pro každé pole v modelu (s výjimkou ID) podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="a49b1-178">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="a49b1-179">[Ověření značky Pomocníci](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` a ` <span asp-validation-for`) zobrazit chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="a49b1-179">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="a49b1-180">Ověření je podrobněji popsány dále v této série.</span><span class="sxs-lookup"><span data-stu-id="a49b1-180">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="a49b1-181">[Popisek značky pomocná](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje popisek popisek a `for` atribut pro `Title` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a49b1-181">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="a49b1-182">[Vstupní značka pomocná](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) používá [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atributů a atributů HTML, které jsou potřebné pro architekturu jQuery ověření na straně klienta vytváří.</span><span class="sxs-lookup"><span data-stu-id="a49b1-182">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="a49b1-183">V dalším kurzu se dozvíte LocalDB serveru SQL a synchronizace replik indexů databáze.</span><span class="sxs-lookup"><span data-stu-id="a49b1-183">The next tutorial explains SQL Server LocalDB and seeding the database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a49b1-184">[Předchozí: Přidání model](xref:tutorials/razor-pages/model)
[Další: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="a49b1-184">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span></span>
