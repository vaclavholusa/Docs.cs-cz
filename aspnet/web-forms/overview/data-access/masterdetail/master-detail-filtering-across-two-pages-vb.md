---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
title: "Filtrování napříč dvěma stránkami (VB) podrobností | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu jsme budete implementovat tento vzor pomocí GridView seznam dodavatelů v databázi. Každý řádek dodavatele GridView bude obsahovat Vie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 361d6a44-3f1f-4daf-85df-d4c2b8bf065d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: d8d43475ae2f05d83d51c53d76de4f07db70ca7a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Filtrování napříč dvěma stránkami (VB) podrobností
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_9_VB.exe) nebo [stáhnout PDF](master-detail-filtering-across-two-pages-vb/_static/datatutorial09vb1.pdf)

> V tomto kurzu jsme budete implementovat tento vzor pomocí GridView seznam dodavatelů v databázi. Každý řádek dodavatele GridView bude obsahovat odkaz zobrazit produkty, že při kliknutí na, přenese uživatele na zvláštní stránce, jsou uvedené tyto produkty pro vybrané dodavatele.


## <a name="introduction"></a>Úvod

V předchozích dvou kurzech jsme viděli zobrazení sestavy a podrobností v jedné webové stránce pomocí DropDownLists zobrazíte "hlavní" záznamy a [GridView](master-detail-filtering-with-a-dropdownlist-vb.md) nebo [DetailsView](master-detail-filtering-with-two-dropdownlists-vb.md) ovládacího prvku pro zobrazení " Podrobnosti o." Další běžné způsobem používaným pro hlavní/podrobnosti sestavy se se hlavní záznamů na jedné webové stránky a zobrazí na další podrobnosti. Fórum pro web, například [fóra ASP.NET](https://forums.asp.net/), je skvělým příklad tohoto vzoru v praxi. Fóra ASP.NET se skládají z různých fóra Začínáme, webových formulářů, ovládací prvky prezentace dat a tak dále. Fórum pro každý se skládá z mnoha vláken a každé vlákno se skládá z řady příspěvky. Na domovské stránce ASP.NET fóra jsou uvedeny ve fórech. Kliknutím na fóru whisks vám `ShowForum.aspx` stránka, která zobrazí seznam vláken pro fóra. Kliknutím na vlákno přejdete k `ShowPost.aspx`, který zobrazuje v příspěvcích pro vlákno, které bylo kliknuto.

V tomto kurzu jsme budete implementovat tento vzor pomocí GridView seznam dodavatelů v databázi. Každý řádek dodavatele GridView bude obsahovat odkaz zobrazit produkty, že při kliknutí na, přenese uživatele na zvláštní stránce, jsou uvedené tyto produkty pro vybrané dodavatele.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Krok 1: Přidání`SupplierListMaster.aspx`a`ProductsForSupplierDetails.aspx`stránky k`Filtering`složky

Při definování rozložení stránky v třetí kurzu jsme přidali počet stránek "počáteční" v `BasicReporting`, `Filtering`, a `CustomFormatting` složek. Ale jsme nepřidali úvodní stránky pro tento kurz v daném čase, takže pozorně přidejte dva nové stránky k `Filtering` složky: `SupplierListMaster.aspx` a `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx`Zobrazí seznam záznamů "hlavní" (dodavatelů) při `ProductsForSupplierDetails.aspx` zobrazí produkty pro vybrané dodavatele.

Při vytváření těchto dvou nových stránek být jistý, a přiřadit je pomocí `Site.master` stránky předlohy.


![Přidejte SupplierListMaster.aspx a ProductsForSupplierDetails.aspx stránky ke složce pro filtrování](master-detail-filtering-across-two-pages-vb/_static/image1.png)

**Obrázek 1**: Přidat `SupplierListMaster.aspx` a `ProductsForSupplierDetails.aspx` stránky k `Filtering` složky


Navíc při přidávání nové stránky na projekt, nezapomeňte aktualizovat souboru mapy webu `Web.sitemap`, odpovídajícím způsobem. V tomto kurzu jednoduše přidat `SupplierListMaster.aspx` stránku a mapy webu pomocí následující obsah XML jako podřízený filtrování sestavy `<siteMapNode>` element:


[!code-xml[Main](master-detail-filtering-across-two-pages-vb/samples/sample1.xml)]

> [!NOTE]
> Automatizovat proces aktualizace souboru mapy webu při přidávání nové ASP.NET stránky pomocí [K. Scott Allen](http://odetocode.com/Blogs/scott/)je volné Visual Studio [makra mapy webu](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Krok 2: Zobrazení seznamu dodavatele v`SupplierListMaster.aspx`

Pomocí `SupplierListMaster.aspx` a `ProductsForSupplierDetails.aspx` stránky vytvořené naše dalším krokem je vytvoření GridView dodavatelů v `SupplierListMaster.aspx`. Přidejte GridView na stránku a navázat jej na nové ObjectDataSource. Tato ObjectDataSource by měl používat `SuppliersBLL` třídy `GetSuppliers()` metody, která vrátí všechny dodavatelů.


[![Vyberte třídu SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image3.png)](master-detail-filtering-across-two-pages-vb/_static/image2.png)

**Obrázek 2**: vyberte `SuppliersBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image4.png))


[![Konfigurace ObjectDataSource lze pomocí této metody GetSuppliers()](master-detail-filtering-across-two-pages-vb/_static/image6.png)](master-detail-filtering-across-two-pages-vb/_static/image5.png)

**Obrázek 3**: Konfigurace ObjectDataSource pro použití `GetSuppliers()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image7.png))


