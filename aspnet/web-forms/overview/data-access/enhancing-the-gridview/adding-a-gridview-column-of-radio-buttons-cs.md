---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: Přidání sloupce GridView přepínačů (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Tento kurz se zabývá postup přidání sloupce přepínačů do ovládacího prvku GridView poskytnutí uživatelského intuitivnější možnost výběru jednoho řádku...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: 1e1691b3c0c5fb576f25b84e8f4d7125a8d0c698
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366913"
---
<a name="adding-a-gridview-column-of-radio-buttons-c"></a>Přidání sloupce GridView přepínačů (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe) nebo [stahovat PDF](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> V tomto kurzu snaží přidání sloupce přepínačů do ovládacího prvku GridView poskytnutí uživatelského intuitivnější možnost výběru jednoho řádku prvku GridView.


## <a name="introduction"></a>Úvod

Ovládací prvek GridView nabízí spoustu integrované funkce. Zahrnuje celou řadou různých polí k zobrazení textu, obrázků, hypertextové odkazy a tlačítka. Podporuje šablony pro další vlastní nastavení. Pomocí několika kliknutí myší je možné provést ovládacího prvku GridView, kde každý řádek dají vybrat pomocí tlačítka nebo povolit úpravy nebo odstranění funkce s. Bez ohledu na velkém zadané funkce se často nastat situace, ve kterých dalších, bude potřeba přidat nepodporované funkce. V tomto kurzu a další dva prozkoumáme jak vylepšit funkce GridView s zahrnout další funkce.

V tomto kurzu a další příkaz, zaměřte se na zlepšení procesu výběr řádku. Jak kontrolován [Master/Detail pomocí volitelných GridView hlavní DetailView podrobnosti](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md), přidáme CommandField do prvku GridView, která obsahuje tlačítko pro výběr. Po kliknutí na zpětné volání vyplývá a GridView s `SelectedIndex` vlastnost je index řádku, vyberte tlačítko došlo ke kliknutí na aktualizovat. V [Master/Detail pomocí volitelných GridView hlavní DetailView podrobnosti](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) kurzu jsme viděli, jak tato funkce slouží k zobrazení podrobností pro vybraný řádek prvku GridView.

Když tlačítko pro výběr funguje v mnoha situacích, ho nemusí fungovat i pro ostatní uživatele. Místo použití tlačítka, se běžně používají dvě další prvky uživatelského rozhraní pro výběr: přepínač a zaškrtávací políčko. GridView jsme můžete rozšířit tak, aby místo tlačítko pro výběr, každý řádek obsahuje přepínače nebo zaškrtávacího políčka. Ve scénářích, kde uživatel může vybrat jen jednu z záznamy ovládacího prvku GridView může být přepínač upřednostňované nad tlačítko pro výběr. V situacích, kdy uživatel může potenciálně vybrat více záznamů, jako v aplikaci webového e-mailu, kde uživatel může být vhodné k výběru více zpráv odstranit zaškrtávacího políčka nabízí funkce, které nejsou k dispozici tlačítko pro výběr nebo přepínací tlačítka uživatelská rozhraní.

V tomto kurzu zjistí přidání sloupce přepínačů do prvku GridView. Řízení kurz se věnuje používání zaškrtávacích políček.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Krok 1: Vytvoření rozšíření GridView webové stránky

Než začneme, vylepšení ovládacího prvku GridView, aby zahrnovaly sloupec přepínačů, umožní s nejdřív využít k vytvoření stránky technologie ASP.NET v našem projektu webu, který budeme potřebovat pro tento kurz a další dva. Začněte přidáním novou složku s názvem `EnhancedGridView`. Dále přidejte následující stránky ASP.NET do této složky, nezapomeňte přiřadit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Přidání stránky technologie ASP.NET pro SqlDataSource související kurzy](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**Obrázek 1**: Přidání stránky technologie ASP.NET pro SqlDataSource související kurzy


V jiných složkách, jako jsou `Default.aspx` v `EnhancedGridView` složky zobrazí seznam kurzů v příslušném oddílu. Vzpomeňte si, že `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek tuto funkci poskytuje. Proto přidat tento uživatelský ovládací prvek `Default.aspx` přetažením v Průzkumníku řešení na stránku s návrhové zobrazení.


[![Přidat na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**Obrázek 2**: Přidejte `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))


