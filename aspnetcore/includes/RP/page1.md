# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Vygenerované Razor stránky v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu prověří stránky Razor vytvořené generování uživatelského rozhraní v předchozí kurzu. 

[Zobrazení nebo stažení](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ukázka.

## <a name="the-create-delete-details-and-edit-pages"></a>Vytvořit, odstranit, podrobnosti a upravit stránky.

Zkontrolujte *Pages/Movies/Index.cshtml.cs* Model stránky:[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Stránky Razor jsou odvozeny od `PageModel`. Podle konvence `PageModel`-odvozené třídy se nazývá `<PageName>Model`. Používá konstruktoru [vkládání závislostí](xref:fundamentals/dependency-injection) přidat `MovieContext` na stránku. Všechny vygenerované stránky postupujte podle tohoto vzoru. V tématu [asynchronní kód](xref:data/ef-rp/intro#asynchronous-code) Další informace o asynchronní programing s platformou Entity Framework.

Po odeslání žádosti pro stránku, `OnGetAsync` metoda vrátí seznam hodnot filmy na stránku Razor. `OnGetAsync`nebo `OnGet` nazývá na stránce Razor k chybě při inicializaci stavu pro stránku. V takovém případě `OnGetAsync` získá seznam filmy a zobrazí je. 

Když `OnGet` vrátí `void` nebo `OnGetAsync` vrátí`Task`, žádný návratový metoda se používá. Pokud je návratový typ `IActionResult` nebo `Task<IActionResult>`, je třeba zadat příkaz return. Například *Pages/Movies/Create.cshtml.cs* `OnPostAsync` metoda:

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
Zkontrolujte *Pages/Movies/Index.cshtml* Razor stránky:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Syntaxe Razor můžete přejít z kódu HTML do jazyka C# nebo do značky specifické pro syntaxi Razor. Když `@` následuje symbol [Razor vyhrazené – klíčové slovo](xref:mvc/views/razor#razor-reserved-keywords)přechází do konkrétní Razor značek, jinak přejde do jazyka C#.

`@page` Direktivu Razor vytvoří soubor do akce MVC &mdash; to znamená, že může zpracovávat požadavky. `@page`musí být první direktivu Razor na stránce. `@page`je příkladem přechod do kódu specifické pro syntaxi Razor. V tématu [syntaxe Razor](xref:mvc/views/razor#razor-syntax) Další informace.

Zkontrolujte výrazu lambda použít v následujících pomocné rutiny HTML:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

`DisplayNameFor` Zkontroluje pomocné rutiny HTML `Title` odkazovaným v tomto výrazu lambda k určení zobrazovaný název vlastnosti. Výraz lambda prověřovány místo bude vyhodnocena. To znamená, že neexistuje žádná porušení přístupu při `model`, `model.Movie`, nebo `model.Movie[0]` jsou `null` nebo je prázdný. Při vyhodnocení výrazu lambda (například s `@Html.DisplayFor(modelItem => item.Title)`), se vyhodnocují hodnoty vlastností modelu.

<a name="md"></a>
### <a name="the-model-directive"></a>@model – Direktiva

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` – Direktiva Určuje typ modelu předána na stránku Razor. V předchozím příkladu `@model` řádek díky `PageModel`-odvozené třídy, které jsou k dispozici pro stránky Razor. Model se používá v `@Html.DisplayNameFor` a `@Html.DisplayName` [pomocné objekty HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stránce.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
###ViewData a rozložení

Vezměte v úvahu následující kód:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Předchozí zvýrazněný kód je příkladem Razor přechod do jazyka C#. `{` a `}` znaky, uzavřete blok kódu jazyka C#.

`PageModel` Základní třída má `ViewData` slovníku vlastnost, která můžete použít k přidání data, která chcete předat do zobrazení. Přidání objektů do `ViewData` slovník pomocí vzoru klíč/hodnota. V předchozím příkladu je vlastnost "Title" přidat do `ViewData` slovníku. Vlastnost "Title" se používá v *Pages/_Layout.cshtml* souboru. Následující kód ukazuje několik prvních řádků *Pages/_Layout.cshtml* souboru.

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

Na řádku `@*Markup removed for brevity.*@` je komentáře syntaxe Razor. Na rozdíl od komentáře HTML (`<!-- -->`), komentáře syntaxe Razor neodešlou do klienta.

Spusťte aplikaci a otestovat odkazů v projektu (**Domů**, **o**, **kontaktujte**, **vytvořit**, **upravit**, a **odstranit**). Každé stránce nastaví název, který se zobrazí na záložce prohlížeče. Když vytvoříte záložku na stránce, název se používá pro záložky. *Pages/Index.cshtml* a *Pages/Movies/Index.cshtml* aktuálně mají stejný název, ale můžete je do mají různé hodnoty upravit.

`Layout` Je nastavena *Pages/_ViewStart.cshtml* souboru:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

Předchozí kód nastaví rozložení souboru *Pages/_Layout.cshtml* pro všechny soubory Razor pod *stránky* složky. V tématu [rozložení](xref:mvc/razor-pages/index#layout) Další informace.

### <a name="update-the-layout"></a>Aktualizace rozložení

Změna `<title>` element v *Pages/_Layout.cshtml* soubor, použijte kratší řetězec.

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

Najít následující element anchor v *Pages/_Layout.cshtml* souboru.

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
Nahraďte element předchozí následující kód.

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

Předchozí element anchor je [značky pomocná](xref:mvc/views/tag-helpers/intro). V takovém případě má [pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). `asp-page="/Movies/Index"` Značky pomocný atribut a hodnotu vytvoří odkaz `/Movies/Index` stránky Razor.

Uložte změny a aplikaci otestovat a kliknutím na **RpMovie** odkaz. Najdete v článku [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) souboru na Githubu.

### <a name="the-create-page-model"></a>Vytvořit model stránky

Zkontrolujte *Pages/Movies/Create.cshtml.cs* model stránky:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

`OnGet` Metoda inicializuje jakýkoli stav potřebné pro stránku. Stránka pro vytvoření nemá žádný stav k chybě při inicializaci. `Page` Metoda vytvoří `PageResult` objekt, který vykreslí *Create.cshtml* stránky.

`Movie` Používá vlastnost `[BindProperty]` atribut zapojit [model vazby](xref:mvc/models/model-binding). Když vytvořit formulář provede hodnot formuláře, modul runtime ASP.NET Core váže odeslaných hodnoty, které mají `Movie` modelu.

`OnPostAsync` Metoda je spustit, když je stránka odeslána data formuláře:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Pokud nejsou žádné chyby modelu, formulář se zobrazí znovu, spolu s daty formuláře odeslány. Nejčastější chyby modelu můžete zachycena na straně klienta, před odesláním formuláře. Příkladem chybu modelu je publikování hodnotu pole pro datum, kterou nelze převést na datum. Budeme mluvit o další informace o ověřování na straně klienta a ověření modelu později v tomto kurzu.

Pokud nejsou žádné chyby modelu, uložení dat a prohlížeč je přesměrován na indexovou stránku.

### <a name="the-create-razor-page"></a>Vytvoření stránky Razor

Zkontrolujte *Pages/Movies/Create.cshtml* Razor stránkovacího souboru:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
