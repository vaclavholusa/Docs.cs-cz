---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: "Přidat sloupec GridView přepínačů (C#) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu vypadá na tom, jak přidat sloupec přepínacích tlačítek do ovládacího prvku GridView poskytnout více intuitivní způsob výběru jednoho řádku uživatele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: 386fcb1cef896edbb465ba36415712af70d916ec
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-gridview-column-of-radio-buttons-c"></a>Přidat sloupec GridView přepínačů (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe) nebo [stáhnout PDF](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> V tomto kurzu vypadá na tom, jak přidat sloupec přepínacích tlačítek do ovládacího prvku GridView poskytnout více intuitivní způsob výběru jednoho řádku prvku GridView uživatele.


## <a name="introduction"></a>Úvod

Ovládací prvek GridView nabízí mnoho integrovanou funkci. Obsahuje počet různých polí pro zobrazení text, obrázky, hypertextové odkazy a tlačítka. Podporuje šablony pro další přizpůsobení. Pomocí několika kliknutí myší je možné, aby GridView, kde každý řádek lze vybrat pomocí tlačítka, nebo aby úpravy nebo odstranění možnosti s. Navzdory nadbytku zadaná funkce se často nastat situace, ve kterých další, bude třeba přidat nepodporované funkce. V tomto kurzu a další dvě podíváme, jak rozšířit funkce s GridView zahrnout další funkce.

Tento kurz a dalším soustředit na rozšíření procesu výběru řádků. Jako ověřuje při [hlavní/podrobností volitelný GridView hlavní pomocí podrobnosti DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md), přidáme CommandField k GridView, která zahrnuje vyberte tlačítko. Po kliknutí na zpětné volání vyplývá a GridView s `SelectedIndex` vlastnost se aktualizuje na index řádku, jehož vyberte tlačítko bylo kliknuto. V [hlavní/podrobností volitelný GridView hlavní pomocí DetailView podrobnosti](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) kurz, jsme viděli, jak pomocí této funkce lze zobrazit podrobnosti vybraného řádku GridView.

Když vyberte tlačítko funguje v mnoha situacích, nemusí také fungovat pro ostatní. Místo pomocí tlačítka, se běžně používají dva další prvky uživatelského rozhraní pro výběr: přepínač a zaškrtávací políčko. GridView jsme můžete rozšířit tak, aby místo vyberte tlačítko, každý řádek obsahuje přepínač nebo zaškrtávací políčko. Ve scénářích, kde uživatel může vybrat jen jednu GridView záznamy může být přepínač upřednostňované přes tlačítka Vybrat. V situacích, kde si uživatel může potenciálně vybrat více záznamů, jako v aplikaci e-mailové zprávy, kde může uživatel chcete vybrat více zpráv odstranit políčko nabízí funkce, které nejsou dostupné z vyberte tlačítko nebo přepínač uživatelská rozhraní.

V tomto kurzu vypadá na tom, jak přidat sloupec přepínačů GridView. Tento kurz budete pokračovat popisuje pomocí zaškrtávacích políček.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Krok 1: Vytvoření rozšíření GridView webové stránky

Než začneme rozšíření GridView, aby zahrnovaly sloupec přepínačů, umožní s nejprve vytváření stránek ASP.NET v našem webu projekt, který budeme potřebovat pro tento kurz a další dva chvíli trvat. Nejprve přidejte novou složku s názvem `EnhancedGridView`. Dál přidejte následující stránky ASP.NET do této složky, a zkontrolujte, zda přidružit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Přidání stránky ASP.NET pro kurzy související SqlDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**Obrázek 1**: Přidání stránky ASP.NET pro kurzy související SqlDataSource


V jiných složkách, jako `Default.aspx` v `EnhancedGridView` složky zobrazí seznam kurzů k v jeho části. Odvolat, který `SectionLevelTutorialListing.ascx` tuto funkci zajišťuje uživatelský ovládací prvek. Proto přidat tento uživatelský ovládací prvek pro `Default.aspx` přetažením z Průzkumníka řešení na stránku s zobrazení návrhu.


[![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**Obrázek 2**: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))


