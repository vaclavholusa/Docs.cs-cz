---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
title: "Vlastní tlačítka v DataList a opakovače (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu využijeme rozhraní, které používá prvku Repeater seznam kategorií v systému, s každou kategorii poskytování tlačítko zobrazíte jeho associ..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1afdb14d-6e49-4e1f-aead-2934730d472e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: fc6c297f08790cdcc74867df21e32258017c5a7d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="custom-buttons-in-the-datalist-and-repeater-vb"></a>Vlastní tlačítka v DataList a opakovače (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_VB.exe) nebo [stáhnout PDF](custom-buttons-in-the-datalist-and-repeater-vb/_static/datatutorial46vb1.pdf)

> V tomto kurzu využijeme rozhraní, které používá k zobrazení seznamu kategorií v systému, s každou kategorii poskytování tlačítko zobrazíte její přidružené produkty pomocí ovládacího prvku BulletedList prvku Repeater.


## <a name="introduction"></a>Úvod

V průběhu posledních sedmnáct DataList a opakovače kurzy jsme vytvořili jste příklady jen pro čtení a úpravy i odstranění příklady. Pro usnadnění úpravy a odstraňování možnosti v rámci DataList, jsme přidali tlačítka pro DataList s `ItemTemplate` která po kliknutí na způsobeno zpětné volání a vyvolá událost související s DataList odpovídající tlačítko s `CommandName` vlastnost. Například přidání tlačítka do `ItemTemplate` s `CommandName` vlastnost hodnota úpravy způsobí, že DataList s `EditCommand` má provést při zpětném odeslání; s `CommandName` odstranit vyvolá `DeleteCommand`.

Kromě toho upravovat a odstraňovat tlačítka, ovládací prvky DataList a opakovače může také zahrnovat tlačítka, LinkButtons nebo ImageButtons která po kliknutí na provádět některé vlastní logiky na straně serveru. V tomto kurzu využijeme rozhraní, které používá prvku Repeater seznam kategorií v systému. Pro každou kategorii opakovače bude obsahovat tlačítko zobrazíte kategorie produktů spojené s pomocí ovládacího prvku BulletedList (viz obrázek 1).


