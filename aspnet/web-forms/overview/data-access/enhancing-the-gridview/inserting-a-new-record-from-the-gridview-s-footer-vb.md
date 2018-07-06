---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: Vložení nového záznamu ze zápatí prvku GridView (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Zatímco ovládacím prvku GridView neposkytuje integrovanou podporu pro vložení nového záznamu dat, v tomto kurzu ukazuje, jak rozšířit GridView zahrnout...
ms.author: aspnetcontent
ms.date: 03/06/2007
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 69aa6659a6c18ed6d16e2916f0f9088ef38a453f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828316"
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>Vložení nového záznamu ze zápatí prvku GridView (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe) nebo [stahovat PDF](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> Zatímco ovládacím prvku GridView neposkytuje integrovanou podporu pro vložení nového záznamu dat, tento kurz ukazuje postupy k posílení GridView zahrnout vkládání rozhraní.


## <a name="introduction"></a>Úvod

Jak je popsáno v [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) výukový program, prvky GridView, DetailsView a FormView webové obsahují předdefinované datové úpravy funkce. Při použití s ovládací prvky zdroje dat deklarativní, tyto tři ovládací prvky webové lze rychle a snadno nastavit pro změny dat – a v situacích, aniž byste museli napsat jediný řádek kódu. Bohužel pouze ovládací prvky prvku DetailsView a FormView poskytují integrovanou vložení, úpravy a odstranění funkce. GridView nabízí úpravy a odstranění podpory. S trochou Pravoúhlá jeho jsme ale rozšířit GridView zahrnout vkládání rozhraní.

Při přidávání možnosti vložení do prvku GridView, máme odpovědnost za rozhodování o tom, jak nové záznamy se přidají, vytváření rozhraní vkládání a psát kód k vložení nového záznamu. V tomto kurzu se podíváme na přidávání rozhraní vkládání do zápatí prvku GridView s řádků (viz obrázek 1). Buňka zápatí pro každý sloupec obsahuje příslušná data uživatele rozhraní prvek kolekce (textové pole pro název produktu s, DropDownList pro dodavatele a tak dále). Potřebujeme sloupec pro sčítání tlačítko, které při kliknutí na vyvolávají zpětné odeslání a vložit nový záznam do `Products` pomocí hodnoty poskytnuté na řádku zápatí tabulky.


[![Řádek zápatí poskytuje rozhraní pro přidání nové produkty](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**Obrázek 1**: řádku zápatí poskytuje rozhraní pro přidání nové produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Krok 1: Zobrazení informací o produktu v GridView

Předtím, než jsme si problém s vytvářením rozhraní vkládání v zápatí prvku GridView s, umožnit s první zaměřit se na přidání GridView na stránku, která zobrazuje seznam produktů, které v databázi. Začněte otevřením `InsertThroughFooter.aspx` stránku `EnhancedGridView` složky a GridView přetáhněte z panelu nástrojů do Návrháře nastavení GridView s `ID` vlastnost `Products`. Pak pomocí inteligentních značek s GridView a vytvořte jeho vazbu nového prvku ObjectDataSource s názvem `ProductsDataSource`.


[![Vytvoření nového prvku ObjectDataSource s názvem ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**Obrázek 2**: vytvoření nového prvku ObjectDataSource s názvem `ProductsDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


Konfigurace ObjectDataSource používat `ProductsBLL` třída s `GetProducts()` metodu pro načtení informací o produktu. Pro účely tohoto kurzu umožňují soustředit s výhradně na přidání vkládání funkcí a bez starostí o úpravy a odstranění. Proto se ujistěte, že rozevíracího seznamu na kartě Vložení je nastavena na `AddProduct()` a že rozevírací seznamy na kartách UPDATE a DELETE jsou nastavena na (žádný).


[![Map – metoda AddProduct metodě Insert() s prvku ObjectDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**Obrázek 3**: mapování `AddProduct` metody prvku ObjectDataSource s `Insert()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![Nastavte rozevírací seznamy aktualizace a odstranění karty na (žádný)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**Obrázek 4**: nastavte aktualizace a odstranění karty rozevírací seznamy na (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


Po dokončení Průvodce nakonfigurovat zdroj dat s prvek ObjectDataSource, Visual Studio automaticky přidá pole do prvku GridView. pro každou z odpovídajících datových polí. Prozatím ponechejte všechna pole, které jsou přidány pomocí sady Visual Studio. Později v tomto kurzu jsme sem vrátíme a odebrat některá pole, jehož hodnoty nejsou t musí být zadána při přidání nového záznamu.

Vzhledem k tomu, že blíží 80 produktů v databázi, uživatel bude muset posuňte se úplně dolů webové stránky, chcete-li přidat nový záznam. Proto umožní s povolit stránkování provést vložení rozhraní viditelné a přístupné. Zapnutí stránkování, stačí zaškrtněte políčko Povolit stránkování v prvku GridView s inteligentním.

V tomto okamžiku ovládacími prvky GridView a prvku ObjectDataSource s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![Všechna datová pole produktu se zobrazí v GridView stránkovaného fondu](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**Obrázek 5**: všechna datová pole produktu se zobrazí v GridView stránkovaného fondu ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>Krok 2: Přidání řádku zápatí

Spolu s jeho záhlaví a řádky dat prvku GridView obsahuje řádek zápatí. Řádky záhlaví a zápatí se zobrazují v závislosti na hodnoty GridView s [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) a [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) vlastnosti. Chcete-li zobrazit zápatí řádek, jednoduše nastavte `ShowFooter` vlastnost `True`. Jak znázorňuje obrázek 6 nastavení `ShowFooter` vlastnost `True` přidá zápatí řádek do mřížky.


[![Zobrazit řádek zápatí, nastavte ShowFooter na hodnotu True](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**Obrázek 6**: nastavení k zobrazení řádku zápatí `ShowFooter` k `True` ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))


Všimněte si, že řádek zápatí má barvu pozadí tmavě červenou. Je to z důvodu motiv DataWebControls jsme vytvořili a použijí pro všechny stránky zpátky [zobrazení dat se prvku ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) kurzu. Konkrétně `GridView.skin` soubor nastaví `FooterStyle` vlastnost, takový, který se používá `FooterStyle` třídu šablony stylů CSS. `FooterStyle` Třída je definována v `Styles.css` následujícím způsobem:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> Jsme ve Prozkoumali jste ji pomocí řádku prvku GridView s zápatí v předchozích kurzech. V případě potřeby odkazovat zpět [zobrazuje souhrnné informace v zápatí prvku GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) kurzu aktualizačního programu.


Po nastavení `ShowFooter` vlastnost `True`, věnujte chvíli výstup zobrazit v prohlížeči. Aktuálně t zápatí řádek kódu obsahovat text ani webové ovládací prvky. V kroku 3 upravíme zápatí pro každé pole ovládacího prvku GridView tak, že obsahují odpovídající vložení rozhraní.


[![Řádek prázdný zápatí se zobrazí nad the stránkovací rozhraní ovládacích prvků](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**Obrázek 7**: zápatí prázdný řádek se zobrazí nad the stránkovací rozhraní ovládacích prvků ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Krok 3: Přizpůsobení řádku zápatí

Zpátky [použití vlastností TemplateField v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) kurzu jsme viděli, jak výrazně přizpůsobit zobrazení konkrétní sloupce GridView použití vlastností TemplateField (na rozdíl od BoundFields nebo CheckBoxFields); v [ Přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) jsme se podívali na použití vlastností TemplateField k přizpůsobení úpravy rozhraní v GridView. Vzpomínáte, že TemplateField tvoří několik šablon, která definuje kombinací kódu, webové ovládací prvky a syntaxe vázání dat použité pro určité typy řádků. `ItemTemplate`, Například určuje šablona použitá pro řádky jen pro čtení, zatímco `EditItemTemplate` definuje šablonu upravit řádek.

Spolu s `ItemTemplate` a `EditItemTemplate`, pole TemplateField zahrnuje také `FooterTemplate` určující obsah pro řádek zápatí. Proto můžeme přidat ovládací prvky webového potřebné pro každé pole s vkládání rozhraní portálu `FooterTemplate`. Pokud chcete začít, převeďte na vlastností TemplateField všechna pole v prvku GridView. To můžete provést klepnutím na odkaz Upravit sloupce v prvku GridView s inteligentní značky, vyberte každé pole v levém dolním rohu a kliknutím na odkaz TemplateField převést toto pole.


![Převést každé pole TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**Obrázek 8**: převést každé pole TemplateField


Kliknutím na převést toto pole na pole TemplateField změní aktuální typ pole do ekvivalentní TemplateField. Například každá vlastnost BoundField nahrazuje TemplateField s `ItemTemplate` , který obsahuje popisek, který se zobrazí odpovídající pole data a `EditItemTemplate` , který zobrazí pole data do textového pole. `ProductName` Vlastnost BoundField byla převedena na následující TemplateField značky:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

Podobně `Discontinued` třídě CheckBoxField byl převeden na pole TemplateField jehož `ItemTemplate` a `EditItemTemplate` obsahovat ovládací prvek zaškrtávací políčko Web (s `ItemTemplate` s zaškrtávací políčko zakázán). Jen pro čtení `ProductID` Vlastnost BoundField byl převeden na pole TemplateField pomocí ovládacího prvku popisku v obou `ItemTemplate` a `EditItemTemplate`. Stručně řečeno převod existující GridView pole na pole TemplateField je rychlý a snadný způsob, jak přepnout na lépe přizpůsobitelnou TemplateField bez ztráty stávajících pole s funkcí.

Od prvku GridView jsme k práci s kódu t podpora úpravy, můžete bez obav odstranit `EditItemTemplate` z každé TemplateField byste museli opustit jenom `ItemTemplate`. Po této, vaše GridView s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

Teď, když každé pole ovládacího prvku GridView byl převeden na pole TemplateField, abychom mohli zadat odpovídající rozhraní vkládání do každého pole s `FooterTemplate`. Některá pole nebude mít vkládání rozhraní (`ProductID`, například); jiné se liší ve webových ovládacích prvcích používá ke shromažďování nové informace o produktu s.

K vytvoření úpravy rozhraní, zvolte odkaz Upravit šablony z inteligentních značek s ovládacího prvku GridView. Potom z rozevíracího seznamu vyberte odpovídající pole s `FooterTemplate` a přetáhněte příslušný ovládací prvek z panelu nástrojů do návrháře.


[![Přidejte příslušné vkládání rozhraní pro každé pole s FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**Obrázek 9**: přidat odpovídající rozhraní vložení do každého pole s `FooterTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


Následující seznam s odrážkami vytváří výčet polí ovládacího prvku GridView, určení vkládání rozhraní pro přidání:

- `ProductID` Žádné.
- `ProductName` přidejte textové pole a nastavte jeho `ID` k `NewProductName`. Přidání ovládacího prvku RequiredFieldValidator také zajistit, aby uživatel zadal hodnotu pro nový název produktu s.
- `SupplierID` Žádné.
- `CategoryID` Žádné.
- `QuantityPerUnit` Přidání textového pole, nastavení jeho `ID` k `NewQuantityPerUnit`.
- `UnitPrice` přidat textové pole s názvem `NewUnitPrice` a CompareValidator, který zajišťuje zadaná hodnota je hodnota měny větší než nebo rovna hodnotě nula.
- `UnitsInStock` použijte textové pole jejichž `ID` je nastavena na `NewUnitsInStock`. Zahrnout CompareValidator, který zajišťuje, že zadaná hodnota je celočíselná hodnota větší než nebo rovna hodnotě nula.
- `UnitsOnOrder` použijte textové pole jejichž `ID` je nastavena na `NewUnitsOnOrder`. Zahrnout CompareValidator, který zajišťuje, že zadaná hodnota je celočíselná hodnota větší než nebo rovna hodnotě nula.
- `ReorderLevel` použijte textové pole jejichž `ID` je nastavena na `NewReorderLevel`. Zahrnout CompareValidator, který zajišťuje, že zadaná hodnota je celočíselná hodnota větší než nebo rovna hodnotě nula.
- `Discontinued` Přidání zaškrtávacího políčka, nastavením jeho `ID` k `NewDiscontinued`.
- `CategoryName` Přidejte DropDownList a nastavte jeho `ID` k `NewCategoryID`. Vázat na nového prvku ObjectDataSource s názvem `CategoriesDataSource` a nakonfigurujte ho na použití `CategoriesBLL` třída s `GetCategories()` metody. Mít DropDownList s `ListItem` s zobrazení `CategoryName` dat pole, pomocí `CategoryID` datové pole jako jejich hodnoty.
- `SupplierName` Přidejte DropDownList a nastavte jeho `ID` k `NewSupplierID`. Vázat na nového prvku ObjectDataSource s názvem `SuppliersDataSource` a nakonfigurujte ho na použití `SuppliersBLL` třída s `GetSuppliers()` metody. Mít DropDownList s `ListItem` s zobrazení `CompanyName` dat pole, pomocí `SupplierID` datové pole jako jejich hodnoty.

Pro každou z validačních ovládacích prvků, smažte `ForeColor` vlastnost tak, aby `FooterStyle` šablon stylů CSS třídy s bílou barvou popředí použije se místo výchozího červené. Použít také `ErrorMessage` podrobný popis, ale nastavenou `Text` vlastnost na hvězdičku. Chcete-li zabránit způsobí vkládání rozhraní zabalit do dvou řádků textu ovládacího prvku s ověření, nastavte `FooterStyle` s `Wrap` vlastnost na hodnotu false pro každou `FooterTemplate` , které ovládací prvek ověření použít. Nakonec přidejte ovládací prvek souhrnu ověření pod GridView a nastavte jeho `ShowMessageBox` vlastnost `True` a jeho `ShowSummary` vlastnost `False`.

Při přidání nového produktu, potřebujeme pro poskytování `CategoryID` a `SupplierID`. Tyto informace jsou zachyceny pomocí DropDownLists v buňkách zápatí pro `CategoryName` a `SupplierName` pole. Můžu se rozhodli používat tato pole, nikoli `CategoryID` a `SupplierID` vlastností TemplateField v mřížce s řádky dat, uživatel je pravděpodobně více zajímá, zobrazovat název kategorie a dodavatele, nikoli jejich hodnoty ID. Protože `CategoryID` a `SupplierID` hodnoty jsou teď zachytí ve `CategoryName` a `SupplierName` pole s vkládáním rozhraní, můžeme odebrat `CategoryID` a `SupplierID` vlastností TemplateField v prvku GridView.

Podobně `ProductID` se nepoužívá při přidání nového produktu, proto `ProductID` TemplateField můžete také odebrat. Ale nechat s ponechte `ProductID` pole v mřížce. Kromě textových polí, DropDownLists, zaškrtávací políčka a validačních ovládacích prvků, které tvoří rozhraní vložení, budeme také potřebovat Add tlačítko, které, po kliknutí na provádí logiku pro přidání nového produktu do databáze. V kroku 4 budete zahrnujeme tlačítko Přidat ve vkládání uživatelského rozhraní `ProductID` TemplateField s `FooterTemplate`.

Můžete zlepšit vzhled různá pole ovládacího prvku GridView. Například můžete chtít formátovat `UnitPrice` hodnoty jako měnu s Zarovnat doprava `UnitsInStock`, `UnitsOnOrder`, a `ReorderLevel` pole a aktualizace `HeaderText` hodnoty vlastností TemplateField.

Po vytvoření slew vložení rozhraní `FooterTemplate` s, odebrání `SupplierID`, a `CategoryID` vlastností TemplateField a vylepšování estetiku stopy prostřednictvím formátování a upravení vlastností TemplateField vaše GridView s deklarativní značka by měla vypadat nějak takto:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

Při prohlížení prostřednictvím prohlížeče, řádek zápatí prvku GridView s teď zahrnuje dokončené vložení rozhraní (viz obrázek 10). Vkládání kódu t rozhraní v tomto okamžiku obsahovat prostředky pro uživatele znamená, že she s zadání dat pro nový produkt a chce vložit nový záznam do databáze. Kromě toho jsme ve ještě k vyřešení, jak se data zadaná v zápatí přeloží do nového záznamu v `Products` databáze. V kroku 4 podíváme na zahrnutí tlačítko Přidat k vložení rozhraní a jak spustit kód na odeslat zpět při jeho s kliknutí. Krok 5 ukazuje, jak vložit nový záznam pomocí dat z zápatí.


[![Zápatí prvku GridView poskytuje rozhraní pro přidání nového záznamu](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**Obrázek 10**: zápatí prvku GridView poskytuje rozhraní pro přidání nového záznamu ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Krok 4: Tlačítko Přidat včetně vkládání rozhraní

Potřebujeme zahrnout tlačítko Přidat někde v rozhraní vkládání od s řádek zápatí vkládání rozhraní aktuálně nemá znamená, že pro uživatele k označení, že nebude dokončena zadáním nové informace o produktu s. To můžete umístit do jednoho ze stávající `FooterTemplate` s nebo můžeme přidat nový sloupec do mřížky pro tento účel. Pro účely tohoto kurzu vám umožňují s umístit v tlačítko Přidat `ProductID` TemplateField s `FooterTemplate`.

Z návrháře, klikněte na odkaz Upravit šablony v prvku GridView s inteligentním a klikněte na tlačítko `ProductID` pole s `FooterTemplate` z rozevíracího seznamu. Přidejte tlačítko webový ovládací prvek (nebo odkazem (LinkButton) nebo ImageButton, pokud dáváte přednost) do šablony, nastavením jeho ID na `AddProduct`, jeho `CommandName` pro vkládání a jeho `Text` vlastnost přidat, jak je znázorněno na obrázku 11.


[![Umístit na tlačítko Přidat šablona FooterTemplate s ProductID TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**Obrázek 11**: umístěte přidat tlačítko v `ProductID` TemplateField s `FooterTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


Jakmile jste již zahrnut na tlačítko Přidat, otestujte si stránku v prohlížeči. Všimněte si, že když kliknete na tlačítko Přidat se neplatná data v rozhraní vložení, zpětné volání je krátké circuited a prvek znamená neplatná data (viz obrázek 12). S odpovídající data zadaná kliknutím na tlačítko Přidat vyvolá zpětné volání. Žádný záznam se přidá do databáze, ale. Budeme muset psát hodně kódu aktuálně provádějí insert.


[![Přidejte tlačítko s zpětné volání je krátký Circuited Pokud rozhraní vložení obsahuje neplatná Data](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**Obrázek 12**: The přidat tlačítko s zpětné volání je krátký Circuited Pokud neplatná Data v rozhraní vložení ([kliknutím ji zobrazíte obrázek v plné velikosti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> Ovládací prvky ověřování v rozhraní vkládání nebyly přiřazeny do ověření skupiny. To funguje správně, pokud je vložení rozhraní pouze sadu validačních ovládacích prvků na stránce. Pokud jsou však další validačních ovládacích prvků na stránce (například validačních ovládacích prvků mřížka s úpravy rozhraní), validačních ovládacích prvků v vkládání rozhraní a přidejte tlačítko s `ValidationGroup` vlastnosti by měla být přiřazena stejná hodnota tak, aby Tyto ovládací prvky přidružte konkrétní skupiny. Zobrazit [rozbor validačních ovládacích prvků v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) Další informace o dělení validačních ovládacích prvků a tlačítka na stránce do ověření skupiny.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Krok 5: Vložení nového záznamu do`Products`tabulky

Při použití integrovaných funkcí pro úpravy prvku GridView, prvku GridView automaticky zpracovává všechny pracovní potřebný k provedení aktualizace. Konkrétně se při kliknutí na tlačítko Aktualizovat zkopíruje hodnoty zadané rozhraní pro úpravy pro parametry v prvku ObjectDataSource s `UpdateParameters` kolekce a zejména kopance vypnout aktualizace vyvoláním ObjectDataSource s `Update()` metody. Protože prvku GridView neposkytuje tyto integrované funkce pro vkládání, musíme implementovat kód, který volá ObjectDataSource s `Insert()` metoda a zkopíruje hodnoty ze vkládání rozhraní do prvku ObjectDataSource s `InsertParameters` kolekce .

Tuto logiku vložení by měla být provedeny po kliknutí na tlačítko Přidat. Jak je popsáno v [přidání a reakce na tlačítka v GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md) kurzu kdykoli tlačítko, odkazem (LinkButton) nebo ImageButton v GridView kliknutí na prvek GridView s `RowCommand` událost je aktivována při zpětném odeslání. Tato událost aktivuje se, zda tlačítko, odkazem (LinkButton) nebo ImageButton byla explicitně přidána jako je například přidat tlačítko v řádku zápatí, nebo pokud byl automaticky přidán v prvku GridView (jako je například LinkButtons v horní části každého sloupce, pokud je vybrána možnost Povolit řazení, nebo LinkButtons v rozhraní stránkování, pokud je vybrána povolit stránkování).

Proto musíme reagovat na kliknutí na tlačítko Přidat uživatele, vytvořit obslužnou rutinu události pro prvek GridView s `RowCommand` událostí. Vzhledem k tomu, že tato událost se aktivuje vždy, když *žádné* je kliknutí na tlačítko, odkazem (LinkButton) nebo ImageButton v prvku GridView, je důležité, že jsme pouze pokračujte vkládání logic-li s `CommandName` vlastnost předán do mapy obslužné rutiny událostí `CommandName` hodnotu na tlačítko Přidat (Vložit). Kromě toho jsme také pokračujte, pouze pokud validačních ovládacích prvků sestav platná data. K tomuto účelu vytvořte obslužnou rutinu události pro `RowCommand` události s následujícím kódem:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> Asi vás zajímá proč obslužná rutina události bothers kontrolu `Page.IsValid` vlastnost. Po všech získaných oproti očekávaným t postback potlačit-li neplatná data v rozhraní vložení? Tento předpoklad je správná, tak dlouho, dokud uživatel nezakázala JavaScript nebo má provést kroky pro obejití logiky ověřování na straně klienta. Stručně řečeno jeden se nikdy spoléhají výhradně na ověřování na straně klienta; na straně serveru kontrolu platnosti vždy je třeba provést před zahájením práce s daty.


V kroku 1 jsme vytvořili `ProductsDataSource` ObjectDataSource tak, aby jeho `Insert()` metoda je namapována na `ProductsBLL` třída s `AddProduct` metoda. K vložení nového záznamu do `Products` tabulky, můžete jednoduše vyvolat ObjectDataSource s `Insert()` metody:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

Teď, když `Insert()` zavolání metody, vše, co už jen zbývá ke kopírování hodnoty z rozhraní vkládání do parametry předány `ProductsBLL` třída s `AddProduct` metody. Jak jsme viděli v [zkoumání události spojené s vložení, aktualizace a odstranění](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) výukový program, můžete to provést prostřednictvím prvku ObjectDataSource s `Inserting` událostí. V `Inserting` budeme potřebovat programově odkazovat na ovládací prvky z událostí `Products` zápatí prvku GridView s řádek a přiřadit jejich hodnot tak `e.InputParameters` kolekce. Pokud uživatel vynechá hodnota jako například opuštění `ReorderLevel` textové pole prázdné, musíme určit, že by měla být hodnota vložena do databáze `NULL`. Vzhledem k tomu, `AddProducts` metoda přijímá typy připouštějící hodnotu Null pro pole v databázi s povolenou hodnotou Null, jednoduše použijte nulovatelný typ a nastavte jej na hodnotu `Nothing` v případě, kdy je vynechán uživatelský vstup.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

S `Inserting` obslužná rutina události dokončení nové záznamy lze přidat do `Products` databázové tabulky prostřednictvím řádku s zápatí prvku GridView. Pokračujte a pokuste se přidat několik nových produktů.

## <a name="enhancing-and-customizing-the-add-operation"></a>Rozšíření a přizpůsobení operace přidání

V současné době kliknutím na tlačítko Přidat přidá nový záznam do tabulky databáze ale neposkytuje jakýkoli druh vizuální zpětnou vazbu, že záznam úspěšně přidala. V ideálním případě by popisek webové ovládací prvek nebo na straně klienta upozornění pole by uživatele informuje, že jejich vložení byla dokončena úspěšně. Můžu ponechte toto cvičení pro čtečku.

Prvku GridView, použitá v tomto kurzu se nevztahují žádné řazení na uvedené produkty ani neumožňuje řadit data koncovému uživateli. Záznamy jsou uspořádány v důsledku toho se nacházejí v databázi tak, že jejich primárního klíče. Vzhledem k tomu, že má každý nový záznam `ProductID` hodnotu větší než poslední z nich, pokaždé, když se přidá nový produkt ho je skládaný konec mřížky. Proto můžete chtít automaticky uživatele poslat na poslední stránky prvku GridView. Po přidání nového záznamu. Toho můžete docílit tak, že přidáte následující řádek kódu po volání `ProductsDataSource.Insert()` v `RowCommand` obslužné rutiny události k označení, že uživatel musí k odeslání na poslední stránku po vazbě dat na prvku GridView:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` je přiřazen proměnné typu Boolean úrovni stránky, které je zpočátku hodnotu `False`. V prvku GridView s `DataBound` obslužná rutina události, pokud `SendUserToLastPage` má hodnotu false, `PageIndex` vlastností se aktualizuje na uživatele poslat na poslední stránce.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

Důvod `PageIndex` je nastavena `DataBound` obslužné rutiny události (nikoli `RowCommand` obslužná rutina události) totiž při `RowCommand` obslužná rutina události je vyvoláno jsme ještě chcete-li přidat nový záznam, který chcete uložit `Products` databázové tabulky. Proto v `RowCommand` obslužná rutina události poslední index stránky (`PageCount - 1`) představuje poslední index stránky *před* nového produktu se přidala. Pro většinu produktů, které přidávají poslední index stránky je stejná po přidání nového produktu. Ale když přidaný produkt výsledkem nového poslední stránka indexu, pokud nesprávně aktualizujeme `PageIndex` v `RowCommand` obslužná rutina události pak jsme přejdou na druhý na poslední stránku (poslední index stránky před přidáním nového produktu) na rozdíl od nové poslední stránky i Ndex. Protože `DataBound` obslužná rutina události aktivuje se po nového produktu se přidala a data znovu připojeno k mřížce, tak, že nastavíte `PageIndex` existuje vlastnost víme jsme opětovné získání správné poslední index stránky.

Prvku GridView, použitá v tomto kurzu je nakonec úplně široký kvůli počet polí, které musí být shromažďovány pro přidání nového produktu. Z důvodu šířku může být DetailsView svislé rozložení s upřednostňované. GridView s celkovou šířku může být snížena shromažďování méně vstupů. Možná jsme zadávat t potřebu shromažďovat `UnitsOnOrder`, `UnitsInStock`, a `ReorderLevel` polí při přidání nového produktu, v takovém případě tato pole nebylo možné odebrat z prvku GridView.

Chcete-li upravit data shromážděná, můžeme použít jednu ze dvou následujících metod:

- Pokračovat v používání `AddProduct` metodu, která očekává, že hodnoty `UnitsOnOrder`, `UnitsInStock`, a `ReorderLevel` pole. V `Inserting` obslužná rutina události, poskytnout pevně zakódované, výchozí hodnoty pro tyto vstupy, které byly odebrány z rozhraní vkládání.
- Vytvořit nové přetížení `AddProduct` metodu `ProductsBLL` třídu, která nepřijímá žádné vstupy pro `UnitsOnOrder`, `UnitsInStock`, a `ReorderLevel` pole. Na stránce technologie ASP.NET nakonfigurujte ObjectDataSource používat toto nové přetížení.

Jednu z možností bude fungovat stejně jako dobře. V minulosti kurzy jsme použili druhou možnost vytváření více přetížení pro `ProductsBLL` třída s `UpdateProduct` metody.

## <a name="summary"></a>Souhrn

Integrované funkce vkládání v prvku DetailsView a FormView chybí prvku GridView, ale u hodně úsilí vkládání rozhraní jde přidat do řádku zápatí. Chcete-li zobrazit zápatí řádek GridView jednoduše nastavte jeho `ShowFooter` vlastnost `True`. Řádek obsahu zápatí může přizpůsobit pro každé pole převádí pole na pole TemplateField a přidání vkládání do rozhraní `FooterTemplate`. Jak jsme viděli v tomto kurzu `FooterTemplate` může obsahovat tlačítka, textová pole, DropDownLists, zaškrtávací políčka, ovládací prvky zdroje dat k naplnění datově řízených webových ovládacích prvcích (například DropDownLists) a validačních ovládacích prvků. Spolu s ovládací prvky pro shromažďování vstupu uživatele s je potřeba přidat tlačítko, odkazem (LinkButton) nebo ImageButton.

Když tlačítko Přidat kliknutí na prvek ObjectDataSource s `Insert()` vyvolána metoda vkládání workflow spusťte. ObjectDataSource bude poté zavolejte metodu nakonfigurované vložit ( `ProductsBLL` třída s `AddProduct` metoda v tomto kurzu). Jsme musíte zkopírovat hodnoty z ovládacího prvku GridView s vkládání rozhraní ObjectDataSource s `InsertParameters` kolekce před volaná metoda vložit. Můžete to udělat prostřednictvím kódu programu odkazující na vkládání ovládacích prvků webového rozhraní v prvku ObjectDataSource s `Inserting` obslužné rutiny události.

V tomto kurzu dokončíte naše pohled na vylepšení vzhledu ovládacího prvku GridView s techniky. Jak pracovat s binární data, jako jsou obrázky, soubory PDF, dokumentů aplikace Word a tak dále a data webové ovládací prvky v souvislosti se další sérii kurzů.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Bernadette Leigh. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](adding-a-gridview-column-of-checkboxes-vb.md)
