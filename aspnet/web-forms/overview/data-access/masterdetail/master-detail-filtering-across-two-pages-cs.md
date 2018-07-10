---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Na dvou stránkách (C#) filtrování záznamů Master/Detail | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu jsme vám tento model implementovat použitím GridView seznam dodavatelů v databázi. Každý řádek dodavatele v prvku GridView, bude obsahovat Vie...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 13f5a13ac33ec9dfbff1952bc3bbb4dc951ab139
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823976"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Na dvou stránkách (C#) filtrování záznamů Master/Detail
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) nebo [stahovat PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> V tomto kurzu jsme vám tento model implementovat použitím GridView seznam dodavatelů v databázi. Každý řádek dodavatele v prvku GridView, bude obsahovat odkaz zobrazit produkty, že při kliknutí na, přenese uživatele na zvláštní stránce, který obsahuje tyto produkty pro vybrané dodavatele.


## <a name="introduction"></a>Úvod

V předchozích dvou kurzů jsme viděli jak [zobrazení záznamů master/detail sestav v jedné webové stránky s využitím DropDownLists](master-detail-filtering-with-a-dropdownlist-cs.md) k [zobrazení "hlavní" záznamy a ovládacího prvku GridView nebo prvku DetailsView](master-detail-filtering-with-two-dropdownlists-cs.md) zobrazíte " Podrobnosti." Další běžné používaným hlavních/podrobných sestav je hlavní záznamy na jedné webové stránky a podrobné informace uvedené na jiném. Fórum pro web, třeba [fóra ASP.NET](https://forums.asp.net/), je skvělý příkladem tohoto modelu v praxi. Fóra ASP.NET se skládají z mnoha fór Začínáme, webových formulářů, ovládací prvky prezentace dat a tak dále. Každé fórum se skládá z několika vlákny a každé vlákno se skládá z počet příspěvků. Na domovské stránce ASP.NET fóra jsou uvedeny ve fórech. Kliknutím na fórum whisks vám `ShowForum.aspx` stránka, která zobrazí seznam vláken pro fóra. Podobně, kliknutím na vlákno přejdete k `ShowPost.aspx`, který zobrazuje příspěvků pro vlákno, ke které došlo ke kliknutí na.

V tomto kurzu jsme vám tento model implementovat použitím GridView seznam dodavatelů v databázi. Každý řádek dodavatele v prvku GridView, bude obsahovat odkaz zobrazit produkty, že při kliknutí na, přenese uživatele na zvláštní stránce, který obsahuje tyto produkty pro vybrané dodavatele.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Krok 1: Přidání`SupplierListMaster.aspx`a`ProductsForSupplierDetails.aspx`stránky`Filtering`složky

Při definování rozložení stránky ve třetím kurzu jsme přidali počet stránek "počáteční" `BasicReporting`, `Filtering`, a `CustomFormatting` složek. Ale jsme nebyl přidán úvodní stránky pro účely tohoto kurzu v daném čase, a proto věnujte chvíli přidejte dva nové stránky `Filtering` složky: `SupplierListMaster.aspx` a `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` Zobrazí se seznam záznamů "hlavní" (suppliers) při `ProductsForSupplierDetails.aspx` zobrazí produkty pro vybrané dodavatele.

Při vytváření těchto dvou nových stránek si být jisti, a přiřadit je s `Site.master` stránky předlohy.


![Přidat SupplierListMaster.aspx a ProductsForSupplierDetails.aspx stránky ke složce pro filtrování](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Obrázek 1**: Přidat `SupplierListMaster.aspx` a `ProductsForSupplierDetails.aspx` stránky `Filtering` složky


Kromě toho při přidávání nové stránky do projektu, nezapomeňte aktualizovat soubor mapy webu, `Web.sitemap`, odpovídajícím způsobem. Pro účely tohoto kurzu jednoduše přidat `SupplierListMaster.aspx` stránky do mapy webu použijte následující obsah XML jako podřízený objekt sestavy filtrování `<siteMapNode>` element:


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Můžete automatizovat proces aktualizace souboru mapy webu při přidání nové technologie ASP.NET stránek pomocí [K. Scott Allen](http://odetocode.com/Blogs/scott/)uživatele bezplatné sady Visual Studio [makra mapy webu](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Krok 2: Zobrazení seznamu dodavatele v`SupplierListMaster.aspx`

S `SupplierListMaster.aspx` a `ProductsForSupplierDetails.aspx` stránky vytvořené naším dalším krokem je vytvoření prvku GridView dodavatelů v `SupplierListMaster.aspx`. Přidat na stránku GridView a jeho vazbu na nového prvku ObjectDataSource. Tento prvek ObjectDataSource, používejte `SuppliersBLL` třídy `GetSuppliers()` metody, která vrátí všechny dodavatelů.


[![Vyberte třídu SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Obrázek 2**: vyberte `SuppliersBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![Konfigurace ObjectDataSource GetSuppliers() metody](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Obrázek 3**: Konfigurace ObjectDataSource k použití `GetSuppliers()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image7.png))


Potřebujeme zahrnout odkaz s názvem zobrazit produkty v jednotlivých řádcích ovládacího prvku GridView, po kliknutí na kterým uživatel přejde na `ProductsForSupplierDetails.aspx` předávání vybraného řádku `SupplierID` hodnotné řetězec dotazu. Například, pokud uživatel klikne na odkaz zobrazit produkty pro dodavatele Traders Tokio (který má `SupplierID` hodnotu 4), by měly být odeslány na `ProductsForSupplierDetails.aspx?SupplierID=4`.

Chcete-li to provést, přidejte [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) do prvku GridView, který přidá hypertextový odkaz na každý řádek prvku GridView. Začněte kliknutím na odkaz Upravit sloupce v prvku GridView inteligentních značek. V dalším kroku vyberte HyperLinkField ze seznamu v levém horním rohu a klikněte na Přidat a zahrnout HyperLinkField v seznamu polí v prvku GridView.


[![Přidat HyperLinkField do prvku GridView.](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Obrázek 4**: Přidání HyperLinkField do prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image10.png))


HyperLinkField dá používat stejný text nebo adresa URL hodnot odkaz na každém řádku prvku GridView, nebo můžete založit tyto hodnoty hodnot dat, které je vázána na každý řádek. Pokud chcete nastavit statickou hodnotu napříč všemi řádky použijte HyperLinkField `Text` nebo `NavigateUrl` vlastnosti. Protože chceme, aby propojení text, který má být stejný pro všechny řádky, nastavte HyperLinkField `Text` vlastnost zobrazit produkty.


[![Nastavte vlastnost Text HyperLinkField na Zobrazit produkty](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Obrázek 5**: nastavte HyperLinkField `Text` vlastnost zobrazit produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image13.png))


Pokud chcete nastavit text nebo hodnoty adresy URL byl založený na podkladová data vázaná na řádku prvku GridView, zadejte data pole, text nebo hodnoty adresy URL by měla být získaných z v `DataTextField` nebo `DataNavigateUrlFields` vlastnosti. `DataTextField` lze nastavit pouze do jednoho datového pole; `DataNavigateUrlFields`, ale může být nastavená na čárkami oddělený seznam datových polí. Často je potřeba základní text nebo adresy URL na hodnotu pole aktuální řádek dat i některé statické značek. V tomto kurzu jsme třeba odkazy HyperLinkField adresa URL se `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, kde *`supplierID`* je každý prvek GridView řádek `SupplierID` hodnotu. Všimněte si, že potřebujeme statické a datové hodnoty tady: `ProductsForSupplierDetails.aspx?SupplierID=` část adresy URL, na odkaz je statická, že *`supplierID`* část je s daty jako jeho hodnotu každého řádku vlastní `SupplierID` hodnotu.

K označení kombinaci statických a datové hodnoty, použijte `DataTextFormatString` a `DataNavigateUrlFormatString` vlastnosti. Na tyto vlastnosti zadat statický kód podle potřeby a potom použít značky `{0}` požadované hodnoty zadané do pole `DataTextField` nebo `DataNavigateUrlFields` vlastnosti, které chcete zobrazit. Pokud `DataNavigateUrlFields` vlastnost má více polí zadané použití `{0}` tam, kde chcete, aby první hodnota pole vložili, `{1}` pro druhý hodnotu pole a tak dále.

Použití to pro náš kurz, musíme nastavit `DataNavigateUrlFields` vlastnost `SupplierID`, protože to je datové pole, jehož hodnota musíme přizpůsobit na základě na řádek a `DataNavigateUrlFormatString` vlastnost `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Konfigurace HyperLinkField zahrnout správnou adresu URL odkazu na základě pole SupplierID](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Obrázek 6**: Konfigurace HyperLinkField zahrnout správné odkaz URL na základě při `SupplierID` ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image16.png))


Po přidání HyperLinkField, můžete přizpůsobit a změna pořadí polí v prvku GridView. Následující kód ukazuje prvku GridView, po provedli některé dílčí přizpůsobení na úrovni pole.


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Za chvíli zobrazíte `SupplierListMaster.aspx` stránky prostřednictvím prohlížeče. Jak je vidět na obrázku 7, na stránce aktuálně obsahuje seznam všech dodavatelů včetně odkazu zobrazit produkty. Kliknutím na Zobrazit produkty odkaz přejdete na `ProductsForSupplierDetails.aspx`, předejte podél dodavatele `SupplierID` v řetězec dotazu.


[![Každý řádek dodavatele obsahuje odkaz produkty zobrazení](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Obrázek 7**: každý řádek dodavatele obsahuje odkaz zobrazit produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Krok 3: Výpis dodavatele produkty`ProductsForSupplierDetails.aspx`

V tomto okamžiku `SupplierListMaster.aspx` stránky odesílá uživatelům `ProductsForSupplierDetails.aspx`, předávání vybrané dodavatele `SupplierID` v řetězec dotazu. Tento kurz posledním krokem je zobrazit produkty v prvku GridView v `ProductsForSupplierDetails.aspx` jehož `SupplierID` rovná `SupplierID` předává řetězec dotazu. K provedení této úvodní přidáním GridView k `ProductsForSupplierDetails.aspx` stránky, můžete použít nový ovládací prvek ObjectDataSource s názvem `ProductsBySupplierDataSource` , která vyvolává `GetProductsBySupplierID(supplierID)` metodu z `ProductsBLL` třídy.


[![Přidat nový prvek ObjectDataSource s názvem ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Obrázek 8**: přidejte nový prvek ObjectDataSource s názvem `ProductsBySupplierDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![Vyberte třídu ProductsBLL](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Obrázek 9**: vyberte `ProductsBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![Mít ObjectDataSource Invoke GetProductsBySupplierID(supplierID) – metoda](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Obrázek 10**: mají ObjectDataSource vyvolat `GetProductsBySupplierID(supplierID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image28.png))


Posledním krokem v průvodci Konfigurace zdroje dat dotazem, abychom mohli poskytovat zdroj `GetProductsBySupplierID(supplierID)` metody *`supplierID`* parametru. Pokud chcete použít hodnotu querystring, nastavit zdroj parametru řetězce dotazu a zadejte název hodnoty řetězce dotazu, který má použít v textovém poli vlastnost QueryStringField (`SupplierID`).


[![Naplnění KódDodavatele hodnota parametru z hodnoty Querystring KódDodavatele](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Obrázek 11**: naplnění *`supplierID`* hodnota parametru `SupplierID` hodnotu řetězce dotazu ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image31.png))


To je všechno je to! Obrázek 12 se zobrazí `ProductsForSupplierDetails.aspx` stránce, když uživatel klepnutím na odkaz Tokio Traders z `SupplierListMaster.aspx`.


[![Jsou uvedeny produkty poskytnutých obchodníci Tokio](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Obrázek 12**: jsou uvedeny produkty poskytnutých obchodníci Tokio ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Zobrazení informací o dodavatele v`ProductsForSupplierDetails.aspx`

Jak ukazuje obrázek 12 `ProductsForSupplierDetails.aspx` stránky jednoduše seznam produktů, které jsou poskytovány společností `SupplierID` podle řetězec dotazu. Někdo poslal přímo na tuto stránku však nebude vědět, že obrázek 12 se zobrazuje Tokio Traders products. Chcete-li to že můžeme zobrazit informace o dodavateli na této stránce a napravit.

Začněte přidáním nad produkty GridView FormView. Vytvoření nového ovládacího prvku ObjectDataSource s názvem `SuppliersDataSource` , která vyvolává `SuppliersBLL` třídy `GetSupplierBySupplierID(supplierID)` metody.


[![Vyberte třídu SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Obrázek 13**: vyberte `SuppliersBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![Mít ObjectDataSource Invoke GetSupplierBySupplierID(supplierID) – metoda](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Obrázek 14**: mají ObjectDataSource vyvolat `GetSupplierBySupplierID(supplierID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image40.png))


Stejně jako u `ProductsBySupplierDataSource`, jste *`supplierID`* parametru přiřazena hodnota `SupplierID` hodnotu řetězce dotazu.


[![Naplnění KódDodavatele hodnota parametru z hodnoty Querystring KódDodavatele](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Obrázek 15**: naplnění *`supplierID`* hodnota parametru `SupplierID` hodnotu řetězce dotazu ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image43.png))


Při vytváření vazby FormView k prvku ObjectDataSource v okně návrhu, Visual Studio automaticky vytvoří ovládacího prvku FormView `ItemTemplate`, `InsertItemTemplate`, a `EditItemTemplate` s popisku a textového pole webové ovládací prvky pro každé pole dat vrácených Prvek ObjectDataSource. Protože chceme jenom zobrazit dodavatele informace teď můžete odebrat `InsertItemTemplate` a `EditItemTemplate`. Potom upravte vlastnosti ItemTemplate, které tak, aby zobrazil název společnosti dodavatele `<h3>` elementu a adresu, Město, země a telefonní číslo pod název společnosti. Alternativně můžete ručně nastavit ovládacího prvku FormView `DataSourceID` a vytvořit `ItemTemplate` značek, jako jsme to udělali v "[zobrazení dat se prvku ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" kurzu.

Za tyto úpravy ovládacího prvku FormView deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

Snímek obrazovky znázorňuje obrázek 16 `ProductsForSupplierDetails.aspx` stránce výše uvedené informace o dodavateli byla zahrnuta.


[![Seznamu produktů, které obsahuje souhrnné informace o dodavateli](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Obrázek 16**: seznamu produktů, které obsahuje souhrnné informace o dodavatele ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Použití finální dotýká pro`ProductsForSupplierDetails.aspx`uživatelského rozhraní

K vylepšení uživatelského prostředí pro tuto sestavu existuje několik dodatky jsme by mělo být tak, aby `ProductsForSupplierDetails.aspx` stránky. Aktuálně jediným způsobem, uživatel může přecházet od `ProductsForSupplierDetails.aspx` stránka zpět do seznamu poskytovatelů je na jejich prohlížeči tlačítko Zpět. Přidejte ovládací prvek hypertextového odkazu do `ProductsForSupplierDetails.aspx` stránka, která odkazuje zpět na `SupplierListMaster.aspx`, poskytuje další způsob pro uživatele se vraťte do seznamu hlavní.


[![Přidejte ovládací prvek hypertextového odkazu, který má uživatel přejít zpět k SupplierListMaster.aspx](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Obrázek 17**: Přidání ovládacího prvku hypertextový odkaz uživatele zpět na přijmout `SupplierListMaster.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image49.png))


Pokud uživatel klikne na odkaz zobrazit produkty pro dodavatele, který nemá žádné produkty `ProductsBySupplierDataSource` ObjectDataSource v `ProductsForSupplierDetails.aspx` nebudou nalezeny žádné výsledky. GridView vázán na ObjectDataSource nebudou vykreslovat žádné značky, což vede k prázdné oblasti na stránce v prohlížeči uživatele. Pro větší přehlednost komunikaci uživateli, že neexistují žádné produkty související s vybranou dodavatele nastavíme prvku GridView `EmptyDataText` vlastnost zprávy chceme zobrazené, pokud taková situace nastane. Nastavím tuto vlastnost na "Nejsou žádné produkty poskytnuté tohoto dodavatele"

Všichni dodavatelé databáze Lhota ve výchozím nastavení, zadejte aspoň jeden produkt. Ale pro účely tohoto kurzu můžu ručně upravili `Products` tabulku tak, aby od dodavatele Escargots Nouveaux už nejsou přidružené žádné produkty. Obrázek 18 zobrazuje na stránce podrobností pro Escargots Nouveaux po této změně.


[![Uživatelé budou informováni, že dodavatel neposkytuje žádné produkty](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Obrázek 18**: uživatelé budou informováni, že dodavatel neposkytuje žádné produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>Souhrn

Při záznamů master/detail sestavy můžete zobrazit hlavního uzlu i podrobné záznamy na jednu stránku, v mnoha webů, rozdělené dvě webové stránky. V tomto kurzu jsme se podívali na tom, jak implementovat záznamů master/detail sestavy tak, že dodavatelé uvedené v prvku GridView v "hlavní" webové stránky a související produkty uvedené na stránce "details". Každý řádek dodavatele na webové stránce předlohy obsahovala odkaz na stránku podrobností, která se předají na řádku `SupplierID` hodnotu. Tyto odkazy specifické pro řádek můžete snadno přidat pomocí HyperLinkField prvku GridView.

Na stránce podrobností načítání těchto produktů pro zadaný dodavatele bylo dosaženo vyvoláním `ProductsBLL` třídy `GetProductsBySupplierID(supplierID)` metody. *`supplierID`* Byla zadána hodnota parametru deklarativně pomocí řetězce dotazu jako zdroj parametru. Také jsme se podívali na tom, jak zobrazit podrobnosti o dodavatele na stránce podrobností FormView pomocí.

Naše [další kurz](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) je finálního na master/detail v sestavách. Podíváme se na tom, jak zobrazit seznam produktů v ovládacího prvku GridView, kde každý řádek obsahuje tlačítko pro výběr. Kliknutím na tlačítko pro výběr zobrazíte podrobnosti o konkrétního produktu v ovládacím prvku DetailsView na stejné stránce.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Hilton Giesenow. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](master-detail-filtering-with-two-dropdownlists-cs.md)
> [další](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
