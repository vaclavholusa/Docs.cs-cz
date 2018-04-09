---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: Přizpůsobení DataList je úpravy rozhraní (VB) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu vytvoříme bohatší úpravy rozhraní pro DataList, ten, který zahrnuje DropDownLists a zaškrtávací políčko.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 7fbdb4687a23604b9f505bbf782d59a8c78e5a02
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="customizing-the-datalists-editing-interface-vb"></a>Přizpůsobení DataList úpravy rozhraní (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) nebo [stáhnout PDF](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> V tomto kurzu vytvoříme bohatší úpravy rozhraní pro DataList, ten, který zahrnuje DropDownLists a zaškrtávací políčko.


## <a name="introduction"></a>Úvod

Značky a ovládací prvky webových v DataList s `EditItemTemplate` definovat jeho upravitelné rozhraní. Ve všech upravovat příklady DataList jsme sunout zkontrolován proto mnohem upravovat rozhraní byla tvořený ovládací prvky webového textové pole. V [předchozí kurzu](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) vylepšili činnost koncového uživatele během úprav přidáním ovládací prvky pro ověřování.

`EditItemTemplate` Můžete dále rozšířit, aby obsahovaly ovládací prvky webového než textovému poli, například DropDownLists, RadioButtonLists, kalendáře a tak dále. Stejně jako u textových polí při přizpůsobení úpravy rozhraní, které chcete zahrnout další ovládací prvky webového využít následující kroky:

1. Přidání do ovládacího prvku webové `EditItemTemplate`.
2. Pomocí syntaxe datové vazby k přiřazení odpovídající pole hodnotu dat pro odpovídající vlastnost.
3. V `UpdateCommand` obslužné rutiny události, prostřednictvím kódu programu přístup k webu řídí hodnota a předejte ji do odpovídající metodu BLL.

V tomto kurzu vytvoříme bohatší úpravy rozhraní pro DataList, ten, který zahrnuje DropDownLists a zaškrtávací políčko. Konkrétně vytvoříme DataList, který obsahuje informace o produktu a umožňuje s název produktu, Dodavatel, kategorie a – starší formáty stav aktualizovat (viz obrázek 1).


[![Úpravy rozhraní obsahuje textové pole, dvě DropDownLists a zaškrtávací políčko](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Obrázek 1**: úpravy rozhraní obsahuje textové pole, dvě DropDownLists a zaškrtávací políčko ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Krok 1: Zobrazení informací o produktu

Můžeme vytvořit rozhraní DataList s upravovat, nejprve musíme sestavení rozhraní jen pro čtení. Začněte otevřením `CustomizedUI.aspx` stránku z `EditDeleteDataList` složky a přidat DataList na stránku nastavení z návrháře, jeho `ID` vlastnost `Products`. Vytvořte nový ObjectDataSource z DataList s inteligentních značek. Název této nové ObjectDataSource `ProductsDataSource` a nakonfigurovat ji k načtení dat z `ProductsBLL` třídu s `GetProducts` metoda. Jako s předchozí upravitelné DataList kurzy, budeme budete aktualizovat informace o upravená produktu s tak, že přejdete přímo na vrstvu obchodní logiky. Podle toho nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný).


[![Nastavte rozevírací seznamy UPDATE, INSERT a DELETE karty na (žádný)](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**Obrázek 2**: nastavení UPDATE, INSERT a odstranit karty rozevírací seznamy (None) ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


Po dokončení konfigurace ObjectDataSource, Visual Studio vytvoří výchozí `ItemTemplate` pro vrácené DataList, který obsahuje název a hodnota pro každé pole data. Upravit `ItemTemplate` tak, aby se uvádí název produktu v šabloně `<h4>` element společně s název kategorie, název dodavatele, ceny a stav – starší formáty. Kromě toho přidat tlačítko pro úpravy, a ověřte, zda jeho `CommandName` je nastavena na Upravit. Deklarativní Moje `ItemTemplate` následuje:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Výše uvedený kód rozložen produktu informací pomocí &lt;h4&gt; záhlaví pro název produktu s a čtyři sloupce `<table>` pro ostatní pole. `ProductPropertyLabel` a `ProductPropertyValue` třídy CSS, definované v `Styles.css`, bylo popsané v předchozí kurzy. Obrázek 3 ukazuje naše průběh při zobrazení prostřednictvím prohlížeče.


[![Zobrazí se název, Dodavatel, kategorie, zastaví stavu a cena každý produkt](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Obrázek 3**: Name, Dodavatel, kategorie, zastaví stavu a cena každý produkt se zobrazí ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Krok 2: Přidání ovládacích prvků webové rozhraní pro úpravy

Prvním krokem při vytváření vlastní DataList úpravy rozhraní je přidání potřebné webové ovládací prvky `EditItemTemplate`. Konkrétně potřebujeme rozevírací seznam pro kategorii, druhý pro dodavatele a zaškrtávací políčko pro – starší formáty stavu. Vzhledem k tomu, že s cena produktu se nedá upravovat v tomto příkladu, abychom mohli pokračovat ji pomocí ovládacího prvku popisek webové zobrazení.

Chcete-li přizpůsobit rozhraní úprav, klikněte na odkaz Upravit šablony z DataList s inteligentním a zvolte `EditItemTemplate` možnost z rozevíracího seznamu. Rozevírací seznam pro přidání `EditItemTemplate` a nastavit jeho `ID` k `Categories`.


[![Přidat rozevírací seznam pro kategorie](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Obrázek 4**: Přidání rozevírací seznam pro kategorií ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


V dalším kroku vyberte možnost zvolit zdroj dat z rozevírací seznam s inteligentních značek a vytvořit nové ObjectDataSource s názvem `CategoriesDataSource`. Konfigurace této ObjectDataSource používat `CategoriesBLL` třídu s `GetCategories()` – metoda (viz obrázek 5). V dalším kroku rozevírací seznam s Průvodce konfigurací zdroje dat vyzve k zadání pole dat, která chcete použít pro každý `ListItem` s `Text` a `Value` vlastnosti. Mít zobrazení rozevírací seznam `CategoryName` pole data a použití `CategoryID` jako hodnotu, jak je znázorněno na obrázku 6.


[![Vytvořit nový ObjectDataSource s názvem CategoriesDataSource](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Obrázek 5**: vytvoření nové ObjectDataSource s názvem `CategoriesDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![Konfigurace zobrazení s rozevírací seznam a hodnota pole](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Obrázek 6**: Konfigurace rozevírací seznam s zobrazení a pole hodnot ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


Opakujte tuto řadu kroky k vytvoření rozevírací seznam pro dodavatelů. Nastavte `ID` pro tento rozevírací seznam pro `Suppliers` a pojmenujte jeho ObjectDataSource `SuppliersDataSource`.

Po přidání dva DropDownLists, přidejte zaškrtávací políčko pro – starší formáty stav a textové pole pro název produktu s. Nastavte `ID` s zaškrtávací políčko a textové pole k `Discontinued` a `ProductName`, v uvedeném pořadí. Přidejte RequiredFieldValidator zajistit, že uživatel poskytuje hodnotu pro název produktu s.

Nakonec přidejte tlačítka aktualizace a zrušit. Mějte na paměti, že pro tyto dvě tlačítka je nutné, jejich `CommandName` vlastnosti jsou nastaveny k aktualizaci a zrušit, v uvedeném pořadí.

Neváhejte rozložení rozhraní úprav, ale chcete. Mohu používat stejné čtyři sloupce vyjádřit výslovný souhlas sunout `<table>` rozložení z rozhraní jen pro čtení jako deklarativní syntaxi a snímek obrazovky ukazuje:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![Úpravy rozhraní je umístěné na jako rozhraní jen pro čtení](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Obrázek 7**: úpravy rozhraní je umístěné na jako rozhraní jen pro čtení ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Krok 3: Vytvoření EditCommand a obslužné rutiny událostí CancelCommand

V současné době není žádné Syntaxe datové vazby v `EditItemTemplate` (s výjimkou `UnitPriceLabel`, který jste zkopírovali přes doslovné z `ItemTemplate`). Přidáme Syntaxe datové vazby na okamžik, ale první umožňují s vytváření obslužných rutin událostí pro DataList s `EditCommand` a `CancelCommand` události. Odvolat, který je zodpovědností `EditCommand` je obslužná rutina události pro vykreslení úpravy rozhraní pro položku DataList bylo stisknuto tlačítko jejichž upravit, zatímco `CancelCommand` s úlohy je vrátit DataList do předem úpravy stavu.

Vytvořit tyto dvě obslužné rutiny a uživatelé použít následující kód:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

Pomocí těchto dvou obslužné rutiny událostí v místě, kliknutím na tlačítko Upravit zobrazí úpravy rozhraní a klepnutím na tlačítko Storno vrátí upravená položku do jeho režimu jen pro čtení. Obrázek 8 ukazuje DataList po kliknutí na tlačítko Upravit pro Chef Anton s Gumbo Mix. Od jsme jste ještě pro přidání jakékoli Syntaxe datové vazby rozhraní úpravy `ProductName` textové pole je prázdné, `Discontinued` nezaškrtnuté políčko, přičemž první položky vybrat z `Categories` a `Suppliers` DropDownLists.


[![Kliknutím na tlačítko zobrazí upravit úpravy rozhraní](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Obrázek 8**: Kliknutím na tlačítko Upravit zobrazuje rozhraní pro úpravy ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Krok 4: Přidání datové vazby syntaxe rozhraní úpravy

Pokud chcete, aby rozhraní úpravy, zobrazit aktuální hodnoty s produktu, musíme syntaxí datové vazby k přiřazení hodnoty polí data na odpovídající hodnoty ovládacího prvku webového. Syntaxe vazby dat se dá použít prostřednictvím návrháře tak, že přejdete na obrazovku Upravit šablony a výběr z webu na odkaz Upravit datové vazby řídí inteligentních značek. Alternativně lze přidat Syntaxe vazby dat přímo do deklarativní.

Přiřadit `ProductName` data pole hodnotu `ProductName` textové pole s `Text` vlastnost, `CategoryID` a `SupplierID` pole hodnoty dat `Categories` a `Suppliers` DropDownLists `SelectedValue` vlastnosti a `Discontinued` data pole hodnotu `Discontinued` zaškrtávacího políčka s `Checked` vlastnost. Po provedení těchto změn, a to buď pomocí návrháře nebo přímo pomocí deklarativní, otevírat stránku prostřednictvím prohlížeče a klikněte na tlačítko Upravit pro Chef Anton s Gumbo Mix. Jak je vidět na obrázku 9, vazby dat syntaxe přidal aktuální hodnoty do textového pole, DropDownLists a zaškrtávací políčko.


[![Kliknutím na tlačítko zobrazí upravit úpravy rozhraní](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Obrázek 9**: Kliknutím na tlačítko Upravit zobrazuje rozhraní pro úpravy ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Krok 5: Uložení změn s uživatele v obslužné rutině události UpdateCommand

Když uživatel upravuje produktu a klikne na tlačítko Aktualizovat, dojde k zpětné volání a DataList s `UpdateCommand` je aktivována událost. Události obslužnou rutinu, musíme čtení hodnoty z ovládací prvky webového v `EditItemTemplate` a rozhraní s BLL aktualizace produktu v databázi. Jako jsme jste viděli v předchozí kurzech `ProductID` aktualizované produktu je přístupná prostřednictvím `DataKeys` kolekce. Uživatel zadal pole přistupuje prostřednictvím kódu programu odkazující na ovládací prvky webového pomocí `FindControl("controlID")`, jak ukazuje následující kód:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

Kód začne konzultace ohledně `Page.IsValid` vlastnost zajistit, že jsou platné všechny ovládací prvky pro ověřování na stránce. Pokud `Page.IsValid` je `True`, upravená produktu s `ProductID` je načíst hodnotu `DataKeys` shromažďování a vkládání dat webových ovládacích prvků do `EditItemTemplate` se odkazuje prostřednictvím kódu programu. Dále jsou do proměnné, které jsou potom předána do příslušné čtení hodnoty z těchto ovládacích prvků webového `UpdateProduct` přetížení. Po aktualizaci dat, je vrácena DataList do stavu předem úpravy.

> [!NOTE]
> I sunout vynechání zpracování logiky přidán do výjimek [zpracování BLL - a výjimky na úrovni DAL](handling-bll-and-dal-level-exceptions-vb.md) zaměřuje kurzu abychom zachovali kód a v tomto příkladu. Jako cvičení přidejte tuto funkci po dokončení tohoto kurzu.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Krok 6: NULL CategoryID zpracování a KódDodavatele hodnoty

Databáze Northwind umožňuje `NULL` hodnoty `Products` tabulky s `CategoryID` a `SupplierID` sloupce. Však aktuálně zohlednit naše úpravy nemá t rozhraní `NULL` hodnoty. Pokud jsme pokus upravit produktu, který má `NULL` hodnotu buď pro jeho `CategoryID` nebo `SupplierID` sloupců, nám budete získat `ArgumentOutOfRangeException` s chybovou zprávou, která je podobná: *"Kategorií" má SelectedValue, který je neplatný, protože provádí nejsou k dispozici v seznamu položek.* Také, že s aktuálně žádný způsob, jak změnit kategorie produktů s nebo dodavatele z hodnoty jinou hodnotu než`NULL` hodnotu `NULL` jeden.

Pro podporu `NULL` hodnoty pro danou kategorii a dodavatele DropDownLists, je potřeba přidat další `ListItem`. I jste se rozhodli použít (žádné), jako `Text` hodnotu pro tento `ListItem`, ale můžete ji změnit na něco jiného (např. prázdný řetězec) Pokud d chcete. Nakonec nezapomeňte nastavit DropDownLists `AppendDataBoundItems` k `True`; Pokud zapomenete Uděláte to tak, kategorie a dodavatelé vázána rozevírací seznam přepíše staticky přidané `ListItem`.

Po provedení těchto změn značku DropDownLists v DataList s `EditItemTemplate` by měl vypadat podobně jako následující:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Statické `ListItem` s mohou být přidány do rozevírací seznam pomocí návrháře nebo přímo pomocí deklarativní syntaxe. Při přidávání rozevírací seznam položky představují databáze `NULL` hodnotu, je nutné přidat `ListItem` využijte deklarativní syntaxi. Pokud použijete `ListItem` Editor kolekce v návrháři, bude vynechejte generovaného deklarativní syntaxi `Value` nastavení zcela při přiřazené prázdné řetězce, vytvoří deklarativní jako: `<asp:ListItem>(None)</asp:ListItem>`. Když to vypadat, že se neškodné, chybějící `Value` způsobí, že rozevírací seznam používat `Text` hodnotu vlastnosti na příslušné místo. To znamená, že pokud to `NULL` `ListItem` je vybrána, hodnota (None) se pokusí přiřazení pole dat produktu (`CategoryID` nebo `SupplierID`, v tomto kurzu), který bude mít za následek výjimku. Explicitně nastavením `Value=""`, `NULL` produktu se přiřadí hodnota pole dat, kdy `NULL` `ListItem` je vybrána.


Chcete-li zobrazit naše průběh prostřednictvím prohlížeče chvíli trvat. Při úpravě produktu, Všimněte si, že `Categories` a `Suppliers` DropDownLists oba mají (None) možnost na začátku rozevírací seznam.


[![Kategorie a dodavatelé DropDownLists zahrnout, (None) možnost](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Obrázek 10**: `Categories` a `Suppliers` DropDownLists zahrnout, (None) možnost ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


Uložte (None) možnost jako databázi `NULL` hodnotu, potřebujeme se vrátíte do `UpdateCommand` obslužné rutiny události. Změna `categoryIDValue` a `supplierIDValue` proměnné, které chcete být celá čísla s možnou hodnotou Null a přiřaďte je jiné než hodnotu `Nothing` pouze v případě rozevírací seznam s `SelectedValue` není prázdný řetězec:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Díky této změně hodnotu `Nothing` se předají do `UpdateProduct` BLL metoda, pokud uživatel vybral (None) možnost pomocí kteréhokoli z rozevíracího seznamu, které odpovídá `NULL` databáze hodnotu.

## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli, jak vytvořit složitější úpravy rozhraní DataList, které zahrnuté tři různé vstupní ovládací prvky webového textové pole, dvě DropDownLists a zaškrtávací políčko společně s ovládací prvky pro ověřování. Při sestavování rozhraní úprav, postup se stejný bez ohledu na ovládací prvky webového používá: Začněte přidáním ovládací prvky webového k DataList s `EditItemTemplate`; přiřadit odpovídající pole hodnoty dat webové služby se odpovídající pomocí syntaxe datové vazby vlastnosti ovládacích prvků; a v `UpdateCommand` obslužné rutiny události, ovládací prvky webového a jejich odpovídající vlastnosti předávání jejich hodnoty do BLL programový přístup.

Při vytváření úpravy rozhraní, jestli ho s tvořený jenom textových polí nebo kolekci různé webové ovládací prvky, ujistěte se, zda jste správně zpracovat databáze `NULL` hodnoty. Při monitorování účtů pro `NULL` s, je nutné, které můžete zobrazit jenom není správně existující `NULL` hodnotu rozhraní úprav, ale také, které nabízejí způsob pro označení hodnotu jako `NULL`. Pro DropDownLists v DataLists, která obvykle znamená Přidání statického `ListItem` jejichž `Value` vlastnost je explicitně nastavit na prázdný řetězec (`Value=""`) a přidáním bit kódu `UpdateCommand` obslužné rutiny události k určení, jestli `NULL``ListItem` byl vybrán.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly společnosti Dennis Patterson David Suru a Randy Schmidt. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
