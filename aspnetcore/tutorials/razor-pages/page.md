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
ms.openlocfilehash: 599894c301db3412d7ff23c0fb58fd8799a9588f
ms.sourcegitcommit: bc723b483182fbcbf8c4c7098f70443662076905
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Vygenerované Razor stránky v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu prozkoumá stránky Razor vytvořené generování uživatelského rozhraní v předchozí kurz tématu [přidání model](xref:tutorials/razor-pages/model#scaffold-the-movie-model). 

[Zobrazení nebo stažení](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ukázka.

## <a name="the-create-delete-details-and-edit-pages"></a>Vytvořit, odstranit, podrobnosti a upravit stránky.

Zkontrolujte *Pages/Movies/Index.cshtml.cs* Model stránky:[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Stránky Razor jsou odvozeny od `PageModel`. Podle konvence `PageModel`-odvozené třídy se nazývá `<PageName>Model`. Používá konstruktoru [vkládání závislostí](xref:fundamentals/dependency-injection) přidat `MovieContext` na stránku. Všechny vygenerované stránky postupujte podle tohoto vzoru. V tématu [asynchronní kód](xref:data/ef-rp/intro#asynchronous-code) Další informace o asynchronní programing s platformou Entity Framework.

Po odeslání žádosti pro stránku, `OnGetAsync` metoda vrátí seznam hodnot filmy na stránku Razor. `OnGetAsync`nebo `OnGet` nazývá na stránce Razor k chybě při inicializaci stavu pro stránku. V takovém případě `OnGetAsync` získá seznam filmy a zobrazí je. 

Když `OnGet` vrátí `void` nebo `OnGetAsync` vrátí`Task`, žádný návratový metoda se používá. Pokud je návratový typ `IActionResult` nebo `Task<IActionResult>`, je třeba zadat příkaz return. Například *Pages/Movies/Create.cshtml.cs* `OnPostAsync` metoda:

<!-- TODO - replace with snippet
[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
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

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Syntaxe Razor můžete přejít z kódu HTML do jazyka C# nebo do značky specifické pro syntaxi Razor. Když `@` následuje symbol [Razor vyhrazené – klíčové slovo](xref:mvc/views/razor#razor-reserved-keywords)přechází do konkrétní Razor značek, jinak přejde do jazyka C#.

`@page` Direktivu Razor vytvoří soubor do akce MVC &mdash; to znamená, že může zpracovávat požadavky. `@page`musí být první direktivu Razor na stránce. `@page`je příkladem přechod do kódu specifické pro syntaxi Razor. V tématu [syntaxe Razor](xref:mvc/views/razor#razor-syntax) Další informace.

Zkontrolujte výrazu lambda použít v následujících pomocné rutiny HTML:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

`DisplayNameFor` Zkontroluje pomocné rutiny HTML `Title` odkazovaným v tomto výrazu lambda k určení zobrazovaný název vlastnosti. Výraz lambda prověřovány místo bude vyhodnocena. To znamená, že neexistuje žádná porušení přístupu při `model`, `model.Movie`, nebo `model.Movie[0]` jsou `null` nebo je prázdný. Při vyhodnocení výrazu lambda (například s `@Html.DisplayFor(modelItem => item.Title)`), se vyhodnocují hodnoty vlastností modelu.

<a name="md"></a>
### <a name="the-model-directive"></a>@model – Direktiva

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` – Direktiva Určuje typ modelu předána na stránku Razor. V předchozím příkladu `@model` řádek díky `PageModel`-odvozené třídy, které jsou k dispozici pro stránky Razor. Model se používá v `@Html.DisplayNameFor` a `@Html.DisplayName` [pomocné objekty HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stránce.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
###ViewData a rozložení

Vezměte v úvahu následující kód:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

Předchozí zvýrazněný kód je příkladem Razor přechod do jazyka C#. `{` a `}` znaky, uzavřete blok kódu jazyka C#.

`PageModel` Základní třída má `ViewData` slovníku vlastnost, která můžete použít k přidání data, která chcete předat do zobrazení. Přidání objektů do `ViewData` slovník pomocí vzoru klíč/hodnota. V předchozím příkladu je vlastnost "Title" přidat do `ViewData` slovníku. Vlastnost "Title" se používá v *Pages/_Layout.cshtml* souboru. Následující kód ukazuje několik prvních řádků *Pages/_Layout.cshtml* souboru.

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

Na řádku `@*Markup removed for brevity.*@` je komentáře syntaxe Razor. Na rozdíl od komentáře HTML (`<!-- -->`), komentáře syntaxe Razor neodešlou do klienta.

Spusťte aplikaci a otestovat odkazů v projektu (**Domů**, **o**, **kontaktujte**, **vytvořit**, **upravit**, a **odstranit**). Každé stránce nastaví název, který se zobrazí na záložce prohlížeče. Když vytvoříte záložku na stránce, název se používá pro záložky. *Pages/Index.cshtml* a *Pages/Movies/Index.cshtml* aktuálně mají stejný název, ale můžete je do mají různé hodnoty upravit.

`Layout` Je nastavena *Pages/_ViewStart.cshtml* souboru:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

Předchozí kód nastaví rozložení souboru *Pages/_Layout.cshtml* pro všechny soubory Razor pod *stránky* složky. V tématu [rozložení](xref:mvc/razor-pages/index#layout) Další informace.

### <a name="update-the-layout"></a>Aktualizace rozložení

Změna `<title>` element v *Pages/_Layout.cshtml* soubor, použijte kratší řetězec.

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

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

### <a name="the-create-code-behind-page"></a>Vytvoření kódu stránky

Zkontrolujte *Pages/Movies/Create.cshtml.cs* souboru kódu na pozadí:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

`OnGet` Metoda inicializuje jakýkoli stav potřebné pro stránku. Stránka pro vytvoření nemá žádný stav k chybě při inicializaci. `Page` Metoda vytvoří `PageResult` objekt, který vykreslí *Create.cshtml* stránky.

`Movie` Používá vlastnost `[BindProperty]` atribut zapojit [model vazby](xref:mvc/models/model-binding). Když vytvořit formulář provede hodnot formuláře, modul runtime ASP.NET Core váže odeslaných hodnoty, které mají `Movie` modelu.

`OnPostAsync` Metoda je spustit, když je stránka odeslána data formuláře:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Pokud nejsou žádné chyby modelu, formulář se zobrazí znovu, spolu s daty formuláře odeslány. Nejčastější chyby modelu můžete zachycena na straně klienta, před odesláním formuláře. Příkladem chybu modelu je publikování hodnotu pole pro datum, kterou nelze převést na datum. Budeme mluvit o další informace o ověřování na straně klienta a ověření modelu později v tomto kurzu.

Pokud nejsou žádné chyby modelu, uložení dat a prohlížeč je přesměrován na indexovou stránku.

### <a name="the-create-razor-page"></a>Vytvoření stránky Razor

Zkontrolujte *Pages/Movies/Create.cshtml* Razor stránkovacího souboru:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

Visual Studio zobrazí `<form method="post">` značku rozlišovací písmo použité pro značku pomocné rutiny. `<form method="post">` Element je [pomocná značku formuláře](xref:mvc/views/working-with-forms#the-form-tag-helper). Pomocník značku formuláře automaticky zahrne [antiforgery token](xref:security/anti-request-forgery).

![VS17 zobrazení Create.cshtml stránky](page/_static/th.png)

Modul generování uživatelského rozhraní vytvoří kódu Razor pro každé pole v modelu (s výjimkou ID) podobný následujícímu:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Ověření značky Pomocníci](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` a ` <span asp-validation-for`) zobrazit chyby ověření. Ověření je podrobněji popsány dále v této série.

[Popisek značky pomocná](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje popisek popisek a `for` atribut pro `Title` vlastnost.

[Vstupní značka pomocná](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) používá [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atributů a atributů HTML, které jsou potřebné pro architekturu jQuery ověření na straně klienta vytváří.

V dalším kurzu se dozvíte LocalDB serveru SQL a synchronizace replik indexů databáze.

>[!div class="step-by-step"]
[Předchozí: Přidání model](xref:tutorials/razor-pages/model)
[Další: SQL Server LocalDB](xref:tutorials/razor-pages/sql)
