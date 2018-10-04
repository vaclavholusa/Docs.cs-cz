---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Zkoumání, jak ASP.NET MVC vygeneruje uživatelské rozhraní pomocné rutiny DropDownList | Dokumentace Microsoftu
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 542790b7f475cc641ed26ff3187c25c25118e0ed
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577844"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Zkoumání, jak ASP.NET MVC vygeneruje uživatelské rozhraní pomocné rutiny DropDownList
====================
Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))

V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *řadiče* složku a pak vyberte **přidat kontroler**. Název kontroleru **StoreManagerController**. Nastavení možností pro **přidat kontroler** dialogového okna, jak je znázorněno na následujícím obrázku.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Upravit *StoreManager\Index.cshtml* zobrazení a odebrání `AlbumArtUrl`. Odebrání `AlbumArtUrl` se čitelnost prezentaci. Dokončený kód je uveden níže.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Otevřít *Controllers\StoreManagerController.cs* souborů a vyhledejte `Index` metody. Přidat `OrderBy` klauzule tak alba budou seřazeny podle ceny. Kompletní kód je uveden níže.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Řazení podle ceny vám usnadní testování změn do databáze. Při testování upravit a vytvořit metody, můžete tak uložených dat budou zobrazovat první nízkou cenu.

Otevřít *StoreManager\Edit.cshtml* souboru. Přidejte následující řádek bezprostředně po tagu legendy.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Následující kód ukazuje kontextu této změny:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId` Je potřeba provést změny na alba záznam.

Stisknutím kláves CTRL + F5 spusťte aplikaci. Kliknutím **správce** propojení a potom vyberte **vytvořit nový** odkaz pro vytvoření nové album. Ověřte, zda že byl uložen alba informace. Upravit alba a ověřte, zda jsou trvalé, provedené změny.

### <a name="the-album-schema"></a>Alba schématu

`StoreManager` Řadič vytvořené mechanismu generování uživatelského rozhraní MVC umožňuje přístup CRUD (vytváření, čtení, Update, Delete) ke alba v databázi music store. Schéma alba informace naleznete níže:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums` Tabulky neukládá žánr alba a popis, ukládá cizí klíč `Genres` tabulky. `Genres` Tabulka obsahuje žánr název a popis. Podobně `Albums` tabulka neobsahuje název alba umělcům, ale cizí klíč `Artists` tabulky. `Artists` Tabulka obsahuje jméno. Pokud prozkoumáte data v `Albums` tabulky, se zobrazí každý řádek obsahuje cizí klíč `Genres` tabulky a cizí klíč `Artists` tabulky. Na následujícím obrázku ukazují některé data tabulky z `Albums` tabulky.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Značka HTML Select

Kód HTML `<select>` – element (vytvořené pomocí jazyka HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) pomocné rutiny) umožňuje zobrazit úplný seznam hodnot (jako je například seznam žánry). Pro editační formuláře Pokud aktuální hodnota je známý, můžete seznamu výběru zobrazit aktuální hodnotu. Jsme viděli dříve tomto při nastavíme na vybranou hodnotu **komedie**. Seznam select je ideální pro zobrazení kategorií nebo data cizího klíče. `<select>` – Element pro cizí klíč žánr zobrazí seznam názvů možné genre, ale při ukládání formuláři vlastnost žánr se aktualizuje s rozšířením podle tematických hodnoty cizího klíče, nikoli název zobrazený žánr. Na následujícím obrázku je rozšířením podle tematických vybrané **Roz** a je umělce **Donnou léto**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Zkoumání rozhraní ASP.NET MVC automaticky generovaný kód

