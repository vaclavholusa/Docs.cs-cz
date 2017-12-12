---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: "Představení technologie ASP.NET Web Pages – odstranění dat z databáze | Microsoft Docs"
author: tfitzmac
description: "V tomto kurzu se dozvíte, jak odstranit položku jednotlivé databáze. Předpokládá se, že jste dokončili řady prostřednictvím aktualizace dat databáze v adrese poskytovatele. technologie ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: aef31b6170cc3bba2421eb8c2c41e83aadc129c5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="20e35-104">Představení technologie ASP.NET Web Pages – odstranění dat z databáze</span><span class="sxs-lookup"><span data-stu-id="20e35-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="20e35-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="20e35-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="20e35-106">V tomto kurzu se dozvíte, jak odstranit položku jednotlivé databáze.</span><span class="sxs-lookup"><span data-stu-id="20e35-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="20e35-107">Předpokládá, že jste dokončili řady prostřednictvím [aktualizace dat databáze na webových stránkách ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251583).</span><span class="sxs-lookup"><span data-stu-id="20e35-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251583).</span></span>
> 
> <span data-ttu-id="20e35-108">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="20e35-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="20e35-109">Jak vybrat jednotlivé záznamy ze seznamu záznamů.</span><span class="sxs-lookup"><span data-stu-id="20e35-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="20e35-110">Jak odstranit jeden záznam z databáze.</span><span class="sxs-lookup"><span data-stu-id="20e35-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="20e35-111">Postup kontroly, aby se určité tlačítko označeného ve formuláři.</span><span class="sxs-lookup"><span data-stu-id="20e35-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="20e35-112">Funkce nebo technologie popsané:</span><span class="sxs-lookup"><span data-stu-id="20e35-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="20e35-113">`WebGrid` Pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="20e35-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="20e35-114">SQL `Delete` příkaz.</span><span class="sxs-lookup"><span data-stu-id="20e35-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="20e35-115">`Database.Execute` Metodu pro spuštění SQL `Delete` příkaz.</span><span class="sxs-lookup"><span data-stu-id="20e35-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="20e35-116">Co budete sestavení</span><span class="sxs-lookup"><span data-stu-id="20e35-116">What You'll Build</span></span>

<span data-ttu-id="20e35-117">V tomto kurzu předchozí jste zjistili, jak k aktualizaci záznamů existující databáze.</span><span class="sxs-lookup"><span data-stu-id="20e35-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="20e35-118">V tomto kurzu je podobný, s tím rozdílem, že místo aktualizace záznamu, budete ho odstranit.</span><span class="sxs-lookup"><span data-stu-id="20e35-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="20e35-119">Procesů, které jsou podobné, s tím rozdílem, že odstranění je jednodušší, tak v tomto kurzu bude krátký.</span><span class="sxs-lookup"><span data-stu-id="20e35-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="20e35-120">V *filmy* stránky, budete aktualizovat `WebGrid` pomocné rutiny, které se zobrazí **odstranit** odkaz vedle jednotlivých film, který může doprovázet **upravit** propojení, které jste přidali dříve.</span><span class="sxs-lookup"><span data-stu-id="20e35-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Filmy stránky zobrazující odstraní propojení pro každý film](deleting-data/_static/image1.png)

<span data-ttu-id="20e35-122">Stejně jako u úpravy, když kliknete **odstranit** odkaz, tím přejdete na jinou stránku, kde informace film je již ve formuláři:</span><span class="sxs-lookup"><span data-stu-id="20e35-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Odstranit stránky film film zobrazí](deleting-data/_static/image2.png)

<span data-ttu-id="20e35-124">Klikněte na tlačítko trvale odstranit tento záznam.</span><span class="sxs-lookup"><span data-stu-id="20e35-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="20e35-125">Přidání odkazu odstranění na film výpis</span><span class="sxs-lookup"><span data-stu-id="20e35-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="20e35-126">Budete Začněte přidáním **odstranit** propojit `WebGrid` pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="20e35-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="20e35-127">Tento odkaz je podobná **upravit** propojení, které jste přidali v předchozím kurzu.</span><span class="sxs-lookup"><span data-stu-id="20e35-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="20e35-128">Otevřete *Movies.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="20e35-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="20e35-129">Změna `WebGrid` značek v těle stránky přidáním sloupce.</span><span class="sxs-lookup"><span data-stu-id="20e35-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="20e35-130">Tady je upravených značek:</span><span class="sxs-lookup"><span data-stu-id="20e35-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="20e35-131">Nový sloupec, je tato:</span><span class="sxs-lookup"><span data-stu-id="20e35-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="20e35-132">Způsob, jak je nakonfigurovaný mřížky, **upravit** sloupec je krajní levá v mřížce a **odstranit** sloupec je nejvíce vpravo.</span><span class="sxs-lookup"><span data-stu-id="20e35-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="20e35-133">(Středník po `Year` teď sloupce v případě, že nebyla zjistíte, že.) Není co speciální o tom, kde navštěvují tyto sloupce odkaz a by snadno mohlo vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="20e35-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="20e35-134">V takovém případě jsou samostatný, aby byly těžší získat promíchají.</span><span class="sxs-lookup"><span data-stu-id="20e35-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Označena filmy stránka s odkazy upravit a podrobnosti zobrazíte, že nejsou vedle sebe](deleting-data/_static/image3.png)

