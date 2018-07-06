---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
title: Úpravy prvku DataList uživatele úpravy rozhraní (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu vytvoříme podrobnější editační rozhraní pro DataList, takový, který obsahuje DropDownLists a zaškrtávací políčko.
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: a5d13067-ddfb-4c36-8209-0f69fd40e45c
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: c76e3bc46c7d38140320834a27ec7710d289e7d3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826115"
---
<a name="customizing-the-datalists-editing-interface-c"></a>Přizpůsobení rozhraní pro úpravy prvku DataList (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_CS.exe) nebo [stahovat PDF](customizing-the-datalist-s-editing-interface-cs/_static/datatutorial40cs1.pdf)

> V tomto kurzu vytvoříme podrobnější editační rozhraní pro DataList, takový, který obsahuje DropDownLists a zaškrtávací políčko.


## <a name="introduction"></a>Úvod

Značky a ovládacích prvků v ovládacím prvku DataList s `EditItemTemplate` definovat jeho upravitelné rozhraní. Ve všech upravovat příklady DataList jsme ve prověřit, jaký upravovat rozhraní má se skládá z ovládacích prvků textového pole. V [předchozím kurzu](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md) vylepšili jsme během úprav uživatelského prostředí pomocí přidání validačních ovládacích prvků.

`EditItemTemplate` Lze dále rozšířit zahrnout webových ovládacích prvcích než textovém poli, jako je například DropDownLists RadioButtonLists, kalendáře a tak dále. Stejně jako u textových polí, při přizpůsobení rozhraní pro úpravy pro jiné webové ovládací prvky, použít následující kroky:

1. Přidejte ovládací prvek webové `EditItemTemplate`.
2. Pomocí syntaxe vázání dat přiřaďte odpovídající hodnotu pole data na příslušnou vlastnost.
3. V `UpdateCommand` obslužná rutina události, programově přístup k webu řídí hodnota a předejte ho do odpovídající metodu BLL.

V tomto kurzu vytvoříme podrobnější editační rozhraní pro DataList, takový, který obsahuje DropDownLists a zaškrtávací políčko. Zejména, vytvoříme a v prvku DataList, který obsahuje informace o produktu a umožňuje s název produktu, Dodavatel, kategorie a ukončená stav aktualizovat (viz obrázek 1).


[![Úpravy rozhraní obsahuje textové pole, dva DropDownLists a zaškrtávací políčko](customizing-the-datalist-s-editing-interface-cs/_static/image2.png)](customizing-the-datalist-s-editing-interface-cs/_static/image1.png)

**Obrázek 1**: úpravy rozhraní obsahuje textové pole, dva DropDownLists a zaškrtávací políčko ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-cs/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Krok 1: Zobrazení informací o produktu

Než vytvoříme rozhraní DataList s upravovat, musíme nejprve sestavení rozhraní jen pro čtení. Začněte otevřením `CustomizedUI.aspx` stránku ze `EditDeleteDataList` složky a přidat a v prvku DataList na stránku nastavení z návrháře, jeho `ID` vlastnost `Products`. Vytvořte nový prvek ObjectDataSource z inteligentních značek v prvku DataList s. Pojmenujte tento nový prvek ObjectDataSource `ProductsDataSource` a jeho konfigurace pro načtení dat z `ProductsBLL` třída s `GetProducts` metody. Jako předchozí upravitelné DataList kurzy aktualizujeme informace s upravených produktu tak, že přejdete přímo do vrstvy obchodní logiky. Odpovídajícím způsobem nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný).


[![Nastavte rozevírací seznamy UPDATE, INSERT a DELETE karty na (žádný)](customizing-the-datalist-s-editing-interface-cs/_static/image5.png)](customizing-the-datalist-s-editing-interface-cs/_static/image4.png)

**Obrázek 2**: nastavte aktualizace, vložení a odstranění karty rozevírací seznamy na (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-cs/_static/image6.png))