Musíme vložit odkaz s názvem zobrazit produkty v jednotlivých řádcích GridView, po kliknutí na uživatele na `ProductsForSupplierDetails.aspx` předávání v vybraného řádku `SupplierID` hodnotné řetězec dotazu. Například, pokud uživatel klikne na odkaz zobrazit produkty pro dodavatele Tokio Traders (který má `SupplierID` hodnotu 4), by měly být odeslány na `ProductsForSupplierDetails.aspx?SupplierID=4`.

Chcete-li dosáhnout, přidejte [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) k GridView, který přidá hypertextový odkaz na každý řádek GridView. Spusťte kliknutím na odkaz Upravit sloupce z prvku GridView inteligentních značek. V dalším kroku vyberte HyperLinkField ze seznamu v levém horním a klikněte na tlačítko Přidat mají být zahrnuty HyperLinkField prvku GridView seznam polí.


[![Přidání HyperLinkField do GridView](master-detail-filtering-across-two-pages-vb/_static/image9.png)](master-detail-filtering-across-two-pages-vb/_static/image8.png)

**Obrázek 4**: Přidání HyperLinkField do GridView ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image10.png))


HyperLinkField lze nakonfigurovat k využívání stejný text nebo adresa URL hodnoty na odkaz v jednotlivých řádcích GridView nebo může základní tyto hodnoty u hodnot data vázaná na každý řádek konkrétní. Určení statické hodnotu mezi všechny řádky, použijte HyperLinkField `Text` nebo `NavigateUrl` vlastnosti. Vzhledem k tomu, že chceme text odkazu, který má být stejná pro všechny řádky, nastavte HyperLinkField `Text` vlastnost zobrazit produkty.


[![Nastavte vlastnost HyperLinkField Text na zobrazení produktů](master-detail-filtering-across-two-pages-vb/_static/image12.png)](master-detail-filtering-across-two-pages-vb/_static/image11.png)

**Obrázek 5**: nastavte HyperLinkField `Text` vlastnost zobrazit produkty ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image13.png))


