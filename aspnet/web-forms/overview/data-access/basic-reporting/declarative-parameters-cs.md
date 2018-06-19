---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
title: Deklarativní parametrů (C#) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu jsme budete ukazují, jak použít parametr nastavit na hodnotu pevně vyberte data pro zobrazení v ovládacím prvku DetailsView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 603c9bd3-b895-4ec6-853b-0c81ff36d580
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
msc.type: authoredcontent
ms.openlocfilehash: 840630852d28f49f4f4387f1d2cc6b275b468fc2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875936"
---
<a name="declarative-parameters-c"></a>Deklarativní parametrů (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_5_CS.exe) nebo [stáhnout PDF](declarative-parameters-cs/_static/datatutorial05cs1.pdf)

> V tomto kurzu jsme budete ukazují, jak použít parametr nastavit na hodnotu pevně vyberte data pro zobrazení v ovládacím prvku DetailsView.


## <a name="introduction"></a>Úvod

V [poslední kurzu](displaying-data-with-the-objectdatasource-cs.md) jsme se podívali na zobrazení dat s GridView DetailsView a FormView ovládací prvky vázané na ovládacího prvku ObjectDataSource, který volá `GetProducts()` metoda z `ProductsBLL` třídy. `GetProducts()` Metoda vrátí hodnotu silného typu DataTable naplněný všechny záznamy z databáze Northwind `Products` tabulky. `ProductsBLL` Třída obsahuje další metody pro vracejících jenom podmnožiny produkty - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, a `GetProductsBySupplierID(supplierID)`. Tyto tři metody očekávají, že vstupní parametr označující postupy filtrovat informace o vrácený produktu.

ObjectDataSource lze volat metody, které očekávají vstupní parametry, ale pokud to chcete proto jsme musí určit, odkud pocházejí hodnoty pro tyto parametry. Hodnoty parametru může být pevně nebo může pocházet z různých zdrojů dynamické, včetně: hodnoty řetězce dotazu, proměnné relace, hodnota vlastnosti ovládacího prvku webové stránky nebo jiné.

V tomto kurzu Začněme tím znázorňující způsob použití parametru nastavená na hodnotu pevně. Konkrétně podíváme přidání DetailsView na stránku, který zobrazí informace o konkrétním produktu, a to Chef Anton Gumbo Mix, který má `ProductID` 5. V dalším kroku uvidíme, jak nastavit hodnotu parametru založené na ovládací prvek webu. Konkrétně aby mohl uživatel, zadejte v určité zemi, po jejímž uplynutí se kliknutím na tlačítko Zobrazit seznam dodavatelů, které jsou umístěny v dané zemi použijeme textové pole.

## <a name="using-a-hard-coded-parameter-value"></a>Pomocí pevně parametr hodnoty

Pro v prvním příkladu, začněte přidáním ovládacího prvku DetailsView `DeclarativeParams.aspx` stránku `BasicReporting` složky. Inteligentní značky prvku DetailsView, vyberte &lt;nový zdroj dat&gt; z rozevíracího seznamu a vyberte, chcete-li přidat ObjectDataSource.


[![Přidat ObjectDataSource na stránku](declarative-parameters-cs/_static/image2.png)](declarative-parameters-cs/_static/image1.png)

**Obrázek 1**: přidejte ObjectDataSource stránku ([Kliknutím zobrazit obrázek v plné velikosti](declarative-parameters-cs/_static/image3.png))


Tento program automaticky spustí Průvodce zvolit zdroj dat ovládacího prvku ObjectDataSource. Vyberte `ProductsBLL` třídy z první obrazovce průvodce.


[![Vyberte třídu ProductsBLL](declarative-parameters-cs/_static/image5.png)](declarative-parameters-cs/_static/image4.png)

**Obrázek 2**: vyberte `ProductsBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](declarative-parameters-cs/_static/image6.png))


Vzhledem k tomu, že chceme zobrazit informace o konkrétní produkt chceme používat `GetProductByProductID(productID)` metoda.


[![Zvolte metodu GetProductByProductID(productID)](declarative-parameters-cs/_static/image8.png)](declarative-parameters-cs/_static/image7.png)

**Obrázek 3**: Zvolte `GetProductByProductID(productID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](declarative-parameters-cs/_static/image9.png))