Po dokončení konfigurace ObjectDataSource, Visual Studio vytvoříte výchozí `ItemTemplate` pro vrácené, který se zobrazuje název a hodnotu pro každé pole datového prvku DataList. Upravit `ItemTemplate` tak, aby se uvádí název produktu v šabloně `<h4>` element spolu s názvem kategorie, název dodavatele, ceny a ukončená stav. Kromě toho přidat tlačítko pro úpravy, a ověřte, zda jeho `CommandName` vlastnost nastavena na hodnotu upravit. Deklarativní Moje `ItemTemplate` následující:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Výše uvedené značky rozložen produktu informací pomocí &lt;h4&gt; záhlaví pro produkt s názvem a čtyřmi sloupci `<table>` pro zbývající pole. `ProductPropertyLabel` a `ProductPropertyValue` třídy CSS, definované v `Styles.css`, popsaná v předchozích kurzech. Obrázek 3 zobrazuje náš postup při prohlížení prostřednictvím prohlížeče.


[![Zobrazí se název, Dodavatel, kategorie, vyřazuje, stav a cena každého produktu](customizing-the-datalist-s-editing-interface-cs/_static/image8.png)](customizing-the-datalist-s-editing-interface-cs/_static/image7.png)

**Obrázek 3**: Zobrazí se název, Dodavatel, kategorie, vyřazuje, stav a cena každý produkt ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-cs/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Krok 2: Přidání ovládacích prvků webové rozhraní pro úpravy

Prvním krokem při vytváření přizpůsobených DataList úpravy rozhraní je přidání potřebných webové ovládací prvky `EditItemTemplate`. Konkrétně budeme potřebovat DropDownList kategorie, druhý pro dodavatele a zaškrtávací políčko pro ukončená stavu. Protože cena produktu s se nedá upravovat v tomto příkladu, abychom mohli pokračovat v jeho použití ovládacího prvku popisku webové zobrazení.

Přizpůsobení rozhraní pro úpravy, klikněte na odkaz Upravit šablony v prvku DataList s inteligentním a zvolte `EditItemTemplate` možnost z rozevíracího seznamu. Přidat DropDownList k `EditItemTemplate` a nastavte jeho `ID` k `Categories`.


[![Přidat DropDownList pro kategorie](customizing-the-datalist-s-editing-interface-cs/_static/image11.png)](customizing-the-datalist-s-editing-interface-cs/_static/image10.png)

**Obrázek 4**: Přidání DropDownList pro kategorii ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-cs/_static/image12.png))


V dalším kroku z DropDownList s inteligentní značky, vyberte možnost zvolit zdroj dat a vytvoření nového prvku ObjectDataSource s názvem `CategoriesDataSource`. Konfigurace tohoto prvku ObjectDataSource používat `CategoriesBLL` třída s `GetCategories()` – metoda (viz obrázek 5). V dalším kroku DropDownList s Průvodce konfigurací zdroje dat zobrazí výzvu pro datová pole pro použití u každého `ListItem` s `Text` a `Value` vlastnosti. Zobrazit DropDownList `CategoryName` pole data a použít `CategoryID` jako hodnota, jak je znázorněno na obrázku 6.


[![Vytvoření nového prvku ObjectDataSource s názvem CategoriesDataSource](customizing-the-datalist-s-editing-interface-cs/_static/image14.png)](customizing-the-datalist-s-editing-interface-cs/_static/image13.png)