[![Kliknutím na tlačítko Zobrazit produkty odkaz zobrazí kategorie s produkty v seznamu s odrážkami](custom-buttons-in-the-datalist-and-repeater-vb/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image1.png)

**Obrázek 1**: Kliknutím na tlačítko zobrazí zobrazit odkaz produkty kategorie s produkty v seznamu s odrážkami ([Kliknutím zobrazit obrázek v plné velikosti](custom-buttons-in-the-datalist-and-repeater-vb/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Krok 1: Přidání vlastního tlačítka kurz webové stránky

Předtím, než se podíváme na tom, jak přidat vlastní tlačítka, umožní s nejprve pozorně vytváření stránek ASP.NET v našem webu projekt, který budeme potřebovat pro účely tohoto kurzu. Nejprve přidejte novou složku s názvem `CustomButtonsDataListRepeater`. Dál přidejte následující dvě stránky ASP.NET do této složky, a zkontrolujte, zda přidružit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `CustomButtons.aspx`


![Přidání stránky ASP.NET pro vlastní kurzy související tlačítka](custom-buttons-in-the-datalist-and-repeater-vb/_static/image4.png)

**Obrázek 2**: Přidání stránky ASP.NET pro vlastní kurzy související tlačítka


V jiných složkách, jako `Default.aspx` v `CustomButtonsDataListRepeater` složky zobrazí seznam kurzů k v jeho části. Odvolat, který `SectionLevelTutorialListing.ascx` tuto funkci zajišťuje uživatelský ovládací prvek. Přidat tento uživatelský ovládací prvek `Default.aspx` přetažením z Průzkumníka řešení na stránku s zobrazení návrhu.


[![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](custom-buttons-in-the-datalist-and-repeater-vb/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image5.png)

**Obrázek 3**: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](custom-buttons-in-the-datalist-and-repeater-vb/_static/image7.png))


Nakonec přidejte stránky jako položky na `Web.sitemap` souboru. Konkrétně, přidejte následující kód po stránkování a řazení s DataList a opakovače `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, pozorně Zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro úpravy, vkládání a odstraňování kurzy.


![Mapy webu nyní obsahuje položku pro tento kurz vlastních tlačítek](custom-buttons-in-the-datalist-and-repeater-vb/_static/image8.png)

**Obrázek 4**: mapy webu nyní obsahuje položku pro tento kurz vlastních tlačítek


## <a name="step-2-adding-the-list-of-categories"></a>Krok 2: Přidání seznamu kategorií

V tomto kurzu budeme muset vytvořit opakovače, který obsahuje seznam všech kategorií společně s produkty LinkButton zobrazit, po kliknutí na, zobrazí kategorie související s produkty v seznamu s odrážkami. Umožní s nejprve vytvořit jednoduché opakovače, který obsahuje seznam kategorií v systému. Začněte otevřením `CustomButtons.aspx` stránku `CustomButtonsDataListRepeater` složky. Přetáhněte prvku Repeater z panelu nástrojů na Designer a sadu jeho `ID` vlastnost `Categories`. Dále vytvořte nový ovládací prvek zdroje dat z opakovače s inteligentním. Konkrétně vytvoření nového ovládacího prvku ObjectDataSource s názvem `CategoriesDataSource` , vybere data z `CategoriesBLL` třídu s `GetCategories()` metoda.


[![Konfigurace ObjectDataSource lze pomocí této metody GetCategories() s CategoriesBLL – třída](custom-buttons-in-the-datalist-and-repeater-vb/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image9.png)

**Obrázek 5**: Konfigurace ObjectDataSource pro použití `CategoriesBLL` třídu s `GetCategories()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](custom-buttons-in-the-datalist-and-repeater-vb/_static/image11.png))


Na rozdíl od řízení DataList, pro který Visual Studio vytvoří výchozí `ItemTemplate` založené na zdroji dat, šablony opakovače s musí být ručně definován. Kromě toho musí být opakovače s šablony vytvořit a upravit deklarativně (to znamená, že s žádné šablony upravit možnost opakovače s inteligentním).

Klikněte na kartě Zdroj v levém dolním rohu a přidejte `ItemTemplate` který zobrazí název kategorie s v `<h3>` elementu a jeho popis v odstavec značka; patří `SeparatorTemplate` který zobrazí vodorovné pravítko (`<hr />`) mezi jednotlivými kategorie. Také přidat LinkButton s jeho `Text` vlastnost nastavena na hodnotu zobrazit produkty. Po dokončení těchto kroků, vaše stránka s deklarativní by měl vypadat následovně:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample2.aspx)]

Obrázek 6 zobrazuje stránku při zobrazení prostřednictvím prohlížeče. Každý název kategorie a popis je uveden. Tlačítko Zobrazit produkty, po kliknutí na způsobí, že zpětné volání, ale ještě neprovede žádnou akci.


