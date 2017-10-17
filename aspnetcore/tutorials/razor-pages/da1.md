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
ms.openlocfilehash: 290d752ea5f177348ff3e749cc125e946ae6e763
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2017
---
# <a name="updating-the-generated-pages"></a><span data-ttu-id="2656b-104">Aktualizace generovaného stránek</span><span class="sxs-lookup"><span data-stu-id="2656b-104">Updating the generated pages</span></span>

<span data-ttu-id="2656b-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2656b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2656b-106">Máme správné spuštění na filmová aplikace, ale prezentaci není ideální.</span><span class="sxs-lookup"><span data-stu-id="2656b-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="2656b-107">Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být **datum vydání** (dvě slova).</span><span class="sxs-lookup"><span data-stu-id="2656b-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="2656b-109">Aktualizace generovaný kód</span><span class="sxs-lookup"><span data-stu-id="2656b-109">Update the generated code</span></span>

<span data-ttu-id="2656b-110">Otevřete *Models/Movie.cs* souboru a přidejte následující kód ukazuje zvýrazněné řádky:</span><span class="sxs-lookup"><span data-stu-id="2656b-110">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="2656b-111">Klikněte pravým tlačítkem na červenou vlnovkou řádku > ** rychlé akce a refaktoring **.</span><span class="sxs-lookup"><span data-stu-id="2656b-111">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Kontextové nabídky ukazuje ** > rychlé akce a refaktoring **.](da1/qa.png)

