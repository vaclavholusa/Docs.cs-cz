---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: "Prozkoumání, jak rozhraní ASP.NET MVC scaffolds pomocná rozevírací seznam | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: abd9b5c09e942b966eb3eaaebe1b315c30b8e0c0
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="67b5a-102">Prozkoumání, jak rozhraní ASP.NET MVC scaffolds pomocná rozevírací seznam</span><span class="sxs-lookup"><span data-stu-id="67b5a-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="67b5a-103">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="67b5a-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="67b5a-104">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složku a potom vyberte **přidat kontroler**.</span><span class="sxs-lookup"><span data-stu-id="67b5a-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="67b5a-105">Název kontroleru **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="67b5a-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="67b5a-106">Nastavení možností pro **přidat kontroler** dialogové okno, jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="67b5a-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="67b5a-107">Upravit *StoreManager\Index.cshtml* zobrazení a odebrání `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="67b5a-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="67b5a-108">Odebrání `AlbumArtUrl` bude čitelnost prezentaci.</span><span class="sxs-lookup"><span data-stu-id="67b5a-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="67b5a-109">Dokončený kód je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="67b5a-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="67b5a-110">Otevřete *Controllers\StoreManagerController.cs* souboru a najděte `Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="67b5a-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="67b5a-111">Přidat `OrderBy` klauzule, alba seřadí podle ceny.</span><span class="sxs-lookup"><span data-stu-id="67b5a-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="67b5a-112">Kód dokončení jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="67b5a-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="67b5a-113">Řazení podle ceny budou usnadňují testování změny databáze.</span><span class="sxs-lookup"><span data-stu-id="67b5a-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="67b5a-114">Pokud testujete úpravy a vytvoření metody, můžete tak uložená data se zobrazí první nízkou cenu.</span><span class="sxs-lookup"><span data-stu-id="67b5a-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="67b5a-115">Otevřete *StoreManager\Edit.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="67b5a-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="67b5a-116">Přidejte následující řádek bezprostředně za značce legendy.</span><span class="sxs-lookup"><span data-stu-id="67b5a-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="67b5a-117">Následující kód ukazuje kontextu této změny:</span><span class="sxs-lookup"><span data-stu-id="67b5a-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="67b5a-118">`AlbumId` Je potřeba provést změny k záznamu alb.</span><span class="sxs-lookup"><span data-stu-id="67b5a-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="67b5a-119">Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="67b5a-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="67b5a-120">Vyberte k **správce** propojení a pak vyberte **vytvořit nový** odkaz pro vytvoření nové album.</span><span class="sxs-lookup"><span data-stu-id="67b5a-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="67b5a-121">Ověřte, zda že byla uložena alba informace.</span><span class="sxs-lookup"><span data-stu-id="67b5a-121">Verify the album information was saved.</span></span> <span data-ttu-id="67b5a-122">Upravit alba a ověřte, zda jsou nastavené jako trvalé změny, které jste provedli.</span><span class="sxs-lookup"><span data-stu-id="67b5a-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="67b5a-123">Schéma Album</span><span class="sxs-lookup"><span data-stu-id="67b5a-123">The Album Schema</span></span>

<span data-ttu-id="67b5a-124">`StoreManager` Kontroler vytvořený mechanismus generování uživatelského rozhraní MVC umožňuje CRUD (vytvořit, číst, Update, Delete) přístup k alba v databázi Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="67b5a-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="67b5a-125">Schéma album informace jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="67b5a-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="67b5a-126">`Albums` Tabulky neukládá genre alba a popis, ukládá cizí klíč k `Genres` tabulky.</span><span class="sxs-lookup"><span data-stu-id="67b5a-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="67b5a-127">`Genres` Tabulka obsahuje genre název a popis.</span><span class="sxs-lookup"><span data-stu-id="67b5a-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="67b5a-128">Podobně `Albums` tabulka neobsahuje název alba umělci, ale cizí klíč k `Artists` tabulky.</span><span class="sxs-lookup"><span data-stu-id="67b5a-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="67b5a-129">`Artists` Tabulka obsahuje jméno.</span><span class="sxs-lookup"><span data-stu-id="67b5a-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="67b5a-130">Pokud si projdete data v `Albums` tabulky, můžete zobrazit každý řádek obsahuje cizí klíč k `Genres` tabulky a cizí klíč `Artists` tabulky.</span><span class="sxs-lookup"><span data-stu-id="67b5a-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="67b5a-131">Následující obrázek zobrazit některá data tabulky z `Albums` tabulky.</span><span class="sxs-lookup"><span data-stu-id="67b5a-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="67b5a-132">Značka HTML vyberte</span><span class="sxs-lookup"><span data-stu-id="67b5a-132">The HTML Select Tag</span></span>

<span data-ttu-id="67b5a-133">HTML `<select>` – element (vytvořený HTML [rozevírací seznam](https://msdn.microsoft.com/library/dd492948.aspx) pomocné rutiny) se používá k zobrazení úplný seznam hodnot (například seznam žánry).</span><span class="sxs-lookup"><span data-stu-id="67b5a-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="67b5a-134">Pro úpravy formulářů Pokud se aktuální hodnota označuje, můžete seznamu výběru zobrazit aktuální hodnota.</span><span class="sxs-lookup"><span data-stu-id="67b5a-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="67b5a-135">Jsme viděli dříve tomto při Nastaví vybrané hodnoty **komedie**.</span><span class="sxs-lookup"><span data-stu-id="67b5a-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="67b5a-136">Seznam select je ideální pro zobrazení kategorie nebo dat cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="67b5a-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="67b5a-137">`<select>` Element pro cizí klíč Genre zobrazí seznam názvů možné genre, ale při ukládání formuláře je vlastnost Genre aktualizovat hodnotou Genre cizího klíče, nikoli název zobrazený genre.</span><span class="sxs-lookup"><span data-stu-id="67b5a-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="67b5a-138">Na následujícím obrázku je genre vybrané **Disco** a umělce **Donna letní**.</span><span class="sxs-lookup"><span data-stu-id="67b5a-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="67b5a-139">Zkoumání rozhraní ASP.NET MVC vygeneroval kód</span><span class="sxs-lookup"><span data-stu-id="67b5a-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="67b5a-140">Otevřete *Controllers\StoreManagerController.cs* souboru a najděte `HTTP GET Create` metoda.</span><span class="sxs-lookup"><span data-stu-id="67b5a-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="67b5a-141">`Create` Metoda přidá dva [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objekty ke `ViewBag`, jeden, který bude obsahovat informace genre a jeden, který bude obsahovat umělcem informace.</span><span class="sxs-lookup"><span data-stu-id="67b5a-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="67b5a-142">[SelectList](https://msdn.microsoft.com/library/dd505286.aspx) přetížení konstruktoru používá výše má tři argumenty:</span><span class="sxs-lookup"><span data-stu-id="67b5a-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="67b5a-143">*položky*: [rozhraní IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) obsahující položky v seznamu.</span><span class="sxs-lookup"><span data-stu-id="67b5a-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="67b5a-144">V příkladu nahoře, vrácený seznam žánry `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="67b5a-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="67b5a-145">*dataValueField*: název vlastnosti v **rozhraní IEnumerable** seznamu, který obsahuje hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="67b5a-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="67b5a-146">V příkladu nahoře `GenreId` a `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="67b5a-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="67b5a-147">*dataTextField*: název vlastnosti v **rozhraní IEnumerable** seznamu, který obsahuje informace k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="67b5a-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="67b5a-148">V umělci a tabulka genre `name` pole se používá.</span><span class="sxs-lookup"><span data-stu-id="67b5a-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="67b5a-149">Otevřete *Views\StoreManager\Create.cshtml* soubor a zkontrolujte `Html.DropDownList` kód pomocné rutiny pro genre pole.</span><span class="sxs-lookup"><span data-stu-id="67b5a-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="67b5a-150">První řádek ukazuje, že zobrazení pro vytváření trvá `Album` modelu.</span><span class="sxs-lookup"><span data-stu-id="67b5a-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="67b5a-151">V `Create` výše uvedená metoda, byl předán žádný model, tak získá zobrazení **null** `Album` modelu.</span><span class="sxs-lookup"><span data-stu-id="67b5a-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="67b5a-152">V tomto okamžiku se vytváří nové album proto nemáme žádné `Album` data pro ni.</span><span class="sxs-lookup"><span data-stu-id="67b5a-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="67b5a-153">[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) přetížení výše uvedeném převezme název pole, které chcete vytvořit vazbu k modelu.</span><span class="sxs-lookup"><span data-stu-id="67b5a-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="67b5a-154">Tento název se používá také k Hledat **ViewBag** objekt obsahující [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objektu.</span><span class="sxs-lookup"><span data-stu-id="67b5a-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="67b5a-155">Pomocí této přetížení, je nutné na název **ViewBag SelectList** objekt `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="67b5a-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="67b5a-156">Druhý parametr (`String.Empty`) je text, který se zobrazí, pokud je vybrána žádná položka.</span><span class="sxs-lookup"><span data-stu-id="67b5a-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="67b5a-157">Toto je přesně co chceme, při vytváření nové album.</span><span class="sxs-lookup"><span data-stu-id="67b5a-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="67b5a-158">Je-li druhý parametr odebrat a použít následující kód:</span><span class="sxs-lookup"><span data-stu-id="67b5a-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="67b5a-159">V seznamu vyberte výchozí první prvek nebo Rock ve výběru.</span><span class="sxs-lookup"><span data-stu-id="67b5a-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="67b5a-160">Zkoumání `HTTP POST Create` metoda.</span><span class="sxs-lookup"><span data-stu-id="67b5a-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="67b5a-161">Toto přetížení `Create` metoda trvá `album` objekt, vytvořený systémem vazby modelu ASP.NET MVC z formuláře hodnoty odeslány.</span><span class="sxs-lookup"><span data-stu-id="67b5a-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="67b5a-162">Při odesílání nové album, pokud stav modelu, který je platný, a neexistují žádné chyby databáze, se přidá nové album databáze.</span><span class="sxs-lookup"><span data-stu-id="67b5a-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="67b5a-163">Následující obrázek ukazuje vytvoření nové album.</span><span class="sxs-lookup"><span data-stu-id="67b5a-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="67b5a-164">Můžete použít [nástroj fiddler](http://www.fiddler2.com/fiddler2/) prozkoumat hodnoty odeslaného formuláře vazbou modelu ASP.NET MVC používá k vytvoření objektu alb.</span><span class="sxs-lookup"><span data-stu-id="67b5a-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="67b5a-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="67b5a-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="67b5a-166">Refaktoring vytvoření ViewBag SelectList</span><span class="sxs-lookup"><span data-stu-id="67b5a-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="67b5a-167">Obě `Edit` metody a `HTTP POST Create` metoda mít identické kódu nastavit **SelectList** v **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="67b5a-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="67b5a-168">Ve smyslu [SUCHÝCH](http://en.wikipedia.org/wiki/Don't_repeat_yourself), jsme se Refaktorovat tento kód.</span><span class="sxs-lookup"><span data-stu-id="67b5a-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="67b5a-169">Vytočit použití tohoto rozdělili kód později.</span><span class="sxs-lookup"><span data-stu-id="67b5a-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="67b5a-170">Vytvoření nové metody pro přidání genre a umělcem **SelectList** k **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="67b5a-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="67b5a-171">Nahraďte dva řádky nastavení `ViewBag` v každé z `Create` a `Edit` metody s volání `SetGenreArtistViewBag` metoda.</span><span class="sxs-lookup"><span data-stu-id="67b5a-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="67b5a-172">Dokončený kód je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="67b5a-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="67b5a-173">Vytvořit nového alba a upravit album k ověření, že změny fungovat.</span><span class="sxs-lookup"><span data-stu-id="67b5a-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="67b5a-174">Explicitně předávání SelectList rozevírací seznam</span><span class="sxs-lookup"><span data-stu-id="67b5a-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="67b5a-175">Vytvoření zobrazení vytvořit a upravit pomocí generování uživatelského rozhraní ASP.NET MVC následující **rozevírací seznam** přetížení:</span><span class="sxs-lookup"><span data-stu-id="67b5a-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="67b5a-176">`DropDownList` Kód pro vytvoření zobrazení je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="67b5a-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="67b5a-177">Protože `ViewBag` vlastnost pro `SelectList` jmenuje `GenreId`, **rozevírací seznam** pomocníka použijete `GenreId` **SelectList** v **ViewBag** .</span><span class="sxs-lookup"><span data-stu-id="67b5a-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="67b5a-178">V následujícím **rozevírací seznam** přetížení, `SelectList` explicitně předán.</span><span class="sxs-lookup"><span data-stu-id="67b5a-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="67b5a-179">Otevřete *Views\StoreManager\Edit.cshtml* soubor a změňte **rozevírací seznam** volání explicitně předávat **SelectList**, pomocí výše uvedených přetížení.</span><span class="sxs-lookup"><span data-stu-id="67b5a-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="67b5a-180">To lze proveďte pro Genre kategorii.</span><span class="sxs-lookup"><span data-stu-id="67b5a-180">Do this for the Genre category.</span></span> <span data-ttu-id="67b5a-181">Dokončený kód je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="67b5a-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="67b5a-182">Spusťte aplikaci a klikněte na tlačítko **správce** odkaz, pak přejděte do alb Jazz a vyberte **upravit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="67b5a-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="67b5a-183">Místo zobrazení Jazz jako aktuálně vybrané genre, zobrazí se Rock.</span><span class="sxs-lookup"><span data-stu-id="67b5a-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="67b5a-184">Pokud argument řetězce (vlastnost pro vazbu) a **SelectList** objekt mají stejný název, vybrané hodnoty se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="67b5a-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="67b5a-185">Pokud žádné vybrané hodnoty zadané, prohlížečů výchozí prvním elementem v **SelectList**(což je **Rock** v předchozím příkladu).</span><span class="sxs-lookup"><span data-stu-id="67b5a-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="67b5a-186">Jedná se o známé omezení **rozevírací seznam** pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="67b5a-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="67b5a-187">Otevřete *Controllers\StoreManagerController.cs* soubor a změňte **SelectList** objektu názvy `Genres` a `Artists`.</span><span class="sxs-lookup"><span data-stu-id="67b5a-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="67b5a-188">Dokončený kód je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="67b5a-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="67b5a-189">Názvy žánry a umělci jsou lepší názvy kategorií, obsahují více než jen ID každé kategorii.</span><span class="sxs-lookup"><span data-stu-id="67b5a-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="67b5a-190">Refaktoring, kterou jsme provedli dříve placené.</span><span class="sxs-lookup"><span data-stu-id="67b5a-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="67b5a-191">Místo změna **ViewBag** v čtyři metody byly izolované na našem změny `SetGenreArtistViewBag` metoda.</span><span class="sxs-lookup"><span data-stu-id="67b5a-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="67b5a-192">Změna **rozevírací seznam** volání v části Vytvoření a úprava zobrazení s novým **SelectList** názvy.</span><span class="sxs-lookup"><span data-stu-id="67b5a-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="67b5a-193">Nový kód pro úpravy zobrazení je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="67b5a-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="67b5a-194">Zobrazení pro vytváření vyžaduje k zabránění zobrazení první položky SelectList prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="67b5a-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="67b5a-195">Vytvořit nového alba a upravit album k ověření, že změny fungovat.</span><span class="sxs-lookup"><span data-stu-id="67b5a-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="67b5a-196">Testování kódu upravit tak, že vyberete album s genre než Rock.</span><span class="sxs-lookup"><span data-stu-id="67b5a-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="67b5a-197">Model zobrazení pomocí pomocníka rozevírací seznam</span><span class="sxs-lookup"><span data-stu-id="67b5a-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="67b5a-198">Vytvořte novou třídu ve složce ViewModels s názvem `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="67b5a-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="67b5a-199">Nahraďte kód v `AlbumSelectListViewModel` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="67b5a-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="67b5a-200">`AlbumSelectListViewModel` Konstruktor přebírá album, seznam umělci a žánry a vytvoří objekt obsahující alba a `SelectList` žánry a umělci.</span><span class="sxs-lookup"><span data-stu-id="67b5a-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="67b5a-201">Sestavení projektu proto `AlbumSelectListViewModel` je k dispozici při vytváření zobrazení v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="67b5a-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="67b5a-202">Přidat `EditVM` metodu `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="67b5a-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="67b5a-203">Dokončený kód je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="67b5a-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="67b5a-204">Klikněte pravým tlačítkem na `AlbumSelectListViewModel`, vyberte **vyřešit**, pak **pomocí MvcMusicStore.ViewModels;**.</span><span class="sxs-lookup"><span data-stu-id="67b5a-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="67b5a-205">Alternativně můžete přidat následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="67b5a-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="67b5a-206">Klikněte pravým tlačítkem na `EditVM` a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="67b5a-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="67b5a-207">Pomocí možností vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="67b5a-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="67b5a-208">Vyberte **přidat**, potom můžete nahradit obsah *Views\StoreManager\EditVM.cshtml* soubor s následující:</span><span class="sxs-lookup"><span data-stu-id="67b5a-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="67b5a-209">`EditVM` Značek je velmi podobný původní `Edit` značek s následujícími výjimkami.</span><span class="sxs-lookup"><span data-stu-id="67b5a-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="67b5a-210">Vlastnosti v modelu `Edit` zobrazení jsou ve formátu `model.property`(například `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="67b5a-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="67b5a-211">Vlastnosti v modelu `EditVm` zobrazení jsou ve formátu `model.Album.property`(například `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="67b5a-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="67b5a-212">Důvodem je, že `EditVM` zobrazení byla předána kontejner pro `Album`, nikoli `Album` jako v `Edit` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="67b5a-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="67b5a-213">**Rozevírací seznam** druhý parametr pochází z modelu zobrazení, není **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="67b5a-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="67b5a-214">**BeginForm** Pomocník `EditVM` zobrazit explicitně příspěvky zpět `Edit` metody akce.</span><span class="sxs-lookup"><span data-stu-id="67b5a-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="67b5a-215">Zveřejněním zpět do `Edit` akce, nemáme k zápisu `HTTP POST EditVM` akce a můžete opakovaně použít `HTTP POST` `Edit` akce.</span><span class="sxs-lookup"><span data-stu-id="67b5a-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="67b5a-216">Spusťte aplikaci a upravit album.</span><span class="sxs-lookup"><span data-stu-id="67b5a-216">Run the application and edit an album.</span></span> <span data-ttu-id="67b5a-217">Změňte adresu URL používat `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="67b5a-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="67b5a-218">Změňte pole a stiskněte tlačítko **Uložit** tlačítko ověřit kód funguje.</span><span class="sxs-lookup"><span data-stu-id="67b5a-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="67b5a-219">Jaký přístup mám použít?</span><span class="sxs-lookup"><span data-stu-id="67b5a-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="67b5a-220">Všechny tři přístupy, zobrazí se acceptible.</span><span class="sxs-lookup"><span data-stu-id="67b5a-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="67b5a-221">Celá řada vývojářů přednost explictily průchodu `SelectList` k `DropDownList` pomocí `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="67b5a-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="67b5a-222">Tento přístup má výhodu, která poskytuje flexibilní použití vhodnější název kolekce.</span><span class="sxs-lookup"><span data-stu-id="67b5a-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="67b5a-223">Přímý přístup do jedné paměti je nelze pojmenovat `ViewBag SelectList` objektu stejný název jako vlastnost modelu.</span><span class="sxs-lookup"><span data-stu-id="67b5a-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="67b5a-224">Někteří vývojáři přednost ViewModel přístup.</span><span class="sxs-lookup"><span data-stu-id="67b5a-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="67b5a-225">Ostatní zvažte podrobnější značek a generovaný kód HTML přístupu ViewModel nevýhodou.</span><span class="sxs-lookup"><span data-stu-id="67b5a-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="67b5a-226">V této části jsme se dozvěděli, tři způsoby používání **rozevírací seznam** daty kategorie.</span><span class="sxs-lookup"><span data-stu-id="67b5a-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="67b5a-227">V další části ukážeme, jak přidat novou kategorii.</span><span class="sxs-lookup"><span data-stu-id="67b5a-227">In the next section, we'll show how to add a new category.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="67b5a-228">[Předchozí](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[další](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="67b5a-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
