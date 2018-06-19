---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Dávkové aktualizace (VB) | Microsoft Docs
author: rick-anderson
description: Zjistěte, jak aktualizovat více záznamů databáze v rámci jedné operace. V uživatelské rozhraní vrstvě využijeme GridView, kde je každý řádek upravovat. V datech...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: 2c5119410057b39e7b9a03eca3a2dbdbc315ce00
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888247"
---
<a name="batch-updating-vb"></a>Dávkové aktualizace (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) nebo [stáhnout PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Zjistěte, jak aktualizovat více záznamů databáze v rámci jedné operace. V uživatelské rozhraní vrstvě využijeme GridView, kde je každý řádek upravovat. V Data Access Layer jsme zabalit více operací aktualizace v rámci transakce zajistit, že všechny aktualizace úspěch nebo všechny aktualizace jsou vrácena zpět.


## <a name="introduction"></a>Úvod

V [předchozí kurzu](wrapping-database-modifications-within-a-transaction-vb.md) jsme viděli, jak rozšířit Data Access Layer přidání podpory pro databázové transakce. Databázové transakce zaručit řadu příkazů změny dat, bude považována za jeden atomické operace, která zajišťuje, že selžou všechny změny, nebo všechny bude úspěšné. Pomocí této nízké úrovně DAL funkce stranou jsme re připraveni zapnout naše pozornost vytváření dávky dat úpravy rozhraní.

V tomto kurzu využijeme GridView, kde každý řádek je upravovat (viz obrázek 1). Vzhledem k tomu, že každý řádek je vykreslen v jeho úpravy rozhraní zde s není nutné pro sloupec upravit, aktualizovat a stornovací tlačítka. Místo toho existují dvě aktualizace produktů tlačítka na stránce, která po kliknutí na vytvořit výčet GridView řádky a aktualizaci databáze.


[![Každý řádek v GridView je upravit](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Obrázek 1**: každý řádek v GridView je upravit ([Kliknutím zobrazit obrázek v plné velikosti](batch-updating-vb/_static/image2.png))


Umožňují s začít!

> [!NOTE]
> V [provádění dávková aktualizace](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) kurzu jsme vytvořili batch úpravy rozhraní pomocí ovládacího prvku DataList. V tomto kurzu se liší od předchozí jednu v tom, že se používá GridView a dávková aktualizace se provádí v rámci oboru transakce. Po dokončení tohoto kurzu I doporučujeme, abyste vraťte do starší kurzu a aktualizovat ji používat funkce související s transakcí databáze přidali v předchozím kurzu.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Zkoumání kroky pro vytváření upravovat všechny řádky GridView

Jak je popsáno v [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) kurzu GridView má integrovanou podporu pro úpravy podkladová data na základě na řádek. Interně GridView poznámky, jaké řádek je upravovat pomocí jeho [ `EditIndex` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Jako GridView je vázán na zdroj dat, zkontroluje každý řádek pro zobrazení, je-li index řádku rovná hodnotě `EditIndex`. Pokud ano, tento řádek s, který pole jsou vykreslovány pomocí jejich úpravy rozhraní. Textové pole je rozhraní úpravy BoundFields, jejichž `Text` vlastnost přiřazena hodnota pole dat určeného BoundField s `DataField` vlastnost. Pro TemplateFields `EditItemTemplate` slouží místě `ItemTemplate`.

Odvolat, aby úpravy pracovní postup spustí, když uživatel klikne tlačítko Upravit řádek s. To způsobí, že zpětné volání, nastaví GridView s `EditIndex` vlastnost index kliknutelnou řádku s a znovu připojí data k mřížce. Při stisknutí tlačítka Storno řádek s, na zpětné volání `EditIndex` je nastaven na hodnotu `-1` před obnovení vazby dat k mřížce. Vzhledem k tomu, že řádky s GridView Spustit indexování od nuly, nastavení `EditIndex` k `-1` má za následek zobrazení GridView v režimu jen pro čtení.

`EditIndex` Vlastnost funguje dobře pro úpravy na řádek, ale není určen pro úpravy batch. Chcete-li upravovat celou GridView, potřebujeme mít každý řádek vykreslit pomocí jeho úpravy rozhraní. Nejjednodušší způsob, jak tomu je vytvoření, kde každé upravitelné pole je implementovaný jako TemplateField s jeho úpravy rozhraní definované v `ItemTemplate`.

V dalším několik kroků vytvoříme úplně upravitelné GridView. V kroku 1 jsme budete začněte vytvořením GridView a jeho ObjectDataSource a převést jeho BoundFields a vlastnost CheckBoxField TemplateFields. Kroky 2 a 3 přesunete úpravy rozhraní z TemplateFields `EditItemTemplate` s k jejich `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Krok 1: Zobrazení informací o produktu

Před jsme starat o vytváření GridView kde jsou řádky je možné upravovat, umožní s začít tím, že jednoduše zobrazuje informace o produktu. Otevřete `BatchUpdate.aspx` stránku `BatchData` složku a přetáhněte GridView z panelu nástrojů na návrháře. Nastavit GridView s `ID` k `ProductsGrid` a z jeho inteligentních značek, zvolte pro vazbu k nové ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource načíst data z `ProductsBLL` třídu s `GetProducts` metoda.


[![Konfigurace ObjectDataSource použití třídy ProductsBLL](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Obrázek 2**: Konfigurace ObjectDataSource pro použití `ProductsBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](batch-updating-vb/_static/image4.png))


[![Načtení dat produktu pomocí metody GetProducts](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Obrázek 3**: načtení dat produktu pomocí `GetProducts` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](batch-updating-vb/_static/image6.png))


Jako GridView jsou funkce ObjectDataSource s úpravy navrženy pro práci na základě na řádek. Aby bylo možné aktualizovat sadu záznamů, budeme potřebovat k zápisu ve třídě kódu stránky s ASP.NET, která dávek dat a předává je BLL bit kódu. Proto nastavte rozevírací seznamy v ObjectDataSource s UPDATE, INSERT a DELETE karty na (žádný). Kliknutím na tlačítko Dokončit ukončete průvodce.


[![Nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Obrázek 4**: rozevíracího seznamu jsou uvedeny ve UPDATE, INSERT a odstranit karty na hodnotu (žádná) ([Kliknutím zobrazit obrázek v plné velikosti](batch-updating-vb/_static/image8.png))


Po dokončení průvodce Konfigurace zdroje dat, ObjectDataSource s deklarativní by měl vypadat následovně:


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

Probíhá dokončování Průvodce konfigurace zdroje dat také způsobí, že chcete vytvořit v GridView BoundFields a vlastnost CheckBoxField pro datová pole produktu Visual Studio. V tomto kurzu mohli s pouze povolit uživatelům zobrazení a úprava s název produktu, kategorie, ceny a stav – starší formáty. Odeberte všechny ale na `ProductName`, `CategoryName`, `UnitPrice`, a `Discontinued` pole a přejmenujte `HeaderText` vlastnosti první tři pole na produkt, kategorie a cenu v uvedeném pořadí. Nakonec zkontrolujte zaškrtávací políčka Povolit stránkování a Povolit řazení v GridView s inteligentním.

V tomto okamžiku GridView má tři BoundFields (`ProductName`, `CategoryName`, a `UnitPrice`) a třídy CheckBoxField (`Discontinued`). Je potřeba převést TemplateFields tyto čtyři pole a poté přesuňte úpravy rozhraní z TemplateField s `EditItemTemplate` k jeho `ItemTemplate`.

> [!NOTE]
> Jsme prozkoumali vytváření a přizpůsobení TemplateFields v [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurzu. Projdeme kroky převodu BoundFields a vlastnost CheckBoxField TemplateFields a jejich úpravy definice rozhraní v jejich `ItemTemplate` s, ale pokud získat zablokované nebo potřebovat aktualizační program, tento t váhání odkazovat zpět do tohoto kurzu starší.


Z s GridView inteligentních značek klikněte na odkaz Upravit sloupce otevřete dialogové okno pole. V dalším kroku vyberte každé pole a klikněte na tlačítko Převést toto pole na TemplateField odkaz.


![Převést stávající BoundFields a vlastnost CheckBoxField TemplateFields](batch-updating-vb/_static/image5.gif)

**Obrázek 5**: převést stávající BoundFields a vlastnost CheckBoxField TemplateFields


Teď, když každé pole je TemplateField, jsme re připravení přesunout úpravu rozhraní z `EditItemTemplate` s k `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Krok 2: Vytvoření`ProductName`,`UnitPrice`, a`Discontinued`úpravy rozhraní

Vytváření `ProductName`, `UnitPrice`, a `Discontinued` úpravy rozhraní jsou tématu tohoto kroku a jsou poměrně jednoduché, jako je každé rozhraní již je definována v TemplateField s `EditItemTemplate`. Vytváření `CategoryName` úpravy rozhraní je složitější, protože je potřeba vytvořit rozevírací seznam příslušné kategorie. To `CategoryName` úpravy rozhraní, se zabývá v kroku 3.

Umožní s začínat `ProductName` TemplateField. Klikněte na odkaz Upravit šablony ze inteligentní značky s GridView a přejdete dolů k `ProductName` TemplateField s `EditItemTemplate`. Vyberte textové pole, zkopírujte jej do schránky a vložte jej do `ProductName` TemplateField s `ItemTemplate`. Změna textového pole s `ID` vlastnost `ProductName`.

Dál přidejte RequiredFieldValidator k `ItemTemplate` zajistit, že uživatel nezadá hodnotu pro každý název produktu s. Nastavte `ControlToValidate` Vlastnost ProductName, `ErrorMessage` vlastnost je nutné zadat název produktu. a `Text` vlastnost \*. Po provedení těchto doplňky `ItemTemplate`, obrazovky by měl vypadat podobně jako na obrázku 6.


[![Nyní TemplateField ProductName zahrnuje textové pole a RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Obrázek 6**: `ProductName` TemplateField teď obsahuje textové pole a RequiredFieldValidator ([Kliknutím zobrazit obrázek v plné velikosti](batch-updating-vb/_static/image10.png))


Pro `UnitPrice` úpravy rozhraní, spusťte zkopírováním do textového pole z `EditItemTemplate` k `ItemTemplate`. V dalším kroku umístit $ před do textového pole a nastavte její `ID` vlastnost UnitPrice a jeho `Columns` vlastnost na 8.

Také přidat CompareValidator k `UnitPrice` s `ItemTemplate` zajistit, že uživatelem zadaná hodnota je platná měna hodnotu větší než nebo rovna hodnotě 0,00 Kč. Nastavit s validátoru `ControlToValidate` vlastnost UnitPrice, jeho `ErrorMessage` vlastnost je třeba zadat hodnotu platný měny. Prosím vynechejte žádné měna symboly., jeho `Text` vlastnost \*, jeho `Type` vlastnost `Currency`, jeho `Operator` vlastnost `GreaterThanEqual`a jeho `ValueToCompare` vlastnost na hodnotu 0.


[![Přidat CompareValidator zajistit zadali cena je nezáporná hodnota měny](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Obrázek 7**: přidejte CompareValidator zajistit zadali cena se nezáporná hodnota měny ([Kliknutím zobrazit obrázek v plné velikosti](batch-updating-vb/_static/image12.png))


Pro `Discontinued` TemplateField můžete použít políčko již definována v `ItemTemplate`. Jednoduše nastavit jeho `ID` pro nákup ukončen a jeho `Enabled` vlastnost `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Krok 3: Vytvoření`CategoryName`úpravy rozhraní

Úpravy rozhraní v `CategoryName` TemplateField s `EditItemTemplate` obsahuje textové pole, které se zobrazí hodnota `CategoryName` datové pole. Je potřeba nahradit rozevírací seznam, který obsahuje seznam možných kategorií.

> [!NOTE]
> [Přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurz obsahuje důkladnější a dokončení diskusi k přizpůsobení šablonu, která má obsahovat rozevírací seznam a textové pole. Při dokončení kroků v tomto poli se se mají zobrazovat tersely. Podrobnější pohled na vytvoření a konfigurace kategorie rozevírací seznam, najdete v části zpět [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurzu.


Přetáhněte rozevírací seznam z panelu nástrojů na `CategoryName` TemplateField s `ItemTemplate`, nastavení jeho `ID` k `Categories`. V tomto okamžiku by obvykle definujeme zdroj dat s DropDownLists pomocí inteligentních značek, vytváření nových ObjectDataSource. To však přidá ObjectDataSource v rámci `ItemTemplate`, který bude mít za následek ObjectDataSource instance vytvořené pro každý řádek GridView. Místo toho umožní vytvořit ObjectDataSource mimo GridView s TemplateFields s. Ukončení úprav šablony a přetáhněte ji ObjectDataSource z panelu nástrojů na návrháře pod `ProductsDataSource` ObjectDataSource. Název nové ObjectDataSource `CategoriesDataSource` a nakonfigurujte ho na používání `CategoriesBLL` třídu s `GetCategories` metoda.


[![Konfigurace ObjectDataSource použití třídy CategoriesBLL](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Obrázek 8**: Konfigurace ObjectDataSource pro použití `CategoriesBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](batch-updating-vb/_static/image14.png))


[![Načtení dat kategorie metodou GetCategories](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Obrázek 9**: načtení kategorie dat pomocí `GetCategories` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](batch-updating-vb/_static/image16.png))


Vzhledem k tomu, že tento ObjectDataSource slouží jenom k načtení dat, nastavte rozevírací seznamy v karty aktualizace a odstranění na (žádný). Kliknutím na tlačítko Dokončit ukončete průvodce.


[![Sada rozevíracích seznamů v aktualizaci a odstranění karty na (žádný)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Obrázek 10**: rozevíracího seznamu jsou uvedeny v aktualizaci a odstranění karty na hodnotu (žádná) ([Kliknutím zobrazit obrázek v plné velikosti](batch-updating-vb/_static/image18.png))


Po dokončení průvodce, `CategoriesDataSource` s deklarativní by měl vypadat třeba takto:


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

Pomocí `CategoriesDataSource` vytvořený a nakonfigurovaný, vraťte se do `CategoryName` TemplateField s `ItemTemplate` a z rozevírací seznam s inteligentní značky klikněte na odkaz zvolit zdroj dat. V průvodci Konfigurace zdroje dat, vyberte `CategoriesDataSource` možnost v prvním rozevíracím seznamu a zvolte tak, aby měl `CategoryName` používá pro zobrazení a `CategoryID` jako hodnotu.


[![Vytvořit vazbu rozevírací seznam CategoriesDataSource](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Obrázek 11**: rozevírací seznam pro vytvoření vazby `CategoriesDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](batch-updating-vb/_static/image20.png))


V tomto okamžiku `Categories` rozevírací seznam uvádí všechny kategorie, ale jeho není ještě automaticky vyberte odpovídající kategorii produktu vázaný na GridView řádek. K tomu je potřeba nastavit `Categories` rozevírací seznam s `SelectedValue` na produkt s `CategoryID` hodnotu. Klikněte na odkaz Upravit datové vazby z rozevírací seznam s inteligentním a přidružit `SelectedValue` vlastnost s `CategoryID` pole dat, jak ukazuje obrázek 12.


![Vazby pro vlastnost SelectedValue rozevírací seznam s hodnotě v poli produktu s](batch-updating-vb/_static/image12.gif)

**Obrázek 12**: vytvoření vazby produktu s `CategoryID` hodnotu na rozevírací seznam s `SelectedValue` vlastnost


Jeden poslední problém zůstane: Pokud máte t nemá produktu `CategoryID` zadána hodnota poté příkaz datové vazby v `SelectedValue` bude mít za následek výjimku. Důvodem je, že rozevírací seznam obsahuje pouze položky pro kategorie a nenabízí možnost pro tyto produkty, které mají `NULL` databáze hodnotu `CategoryID`. Chcete-li to opravit, nastavte rozevírací seznam s `AppendDataBoundItems` vlastnost `True` a přidat novou položku do rozevírací seznam, vynechání `Value` vlastnost z deklarativní syntaxi. To znamená, ujistěte se, že `Categories` rozevírací seznam s deklarativní syntaxe vypadá takto:


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Poznámka: Jak `<asp:ListItem Value="">` – vyberte jednu – má jeho `Value` atribut explicitně nastavit na prázdný řetězec. Odkazovat zpět [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurz podrobnější diskuzi na Proč je potřeba tato další položka rozevírací seznam pro zpracování `NULL` případu a proč přiřazení `Value` je nezbytné vlastnosti na prázdný řetězec.

> [!NOTE]
> Potenciální výkon a škálovatelnost problém tady, je důležité zmínit, není k dispozici. Vzhledem k tomu, že každý řádek obsahuje rozevírací seznam, který používá `CategoriesDataSource` jako svůj zdroj dat `CategoriesBLL` třídu s `GetCategories` bude volána metoda *n* navštěvovat za stránky, kde *n* je počet řádky v GridView. Tyto *n* volání `GetCategories` za následek *n* dotazy do databáze. Tento dopad na databázi může dojít ke snížení za pomocí ukládání do mezipaměti vrácený kategorií v mezipaměti na požadavek nebo prostřednictvím vrstvě ukládání do mezipaměti pomocí ukládání do mezipaměti závislosti nebo velmi založené na krátkou dobu vypršení platnosti SQL. Další informace o za požadavek ukládání do mezipaměti možnost, najdete v části [ `HttpContext.Items` mezipaměť za požadavku](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Krok 4: Dokončení úprav rozhraní

Jsme jste provedli několik změny šablon s GridView bez pozastavení zobrazíte naše průběh. Chcete-li zobrazit naše průběh prostřednictvím prohlížeče chvíli trvat. Jak ukazuje obrázek 13, každý řádek je vykreslen pomocí jeho `ItemTemplate`, který obsahuje s buňky úpravy rozhraní.


[![Každý řádek GridView je upravit](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Obrázek 13**: každý řádek GridView je upravit ([Kliknutím zobrazit obrázek v plné velikosti](batch-updating-vb/_static/image22.png))


Existuje několik menší problémy formátování, které by měl staráme o v tomto okamžiku. První, Všimněte si, že `UnitPrice` hodnota obsahuje čtyři desetinných míst. Chcete-li odstranit tento problém, vraťte se k `UnitPrice` TemplateField s `ItemTemplate` a z textové pole s inteligentní značky klikněte na odkaz Upravit datové vazby. Dále určit, že `Text` vlastnost musí být formátována jako číslo.


![Formátuje vlastnost Text jako číslo](batch-updating-vb/_static/image14.gif)

**Obrázek 14**: formát `Text` vlastnost jako číslo


Druhý, umožňují s center políčko ve `Discontinued` sloupce (místo nutnosti ho zarovnaný doleva). Klikněte na Upravit sloupce z GridView s inteligentním a vyberte `Discontinued` TemplateField ze seznamu polí v levém dolním rohu. K podrobnostem `ItemStyle` a nastavte `HorizontalAlign` vlastnost Center, jak ukazuje obrázek 15.


![Center – starší formáty zaškrtávací políčko](batch-updating-vb/_static/image15.gif)

**Obrázek 15**: Center `Discontinued` zaškrtávací políčko


V dalším kroku přidání ValidationSummary ovládacího prvku na stránku a nastavit jeho `ShowMessageBox` vlastnost `True` a jeho `ShowSummary` vlastnost `False`. Také přidat že tlačítko webové ovládací prvky, při kliknutí na, dojde k aktualizaci s změny uživatelů. Konkrétně přidat dva ovládací prvky webového tlačítko, nad GridView a jeden pod něj nastavení obou ovládacích prvků `Text` vlastnosti, které chcete aktualizace produktů.

Protože rutina GridView s úpravy rozhraní je definována v jeho TemplateFields `ItemTemplate` s, `EditItemTemplate` s jsou nadbytečné a může dojít k odstranění.

Po provedení výše uvedených změny formátování, ovládací prvky tlačítek přidání a odebrání nadbytečné `EditItemTemplate` s, deklarativní syntaxi stránky s by měl vypadat asi takto:


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

Obrázek 16 zobrazuje tato stránka při zobrazení prostřednictvím prohlížeče, po přidání ovládací prvky webového tlačítko a formátování změny.


[![Stránka teď obsahuje dvě tlačítka aktualizace produktů](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Obrázek 16**: stránky teď obsahuje dvě aktualizace produktů tlačítka ([Kliknutím zobrazit obrázek v plné velikosti](batch-updating-vb/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Krok 5: Aktualizace produktů

Pokud uživatel navštíví této stránce se budou proveďte jejich úpravy a pak klikněte na jednu pro dvě tlačítka aktualizace produktů. V tomto bodě musíme nějakým způsobem uložit hodnot zadaných uživatelem pro každý řádek do `ProductsDataTable` instance a předat který BLL metodu, která se pak předejte, `ProductsDataTable` instance DAL s `UpdateWithTransaction` metoda. `UpdateWithTransaction` Metodu, která jsme vytvořili v [předchozí kurzu](wrapping-database-modifications-within-a-transaction-vb.md), zajišťuje dávku změny budou aktualizaci jako atomickou operaci.

Vytvořit metodu s názvem `BatchUpdate` v `BatchUpdate.aspx.vb` a přidejte následující kód:


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Tato metoda začíná získáním na všechny produkty zpět v `ProductsDataTable` prostřednictvím volání BLL s `GetProducts` metoda. Pak zobrazí `ProductGrid` GridView s [ `Rows` kolekce](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). `Rows` Kolekce obsahuje [ `GridViewRow` instance](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) pro každý řádek v GridView zobrazí. Vzhledem k tomu, že jsme zobrazují maximálně deset řádků na každé stránce, rutina GridView s `Rows` kolekce bude obsahovat více než deset položky.

Pro každý řádek `ProductID` bude převzat z `DataKeys` kolekce a odpovídající `ProductsRow` je vybraný `ProductsDataTable`. Čtyři vstupní ovládací prvky TemplateField se odkazuje prostřednictvím kódu programu a jejich hodnoty přiřazené `ProductsRow` s vlastnosti instance. Po každé GridView hodnoty řádků s použily aktualizace `ProductsDataTable`, ho s předaný BLL s `UpdateWithTransaction` metodu, která jako jsme viděli v tomto kurzu předchozí jednoduše volá do DAL s `UpdateWithTransaction` metoda.

Algoritmus aktualizace batch používá pro účely tohoto kurzu aktualizuje každý řádek `ProductsDataTable` odpovídající řádek v GridView, bez ohledu na to, jestli se změnil informace o produktu s. Při takové blind aktualizací t nejsou obvykle problémy výkonem, mohou vést nadbytečné záznamy li níž auditování změn do databázové tabulky. Zpět v [provádění dávková aktualizace](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) kurzu jsme prozkoumali dávce aktualizuje rozhraní s prvku DataList a přidat kód, který by aktualizovat pouze tyto záznamy, které byly upraveny ve skutečnosti uživatelem. Nebojte se použít techniky z [provádění dávková aktualizace](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) v případě potřeby aktualizujte kód v tomto kurzu.

> [!NOTE]
> Při vytváření vazby zdroje dat pro GridView prostřednictvím jeho inteligentních značek, Visual Studio automaticky přiřadí datový zdroj s primární klíče hodnoty GridView s `DataKeyNames` vlastnost. Pokud jste není navázán ObjectDataSource na GridView prostřednictvím GridView s inteligentním jak je uvedeno v kroku 1, pak budete muset ručně nastavit GridView s `DataKeyNames` vlastnost ProductID, aby měli přístup `ProductID` hodnotu pro každý řádek prostřednictvím `DataKeys` kolekce.


Kód použitý v `BatchUpdate` je podobná používán BLL s `UpdateProduct` metody, hlavní rozdíl je, že v `UpdateProduct` metody jediným `ProductRow` instance se načítají architekturu. Kód, který přiřazuje vlastnosti `ProductRow` je stejný mezi `UpdateProducts` metody a kódu v rámci `For Each` smyčky v `BatchUpdate`, jako je celkový vzor.

K dokončení tohoto kurzu, je potřeba mít `BatchUpdate` metoda volána, když po kliknutí na některý z tlačítka aktualizace produktů. Vytváření obslužných rutin událostí pro `Click` události těchto dvou tlačítko – ovládací prvky a přidejte následující kód v obslužné rutiny událostí:


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

Nejprve je volána k `BatchUpdate`. Dále [ `ClientScript` vlastnost](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) slouží k vložení JavaScript, který se zobrazí messagebox, který čte produkty byly aktualizovány.

Trvat několik minut k otestování tohoto kódu. Navštivte `BatchUpdate.aspx` prostřednictvím prohlížeče, upravit počet řádků a klikněte na jednu z tlačítka aktualizace produktů. Za předpokladu, že nejsou žádné chyby ověření vstupu, měli byste vidět messagebox, který čte, že byly aktualizovány produkty. Chcete-li ověřit nedělitelnost aktualizace, zvažte přidání náhodnou `CHECK` omezení, jako je ten, který zakáže `UnitPrice` hodnoty 1234.56. Potom z `BatchUpdate.aspx`, upravit počet záznamů, a zkontrolujte, zda nastavte jednu z produktu s `UnitPrice` hodnotu zakázané hodnotou (1234.56). Výsledkem by mělo chybu při kliknutí na aktualizace produktů s další změny během této operace batch vrátit zpět na původní hodnoty.

## <a name="an-alternativebatchupdatemethod"></a>Alternativu`BatchUpdate`– metoda

`BatchUpdate` Metoda jsme právě zkontrolován načte *všechny* produkty z BLL s `GetProducts` metoda a pak aktualizuje pouze tyto záznamy, které se zobrazují v GridView. Tento přístup je ideální, pokud GridView nepoužívá stránkování, ale pokud ano, může existovat stovky, tisíc nebo desetitisíce produkty, ale maximálně deset řádků v prvku GridView. V takovém případě je menší než vhodné získávání všechny produkty z databáze pouze k úpravě 10 z nich.

Pro tyto typy situacích zvažte použití následujících `BatchUpdateAlternate` metoda místo:


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` začne tím, že vytvoříte nový prázdný `ProductsDataTable` s názvem `products`. Následně kroky prostřednictvím GridView s `Rows` kolekce a pro každý řádek získá informace o konkrétním produktu pomocí BLL s `GetProductByProductID(productID)` metoda. Načtený `ProductsRow` instance má jeho vlastnosti aktualizovat stejným způsobem jako `BatchUpdate`, ale po aktualizaci řádku je importovat do `products` `ProductsDataTable` prostřednictvím DataTable s [ `ImportRow(DataRow)` metoda](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Po `For Each` dokončení smyčky `products` obsahuje jeden `ProductsRow` instance pro každý řádek v GridView. Od těchto `ProductsRow` instance jsou přidané do `products` (místo aktualizaci), pokud jsme slepě předejte ji do `UpdateWithTransaction` metoda `ProductsTableAdatper` se pokusí každý záznam vložit do databáze. Místo toho je potřeba zadat, že všechny tyto řádky se změnila (není přidáno).

Můžete to provést přidáním nové metody do BLL s názvem `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, vidíte níže, nastaví `RowState` jednotlivých `ProductsRow` instancí v `ProductsDataTable` k `Modified` a pak předá `ProductsDataTable` k DAL s `UpdateWithTransaction` metoda.


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Souhrn

GridView poskytuje možnosti úprav předdefinované na řádek, ale chybí podpora pro vytváření plně upravitelné rozhraní. Jak jsme viděli v tomto kurzu, taková rozhraní je možné, ale vyžadují bit práce. Pokud chcete vytvořit GridView, kde je každý řádek upravovat, je potřeba převést pole s GridView TemplateFields a definovat úpravy rozhraní v rámci `ItemTemplate` s. Kromě toho aktualizovat všechny – tlačítko webové ovládací prvky typu musí být přidaný do stránce odděleně od GridView. Tato tlačítka `Click` obslužné rutiny událostí muset výčet GridView s `Rows` kolekce, uložit změny v `ProductsDataTable`a předat do odpovídající metodu BLL aktualizované informace.

V dalším kurzu vidíte vytvoření rozhraní pro dávkové odstranění. Konkrétně, bude každý řádek GridView zahrnout zaškrtávací políčko a místo aktualizovat všechny – typ tlačítka, budeme mít tlačítka Odstranit vybrané řádky.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Teresy Murphy a David Suru. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](wrapping-database-modifications-within-a-transaction-vb.md)
> [další](batch-deleting-vb.md)
