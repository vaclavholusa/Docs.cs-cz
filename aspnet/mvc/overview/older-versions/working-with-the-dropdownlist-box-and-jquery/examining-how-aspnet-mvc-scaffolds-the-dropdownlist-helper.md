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
ms.openlocfilehash: 737773ab424b3ec3b6139b8c238a60ca23de2e69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Prozkoumání, jak rozhraní ASP.NET MVC scaffolds pomocná rozevírací seznam
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složku a potom vyberte **přidat kontroler**. Název kontroleru **StoreManagerController**. Nastavení možností pro **přidat kontroler** dialogové okno, jak je znázorněno na obrázku níže.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Upravit *StoreManager\Index.cshtml* zobrazení a odebrání `AlbumArtUrl`. Odebrání `AlbumArtUrl` bude čitelnost prezentaci. Dokončený kód je uveden níže.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Otevřete *Controllers\StoreManagerController.cs* souboru a najděte `Index` metoda. Přidat `OrderBy` klauzule, alba seřadí podle ceny. Kód dokončení jsou uvedeny níže.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Řazení podle ceny budou usnadňují testování změny databáze. Pokud testujete úpravy a vytvoření metody, můžete tak uložená data se zobrazí první nízkou cenu.

Otevřete *StoreManager\Edit.cshtml* souboru. Přidejte následující řádek bezprostředně za značce legendy.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Následující kód ukazuje kontextu této změny:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId` Je potřeba provést změny k záznamu alb.

Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci. Vyberte k **správce** propojení a pak vyberte **vytvořit nový** odkaz pro vytvoření nové album. Ověřte, zda že byla uložena alba informace. Upravit alba a ověřte, zda jsou nastavené jako trvalé změny, které jste provedli.

### <a name="the-album-schema"></a>Schéma Album

`StoreManager` Kontroler vytvořený mechanismus generování uživatelského rozhraní MVC umožňuje CRUD (vytvořit, číst, Update, Delete) přístup k alba v databázi Hudba úložiště. Schéma album informace jsou uvedeny níže:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums` Tabulky neukládá genre alba a popis, ukládá cizí klíč k `Genres` tabulky. `Genres` Tabulka obsahuje genre název a popis. Podobně `Albums` tabulka neobsahuje název alba umělci, ale cizí klíč k `Artists` tabulky. `Artists` Tabulka obsahuje jméno. Pokud si projdete data v `Albums` tabulky, můžete zobrazit každý řádek obsahuje cizí klíč k `Genres` tabulky a cizí klíč `Artists` tabulky. Následující obrázek zobrazit některá data tabulky z `Albums` tabulky.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Značka HTML vyberte