Nakonec přidejte tyto čtyři stránky jako položky na `Web.sitemap` souboru. Konkrétně, přidejte následující kód po pomocí ovládacího prvku SqlDataSource `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, pozorně Zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro úpravy, vkládání a odstraňování kurzy.


![Mapy webu nyní zahrnuje položky kurzy GridView rozšíření](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**Obrázek 3**: mapy webu nyní zahrnuje položky kurzy GridView rozšíření


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Krok 2: Zobrazení dodavatelů v GridView

Pro tento kurz umožní s sestavení GridView, obsahující dodavatelů z USA, s každým řádkem GridView poskytování přepínače. Po výběru dodavatele přes přepínač, můžete uživatele kliknutím na tlačítko Zobrazit produkty s dodavatele. Když tato úloha může zvukových trivial, je počet odlišnosti, které obzvláště složité. Předtím, než se pustíte jsme do těchto odlišnosti, umožní s nejdřív získat GridView výpis dodavatelů.

Začněte otevřením `RadioButtonField.aspx` stránku `EnhancedGridView` složky tak, že přetáhnete GridView z panelu nástrojů na návrháře. Nastavit GridView s `ID` k `Suppliers` a z jeho inteligentních značek, můžete vytvořit nový zdroj dat. Konkrétně vytvořit ObjectDataSource s názvem `SuppliersDataSource` který získává jeho data ze `SuppliersBLL` objektu.


[![Vytvořit nový ObjectDataSource s názvem SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**Obrázek 4**: vytvoření nové ObjectDataSource s názvem `SuppliersDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))


[![Konfigurace ObjectDataSource použití třídy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**Obrázek 5**: Konfigurace ObjectDataSource pro použití `SuppliersBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))


Vzhledem k tomu, že chceme seznam těchto dodavatelů v USA, vyberte `GetSuppliersByCountry(country)` metoda z rozevíracího seznamu vyberte kartě.


[![Konfigurace ObjectDataSource použití třídy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**Obrázek 6**: Konfigurace ObjectDataSource pro použití `SuppliersBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))


AKTUALIZACE kartě, vyberte možnost (žádné) možnost a kliknutím na tlačítko Další.


[![Konfigurace ObjectDataSource použití třídy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**Obrázek 7**: Konfigurace ObjectDataSource pro použití `SuppliersBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))


Vzhledem k tomu `GetSuppliersByCountry(country)` metoda přijímá parametr, Průvodce konfigurace zdroje dat nám vyzve ke zdroji tohoto parametru. Zadat hodnotu naprogramováno (USA, v tomto příkladu), ponechte parametr nastaven na žádný zdroj rozevíracího seznamu a do textového pole zadejte výchozí hodnotu. Kliknutím na tlačítko Dokončit ukončete průvodce.


[![Použít USA jako výchozí hodnota pro parametr země](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**Obrázek 8**: USA použijte jako výchozí hodnota `country` parametr ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))


Po dokončení průvodce bude GridView obsahovat BoundField pro každé pole dat dodavatele. Odeberte všechny ale na `CompanyName`, `City`, a `Country` BoundFields a přejmenujte `CompanyName` BoundFields `HeaderText` vlastnost dodavatele. Až to uděláte, by měla vypadat podobně jako následující rutina GridView a ObjectDataSource deklarativní syntaxi.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

V tomto kurzu mohli s umožňují uživateli zobrazit vybraný dodavatele s produkty na stejné stránce jako seznam dodavatele, nebo na jinou stránku. K tomuto požadavku vyhovělo, přidejte na stránku dva ovládací prvky webového tlačítko. I jste sadu `ID` s tato dvě tlačítka `ListProducts` a `SendToProducts`, za účelem který po `ListProducts` po kliknutí na zpětné volání dojde a vybrané dodavatele s produkty se objeví na stejné stránce, ale po `SendToProducts` po kliknutí na uživatel bude whisked na jinou stránku, která jsou uvedeny produkty.

Obrázek 9 ukazuje `Suppliers` GridView a dvě tlačítka webové ovládací prvky při zobrazení prostřednictvím prohlížeče.


[![Dodavatelé, z USA mít jejich název města a uvedené informace o zemi](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**Obrázek 9**: dodavatelé ty z jejich název mít USA, města a zemi informace uvedena ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Krok 3: Přidání sloupce přepínačů

V tomto okamžiku `Suppliers` GridView má tři BoundFields zobrazení název společnosti, Město a země každého dodavatele v USA. Sloupec přepínačů, je stále nedostatečná ale. Bohužel t nemá GridView zahrnují integrované RadioButtonField, jinak hodnota jsme právě, přidat do mřížky a provést. Místo toho jsme můžete přidávat TemplateField a konfigurovat jeho `ItemTemplate` k vykreslení přepínače, což vede k přepínač pro každý řádek GridView.

Na začátku může předpokládáme, že požadované uživatelské rozhraní můžete implementovat přidání do ovládacího prvku RadioButton webové `ItemTemplate` z TemplateField. Během to skutečně přidá jednoho přepínače na každý řádek GridView, přepínačů nelze seskupit a proto nejsou vzájemně vylučují. To znamená že koncový uživatel je možné vybrat více přepínačů současně z GridView.

I když pomocí TemplateField ovládací prvky webového přepínač nenabízí funkci potřebujeme, umožňují s implementovat tento přístup, protože s smysl zjistit, proč nejsou seskupeny výsledné přepínačů. Začněte přidáním TemplateField k GridView dodavatelů, takže je krajní levé pole. V dalším kroku klikněte na odkaz Upravit šablony ze GridView s inteligentní značky a přetáhněte ovládacího prvku RadioButton webové z panelu nástrojů do TemplateField s `ItemTemplate` (viz obrázek 10). Nastavit RadioButton s `ID` vlastnost `RowSelector` a `GroupName` vlastnost `SuppliersGroup`.


[![Přidání ovládacího prvku RadioButton do ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**Obrázek 10**: Přidání ovládacího prvku RadioButton do `ItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))