Nastavit text nebo adresa URL hodnoty byl založen na základní data vázaná na GridView řádek, zadejte data pole text nebo hodnoty adresa URL by měla být stažen z v `DataTextField` nebo `DataNavigateUrlFields` vlastnosti. `DataTextField`můžete nastavit pouze na jedno datové pole; `DataNavigateUrlFields`, ale může být nastaven na seznam s položkami oddělenými čárkou datová pole. Musíme často základní text nebo adresy URL na kombinaci hodnotu pole na aktuálním řádku dat a některé statické značek. V tomto kurzu, například chceme adresu URL odkazy HyperLinkField být `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, kde  *`supplierID`*  je každý GridView řádek `SupplierID` hodnotu. Všimněte si, že potřebujeme statické a datové hodnoty zde: `ProductsForSupplierDetails.aspx?SupplierID=` část adresy URL, na odkaz je statická, zatímco  *`supplierID`*  část je řízené daty jako jeho hodnota může být každý řádek na vlastní `SupplierID` hodnotu.

Pokud chcete zadat kombinaci statických a datové hodnoty, použijte `DataTextFormatString` a `DataNavigateUrlFormatString` vlastnosti. V těchto vlastností zadejte kód statické podle potřeby a potom pomocí značky `{0}` místo hodnotě zadané v pole `DataTextField` nebo `DataNavigateUrlFields` vlastnosti, které chcete zobrazit. Pokud `DataNavigateUrlFields` vlastnost má více polí zadané použití `{0}` tam, kde chcete vložit, první hodnota pole `{1}` pro druhý hodnota pole a tak dále.

Použití to na našem kurzu, je potřeba nastavit `DataNavigateUrlFields` vlastnost `SupplierID`, protože se jedná o datové pole, jehož hodnota je potřeba upravit na základě na řádek, a `DataNavigateUrlFormatString` vlastnost `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Konfigurace HyperLinkField zahrnout správnou adresu URL odkazu na základě pole SupplierID](master-detail-filtering-across-two-pages-vb/_static/image15.png)](master-detail-filtering-across-two-pages-vb/_static/image14.png)

**Obrázek 6**: Konfigurace HyperLinkField zahrnout správné odkaz adresy URL na základě při `SupplierID` ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image16.png))


Po přidání HyperLinkField, klidně přizpůsobit a změna pořadí polí GridView. Následující kód ukazuje GridView poté, co jste provedli v některé menší úpravy na úrovni pole.


[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample2.aspx)]

Za chvíli zobrazíte `SupplierListMaster.aspx` stránku prostřednictvím prohlížeče. Jak je vidět na obrázku 7, stránce aktuálně zobrazuje seznam všech dodavatelů, včetně odkaz zobrazit produkty. Kliknutím na Zobrazit produkty odkazu přejdete na `ProductsForSupplierDetails.aspx`, předejte podél dodavatele `SupplierID` v řetězci dotazu.


[![Každý řádek dodavatele obsahuje odkaz produkty zobrazení](master-detail-filtering-across-two-pages-vb/_static/image18.png)](master-detail-filtering-across-two-pages-vb/_static/image17.png)

**Obrázek 7**: každý dodavatele řádek obsahuje odkaz produkty zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Krok 3: Výpis přísluší v`ProductsForSupplierDetails.aspx`

V tomto okamžiku `SupplierListMaster.aspx` stránky odesílá uživatelům `ProductsForSupplierDetails.aspx`, předávání vybrané dodavatele `SupplierID` v řetězci dotazu. Posledním krokem tohoto kurzu je zobrazíte produkty v GridView v `ProductsForSupplierDetails.aspx` jejichž `SupplierID` rovná `SupplierID` předána v řetězci dotazu. K provedení této počáteční přidáním GridView k `ProductsForSupplierDetails.aspx` stránky, pomocí nového prvku ObjectDataSource s názvem `ProductsBySupplierDataSource` , který spustí `GetProductsBySupplierID(supplierID)` metoda z `ProductsBLL` třídy.


[![Přidat nové ObjectDataSource s názvem ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-vb/_static/image21.png)](master-detail-filtering-across-two-pages-vb/_static/image20.png)

**Obrázek 8**: Přidání nové ObjectDataSource s názvem `ProductsBySupplierDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image22.png))


[![Vyberte třídu ProductsBLL](master-detail-filtering-across-two-pages-vb/_static/image24.png)](master-detail-filtering-across-two-pages-vb/_static/image23.png)

**Obrázek 9**: vyberte `ProductsBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image25.png))


[![Mít ObjectDataSource vyvolání metody GetProductsBySupplierID(supplierID)](master-detail-filtering-across-two-pages-vb/_static/image27.png)](master-detail-filtering-across-two-pages-vb/_static/image26.png)

