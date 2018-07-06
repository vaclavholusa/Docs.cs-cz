---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
title: Vlastní tlačítka v ovládacích prvcích DataList a Repeater (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu vytvoříme rozhraní, které používá Repeateru seznam kategorií v systému s každou kategorii poskytuje možnost zobrazit jeho associ...
ms.author: aspnetcontent
ms.date: 11/13/2006
ms.assetid: 1afdb14d-6e49-4e1f-aead-2934730d472e
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: ab580a706b76325fc4c0eccfc130ffa7db22fbd3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812948"
---
<a name="custom-buttons-in-the-datalist-and-repeater-vb"></a>Vlastní tlačítka v ovládacích prvcích DataList a Repeater (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_VB.exe) nebo [stahovat PDF](custom-buttons-in-the-datalist-and-repeater-vb/_static/datatutorial46vb1.pdf)

> V tomto kurzu vytvoříme rozhraní, které používá Repeateru seznam kategorií v systému s každou kategorii poskytuje možnost zobrazit jeho související produkty pomocí ovládacího prvku BulletedList.


## <a name="introduction"></a>Úvod

V průběhu posledních sedmnáct ovládacích prvků DataList a Repeater kurzy jsme ve vytvořené příklady jen pro čtení a úpravy i odstranění příklady. Pro usnadnění úpravy a odstranění možnosti a v prvku DataList, jsme přidali tlačítka na ovládacím prvku DataList s `ItemTemplate` , po kliknutí na způsobila zpětné volání a vyvolala událost DataList, odpovídající tlačítko s `CommandName` vlastnost. Například tlačítko pro přidání `ItemTemplate` s `CommandName` hodnota vlastnosti úpravy prvku DataList s způsobí, že `EditCommand` která se aktivuje při zpětné volání; s `CommandName` odstranit vyvolá `DeleteCommand`.

Kromě toho pro úpravy a odstraňování tlačítek, ovládacích prvků DataList a Repeater může také zahrnovat tlačítka, LinkButtons nebo ImageButtons, po kliknutí na provést nějakou vlastní logiku na straně serveru. V tomto kurzu vytvoříme rozhraní, které používá Repeateru seznam kategorií v systému. Pro každou kategorii, bude obsahovat Opakovači tlačítka zobrazíte kategorie produktů s přidružené použití ovládacího prvku BulletedList (viz obrázek 1).


[![Kliknutím na Zobrazit produkty odkaz zobrazí kategorie s produkty v seznamu s odrážkami](custom-buttons-in-the-datalist-and-repeater-vb/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image1.png)

**Obrázek 1**: Kliknutím na odkaz zobrazení zobrazit produkty kategorie s produkty v seznamu s odrážkami ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-buttons-in-the-datalist-and-repeater-vb/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Krok 1: Přidání vlastního tlačítka kurz webových stránek

Předtím, než se podíváme, jak přidat vlastní tlačítko, umožní s nejdřív využít k vytvoření stránky technologie ASP.NET v našem projektu webu, který budeme potřebovat pro účely tohoto kurzu. Začněte přidáním novou složku s názvem `CustomButtonsDataListRepeater`. Dále přidejte následující dvě stránky ASP.NET do této složky, nezapomeňte přiřadit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `CustomButtons.aspx`


![Přidání stránky technologie ASP.NET pro vlastní tlačítka související kurzy](custom-buttons-in-the-datalist-and-repeater-vb/_static/image4.png)

**Obrázek 2**: Přidání stránky technologie ASP.NET pro vlastní tlačítka související kurzy


V jiných složkách, jako jsou `Default.aspx` v `CustomButtonsDataListRepeater` složky zobrazí seznam kurzů v příslušném oddílu. Vzpomeňte si, že `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek tuto funkci poskytuje. Přidejte tento uživatelský ovládací prvek `Default.aspx` přetažením v Průzkumníku řešení na stránku s návrhové zobrazení.


[![Přidat na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](custom-buttons-in-the-datalist-and-repeater-vb/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image5.png)

**Obrázek 3**: Přidejte `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-buttons-in-the-datalist-and-repeater-vb/_static/image7.png))