HTML `<select>` – element (vytvořený HTML [rozevírací seznam](https://msdn.microsoft.com/library/dd492948.aspx) pomocné rutiny) se používá k zobrazení úplný seznam hodnot (například seznam žánry). Pro úpravy formulářů Pokud se aktuální hodnota označuje, můžete seznamu výběru zobrazit aktuální hodnota. Jsme viděli dříve tomto při Nastaví vybrané hodnoty **komedie**. Seznam select je ideální pro zobrazení kategorie nebo dat cizího klíče. `<select>` Element pro cizí klíč Genre zobrazí seznam názvů možné genre, ale při ukládání formuláře je vlastnost Genre aktualizovat hodnotou Genre cizího klíče, nikoli název zobrazený genre. Na následujícím obrázku je genre vybrané **Disco** a umělce **Donna letní**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Zkoumání rozhraní ASP.NET MVC vygeneroval kód

Otevřete *Controllers\StoreManagerController.cs* souboru a najděte `HTTP GET Create` metoda.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create` Metoda přidá dva [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objekty ke `ViewBag`, jeden, který bude obsahovat informace genre a jeden, který bude obsahovat umělcem informace. [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) přetížení konstruktoru používá výše má tři argumenty:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *položky*: [rozhraní IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) obsahující položky v seznamu. V příkladu nahoře, vrácený seznam žánry `db.Genres`.
2. *dataValueField*: název vlastnosti v **rozhraní IEnumerable** seznamu, který obsahuje hodnotu klíče. V příkladu nahoře `GenreId` a `ArtistId`.
3. *dataTextField*: název vlastnosti v **rozhraní IEnumerable** seznamu, který obsahuje informace k zobrazení. V umělci a tabulka genre `name` pole se používá.

Otevřete *Views\StoreManager\Create.cshtml* soubor a zkontrolujte `Html.DropDownList` kód pomocné rutiny pro genre pole.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

První řádek ukazuje, že zobrazení pro vytváření trvá `Album` modelu. V `Create` výše uvedená metoda, byl předán žádný model, tak získá zobrazení **null** `Album` modelu. V tomto okamžiku se vytváří nové album proto nemáme žádné `Album` data pro ni.

[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) přetížení výše uvedeném převezme název pole, které chcete vytvořit vazbu k modelu. Tento název se používá také k Hledat **ViewBag** objekt obsahující [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objektu. Pomocí této přetížení, je nutné na název **ViewBag SelectList** objekt `GenreId`. Druhý parametr (`String.Empty`) je text, který se zobrazí, pokud je vybrána žádná položka. Toto je přesně co chceme, při vytváření nové album. Je-li druhý parametr odebrat a použít následující kód:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

V seznamu vyberte výchozí první prvek nebo Rock ve výběru.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Zkoumání `HTTP POST Create` metoda.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Toto přetížení `Create` metoda trvá `album` objekt, vytvořený systémem vazby modelu ASP.NET MVC z formuláře hodnoty odeslány. Při odesílání nové album, pokud stav modelu, který je platný, a neexistují žádné chyby databáze, se přidá nové album databáze. Následující obrázek ukazuje vytvoření nové album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Můžete použít [nástroj fiddler](http://www.fiddler2.com/fiddler2/) prozkoumat hodnoty odeslaného formuláře vazbou modelu ASP.NET MVC používá k vytvoření objektu alb.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Refaktoring vytvoření ViewBag SelectList

Obě `Edit` metody a `HTTP POST Create` metoda mít identické kódu nastavit **SelectList** v **ViewBag**. Ve smyslu [SUCHÝCH](http://en.wikipedia.org/wiki/Don't_repeat_yourself), jsme se Refaktorovat tento kód. Vytočit použití tohoto rozdělili kód později.

Vytvoření nové metody pro přidání genre a umělcem **SelectList** k **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Nahraďte dva řádky nastavení `ViewBag` v každé z `Create` a `Edit` metody s volání `SetGenreArtistViewBag` metoda. Dokončený kód je uveden níže.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Vytvořit nového alba a upravit album k ověření, že změny fungovat.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Explicitně předávání SelectList rozevírací seznam

Vytvoření zobrazení vytvořit a upravit pomocí generování uživatelského rozhraní ASP.NET MVC následující **rozevírací seznam** přetížení:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList` Kód pro vytvoření zobrazení je uveden níže.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Protože `ViewBag` vlastnost pro `SelectList` jmenuje `GenreId`, **rozevírací seznam** pomocníka použijete `GenreId` **SelectList** v **ViewBag** . V následujícím **rozevírací seznam** přetížení, `SelectList` explicitně předán.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Otevřete *Views\StoreManager\Edit.cshtml* soubor a změňte **rozevírací seznam** volání explicitně předávat **SelectList**, pomocí výše uvedených přetížení. To lze proveďte pro Genre kategorii. Dokončený kód je zobrazena níže:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Spusťte aplikaci a klikněte na tlačítko **správce** odkaz, pak přejděte do alb Jazz a vyberte **upravit** odkaz.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Místo zobrazení Jazz jako aktuálně vybrané genre, zobrazí se Rock. Pokud argument řetězce (vlastnost pro vazbu) a **SelectList** objekt mají stejný název, vybrané hodnoty se nepoužívá. Pokud žádné vybrané hodnoty zadané, prohlížečů výchozí prvním elementem v **SelectList**(což je **Rock** v předchozím příkladu). Jedná se o známé omezení **rozevírací seznam** pomocné rutiny.

Otevřete *Controllers\StoreManagerController.cs* soubor a změňte **SelectList** objektu názvy `Genres` a `Artists`. Dokončený kód je zobrazena níže:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Názvy žánry a umělci jsou lepší názvy kategorií, obsahují více než jen ID každé kategorii. Refaktoring, kterou jsme provedli dříve placené. Místo změna **ViewBag** v čtyři metody byly izolované na našem změny `SetGenreArtistViewBag` metoda.

Změna **rozevírací seznam** volání v části Vytvoření a úprava zobrazení s novým **SelectList** názvy. Nový kód pro úpravy zobrazení je zobrazena níže:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Zobrazení pro vytváření vyžaduje k zabránění zobrazení první položky SelectList prázdný řetězec.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Vytvořit nového alba a upravit album k ověření, že změny fungovat. Testování kódu upravit tak, že vyberete album s genre než Rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Model zobrazení pomocí pomocníka rozevírací seznam

Vytvořte novou třídu ve složce ViewModels s názvem `AlbumSelectListViewModel`. Nahraďte kód v `AlbumSelectListViewModel` třídy následujícím kódem:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel` Konstruktor přebírá album, seznam umělci a žánry a vytvoří objekt obsahující alba a `SelectList` žánry a umělci.

Sestavení projektu proto `AlbumSelectListViewModel` je k dispozici při vytváření zobrazení v dalším kroku.

Přidat `EditVM` metodu `StoreManagerController`. Dokončený kód je uveden níže.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Klikněte pravým tlačítkem na `AlbumSelectListViewModel`, vyberte **vyřešit**, pak **pomocí MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Alternativně můžete přidat následující příkaz using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Klikněte pravým tlačítkem na `EditVM` a vyberte **přidat zobrazení**. Pomocí možností vidíte níže.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Vyberte **přidat**, potom můžete nahradit obsah *Views\StoreManager\EditVM.cshtml* soubor s následující:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM` Značek je velmi podobný původní `Edit` značek s následujícími výjimkami.

- Vlastnosti v modelu `Edit` zobrazení jsou ve formátu `model.property`(například `model.Title` ). Vlastnosti v modelu `EditVm` zobrazení jsou ve formátu `model.Album.property`(například `model.Album.Title`). Důvodem je, že `EditVM` zobrazení byla předána kontejner pro `Album`, nikoli `Album` jako v `Edit` zobrazení.
- **Rozevírací seznam** druhý parametr pochází z modelu zobrazení, není **ViewBag**.
- **BeginForm** Pomocník `EditVM` zobrazit explicitně příspěvky zpět `Edit` metody akce. Zveřejněním zpět do `Edit` akce, nemáme k zápisu `HTTP POST EditVM` akce a můžete opakovaně použít `HTTP POST` `Edit` akce.

Spusťte aplikaci a upravit album. Změňte adresu URL používat `EditVM`. Změňte pole a stiskněte tlačítko **Uložit** tlačítko ověřit kód funguje.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Jaký přístup mám použít?

Všechny tři přístupy, zobrazí se acceptible. Celá řada vývojářů přednost explictily průchodu `SelectList` k `DropDownList` pomocí `ViewBag`. Tento přístup má výhodu, která poskytuje flexibilní použití vhodnější název kolekce. Přímý přístup do jedné paměti je nelze pojmenovat `ViewBag SelectList` objektu stejný název jako vlastnost modelu.

Někteří vývojáři přednost ViewModel přístup. Ostatní zvažte více podrobné značek a vygenerovaný HTML ViewModel dosahují nevýhodou.

V této části jsme se dozvěděli, tři způsoby používání **rozevírací seznam** daty kategorie. V další části ukážeme, jak přidat novou kategorii.

>[!div class="step-by-step"]
[Předchozí](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[další](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
