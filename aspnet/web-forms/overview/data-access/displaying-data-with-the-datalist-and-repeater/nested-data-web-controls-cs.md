---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: Vnořená Data webové ovládací prvky (C#) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu se podíváme na použití prvku Repeater vnořit do jiné opakovače. Příklady ilustruje způsob naplnění vnitřní opakovače obou d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 4957f555691efaeaafa5bcf92141e0bef1cb1de9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="nested-data-web-controls-c"></a>Ovládací prvky webového vnořená Data (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe) nebo [stáhnout PDF](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> V tomto kurzu se podíváme na použití prvku Repeater vnořit do jiné opakovače. Příklady ilustruje způsob naplnění vnitřní opakovače deklarativně i prostřednictvím kódu programu.


## <a name="introduction"></a>Úvod

Kromě statické HTML a syntaxe vazby dat šablony mohou také obsahovat ovládací prvky webového a uživatelské ovládací prvky. Tyto ovládací prvky webového může mít jejich vlastnosti přiřazené prostřednictvím syntaxe deklarativní, vazby dat, nebo je možné programově přistupovat v obslužné rutiny odpovídající událost na straně serveru.

Vložením ovládacích prvků v rámci šablon můžete přizpůsobit vzhled a uživatelské prostředí a v porovnání s. Například v [pomocí TemplateFields v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) kurz, jsme viděli, jak přizpůsobit zobrazení GridView s přidáním ovládacího prvku kalendář v TemplateField do zaměstnanec s přijetím datum; ve [přidání Ověřovací ovládací prvky pro úpravy a vkládání rozhraní](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) a [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) kurzy, jsme viděli postup přizpůsobení, úpravu a vkládání rozhraní přidáním ověření ovládací prvky, textových polí, DropDownLists a jiných webových ovládacích prvků.

Šablony může také obsahovat další data ovládací prvky webového. To znamená, že jsme může mít DataList, který obsahuje jiné DataList (nebo opakovače nebo GridView nebo DetailsView a tak dále) v rámci jeho šablony. Problém s takového rozhraní se navazuje příslušná data vnitřních dat ovládací prvek webu. Nejsou k dispozici od deklarativní možnosti pomocí ObjectDataSource chcete programový několik různý přístup.

V tomto kurzu se podíváme na použití prvku Repeater vnořit do jiné opakovače. Vnější opakovače bude obsahovat položky pro každou kategorii v databázi, zobrazení kategorie s název a popis. Každá položka kategorie s vnitřní opakovače se zobrazí informace pro jednotlivé produkty z této kategorie (viz obrázek 1) v seznamu s odrážkami. Příklady ilustruje způsob naplnění vnitřní opakovače deklarativně i prostřednictvím kódu programu.


[![Každou kategorii, společně s jeho produktů, jsou uvedeny.](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**Obrázek 1**: každou kategorii, společně s jeho produktů, jsou uvedeny ([Kliknutím zobrazit obrázek v plné velikosti](nested-data-web-controls-cs/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Krok 1: Vytvoření seznamu Kategorie

Při vytváření stránky, která používá vnořené ovládací prvky webového data, I být užitečné při návrhu, vytvoření a testování ovládací prvek webu nejkrajnější dat nejprve bez obav i o informacích o vnitřní vnořené řízení. Proto umožní s začněte tím, že s návodem kroky potřebné k přidání prvku Repeater na stránku, který zobrazí název a popis pro každou kategorii.

Začněte otevřením `NestedControls.aspx` stránku `DataListRepeaterBasics` složky a přidejte na stránku nastavení ovládacího prvku opakovače jeho `ID` vlastnost, která má `CategoryList`. Z opakovače s inteligentním, můžete vytvořit nové ObjectDataSource s názvem `CategoriesDataSource`.


[![Název nové ObjectDataSource CategoriesDataSource](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**Obrázek 2**: název nové ObjectDataSource `CategoriesDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](nested-data-web-controls-cs/_static/image6.png))


Nakonfigurovat ObjectDataSource tak, aby ho získává jeho data ze `CategoriesBLL` třídu s `GetCategories` metoda.


[![Konfigurace ObjectDataSource lze pomocí této metody GetCategories CategoriesBLL třídu s](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**Obrázek 3**: Konfigurace ObjectDataSource pro použití `CategoriesBLL` třídu s `GetCategories` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](nested-data-web-controls-cs/_static/image9.png))


Chcete-li určit šablonu opakovače s obsahu musíme přejděte do zobrazení zdroje a ručně zadejte deklarativní syntaxi. Přidat `ItemTemplate` který zobrazí název kategorie s v `<h4>` elementu a popis kategorie s v element odstavce (`<p>`). Navíc umožňují s oddělit každou kategorii vodorovné pravítko (`<hr>`). Po provedení těchto změn vaše stránka by měla obsahovat deklarativní syntaxe pro opakovače a ObjectDataSource, který je podobný následujícímu:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

Obrázek 4 ukazuje naše průběh při zobrazení prostřednictvím prohlížeče.


[![Každou kategorii s název a popis je uvedena, oddělených vodorovné pravítko](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**Obrázek 4**: s každou kategorii název a popis je uvedena, oddělených vodorovné pravítko ([Kliknutím zobrazit obrázek v plné velikosti](nested-data-web-controls-cs/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Krok 2: Přidání opakovače vnořené produktu

V kategorii výpis dokončení, je přidání opakovače k naše další úlohy `CategoryList` s `ItemTemplate` který zobrazí informace o těchto produktů, které patří k příslušné kategorii. Existuje několik způsobů můžeme načíst data pro tento vnitřní opakovače dvě z nich jsme budete prozkoumat za chvíli. Teď, umožňují s jenom vytvářet produkty opakovače v rámci `CategoryList` opakovače s `ItemTemplate`. Konkrétně umožní s mít produktu opakovače zobrazení každý produkt v seznamu s odrážkami s jednotlivými položka seznamu včetně názvu produktu s a ceny.

Chcete-li vytvořit tuto opakovače, je potřeba zadat ručně vnitřní opakovače s deklarativní syntaxe a šablony do `CategoryList` s `ItemTemplate`. Přidejte následující kód v rámci `CategoryList` opakovače s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Krok 3: Vytvoření vazby podle kategorií produktů opakovače ProductsByCategoryList

Pokud v tomto okamžiku můžete navštívit stránku prostřednictvím prohlížeče, obrazovce bude vypadají stejně jako na obrázku 4 protože jsme jste zatím k vytvoření vazby. všechna data opakovače. Existuje několik způsobů, můžeme získat záznamy příslušnému produktu a navázat je opakovače, některé efektivnější než jiné. Hlavní těžkostí je moct vrátit vhodné produkty pro zadané kategorii.

Data pro vazbu k ovládacímu prvku opakovače vnitřní se dá buď dostat deklarativně, ObjectDataSource v `CategoryList` opakovače s `ItemTemplate`, nebo prostřednictvím kódu programu, ze stránky ASP.NET stránky s kódem v pozadí. Podobně, tato data mohou být vázány na vnitřní opakovače buď deklarativně - prostřednictvím vnitřní opakovače s `DataSourceID` vlastnost nebo prostřednictvím syntaxe deklarativní vazby dat, nebo prostřednictvím kódu programu pod položkou vnitřní opakovače v `CategoryList` opakovače s `ItemDataBound` obslužné rutiny události prostřednictvím kódu programu nastavení jeho `DataSource` vlastnost a volání jeho `DataBind()` metoda. Umožní s prozkoumat jednotlivých přístupů.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Přístup k datům deklarativně pomocí ovládacího prvku ObjectDataSource a`ItemDataBound`obslužné rutiny události

Protože jsme použít sunout ObjectDataSource hojně v rámci tohoto kurzu řady nejvíce přirozené volba pro přístup k datům v tomto příkladu je přilepit s ObjectDataSource. `ProductsBLL` Třída má `GetProductsByCategoryID(categoryID)` metodu, která vrátí informace o těchto produktů, které patří do zadané *`categoryID`*. Proto jsme můžete přidat ObjectDataSource k `CategoryList` opakovače s `ItemTemplate` a nakonfigurujte ho pro přístup k jeho data z této třídy s metody.

Bohužel t nemá opakovače povolí jeho šablony k provádění úprav prostřednictvím zobrazení návrhu, takže je potřeba ručně přidat deklarativní syntaxe tohoto ovládacího prvku ObjectDataSource. Následující syntaxe ukazuje `CategoryList` opakovače s `ItemTemplate` po přidání tohoto nového ObjectDataSource (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

Při použití ObjectDataSource přístup, je potřeba nastavit `ProductsByCategoryList` opakovače s `DataSourceID` vlastnost, která má `ID` prvku ObjectDataSource (`ProductsByCategoryDataSource`). Navíc Všimněte si, že je naše ObjectDataSource `<asp:Parameter>` element, který určuje *`categoryID`* hodnotu, která se předají do `GetProductsByCategoryID(categoryID)` metoda. Ale jak jsme tuto hodnotu zadat? V ideálním případě jsme d moct stačí nastavit `DefaultValue` vlastnost `<asp:Parameter>` elementu s použitím syntaxe vazby dat, například takto:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

Bohužel Syntaxe vazby dat je platná pouze v ovládacích prvcích, které mají `DataBinding` událostí. `Parameter` Třída chybí takové události, a proto výše uvedené syntaxe je neplatný a výsledkem bude chyba za běhu.

Pokud chcete nastavit tuto hodnotu, je potřeba vytvořit obslužnou rutinu události pro `CategoryList` opakovače s `ItemDataBound` událostí. Odvolat, který `ItemDataBound` událost aktivuje jednou pro každou položku vázána na opakovače. Proto pokaždé, když tato událost aktivuje pro vnější opakovače jsme můžete přiřadit aktuální `CategoryID` hodnotu `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` parametr.

Vytvoření obslužné rutiny událostí `CategoryList` opakovače s `ItemDataBound` událostí s následujícím kódem:


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

Spustí se zajištěním jsme re práci daty položky místo položky záhlaví, zápatí nebo oddělovač této obslužné rutiny události. V dalším kroku jsme odkazovat na skutečnou `CategoriesRow` instanci, která právě bylo svázáno se aktuální `RepeaterItem`. Nakonec jsme odkazovat ObjectDataSource v `ItemTemplate` a přiřaďte jeho `CategoryID` hodnotu parametru `CategoryID` aktuálního `RepeaterItem`.

Pomocí této obslužné rutiny události `ProductsByCategoryList` opakovače v každé `RepeaterItem` je vázána na tyto produkty ve `RepeaterItem` s kategorie. Obrázek 5 ukazuje snímek obrazovky výsledný výstup.


[![Vnější opakovače uvádí každou kategorii; Vnitřní jeden seznamy produktů, pro danou kategorii](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**Obrázek 5**: vnější opakovače uvádí každou kategorii; jeden seznamy vnitřní produkty pro danou kategorii ([Kliknutím zobrazit obrázek v plné velikosti](nested-data-web-controls-cs/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Přístup k produkty podle kategorie dat prostřednictvím kódu programu

Místo použití ObjectDataSource načíst produktů pro aktuální kategorii, jsme může vytvoření metody ve třídě kódu stránky s naše ASP.NET (nebo v `App_Code` složku nebo v samostatné projektu knihovny tříd), který vrací příslušné sady produkty při předaná `CategoryID`. Představte si jsme měli tato metoda v Naše třída kódu stránky s ASP.NET a že se s názvem `GetProductsInCategory(categoryID)`. Tato metoda zavedené jsme může produktů pro aktuální kategorii svázat s vnitřní opakovače pomocí deklarativní syntaxi:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

Opakovače s `DataSource` vlastnost se používá syntaxe datové vazby k označení, že jeho data pocházejí z `GetProductsInCategory(categoryID)` metoda. Vzhledem k tomu `Eval("CategoryID")` vrátí hodnotu typu `Object`, nemůžeme převést objekt, který má `Integer` před předáním do `GetProductsInCategory(categoryID)` metoda. Všimněte si, že `CategoryID` používaná tady prostřednictvím vazby dat je syntaxe `CategoryID` v *vnější* opakovače (`CategoryList`), že s vázána na záznamy v `Categories` tabulky. Proto jsme vědět, že `CategoryID` nelze databázi `NULL` hodnotu, kterou je důvod, proč může slepě přetypovat `Eval` metoda bez kontroly, pokud jsme re práci s `DBNull`.

S tímto přístupem, je potřeba vytvořit `GetProductsInCategory(categoryID)` metoda a mějte ho načíst příslušné sady produktů zadané zadaných *`categoryID`*. Jsme to můžete udělat jednoduše vrácení `ProductsDataTable` vrácený `ProductsBLL` třídu s `GetProductsByCategoryID(categoryID)` metoda. Umožní s vytvořit `GetProductsInCategory(categoryID)` metodu v třídě kódu pro naše `NestedControls.aspx` stránky. Učinit pomocí následujícího kódu:


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

Tato metoda jednoduše vytvoří instanci `ProductsBLL` metodu a vrátí výsledky `GetProductsByCategoryID(categoryID)` metoda. Všimněte si, že metoda musí být označen `Public` nebo `Protected`; Pokud je označena jako metodu `Private`, nebude dostupný z deklarativní kódu stránky s technologie ASP.NET.

Po provedení těchto změn pro použití této nové techniky, pozorně zobrazit stránku prostřednictvím prohlížeče. Výstup by měl být stejný jako výstup při použití ObjectDataSource a `ItemDataBound` přístup obslužné rutiny události (odkazuje zpět na obrázku 5 zobrazíte snímku obrazovky).

> [!NOTE]
> To může jevit jako busywork vytvořit `GetProductsInCategory(categoryID)` metodu v třídě kódu stránky s ASP.NET. Po všech, tato metoda jednoduše vytvoří instanci `ProductsBLL` třídy a vrátí výsledky z jeho `GetProductsByCategoryID(categoryID)` metoda. Proč právě tato metoda není volána přímo z Syntaxe datové vazby v informacích o vnitřní opakovače, jako je: `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`? I když tato syntaxe won t pracují s naše aktuální implementace `ProductsBLL` – třída (vzhledem k tomu, `GetProductsByCategoryID(categoryID)` metoda je metoda instance), může změnit `ProductsBLL` chcete zahrnout statický `GetProductsByCategoryID(categoryID)` metoda nebo mají třídu zahrnují statického `Instance()` metoda vrátí novou instanci třídy `ProductsBLL` třídy.


Když tyto změny by eliminovat potřebu `GetProductsInCategory(categoryID)` metodu v třídě kódu stránky s ASP.NET, metody třídy kódu nám poskytuje flexibilitu při práci s daty načíst, protože jsme krátce zobrazí.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Načítání všechny informace o produktu najednou

Dvě techniky zkontrolují jsme sunout zkontrolován získat tyto produkty pro aktuální kategorii tím, že zavoláte na `ProductsBLL` třídu s `GetProductsByCategoryID(categoryID)` – metoda (první přístup se tak prostřednictvím ObjectDataSource, druhý prostřednictvím `GetProductsInCategory(categoryID)` metoda v třída kódu). Pokaždé, když tato metoda je volána, volání vrstvu obchodní logiky dolů Data Access Layer, který dotazuje databázi s příkazem SQL, která vrací řádky z `Products` tabulky, jehož `CategoryID` pole odpovídá zadané vstupní parametr.

Zadané *N* kategorií v systému, tento přístup propojí *N* + 1 volání databáze jeden databázový dotaz získat všechny kategorie a potom *N* volání k získání produktů specifické pro jednotlivé kategorie. Můžeme však načíst všechna potřebná data v ještě dvě databáze volání jednoho volání získat všechny kategorie a jiné získat všechny produkty. Jakmile jsme všechny produkty, jsme tyto produkty filtrovat tak pouze produkty odpovídající aktuální `CategoryID` je vázána na této kategorie s vnitřní opakovače.

Abyste si mohli tuto funkci, musíme pouze provedete mírné změny `GetProductsInCategory(categoryID)` metodu v třídě kódu stránky s naše technologie ASP.NET. Místo slepě vrací výsledky `ProductsBLL` třídu s `GetProductsByCategoryID(categoryID)` metoda, můžeme místo toho nejdřív Přejít *všechny* produkty (Pokud se nebyla některá t byla získat přístup k již) a pak se vraťte právě filtrované zobrazení na předané na základě produktů `CategoryID`.


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

Poznámka: Přidání proměnné úrovni stránky `allProducts`. Ukládá informace o všech produktů a je vyplněný poprvé `GetProductsInCategory(categoryID)` metoda je volána. Po zajištění, že `allProducts` objekt byly vytvořeny a naplněny, metoda vyfiltruje výsledky s DataTable tak, aby pouze ty řádků, jehož `CategoryID` odpovídá zadané `CategoryID` jsou dostupné. Tento přístup snižuje počet databáze je k němu přistupovat z *N* + 1 dolů dva.

Toto vylepšení nezavádí všechny změny do vykreslované značky stránky, ani přináší záznamy zpět méně než ostatní přístup. Jednoduše snižuje počet volání do databáze.

> [!NOTE]
> Jeden může intuitivně důvodu, že snižuje počet přístupům do databáze by assuredly zlepšit výkon. To však nemusí být tento případ. Pokud máte velký počet produktů, jejichž `CategoryID` je `NULL`pro příklad, pak volání `GetProducts` metoda vrátí počet produktů, které se nikdy zobrazují. Vrací všechny produkty, lze plýtvání-li níž zobrazuje pouze podmnožinu kategorií, které může být v případě, pokud jste implementovali stránkování.


Vždy, když jde o Analýza výkonu dvě techniky, pouze surefire míry se řízené testy přizpůsobit pro vaše aplikace s běžných scénářích případů.

## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli jak lze vnořit jeden datový ovládací prvek webu v rámci druhého, konkrétně zkoumání jak vám má vnější opakovače položku pro každou kategorii zobrazení se vnitřní opakovače výpis produktů pro každou kategorii v seznamu s odrážkami. Hlavní těžkostí při vytváření vnořených uživatelské rozhraní je v přístupu k a správná data vytvoření vazby na ovládací prvek webu vnitřní data. Existuje mnoho různých technik k dispozici, dvě z nich jsme se zaměřili v tomto kurzu. Prvním přístupem zkontrolován použít ObjectDataSource ve vnější data ovládací prvek webu s `ItemTemplate` která byla vázána na ovládací prvek webu vnitřních dat prostřednictvím jeho `DataSourceID` vlastnost. Druhý způsob spočívá používaná data prostřednictvím metody ve třídě ASP.NET stránky s kódem v pozadí. Tato metoda může být pak vázaný na vnitřních dat ovládací prvek webu s `DataSource` vlastnosti prostřednictvím syntaxe datové vazby.

Při vnořené uživatelské rozhraní v tomto kurzu prvku Repeater vnořené v rámci prvku Repeater, tyto postupy lze rozšířit na jiné webové ovládací prvky pro data. Lze vnořit opakovače v rámci GridView nebo GridView v rámci DataList a tak dále.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Zack Petr a Liz Shulok. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
> [další](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