A konečně, přidejte na stránkách jako položky `Web.sitemap` souboru. Konkrétně, přidejte následující kód za stránkování a řazení ovládacími prvky DataList a Repeater `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, věnujte chvíli zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro úpravy, vložení a odstranění kurzy.


![Mapa webu nyní obsahuje položku pro tento kurz vlastních tlačítek](custom-buttons-in-the-datalist-and-repeater-vb/_static/image8.png)

**Obrázek 4**: mapy webu nyní obsahuje položku pro tento kurz vlastních tlačítek


## <a name="step-2-adding-the-list-of-categories"></a>Krok 2: Přidání seznamu kategorií

Pro účely tohoto kurzu potřebujeme vytvořit Repeateru, který obsahuje seznam všech kategorií spolu zobrazit produkty odkazem (LinkButton), po kliknutí na zobrazí kategorie související s produkty v seznamu s odrážkami. Umožní s nejprve vytvořit jednoduché Repeateru, který obsahuje seznam kategorií v systému. Začněte otevřením `CustomButtons.aspx` stránku `CustomButtonsDataListRepeater` složky. Přetáhněte Repeateru z panelu nástrojů do návrháře a nastavte jeho `ID` vlastnost `Categories`. Dále vytvořte nový ovládací prvek zdroje dat z inteligentních značek s opakovače. Konkrétně vytvořte nový ovládací prvek ObjectDataSource s názvem `CategoriesDataSource` , který vybere data z `CategoriesBLL` třída s `GetCategories()` metody.


[![Konfigurace ObjectDataSource GetCategories() metody s CategoriesBLL třídy](custom-buttons-in-the-datalist-and-repeater-vb/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image9.png)

**Obrázek 5**: Konfigurace ObjectDataSource k použití `CategoriesBLL` třída s `GetCategories()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-buttons-in-the-datalist-and-repeater-vb/_static/image11.png))


Na rozdíl od ovládacího prvku DataList, pro kterou Visual Studio vytvoří výchozí `ItemTemplate` založené na zdroji dat, opakovače s šablony musí definovat manuálně. Kromě toho musí vytvořit a upravit pomocí deklarace šablony opakovače s (to znamená, s neexistuje žádné úpravy šablony možnost opakovače s inteligentním).

Klikněte na kartě Zdroj v levém dolním rohu a přidat `ItemTemplate` , který zobrazuje kategorii s názvem v `<h3>` elementu a jeho popis v odstavci označit; patří `SeparatorTemplate` , který zobrazí vodorovná čára (`<hr />`) mezi jednotlivými kategorie. Přidejte také odkazem (LinkButton) s jeho `Text` nastavenou na Zobrazit produkty. Po dokončení těchto kroků, by vaše stránka s deklarativní vypadat nějak takto:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample2.aspx)]

Obrázek 6 ukazuje na stránku při zobrazit pomocí prohlížeče. Každý název a popis kategorie je uvedena. Tlačítko Zobrazit produkty, po kliknutí na vyvolá zpětné volání, ale zatím neprovádí žádnou akci.