[![Každou kategorii s název a popis se zobrazí, společně s produkty LinkButton zobrazit](custom-buttons-in-the-datalist-and-repeater-vb/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image12.png)

**Obrázek 6**: s každou kategorii název a popis se zobrazí, společně s produkty LinkButton zobrazit ([Kliknutím zobrazit obrázek v plné velikosti](custom-buttons-in-the-datalist-and-repeater-vb/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Krok 3: Provádění serverové logiku při the zobrazit produkty LinkButton po kliknutí

Kdykoliv po kliknutí na tlačítko, LinkButton nebo ImageButton v rámci šablony v DataList nebo opakovače, dojde k zpětné volání a DataList nebo opakovače s `ItemCommand` je aktivována událost. Kromě `ItemCommand` událostí, DataList řízení může také vyvolat jiné, více konkrétních událostí, pokud tlačítko s `CommandName` je nastavena na jednu vyhrazenou řetězce (odstranit, upravit, zrušení, aktualizace nebo vyberte), ale `ItemCommand` událostí je *vždy* aktivováno.

Při stisknutí tlačítka v rámci DataList nebo opakovače často musíme (například předají které tlačítko bylo kliknuto (v případě, že může existovat více tlačítek v ovládacím prvku, jako je například obě upravíte a tlačítko Odstranit) a případně nějaké další informace primární hodnota klíče položky, jejichž tlačítko označeného). Tlačítko, LinkButton a ImageButton poskytují dvě vlastnosti, jejichž hodnoty jsou předávány `ItemCommand` obslužné rutiny události:

- `CommandName`řetězec se obvykle používá k identifikaci každé tlačítko v šabloně
- `CommandArgument`běžně používané pro udržení hodnotu některé pole data, jako je například hodnotu primárního klíče

V tomto příkladu nastavit LinkButton s `CommandName` vlastnost ShowProducts a vazby aktuální záznam s hodnotu primárního klíče `CategoryID` k `CommandArgument` vlastnost pomocí syntaxe datové vazby `CategoryArgument='<%# Eval("CategoryID") %>'`. Po zadání tyto dvě vlastnosti, deklarativní syntaxi s LinkButton by měl vypadat následovně:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample3.aspx)]

Pokud po kliknutí na tlačítko, dojde k zpětné volání a DataList nebo opakovače s `ItemCommand` je aktivována událost. Obslužné rutiny události je předána na tlačítko s `CommandName` a `CommandArgument` hodnoty.

Vytvoření obslužné rutiny události pro opakovače s `ItemCommand` událostí a Poznámka: druhý parametr předaný do obslužné rutiny události (s názvem `e`). Tento druhý parametr je typu [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) a má následující čtyři vlastnosti:

- `CommandArgument`Hodnota tlačítka s `CommandArgument` vlastnost
- `CommandName`Hodnota tlačítko s `CommandName` vlastnost
- `CommandSource`odkaz na ovládací prvek tlačítko, který označeného kliknutím
- `Item`odkaz na [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) obsahující tlačítko označeného; každý záznam vázána na opakovače označované jako`RepeaterItem`

Od vybrané kategorie s `CategoryID` je předaná prostřednictvím `CommandArgument` vlastnost nám získat sadu produktů, které jsou spojené s vybranou kategorii v `ItemCommand` obslužné rutiny události. Tyto produkty mohou být vázány pak do ovládacího prvku BulletedList v `ItemTemplate` (které jsme jste ještě chcete-li přidat). Všechny, které zůstává a pak je přidání BulletedList, v odkazovat `ItemCommand` obslužné rutiny události a vytvořit vazbu k němu sadu produktů pro vybranou kategorii, které jsme budete řešení v kroku 4.

> [!NOTE]
> DataList s `ItemCommand` obslužné rutiny události se předá objekt typu [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), který nabízí stejné čtyři vlastnosti, jako `RepeaterCommandEventArgs` třídy.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Krok 4: Zobrazení vybrané kategorie s produkty v seznamu s odrážkami

Vybrané kategorie s produkty, může se zobrazit v rámci opakovače s `ItemTemplate` pomocí libovolný počet ovládacích prvků. Jsme může přidat že další vnořené opakovače, DataList, rozevírací seznam, rutina GridView a tak dále. Vzhledem k tomu, že chceme zobrazit tyto produkty jako seznam s odrážkami, ale použijeme ovládací prvek BulletedList. Vrácením `CustomButtons.aspx` stránky s deklarativní, přidání ovládacího prvku BulletedList `ItemTemplate` po LinkButton zobrazit produkty. Nastavit BulletedLists s `ID` k `ProductsInCategory`. BulletedList zobrazuje hodnotu pole dat, který je zadaný prostřednictvím `DataTextField` vzhledem k tomu, že bude mít tento ovládací prvek informace o produktu na něj navázaná, nastavte vlastnost `DataTextField` vlastnost `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample4.aspx)]

V `ItemCommand` obslužné rutiny události, odkazovat na tento ovládací prvek pomocí `e.Item.FindControl("ProductsInCategory")` a navázat jej na sadu produktů, které jsou spojené s vybranou kategorii.


[!code-vb[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample5.vb)]

Před provedením jakékoli akce v `ItemCommand` obslužné rutiny události, je doporučeno nejprve zkontrolujte hodnotu příchozí zprávy s `CommandName`. Vzhledem k tomu, `ItemCommand` obslužné rutiny události aktivuje se při *žádné* po kliknutí na tlačítko, pokud jsou více tlačítek v použití šablony `CommandName` hodnotu rozpoznat jakou akci chcete provést. Kontrola `CommandName` tady je moot, protože máme pouze jedno tlačítko, ale je dobrým která podporují do formuláře. Dále `CategoryID` vybrané kategorie se načítají `CommandArgument` vlastnost. Ovládací prvek BulletedList v šabloně je pak odkazovat a vázaný k výsledky `ProductsBLL` třídu s `GetProductsByCategoryID(categoryID)` metoda.

V předchozí kurzy, které používají tlačítka v rámci DataList, jako například [přehled o úpravy a odstraňování dat v prvku DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md), zjistili jsme hodnotu primárního klíče dané položky prostřednictvím `DataKeys` kolekce. Tento přístup funguje dobře u prvku DataList, opakovače nemá `DataKeys` vlastnost. Místo toho jsme musíte použít alternativní způsob pro zadávání hodnotu primárního klíče, například prostřednictvím tlačítko s `CommandArgument` vlastnost nebo přiřazení k skrytý ovládací prvek popisek Web v rámci šablony hodnotu primárního klíče a čtení její hodnota zpět v `ItemCommand`pomocí obslužných rutin událostí `e.Item.FindControl("LabelID")`.

Po dokončení `ItemCommand` obslužné rutiny události, za chvíli k otestování této stránce v prohlížeči. Jak je vidět na obrázku 7, kliknutím na tlačítko Zobrazit produkty odkaz způsobí, že zpětné volání a zobrazí produkty pro vybranou kategorii v BulletedList. Kromě toho Všimněte si, že tyto informace produktu zůstane, i v případě ostatních kategorií odkazy zobrazit produkty jsou zrušili.

> [!NOTE]
> Pokud chcete změnit chování této sestavy tak, aby pouze jednu kategorii s produkty jsou uvedeny v čase, jednoduše nastavit ovládací prvek BulletedList s `EnableViewState` vlastnost `False`.


[![BulletedList se používá k zobrazení vybrané kategorie produktů](custom-buttons-in-the-datalist-and-repeater-vb/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image15.png)

**Obrázek 7**: A BulletedList slouží k zobrazení vybrané kategorie produktů ([Kliknutím zobrazit obrázek v plné velikosti](custom-buttons-in-the-datalist-and-repeater-vb/_static/image17.png))


## <a name="summary"></a>Souhrn

Ovládací prvky DataList a opakovače může obsahovat libovolný počet tlačítek, LinkButtons nebo ImageButtons v rámci své šablony. Tlačítka, po kliknutí na způsobit zpětné volání a zvýšit `ItemCommand` událostí. Chcete-li přidružit se kliknutí na tlačítko Vlastní akce na straně serveru, vytvořit obslužnou rutinu události pro `ItemCommand` událostí. V této obslužné rutiny události nejprve zkontrolujte příchozí `CommandName` hodnotu, která určí, které tlačítko bylo kliknuto. Další informace můžete volitelně zadat prostřednictvím tlačítko s `CommandArgument` vlastnost.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl společnosti Dennis Patterson. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](custom-buttons-in-the-datalist-and-repeater-cs.md)
