---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
title: "AJAX stránky předlohy a ASP.NET (VB) | Microsoft Docs"
author: rick-anderson
description: "Popisuje možnosti pomocí prvku ASP.NET AJAX a stránky předlohy. Vypadá v použití třídy ScriptManagerProxy; Popisuje, jak jsou různé soubory JS načíst dependi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 0ee9318c-29bb-4d58-b1dc-94e575b8ae10
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
msc.type: authoredcontent
ms.openlocfilehash: b25234f82c46437d853d1ab5b240f8a688995ccc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="master-pages-and-aspnet-ajax-vb"></a>AJAX stránky předlohy a ASP.NET (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_VB.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_VB.pdf)

> Popisuje možnosti pomocí prvku ASP.NET AJAX a stránky předlohy. Vypadá v použití třídy ScriptManagerProxy; Popisuje, jak jsou různé soubory JS načíst v závislosti na tom, jestli se používá ScriptManager v hlavní stránky nebo stránky obsahu.


## <a name="introduction"></a>Úvod

V posledních několika letech mít byla více vývojářům tvorbu podporou AJAXU webových aplikací. Webu technologie AJAX používá počet související webové technologie nabízet rychleji reagující uživatelské prostředí. Vytvoření aplikace AJAX ASP.NET je amazingly snadno Děkujeme k rozhraní ASP.NET AJAX společnosti Microsoft. Technologie ASP.NET AJAX je integrovaná technologie ASP.NET 3.5 a Visual Studio 2008; je k dispozici jako samostatný soubor ke stažení pro aplikace ASP.NET 2.0.

Při vytváření technologie AJAX webové stránky s rozhraní ASP.NET AJAX, je nutné přidat přesně jeden ovládací prvek ScriptManager na každé stránce, která používá rozhraní. Jak již název napovídá, spravuje prvek ScriptManager klientský skript, který je používán podporou AJAXU webové stránky. Minimálně ScriptManager vysílá HTML, které dostane pokyn, aby stahování souborů JavaScript tento způsob vytvoření klientské knihovny ASP.NET AJAX. Je také lze registrovat vlastní soubory JavaScript, skript webovým službám a funkčnost služby vlastní aplikace.

Pokud vaše lokality používá hlavní stránky (jako je), není nutné nutně přidat ovládací prvek ScriptManager do každé jediné obsahu stránce; Místo toho můžete přidat ovládací prvek ScriptManager na hlavní stránku. Tento kurz ukazuje, jak přidat ovládací prvek ScriptManager na hlavní stránku. Vypadá to také o tom, jak používat ovládací prvek ScriptManagerProxy zaregistrovat vlastní skripty a skriptu služby na konkrétní stránku obsahu.

> [!NOTE]
> V tomto kurzu není prozkoumejte návrhu nebo vytváření podporou AJAXU webových aplikací pomocí rozhraní ASP.NET AJAX. Další informace o použití AJAX naleznete videa technologie ASP.NET AJAX a kurzy, jakož i tyto prostředky uvedené v části Další čtení na konci tohoto kurzu.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Zkoumání kódu vysílaných ovládací prvek ScriptManager

Ovládací prvek ScriptManager vysílá značky, které dostane pokyn, aby stahování souborů JavaScript tento způsob vytvoření klientské knihovny ASP.NET AJAX. Také přidá kousek vložené JavaScript na stránku, která inicializuje této knihovny. Následující kód ukazuje obsah, který se přidá do výstupu vykreslené stránky, který obsahuje ovládací prvek ScriptManager:


[!code-html[Main](master-pages-and-asp-net-ajax-vb/samples/sample1.html)]

`<script src="url"></script>` Značky pokyn ke stažení a spuštění souboru JavaScript v prohlížeči *url*. Prvek ScriptManager vysílá tři tyto značky; jeden odkazuje na soubor `WebResource.axd`, zatímco ostatní dva odkazu na soubor `ScriptResource.axd`. Tyto soubory neexistují ve skutečnosti jako soubory ve vašem webu. Místo toho pokud dorazí požadavek na jednu z těchto souborů na webovém serveru, modul ASP.NET zkoumá řetězec dotazu a vrátí příslušný obsah JavaScript. Skript poskytuje tyto tři soubory JavaScript externí tvoří Klientská knihovna pro rozhraní ASP.NET AJAX. Druhá `<script>` značky vysílaných prvek ScriptManager zahrnují vloženého skriptu, který inicializuje této knihovny.