A konečně, přidejte tyto čtyři stránky jako položky `Web.sitemap` souboru. Konkrétně, přidejte následující kód za použití ovládacím prvkem SqlDataSource `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, věnujte chvíli zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro úpravy, vložení a odstranění kurzy.


![Mapa webu nyní obsahuje záznamy pro zlepšení kurzy GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**Obrázek 3**: mapy webu nyní obsahuje záznamy pro zlepšení kurzy GridView


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Krok 2: Zobrazení dodavatelů v GridView

Pro tento kurz umožní s sestavení, který obsahuje seznam dodavatelů z USA, s každým řádkem GridView poskytuje přepínač GridView. Po výběru dodavateli přes přepínač, uživatel může zobrazit produkty s dodavatelem klepnutím na tlačítko. Zatímco tato úloha může připadat triviální, existuje několik odlišností, kterým jsou obzvláště složité. Předtím, než jsme se pustíte do těchto odlišností, umožní s nejdřív získat GridView výpis dodavatelů.

Začněte otevřením `RadioButtonField.aspx` stránku `EnhancedGridView` složky přetažením GridView z panelu nástrojů do návrháře. Nastavit prvek GridView s `ID` k `Suppliers` a z inteligentních značek, můžete vytvořit nový zdroj dat. Konkrétně vytvořte prvku ObjectDataSource s názvem `SuppliersDataSource` , který si vyžádá data z `SuppliersBLL` objektu.


[![Vytvoření nového prvku ObjectDataSource s názvem SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**Obrázek 4**: vytvoření nového prvku ObjectDataSource s názvem `SuppliersDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))


[![Konfigurace ObjectDataSource pomocí třídy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**Obrázek 5**: Konfigurace ObjectDataSource k použití `SuppliersBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))


Protože chceme seznam těchto dodavatelů v USA, zvolte `GetSuppliersByCountry(country)` z rozevíracího seznamu na kartě vyberte metodu.


[![Konfigurace ObjectDataSource pomocí třídy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**Obrázek 6**: Konfigurace ObjectDataSource k použití `SuppliersBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))


Z kartu aktualizace, vyberte možnost (žádné) možnost a klikněte na tlačítko Další.


[![Konfigurace ObjectDataSource pomocí třídy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**Obrázek 7**: Konfigurace ObjectDataSource k použití `SuppliersBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))


Vzhledem k tomu, `GetSuppliersByCountry(country)` metoda přijímá parametr, pokynů nám průvodce Konfigurovat zdroj dat pro zdroj tohoto parametru. K určení pevně zakódované hodnotu (USA, v tomto příkladu), ponechte tento parametr nastaven na hodnotu None zdroj rozevíracího seznamu a zadejte výchozí hodnotu v textovém poli. Kliknutím na Dokončit dokončíte průvodce.


[![Použít USA jako výchozí hodnota pro parametr země](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**Obrázek 8**: USA použijte jako výchozí hodnota pro `country` parametr ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))


Po dokončení průvodce bude obsahovat prvku GridView. Vlastnost BoundField pro každé pole data na dodavatele. Odeberte všechny kromě na `CompanyName`, `City`, a `Country` BoundFields a přejmenovat `CompanyName` BoundFields `HeaderText` vlastnost dodavateli. Až to uděláte, ovládacími prvky GridView a ObjectDataSource deklarativní syntaxe by měl vypadat nějak takto.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

Pro účely tohoto kurzu nechte s povolit uživatelům zobrazit vybraný poskytovatel s produkty na stejné stránce jako seznam dodavatele nebo na jiné stránce. Tuto skutečnost zohlednit, přidejte dva ovládací prvky tlačítka webové stránce. Můžu odebrat sadu `ID` s tyto dvě tlačítka `ListProducts` a `SendToProducts`, s myšlenkou, že `ListProducts` dojde ke kliknutí na zpětného odeslání dojde a zobrazí vybrané dodavatele s produkty na stejné stránce, ale po `SendToProducts` kliknutí na uživatel bude whisked na jinou stránku, která obsahuje seznam produktů.

Obrázek 9 ukazuje `Suppliers` prvky GridView a dvě tlačítka webové při prohlížení prostřednictvím prohlížeče.


[![Tito poskytovatelé z USA mají jejich název, Město a zemi informace](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**Obrázek 9**: dodavatelé ty z USA mají jejich název, Město a zemi uvedené informace ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Krok 3: Přidání sloupce přepínačů

V tomto okamžiku `Suppliers` GridView má tři BoundFields zobrazení název společnosti, Město a zemi každého dodavatele v USA. Sloupec přepínačů, je stále ale nedostatečná. Bohužel zahrnují t kódu ovládacího prvku GridView integrované RadioButtonField, jinak může stačí přidat elementy, které k mřížce jsme provést. Místo toho můžeme přidat TemplateField a nakonfigurovat jeho `ItemTemplate` k vykreslení přepínací tlačítko, což vede k přepínač pro každý řádek prvku GridView.

Na začátku může předpokládáme, že požadované uživatelské rozhraní je implementovat tak, že přidáte ovládací prvek RadioButton Web pro `ItemTemplate` z TemplateField. Během jednoho přepínače ve skutečnosti se přidá do každého řádku prvku GridView, přepínačů není možné seskupit a proto se vzájemně nevylučují. To znamená že koncový uživatel je možné současně vybrat vícenásobných přepínačů z prvku GridView.

Přestože pomocí TemplateField ovládacích prvků RadioButton se nebude poskytovat funkce potřebujeme, umožňují s implementaci tohoto přístupu, protože s vhodné prozkoumat, proč nejsou seskupeny výsledný přepínací tlačítka. Začněte přidáním TemplateField do prvku GridView dodavatelů, takže pole nejvíce vlevo. V dalším kroku z ovládacího prvku GridView s inteligentní značky, klikněte na odkaz Upravit šablony a přetáhněte ovládací prvek RadioButton webového z panelu nástrojů do TemplateField s `ItemTemplate` (viz obrázek 10). Nastavení RadioButton s `ID` vlastnost `RowSelector` a `GroupName` vlastnost `SuppliersGroup`.


[![Přidání ovládacího prvku RadioButton ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**Obrázek 10**: Přidání ovládacího prvku RadioButton do `ItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))