Otevřít *Controllers\StoreManagerController.cs* souborů a vyhledejte `HTTP GET Create` metody.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create` Metoda přidá dvě [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objektů `ViewBag`, jeden tak, aby obsahovala žánr informace a jeden obsahuje informace o interpreta. [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) přetížení konstruktoru využité nad má tři argumenty:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *položky*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) obsahující položky v seznamu. V příkladu výše, vrácený seznam žánry `db.Genres`.
2. *dataValueField*: název vlastnosti v **IEnumerable** seznam, který obsahuje hodnotu klíče. V příkladu výše `GenreId` a `ArtistId`.
3. *dataTextField*: název vlastnosti v **IEnumerable** seznam, který obsahuje informace k zobrazení. V vašim animátorům a rozšířením podle tematických tabulky `name` pole se používá.

Otevřít *Views\StoreManager\Create.cshtml* souboru a zkoumat `Html.DropDownList` pomocné rutiny značky žánr pole.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

První řádek ukazuje, že zobrazení pro vytváření trvá `Album` modelu. V `Create` metody uvedené výše, byl předán žádný model, takže získá zobrazení **null** `Album` modelu. V tomto okamžiku vytváříme nové album, takže nemáme žádné `Album` data pro něj.

[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) přetížení výše uvedené přebírá název pole, které chcete vytvořit vazbu modelu. Také používá tento název k vyhledání **objekt ViewBag** objekt obsahující [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objektu. Pomocí tohoto přetížení, je nutné provést název **objekt ViewBag SelectList** objekt `GenreId`. Druhý parametr (`String.Empty`) je text, který se zobrazí, když není vybrána žádná položka. To je přesně chceme při vytváření nové album. Je-li odebrat druhý parametr a použít následující kód:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

První prvek nebo Rock výchozí seznamu příkazu select v naší ukázce.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Zkoumání `HTTP POST Create` metody.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Toto přetížení `Create` přijímá metodu `album` objekt vytvořený pomocí systému vazby modelu ASP.NET MVC z hodnot formuláře, pošle. Při odeslání nové album, pokud je platný stav modelu a nejsou žádné chyby databáze, se přidá nový alba databáze. Vytvoření nové album naleznete na následujícím obrázku.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Můžete použít [nástroj fiddler](http://www.fiddler2.com/fiddler2/) ke kontrole hodnot odeslaného formuláře, že vazba modelu ASP.NET MVC používá k vytvoření objektu alba.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Refaktoring vytváření SelectList objekt ViewBag

Oba `Edit` metody a `HTTP POST Create` metoda mít stejný kód nastavit **SelectList** v **objekt ViewBag**. V duchu [suchého](http://en.wikipedia.org/wiki/Don't_repeat_yourself), jsme se tento kód Refaktorovat. Teď Uděláme to využívání Refaktorovat kód později.

Vytvoření nové metody pro přidání žánr a interpreta **SelectList** k **objekt ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Nahraďte dva řádky nastavení `ViewBag` ve všech `Create` a `Edit` pomocí volání metody `SetGenreArtistViewBag` metoda. Dokončený kód je uveden níže.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Vytvořte nové album a upravte alba k ověření, že změny fungovat.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Explicitně předávání SelectList DropDownList

Vytvoření a úprava zobrazení vytvořené pomocí generování uživatelského rozhraní ASP.NET MVC následující **DropDownList** přetížení:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList` Kód pro vytvoření zobrazení se zobrazuje níže.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Protože `ViewBag` vlastnost `SelectList` jmenuje `GenreId`, **DropDownList** pomocné rutiny použije `GenreId` **SelectList** v **objekt ViewBag** . V následujícím **DropDownList** přetížení, `SelectList` explicitně je předán.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Otevřít *Views\StoreManager\Edit.cshtml* soubor a změňte **DropDownList** volání explicitně předávat **SelectList**, pomocí přetížení výše. To lze proveďte pro kategorii Žánr. Dokončený kód je zobrazena níže:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Spusťte aplikaci a klikněte na tlačítko **správce** propojení, pak přejděte do alb Jazz a vyberte **upravit** odkaz.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Místo zobrazení Jazz jako aktuálně vybraný genre, zobrazí se Rock. Pokud argument řetězce (vlastnost pro vazbu) a **SelectList** objekt mají stejný název, vybrané hodnoty se nepoužívá. Pokud neexistuje žádná vybraná hodnota k dispozici, výchozí prohlížeče na první prvek v **SelectList**(což je **Rock** v předchozím příkladu). Toto je známé omezení **DropDownList** pomocné rutiny.

