---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
title: "Filtrování napříč dvěma stránkami (VB) podrobností | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu podíváme na to, jak jednotlivé sestavy a podrobností mezi dvěma stránkami. Na stránce \"hlavní\" používáme k vykreslení seznam categ ovládacího prvku opakovače..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2010
ms.topic: article
ms.assetid: f1a1be2c-6fd9-4a09-916e-aa1b98d5cf17
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f43fa998b81800cb1a2b7796ebb3922fc1caeb8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Filtrování napříč dvěma stránkami (VB) podrobností
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_VB.exe) nebo [stáhnout PDF](master-detail-filtering-acess-two-pages-datalist-vb/_static/datatutorial34vb1.pdf)

> V tomto kurzu podíváme na to, jak jednotlivé sestavy a podrobností mezi dvěma stránkami. Na stránce "hlavní" používáme k vykreslení seznam kategorií, které po kliknutí na, bude trvat uživatele na stránku "Podrobnosti", kde DataList dva sloupce zobrazí tyto produkty, které patří do vybrané kategorie ovládacího prvku opakovače.


## <a name="introduction"></a>Úvod

V [filtrování a podrobností mezi dvěma stránkami](../masterdetail/master-detail-filtering-across-two-pages-vb.md) kurzu jsme se zaměřili na tento vzor pomocí GridView pro zobrazení všech dodavatelů v systému. Tato rutina GridView zahrnuté HyperLinkField, který se vykresluje jako odkaz na druhé stránce, předávání společně `SupplierID` v řetězci dotazu. Druhá stránka používá GridView seznam těchto produktů poskytnutých vybrané dodavatelem.

Tyto dvě stránky a podrobností sestavy můžete udělat pomocí DataList opakovače ovládací prvky a také. Jediným rozdílem je, DataList ani opakovače poskytuje podporu pro ovládací prvek HyperLinkField. Místo toho jsme musíte přidat ovládacího prvku hypertextový odkaz webové nebo element anchor HTML (`<a>`) v rámci ovládacího prvku `ItemTemplate`. Na hypertextový odkaz `NavigateUrl` vlastnost nebo s ukotvením `href` atribut pak jde přizpůsobit pomocí deklarativní nebo programové přístupy.

V tomto kurzu jsme budete prozkoumejte příklad, který obsahuje seznam kategorií v seznamu s odrážkami na jedné stránce pomocí prvku opakovače. Jednotlivé položky seznamu bude obsahovat název a popis, kategorie s kategorií název zobrazený jako odkaz na druhé stránce. Kliknutím na tento odkaz bude whisk uživatele na druhou stránku, kde DataList zobrazí tyto produkty, které patří do vybrané kategorie.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Krok 1: Zobrazení kategorií v seznamu s odrážkami

Prvním krokem při vytváření všech sestav a podrobností je zahájíte tím, že zobrazuje "hlavní" záznamy. Naše první úkolů je proto na stránce "hlavní" zobrazte kategorie. Otevřete `CategoryListMaster.aspx` stránku `DataListRepeaterFiltering` složku, přidání ovládacího prvku opakovače a ze inteligentní značky opt přidat nové ObjectDataSource. Nakonfigurujte novou ObjectDataSource tak, aby ho přistupuje ke svým datům z `CategoriesBLL` třídy `GetCategories` – metoda (viz obrázek 1).


[![Konfigurace ObjectDataSource lze pomocí metody GetCategories CategoriesBLL – třída](master-detail-filtering-acess-two-pages-datalist-vb/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image1.png)

**Obrázek 1**: Konfigurace ObjectDataSource pro použití `CategoriesBLL` třídy `GetCategories` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-vb/_static/image3.png))


V dalším kroku definujte šablony opakovače tak, že zobrazuje každou kategorii název a popis jako položku v seznamu s odrážkami. Pojďme ještě obávat o každou kategorii odkaz na stránku podrobností. Na obrázku je deklarativní opakovače a ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample1.aspx)]

S Tento kód dokončeno pozorně zobrazit naše průběh prostřednictvím prohlížeče. Jak je vidět na obrázku 2, opakovače vykreslí jako seznam s odrážkami zobrazuje název a popis každou kategorii.