Po provedení tyto doplňky prostřednictvím návrháře, vaše značky ovládacího prvku GridView s by měl vypadat nějak takto:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

RadioButton – s [ `GroupName` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) se používá k seskupení řady přepínací tlačítka. Všechny ovládací prvky RadioButton se stejným `GroupName` hodnotu se považují za seskupené; pouze jeden přepínač lze vybrat ze skupiny najednou. `GroupName` Vlastnost určuje hodnotu vykreslené přepínač s `name` atribut. Prohlížeč prozkoumá přepínačů `name` atributů k určení přepínače tlačítko seskupení.

Pomocí ovládacího prvku RadioButton Web přidán do `ItemTemplate`, navštivte tuto stránku prostřednictvím prohlížeče a klikněte na přepínací tlačítka ve mřížka s řádky. Všimněte si, jak nejsou seskupeny přepínačů, což umožňuje vybrat všechny řádky, jako obrázek 11 ukazuje.


[![GridView s přepínače nejsou seskupeny](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**Obrázek 11**: komponenta GridView s přepínací tlačítka nejsou seskupeny ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))


Z důvodu nejsou seskupeny přepínačů je, protože jejich vykreslené `name` atributy se liší, bez ohledu na stejných `GroupName` nastavení vlastnosti. Pokud chcete zobrazit tyto rozdíly, proveďte zobrazení/zdroj z prohlížeče a zkontrolujte tlačítko na přepínač:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

Všimněte si, že jak i `name` a `id` atributy nejsou tak přesné hodnoty uvedené v okně Vlastnosti, ale jsou před s celou řadou dalších `ID` hodnoty. Další `ID` hodnoty přidané do přední části vygenerované `id` a `name` atributy jsou `ID` s přepínače tlačítka nadřazených ovládacích prvků `GridViewRow` s `ID` s, GridView s `ID`, Obsah ovládacího prvku s `ID`a tím webový formulář s `ID`. Tyto `ID` s přidají tak, aby každý vykreslí ovládací prvek webu v prvku GridView má jedinečnou `id` a `name` hodnoty.

Každý ovládací prvek musí vykreslen jiný `name` a `id` vzhledem k tomu, že je to jak v prohlížeči jednoznačně identifikoval každý ovládací prvek na straně klienta a způsobu, jakým se identifikuje na webový server jakou akci nebo došlo ke změně na zpětné volání. Představte si například, že jsme chtěli spouštět nějaký kód na straně serveru pokaždé, když se RadioButton s zkontrolovat, že se změnil stav. Jsme může dosáhnout nastavením RadioButton s `AutoPostBack` vlastnost `true` a vytváření obslužné rutiny události pro `CheckChanged` událostí. Ale pokud vygenerované `name` a `id` hodnoty pro všechny přepínačů bylo stejné na odeslat zpět jsme nelze určit, jaké konkrétní došlo ke kliknutí na RadioButton.