[![Každá kategorie s název a popis se zobrazí, spolu zobrazit produkty odkazem (LinkButton)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image12.png)

**Obrázek 6**: s každou kategorii název a popis se zobrazí, spolu zobrazit produkty odkazem (LinkButton) ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-buttons-in-the-datalist-and-repeater-vb/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Krok 3: Spuštění na straně serveru logiku při the zobrazit produkty odkazem (LinkButton) dojde ke kliknutí na

Kdykoli je kliknutí na tlačítko, odkazem (LinkButton) nebo ImageButton v rámci šablony v prvku DataList nebo Repeater, dojde k postbacku a s DataList nebo Repeater `ItemCommand` dojde k aktivaci události. Kromě `ItemCommand` událostí ovládacích prvků DataList ovládacího prvku může také vyvolat jiný, konkrétnější událost, pokud tlačítko s `CommandName` je nastavena na jednu z vyhrazené řetězce (odstranit, upravit, zrušení, aktualizace nebo vyberte), ale `ItemCommand` jeudálost*vždy* aktivována.

Po kliknutí na tlačítko v rámci prvku DataList nebo Repeater často potřebujeme (například předání došlo ke kliknutí na tlačítko, které (v případě, že může existovat více tlačítek v ovládacím prvku, jako jsou obě úpravy a tlačítko Odstranit) a případně určité další informace primární hodnota klíče položky došlo ke kliknutí na tlačítko, jejichž). Tlačítko, odkazem (LinkButton) a ImageButton poskytují dvě vlastnosti, jejichž hodnoty jsou předány `ItemCommand` obslužné rutiny události:

- `CommandName` řetězec se obvykle používá k identifikaci každé tlačítko v šabloně
- `CommandArgument` běžně používá pro uchování hodnoty některé pole data, jako je například hodnota primárního klíče

V tomto příkladu nastavte s odkazem (LinkButton) `CommandName` vlastnost ShowProducts a vazby aktuální záznam s hodnotu primárního klíče `CategoryID` k `CommandArgument` vlastnost pomocí syntaxe databinding `CategoryArgument='<%# Eval("CategoryID") %>'`. Po zadání těchto dvou vlastností, deklarativní syntaxe s odkazem (LinkButton) by měl vypadat nějak takto:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample3.aspx)]

Při klepnutí na tlačítko, dojde k postbacku a s DataList nebo Repeater `ItemCommand` dojde k aktivaci události. Obslužná rutina události je předána na tlačítko s `CommandName` a `CommandArgument` hodnoty.

Vytvořte obslužnou rutinu události pro opakovače s `ItemCommand` událostí a Všimněte si druhý parametr předaný do obslužné rutiny události (s názvem `e`). Tento druhý parametr je typu [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) a obsahuje následující čtyři vlastnosti:

- `CommandArgument` Hodnota na kliknutí na tlačítko s `CommandArgument` vlastnost
- `CommandName` Hodnota tlačítka s `CommandName` vlastnost
- `CommandSource` odkaz na ovládací prvek tlačítko, které bylo kliknuto
- `Item` odkaz na [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) , který obsahuje tlačítka, které bylo kliknuto; každý záznam vázán na Opakovači označované jako `RepeaterItem`

Od verze s vybranou kategorii `CategoryID` předaný prostřednictvím `CommandArgument` vlastnost, abychom se mohli sadu produktů, které jsou spojené s vybranou kategorii v `ItemCommand` obslužné rutiny události. Tyto produkty můžete pak měla být vázána k ovládacímu prvku BulletedList v `ItemTemplate` (které jsme ve ještě chcete-li přidat). Všechny, které zůstanou potom je přidat BulletedList, odkažte v `ItemCommand` obslužná rutina události a vytvořit vazbu k ní sadu produktů pro vybranou kategorii, která nám budete řešit v kroku 4.

> [!NOTE]
> DataList s `ItemCommand` obslužná rutina události je předán objekt typu [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), který nabízí čtyři stejné vlastnosti jako `RepeaterCommandEventArgs` třídy.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Krok 4: Zobrazení vybrané kategorie s produkty v seznamu s odrážkami