[![Každá kategorie se zobrazí jako položku seznamu s odrážkami](master-detail-filtering-acess-two-pages-datalist-vb/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image4.png)

**Obrázek 2**: Každá kategorie se zobrazí jako položku seznamu s odrážkami ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-vb/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Krok 2: Zapnutí název kategorie na odkaz na stránku podrobností

Povolit uživatelům zobrazit informace o "Podrobnosti" pro danou kategorii, je potřeba přidat odkaz na každém s odrážkami seznamu položku, která, při kliknutí na, přenese uživatele na druhé stránce (`ProductsForCategoryDetails.aspx`). Této druhé stránce se pak zobrazí produkty pro vybrané kategorie pomocí DataList. Chcete-li zjistit, kategorie, jehož propojení označeného, musíme předat kliknutelnou kategorie `CategoryID` na druhou stránku prostřednictvím některé mechanismus. Nejjednodušší a nejjednodušší způsob přenosu skalární dat z jedné stránky na jiný je pomocí řetězce dotazu, což je možnosti, kterou použijeme v tomto kurzu. Konkrétně `ProductsForCategoryDetails.aspx` stránka bude očekávat vybraný  *`categoryID`*  hodnota, která má být předána pole řetězce dotazu s názvem `CategoryID`. Chcete-li například zobrazit produkty pro kategorii Nápoje, který má `CategoryID` 1, by uživatel navštíví `ProductsForCategoryDetails.aspx?CategoryID=1`.

Vytvoření hypertextového odkazu pro každou položku seznamu s odrážkami v Opakovači musíme buď přidání ovládacího prvku hypertextový odkaz webové nebo HTML anchor element (`<a>`) na `ItemTemplate`. Ve scénářích, kdy hypertextový odkaz zobrazí stejné pro každý řádek, postačí buď přístup. Pro opakovače raději nechci pomocí elementu anchor. Pomocí elementu anchor, aktualizujte opakovače ItemTemplate na:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample2.aspx)]

Všimněte si, že `CategoryID` může vložit přímo v rámci elementu anchor `href` atribut; však k tak být jistý, a tím vymezit `href` hodnotu atributu s apostrofy (a Poznámka uvozovek), protože `Eval` – metoda v rámci `href` atribut vymezuje jeho řetězce (`"CategoryID"`) v uvozovkách. Ovládacího prvku hypertextový odkaz webové Alternativně lze použít místo:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample3.aspx)]

Poznámka: jak statické část adresy URL, – `ProductsForCategoryDetails.aspx?CategoryID` – se připojí k výsledek `Eval("CategoryID")` přímo v rámci datové vazby syntaxe pomocí zřetězení řetězců.

Jednou z výhod použití ovládacího prvku hypertextový odkaz je, že ji prostřednictvím kódu programu přístupné opakovače `ItemDataBound` obslužné rutiny události v případě potřeby. Například můžete chtít zobrazit název kategorie, jako text a nikoli jako odkaz pro kategorie s žádné související produkty. Tuto kontrolu můžete v prostřednictvím kódu programu provést `ItemDataBound` obslužné rutiny události; pro kategorií bez přidružené produkty, hypertextový odkaz na `NavigateUrl` může být nastavena na prázdný řetězec, což by vedlo k tento název určité kategorie vykreslování jako prostý text (nikoli jako odkaz). Odkazovat zpět [formátování DataList a opakovače dat na základě při](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) kurzu Další informace o formátování DataList a opakovače na obsah na základě programové logiky prostřednictvím `ItemDataBound` obslužné rutiny události.

Pokud postupujete podle, klidně používat buď anchor element nebo přístup ovládacího prvku hypertextový odkaz v stránku. Bez ohledu na přístup, při zobrazení stránky prostřednictvím prohlížeče každý název kategorie má být vykreslen jako odkaz na `ProductsForCategoryDetails.aspx`, předejte do odpovídajících `CategoryID` hodnotu (viz obrázek 3).


