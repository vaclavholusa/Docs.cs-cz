---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Dávkové aktualizace (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Zjistěte, jak aktualizace více záznamů databáze v rámci jedné operace. Ve vrstvě uživatelského rozhraní při sestavování ovládacího prvku GridView, kde každý řádek je možné upravovat. V datech...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: 7830e010ea8dbf6ce9bd59154c10eb4c30a3dceb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363310"
---
<a name="batch-updating-vb"></a>Dávkové aktualizace (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) nebo [stahovat PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Zjistěte, jak aktualizace více záznamů databáze v rámci jedné operace. Ve vrstvě uživatelského rozhraní při sestavování ovládacího prvku GridView, kde každý řádek je možné upravovat. V datové vrstvě přístupu jsme zabalit více operací aktualizace v rámci transakce zajistit, že všechny aktualizace proběhnou úspěšně, nebo všechny aktualizace jsou vráceny zpět.


## <a name="introduction"></a>Úvod

V [předchozím kurzu](wrapping-database-modifications-within-a-transaction-vb.md) jsme viděli, jak rozšířit vrstvy přístupu k datům s přidanou podporou pro databázové transakce. Databázové transakce zaručit, že řadu příkazů změny dat, bude zacházeno jako jednu atomickou operaci, což zajistí, že selže, všechny změny, nebo všechny proběhne úspěšně. Pomocí této nízké úrovně vrstvy DAL funkce eliminuje jsme re připravené k zapnutí naši pozornost k vytvoření rozhraní pro úpravu dat služby batch.

V tomto kurzu vytvoříme ovládacího prvku GridView, kde každý řádek představuje upravitelné (viz obrázek 1). Protože každý řádek se vykreslí v jeho úpravy rozhraní tam s není nutné pro sloupec upravit, aktualizovat a stornovací tlačítka. Místo toho se nacházejí dvě aktualizace produktů tlačítka na stránce, která po kliknutí na vytvořit výčet řádky GridView a aktualizaci databáze.


[![Upravit je každý řádek v prvku GridView.](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Obrázek 1**: upravit je každý řádek v prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-updating-vb/_static/image2.png))


Začínáme s let!

> [!NOTE]
> V [provádění dávkových aktualizací](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) kurzu jsme vytvořili batch úpravy rozhraní, pomocí ovládacího prvku DataList. V tomto kurzu se liší od předchozí v, který se používá GridView a aktualizace služby batch se provádí v rámci oboru transakce. Po dokončení tohoto kurzu neváhejte se vrátit k předchozí kurzu a aktualizovat ho na použití přidali v předchozím kurzu funkce související s transakce databáze.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Zkoumání kroky pro vytvoření upravovat všechny řádky GridView

Jak je popsáno v [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) kurzu prvku GridView nabízí vestavěnou podporu pro úpravy podkladová data na základě na řádek. Interně prvku GridView poznámky řádků je možné upravovat prostřednictvím jeho [ `EditIndex` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Jak prvku GridView svázaný s jeho zdrojovými daty, zkontroluje každý řádek zobrazíte-li index řádku, který se rovná hodnotě `EditIndex`. Pokud ano, s, která pole jsou vykreslovány pomocí jejich úpravy řádku rozhraní. Úpravy rozhraní pro BoundFields, je textové pole jejichž `Text` vlastnost je přiřazena hodnota datového pole určená Vlastnost BoundField s `DataField` vlastnost. Pro vlastností TemplateField `EditItemTemplate` je použito místo `ItemTemplate`.

Připomínáme, že úpravy pracovní postup se spustí po kliknutí tlačítko Upravit řádek s. To způsobí, že zpětné volání, nastaví prvek GridView s `EditIndex` na kliknutí na řádku s indexem a znovu připojí data k mřížce. Když řádek s je kliknutí na tlačítko Storno, na zpětného odeslání `EditIndex` je nastavena na hodnotu `-1` před obnovení vazby dat k mřížce. Protože řádky GridView s zahájení indexování od nuly, nastavení `EditIndex` k `-1` má za následek zobrazení GridView v režimu jen pro čtení.

`EditIndex` Vlastnost funguje dobře pro úpravy na řádek, ale není určená pro úpravy služby batch. Chcete-li upravovat celou GridView, potřebujeme mít každý řádek vykreslit pomocí rozhraní pro úpravy. Nejjednodušší způsob, jak to provést je vytvořit, kde každé upravitelné pole se implementuje podle TemplateField s jeho úpravy rozhraní `ItemTemplate`.

Příštích několika krocích vytvoříme zcela upravitelné ovládacího prvku GridView. V kroku 1 začneme vytvořením prvku GridView a jeho ObjectDataSource a jeho BoundFields a třídě CheckBoxField převést vlastností TemplateField. V krocích 2 a 3 přejdeme rozhraní pro úpravy z vlastností TemplateField `EditItemTemplate` s jejich `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Krok 1: Zobrazení informací o produktu

Předtím, než jsme starat o vytváření GridView kde jsou řádky se upravovat, ať s začněte tím, že jednoduše zobrazení informací o produktu. Otevřít `BatchUpdate.aspx` stránku `BatchData` složky a GridView přetáhněte z panelu nástrojů do návrháře. Nastavit prvek GridView s `ID` k `ProductsGrid` a z inteligentních značek, vyberte a vytvořte jeho vazbu nového prvku ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource načíst data z `ProductsBLL` třída s `GetProducts` metody.


[![Konfigurace ObjectDataSource pomocí třídy ProductsBLL](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Obrázek 2**: Konfigurace ObjectDataSource k použití `ProductsBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-updating-vb/_static/image4.png))


[![Načíst pomocí metody GetProducts Data produktu.](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Obrázek 3**: načtení dat pomocí produktu `GetProducts` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-updating-vb/_static/image6.png))


Jako prvku GridView jsou funkce úprav ObjectDataSource s navrženy pro práci na základě na řádek. Pokud chcete aktualizovat sadu záznamů, potřebujeme psát hodně kódu ve třídě použití modelu code-behind stránky s ASP.NET, která seskupuje data do dávek a předává je BLL. Proto nastavte rozevírací seznamy v prvku ObjectDataSource s UPDATE, INSERT a DELETE karty na (žádný). Kliknutím na Dokončit dokončíte průvodce.


[![Nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Obrázek 4**: Nastavte rozevírací seznam obsahuje v UPDATE, INSERT a odstranit záložky (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-updating-vb/_static/image8.png))


Po dokončení Průvodce nakonfigurovat zdroj dat, ObjectDataSource s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

Dokončuje se Průvodce konfigurace zdroje dat navíc způsobí, že Visual Studio k vytvoření BoundFields a třídě CheckBoxField pro datová pole produktu v prvku GridView. Pro účely tohoto kurzu nechte s pouze povolit uživatelům zobrazení a úprava s názvem produktu, kategorie, ceny a ukončená stav. Odeberte všechny kromě na `ProductName`, `CategoryName`, `UnitPrice`, a `Discontinued` pole a přejmenování `HeaderText` vlastnosti první tři pole na produkt, kategorie a ceny, v uvedeném pořadí. A konečně zaškrtněte zaškrtávací políčka Povolit stránkování a Povolit řazení v prvku GridView s inteligentním.

V tuto chvíli má tři BoundFields v prvku GridView (`ProductName`, `CategoryName`, a `UnitPrice`) a třídě CheckBoxField (`Discontinued`). Potřebujeme převést vlastností TemplateField těchto čtyř polí a poté přesuňte úpravy rozhraní z TemplateField s `EditItemTemplate` k jeho `ItemTemplate`.

> [!NOTE]
> Prozkoumali jsme vytvoření a úprava vlastností TemplateField v [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurzu. Projdeme kroky převedením BoundFields ve třídě CheckBoxField do vlastností TemplateField a jejich úpravy definice rozhraní v jejich `ItemTemplate` s, ale pokud jste nevíte rady nebo potřebujete občerstvit don t váhání jako zpětná reference na tento starší kurz.


Na prvek GridView s inteligentní značku klikněte na odkaz Upravit sloupce otevřete dialogové okno pole. V dalším kroku vyberte každé pole a klikněte na odkaz TemplateField převést toto pole.


![Převést existující BoundFields ve třídě CheckBoxField vlastností TemplateField](batch-updating-vb/_static/image5.gif)

**Obrázek 5**: převést existující BoundFields ve třídě CheckBoxField vlastností TemplateField


Teď, když je každé pole TemplateField, můžeme znovu připravený k přesunutí úpravy rozhraní z `EditItemTemplate` s `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Krok 2: Vytvoření`ProductName`,`UnitPrice`, a`Discontinued`úpravy rozhraní

Vytváří `ProductName`, `UnitPrice`, a `Discontinued` úpravy rozhraní jsou téma tento krok a jsou poměrně jednoduchý, protože každé rozhraní je již definován v TemplateField s `EditItemTemplate`. Vytváří `CategoryName` úpravy rozhraní je trochu složitější, protože je nutné vytvořit DropDownList příslušné kategorie. To `CategoryName` úpravy rozhraní je řešeny v kroku 3.

Umožní s začínat `ProductName` TemplateField. Klikněte na odkaz Upravit šablony z ovládacího prvku GridView s inteligentním a přejít k podrobnostem `ProductName` TemplateField s `EditItemTemplate`. Vyberte textového pole, zkopírujte do schránky a vložte ho do `ProductName` TemplateField s `ItemTemplate`. Změna textového pole s `ID` vlastnost `ProductName`.

V dalším kroku přidejte RequiredFieldValidator k `ItemTemplate` zajistit, že uživatel zadá hodnotu pro každý produkt s názvem. Nastavte `ControlToValidate` Vlastnost ProductName; `ErrorMessage` vlastnost je nutné zadat název produktu. a `Text` vlastnost \*. Po provedení těchto doplňky `ItemTemplate`, vaše obrazovka by měla vypadat podobně jako na obrázku 6.


[![Nyní TemplateField ProductName obsahuje textové pole a RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Obrázek 6**: `ProductName` TemplateField teď obsahuje textové pole a RequiredFieldValidator ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-updating-vb/_static/image10.png))


Pro `UnitPrice` úpravy rozhraní, začněte tím, že kopírování z textového pole `EditItemTemplate` k `ItemTemplate`. V dalším kroku umístit $ před do textového pole a nastavte jeho `ID` vlastnost UnitPrice a jeho `Columns` vlastnost na 8.

Přidejte také CompareValidator k `UnitPrice` s `ItemTemplate` tak, aby byl tímto uživatelem zadaná hodnota platná měny hodnotu větší než nebo rovna hodnotě 0,00 USD. Nastavit program pro ověření s `ControlToValidate` vlastnost UnitPrice, jeho `ErrorMessage` vlastnost, musíte zadat hodnotu měny platný. Prosím vynechat všechny měny symboly., jeho `Text` vlastnost \*, jeho `Type` vlastnost `Currency`, jeho `Operator` vlastnost `GreaterThanEqual`a jeho `ValueToCompare` vlastnost na hodnotu 0.


[![Umožňuje přidat CompareValidator zajistit Zadaná cena je nezáporná hodnota měny](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Obrázek 7**: Přidání CompareValidator zajistit Zadaná cena je nezáporná hodnota měny ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-updating-vb/_static/image12.png))


Pro `Discontinued` TemplateField můžete použít políčko již definována v `ItemTemplate`. Stačí nastavit jeho `ID` k vyřazeno a jeho `Enabled` vlastnost `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Krok 3: Vytvoření`CategoryName`úpravy rozhraní

Úpravy uživatelského rozhraní `CategoryName` TemplateField s `EditItemTemplate` obsahuje textové pole, která zobrazuje hodnotu `CategoryName` datové pole. Musíme nahraďte ovládacím prvkem DropDownList, který obsahuje seznam možných kategorií.

> [!NOTE]
> [Přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurz obsahuje důkladné a kompletní informace o přizpůsobení šablony zahrnout DropDownList textové pole na rozdíl od. Zde uvedené kroky jsou kompletní, zobrazí se jim tersely. Pro podrobnější pohled na vytváření a konfiguraci kategorií DropDownList, vraťte se do [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurzu.


Přetáhněte z panelu nástrojů do DropDownList `CategoryName` TemplateField s `ItemTemplate`a nastavte jeho `ID` k `Categories`. V tuto chvíli jsme byste obvykle definovali zdroj dat s DropDownLists prostřednictvím inteligentních značek, vytváření nového prvku ObjectDataSource. Ale tato možnost přidá ObjectDataSource v rámci `ItemTemplate`, jejímž výsledkem bude ObjectDataSource instance vytvořené pro každý řádek prvku GridView. Místo toho umožní vytvořit ObjectDataSource mimo GridView s vlastností TemplateField s. Ukončit úpravu šablony a prvku ObjectDataSource přetáhněte z panelu nástrojů na Návrhář pod `ProductsDataSource` ObjectDataSource. Název nového prvku ObjectDataSource `CategoriesDataSource` a nakonfigurujte ho na použití `CategoriesBLL` třída s `GetCategories` metody.


[![Konfigurace ObjectDataSource pomocí třídy CategoriesBLL](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Obrázek 8**: Konfigurace ObjectDataSource k použití `CategoriesBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-updating-vb/_static/image14.png))


[![Načtení kategorie dat pomocí GetCategories – metoda](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Obrázek 9**: načtení dat pomocí kategorie `GetCategories` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-updating-vb/_static/image16.png))


Protože tento prvek ObjectDataSource slouží pouze k načtení dat, nastavte rozevírací seznamy na kartách UPDATE a DELETE na (žádný). Kliknutím na Dokončit dokončíte průvodce.


[![Sada rozevírací seznamy v aktualizaci a odstranění karty na (žádný)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Obrázek 10**: Nastavte rozevírací seznam obsahuje v aktualizaci a odstranění karty (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-updating-vb/_static/image18.png))


Po dokončení průvodce, `CategoriesDataSource` s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

S `CategoriesDataSource` vytvoření a konfiguraci, vrátit `CategoryName` TemplateField s `ItemTemplate` a z s DropDownList inteligentní značky, klikněte na odkaz zvolit zdroj dat. V Průvodci konfigurací zdroje dat, vyberte `CategoriesDataSource` možnost v prvním rozevíracím seznamu a rozhodnout, že `CategoryName` použitý pro zobrazení a `CategoryID` jako hodnotu.


[![Svázat CategoriesDataSource DropDownList](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Obrázek 11**: DropDownList k vytvoření vazby `CategoriesDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-updating-vb/_static/image20.png))


V tomto okamžiku `Categories` DropDownList uvádí všechny kategorie, ale to není automaticky, ale vyberte odpovídající kategorii produktu, který je vázán na řádku prvku GridView. K tomu budeme muset nastavit `Categories` DropDownList s `SelectedValue` produktu s `CategoryID` hodnotu. Klikněte na odkaz upravit vlastnosti DataBindings z DropDownList s inteligentním a přidružte `SelectedValue` vlastnost s `CategoryID` datové pole, jak ukazuje obrázek 12.


![Hodnota ID kategorie produktů s svázat vlastnost SelectedValue DropDownList s](batch-updating-vb/_static/image12.gif)

**Obrázek 12**: vytvoření vazby produkt s `CategoryID` hodnota, která má DropDownList s `SelectedValue` vlastnost


Jeden poslední problém zůstane: Pokud máte t kódu produktu `CategoryID` zadána hodnota potom příkaz vázání dat na `SelectedValue` způsobí výjimku. Důvodem je, že DropDownList obsahuje pouze položky v kategoriích a nenabízí možnost pro tyto produkty, které mají `NULL` databáze hodnotu `CategoryID`. Chcete-li to napravit, nastavte DropDownList s `AppendDataBoundItems` vlastnost `True` a přidat novou položku do DropDownList, vynechání `Value` vlastnost z deklarativní syntaxe. To znamená, ujistěte se, že `Categories` DropDownList s deklarativní syntaxe vypadá takto:


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Poznámka: Jak `<asp:ListItem Value="">` --vyberte jednu – má jeho `Value` atribut explicitně nastavit na prázdný řetězec. Vraťte se do [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurz podrobnější informace o Proč je potřeba tato další položka DropDownList ke zpracování `NULL` případ a proč přiřazení `Value` vlastnost na prázdný řetězec, je nezbytné.

> [!NOTE]
> Existuje problém potenciální výkon a škálovatelnost tady, který je za zmínku. Protože každý řádek obsahuje DropDownList, který používá `CategoriesDataSource` jako zdroj dat `CategoriesBLL` třída s `GetCategories` volaná metoda *n* za stránku navštíví, kde *n* je počet řádky v prvku GridView. Tyto *n* volání `GetCategories` za následek *n* dotazy do databáze. Tento dopad na databázi může trh díky ukládání do mezipaměti vrácené kategorií v mezipaměti na základě žádosti nebo prostřednictvím ukládání do mezipaměti vrstvy pomocí ukládání do mezipaměti, závislost nebo velmi na základě krátkého formátu času vypršení platnosti SQL. Další informace o základě požadavku možnost ukládání do mezipaměti najdete v článku [ `HttpContext.Items` Store mezipaměti jednotlivé žádosti](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Krok 4: Dokončení úprav rozhraní

Jsme ve provedli několik změn do šablon GridView s bez pozastavení zobrazíte náš postup. Chcete-li zobrazit náš postup prostřednictvím prohlížeče chvíli trvat. Jak ukazuje obrázek 13, každý řádek je vykreslen pomocí jeho `ItemTemplate`, který obsahuje buňky s úpravy rozhraní.


[![Upravit je každý řádek prvku GridView](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Obrázek 13**: upravit je každý řádek prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-updating-vb/_static/image22.png))


Existuje několik menších problémů formátování, které jsme byste měli věnovat pozornost v tomto okamžiku. Nejprve, Všimněte si, že `UnitPrice` hodnota obsahuje čtyři desetinné čárky. Tento problém můžete vrátit `UnitPrice` TemplateField s `ItemTemplate` a z textového pole s inteligentní značky, klikněte na odkaz Upravit datové vazby. Dále určit, že `Text` vlastnost by měla být formátovaný jako číslo.


![Vlastnost Text naformátovat jako číslo](batch-updating-vb/_static/image14.gif)

**Obrázek 14**: formát `Text` vlastnost jako číslo


Za druhé, vám umožňují s center zaškrtávací políčko ve `Discontinued` sloupec (spíše než to zarovnané vlevo). Klikněte na Upravit sloupce z ovládacího prvku GridView s inteligentním a vyberte `Discontinued` TemplateField ze seznamu polí v levém dolním rohu. K podrobnostem `ItemStyle` a nastavit `HorizontalAlign` vlastnost na System Center, jak ukazuje obrázek 15.


![System Center ukončená zaškrtávací políčko](batch-updating-vb/_static/image15.gif)

**Obrázek 15**: System Center `Discontinued` zaškrtávací políčko


V dalším kroku přidejte ovládací prvek souhrnu ověření na stránku a nastavte jeho `ShowMessageBox` vlastnost `True` a jeho `ShowSummary` vlastnost `False`. Také přidat tlačítko webové ovládací prvky, které při kliknutí na, aktualizuje uživatelské změny s. Konkrétně, přidejte dva ovládací prvky tlačítka Web, nad prvku GridView a uvedený níže, nastavení oba ovládací prvky `Text` vlastnosti k aktualizaci produktů.

Od GridView s úpravy rozhraní je definováno v jeho vlastností TemplateField `ItemTemplate` s, `EditItemTemplate` s jsou nadbytečné a můžou se odstranit.

Po provedení výše uvedených změny formátování, přidávání ovládací prvky tlačítka a odebírání nadbytečné `EditItemTemplate` s, vaše stránka s deklarativní syntaxe by měl vypadat nějak takto:


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

Obrázek 16 zobrazuje tuto stránku při prohlížení prostřednictvím prohlížeče, po přidání ovládacích prvků webového tlačítko a formátování změny.


[![Stránka nyní obsahuje dvě tlačítka aktualizace produktů](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Obrázek 16**: na stránce teď zahrnuje dvě aktualizace produktů tlačítka ([kliknutím ji zobrazíte obrázek v plné velikosti](batch-updating-vb/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Krok 5: Aktualizace produktů

Když uživatel navštíví tuto stránku, bude proveďte své změny a klikněte na jeden z dvě tlačítka aktualizací produktů. Od tohoto okamžiku musíme nějakým způsobem uložit hodnoty uživatel zadal pro každý řádek do `ProductsDataTable` instance a předat ho do knihoven BLL metodu, která se pak předejte, který `ProductsDataTable` instance s vrstvou DAL `UpdateWithTransaction` metody. `UpdateWithTransaction` Metody, které jsme vytvořili v [předchozím kurzu](wrapping-database-modifications-within-a-transaction-vb.md), zajistí, že batch změny se pak zaktualizuje jako atomickou operaci.

Vytvořit metodu s názvem `BatchUpdate` v `BatchUpdate.aspx.vb` a přidejte následující kód:


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Tato metoda začíná tím, že získáme všechny produkty zpět v `ProductsDataTable` prostřednictvím volání BLL s `GetProducts` metody. Pak zobrazí `ProductGrid` GridView s [ `Rows` kolekce](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). `Rows` Kolekce obsahuje [ `GridViewRow` instance](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) pro každý řádek zobrazí v prvku GridView. Protože jsme se zobrazují maximálně deset řádků na stránce GridView s `Rows` kolekce bude obsahovat více než deset položek.

Pro každý řádek `ProductID` je převzatý z `DataKeys` kolekce a odpovídající `ProductsRow` vyberete ze `ProductsDataTable`. Čtyři ovládací prvky vstupu TemplateField je odkazováno prostřednictvím kódu programu a jejich hodnoty přiřazené `ProductsRow` instance s vlastností. Po každé GridView byly použity hodnoty řádků s aktualizovat `ProductsDataTable`, ho s předán BLL s `UpdateWithTransaction` metodu, která jako jsme viděli v předchozím kurzu, provede jednoduché volání s vrstvou DAL `UpdateWithTransaction` metoda.

Dávkové aktualizace algoritmus používaný pro účely tohoto kurzu aktualizuje každý řádek `ProductsDataTable` odpovídající řádek v prvku GridView, bez ohledu na to, jestli se informace o produktu s změnil. Při takové blind aktualizuje t nejsou obvykle problému s výkonem, se může vést k nadbytečný záznamy Pokud re auditování změny do tabulky databáze. Zpátky [provádění dávkových aktualizací](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) kurzu jsme prozkoumali dávkové aktualizace rozhraní pomocí prvku DataList a přidat kód, který by aktualizovat pouze ty záznamy, které byly ve skutečnosti upravil uživatel. Nebojte se využít techniky z [provádění dávkových aktualizací](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) aktualizovat kód v tomto kurzu se v případě potřeby.

> [!NOTE]
> Po vytvoření vazby zdroje dat na GridView prostřednictvím jeho inteligentních značek, Visual Studio automaticky přiřadí klíčové hodnoty datového zdroje s primární GridView s `DataKeyNames` vlastnost. Pokud jste se neváže ObjectDataSource do prvku GridView. pomocí inteligentních značek GridView s jak je uvedeno v kroku 1, pak budete muset ručně nastavit prvek GridView s `DataKeyNames` vlastnosti ProductID za účelem přístupu k `ProductID` hodnotu pro každý řádek prostřednictvím `DataKeys` kolekce.


Kód použitý v `BatchUpdate` je podobný, který používá v BLL s `UpdateProduct` metod, hlavní rozdíl je, že `UpdateProduct` metody pouze jediný `ProductRow` instance je načten z architektury. Kód, který se přiřadí vlastnosti `ProductRow` stejná mezi `UpdateProducts` metody a kódu v rámci `For Each` smyčky v `BatchUpdate`, jak je na celkový systém.

K dokončení tohoto kurzu, musíme mít `BatchUpdate` metoda vyvolána v případě kliknutí na některý z tlačítka aktualizací produktů. Vytváření obslužných rutin událostí pro `Click` událostí, které tyto dva ovládací prvky tlačítka a přidejte následující kód do obslužné rutiny událostí:


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

Nejdříve je provedeno volání `BatchUpdate`. Dále [ `ClientScript` vlastnost](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) slouží k vložení jazyka JavaScript, který se zobrazí prvek messagebox, která čte produkty byly aktualizovány.

K otestování tohoto kódu chvíli trvat. Navštivte `BatchUpdate.aspx` prostřednictvím prohlížeče, upravte počet řádků a klikněte na jedno z tlačítek aktualizací produktů. Za předpokladu, že nejsou žádné chyby ověření vstupu, měli byste vidět messagebox, která přečte že produkty byly aktualizovány. Pokud chcete ověřit atomicitu aktualizace, zvažte přidání náhodnou `CHECK` omezení, jako je ten, který zakazuje `UnitPrice` hodnoty 1234.56. Potom z `BatchUpdate.aspx`, upravte počet záznamů, nezapomeňte nastavit jeden produkt s `UnitPrice` hodnota, která má zakázaná hodnota (1234.56). Výsledkem by měl být chybu při kliknutím aktualizací produktů se změnami ostatních členů během tohoto dávkové operace vrátit zpět na původní hodnoty.

## <a name="an-alternativebatchupdatemethod"></a>Alternativu`BatchUpdate`– metoda

`BatchUpdate` Metoda jsme právě prozkoumat načte *všechny* produktů z knihoven BLL s `GetProducts` metoda a následně aktualizuje jenom ty záznamy, které se zobrazují v prvku GridView. Tento přístup je ideální, pokud prvku GridView nepoužívá stránkování, ale pokud ano, můžou existovat stovky, tisíce nebo desítek tisíců produktů, ale pouze deset řádků v prvku GridView. V takovém případě všechny produkty z databáze pouze k úpravám 10 z nich je menší než ideální.

Pro tyto typy situací označuje termín, zvažte použití následujících `BatchUpdateAlternate` metoda místo:


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` začne tím, že vytvoříte nová prázdná `ProductsDataTable` s názvem `products`. Následně prochází GridView s `Rows` kolekce a pro každý řádek získá informace o daném produktu pomocí BLL s `GetProductByProductID(productID)` metody. Načtený `ProductsRow` instance má jeho vlastnosti aktualizovat stejným způsobem jako `BatchUpdate`, ale po aktualizaci řádku je importovat do `products` `ProductsDataTable` prostřednictvím DataTable s [ `ImportRow(DataRow)` metoda](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Po `For Each` dokončení smyčky `products` obsahuje jeden `ProductsRow` instance pro každý řádek v prvku GridView. Od všech `ProductsRow` instancí jsou přidané do `products` (místo aktualizace), pokud jsme slepě předejte ji do `UpdateWithTransaction` metoda `ProductsTableAdatper` se pokusí vložit jednotlivých záznamů do databáze. Místo toho musíme určit, že každý z těchto řádků se změnila (nepřidáno).

Toho můžete docílit tak, že přidáte novou metodu BLL s názvem `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, jak vidíte níže, nastaví `RowState` jednotlivých `ProductsRow` instance v `ProductsDataTable` k `Modified` a poté předá `ProductsDataTable` do vrstvy DAL s `UpdateWithTransaction` – metoda.


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Souhrn

Poskytuje možnosti úprav integrovaných na řádek prvku GridView, ale schází podpora vytváření plně upravitelné rozhraní. Jak jsme viděli v tomto kurzu, těchto rozhraní je možné, ale vyžadují hodně práce. Vytvoření ovládacího prvku GridView, kde každý řádek je možné upravovat, potřebujeme převést pole ovládacího prvku GridView s vlastností TemplateField a definovat v rámci úprav rozhraní `ItemTemplate` s. Kromě toho aktualizovat vše – tlačítko webové ovládací prvky typu musí přidat na stránku, která je oddělená od prvku GridView. Tato tlačítka `Click` obslužné rutiny události musí se vytvořit výčet GridView s `Rows` shromažďování, ukládání změn v `ProductsDataTable`a předat příslušnou metodu BLL aktualizované informace.

V dalším kurzu uvidíme, jak vytvořit rozhraní pro odstranění služby batch. Konkrétně se bude každý řádek prvku GridView zahrnují zaškrtávací políčko a místo aktualizovat všechny – typ tlačítka, jsme si tlačítka Odstranit vybrané řádky.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Teresy Murphy a David Suru. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](wrapping-database-modifications-within-a-transaction-vb.md)
> [další](batch-deleting-vb.md)