Odkazy na externí skripty a vloženého skriptu vysílaných prvek ScriptManager jsou nezbytné pro stránky, který používá rozhraní ASP.NET AJAX, ale není potřeba pro stránky, které nepoužívají rozhraní. Proto může důvodu, že je ideální pro tyto stránek, které používají rozhraní ASP.NET AJAX přidat pouze ovládací prvek ScriptManager. Toto je dostatečná, ale pokud máte mnoho stránek, které používají rozhraní zobrazí přidání ovládacího prvku ScriptManager na všechny stránky - opakovaných úloh, k vyslovení nejmenší. Alternativně můžete přidat ovládací prvek ScriptManager na hlavní stránku, který pak vloží tento nezbytné skript na všechny stránky obsahu. S tímto přístupem není potřeba nezapomeňte přidat ovládací prvek ScriptManager na novou stránku, který používá rozhraní ASP.NET AJAX, protože je již zahrnut stránky předlohy. Krok 1 nevystavíte slabé stránky zabezpečení provede přidáním ovládací prvek ScriptManager na hlavní stránku.

> [!NOTE]
> Pokud plánujete včetně funkci AJAX v rámci uživatelského rozhraní stránky předlohy, mít žádný výběr ve věci – je nutné zahrnout prvek ScriptManager stránky předlohy.


Přidání prvek ScriptManager k hlavní stránce jeden nevýhodou je, že je skript výše vygenerované v *každých* stránky, bez ohledu na to, jestli se jeho potřebné. To jasně vede k nevyužité šířky pásma pro tyto stránek, které mají ScriptManager zahrnuté (prostřednictvím stránky předlohy) ještě nepoužívají žádné funkce rozhraní ASP.NET AJAX. Ale právě kolik je šířky pásma ke znehodnocení části?

- Skutečný obsah vysílaných ScriptManager (viz výše) Celkový počet zpracovaných položek trochu více než 1KB.
- Tři soubory externího skriptu odkazuje `<script>` elementu, ale tvoří zhruba 450 KB dat nekomprimovaným; na webu, který používá kompresi gzip, tato celková šířka pásma, se může snížit téměř 100 KB. Ale tyto soubory skriptů jsou uložená v mezipaměti v prohlížeči pro jeden rok, což znamená, že potřebují jenom se stahuje jenom jednou a pak můžete znovu použít v jiné stránky na webu.

V případě nejlepší, pokud soubory skriptů jsou uložené v mezipaměti, celkové náklady na je 1KB, což je nepatrné. V nejhorším případě ale – tedy pokud nebyly soubory skriptů zatím staženy a webový server nepoužívá žádný formulář komprese – podle šířky pásma je přibližně 450 KB, které můžete přidat libovolné místo z druhý nebo dvě přes širokopásmové připojení k až několik minut pro  Uživatelé přes modemů. Dobrá zpráva je, že vzhledem k tomu, že soubory externích skriptů jsou uložené v mezipaměti v prohlížeči, nejhorší případu k této situaci dojde zřídka.

> [!NOTE]
> Pokud jste stále podezřelé umístění ovládacího prvku ScriptManager na hlavní stránce, zvažte ve webovém formuláři ( `<form runat="server">` značek na hlavní stránce). Každé stránky ASP.NET, která používá postback model musí obsahovat přesně jeden webového formuláře. Přidání webového formuláře přidá další obsah: a počet skrytého pole `<form>` samostatně, značky a v případě potřeby funkci jazyka JavaScript za inicializaci zpětné volání ze skriptu. Tento kód není nutný pro stránky, které nejsou odeslat zpět. Tento kód nadbytečné může eliminovat odebráním webového formuláře ze stránky předlohy a ručně ho přidat do každé obsahu stránce, které je potřeba. Výhody použití webového formuláře na hlavní stránce však převáží nad nevýhody odpadne ho zbytečně přidány do určité obsahu stránky.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Krok 1: Přidání ovládacího prvku ScriptManager na hlavní stránku