Po provedení těchto přidané prostřednictvím návrháře, vaše značky s GridView by měl vypadat takto:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

RadioButton s [ `GroupName` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) co se používá k seskupení řady přepínačů. Všechny ovládací prvky přepínač se stejným `GroupName` hodnota jsou považovány za seskupené; ze skupiny, lze vybrat pouze jeden přepínač najednou. `GroupName` Vlastnost určuje hodnotu vykreslené přepínač s `name` atribut. V prohlížeči prozkoumá přepínačů `name` atributů k určení přepínač tlačítko seskupení.

Pomocí ovládacího prvku RadioButton Web přidat do `ItemTemplate`, navštivte tuto stránku prostřednictvím prohlížeče a klikněte na přepínače v mřížka s řádky. Všimněte si, jak nejsou seskupení přepínačů, což umožňuje vybrat všechny řádky, jako obrázek 11 zobrazuje.


[![Rutina GridView s přepínačů jsou seskupené není](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**Obrázek 11**: The GridView s přepínače není seskupené ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))


Z důvodu nejsou seskupení přepínačů je, protože jejich vykreslené `name` atributy se liší, i přes stejných `GroupName` nastavení vlastnosti. Pokud chcete zobrazit tyto rozdíly, proveďte zobrazení nebo zdroj z prohlížeče a zkontrolujte kód tlačítko přepínačů:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

Všimněte si jak i `name` a `id` atributy, nejsou přesné hodnoty jako zadané v okně vlastností, ale se přidá jako předpona s číslem jiné `ID` hodnoty. Další `ID` hodnoty přidané do popředí vygenerované `id` a `name` atributy jsou `ID` s přepínače tlačítka nadřazené ovládací prvky `GridViewRow` s `ID` s, rutina GridView s `ID`, Ovládací prvek s obsahu `ID`a webového formuláře s `ID`. Tyto `ID` s přidají tak, aby každý vykresluje ovládací prvek webu v GridView má jedinečnou `id` a `name` hodnoty.

Každý vykresluje potřebám ovládací prvek v jiné `name` a `id` vzhledem k tomu, že je to jak v prohlížeči jednoznačně identifikuje každý ovládací prvek na straně klienta a jak ji identifikuje na webový server akce, nebo došlo ke změně na zpětné volání. Představte si například, že jsme chtěli spuštění některých kódu na straně serveru, kdykoli RadioButton s zkontrolovat, že se změnil stav. Jsme může dosáhnout nastavením RadioButton s `AutoPostBack` vlastnost `true` a vytvoření obslužné rutiny události pro `CheckChanged` událostí. Ale pokud vygenerované `name` a `id` hodnoty pro všechny přepínačů byly stejné na odeslat zpět se nepodařilo určit jaké konkrétní RadioButton označeného.

Krátké ho je, že jsme nelze vytvořit sloupec přepínačů v GridView pomocí ovládacího prvku RadioButton webové. Místo toho jsme musí zajistit, že je odpovídající značky vloženy do každý řádek rutina GridView použijte místo archaickým techniky.

