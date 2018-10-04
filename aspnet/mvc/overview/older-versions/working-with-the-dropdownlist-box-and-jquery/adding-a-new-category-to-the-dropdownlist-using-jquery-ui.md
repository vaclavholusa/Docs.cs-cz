---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Přidat novou kategorii DropDownList pomocí uživatelské rozhraní jQuery | Dokumentace Microsoftu
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 9fb95d22be473a4318520a391fa424106246a054
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576349"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="9ab17-102">Přidat novou kategorii DropDownList pomocí uživatelské rozhraní jQuery</span><span class="sxs-lookup"><span data-stu-id="9ab17-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="9ab17-103">Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="9ab17-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="9ab17-104">Kód HTML `Select` značka je ideální pro zobrazení seznamu pevné kategorie dat, ale často je potřeba přidat novou kategorii.</span><span class="sxs-lookup"><span data-stu-id="9ab17-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="9ab17-105">Předpokládejme, že chceme přidat žánr "Opera" do kategorie v naší databázi?</span><span class="sxs-lookup"><span data-stu-id="9ab17-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="9ab17-106">V této části budeme používat uživatelské rozhraní jQuery přidat dialogové okno, které můžete přidat novou kategorii.</span><span class="sxs-lookup"><span data-stu-id="9ab17-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="9ab17-107">Následující obrázek ukazuje, jak bude k dispozici uživatelské rozhraní v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="9ab17-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="9ab17-108">Když uživatel vybere **přidat nové žánr** odkaz, místní dialogové okno se zobrazí výzva pro nový název žánru (a volitelně jeho popis).</span><span class="sxs-lookup"><span data-stu-id="9ab17-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="9ab17-109">Na obrázku níže ukazují **přidat žánr** automaticky otevíraná okna.</span><span class="sxs-lookup"><span data-stu-id="9ab17-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="9ab17-110">Když se zadá nový název žánr a **Uložit** vloženo tlačítko se stane toto:</span><span class="sxs-lookup"><span data-stu-id="9ab17-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="9ab17-111">Volání AJAX odešle data na Vytvořit metodu řadičem Genre, který uloží nový žánr do databáze a vrátí nové informace žánru (žánr název a ID) jako dokumenty JSON.</span><span class="sxs-lookup"><span data-stu-id="9ab17-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="9ab17-112">JavaScript přidává nová data žánr do seznamu příkazu select.</span><span class="sxs-lookup"><span data-stu-id="9ab17-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="9ab17-113">JavaScript je nový žánr vybranou položku.</span><span class="sxs-lookup"><span data-stu-id="9ab17-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="9ab17-114">Na obrázku níže **Opera** byla přidána do databáze a vybrané v **žánr** rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="9ab17-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="9ab17-115">Otevřít *Views\StoreManager\Create.cshtml* souboru a nahraďte následující značky žánr následující kód:</span><span class="sxs-lookup"><span data-stu-id="9ab17-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="9ab17-116">`_ChooseGenre` Částečné zobrazení bude obsahovat veškerou logiku pro připojení jazyka JavaScript a jQuery používaný k implementaci novou funkci žánr přidat.</span><span class="sxs-lookup"><span data-stu-id="9ab17-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="9ab17-117">Když jsme dokončili kód bude snadno udělat stejným způsobem pracovat s interpreta uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9ab17-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="9ab17-118">V Průzkumníku řešení klikněte pravým tlačítkem *Views\StoreManager* a pak zvolte položku **přidat**, pak **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="9ab17-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="9ab17-119">V **název zobrazení** vstup, zadejte `_ChooseGenre` vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9ab17-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="9ab17-120">Nahraďte kód v *Views\StoreManager\\_ChooseGenre.cshtml* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="9ab17-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="9ab17-121">První řádek deklaruje, že jsme v prochází `Album` jako náš model přesně stejný příkaz v zobrazení pro vytváření modelu.</span><span class="sxs-lookup"><span data-stu-id="9ab17-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="9ab17-122">Následujících několik řádků jsou **popisek** pomocné rutiny značky.</span><span class="sxs-lookup"><span data-stu-id="9ab17-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="9ab17-123">Další řádek je **DropDownList** volání pomocné rutiny, stejně jako v původním zobrazení pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="9ab17-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="9ab17-124">Další řádek přidá odkaz s názvem `Add New Genre`, a styly jako tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ab17-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="9ab17-125">Na řádek obsahující `ValidationMessageFor` je zkopírován přímo ze zobrazení pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="9ab17-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="9ab17-126">Následující řádky:</span><span class="sxs-lookup"><span data-stu-id="9ab17-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="9ab17-127">vytvoří skrytou div s ID `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="9ab17-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="9ab17-128">Budeme používat k připojení jQuery naše **přidat žánr** dialogové okno s ID `genreDialog` v tomto div.</span><span class="sxs-lookup"><span data-stu-id="9ab17-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="9ab17-129">Poslední dvě značky skriptu obsahují odkazy na soubory jazyka JavaScript, které budeme používat k implementaci přidat novou funkci žánr.</span><span class="sxs-lookup"><span data-stu-id="9ab17-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="9ab17-130">*/Scripts/chooseGenre.js* soubor je k dispozici pro vás v projektu, prozkoumáme ho později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="9ab17-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="9ab17-131">Spusťte aplikaci a klikněte na **přidat nové žánr** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ab17-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="9ab17-132">V **přidat žánr** dialogového okna zadejte **Opera** v **název** vstupního pole.</span><span class="sxs-lookup"><span data-stu-id="9ab17-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="9ab17-133">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ab17-133">Click the **Save** button.</span></span> <span data-ttu-id="9ab17-134">Volání AJAX vytvoří Opera kategorie a pak naplní rozevírací seznam s Opera a nastaví Opera jako vybrané žánr.</span><span class="sxs-lookup"><span data-stu-id="9ab17-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="9ab17-135">Zadejte interpreta, název a ceny a pak vyberte **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ab17-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="9ab17-136">Pokud zadáte cena menší než $8.99, zobrazí se nové album v horní části zobrazení indexu.</span><span class="sxs-lookup"><span data-stu-id="9ab17-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="9ab17-137">Ověřte, že nový záznam alba byla uložena v databázi.</span><span class="sxs-lookup"><span data-stu-id="9ab17-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="9ab17-138">Zkuste vytvořit novou žánr s pouze jedno písmeno.</span><span class="sxs-lookup"><span data-stu-id="9ab17-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="9ab17-139">Následující kód na *Models\Genre.cs* souboru Nastaví minimální a maximální délka názvu žánr.</span><span class="sxs-lookup"><span data-stu-id="9ab17-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="9ab17-140">Ověřování na straně klienta se hlásí, že musíte zadat řetězec dlouhý 2 až 20 znaků.</span><span class="sxs-lookup"><span data-stu-id="9ab17-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="9ab17-141">Zkoumání jak nové žánr se přidá do databáze a seznamu Select.</span><span class="sxs-lookup"><span data-stu-id="9ab17-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="9ab17-142">Otevřít *Scripts\chooseGenre.js* souboru a prozkoumejte kód.</span><span class="sxs-lookup"><span data-stu-id="9ab17-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="9ab17-143">Druhý řádek používá ID `genreDialog` k vytvoření dialogového okna na značky div v *Views\StoreManager\\_ChooseGenre.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="9ab17-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="9ab17-144">Většina pojmenované parametry jsou vlastní vysvětlující.</span><span class="sxs-lookup"><span data-stu-id="9ab17-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="9ab17-145">`autoOpen` Parametr je nastaven na hodnotu false, vyberete **vytvořit žánr** tlačítko otevře dialog explicitně (to je popsáno v druhém).</span><span class="sxs-lookup"><span data-stu-id="9ab17-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="9ab17-146">Dialogové okno má dvě tlačítka **Uložit** a **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="9ab17-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="9ab17-147">**Zrušit** tlačítko zavře dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ab17-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="9ab17-148">Následující kód ukazuje **Uložit** tlačítko funkce.</span><span class="sxs-lookup"><span data-stu-id="9ab17-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="9ab17-149">`var createGenreForm` Vybrán `createGenreForm` ID.</span><span class="sxs-lookup"><span data-stu-id="9ab17-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="9ab17-150">`createGenreForm` ID byla nastavena v následujícím kódu v *Views\Genre\\_CreateGenre.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="9ab17-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="9ab17-151">[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) přetížení pomocné rutiny používané *Views\Genre\\_CreateGenre.cshtml* generuje soubor HTML s atributem akce obsahující adresu URL pro odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="9ab17-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="9ab17-152">Uvidíte toto zobrazení stránky alba vytvořit v prohlížeči a výběrem zdroj zobrazit v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="9ab17-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="9ab17-153">Následující kód ukazuje generovaný kód jazyka HTML obsahující značku formuláře.</span><span class="sxs-lookup"><span data-stu-id="9ab17-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="9ab17-154">JQuery `$.post` řádek provede volání AJAX do atributu akce (`/StoreManager/Create`) a předá z data **vytvořit žánr** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ab17-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="9ab17-155">Data se skládá z názvu pro nové žánr a volitelně také popis.</span><span class="sxs-lookup"><span data-stu-id="9ab17-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="9ab17-156">Pokud je volání AJAX úspěšné, nový žánr název a hodnota se přidají do vyberte značky a nové žánr je nastavena na hodnotu vybrané.</span><span class="sxs-lookup"><span data-stu-id="9ab17-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="9ab17-157">Protože je dynamicky generovaných značek, neuvidí novou možnost vybrat zobrazením zdroje v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="9ab17-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="9ab17-158">Zobrazí se nové HTML pomocí vývojářských nástrojů F12 aplikace Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="9ab17-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="9ab17-159">Chcete-li zobrazit novou vyberte možnost v aplikaci Internet Explorer 9, stiskněte klávesu F12 spustit vývojářské nástroje F12.</span><span class="sxs-lookup"><span data-stu-id="9ab17-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="9ab17-160">Přejděte na stránku vytvořit a přidat nový žánr tak nové žánr je vybrán v seznamu vyberte žánr.</span><span class="sxs-lookup"><span data-stu-id="9ab17-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="9ab17-161">Ve vývojářské nástroje F12 pomáhají:</span><span class="sxs-lookup"><span data-stu-id="9ab17-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="9ab17-162">Vyberte kartu HTML.</span><span class="sxs-lookup"><span data-stu-id="9ab17-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="9ab17-163">Klikněte na ikonu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9ab17-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="9ab17-164">Do vyhledávacího pole zadejte GenreID.</span><span class="sxs-lookup"><span data-stu-id="9ab17-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="9ab17-165">Pomocí další ikony</span><span class="sxs-lookup"><span data-stu-id="9ab17-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="9ab17-166">přejděte na následující vyberte značku:</span><span class="sxs-lookup"><span data-stu-id="9ab17-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="9ab17-167">Rozbalte hodnotu poslední možnosti.</span><span class="sxs-lookup"><span data-stu-id="9ab17-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="9ab17-168">Následující kód na *Scripts\chooseGenre.js* souboru ukazuje **přidat nové žánr** získá tlačítko připojené k událost click a jak **přidat nové žánr** dialogové okno vytvořit.</span><span class="sxs-lookup"><span data-stu-id="9ab17-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="9ab17-169">První řádek vytvoří funkci klikněte na připojené k **přidat nové žánr** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ab17-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="9ab17-170">Následující kód z Views\StoreManager\\_ChooseGenre.cshtml soubor ukazuje jak **přidat nové žánr** tlačítko se vytvoří:</span><span class="sxs-lookup"><span data-stu-id="9ab17-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="9ab17-171">Metoda load vytvoří a otevře se dialogové okno Přidat žánr a volá jQuery `parse` metoda tak ověřování na straně klienta, proběhne údaje zadávané do dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="9ab17-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="9ab17-172">V této části jste se dozvěděli, jak vytvořit dialogové okno, které slouží k přidání nové kategorie dat do vybraného seznamu.</span><span class="sxs-lookup"><span data-stu-id="9ab17-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="9ab17-173">Provedením stejný postup k vytvoření uživatelského rozhraní pro přidání do seznamu příkazu select interpreta novou interpreta.</span><span class="sxs-lookup"><span data-stu-id="9ab17-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="9ab17-174">V tomto kurzu, má udělené Přehled práce s pomocné rutiny ASP.NET MVC HTML **DropDownList**.</span><span class="sxs-lookup"><span data-stu-id="9ab17-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="9ab17-175">Další informace o práci s **DropDownList**, naleznete v části Další odkazy níže.</span><span class="sxs-lookup"><span data-stu-id="9ab17-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="9ab17-176">Dejte nám vědět, pokud v tomto kurzu byla užitečná.</span><span class="sxs-lookup"><span data-stu-id="9ab17-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="9ab17-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="9ab17-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="9ab17-178">Další odkazy</span><span class="sxs-lookup"><span data-stu-id="9ab17-178">Additional References</span></span>

- <span data-ttu-id="9ab17-179">[ASP.NET MVC – kaskádové rozevírací seznam uvádí kurzu](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) podle [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="9ab17-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="9ab17-180">[Zvolená](http://harvesthq.github.com/chosen/) modulu plug-in jazyka JavaScript, které podporují vícenásobný výběr a filtrování.</span><span class="sxs-lookup"><span data-stu-id="9ab17-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="9ab17-181">Contributors</span><span class="sxs-lookup"><span data-stu-id="9ab17-181">Contributors</span></span>

- [<span data-ttu-id="9ab17-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="9ab17-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="9ab17-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="9ab17-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="9ab17-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="9ab17-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="9ab17-185">Revidující</span><span class="sxs-lookup"><span data-stu-id="9ab17-185">Reviewers</span></span>

- <span data-ttu-id="9ab17-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="9ab17-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="9ab17-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="9ab17-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="9ab17-188">Mike Pope</span><span class="sxs-lookup"><span data-stu-id="9ab17-188">Mike Pope</span></span>
- <span data-ttu-id="9ab17-189">Petr Dykstra</span><span class="sxs-lookup"><span data-stu-id="9ab17-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9ab17-190">Předchozí</span><span class="sxs-lookup"><span data-stu-id="9ab17-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
