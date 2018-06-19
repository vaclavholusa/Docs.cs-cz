---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: Filtrování s dvěma DropDownLists (C#) podrobností | Microsoft Docs
author: rick-anderson
description: V tomto kurzu rozšíří vztahu seznam podrobnosti k přidání třetí vrstvy, pomocí dvou ovládacích prvků rozevírací seznam a vyberte požadované nadřazených a nadřazený recor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: d971dcb3814dc088202c3a3e4addb03375049ca0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887249"
---
<a name="masterdetail-filtering-with-two-dropdownlists-c"></a>Seznam podrobnosti filtrování s dvěma DropDownLists (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe) nebo [stáhnout PDF](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> V tomto kurzu rozšíří vztahu seznam podrobnosti k přidání třetí vrstvy, pomocí dvou ovládacích prvků rozevírací seznam a vyberte požadované záznamy nadřazené a nadřazený.


## <a name="introduction"></a>Úvod

V [předchozí kurzu](master-detail-filtering-with-a-dropdownlist-cs.md) jsme se zaměřili na tom, jak zobrazit sestavu jednoduché hlavní/podrobnosti o použití jedné rozevírací seznam, naplní kategorie a GridView, zobrazuje tyto produkty, které patří do vybrané kategorie. Tento vzor sestavy funguje dobře, pokud záznamy, které mají vztah jeden mnoho a lze snadno rozšířit pro scénáře, které zahrnují více vztahů na více zobrazení. Například systém vstup pořadí by měla mít tabulky, které odpovídají zákazníky, objednávky a pořadí řádky. Daný zákazník může mít několik objednávek se každý pořadí, který se skládá z více položek. Taková data můžete pro uživatele s dvěma DropDownLists a GridView zobrazí. První rozevírací seznam by mohlo mít položku seznamu pro každého zákazníka a to v databázi s druhou jeden na obsah se objednávky vybraného zákazníka. GridView by seznam položek řádku z vybrané pořadí.

Zatímco databáze Northwind zahrnují informace kanonický zákazníka objednávek podrobnosti v jeho `Customers`, `Orders`, a `Order Details` tabulky, tyto tabulky nejsou zaznamenané v Naše architektura. Nicméně jsme můžete stále ilustrují použití dvou DropDownLists závislé. První rozevírací seznam se zobrazí seznam kategorií a druhý produkty, které patří do vybrané kategorie. DetailsView se potom zobrazí seznam podrobnosti vybrané produktu.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Krok 1: Vytvoření a naplnění rozevírací seznam kategorií

Naším cílem první je přidání rozevírací seznam, který obsahuje seznam kategorií. Tyto kroky byly prozkoumány podrobně v předchozím kurzu, ale jsou zde souhrnu pro úplnost.

Otevřete `MasterDetailsDetails.aspx` stránky v `Filtering` složky, přidejte na stránku rozevírací seznam nastavit jeho `ID` vlastnost `Categories`a pak klikněte na odkaz Konfigurace zdroje dat do jeho inteligentní značky. Z Průvodce konfigurací zdroje dat vyberte, chcete-li přidat nový zdroj dat.


[![Přidat nový zdroj dat pro rozevírací seznam](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**Obrázek 1**: Přidat nový zdroj dat pro rozevírací seznam ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))


Nový zdroj dat musí být samozřejmě ObjectDataSource. Název této nové ObjectDataSource `CategoriesDataSource` a mějte ho vyvolání `CategoriesBLL` objektu `GetCategories()` metoda.


[![Rozhodnete použít CategoriesBLL – třída](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**Obrázek 2**: Vyberte pro použití `CategoriesBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))


[![Konfigurace ObjectDataSource lze pomocí této metody GetCategories()](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**Obrázek 3**: Konfigurace ObjectDataSource pro použití `GetCategories()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))


Po dokončení konfigurace ObjectDataSource stále potřebujeme k určení, které pole datového zdroje, které má být zobrazena v `Categories` rozevírací seznam a které z nich by měl být nakonfigurovaný jako hodnotu pro položku seznamu. Nastavte `CategoryName` pole jako zobrazení a `CategoryID` jako hodnota pro každou položku seznamu.


[![Mají zobrazení rozevírací seznam CategoryName pole a použití CategoryID jako hodnota](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**Obrázek 4**: mít zobrazení rozevírací seznam `CategoryName` pole a použití `CategoryID` jako hodnotu ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))


V tomto okamžiku máme ovládací prvek rozevírací seznam (`Categories`), se zobrazí v záznamy ze `Categories` tabulky. Když uživatel vybere novou kategorii z rozevírací seznam chceme budete postback dojít k aktualizaci produktu rozevírací seznam, který vytvoříme vytvořit v kroku 2. Proto zkontrolujte možnost Povolit automatické odeslání `categories` inteligentních značek na rozevírací seznam.


[![Povolit automatické odeslání pro rozevírací seznam kategorií](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**Obrázek 5**: Povolit automatické odeslání pro `Categories` rozevírací seznam ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Krok 2: Zobrazení vybrané kategorie produktů v druhé rozevírací seznam

Pomocí `Categories` rozevírací seznam dokončen, naše dalším krokem je zobrazíte rozevírací seznam produktů, které patří do vybrané kategorie. K tomu, přidejte na stránku s názvem jiné rozevírací seznam `ProductsByCategory`. Stejně jako u `Categories` rozevírací seznam, vytvoření nové ObjectDataSource pro `ProductsByCategory` rozevírací seznam s názvem `ProductsByCategoryDataSource`.


[![Přidat nový zdroj dat pro rozevírací seznam ProductsByCategory](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**Obrázek 6**: Přidat nový zdroj dat pro `ProductsByCategory` rozevírací seznam ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))


[![Vytvořit nový ObjectDataSource s názvem ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**Obrázek 7**: vytvoření nové ObjectDataSource s názvem `ProductsByCategoryDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))


Vzhledem k tomu `ProductsByCategory` rozevírací seznam zobrazit pouze tyto produkty, které patří do vybrané kategorie, musí mít ObjectDataSource vyvolání `GetProductsByCategoryID(categoryID)` metoda z `ProductsBLL` objektu.


[![Rozhodnete použít ProductsBLL – třída](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**Obrázek 8**: Vyberte pro použití `ProductsBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))


[![Konfigurace ObjectDataSource lze pomocí této metody GetProductsByCategoryID(categoryID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**Obrázek 9**: Konfigurace ObjectDataSource pro použití `GetProductsByCategoryID(categoryID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))


V posledním kroku průvodce, je potřeba zadat hodnotu *`categoryID`* parametr. Tento parametr přiřadit vybrané položky ze `Categories` rozevírací seznam.


[![Hodnota parametru categoryID pro vyžádání obsahu z rozevírací seznam kategorií](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**Obrázek 10**: pro vyžádání obsahu *`categoryID`* hodnota parametru `Categories` rozevírací seznam ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))


S ObjectDataSource nakonfigurované zbývá určit, jaké pole zdroje dat se používají pro zobrazení a hodnoty rozevírací seznam položky. Zobrazení `ProductName` pole a použít `ProductID` pole jako hodnotu.


[![Zadejte pole zdroje dat použít pro Text a hodnotu vlastnosti položky rozevírací seznam ListItems.](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**Obrázek 11**: Zadejte pole zdroj dat používá pro rozevírací seznam `ListItem` s' `Text` a `Value` vlastnosti ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))


S ObjectDataSource a `ProductsByCategory` rozevírací seznam nakonfigurované naše stránky zobrazí dvě DropDownLists: první zobrazí seznam všechny kategorie při druhý zobrazí seznam těchto produktů, které patří do vybrané kategorie. Když si uživatel vybere ze první rozevírací seznam novou kategorii, výsledkem by měla být zpětné volání a druhý rozevírací seznam se odrážejí, zobrazuje tyto produkty, které patří do nově zvolené kategorii. Obrázky 12 a 13 zobrazit `MasterDetailsDetails.aspx` v akci při zobrazení prostřednictvím prohlížeče.


[![Při první návštěvě stránky, je vybrané kategorie Nápoje](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**Obrázek 12**: při první návštěvě stránky, je vybrané kategorie nápoje ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))


[![Výběr různé kategorie zobrazuje nové kategorie produktů](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**Obrázek 13**: výběr nové kategorie produktů různé kategorie zobrazí ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))


Aktuálně `productsByCategory` rozevírací seznam, pokud změněn, nemá *není* způsobit zpětné volání. Ale chceme se zpětné volání proběhnout po přidáme DetailsView si můžete zobrazit podrobnosti vybrané produktu (krok 3). Proto, zaškrtněte políčko Povolit automatické odeslání z `productsByCategory` inteligentních značek na rozevírací seznam.


[![Povolit automatické odeslání funkci pro productsByCategory rozevírací seznam](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**Obrázek 14**: Povolit automatické odeslání funkci pro `productsByCategory` rozevírací seznam ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Krok 3: Použití DetailsView si můžete zobrazit podrobnosti pro vybranou produktu

Posledním krokem je zobrazíte podrobnosti vybrané produktu v DetailsView. DetailsView dosáhnout, přidat na stránku, nastavte jeho `ID` vlastnost `ProductDetails`a vytvořit nové ObjectDataSource pro ni. Konfigurace této ObjectDataSource načítat data z `ProductsBLL` třídy `GetProductByProductID(productID)` metoda pomocí vybrané hodnoty `ProductsByCategory` rozevírací seznam pro hodnotu *`productID`* parametr.


[![Rozhodnete použít ProductsBLL – třída](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**Obrázek 15**: Vyberte pro použití `ProductsBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))


[![Konfigurace ObjectDataSource lze pomocí této metody GetProductByProductID(productID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**Obrázek 16**: Konfigurace ObjectDataSource pro použití `GetProductByProductID(productID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))


[![ProductID hodnota parametru pro vyžádání obsahu z ProductsByCategory rozevírací seznam](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**Obrázek 17**: pro vyžádání obsahu *`productID`* hodnota parametru `ProductsByCategory` rozevírací seznam ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))


Můžete zobrazit všechny dostupné polí v ovládacím prvku DetailsView. Jste se rozhodli odebrat `ProductID`, `SupplierID`, a `CategoryID` polí a přeuspořádány a formátu zbývající pole. Kromě toho vymazán out prvku DetailsView `Height` a `Width` vlastnosti, což DetailsView rozšiřovat na šířku potřebné pro nejlepší zobrazení data, místo aby je omezené na zadanou velikost. Úplný kód se zobrazí níže:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

Pozorně si můžete vyzkoušet `MasterDetailsDetails.aspx` stránku v prohlížeči. Na první pohled může zdát, že vše funguje podle potřeby, ale došlo k potížím jemně. Pokud vyberete novou kategorii `ProductsByCategory` rozevírací seznam se aktualizuje produktů pro vybranou kategorii, ale `ProductDetails` DetailsView pokračovala v předchozí informace o produktu. DetailsView je aktualizována při výběru jiného produktu pro vybranou kategorii. Kromě toho, pokud je dostatečně důkladně otestovat, budete zjistíte, že pokud se rozhodnete průběžně nové kategorie (například zvolit nápoje z `Categories` rozevírací seznam a potom přísady, pak Confections) každých výběru kategorie způsobí, že `ProductDetails`DetailsView nutné aktualizovat.

Chcete-li tento problém upřesnění, podíváme se na konkrétní příklad. Při první návštěvě stránky je vybraná kategorie Nápoje a související produkty jsou načteny v `ProductsByCategory` rozevírací seznam. Chai je vybrané produkt a jeho podrobnosti jsou zobrazeny v `ProductDetails` DetailsView, jak ukazuje obrázek 18.


[![Podrobnosti o produktu vybrané se zobrazují v DetailsView](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**Obrázek 18**: vybrané produkt podrobnosti jsou zobrazeny v DetailsView ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))


Pokud změníte výběru kategorie ze nápojů na přísady, dojde k zpětné volání a `ProductsByCategory` rozevírací seznam se příslušným způsobem aktualizuje, ale DetailsView stále zobrazí podrobnosti pro Chai.


[![Dříve vybrané produktu podrobnosti jsou pořád zobrazuje](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**Obrázek 19**: dříve vybrané produkt podrobnosti jsou pořád zobrazuje ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))


Výběr nového produktu ze seznamu aktualizuje DetailsView podle očekávání. Pokud vyberete novou kategorii po změně produktu, nebude aktualizovat znovu DetailsView. Pokud místo výběru nového produktu vyberete novou kategorii, by aktualizovat DetailsView. Co v celém světě se děje tady?

Problém je časové potíže v průběhu životního cyklu stránky. Na stránce je vždy, když požadovaného že pokračuje prostřednictvím několika kroků jako vykreslování. V jednom z těchto kroků ObjectDataSource řídí zkontrolujte, zda má některý jejich `SelectParameters` hodnoty změnily. Pokud ano, ovládací prvek webu data propojena se ObjectDataSource ví, že je nutné aktualizovat jeho zobrazení. Pokud se například zaškrtnuto novou kategorii, `ProductsByCategoryDataSource` ObjectDataSource zjistí, že došlo ke změně jeho hodnoty parametrů a `ProductsByCategory` rozevírací seznam znovu připojí samostatně, získávání produkty pro vybrané kategorie.

Problém, který nastane v této situaci je, že dojde k bodu v průběhu životního cyklu stránky, který ObjectDataSources Zkontrolujte změněné parametry *před* obnovení vazeb ovládací prvky webového přidružená data. Proto když vyberete novou kategorii `ProductsByCategoryDataSource` ObjectDataSource zjistí změnu v hodnotě její parametr. ObjectDataSource používané `ProductDetails` DetailsView, však není Poznámka: tyto změny `ProductsByCategory` rozevírací seznam dosud být odrážejí. Dále v životním cyklu `ProductsByCategory` rozevírací seznam znovu připojí k jeho ObjectDataSource metodou produktů pro nově zvolené kategorii. Při `ProductsByCategory` došlo ke změně hodnoty na rozevírací seznam `ProductDetails` prvku DetailsView ObjectDataSource již provedl jeho kontrola hodnoty parametru; proto DetailsView zobrazí jeho předchozí výsledky. Tato interakce je znázorněno na obrázku 20.


[![Hodnota rozevírací seznam ProductsByCategory změny po ProductDetails DetailsView ObjectDataSource kontroluje změny](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**Obrázek 20**: `ProductsByCategory` rozevírací seznam hodnotu změny po `ProductDetails` prvku DetailsView ObjectDataSource kontroluje změny ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))


Chcete-li opravit to je potřeba explicitně rebind `ProductDetails` DetailsView po `ProductsByCategory` byla svázána rozevírací seznam. Jsme se dá dosáhnout pomocí volání `ProductDetails` prvku DetailsView `DataBind()` metoda při `ProductsByCategory` na rozevírací seznam `DataBound` je aktivována událost. Přidejte následující kód obslužné rutiny události pro `MasterDetailsDetails.aspx` třídy kódu stránky (odkazovat "[prostřednictvím kódu programu nastavování hodnot parametrů ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" podrobné informace o tom, jak přidat obslužné rutiny události):


[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

Po explicitní volání `ProductDetails` prvku DetailsView `DataBind()` metoda byla přidána, kurz funguje podle očekávání. Obrázek 21 označuje, jak to změnit napravit naše starší problém.


[![ProductDetails DetailsView se aktivuje událost Vycentrovat explicitně aktualizovat, když ProductsByCategory rozevírací seznam pro](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**Obrázek 21**: `ProductDetails` DetailsView je explicitně aktualizovat, když `ProductsByCategory` na rozevírací seznam `DataBound` aktivuje událost ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))


## <a name="summary"></a>Souhrn

Rozevírací seznam slouží jako element ideální uživatelské rozhraní pro sestavy a podrobností tam, kde je vztah jeden mnoho mezi záznamy seznamu a podrobností. V předchozím kurzu jsme viděli, jak používat jeden rozevírací seznam pro filtrování produktů zobrazí ve vybrané kategorie. V tomto kurzu jsme nahradí GridView produktů rozevírací seznam a použít DetailsView k zobrazení podrobností o vybrané produktu. Principy probírané v tomto kurzu lze snadno rozšířit o datových modelech zahrnující více vztahů na více, jako je například zákazníky, objednávky a pořadí položek. Obecně platí můžete vždy přidat rozevírací seznam pro každou "1" entit v na více relací.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Hilton Giesenow. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](master-detail-filtering-with-a-dropdownlist-cs.md)
> [další](master-detail-filtering-across-two-pages-cs.md)