Short jeho je, že nemůžeme vytvořit sloupce přepínačů do ovládacího prvku GridView, pomocí ovládacího prvku RadioButton Web. Místo toho musíme použít místo toho archaickým techniky k zajištění, že odpovídající kód se vloží do každého řádku prvku GridView.

> [!NOTE]
> Jako ovládací prvek RadioButton webového, přepínač ovládací prvek, když se přidá do šablony, bude obsahovat jedinečný `name` atribut mřížky neseskupené – díky tomu mají přepínací tlačítka. Pokud nejste obeznámeni s ovládacími prvky jazyka HTML, klidně ignorujte tuto poznámku jako ovládací prvky HTML se používá jen občas, zejména v technologii ASP.NET 2.0. Ale pokud jste se chcete dozvědět více, přečtěte si téma [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s blogu [webové ovládací prvky a ovládací prvky HTML](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Pomocí prvku Literal Control vkládat značky tlačítko přepínače

Aby bylo možné správně skupinu pro všechny přepínače v prvku GridView, musíme vložit ručně značek tlačítek přepínače do `ItemTemplate`. Každý přepínač musí stejné `name` atribut, ale musí mít jedinečnou `id` atribut (v případě chceme přistupovat k přepínač prostřednictvím skriptu na straně klienta). Když uživatel vybere tlačítko přepínače a příspěvky zpět na stránku, prohlížeč odešle zpět hodnota vybraného přepínače s `value` atribut. Proto bude nutné každý přepínač jedinečný `value` atribut. Nakonec musíme na zpětné volání, nezapomeňte přidat `checked` atribut pro jeden přepínač, který je vybrán, v opačném případě po uživateli zajistí výběr a příspěvky zpět, přepínačů vrátí do výchozího stavu (všech nevybraných).

Existují dva přístupy, které je možné provést, aby bylo možné vložit kód nízké úrovně do šablony. Jeden je provést kombinaci značek a volání metody definované ve třídě použití modelu code-behind formátování. Tento postup se nejprve podrobněji [použití vlastností TemplateField v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) kurzu. V našem případě to může vypadat:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

Tady `GetUniqueRadioButton` a `GetRadioButtonValue` by být metody definované v modelu code-behind třídu, která vrátí odpovídající `id` a `value` hodnoty u obou přepínačů atributů. Tento přístup dobře funguje pro přiřazení `id` a `value` atributy, ale spadá krátký, při určování `checked` hodnotu atributu, protože syntaxe vázání dat se spustí pouze v případě nejprve vázaná na prvku GridView. Proto pokud prvku GridView má povolen stav zobrazení, metody formátování se pouze aktivuje při prvním načtení stránky (nebo když prvku GridView je explicitně znovu připojeno ke zdroji dat) a proto funkce, která nastaví `checked` atribut vyhráli t nelze volat pro zpětné volání. To s spíše drobný problém a trochu nad rámec tohoto článku, tak budete nechte si to. Jsem ale proveďte doporučujeme, abyste použijte výše uvedené přístup a pracovat prostřednictvím do bodu, kde je budete zablokuje. Při takové cvičení vyhráli t získat všechny blíže pracovní verzi, pomůže podporovat lepší představu o prvku GridView a životního cyklu datové vazby.

Jiný přístup k vlastní vloženého nízké úrovně značek v šabloně a si přístup, který budeme používat pro účely tohoto kurzu je přidat [prvku Literal control](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) do šablony. Potom v prvku GridView s `RowCreated` nebo `RowDataBound` obslužná rutina události ovládacím prvku Literal control lze přistupovat programově a jeho `Text` nastavenou na kód a vygenerovat.

Začněte tím, že ovládací prvek RadioButton odebrání TemplateField s `ItemTemplate`, jeho nahrazení atributem prvku Literal control. Nastavte ovládacím prvku Literal control s `ID` k `RadioButtonMarkup`.


[![Přidejte prvek literál šablony ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**Obrázek 12**: Přidání literálu ovládacího prvku `ItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))


Dále vytvořte obslužnou rutinu události pro prvek GridView s `RowCreated` událostí. `RowCreated` Událostí je spuštěna jednou pro každý řádek přidán, určuje, jestli data je právě znovu připojeno do prvku GridView. To znamená, že i při zpětném odeslání při dat je znovu načten ze zobrazení stavu `RowCreated` stále aktivuje událost, a to je důvod, proč používáme místo něj `RowDataBound` (což je vyvoláno pouze když explicitně vázaná k datům webový ovládací prvek).

V této obslužné rutiny události pouze chceme, aby bylo možné pokračovat, pokud jsme re se zabývá datovém řádku. Pro každý řádek dat chceme programově odkazovat `RadioButtonMarkup` prvku Literal control a nastavte jeho `Text` vlastnost vygenerovat kód. Jak ukazuje následující kód, značky, protože ho vytvoří rádio tlačítko, jehož `name` atribut je nastaven na `SuppliersGroup`, jehož `id` atribut je nastaven na `RowSelectorX`, kde *X* je index řádku prvku GridView a jejichž `value` atribut je nastaven na index řádku prvku GridView.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

Při výběru řádku prvku GridView a vyvolá zpětné volání, nás zajímají `SupplierID` vybraného poskytovatele. Proto jeden myslíte, že hodnota obou přepínačů by měla být skutečný `SupplierID` (místo index řádku prvku GridView). Přestože to může fungovat v některých případech, bylo by pro slepě přijímal a zpracovával bezpečnostní riziko `SupplierID`. Naše prvku GridView, například uvádí pouze dodavatele v USA. Ale pokud `SupplierID` předána přímo z přepínací tlačítko, které s mischievous uživatelům z manipulace s `SupplierID` hodnotu odeslána zpět na zpětné? Pomocí index řádku, jako `value`a potom `SupplierID` na zpětné volání z `DataKeys` kolekci, můžete zajišťujeme, že uživatel je pouze jedním z `SupplierID` hodnoty přiřazené k některému z řádků ovládacího prvku GridView.

Po přidání tohoto kódu obslužné rutiny události, trvat minutu, k otestování stránky v prohlížeči. Nejprve, mějte na paměti tohoto jediného přepínače současně můžete vybrat tlačítko v mřížce. Ale když výběrem přepínače a kliknutím na jedno z tlačítek, dojde k postbacku a všech přepínačů vrátit k jejich počátečního stavu (to znamená zpětné volání, vybraný přepínač je už zapnutá). To pokud chcete napravit, potřebujeme k posílení `RowCreated` tak, že zkontroluje index vybraného přepínače tlačítko odeslané ze zpětného volání a přidá obslužnou rutinu události `checked="checked"` atribut emitovaný kód odpovídá index řádku.

Zpětného odeslání dojde, prohlížeč odesílá zpět `name` a `value` z vybraného přepínače. Hodnota je možné načíst programově, pomocí `Request.Form["name"]`. [ `Request.Form` Vlastnost](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) poskytuje [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) představující proměnných formuláře. Formulář proměnné jsou názvy a hodnoty polí formuláře na webové stránce a jsou odesílány zpět ve webovém prohlížeči, při každém postbacku vyplývá. Protože vygenerované `name` atribut přepínacích tlačítek v prvku GridView `SuppliersGroup`, když webové stránky se pošle zpět v prohlížeči bude odesílat `SuppliersGroup=valueOfSelectedRadioButton` zpět na webový server (společně s další pole formuláře). Tyto informace lze poté přistupovat z `Request.Form` pomocí vlastnosti: `Request.Form["SuppliersGroup"]`.

Od jsme budete potřebovat k určení vybraného přepínače indexu není pouze v `RowCreated` obslužná rutina události, ale `Click` přidat obslužné rutiny pro ovládací prvky tlačítka webového a umožňují s `SuppliersSelectedIndex` vlastností do třídy modelu code-behind, která vrátí `-1`Pokud bylo vybráno žádné tlačítko přepínače a vybraného indexu, pokud je vybrána jedna přepínačů.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

S touto vlastností přidali, víme přidat `checked="checked"` značek v `RowCreated` obslužné rutiny události při `SuppliersSelectedIndex` rovná `e.Row.RowIndex`. Aktualizujte obslužnou rutinu události pro tuto logiku zahrnout:


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

Díky této změně vybraného přepínače zaškrtnuto po zpětném odeslání. Když teď máme možnost určit, jaké přepínače, můžeme změnit chování tak, aby při stránce byla navštívená poprvé, byla vybrána první řádek s přepínač ovládacího prvku GridView (a ne s žádné přepínací tlačítka ve výchozím nastavení vybrané, což je aktuální chování). Pokud chcete, aby první přepínací tlačítko vybrané ve výchozím nastavení, jednoduše změňte `if (SuppliersSelectedIndex == e.Row.RowIndex)` příkaz takto: `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`.

V tuto chvíli jsme přidali sloupec seskupené přepínačů do ovládacího prvku GridView, umožňující pro jeden řádek prvku GridView vybrané a zapamatovaných postbacků. Chcete-li zobrazit produkty poskytnuté dodavatelem vybrané jsou naše další kroky. V kroku 4 jsme ukážeme, jak přesměrovat uživatele na jinou stránku, odesílání podél vybrané `SupplierID`. V kroku 5 uvidíme, jak zobrazit produkty s vybranou dodavatele v prvku GridView na stejné stránce.

> [!NOTE]
> Místo použití TemplateField (fokus tento zdlouhavé krok 3), můžeme vytvořit vlastní `DataControlField` třídy, který vykreslí odpovídající uživatelské rozhraní a funkce. [ `DataControlField` Třídy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) je základní třída, ze kterého Vlastnost BoundField, třídě CheckBoxField, TemplateField a dalších předdefinovaných polí ovládacími prvky GridView a DetailsView odvodit. Vytváří se vlastní `DataControlField` třídy by znamenal, že může být přidán jenom pomocí deklarativní syntaxe sloupce přepínačů a také s žádným replikaci funkce na jiných webových stránek a jiných webových aplikací výrazně usnadňuje.


Pokud jste již někdy vytvářeli vlastní, kompilaci ovládacích prvků v ASP.NET, ale víte, že to vyžaduje množství legwork a provede s ní celou řadu odlišností a hraniční případy, které musí být pečlivě zpracovat. Proto jsme se forgo sloupce přepínačů jako vlastní implementace `DataControlField` třídy teď a zůstaňte s možností TemplateField. Možná jsme budete mít příležitost k prozkoumání vytvoření, použití a nasazení vlastních `DataControlField` třídy v budoucích kurzech.

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Krok 4: Zobrazení produkty s vybranou dodavatele na samostatné stránce

Jakmile uživatel vybral řádku prvku GridView, potřebujeme zobrazit produkty s vybranou dodavatele. V některých případech může chceme zobrazit tyto produkty na samostatné stránce, v jiných jsme pravděpodobně chtít provést na stejné stránce. Umožní s nejprve zkontrolujte způsob zobrazení produkty na samostatné stránce; v kroku 5 podíváme na přidání GridView k `RadioButtonField.aspx` k zobrazení vybrané dodavatele s produkty.

Aktuálně jsou dostupné dvě tlačítka webové ovládací prvky na stránce `ListProducts` a `SendToProducts`. Když `SendToProducts` po kliknutí na tlačítko, nám chcete poslat uživateli `~/Filtering/ProductsForSupplierDetails.aspx`. Na této stránce byl vytvořen v [filtrování záznamů Master/Detail napříč dvěma stránkami](../masterdetail/master-detail-filtering-across-two-pages-cs.md) kurz a zobrazí produkty pro dodavatele jehož `SupplierID` prochází skrze pole řetězce dotazu s názvem `SupplierID`.

Chcete-li poskytují tuto funkci, vytvořit obslužnou rutinu události pro `SendToProducts` tlačítko s `Click` událostí. V kroku 3 jsme přidali `SuppliersSelectedIndex` vybraná vlastnost, která vrátí index řádku, jehož přepínač. Odpovídající `SupplierID` lze získat v prvku GridView s `DataKeys` kolekce a uživatel je pak možné odeslat `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` pomocí `Response.Redirect("url")`.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

Tento kód funguje nádherně tak dlouho, dokud jeden z přepínačů je vybrat z prvku GridView. Pokud na začátku prvku GridView. nemá žádné přepínací tlačítka vybrána, a uživatel klikne `SendToProducts` tlačítko `SuppliersSelectedIndex` bude `-1`, což způsobí výjimku, která je vyvolána od `-1` je mimo rozsah indexu `DataKeys`kolekce. To není žádný problém, ale pokud jste se rozhodli aktualizovat `RowCreated` obslužná rutina události, jak je popsáno v kroku 3 tak, aby byl první přepínací tlačítko v prvku GridView. původně vybraná.

S ohledem `SuppliersSelectedIndex` hodnotu `-1`, přidání ovládacího prvku popisek na stránku nad prvku GridView. Nastavte jeho `ID` vlastnost `ChooseSupplierMsg`, jeho `CssClass` vlastnost `Warning`, jeho `EnableViewState` a `Visible` vlastností `false`a jeho `Text` vlastnosti prosím vyberte jiného dodavatele z mřížky. Třída CSS `Warning` text se zobrazí červený, kurzíva, tučné písmo, velké písma a je definován v `Styles.css`. Tím, že nastavíte `EnableViewState` a `Visible` vlastností `false`, popisek není generován s výjimkou pro jenom na ty, kde zpětnému volání ovládacího prvku s `Visible` prostřednictvím kódu programu je vlastnost nastavena na `true`.


[![Přidání ovládacího prvku popisek nad prvku GridView.](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**Obrázek 13**: Přidání popisku webové ovládací prvek výše prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))


Dále rozšířit `Click` obslužnou rutinu události pro zobrazení `ChooseSupplierMsg` popisek Pokud `SuppliersSelectedIndex` je menší než nula a přesměruje uživatele na `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` jinak.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

Na stránce v prohlížeči a klikněte na tlačítko `SendToProducts` tlačítko před výběrem jiného dodavatele z prvku GridView. Jak ukazuje obrázek 14, zobrazí se `ChooseSupplierMsg` popisek. V dalším kroku vyberte dodavatele a klikněte na tlačítko `SendToProducts` tlačítko. To bude whisk na stránku se seznamem poskytl dodavatel vybrané produkty. Obrázek 15 ukazuje `ProductsForSupplierDetails.aspx` stránky, kde byla vybrána Bigfoot pivovary dodavatele.


[![Pokud je vybraný žádný poskytovatel, zobrazí se popisek ChooseSupplierMsg](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**Obrázek 14**: `ChooseSupplierMsg` popisek se zobrazí, pokud je vybraný žádný poskytovatel ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))


[![Produkty s vybraný poskytovatel se zobrazují v ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**Obrázek 15**: The vybrali dodavatele s produkty jsou zobrazeny v `ProductsForSupplierDetails.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Krok 5: Zobrazení produkty s vybranou dodavatele na stejné stránce.

V kroku 4 jsme viděli, jak uživatele poslat na jiné webové stránky k zobrazení vybrané dodavatele s produkty. Případně lze zobrazit produkty s vybranou dodavatele na stejné stránce. Pro znázornění, přidáme jiný GridView k `RadioButtonField.aspx` k zobrazení vybrané dodavatele s produkty.

Od nás zajímá jenom tohoto prvku GridView produktů zobrazíte po vybral jiného dodavatele, přidejte ovládací prvek Panel webová pod `Suppliers` prvku GridView, nastavení jeho `ID` k `ProductsBySupplierPanel` a jeho `Visible` vlastnost `false`. V rámci panelu, přidejte text produkty pro vybrané dodavatele, za nímž následuje GridView s názvem `ProductsBySupplier`. V prvku GridView s inteligentních značek zvolte a vytvořte jeho vazbu nového prvku ObjectDataSource s názvem `ProductsBySupplierDataSource`.


[![Svázání prvku ProductsBySupplier GridView nového prvku ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**Obrázek 16**: vytvoření vazby `ProductsBySupplier` GridView pro nový prvek ObjectDataSource ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))


V dalším kroku nakonfigurujte ObjectDataSource používat `ProductsBLL` třídy. Protože chceme získat tyto produkty poskytnuté dodavatelem vybrané, zadat, že by měla vyvolat ObjectDataSource `GetProductsBySupplierID(supplierID)` metodu pro načtení jeho data. Vyberte (žádné) z rozevíracích seznamů v UPDATE, INSERT a DELETE karty.


[![Konfigurace ObjectDataSource GetProductsBySupplierID(supplierID) metody](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**Obrázek 17**: Konfigurace ObjectDataSource k použití `GetProductsBySupplierID(supplierID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))


[![Nastavte rozevírací seznamy na (žádný) v UPDATE, INSERT a odstranit záložky](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**Obrázek 18**: Nastavte rozevírací seznamy na (žádný) v UPDATE, INSERT a odstranit záložky ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))


Po dokončení konfigurace SELECT, UPDATE, Vložit a odstranit karty, klikněte na tlačítko Další. Vzhledem k tomu, `GetProductsBySupplierID(supplierID)` metoda očekává, že vstupní parametr, průvodce vytvořit zdroj dat vyzve nám jako zdroj pro hodnotu parametru s.

Máme k dispozici několik možností, které jsou tady v určení zdroje s hodnotu parametru. Jsme může používat výchozí parametr objekt a programově přiřadit hodnotu `SuppliersSelectedIndex` vlastnost s parametrem `DefaultValue` vlastnost v prvku ObjectDataSource s `Selecting` obslužné rutiny události. Vraťte se do [programové nastavení hodnot parametru ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md) kurzu si můžete znovu projít programově přiřazuje hodnoty prvku ObjectDataSource s parametry.

Můžeme také používat třídě ControlParameter a odkazovat na `Suppliers` GridView s [ `SelectedValue` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (viz obrázek 19). GridView s `SelectedValue` vrátí vlastnost `DataKey` odpovídající hodnotu [ `SelectedIndex` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Aby tato možnost fungovala, budeme potřebovat programově nastavit prvek GridView s `SelectedIndex` vlastnosti pro vybraný řádek při `ListProducts` po kliknutí na tlačítko. Jako dodatečná výhoda, tak, že nastavíte `SelectedIndex`, vybraný záznam se převezmou `SelectedRowStyle` definované v `DataWebControls` motivu (žlutým pozadím).


[![Třídě ControlParameter použijte k určení SelectedValue ovládacího prvku GridView s jako zdroj parametru](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**Obrázek 19**: použijte k určení GridView s SelectedValue jako zdroj parametru třídě ControlParameter ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))


Po dokončení průvodce bude Visual Studio automaticky přidá pole pro produkt s datová pole. Odeberte všechny kromě na `ProductName`, `CategoryName`, a `UnitPrice` BoundFields a změňte `HeaderText` vlastnosti na produkt, kategorie a ceny. Konfigurace `UnitPrice` Vlastnost BoundField tak, aby její hodnota je formátován jako měnu. Po provedení těchto změn, Panel ovládacího prvku GridView a prvku ObjectDataSource s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

K provedení tohoto cvičení, musíme nastavit prvek GridView s `SelectedIndex` vlastnost `SelectedSuppliersIndex` a `ProductsBySupplierPanel` Panel s `Visible` vlastnost `true` při `ListProducts` po kliknutí na tlačítko. Chcete-li to provést, vytvořte obslužnou rutinu události pro `ListProducts` tlačítko webový ovládací prvek s `Click` událostí a přidejte následující kód:


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

Pokud dodavatel nebyla vybrána z prvku GridView, `ChooseSupplierMsg` popisek se zobrazí a `ProductsBySupplierPanel` Panel skrytý. Jinak, pokud byl vybrán jiného dodavatele, `ProductsBySupplierPanel` se zobrazí a GridView s `SelectedIndex` vlastnost aktualizovat.

Obrázek 20 zobrazuje výsledky poté, co byla vybrána Bigfoot pivovary dodavatele a zobrazit produkty na tlačítku pro stránky se kliklo.


[![Produkty poskytnuté Bigfoot pivovary jsou uvedeny na stejné stránce.](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**Obrázek 20**: The produkty poskytnuté Bigfoot pivovary jsou uvedeny na stejné stránce ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))


## <a name="summary"></a>Souhrn

Jak je popsáno v [Master/Detail pomocí volitelných GridView hlavní DetailView podrobnosti](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) výukový program, záznamy můžete vybrat z ovládacího prvku GridView, použití CommandField jehož `ShowSelectButton` je nastavena na `true`. Ale CommandField zobrazí jeho tlačítka jako regulární tlačítek, odkazy nebo Image. Uživatelské rozhraní alternativní výběr řádku je poskytnout přepínače nebo zaškrtávacího políčka v každém řádku prvku GridView. V tomto kurzu jsme se zaměřili na přidání sloupce přepínačů.

Bohužel přidává sloupec rádiových tlačítek není t jako jednoduchá nebo jednoduše očekávat. Neexistuje žádné předdefinované RadioButtonField přidaný klikněte na tlačítko a pomocí ovládacího prvku RadioButton Web v rámci TemplateField přináší řadu problémů. Nakonec k poskytování těchto rozhraní buď máme můžete vytvořit vlastní `DataControlField` třídy nebo možnost s injektáží odpovídající kód HTML na pole TemplateField během `RowCreated` událostí.

S prozkoumali přidání sloupce přepínačů, dejte nám zapnout pozornost na přidání sloupce zaškrtávacích políček. Pomocí sloupce zaškrtávacích políček může uživatel vyberte jeden nebo více řádků ovládacího prvku GridView a pak proveďte nějaké operace na všech vybraných řádků (například výběrem sady e-mailů z webové e-mailového klienta a potom se rozhodnete odstranit všechny vybrané e-mailů). V dalším kurzu uvidíme, jak přidat tyto sloupce.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla David Suru. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](adding-a-gridview-column-of-checkboxes-cs.md)
