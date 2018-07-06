---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Stránky předlohy a ASP.NET AJAX (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Tento článek popisuje možnosti použití technologie ASP.NET AJAX a stránky předlohy. Vyhledá v horizontálních oddílů pomocí třídy ScriptManagerProxy; Popisuje, jak různé soubory JS načtou dependi...
ms.author: aspnetcontent
ms.date: 07/11/2008
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: e8a4f9780b41c5ff77b996894d9f91a532877245
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842481"
---
<a name="master-pages-and-aspnet-ajax-c"></a>Stránky předlohy a ASP.NET AJAX (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Tento článek popisuje možnosti použití technologie ASP.NET AJAX a stránky předlohy. Vyhledá v horizontálních oddílů pomocí třídy ScriptManagerProxy; Tento článek popisuje, jak různé soubory JS jsou načteny v závislosti na tom, zda prvek ScriptManager se používají v hlavní stránku nebo stránky obsahu.


## <a name="introduction"></a>Úvod

Během posledních několika let bylo vytváření více a více vývojářů [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-webových aplikacích s povoleným. Na webu s povoleným AJAX používá řadu související technologie nabízí a rychleji reagující uživatelské prostředí. Vytváření aplikací s povoleným AJAX technologie ASP.NET je neuvěřitelně jednoduché díky Microsoftu [technologie ASP.NET AJAX framework](../../../../ajax/index.md). ASP.NET 3.5 a Visual Studio 2008; je součástí technologie ASP.NET AJAX je také dostupné jako samostatný soubor ke stažení pro aplikace ASP.NET 2.0.

Při sestavování s povoleným AJAX webové stránky s použitím rozhraní ASP.NET AJAX framework, je třeba přidat přesně jeden [ovládací prvek ScriptManager](https://msdn.microsoft.com/library/bb398863.aspx) pro každou jednotlivou stránku, která používá rozhraní framework. Jak již název napovídá, spravuje ScriptManager skript na straně klienta používá v webové stránky s povoleným AJAX. Minimálně ScriptManager emituje kód HTML, který dává pokyn prohlížeče a stáhněte soubory jazyka JavaScript tuto strukturu Klientská knihovna ASP.NET AJAX. To lze také zaregistrovat vlastní soubory jazyka JavaScript povolen skript webových služeb, funkcí a vlastní aplikace služby.

Pokud web používá hlavní stránky (jako je), nepotřebujete nutně přidání ovládacího prvku ScriptManager pro každou jednotlivou stránku obsahu; Místo toho můžete přidat ovládací prvek ScriptManager na stránce předlohy. Tento kurz ukazuje, jak přidat ovládací prvek ScriptManager na stránce předlohy. Dohlíží taky na tom, jak pomocí ovládacího prvku ScriptManagerProxy registrovat vlastní skripty a skript služby na konkrétní stránce obsahu.

> [!NOTE]
> V tomto kurzu není zkoumání, návrh nebo vytváření s povoleným AJAX webových aplikací pomocí ASP.NET AJAX framework. Další informace o používání jazyka AJAX [videa technologie ASP.NET AJAX](../../../videos/aspnet-ajax/index.md) a [kurzy](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md), stejně jako tyto prostředky uvedené v části Další čtení na konci tohoto kurzu.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Zkoumání kódu, protože ho vygeneroval ovládací prvek správce skriptů

Ovládací prvek ScriptManager generuje kód, který dává pokyn prohlížeče a stáhněte soubory jazyka JavaScript tuto strukturu Klientská knihovna ASP.NET AJAX. Přidá také bit vložený kód JavaScript na stránce, která inicializuje tuto knihovnu. Následující kód zobrazí obsah, který je přidán do stránky, která obsahuje ovládací prvek ScriptManager vykresleného výstupu:


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

`<script src="url"></script>` Značky dáte pokyn, aby stahoval a spouštěl souboru jazyka JavaScript v prohlížeči *url*. Prvek ScriptManager vysílá tři tyto značky; jeden odkazuje na soubor `WebResource.axd`, ale další dvě jako odkaz na soubor `ScriptResource.axd`. Tyto soubory neexistují ve skutečnosti jako soubory ve vašem webu. Místo toho dorazí požadavek pro jednu z těchto souborů na webovém serveru modul ASP.NET zkontroluje řetězec dotazu a vrátí odpovídající Javascriptového obsahu. Skript poskytuje tyto tři externí soubory JavaScriptu představují Klientská knihovna ASP.NET AJAX framework. Druhý `<script>` značky, protože ho vygeneroval prvek ScriptManager zahrnout vloženého skriptu, který inicializuje tuto knihovnu.

Odkazy na externí skript a zpracování vloženého skriptu, protože ho vygeneroval ScriptManager jsou nezbytné pro stránku, která používá rozhraní ASP.NET AJAX framework, ale není potřeba pro stránky, které nepoužívají rozhraní framework. Proto může být důvod, že je ideální pro ovládací prvek ScriptManager přidat pouze na tyto stránky, které používají rozhraní ASP.NET AJAX framework. To je dostatečné, ale pokud máte mnoho stránek, které používají rozhraní budete mít nakonec přidání ovládacího prvku ScriptManager na všechny stránky – opakované úlohy, Řekněme, že nejméně. Alternativně můžete přidat ovládací prvek ScriptManager na hlavní stránku, která pak vloží tento nezbytné skript do všech obsahu stránek. Díky tomuto přístupu není potřeba nezapomeňte přidat ovládací prvek ScriptManager na novou stránku, která používá rozhraní ASP.NET AJAX framework, protože je již zahrnut na hlavní stránce. Krok 1 procházení prostřednictvím přidání ovládacího prvku ScriptManager na hlavní stránku.

> [!NOTE]
> Pokud plánujete včetně funkcí AJAX v rámci uživatelského rozhraní stránky předlohy, můžete se v této věci – musí obsahovat prvek ScriptManager na stránce předlohy.


Přidání ScriptManager na hlavní stránku jeden nevýhodou je, že skript výše je vygenerován v *každý* stránky, bez ohledu na to, jestli se jeho potřebné. To jasně vede k nevyužité šířku pásma pro stránky, které mají prvek ScriptManager zahrnout (prostřednictvím stránky předlohy), ale nepoužívejte žádné funkce rozhraní ASP.NET AJAX framework. Ale stejně množství nevyužité šířku pásma?

- Skutečný obsah, protože ho vygeneroval ScriptManager (popsaný výš) Celkový počet zpracovaných položek trochu více než 1KB.
- Tři soubory externího skriptu odkazovat `<script>` elementu, ale tvoří přibližně 450 KB dat, nekomprimovaný; na webu, který používá kompresi gzip, je možné snížit Tato celková šířka pásma téměř 100 KB. Ale tyto soubory skriptu jsou uložené v mezipaměti v prohlížeči dobu jednoho roku, což znamená, že potřebují pouze stahuje jenom jednou a potom je možné využít v ostatních v lokalitě.

V nejlepším případě pak soubory skriptů jsou uložené v mezipaměti, celkové náklady při 1KB, tedy zanedbatelné. V nejhorším případě ale – tedy pokud soubory skriptů ještě nebyly staženy a webový server nepoužívá žádnou formu komprese - přístupů šířky pásma je přibližně 450 KB, které můžete přidat kamkoli z sekundy či dvou přes širokopásmové připojení k až minutu pro  Uživatelé přes modemem. Dobrou zprávou je, že vzhledem k tomu, že soubory externích skriptů jsou uložené v mezipaměti v prohlížeči, nejhorší případ k této situaci dochází zřídka.

> [!NOTE]
> Pokud jste pořád podezřelé umístění ovládacího prvku ScriptManager na stránce předlohy, vezměte v úvahu webový formulář ( `<form runat="server">` značky na stránce předlohy). Každé stránky ASP.NET, která používá postback model musí obsahovat přesně jeden webový formulář. Přidání webového formuláře přidá další obsah: číslo skrytého pole `<form>` označení, a v případě potřeby funkci jazyka JavaScript za inicializaci zpětné volání ze skriptu. Tento kód je zbytečné pro stránky, které nechcete odeslat zpět. Tento cizí kód by mohl být odstraněny odebráním webový formulář ze stránky předlohy a ručně ho přidat na každou stránku obsahu, které je potřeba. Ale o výhodách webového formuláře na stránce předlohy převažují nad nevýhody nemusíte ho zbytečně přidána do určité obsahu stránky.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Krok 1: Přidání ovládacího prvku ScriptManager na stránku předlohy

Každé webové stránky, která používá rozhraní ASP.NET AJAX framework musí obsahovat přesně jeden ovládací prvek ScriptManager. Kvůli tomuto požadavku je obvykle vhodné umístit jeden ovládací prvek ScriptManager na stránce předlohy tak, aby všechny stránky obsahu ovládacího prvku ScriptManager automaticky zahrnuty. Kromě toho ScriptManager musí předcházet všechny technologie ASP.NET AJAX serverové ovládací prvky, jako je například ovládací prvky UpdatePanel a UpdateProgress. Proto je vhodné umístit ScriptManager dříve než kterýkoli prvek ContentPlaceHolder v rámci webového formuláře.

Otevřít `Site.master` stránku předlohy a přidání ovládacího prvku ScriptManager na stránku v rámci webového formuláře, ale předtím, než `<div id="topContent">` – element (viz obrázek 1). Pokud používáte aplikaci Visual Web Developer 2008 nebo Visual Studio 2008, ovládací prvek ScriptManager se nachází na panelu nástrojů na kartě Rozšíření AJAX. Pokud používáte Visual Studio 2005, je potřeba nejprve nainstalujte rozhraní framework ASP.NET AJAX a přidání ovládacích prvků do panelu nástrojů. Přejděte [technologie ASP.NET AJAX Wiki](https://github.com/DevExpress/AjaxControlToolkit/wiki) získat rozhraní pro technologii ASP.NET 2.0.

Po přidání ScriptManager na stránku, změnit její `ID` z `ScriptManager1` k `MyManager`.


[![Přidat prvek ScriptManager na stránku předlohy](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Obrázek 01**: prvek ScriptManager přidat na stránku předlohy ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Krok 2: Použití technologie ASP.NET AJAX Framework z obsahu stránky

Pomocí ovládacího prvku ScriptManager přidat na stránku předlohy jsme nyní můžete přidat funkce rozhraní framework ASP.NET AJAX na libovolnou stránku obsahu. Vytvoříme novou stránku ASP.NET, která zobrazuje namátkou vybraného produktu z databáze Northwind. Rozhraní ASP.NET AJAX – ovládací prvek časovače použijeme k aktualizaci zobrazení každých 15 sekund, zobrazuje nový produkt.

Začněte tím, že vytvoříte novou stránku v kořenovém adresáři s názvem `ShowRandomProduct.aspx`. Nezapomeňte vytvořit vazbu na tuto novou stránku `Site.master` stránky předlohy.


[![Přidejte novou stránku ASP.NET na web](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Obrázek 02**: přidejte novou stránku ASP.NET na web ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-asp-net-ajax-cs/_static/image6.png))


Připomínáme, že v [ *zadáním názvu, metaznaček a ostatní hlaviček HTML na stránce předlohy* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) kurzu jsme vytvořili vlastní stránku základní třídu s názvem `BasePage` , který v případě, že byla vygenerována název stránky není explicitně nastavena. Přejděte `ShowRandomProduct.aspx` kódu stránky třídy a nechat ji odvodit z `BasePage` (místo z `System.Web.UI.Page`).

Nakonec aktualizujte `Web.sitemap` soubor zahrnout položku pro tento účel. Přidejte následující kód pod `<siteMapNode>` pro hlavní lekce interakce obsahu stránky:


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

Přidání tohoto `<siteMapNode>` element se projeví v lekcí seznamu (viz obrázek 5).

### <a name="displaying-a-randomly-selected-product"></a>Zobrazení namátkou vybraného produktu

Vraťte se na `ShowRandomProduct.aspx`. Z návrháře, přetáhněte z panelu nástrojů do ovládacího prvku UpdatePanel `MainContent` ovládacího prvku obsahu a nastavte jeho `ID` vlastnost `ProductPanel`. Prvku UpdatePanel představuje oblast na obrazovce, která je možné aktualizovat asynchronně prostřednictvím částečná stránka postback.

Naše prvního úkolu je pro zobrazení informací o namátkou vybraného produktu v rámci prvku UpdatePanel. Začněte tím, že přetáhnete do prvku UpdatePanel ovládacího prvku DetailsView. Nastavení ovládacího prvku DetailsView `ID` vlastnost `ProductInfo` a vymažte její `Height` a `Width` vlastnosti. Rozbalte ovládacím prvku DetailsView inteligentních značek a z rozevíracího seznamu zvolit zdroj dat, vyberte možnost vytvoření vazby mezi ovládacím prvku DetailsView. nový ovládací prvek SqlDataSource s názvem `RandomProductDataSource`.


[![Svázat s novou ovládacím prvkem SqlDataSource ovládacím prvku DetailsView.](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Obrázek 03**: svázat ovládacím prvku DetailsView nové ovládacím prvkem SqlDataSource ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-asp-net-ajax-cs/_static/image9.png))


Konfigurace pro připojení k databázi Northwind pomocí ovládacího prvku SqlDataSource `NorthwindConnectionString` (které jsme vytvořili v [ *interakce stránky obsahu se stránkou předlohy* ](interacting-with-the-content-page-from-the-master-page-cs.md) kurzu). Při konfiguraci příkazu select vyberte vlastní příkaz jazyka SQL a pak zadejte následující dotaz:


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

`TOP 1` – Klíčové slovo v `SELECT` klauzule vrátí jenom první záznam vrácených dotazem. [ `NEWID()` Funkce](https://msdn.microsoft.com/library/ms190348.aspx) vygeneruje nový [hodnoty globálně jedinečného identifikátoru (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) a je možné v `ORDER BY` klauzule vrátí v tabulce záznamy v náhodném pořadí.


[![Konfigurace ve třídě SqlDataSource k vrácení jednoho, namátkou vybraného záznamu](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Obrázek 04**: konfigurace ve třídě SqlDataSource k vrácení jednoho náhodně vybraný záznam ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-asp-net-ajax-cs/_static/image12.png))


Po dokončení průvodce se sada Visual Studio vytvoří vlastnost BoundField pro dva sloupce vrácené dotazem výše. Na stránce deklarativní v tuto chvíli by měl vypadat nějak takto:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

Obrázek 5 ukazuje, `ShowRandomProduct.aspx` stránce při prohlížení prostřednictvím prohlížeče. Klikněte na tlačítko Aktualizovat v prohlížeči k opětovnému načtení stránky. měli byste vidět `ProductName` a `UnitPrice` hodnoty nového namátkou vybraného záznamu.


[![Zobrazí se název náhodných produktu a cena](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Obrázek 05**: Zobrazí se název a cenu A náhodné produktu ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Automaticky zobrazení nového produktu každých 15 sekund

ASP.NET AJAX framework obsahuje ovládací prvek časovače, který provede zpětné volání v určitou dobu; na odeslat zpět časovače `Tick` událost se vyvolá. Pokud je ovládací prvek časovače je umístěn v rámci ovládacího prvku UpdatePanel aktivuje částečná stránka zpětné volání, během kterého jsme obnovení vazby dat k ovládacím prvku DetailsView. pro zobrazení nového namátkou vybraného produktu.

K dosažení tohoto časovače přetáhněte z panelu nástrojů a umístěte jej do prvku UpdatePanel. Změnit časovače `ID` z `Timer1` k `ProductTimer` a jeho `Interval` vlastnost z 60000 na 15 000. `Interval` Vlastnost označuje počet milisekund mezi jednotlivými zpětnými odesláními; nastavení na 15 000 způsobí, že aby se časovač spustily částečná stránka každých 15 sekund. Deklarativní časovače v tomto okamžiku by měl vypadat nějak takto:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Vytvořte obslužnou rutinu události pro časovače `Tick` událostí. V této obslužné rutiny události potřebujeme rebind data do ovládacího prvku DetailsView voláním ovládacím prvku DetailsView `DataBind` metody. To dává pokyn ovládacím prvku DetailsView znovu načíst data ze svých dat správy zdrojového kódu, který bude vybrat a zobrazit novou náhodně vybrali záznam (stejně jako při znovu načíst tuto stránku kliknutím na tlačítko pro aktualizaci prohlížeče).


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

To je všechno je to! Otevírat stránku prostřednictvím prohlížeče. Na začátku se zobrazí informace o náhodných produktu. Pokud dokončení sledovat obrazovky můžete si všimnout, že po 15 sekundách, informace o nový produkt kouzelného nahradí stávající zobrazení.

Zobrazíte lépe čemu tady dochází, přidáme UpdatePanel, která zobrazuje čas, kdy došlo k poslední aktualizaci zobrazení ovládacího prvku popisku. Přidejte popisek webového ovládacího prvku UpdatePanel, nastavte jeho `ID` k `LastUpdateTime`a zrušte jeho `Text` vlastnost. Dále vytvořte obslužnou rutinu události pro prvku UpdatePanel `Load` událostí a zobrazit aktuální čas v popisku. (Prvku UpdatePanel `Load` událost se aktivuje při každém postbacku celé nebo jeho část stránky.)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Díky této změně kompletní stránka obsahuje čas, kdy byl načten aktuálně zobrazený produkt. Obrázek 6 ukazuje na stránku, když první uživatel. Obrázek 7 znázorňuje stránky 15 sekund později po ovládacím prvku časovač má "zaškrtnuté" a prvku UpdatePanel byl aktualizován pro zobrazení informací o nový produkt.


[![Zobrazí se náhodně vybrané produktu při načtení stránky](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Obrázek 06**: Zobrazí se A náhodně vybrané produktu při načtení stránky ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![Každých 15 sekund, které se zobrazí nový náhodně vybraný produkt](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Obrázek 07**: každých 15 sekund se zobrazí nový náhodně vybraný produkt ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Krok 3: Použití ovládacího prvku ScriptManagerProxy

Spolu s nezbytné skript pro rozhraní ASP.NET AJAX framework klientskou knihovnu, včetně ScriptManager také zaregistrovat vlastní soubory jazyka JavaScript, odkazy na skript povolené webové služby a vlastní ověřování, autorizace a služby profilu. Obvykle tyto úpravy jsou specifické pro určitou stránku. Ale pokud vlastní skripty, soubory, odkazy na webové služby nebo ověřování, autorizaci nebo služby profilů jsou odkazovány v prvku ScriptManager na stránce předlohy, budou obsaženy ve *všechny* stránky na webu.

Chcete-li přidat přizpůsobení vztahující se k ovládacímu prvku ScriptManager na základě stránku po stránce pomocí ovládacího prvku ScriptManagerProxy. Můžete přidat na stránku obsahu ScriptManagerProxy a zaregistrujte vlastní soubor jazyka JavaScript, odkaz na webovou službu, nebo ověřování, autorizaci nebo služby profilů z ScriptManagerProxy; To má vliv na registraci těchto služeb pro konkrétní stránky obsahu.

> [!NOTE]
> Stránky ASP.NET může mít pouze k dispozici více než jeden prvek ScriptManager. Proto nelze přidat ovládacího prvku ScriptManager na stránku obsahu, pokud ovládací prvek ScriptManager je již definován na hlavní stránce. Jediným účelem ScriptManagerProxy je umožňují vývojářům definovat ScriptManager na stránce předlohy, ale stále mít možnost přidávat vlastní nastavení ovládacímu prvku ScriptManager na základě stránku po stránce.


Pokud chcete zobrazit ovládacího prvku ScriptManagerProxy v akci, můžeme rozšířit UpdatePanel v `ShowRandomProduct.aspx` zahrnout tlačítko, které používá skript na straně klienta pro pozastavení a pokračování v ovládacím prvku časovač. Ovládací prvek časovače má tři metody na straně klienta, které můžete použít k dosažení této požadované funkce:

- `_startTimer()` -začne ovládací prvek časovače
- `_raiseTick()` -způsobí, že ovládací prvek časovače "popisky", a tím zpětného odesílání a zvýšení jeho `Tick` událostí na serveru
- `_stopTimer()` -ovládacím prvku časovač se zastaví

Pojďme vytvořit soubor JavaScriptu s proměnnou s názvem `timerEnabled` a funkci s názvem `ToggleTimer`. `timerEnabled` Proměnná Určuje, zda ovládací prvek časovače je aktuálně povoleno nebo zakázáno; výchozí hodnota je true. `ToggleTimer` Funkce přijímá dva vstupní parametry: odkaz na tlačítko Pozastavit/pokračovat a na straně klienta `id` ovládacím prvku časovač. Tato funkce přepne hodnotu `timerEnabled`, získá odkaz na ovládacím prvku časovač, spuštění nebo zastavení časovače (v závislosti na hodnotě z `timerEnabled`) a aktualizuje zobrazení textu tlačítka "Pozastavení" nebo "Obnovit". Tato funkce bude volána pokaždé, když dojde ke kliknutí na tlačítko Pozastavit/Pokračovat.

Začněte tím, že vytvoříte novou složku na webu s názvem `Scripts`. V dalším kroku přidejte nový soubor do složky Scripts, s názvem `TimerScript.js` typu soubor JScript.


[![Přidejte nový soubor JavaScript do složky skriptů](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Obrázek 08**: přidejte nový soubor JavaScript, aby `Scripts` složky ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![Nový soubor JavaScript je přidaný na web](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Obrázek 09**: nového souboru JavaScriptu se přidala na web ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-asp-net-ajax-cs/_static/image27.png))


Do souboru TimerScript.js v dalším kroku přidejte následující skript:


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

Nyní potřebujeme k registraci tohoto vlastního souboru jazyka JavaScript v `ShowRandomProduct.aspx`. Vraťte se na `ShowRandomProduct.aspx` a přidání ovládacího prvku ScriptManagerProxy na stránce; nastavit jeho `ID` k `MyManagerProxy`. K registraci vlastního jazyka JavaScript souboru vyberte ovládacího prvku ScriptManagerProxy v návrháři a potom přejděte do okna Vlastnosti. Jedna z vlastností má název skripty. Tato vlastnost vyberete, zobrazí se Editor kolekce ScriptReference je znázorněno na obrázku 10. Kliknutím na tlačítko Přidat obsahovat nový odkaz na skript a pak zadejte cestu k souboru skriptu v vlastnost Path: `~/Scripts/TimerScript.js`.


[![Přidání odkazu na skript do ovládacího prvku ScriptManagerProxy](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Obrázek 10**: Přidání odkazu na skript do ovládacího prvku ScriptManagerProxy ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-asp-net-ajax-cs/_static/image30.png))


Po přidání odkazu na skript ovládacího prvku ScriptManagerProxy je deklarativní značek je aktualizováno, aby zahrnovalo `<Scripts>` kolekce s jedním `ScriptReference` položky, jako následující fragment kódu ukazuje:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

`ScriptReference` Položka dává pokyn ScriptManagerProxy zahrnout odkaz na soubor jazyka JavaScript v jeho vykreslované značky. To znamená, když si zaregistrujete vlastní skript v ScriptManagerProxy `ShowRandomProduct.aspx` vykresleného výstupu stránky teď obsahuje jiný `<script src="url"></script>` značky: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Můžete teď říkáme `ToggleTimer` funkci definovanou v `TimerScript.js` z klientského skriptu v `ShowRandomProduct.aspx` stránky. Přidejte následující kód HTML v rámci prvku UpdatePanel:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Zobrazí se tlačítko s textem "Pozastavení". Vždy, když se po kliknutí na funkce JavaScript, která `ToggleTimer` je volána při předávání v odkazu na tlačítko a hodnotu id ovládacího prvku časovače (`ProductTimer`). Všimněte si, syntaxe pro získání `id` ovládacím prvku časovač. `<%=ProductTimer.ClientID%>` vysílá hodnotu `ProductTimer` ovládací prvek Timer `ClientID` vlastnost. V [ *pojmenování ID ovládacích prvků na stránkách obsahu* ](control-id-naming-in-content-pages-cs.md) kurzu jsme probírali rozdíly mezi na straně serveru `ID` hodnotu a výsledné na straně klienta `id` hodnotu a jak `ClientID` vrátí na straně klienta `id`.

Obrázku 11 můžete vidět tuto stránku, když uživatel poprvé prostřednictvím prohlížeče. Časovač aktuálně běží a aktualizuje informace zobrazené produktu každých 15 sekund. Obrázek 12 se zobrazuje obrazovka po kliknutí na tlačítko Pozastavit. Kliknutím na tlačítko Pozastavit časovač se zastaví a aktualizuje text na tlačítko "Obnovit". Informace o produktu aktualizujte (a i nadále aktualizovat každých 15 sekund) po kliknutí na tlačítko Pokračovat.


[![Kliknutím na tlačítko Zastavit ovládacím prvku časovač](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Obrázek 11**: Kliknutím na tlačítko Zastavit na ovládací prvek Timer ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![Klikněte na tlačítko Obnovit restartovat časovač](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Obrázek 12**: klikněte na tlačítko Obnovit restartovat časovač ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>Souhrn

Při sestavování s povoleným AJAX webových aplikací pomocí ASP.NET AJAX framework je nutné, každé webové stránky s povoleným AJAX zahrnují ovládacího prvku ScriptManager. Abyste to mohli provést, jsme stránky předlohy a nemusíte pamatovat si pro každou jednotlivou stránku obsahu přidat ovládací prvek ScriptManager přidat ovládací prvek ScriptManager. Krok 1 jsme si ukázali, jak přidat prvek ScriptManager na stránce předlohy při kroku 2 zvažovali implementace funkcí AJAX na stránce obsahu.

Pokud potřebujete přidat vlastní skripty, odkazy na skript povolené webové služby, nebo vlastní ověřování, autorizaci nebo služby profilů na konkrétní stránce obsahu, přidání ovládacího prvku ScriptManagerProxy na stránce obsahu a potom nakonfigurujte vlastní nastavení existuje. Krok 3 prověřit, jak používat ScriptManagerProxy zaregistrovat vlastní soubor jazyka JavaScript na konkrétní stránce obsahu.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [ASP.NET AJAX Framework](../../../../ajax/index.md)
- [Kurzy k ASP.NET AJAX](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Videa o ASP.NET AJAX](../../../videos/aspnet-ajax/index.md)
- [Vytváření interaktivní uživatelské rozhraní pomocí technologie Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Pomocí NEWID náhodně řazení záznamů](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Použití ovládacího prvku časovače](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více ASP/ASP.NET knih a Zakladatel 4GuysFromRolla.com pracuje s Microsoft webových technologií od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott může být dostupný na adrese [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](interacting-with-the-content-page-from-the-master-page-cs.md)
> [další](specifying-the-master-page-programmatically-cs.md)
