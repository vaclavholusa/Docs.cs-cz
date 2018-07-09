---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
title: Dva DropDownList (VB) filtrování záznamů Master/Detail | Dokumentace Microsoftu
author: rick-anderson
description: Tento kurz rozšiřuje vztah záznamů master/detail přidat třetí vrstvou, pomocí dvou ovládacích prvků DropDownList a vyberte požadovaný nadřazený a dvě úrovně nadřazený recor...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 11ae4f64-01ba-4823-95f4-a2fe1f84f7d7
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
msc.type: authoredcontent
ms.openlocfilehash: 17bbaa346925585b5b184127fa80fd2203869492
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805282"
---
<a name="masterdetail-filtering-with-two-dropdownlists-vb"></a>Dva DropDownList (VB) filtrování záznamů Master/Detail
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_8_VB.exe) nebo [stahovat PDF](master-detail-filtering-with-two-dropdownlists-vb/_static/datatutorial08vb1.pdf)

> Tento kurz rozšiřuje vztah záznamů master/detail přidat třetí vrstvou, pomocí dvou ovládacích prvků DropDownList a vyberte požadované záznamy nadřazený a dvě úrovně nadřazený.


## <a name="introduction"></a>Úvod

V [předchozí kurz o službě](master-detail-filtering-with-a-dropdownlist-vb.md) jsme se zaměřili na způsob zobrazení jednoduché hlavních/podrobných sestavy pomocí jednoho DropDownList vyplní kategorií a GridView zobrazující těchto produktů, které patří do vybrané kategorie. Tento model sestavy dobře funguje při zobrazení záznamů, které mají vztah jeden mnoho a je možné snadno rozšířit pro scénáře, které zahrnují více vztahů jednoho k několika. Systém pro zadávání objednávek byste mít například tabulky, které odpovídají zákazníky, objednávky a položky řádku pořadí. Si daný zákazník může mít více příkazy se jednotlivé objednávky, který se skládá z více položek. Tato data lze zobrazit uživateli pomocí dvou DropDownLists a GridView. První DropDownList by mohlo mít položku seznamu pro každého zákazníka v databázi s druhým jeden z obsah se objednávek podle vybraného zákazníka. GridView by zobrazila seznam položky řádku z vybrané objednávky.

Když databázi Northwind zahrnout informace o canonical podrobnosti zákazníka objednávky v jeho `Customers`, `Orders`, a `Order Details` tabulek, tyto tabulky nejsou zachyceny v naší architektury. Nicméně jsme můžete stále ukazují pomocí dvou DropDownLists závislé. První DropDownList zobrazí seznam kategorií a druhá produkty, které patří do vybrané kategorie. DetailsView se potom zobrazí seznam podrobnosti vybraného produktu.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Krok 1: Vytvoření a naplnění DropDownList kategorie

Naším prvním cílem je přidání DropDownList, který obsahuje seznam kategorií. Tyto kroky se podrobně v předchozím kurzu, ale jsou tady shrnuté pro úplnost.

Otevřít `MasterDetailsDetails.aspx` stránku `Filtering` DropDownList přidat na stránku nastavení složky, jeho `ID` vlastnost `Categories`a pak klikněte na odkaz Konfigurovat zdroj dat v jeho inteligentních značek. Z Průvodce konfigurací zdroje dat vyberte a přidejte nový zdroj dat.


[![Přidat nový zdroj dat pro DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image1.png)

**Obrázek 1**: Přidat nový zdroj dat pro DropDownList ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image3.png))


Nový zdroj dat přirozeně, třeba prvku ObjectDataSource. Pojmenujte tento nový prvek ObjectDataSource `CategoriesDataSource` a jeho vyvolání `CategoriesBLL` objektu `GetCategories()` metody.


[![Zvolte možnost použití třídy CategoriesBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image4.png)

**Obrázek 2**: Zvolte možnost k použití `CategoriesBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image6.png))


[![Konfigurace ObjectDataSource GetCategories() metody](master-detail-filtering-with-two-dropdownlists-vb/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image7.png)

**Obrázek 3**: Konfigurace ObjectDataSource k použití `GetCategories()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image9.png))


Po dokončení konfigurace ObjectDataSource stále potřebujeme k určení, které pole zdroje dat má být zobrazen v `Categories` DropDownList a který by měl být nakonfigurovaný jako hodnotu pro položku seznamu. Nastavte `CategoryName` pole jako zobrazení a `CategoryID` jako hodnotu pro každou položku seznamu.