Otevřít *Controllers\StoreManagerController.cs* soubor a změňte **SelectList** názvy do objektů `Genres` a `Artists`. Dokončený kód je zobrazena níže:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Názvy žánry a umělci jsou lepší názvy kategorií, obsahují více než jen ID každé kategorie. Vyplatilo refaktoring, který jsme to udělali dříve. Místo změny **objekt ViewBag** v čtyři metody byly izolován na naše změny `SetGenreArtistViewBag` metoda.

Změnit **DropDownList** volání v části Vytvoření a úprava zobrazení s novým **SelectList** názvy. Nový kód pro zobrazení pro úpravy je zobrazena níže:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Vytvořit zobrazení vyžaduje zabránit se zobrazí první položku SelectList prázdný řetězec.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Vytvořte nové album a upravte alba k ověření, že změny fungovat. Testování kódu upravit tak, že vyberete alba s žánr než Rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Pomocí zobrazení modelu s pomocné rutiny DropDownList

Vytvořte novou třídu v modely ViewModels složku s názvem `AlbumSelectListViewModel`. Nahraďte kód v `AlbumSelectListViewModel` třídy následujícím kódem:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel` Konstruktor přijímá alba, seznam vašim animátorům a žánry a vytvoří objekt, který obsahuje alba a `SelectList` žánry a umělci.

Sestavte projekt proto `AlbumSelectListViewModel` je k dispozici, když budeme vytvářet zobrazení v dalším kroku.

Přidat `EditVM` metodu `StoreManagerController`. Dokončený kód je uveden níže.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Klikněte pravým tlačítkem myši `AlbumSelectListViewModel`vyberte **vyřešit**, pak **pomocí MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Alternativně můžete přidat následující příkaz using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Klikněte pravým tlačítkem myši `EditVM` a vyberte **přidat zobrazení**. Pomocí možnosti zobrazené níže.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Vyberte **přidat**, poté nahraďte obsah *Views\StoreManager\EditVM.cshtml* souboru následujícím kódem:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM` Značek je velmi podobný původní `Edit` značky s následujícími výjimkami.

- Vlastnosti v modelu `Edit` zobrazení jsou ve tvaru `model.property`(například `model.Title` ). Vlastnosti v modelu `EditVm` zobrazení jsou ve tvaru `model.Album.property`(například `model.Album.Title`). Důvodem je, že `EditVM` zobrazení se předá kontejner `Album`, nikoli `Album` stejně jako v `Edit` zobrazení.
- **DropDownList** druhý parametr pochází z modelu zobrazení není **objekt ViewBag**.
- **BeginForm** Pomocník `EditVM` zobrazení explicitně odeslat zpět `Edit` metody akce. Tím, že publikuje zpět `Edit` akce, nemáme pro zápis `HTTP POST EditVM` akce a můžete znovu použít `HTTP POST` `Edit` akce.

Spusťte aplikaci a upravit alba. Změňte adresu URL na použití `EditVM`. Změnit pole a klikněte **Uložit** tlačítko ověřit kód funguje.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Jaký přístup byste měli použít?

Všechny tři postupy uvedené jsou acceptible. Mnoho vývojářů raději explictily pass `SelectList` k `DropDownList` pomocí `ViewBag`. Tento přístup má výhodu současného vám poskytuje flexibilitu používat více vhodný název pro kolekci. Jeden výstrahou je nelze pojmenovat `ViewBag SelectList` stejný název jako vlastnost modelu objektu.

Někteří vývojáři radši ViewModel přístup. Ostatní vezměte v úvahu podrobnější značek a generovaný kód HTML přístupu ViewModel nevýhodu.

V této části jsme se naučili tři způsoby používání **DropDownList** s daty kategorie. V další části vám ukážeme, jak přidat novou kategorii.

> [!div class="step-by-step"]
> [Předchozí](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [další](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