<span data-ttu-id="20e35-136">Nové sloupci se zobrazuje odkaz (`<a>` element) jejíž text říká "Odstranit".</span><span class="sxs-lookup"><span data-stu-id="20e35-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="20e35-137">Cíl odkazu (jeho `href` atribut) je kód, který se nakonec přeloží na něco podobného jako tato adresa URL se `id` hodnotu pro každý film jiné:</span><span class="sxs-lookup"><span data-stu-id="20e35-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="20e35-138">Tento odkaz bude vyvolat stránku s názvem *DeleteMovie* a předejte ji ID film jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="20e35-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="20e35-139">V tomto kurzu nepřejde do podrobnosti o tom, jak je vytvořený tento odkaz, protože je téměř identický **upravit** odkaz z předchozí kurzu ([aktualizace dat databáze na webových stránkách ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251583)).</span><span class="sxs-lookup"><span data-stu-id="20e35-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251583)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="20e35-140">Vytvoření stránky odstranění</span><span class="sxs-lookup"><span data-stu-id="20e35-140">Creating the Delete Page</span></span>

<span data-ttu-id="20e35-141">Nyní můžete vytvořit stránku, která budou cílem pro **odstranit** odkaz v mřížce.</span><span class="sxs-lookup"><span data-stu-id="20e35-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="20e35-142">**Důležité** technika výběrem položky záznam odstranit a potom pomocí samostatné stránce a tlačítko potvrďte proces je velmi důležité pro zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="20e35-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="20e35-143">Protože jste si přečetli v předchozí kurzy, což *žádné* řazení změny na váš web by měl *vždy* provést pomocí formuláře &mdash; tedy pomocí operace HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="20e35-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="20e35-144">Pokud je možné změnit webu právě kliknutím na odkaz (který používá operaci GET), osoby může provádět jednoduché požadavky na váš web a odstraňovat data.</span><span class="sxs-lookup"><span data-stu-id="20e35-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="20e35-145">I vyhledávání prohledávací modul, který je indexování webu může nechtěně odstranit data jenom pomocí následujících odkazů.</span><span class="sxs-lookup"><span data-stu-id="20e35-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="20e35-146">Pokud vaše aplikace umožňuje změnit záznam osoby, budete muset prezentovat záznam uživatele pro úpravy přesto.</span><span class="sxs-lookup"><span data-stu-id="20e35-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="20e35-147">Ale mohou být tendenci Přeskočit tento krok pro odstranění záznamu.</span><span class="sxs-lookup"><span data-stu-id="20e35-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="20e35-148">Není ale Přeskočit tento krok.</span><span class="sxs-lookup"><span data-stu-id="20e35-148">Don't skip that step, though.</span></span> <span data-ttu-id="20e35-149">(Je také užitečné pro uživatele, které chcete zobrazit záznam a potvrďte, že se při odstraňování záznamů, které jsou určeny.)</span><span class="sxs-lookup"><span data-stu-id="20e35-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="20e35-150">V dalších kurz sady zobrazí se postup přidání funkce přihlášení, takže uživatel bude mít k přihlášení před odstraněním záznamu.</span><span class="sxs-lookup"><span data-stu-id="20e35-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="20e35-151">Vytvoření stránky s názvem *DeleteMovie.cshtml* a nahraďte, co je v souboru s následující kód:</span><span class="sxs-lookup"><span data-stu-id="20e35-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="20e35-152">Tento kód je stejná jako *EditMovie* stránky, vyjma toho, že místo použití polí (`<input type="text">`), obsahuje kód `<span>` elementy.</span><span class="sxs-lookup"><span data-stu-id="20e35-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="20e35-153">Není co zde můžete upravit.</span><span class="sxs-lookup"><span data-stu-id="20e35-153">There's nothing here to edit.</span></span> <span data-ttu-id="20e35-154">Všechny, které musíte udělat, je zobrazit podrobnosti o film tak, aby uživatelé měli jistotu, že se při odstraňování správné film.</span><span class="sxs-lookup"><span data-stu-id="20e35-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="20e35-155">Kód již obsahuje odkaz, který umožňuje uživateli návrat na stránku film výpis.</span><span class="sxs-lookup"><span data-stu-id="20e35-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="20e35-156">Jako v *EditMovie* stránky, ID vybrané film je uložený ve skrytém poli.</span><span class="sxs-lookup"><span data-stu-id="20e35-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="20e35-157">(Předána na stránku na prvním místě jako hodnotu řetězce dotazu.) Došlo `Html.ValidationSummary` volání, které se zobrazí chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="20e35-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="20e35-158">V takovém případě chyba může být, že žádné ID film byla předána na stránku nebo že film ID je neplatné.</span><span class="sxs-lookup"><span data-stu-id="20e35-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="20e35-159">Tato situace může nastat, pokud někdo spuštěn této stránky bez první výběr video v *filmy* stránky.</span><span class="sxs-lookup"><span data-stu-id="20e35-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="20e35-160">Tlačítko titulek **odstranit film**, a jeho název atributu je nastavena na `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="20e35-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="20e35-161">`name` Atribut bude použit v kódu k identifikaci tlačítko odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="20e35-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="20e35-162">Budete muset napsat kód pro 1) přečíst podrobnosti o film při prvním zobrazení stránky a 2) ve skutečnosti odstraňte video, když uživatel klikne na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="20e35-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="20e35-163">Přidání kódu ke čtení jeden film</span><span class="sxs-lookup"><span data-stu-id="20e35-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="20e35-164">V horní části *DeleteMovie.cshtml* přidejte následující blok kódu:</span><span class="sxs-lookup"><span data-stu-id="20e35-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="20e35-165">Tento kód je stejné jako odpovídající kód *EditMovie* stránky.</span><span class="sxs-lookup"><span data-stu-id="20e35-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="20e35-166">Získá ID film mimo řetězec dotazu a používá ID přečíst záznam z databáze.</span><span class="sxs-lookup"><span data-stu-id="20e35-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="20e35-167">Tento kód obsahuje ověřovací test (`IsInt()` a `row != null`) a ujistěte se, zda je platné ID film předávány na stránku.</span><span class="sxs-lookup"><span data-stu-id="20e35-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="20e35-168">Mějte na paměti, že tento kód měly být spuštěny pouze při prvním spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="20e35-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="20e35-169">Nechcete, aby znovu načíst film záznam z databáze, když uživatel klikne **odstranit film** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="20e35-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="20e35-170">Proto, chcete-li číst film je uvnitř testu, která uvádí, že kód `if(!IsPost)` &mdash; tedy *Pokud se nejedná o požadavek operaci post (odeslání formuláře)*.</span><span class="sxs-lookup"><span data-stu-id="20e35-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="20e35-171">Přidání kódu k odstranění vybraného videa</span><span class="sxs-lookup"><span data-stu-id="20e35-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="20e35-172">Pokud chcete odstranit videa, když uživatel klikne na tlačítko, přidejte následující kód pouze uvnitř složená závorka `@` bloku:</span><span class="sxs-lookup"><span data-stu-id="20e35-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="20e35-173">Tento kód je podobný kódu pro aktualizaci stávajícího záznamu, ale jednodušší.</span><span class="sxs-lookup"><span data-stu-id="20e35-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="20e35-174">Spuštění kódu v podstatě SQL `Delete` příkaz.</span><span class="sxs-lookup"><span data-stu-id="20e35-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="20e35-175">Jako v *EditMovie* stránky, je kód v `if(IsPost)` bloku.</span><span class="sxs-lookup"><span data-stu-id="20e35-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="20e35-176">Tato doba `if()` je trochu složitější podmínky:</span><span class="sxs-lookup"><span data-stu-id="20e35-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="20e35-177">Existují dvě podmínky sem.</span><span class="sxs-lookup"><span data-stu-id="20e35-177">There are two conditions here.</span></span> <span data-ttu-id="20e35-178">První je, že je odeslání stránky, jako jste se seznámili s před &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="20e35-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="20e35-179">Druhou podmínku, která je `!Request["buttonDelete"].IsEmpty()`, což znamená, že žádost má objekt s názvem `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="20e35-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="20e35-180">Admittedly je nepřímým způsobem testování, které tlačítko odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="20e35-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="20e35-181">Pokud formulář obsahuje více tlačítka pro odeslání dat, zobrazí se pouze název tlačítka, které bylo kliknuto v požadavku.</span><span class="sxs-lookup"><span data-stu-id="20e35-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="20e35-182">Proto, logicky Pokud název konkrétní tlačítko se zobrazí v požadavku &mdash; nebo jak jsme uvedli v kódu, pokud toto tlačítko není prázdný &mdash; , je tlačítko odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="20e35-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="20e35-183">`&&` Operátor znamená "a" (logické a).</span><span class="sxs-lookup"><span data-stu-id="20e35-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="20e35-184">Proto celý `if` podmínka je...</span><span class="sxs-lookup"><span data-stu-id="20e35-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="20e35-185">*Tento požadavek je post (ne prvního požadavku)*</span><span class="sxs-lookup"><span data-stu-id="20e35-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="20e35-186">AND</span><span class="sxs-lookup"><span data-stu-id="20e35-186">AND</span></span>  
  
