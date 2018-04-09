---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: Vkládání nového záznamu z prvku GridView zápatí (VB) | Microsoft Docs
author: rick-anderson
description: Při prvek GridView neposkytuje integrovanou podporu pro vložení nového záznamu dat, tento kurz ukazuje, jak k posílení GridView zahrnout...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 32f3cb23805813135bf463720e7479f5f819deb7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>Vkládání nového záznamu z prvku GridView zápatí (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe) nebo [stáhnout PDF](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> Při prvek GridView neposkytuje integrovanou podporu pro vložení nového záznamu dat, tento kurz ukazuje, jak k posílení GridView zahrnout vkládání rozhraní.


## <a name="introduction"></a>Úvod

Jak je popsáno v [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) kurz, GridView DetailsView a FormView webové ovládací prvky každý zahrnují funkce integrované dat úpravy. Při použití s ovládacími prvky zdroje deklarativní data, tyto tři ovládací prvky webového lze rychle a snadno nastavit upravit data – a ve scénářích bez nutnosti napsat jediný řádek kódu. Bohužel pouze ovládací prvky DetailsView a FormView poskytovat předdefinované vkládání, úpravy a odstraňování možnosti. GridView pouze nabízí úpravy a odstraňování podpory. S malým množstvím Pravoúhlá jeho, můžeme však posílení GridView zahrnout vkládání rozhraní.

V přidání vkládání funkcí do GridView, jsou zodpovědní za rozhodování o tom, jak nové záznamy se přidá, vytváření rozhraní vkládání a psaní kódu vložit nový záznam. V tomto kurzu se podíváme na přidávání rozhraní vkládání do zápatí s GridView řádek (viz obrázek 1). Zápatí buňky pro každý sloupec obsahuje příslušná data kolekce element uživatelského rozhraní (textové pole pro název produktu s, rozevírací seznam pro dodavatele a tak dále). Potřebujeme sloupec pro Add tlačítko, při kliknutí na způsobit zpětné volání a vložit nový záznam do `Products` pomocí hodnoty zadané v řádku zápatí tabulky.


[![Řádek zápatí poskytuje rozhraní pro přidání nové produkty](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**Obrázek 1**: řádku zápatí poskytuje rozhraní pro přidání nové produkty ([Kliknutím zobrazit obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Krok 1: Zobrazení informací o produktu v GridView

Před jsme týkají označována s vytvářením rozhraní vkládání v zápatí s GridView, umožní s první zaměřit na přidávání GridView na stránku, který obsahuje seznam produktů v databázi. Začněte otevřením `InsertThroughFooter.aspx` stránky v `EnhancedGridView` složku a přetáhněte GridView z panelu nástrojů na návrháře nastavení GridView s `ID` vlastnost `Products`. Pak pomocí inteligentních značek s GridView pro vazbu k nové ObjectDataSource s názvem `ProductsDataSource`.


[![Vytvořit nový ObjectDataSource s názvem ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**Obrázek 2**: vytvoření nové ObjectDataSource s názvem `ProductsDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


Konfigurace ObjectDataSource používat `ProductsBLL` třídu s `GetProducts()` metodu za účelem načtení informací o produktu. V tomto kurzu umožňují zaměřit s výhradně na přidání vkládání funkcí a nestarat se o úpravy a odstranění. Proto se ujistěte, že rozevíracím seznamu na kartě Vložení je nastavena na `AddProduct()` a jestli rozevírací seznamy na kartách aktualizace a odstranění jsou nastavené na (žádný).


[![Map – metoda AddProduct metodu Insert() s ObjectDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**Obrázek 3**: mapy `AddProduct` metodu ObjectDataSource s `Insert()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![Nastavte rozevírací seznamy aktualizace a odstranění karty na (žádný)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**Obrázek 4**: nastavte aktualizace a odstranění karty rozevírací seznamy (None) ([Kliknutím zobrazit obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


Po dokončení průvodce Konfigurace zdroje dat s ObjectDataSource, Visual Studio automaticky přidá pole do GridView pro každé pole odpovídající data. Teď ponechejte všechna pole přidat Visual Studio. Později v tomto kurzu jsme budete vraťte a odebrat některá pole, jejichž hodnoty nejsou zobrazeny t muset zadat při přidávání nového záznamu.

Vzhledem k tomu, že je blízko 80 produktů v databázi, uživatel bude muset posuňte se úplně dolů webové stránky, chcete-li přidat nový záznam. Proto umožní s povolit stránkování, aby rozhraní vkládání víc viditelné a přístupné. Chcete-li na stránkování, stačí zaškrtněte políčko Povolit stránkování z GridView s inteligentním.

Rutina GridView a ObjectDataSource s deklarativní v tomto okamžiku by měl vypadat podobně jako následující:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![Všechna datová pole produktu se zobrazují v GridView stránkovaného fondu](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**Obrázek 5**: všechna Data pole produktu se zobrazují v GridView stránkovaného fondu ([Kliknutím zobrazit obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>Krok 2: Přidání řádek zápatí

Spolu s jeho záhlaví a řádky dat zahrnuje GridView řádek zápatí stránky. Řádky záhlaví a zápatí se zobrazují v závislosti na hodnoty GridView s [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) a [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) vlastnosti. Chcete-li zobrazit zápatí řádek, jednoduše nastavte `ShowFooter` vlastnost `True`. Jak ukazuje obrázek 6, nastavení `ShowFooter` vlastnost `True` přidá řádek zápatí k mřížce.


[![Chcete-li zobrazit zápatí řádek, nastavte ShowFooter na hodnotu True](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**Obrázek 6**: K zobrazení řádku zápatí nastavit `ShowFooter` k `True` ([Kliknutím zobrazit obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))


Všimněte si, že má řádek zápatí barvu pozadí tmavý red. Je to z důvodu DataWebControls motiv jsme vytvoří a použijí na všechny stránky zpět v [zobrazení dat pomocí ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) kurzu. Konkrétně `GridView.skin` nakonfiguruje souboru `FooterStyle` vlastnost, takový, který se používá `FooterStyle` třídu CSS. `FooterStyle` Třída definovaná v `Styles.css` následujícím způsobem:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> Jsme jste prozkoumali v předchozí kurzy řádku GridView s zápatí. V případě potřeby odkazovat zpět [informace souhrn zobrazení v prvku GridView zápatí](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) kurzu aktualizačního programu.


Po nastavení `ShowFooter` vlastnost `True`, zobrazte výstup v prohlížeči za chvíli. Aktuálně t zápatí řádek nemá obsahovat jakýkoli text nebo ovládací prvky webového. V kroku 3 jsme budete zápatí pro každé pole GridView upravit tak, že obsahují rozhraní odpovídající vkládání.


[![Zápatí prázdný řádek je zobrazen výše stránkování rozhraní ovládací prvky](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**Obrázek 7**: zápatí prázdný řádek je zobrazen výše stránkování rozhraní ovládacích prvků ([Kliknutím zobrazit obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Krok 3: Přizpůsobení řádku zápatí

Zpět v [pomocí TemplateFields v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) kurzu jsme viděli výrazně přizpůsobení zobrazení konkrétní sloupec GridView pomocí TemplateFields (na rozdíl od BoundFields nebo CheckBoxFields); v [ Přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) jsme se podívali na použití TemplateFields k přizpůsobení úpravy rozhraní v GridView. Odvolání, TemplateField se skládá z řady šablon definující směs značky, ovládací prvky webového a syntaxe datové vazby používá pro určité typy řádků. `ItemTemplate`, Například k šabloně použité pro řádky jen pro čtení, určuje při `EditItemTemplate` definuje šablonu pro upravovatelné řádek.

Spolu s `ItemTemplate` a `EditItemTemplate`, TemplateField také zahrnuje `FooterTemplate` určující obsah pro řádek zápatí. Proto jsme můžete přidat ovládací prvky webového potřebné pro každé pole s vkládání rozhraní do `FooterTemplate`. Pokud chcete spustit, převeďte na TemplateFields všechna pole v GridView. To můžete provést kliknutím na odkaz Upravit sloupce v GridView s inteligentní značky, výběrem každé pole v levém dolním rohu a kliknutím na převést toto pole na TemplateField odkaz.


![Převést každé pole TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**Obrázek 8**: převést každé pole TemplateField


Toto pole do TemplateField kliknete převést změní aktuální typ pole do ekvivalentní TemplateField. Například každý BoundField nahrazuje TemplateField s `ItemTemplate` obsahující popisek, který zobrazuje odpovídající pole data a `EditItemTemplate` který zobrazí pole dat do textového pole. `ProductName` BoundField byl převeden do následující kód TemplateField:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

Podobně `Discontinued` Vlastnost CheckBoxField byl převeden do TemplateField jehož `ItemTemplate` a `EditItemTemplate` obsahují ovládací prvek zaškrtávací políčko Web (s `ItemTemplate` s zaškrtávací políčko Zakázat). Jen pro čtení `ProductID` BoundField byl převeden do TemplateField pomocí ovládacího prvku popisek v obou `ItemTemplate` a `EditItemTemplate`. Stručně řečeno převod existující GridView pole do TemplateField je rychlý a snadný způsob, jak přepnout na další přizpůsobitelné TemplateField bez ztráty stávajících pole s funkcí.

Od GridView jsme re práce nemá t podpora úpravy můžete bez obav odstranit `EditItemTemplate` z každé TemplateField ponechat jenom `ItemTemplate`. Po této, vaše GridView s deklarativní by měl vypadat následovně:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

Teď, když každé pole GridView byl převeden do TemplateField, jsme zadejte odpovídající rozhraní vkládání do každého pole s `FooterTemplate`. Některá pole nebude mít vkládání rozhraní (`ProductID`, například); ostatní se bude lišit ve webových ovládacích prvcích používá ke shromažďování nové informace o produktu s.

Vytvoření rozhraní úprav, vyberte GridView s inteligentním na odkaz Upravit šablony. Pak v rozevíracím seznamu vyberte odpovídající pole s `FooterTemplate` a přetáhněte ji vhodný ovládací prvek z panelu nástrojů na návrháře.


[![Přidejte příslušné vkládání rozhraní, každý s FooterTemplate pole](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**Obrázek 9**: přidejte příslušné vkládání rozhraní s každou pole `FooterTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


V následujícím seznamu zobrazí pole GridView zadání vkládání rozhraní, které chcete přidat:

- `ProductID` Žádné.
- `ProductName` přidat textové pole a nastavit jeho `ID` k `NewProductName`. Přidání ovládacího prvku RequiredFieldValidator také zajistit, aby uživatel zadal hodnotu pro název nového produktu s.
- `SupplierID` Žádné.
- `CategoryID` Žádné.
- `QuantityPerUnit` přidat textové pole, nastavení jeho `ID` k `NewQuantityPerUnit`.
- `UnitPrice` přidat textové pole s názvem `NewUnitPrice` a CompareValidator, které zajišťuje, aby zadaná hodnota je currency hodnota větší než nebo rovna hodnotě nula.
- `UnitsInStock` použijte textové pole jehož `ID` je nastaven na `NewUnitsInStock`. Zahrnout CompareValidator, která zajišťuje, že zadaná hodnota je celé číslo větší než nebo rovna hodnotě nula.
- `UnitsOnOrder` použijte textové pole jehož `ID` je nastaven na `NewUnitsOnOrder`. Zahrnout CompareValidator, která zajišťuje, že zadaná hodnota je celé číslo větší než nebo rovna hodnotě nula.
- `ReorderLevel` použijte textové pole jehož `ID` je nastaven na `NewReorderLevel`. Zahrnout CompareValidator, která zajišťuje, že zadaná hodnota je celé číslo větší než nebo rovna hodnotě nula.
- `Discontinued` Přidat zaškrtávací políčko, nastavení jeho `ID` k `NewDiscontinued`.
- `CategoryName` Přidejte rozevírací seznam a nastavte její `ID` k `NewCategoryID`. Navázat jej na nové ObjectDataSource s názvem `CategoriesDataSource` a nakonfigurujte ho na používání `CategoriesBLL` třídu s `GetCategories()` metoda. Mít rozevírací seznam s `ListItem` s zobrazení `CategoryName` data pole, pomocí `CategoryID` datové pole jako jejich hodnoty.
- `SupplierName` Přidejte rozevírací seznam a nastavte její `ID` k `NewSupplierID`. Navázat jej na nové ObjectDataSource s názvem `SuppliersDataSource` a nakonfigurujte ho na používání `SuppliersBLL` třídu s `GetSuppliers()` metoda. Mít rozevírací seznam s `ListItem` s zobrazení `CompanyName` data pole, pomocí `SupplierID` datové pole jako jejich hodnoty.

Pro každou ovládacích prvků ověřování, vymažte `ForeColor` vlastnost tak, aby `FooterStyle` šablon stylů CSS třídu s bílým popředí barva se použije místo výchozího red. Také použít `ErrorMessage` podrobný popis, ale hodnota nastavena `Text` vlastnost hvězdičku. Abyste zabránili text pro ovládací prvek s ověření působit rozhraní vkládání zabalit do dvou řádků, nastavte `FooterStyle` s `Wrap` vlastnost na hodnotu false pro jednotlivé `FooterTemplate` s, použijte ovládací prvek ověření. Nakonec přidejte ovládacího prvku ValidationSummary pod GridView a sadu jeho `ShowMessageBox` vlastnost `True` a jeho `ShowSummary` vlastnost `False`.

Při přidání nového produktu, je potřeba zadat `CategoryID` a `SupplierID`. Tyto informace pořízen prostřednictvím DropDownLists v zápatí buňky `CategoryName` a `SupplierName` pole. Se rozhodli používat tato pole naproti tomu `CategoryID` a `SupplierID` TemplateFields, protože v mřížce s řádky dat uživatele je více pravděpodobně nepotřebujete vidět názvy kategorií a dodavatele, nikoli jejich hodnoty ID. Vzhledem k tomu `CategoryID` a `SupplierID` hodnoty jsou nyní zaznamenávané v `CategoryName` a `SupplierName` pole s vkládáním rozhraní, jsme odebrat `CategoryID` a `SupplierID` TemplateFields z GridView.

Podobně `ProductID` se nepoužívá při přidání nového produktu, proto `ProductID` TemplateField lze také odebrat. Ale nechat s ponechte `ProductID` pole v mřížce. Kromě textových polí, DropDownLists, zaškrtávací políčka a ovládací prvky ověřování, která tvoří rozhraní vkládání, je třeba také Add tlačítko, která po kliknutí na provede logiku pro přidání nového produktu do databáze. V kroku 4 budete tlačítko Přidat v rozhraní vkládání v zahrnuta `ProductID` TemplateField s `FooterTemplate`.

Nebojte se, že vylepšení vzhledu různá pole GridView. Například můžete chtít formátu `UnitPrice` hodnoty jako měny, vpravo align `UnitsInStock`, `UnitsOnOrder`, a `ReorderLevel` pole a aktualizace `HeaderText` hodnoty TemplateFields.

Po vytvoření slew vložení rozhraní `FooterTemplate` s, odebrání `SupplierID`, a `CategoryID` TemplateFields a vylepšuje estetiku mřížky prostřednictvím formátování a zarovnání TemplateFields, vaše GridView s deklarativní Kód by měl vypadat takto:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

Při zobrazení prostřednictvím prohlížeče, řádku GridView s zápatí nyní zahrnuje dokončené vkládání rozhraní (viz obrázek 10). V tomto okamžiku vkládání nemá t rozhraní zahrnují znamená pro uživatele znamená, že s she zadaná data pro nového produktu a chce vložit nový záznam do databáze. Také jsme jste ještě chcete vyřešit, jak budou data zadaná do zápatí převede na nový záznam v `Products` databáze. V kroku 4 podíváme postup zahrnují tlačítko Přidat rozhraní vkládání a ke spouštění kódu na odeslat zpět kdy ji s klikli. Krok 5 ukazuje, jak chcete-li vložit nový záznam pomocí dat z zápatí.


[![Rutina GridView zápatí poskytuje rozhraní pro přidávání nového záznamu](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**Obrázek 10**: zápatí GridView poskytuje rozhraní pro přidávání nového záznamu ([Kliknutím zobrazit obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Krok 4: Včetně tlačítko Přidat v vkládání rozhraní

Musíme zahrnout tlačítko Přidat někde v rozhraní vkládání vzhledem k tomu, že s řádek zápatí stránky, který je aktuálně vkládání rozhraní chybí znamená pro uživatele k označení, že nebude dokončena zadáním nové informace o produktu s. To může umístěné v jednom z existujících `FooterTemplate` s nebo jsme může přidat nový sloupec do mřížky pro tento účel. V tomto kurzu umožňují s umístit na tlačítko Přidat v `ProductID` TemplateField s `FooterTemplate`.

Z návrháře, klikněte na odkaz Upravit šablony v GridView s inteligentním a potom vyberte `ProductID` pole s `FooterTemplate` z rozevíracího seznamu. Přidání ovládacího prvku tlačítko webové (nebo LinkButton nebo ImageButton, pokud dáváte přednost) do šablony, nastavení jeho ID na `AddProduct`, jeho `CommandName` k vložení a jeho `Text` vlastnost přidat, jak ukazuje obrázek 11.


[![Umístit na tlačítko Přidat FooterTemplate s ProductID TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**Obrázek 11**: umístit na tlačítko Přidat v `ProductID` TemplateField s `FooterTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


Jakmile jste již zahrnut na tlačítko Přidat, otestování stránku v prohlížeči. Všimněte si, že při kliknutí na tlačítko Přidat se neplatná data v vkládání rozhraní zpětné volání je prostě circuited a ovládacího prvku ValidationSummary označuje neplatná data (viz obrázek 12). U příslušné zadaná data klepnutím na tlačítko Přidat způsobí zpětné volání. Žádný záznam se přidá do databáze, ale. Je potřeba zapsat malou část kódu, aby mohly skutečně provést úlohy insert.


[![Přidat tlačítko s zpětné volání je krátký Circuited Pokud je v rozhraní vkládání neplatná Data.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**Obrázek 12**: přidat tlačítko s zpětné volání je krátký Circuited případě neplatná Data v rozhraní vložení ([Kliknutím zobrazit obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> Ovládací prvky ověřování v rozhraní vkládání nebyly přiřazeny do skupiny ověření. Tento postup funguje bez problémů, tak dlouho, dokud vkládání rozhraní je pouze sadu ověřovacích ovládacích prvků na stránce. Pokud jsou však další ověření ovládací prvky na stránce (například ověření ovládací prvky v mřížce s úpravy rozhraní), ověření ovládacích prvků v vkládání rozhraní a přidejte tlačítko s `ValidationGroup` vlastnosti by se měla přiřadit stejnou hodnotu tak, aby Tyto ovládací prvky přidružte konkrétní ověřovací skupiny. V tématu [Rozvěrače ověřovacích ovládacích prvků v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) Další informace o vytváření oddílů tlačítka na stránku a ovládací prvky pro ověřování do skupin ověření.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Krok 5: Vkládání nových záznamů do`Products`tabulky

Při použití funkce integrované úprav GridView, GridView automaticky zpracovává všechny pracovní potřebné k provedení aktualizace. Zejména při kliknutí na tlačítko Aktualizovat kopíruje hodnoty zadané z rozhraní úprav na parametry ObjectDataSource s `UpdateParameters` kolekce a zejména kopance vypnout aktualizace vyvoláním ObjectDataSource s `Update()` metoda. Vzhledem k tomu, že GridView neposkytuje takové integrovanou funkci pro vložení, musíme implementovat kód, který volá ObjectDataSource s `Insert()` metoda a zkopíruje hodnoty ze vkládání rozhraní k ObjectDataSource s `InsertParameters` kolekce .

Tato logika insert se má provést po klepnutí na tlačítko Přidat. Jak je popsáno v [přidávání a reagovat na tlačítka v GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md) kurzu, kdykoliv tlačítko, LinkButton nebo ImageButton v GridView po kliknutí na GridView s `RowCommand` aktivuje událost na zpětné volání. Tato událost se aktivuje, zda tlačítko, LinkButton nebo ImageButton byla explicitně přidat například na tlačítko Přidat v řádku zápatí nebo pokud byl automaticky přidat pomocí GridView (například LinkButtons v horní části každého sloupce, pokud je vybraná možnost Povolit řazení, nebo LinkButtons v rozhraní stránkování, pokud je vybrána povolit stránkování).

Proto reakce na kliknutí na tlačítko Přidat uživatele, je potřeba vytvořit obslužnou rutinu události pro GridView s `RowCommand` událostí. Vzhledem k tomu, že tato událost se aktivuje vždy, když *všechny* po kliknutí na tlačítko, LinkButton nebo ImageButton v GridView, je důležité, že jsme pouze pokračovat s vkládáním logiku by s `CommandName` vlastnost předán do mapy obslužné rutiny událostí `CommandName` hodnotu na tlačítko Přidat (Vložit). Kromě toho jsme také pokračujte, pouze pokud ovládací prvky pro ověřování sestav platná data. K tomuto požadavku vyhovělo, vytvoření obslužné rutiny událostí `RowCommand` událostí s následujícím kódem:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> Asi vás zajímá, proč obslužné rutiny události bothers kontrola `Page.IsValid` vlastnost. Po všech získaných t zpětné volání potlačit, pokud je zadaná neplatná data v vkládání rozhraní? Tento předpoklad je správná, tak dlouho, dokud uživatel nebyl zakázán JavaScript nebo má provedených kroků k obcházení logiku ověřování na straně klienta. Stručně řečeno jeden se nikdy spoléhají výhradně na ověření na straně klienta; serverové kontrolu platnosti vždy je třeba provést před zahájením práce s daty.


V kroku 1 jsme vytvořili `ProductsDataSource` ObjectDataSource tak, aby jeho `Insert()` metoda je namapována na `ProductsBLL` třídu s `AddProduct` metoda. Chcete-li vložit nový záznam do `Products` tabulky, můžeme jednoduše vyvolat ObjectDataSource s `Insert()` metoda:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

Teď, když `Insert()` byla volána metoda, všechny, který zbývá ke kopírování hodnoty z rozhraní vkládání k parametrům předaný `ProductsBLL` třídu s `AddProduct` metoda. Jak jsme viděli zpět v [zkoumání události spojené s vložení, aktualizace a odstranění](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) kurz, můžete to provést prostřednictvím ObjectDataSource s `Inserting` událostí. V `Inserting` událostí musíme programově odkazovat ovládacích prvků z `Products` zápatí s GridView řádek a jejich hodnoty pro přiřazení `e.InputParameters` kolekce. Pokud uživatel vynechá hodnotu třeba ponechat `ReorderLevel` textové pole prázdné, je potřeba určit, aby hodnota vložená do databáze `NULL`. Vzhledem k tomu `AddProducts` metoda přijímá typy s možnou hodnotou Null pro pole v databázi s povolenou hodnotou Null, jednoduše používat typ s možnou hodnotou Null a jeho hodnotu nastavte `Nothing` v případě, kdy je vynechán vstup uživatele.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

S `Inserting` obslužné rutiny události dokončit nové záznamy lze přidat do `Products` databázové tabulky prostřednictvím řádku GridView s zápatí. Pokračujte a zkuste přidat několik nových produktů.

## <a name="enhancing-and-customizing-the-add-operation"></a>Rozšíření a přizpůsobení operace přidání

V současné době klepnutím na tlačítko Přidat přidá do tabulky databáze nový záznam ale neposkytuje žádné řazení vizuální zpětnou vazbu záznam úspěšně přidán. V ideálním případě by popisek webové ovládací prvek nebo klienta výstrahy pole informujte uživatele, že jejich vložení má byl úspěšně dokončen. Nechat to jako cvičení pro čtečku.

Rutina GridView použili v tomto kurzu se nevztahuje žádné řazení uvedené produkty tomu koncového uživatele k řazení dat. ani. V důsledku toho jsou záznamy seřazené jako v databázi tak, že jejich primárního klíče. Vzhledem k tomu, že každý nový záznam má `ProductID` hodnotu vyšší než poslední, při každém přidání nového produktu ho je standardní konci mřížky. Proto můžete automaticky odesílat uživatele na poslední stránku GridView po přidání nového záznamu. Můžete to provést přidáním následující kód po volání `ProductsDataSource.Insert()` v `RowCommand` obslužné rutiny události indikující, že uživatel musí k odeslání na poslední stránku po vazbě dat na GridView:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` je logická hodnota proměnné úrovně stránky, která je původně přiřazena hodnota `False`. V GridView s `DataBound` obslužné rutiny události, pokud `SendUserToLastPage` je nastavena hodnota false, `PageIndex` vlastnost se aktualizuje na uživateli odeslat na poslední stránce.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

Z důvodu `PageIndex` je nastavena `DataBound` obslužné rutiny události (Naproti tomu `RowCommand` obslužné rutiny události) je, protože při `RowCommand` jsme obslužnou rutinu události aktivuje jste ještě pro přidání nového záznamu `Products` databázové tabulky. Proto v `RowCommand` obslužné rutiny události poslední index stránky (`PageCount - 1`) představuje poslední index stránky *před* byl přidán nový produkt. Pro většinu produkty, které přidávají poslední index stránky je stejná po přidání nového produktu. Ale když přidané produktu výsledkem nový poslední index stránky, pokud nesprávně aktualizujeme `PageIndex` v `RowCommand` obslužné rutiny události pak jsme budete přesměrováni na druhý na poslední stránce (poslední index stránky před přidáním nového produktu) oproti nové poslední stránky i Ndex. Vzhledem k tomu `DataBound` obslužné rutiny události aktivuje se po byl přidán nový produkt a data odrážejí do mřížky, a to nastavením `PageIndex` existuje vlastnost víme jsme re získávání správné poslední index stránky.

Nakonec GridView použili v tomto kurzu je velmi široký kvůli počet polí, které je třeba shromažďovat pro přidání nového produktu. Z důvodu této šířku může být svislém rozložení s DetailsView upřednostňované. Rutina GridView s může být snížit celkovou šířku shromažďování méně vstupy. Možná jsme nejsou zobrazeny potřeba t shromažďovat `UnitsOnOrder`, `UnitsInStock`, a `ReorderLevel` polí při přidání nového produktu, v takovém případě tato pole nebylo možné odebrat z GridView.

Chcete-li upravit data shromážděná, můžeme použít jednu ze dvou následujících metod:

- Nadále používat `AddProduct` metoda, která očekává hodnoty `UnitsOnOrder`, `UnitsInStock`, a `ReorderLevel` pole. V `Inserting` obslužné rutiny události, zadejte pevně, výchozí hodnoty pro tyto vstupních hodnot, které byly odebrány z rozhraní vkládání.
- Vytvoření nové přetížení `AddProduct` metoda v `ProductsBLL` třídu, která nepřijímá vstupy pro `UnitsOnOrder`, `UnitsInStock`, a `ReorderLevel` pole. Potom nakonfigurujte na stránce ASP.NET ObjectDataSource používat tento nový přetížení.

Jednu z možností bude fungovat stejně také. V minulosti kurzy jsme použili druhou možnost, vytváření více přetížení pro `ProductsBLL` třídu s `UpdateProduct` metoda.

## <a name="summary"></a>Souhrn

Integrované funkce vkládání nalezených v DetailsView a FormView chybí GridView, ale s trochu úsilí vkládání rozhraní lze přidat na řádek zápatí stránky. Zobrazit zápatí řádek v GridView jednoduše nastavte její `ShowFooter` vlastnost `True`. Obsah zápatí řádku lze přizpůsobit pro každé pole převedením pole na TemplateField a přidání vkládání rozhraní na `FooterTemplate`. Jak jsme viděli v tomto kurzu `FooterTemplate` může obsahovat tlačítka, textových polí, DropDownLists, zaškrtávací políčka, ovládací prvky zdroje dat pro naplnění datové ovládací prvky webového (například DropDownLists) a ovládací prvky pro ověřování. Společně s ovládací prvky pro vstup uživatele s shromažďování je požadován přidání tlačítka, LinkButton nebo ImageButton.

Když přidat po kliknutí na tlačítko, ObjectDataSource s `Insert()` metoda je volána vkládání workflow spusťte. ObjectDataSource bude potom volejte metodu nakonfigurované insert ( `ProductsBLL` třídu s `AddProduct` metody v tomto kurzu). Jsme musí zkopírujte hodnoty z s GridView vkládání rozhraní ObjectDataSource s `InsertParameters` kolekce před ke kterému volaná metoda insert. Můžete to provést prostřednictvím kódu programu odkazem vkládání ovládacích prvků webové rozhraní v ObjectDataSource s `Inserting` obslužné rutiny události.

V tomto kurzu dokončení naše pohled na techniky rozšíření vzhled s GridView. Jak pracovat s binární data, jako jsou bitové kopie, souborů PDF, dokumentů aplikace Word a tak dále a data ovládací prvky webového bude vyhledejte sadu další kurzy.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Bernadette Leigh. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](adding-a-gridview-column-of-checkboxes-vb.md)