[![Názvy kategorií nyní může propojit k ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-vb/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image7.png)

**Obrázek 3**: kategorii názvy nyní odkaz na `ProductsForCategoryDetails.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-vb/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Krok 3: Výpis produkty, které patří do vybrané kategorie

Pomocí `CategoryListMaster.aspx` stránka dokončení, je vše připraveno k vypnutí naše pozornost implementace stránce "Podrobnosti" `ProductsForCategoryDetails.aspx`. Otevřete tuto stránku, přetáhněte ji DataList z panelu nástrojů na návrháře a nastavit jeho `ID` vlastnost `ProductsInCategory`. V dalším kroku z inteligentních značek DataList vyberte Přidat nové ObjectDataSource na stránku pojmenování ho `ProductsInCategoryDataSource`. Nakonfigurujte ji tak, aby zavolá `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` metoda; nastavte rozevírací seznam seznamů v kartě INSERT, UPDATE a DELETE (None).


[![Konfigurace ObjectDataSource lze pomocí metody GetProductsByCategoryID(categoryID) ProductsBLL – třída](master-detail-filtering-acess-two-pages-datalist-vb/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image10.png)

**Obrázek 4**: Konfigurace ObjectDataSource pro použití `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-vb/_static/image12.png))


Vzhledem k tomu `GetProductsByCategoryID(categoryID)` metoda přijímá vstupní parametr (*`categoryID`*), průvodce Zvolit zdroj dat nám nabízí možnost zadejte pro parametr zdroj. Nastavení zdroje parametru do řetězce dotazu pomocí vlastnost QueryStringField `CategoryID`.


[![Použít jako zdroj pro parametr CategoryID pole řetězce dotazu](master-detail-filtering-acess-two-pages-datalist-vb/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image13.png)

**Obrázek 5**: pole řetězce dotazu použít `CategoryID` jako zdroj pro parametr ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-vb/_static/image15.png))


Jak jsme viděli v předchozí kurzy, po dokončení průvodce Zvolit zdroj dat, sada Visual Studio automaticky vytvoří `ItemTemplate` pro DataList, který uvádí každý název datového pole a hodnotu. Tato šablona nahraďte ten, který uvádí jenom produktu název dodavatele a ceny. Také nastavena DataList `RepeatColumns` vlastnost na hodnotu 2. Po provedení těchto změn DataList a na ObjectDataSource deklarativní by měl vypadat takto:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample4.aspx)]

Chcete-li zobrazit tuto stránku v akci, spusťte z `CategoryListMaster.aspx` stránce; potom klikněte na odkaz v seznamu s odrážkami kategorií. Tím přejdete na `ProductsForCategoryDetails.aspx`, předejte společně `CategoryID` prostřednictvím řetězec dotazu. `ProductsInCategoryDataSource` ObjectDataSource v `ProductsForCategoryDetails.aspx` se pak získat pouze tyto produkty pro zadaná kategorie a zobrazí je v DataList, která vykreslí dva produkty na řádek. Snímek obrazovky ukazuje obrázek 6 `ProductsForCategoryDetails.aspx` při prohlížení nápoje.


