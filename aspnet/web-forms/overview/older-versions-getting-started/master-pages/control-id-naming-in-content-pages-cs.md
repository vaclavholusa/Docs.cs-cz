---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: "Řízení ID pojmenování v obsahu stránky (C#) | Microsoft Docs"
author: rick-anderson
description: "Ukazuje, jak ovládacích prvků ContentPlaceHolder sloužit jako názvový kontejner a proto ujistěte se, prostřednictvím kódu programu práce s ovládacím prvkem obtížné (prostřednictvím FindConrol)..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0c0db7fd76a7a486ff45085329ef7c77b0af5ebe
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="control-id-naming-in-content-pages-c"></a>ID ovládacího prvku pojmenování v obsahu stránky (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> Ukazuje, jak ovládacích prvků ContentPlaceHolder sloužit jako názvový kontejner a proto ujistěte se, prostřednictvím kódu programu práce s ovládacím prvkem obtížné (prostřednictvím FindConrol). Vyhledá v tomto problému a řešení. Také popisuje, jak k programovému přístupu ke výsledná hodnota ClientID.


## <a name="introduction"></a>Úvod

Zahrnout všechny serverových ovládacích prvků ASP.NET `ID` vlastnost, která jednoznačně identifikuje ovládacího prvku a prostředky, se kterým je ovládací prvek programově přistupovat ve třídě kódu. Podobně, může zahrnovat elementů v dokumentu HTML `id` atribut, který jednoznačně identifikuje element; tato `id` hodnoty se často používají ve skriptu na straně klienta prostřednictvím kódu programu odkazovat určitý element HTML. Zadána to vám může předpokládáme, že při vykreslení ovládacího prvku ASP.NET do kódu HTML, jeho `ID` hodnota se používá jako `id` hodnotu vykreslovaného elementu HTML. To není nezbytně případě protože za určitých okolností jedné řídit s jedním `ID` hodnota se mohou objevit vícekrát v vykreslované značky. Vezměte v úvahu GridView ovládací prvek, který obsahuje TemplateField pomocí ovládacího prvku popisek Web s `ID` hodnotu ProductName. Pokud GridView je vázán na zdroj dat za běhu, je tento popisek pro každý řádek GridView jednou opakuje. Každý popisek musí vykresluje jedinečný `id` hodnotu.

Technologie ASP.NET pro zpracování takových scénářů, umožňuje některé ovládací prvky pro být označené jako pojmenování kontejnerů. Pojmenování kontejneru slouží jako nový `ID` oboru názvů. Všech ovládacích prvků serveru, které se zobrazují v rámci názvový kontejner mít jejich vykreslené `id` předponu hodnotu `ID` pojmenování kontejneru ovládacího prvku. Například `GridView` a `GridViewRow` třídy jsou obě pojmenování kontejnerů. V důsledku toho ovládací prvek popisek, které jsou definované v GridView TemplateField s `ID` ProductName je uveden vykreslovaných `id` hodnotu `GridViewID_GridViewRowID_ProductName`. Protože *GridViewRowID* je jedinečný pro každý řádek GridView výsledná `id` jsou jedinečné hodnoty.

> [!NOTE]
> [ `INamingContainer` Rozhraní](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) slouží k označení, že konkrétní ovládací prvek ASP.NET serveru by měla fungovat jako názvový kontejner. `INamingContainer` Rozhraní není pravopisu na všech metod, které musí implementovat ovládacího prvku serveru; místo toho se používá jako značku. Při generování vykreslované značky, pokud toto rozhraní implementuje ovládacího prvku pak modul ASP.NET automaticky předpony jeho `ID` hodnotu jejího podřízeného prvku vykresluje `id` hodnoty atributu. Tento proces je podrobněji popsána v kroku 2.


Pojmenování kontejnerů nejen změnit vygenerované `id` hodnota atributu, ale také mít vliv na způsob řízení může prostřednictvím kódu programu na něj odkazovat z třídy kódu stránky ASP.NET. `FindControl("controlID")` Metoda se často používá ke programově odkazovat ovládací prvek webu. Ale `FindControl` není vniknutí prostřednictvím pojmenování kontejnerů. V důsledku toho nelze použít přímo `Page.FindControl` metoda tak, aby odkazovaly ovládacích prvků v GridView nebo jiných názvový kontejner.

Jak vám může mít surmised, hlavní stránky a ContentPlaceHolders jsou obě implementovány jako pojmenování kontejnerů. V tomto kurzu jsme zkontrolujte jak hlavní prvek HTML stránky vliv `id` hodnoty a způsoby, jak programově odkazovat ovládací prvky webového v rámci stránky obsahu pomocí `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Krok 1: Přidání nové stránky ASP.NET

K předvedení konceptů popsaných v tomto kurzu, umožňuje přidat novou stránku ASP.NET na našem webu. Vytvoření nového obsahu stránky s názvem `IDIssues.aspx` v kořenové složce vazby, aby `Site.master` stránky předlohy.


![Přidání obsahu stránce IDIssues.aspx ke kořenové složce](control-id-naming-in-content-pages-cs/_static/image1.png)

**Obrázek 01**: Přidání stránky obsahu `IDIssues.aspx` ke kořenové složce


Visual Studio automaticky vytvoří ovládací prvek obsahu pro všechny čtyři ContentPlaceHolders stránky předlohy. Jak jsme uvedli v [ *více ContentPlaceHolders a výchozí obsah* ](multiple-contentplaceholders-and-default-content-cs.md) kurz, pokud není k dispozici ovládací prvek obsahu je místo toho vygenerované obsah ContentPlaceHolder výchozí stránky předlohy. Protože `QuickLoginUI` a `LeftColumnContent` ContentPlaceHolders obsahovat značek vhodná výchozí hodnota pro tuto stránku, pokračujte a odebrání jejich odpovídajících ovládací prvky obsahu z `IDIssues.aspx`. V tomto okamžiku obsahu stránce deklarativní by měl vypadat následovně:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

V [ *zadáte název, značky Meta a ostatní hlavičky HTML na hlavní stránce* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) kurzu jsme vytvořili vlastní základní stránky třídu (`BasePage`), automaticky nakonfiguruje nadpis stránky, pokud je není explicitně nastaven. Pro `IDIssues.aspx` stránky tuto funkci využívat, třídy kódu stránky musí být odvozeny od `BasePage` – třída (místo `System.Web.UI.Page`). Upravte definici třídy kódu tak, aby vypadal jako následující:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

Nakonec aktualizujte `Web.sitemap` souboru záznam pro tento nový lekce. Přidat `<siteMapNode>` elementu a sadu jeho `title` a `url` atributy na "Řízení ID pojmenování problémy" a `~/IDIssues.aspx`, v uvedeném pořadí. Po provedení tohoto přidání vaší `Web.sitemap` souboru značek by měl vypadat podobně jako následující:


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

Jak ukazuje obrázek 2, nové položky mapy webu v `Web.sitemap` se okamžitě projeví v části lekce v levém sloupci.


![V části lekce nyní zahrnuje odkaz na &quot;pojmenování problémy ID ovládacího prvku&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**Obrázek 02**: část lekce teď obsahuje odkaz na "ID ovládacího prvku pojmenování problémy"


## <a name="step-2-examining-the-renderedidchanges"></a>Krok 2: Prozkoumání vygenerované`ID`změny

Abyste lépe pochopili úpravy ASP.NET modul provede vygenerované `id` hodnoty serveru ovládací prvky, přidejme několik webových ovládacích prvků do `IDIssues.aspx` stránky a zobrazte vykreslované značky odesláno prohlížeči. Konkrétně typu v textu "Zadejte prosím svůj věk:" následované ovládacího prvku TextBox webového. Další dolů na stránce přidáte ovládacího prvku tlačítko webové a ovládacího prvku popisek Web. Nastavit textového pole `ID` a `Columns` vlastnosti, které chcete `Age` a 3, v uvedeném pořadí. Nastavte na tlačítko `Text` a `ID` vlastnosti, které chcete "Odeslat" a `SubmitButton`. Vymažte jmenovky `Text` vlastnost a sadu jeho `ID` k `Results`.

V tomto okamžiku deklarativní obsahu ovládacího prvku by měl vypadat takto:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

Obrázek 3 ukazuje na stránku při zobrazení v návrháři Visual Studio.


[![Stránka obsahuje tři ovládací prvky webového: textové pole, tlačítko a popisku](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**Obrázek 03**: zahrnuje tři webové ovládací prvky stránky: textové pole, tlačítka a popisku ([Kliknutím zobrazit obrázek v plné velikosti](control-id-naming-in-content-pages-cs/_static/image5.png))


Navštívit stránku prostřednictvím prohlížeče a pak zobrazit zdrojový kód HTML. Jako značka níže ukazuje `id` hodnoty elementů HTML pro textové pole, tlačítka a popisek webové ovládací prvky jsou kombinaci `ID` hodnoty webových ovládacích prvků a `ID` hodnoty pojmenování kontejnerů na stránce.


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

Jak jsme uvedli dříve v tomto kurzu, hlavní stránky a jeho ContentPlaceHolders sloužit jako pojmenování kontejnerů. V důsledku toho, jak přispívat vygenerované `ID` hodnoty jejich vnořené ovládacích prvků. Trvat textového pole `id` atributů, například: `ctl00_MainContent_Age`. Odvolat, ovládacího prvku TextBox `ID` hodnota byla `Age`. To je předponu jeho prvek ContentPlaceHolder `ID` hodnotu `MainContent`. Kromě toho je tato hodnota předponu stránky předlohy `ID` hodnotu `ctl00`. Se projeví `id` hodnota atributu, který se skládá z `ID` hodnoty stránky předlohy, ovládací prvek ContentPlaceHolder a do textového pole sám sebe.

Obrázek 4 ukazuje toto chování. K určení vygenerované `id` z `Age` textovému poli, začněte s `ID` hodnotu prvku TextBox `Age`. V dalším kroku fungují vaše hierarchie ovládacího prvku. Na každý názvový kontejner (tyto uzly broskvoněmi barvou) předpony aktuální vykresluje `id` s názvový kontejner `id`.


![Atributy id přepuštěné jsou založené na ID hodnoty pojmenování kontejnerů](control-id-naming-in-content-pages-cs/_static/image6.png)

**Obrázek 04**: The vykreslen `id` atributy jsou založené na `ID` hodnoty pojmenování kontejnerů


> [!NOTE]
> Jak již bylo zmíněno, `ctl00` část vygenerované `id` atribut se považuje za `ID` hodnoty stránky předlohy, ale možná vás zajímá jak tato `ID` hodnoty pocházejí. Jsme nezadali ho kdekoli v naší hlavní nebo obsahu stránce. Většina ovládacích prvků serveru na stránku ASP.NET se přidají explicitně prostřednictvím stránky deklarativní značky. `MainContent` Prvek ContentPlaceHolder explicitně zadaná ve značkách `Site.master`; `Age` byl definován TextBox `IDIssues.aspx`na značek. Lze zadat `ID` hodnoty pro tyto typy ovládacích prvků prostřednictvím okna vlastnosti nebo z deklarativní syntaxi. V deklarativní nejsou definovány další ovládací prvky, jako jsou vlastní stránky předlohy. V důsledku toho jejich `ID` hodnoty musí být pro nás automaticky generovány. Modul sady ASP.NET `ID` hodnoty v době běhu pro tyto ovládací prvky, jehož ID nebyly explicitně nastavena. Používá vzoru pro pojmenovávání `ctlXX`, kde *XX* postupně roste celočíselná hodnota.


Vzhledem k tomu, že je hlavní server samotné stránky slouží jako názvový kontejner, ovládací prvky webového na hlavní stránce definován také změnili vykreslené `id` hodnoty atributu. Například `DisplayDate` popisek jsme přidali na hlavní stránku [ *vytvoření rozložení na webu pomocí stránky předlohy* ](creating-a-site-wide-layout-using-master-pages-cs.md) kurzu má následující vykreslení značek:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

Všimněte si, že `id` atribut zahrnuje obě stránky předlohy `ID` hodnotu (`ctl00`) a `ID` hodnotu ovládacího prvku popisek webového (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Krok 3: Prostřednictvím kódu programu odkazující na ovládací prvky webového prostřednictvím`FindControl`

Každý ovládací prvek ASP.NET serveru zahrnuje `FindControl("controlID")` metoda, která hledá podřízeného ovládacího prvku pro ovládací prvek s názvem *controlID*. Pokud je nalezen takový ovládací prvek, je vrácena; Pokud se nenajde žádný odpovídající ovládací prvek, `FindControl` vrátí `null`.

`FindControl`je užitečné v situacích, kdy je potřeba řízení přístupu, ale nemáte přímý odkaz na jeho. Při práci s daty ovládací prvky webového jako GridView, například ovládací prvky v rámci prvku GridView pole jsou definovány jednou v deklarativní syntaxi, ale v době běhu je vytvořena instance ovládacího prvku pro každý řádek GridView. V důsledku toho existují ovládací prvky generovaná za běhu, ale nemáme k dispozici od třídy kódu přímý odkaz. Proto je potřeba použít `FindControl` prostřednictvím kódu programu pracovat s určitý ovládací prvek v rámci prvku GridView pole. (Další informace o používání `FindControl` přístup k ovládacím prvkům v rámci prvku data webové šablony, najdete v tématu [vlastní formátování dat na základě při](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md).) K této stejné situaci dojde, když dynamicky přidávání ovládacích prvků do webového formuláře, téma se zabývá [vytváření dynamické uživatelská rozhraní vstupní Data](https://msdn.microsoft.com/library/aa479330.aspx).

Pro ilustraci použití `FindControl` metody na hledání pro ovládací prvky obsahu stránce, vytvoření obslužné rutiny události pro `SubmitButton`na `Click` událostí. V obslužné rutiny události, přidejte následující kód, který se odkazuje prostřednictvím kódu programu `Age` textové pole a `Results` popisku pomocí `FindControl` metoda a poté zobrazí zprávu v `Results` založené na vstup uživatele.

> [!NOTE]
> Samozřejmě, není třeba použít `FindControl` tak, aby odkazovaly ovládací prvky popisek a textové pole v tomto příkladu. Jsme může odkazovat na ně přímo prostřednictvím jejich `ID` hodnot vlastností. Používám `FindControl` zde k objasnění, co se stane při použití `FindControl` ze stránky obsahu.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

Při syntaxe používá k volání `FindControl` metoda se mírně liší v první dva řádky `SubmitButton_Click`, jsou sémanticky ekvivalentní. Odvolání, které zahrnují všechny serverových ovládacích prvků ASP.NET `FindControl` metoda. To zahrnuje `Page` třída, ze které všechny ASP.NET kódu třídy musí být odvozeny od. Proto volání `FindControl("controlID")` je ekvivalentní volání `Page.FindControl("controlID")`, za předpokladu, že nemají potlačena `FindControl` metodu v třídě kódu nebo ve vlastní základní třídy.

Po zadání tohoto kódu, přejděte `IDIssues.aspx` stránky prostřednictvím prohlížeče, zadejte svůj věk a klikněte na tlačítko "Odeslat". Po kliknutí na tlačítko "Odeslat" `NullReferenceException` se vyvolá (viz obrázek 5).


[![NullReferenceException je vyvolána.](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**Obrázek 05**: A `NullReferenceException` se vyvolá ([Kliknutím zobrazit obrázek v plné velikosti](control-id-naming-in-content-pages-cs/_static/image9.png))


Pokud nastavíte zarážky `SubmitButton_Click` obslužné rutiny události se zobrazí oba volá na `FindControl` vrátit `null` hodnotu. `NullReferenceException` Se vyvolá, když jsme se pokusí o přístup `Age` textové pole na `Text` vlastnost.

Problém je, že `Control.FindControl` pouze vyhledá *řízení*na podřízeného prvku, které jsou *ve stejném kontejneru pojmenování*. Vzhledem k hlavní stránce představují nové názvový kontejner, volání `Page.FindControl("controlID")` nikdy permeates objekt stránky předlohy `ctl00`. (Odkazuje zpět na obrázku 4 k zobrazení hierarchie ovládací prvek, který ukazuje `Page` objektu jako nadřazený objekt stránky předlohy `ctl00`.) Proto `Results` popisek a `Age` nebyly nalezeny textové pole a `ResultsLabel` a `AgeTextBox` přiřazené hodnoty `null`.

Existují dvě řešení pro tento problém: jsme přejít k podrobnostem, jeden názvový kontejner současně, do ovládacího prvku odpovídající; nebo můžete vytvořit vlastní `FindControl` metoda, která permeates pojmenování kontejnerů. Podívejme se na každý z těchto možností.

### <a name="drilling-into-the-appropriate-naming-container"></a>Procházení do příslušné pojmenování kontejneru

Použít `FindControl` k odkazu `Results` popisek nebo `Age` textovému poli, musíme volání `FindControl` z ovládacího prvku nadřazeného ve stejném pojmenování kontejneru. Jako na obrázku 4 vám ukázal, `MainContent` prvek ContentPlaceHolder je pouze nadřazeného `Results` nebo `Age` se v rámci stejného kontejneru názvů. Jinými slovy, volání `FindControl` metoda z `MainContent` řízení, jak je znázorněno v následujícím, fragmentu kódu správně vrátí odkaz na `Results` nebo `Age` ovládací prvky.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

Ale jsme nemůže pracovat s `MainContent` ContentPlaceHolder z naší obsahu stránce kódu třídy pomocí výše uvedené syntaxe, protože ContentPlaceHolder je definováno v stránky předlohy. Místo toho se musí použít `FindControl` získat odkaz na `MainContent`. Nahraďte kód v `SubmitButton_Click` obslužné rutiny události s těmito změnami:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

Pokud navštívíte stránku prostřednictvím prohlížeče, zadejte svůj věk a klikněte na tlačítko "Odeslat" `NullReferenceException` je vyvolána. Pokud nastavíte zarážky `SubmitButton_Click` uvidíte, že k této výjimce při pokusu o volání obslužné rutiny události `MainContent` objektu `FindControl` metoda. `MainContent` Objekt `null` protože `FindControl` metoda nelze najít objekt s názvem "MainContent". Základní důvod je stejný jako s `Results` popisek a `Age` ovládacích prvcích TextBox: `FindControl` spustí jeho vyhledávání od nejvyšší úrovně v hierarchii řízení a není vniknutí pojmenování kontejnerů, ale `MainContent` ContentPlaceHolder je v rámci stránky předlohy, což je názvový kontejner.

Před můžeme použít `FindControl` získat odkaz na `MainContent`, potřebujeme odkaz na ovládací prvek stránky předlohy. Jakmile odkaz na hlavní stránce jsme získat odkaz na `MainContent` ContentPlaceHolder prostřednictvím `FindControl` a z ní, odkazuje na `Results` popisek a `Age` TextBox (znovu, prostřednictvím `FindControl`). Ale jak jsme získat odkaz na hlavní stránce? Zkontrolováním `id` atributů v vykreslované značky je zřejmé, hlavní stránky `ID` hodnota je `ctl00`. Proto může používáme `Page.FindControl("ctl00")` k získání odkazu na stránku předlohy, potom použijte objekt získat odkaz na `MainContent`a tak dále. Následující fragment kódu ukazuje tuto logiku:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

Když tento kód bude určitě fungovat, předpokládá, že automaticky vygenerovanou stránky předlohy `ID` bude vždy `ctl00`. Nikdy je vhodné nastavit předpoklady o hodnotách generován automaticky.

Naštěstí je přístupná prostřednictvím odkaz na hlavní stránce `Page` třídy `Master` vlastnost. Proto místo nutnosti použít `FindControl("ctl00")` k získání odkazu stránky předlohy, aby měli přístup `MainContent` ContentPlaceHolder, můžete místo toho používáme `Page.Master.FindControl("MainContent")`. Aktualizace `SubmitButton_Click` obslužné rutiny události s následujícím kódem:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

Tentokrát na stránce prostřednictvím prohlížeče, zadáte svůj věk a kliknutím na tlačítko "Odeslat" zobrazí zprávu ve `Results` popisku podle očekávání.


[![Stáří uživatele se zobrazí v popisku](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**Obrázek 06**: stáří uživatele se zobrazí v popisku ([Kliknutím zobrazit obrázek v plné velikosti](control-id-naming-in-content-pages-cs/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>Rekurzivní hledání prostřednictvím pojmenování kontejnerů

Z důvodu předchozí příklad kódu odkazovat `MainContent` prvek ContentPlaceHolder na hlavní stránce, na a pak `Results` popisek a `Age` TextBox ovládací prvky z `MainContent`, je, protože `Control.FindControl` pouze vyhledá – metoda v rámci *řízení*je pojmenování kontejneru. S `FindControl` zůstat v rámci názvový kontejner dává smysl ve většině scénářů, protože dvou ovládacích prvků ve dvou různých pojmenování kontejnerů může mít stejný `ID` hodnoty. Podívejte se rutina GridView, který definuje ovládací prvek popisek Web s názvem `ProductName` do jedné z jeho TemplateFields. Pokud data je vázán na GridView za běhu, `ProductName` vytvořen popisek pro každý řádek GridView. Pokud `FindControl` vyhledávat prostřednictvím všechny názvy kontejnerů a jsme volají `Page.FindControl("ProductName")`, jaké instance popisek by měl `FindControl` vrátit? `ProductName` Popisek na prvním řádku GridView? Jeden na posledním řádku?

Proto s `Control.FindControl` vyhledávání právě *řízení*názvů prvku kontejneru smysl ve většině případů. Ale existují ostatních případech, jako je třeba čelí nám, kam máme jedinečný `ID` napříč všemi pojmenovávání kontejnerů a chcete vyhnout se nutnosti důkladně odkazovat na každý názvový kontejner v hierarchii ovládacích prvků pro přístup k ovládacím prvku. S `FindControl` variant, která rekurzivní hledání všechny kontejnery pojmenování umožňuje příliš smysl. Rozhraní .NET Framework bohužel neobsahuje tato metoda.

Dobrá zpráva je, že jsme můžete vytvořit vlastní `FindControl` metoda této rekurzivně prohledává všechny pojmenování kontejnerů. Ve skutečnosti pomocí *rozšiřující metody* jsme můžete tak `FindControlRecursive` metodu `Control` třídy, který může doprovázet jeho existující `FindControl` metoda.

> [!NOTE]
> Rozšiřující metody jsou funkcí nové C# 3.0 a Visual Basic 9, které jsou jazyky, které se dodávají spolu s rozhraní .NET Framework verze 3.5 a Visual Studio 2008. Stručně řečeno povolí rozšiřující metody pro vývojáře k vytvoření nové metody pro typ existující třída prostřednictvím speciální syntaxi. Další informace o této funkci užitečné, najdete v části Moje článku [rozšíření základní typ funkce s rozšiřující metody](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Create – metoda rozšíření, přidat nový soubor k `App_Code` složku s názvem `PageExtensionMethods.cs`. Přidání metody rozšíření s názvem `FindControlRecursive` , která má jako vstup `string` parametr s názvem `controlID`. Rozšiřující metody pro správně fungovat, je důležité, aby vlastní třídy a její rozšiřující metody označit `static`. Kromě toho musí přijmout všechny rozšiřující metody, jako jejich první parametr na objektu typu, na které se vztahuje metodě rozšíření a tento vstupní parametr musí začínat slovem `this`.

Přidejte následující kód, který `PageExtensionMethods.cs` soubor třídy k definování této třídy a `FindControlRecursive` metoda rozšíření:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

S tímto kódem na místě, vraťte se do `IDIssues.aspx` třída kódu a komentář aktuální stránky `FindControl` volání metody. Nahraďte volání `Page.FindControlRecursive("controlID")`. Co je úhledné o rozšiřující metody je, že se objeví přímo v rozevíracích seznamech IntelliSense. Jak je vidět na obrázku 7, když je typ stránky a poté stiskněte období, `FindControlRecursive` metoda je součástí IntelliSense rozevíracího seznamu společně s druhým `Control` metody třídy.


[![Rozšiřující metody jsou zahrnuty v IntelliSense rozevírací seznamy](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**Obrázek 07**: rozšiřující metody jsou zahrnuty v IntelliSense rozevírací seznamy ([Kliknutím zobrazit obrázek v plné velikosti](control-id-naming-in-content-pages-cs/_static/image15.png))


Zadejte následující kód do `SubmitButton_Click` obslužné rutiny události a otestovat ji na stránce, zadáte svůj věk a kliknutím na tlačítko "Odeslat". Jak je znázorněno na obrázku 6 zpět, výsledný výstup bude zpráva, "Můžete jsou stáří let!"


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> Protože rozšiřující metody jsou nové pro C# 3.0 a Visual Basic 9, pokud používáte Visual Studio 2005 metody rozšíření nelze použít. Místo toho bude nutné implementovat `FindControlRecursive` metodu v třídě pomocné rutiny. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) má takový příklad v jeho příspěvku na blogu [Maser stránek ASP.NET a `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Krok 4: Využití správný`id`atribut hodnota skriptu na straně klienta

Jak jsme uvedli v tomto kurzu úvod, je vykreslení ovládacího prvku webového `id` atribut se často používá v klientský skript pro prostřednictvím kódu programu odkazovat na konkrétní prvek HTML. Například následující JavaScript odkazuje na element HTML pomocí jeho `id` a potom zobrazí jeho hodnota modální se zprávou:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

Odvolání, že v ASP.NET stránky, která neobsahují názvový kontejner, vykreslovaného elementu HTML na `id` atribut je stejný jako ovládací prvek webu `ID` hodnotu vlastnosti. Z toho důvodu je tempting pevný kód v `id` hodnoty atributu do kódu jazyka JavaScript. To znamená, pokud víte, kterému chcete přistupovat `Age` ovládací prvek webu TextBox prostřednictvím skript na straně klienta, tak učinit prostřednictvím volání `document.getElementById("Age")`.

Problém s tímto přístupem je, že při použití stránky předlohy (nebo jiné pojmenování ovládací prvky kontejneru), vykreslené HTML `id` není shodný s ovládacího prvku webového `ID` vlastnost. Navštívit stránku prostřednictvím prohlížeče a zobrazte zdroj k určení skutečnou může být vaše první náklon `id` atribut. Znáte vygenerované `id` hodnotu, můžete vložit ji do volání `getElementById` pro přístup k prvku HTML, který potřebujete k práci s prostřednictvím klientský skript. Tento přístup je menší než ideální, protože některé změny na stránku řídit hierarchie nebo změny `ID` vlastností ovládacích prvků pojmenování změní, výsledná `id` atributů, a tím nejnovější kódu jazyka JavaScript.

Dobrá zpráva je, že `id` hodnota atributu, který je vykreslen je přístupný v kódu na straně serveru pomocí ovládacího prvku webového [ `ClientID` vlastnost](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). Tato vlastnost by měla použít k určení `id` atribut hodnoty používané ve skriptu na straně klienta. Chcete-li například přidat funkce jazyka JavaScript na stránku, při volání, zobrazuje hodnotu `Age` TextBox modální se zprávou, přidejte následující kód, který `Page_Load` obslužné rutiny události:


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

Ve výše uvedeném kódu vloží hodnota `Age` textové pole na vlastnost ClientID do volání jazyka JavaScript `getElementById`. Pokud najdete na této stránce přes prohlížeč a zobrazit zdrojový kód HTML, zjistíte následující kód v JavaScriptu:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

Všimněte si jak správný `id` hodnota atributu `ctl00_MainContent_Age`, zobrazí se v rámci volání `getElementById`. Protože tato hodnota se vypočítá za běhu, funguje bez ohledu na novější změny stránky hierarchii ovládacích prvků.

> [!NOTE]
> Tento příklad v jazyce JavaScript jenom ukazuje, jak přidat funkce JavaScript, která správně odkazuje na element HTML pro vykreslení ovládacího prvku serveru. Použití této funkce musíte vytvořit další JavaScript pro volání funkce při načtení dokumentu nebo při ukáže některé akce konkrétního uživatele. Další informace o tyto a související témata, přečtěte si [práce s klientský skript](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Souhrn

Některé ovládací prvky ASP.NET server fungovat jako pojmenování kontejnerů, jenž ovlivňuje vygenerované `id` atribut hodnoty jejich podřízené ovládací prvky, a také oboru canvassed pomocí ovládacích prvků `FindControl` metoda. S ohledem na hlavní stránky samotné stránky předlohy a jeho ovládacích prvků ContentPlaceHolder vytváření názvů kontejnerů. V důsledku toho je potřeba uvést stanovilo trochu další práci prostřednictvím kódu programu odkazovat ovládacích prvků v obsahu stránce pomocí `FindControl`. V tomto kurzu jsme se zaměřili na dvě techniky: vrtací do ovládacího prvku ContentPlaceHolder a volání jeho `FindControl` metoda; a vrácení vlastní `FindControl` implementace tohoto rekurzivně prohledává prostřednictvím všech pojmenování kontejnerů.

Na straně serveru problémy se pojmenování kontejnerů zavést s ohledem na odkazující na ovládací prvky webového existují také problémy na straně klienta. Chybí názvy kontejnerů ovládací prvek webu na `ID` hodnotu vlastnosti a vykresluje `id` hodnota atributu je jeden ve stejné. Ale s přidáním systému názvový kontejner, vygenerované `id` atribut zahrnuje i `ID` hodnoty ovládací prvek webu a pojmenování kontejnery v hierarchii řízení dědičností. Tyto problémy pojmenování jsou jiný problém, dokud pomocí ovládacího prvku webového `ClientID` vlastnosti k určení vygenerované `id` atribut hodnota ve vašem skriptu na straně klienta.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Stránky předlohy technologie ASP.NET a`FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Vytváření dynamických dat záznam uživatelského rozhraní](https://msdn.microsoft.com/library/aa479330.aspx)
- [Funkce rozšíření základní typ pomocí metody rozšíření](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Postupy: Odkaz na obsah stránky předlohy ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Stránky předlohy: Tipy, triky a depeše](http://www.odetocode.com/articles/450.aspx)
- [Práce s skript na straně klienta](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott lze dosáhnout za [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Zack Petr a Suchi Barnerjee. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Předchozí](urls-in-master-pages-cs.md)
[další](interacting-with-the-master-page-from-the-content-page-cs.md)