Všechny webové stránky, který používá rozhraní ASP.NET AJAX musí obsahovat přesně jeden ovládací prvek ScriptManager. Kvůli tomuto požadavku většinou má smysl umístit jeden ovládací prvek ScriptManager na hlavní stránce tak, aby ovládací prvek ScriptManager automaticky zahrnuty všechny stránky obsahu. Kromě toho ScriptManager musí předcházet jakékoli prvku ASP.NET AJAX serveru ovládací prvky, jako je například ovládací prvky UpdatePanel a UpdateProgress. Proto je nejlepší umístit ScriptManager před všechny ovládací prvky ContentPlaceHolder ve webovém formuláři.

Otevřete `Site.master` hlavní stránky a přidat ovládací prvek ScriptManager na stránku ve webovém formuláři, ale předtím, než `<div id="topContent">` – element (viz obrázek 1). Pokud používáte Visual Web Developer 2008 nebo Visual Studio 2008, ovládací prvek ScriptManager se nachází v panelu nástrojů na kartě Rozšíření AJAX. Pokud používáte Visual Studio 2005, musíte nejprve nainstalovat rozhraní ASP.NET AJAX a přidání ovládacích prvků do sady nástrojů. Navštivte stránku ASP.NET AJAX stažení se získat rozhraní pro technologii ASP.NET 2.0.

Po přidání prvek ScriptManager na stránku, změnit jeho `ID` z `ScriptManager1` k `MyManager`.


[![Přidá prvek ScriptManager do hlavní stránky](master-pages-and-asp-net-ajax-vb/_static/image2.png)](master-pages-and-asp-net-ajax-vb/_static/image1.png)

**Obrázek 01**: Přidat prvek ScriptManager na stránku předlohy ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-asp-net-ajax-vb/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Krok 2: Pomocí rozhraní ASP.NET AJAX ze stránky obsahu

Pomocí ovládacího prvku ScriptManager přidat k hlavní stránce jsme nyní můžete přidat funkce framework ASP.NET AJAX na všechny stránky obsahu. Umožňuje vytvořit novou stránku ASP.NET, která zobrazuje náhodně vybrané produktu z databáze Northwind. Rozhraní ASP.NET AJAX časovače řízení použijeme k aktualizaci této zobrazení každých 15 sekund zobrazující nového produktu.

Začněte vytvořením novou stránku v kořenovém adresáři s názvem `ShowRandomProduct.aspx`. Nezapomeňte vytvořit vazbu této nové stránce `Site.master` stránky předlohy.


[![Přidat novou stránku ASP.NET na web](master-pages-and-asp-net-ajax-vb/_static/image5.png)](master-pages-and-asp-net-ajax-vb/_static/image4.png)

**Obrázek 02**: Přidat novou stránku ASP.NET na web ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-asp-net-ajax-vb/_static/image6.png))


Odvolat, v zadání názvu, značky Meta a další záhlaví ve formátu HTML v tomto kurzu stránky předlohy [SKM1] jsme vytvořili vlastní základní stránky třídy s názvem `BasePage` který vygenerován nadpis stránky, pokud nebyl explicitně nastaven. Přejděte na `ShowRandomProduct.aspx` kódu stránky třídy a ji odvodit z `BasePage` (místo z `System.Web.UI.Page`).

Nakonec aktualizujte `Web.sitemap` souboru záznam pro tento účel. Přidejte následující kód pod `<siteMapNode>` pro hlavní server k obsahu stránce interakce lekce:


[!code-xml[Main](master-pages-and-asp-net-ajax-vb/samples/sample2.xml)]

Přidání tohoto `<siteMapNode>` element se odrazí v poznatky seznamu (viz obrázek 5).

### <a name="displaying-a-randomly-selected-product"></a>Zobrazení náhodně vybrané produktu