Vzhledem k tomu, že metoda, kterou jsme vybrali obsahuje parametr, je jedna další obrazovka průvodce, kde budeme požádáni o definovat hodnotu pro parametr. V seznamu na levé straně jsou uvedeny všechny parametry pro vybranou metodu. Pro `GetProductByProductID(productID)` existuje pouze jeden `productID`. Na pravé straně jsme můžete zadat hodnotu pro parametr vybrané. Parametr zdrojového rozevíracího seznamu uvádí různé možné zdroje pro hodnotu parametru. Vzhledem k tomu, že chcete zadat hodnotu 5 pro pevně `productID` parametr, nechte zdroji parametr jako None a zadejte do textového pole Výchozí hodnota 5.


[![Hard-Coded parametr z 5 bude použita hodnota pro parametr productID](declarative-parameters-cs/_static/image11.png)](declarative-parameters-cs/_static/image10.png)

**Obrázek 4**: A Hard-Coded parametr hodnota z 5 se použije pro `productID` parametr ([Kliknutím zobrazit obrázek v plné velikosti](declarative-parameters-cs/_static/image12.png))


Po dokončení průvodce Konfigurace zdroje dat, deklarativní značky ovládacího prvku ObjectDataSource zahrnuje `Parameter` objekt v `SelectParameters` kolekce pro každý vstupní parametry očekává metodou definovaný v `SelectMethod` Vlastnost. Vzhledem k tomu, že používáme v tomto příkladu metoda očekává právě jeden vstupní parametr, `parameterID`, existuje pouze jedna položka v tomto poli. `SelectParameters` Kolekce může obsahovat libovolnou třídu odvozenou z `Parameter` třídy v `System.Web.UI.WebControls` oboru názvů. Pro parametr pevně hodnoty základní `Parameter` třída se používá, ale pro druhý parametr možnosti zdroje odvozené `Parameter` třída se používá; můžete také vytvořit vlastní [vlastních typů parametrů](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), v případě potřeby.

[!code-aspx[Main](declarative-parameters-cs/samples/sample1.aspx)]

> [!NOTE]
> Pokud jste podle ve vašem počítači deklarativní se zobrazí v tomto okamžiku může obsahovat hodnoty `InsertMethod`, `UpdateMethod`, a `DeleteMethod` vlastnosti, a také `DeleteParameters`. Průvodce ObjectDataSource zvolit zdroj dat automaticky určuje metody z `ProductBLL` chcete použít pro vložení, aktualizace a odstranění, takže pokud jste explicitně vymazali ty out, budete zahrnuty ve výše uvedených značek.


Při návštěvě této stránky, budou data ovládací prvek webu vyvolání ObjectDataSource `Select` metodu, která bude volat `ProductsBLL` třídy `GetProductByProductID(productID)` metodu pomocí hodnoty 5 pro pevně `productID` vstupní parametr. Metoda vrátí silného typu `ProductDataTable` objekt, který obsahuje jeden řádek s informacemi o Chef Anton Gumbo Mix (produktu s `ProductID` 5).


