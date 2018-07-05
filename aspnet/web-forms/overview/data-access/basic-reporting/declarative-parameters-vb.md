---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Deklarované parametry (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu jsme vám ukazují, jak použít parametrem nastaveným na hodnotu pevně zakódované vyberte data pro zobrazení v ovládacím prvku DetailsView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: 86590faf30b41acdd8a0922f9cc33feabfded23f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397727"
---
<a name="declarative-parameters-vb"></a>Deklarované parametry (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) nebo [stahovat PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> V tomto kurzu jsme vám ukazují, jak použít parametrem nastaveným na hodnotu pevně zakódované vyberte data pro zobrazení v ovládacím prvku DetailsView.


## <a name="introduction"></a>Úvod

V [poslední kurz](displaying-data-with-the-objectdatasource-vb.md) jsme se podívali na zobrazení dat pomocí ovládacího prvku GridView, DetailsView a FormView ovládací prvky vázané na ovládací prvek ObjectDataSource, která vyvolá `GetProducts()` metodu z `ProductsBLL` třídy. `GetProducts()` Vrátí metoda hodnotu silného typu DataTable naplněný všemi záznamy z databáze Northwind `Products` tabulky. `ProductsBLL` Třída obsahuje další metody pro vrácení pouze podmnožiny produkty - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, a `GetProductsBySupplierID(supplierID)`. Tyto tři metody očekávají vstupního parametru, jak filtrovat informace vrácené produktu.

Prvku ObjectDataSource je možné volat metody, které očekávají vstupní parametry, ale aby bylo možné provést, proto jsme musí určit, odkud pochází hodnoty těchto parametrů. Hodnoty parametru může být pevně zakódované nebo může pocházet z nejrůznějších dynamické zdrojů, včetně: hodnoty řetězce dotazu, proměnné relace hodnotu vlastnosti webový ovládací prvek na stránce nebo jiné.

Pro účely tohoto kurzu Začněme tím, že ilustrující způsob použití parametrem nastaveným na hodnotu pevně zakódované. Konkrétně zaměříme na přidání DetailsView na stránku, která zobrazí informace o konkrétního produktu, konkrétně Chef Anton Gumbo poměr, který má `ProductID` 5. V dalším kroku uvidíme, jak nastavit hodnotu parametru založené na webový ovládací prvek. Zejména použijeme textové pole, umožníte uživateli, zadejte v zemi, po jejímž uplynutí se kliknutím na tlačítko Zobrazit seznam dodavatelů, které se nacházejí v dané zemi.

## <a name="using-a-hard-coded-parameter-value"></a>Použití pevně zakódované parametr hodnoty

První příklad, začněte přidáním ovládacího prvku DetailsView `DeclarativeParams.aspx` stránku `BasicReporting` složky. V ovládacím prvku DetailsView inteligentní značky, vyberte &lt;nový zdroj dat&gt; z rozevíracího seznamu a zvolte možnost Přidat prvku ObjectDataSource.


[![Přidat na stránku prvku ObjectDataSource](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Obrázek 1**: Přidat na stránku prvku ObjectDataSource ([kliknutím ji zobrazíte obrázek v plné velikosti](declarative-parameters-vb/_static/image3.png))


To se automaticky spustí Průvodce zvolit zdroj dat ovládacího prvku ObjectDataSource. Vyberte `ProductsBLL` třídy na první obrazovce průvodce.


[![Vyberte třídu ProductsBLL](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Obrázek 2**: vyberte `ProductsBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](declarative-parameters-vb/_static/image6.png))


Protože chceme, aby k zobrazení informací o daném produktu chceme použít `GetProductByProductID(productID)` metody.


[![Zvolte GetProductByProductID(productID) – metoda](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Obrázek 3**: Zvolte `GetProductByProductID(productID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](declarative-parameters-vb/_static/image9.png))


Protože metoda, kterou jsme vybrali obsahuje parametr, je jedna další obrazovka průvodce, kde jsme vyzváni k definování hodnota, která má být použitý pro parametr. V seznamu na levé straně jsou uvedeny všechny parametry pro vybranou metodu. Pro `GetProductByProductID(productID)` existuje pouze jeden `productID`. Na pravé straně lze zadat hodnotu pro vybraný parametr. Seznamu parametrů zdrojové rozevírací seznam uvádí různé možné zdroje pro hodnotu parametru. Protože chceme zadejte hodnotu 5 pro pevně zakódované `productID` parametr, nechte zdroji parametru jako žádné a zadejte do textového pole Výchozí hodnota 5.


[![Hard-Coded parametr z 5 bude použita hodnota pro parametr productID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Obrázek 4**: A Hard-Coded parametr z 5 bude použita hodnota pro `productID` parametr ([kliknutím ji zobrazíte obrázek v plné velikosti](declarative-parameters-vb/_static/image12.png))


Po dokončení Průvodce nakonfigurovat zdroj dat, deklarativní ovládací prvek ObjectDataSource zahrnuje `Parameter` objekt `SelectParameters` kolekce pro každý vstupní parametry očekává metody definované v `SelectMethod` Vlastnost. Protože používáme v tomto příkladu metoda očekává právě jeden vstupní parametr, `parameterID`, existuje pouze jedna položka tady. `SelectParameters` Kolekce může obsahovat jakoukoli třídu, která je odvozena z `Parameter` třídy v `System.Web.UI.WebControls` oboru názvů. Pro základní hodnoty parametrů pevně zakódované `Parameter` třída se používá, ale pro druhý parametr možnosti zdroj odvozený `Parameter` třída se používá, můžete také vytvořit svoje vlastní [vlastních typů parametrů](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), v případě potřeby.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Pokud jste postupovali na vlastním počítači deklarativní se zobrazí v tomto okamžiku může obsahovat hodnoty `InsertMethod`, `UpdateMethod`, a `DeleteMethod` vlastnosti, jakož i `DeleteParameters`. Průvodce ObjectDataSource zvolit zdroj dat automaticky určuje metod z `ProductBLL` má použít pro vložení, aktualizace nebo odstranění, takže pokud jste výslovně zrušili ty navýšení kapacity, budete zahrnuty v kódu výše.


Při návštěvě této stránky, vyvolá data webový ovládací prvek ObjectDataSource `Select` metodu, která bude volat `ProductsBLL` třídy `GetProductByProductID(productID)` metodu pomocí pevně zakódovaného hodnotu 5 pro `productID` vstupního parametru. Metoda vrátí silného typu `ProductDataTable` objekt, který obsahuje jeden řádek s informacemi o Chefu Anton Gumbo Mix (produkt s `ProductID` 5).


[![Informace o Chefu Antonova Gumbo Mix jsou zobrazeny.](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Obrázek 5**: se zobrazují informace o Chefu Antonova Gumbo Mix ([kliknutím ji zobrazíte obrázek v plné velikosti](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Nastavením parametru na hodnotu k hodnotě vlastnosti webového ovládacího prvku

Parametr ObjectDataSource, můžete také nastavit hodnoty na základě hodnoty webového ovládacího prvku na stránce. Pro znázornění, dopřejeme si prvku GridView, který obsahuje seznam všech dodavatelů, které se nacházejí v určité zemi specifikovaných uživatelem. K provedení této úvodní tak, že přidáte textové pole na stránku, do kterého může uživatel zadat název země. Nastavit tento ovládací prvek TextBox `ID` vlastnost `CountryName`. Také přidání ovládacího prvku tlačítko.


[![Přidání textového pole na stránku s ID země](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Obrázek 6**: Přidání textového pole na stránku s `ID` `CountryName` ([kliknutím ji zobrazíte obrázek v plné velikosti](declarative-parameters-vb/_static/image18.png))


Chcete-li přidat nový prvek ObjectDataSource GridView přidat na stránku a z inteligentních značek zvolte v dalším kroku. Protože chceme zobrazit vyberte informace dodavateli `SuppliersBLL` třídy z Průvodce první obrazovky. Na druhé obrazovce, vyberte `GetSuppliersByCountry(country)` metody.


[![Zvolte GetSuppliersByCountry(country) – metoda](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Obrázek 7**: Zvolte `GetSuppliersByCountry(country)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](declarative-parameters-vb/_static/image21.png))


Vzhledem k tomu, `GetSuppliersByCountry(country)` metoda má vstupní parametr, průvodce znovu obsahuje finální obrazovce pro výběr hodnotu parametru. Tentokrát nastavte zdroj parametru na ovládací prvek. To se vyplní ControlID rozevíracím seznamu se názvy ovládacích prvků na stránce. Vyberte `CountryName` ovládací prvek ze seznamu. Při první návštěvě stránky `CountryName` textového pole bude prázdné, takže nevrátí žádné výsledky a nezobrazí se. Pokud chcete zobrazit některé výsledky ve výchozím nastavení, nastavení textového pole DefaultValue odpovídajícím způsobem.


[![Nastavte hodnotu parametru na hodnotu ovládacího prvku země](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Obrázek 8**: nastavte na hodnotu parametru `CountryName` hodnota ovládacího prvku ([kliknutím ji zobrazíte obrázek v plné velikosti](declarative-parameters-vb/_static/image24.png))


Deklarativní ObjectDataSource se mírně liší od našeho prvního příkladu pomocí [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) místo standardní `Parameter` objektu. A `ControlParameter` má další vlastnosti k určení `ID` webové kontroly a hodnota vlastnosti pro parametr (`PropertyName`). Průvodce konfigurace zdroje dat byla dostatečně inteligentní, chcete-li zjistit, že pro textové pole, jsme budete pravděpodobně chtít použít `Text` vlastností pro hodnotu parametru. Pokud však chcete použít hodnotu různé vlastnosti z ovládacího prvku webového můžete změnit `PropertyName` hodnoty tady nebo kliknutím na odkaz "Zobrazit pokročilé vlastnosti" v průvodci.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Při první návštěvě stránky `CountryName` textové pole je prázdné. Prvku ObjectDataSource `Select` stále vyvolání metody prvku GridView, ale hodnota `Nothing` je předána do `GetSuppliersByCountry(country)` metody. Převede TableAdapter `Nothing` do databáze `NULL` hodnotu (`DBNull.Value`), ale dotazu použitého `GetSuppliersByCountry(country)` metoda je zapsána tak, že nevrací žádné hodnot, když `NULL` je zadaná hodnota pro `@CategoryID`parametru. Stručně řečeno jsou vráceny žádné dodavatelů.

Jakmile návštěvníka zadá v zemi, ale a klikne na tlačítko Zobrazit dodavatelé pro vyvolávají zpětné odeslání, ObjectDataSource společnosti `Select` fokusu metoda předávání v ovládacím prvku TextBox `Text` hodnoty jako `country` parametr.


[![Tito poskytovatelé z Kanady se zobrazí.](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Obrázek 9**: dodavatelé ty z Kanady se zobrazí ([kliknutím ji zobrazíte obrázek v plné velikosti](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Zobrazení všech dodavatelů ve výchozím nastavení

Spíše než jeden z dodavatelů zobrazit při prvním zobrazení stránky může být vhodné zobrazit *všechny* dodavatelé zpočátku, které uživateli umožňují Zredukovat seznamu tak, že zadáte název země v textovém poli. Pokud textové pole je prázdné, `SuppliersBLL` třídy `GetSuppliersByCountry(country)` předaný metodě `Nothing` pro jeho *`country`* vstupního parametru. To `Nothing` hodnota je pak předá do DAL `GetSupplierByCountry(country)` metody, kde se přeloží na databázi `NULL` hodnotu pro `@Country` parametr do ní následující dotaz:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

Výraz `Country = NULL` vždy vrátí hodnotu False, i pro záznamy obsahují `Country` má sloupec `NULL` hodnota; proto nejsou vráceny žádné záznamy.

Vrátit *všechny* dodavatelé při zemi textové pole je prázdné, můžeme rozšířit `GetSuppliersByCountry(country)` BLL vyvolat metodu `GetSuppliers()` metoda při jeho země parametr `Nothing` a k volání DAL `GetSuppliersByCountry(country)` Metoda jinak. To bude mít vliv vrácení všech dodavatelů-li zadána žádná země a příslušnou podmnožinu dodavatelé při zemi parametru je součástí.

Změnit `GetSuppliersByCountry(country)` metodu `SuppliersBLL` třídy pro následující:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Díky této změně `DeclarativeParams.aspx` stránka zobrazuje všechny dodavatele, když první uživatel (nebo vždy, když `CountryName` textové pole je prázdné).


[![Všichni dodavatelé se teď zobrazují ve výchozím nastavení](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Obrázek 10**: Všichni dodavatelé se teď zobrazují ve výchozím nastavení ([kliknutím ji zobrazíte obrázek v plné velikosti](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Souhrn

Aby bylo možné použít metody se vstupními parametry, musíme zadejte hodnoty pro parametry v prvku ObjectDataSource `SelectParameters` kolekce. Různé typy parametrů umožňují hodnotu parametru ho získat z různých zdrojů. Typ parametru výchozí používá pevně zakódovaný hodnotu, ale stejně snadno (a se bez jediného řádku kódu) můžete získat hodnoty parametrů z řetězce dotazu, proměnné relace, soubory cookie a dokonce i uživatele zadaných hodnot z ovládacích prvků na stránce.

Příklady, které jsme se podívali na v tomto kurzu ukazuje způsob použití deklarativní parametr hodnoty. Nicméně mohou nastat situace, kdy budeme potřebovat budou používat zdroj parametr, který není k dispozici, jako je aktuální datum a čas, nebo pokud našeho webu používala členství, ID uživatele návštěvníka. Pro takové scénáře jsme můžete nastavit hodnoty parametrů prostřednictvím kódu programu před ObjectDataSource volání metody základní objekt. Uvidíme, jak to provést [další kurz](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Hilton Giesenow. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](displaying-data-with-the-objectdatasource-vb.md)
> [další](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
