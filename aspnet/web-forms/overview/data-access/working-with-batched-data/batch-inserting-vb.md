---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: Dávkové vložení (VB) | Microsoft Docs
author: rick-anderson
description: Naučte se vložit více záznamů databáze v rámci jedné operace. V uživatelské rozhraní vrstvě rozšiřujeme GridView umožňující uživateli zadat více n...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: a25c889784ccc6cee3ae01df59bd489b48114e74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="batch-inserting-vb"></a>Dávkové vložení (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip) nebo [stáhnout PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> Naučte se vložit více záznamů databáze v rámci jedné operace. V uživatelské rozhraní vrstvě rozšiřujeme GridView umožňující uživateli zadat více nové záznamy. V Data Access Layer jsme zabalit více operací vložení v rámci transakce zajistit, že všechny vložení úspěch nebo budou vráceny všechny vložení.


## <a name="introduction"></a>Úvod

V [aktualizace Batch](batch-updating-vb.md) kurzu jsme se podívali na přizpůsobení ovládacího prvku GridView k dispozici rozhraní, které byly upravovat více záznamů. Uživatel na stránce může Zkontrolujte řadu změny a pak klepnutím jednom tlačítko provádět dávková aktualizace. V situacích, kde uživatelé běžně aktualizovat mnoho záznamů najednou, můžete uložit takového rozhraní obrovském množství kliknutí a přepínače kontextu klávesnice myši ve srovnání s výchozí za řádek úpravy funkce, které byly nejprve prozkoumali zpět v [ Přehled vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) kurzu.

Tento koncept můžete také použít při přidávání záznamů. Představte si, které zde společnosti Northwind Traders běžně obdržíme dodávky od dodavatelů, které obsahují více produktů pro určité kategorie. Jako příklad jsme může přijímat dodávky šesti různých kávy a kávy produkty z Tokio Traders. Pokud uživatel zadá šesti produkty jeden v čase prostřednictvím ovládacího prvku DetailsView, budou muset zvolit řadu stejné hodnoty opakovaně: bude třeba vybrat stejné kategorii (nápoje) stejného dodavatele (Tokio Traders), stejné zastaveny (hodnota False) a stejnou jednotek na hodnota pořadí (0). Tato položka opakující se data jenom není časově náročné, ale jsou náchylné na chyby.

S malým množstvím pracovní vytvoříme dávce vkládání rozhraní, které umožňuje uživatelům zvolit dodavatele a kategorie jednou, zadejte řadu názvy produktů a jednotkové ceny a pak klikněte na tlačítko pro přidání nové produkty do databáze (viz obrázek 1). Po přidání každý produkt, jeho `ProductName` a `UnitPrice` datová pole jsou přiřazené hodnoty zadané v textových polích, při jeho `CategoryID` a `SupplierID` hodnoty přiřazené hodnoty z DropDownLists v horní fo formuláře. `Discontinued` a `UnitsOnOrder` hodnoty jsou nastaveny na hodnoty pevně `False` a 0, v uvedeném pořadí.


[![Rozhraní vložení dávky](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**Obrázek 1**: Služba Batch vkládání rozhraní ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image3.png))


V tomto kurzu vytvoříme stránky, který implementuje dávkové vložení rozhraní zobrazené na obrázku 1. Jako s předchozí dva kurzy, jsme zalomen vložení v rámci rozsahu transakce zajistit nedělitelnost. Umožňují s začít!

## <a name="step-1-creating-the-display-interface"></a>Krok 1: Vytvoření rozhraní zobrazení

V tomto kurzu bude skládat z jedné stránky, který je rozdělený do dvou oblastí: oblast zobrazení a vkládání oblast. Zobrazení rozhraní, které vytvoříme v tomto kroku, zobrazí produkty v GridView a obsahuje tlačítko s názvem procesu dodávky produktu. Při kliknutí na toto tlačítko zobrazení rozhraní se nahradí vkládání rozhraní, což je znázorněno na obrázku 1. Zobrazení rozhraní vrátí po přidání produkty z dodávky nebo klepnutí na tlačítka Storno. Vytvoříme rozhraní vkládání v kroku 2.

Při vytvoření stránky, která má dvě rozhraní, pouze jeden z nich je viditelná najednou, každý rozhraní je umístěn obvykle v rámci [ovládací prvek panelu webu](http://www.w3schools.com/aspnet/control_panel.asp), který slouží jako kontejner pro další ovládací prvky. Naši stránku proto bude mít dva ovládací prvky Panel jeden pro každé rozhraní.

Začněte otevřením `BatchInsert.aspx` stránku `BatchData` složku a přetáhněte panelu z panelu nástrojů na návrháře (viz obrázek 2). Nastavit panelu s `ID` vlastnost `DisplayInterface`. Při přidávání panelu do návrháře, jeho `Height` a `Width` vlastnosti jsou nastavené 50px a 125px, v uvedeném pořadí. Zruší se tyto hodnoty vlastností v okně Vlastnosti.


[![Přetáhněte panelu z panelu nástrojů na návrháře](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**Obrázek 2**: přetáhněte panelu z panelu nástrojů na návrháře ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image6.png))


V dalším kroku přetáhněte ovládací prvek tlačítko a GridView do panelu. Nastavte na tlačítko s `ID` vlastnost `ProcessShipment` a jeho `Text` vlastnost do procesu dodávky produktu. Nastavit GridView s `ID` vlastnost `ProductsGrid` a z jeho inteligentních značek navázat jej na nové ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource načítat data z `ProductsBLL` třídu s `GetProducts` metoda. Vzhledem k tomu, že tato rutina GridView se používají pouze pro zobrazení dat, nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný). Kliknutím na tlačítko Dokončit ukončete průvodce Konfigurace zdroje dat.


[![Zobrazit Data vrácená z metody třídy ProductsBLL s GetProducts](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**Obrázek 3**: Zobrazit Data vrácená z `ProductsBLL` třídu s `GetProducts` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image9.png))


[![Nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný)](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**Obrázek 4**: rozevíracího seznamu jsou uvedeny ve UPDATE, INSERT a odstranit karty na hodnotu (žádná) ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image12.png))


