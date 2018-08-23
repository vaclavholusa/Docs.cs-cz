---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: Na dvou stránkách (C#) filtrování záznamů Master/Detail | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu podíváme na tom, jak oddělit sestavy záznamů master/detail na dvou stránkách. Na stránce "hlavní" používáme k vykreslení seznam categ ovládacím prvkem Repeater...
ms.author: riande
ms.date: 10/30/2010
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 6d4f38201eaa25097dcba589f35506204d3f73a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752928"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Na dvou stránkách (C#) filtrování záznamů Master/Detail
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) nebo [stahovat PDF](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> V tomto kurzu podíváme na tom, jak oddělit sestavy záznamů master/detail na dvou stránkách. Na stránce "hlavní" používáme k vykreslení seznam kategorií, že při kliknutí na, bude uživatel přejít na stránku "details" kde DataList dvěma sloupci zobrazí tyto produkty, které patří do vybrané kategorie. ovládacím prvku Repeater.


## <a name="introduction"></a>Úvod

V [předchozím kurzu](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) jsme viděli, jak zobrazení záznamů master/detail sestav v jedné webové stránky s využitím DropDownLists zobrazíte "hlavní" záznamy a a v prvku DataList zobrazíte "details". Další běžné používaným pro záznamů master/detail sestavy je, aby hlavní záznamy na jedné webové stránce a na další podrobnosti. V předchozím [filtrování záznamů Master/Detail napříč dvěma stránkami](../masterdetail/master-detail-filtering-across-two-pages-cs.md) kurzu jsme se zaměřili na tento model pomocí GridView zobrazíte všechny dodavatele v systému. Tento prvek GridView zahrnuté HyperLinkField, který vykreslen jako odkaz na druhé stránce předávání podél `SupplierID` v řetězec dotazu. Na druhé stránce používá k vypsání tyto produkty poskytnuté dodavatelem vybrané GridView.

Tyto dvě stránky záznamů master/detail zprávy můžete udělat pomocí ovládacích prvků DataList a Repeater ovládací prvky také. Jediným rozdílem je, že prvku DataList ani opakovače poskytuje podporu pro ovládací prvek HyperLinkField. Místo toho jsme musíte přidat ovládací prvek webového hypertextový odkaz nebo element anchor HTML (`<a>`) v rámci ovládacího prvku `ItemTemplate`. Na hypertextový odkaz `NavigateUrl` vlastnost nebo ukotvení `href` atribut lze přizpůsobit pak pomocí deklarativní a programová přístupů.

V tomto kurzu se podíváme příklad, který obsahuje seznam kategorií v seznamu s odrážkami na jedné stránce pomocí prvku opakovače. Každá položka seznamu bude obsahovat název a popis, do kategorie s názvem kategorie, zobrazí jako odkaz na druhé stránce. Kliknutím na tento odkaz se whisk uživateli na druhé stránce, kde a v prvku DataList zobrazí tyto produkty, které patří do vybrané kategorie.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Krok 1: Zobrazení kategorií v seznamu s odrážkami

Prvním krokem při vytváření sestavy všech záznamů master/detail je začít tím, že zobrazení "hlavní" záznamů. Naše první úloha je proto zobrazení kategorií v "hlavní" stránky. Otevřít `CategoryListMaster.aspx` stránku `DataListRepeaterFiltering` složky, přidejte ovládací prvek Repeater a z inteligentních značek optimalizované pro přidání nového prvku ObjectDataSource. Nakonfigurujte nový prvek ObjectDataSource, aby ho přistupuje ke svým datům z `CategoriesBLL` třídy `GetCategories` – metoda (viz obrázek 1).


[![Konfigurace ObjectDataSource třídy CategoriesBLL GetCategories metody](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Obrázek 1**: Konfigurace ObjectDataSource k použití `CategoriesBLL` třídy `GetCategories` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))


Dále definujte Repeateru šablony tak, aby zobrazil každý název a popis kategorie jako položka v seznamu s odrážkami. Pojďme se dosud obávat o jednotlivých kategorií odkaz na stránku podrobností. Následuje ukázka deklarativní pro prvek Repeater a ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

S Tento kód dokončeno věnujte chvíli zobrazíte náš postup prostřednictvím prohlížeče. Jak znázorňuje obrázek 2 Opakovači se vykreslí jako seznam s odrážkami zobrazuje název a popis jednotlivých kategorií.


[![Každá kategorie se zobrazí jako položku seznamu s odrážkami](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Obrázek 2**: Každá kategorie se zobrazí jako položku seznamu s odrážkami ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Krok 2: Zapnutí název kategorie na odkaz na stránku podrobností

Pokud chcete, aby uživatel zobrazit informace "o" pro dané kategorie, potřebujeme přidat odkaz pro každý seznam s odrážkami položky, které při kliknutí na, bude trvat uživateli na druhé stránce (`ProductsForCategoryDetails.aspx`). Této druhé stránce se pak zobrazí produkty pro vybrané kategorie pomocí a v prvku DataList. Aby bylo možné určit kategorie došlo ke kliknutí na odkaz, jehož, musíme předat kliknutí na kategorii `CategoryID` na druhou stránku pomocí některé mechanismus. Nejjednodušší a nejjednodušší způsob, jak přenášet skalární data z jedné stránky na jiný je pomocí řetězce dotazu, které možnosti, kterou v tomto kurzu použijeme. Konkrétně se `ProductsForCategoryDetails.aspx` stránky bude očekávat vybrané *`categoryID`* hodnoty, které se budou předávat pole řetězce dotazu s názvem `CategoryID`. Například, chcete-li zobrazit produkty pro kategorie Nápoje, který má `CategoryID` 1, by uživatel navštívil `ProductsForCategoryDetails.aspx?CategoryID=1`.

Vytvoření hypertextového odkazu pro každou položku seznamu s odrážkami v Opakovači potřebujeme přidat ovládací prvek webového hypertextový odkaz nebo HTML anchor element (`<a>`) k `ItemTemplate`. Ve scénářích, kdy hypertextový odkaz zobrazí stejný pro každý řádek, bude stačit kterýkoliv přístup. Pro opakovače dávám přednost používání anchor element. Pokud chcete použít anchor element, aktualizace ItemTemplate Opakovači na:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Všimněte si, že `CategoryID` můžete přímo v rámci elementu anchor vloženy `href` atribut; však k tak být některé omezíte `href` hodnotu atributu s apostrofy (a Poznámka uvozovek), protože `Eval` – metoda v rámci `href` atribut vymezuje její řetězec (`"CategoryID"`) uvozovek. Ovládací prvek hypertextového odkazu webového Alternativně lze použít místo toho:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Poznámka: Jak statická část adresy URL, – `ProductsForCategoryDetails.aspx?CategoryID` – je připojen na výsledek `Eval("CategoryID")` přímo v rámci syntaxe vázání dat pomocí zřetězení řetězců.

Jednou z výhod použití ovládacího prvku hypertextový odkaz je, že jej lze přistupovat programově z prvku Repeater `ItemDataBound` obslužné rutiny události v případě potřeby. Například můžete chtít zobrazit název kategorie, jako text, nikoli jako odkaz pro kategorie se žádné související produkty. Tuto kontrolu je možné programově provádět v `ItemDataBound` obslužné rutiny události; pro kategorie bez související produkty, hypertextový odkaz na `NavigateUrl` vlastnost může být nastaven na prázdný řetězec, což by vedlo k nejmenuje určité kategorie vykreslení jako prostý text (nikoli jako odkaz). Vraťte se do [formátování ovládacích prvků DataList a Repeater na základě na Data](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) kurz pro další informace o formátování ovládacích prvků DataList a Repeater na obsah založený na programovou logiku prostřednictvím `ItemDataBound` obslužné rutiny události.

Pokud postupujete, můžete použít buď anchor element, nebo přístup ovládacího prvku hypertextový odkaz na stránce. Bez ohledu na přístup, při zobrazení stránky prostřednictvím prohlížeče, každý název kategorie má být vykreslen jako odkaz na `ProductsForCategoryDetails.aspx`a předejte příslušný `CategoryID` hodnoty (viz obrázek 3).


[![Názvy kategorií nyní může propojit k ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Obrázek 3**: The kategorie názvy nyní odkaz `ProductsForCategoryDetails.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Krok 3: Seznam produktů, které patří do vybrané kategorie.

S `CategoryListMaster.aspx` stránka dokončení jsme připraveni realizovat naši pozornost k implementaci na stránce "details" `ProductsForCategoryDetails.aspx`. Otevřete tuto stránku, a v prvku DataList přetáhněte z panelu nástrojů do návrháře a nastavte jeho `ID` vlastnost `ProductsInCategory`. V dalším kroku z inteligentních značek ovládacích prvcích DataList vybrat přidání nového prvku ObjectDataSource na stránku jeho pojmenování `ProductsInCategoryDataSource`. Nakonfigurujte ho tak, aby volá `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` metoda; nastavte rozevírací seznam na kartách INSERT, UPDATE a DELETE na (žádný).


[![Konfigurace ObjectDataSource třídy ProductsBLL GetProductsByCategoryID(categoryID) metody](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Obrázek 4**: Konfigurace ObjectDataSource k použití `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))


Vzhledem k tomu, `GetProductsByCategoryID(categoryID)` metoda přijímá jako vstupní parametr (*`categoryID`*), průvodce Zvolit zdroj dat nám nabízí příležitost k určení zdroje parametru. Nastavit zdroj parametru řetězce dotazu pomocí vlastnost QueryStringField `CategoryID`.


[![Použijte pole řetězce dotazu CategoryID jako zdroj parametru](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Obrázek 5**: použijte pole řetězce dotazu `CategoryID` jako zdroj parametru ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))


Jak jsme viděli v předchozích kurzech se po dokončení průvodce Zvolit zdroj dat, sada Visual Studio automaticky vytvoří `ItemTemplate` pro každý název datového pole a hodnota prvku DataList. Tato šablona nahraďte ten, který obsahuje pouze produkt název dodavatele a ceny. Nastavuje prvku DataList `RepeatColumns` vlastnost na 2. Po provedení těchto změn ovládacích prvků DataList a ObjectDataSource deklarativní by měl vypadat nějak takto:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

K zobrazení této stránky v akci, spusťte z `CategoryListMaster.aspx` stránce; potom klikněte na odkaz v seznamu s odrážkami kategorie. Tím přejdete na `ProductsForCategoryDetails.aspx`, předejte podél `CategoryID` prostřednictvím řetězec dotazu. `ProductsInCategoryDataSource` ObjectDataSource v `ProductsForCategoryDetails.aspx` pak získat pouze tyto produkty pro zadané kategorie, který se zobrazí v ovládacím prvku DataList, který se vykreslí dva produkty na každém řádku. Snímek obrazovky znázorňuje obrázek 6 `ProductsForCategoryDetails.aspx` při prohlížení nápoje.


[![Jsou zobrazeny nápoje, dvě na řádek](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Obrázek 6**: The nápoje jsou zobrazeny, dvě na řádek ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Krok 4: Zobrazení informací o kategorie na ProductsForCategoryDetails.aspx

Když uživatel klikne na kategorii v `CategoryListMaster.aspx`, přejdou `ProductsForCategoryDetails.aspx` a produktů, které patří do vybrané kategorie. Nicméně v `ProductsForCategoryDetails.aspx` neexistují žádné vizuální prvky, jaké kategorie nebyla vybrána. Uživatel, který má klikněte na tlačítko nápoje, ale náhodně kliknutí na produkty koření nemá žádnou možnost porozumění jejich chybu, když dosáhnou `ProductsForCategoryDetails.aspx`. Ke zmírnění potenciální potíže, můžeme zobrazit informace o vybrané kategorie – jeho název a popis, v horní části `ProductsForCategoryDetails.aspx` stránky.

Chcete-li to provést, přidejte FormView nad ovládacím prvkem Repeater v `ProductsForCategoryDetails.aspx`. Pak přidejte nový prvek ObjectDataSource do stránky z ovládacího prvku FormView inteligentní značky s názvem `CategoryDataSource` a nakonfigurujte ho na použití `CategoriesBLL` třídy `GetCategoryByCategoryID(categoryID)` metody.


[![Přístup k informacím o kategorii prostřednictvím metody GetCategoryByCategoryID(categoryID) CategoriesBLL třídy](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Obrázek 7**: přístup k informacím o kategorii prostřednictvím `CategoriesBLL` třídy `GetCategoryByCategoryID(categoryID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))


Stejně jako u `ProductsInCategoryDataSource` ObjectDataSource přidali v kroku 3 `CategoryDataSource`společnosti vyzve nám Průvodce konfigurací zdroje dat pro zdroj `GetCategoryByCategoryID(categoryID)` metodu vstupního parametru. Pomocí naprosto stejného nastavení jako předtím, nastavení zdroji parametru řetězce dotazu a hodnota vlastnost QueryStringField `CategoryID` (vrátit zpět k obrázek 5).

Po dokončení průvodce se sada Visual Studio automaticky vytvoří `ItemTemplate`, `EditItemTemplate`, a `InsertItemTemplate` pro FormView. Jelikož poskytujeme rozhraní jen pro čtení, můžete bez obav odstranit `EditItemTemplate` a `InsertItemTemplate`. Navíc libovolně přizpůsobit ovládacího prvku FormView `ItemTemplate`. Po odebrání nadbytečný šablon a přizpůsobení šablony ItemTemplate, FormView a ObjectDataSource deklarativní by měl vypadat nějak takto:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

Obrázek 8 ukazuje obrazovky při zobrazení této stránky prostřednictvím prohlížeče.

> [!NOTE]
> Kromě FormView, jsme také přidali ovládací prvek hypertextového odkazu výše FormView, který přenese uživatele zpět na seznam kategorií (`CategoryListMaster.aspx`). Teď můžete umístit tento odkaz jinde nebo ji vynechte úplně se vynechá.


[![Informace o kategoriích se nyní zobrazí v horní části stránky](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Obrázek 8**: informace o kategoriích se nyní zobrazí v horní části stránky ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Krok 5: Zobrazení zprávy, pokud žádné produkty patří do vybrané kategorie.

`CategoryListMaster.aspx` Stránka obsahuje seznam všech kategorií v systému, bez ohledu na to, zda existují související produkty. Pokud uživatel klikne na kategorie se žádné související produkty v prvku DataList `ProductsForCategoryDetails.aspx` se nevykreslí, jako zdroj dat nemá žádné položky. Jak jsme viděli v posledních kurzy, poskytuje prvku GridView `EmptyDataText` vlastnost, která slouží k určení textová zpráva se zobrazí, pokud neexistují žádné záznamy ve zdroji dat. Tato vlastnost nemá bohužel DataList ani opakovače.

Aby bylo možné zobrazit zprávu informující uživatele, že neexistují žádné odpovídající produkty pro vybrané kategorie, je potřeba přidat popisek ovládací prvek, na stránce jehož `Text` vlastnost je přiřazena zpráva zobrazená v případě, že neexistují žádné odpovídající produkty. Následně je potřeba prostřednictvím kódu programu nastavit jeho `Visible` nastavenou na Určuje, jestli obsahuje jakékoli položky v ovládacím prvku DataList.

Chcete-li to provést, začněte přidáním popisek pod ovládacím prvku DataList. Nastavte jeho `ID` vlastnost `NoProductsMessage` a jeho `Text` vlastnost "Nejsou žádné produkty pro vybranou kategorii..." V dalším kroku budeme potřebovat programově nastavit tento popisek `Visible` Určuje, jestli se všechna data vázané na základě vlastností `ProductsInCategory` DataList. Toto přiřazení musí být provedena po prvku DataList byl vázán data. GridView DetailsView a FormView, můžeme vytvořit obslužnou rutinu události pro ovládací prvek `DataBound` událost, která aktivuje se po dokončení datové vazby. Však nemá prvku DataList ani opakovače `DataBound` události, které jsou k dispozici.

V tomto konkrétním příkladu jsme toto označení může přiřadit `Visible` vlastnost `Page_Load` obslužná rutina události, protože data se mají přiřazený k prvku DataList před na stránce `Load` událostí. Tento přístup však nebude fungovat v tomto obecném případě jako data z ObjectDataSource může být vázaný na ovládacím prvku DataList dále v cyklu stránky. Například pokud jsou data zobrazená je založena na hodnotě v jiném ovládacím prvku, jako je při zobrazení záznamů master/detail sestavy pomocí DropDownList pro uložení "hlavní" záznamy, data nemusí znovu připojeno k ovládacímu prvku webových dat až do `PreRender` fází životní cyklus stránky.

Jedno řešení, která bude fungovat ve všech případech je přiřadit `Visible` vlastnost `False` v ovládacím prvku DataList `ItemDataBound` (nebo `ItemCreated`) obslužné rutiny události při vytváření vazby typ položky `Item` nebo `AlternatingItem`. V takovém případě víme, že není nejméně jeden datový položky ve zdroji dat a proto můžete skrýt `NoProductsMessage` popisek. Kromě této obslužné rutiny události také potřebujeme obslužnou rutinu události pro prvku DataList `DataBinding` událostí, kde jsme inicializovat popisku `Visible` vlastnost `True`. Protože `DataBinding` událost se aktivuje před `ItemDataBound` události, popisek na `Visible` bude zpočátku být nastavenou na `True`; Pokud neexistují žádné položky dat však bude nastavena `False`. Následující kód implementuje tuto logiku:

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Všechny kategorie v databázi Northwind jsou přidruženy k jedné nebo více produktů. K otestování této funkce, můžu jste ručně upravit databázi Northwind pro účely tohoto kurzu přiřazení všechny produkty, které jsou spojené s kategorií produktů (`CategoryID` = 7) ke kategorii ryby (`CategoryID` = 8). Můžete to provést z Průzkumníka serveru výběrem nový dotaz a použijte tento `UPDATE` – příkaz:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

Po aktualizaci databáze odpovídajícím způsobem se vrátit `CategoryListMaster.aspx` stránky a klikněte na odkaz produktu. Protože jsou už nějaké produkty patřící do kategorie produktu, měli byste vidět zprávu "Nejsou žádné produkty pro vybranou kategorii...", jak je znázorněno na obrázku 9.


[![Zobrazí se zpráva, pokud neexistují žádné produkty, který patří do vybrané kategorie](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Obrázek 9**: zpráva se zobrazí, pokud neexistují žádné produkty, který patří do vybrané kategorie ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))


## <a name="summary"></a>Souhrn

Při záznamů master/detail sestavy můžete zobrazit hlavního uzlu i podrobné záznamy na jednu stránku, v mnoha webů, rozdělené dvě webové stránky. V tomto kurzu jsme se podívali na tom, jak implementovat záznamů master/detail sestavy tak, že kategorie uvedené v seznamu s odrážkami pomocí Repeateru v "hlavní" webové stránky a související produkty uvedené na stránce "details". Jednotlivé položky seznamu na stránce předlohy webových obsahovala odkaz na stránku podrobností, která se předají na řádku `CategoryID` hodnotu.

Na stránce podrobností načítání těchto produktů pro zadaný dodavatele bylo dosaženo pomocí `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` metody. *`categoryID`* Byla zadána hodnota parametru deklarativně pomocí `CategoryID` hodnotu řetězce dotazu jako zdroj parametru. Také jsme se podívali na zobrazení podrobností o kategorii na stránce podrobností pomocí FormView a zobrazit zprávu, pokud nebyly žádné produkty, které patří do vybrané kategorie.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k...

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Zack Jones a Liz Shulok. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [další](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
