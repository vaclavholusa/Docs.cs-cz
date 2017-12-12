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
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Přidat novou kategorii k rozevírací seznam pomocí uživatelského rozhraní jQuery
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

HTML `Select` značka je ideální pro prezentace seznamu pevné kategorie dat, ale často je nutné přidat novou kategorii. Předpokládejme, že nám chcete přidat do kategorie v naší databázi genre "Opera"? V této části se budeme používat jQuery UI přidat dialogové okno, které jsme můžete použít k přidání nové kategorie. Následující obrázek ukazuje, jak se v prohlížeči prezentovat uživatelského rozhraní.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Když uživatel vybere **přidat nové Genre** odkazu, který je automaticky otevírané okno dialogové okno zobrazí výzvu k zadání nový název genre (a volitelně jeho popis). Obrázek níže ukazují **přidat Genre** automaticky otevíraná okna.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Kdy je nový název genre zadáno a **Uložit** nabídnutých tlačítko těchto podmínek:

1. Volání AJAX odešle data, která mají metodu Create řadiče Genre, která uloží novou genre do databáze a vrátí nové informace genre (genre název a ID) jako JSON.
2. JavaScript přidává nová data genre do seznamu příkazu select.
3. JavaScript díky nové genre vybranou položku.

 Na obrázku níže **Opera** byla přidána do databáze a vybrán v **Genre** rozevíracím seznamu. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Otevřete *Views\StoreManager\Create.cshtml* a nahraďte tento kód genre následující kód:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre` Částečné zobrazení bude obsahovat všechnu logiku spojit JavaScript a použít k implementaci přidat novou funkci genre jQuery. Když jsme dokončili kód bude snadno provést totéž s umělcem uživatelského rozhraní.

V Průzkumníku řešení klikněte pravým tlačítkem *Views\StoreManager* složky a vyberte **přidat**, pak **zobrazení**. V **název zobrazení** vstup, zadejte `_ChooseGenre` vyberte **přidat**. Nahraďte kód v *Views\StoreManager\\_ChooseGenre.cshtml* soubor s následující:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

První řádek deklaruje, že jsme se předává v `Album` jako naše model stejně příkaz Najít v zobrazení pro vytváření modelu. Další několika řádků jsou **popisek** značek pomocné rutiny. Další řádek je **rozevírací seznam** volání pomocné rutiny, stejně jako původní zobrazení pro vytváření. Na další řádek přidá odkaz s názvem `Add New Genre`, a styly jako tlačítko. Řádek obsahující `ValidationMessageFor` zkopíruje přímo ze zobrazení pro vytváření. Následující řádky:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

Vytvoří skrytá div, s ID `genreDialog`. Použijeme jQuery spojit naše **přidat Genre** dialogové okno s ID `genreDialog` v této div. Posledních dvou značek skriptu obsahují odkazy na soubory JavaScript, které budeme používat k implementaci přidat novou funkci genre. */Scripts/chooseGenre.js* soubor je zadaný pro vás v projektu, vyzkoušíme ho později v tomto kurzu.

Spusťte aplikaci a klikněte na **přidat nové Genre** tlačítko. V **přidat Genre** dialogovém okně zadejte **Opera** v **název** vstupního pole.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Klikněte **Uložit** tlačítko. Volání AJAX vytvoří kategorie Opera a pak naplní rozevírací seznam s prohlížeči Opera a nastaví Opera jako vybrané genre.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Zadejte umělcem, název a ceny a pak vyberte **vytvořit** tlačítko. Pokud zadáte cena menší než $8.99, zobrazí se nové album v horní části zobrazení indexu. Ověřte, že nový záznam album byla uložena v databázi.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Zkuste vytvořit novou genre s pouze jedním písmenem. Následující kód na *Models\Genre.cs* souboru Nastaví minimální a maximální délka názvu genre.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Ověřování na straně klienta se hlásí, že je třeba zadat řetězec mezi 2 a nejvíce 20 znaků.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Zkoumání jak nové Genre se přidá do databáze a seznamu Select.

Otevřete *Scripts\chooseGenre.js* soubor a zkontrolujte kód.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

Druhý řádek používá ID `genreDialog` vytvořit dialogové okno na značce div v *Views\StoreManager\\_ChooseGenre.cshtml* souboru. Většina pojmenované parametry jsou vysvětlující sám sebou. `autoOpen` Parametr je nastaven na hodnotu false, vyberete **vytvořit Genre** tlačítko otevře dialog explicitně (to je popsaný v pozdější). Dialogové okno má dvě tlačítka **Uložit** a **zrušit**. **Zrušit** tlačítko zavře dialogové okno. Následující kód ukazuje **Uložit** tlačítko funkce.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm` Je vybraný `createGenreForm` ID. `createGenreForm` ID byla nastavena v následujícím kódu v nalezen *Views\Genre\\_CreateGenre.cshtml* souboru.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd492714.aspx) pomocná přetížení použít v *Views\Genre\\_CreateGenre.cshtml* generuje soubor HTML s atributem akce, který obsahuje adresu URL pro odeslání formuláře. Tím zobrazíte tak zobrazení stránky album vytvořit v prohlížeči a vyberte zdroj zobrazit v prohlížeči. Následující kód ukazuje generovaný kód HTML obsahující značku formuláře.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery `$.post` řádku provede volání AJAX do atributu akce (`/StoreManager/Create`) a předá data z **vytvořit Genre** dialogové okno. Data se skládá z názvu pro nové genre a volitelný popis. Pokud je úspěšné volání AJAX, nový genre název a hodnotu jsou přidány do vyberte značek a nové genre nastavena na vybrané hodnoty. Protože je to dynamicky generovaných značek, nezobrazí nové vyberte možnost zobrazením zdroje v prohlížeči. Zobrazí se nové HTML pomocí nástrojů pro vývojáře aplikace Internet Explorer 9 F12. Pokud chcete zobrazit nové vyberte možnost, v aplikaci Internet Explorer 9, stisknutí klávesy F12 spuštění nástrojů pro vývojáře F12. Přejděte na stránku vytvořit a přidat nové genre, aby nové genre je vybrán v seznamu select genre. V nástrojích pro vývojáře F12:

