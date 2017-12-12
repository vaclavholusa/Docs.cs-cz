---
title: "Aktualizace generovaného stránek"
author: rick-anderson
description: "Aktualizace generovaného stránek s lepší zobrazení."
keywords: "ASP.NET Core, stránky Razor"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: c66bb3a9d766e02c7775906cdd547a0e12c15336
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="updating-the-generated-pages"></a>Aktualizace generovaného stránek

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Máme správné spuštění na filmová aplikace, ale prezentaci není ideální. Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být **datum vydání** (dvě slova).

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizace generovaný kód

Otevřete *Models/Movie.cs* souboru a přidejte následující kód ukazuje zvýrazněné řádky:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Klikněte pravým tlačítkem na červenou vlnovkou řádku > ** rychlé akce a refaktoring **.

  ![Kontextové nabídky ukazuje ** > rychlé akce a refaktoring **.](da1/qa.png)

Vyberte`using System.ComponentModel.DataAnnotations;`

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](da1/da.png)

  Visual studio. přidá `using System.ComponentModel.DataAnnotations;`.

Budeme se zabývat těmito tématy [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) v dalším kurzu. [Zobrazit](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) atribut určuje, co má být zobrazen pro název pole (v tomto případě "Datum vydání" místo "ReleaseDate"). [Datový typ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atribut určuje typ dat (datum), a proto není zobrazit čas informace uložené v poli.

Přejděte na stránkách nebo filmy a najeďte myší **upravit** odkaz zobrazíte cílová adresa URL.

![Okno prohlížeče s myši přes odkaz pro úpravy a odkaz Adresa Url http://localhost:1234 nebo filmy/Edit/5 jsou uvedené.](da1/edit7.png)

**Upravit**, **podrobnosti**, a **odstranit** generované odkazy [pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) v *stránkách nebo filmy nebo Index.cshtml* souboru.

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[Značka pomocné rutiny](xref:mvc/views/tag-helpers/intro) povolit kódu na straně serveru k účasti ve vytváření a vykreslení elementů HTML v souborech Razor. V předchozí kód `AnchorTagHelper` dynamicky vygeneruje HTML `href` hodnotu atributu ze stránky Razor (trasy, která je relativní), `asp-page`a id trasy (`asp-route-id`). V tématu [generování adresy URL pro stránky](xref:mvc/razor-pages/index#url-generation-for-pages) Další informace.

Použití **zobrazit zdroj** z oblíbeném prohlížeči prozkoumat vygenerovaný kód. Část generovaný kód jazyka HTML, je zobrazena níže:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Dynamicky generované odkazy předají ID film s řetězec dotazu (například `http://localhost:5000/Movies/Details?id=2` ). 

Aktualizujte úpravy, podrobnosti a odstranit stránky Razor používat šablonu trasy "{id: int}". Změňte direktivu stránky pro každou tyto stránek z `@page` k `@page "{id:int}"`. Spusťte aplikaci a zobrazte zdroj. Generovaný kód HTML přidá ID část adresy obsahující cestu adresy URL:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Požadavek na stránku s šablonou cesty "{id: int}", která nemá **není** zahrnují celé číslo, vrátí chybu HTTP 404 (není nalezena). Například `http://localhost:5000/Movies/Details` vrátí chybu 404. Chcete-li nastavit ID volitelný, připojte `?` pro dané omezení trasy:

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a>Aktualizace souběžného zpracování výjimek

Aktualizace `OnPostAsync` metoda v *Pages/Movies/Edit.cshtml.cs* souboru. Následující zvýrazněný kód ukazuje změny:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

Při první souběžných klientských odstraní film a druhý souběžných klientských odešle změny na film, předchozí kód zjišťuje pouze výjimky souběžnosti.

K testování `catch` bloku:

* Nastavit zarážky`catch (DbUpdateConcurrencyException)`
* Upravte film.
* V jiném okně prohlížeče, vyberte **odstranit** propojit pro stejné film a pak odstraňte video.
* V okně prohlížeče předchozí jakýchkoli změn videa.

Produkčním kódu by obvykle zjistit konfliktů souběžnosti Pokud dvě nebo víc klientů současně aktualizovat záznam. V tématu [zpracování konfliktů souběžnosti](xref:data/ef-rp/concurrency) Další informace.

### <a name="posting-and-binding-review"></a>Publikování a vazbu zkontrolujte

Zkontrolujte *Pages/Movies/Edit.cshtml.cs* souboru:[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]

Když se provádí požadavek HTTP GET na stránku filmy či upravit (například `http://localhost:5000/Movies/Edit/2`):

* `OnGetAsync` Metoda načte videa z databáze a vrátí `Page` metoda. 
* `Page` Metoda vykreslí *Pages/Movies/Edit.cshtml* stránky Razor. *Pages/Movies/Edit.cshtml* soubor obsahuje direktiva modelu (`@model RazorPagesMovie.Pages.Movies.EditModel`), které zpřístupňuje model film na stránce.
* Upravit formulář zobrazen hodnotami z videa.

Při odeslání stránky filmy či upravit:

* Hodnot formuláře na stránce je vázána na `Movie` vlastnost. `[BindProperty]` Atribut umožňuje [Model vazby](xref:mvc/models/model-binding).

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* Pokud nejsou chyby ve stavu modelu (například `ReleaseDate` nelze převést na datum), opakujte odeslání formuláře se odeslaná hodnotami.
* Pokud nejsou žádné chyby modelu, film je uložit.

Metody GET protokolu HTTP v stránky indexu, vytvořit a odstranit Razor podle podobný Princip. HTTP POST `OnPostAsync` metoda na stránce vytvořit Razor následuje a podobným způsobem, aby `OnPostAsync` metoda na stránce Upravit Razor.

V dalším kurzu se přidá vyhledávání.

>[!div class="step-by-step"]
[Předchozí: Práce s SQL serveru LocalDB](xref:tutorials/razor-pages/sql)
[přidání vyhledávání](xref:tutorials/razor-pages/search)