[![Do této skupiny nápoje se zobrazují, dvě na řádek](master-detail-filtering-acess-two-pages-datalist-vb/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image16.png)

**Obrázek 6**: se zobrazují nápoje, dvě na řádek ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-vb/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Krok 4: Zobrazení informace o kategoriích v ProductsForCategoryDetails.aspx

Když uživatel klikne na kategorii v `CategoryListMaster.aspx`, jsou přesměrováni na `ProductsForCategoryDetails.aspx` a zobrazí produkty, které patří do vybrané kategorie. Však `ProductsForCategoryDetails.aspx` neexistují žádné vizuální upozornění, jaké kategorie nebyla vybrána. Uživatel, který určená klikněte na tlačítko nápoje, ale omylem kliknutelnou přísady nemá žádnou možnost porozumění jejich chybu po uplynutí `ProductsForCategoryDetails.aspx`. Má zmírnit potenciální potíže, jsme můžete zobrazit informace o vybrané kategorie – její název a popis – v horní části `ProductsForCategoryDetails.aspx` stránky.

K tomu, přidejte FormView výše ovládacím prvku Repeater v `ProductsForCategoryDetails.aspx`. Dál přidejte nové ObjectDataSource na stránku ze třídy FormView inteligentní značky s názvem `CategoryDataSource` a nakonfigurujte ho na používání `CategoriesBLL` třídy `GetCategoryByCategoryID(categoryID)` metoda.


[![Přístup k informacím o kategorie prostřednictvím metody GetCategoryByCategoryID(categoryID) CategoriesBLL – třída](master-detail-filtering-acess-two-pages-datalist-vb/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image19.png)

**Obrázek 7**: přístup k informacím o kategorie prostřednictvím `CategoriesBLL` třídy `GetCategoryByCategoryID(categoryID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-vb/_static/image21.png))


Stejně jako u `ProductsInCategoryDataSource` ObjectDataSource přidali v kroku 3 `CategoryDataSource`na výzvy nám Průvodce konfigurací zdroje dat pro zdroj pro `GetCategoryByCategoryID(categoryID)` metoda vstupního parametru. Použít přesný stejné nastavení jako před, nastavení Zdroj parametru řetězce dotazu a vlastnost QueryStringField hodnota, která má `CategoryID` (odkazuje zpět na obrázku 5).

Po dokončení průvodce, Visual Studio automaticky vytvoří `ItemTemplate`, `EditItemTemplate`, a `InsertItemTemplate` pro FormView. Vzhledem k tomu, že poskytujeme rozhraní, které je jen pro čtení, můžete bez obav odstranit `EditItemTemplate` a `InsertItemTemplate`. Navíc Nebojte se přizpůsobení FormView `ItemTemplate`. Po odebrání nadbytečné šablony a přizpůsobení ItemTemplate, FormView a na ObjectDataSource deklarativní by měl vypadat takto:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample5.aspx)]

Obrázek 8 znázorňuje snímku obrazovky při zobrazení této stránky prostřednictvím prohlížeče.

> [!NOTE]
> Kromě FormView, I taky přidali ovládacího prvku hypertextový odkaz výše FormView, který přenese uživatele zpět na seznam kategorií (`CategoryListMaster.aspx`). Nebojte se, že tento odkaz umístit jinam nebo zcela vynechat.


[![Informace o kategoriích se teď zobrazí v horní části stránky](master-detail-filtering-acess-two-pages-datalist-vb/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image22.png)

**Obrázek 8**: informace o kategoriích se teď zobrazí v horní části stránky ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-vb/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Krok 5: Zobrazení zprávy, pokud žádné produkty patří do vybrané kategorie

`CategoryListMaster.aspx` Stránka obsahuje seznam všech kategorií v systému, bez ohledu na to zda existují související produkty. Pokud uživatel klikne na kategorii s žádné související produkty DataList v `ProductsForCategoryDetails.aspx` nesmí být vykreslen, jako svůj zdroj dat nebude mít žádné položky. Jak jsme viděli v posledních kurzy, GridView poskytuje `EmptyDataText` vlastnost, která slouží k zadání text zprávy zobrazí, pokud v jeho zdroje dat nejsou žádné záznamy. Tato vlastnost má bohužel DataList ani opakovače.

Aby bylo možné zobrazit zprávu informující uživatele, že nejsou žádné odpovídající produkty pro vybrané kategorie, je potřeba přidat štítek řízení na stránku jehož `Text` vlastnost je přiřazen zprávu, která se zobrazí v případě, že nejsou žádné odpovídající produkty. Pak je potřeba nastavit prostřednictvím kódu programu jeho `Visible` vlastnost založené na tom, jestli DataList obsahuje všechny položky.

K tomu, začněte přidáním štítek pod prvku DataList. Nastavte její `ID` vlastnost `NoProductsMessage` a jeho `Text` vlastnost "Nejsou žádné produkty pro vybrané kategorie..." V dalším kroku musíme programově nastavit tento popisek `Visible` vlastnost založené na tom, zda byl spojen všechna data `ProductsInCategory` DataList. Toto přiřazení musí být provedeny po data byla svázána se prvku DataList. Rutina GridView, DetailsView a FormView jsme může vytvořit obslužnou rutinu události pro ovládací prvek `DataBound` událost, která aktivuje se po dokončení datové vazby. Však má prvku DataList ani opakovače `DataBound` událostí, které jsou k dispozici.

V tomto konkrétním příkladu jsme můžete přiřadit jmenovky `Visible` vlastnost v `Page_Load` obslužné rutiny události, protože data se mají přiřazený k DataList před stránky `Load` událostí. Však tento postup nebude fungovat v případě obecné podle data z ObjectDataSource může být vázána na DataList později v průběhu životního cyklu stránky. Například pokud je zobrazená data založená na hodnotě v další ovládací prvek, jako je při zobrazení sestavy a podrobností pomocí rozevírací seznam pro uložení "hlavní" záznamy, data nemusí odrážejí na ovládací prvek webu dat, dokud `PreRender` dvoufázové instalace v životního cyklu stránky.

Jedno řešení, která bude fungovat pro všechny případy je přiřadit `Visible` vlastnost `False` v DataList `ItemDataBound` (nebo `ItemCreated`) obslužné rutiny události při vytváření vazby typem položky `Item` nebo `AlternatingItem`. V takovém případě abychom věděli, který je nejméně jeden datový položky ve zdroji dat a proto můžete skrýt `NoProductsMessage` popisek. Kromě této obslužné rutiny události také potřebujeme obslužné rutiny události pro DataList `DataBinding` událostí, kde jsme inicializovat jmenovky `Visible` vlastnost `True`. Vzhledem k tomu, `DataBinding` událost se aktivuje před `ItemDataBound` události, popisek na `Visible` vlastnost zpočátku bude nastavena pro `True`; Pokud jsou všechny položky, data, ale bude nastavena pro `False`. Následující kód implementuje tuto logiku:

[!code-vb[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample6.vb)]

Všechny kategorie v databázi Northwind jsou spojené s jeden nebo více produktů. K otestování této funkce, I jste ručně upraveno databázi Northwind v tomto kurzu přeřazení všechny produkty, které jsou přidružené k kategorii produktu (`CategoryID` = 7) do kategorie ryby (`CategoryID` = 8). Můžete to provést z Průzkumníka serveru zvolením nový dotaz a pomocí následujících `UPDATE` příkaz:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample7.sql)]

Po aktualizaci databáze odpovídajícím způsobem se vraťte ke `CategoryListMaster.aspx` stránky a klikněte na odkaz produktu. Vzhledem k tomu, že už existují jakékoli produkty, které patří do kategorie produktů, měli byste vidět "Nejsou žádné produkty pro vybrané kategorie..." zpráva, jak je znázorněno na obrázku 9.


[![Pokud neexistují žádné produkty patřící do vybrané kategorie se zobrazí zpráva](master-detail-filtering-acess-two-pages-datalist-vb/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image25.png)

**Obrázek 9**: Pokud neexistují žádné produkty patřící do vybrané kategorie se zobrazí zpráva A ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-acess-two-pages-datalist-vb/_static/image27.png))


## <a name="summary"></a>Souhrn

Při a podrobností sestavy můžete zobrazit hlavní i podrobné záznamy na jedné stránce, v mnoha webů budou odděleny mezi dvěma webové stránky. V tomto kurzu jsme se podívali na tom, jak implementovat a podrobností sestavu tak, že kategorií uvedených v seznamu s odrážkami pomocí prvku na webové stránce "hlavní" a související produkty uvedené na stránce "Podrobnosti". Jednotlivé položky seznamu v hlavní webové stránky obsahovala odkaz na stránku podrobností, které se předají na řádku `CategoryID` hodnotu.

Na stránce podrobností načítání těchto produktů pro zadaný dodavatele dosáhlo prostřednictvím `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` metoda.  *`categoryID`*  Byla zadána hodnota parametru deklarativně pomocí `CategoryID` hodnotu řetězce dotazu jako zdroj parametru. Také jsme se podívali na tom, jak zobrazit podrobnosti o kategorií na stránce Podrobnosti o použití FormView a jak zobrazit zprávu, pokud nebyly žádné produkty, které patří do vybrané kategorie.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování...

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Zack Petr a Liz Shulok. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
[další](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)
