---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: Dávkové vkládání (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Zjistěte, jak vložit více záznamů databáze v rámci jedné operace. Ve vrstvě uživatelského rozhraní rozšíříme prvku GridView, aby uživatel mohl zadat více n...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ec5c35417f4f986c662201da58ca3441e8944ca
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824293"
---
<a name="batch-inserting-vb"></a>Dávkové vkládání (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip) nebo [stahovat PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> Zjistěte, jak vložit více záznamů databáze v rámci jedné operace. Ve vrstvě uživatelského rozhraní rozšíříme prvku GridView, aby uživatel mohl zadat několik nových záznamů. V datové vrstvě přístupu jsme zabalit několik operací vložení v rámci transakce zajistit, že všechny vložení úspěch, nebo všechny vložené položky jsou vráceny zpět.


## <a name="introduction"></a>Úvod

V [dávkové aktualizace](batch-updating-vb.md) kurzu jsme se podívali na přizpůsobení ovládacího prvku GridView. Chcete-li k dispozici rozhraní, ve kterém byly upravitelné více záznamů. Uživatel na stránce může provést řadu změn a potom kliknutím jediné tlačítko proveďte aktualizace služby batch. Pro situace, kdy uživatelé běžně aktualizovat mnoha záznamů najednou, můžete uložit toto rozhraní aplikací kliknutí a přepnutí kontextu myš a klávesnici ve srovnání s výchozí za řádek editačních funkcí, které byly nejprve prozkoumali zpět v [ Přehled vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) kurzu.

Tento koncept je použít také při přidání záznamů. Představte si, že tady společnosti Northwind Traders běžně státem dodávek od dodavatelů, které obsahují několik produktů pro určitou kategorii. Jako příklad může státem dodávky šesti různých čaj a kávy produktů z Tokio Traders. Pokud uživatel zadá šest produkty jedním současně prostřednictvím ovládacího prvku DetailsView, budou muset řadu stejné hodnoty znovu a znovu zvolte: bude nutné vybrat stejné kategorie (nápoje) stejného dodavatele (Tokio Traders), stejná hodnota (ukončena False) a stejné jednotky na hodnotu pořadí (0). Tato položka opakující se data pouze není časově náročné, ale je náchylná k chybám.

S trochou pracovní vytvoříme dávkové vložení rozhraní, které umožňuje uživateli zvolit dodavatele a kategorie jednou, zadejte řadu názvů produktů a jednotkové ceny a potom klikněte na tlačítko pro přidání nové produkty k databázi (viz obrázek 1). Po přidání každého produktu, jeho `ProductName` a `UnitPrice` datová pole jsou přiřazeny hodnoty zadané v textových polí, zatímco jeho `CategoryID` a `SupplierID` hodnoty jsou přiřazeny hodnoty z DropDownLists na začátek fo formuláře. `Discontinued` a `UnitsOnOrder` hodnoty jsou nastaveny na pevně definovaných hodnot z `False` a 0, v uvedeném pořadí.


[![Vložení rozhraní služby Batch](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**Obrázek 1**: dávkové vložení rozhraní ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image3.png))


V tomto kurzu vytvoříme stránku, která implementuje dávkové vložení rozhraní zobrazené na obrázku 1. Jako předchozí dva kurzy, jsme se zalomí vložení v rámci oboru transakce zajistit atomicitu. Začínáme s let!

## <a name="step-1-creating-the-display-interface"></a>Krok 1: Vytvoření rozhraní pro zobrazení

V tomto kurzu budou obsahovat jednu stránku, která je rozdělena do dvou oblastí: zobrazení oblasti a vkládání oblasti. Rozhraní zobrazení, které vytvoříme v tomto kroku, zobrazí produkty v GridView a obsahuje tlačítko s názvem procesu dodávky produktu. Když dojde ke kliknutí na toto tlačítko, rozhraní zobrazení se nahradí vkládání rozhraní, která je znázorněna na obrázku 1. Vrátí zobrazení rozhraní po přidání produktů z dodávky nebo kliknutím na tlačítka Storno. Vložení rozhraní vytvoříme v kroku 2.

Při vytvoření stránky, která má dvě rozhraní, najednou je viditelná pouze jeden z nich, každé rozhraní obvykle nachází v rámci [ovládací prvek panelu webu](http://www.w3schools.com/aspnet/control_panel.asp), který slouží jako kontejner pro ostatní ovládací prvky. Naši stránku proto bude mít dva ovládací prvky panelu jeden pro každé rozhraní.

Začněte otevřením `BatchInsert.aspx` stránku `BatchData` složky a Panel přetáhněte z panelu nástrojů do návrháře (viz obrázek 2). Nastavení panelu s `ID` vlastnost `DisplayInterface`. Při přidání panelu do návrháře jeho `Height` a `Width` vlastnosti nastavené na 50px a 125px, v uvedeném pořadí. Vymazání hodnoty těchto vlastností v okně Vlastnosti.


[![Přetáhněte z panelu nástrojů na Návrhář panelu](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**Obrázek 2**: přetáhněte z panelu nástrojů na Návrhář panelu ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image6.png))


V dalším kroku přetáhněte ovládací prvek tlačítko a GridView do panelu. Tlačítka s nastavte `ID` vlastnost `ProcessShipment` a jeho `Text` vlastnost procesu dodávky produktu. Nastavit prvek GridView s `ID` vlastnost `ProductsGrid` a z inteligentních značek, jeho vazbu na nového prvku ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource přebírat jeho data ze `ProductsBLL` třída s `GetProducts` metody. Od tohoto ovládacího prvku GridView slouží pouze k zobrazení dat, nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný). Klikněte na tlačítko Dokončit dokončete průvodce pro zdroj dat nakonfigurovat.


[![Zobrazení dat vrácených z metody GetProducts ProductsBLL třída s](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**Obrázek 3**: zobrazení Data vrácená z `ProductsBLL` třída s `GetProducts` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image9.png))


[![Nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný)](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**Obrázek 4**: Nastavte rozevírací seznam obsahuje v UPDATE, INSERT a odstranit záložky (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image12.png))


Po dokončení Průvodce prvek ObjectDataSource, Visual Studio přidá BoundFields a třídě CheckBoxField pro datová pole produktu. Odeberte všechny kromě na `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, a `Discontinued` pole. Nebojte se provádět jakákoli aesthetic vlastní nastavení. Jsem se rozhodla pro formátování `UnitPrice` pole jako hodnotu měny a pořadí změníte pole a přejmenovat několik polí `HeaderText` hodnoty. Také nakonfigurujte stránkování a řazení podporu zaškrtnutím políčka Povolit stránkování a Povolit řazení v prvku GridView s inteligentním prvku GridView.

Po přidání ovládacích prvků panelu, tlačítko, ovládacího prvku GridView a ObjectDataSource a přizpůsobení polí s ovládacího prvku GridView, stránka s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

Všimněte si, že se zobrazí v rámci otevírací a zavírací značky pro tlačítka a GridView `<asp:Panel>` značky. Protože jsou tyto ovládací prvky v rámci `DisplayInterface` panelu jsme je můžete skrýt Panel s nastavením jednoduše `Visible` vlastnost `False`. Krok 3 zkoumá programově změnit panelu s `Visible` v odezvě na tlačítku kliknutím zobrazíte jedno rozhraní při skrytí druhé.

Chcete-li zobrazit náš postup prostřednictvím prohlížeče chvíli trvat. Jak je vidět na obrázku 5, měli byste vidět tlačítko procesu dodávky produktu nad prvku GridView, která zobrazuje seznam produktů deset najednou.


[![Seznamy produktů, na prvku GridView a nabízí řazení a stránkování](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**Obrázek 5**: produkty a nabízí řazení a stránkování možnosti jsou uvedeny prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Krok 2: Vytvoření rozhraní pro vložení

Zobrazení v rozhraní dokončení, můžeme znovu připravený k vytvoření vkládání rozhraní. Pro účely tohoto kurzu nechte s vytvořit vkládání rozhraní, které zobrazí výzvu pro hodnotu jednoho dodavatele a kategorie a pak umožní uživateli zadat až pět názvy produktů a hodnoty cena jednotky. S tímto rozhraním může uživatel přidat jedné do pěti nových produktů, které sdílejí stejné kategorie a dodavatele, ale mají jedinečné produkty, názvy a ceny.

Začněte tím, že Panel přetažením z panelu nástrojů na Návrhář, že ho umístíte pod existující `DisplayInterface` panelu. Nastavte `ID` nově přidána vlastnost tohoto panelu `InsertingInterface` a nastavte jeho `Visible` vlastnost `False`. Přidáme kód, který nastaví `InsertingInterface` Panel s `Visible` vlastnost `True` v kroku 3. Také smažte na panelu s `Height` a `Width` hodnot vlastností.

V dalším kroku se musíme vytvořit vkládání rozhraní, které se zobrazilo zpět na obrázku 1. Toto rozhraní je možné vytvářet přes různé techniky HTML, ale budeme používat poměrně jednoduché jeden: čtyři sloupce, řádku sedm tabulek.

> [!NOTE]
> Při zadávání kódu pro kód HTML `<table>` prvky, chci raději použít zobrazení zdroje. Visual Studio mají nástroje pro přidání `<table>` prvky pomocí návrháře návrháře zdá se, že všechny příliš chce vložit nevyžádaný pro `style` nastavení do kódu. Jakmile vytvořil(a) jsem `<table>` kód, lze obvykle vrátit do návrháře přidat ovládací prvky, Web a nastavte jejich vlastnosti. Při vytváření tabulky s předem určené sloupce a řádky dávám přednost používání statických HTML místo [tabulky webový ovládací prvek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) vzhledem k tomu, že všechny webové ovládací prvky umístěné v rámci ovládacího prvku Web tabulky lze přistupovat pouze pomocí `FindControl("controlID")` vzor. , Ale používám ovládacích prvků tabulka pro dynamicky velikost tabulky (balíčky, jejichž řádky nebo sloupce jsou založené na některé databáze nebo uživatelem zadaných kritérií), od Webová tabulka ovládací prvek lze zkonstruovat prostřednictvím kódu programu.


Zadejte následující kód v rámci `<asp:Panel>` značky `InsertingInterface` panelu:


[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

To `<table>` značek nezahrnuje žádné webové ovládací prvky, ale ty přidáme okamžik. Všimněte si, že každá `<tr>` prvek obsahuje konkrétní nastavení třídy šablony stylů CSS: `BatchInsertHeaderRow` pro řádek záhlaví, kam se obrátit dodavatele a kategorie DropDownLists; `BatchInsertFooterRow` pro zápatí řádku, kde přidat produkty z tlačítka zrušit a dodávky půjdou; a každou druhou `BatchInsertRow` a `BatchInsertAlternatingRow` hodnoty řádků, které budou obsahovat produktu a jednotky cena TextBox – ovládací prvky. Můžu ve vytvořené odpovídající tříd CSS ve `Styles.css` soubor poskytnout vkládání rozhraní podobné GridView a DetailsView vzhled ovládací prvky, jsme ve používají v těchto kurzech. Níže se zobrazují tyto třídy šablony stylů CSS.


[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

Tento kód zadali vrátí do zobrazení návrhu. To `<table>` by se zobrazit jako čtyři sloupce, řádku sedm tabulky v návrháři, jak znázorňuje obrázek 6.


[![Vložení rozhraní se skládá čtyři sloupce, sedm řádek tabulky](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**Obrázek 6**: tvoří rozhraní vložení čtyři sloupce, sedm řádek tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image18.png))


Můžeme znovu teď jste připravení přidat ovládací prvky webového rozhraní vkládání. Dvě DropDownLists přetáhněte z panelu nástrojů do příslušné buněk v tabulce, jeden pro dodavatele a jeden pro kategorii.

Nastavte DropDownList s od dodavatele `ID` vlastnost `Suppliers` a jeho vazbu na nového prvku ObjectDataSource s názvem `SuppliersDataSource`. Konfigurace nového prvku ObjectDataSource načíst data z `SuppliersBLL` třída s `GetSuppliers` metoda a nastavte aktualizace kartu s rozevíracím seznamu na (žádný). Kliknutím na Dokončit dokončíte průvodce.


[![Konfigurace ObjectDataSource metody GetSuppliers SuppliersBLL třída s](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**Obrázek 7**: Konfigurace ObjectDataSource k použití `SuppliersBLL` třída s `GetSuppliers` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image21.png))


Máte `Suppliers` DropDownList zobrazení `CompanyName` pole data a použít `SupplierID` datové pole jako jeho `ListItem` s hodnotami.


[![Zobrazení pole CompanyName Data a použít KódDodavatele jako hodnotu](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**Obrázek 8**: zobrazení `CompanyName` pole dat a použití `SupplierID` jako hodnotu ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image24.png))


Název druhé DropDownList `Categories` a jeho vazbu na nového prvku ObjectDataSource s názvem `CategoriesDataSource`. Konfigurace `CategoriesDataSource` ObjectDataSource používat `CategoriesBLL` třída s `GetCategories` metoda; nastavte rozevírací seznam uvádí UPDATE a DELETE karty na (žádný) a klikněte na Dokončit kroky průvodce dokončete. Nakonec jste DropDownList zobrazení `CategoryName` pole data a použít `CategoryID` jako hodnotu.

Po přidání těchto dvou DropDownLists a vázán na správně nakonfigurovaných ObjectDataSources, vaše obrazovka by měla vypadat podobně jako na obrázku 9.


[![Nyní obsahuje řádek záhlaví dodavatelů a DropDownLists kategorie](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**Obrázek 9**: záhlaví řádku nyní obsahuje `Suppliers` a `Categories` DropDownLists ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image27.png))


Nyní potřebujeme vytvořit textová pole pro sběr název a ceny pro každý nový produkt. Přetáhněte ovládací prvek textového pole z panelu nástrojů na Návrhář pro každý z pěti produktu název a cena řádky. Nastavte `ID` vlastnosti textových polí do `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`, a tak dále.

Přidat CompareValidator po každém z nich jednotkové ceny textová pole, nastavení `ControlToValidate` vlastnosti na příslušné `ID`. Také nastavit `Operator` vlastnost `GreaterThanEqual`, `ValueToCompare` na hodnotu 0, a `Type` k `Currency`. Tato nastavení dáte pokyn, aby CompareValidator tak, aby byl ceny, je-li zadán, platný měny hodnotu, která je větší než nebo rovna hodnotě nula. Nastavte `Text` vlastnost \*, a `ErrorMessage` ceny musí být větší než nebo rovna hodnotě nula. Kromě toho prosím vynechte jakýchkoli symbolů měny.

> [!NOTE]
> Vložení rozhraní neobsahuje žádné ovládací prvky RequiredFieldValidator, i když `ProductName` pole `Products` databázové tabulky neumožňuje `NULL` hodnoty. Je to proto, že chcete uživatelům povolit, zadejte až o pěti produktech. Například pokud uživatel poskytnout product name a Jednotková cena pro první tři řádky, prázdné poslední dva řádky d právě přidáme tři nové produkty do systému. Protože `ProductName` je vyžadováno, ale budeme muset kontrola prostřednictvím kódu programu, ujistěte se, že pokud jednotková cena je zadán, je k dispozici odpovídající hodnota názvu produktu. Tato kontrola v kroku 4 jsme budete řešit.


Při ověřování vstupu uživatele s CompareValidator sestavy neplatná data, pokud hodnota obsahuje symbol měny. Přidáte $ před každou je cena ze jednotku textových polí, která bude sloužit jako vizuální upozornění, která informuje uživatele, aby vynechat symbol měny, při zadávání cena.

A konečně, přidejte ovládací prvek souhrnu ověření v rámci `InsertingInterface` panelu nastavení jeho `ShowMessageBox` vlastnost `True` a jeho `ShowSummary` vlastnost `False`. S tímto nastavením Pokud uživatel zadá hodnotu cena neplatná jednotka, hvězdička se zobrazí vedle problematický ovládací prvky textového pole a souhrnu ověření, zobrazí se pole messagebox na straně klienta, který zobrazuje chybovou zprávu, kterou jsme dříve zadaný.

V tomto okamžiku vaše obrazovka by měla vypadat podobně jako na obrázku 10.


[![Vložení rozhraní nyní zahrnuje textová pole pro produkty názvy a ceny](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**Obrázek 10**: The vkládání rozhraní nyní zahrnuje textová pole pro názvy produktů a ceny ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image30.png))


Dále musíme přidání produktů přidat z tlačítka dodávky a Storno do řádku zápatí. Přetáhněte dva ovládací prvky tlačítek z panelu nástrojů do v zápatí je uvedené rozhraní vkládání nastavení tlačítek `ID` vlastností `AddProducts` a `CancelButton` a `Text` vlastnosti, které chcete přidat produkty z dodávky a zrušit, v uvedeném pořadí. Navíc nastavte `CancelButton` ovládacího prvku s `CausesValidation` vlastnost `false`.

Nakonec musíme přidat ovládací prvek popisek Web, který se zobrazí stavové zprávy pro tato dvě rozhraní. Například když uživatel úspěšně přidá nové dodávky produktů, chceme pro návrat do zobrazení rozhraní a zobrazí se potvrzovací zpráva. Pokud však uživatel zadá cena nového produktu, ale ponechá vypnout název produktu, potřebujeme pro zobrazení zprávy upozornění od `ProductName` pole je povinné. Protože potřebujeme tato zpráva má být zobrazen pro obě rozhraní, umístěte ho v horní části stránky mimo panelů.

Přetáhněte popisek webový ovládací prvek z panelu nástrojů do horní části stránky v návrháři. Nastavte `ID` vlastnost `StatusLabel`, zrušte zaškrtnutí políčka navýšení kapacity `Text` vlastnost a nastavte `Visible` a `EnableViewState` vlastností `False`. Jak jsme viděli v předchozích kurzech, nastavení `EnableViewState` vlastnost `False` umožňuje programově změnit hodnoty vlastností Popisek s a potom kliknul automaticky vrátit se k jejich výchozí hodnoty na následné zpětného odeslání. To zjednodušuje kód k zobrazení stavové zprávy v reakci na některé akce uživatele, která zmizí při následné postbacku. Nastavte `StatusLabel` ovládacího prvku s `CssClass` definována vlastnost na upozornění, což je název třídy šablony stylů CSS v `Styles.css` , který zobrazí text velké, kurzíva, tučné písmo, červenou písmem.

Obrázku 11 můžete vidět Návrhář Visual Studio po popisek byl přidán a nakonfigurován.


[![Umístit ovládací prvek StatusLabel nad dva ovládací prvky Panel](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**Obrázek 11**: místo `StatusLabel` ovládací prvek výše dva ovládací prvky panelu ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Krok 3: Přepínání mezi režimy zobrazení a vložení rozhraní

V tuto chvíli jsme pro náš zobrazení a vložení rozhraní, ale můžeme znovu ještě zbývá dvě úlohy dokončení značky:

- Přepínání mezi režimy zobrazení a vložení rozhraní
- Přidání produktů v dodávce do databáze

V současné době je viditelné rozhraní zobrazení ale vkládání rozhraní. Důvodem je, že `DisplayInterface` Panel s `Visible` je nastavena na `True` (výchozí hodnota), zatímco `InsertingInterface` Panel s `Visible` je nastavena na `False`. Chcete-li přepnout mezi dvě rozhraní, potřebujeme jen přepnout každý ovládací prvek s `Visible` hodnotu vlastnosti.

Chcete přesunout z rozhraní zobrazení pro vkládání rozhraní při kliknutí na tlačítko procesu dodávky produktu. Proto vytvořit obslužnou rutinu události pro toto tlačítko s `Click` událost, která obsahuje následující kód:


[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

Tento kód jednoduše skryje `DisplayInterface` panelu a zobrazí `InsertingInterface` panelu.

Dále vytvořte obslužné rutiny událostí pro přidání produkty z ovládacích prvků v rozhraní vkládání dodávky a tlačítko Storno. Některé z těchto tlačítek se při kliknutí na, je potřeba vrátit se k zobrazení rozhraní. Vytvoření `Click` obslužné rutiny událostí pro obě tlačítko – ovládací prvky tak, aby volání `ReturnToDisplayInterface`, metoda přidáme okamžik. Kromě skrytí `InsertingInterface` panely a zobrazení `DisplayInterface` panelu `ReturnToDisplayInterface` metoda musí vracet webové ovládací prvky do předem úprav stavu. To zahrnuje nastavení DropDownLists `SelectedIndex` vlastnosti a 0 zrušením navýšení kapacity `Text` vlastností ovládacích prvků textového pole.

> [!NOTE]
> Zvažte, co může dojít, pokud jsme nefungoval t vrátí ovládacích prvků do předem úprav stavu před vrácením rozhraní zobrazení. Uživatel může klikněte na tlačítko procesu dodávky produktu, zadejte tyto produkty z dodávky a pak klikněte na tlačítko Přidat produkty z dodávky. To by přidejte produkty a vrátí uživatele k zobrazení rozhraní. V tomto okamžiku uživatel může chtít přidat jiné dodávky. Po kliknutí na tlačítko procesu dodávky produktu, které budou vráceny vkládání rozhraní, ale DropDownList výběry a textové pole hodnot by stále se vyplní jejich předchozí hodnoty.


[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

Obě `Click` obslužné rutiny událostí jednoduše zavoláte `ReturnToDisplayInterface` metoda, i když se vrátíme k produktům přidat z dodávky `Click` obslužné rutiny události v kroku 4 a přidat kód pro uložení produktů. `ReturnToDisplayInterface` Spustí vrácením `Suppliers` a `Categories` DropDownLists jejich první možnosti. Dvě konstanty `firstControlID` a `lastControlID` označit počáteční a koncové hodnoty ovládacího prvku indexu v názvu produktu název a jednotku cena textová pole v vkládání rozhraní a používají se v hranice `For` smyčku, která nastaví `Text`vlastností ovládacích prvků TextBox zpět na prázdný řetězec. Nakonec panelů `Visible` vlastnosti resetují tak, aby vkládání rozhraní a zobrazení rozhraní zobrazeny.

Využijte k otestování této stránky v prohlížeči. Při první návštěvě stránky byste měli vidět zobrazení rozhraní, jak je znázorněno na obrázku 5. Klikněte na tlačítko procesu dodávky produktu. Na stránce se odeslat zpět a měli byste vidět vkládání rozhraní jak ukazuje obrázek 12. Klepnutím na tlačítko buď přidejte produkty z dodávky, nebo zrušte tlačítka, vrátíte se na rozhraní zobrazení.

> [!NOTE]
> Při zobrazování vkládání rozhraní, využijte k otestování CompareValidators na je cena ze jednotku textových polí. Zobrazí se pole messagebox na straně klienta upozornění, když kliknete na Přidat produkty z dodávky tlačítko s neplatnou měny nebo ceny s hodnotu menší než nula.


[![Vložení rozhraní se zobrazí po kliknutí na tlačítko dodávky produktu procesu](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**Obrázek 12**: rozhraní vložení se zobrazí po kliknutí na tlačítko dodávky produktu procesu ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Krok 4: Přidání produktů

Vše, který zůstane pro tento kurz, je uložit do databáze v produktech přidat produkty z dodávky tlačítko s `Click` obslužné rutiny události. Toho můžete docílit tak, že vytvoříte `ProductsDataTable` a přidání `ProductsRow` instance pro každý ze zadaných názvů produktů. Jednou tyto `ProductsRow` s byly přidány, budeme volání `ProductsBLL` třída s `UpdateWithTransaction` metoda předává `ProductsDataTable`. Vzpomeňte si, že `UpdateWithTransaction` metodu, která byla vytvořena v [zabalení úprav databáze do transakce](wrapping-database-modifications-within-a-transaction-vb.md) výukový program, předá `ProductsDataTable` k `ProductsTableAdapter` s `UpdateWithTransaction` metoda. Odtud, je spustit transakci ADO.NET a TableAdatper problémy `INSERT` příkaz k databázi pro každou přidali `ProductsRow` v objektu DataTable. Za předpokladu, že všechny produkty jsou přidány bez chyb, že transakce se potvrzeny, jinak je vrácena zpět.

Kód pro přidání produkty z dodávky tlačítko s `Click` obslužné rutiny události musí také provést trochu kontroly chyb. Vzhledem k tomu, že neexistují žádné RequiredFieldValidators používaných pro vkládání rozhraní, může uživatel zadat při vynechání názvu cenu pro produkt. Protože název produktu s je povinný, pokud takovou podmínku, kterou se musíme upozornit uživatele a ne pokračujte vložení informací. Kompletní `Click` následuje kód obslužné rutiny události:


[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

Obslužná rutina události začíná tím, že zajišťuje, `Page.IsValid` vlastnost vrací hodnotu `True`. Vrátí-li `False`a pak to znamená, že jeden nebo více CompareValidators nahlašujete neplatná data; v takovém případě tudy pokusit vložit zadané produkty nebo jsme nakonec se dostanete k výjimce při pokusu o přiřazení je cena ze jednotku zadanou uživatelem hodnota, která se `ProductsRow` s `UnitPrice` vlastnost.

Další, nové `ProductsDataTable` je vytvořena instance (`products`). A `For` smyčce se používá k iteraci v rámci produktu název a Jednotková cena textových polí a `Text` do místní proměnné jsou načteny vlastnosti `productName` a `unitPrice`. Pokud uživatel zadal hodnotu pro je cena ze jednotku, ale nikoli pro odpovídající název produktu, `StatusLabel` zobrazí zprávu, pokud zadáte ceny jednotka, kterou jste také musí obsahovat název produktu a obslužná rutina události je byl ukončen.

Pokud byl poskytnut název produktu, nový `ProductsRow` instance je vytvořena pomocí `ProductsDataTable` s `NewProductsRow` metoda. Tato nová `ProductsRow` instance s `ProductName` je nastavena na aktuální produkt textového pole při s názvem `SupplierID` a `CategoryID` vlastností jsou přiřazeny k `SelectedValue` vlastnosti DropDownLists v záhlaví vložení rozhraní s. Pokud uživatel zadal hodnotu pro produkt s cenu, se přiřadí `ProductsRow` instance s `UnitPrice` vlastnosti; v opačném případě je vlastnost nepřiřazené, vlevo, která bude mít za následek `NULL` hodnotu pro `UnitPrice` v databázi. Nakonec `Discontinued` a `UnitsOnOrder` vlastností jsou přiřazeny k pevně definovaných hodnot `False` a 0, v uvedeném pořadí.

Jakmile se přiřadily vlastnosti `ProductsRow` instance je přidána do `ProductsDataTable`.

Po dokončení `For` smyčky, zkontrolujeme, jestli byly přidány žádné produkty. Uživatel může jste klikli koneckonců, přidejte produkty z dodávky před zadáním cen ani názvů produktů. Pokud je aspoň jeden produkt v `ProductsDataTable`, `ProductsBLL` třída s `UpdateWithTransaction` metoda je volána. V dalším kroku znovu data na připojeno `ProductsGrid` GridView tak, aby nově přidaných produktech se zobrazí v zobrazení rozhraní. `StatusLabel` Se aktualizuje a zobrazí se potvrzovací zpráva a `ReturnToDisplayInterface` je vyvolána, skrytí vkládání rozhraní a zobrazuje rozhraní zobrazení.

Pokud byly zadány žádné produkty, vkládání rozhraní zůstane zobrazená ale zprávy, které byly přidány žádné produkty. Zadejte názvy produktů a jednotkové ceny v textových polí se zobrazí.

Obrázek s 13, 14 a 15 zobrazit vkládání a zobrazí rozhraní v akci. Obrázek 13 uživatel zadal hodnotu cena jednotky bez odpovídající název produktu. Obrázek 14 ukazuje rozhraní zobrazované za tři nové produkty byly přidány úspěšně, 15 obrázek se zobrazí dvě nově přidaných produktech v prvku GridView (třetí disk se na předchozí stránce).


[![Název produktu je nutné při zadávání cena za jednotku](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**Obrázek 13**: název produktu je nutné při zadávání cena za jednotku ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image39.png))


[![Tři nové Veggies byly přidány pro dodavatele Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**Obrázek 14**: tři nové Veggies byly přidány pro dodavatele Mayumi s ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image42.png))


[![Nové produkty nachází v poslední stránky prvku GridView.](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**Obrázek 15**: The nové produktům najdete na poslední stránce prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-inserting-vb/_static/image45.png))


> [!NOTE]
> Dávkové vkládání logikou používanou v tomto kurzu zabalí vložení informací v rámci oboru transakce. Chcete-li to ověřit, záměrně zavést chybě na úrovni databáze. Třeba místo toho novou `ProductsRow` instance s `CategoryID` k vybrané hodnotě v `Categories` DropDownList, přiřadit ho na hodnotu, jako jsou `i * 5`. Tady `i` je smyčka indexeru a má hodnoty od 1 do 5. Při přidání dvou nebo více produktů ve službě batch vložit první produkt bude mít tudíž platný `CategoryID` bude mít hodnotu (5), ale následné produkty `CategoryID` hodnoty, které se neshodují s až `CategoryID` hodnoty v `Categories` tabulky. Výsledkem je, že při první `INSERT` proběhne úspěšně, dojde k selhání dalších ty s narušení omezení pro cizí klíč. Protože dávkové vložení je Atomický, první `INSERT` bude vrácena zpět, vrácení databáze do stavu před vložením proces služby batch začalo.


## <a name="summary"></a>Souhrn

Přes to a předchozích dvou kurzů jsme vytvořili rozhraní, které umožňují aktualizaci, odstranění, a vložení dávky dat, z nichž všechny používají podpora transakcí, přidali jsme do vrstvy přístupu k datům v [zabalení úprav databáze v rámci transakce](wrapping-database-modifications-within-a-transaction-vb.md) kurzu. Pro určité scénáře, těchto uživatelských rozhraní dávkové zpracování výrazně zlepšit efektivitu koncový uživatel řezem dolů na počet kliknutí, zpětná volání a přepnutí kontextu myš a klávesnici, současně zachovává integrity podkladová data.

V tomto kurzu dokončíte naše jak se pracuje s daty uspořádanými do dávek. Další sadu kurzy vám umožní prozkoumat různé pokročilé scénáře vrstvy přístupu k datům, včetně použití uložených procedur v metodách s TableAdapter, konfigurace nastavení připojení a příkaz úrovně vrstvy DAL, šifrování připojovacích řetězců a dalších!

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Hilton Giesenow a S ren Jakub Lauritsen. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](batch-deleting-vb.md)
