---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Přidat novou kategorii DropDownList pomocí uživatelské rozhraní jQuery | Dokumentace Microsoftu
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: e2528b74b714a3f691b07ed2429b3fe9eb3c2074
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802523"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Přidat novou kategorii DropDownList pomocí uživatelské rozhraní jQuery
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

Kód HTML `Select` značka je ideální pro zobrazení seznamu pevné kategorie dat, ale často je potřeba přidat novou kategorii. Předpokládejme, že chceme přidat žánr "Opera" do kategorie v naší databázi? V této části budeme používat uživatelské rozhraní jQuery přidat dialogové okno, které můžete přidat novou kategorii. Následující obrázek ukazuje, jak bude k dispozici uživatelské rozhraní v prohlížeči.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Když uživatel vybere **přidat nové žánr** odkaz, místní dialogové okno se zobrazí výzva pro nový název žánru (a volitelně jeho popis). Na obrázku níže ukazují **přidat žánr** automaticky otevíraná okna.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Když se zadá nový název žánr a **Uložit** vloženo tlačítko se stane toto:

1. Volání AJAX odešle data na Vytvořit metodu řadičem Genre, který uloží nový žánr do databáze a vrátí nové informace žánru (žánr název a ID) jako dokumenty JSON.
2. JavaScript přidává nová data žánr do seznamu příkazu select.
3. JavaScript je nový žánr vybranou položku.

   Na obrázku níže **Opera** byla přidána do databáze a vybrané v **žánr** rozevírací seznam. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Otevřít *Views\StoreManager\Create.cshtml* souboru a nahraďte následující značky žánr následující kód:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre` Částečné zobrazení bude obsahovat veškerou logiku pro připojení jazyka JavaScript a jQuery používaný k implementaci novou funkci žánr přidat. Když jsme dokončili kód bude snadno udělat stejným způsobem pracovat s interpreta uživatelského rozhraní.

V Průzkumníku řešení klikněte pravým tlačítkem *Views\StoreManager* a pak zvolte položku **přidat**, pak **zobrazení**. V **název zobrazení** vstup, zadejte `_ChooseGenre` vyberte **přidat**. Nahraďte kód v *Views\StoreManager\\_ChooseGenre.cshtml* souboru následujícím kódem:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

První řádek deklaruje, že jsme v prochází `Album` jako náš model přesně stejný příkaz v zobrazení pro vytváření modelu. Následujících několik řádků jsou **popisek** pomocné rutiny značky. Další řádek je **DropDownList** volání pomocné rutiny, stejně jako v původním zobrazení pro vytváření. Další řádek přidá odkaz s názvem `Add New Genre`, a styly jako tlačítko. Na řádek obsahující `ValidationMessageFor` je zkopírován přímo ze zobrazení pro vytváření. Následující řádky:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

vytvoří skrytou div s ID `genreDialog`. Budeme používat k připojení jQuery naše **přidat žánr** dialogové okno s ID `genreDialog` v tomto div. Poslední dvě značky skriptu obsahují odkazy na soubory jazyka JavaScript, které budeme používat k implementaci přidat novou funkci žánr. */Scripts/chooseGenre.js* soubor je k dispozici pro vás v projektu, prozkoumáme ho později v tomto kurzu.

Spusťte aplikaci a klikněte na **přidat nové žánr** tlačítko. V **přidat žánr** dialogového okna zadejte **Opera** v **název** vstupního pole.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Klikněte na tlačítko **Uložit** tlačítko. Volání AJAX vytvoří Opera kategorie a pak naplní rozevírací seznam s Opera a nastaví Opera jako vybrané žánr.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Zadejte interpreta, název a ceny a pak vyberte **vytvořit** tlačítko. Pokud zadáte cena menší než $8.99, zobrazí se nové album v horní části zobrazení indexu. Ověřte, že nový záznam alba byla uložena v databázi.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Zkuste vytvořit novou žánr s pouze jedno písmeno. Následující kód na *Models\Genre.cs* souboru Nastaví minimální a maximální délka názvu žánr.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Ověřování na straně klienta se hlásí, že musíte zadat řetězec dlouhý 2 až 20 znaků.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Zkoumání jak nové žánr se přidá do databáze a seznamu Select.

Otevřít *Scripts\chooseGenre.js* souboru a prozkoumejte kód.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

Druhý řádek používá ID `genreDialog` k vytvoření dialogového okna na značky div v *Views\StoreManager\\_ChooseGenre.cshtml* souboru. Většina pojmenované parametry jsou vlastní vysvětlující. `autoOpen` Parametr je nastaven na hodnotu false, vyberete **vytvořit žánr** tlačítko otevře dialog explicitně (to je popsáno v druhém). Dialogové okno má dvě tlačítka **Uložit** a **zrušit**. **Zrušit** tlačítko zavře dialogové okno. Následující kód ukazuje **Uložit** tlačítko funkce.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm` Vybrán `createGenreForm` ID. `createGenreForm` ID byla nastavena v následujícím kódu v *Views\Genre\\_CreateGenre.cshtml* souboru.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) přetížení pomocné rutiny používané *Views\Genre\\_CreateGenre.cshtml* generuje soubor HTML s atributem akce obsahující adresu URL pro odeslání formuláře. Uvidíte toto zobrazení stránky alba vytvořit v prohlížeči a výběrem zdroj zobrazit v prohlížeči. Následující kód ukazuje generovaný kód jazyka HTML obsahující značku formuláře.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery `$.post` řádek provede volání AJAX do atributu akce (`/StoreManager/Create`) a předá z data **vytvořit žánr** dialogové okno. Data se skládá z názvu pro nové žánr a volitelně také popis. Pokud je volání AJAX úspěšné, nový žánr název a hodnota se přidají do vyberte značky a nové žánr je nastavena na hodnotu vybrané. Protože je dynamicky generovaných značek, neuvidí novou možnost vybrat zobrazením zdroje v prohlížeči. Zobrazí se nové HTML pomocí vývojářských nástrojů F12 aplikace Internet Explorer 9. Chcete-li zobrazit novou vyberte možnost v aplikaci Internet Explorer 9, stiskněte klávesu F12 spustit vývojářské nástroje F12. Přejděte na stránku vytvořit a přidat nový žánr tak nové žánr je vybrán v seznamu vyberte žánr. Ve vývojářské nástroje F12 pomáhají:

1. Vyberte kartu HTML.
2. Klikněte na ikonu aktualizace.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Do vyhledávacího pole zadejte GenreID.
4. Pomocí další ikony   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   přejděte na následující vyberte značku:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Rozbalte hodnotu poslední možnosti.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Následující kód na *Scripts\chooseGenre.js* souboru ukazuje **přidat nové žánr** získá tlačítko připojené k událost click a jak **přidat nové žánr** dialogové okno vytvořit.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

První řádek vytvoří funkci klikněte na připojené k **přidat nové žánr** tlačítko. Následující kód z Views\StoreManager\\_ChooseGenre.cshtml soubor ukazuje jak **přidat nové žánr** tlačítko se vytvoří:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Metoda load vytvoří a otevře se dialogové okno Přidat žánr a volá jQuery `parse` metoda tak ověřování na straně klienta, proběhne údaje zadávané do dialogového okna.

V této části jste se dozvěděli, jak vytvořit dialogové okno, které slouží k přidání nové kategorie dat do vybraného seznamu. Provedením stejný postup k vytvoření uživatelského rozhraní pro přidání do seznamu příkazu select interpreta novou interpreta. V tomto kurzu, má udělené Přehled práce s pomocné rutiny ASP.NET MVC HTML **DropDownList**. Další informace o práci s **DropDownList**, naleznete v části Další odkazy níže. Dejte nám vědět, pokud v tomto kurzu byla užitečná.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Další odkazy

- [ASP.NET MVC – kaskádové rozevírací seznam uvádí kurzu](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) podle [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Zvolená](http://harvesthq.github.com/chosen/) modulu plug-in jazyka JavaScript, které podporují vícenásobný výběr a filtrování.

### <a name="contributors"></a>Contributors

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Revidující

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike Pope
- Petr Dykstra

> [!div class="step-by-step"]
> [Předchozí](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
