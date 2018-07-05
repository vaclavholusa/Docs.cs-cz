---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: Vnořené dat webové ovládací prvky (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu se podíváme na tom, jak používat Repeateru vnořit do jiného opakovače. V příkladech se ukazují, jak naplnit obě d vnitřní Repeater...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f7980d22d6ebc15a033cca321644a2bf1d4e3bb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390022"
---
<a name="nested-data-web-controls-vb"></a>Webové ovládací prvky vnořených dat (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe) nebo [stahovat PDF](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> V tomto kurzu se podíváme na tom, jak používat Repeateru vnořit do jiného opakovače. V příkladech se ukazují, jak naplnit vnitřní Repeater deklarativně i prostřednictvím kódu programu.


## <a name="introduction"></a>Úvod

Kromě statický kód HTML a datové vazby syntaxe šablony mohou také obsahovat webové ovládací prvky a uživatelských ovládacích prvků. Tyto ovládací prvky webového může mít své vlastnosti přiřazena pomocí syntaxe deklarativní, vázání dat, nebo je možné programově přistupovat v obslužné rutině události na straně serveru.

S využitím vkládání ovládacích prvků v rámci šablony služby, je možné přizpůsobit vzhled vzhled a kvalitní ovládání a vylepšené při. Například v [použití vlastností TemplateField v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) kurzu jsme viděli, jak přizpůsobit zobrazení GridView s přidáním ovládacího prvku kalendáře v TemplateField zobrazíte zaměstnanec s přijetím datum; ve [přidání Validačních ovládacích prvků pro úpravy a vložení rozhraní](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) a [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurzy, jsme viděli přizpůsobení, úpravy a vložení rozhraní tak, že přidáte ověření ovládací prvky, textových polí, DropDownLists a jiných ovládacích prvků.

Šablony mohou obsahovat také další data webové ovládací prvky. To znamená můžeme nechat prvku DataList, který obsahuje jiné prvku DataList (nebo Repeater nebo GridView nebo prvku DetailsView a tak dále) v rámci jeho šablony. Výzvy se toto rozhraní je svázán příslušná data na vnitřní data webový ovládací prvek. Nejsou k dispozici od deklarativních možností, která pomocí prvku ObjectDataSource do programové těch několik různých přístupů.

V tomto kurzu se podíváme na tom, jak používat Repeateru vnořit do jiného opakovače. Vnější Repeater bude obsahovat položku pro každou kategorii v databázi, kategorie s název a popis zobrazení. Každá položka kategorie s vnitřní Repeater se zobrazí informace pro jednotlivé produkty, které patří do této kategorie (viz obrázek 1) v seznamu s odrážkami. Našich příkladů se ukazují, jak naplnit vnitřní Repeater deklarativně i prostřednictvím kódu programu.


[![Každou kategorii, společně s jeho produkty jsou uvedené.](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**Obrázek 1**: jsou uvedeny všechny kategorie, spolu s jeho produktů ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-data-web-controls-vb/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Krok 1: Vytvoření seznamu Kategorie

Při vytváření stránky, která používá vnořené webových ovládacích prvcích dat, mi užitečné návrhu, vytváření a testování ovládacího prvku webové nejkrajnější dat nejprve bez i byste se museli starat o vnitřní vnořené ovládací prvek. Proto umožní s začněte tím, že provede kroky potřebnými k přidání Repeateru na stránce, která se zobrazuje název a popis pro každou kategorii.

Začněte otevřením `NestedControls.aspx` stránku `DataListRepeaterBasics` složky a přidat na stránku nastavení ovládacím prvkem Repeater jeho `ID` vlastnost `CategoryList`. Z opakovače s inteligentním, můžete vytvořit nového prvku ObjectDataSource s názvem `CategoriesDataSource`.


[![Název nové CategoriesDataSource prvku ObjectDataSource](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**Obrázek 2**: název nového prvku ObjectDataSource `CategoriesDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-data-web-controls-vb/_static/image6.png))


Nakonfigurujte prvku ObjectDataSource tak, aby načítá data z `CategoriesBLL` třída s `GetCategories` metody.


[![Konfigurace ObjectDataSource metody GetCategories CategoriesBLL třída s](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**Obrázek 3**: Konfigurace ObjectDataSource k použití `CategoriesBLL` třída s `GetCategories` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-data-web-controls-vb/_static/image9.png))


Zadat šablonu opakovače s obsahu musíme přejít do zobrazení zdroje a deklarativní syntaxe lze zadat ručně. Přidat `ItemTemplate` , který zobrazuje kategorii s názvem v `<h4>` elementu a popis kategorie s v element odstavce (`<p>`). Kromě toho vám umožňují s oddělte každou kategorii vodorovná čára (`<hr>`). Po provedení těchto změn vaše stránka by měla obsahovat deklarativní syntaxe pro prvek Repeater a prvek ObjectDataSource, který je podobný následujícímu:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

Obrázek 4 ukazuje náš postup při prohlížení prostřednictvím prohlížeče.


[![Každá kategorie s název a popis je uveden, oddělené vodorovná čára](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**Obrázek 4**: s každou kategorii název a popis je uveden, oddělené vodorovná čára ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-data-web-controls-vb/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Krok 2: Přidání Repeater vnořené produktu

Kategorii úplný výpis naše dalším krokem je přidání Repeater na `CategoryList` s `ItemTemplate` , která zobrazí informace o těchto produktů, které patří do příslušné kategorie. Existuje mnoho způsobů, načteme data pro tento vnitřní Repeater, z nichž dva se podíváme za chvíli. Teď umožňují s stačí vytvořit Repeater produkty v rámci `CategoryList` opakovače s `ItemTemplate`. Konkrétně umožní s Repeater zobrazení každý produkt v seznamu s odrážkami s každým položky seznamu včetně produkt s názvem a cenu produktů.

Chcete-li vytvořit tento Repeater budeme muset ručně zadat vnitřní opakovače s deklarativní syntaxe a do šablony `CategoryList` s `ItemTemplate`. Přidejte následující kód v rámci `CategoryList` opakovače s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Krok 3: Vytvoření vazby podle kategorií produktů na ProductsByCategoryList Repeater

Pokud jste v tomto okamžiku naleznete na stránce prostřednictvím prohlížeče, obrazovce budou vypadat stejně jako obrázek 4 protože jsme ve ještě pro všechna data svázat opakovače. Existuje několik způsobů, že jsme vzít záznamy produkt a svázat ho s Opakovači některé efektivnější než jiné. Hlavní těžkostí zpátky prochází vhodné produkty pro zadané kategorie.

Data k vytvoření vazby na vnitřní ovládací prvek Repeater buď přístupné deklarativně, prostřednictvím ObjectDataSource v `CategoryList` opakovače s `ItemTemplate`, nebo prostřednictvím kódu programu, ze stránky ASP.NET stránky s kódem na pozadí. Podobně, tato data mohou být vázány na vnitřní Repeater buď deklarativně - prostřednictvím vnitřní opakovače s `DataSourceID` vlastnost nebo prostřednictvím datové vazby deklarativní syntaxe, nebo prostřednictvím kódu programu pomocí odkazu na vnitřní Repeater v `CategoryList` opakovače s `ItemDataBound` obslužná rutina události, programové nastavení jeho `DataSource` vlastnosti a volání jeho `DataBind()` metoda. Umožní s prozkoumejte každou z těchto přístupů.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Přístup k datům pomocí deklarace s ovládacím prvkem ObjectDataSource a`ItemDataBound`obslužné rutiny události

Protože jsme použít ve ObjectDataSource výrazně v celé této sérii kurzů se nejvíce přirozenou volbou pro přístup k datům pro tento příklad je se toho držet ovládacím prvkem ObjectDataSource. `ProductsBLL` Třída nemá `GetProductsByCategoryID(categoryID)` metodu, která vrací informace o těchto produktů, které patří do zadané *`categoryID`*. Proto jsme můžete přidat prvku ObjectDataSource k `CategoryList` opakovače s `ItemTemplate` a nakonfigurujte ho pro přístup k jeho datům z této metody třídy s.

Bohužel jeho šablonám se dá upravit v okně návrhu, je potřeba ručně přidat deklarativní syntaxe pro tento ovládací prvek ObjectDataSource povolit t kódu opakovače. Následující syntaxe zobrazuje `CategoryList` opakovače s `ItemTemplate` po přidání tohoto nového prvku ObjectDataSource (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

Při použití prvku ObjectDataSource přístup musíme nastavit `ProductsByCategoryList` opakovače s `DataSourceID` vlastnost `ID` z ObjectDataSource (`ProductsByCategoryDataSource`). Také, Všimněte si, že má naše ObjectDataSource `<asp:Parameter>` prvek, který určuje *`categoryID`* hodnotu, která se předají do `GetProductsByCategoryID(categoryID)` metoda. Ale jak jsme tuto hodnotu zadat? V ideálním případě d budeme moct stačí nastavit `DefaultValue` vlastnost `<asp:Parameter>` element pomocí syntaxe pro vázání dat, například:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

Bohužel syntaxe vázání dat je platná pouze v ovládacích prvcích, které mají `DataBinding` událostí. `Parameter` Třída nemá takových, a proto výše uvedené syntaxi je neplatný a výsledkem bude chyba za běhu.

Nastavení této hodnoty, musíme vytvořit obslužná rutina události `CategoryList` opakovače s `ItemDataBound` událostí. Vzpomeňte si, že `ItemDataBound` události spustí jednou pro každou položku vázán na opakovače. Proto se pokaždé, když tato událost aktivuje pro vnější Repeater My je můžeme přiřadit aktuální `CategoryID` hodnota, která se `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` parametru.

Vytvořte obslužnou rutinu události pro `CategoryList` opakovače s `ItemDataBound` události s následujícím kódem:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

Tato obslužná rutina události začíná tím, že zajišťuje, že jsme k práci s datovým položky místo položky záhlaví, zápatí nebo oddělovač. V dalším kroku budeme odkazovat na skutečné `CategoriesRow` instanci, která se právě byla svázána se aktuální `RepeaterItem`. Nakonec jsme odkazovat na prvek ObjectDataSource v `ItemTemplate` a přiřaďte jeho `CategoryID` pro parametr `CategoryID` aktuálního `RepeaterItem`.

Pomocí této obslužné rutiny události `ProductsByCategoryList` Repeater v každém `RepeaterItem` je vázán na tyto produkty v `RepeaterItem` s kategorií. Obrázek 5 ukazuje snímek obrazovky výsledný výstup.


[![Vnější Repeater uvádí každou kategorii; Vnitřní jeden seznam produktů pro tuto kategorii](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**Obrázek 5**: The vnější Repeater uvádí každou kategorii; jeden seznamy vnitřní produkty dané kategorie ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-data-web-controls-vb/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Přístup k produktů podle kategorie dat prostřednictvím kódu programu

Namísto použití ObjectDataSource načíst produkty pro aktuální kategorii, můžeme vytvořit metodu v třídě použití modelu code-behind stránky s naší technologie ASP.NET (nebo `App_Code` složky nebo v samostatném projektu knihovny tříd), která vrací příslušné sady produkty, když předaný `CategoryID`. Představte si, že jsme měli metody ve třídě použití modelu code-behind stránky s naší technologie ASP.NET a, že se s názvem `GetProductsInCategory(categoryID)`. Pomocí této metody na místě jsme může svázat s produkty pro aktuální kategorii vnitřní Repeater pomocí deklarativní syntaxe:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

Opakovače s `DataSource` vlastnost používá syntaxi datové vazby k označení, že svá data pochází z `GetProductsInCategory(categoryID)` metody. Protože `Eval("CategoryID")` vrátí hodnotu typu `Object`, jsme objekt přetypujte na `Integer` před předáním do `GetProductsInCategory(categoryID)` metoda. Všimněte si, že `CategoryID` používaná tady prostřednictvím vázání dat je syntaxe `CategoryID` v *vnější* Repeater (`CategoryList`), že s vázána na záznamy ve `Categories` tabulky. Proto víme, že `CategoryID` nemůže být databáze `NULL` hodnotu, což je důvod, proč jsme slepě přetypovat `Eval` metody bez kontroly, jestli můžeme znovu řešení `DBNull`.

S tímto přístupem, je nutné vytvořit `GetProductsInCategory(categoryID)` metoda a jeho načtení příslušné sady produktů vzhledem zadané *`categoryID`*. Můžeme to udělat tak, že jednoduše vrací `ProductsDataTable` vrácených `ProductsBLL` třída s `GetProductsByCategoryID(categoryID)` metody. Umožní s vytvořit `GetProductsInCategory(categoryID)` metody ve třídě použití modelu code-behind pro naše `NestedControls.aspx` stránky. Proveďte pomocí následujícího kódu:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

Tato metoda jednoduše vytvoří instanci `ProductsBLL` metodu a vrátí výsledky `GetProductsByCategoryID(categoryID)` metoda. Všimněte si, že metoda musí být označen `Public` nebo `Protected`; Pokud metoda je označena jako `Private`, nebude dostupný z deklarativním označení stránky s ASP.NET.

Po provedení těchto změn pro použití této nové techniky, věnujte chvíli zobrazení stránky prostřednictvím prohlížeče. Výstup by měl být stejný jako výstup, při použití ObjectDataSource a `ItemDataBound` přístup obslužné rutiny události (vrátit zpět na obrázku 5 zobrazíte snímku obrazovky).

> [!NOTE]
> To může jevit jako práce pro práci, chcete-li vytvořit `GetProductsInCategory(categoryID)` metody ve třídě použití modelu code-behind stránky s ASP.NET. Koneckonců, tato metoda jednoduše vytvoří instanci `ProductsBLL` třídy a vrátí výsledky z jeho `GetProductsByCategoryID(categoryID)` metoda. Případně proč bezpečná není právě tuto metodu volat přímo z Syntaxe datové vazby v popisu vnitřní Opakovači jako: `DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`? I když tato syntaxe vyhráli nefungují s naší aktuální implementace `ProductsBLL` třídy (od `GetProductsByCategoryID(categoryID)` metoda je metoda instance), můžete změnit tak, `ProductsBLL` chcete zahrnout statický `GetProductsByCategoryID(categoryID)` metoda nebo mají třídy zahrnují statickou `Instance()` metoda vrátí novou instanci třídy `ProductsBLL` třídy.


Zatímco tyto změny by eliminuje nutnost `GetProductsInCategory(categoryID)` metoda v třídě modelu code-behind stránky s ASP.NET, metoda třídy modelu code-behind nám poskytuje větší flexibilitu v práci s daty, načíst, protože za chvíli uvidíme.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Načíst všechny informace o produktu najednou

Dvě techniky zkontrolují jsme ve prověřit, získejte tyto produkty pro aktuální kategorii tím, že zavoláte na `ProductsBLL` třída s `GetProductsByCategoryID(categoryID)` – metoda (prvního postupu nebyla tak prostřednictvím ObjectDataSource, druhé až `GetProductsInCategory(categoryID)` metoda ve použití modelu Code-behind třídy). Pokaždé, když tato metoda vyvolána, vrstvy obchodní logiky volání do vrstvy přístupu k datům, který dotazuje databázi s příkazem SQL, který vrátí řádky z `Products` tabulky, jejichž `CategoryID` pole odpovídá zadaných vstupních parametrů.

Zadaný *N* kategorií v systému tohoto přístupu propojí *N* + 1 volání jednoho databázového dotazu databáze zobrazíte všechny kategorie a potom *N* volání k získání produktů specifické pro každou kategorii. Nemůžeme však načíst všechna potřebná data v jednom volání volání pouze dvě databáze a mějte všechny kategorie a druhou pro získání všech produktů. Jakmile budeme mít všechny produkty, můžeme filtrovat tyto produkty tedy pouze produkty odpovídající aktuální `CategoryID` jsou vázány na tuto kategorii s vnitřní opakovače.

Tuto funkci zajistí potřebujeme jen drobné změny provést `GetProductsInCategory(categoryID)` metody ve třídě použití modelu code-behind stránky s naší technologie ASP.NET. Místo slepě vrácení výsledků z `ProductsBLL` třída s `GetProductsByCategoryID(categoryID)` metoda, můžeme místo toho nejdřív Přejít *všechny* produktů (pokud jsou některé t byl již přistupovat) a vrátíte se pouze filtrované zobrazení produkty podle předaným `CategoryID`.


[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

Poznámka: Přidání proměnné úrovni stránky `allProducts`. To obsahuje informace o všech produktů a naplní při prvním `GetProductsInCategory(categoryID)` vyvolání metody. Až se ujistíte, `allProducts` objekt byly vytvořeny a naplněny, metoda výsledky filtruje na objekt DataTable s tak, aby pouze řádky, jejichž `CategoryID` odpovídá zadanému `CategoryID` jsou k dispozici. Tento přístup snižuje počet, kolikrát databázi přistupuje z *N* + 1 až dvě.

Toto vylepšení nezavádí vykreslované značky na stránce změny ani nemá přenést zpět méně záznamů než jiný přístup. Jednoduše snižuje počet volání do databáze.

> [!NOTE]
> Jeden intuitivně může důvod, snížení počtu databáze by assuredly poskytnout zvýšení výkonu. To však nemusí být případ. Pokud máte velké množství produktů, jehož `CategoryID` je `NULL`, příklad poté volání `GetProducts` metoda vrátí počet produktů, které nejsou nikdy zobrazeny. Kromě toho vrácení všech produktů, které mohou být plýtvání Pokud níž zobrazují se jenom podmnožinu kategorií, které může být v případě, Pokud implementujete stránkování.


Jako vždy, když jde o analýzy výkonu dvě techniky, pouze surefire opatření je řízené testy přizpůsobená pro vaši aplikaci s běžné scénáře.

## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli, jak vnořit jeden datový webový ovládací prvek v rámci druhého, konkrétně zkoumání jak vnější Repeater položku pro každou kategorii zobrazení se vnitřní Repeater výpis produktů pro každou kategorii v seznamu s odrážkami. Hlavní těžkostí při vytváření vnořených uživatelské rozhraní spočívá v přístupu k a správná data vytvoření vazby na ovládací prvek webového vnitřní data. Existuje řada různých technik k dispozici, z nichž dva jsme se zaměřili na v tomto kurzu. První přístup prověřit, používá prvku ObjectDataSource v datech vnější ovládací prvek webu s `ItemTemplate` , která byla vázaná na ovládací prvek webového vnitřních dat prostřednictvím jeho `DataSourceID` vlastnost. Druhý způsob spočívá používaná data prostřednictvím metody ve třídě s použití modelu code-behind stránky technologie ASP.NET. Tato metoda může být potom vázaný na data vnitřní ovládací prvek webu s `DataSource` vlastnosti prostřednictvím syntaxe datové vazby.

I když vnořené uživatelské rozhraní vyšetřovány v tomto kurzu použít Repeateru vnořené Repeateru, těchto postupů je možné rozšířit na jiných webových ovládacích prvcích dat. Můžete vnořovat v rámci GridView nebo GridView v rámci a v prvku DataList Repeater a tak dále.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Zack Jones a Liz Shulok. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
