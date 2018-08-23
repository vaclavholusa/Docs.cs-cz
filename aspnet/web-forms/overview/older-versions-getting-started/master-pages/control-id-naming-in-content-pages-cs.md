---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: Ovládací prvek ID pojmenování stránkách obsahu (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Ilustruje způsob ovládacích prvků ContentPlaceHolder sloužit jako pojmenování kontejnerů a proto ujistěte se, prostřednictvím kódu programu pracovat s ovládacím prvkem obtížné (prostřednictvím FindConrol)...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0c8617bb14c7023cfd926022b66c69bb5762758b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756849"
---
<a name="control-id-naming-in-content-pages-c"></a>ID ovládacího prvku pojmenování stránkách obsahu (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> Ilustruje způsob ovládacích prvků ContentPlaceHolder sloužit jako pojmenování kontejnerů a proto ujistěte se, prostřednictvím kódu programu pracovat s ovládacím prvkem obtížné (prostřednictvím FindConrol). Vyhledá v tomto problému a alternativní řešení. Také popisuje, jak programově přistupovat k výsledná hodnota ClientID.


## <a name="introduction"></a>Úvod

Zahrnout všechny serverové ovládací prvky technologie ASP.NET `ID` vlastnost, která jednoznačně identifikuje ovládací prvek a prostředek, ve které ovládací prvek programově přistupuje ve třídě použití modelu code-behind. Podobně může zahrnovat elementy v dokumentu HTML `id` atribut, který jednoznačně identifikuje prvek; tyto `id` hodnoty se často používají ve skriptu na straně klienta k prostřednictvím kódu programu odkazovat na konkrétní elementu HTML. To směru, je může předpokládá, že při vykreslování ovládacího prvku ASP.NET do kódu HTML, jeho `ID` hodnota se používá jako `id` hodnotu vykreslovaného elementu HTML. Toto není nutně případ vzhledem k tomu, že za určitých okolností jeden ovládací prvek pomocí jediného `ID` hodnota se může vyskytovat vícekrát vykreslované značky. Vezměte v úvahu ovládacího prvku GridView, která zahrnuje TemplateField s ovládacím prvkem webového popisek s `ID` hodnotu ProductName. Když prvku GridView je vázán na zdroj dat za běhu, tento popisek se opakuje jednou pro každý řádek prvku GridView. Každý popisek musí vykreslen jedinečný `id` hodnotu.

Technologie ASP.NET pro zpracování scénářů, umožňuje některé ovládací prvky, chcete-li označením jako pojmenování kontejnerů. Názvový kontejner slouží jako nový `ID` oboru názvů. Žádné serverové ovládací prvky, které se zobrazují v rámci názvový kontejner mají jejich vykreslené `id` předponu hodnotu `ID` pojmenování kontejneru ovládacího prvku. Například `GridView` a `GridViewRow` třídy jsou obě pojmenování kontejnerů. V důsledku toho ovládacího prvku popisku podle GridView TemplateField s `ID` ProductName dostane vykreslené `id` hodnotu `GridViewID_GridViewRowID_ProductName`. Protože *GridViewRowID* je jedinečný pro každý řádek prvku GridView, výsledná `id` jsou jedinečné hodnoty.

> [!NOTE]
> [ `INamingContainer` Rozhraní](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) se používá k označení, že konkrétní serverový ovládací prvek ASP.NET by mělo fungovat jako názvový kontejner. `INamingContainer` Rozhraní není pravopisu si jakékoli metody, které musí implementovat serverový ovládací prvek; místo toho se používá jako značku. Při generování vykreslované značky, pokud ovládací prvek implementuje toto rozhraní potom modul ASP.NET automaticky předpon jeho `ID` vykreslen hodnotu na jeho následovníky `id` hodnoty atributů. Tento proces je popsáno podrobněji v kroku 2.


Pojmenování kontejnerů nejenom změnit vygenerované `id` hodnotu atributu, ale také ovlivňují, jak ovládací prvek může prostřednictvím kódu programu odkazovat z třídy modelu code-behind stránky technologie ASP.NET. `FindControl("controlID")` Metoda běžně slouží jako odkaz prostřednictvím kódu programu webový ovládací prvek. Nicméně `FindControl` není proniknout prostřednictvím pojmenování kontejnerů. V důsledku toho nelze použít přímo `Page.FindControl` metoda odkazovat na ovládací prvky GridView nebo jiných názvový kontejner.

Jak vám může mít surmised, hlavní stránky a prvků ContentPlaceHolder jsou obě implementovány jako pojmenování kontejnerů. V tomto kurzu Zkoumáme, jak hlavní prvek HTML stránky vliv `id` hodnoty a způsoby, jak prostřednictvím kódu programu odkazovat webové ovládací prvky v rámci stránky obsahu pomocí `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Krok 1: Přidání nové stránky ASP.NET

Abychom si předvedli Principy probírané v tomto kurzu, přidejme novou stránku ASP.NET na našem webu. Vytvoření nové stránky obsahu s názvem `IDIssues.aspx` v kořenové složce vytvoříte jejich vazbu na `Site.master` stránky předlohy.


![Přidání obsahu stránky IDIssues.aspx ke kořenové složce](control-id-naming-in-content-pages-cs/_static/image1.png)

**Obrázek 01**: Přidat stránku obsahu `IDIssues.aspx` ke kořenové složce


Visual Studio automaticky vytvoří ovládací prvek obsahu pro všechny čtyři prvků ContentPlaceHolder na hlavní stránce. Jak je uvedeno v [ *několik prvků ContentPlaceHolder a výchozí obsah* ](multiple-contentplaceholders-and-default-content-cs.md) výukový program, pokud ovládací prvek obsahu není k dispozici je místo toho vygenerován obsah ContentPlaceHolder výchozí stránky předlohy. Vzhledem k tomu, `QuickLoginUI` a `LeftColumnContent` prvků ContentPlaceHolder obsahovat vhodné výchozí kód pro tuto stránku, pokračujte a odeberte odpovídající ovládací prvky obsahu z `IDIssues.aspx`. V tomto okamžiku deklarativním označení stránky obsahu by měl vypadat nějak takto:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

V [ *zadáním názvu, metaznaček a ostatní hlaviček HTML na stránce předlohy* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) kurzu jsme vytvořili vlastní stránku základní třídu (`BasePage`), který automaticky nakonfiguruje název stránky, pokud je není explicitně nastavena. Pro `IDIssues.aspx` stránce používat tuto funkci, na stránce použití modelu code-behind třída musí být odvozen od `BasePage` třídy (místo `System.Web.UI.Page`). Upravte definici třídy modelu code-behind tak, aby to vypadá takto:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

Nakonec aktualizujte `Web.sitemap` soubor zahrnout položku pro tento nový lekce. Přidat `<siteMapNode>` elementu a nastavte jeho `title` a `url` atributy "Ovládací prvek ID pojmenování problémy" a `~/IDIssues.aspx`v uvedeném pořadí. Po provedení tohoto přidání vašeho `Web.sitemap` souboru značek by měl vypadat nějak takto:


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

Jak znázorňuje obrázek 2, nová položka mapy webu v `Web.sitemap` se okamžitě projeví v lekcích části v levém sloupci.


![Poznatky oddílu teď obsahuje odkaz na &quot;pojmenování problémy s ID ovládacího prvku&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**Obrázek 02**: části lekce teď obsahuje odkaz na "ID ovládacího prvku pojmenování problémy s"


## <a name="step-2-examining-the-renderedidchanges"></a>Krok 2: Prozkoumání vygenerované`ID`změny

Pro lepší pochopení úpravy ASP.NET modul umožňuje vygenerované `id` hodnoty serveru řídí, přidáme několik ovládacích prvků na `IDIssues.aspx` stránce a potom si prohlédněte vykreslované značky odesláno prohlížeči. Konkrétně typ v textu "Zadejte prosím svůj věk:" následované ovládacího prvku TextBox webového. Další dolů na stránce přidejte ovládací prvek webového tlačítko a popisek webový ovládací prvek. Nastavit textové pole `ID` a `Columns` vlastností `Age` a 3, v uvedeném pořadí. Tlačítka nastavte `Text` a `ID` vlastnosti "Odeslat" a `SubmitButton`. Vymazání popisku `Text` vlastnost a nastavte jeho `ID` k `Results`.

Deklarativní obsahu ovládacího prvku v tomto okamžiku by měl vypadat nějak takto:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

Obrázek 3 ukazuje na stránku při zobrazit pomocí návrháře aplikace Visual Studio.


[![Stránka obsahuje tři ovládací prvky webové: textové pole, tlačítko a popisek](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**Obrázek 03**: obsahuje tři webové ovládací prvky stránky: textové pole, tlačítko a popisek ([kliknutím ji zobrazíte obrázek v plné velikosti](control-id-naming-in-content-pages-cs/_static/image5.png))


Na stránce prostřednictvím prohlížeče a pak zobrazit zdrojový kód HTML. Jako kód níže ukazuje `id` hodnoty prvků HTML pro textové pole, tlačítko a popisek webové ovládací prvky jsou kombinací `ID` hodnot ovládacích prvků webové a `ID` hodnoty pojmenování kontejnerů na stránce.


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

Jak je uvedeno výše v tomto kurzu, stránky předlohy a jeho prvků ContentPlaceHolder sloužit jako pojmenování kontejnerů. V důsledku toho, jak přispívat vygenerované `ID` hodnot jejich vnořené ovládacích prvků. Provést textového pole `id` atribut, například: `ctl00_MainContent_Age`. Vzpomeňte si, že ovládací prvek TextBox `ID` byla hodnota `Age`. To je s předponou prvku ContentPlaceHolder `ID` hodnotu `MainContent`. Kromě toho je tato hodnota předponu stránky předlohy `ID` hodnotu `ctl00`. Výsledkem je `id` hodnotu atributu, který se skládá z `ID` hodnoty hlavní stránky, ovládací prvek ContentPlaceHolder a samotného textového pole.

Obrázek 4 ukazuje toto chování. K určení vygenerované `id` z `Age` textového pole začíná `ID` hodnota ovládacího prvku TextBox `Age`. V dalším kroku nahlíželi hierarchii ovládacího prvku. Na každý názvový kontejner (ty uzly broskvoněmi barvou) předpony aktuální vykreslen `id` s názvový kontejner `id`.


![Atributy id přepuštěné jsou založené na ID hodnoty pojmenování kontejnerů](control-id-naming-in-content-pages-cs/_static/image6.png)

**Obrázek 04**: The vykreslen `id` atributy jsou založené na `ID` hodnoty pojmenování kontejnerů


> [!NOTE]
> Jak jsme probírali `ctl00` část vygenerované `id` představuje atribut `ID` hodnoty stránky předlohy, ale může zajímat, jak tento `ID` hodnotu přišel. Jsme nezadali ho kdekoli v naší hlavní nebo obsahu stránky. Většina serverové ovládací prvky na stránce ASP.NET jsou explicitně přidat prostřednictvím deklarativním označení stránky. `MainContent` Prvek ContentPlaceHolder byla explicitně zadána ve značkách `Site.master`; `Age` textového pole byl definován `IDIssues.aspx`vaší značky. Lze zadat `ID` hodnoty pro tyto typy ovládacích prvků v okně vlastností nebo z deklarativní syntaxe. Další ovládací prvky, jako jsou stránky předlohy, nejsou definovány v deklarativním označení. V důsledku toho jejich `ID` hodnoty musí být automaticky generovány pro nás. Modul sady ASP.NET `ID` hodnoty v době běhu pro tyto ovládací prvky, jejichž ID nebyly explicitně nastavena. Používá vzor pro pojmenování `ctlXX`, kde *XX* je postupně rostoucí celočíselnou hodnotu.


Vzhledem k tomu, že hlavní stránce slouží jako kontejneru, webové ovládací prvky definované v hlavní stránka také změnily vykreslené `id` hodnoty atributů. Například `DisplayDate` jsme přidali na stránku předlohy v popisku [ *vytvoření rozložení platného pro celý web pomocí stránek předlohy* ](creating-a-site-wide-layout-using-master-pages-cs.md) kurzu má následující vykreslení značek:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

Všimněte si, že `id` atribut obsahuje oba hlavní stránky `ID` hodnotu (`ctl00`) a `ID` ovládacím prvku popisek Web (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Krok 3: Programově odkazující na ovládací prvky webového prostřednictvím`FindControl`

Každý serverový ovládací prvek ASP.NET zahrnuje `FindControl("controlID")` metoda, která hledá následovníky ovládacího prvku pro ovládací prvek s názvem *controlID*. Pokud není nalezen takový ovládací prvek, je vrácena. Pokud není nalezen žádný odpovídající ovládací prvek, `FindControl` vrátí `null`.

`FindControl` je užitečné v situacích, kdy potřebujete přístup k ovládacímu prvku, ale nemáte přímý odkaz na něj. Při práci s daty webové ovládací prvky jako prvku GridView, například ovládací prvky v rámci prvku GridView pole jsou definována jednou v deklarativní syntaxe, ale za běhu je vytvořena instance ovládacího prvku pro každý řádek prvku GridView. V důsledku toho ovládací prvky, vygenerované za běhu existuje, ale nejsou k dispozici ze třídy modelu code-behind přímý odkaz. Díky tomu budeme muset použít `FindControl` programově pracovat s konkrétní ovládací prvek v rámci pole prvku GridView. (Další informace o používání `FindControl` přístup k ovládacím prvkům v rámci šablony řízení webových dat, najdete v článku [vlastní formátování založené na Data](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md).) Této stejné situaci dojde při dynamické přidání ovládacích prvků webového formuláře, téma se zabývá [vytváření dynamického uživatelské rozhraní položky dat](https://msdn.microsoft.com/library/aa479330.aspx).

Pro ilustraci použití `FindControl` metody na hledání pro ovládací prvky v rámci stránky obsahu, vytvořit obslužnou rutinu události pro `SubmitButton`společnosti `Click` událostí. V obslužné rutině události, přidejte následující kód, který se odkazuje prostřednictvím kódu programu `Age` textového pole a `Results` popisek pomocí `FindControl` metodu a poté zobrazí zprávu v `Results` na základě vstupu uživatele.

> [!NOTE]
> Samozřejmě, není potřeba použít `FindControl` odkazovat ovládací prvky popisku a textového pole v tomto příkladu. Společnost Microsoft může odkazovat přímo pomocí jejich `ID` hodnot vlastností. Můžu použít `FindControl` tady pro ilustraci, co se stane při použití `FindControl` z obsahu stránky.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

Zatímco syntaxi pro volání `FindControl` metody se mírně liší v první dva řádky `SubmitButton_Click`, jsou sémanticky ekvivalentní. Vzpomínáte, které zahrnují všechny serverové ovládací prvky technologie ASP.NET `FindControl` metoda. Jedná se o `Page` třída, ze které všechny technologie ASP.NET použití modelu code-behind třídy se musí odvozovat z. Proto volání `FindControl("controlID")` je ekvivalentní volání `Page.FindControl("controlID")`, za předpokladu, že nemají potlačena `FindControl` metody ve třídě použití modelu code-behind nebo vlastní základní třídy.

Jakmile zadáte tento kód, přejděte `IDIssues.aspx` stránce prostřednictvím prohlížeče, zadejte svůj věk a klikněte na tlačítko "Odeslat". Po kliknutí na tlačítko "Odeslat" `NullReferenceException` je vyvolána (viz obrázek 5).


[![Je aktivována NullReferenceException](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**Obrázek 05**: A `NullReferenceException` je vyvolána ([kliknutím ji zobrazíte obrázek v plné velikosti](control-id-naming-in-content-pages-cs/_static/image9.png))


Pokud nastavíte zarážku `SubmitButton_Click` uvidíte, že obě volání obslužné rutiny události `FindControl` vrátit `null` hodnotu. `NullReferenceException` Se vyvolá, když jsme pokus o přístup `Age` textového `Text` vlastnost.

Problém je, že `Control.FindControl` hledá jenom v *ovládací prvek*jeho následovníky, které jsou *ve stejném pojmenování kontejneru*. Protože stránky předlohy se považuje za nový názvový kontejner volání `Page.FindControl("controlID")` nikdy permeates objekt stránky předlohy `ctl00`. (Vrátit zpět k obrázek 4, chcete-li zobrazit hierarchii ovládací prvek, který ukazuje `Page` objektů jako nadřazený objekt stránky předlohy `ctl00`.) Proto `Results` popisek a `Age` textového pole nebyly nalezeny a `ResultsLabel` a `AgeTextBox` jsou přiřazeny hodnoty `null`.

Existují dvě alternativní řešení na tuto výzvu: můžeme přejít k podrobnostem, jeden názvový kontejner v době, na odpovídající ovládací prvek; nebo můžete vytvořit vlastní `FindControl` metodu, která permeates pojmenování kontejnerů. Podívejme se na každou z těchto možností.

### <a name="drilling-into-the-appropriate-naming-container"></a>Při bližším pohledu odpovídající pojmenování kontejneru

Použití `FindControl` na odkaz `Results` popisek nebo `Age` textové pole, musíme volání `FindControl` z ovládacího prvku předchůdce ve stejném pojmenování kontejneru. Jako obrázek 4 jsme si ukázali, `MainContent` je ovládací prvek ContentPlaceHolder pouze nadřazeného člena pro `Results` nebo `Age` , který je v rámci stejného pojmenování kontejneru. Jinými slovy, volání `FindControl` metodu z `MainContent` ovládacího prvku, jak je znázorněno v následujícím fragmentu kódu správně vrátí odkaz na `Results` nebo `Age` ovládacích prvků.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

Však nelze spolupracujeme se `MainContent` ContentPlaceHolder z našich obsahu stránky použití modelu code-behind třídy pomocí výše uvedené syntaxi, protože je definována ContentPlaceHolder na stránce předlohy. Místo toho, musíme použít `FindControl` k získání odkazu na `MainContent`. Nahraďte kód v `SubmitButton_Click` obslužné rutiny události s těmito změnami:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

Pokud navštívíte stránku prostřednictvím prohlížeče, zadejte svůj věk a klikněte na tlačítko "Odeslat" `NullReferenceException` je vyvolána. Pokud nastavíte zarážku `SubmitButton_Click` uvidíte, že dojde k této výjimce při pokusu o volání obslužné rutiny události `MainContent` objektu `FindControl` metody. `MainContent` Objekt `null` protože `FindControl` metody nelze najít objekt s názvem "MainContent". Základní příčina toho je stejná jako u `Results` popisek a `Age` TextBox – ovládací prvky: `FindControl` spustí vyhledávání od horního okraje ovládacího prvku hierarchii a ne proniknout pojmenování kontejnerů, ale `MainContent` ContentPlaceHolder je v rámci stránky předlohy, což je názvový kontejner.

Dřív, než můžete `FindControl` k získání odkazu na `MainContent`, potřebujeme odkaz na ovládací prvek hlavní stránky. Jakmile budeme mít odkaz na hlavní stránku jsme získat odkaz na `MainContent` ContentPlaceHolder prostřednictvím `FindControl` a z něj, odkazy na `Results` popisek a `Age` textového pole (znovu, až pomocí `FindControl`). Ale jak jsme získat odkaz na hlavní stránku? Zkontrolováním `id` atributy v vykreslované značky je zřejmé, která na hlavní stránce `ID` hodnotu `ctl00`. Proto bychom mohli použít `Page.FindControl("ctl00")` získáte odkaz na stránku předlohy, pak použít tento objekt k získání odkazu na `MainContent`, a tak dále. Následující fragment kódu ukazuje tuto logiku:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

Přestože tento kód bude jistě pracovat, předpokládá, že automaticky generované stránky předlohy `ID` bude vždy `ctl00`. Nikdy je vhodné vytvořit předpoklady o automaticky generované hodnoty.

Naštěstí je přístupný prostřednictvím odkazu na stránce předlohy `Page` třídy `Master` vlastnost. Proto se místo nutnosti použít `FindControl("ctl00")` získáte odkaz na hlavní stránce za účelem přístupu k `MainContent` ContentPlaceHolder, můžeme místo toho použít `Page.Master.FindControl("MainContent")`. Aktualizace `SubmitButton_Click` obslužné rutiny události s následujícím kódem:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

Tentokrát, na stránce v prohlížeči zadáním váš věk a kliknutím na tlačítko "Odeslat" zobrazí zprávu v `Results` popiskem nebo podle očekávání.


[![Věk uživatele je zobrazený v popisku](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**Obrázek 06**: věku The uživatele je zobrazený v popisku ([kliknutím ji zobrazíte obrázek v plné velikosti](control-id-naming-in-content-pages-cs/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>Rekurzivně prohledávat pojmenování kontejnerů

Z důvodu předchozí příklad kódu odkazovat `MainContent` prvek ContentPlaceHolder na hlavní stránce a pak `Results` popisek a `Age` textového pole ovládacích prvků z `MainContent`, totiž `Control.FindControl` hledání – metoda v rámci *ovládací prvek*názvů prvku kontejneru. S `FindControl` zůstat v rámci názvový kontejner dává smysl ve většině scénářů, protože dva ovládací prvky ve dvou různých pojmenování kontejnerů může mít stejný `ID` hodnoty. Podívejte se na prvku GridView, který definuje ovládacího prvku popisku Web s názvem `ProductName` v jednom z jeho vlastností TemplateField. Když vázaná k prvku GridView v době běhu `ProductName` popisek se vytvoří pro každý řádek prvku GridView. Pokud `FindControl` prohledána prostřednictvím všech pojmenování kontejnerů a budeme volat `Page.FindControl("ProductName")`, jaký popisek instance by měl `FindControl` vrátit? `ProductName` Popisek v prvním řádku prvku GridView? Je na posledním řádku?

Proto s `Control.FindControl` hledat jenom *ovládací prvek*názvů prvku kontejneru dává smysl ve většině případů. Ale existují ostatních případech, jako je třeba protilehlé nám, kde máme jedinečné `ID` napříč všemi pojmenování kontejnerů a chcete se vyhnout se tak nutnosti pečlivě odkazovat na každý názvový kontejner v hierarchii ovládacích prvků pro přístup k ovládacímu prvku. S `FindControl` variantu, která vyhledá rekurzivně všechny pojmenování kontejnerů díky moc smysl. Bohužel součástí rozhraní .NET Framework není tato metoda.

Dobrou zprávou je, že můžeme vytvořit vlastní `FindControl` metody tohoto rekurzivně prohledává všechny pojmenování kontejnerů. Ve skutečnosti pomocí *rozšiřující metody* jsme můžete tak `FindControlRecursive` metodu `Control` třídy vyvíjený existujících `FindControl` – metoda.

> [!NOTE]
> Rozšiřující metody jsou funkce, která nové 3.0 jazyka C# a Visual Basic 9, které jsou jazyky, které se dodávají pomocí rozhraní .NET Framework verze 3.5 a Visual Studio 2008. Stručně řečeno rozšiřující metody umožňují vývojář, abyste mohli vytvořit novou metodu pro existující typ třídy pomocí speciální syntaxe. Další informace o této užitečné funkce, najdete v mé článku [rozšíření základní typ funkce se rozšiřující metody](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Vytvořit metodu rozšíření, přidejte nový soubor, který `App_Code` složku s názvem `PageExtensionMethods.cs`. Přidat metodu rozšíření s názvem `FindControlRecursive` , která přijímá jako vstup `string` parametr s názvem `controlID`. Metody rozšíření fungovalo správně, je důležité, že vlastní třídy a metody rozšíření označit `static`. Kromě toho musíte přijmout všechny metody rozšíření, jako objekt typu, ke kterému se vztahuje rozšiřující metoda jejich první parametr a tento vstupní parametr musí před sebou mít s klíčovým slovem `this`.

Přidejte následující kód, který `PageExtensionMethods.cs` soubor třídy této třídy a `FindControlRecursive` – metoda rozšíření:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

S tímto kódem na místě, vraťte se do `IDIssues.aspx` použití modelu code-behind třídy a Odkomentujte aktuální stránky `FindControl` volání metody. Nahraďte volání `Page.FindControlRecursive("controlID")`. Co je úhledné o metodách rozšíření je, že se zobrazí přímo v rozevíracích seznamech technologie IntelliSense. Jak je vidět na obrázku 7, po zadání stránky a pak klikněte na tlačítko období, `FindControlRecursive` metoda je součástí technologie IntelliSense rozevíracího seznamu spolu s druhou `Control` metody třídy.


[![Rozšiřující metody jsou zahrnuty v IntelliSense rozevíracích](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**Obrázek 07**: metody rozšíření jsou zahrnuty v IntelliSense rozevíracích ([kliknutím ji zobrazíte obrázek v plné velikosti](control-id-naming-in-content-pages-cs/_static/image15.png))


Zadejte následující kód do `SubmitButton_Click` obslužné rutiny události a pak ho otestujte na stránce, zadáte svůj věk a kliknutím na tlačítko "Odeslat". Jak je znázorněno na obrázku 6 zpět, výsledný výstup bude zpráva, "Jste stáří let!"


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> Protože rozšiřující metody jsou nové 3.0 jazyka C# a Visual Basic 9, pokud používáte Visual Studio 2005 nelze použít rozšiřující metody. Místo toho budete muset implementovat `FindControlRecursive` metody ve třídě pomocné rutiny. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) má takových příkladů v jeho blogovém příspěvku [Maser stránek ASP.NET a `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Krok 4: Využití správné`id`atribut hodnoty ve skriptu na straně klienta

Jak jsme uvedli v úvodu tohoto kurzu, je webový ovládací prvek vykreslen `id` atribut se často používá ve skriptu na straně klienta na prostřednictvím kódu programu odkazovat na konkrétní elementu HTML. Například následující JavaScript odkazuje na element HTML pomocí jeho `id` a potom zobrazí jeho hodnota modální okno se zprávou:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

Vzpomínáte, že v ASP.NET, které stránky nezahrnujte pojmenování kontejneru, vykreslovaného elementu HTML `id` atribut je stejný jako ovládací prvek webového `ID` hodnotu vlastnosti. Z toho důvodu je lákavé pevný kód v `id` hodnoty atributů do kódu jazyka JavaScript. To znamená, pokud víte, kterému chcete získat přístup `Age` ovládací prvek TextBox webu pomocí skriptu na straně klienta, to udělat prostřednictvím volání `document.getElementById("Age")`.

Problém s tímto přístupem je, že při použití stránky předlohy (nebo jiné ovládací prvky pojmenování kontejneru), zobrazený HTML `id` není shodný s ovládacím prvkem webového `ID` vlastnost. První náklon může být na stránce přes prohlížeč a zobrazit zdroj k určení skutečné `id` atribut. Když víte, vykreslený `id` hodnotu, můžete ho vložit do volání `getElementById` pro přístup k prvku HTML, budete potřebovat pro práci s pomocí skriptu na straně klienta. Tento přístup je méně vhodné, protože některé změny na stránku řízení hierarchie nebo změny `ID` vlastností ovládacích prvků pojmenování změní, výsledná `id` atribut, a tím zásadní kódu JavaScriptu.

Dobrou zprávou je, že `id` hodnotu atributu, který je vykreslen je přístupná v kódu na straně serveru pomocí ovládacího prvku webového [ `ClientID` vlastnost](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). Tuto vlastnost měli použít k určení `id` atribut hodnoty používané ve skriptu na straně klienta. Například pro funkce jazyka JavaScript přidejte na stránku, při volání, zobrazuje hodnotu `Age` TextBox modální okno se zprávou, přidejte následující kód, který `Page_Load` obslužné rutiny události:


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

Výše uvedený kód vkládá hodnoty `Age` textové pole pro vlastnost ClientID do volání JavaScript `getElementById`. Pokud tuto stránku prostřednictvím prohlížeče a zobrazit zdrojový kód HTML, najdete následující kód jazyka JavaScript:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

Všimněte si, že jak správné `id` hodnotu, atribut `ctl00_MainContent_Age`, zobrazí se v rámci volání `getElementById`. Protože tato hodnota se počítá za běhu, funguje bez ohledu na novější změny v hierarchii ovládacích prvků stránky.

> [!NOTE]
> Tento příklad v jazyce JavaScript pouze ukazuje, jak přidat funkce JavaScriptu, která správně odkazuje na element HTML, který je vykreslen metodou serverový ovládací prvek. Použití této funkce je třeba vytvořit další jazyka JavaScript pro volání funkce po načtení dokumentu nebo když ukáže některá z akcí konkrétního uživatele. Další informace na tyto a související témata, přečtěte si [práce pomocí skriptu na straně klienta](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Souhrn

Některé serverové ovládací prvky ASP.NET fungují jako kontejnery pojmenování, což ovlivňuje vygenerované `id` atribut hodnoty jejich podřízené ovládací prvky, stejně jako obor canvassed pomocí ovládacích prvků `FindControl` metody. S ohledem na stránky předlohy jsou samotné stránky předlohy a jejích ovládacích prvků ContentPlaceHolder pojmenování kontejnerů. V důsledku toho musíme vložit vpřed o něco více práce prostřednictvím kódu programu odkazovat na ovládací prvky v rámci stránky obsahu pomocí `FindControl`. V tomto kurzu jsme se zaměřili na dva postupy: procházení do ovládacího prvku ContentPlaceHolder a volání jeho `FindControl` metoda; a Hromadná našim vyrovnají `FindControl` implementace tohoto rekurzivně prohledává všechny pojmenování kontejnerů.

Kromě zavést pojmenování kontejnerů s ohledem na odkazující na ovládací prvky webového problémy na straně serveru dojde k problémům také na straně klienta. Chybí názvy kontejnerů, ovládací prvek na `ID` hodnotu vlastnosti a vykreslí `id` hodnota atributu jsou ve stejné. Ale přidání názvový kontejner, vygenerované `id` obsahuje atribut `ID` hodnoty ovládacího prvku webového a pojmenování kontejnerů v hierarchii řízení původ. Tyto zásady se týká jsou není problém, dokud používáte ovládací prvek webové `ClientID` a určí vygenerované `id` atribut hodnoty ve skriptu na straně klienta.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Stránek předloh ASP.NET a `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Vytváření dynamických dat záznam uživatelského rozhraní](https://msdn.microsoft.com/library/aa479330.aspx)
- [Rozšíření funkcí základní typ pomocí metody rozšíření](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Postupy: Referenční obsah stránky předlohy technologie ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Stránky předlohy: Tipy, triky a depeše](http://www.odetocode.com/articles/450.aspx)
- [Práce s skriptu na straně klienta](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více ASP/ASP.NET knih a Zakladatel 4GuysFromRolla.com pracuje s Microsoft webových technologií od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott může být dostupný na adrese [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Zack Jones a Suchi Barnerjee. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](urls-in-master-pages-cs.md)
> [další](interacting-with-the-master-page-from-the-content-page-cs.md)