> [!NOTE]
> Jako prvku RadioButton webové přepínač ovládací prvek HTML, když je přidán do šablony, bude obsahovat jedinečný `name` atributů, což přepínací tlačítka v mřížce neseskupení. Pokud nejste obeznámeni s ovládacích prvků jazyka HTML, klidně ignorovat tato poznámka jako ovládací prvky HTML se používá jen občas, zejména v technologii ASP.NET 2.0. Pokud chtějí dozvědět víc, podívejte se, ale [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) položku blogu s [webové a ovládacích prvků HTML](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Použití ovládacího prvku literálu vložení přepínač tlačítko značek

Aby bylo možné správně skupinu všech přepínačů v prvku GridView, je potřeba ručně vložit kód přepínacích tlačítek do `ItemTemplate`. Každý přepínač musí stejné `name` atribut, ale musí mít jedinečnou `id` atribut (v případě, že chceme přístup přepínač pomocí skriptu na straně klienta). Když uživatel vybere přepínače a příspěvcích zpět na stránku, prohlížeč odešle zpět hodnota vybraného přepínače s `value` atribut. Proto bude nutné každý přepínač jedinečný `value` atribut. Nakonec musíme na zpětné volání nezapomeňte přidat `checked` atribut jeden přepínač, který je vybrána, jinak po uživatel provede výběr a příspěvcích zpět, přepínačů vrátí do výchozího stavu (všechny nezaškrtnuté).

Existují dva přístupy, které lze provést tak, aby bylo možné vložit nízké úrovně značek do šablony. Jeden je provést směs značek a volání metodě formátování definovaný ve třídě kódu. Nejprve zabývá tato technika [pomocí TemplateFields v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) kurzu. V našem případě ho může vypadat podobně jako:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

Zde `GetUniqueRadioButton` a `GetRadioButtonValue` by metody definovaný ve třídě kódu, který vrátí odpovídající `id` a `value` atribut hodnoty pro každý přepínač. Tento postup funguje dobře pro přiřazení `id` a `value` atributy, ale spadá krátký, když je potřeba určit `checked` hodnota atributu, protože syntaxe vazby dat je pouze spuštěn, když data jsou nejprve svázána s GridView. Proto pokud GridView má povolen stav zobrazení, formátování metody bude pouze platit při prvním načtení stránky (nebo když GridView explicitně odrážejí ke zdroji dat) a proto funkce, která nastaví `checked` atribut won t volat na zpětné volání. Ho s místo jemně problém a trochu nad rámec tohoto článku, takže budete ponechat v této. I však provést doporučujeme, abyste použijte výše uvedený přístup a fungovat přes do bodu, kde je budete zablokuje. Při takové cvičení won t získat žádné blíže k pracovní verze, pomůže podporovat lépe pochopili, GridView a životního cyklu datové vazby.

Další přístup k vložení vlastní, nízké úrovně značek v šablonu a přístup, který budeme používat pro účely tohoto kurzu je přidání [prvku Literal control](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) do šablony. Potom v GridView s `RowCreated` nebo `RowDataBound` obslužné rutiny události, ovládacím prvku Literal control lze programově přistupovat a jeho `Text` vlastností nastavenou na kód pro vydávání.

Začněte tím, že RadioButton odebráním TemplateField s `ItemTemplate`, nahraďte ho prvku Literal control. Nastavit ovládacím prvku Literal control s `ID` k `RadioButtonMarkup`.


[![Přidání prvku Literal Control do ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**Obrázek 12**: Přidání literálu ovládacího prvku `ItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))


Dále vytvořte obslužnou rutinu události pro GridView s `RowCreated` událostí. `RowCreated` Událost aktivuje se po pro každý řádek přidat, zda data je právě odrážejí na GridView. To znamená, že i na zpětné volání při dat je znovu načíst ze zobrazení stavu, `RowCreated` stále aktivuje událost, a to je důvod, proč se používá se místo `RowDataBound` (který aktivuje se při dat je explicitně vázaný jenom na data ovládací prvek webu).

V této obslužné rutiny události jenom chceme, aby bylo možné pokračovat, pokud jsme re práci s datovým řádkem. Pro každý řádek dat chceme programově odkazovat `RadioButtonMarkup` prvku Literal control a sadu jeho `Text` vlastnost, která má kód pro vydávání. Jak ukazuje následující kód, značky vygenerované vytvoří přepínač tlačítko, jehož `name` je atribut nastaven na `SuppliersGroup`, jejichž `id` je atribut nastaven na `RowSelectorX`, kde *X* je index řádku GridView a jehož `value` je nastavena na hodnotu index řádku GridView.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

Když je vybrán řádek GridView a dojde k zpětné volání, jsme mají zájem o `SupplierID` vybrané dodavatele. Proto jeden může myslíte, že by měla být skutečnou hodnotu každý přepínač `SupplierID` (místo index řádku GridView). Když to může v některých případech fungovat, je bezpečnostní riziko pro slepě přijímal a zpracovával `SupplierID`. Naše GridView, například uvádí jenom dodavatelé v USA. Ale pokud `SupplierID` je předán přímo z přepínače, které s zastavit mischievous uživatele z manipulace s `SupplierID` hodnotu odesílají zpět na zpětné volání? Pomocí index řádku, jako `value`a potom `SupplierID` na z zpětné volání `DataKeys` kolekce, jsme můžete zajistit, že uživatel pouze používá jednu z `SupplierID` hodnoty jednoho z řádků GridView přidružené.

Po přidání tohoto kódu obslužná rutina události, trvat několik minut k otestování stránku v prohlížeči. První, Všimněte si, že pouze jeden přepínač najednou lze vybrat přepínač v mřížce. Ale když výběrem přepínače a kliknutím na jedno z tlačítek, dojde k zpětné volání a všech přepínačů vrátit původní (tedy na zpětné volání, vybraného přepínače je již vybrána). Chcete-li odstranit tento problém, musíme posílení `RowCreated` obslužné rutiny události, které se kontroluje index vybraného přepínače tlačítko odeslané ze zpětného volání a přidá `checked="checked"` atribut kód emitovaného shod index řádku.

Zpětné volání v případech, prohlížeč odesílá zpět `name` a `value` z vybraného přepínače. Hodnota může být načten prostřednictvím kódu programu, pomocí `Request.Form["name"]`. [ `Request.Form` Vlastnost](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) poskytuje [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) představující proměnných formuláře. Proměnných formuláře jsou názvy a hodnoty polí formuláře na webové stránce a jsou odesílány zpět ve webovém prohlížeči, vždy, když vyplývá zpětné volání. Protože vygenerované `name` atribut přepínače v GridView `SuppliersGroup`, pokud webová stránka je odeslána zpět v prohlížeči bude odesílat `SuppliersGroup=valueOfSelectedRadioButton` zpět na webový server (spolu s další pole formuláře). Tyto informace lze přistupovat z `Request.Form` pomocí vlastnosti: `Request.Form["SuppliersGroup"]`.

Od jsme budete potřebovat k určení vybraného přepínače indexu není pouze v `RowCreated` obslužné rutiny události, ale v `Click` přidejte obslužné rutiny události pro ovládací prvky webového tlačítko, umožňují s `SuppliersSelectedIndex` vlastnost k třídě kódu, který vrací `-1`Pokud nebyl vybrán žádný přepínač a vybraného indexu, pokud je vybrána jedna z přepínačů.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

Tato vlastnost přidat, jsme vědět, chcete-li přidat `checked="checked"` značek v `RowCreated` obslužné rutiny události při `SuppliersSelectedIndex` rovná `e.Row.RowIndex`. Aktualizujte obslužné rutiny události pro tuto logiku zahrnout:


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

Díky této změně vybraného přepínače vybrané po zůstane zpětné volání. Teď, když máme možnost určit, jaké přepínače, jsme může změnit chování, takže pokud se nejprve navštívené stránky, byl vybrán první řádek s přepínač GridView (místo s žádné přepínače ve výchozím nastavení zaškrtnuto, což je aktuální chování). Chcete-li mít první přepínač standardně vybraná, jednoduše změňte `if (SuppliersSelectedIndex == e.Row.RowIndex)` příkaz, který má následující: `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`.

V tuto chvíli jsme přidali sloupec seskupené rádiová tlačítka na GridView, která umožňuje jednoho řádku GridView vybrané a zapamatovaných napříč postback. Naše další kroky se mají zobrazit produkty poskytnutých vybrané dodavatelem. V kroku 4 ukážeme, jak přesměrovat uživatele na jinou stránku, odesílání podél vybraný `SupplierID`. V kroku 5 vidíte zobrazení vybrané dodavatele s produkty v GridView na stejné stránce.

> [!NOTE]
> Místo pomocí TemplateField (fokus tento zdlouhavé krok 3), jsme může vytvořit vlastní `DataControlField` třídu, která vykreslí příslušné uživatelské rozhraní a funkce. [ `DataControlField` Třída](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) je základní třída, ze kterého BoundField, vlastnost CheckBoxField, TemplateField a jiné předdefinovaná pole GridView a DetailsView odvozena. Vytváření vlastní `DataControlField` třída by znamenají, že nebylo možné přidat pouze pomocí deklarativní syntaxe sloupci přepínačů a by také provést replikace funkce na jiné webové stránky a další webové aplikace výrazně jednodušší.


Pokud jste již někdy vytvářeli vlastní, kompilovat ovládacích prvků technologie ASP.NET, ale víte, že to vyžaduje správného množství legwork a představuje s ním hostitele odlišnosti a hraniční případů, které je třeba pečlivě zvládnout. Proto jsme se forgo implementace sloupec přepínačů jako vlastní `DataControlField` třídy prozatím a přilepit s možností TemplateField. Možná jsme budete mít možnost prozkoumat vytvoření, použití a nasazení vlastních `DataControlField` třídy v budoucnu kurzu!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Krok 4: Zobrazení s produkty vybrané dodavatele na samostatné stránce

Poté, co uživatel vybral řádek GridView, musíme zobrazit vybraný dodavatele s produkty. V některých případech může chceme zobrazit tyto produkty na samostatné stránce v jiných jsme může být vhodné provést na stejné stránce. Umožní s nejprve zkontrolujte zobrazení produkty v samostatné stránce; v kroku 5 podíváme GridView k přidání `RadioButtonField.aspx` k zobrazení vybrané dodavatele s produkty.

Aktuálně jsou dvě tlačítka webové ovládací prvky na stránce `ListProducts` a `SendToProducts`. Když `SendToProducts` po kliknutí na tlačítko, nám chcete poslat uživateli `~/Filtering/ProductsForSupplierDetails.aspx`. Tato stránka byla vytvořena v [filtrování a podrobností mezi dvěma stránkami](../masterdetail/master-detail-filtering-across-two-pages-cs.md) kurzu a zobrazí produkty pro dodavatele jejichž `SupplierID` předána pole řetězce dotazu s názvem `SupplierID`.

Aby bylo možné tuto funkci, vytvářet obslužné rutiny události pro `SendToProducts` tlačítko s `Click` událostí. V kroku 3 jsme přidali `SuppliersSelectedIndex` vlastnosti, která vrátí index řádku, jehož přepínač je vybrána. Odpovídající `SupplierID` lze načíst z GridView s `DataKeys` kolekce a uživatel potom můžete odeslat do `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` pomocí `Response.Redirect("url")`.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

Tento kód funguje nádherně tak dlouho, dokud přepínací tlačítka vybrané je jedno z GridView. Pokud na začátku GridView nemá žádné přepínače vybrané, a uživatel klikne `SendToProducts` tlačítko `SuppliersSelectedIndex` bude `-1`, které způsobí výjimku, která je vyvolána od `-1` je mimo rozsah indexu `DataKeys`kolekce. To však není vážný, pokud jste se rozhodli aktualizovat `RowCreated` obslužné rutiny události, jak je popsáno v kroku 3 tak, aby byl na první tlačítko přepínačů v GridView zpočátku vybrali.

Aby bylo možné ošetřit `SuppliersSelectedIndex` hodnotu `-1`, přidání ovládacího prvku popisek na stránku výše GridView. Nastavit jeho `ID` vlastnost `ChooseSupplierMsg`, jeho `CssClass` vlastnost `Warning`, jeho `EnableViewState` a `Visible` vlastnosti, které chcete `false`a jeho `Text` vlastnost prosím zvolte jiného dodavatele z mřížky. Třída CSS, která `Warning` text se zobrazí červený, kurzíva, tučné písmo, velké písma a je definována v `Styles.css`. Nastavením `EnableViewState` a `Visible` vlastnosti, které chcete `false`, popisek není vykreslen s výjimkou pro pouze ty zpětná vystavení kde ovládacího prvku s `Visible` prostřednictvím kódu programu je vlastnost nastavena na `true`.


[![Přidání ovládacího prvku popisek výše GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**Obrázek 13**: přidejte popisek webové ovládací prvek nahoře GridView ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))


V dalším kroku posílení `Click` obslužné rutiny události pro zobrazení `ChooseSupplierMsg` popisku Pokud `SuppliersSelectedIndex` je menší než nula a přesměruje uživatele na `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` jinak.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

Navštívit stránku v prohlížeči a kliknutím `SendToProducts` tlačítko před výběrem dodavatele z GridView. Jak ukazuje obrázek 14, zobrazí se `ChooseSupplierMsg` popisek. V dalším kroku vyberte dodavatele a klikněte na `SendToProducts` tlačítko. To bude whisk na stránku, která jsou uvedeny produkty, které poskytl vybrané dodavatele. Obrázek 15 ukazuje `ProductsForSupplierDetails.aspx` v případě, že byl vybrán Bigfoot pivovary dodavatele.


[![Popisek ChooseSupplierMsg se zobrazí, pokud je vybraný žádný dodavatele](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**Obrázek 14**: `ChooseSupplierMsg` popisek se zobrazí, pokud je vybraný žádný dodavatele ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))


[![Vybrané dodavatele s produkty jsou zobrazeny v ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**Obrázek 15**: vybrané dodavatele s produkty jsou zobrazeny v `ProductsForSupplierDetails.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Krok 5: Zobrazení s produkty vybrané dodavatele na stejné stránce.

V kroku 4 jsme viděli, jak odeslat uživatele na jinou webovou stránku pro zobrazení vybrané dodavatele s produkty. Produkty s vybrané dodavatele Alternativně lze zobrazit na stejné stránce. Pro znázornění je přidáme jiná rutina GridView k `RadioButtonField.aspx` k zobrazení vybrané dodavatele s produkty.

Vzhledem k tomu, že chceme jenom tato rutina GridView produktů pro zobrazení po vybral jiného dodavatele, přidání ovládacího prvku panely webové pod `Suppliers` GridView, nastavení jeho `ID` k `ProductsBySupplierPanel` a jeho `Visible` vlastnost `false`. V panelu, přidejte text produkty pro vybrané dodavatele, za nímž následuje GridView s názvem `ProductsBySupplier`. Ze s GridView inteligentní značky, vyberte pro vytvoření vazby na nové ObjectDataSource s názvem `ProductsBySupplierDataSource`.


[![Rutina ProductsBySupplier GridView vytvořit vazbu k nové ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**Obrázek 16**: vytvoření vazby `ProductsBySupplier` GridView k nové ObjectDataSource ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))


V dalším kroku nakonfigurujte ObjectDataSource používat `ProductsBLL` třídy. Vzhledem k tomu, že chceme načtení těchto produktů vybrané dodavatel, určit, že ObjectDataSource by měl `GetProductsBySupplierID(supplierID)` metoda načíst data. (Žádný) vyberte z rozevíracího seznamu ve službě UPDATE, INSERT a odstraňte karty.


[![Konfigurace ObjectDataSource lze pomocí této metody GetProductsBySupplierID(supplierID)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**Obrázek 17**: Konfigurace ObjectDataSource pro použití `GetProductsBySupplierID(supplierID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))


[![Nastavte rozevírací seznamy (None) v aktualizaci UPDATE, INSERT a odstranit karty](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**Obrázek 18**: Nastavte rozevírací seznamy (None) ve UPDATE, INSERT a odstranit karty ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))


Po dokončení konfigurace SELECT, aktualizovat, Vložit a odstranit karty, klikněte na tlačítko Další. Vzhledem k tomu `GetProductsBySupplierID(supplierID)` metoda očekává vstupní parametr, průvodce vytvořit zdroj dat vyzve, abychom mohli určit zdroj pro hodnotu parametru s.

Máme několik možností, které jsou zde v určení zdroji s hodnotu parametru. Jsme může použít objekt výchozí parametr a prostřednictvím kódu programu přiřadit hodnotu `SuppliersSelectedIndex` vlastnost, která má parametr s `DefaultValue` vlastnost ObjectDataSource s `Selecting` obslužné rutiny události. Odkazovat zpět [prostřednictvím kódu programu nastavování hodnot parametrů ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md) kurzu aktualizačnímu programu na prostřednictvím kódu programu přiřazení hodnoty k ObjectDataSource s parametry.

Alternativně jsme můžete použít ControlParameter a odkazovat `Suppliers` GridView s [ `SelectedValue` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (viz obrázek 19). Rutina GridView s `SelectedValue` vlastnost vrátí `DataKey` odpovídající hodnota [ `SelectedIndex` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Aby tato možnost fungovala, je potřeba nastavit programově GridView s `SelectedIndex` vlastnosti tak, aby vybraný řádek, kdy `ListProducts` po kliknutí na tlačítko. Jako dodatečná výhoda, a to nastavením `SelectedIndex`, vybraný záznam neprovede `SelectedRowStyle` definované v `DataWebControls` motiv (žlutý pozadí).


[![Použijte k určení GridView s SelectedValue jako zdroj parametru ControlParameter](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**Obrázek 19**: použijte k určení GridView s SelectedValue jako zdroj parametru ControlParameter ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))


Po dokončení průvodce, Visual Studio automaticky přidá pole pro produkt s datová pole. Odeberte všechny ale na `ProductName`, `CategoryName`, a `UnitPrice` BoundFields a změňte `HeaderText` vlastností produktu, kategorie a ceny. Konfigurace `UnitPrice` BoundField tak, aby její hodnota je formátován jako měny. Po provedení těchto změn, panely rutina GridView a ObjectDataSource s deklarativní by měl vypadat následovně:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

Dokončete tento postup je potřeba nastavit GridView s `SelectedIndex` vlastnost, která má `SelectedSuppliersIndex` a `ProductsBySupplierPanel` Panel s `Visible` vlastnost `true` při `ListProducts` po kliknutí na tlačítko. K tomu, vytvoření obslužné rutiny událostí `ListProducts` ovládací prvek tlačítko webu s `Click` událostí a přidejte následující kód:


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

Pokud dodavatel nebyl vybrán z GridView, `ChooseSupplierMsg` se zobrazí popisek a `ProductsBySupplierPanel` panely skryté. Jinak, pokud bylo vybráno jiného dodavatele, `ProductsBySupplierPanel` se zobrazí a GridView s `SelectedIndex` vlastnosti.

Obrázek 20 zobrazuje výsledky po vybral Bigfoot pivovary dodavatele a klepnutí na tyto produkty zobrazit na stránce tlačítko.


[![Produkty poskytl Bigfoot pivovary jsou uvedeny na stejné stránce.](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**Obrázek 20**: produkty poskytl Bigfoot pivovary jsou uvedeny na stejné stránce ([Kliknutím zobrazit obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))


## <a name="summary"></a>Souhrn

Jak je popsáno v [hlavní/podrobností volitelný GridView hlavní pomocí podrobnosti DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) kurz, záznamů je možné vybrat z GridView, použití CommandField jejichž `ShowSelectButton` je nastavena na `true`. Ale CommandField zobrazí jeho tlačítka buď jako regulární tlačítek, odkazů nebo bitové kopie. Uživatelské rozhraní alternativní výběr řádku je poskytnout přepínač nebo zaškrtávací políčko v jednotlivých řádcích GridView. V tomto kurzu jsme se zaměřili na tom, jak přidat sloupec přepínačů.

Bohužel přidání sloupce přepínacích tlačítek neběží t jako přehledné nebo jednoduše jako jeden by se dalo očekávat. Neexistuje žádné předdefinované RadioButtonField, který lze přidat na klepnutím na tlačítko a pomocí ovládacího prvku RadioButton Web v rámci TemplateField představuje vlastní sadu problémy. V části end k poskytování takového rozhraní budeme mít buď vytvořit vlastní `DataControlField` třídu nebo možnost pro vložení do TemplateField během odpovídající HTML `RowCreated` událostí.

S prozkoumali jak přidat sloupec přepínačů, dejte nám zapnout naše pozornost přidat sloupec zaškrtávací políčka. Se sloupcem zaškrtávací políčka může uživatel vybrat jeden nebo více řádků GridView a potom provést některé operace na všechny vybrané řádky (například vyberete sadu e-mailů z webové e-mailového klienta a potom se rozhodnete odstranit všechny vybrané e-mailů). V dalším kurzu jsme zobrazí, jak přidat tento sloupce.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl David Suru. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Next](adding-a-gridview-column-of-checkboxes-cs.md)