**Obrázek 10**: mít ObjectDataSource vyvolání `GetProductsBySupplierID(supplierID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image28.png))


Posledním kroku průvodce Konfigurace zdroje dat požádá, abychom mohli poskytovat zdroj `GetProductsBySupplierID(supplierID)` metody  *`supplierID`*  parametr. Pokud chcete použít hodnotu řetězce dotazu, nastavte parametr zdroj do řetězce dotazu a zadejte název hodnoty řetězce dotazu pro použití v textové pole vlastnost QueryStringField (`SupplierID`).


[![Naplnění KódDodavatele hodnota parametru z hodnoty řetězce dotazu KódDodavatele](master-detail-filtering-across-two-pages-vb/_static/image30.png)](master-detail-filtering-across-two-pages-vb/_static/image29.png)

**Obrázek 11**: naplnění  *`supplierID`*  hodnota parametru `SupplierID` hodnotu řetězce dotazu ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image31.png))


To je všechno je k němu! Obrázek 12 znázorňuje `ProductsForSupplierDetails.aspx` stránky po návštěvě kliknutím na odkaz Tokio Traders z `SupplierListMaster.aspx`.


[![Jsou uvedeny produkty poskytl Traders Tokio](master-detail-filtering-across-two-pages-vb/_static/image33.png)](master-detail-filtering-across-two-pages-vb/_static/image32.png)

**Obrázek 12**: produkty poskytl Tokio Traders jsou uvedeny ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Zobrazení informací o dodavatele v`ProductsForSupplierDetails.aspx`

Jak ukazuje obrázek 12, `ProductsForSupplierDetails.aspx` stránce jednoduše jsou uvedeny produkty, které jsou dodány nástrojem `SupplierID` zadaný v řetězci dotazu. Někdo se odešle přímo na této stránce však nebude vědět, že obrázek 12 se zobrazuje Tokio Traders produkty. Chcete-li opravit to že zobrazujeme dodavatele informace na této stránce také.

Začněte přidáním FormView výše produkty GridView. Vytvoření nového ovládacího prvku ObjectDataSource s názvem `SuppliersDataSource` , který spustí `SuppliersBLL` třídy `GetSupplierBySupplierID(supplierID)` metoda.


[![Vyberte třídu SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image36.png)](master-detail-filtering-across-two-pages-vb/_static/image35.png)

**Obrázek 13**: vyberte `SuppliersBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image37.png))


[![Mít ObjectDataSource vyvolání metody GetSupplierBySupplierID(supplierID)](master-detail-filtering-across-two-pages-vb/_static/image39.png)](master-detail-filtering-across-two-pages-vb/_static/image38.png)

**Obrázek 14**: mít ObjectDataSource vyvolání `GetSupplierBySupplierID(supplierID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image40.png))


Stejně jako u `ProductsBySupplierDataSource`, mají  *`supplierID`*  parametr přiřadí hodnota `SupplierID` hodnotu řetězce dotazu.


[![Naplnění KódDodavatele hodnota parametru z hodnoty řetězce dotazu KódDodavatele](master-detail-filtering-across-two-pages-vb/_static/image42.png)](master-detail-filtering-across-two-pages-vb/_static/image41.png)

**Obrázek 15**: naplnění  *`supplierID`*  hodnota parametru z `SupplierID` hodnotu řetězce dotazu ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image43.png))


Při vytváření vazby FormView pro ObjectDataSource v zobrazení návrhu, vytvoří sada Visual Studio automaticky FormView `ItemTemplate`, `InsertItemTemplate`, a `EditItemTemplate` s popisek a textové pole webové ovládací prvky pro každé datové pole vrácené ObjectDataSource. Vzhledem k tomu, že my chceme jenom zobrazit dodavatele informace o chování volné odebrat `InsertItemTemplate` a `EditItemTemplate`. Potom upravte ItemTemplate tak, aby zobrazil název společnosti dodavatele v `<h3>` elementu a adresu, města, země a telefonní číslo pod název společnosti. Alternativně můžete ručně nastavit FormView `DataSourceID` a vytvořte `ItemTemplate` značek, jako jsme to udělali zpět v "[zobrazení dat pomocí ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" kurzu.

Po tyto úpravy FormView deklarativní by měl vypadat takto:


[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample3.aspx)]

Snímek obrazovky ukazuje obrázek 16 `ProductsForSupplierDetails.aspx` stránce po zahrnuté dodavatele informace popsané výše.


[![Seznam produktů obsahuje souhrnné informace o dodavateli](master-detail-filtering-across-two-pages-vb/_static/image45.png)](master-detail-filtering-across-two-pages-vb/_static/image44.png)

**Obrázek 16**: seznam produktů obsahuje souhrnné informace o the dodavatele ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Použití konečné dotykem pro`ProductsForSupplierDetails.aspx`uživatelského rozhraní

Pro zlepšení uživatele je prostředí pro tuto sestavu existuje několik dodatky jsme by mělo být provádět `ProductsForSupplierDetails.aspx` stránky. Aktuálně je jediným způsobem, uživatel může přecházet od `ProductsForSupplierDetails.aspx` stránka zpět do seznamu dodavatelů je kliknout na tlačítko Zpět svého prohlížeče. Umožňuje přidání ovládacího prvku hypertextový odkaz `ProductsForSupplierDetails.aspx` stránky, který je zpětně `SupplierListMaster.aspx`, poskytuje jiný způsob, jak uživatelům vrátit do seznamu hlavní.


[![Přidání ovládacího prvku hypertextový odkaz pro uživatele zpět SupplierListMaster.aspx](master-detail-filtering-across-two-pages-vb/_static/image48.png)](master-detail-filtering-across-two-pages-vb/_static/image47.png)

**Obrázek 17**: Přidání ovládacího prvku hypertextový odkaz uživatele zpět provést `SupplierListMaster.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image49.png))


