---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: "Přidat novou kategorii k rozevírací seznam pomocí uživatelského rozhraní jQuery | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 0cc51fbe84124a62f0c1254faab796cbcdc7efd6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="0fd78-102">Přidat novou kategorii k rozevírací seznam pomocí uživatelského rozhraní jQuery</span><span class="sxs-lookup"><span data-stu-id="0fd78-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="0fd78-103">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0fd78-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="0fd78-104">HTML `Select` značka je ideální pro prezentace seznamu pevné kategorie dat, ale často je nutné přidat novou kategorii.</span><span class="sxs-lookup"><span data-stu-id="0fd78-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="0fd78-105">Předpokládejme, že nám chcete přidat do kategorie v naší databázi genre "Opera"?</span><span class="sxs-lookup"><span data-stu-id="0fd78-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="0fd78-106">V této části se budeme používat jQuery UI přidat dialogové okno, které jsme můžete použít k přidání nové kategorie.</span><span class="sxs-lookup"><span data-stu-id="0fd78-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="0fd78-107">Následující obrázek ukazuje, jak se v prohlížeči prezentovat uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0fd78-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="0fd78-108">Když uživatel vybere **přidat nové Genre** odkazu, který je automaticky otevírané okno dialogové okno zobrazí výzvu k zadání nový název genre (a volitelně jeho popis).</span><span class="sxs-lookup"><span data-stu-id="0fd78-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="0fd78-109">Obrázek níže ukazují **přidat Genre** automaticky otevíraná okna.</span><span class="sxs-lookup"><span data-stu-id="0fd78-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="0fd78-110">Kdy je nový název genre zadáno a **Uložit** nabídnutých tlačítko těchto podmínek:</span><span class="sxs-lookup"><span data-stu-id="0fd78-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="0fd78-111">Volání AJAX odešle data, která mají metodu Create řadiče Genre, která uloží novou genre do databáze a vrátí nové informace genre (genre název a ID) jako JSON.</span><span class="sxs-lookup"><span data-stu-id="0fd78-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="0fd78-112">JavaScript přidává nová data genre do seznamu příkazu select.</span><span class="sxs-lookup"><span data-stu-id="0fd78-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="0fd78-113">JavaScript díky nové genre vybranou položku.</span><span class="sxs-lookup"><span data-stu-id="0fd78-113">JavaScript makes the new genre the selected item.</span></span>

 <span data-ttu-id="0fd78-114">Na obrázku níže **Opera** byla přidána do databáze a vybrán v **Genre** rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="0fd78-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="0fd78-115">Otevřete *Views\StoreManager\Create.cshtml* a nahraďte tento kód genre následující kód:</span><span class="sxs-lookup"><span data-stu-id="0fd78-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="0fd78-116">`_ChooseGenre` Částečné zobrazení bude obsahovat všechnu logiku spojit JavaScript a použít k implementaci přidat novou funkci genre jQuery.</span><span class="sxs-lookup"><span data-stu-id="0fd78-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="0fd78-117">Když jsme dokončili kód bude snadno provést totéž s umělcem uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0fd78-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="0fd78-118">V Průzkumníku řešení klikněte pravým tlačítkem *Views\StoreManager* složky a vyberte **přidat**, pak **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="0fd78-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="0fd78-119">V **název zobrazení** vstup, zadejte `_ChooseGenre` vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0fd78-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="0fd78-120">Nahraďte kód v *Views\StoreManager\\_ChooseGenre.cshtml* soubor s následující:</span><span class="sxs-lookup"><span data-stu-id="0fd78-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="0fd78-121">První řádek deklaruje, že jsme se předává v `Album` jako naše model stejně příkaz Najít v zobrazení pro vytváření modelu.</span><span class="sxs-lookup"><span data-stu-id="0fd78-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="0fd78-122">Další několika řádků jsou **popisek** značek pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="0fd78-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="0fd78-123">Další řádek je **rozevírací seznam** volání pomocné rutiny, stejně jako původní zobrazení pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="0fd78-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="0fd78-124">Na další řádek přidá odkaz s názvem `Add New Genre`, a styly jako tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0fd78-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="0fd78-125">Řádek obsahující `ValidationMessageFor` zkopíruje přímo ze zobrazení pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="0fd78-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="0fd78-126">Následující řádky:</span><span class="sxs-lookup"><span data-stu-id="0fd78-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="0fd78-127">Vytvoří skrytá div, s ID `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="0fd78-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="0fd78-128">Použijeme jQuery spojit naše **přidat Genre** dialogové okno s ID `genreDialog` v této div.</span><span class="sxs-lookup"><span data-stu-id="0fd78-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="0fd78-129">Posledních dvou značek skriptu obsahují odkazy na soubory JavaScript, které budeme používat k implementaci přidat novou funkci genre.</span><span class="sxs-lookup"><span data-stu-id="0fd78-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="0fd78-130">*/Scripts/chooseGenre.js* soubor je zadaný pro vás v projektu, vyzkoušíme ho později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0fd78-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="0fd78-131">Spusťte aplikaci a klikněte na **přidat nové Genre** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0fd78-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="0fd78-132">V **přidat Genre** dialogovém okně zadejte **Opera** v **název** vstupního pole.</span><span class="sxs-lookup"><span data-stu-id="0fd78-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="0fd78-133">Klikněte **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0fd78-133">Click the **Save** button.</span></span> <span data-ttu-id="0fd78-134">Volání AJAX vytvoří kategorie Opera a pak naplní rozevírací seznam s prohlížeči Opera a nastaví Opera jako vybrané genre.</span><span class="sxs-lookup"><span data-stu-id="0fd78-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="0fd78-135">Zadejte umělcem, název a ceny a pak vyberte **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0fd78-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="0fd78-136">Pokud zadáte cena menší než $8.99, zobrazí se nové album v horní části zobrazení indexu.</span><span class="sxs-lookup"><span data-stu-id="0fd78-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="0fd78-137">Ověřte, že nový záznam album byla uložena v databázi.</span><span class="sxs-lookup"><span data-stu-id="0fd78-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="0fd78-138">Zkuste vytvořit novou genre s pouze jedním písmenem.</span><span class="sxs-lookup"><span data-stu-id="0fd78-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="0fd78-139">Následující kód na *Models\Genre.cs* souboru Nastaví minimální a maximální délka názvu genre.</span><span class="sxs-lookup"><span data-stu-id="0fd78-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="0fd78-140">Ověřování na straně klienta se hlásí, že je třeba zadat řetězec mezi 2 a nejvíce 20 znaků.</span><span class="sxs-lookup"><span data-stu-id="0fd78-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="0fd78-141">Zkoumání jak nové Genre se přidá do databáze a seznamu Select.</span><span class="sxs-lookup"><span data-stu-id="0fd78-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="0fd78-142">Otevřete *Scripts\chooseGenre.js* soubor a zkontrolujte kód.</span><span class="sxs-lookup"><span data-stu-id="0fd78-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="0fd78-143">Druhý řádek používá ID `genreDialog` vytvořit dialogové okno na značce div v *Views\StoreManager\\_ChooseGenre.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="0fd78-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="0fd78-144">Většina pojmenované parametry jsou vysvětlující sám sebou.</span><span class="sxs-lookup"><span data-stu-id="0fd78-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="0fd78-145">`autoOpen` Parametr je nastaven na hodnotu false, vyberete **vytvořit Genre** tlačítko otevře dialog explicitně (to je popsaný v pozdější).</span><span class="sxs-lookup"><span data-stu-id="0fd78-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="0fd78-146">Dialogové okno má dvě tlačítka **Uložit** a **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="0fd78-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="0fd78-147">**Zrušit** tlačítko zavře dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0fd78-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="0fd78-148">Následující kód ukazuje **Uložit** tlačítko funkce.</span><span class="sxs-lookup"><span data-stu-id="0fd78-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="0fd78-149">`var createGenreForm` Je vybraný `createGenreForm` ID.</span><span class="sxs-lookup"><span data-stu-id="0fd78-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="0fd78-150">`createGenreForm` ID byla nastavena v následujícím kódu v nalezen *Views\Genre\\_CreateGenre.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="0fd78-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="0fd78-151">[Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd492714.aspx) pomocná přetížení použít v *Views\Genre\\_CreateGenre.cshtml* generuje soubor HTML s atributem akce, který obsahuje adresu URL pro odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="0fd78-151">The [Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="0fd78-152">Tím zobrazíte tak zobrazení stránky album vytvořit v prohlížeči a vyberte zdroj zobrazit v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="0fd78-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="0fd78-153">Následující kód ukazuje generovaný kód HTML obsahující značku formuláře.</span><span class="sxs-lookup"><span data-stu-id="0fd78-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="0fd78-154">JQuery `$.post` řádku provede volání AJAX do atributu akce (`/StoreManager/Create`) a předá data z **vytvořit Genre** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0fd78-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="0fd78-155">Data se skládá z názvu pro nové genre a volitelný popis.</span><span class="sxs-lookup"><span data-stu-id="0fd78-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="0fd78-156">Pokud je úspěšné volání AJAX, nový genre název a hodnotu jsou přidány do vyberte značek a nové genre nastavena na vybrané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0fd78-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="0fd78-157">Protože je to dynamicky generovaných značek, nezobrazí nové vyberte možnost zobrazením zdroje v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="0fd78-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="0fd78-158">Zobrazí se nové HTML pomocí nástrojů pro vývojáře aplikace Internet Explorer 9 F12.</span><span class="sxs-lookup"><span data-stu-id="0fd78-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="0fd78-159">Pokud chcete zobrazit nové vyberte možnost, v aplikaci Internet Explorer 9, stisknutí klávesy F12 spuštění nástrojů pro vývojáře F12.</span><span class="sxs-lookup"><span data-stu-id="0fd78-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="0fd78-160">Přejděte na stránku vytvořit a přidat nové genre, aby nové genre je vybrán v seznamu select genre.</span><span class="sxs-lookup"><span data-stu-id="0fd78-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="0fd78-161">V nástrojích pro vývojáře F12:</span><span class="sxs-lookup"><span data-stu-id="0fd78-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="0fd78-162">Vyberte kartu HTML.</span><span class="sxs-lookup"><span data-stu-id="0fd78-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="0fd78-163">Stiskněte tlačítko ikonu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="0fd78-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="0fd78-164">Do vyhledávacího pole zadejte GenreID.</span><span class="sxs-lookup"><span data-stu-id="0fd78-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="0fd78-165">Pomocí další ikony</span><span class="sxs-lookup"><span data-stu-id="0fd78-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
 <span data-ttu-id="0fd78-166">přejděte ke značce vyberte následující:</span><span class="sxs-lookup"><span data-stu-id="0fd78-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="0fd78-167">Rozbalte hodnotu poslední možnosti.</span><span class="sxs-lookup"><span data-stu-id="0fd78-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="0fd78-168">Následující kód v *Scripts\chooseGenre.js* souboru ukazuje **přidat nové Genre** tlačítko získá připojené k klikněte na události a jak **přidat nové Genre** dialogové okno je vytvořit.</span><span class="sxs-lookup"><span data-stu-id="0fd78-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="0fd78-169">První řádek vytvoří funkci klikněte na připojené k **přidat nové Genre** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0fd78-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="0fd78-170">Následující kód z Views\StoreManager\\_ChooseGenre.cshtml souboru ukazuje jak **přidat nové Genre** tlačítko vytvořeno:</span><span class="sxs-lookup"><span data-stu-id="0fd78-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="0fd78-171">Metoda load vytvoří a otevře se dialogové okno Přidat Genre a volá jQuery `parse` metoda tak ověření klienta, proběhne dat zadané v dialogovém okně.</span><span class="sxs-lookup"><span data-stu-id="0fd78-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="0fd78-172">V této části jste se naučili, jak vytvořit dialogové okno, které je možné přidat nové kategorie dat do seznamu výběru.</span><span class="sxs-lookup"><span data-stu-id="0fd78-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="0fd78-173">Můžete postupujte stejným způsobem pro vytvoření uživatelského rozhraní pro přidání nové umělcem do seznamu příkazu select umělcem.</span><span class="sxs-lookup"><span data-stu-id="0fd78-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="0fd78-174">V tomto kurzu udělil přehled o práci s pomocné rutiny ASP.NET MVC HTML **rozevírací seznam**.</span><span class="sxs-lookup"><span data-stu-id="0fd78-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="0fd78-175">Další informace o práci s **rozevírací seznam**, najdete v části Další odkazy níže.</span><span class="sxs-lookup"><span data-stu-id="0fd78-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="0fd78-176">Dejte nám vědět, pokud v tomto kurzu byla užitečná.</span><span class="sxs-lookup"><span data-stu-id="0fd78-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="0fd78-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="0fd78-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="0fd78-178">Další odkazy</span><span class="sxs-lookup"><span data-stu-id="0fd78-178">Additional References</span></span>

- <span data-ttu-id="0fd78-179">[ASP.NET MVC – kaskádových rozevírací seznamy kurzu](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) podle [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="0fd78-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="0fd78-180">[Zvolený](http://harvesthq.github.com/chosen/) modulu plug-in jazyka JavaScript, která podporuje vícenásobného výběru a filtrování.</span><span class="sxs-lookup"><span data-stu-id="0fd78-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="0fd78-181">Contributors</span><span class="sxs-lookup"><span data-stu-id="0fd78-181">Contributors</span></span>

- [<span data-ttu-id="0fd78-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="0fd78-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="0fd78-183">Jean Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="0fd78-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="0fd78-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="0fd78-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="0fd78-185">Recenzenti</span><span class="sxs-lookup"><span data-stu-id="0fd78-185">Reviewers</span></span>

- <span data-ttu-id="0fd78-186">Jean Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="0fd78-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="0fd78-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="0fd78-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="0fd78-188">Jan Pope</span><span class="sxs-lookup"><span data-stu-id="0fd78-188">Mike Pope</span></span>
- <span data-ttu-id="0fd78-189">Tní Dykstra</span><span class="sxs-lookup"><span data-stu-id="0fd78-189">Tom Dykstra</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="0fd78-190">Předchozí</span><span class="sxs-lookup"><span data-stu-id="0fd78-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