Vraťte se do `ShowRandomProduct.aspx`. Z návrháře, přetáhněte z panelu nástrojů do ovládacího prvku UpdatePanel `MainContent` obsahu ovládacího prvku a nastavit jeho `ID` vlastnost, která má `ProductPanel`. UpdatePanel představuje oblast na obrazovce, která se dá asynchronně aktualizovat přes postback části stránky.

Naše první úloha je pro zobrazení informací o náhodně vybrané produktů v rámci prvku UpdatePanel. Spustit tak, že přetáhnete ovládacího prvku DetailsView do prvku UpdatePanel. Nastavení ovládacího prvku DetailsView `ID` vlastnost `ProductInfo` a vymažte její `Height` a `Width` vlastnosti. Rozbalte DetailsView inteligentních značek a vyberte z rozevíracího seznamu vyberte zdroj dat pro vazbu DetailsView nový ovládací prvek SqlDataSource s názvem `RandomProductDataSource`.


[![Vytvoření vazby DetailsView do ovládacího prvku nové SqlDataSource](master-pages-and-asp-net-ajax-vb/_static/image8.png)](master-pages-and-asp-net-ajax-vb/_static/image7.png)

**Obrázek 03**: vazby DetailsView do ovládacího prvku nové SqlDataSource ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-asp-net-ajax-vb/_static/image9.png))


Konfigurace pro připojení k databázi Northwind prostřednictvím ovládacího prvku SqlDataSource `NorthwindConnectionString` (které jsme vytvořili v práce se stránky předlohy ze stránky obsahu [SKM2] kurzu). Při konfiguraci příkaz select zvolit vlastní příkazu jazyka SQL a pak zadejte následující dotaz:


[!code-sql[Main](master-pages-and-asp-net-ajax-vb/samples/sample3.sql)]

`TOP 1` – Klíčové slovo v `SELECT` klauzule vrací pouze na první záznam vrácených dotazem. `NEWID()` Funkce generuje novou hodnotu globálně jedinečný identifikátor (GUID) a mohou být používány `ORDER BY` klauzuli, která vrátí záznamy v tabulce v náhodném pořadí.


[![Konfigurace SqlDataSource vrátit záznam jeden, náhodně vybrané](master-pages-and-asp-net-ajax-vb/_static/image11.png)](master-pages-and-asp-net-ajax-vb/_static/image10.png)

**Obrázek 04**: Konfigurace SqlDataSource vrátit jedinou náhodně vybrané záznamu ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-asp-net-ajax-vb/_static/image12.png))


Po dokončení průvodce, Visual Studio vytvoří BoundField pro dva sloupce vrácený výše uvedeném dotazu. V tomto okamžiku vaší stránky deklarativní by měl vypadat takto:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample4.aspx)]

Obrázek 5 ukazuje `ShowRandomProduct.aspx` v případě, že zobrazit pomocí prohlížeče. Klikněte na tlačítko Aktualizovat v prohlížeči na znovu načíst stránku; měli byste vidět `ProductName` a `UnitPrice` hodnoty pro nový záznam náhodně vybrané.


[![Se zobrazí název a cena náhodných produktu](master-pages-and-asp-net-ajax-vb/_static/image14.png)](master-pages-and-asp-net-ajax-vb/_static/image13.png)

**Obrázek 05**: Zobrazí se název a cenu A náhodných produktu ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-asp-net-ajax-vb/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Automaticky zobrazení nového produktu každých 15 sekund

Rozhraní ASP.NET AJAX zahrnuje časovače ovládací prvek, který provede zpětné volání v určený čas; na odeslat zpět časovače `Tick` událost se vyvolá. Pokud se ovládací prvek časovače je umístit do ovládacího prvku UpdatePanel aktivuje částečná stránka zpětné volání, během které jsme můžete rebind data, která mají DetailsView zobrazíte nového produktu náhodně vybrané.

K tomu, přetáhněte časovač z panelu nástrojů a umístěte jej do prvku UpdatePanel. Změnit časovače `ID` z `Timer1` k `ProductTimer` a jeho `Interval` vlastnost z 60000 na 15 000. `Interval` Vlastnost udává počet milisekund, po mezi postback; jeho nastavení na 15 000 způsobí, že časovač k spustily částečná stránka každých 15 sekund. V tomto okamžiku časovače deklarativní by měl vypadat takto:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample5.aspx)]