<span data-ttu-id="2656b-113">Vyberte`using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="2656b-113">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](da1/da.png)

  <span data-ttu-id="2656b-115">Visual studio. přidá `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="2656b-115">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="2656b-116">Budeme se zabývat těmito tématy [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="2656b-116">We'll cover [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) in the next tutorial.</span></span> <span data-ttu-id="2656b-117">[Zobrazit](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) atribut určuje, co má být zobrazen pro název pole (v tomto případě "Datum vydání" místo "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="2656b-117">The [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="2656b-118">[Datový typ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atribut určuje typ dat (datum), a proto není zobrazit čas informace uložené v poli.</span><span class="sxs-lookup"><span data-stu-id="2656b-118">The [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field is not displayed.</span></span>

<span data-ttu-id="2656b-119">Přejděte na stránkách nebo filmy a najeďte myší **upravit** odkaz zobrazíte cílová adresa URL.</span><span class="sxs-lookup"><span data-stu-id="2656b-119">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Okno prohlížeče s myši přes odkaz pro úpravy a odkaz Adresa Url http://localhost:1234 nebo filmy/Edit/5 jsou uvedené.](da1/edit7.png)

<span data-ttu-id="2656b-121">**Upravit**, **podrobnosti**, a **odstranit** generované odkazy [pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) v *stránkách nebo filmy nebo Index.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="2656b-121">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="2656b-122">[Značka pomocné rutiny](xref:mvc/views/tag-helpers/intro) povolit kódu na straně serveru k účasti ve vytváření a vykreslení elementů HTML v souborech Razor.</span><span class="sxs-lookup"><span data-stu-id="2656b-122">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="2656b-123">V předchozí kód `AnchorTagHelper` dynamicky vygeneruje HTML `href` hodnotu atributu ze stránky Razor (trasy, která je relativní), `asp-page`a id trasy (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="2656b-123">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="2656b-124">V tématu [generování adresy URL pro stránky](xref:mvc/razor-pages/index#url-generation-for-pages) Další informace.</span><span class="sxs-lookup"><span data-stu-id="2656b-124">See [URL generation for Pages](xref:mvc/razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="2656b-125">Použití **zobrazit zdroj** z oblíbeném prohlížeči prozkoumat vygenerovaný kód.</span><span class="sxs-lookup"><span data-stu-id="2656b-125">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="2656b-126">Část generovaný kód jazyka HTML, je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="2656b-126">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="2656b-127">Dynamicky generované odkazy předají ID film s řetězec dotazu (například `http://localhost:5000/Movies/Details?id=2` ).</span><span class="sxs-lookup"><span data-stu-id="2656b-127">The dynamically-generated links pass the movie ID with a query string (for example, `http://localhost:5000/Movies/Details?id=2` ).</span></span> 

<span data-ttu-id="2656b-128">Aktualizujte úpravy, podrobnosti a odstranit stránky Razor používat šablonu trasy "{id: int}".</span><span class="sxs-lookup"><span data-stu-id="2656b-128">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="2656b-129">Změňte direktivu stránky pro každou z těchto stránek a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="2656b-129">Change the page directive for each of these pages to `@page "{id:int}"`.</span></span> <span data-ttu-id="2656b-130">Spusťte aplikaci a zobrazte zdroj.</span><span class="sxs-lookup"><span data-stu-id="2656b-130">Run the app and then view source.</span></span> <span data-ttu-id="2656b-131">Generovaný kód HTML přidá ID část adresy obsahující cestu adresy URL:</span><span class="sxs-lookup"><span data-stu-id="2656b-131">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="2656b-132">Požadavek na stránku s šablonou cesty "{id: int}", která nemá **není** zahrnují celé číslo, vrátí chybu HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="2656b-132">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="2656b-133">Například `http://localhost:5000/Movies/Details` vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="2656b-133">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="2656b-134">Chcete-li nastavit ID volitelný, připojte `?` pro dané omezení trasy:</span><span class="sxs-lookup"><span data-stu-id="2656b-134">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a><span data-ttu-id="2656b-135">Aktualizace souběžného zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="2656b-135">Update concurrency exception handling</span></span>

<span data-ttu-id="2656b-136">Aktualizace `OnPostAsync` metoda v *Pages/Movies/Edit.cshtml.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="2656b-136">Update the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file.</span></span> <span data-ttu-id="2656b-137">Následující zvýrazněný kód ukazuje změny:</span><span class="sxs-lookup"><span data-stu-id="2656b-137">The following highlighted code shows the changes:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

<span data-ttu-id="2656b-138">Při první souběžných klientských odstraní film a druhý souběžných klientských odešle změny na film, předchozí kód zjišťuje pouze výjimky souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="2656b-138">The previous code only detects concurrency exceptions when the first concurrent client deletes the movie, and the second concurrent client posts changes to the movie.</span></span>

<span data-ttu-id="2656b-139">K testování `catch` bloku:</span><span class="sxs-lookup"><span data-stu-id="2656b-139">To test the `catch` block:</span></span>

* <span data-ttu-id="2656b-140">Nastavit zarážky`catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="2656b-140">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="2656b-141">Upravte film.</span><span class="sxs-lookup"><span data-stu-id="2656b-141">Edit a movie.</span></span>
* <span data-ttu-id="2656b-142">V jiném okně prohlížeče, vyberte **odstranit** propojit pro stejné film a pak odstraňte video.</span><span class="sxs-lookup"><span data-stu-id="2656b-142">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="2656b-143">V okně prohlížeče předchozí jakýchkoli změn videa.</span><span class="sxs-lookup"><span data-stu-id="2656b-143">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="2656b-144">Produkčním kódu by obvykle zjistit konfliktů souběžnosti Pokud dvě nebo víc klientů současně aktualizovat záznam.</span><span class="sxs-lookup"><span data-stu-id="2656b-144">Production code would generally detect concurrency conflicts when two or more clients concurrently updated a record.</span></span> <span data-ttu-id="2656b-145">V tématu [zpracování konfliktů souběžnosti](xref:data/ef-mvc/concurrency) Další informace.</span><span class="sxs-lookup"><span data-stu-id="2656b-145">See [Handling concurrency conflicts](xref:data/ef-mvc/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="2656b-146">Publikování a vazbu zkontrolujte</span><span class="sxs-lookup"><span data-stu-id="2656b-146">Posting and binding review</span></span>

<span data-ttu-id="2656b-147">Zkontrolujte *Pages/Movies/Edit.cshtml.cs* souboru:[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="2656b-147">Examine the *Pages/Movies/Edit.cshtml.cs* file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span></span>

<span data-ttu-id="2656b-148">Když se provádí požadavek HTTP GET na stránku filmy či upravit (například `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="2656b-148">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="2656b-149">`OnGetAsync` Metoda načte videa z databáze a vrátí `Page` metoda.</span><span class="sxs-lookup"><span data-stu-id="2656b-149">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="2656b-150">`Page` Metoda vykreslí *Pages/Movies/Edit.cshtml* stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="2656b-150">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="2656b-151">*Pages/Movies/Edit.cshtml* soubor obsahuje direktiva modelu (`@model RazorPagesMovie.Pages.Movies.EditModel`), takže k dispozici na stránce film modelu.</span><span class="sxs-lookup"><span data-stu-id="2656b-151">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the the movie model available on the page.</span></span>
* <span data-ttu-id="2656b-152">Upravit formulář zobrazen hodnotami z videa.</span><span class="sxs-lookup"><span data-stu-id="2656b-152">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="2656b-153">Při odeslání stránky filmy či upravit:</span><span class="sxs-lookup"><span data-stu-id="2656b-153">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="2656b-154">Hodnot formuláře na stránce je vázána na `Movie` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2656b-154">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="2656b-155">`[BindProperty]` Atribut umožňuje [Model vazby](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="2656b-155">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="2656b-156">Pokud nejsou chyby ve stavu modelu (například `ReleaseDate` nelze převést na datum), opakujte odeslání formuláře se odeslaná hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2656b-156">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is posted again with the submitted values.</span></span>
* <span data-ttu-id="2656b-157">Pokud nejsou žádné chyby modelu, film je uložit.</span><span class="sxs-lookup"><span data-stu-id="2656b-157">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="2656b-158">Metody GET protokolu HTTP v stránky indexu, vytvořit a odstranit Razor podle podobný Princip.</span><span class="sxs-lookup"><span data-stu-id="2656b-158">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="2656b-159">HTTP POST `OnPostAsync` metoda na stránce vytvořit Razor následuje a podobným způsobem, aby `OnPostAsync` metoda na stránce Upravit Razor.</span><span class="sxs-lookup"><span data-stu-id="2656b-159">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="2656b-160">V dalším kurzu se přidá vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="2656b-160">Search is added in the next tutorial.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2656b-161">[Předchozí: Práce s SQL serveru LocalDB](xref:tutorials/razor-pages/sql)
[přidání vyhledávání](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="2656b-161">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