<span data-ttu-id="20e35-187">** `buttonDelete` *Tlačítko byl tlačítko odeslání formuláře.*</span><span class="sxs-lookup"><span data-stu-id="20e35-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="20e35-188">Tento formulář (v faktu, tato stránka) obsahuje pouze jedno tlačítko, takže další test pro `buttonDelete` není technicky povinný.</span><span class="sxs-lookup"><span data-stu-id="20e35-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="20e35-189">Stále Chystáte se provést operaci, která bude trvale odebrat data.</span><span class="sxs-lookup"><span data-stu-id="20e35-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="20e35-190">Chcete proto ujistěte se, co nejblíže se jenom v případě, že uživatel ji explicitně vyžaduje provedení operace.</span><span class="sxs-lookup"><span data-stu-id="20e35-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="20e35-191">Předpokládejme například, rozšířit tuto stránku později a do ní přidat další tlačítka.</span><span class="sxs-lookup"><span data-stu-id="20e35-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="20e35-192">Dokonce i pak kód, který odstraní videa se spustí pouze v případě `buttonDelete` bylo stisknuto tlačítko.</span><span class="sxs-lookup"><span data-stu-id="20e35-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="20e35-193">Jako v *EditMovie* stránky, od skryté pole získat ID a pak spustit příkaz SQL.</span><span class="sxs-lookup"><span data-stu-id="20e35-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="20e35-194">Syntaxe `Delete` příkaz je:</span><span class="sxs-lookup"><span data-stu-id="20e35-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="20e35-195">Je důležité zahrnout `WHERE` klauzule a ID.</span><span class="sxs-lookup"><span data-stu-id="20e35-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="20e35-196">Pokud necháte out klauzuli WHERE *se odstraní všechny záznamy v tabulce*.</span><span class="sxs-lookup"><span data-stu-id="20e35-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="20e35-197">Protože jste viděli, předat hodnotu ID do příkazu SQL pomocí zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="20e35-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="20e35-198">Testování procesu odstranění filmu</span><span class="sxs-lookup"><span data-stu-id="20e35-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="20e35-199">Nyní můžete otestovat.</span><span class="sxs-lookup"><span data-stu-id="20e35-199">Now you can test.</span></span> <span data-ttu-id="20e35-200">Spustit *filmy* a klikněte na tlačítko **odstranit** vedle film.</span><span class="sxs-lookup"><span data-stu-id="20e35-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="20e35-201">Když *DeleteMovie* stránky se zobrazí, klikněte na tlačítko **odstranit film**.</span><span class="sxs-lookup"><span data-stu-id="20e35-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Odstranit stránku film s tlačítkem odstranit filmu](deleting-data/_static/image4.png)

