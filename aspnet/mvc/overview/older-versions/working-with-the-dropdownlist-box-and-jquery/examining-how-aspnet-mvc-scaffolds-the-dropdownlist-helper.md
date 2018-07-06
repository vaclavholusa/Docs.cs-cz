---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Zkoumání, jak ASP.NET MVC vygeneruje uživatelské rozhraní pomocné rutiny DropDownList | Dokumentace Microsoftu
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 8e407d5426ed93388b3c6f7bdb125be5356c002c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837127"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="e5cd4-102">Zkoumání, jak ASP.NET MVC vygeneruje uživatelské rozhraní pomocné rutiny DropDownList</span><span class="sxs-lookup"><span data-stu-id="e5cd4-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="e5cd4-103">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="e5cd4-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="e5cd4-104">V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *řadiče* složku a pak vyberte **přidat kontroler**.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="e5cd4-105">Název kontroleru **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="e5cd4-106">Nastavení možností pro **přidat kontroler** dialogového okna, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="e5cd4-107">Upravit *StoreManager\Index.cshtml* zobrazení a odebrání `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="e5cd4-108">Odebrání `AlbumArtUrl` se čitelnost prezentaci.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="e5cd4-109">Dokončený kód je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="e5cd4-110">Otevřít *Controllers\StoreManagerController.cs* souborů a vyhledejte `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="e5cd4-111">Přidat `OrderBy` klauzule tak alba budou seřazeny podle ceny.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="e5cd4-112">Kompletní kód je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="e5cd4-113">Řazení podle ceny vám usnadní testování změn do databáze.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="e5cd4-114">Při testování upravit a vytvořit metody, můžete tak uložených dat budou zobrazovat první nízkou cenu.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="e5cd4-115">Otevřít *StoreManager\Edit.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="e5cd4-116">Přidejte následující řádek bezprostředně po tagu legendy.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="e5cd4-117">Následující kód ukazuje kontextu této změny:</span><span class="sxs-lookup"><span data-stu-id="e5cd4-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="e5cd4-118">`AlbumId` Je potřeba provést změny na alba záznam.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="e5cd4-119">Stisknutím kláves CTRL + F5 spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="e5cd4-120">Kliknutím **správce** propojení a potom vyberte **vytvořit nový** odkaz pro vytvoření nové album.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="e5cd4-121">Ověřte, zda že byl uložen alba informace.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-121">Verify the album information was saved.</span></span> <span data-ttu-id="e5cd4-122">Upravit alba a ověřte, zda jsou trvalé, provedené změny.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="e5cd4-123">Alba schématu</span><span class="sxs-lookup"><span data-stu-id="e5cd4-123">The Album Schema</span></span>

<span data-ttu-id="e5cd4-124">`StoreManager` Řadič vytvořené mechanismu generování uživatelského rozhraní MVC umožňuje přístup CRUD (vytváření, čtení, Update, Delete) ke alba v databázi music store.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="e5cd4-125">Schéma alba informace naleznete níže:</span><span class="sxs-lookup"><span data-stu-id="e5cd4-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="e5cd4-126">`Albums` Tabulky neukládá žánr alba a popis, ukládá cizí klíč `Genres` tabulky.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="e5cd4-127">`Genres` Tabulka obsahuje žánr název a popis.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="e5cd4-128">Podobně `Albums` tabulka neobsahuje název alba umělcům, ale cizí klíč `Artists` tabulky.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="e5cd4-129">`Artists` Tabulka obsahuje jméno.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="e5cd4-130">Pokud prozkoumáte data v `Albums` tabulky, se zobrazí každý řádek obsahuje cizí klíč `Genres` tabulky a cizí klíč `Artists` tabulky.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="e5cd4-131">Na následujícím obrázku ukazují některé data tabulky z `Albums` tabulky.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="e5cd4-132">Značka HTML Select</span><span class="sxs-lookup"><span data-stu-id="e5cd4-132">The HTML Select Tag</span></span>