Vytvoření obslužné rutiny události pro časovače `Tick` událostí. V této obslužné rutiny události musíme rebind data, která mají DetailsView voláním prvku DetailsView `DataBind` metoda. Díky tomu dá pokyn DetailsView znovu načíst data z jeho prvek zdroje dat, který bude vybrat a zobrazit novou náhodně vybrané záznamu (stejně jako při opětovném načtení stránky kliknutím na tlačítko Aktualizovat v prohlížeči).


[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample6.vb)]

To je všechno je k němu! Otevírat stránku prostřednictvím prohlížeče. Standardně se zobrazují informace náhodných produktu. Pokud jste patiently podívejte se na obrazovce můžete si všimnout, že po 15 sekund, informace o novém produktu magically nahradí existující zobrazení.

Pokud chcete lépe zjistit, co se děje v tomto poli, přidejme UpdatePanel, které se zobrazuje čas poslední aktualizace zobrazení ovládacího prvku popisek. Přidání ovládacího prvku popisek Web v rámci prvku UpdatePanel, nastavte jeho `ID` k `LastUpdateTime`a vymazat jeho `Text` vlastnost. Dále vytvořte obslužnou rutinu události pro prvku UpdatePanel `Load` události a zobrazení aktuální čas v popisku. (UpdatePanel `Load` událost je aktivována na každých postback celé nebo jeho část stránky.)


[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample7.vb)]

Díky této změně dokončení stránka obsahuje dobu, kterou aktuálně zobrazený produktu byla načtena. Obrázek 6 zobrazuje stránku, pokud nejprve prošli nový obsah. Obrázek 7 znázorňuje stránce po ovládacího prvku časovače má "zaškrtnuté" a zobrazení informací o novém produktu byl aktualizován UpdatePanel později 15 sekund.


[![Náhodně vybrané produktu se zobrazí na načtení stránky](master-pages-and-asp-net-ajax-vb/_static/image17.png)](master-pages-and-asp-net-ajax-vb/_static/image16.png)

**Obrázek 06**: A náhodně vybrané produktu se zobrazí na načtení stránky ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-asp-net-ajax-vb/_static/image18.png))


[![Každých 15 sekund, které se zobrazí nový náhodně vybrané produkt](master-pages-and-asp-net-ajax-vb/_static/image20.png)](master-pages-and-asp-net-ajax-vb/_static/image19.png)

**Obrázek 07**: každých 15 sekund se zobrazí nový náhodně vybrané produkt ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-asp-net-ajax-vb/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Krok 3: Použití ScriptManagerProxy ovládacího prvku

Společně s včetně nezbytné skript pro rozhraní ASP.NET AJAX klientské knihovny, ScriptManager také zaregistrovat vlastní soubory JavaScript, odkazy na skript povolené webové služby a vlastní ověřování, autorizace a profilu služby. Tyto úpravy jsou obvykle specifické pro určité stránky. Ale pokud vlastní skript soubory, odkazy na webové služby, nebo ověřování, autorizaci nebo profilu služby se odkazuje v ScriptManager na hlavní stránce pak budou jsou součástí všechny stránky webu.

Chcete-li přidat ScriptManager související přizpůsobení na základě po stránkách použít ovládací prvek ScriptManagerProxy. Můžete přidat ScriptManagerProxy na stránku obsahu a potom proveďte registraci vlastní soubor JavaScript, odkaz na webovou službu, nebo ověřování, autorizaci nebo profilu služby z ScriptManagerProxy; To má za následek registrace tyto služby pro konkrétní stránky obsahu.

> [!NOTE]
> Stránky ASP.NET může mít pouze existuje více než jeden prvek ScriptManager. Ovládací prvek ScriptManager proto nemůžete přidat na stránku obsahu, pokud ovládací prvek ScriptManager již je definována v stránky předlohy. ScriptManagerProxy pouze za účelem je poskytnout způsob pro vývojáře k definování prvek ScriptManager na hlavní stránce, ale stále mít možnost Přidat přizpůsobení ScriptManager na základě po stránkách.