[![Mít zobrazení DropDownList CategoryName pole a CategoryID použijte jako hodnotu](master-detail-filtering-with-two-dropdownlists-vb/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image10.png)

**Obrázek 4**: Zobrazit DropDownList `CategoryName` pole a použití `CategoryID` jako hodnotu ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image12.png))


V tuto chvíli máme ovládací prvek DropDownList (`Categories`), který je naplněný záznamy ze `Categories` tabulky. Když se uživatel rozhodne novou kategorii z DropDownList jsme vhodné zpětného odeslání dojde k aktualizaci produktu DropDownList, který chceme vytvořit v kroku 2. Proto zkontrolujte možnost povolit vlastnost AutoPostBack z `categories` společnosti DropDownList inteligentních značek.


[![Povolit vlastnost AutoPostBack pro kategorie DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image13.png)

**Obrázek 5**: Povolit automatické odeslání pro `Categories` DropDownList ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Krok 2: Zobrazení vybrané kategorie produktů v druhé DropDownList

S `Categories` DropDownList dokončeno, naším dalším krokem je zobrazíte DropDownList produktů, které patří do vybrané kategorie. K tomu přidat na stránku s názvem jiné DropDownList `ProductsByCategory`. Stejně jako u `Categories` DropDownList, vytvořte nový prvek ObjectDataSource pro `ProductsByCategory` DropDownList s názvem `ProductsByCategoryDataSource`.


[![Přidat nový zdroj dat pro ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image16.png)

**Obrázek 6**: Přidat nový zdroj dat pro `ProductsByCategory` DropDownList ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image18.png))


[![Vytvoření nového prvku ObjectDataSource s názvem ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-vb/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image19.png)

**Obrázek 7**: vytvoření nového prvku ObjectDataSource s názvem `ProductsByCategoryDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image21.png))


Protože `ProductsByCategory` DropDownList musí zobrazit pouze tyto produkty, které patří do vybrané kategorie mají ObjectDataSource vyvolat `GetProductsByCategoryID(categoryID)` metody z `ProductsBLL` objektu.


[![Zvolte možnost použití třídy ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image22.png)

**Obrázek 8**: Zvolte možnost k použití `ProductsBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image24.png))


[![Konfigurace ObjectDataSource GetProductsByCategoryID(categoryID) metody](master-detail-filtering-with-two-dropdownlists-vb/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image25.png)

**Obrázek 9**: Konfigurace ObjectDataSource k použití `GetProductsByCategoryID(categoryID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image27.png))


V posledním kroku průvodce musíme určit hodnotu *`categoryID`* parametru. Přiřaďte tento parametr na vybranou položku z `Categories` DropDownList.


[![Hodnota parametru categoryID načítat DropDownList kategorie](master-detail-filtering-with-two-dropdownlists-vb/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image28.png)

**Obrázek 10**: o přijetí změn *`categoryID`* hodnota parametru `Categories` DropDownList ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image30.png))


Ovládacím prvkem ObjectDataSource nakonfigurované už jen zbývá k určení, jaké pole zdroje dat slouží k zobrazení a hodnotu položek DropDownList. Zobrazení `ProductName` pole a použít `ProductID` pole jako hodnotu.


[![Zadejte pole zdroje dat použít pro Text položky DropDownList ListItems' a hodnota vlastnosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image31.png)

**Obrázek 11**: určení polí zdroj dat použít pro DropDownList `ListItem` s " `Text` a `Value` vlastnosti ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image33.png))


Ovládacím prvkem ObjectDataSource a `ProductsByCategory` DropDownList nakonfigurované naší stránce se zobrazí dvě DropDownLists: první zobrazí seznam všech kategorií během druhého, zobrazí se seznam těchto produktů, které patří do vybrané kategorie. Když uživatel vybere z první DropDownList novou kategorii, bude následovat zpětné volání a druhý DropDownList bude odrážejí, zobrazuje tyto produkty, které patří do nově vybranou kategorii. Obrázky 12 a 13 zobrazit `MasterDetailsDetails.aspx` v akci při prohlížení prostřednictvím prohlížeče.


[![Při první návštěvě stránky, je vybrané kategorie Nápoje](master-detail-filtering-with-two-dropdownlists-vb/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image34.png)

**Obrázek 12**: při první návštěvě stránky, je vybrané kategorie nápoje ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image36.png))


[![Výběr obsahuje jinou kategorii zobrazí nové kategorie produktů](master-detail-filtering-with-two-dropdownlists-vb/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image37.png)

**Obrázek 13**: Výběr různé kategorie zobrazuje nové kategorie produktů ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image39.png))


Aktuálně `productsByCategory` DropDownList, když se změní, nemá *není* vyvolávají zpětné odeslání. Nicméně se chceme zpětného odeslání dojde k po přidáme prvku DetailsView. Chcete-li zobrazit podrobnosti o vybrané produktu (krok 3). Proto, zaškrtněte políčko Povolit vlastnost AutoPostBack z `productsByCategory` společnosti DropDownList inteligentních značek.


[![Povolit vlastnost AutoPostBack funkci pro productsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image40.png)

**Obrázek 14**: Povolit vlastnost AutoPostBack funkci pro `productsByCategory` DropDownList ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Krok 3: Použití DetailsView chcete zobrazit podrobnosti pro vybraný produkt

Posledním krokem je chcete zobrazit podrobnosti pro vybraný produkt v DetailsView. Chcete-li DetailsView dosáhnout, přidat na stránku, nastavte jeho `ID` vlastnost `ProductDetails`a vytvoří se pro ni nového prvku ObjectDataSource. Konfigurace tohoto prvku ObjectDataSource přebírat jeho data ze `ProductsBLL` třídy `GetProductByProductID(productID)` metodu pomocí vybrané hodnotě objektu `ProductsByCategory` DropDownList pro hodnotu vlastnosti *`productID`* parametru.


[![Zvolte možnost použití třídy ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image43.png)

**Obrázek 15**: Zvolte možnost k použití `ProductsBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image45.png))