1. Vyberte kartu HTML.
2. Stiskněte tlačítko ikonu aktualizace.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Do vyhledávacího pole zadejte GenreID.
4. Pomocí další ikony   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
 přejděte ke značce vyberte následující:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Rozbalte hodnotu poslední možnosti.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Následující kód v *Scripts\chooseGenre.js* souboru ukazuje **přidat nové Genre** tlačítko získá připojené k klikněte na události a jak **přidat nové Genre** dialogové okno je vytvořit.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

První řádek vytvoří funkci klikněte na připojené k **přidat nové Genre** tlačítko. Následující kód z Views\StoreManager\\_ChooseGenre.cshtml souboru ukazuje jak **přidat nové Genre** tlačítko vytvořeno:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Metoda load vytvoří a otevře se dialogové okno Přidat Genre a volá jQuery `parse` metoda tak ověření klienta, proběhne dat zadané v dialogovém okně.

V této části jste se naučili, jak vytvořit dialogové okno, které je možné přidat nové kategorie dat do seznamu výběru. Můžete postupujte stejným způsobem pro vytvoření uživatelského rozhraní pro přidání nové umělcem do seznamu příkazu select umělcem. V tomto kurzu udělil přehled o práci s pomocné rutiny ASP.NET MVC HTML **rozevírací seznam**. Další informace o práci s **rozevírací seznam**, najdete v části Další odkazy níže. Dejte nám vědět, pokud v tomto kurzu byla užitečná.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Další odkazy

- [ASP.NET MVC – kaskádových rozevírací seznamy kurzu](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) podle [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Zvolený](http://harvesthq.github.com/chosen/) modulu plug-in jazyka JavaScript, která podporuje vícenásobného výběru a filtrování.

### <a name="contributors"></a>Contributors

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Recenzenti

- Jean Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Jan Pope
- Tní Dykstra

>[!div class="step-by-step"]
[Předchozí](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