Zobrazíte ScriptManagerProxy ovládacího prvku akce umožňuje posílení UpdatePanel v `ShowRandomProduct.aspx` zahrnout tlačítko, které používá klientský skript pozastavení nebo obnovení ovládacího prvku časovače. Ovládací prvek časovače má tři metody na straně klienta, které jsme můžete použít k dosažení této požadované funkce:

- `_startTimer()`-spustí časovač ovládací prvek
- `_raiseTick()`-způsobí, že ovládacího prvku časovače pro "značek," a publikování zpět a vyvolá událost jeho značek na serveru
- `_stopTimer()`-Zastaví ovládacího prvku časovače

Umožňuje vytvořit soubor JavaScript se proměnné s názvem `timerEnabled` a funkce s názvem `ToggleTimer`. `timerEnabled` Proměnná Určuje, zda je ovládací prvek časovače aktuálně povoleno nebo zakázáno; je standardně nastavena na hodnotu true. `ToggleTimer` Funkce přijímá dva vstupní parametry: odkaz na tlačítko pozastavit nebo obnovit a na straně klienta `id` hodnota časovače ovládacího prvku. Tato funkce přepíná hodnotu `timerEnabled`, získá odkaz na ovládací prvek časovače, spuštění nebo zastavení časovač (v závislosti na hodnotě `timerEnabled`) a aktualizuje na tlačítko zobrazovaný text na "Pozastavení" nebo "Pokračování". Tato funkce bude volána, když po kliknutí na tlačítko pozastavit nebo obnovit.

Začněte vytvořením novou složku na webu s názvem `Scripts`. Dál přidejte nový soubor do složky skriptů s názvem `TimerScript.js` typu JScript – soubor.


[![Přidat nový soubor JavaScript do složky skriptů](master-pages-and-asp-net-ajax-vb/_static/image23.png)](master-pages-and-asp-net-ajax-vb/_static/image22.png)

**Obrázek 08**: Přidat nový soubor JavaScript, který `Scripts` složky ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-asp-net-ajax-vb/_static/image24.png))


[![Nový soubor JavaScript se přidal k webu](master-pages-and-asp-net-ajax-vb/_static/image26.png)](master-pages-and-asp-net-ajax-vb/_static/image25.png)

**Obrázek 09**: nový soubor JavaScript A přidala na web ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-asp-net-ajax-vb/_static/image27.png))


Dál přidejte následující skript k `TimerScript.js` souboru:


[!code-csharp[Main](master-pages-and-asp-net-ajax-vb/samples/sample8.cs)]

Nyní potřebujeme zaregistrovat tento vlastní soubor JavaScript v `ShowRandomProduct.aspx`. Vraťte se do `ShowRandomProduct.aspx` a přidání ScriptManagerProxy ovládacího prvku na stránku; nastavit jeho `ID` k `MyManagerProxy`. K registraci vlastní JavaScript soubor vyberte ovládací prvek ScriptManagerProxy v návrháři a potom přejděte do okna vlastností. Jedna z vlastností je s názvem skripty. Výběr této vlastnosti se zobrazí Editor kolekce ScriptReference vidět na obrázku 10. Kliknutím na tlačítko Přidat zahrnout odkaz na nový skript a pak zadejte cestu k souboru skriptu ve vlastnosti cesta: `~/Scripts/TimerScript.js`.


[![Přidání odkazu na skript do ovládacího prvku ScriptManagerProxy](master-pages-and-asp-net-ajax-vb/_static/image29.png)](master-pages-and-asp-net-ajax-vb/_static/image28.png)

**Obrázek 10**: Přidání odkazu na skript do ovládacího prvku ScriptManagerProxy ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-asp-net-ajax-vb/_static/image30.png))


Po přidání odkazu na skript ovládacího prvku ScriptManagerProxy je deklarativní značek aktualizovala a obsahovala `<Scripts>` kolekce s jedním `ScriptReference` položky jako následující fragment kódu ukazuje:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample9.aspx)]