Pokud uživatel klikne na odkaz zobrazit produkty pro dodavatele, který nemá všechny produkty `ProductsBySupplierDataSource` ObjectDataSource v `ProductsForSupplierDetails.aspx` nevrátí žádné výsledky. Rutina GridView vázána ObjectDataSource nebude vykreslovat všechny značky, což vede k prázdnou oblast na stránce v prohlížeči uživatele. Pro více zřetelněji sdělují uživateli, že neexistují žádné produkty spojené s vybranou dodavatele jsme můžete nastavit GridView `EmptyDataText` vlastnost na zprávu jsme chcete zobrazit, když tato situace nastane. Nastavení této vlastnosti "Nejsou žádné produkty od dodavatele této"

Ve výchozím nastavení zadejte všechny dodavatelé v databázi Lhota alespoň jeden produktu. Ale v tomto kurzu jste ručně došlo `Products` tabulky tak, aby dodavatele Escargots Nouveaux již není přidružena všechny produkty. Obrázek 18 ukazuje stránce podrobností pro Escargots Nouveaux po této změně.


[![Uživatelé byli informováni, že dodavatele neposkytuje všechny produkty](master-detail-filtering-across-two-pages-vb/_static/image51.png)](master-detail-filtering-across-two-pages-vb/_static/image50.png)

**Obrázek 18**: uživatelé informováni, že dodavatele neposkytuje všechny produkty ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-across-two-pages-vb/_static/image52.png))


## <a name="summary"></a>Souhrn

Při a podrobností sestavy můžete zobrazit hlavní i podrobné záznamy na jedné stránce, v mnoha webů budou odděleny mezi dvěma webové stránky. V tomto kurzu jsme se podívali na tom, jak implementovat a podrobností sestavu tak, že jsou uvedené v GridView na webové stránce "hlavní" dodavatelů a přidružené produkty uvedené na stránce "Podrobnosti". Každý řádek dodavatele hlavní webové stránky obsahovala odkaz na stránku podrobností, které se předají na řádku `SupplierID` hodnotu. Tyto odkazy specifické pro řádek lze snadno přidat pomocí prvku GridView HyperLinkField.

Na stránce podrobností načítání těchto produktů pro zadaný dodavatele dosáhlo vyvoláním `ProductsBLL` třídy `GetProductsBySupplierID(supplierID)` metoda. *`supplierID`*  Byla zadána hodnota parametru deklarativně pomocí řetězce dotazu jako zdroj parametru. Také jsme se podívali na tom, jak zobrazit podrobnosti dodavatele na stránce Podrobnosti o použití FormView.

Naše [další kurz](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) je poslední v sestavách a podrobností. Podíváme jak zobrazit seznam produktů v GridView, kde má každý řádek vyberte tlačítko. Kliknutím na tlačítko vyberte zobrazí podrobnosti o tomto produktu v ovládacím prvku DetailsView na stejné stránce.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Hilton Giesenow. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](master-detail-filtering-with-two-dropdownlists-vb.md)
[další](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)