Po dokončení Průvodce ObjectDataSource, Visual Studio přidá BoundFields a vlastnost CheckBoxField pro produkt datová pole. Odeberte všechny ale na `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, a `Discontinued` pole. Nebojte se provádět jakékoli estetické vlastní nastavení. Rozhodli k formátování `UnitPrice` pole jako hodnotu měny přeuspořádány pole a přejmenovat několik polí `HeaderText` hodnoty. Rutina GridView stránkování a řazení podporu zaškrtnutím políček Povolit stránkování a Povolit řazení v GridView s inteligentním také nakonfigurujte.

Po přidání ovládacích prvků panelů, tlačítko, rutina GridView a ObjectDataSource a přizpůsobení pole s GridView, stránku s deklarativní by měl vypadat takto:


[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

Všimněte si, že kód pro tlačítko a GridView zobrazí v rámci otevření a zavření `<asp:Panel>` značky. Vzhledem k tomu, že tyto ovládací prvky jsou v rámci `DisplayInterface` panelech jsme můžete skrýt je jednoduše nastavení panelu s `Visible` vlastnost `False`. Krok 3 zjistí prostřednictvím kódu programu změna panelu s `Visible` zobrazíte jedno rozhraní při dalších skrytí klikněte na vlastnost v reakci na tlačítko.

Chcete-li zobrazit naše průběh prostřednictvím prohlížeče chvíli trvat. Jak je vidět na obrázku 5, měli byste vidět tlačítko procesu dodávky produktu nad GridView, obsahující seznam produktů deset najednou.


[![GridView seznamy produktů a nabízí řazení a stránkování možnosti](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**Obrázek 5**: GridView uvádí produktů a nabízí řazení a stránkování možnosti ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Krok 2: Vytvoření rozhraní vložení

Pomocí zobrazení rozhraní dokončení jsme re připravené k vytvoření rozhraní vkládání. V tomto kurzu mohli s vytvořit vkládání rozhraní, které zobrazí výzvu pro jednu hodnotu dodavatele a kategorie a pak může uživatel zadat až pět názvy produktů a hodnoty cena jednotky. S tímto rozhraním uživatel může přidávat jedné do pěti nové produkty, které všechny sdílejí stejné kategorii a dodavatele, ale mají názvy produktů jedinečný a ceny.

Spuštění přetažením panelu z panelu nástrojů na návrháře jeho umístění pod existující `DisplayInterface` panelu. Nastavit `ID` vlastnost tohoto objektu nově přidaná panelu `InsertingInterface` a nastavit jeho `Visible` vlastnost `False`. Přidáme kód, který nastaví `InsertingInterface` Panel s `Visible` vlastnost `True` v kroku 3. Také vyčistit panelu s `Height` a `Width` hodnot vlastností.

Dále je potřeba vytvořit vkládání rozhraní, které se zobrazí zpět na obrázku 1. Toto rozhraní se může vytvořit prostřednictvím řady různých způsobů HTML, ale budeme používat přímočará jeden: čtyři sloupce, sedm řádek tabulky.

> [!NOTE]
> Při zadávání kódu pro kód HTML `<table>` elementy, chci raději použít zobrazení zdroje. Když Visual Studio jsou nástroje pro přidání `<table>` prvků prostřednictvím návrháře návrháře zdá se, že všechny příliš ochotni vložit nevyžádaný pro `style` nastavení do kódu. Jakmile mám vytvořené `<table>` značek, I obvykle vrátíte do návrháře přidat ovládací prvky webového a nastavte jejich vlastnosti. Při vytváření tabulky pomocí předem určené sloupců a řádků nechci pomocí statické HTML místo [ovládací prvek webu tabulky](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) vzhledem k tomu, že všechny ovládací prvky webového umístit do ovládacího prvku Web tabulky lze přistupovat pouze pomocí `FindControl("controlID")` vzor. , Ale používám ovládacích prvků tabulka pro dynamicky velikost tabulky (ty, jejichž řádky nebo sloupce jsou založené na některé databáze nebo definované uživatelem kritérií), od Web tabulky řízení konstruovat prostřednictvím kódu programu.


Zadejte následující kód v rámci `<asp:Panel>` značek `InsertingInterface` panelu:


[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

To `<table>` značek nezahrnuje všechny ovládací prvky webového ještě těch, které přidáme na okamžik. Všimněte si, aby se každý `<tr>` element obsahuje konkrétní nastavení třídy CSS: `BatchInsertHeaderRow` pro řádek záhlaví, kde bude přejděte dodavatele a kategorie DropDownLists; `BatchInsertFooterRow` pro řádek zápatí stránky, kde přidat produkty z dodávky a zrušit tlačítka přejde; a střídavých `BatchInsertRow` a `BatchInsertAlternatingRow` hodnot řádků, které budou obsahovat produktu a jednotky cena TextBox – ovládací prvky. I jste vytvořili odpovídající tříd CSS ve `Styles.css` souboru umožnit vkládání rozhraní vzhled podobné GridView a DetailsView ovládací prvky jsme sunout používaných v celém tyto kurzy. Níže jsou uvedeny tyto třídy CSS.


[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

Pomocí této značky zadané vrátíte do návrhového zobrazení. To `<table>` by měl zobrazit jako tabulku čtyři sloupce, řádku sedm v návrháři, jak ukazuje obrázek 6.


[![Vkládání rozhraní se skládá čtyři sloupce, sedm řádek tabulky](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**Obrázek 6**: tvoří vkládání rozhraní čtyři sloupce, sedm řádek tabulky ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image18.png))


Jsme re nyní připraveni k přidávání ovládacích prvků webové rozhraní vkládání. Přetáhněte dvě DropDownLists z panelu nástrojů na příslušné buněk v tabulce, jednu pro dodavatele a jeden pro kategorii.

Nastavte rozevírací seznam s dodavatele `ID` vlastnost `Suppliers` a navázat jej nové ObjectDataSource s názvem `SuppliersDataSource`. Konfigurace nového ObjectDataSource načíst data z `SuppliersBLL` třídu s `GetSuppliers` metoda a sadu aktualizace kartě s rozevíracím seznamu na (žádný). Kliknutím na tlačítko Dokončit ukončete průvodce.


[![Konfigurace ObjectDataSource lze pomocí této metody GetSuppliers SuppliersBLL třídu s](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**Obrázek 7**: Konfigurace ObjectDataSource pro použití `SuppliersBLL` třídu s `GetSuppliers` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image21.png))


Mít `Suppliers` rozevírací seznam zobrazení `CompanyName` pole data a použití `SupplierID` data pole jako jeho `ListItem` s hodnoty.


[![Pole dat NázevSpolečnosti zobrazovat a používat KódDodavatele jako hodnota](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**Obrázek 8**: zobrazení `CompanyName` pole Data a použití `SupplierID` jako hodnotu ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image24.png))


Název druhé rozevírací seznam `Categories` a navázat jej nové ObjectDataSource s názvem `CategoriesDataSource`. Konfigurace `CategoriesDataSource` ObjectDataSource používat `CategoriesBLL` třídu s `GetCategories` metoda; nastavte rozevírací seznam seznamů v aktualizaci a odstranění karty na (žádný) a klikněte na tlačítko Dokončit ukončete průvodce. Nakonec máte zobrazení rozevírací seznam `CategoryName` pole data a použití `CategoryID` jako hodnotu.

Poté, co tyto dvě DropDownLists byly přidány a vázána na ObjectDataSources správně nakonfigurované, by mělo vypadat jako obrázek 9 obrazovky.


[![Řádek záhlaví teď obsahuje dodavatelé a DropDownLists kategorií](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**Obrázek 9**: záhlaví řádků nyní obsahuje `Suppliers` a `Categories` DropDownLists ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image27.png))


Nyní potřebujeme vytvořit textových polí shromažďovat název a ceny pro každý nový produkt. Přetáhněte ovládací prvek textové pole z panelu nástrojů na návrháře pro každou z pěti produktu název a cena řádky. Nastavte `ID` vlastnosti textových polí na `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`a tak dále.

Přidat CompareValidator po každém z nich jednotkové ceny textových polí, nastavení `ControlToValidate` vlastnost na příslušné `ID`. Také nastavit `Operator` vlastnost `GreaterThanEqual`, `ValueToCompare` na hodnotu 0, a `Type` k `Currency`. Tato nastavení, požádejte CompareValidator pro zajištění cenu, pokud zadali, platná měna hodnotu, která je větší než nebo rovna hodnotě nula. Nastavte `Text` vlastnost \*, a `ErrorMessage` ceny musí být větší než nebo rovna hodnotě nula. Navíc prosím vynechejte symboly měny.

> [!NOTE]
> Vkládání rozhraní nezahrnuje všechny ovládací prvky RequiredFieldValidator, i když `ProductName` pole `Products` tabulky databáze nepovoluje `NULL` hodnoty. Je to proto, že chceme, aby mohl uživatel zadat až pět produkty. Například pokud uživatel byly zajistit cena název a jednotka produktu pro první tři řádky, ponechat prázdné, poslední dva řádky d právě přidáme tři nové produkty do systému. Protože `ProductName` je požadováno, ale budeme muset kontrola prostřednictvím kódu programu, ujistěte se, že pokud jednotkové ceny je zadán, je zadána hodnota odpovídající název produktu. Tato kontrola v kroku 4 jsme budete řešení.


Při ověřování vstupu uživatele s, CompareValidator sestavy neplatná data, pokud hodnota obsahuje symbolu měny. Přidáte $ před každou jednotkové ceny textová pole, která bude sloužit jako vizuální upozornění, která dá pokyn, uživatelům při zadávání cenu vynechejte symbolu měny.

Nakonec přidání ovládacího prvku ValidationSummary v rámci `InsertingInterface` panelu nastavení jeho `ShowMessageBox` vlastnost `True` a jeho `ShowSummary` vlastnost `False`. S tímto nastavením Pokud uživatel zadá hodnotu ceny neplatný jednotky, hvězdičku se zobrazí vedle problematické ovládacích prvcích TextBox a ValidationSummary zobrazí messagebox straně klienta, který zobrazuje chybovou zprávu, kterou jsme dříve zadaný.

V tomto okamžiku by mělo vypadat obrazovky jako obrázek 10.


[![Vkládání rozhraní nyní zahrnuje textových polí pro produkty názvy a ceny](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**Obrázek 10**: vkládání rozhraní nyní zahrnuje textových polí pro názvy produktů a ceny ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image30.png))


Další, je potřeba přidat produktů přidat z dodávky a Storno do řádku zápatí. Tlačítko přetáhněte dva ovládací prvky z panelu nástrojů do zápatí rozhraní vkládání nastavení tlačítka `ID` vlastnosti, které chcete `AddProducts` a `CancelButton` a `Text` vlastnosti, které chcete přidat produkty z dodávky a zrušit, v uvedeném pořadí. Kromě toho nastavit `CancelButton` ovládacího prvku s `CausesValidation` vlastnost `false`.

Nakonec je potřeba přidat ovládací prvek webového popisek, který se zobrazí stavové zprávy pro tato dvě rozhraní. Například pokud uživatel úspěšně přidá nové dodávky produktů, chceme vraťte se na rozhraní zobrazení a zobrazí potvrzovací zpráva. Pokud však uživatel poskytuje cenu pro nového produktu, ale nechá vypnout název produktu, musíme zobrazí zprávu s upozorněním od `ProductName` pole je povinné. Vzhledem k tomu, že je třeba tuto zprávu zobrazíte pro obě rozhraní, umístěte ho v horní části stránky mimo panely.

Přetáhněte ovládací prvek popisek webového z panelu nástrojů do horní části stránky v návrháři. Nastavte `ID` vlastnost `StatusLabel`, zrušte na `Text` vlastnost a nastavte `Visible` a `EnableViewState` vlastnosti, které chcete `False`. Jak jsme mohli vidět v předchozích kurzy, nastavení `EnableViewState` vlastnost `False` umožňuje prostřednictvím kódu programu změnit hodnoty popisek s vlastností a nechat je automaticky vrátit zpět na výchozí hodnoty na následné zpětné volání. Tato funkce zjednodušuje kód pro zobrazující zprávu o stavu v odpovědi na některé akce uživatele, který zmizí na následné zpětné volání. Nakonec nastavte `StatusLabel` ovládacího prvku s `CssClass` vlastnost na upozornění, což je název třídy CSS definovaná v `Styles.css` který zobrazí text velký, kurzíva, tučné písmo, red písmeny.

Obrázek 11 ukazuje návrháři sady Visual Studio, po přidání a nakonfigurování popisku.


[![Umístěte ovládací prvek StatusLabel výše dva ovládací prvky Panel](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**Obrázek 11**: místní `StatusLabel` ovládací prvek nahoře dva ovládací prvky Panel ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Krok 3: Přepínání mezi zobrazení a vkládání rozhraní

V tuto chvíli jsme vyplnili kód pro naše zobrazení a vkládání rozhraní, ale nemůžeme re stále ponechaná na dvě úlohy:

- Přepínání mezi zobrazení a vkládání rozhraní
- Přidání produkty v dodávky do databáze

V současné době rozhraní zobrazení je viditelná ale rozhraní vkládání skryt. Důvodem je, že `DisplayInterface` Panel s `Visible` je nastavena na `True` (výchozí hodnota), při `InsertingInterface` Panel s `Visible` je nastavena na `False`. Chcete-li přepnout mezi dvě rozhraní, potřebujeme jen k přepnutí každý ovládací prvek s `Visible` hodnotu vlastnosti.

Chceme přesunout z rozhraní zobrazení rozhraní vkládání při kliknutí na tlačítko procesu dodávky produktu. Proto vytvořit obslužnou rutinu události pro toto tlačítko s `Click` událost, která obsahuje následující kód:


[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

Tento kód jednoduše skryje `DisplayInterface` panelu a ukazuje `InsertingInterface` panelu.

V dalším kroku vytváření obslužných rutin událostí pro přidání produkty z dodávky a tlačítko Zrušit ovládacích prvků v rozhraní vkládání. Pokud některý z těchto tlačítek po kliknutí na musíme vrátit se zpátky do rozhraní zobrazení. Vytvoření `Click` obslužné rutiny události pro obě tlačítko ovládací prvky tak, aby volání `ReturnToDisplayInterface`, metodu přidáme na okamžik. Kromě skrytí `InsertingInterface` panely a zobrazení `DisplayInterface` panelech `ReturnToDisplayInterface` metoda musí vrátit ovládací prvky webového stavu předem úpravy. To zahrnuje nastavení DropDownLists `SelectedIndex` vlastnosti 0 a vymazání `Text` vlastností ovládacích prvků textového pole.

> [!NOTE]
> Zvažte, co může dojít, pokud jsme dodán t vrátit ovládacích prvků do jejich předem úpravy stavu před vrácením rozhraní zobrazení. Uživatel může klikněte na tlačítko procesu dodávky produktu, zadejte produkty z dodávky a pak klikněte na tlačítko Přidat produkty z dodávky. To by přidejte produkty a vrátí uživatele k zobrazení rozhraní. V tomto okamžiku by uživatel chtít přidat jiné dodávky. Po kliknutí na tlačítko procesu dodávky produktu, které by mohly vrátit rozhraní vložení, ale rozevírací seznam výběry a textové pole hodnot by stále možné naplnit jejich předchozí hodnoty.


[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

Obě `Click` jednoduše volání obslužné rutiny událostí `ReturnToDisplayInterface` metoda, i když se vrátíme k produktům přidat z dodávky `Click` obslužné rutiny událostí v kroku 4 a přidejte kód pro uložení produkty. `ReturnToDisplayInterface` Spustí vrácením `Suppliers` a `Categories` DropDownLists jejich první možnosti. Dvě konstanty `firstControlID` a `lastControlID` označit počáteční a koncové hodnoty řízení indexu v názvu produktu název a jednotka cena textových polí v vkládání rozhraní a jsou použity v hranice `For` smyčky, která nastaví `Text`vlastností ovládacích prvků textového pole zpět na prázdný řetězec. Nakonec panelů `Visible` se obnoví tak, aby rozhraní vkládání skrytá a rozhraní zobrazení zobrazí.

Za chvíli k otestování této stránce v prohlížeči. Při první návštěvě stránky zobrazení rozhraní vám ukáže, jak je vidět na obrázku 5. Klikněte na tlačítko procesu dodávky produktu. Bude odeslat zpět na stránku a měli byste vidět vkládání rozhraní, jak ukazuje obrázek 12. Buď přidejte produkty z tlačítka dodávky nebo instalaci zrušte kliknutím na tlačítko se vrátíte do rozhraní zobrazení.

> [!NOTE]
> Při zobrazení rozhraní vkládání, za chvíli k otestování CompareValidators na do jednotkové ceny textových polí. Měli byste vidět klienta messagebox upozornění, když kliknete na Přidat produkty z dodávky tlačítka s hodnoty měny neplatný nebo ceny hodnotu menší než nula.


[![Rozhraní vložení se zobrazí po kliknutí na tlačítko dodávky produktu procesu](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**Obrázek 12**: vkládání rozhraní se zobrazí po kliknutí na tlačítko dodávky produktu procesu ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Krok 4: Přidání produkty

Všechny, které zůstává pro tento kurz je určen k uložení produkty do databáze v produktech přidat z dodávky tlačítko s `Click` obslužné rutiny události. Můžete to provést tak, že vytvoříte `ProductsDataTable` a přidání `ProductsRow` instance pro každý ze zadaných názvů produktu. Jednou tyto `ProductsRow` s byly přidány budeme volání `ProductsBLL` třídu s `UpdateWithTransaction` metoda předávání v `ProductsDataTable`. Odvolat, který `UpdateWithTransaction` metoda, která byla vytvořená zpět v [zabalení změny databáze v rámci transakce](wrapping-database-modifications-within-a-transaction-vb.md) kurz, předává `ProductsDataTable` k `ProductsTableAdapter` s `UpdateWithTransaction` metoda. Zde, je spuštění ADO.NET transakce a problémů TableAdatper `INSERT` příkaz k databázi pro každý přidat `ProductsRow` DataTable. Za předpokladu, že všechny produkty, které jsou přidány bez chyby, že je transakce potvrzena, v opačném případě je vrácena zpět.

Kód pro přidání produkty z dodávky tlačítko s `Click` obslužné rutiny události musí také provést bit kontroly chyb. Vzhledem k tomu, že neexistují žádné RequiredFieldValidators použít v rozhraní vkládání, může uživatel zadat při vynechání jeho název ceny pro produkt. Vzhledem k tomu, že název produktu s je povinný, pokud takové podmínku, kterou se musíme upozornění uživatele a není pokračovat vložení. Kompletní `Click` následuje kód obslužné rutiny událostí:


[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

Obslužné rutiny události začne zajistit, aby `Page.IsValid` vlastnost vrací hodnotu `True`. Vrátí-li `False`, pak to znamená, jeden nebo více CompareValidators se hlásí neplatná data; v takovém případě jsme nechcete, aby k pokusu o vložení zadané produkty nebo jsme nakonec se výjimku při pokusu o přiřazení uživatele zadali jednotkové ceny hodnoty na `ProductsRow` s `UnitPrice` vlastnost.

Další, nové `ProductsDataTable` se vytvoří instance (`products`). A `For` smyčky je použít k iteraci v rámci produktu název a jednotka cena textových polí a `Text` do místní proměnné jsou načteny vlastnosti `productName` a `unitPrice`. Pokud uživatel zadá hodnotu do jednotkové ceny ale ne pro odpovídající název produktu `StatusLabel` zobrazí zprávu, pokud jste zadali, ceny jednotku, kterou jste musí také obsahovat název produktu a obslužné rutiny události je byl ukončen.

Pokud byl zadaný název produktu, a nové `ProductsRow` instance je vytvořený pomocí `ProductsDataTable` s `NewProductsRow` metoda. Tento nový `ProductsRow` instance s `ProductName` je nastavena na aktuální produkt název textového pole při `SupplierID` a `CategoryID` vlastnosti jsou přiřazeny k `SelectedValue` vlastnosti DropDownLists v hlavičce vkládání rozhraní s. Když uživatel zadá hodnotu za cenu produktu s, nebude přiřazen k `ProductsRow` instance s `UnitPrice` vlastnost; jinak, je vlastnost nepřiřazené, vlevo, která bude mít za následek `NULL` hodnota `UnitPrice` v databázi. Nakonec `Discontinued` a `UnitsOnOrder` vlastnosti přiřazené hodnoty pevně `False` a 0, v uvedeném pořadí.

Po přiřazení vlastnosti `ProductsRow` instance je přidán do `ProductsDataTable`.

Po dokončení `For` smyčky, jsme zkontrolujte, zda byly přidány všechny produkty. Uživatel může po všech klikli přidat produkty z dodávky před vstupem všechny názvy produktů nebo ceny. Pokud je alespoň jeden produktu v `ProductsDataTable`, `ProductsBLL` třídu s `UpdateWithTransaction` metoda je volána. Dále je odrážejí data do `ProductsGrid` GridView tak, aby nově přidané produkty se zobrazí v zobrazení rozhraní. `StatusLabel` Se aktualizuje a zobrazí zprávu s potvrzením a `ReturnToDisplayInterface` je vyvolána, skrytí vkládání rozhraní a zobrazující rozhraní zobrazení.

Pokud byla zadána žádná produktů, vkládání rozhraní zůstane zobrazené ale zprávy, které nebyly přidány žádné produkty. Zadejte názvy produktů a jednotkové ceny v textových polí se zobrazí.

Obrázek s 13, 14 až 15 zobrazit vkládání a zobrazit rozhraní v akci. Obrázek 13 uživatelem zadaná hodnota cena jednotky bez odpovídající názvu produktu. Obrázek 14 zobrazuje rozhraní zobrazení po tři nové produkty byly přidány úspěšně, a zobrazí dvě nově přidané produkty obrázek 15 GridView (třetí ten se na předchozí stránce).


[![Název produktu je nutné při zadávání jednotkové ceny](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**Obrázek 13**: název produktu A je potřeba při zadávání jednotkové ceny ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image39.png))


[![Tři nové Veggies byly přidány pro dodavatele Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**Obrázek 14**: tři nové Veggies byly přidány pro s Mayumi dodavatele ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image42.png))


[![Nové produkty naleznete na poslední stránku GridView](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**Obrázek 15**: nové produktům najdete na poslední stránku GridView ([Kliknutím zobrazit obrázek v plné velikosti](batch-inserting-vb/_static/image45.png))


> [!NOTE]
> Dávkové vložení logiky použili v tomto kurzu zabalí vložení v rámci oboru transakce. Chcete-li to ověřit, záměrně zavést na úrovni databáze chybu. Například místo přiřazení nové `ProductsRow` instance s `CategoryID` vlastnost na vybranou hodnotu v `Categories` rozevírací seznam, přiřadit ho na hodnotu jako `i * 5`. Zde `i` indexeru smyčky a má hodnoty v rozsahu od 1 do 5. Proto při přidávání dvě nebo více produktů v dávce vložit první produktu budou mít platný `CategoryID` bude mít hodnotu (5), ale následné produkty `CategoryID` hodnoty, které neodpovídají až `CategoryID` hodnoty ve `Categories` tabulky. Čistý efekt je, že při první `INSERT` bude úspěšné, následných ty se nezdaří s narušení omezení pro cizí klíč. Vzhledem k tomu, že je dávkové vložení atomic, první `INSERT` bude vrácena zpět, vrácení začal databázi do stavu před dávky vložit procesu.


## <a name="summary"></a>Souhrn

Přes toto a předchozí dva kurzy jsme vytvořili rozhraní, které umožňují pro aktualizaci, odstranění, a vkládání balíků dat, které použít podpora transakcí jsme přidali do Data Access Layer v [zabalení změny databáze v rámci transakce](wrapping-database-modifications-within-a-transaction-vb.md) kurzu. Pro určité scénáře, takové dávkové zpracování uživatelského rozhraní výrazně zlepšit efektivitu koncový uživatel vyjímání dolů na počet kliknutí, zpětná vystavení a kontext klávesnice myši přepínače, při zachování také integritu podkladová data.

V tomto kurzu dokončení naše pohled na práci s daty dávkové. Jsou zde popsány další sadu kurzy celou řadu pokročilých scénářích Data Access Layer, včetně použití uložených procedur v TableAdapter s metodami, konfigurace nastavení připojení a příkaz úrovně v DAL, šifrování připojovací řetězce a další!

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Hilton Giesenow a S ren Jakub Lauritsen. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](batch-deleting-vb.md)