`ScriptReference` Položka obsahuje pokyny pro ScriptManagerProxy zahrnout odkaz na soubor JavaScript v jeho vykreslované značky. To znamená, když si zaregistrujete vlastní skript v ScriptManagerProxy `ShowRandomProduct.aspx` výstupu vykreslené stránky teď obsahuje jiný `<script src="url"></script>` značky: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Můžete teď říkáme `ToggleTimer` funkci definovanou v `TimerScript.js` z klientského skriptu v `ShowRandomProduct.aspx` stránky. Přidejte následující kód HTML v rámci prvku UpdatePanel:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample10.aspx)]

Zobrazí tlačítko s textem "Pozastavení". Vždy, když se po kliknutí na, funkce JavaScript, která `ToggleTimer` je volána, předávání v odkaz na tlačítko a `id` hodnota časovače ovládacího prvku (`ProductTimer`). Všimněte si syntaxe pro získání `id` hodnota časovače ovládacího prvku. `<%=ProductTimer.ClientID%>`vysílá hodnotu `ProductTimer` ovládacího prvku Timer `ClientID` vlastnost. V pojmenování řízení ID v kurzu stránky obsahu [SKM3] jsme probrali rozdíly mezi na straně serveru `ID` hodnota a výsledný na straně klienta `id` hodnotu a jak `ClientID` vrátí na straně klienta `id`.

Obrázek 11 ukazuje této stránce, když nejdřív navštívili prostřednictvím prohlížeče. Časovač běží v současné době a aktualizuje informace zobrazené produktu každých 15 sekund. Obrázek 12 znázorňuje obrazovky po bylo stisknuto tlačítko Pozastavit. Klepnutím na tlačítko Pozastavit zastaví časovač a aktualizuje na tlačítko text "Obnovit". Informace o produktu aktualizujte (a pokračovat v aktualizaci každých 15 sekund) po kliknutí na tlačítko Pokračovat.


[![Klikněte na tlačítko Zastavit časovač ovládací prvek](master-pages-and-asp-net-ajax-vb/_static/image32.png)](master-pages-and-asp-net-ajax-vb/_static/image31.png)

**Obrázek 11**: klikněte na tlačítko Zastavit časovač ovládací prvek ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-asp-net-ajax-vb/_static/image33.png))


[![Klikněte na tlačítko Obnovit restartovat časovač](master-pages-and-asp-net-ajax-vb/_static/image35.png)](master-pages-and-asp-net-ajax-vb/_static/image34.png)

**Obrázek 12**: klikněte na tlačítko Obnovit restartovat časovač ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-asp-net-ajax-vb/_static/image36.png))


## <a name="summary"></a>Souhrn

Při vytváření technologie AJAX webových aplikací pomocí rozhraní ASP.NET AJAX, je nutné, aby každý technologie AJAX webová stránka obsahovat ovládací prvek ScriptManager. Pro usnadnění tohoto procesu, jsme stránky předlohy, namísto nutnosti nezapomeňte přidat ovládací prvek ScriptManager každé stránky obsahu přidat ovládací prvek ScriptManager. Krok 1 vám ukázal, jak přidat prvek ScriptManager k hlavní stránce při zvážení implementace funkci AJAX na obsahu stránce kroku 2.

Pokud potřebujete přidat vlastní skripty, odkazy na skript povolené webové služby, nebo vlastní ověřování, autorizaci nebo profilu služby na konkrétní stránku obsahu, přidání ScriptManagerProxy ovládacího prvku na stránku obsahu a potom nakonfigurujte přizpůsobení existuje. Krok 3 zkontrolován postup použití ScriptManagerProxy zaregistrovat vlastní soubor JavaScript na konkrétní stránku obsahu.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Rozhraní ASP.NET AJAX](../../../../ajax/index.md)
- [Kurzy ASP.NET AJAX](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Videa technologie ASP.NET AJAX](../../../videos/aspnet-ajax/index.md)
- [Vytváření interaktivní uživatelské rozhraní pomocí technologie Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Pomocí NEWID náhodně řazení záznamů](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Použití ovládacího prvku časovače](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott lze dosáhnout za [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](interacting-with-the-content-page-from-the-master-page-vb.md)
[další](specifying-the-master-page-programmatically-vb.md)