<span data-ttu-id="20e35-203">Když kliknete na tlačítko, kód odstraní filmy a vrátí na film výpis.</span><span class="sxs-lookup"><span data-stu-id="20e35-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="20e35-204">Existuje vyhledejte odstraněné film a ověřte, zda je odstraněn.</span><span class="sxs-lookup"><span data-stu-id="20e35-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="20e35-205">Objevuje další</span><span class="sxs-lookup"><span data-stu-id="20e35-205">Coming Up Next</span></span>

<span data-ttu-id="20e35-206">V dalším kurzu se dozvíte, jak poskytnout všechny stránky na svém webu běžných vzhled a rozložení.</span><span class="sxs-lookup"><span data-stu-id="20e35-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="20e35-207">Úplný seznam pro stránku Movie (aktualizované odkazy odstranění)</span><span class="sxs-lookup"><span data-stu-id="20e35-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="20e35-208">Úplný seznam DeleteMovie stránky</span><span class="sxs-lookup"><span data-stu-id="20e35-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="20e35-209">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="20e35-209">Additional Resources</span></span>

- [<span data-ttu-id="20e35-210">Úvod do rozhraní ASP.NET Web programování pomocí syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="20e35-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=202890)
- <span data-ttu-id="20e35-211">[Příkaz DELETE SQL](http://www.w3schools.com/sql/sql_delete.asp) na webu W3Schools</span><span class="sxs-lookup"><span data-stu-id="20e35-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="20e35-212">[Předchozí](updating-data.md)
[další](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="20e35-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