**Obrázek 5**: vytvoření nového prvku ObjectDataSource s názvem `CategoriesDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-cs/_static/image15.png))


[![Konfigurace zobrazení s DropDownList a hodnota pole](customizing-the-datalist-s-editing-interface-cs/_static/image17.png)](customizing-the-datalist-s-editing-interface-cs/_static/image16.png)

**Obrázek 6**: nakonfigurovat DropDownList s zobrazení a hodnota pole ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-cs/_static/image18.png))


Opakujte tuto sérii kroků k vytvoření DropDownList pro dodavatele. Nastavte `ID` pro tento DropDownList k `Suppliers` a pojmenovat jeho ObjectDataSource `SuppliersDataSource`.

Po přidání dvou DropDownLists, přidejte ovládací prvek CheckBox ukončená stavu a textové pole pro název produktu s. Nastavte `ID` s pro zaškrtávací políčko a textové pole pro `Discontinued` a `ProductName`v uvedeném pořadí. Přidáte RequiredFieldValidator zajistit, že uživatel zadá hodnotu pro název produktu s.

A konečně přidání tlačítka pro aktualizaci a zrušit. Mějte na paměti, že je nezbytné pro tyto dvě tlačítka, které jejich `CommandName` jsou nastavené vlastnosti k aktualizaci a zrušit, v uvedeném pořadí.

Nebojte se, že rozložení rozhraní úprav libovolně. Můžu uložit se rozhodli použít stejné Čtyřsloupcový Dvoucestný `<table>` znázorňuje rozložení z rozhraní jen pro čtení jako deklarativní syntaxi a snímku obrazovky:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample2.aspx)]


[![Rozhraní pro úpravy je podle výstupní jako rozhraní jen pro čtení](customizing-the-datalist-s-editing-interface-cs/_static/image20.png)](customizing-the-datalist-s-editing-interface-cs/_static/image19.png)

**Obrázek 7**: The rozhraní pro úpravy je podle výstupní jako rozhraní jen pro čtení ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-cs/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Krok 3: Vytvoření EditCommand a obslužné rutiny událostí CancelCommand

V současné době neexistuje žádná Syntaxe datové vazby v `EditItemTemplate` (s výjimkou `UnitPriceLabel`, který byl zkopírován přes verbatim z `ItemTemplate`). Okamžité přidáme syntaxe vázání dat, ale s první umožňují vytváření obslužných rutin událostí pro DataList s `EditCommand` a `CancelCommand` události. Vzpomeňte si, že odpovídá `EditCommand` obslužná rutina události je k vykreslení rozhraní úpravy prvku DataList položky došlo ke kliknutí na tlačítko, upravit, zatímco `CancelCommand` s úkolem je vrátit do stavu předem úpravy prvku DataList.

Vytvořit tyto dvě obslužné rutiny a potom kliknul pomocí následujícího kódu:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample3.cs)]

Pomocí těchto dvou ovladačů událostí v místě, kliknutím na tlačítko Upravit zobrazí rozhraní úprav a kliknutím na tlačítko Storno vrátí upravené položky režimu jen pro čtení. Obrázek 8 ukazuje prvku DataList po kliknutí na tlačítko Upravit pro Chef Anton s Gumbo Mix. Protože jsme ve ještě chcete-li přidat všechny datové vazby syntaxe rozhraní úprav `ProductName` textové pole je prázdné, `Discontinued` nezaškrtnuté políčko a první položky vybrané v `Categories` a `Suppliers` DropDownLists.


[![Kliknutím na tlačítko zobrazí úpravy rozhraní úprav](customizing-the-datalist-s-editing-interface-cs/_static/image23.png)](customizing-the-datalist-s-editing-interface-cs/_static/image22.png)

**Obrázek 8**: Kliknutím na tlačítko Upravit zobrazuje rozhraní pro úpravy ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-cs/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Krok 4: Přidání syntaxe vázání dat do rozhraní pro úpravy

Pokud chcete, aby rozhraní úpravy zobrazit aktuální hodnoty s produktu, potřebujeme syntaxí datové vazby k přiřazení hodnoty datového pole na příslušné hodnoty ovládacího prvku webové. Syntaxe vázání dat se dá použít prostřednictvím návrháře tak, že přejdete na obrazovku pro úpravy šablon a vyberte Upravit vlastnosti DataBindings odkaz z webu řídí inteligentních značek. Syntaxe vázání dat, případně lze přidat přímo do deklarativní.

Přiřazení `ProductName` datové pole hodnota, která má `ProductName` textové pole s `Text` vlastnost, `CategoryID` a `SupplierID` data hodnoty do polí `Categories` a `Suppliers` DropDownLists `SelectedValue` vlastnosti a `Discontinued` datové pole hodnota, která má `Discontinued` zaškrtávacího políčka s `Checked` vlastnost. Po provedení těchto změn prostřednictvím návrháře nebo přímo prostřednictvím deklarativní, otevírat stránku prostřednictvím prohlížeče a klikněte na tlačítko Upravit pro Chef Anton s Gumbo Mix. Jak je vidět na obrázku 9, vázání dat syntaxe přidal aktuální hodnoty do textového pole, DropDownLists a zaškrtávací políčko.


[![Kliknutím na tlačítko zobrazí úpravy rozhraní úprav](customizing-the-datalist-s-editing-interface-cs/_static/image26.png)](customizing-the-datalist-s-editing-interface-cs/_static/image25.png)

**Obrázek 9**: Kliknutím na tlačítko Upravit zobrazuje rozhraní pro úpravy ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-cs/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Krok 5: Uložení změn s uživateli v obslužné rutině události UpdateCommand

Když uživatel upraví produktu a klikne na tlačítko Aktualizovat vyvolá zpětné volání a DataList s `UpdateCommand` dojde k aktivaci události. V případě obslužná rutina, budeme potřebovat číst hodnoty ze webové ovládací prvky v `EditItemTemplate` a interface s BLL k aktualizaci produktu v databázi. Jako jsme viděli v předchozích kurzech se ve `ProductID` aktualizované produktu je přístupný prostřednictvím `DataKeys` kolekce. Pole uživatel zadal jsou přístupné prostřednictvím kódu programu odkazováním ovládacích prvků pomocí `FindControl("controlID")`, jak ukazuje následující kód:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample4.cs)]

Kód na začátku společností consulting `Page.IsValid` vlastnost Ujistěte se, že jsou platné všechny validačních ovládacích prvků na stránce. Pokud `Page.IsValid` je `True`, upravený produktu s `ProductID` načíst hodnotu z `DataKeys` kolekce a vstupní data webové ovládací prvky v `EditItemTemplate` je odkazováno prostřednictvím kódu programu. V dalším kroku se čtou hodnoty z těchto ovládacích prvků do proměnné, které jsou potom předány do příslušné `UpdateProduct` přetížení. Po aktualizaci dat, vrátí stavu předem úpravy prvku DataList.

> [!NOTE]
> Můžu odebrat vynecháno zpracování logiky přidá výjimek [zpracování knihoven BLL a DAL úrovni výjimky](handling-bll-and-dal-level-exceptions-cs.md) kurz, aby bylo možné zachovat kódu a v tomto příkladu, zaměřuje. Jako cvičení přidejte tuto funkci po dokončení tohoto kurzu.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Krok 6: Zpracování NULL CategoryID a KódDodavatele hodnoty

Databáze Northwind umožňuje `NULL` hodnoty `Products` tabulky s `CategoryID` a `SupplierID` sloupce. Však aktuálně podle našich úprav kódu t rozhraní `NULL` hodnoty. Pokud jsme pokus upravit produktu, který má `NULL` hodnotu buď pro jeho `CategoryID` nebo `SupplierID` sloupců, založíme `ArgumentOutOfRangeException` zobrazí se chybová zpráva podobná: *"Kategorie" má SelectedValue, který je neplatný, protože ho nemá v seznamu položek neexistuje.* Kromě toho zde s aktuálně žádný způsob, jak změnit kategorii produktů s nebo na dodavatele hodnotu z non -`NULL` hodnota, která se `NULL` jeden.

Pro podporu `NULL` hodnoty pro danou kategorii a dodavatele DropDownLists, je potřeba přidat další `ListItem`. Můžu ve rozhodli používat (žádný) jako `Text` hodnota `ListItem`, ale můžete ho změnit na něco jiného (např. prázdný řetězec) Pokud je d, jako je. Nakonec se nezapomeňte nastavit DropDownLists `AppendDataBoundItems` k `True`; Pokud zapomenete Uděláte to tak, kategorie a dodavatelů, které jsou vázány na DropDownList dojde k přepsání staticky přidaných `ListItem`.

Po provedení těchto změn DropDownLists značek v ovládacích prvcích DataList s `EditItemTemplate` by měl vypadat nějak takto:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> Statické `ListItem` s lze přidat do DropDownList prostřednictvím návrháře nebo přímo prostřednictvím deklarativní syntaxe. Při přidávání položky DropDownList k reprezentaci databáze `NULL` hodnotu, je nutné přidat `ListItem` využijte deklarativní syntaxi. Pokud používáte `ListItem` Editor kolekce v návrháři, vygenerované deklarativní syntaxe vynechá `Value` nastavení úplně při přiřazen prázdný řetězec, deklarativní, jako je vytváření: `<asp:ListItem>(None)</asp:ListItem>`. Když to může vypadat neškodné, chybějící `Value` způsobí, že DropDownList používat `Text` hodnotu vlastnosti na příslušné místo. To znamená, že pokud to `NULL` `ListItem` je vybrána, hodnota (žádný) proběhne pokus o přiřazení pole data produktu (`CategoryID` nebo `SupplierID`, v tomto kurzu), jejímž výsledkem bude výjimky. Explicitním nastavením `Value=""`, `NULL` produktu se přiřadí hodnotu pole data, kdy `NULL` `ListItem` je vybrána.


Chcete-li zobrazit náš postup prostřednictvím prohlížeče chvíli trvat. Při úpravě produktu, Všimněte si, že `Categories` a `Suppliers` DropDownLists obě (žádná) mají možnost na začátku DropDownList.


[![Kategorie a dodavatelů DropDownLists zahrnují (žádná) možnost](customizing-the-datalist-s-editing-interface-cs/_static/image29.png)](customizing-the-datalist-s-editing-interface-cs/_static/image28.png)

**Obrázek 10**: `Categories` a `Suppliers` DropDownLists zahrnout (žádná) možnost ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-datalist-s-editing-interface-cs/_static/image30.png))


Uložit (žádný) možnost jako databáze `NULL` hodnoty, musíme vrátit `UpdateCommand` obslužné rutiny události. Změnit `categoryIDValue` a `supplierIDValue` proměnných celých čísel s možnou hodnotou Null a přiřadit je jiné než hodnoty `Nothing` pouze tehdy, pokud DropDownList s `SelectedValue` není prázdný řetězec:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample6.cs)]

Díky této změně hodnotu `Nothing` předá `UpdateProduct` BLL metodu, pokud uživatel vybral (žádný) možnost pomocí kteréhokoli z rozevíracích seznamů, které odpovídá `NULL` databáze hodnotu.

## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli, jak vytvořit složitější úprav rozhraní DataList, zahrnující tři různé vstupní webové ovládací prvky textové pole, dva DropDownLists a zaškrtávací políčko spolu s validačních ovládacích prvků. Při sestavování rozhraní úprav, postup je stejný bez ohledu na ovládací prvky webového používá: Začněte přidáním webové ovládací prvky na ovládacím prvku DataList s `EditItemTemplate`; přiřadit odpovídající pole hodnoty dat s webovým rozhraním odpovídající syntaxí vázání dat vlastnosti ovládacích prvků; a v `UpdateCommand` obslužná rutina události, programový přístup k webové ovládací prvky a jejich odpovídající vlastnosti předávání do BLL jejich hodnot.

Při vytváření rozhraní úprav, jestli ho s tvořené pouze textová pole nebo kolekce různé webové ovládací prvky, je nutné správně ošetřit databáze `NULL` hodnoty. Při účtování `NULL` s, je nutné, které můžete zobrazit pouze správně existující `NULL` hodnotu úprav rozhraní, ale také, které nabízí prostředky pro označení hodnotu jako `NULL`. Pro DropDownLists v DataLists, která obvykle znamená, že přidáte statickou `ListItem` jehož `Value` je explicitně nastavena na prázdný řetězec (`Value=""`) a přidání verze kódu, který `UpdateCommand` obslužné rutiny události k určení, zda je nebo není `NULL``ListItem` byla vybrána.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Dennis Patterson, David Suru a Randy Schmidt. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
> [další](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