Vybrané kategorie s produkty můžete zobrazit v rámci opakovače s `ItemTemplate` používat libovolný počet ovládacích prvků. Jsme může přidat že další vnořené Repeater, a v prvku DataList, DropDownList, ovládacího prvku GridView a tak dále. Protože chceme zobrazit produkty jako seznam s odrážkami, ale použijeme ovládacího prvku BulletedList. Vrácení `CustomButtons.aspx` stránky s deklarativní, přidání ovládacího prvku BulletedList `ItemTemplate` po LinkButton zobrazit produkty. Nastavení BulletedLists s `ID` k `ProductsInCategory`. BulletedList zobrazuje hodnotu zadané přes pole data `DataTextField` vlastnost, protože tento ovládací prvek bude mít informace o produktu navázané, nastavte `DataTextField` vlastnost `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample4.aspx)]

V `ItemCommand` obslužná rutina události, odkazují na tento ovládací prvek pomocí `e.Item.FindControl("ProductsInCategory")` a jeho vazbu na sadu produktů, které jsou spojené s vybranou kategorii.


[!code-vb[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample5.vb)]

Před provedením jakékoli akce v `ItemCommand` obslužná rutina události, je vhodné nejdřív zkontrolujte hodnotu příchozí zprávy s `CommandName`. Protože `ItemCommand` obslužná rutina události je vyvoláno, když *jakékoli* po kliknutí na tlačítko, pokud existuje více tlačítek v použití šablon `CommandName` hodnotu jak ověřit, jaké akce se má provést. Kontroluje `CommandName` tady je moot, protože máme pouze jedno tlačítko, ale je dobré se formulář. Dále `CategoryID` vybrané kategorie je načten z `CommandArgument` vlastnost. V šabloně ovládacího prvku BulletedList pak odkazovat a svázaná s výsledky `ProductsBLL` třída s `GetProductsByCategoryID(categoryID)` metody.

V předchozích kurzech, které používají tlačítka v rámci prvku DataList, jako například [přehled o úpravy a odstraňování dat v ovládacím prvku DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md), můžeme určit hodnotu primárního klíče daná položka prostřednictvím `DataKeys` kolekce. Přestože tento přístup funguje dobře v prvku DataList, nemá opakovače `DataKeys` vlastnost. Místo toho alternativním přístupem musíte použít pro poskytnutí hodnotu primárního klíče, jako například prostřednictvím tlačítka s `CommandArgument` vlastnost nebo skrytý ovládací prvek popisek Web v rámci šablony přiřadí hodnotu primárního klíče a čtení jeho hodnoty v `ItemCommand`pomocí obslužné rutiny události `e.Item.FindControl("LabelID")`.

Po dokončení `ItemCommand` obslužná rutina události, využít k otestování této stránky v prohlížeči. Jak je vidět na obrázku 7, kliknutím na Zobrazit produkty odkaz vyvolá zpětné volání a zobrazí produkty pro vybranou kategorii v BulletedList. Kromě toho mějte na paměti, zůstane tato informace o produktu, i v případě, že kliknutím na odkazy zobrazit produkty jiných kategorií.

> [!NOTE]
> Pokud chcete změnit chování této sestavy tak, aby pouze jednu kategorii s produkty jsou uvedené v čase, stačí nastavit ovládacího prvku BulletedList s `EnableViewState` vlastnost `False`.


[![BulletedList slouží k zobrazení vybrané kategorie produktů](custom-buttons-in-the-datalist-and-repeater-vb/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image15.png)

**Obrázek 7**: A BulletedList slouží k zobrazení vybrané kategorie produktů ([kliknutím ji zobrazíte obrázek v plné velikosti](custom-buttons-in-the-datalist-and-repeater-vb/_static/image17.png))


## <a name="summary"></a>Souhrn

Ovládací prvky DataList a Repeater může obsahovat libovolný počet tlačítek, LinkButtons nebo ImageButtons v rámci jejich šablon. Tlačítka, po kliknutí na vyvolávají zpětné odeslání a zvýšit `ItemCommand` událostí. Chcete-li přidružit vlastní akce na straně serveru probíhá kliknutí na tlačítko, vytvořit obslužnou rutinu události pro `ItemCommand` událostí. V této obslužné rutiny události nejdřív zkontrolujte, příchozí `CommandName` hodnotu, která určí, které tlačítko došlo ke kliknutí na. Další informace můžete volitelně zadat pomocí tlačítka s `CommandArgument` vlastnost.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Dennis Patterson. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](custom-buttons-in-the-datalist-and-repeater-cs.md)