[![Zobrazují informace o Chef Anton's Gumbo Mix](declarative-parameters-cs/_static/image14.png)](declarative-parameters-cs/_static/image13.png)

**Obrázek 5**: se zobrazují informace o Chef Anton's Gumbo Mix ([Kliknutím zobrazit obrázek v plné velikosti](declarative-parameters-cs/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Nastavení hodnoty parametru na hodnotu vlastnosti ovládacího prvku webového

ObjectDataSource parametr, který hodnoty lze také nastavit na základě hodnoty ovládacího prvku webového na stránce. Pro znázornění je budeme mít GridView, který zobrazí seznam všech dodavatelů, které se nacházejí v jiné zemi specifikovaných uživatelem. Pokud chcete tento počáteční přidáním textové pole na stránku, do kterého může uživatel zadat název země. Nastavení tohoto ovládacího prvku TextBox `ID` vlastnost `CountryName`. Také přidání ovládacího prvku tlačítko.


[![Přidat textové pole na stránku s ID země](declarative-parameters-cs/_static/image17.png)](declarative-parameters-cs/_static/image16.png)

**Obrázek 6**: přidat textové pole na stránku s `ID` `CountryName` ([Kliknutím zobrazit obrázek v plné velikosti](declarative-parameters-cs/_static/image18.png))


Přidat GridView na stránku a z inteligentních značek v dalším kroku vyberte Přidat nové ObjectDataSource. Vzhledem k tomu, že chceme zobrazit vyberte informace dodavatele `SuppliersBLL` třídy z Průvodce na první obrazovce. Na obrazovce druhý vyberte `GetSuppliersByCountry(country)` metoda.


[![Zvolte metodu GetSuppliersByCountry(country)](declarative-parameters-cs/_static/image20.png)](declarative-parameters-cs/_static/image19.png)

**Obrázek 7**: Zvolte `GetSuppliersByCountry(country)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](declarative-parameters-cs/_static/image21.png))


Vzhledem k tomu `GetSuppliersByCountry(country)` metoda má vstupní parametr, průvodce znovu zahrnuje poslední obrazovka pro výběr hodnotu parametru. Tuto chvíli nastavte zdroj parametru na ovládací prvek. To bude naplnění seznamu ControlID rozevírací seznam s názvy ovládacích prvků na stránce; Vyberte `CountryName` řízení ze seznamu. Když je nejdřív navštívené stránky `CountryName` textového pole bude prázdné, tak, aby se žádné výsledky a nezobrazí se. Pokud chcete zobrazit některé výsledky ve výchozím nastavení, do textového pole DefaultValue odpovídajícím způsobem nastavte.


[![Nastaví hodnotu parametru na hodnotu země ovládací prvek](declarative-parameters-cs/_static/image23.png)](declarative-parameters-cs/_static/image22.png)

**Obrázek 8**: nastavte hodnotu parametru na `CountryName` hodnota ovládacího prvku ([Kliknutím zobrazit obrázek v plné velikosti](declarative-parameters-cs/_static/image24.png))


ObjectDataSource deklarativní se mírně liší z našeho prvního příkladu, použití [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) místo standardní `Parameter` objektu. A `ControlParameter` má další vlastnosti k určení `ID` ovládací prvek webu a hodnota vlastnosti pro parametr (`PropertyName`). Průvodce konfigurace zdroje dat byla dostatečně inteligentní k určení, že pro textové pole, jsme budete pravděpodobně chtít použít `Text` vlastností pro hodnotu parametru. Pokud ale chcete použít jinou vlastnost hodnotu z ovládacího prvku webového můžete změnit `PropertyName` hodnotu zde nebo kliknutím na odkaz "Zobrazit rozšířené vlastnosti" v průvodci.

[!code-aspx[Main](declarative-parameters-cs/samples/sample2.aspx)]

Při první návštěvě stránky `CountryName` textové pole je prázdné. ObjectDataSource `Select` metoda je stále vyvolané GridView, ale hodnota `null` předá do `GetSuppliersByCountry(country)` metoda. Převede TableAdapter `null` do databáze `NULL` hodnotu (`DBNull.Value`), ale dotaz používá `GetSuppliersByCountry(country)` metoda je napsán tak, aby nevrací žádné hodnoty při `NULL` je zadaná hodnota pro `@CategoryID`parametr. Stručně řečeno jsou vráceny žádné dodavatelů.

Jakmile návštěvníka zadá země, ale a klikne na tlačítko Zobrazit dodavatelé a způsobuje postback, ObjectDataSource na `Select` fokusu metoda předávání v ovládacím prvku TextBox `Text` hodnoty jako `country` parametr.


[![Jsou zobrazeny těchto dodavatelů z Kanada](declarative-parameters-cs/_static/image26.png)](declarative-parameters-cs/_static/image25.png)

**Obrázek 9**: jsou uvedené ty Dodavatelé z Kanada ([Kliknutím zobrazit obrázek v plné velikosti](declarative-parameters-cs/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Zobrazení všech dodavatelů ve výchozím nastavení

Spíše než jeden z dodavatelů zobrazit při prvním zobrazení stránky může chceme zobrazit *všechny* dodavatelů na první pohled, které uživateli umožňují porovnat dolů v seznamu tak, že zadáte název země do textového pole. Pokud do textového pole prázdné, `SuppliersBLL` třídy `GetSuppliersByCountry(country)` metodě se předává v `null` hodnotu pro jeho *`country`* vstupní parametr. To `null` hodnota je pak předá do DAL `GetSupplierByCountry(country)` metoda, kde je přeložená na databázi `NULL` hodnota `@Country` parametr v následujícím dotazu:

[!code-sql[Main](declarative-parameters-cs/samples/sample3.sql)]

Výraz `Country = NULL` vždy vrátí hodnotu False, i pro záznamy jehož `Country` sloupec má `NULL` hodnoty; proto se nevrátily žádné záznamy.

Vrátit *všechny* dodavatelé při země textové pole je prázdné, jsme posílení `GetSuppliersByCountry(country)` metoda v BLL má být vyvolán `GetSuppliers()` metoda po její parametr země `null` a k volání DAL `GetSuppliersByCountry(country)` Metoda, jinak hodnota. To bude mít vliv vrácení všech dodavatelů, pokud je zadána žádná země a příslušné podmnožinu dodavatelů, pokud je parametr země zahrnuta.

Změna `GetSuppliersByCountry(country)` metoda v `SuppliersBLL` třídy pro následující:

[!code-csharp[Main](declarative-parameters-cs/samples/sample4.cs)]

V této změně `DeclarativeParams.aspx` stránka zobrazuje všechny dodavatele, když nejdřív navštívili (nebo vždy, když `CountryName` textové pole je prázdné).


[![Nyní se zobrazí ve výchozím nastavení jsou všechny dodavatelé](declarative-parameters-cs/_static/image29.png)](declarative-parameters-cs/_static/image28.png)

**Obrázek 10**: nyní zobrazí ve výchozím nastavení jsou všichni dodavatelé ([Kliknutím zobrazit obrázek v plné velikosti](declarative-parameters-cs/_static/image30.png))


## <a name="summary"></a>Souhrn

Abyste mohli používat metody s vstupní parametry, je potřeba zadat hodnoty pro parametry v prvku ObjectDataSource `SelectParameters` kolekce. Různé typy parametrů umožňují hodnota parametru možné získat z různých zdrojů. Typ parametru výchozí používá pevně zakódovaný hodnotu, ale stejně jako se snadno (a se bez řádek kódu) nelze získat hodnoty parametrů z řetězce dotazu, proměnné relace, soubory cookie a i uživatele zadali hodnoty z webových ovládacích prvků na stránce.

Příklady, které jsme se podívali na v tomto kurzu ukazuje způsob použití deklarativní parametr hodnoty. Můžou ale nastat situace, kdy je potřeba použít parametr zdroj, který není k dispozici, jako je aktuální datum a čas, nebo pokud náš web používal členství, ID uživatele návštěvníka. Pro tyto scénáře jsme můžete nastavit hodnoty parametru prostřednictvím kódu programu před ObjectDataSource volání metody jeho základní objekt. Ukážeme, jak to provést [další kurz](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md).

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Hilton Giesenow. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](displaying-data-with-the-objectdatasource-cs.md)
> [další](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