<span data-ttu-id="e5cd4-133">Kód HTML `<select>` – element (vytvořené pomocí jazyka HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) pomocné rutiny) umožňuje zobrazit úplný seznam hodnot (jako je například seznam žánry).</span><span class="sxs-lookup"><span data-stu-id="e5cd4-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="e5cd4-134">Pro editační formuláře Pokud aktuální hodnota je známý, můžete seznamu výběru zobrazit aktuální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="e5cd4-135">Jsme viděli dříve tomto při nastavíme na vybranou hodnotu **komedie**.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="e5cd4-136">Seznam select je ideální pro zobrazení kategorií nebo data cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="e5cd4-137">`<select>` – Element pro cizí klíč žánr zobrazí seznam názvů možné genre, ale při ukládání formuláři vlastnost žánr se aktualizuje s rozšířením podle tematických hodnoty cizího klíče, nikoli název zobrazený žánr.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="e5cd4-138">Na následujícím obrázku je rozšířením podle tematických vybrané **Roz** a je umělce **Donnou léto**.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="e5cd4-139">Zkoumání rozhraní ASP.NET MVC automaticky generovaný kód</span><span class="sxs-lookup"><span data-stu-id="e5cd4-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="e5cd4-140">Otevřít *Controllers\StoreManagerController.cs* souborů a vyhledejte `HTTP GET Create` metody.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="e5cd4-141">`Create` Metoda přidá dvě [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objektů `ViewBag`, jeden tak, aby obsahovala žánr informace a jeden obsahuje informace o interpreta.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="e5cd4-142">[SelectList](https://msdn.microsoft.com/library/dd505286.aspx) přetížení konstruktoru využité nad má tři argumenty:</span><span class="sxs-lookup"><span data-stu-id="e5cd4-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="e5cd4-143">*položky*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) obsahující položky v seznamu.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="e5cd4-144">V příkladu výše, vrácený seznam žánry `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="e5cd4-145">*dataValueField*: název vlastnosti v **IEnumerable** seznam, který obsahuje hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="e5cd4-146">V příkladu výše `GenreId` a `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="e5cd4-147">*dataTextField*: název vlastnosti v **IEnumerable** seznam, který obsahuje informace k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="e5cd4-148">V vašim animátorům a rozšířením podle tematických tabulky `name` pole se používá.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="e5cd4-149">Otevřít *Views\StoreManager\Create.cshtml* souboru a zkoumat `Html.DropDownList` pomocné rutiny značky žánr pole.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="e5cd4-150">První řádek ukazuje, že zobrazení pro vytváření trvá `Album` modelu.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="e5cd4-151">V `Create` metody uvedené výše, byl předán žádný model, takže získá zobrazení **null** `Album` modelu.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="e5cd4-152">V tomto okamžiku vytváříme nové album, takže nemáme žádné `Album` data pro něj.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="e5cd4-153">[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) přetížení výše uvedené přebírá název pole, které chcete vytvořit vazbu modelu.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="e5cd4-154">Také používá tento název k vyhledání **objekt ViewBag** objekt obsahující [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objektu.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="e5cd4-155">Pomocí tohoto přetížení, je nutné provést název **objekt ViewBag SelectList** objekt `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="e5cd4-156">Druhý parametr (`String.Empty`) je text, který se zobrazí, když není vybrána žádná položka.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="e5cd4-157">To je přesně chceme při vytváření nové album.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="e5cd4-158">Je-li odebrat druhý parametr a použít následující kód:</span><span class="sxs-lookup"><span data-stu-id="e5cd4-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="e5cd4-159">První prvek nebo Rock výchozí seznamu příkazu select v naší ukázce.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="e5cd4-160">Zkoumání `HTTP POST Create` metody.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="e5cd4-161">Toto přetížení `Create` přijímá metodu `album` objekt vytvořený pomocí systému vazby modelu ASP.NET MVC z hodnot formuláře, pošle.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="e5cd4-162">Při odeslání nové album, pokud je platný stav modelu a nejsou žádné chyby databáze, se přidá nový alba databáze.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="e5cd4-163">Vytvoření nové album naleznete na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="e5cd4-164">Můžete použít [nástroj fiddler](http://www.fiddler2.com/fiddler2/) ke kontrole hodnot odeslaného formuláře, že vazba modelu ASP.NET MVC používá k vytvoření objektu alba.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="e5cd4-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="e5cd4-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="e5cd4-166">Refaktoring vytváření SelectList objekt ViewBag</span><span class="sxs-lookup"><span data-stu-id="e5cd4-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="e5cd4-167">Oba `Edit` metody a `HTTP POST Create` metoda mít stejný kód nastavit **SelectList** v **objekt ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="e5cd4-168">V duchu [suchého](http://en.wikipedia.org/wiki/Don't_repeat_yourself), jsme se tento kód Refaktorovat.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="e5cd4-169">Teď Uděláme to využívání Refaktorovat kód později.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="e5cd4-170">Vytvoření nové metody pro přidání žánr a interpreta **SelectList** k **objekt ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="e5cd4-171">Nahraďte dva řádky nastavení `ViewBag` ve všech `Create` a `Edit` pomocí volání metody `SetGenreArtistViewBag` metoda.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="e5cd4-172">Dokončený kód je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="e5cd4-173">Vytvořte nové album a upravte alba k ověření, že změny fungovat.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="e5cd4-174">Explicitně předávání SelectList DropDownList</span><span class="sxs-lookup"><span data-stu-id="e5cd4-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="e5cd4-175">Vytvoření a úprava zobrazení vytvořené pomocí generování uživatelského rozhraní ASP.NET MVC následující **DropDownList** přetížení:</span><span class="sxs-lookup"><span data-stu-id="e5cd4-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="e5cd4-176">`DropDownList` Kód pro vytvoření zobrazení se zobrazuje níže.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="e5cd4-177">Protože `ViewBag` vlastnost `SelectList` jmenuje `GenreId`, **DropDownList** pomocné rutiny použije `GenreId` **SelectList** v **objekt ViewBag** .</span><span class="sxs-lookup"><span data-stu-id="e5cd4-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="e5cd4-178">V následujícím **DropDownList** přetížení, `SelectList` explicitně je předán.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="e5cd4-179">Otevřít *Views\StoreManager\Edit.cshtml* soubor a změňte **DropDownList** volání explicitně předávat **SelectList**, pomocí přetížení výše.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="e5cd4-180">To lze proveďte pro kategorii Žánr.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-180">Do this for the Genre category.</span></span> <span data-ttu-id="e5cd4-181">Dokončený kód je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="e5cd4-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="e5cd4-182">Spusťte aplikaci a klikněte na tlačítko **správce** propojení, pak přejděte do alb Jazz a vyberte **upravit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="e5cd4-183">Místo zobrazení Jazz jako aktuálně vybraný genre, zobrazí se Rock.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="e5cd4-184">Pokud argument řetězce (vlastnost pro vazbu) a **SelectList** objekt mají stejný název, vybrané hodnoty se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="e5cd4-185">Pokud neexistuje žádná vybraná hodnota k dispozici, výchozí prohlížeče na první prvek v **SelectList**(což je **Rock** v předchozím příkladu).</span><span class="sxs-lookup"><span data-stu-id="e5cd4-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="e5cd4-186">Toto je známé omezení **DropDownList** pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="e5cd4-187">Otevřít *Controllers\StoreManagerController.cs* soubor a změňte **SelectList** názvy do objektů `Genres` a `Artists`.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="e5cd4-188">Dokončený kód je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="e5cd4-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="e5cd4-189">Názvy žánry a umělci jsou lepší názvy kategorií, obsahují více než jen ID každé kategorie.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="e5cd4-190">Vyplatilo refaktoring, který jsme to udělali dříve.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="e5cd4-191">Místo změny **objekt ViewBag** v čtyři metody byly izolován na naše změny `SetGenreArtistViewBag` metoda.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="e5cd4-192">Změnit **DropDownList** volání v části Vytvoření a úprava zobrazení s novým **SelectList** názvy.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="e5cd4-193">Nový kód pro zobrazení pro úpravy je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="e5cd4-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="e5cd4-194">Vytvořit zobrazení vyžaduje zabránit se zobrazí první položku SelectList prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="e5cd4-195">Vytvořte nové album a upravte alba k ověření, že změny fungovat.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="e5cd4-196">Testování kódu upravit tak, že vyberete alba s žánr než Rock.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="e5cd4-197">Pomocí zobrazení modelu s pomocné rutiny DropDownList</span><span class="sxs-lookup"><span data-stu-id="e5cd4-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="e5cd4-198">Vytvořte novou třídu v modely ViewModels složku s názvem `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="e5cd4-199">Nahraďte kód v `AlbumSelectListViewModel` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e5cd4-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="e5cd4-200">`AlbumSelectListViewModel` Konstruktor přijímá alba, seznam vašim animátorům a žánry a vytvoří objekt, který obsahuje alba a `SelectList` žánry a umělci.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="e5cd4-201">Sestavte projekt proto `AlbumSelectListViewModel` je k dispozici, když budeme vytvářet zobrazení v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="e5cd4-202">Přidat `EditVM` metodu `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="e5cd4-203">Dokončený kód je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="e5cd4-204">Klikněte pravým tlačítkem myši `AlbumSelectListViewModel`vyberte **vyřešit**, pak **pomocí MvcMusicStore.ViewModels;**.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="e5cd4-205">Alternativně můžete přidat následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="e5cd4-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="e5cd4-206">Klikněte pravým tlačítkem myši `EditVM` a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="e5cd4-207">Pomocí možnosti zobrazené níže.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="e5cd4-208">Vyberte **přidat**, poté nahraďte obsah *Views\StoreManager\EditVM.cshtml* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e5cd4-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="e5cd4-209">`EditVM` Značek je velmi podobný původní `Edit` značky s následujícími výjimkami.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="e5cd4-210">Vlastnosti v modelu `Edit` zobrazení jsou ve tvaru `model.property`(například `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="e5cd4-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="e5cd4-211">Vlastnosti v modelu `EditVm` zobrazení jsou ve tvaru `model.Album.property`(například `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="e5cd4-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="e5cd4-212">Důvodem je, že `EditVM` zobrazení se předá kontejner `Album`, nikoli `Album` stejně jako v `Edit` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="e5cd4-213">**DropDownList** druhý parametr pochází z modelu zobrazení není **objekt ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="e5cd4-214">**BeginForm** Pomocník `EditVM` zobrazení explicitně odeslat zpět `Edit` metody akce.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="e5cd4-215">Tím, že publikuje zpět `Edit` akce, nemáme pro zápis `HTTP POST EditVM` akce a můžete znovu použít `HTTP POST` `Edit` akce.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="e5cd4-216">Spusťte aplikaci a upravit alba.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-216">Run the application and edit an album.</span></span> <span data-ttu-id="e5cd4-217">Změňte adresu URL na použití `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="e5cd4-218">Změnit pole a klikněte **Uložit** tlačítko ověřit kód funguje.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="e5cd4-219">Jaký přístup byste měli použít?</span><span class="sxs-lookup"><span data-stu-id="e5cd4-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="e5cd4-220">Všechny tři postupy uvedené jsou acceptible.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="e5cd4-221">Mnoho vývojářů raději explictily pass `SelectList` k `DropDownList` pomocí `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="e5cd4-222">Tento přístup má výhodu současného vám poskytuje flexibilitu používat více vhodný název pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="e5cd4-223">Jeden výstrahou je nelze pojmenovat `ViewBag SelectList` stejný název jako vlastnost modelu objektu.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="e5cd4-224">Někteří vývojáři radši ViewModel přístup.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="e5cd4-225">Ostatní vezměte v úvahu podrobnější značek a generovaný kód HTML přístupu ViewModel nevýhodu.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="e5cd4-226">V této části jsme se naučili tři způsoby používání **DropDownList** s daty kategorie.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="e5cd4-227">V další části vám ukážeme, jak přidat novou kategorii.</span><span class="sxs-lookup"><span data-stu-id="e5cd4-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e5cd4-228">[Předchozí](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [další](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="e5cd4-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