[![Konfigurace ObjectDataSource GetProductByProductID(productID) metody](master-detail-filtering-with-two-dropdownlists-vb/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image46.png)

**Obrázek 16**: Konfigurace ObjectDataSource k použití `GetProductByProductID(productID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image48.png))


[![Hodnota parametru productID načítat ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image49.png)

**Obrázek 17**: o přijetí změn *`productID`* hodnota parametru `ProductsByCategory` DropDownList ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image51.png))


Můžete také zobrazovat žádné z dostupných polí v `ProductDetails` prvku DetailsView. Jste se rozhodli odebrat `ProductID`, `SupplierID`, a `CategoryID` polí a pořadí změníte a ve formátu zbývající pole. Kromě toho vymaže na ovládacím prvku DetailsView `Height` a `Width` vlastnosti, povolení prvku DetailsView rozbalte šířce potřebné k nejlepšího zobrazení jeho data, namísto nutnosti omezené na zadané velikosti. Úplný kód se zobrazí níže:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample1.aspx)]

Za chvíli si vyzkoušet `MasterDetailsDetails.aspx` stránku v prohlížeči. Na první pohled může zdát, že vše funguje podle potřeby, ale je nějaký drobný problém. Pokud zvolíte novou kategorii `ProductsByCategory` DropDownList je aktualizováno, aby zahrnovalo tyto produkty pro vybrané kategorie, ale `ProductDetails` DetailsView nadále zobrazit předchozí informace o produktu. Ovládacím prvku DetailsView aktualizuje, když zvolíte jiný produkt pro vybranou kategorii. Kromě toho pokud dostatečně důkladně otestujte budete zjistíte, že pokud se rozhodnete průběžně nové kategorie (jako je například výběr nápoje z `Categories` DropDownList produkty koření, a potom Confections) každý výběr kategorie způsobí, že `ProductDetails`DetailsView aktualizovat.

Abyste tento problém konkretizovat, Podívejme se na konkrétní příklad. Při první návštěvě stránky vybrané kategorie Nápoje a související produkty jsou načteny v `ProductsByCategory` DropDownList. Chai je vybraný produkt a jeho podrobnosti jsou zobrazeny v `ProductDetails` DetailsView, jak ukazuje obrázek 18.


[![Podrobnosti o vybrané produktu jsou zobrazeny v DetailsView](master-detail-filtering-with-two-dropdownlists-vb/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image52.png)

**Obrázek 18**: The vybraný produkt podrobnosti jsou zobrazeny v DetailsView ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image54.png))


Pokud změníte výběr kategorie z nápoje produkty koření, dojde k zpětné volání a `ProductsByCategory` DropDownList se podle nich aktualizuje, ale ovládacím prvku DetailsView stále zobrazuje podrobnosti pro Chai.


[![Podrobnosti o dříve vybrané produktu jsou pořád zobrazuje](master-detail-filtering-with-two-dropdownlists-vb/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image55.png)

**Obrázek 19**: dříve vybrané na podrobnosti o produktu jsou pořád zobrazuje ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image57.png))


Výběr nového produktu v seznamu aktualizuje ovládacím prvku DetailsView podle očekávání. Pokud vyberete novou kategorii po změně produktu, nebude aktualizovat znovu ovládacím prvku DetailsView. Pokud místo výběru nového produktu vyberete novou kategorii, by aktualizovat ovládacím prvku DetailsView. V celém světě k čemu tady?

Problém je časování životního cyklu stránky. Na stránce je vždy, když požadovaného že pokračuje několika kroky jako jeho vykreslování. V jednom z těchto kroků ObjectDataSource řídí zkontrolujte, zda má některý jejich `SelectParameters` změní hodnoty. Pokud tedy ovládací prvek webových dat vázány na ObjectDataSource ví, že je nutné aktualizovat zobrazení. Například když novou kategorii je vybrali, `ProductsByCategoryDataSource` ObjectDataSource zjistí, že došlo ke změně jeho hodnoty parametrů a `ProductsByCategory` DropDownList znovu připojí, získávání produkty pro vybrané kategorie.

Problém, který nastane v takové situaci je, že dojde k časovému životního cyklu stránky, která ObjectDataSources vyhledat změněné parametry *před* obnovení vazeb přidružená data webové ovládací prvky. Proto se při výběru nové kategorie `ProductsByCategoryDataSource` ObjectDataSource zjistí změnu v hodnotě její parametr. Prvek ObjectDataSource používá `ProductDetails` DetailsView, ale nebude mějte na paměti tyto změny protože `ProductsByCategory` DropDownList ještě musíme být znovu připojeno. Dále v životní cyklus `ProductsByCategory` DropDownList znovu připojí k jeho ObjectDataSource uchopíte jeho produkty pro nově vybranou kategorii. Zatímco `ProductsByCategory` DropDownList hodnota změnila, `ProductDetails` ObjectDataSource ovládacího prvku DetailsView již provedla kontrola hodnoty jeho parametrů; proto ovládacím prvku DetailsView zobrazí jeho předchozí výsledky. Tato interakce je znázorněno na obrázku 20.


[![Po prvku ProductDetails DetailsView ObjectDataSource kontroluje změny při změně hodnoty ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image58.png)

**Obrázek 20**: `ProductsByCategory` DropDownList hodnotu změny po `ProductDetails` ovládacího prvku DetailsView ObjectDataSource kontroluje změny při ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image60.png))


Quota.MD potřebujeme explicitně znovu připojit `ProductDetails` DetailsView po `ProductsByCategory` DropDownList byla svázána. Jsme toho dosáhli voláním `ProductDetails` ovládacího prvku DetailsView `DataBind()` metoda při `ProductsByCategory` společnosti DropDownList `DataBound` dojde k aktivaci události. Přidejte následující kód obslužné rutiny událostí, který `MasterDetailsDetails.aspx` třídy modelu code-behind stránky (odkazovat "[programové nastavení hodnot parametru ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" informace o tom, jak přidat obslužnou rutinu události):


[!code-vb[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample2.vb)]

Po tomto explicitní volání konstruktoru `ProductDetails` ovládacího prvku DetailsView `DataBind()` metoda byla přidána, tento kurz pracuje podle očekávání. Obrázek 21 stručný přehled, jak toto nastavení změnit napravit naše starší problémy.


[![Prvek ProductDetails DetailsView je dojde k aktivaci události explicitně aktualizují při the ProductsByCategory DropDownList vaší datové vazby](master-detail-filtering-with-two-dropdownlists-vb/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image61.png)

**Obrázek 21**: `ProductDetails` DetailsView je explicitně aktualizují při `ProductsByCategory` společnosti DropDownList `DataBound` dojde k aktivaci události ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-vb/_static/image63.png))


## <a name="summary"></a>Souhrn

DropDownList slouží jako element ideální uživatelské rozhraní pro sestavy záznamů master/detail níž se nachází vztah jeden mnoho mezi záznamy a podrobností. V předchozím kurzu jsme viděli, jak použít jeden DropDownList k filtrování produktů zobrazí vybranou kategorii. V tomto kurzu jsme nahradit GridView produkty ovládacím prvkem DropDownList a DetailsView slouží k zobrazení podrobností vybranému produktu. Principy probírané v tomto kurzu je možné snadno rozšířit na datové modely zahrnující více vztahů jednoho k několika, jako je například zákazníky, objednávky a pořadí položek. Obecně platí můžete vždy přidat DropDownList u každé z "jedna" entit u vztahů jednoho k několika.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Hilton Giesenow. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](master-detail-filtering-with-a-dropdownlist-vb.md)
> [další](master-detail-filtering-across-two-pages-vb.md)
