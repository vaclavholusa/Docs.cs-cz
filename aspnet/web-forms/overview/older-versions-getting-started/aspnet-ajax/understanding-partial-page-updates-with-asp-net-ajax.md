---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Principy částečná stránka se aktualizuje pomocí prvku ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Možná nejvíce viditelné funkce ASP.NET AJAX rozšíření je možnost provést na stránce částečné nebo přírůstkové aktualizace bez provádění úplné postback t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 91a98bf1c9a71ae84c569f7ae40930422cb652e0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891318"
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Principy částečná stránka se aktualizuje pomocí prvku ASP.NET AJAX
====================
podle [Scott Dikovat](https://github.com/scottcate)

[Stáhnout PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Možná nejvíce viditelné funkce ASP.NET AJAX rozšíření je možnost provést na stránce částečné nebo přírůstkové aktualizace bez provádění úplný postback na server bez změn kódu a změny minimální značek. Výhody jsou rozsáhlé – stav vaše multimédia (třeba Adobe Flash nebo Windows Media) je beze změny, se snižuje náklady na šířku pásma a klient nedochází k blikání obvykle spojené se zpětné volání.


## <a name="introduction"></a>Úvod

Technologie ASP.NET společnosti Microsoft poskytuje programovací model objektově orientované a událostmi řízené a jednotek s výhod zkompilovaný kód. Svůj model zpracování na straně serveru, má však několik nevýhody vyplývajících z technologie:

- Stránka aktualizace vyžadují round-trip na server, který vyžaduje aktualizaci stránky.
- Uložení nezachovají důsledky generovaných Javascript nebo jiné technologie klienta (například Adobe Flash)
- Při zpětné volání prohlížeče než Microsoft Internet Explorer nepodporují automaticky obnovení na pozici posunutí. A to i v aplikaci Internet Explorer, je stále blikání jako aktualizaci stránky.
- Zpětná vystavení může zahrnovat vysoký objem šířky pásma, jako \_ \_pole stavu zobrazení formuláře může růst, zejména v případě, že plánování práce s ovládacími prvky, jako je například ovládací prvek GridView nebo opakovače.
- Neexistuje žádné jednotný model pro přístup k webovým službám prostřednictvím JavaScript nebo jiné technologie straně klienta.

Zadejte přípony prvku ASP.NET AJAX společnosti Microsoft. AJAX, který zastupuje **A** synchronní **J** avaScript **A** nd **X** ML, je představuje integrované rozhraní pro poskytování přírůstkové stránky aktualizace prostřednictvím jazyka JavaScript a platformy, se skládají z kódu na straně serveru zahrnující rozhraní AJAX Microsoft a komponenty skript s názvem knihovně skript AJAX společnosti Microsoft. Rozšíření prvku ASP.NET AJAX taky poskytuje podporu pro různé platformy pro přístup k webové služby ASP.NET prostřednictvím jazyka JavaScript.

Tento dokument White Paper prozkoumá funkci částečná stránka aktualizace rozšíření ASP.NET AJAX, který obsahuje součást ScriptManager, ovládacího prvku UpdatePanel a UpdateProgress řízení a zvažuje scénáře, ve kterých by měla nebo by neměl být použít.

Tento dokument White Paper vychází z verze beta verzi 2 sady Visual Studio 2008 a rozhraní .NET Framework 3.5, která integruje rozšíření ASP.NET AJAX v knihovně tříd Base (kde dříve součástí rozšíření, která je k dispozici pro technologii ASP.NET 2.0). Tento dokument White Paper také předpokládá, že používáte Visual Studio 2008 a není Visual Web Developer Express Edition; Některé šablony projektů, které jsou odkazované nemusí být k dispozici uživatelům Visual Web Developer Express.

## <a name="partial-page-updates"></a>Aktualizace části stránky

Možná nejvíce viditelné funkce ASP.NET AJAX rozšíření je možnost provést na stránce částečné nebo přírůstkové aktualizace bez provádění úplný postback na server bez změn kódu a změny minimální značek. Výhody jsou rozsáhlé – stav vaše multimédia (třeba Adobe Flash nebo Windows Media) je beze změny, se snižuje náklady na šířku pásma a klient nedochází k blikání obvykle spojené se zpětné volání.

Možnost integrace vykreslení části stránky je integrována do technologie ASP.NET s minimálními změnami do vašeho projektu.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Návod: Integraci částečným vykreslením do existujícího projektu


1. V aplikaci Microsoft Visual Studio 2008, vytvořte nový projekt ASP.NET webu přechodem na <em>soubor</em>  <em>- &gt; nový</em>  <em>- &gt; webu</em> a výběrem webové stránky ASP.NET z tohoto dialogového okna. Můžete pojmenovat se vám líbí a může ji nainstalovat buď do systému souborů nebo do Internetové informační služby (IIS).
2. Zobrazí se prázdný výchozí stránka s základní značky ASP.NET (formuláře na straně serveru a `@Page` – direktiva). Vyřaďte popisek s názvem `Label1` tlačítko s názvem `Button1` na stránku v rámci elementu formuláře. Můžete nastavit jejich vlastnosti textu k vám líbí.
3. V návrhovém zobrazení, klikněte dvakrát na `Button1` pro vygenerování obslužné rutiny kódu. V rámci této obslužné rutiny události nastavit `Label1.Text` na kliknutí na tlačítko! .

**Výpis 1: Kód pro default.aspx před částečným vykreslením je povoleno.**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Výpis 2: (Oříznut) v default.aspx.cs Codebehind**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Stisknutím klávesy F5 spusťte vašeho webu. Visual Studio zobrazí výzvu k přidá soubor web.config chcete povolit ladění; učinit. Když kliknete na tlačítko, Všimněte si aktualizaci stránky, chcete-li změnit text popisku, a je stručný blikání jak bude překreslen stránky.
2. Po zavření okna prohlížeče, vrátí k sadě Visual Studio a na stránku značek. Přejděte dolů v sadě nástrojů Visual Studio a najít na kartě s názvem bez přípony rozšíření AJAX. (Pokud nemáte na této kartě vzhledem k tomu, že používáte starší verzi rozšíření technologií AJAX nebo Atlas, najdete postup pro registraci položek sady nástrojů AJAX rozšíření později v tomto dokumentu nebo nainstalovat aktuální verzi s instalačním programem systému Windows ke stažení z webu).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Kliknutím zobrazit obrázek v plné velikosti](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>Známý problém:</em>při instalaci sady Visual Studio 2008 do počítače, který již má nainstalované s ASP.NET 2.0 AJAX rozšíření Visual Studio 2005, Visual Studio 2008 naimportuje položek sady nástrojů rozšíření AJAX. Můžete určit, zda je tomu tak, že prověří popis komponenty; musí se, že verze 3.5.0.0. Pokud říká verzi 2.0.0.0, pak jste naimportovali vaše staré položky panelu nástrojů a bude třeba je ručně importovat pomocí dialogu Výběr položek sady nástrojů v sadě Visual Studio. Nebude možné k přidávání ovládacích prvků verze 2 pomocí návrháře.

2. Před `<asp:Label>` začne značky, vytvořte řádek z prázdných znaků a dvakrát klikněte na ovládací prvek UpdatePanel v panelu nástrojů. Všimněte si, že nový `@Register` – direktiva je zahrnuta v horní části stránky, která určuje, že by měly být naimportovány ovládací prvky v rámci oboru názvů System.Web.UI pomocí `asp:` předponu.
3. Přetáhněte ukončovací `</asp:UpdatePanel>` označovat za koncem prvku tlačítko tak, aby element s ovládací prvky popisek a tlačítko zabalené je ve správném formátu.
4. Po otevření `<asp:UpdatePanel>` značky, začněte otevřením novou značku. Všimněte si, že IntelliSense, vyzve vás dvě možnosti. V takovém případě vytvořit `<ContentTemplate>` značky. Ujistěte se, že obtékat kolem tlačítko a popisku této značky, tak, aby kód je ve správném formátu.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Kliknutím zobrazit obrázek v plné velikosti](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. Kdekoli v rámci `<form>` elementu, zahrnují ovládací prvek ScriptManager dvojím kliknutím na `ScriptManager` položku v panelu nástrojů.
2. Upravit `<asp:ScriptManager>` označovat tak, aby obsahuje atribut `EnablePartialRendering= true`.

**Výpis 3: Kód pro default.aspx s částečným vykreslením povoleno**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Otevřete soubor web.config. Všimněte si, že sada Visual Studio automaticky přidala kompilace odkaz na System.Web.Extensions.dll.

1. Co je nového v sadě Visual Studio 2008: souboru web.config, která se dodává se šablonami projektů webové stránky ASP.NET automaticky zahrnuje všechny nezbytné odkazy na rozšíření ASP.NET AJAX a zahrnuje komentáři části informace o konfiguraci, která může být zrušení komentáři k povolení dalších funkcí. Visual Studio 2005 měl podobné šablony při ASP.NET 2.0 AJAX rozšíření byly nainstalovány. Ale v sadě Visual Studio 2008, jsou rozšíření AJAX výslovný nesouhlas s ve výchozím nastavení (to znamená, že se odkazuje ve výchozím nastavení, ale můžete odebrat, protože odkazy).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Kliknutím zobrazit obrázek v plné velikosti](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. Stisknutím klávesy F5 spusťte svůj web. Všimněte si, jak nebyly žádné změny kódu zdroj nezbytný pro podporu částečným vykreslením – pouze kód byl změněn.

Při spuštění webu, měli byste vidět, že částečným vykreslením je teď povolená, protože když kliknete na tlačítko existuje bude bez blikání ani bude k dispozici všechny změny v pozici posunutí stránky (v tomto příkladu není prokázat, že). Pokud byste chtěli podívejte se na zdroji vykreslené stránky po kliknutí na tlačítko, bude zkontrolujte, jestli ve skutečnosti není po zpět došlo k chybě – původní text popisku je stále součástí zdrojový kód a popisku došlo ke změně prostřednictvím jazyka JavaScript.

Visual Studio 2008 nezobrazí spolu s předem definované šablony pro webový server s podporou technologie ASP.NET AJAX. Tyto šablony se však k dispozici v sadě Visual Studio 2005 byly nainstalované Visual Studio 2005 a rozšíření AJAX ASP.NET 2.0. V důsledku toho konfiguraci webu a od verze šablony enabled webu budou pravděpodobně bylo ještě jednodušší, jako šablona by měla obsahovat soubor web.config plně nakonfiguroval (podpora všech rozšíření ASP.NET AJAX, včetně přístupu webové služby a serializace JSON - JavaScript Object Notation) a zahrnuje UpdatePanel a ContentTemplate v rámci hlavní stránky webových formulářů ve výchozím nastavení. Povolení částečným vykreslením tuto stránku výchozí je jednoduché, opětovné návštěvy 10 krok tohoto průvodce a vkládání ovládacích prvků na stránku.

## <a name="the-scriptmanager-control"></a>Ovládací prvek ScriptManager

## <a name="scriptmanager-control-reference"></a>Ovládací prvek ScriptManager odkaz

Povolená značka vlastnosti:

| **Název vlastnosti** | **Typ** | **Popis** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | BOOL | Určuje, zda chcete použít vlastní chybové části souboru web.config se budou zpracovávat chyby. |
| AsyncPostBackError – zpráva | String | Získá nebo nastaví chybovou zprávu odeslat klientovi, pokud je vyvolána chyba. |
| AsyncPostBack-Timeout | Int32 | Získá nebo nastaví výchozí dobu, čas, kdy by měl klient pro asynchronní požadavek dokončit. |
| EnableScript globalizace | BOOL | Získá nebo nastaví, zda je povoleno skriptu globalization. |
| Lokalizace EnableScript | BOOL | Získá nebo nastaví, zda je povoleno lokalizace skriptu. |
| ScriptLoadTimeout | Int32 | Určuje počet sekund, které jsou povoleny pro načtení skripty do klienta |
| ScriptMode | Výčet (automaticky, ladění, verzi, dědí) | Získá nebo nastaví, zda se k vykreslení verze skriptů |
| ScriptPath | String | Získá nebo nastaví kořenovou cestu k umístění souborů skriptů k odeslání do klienta. |

Vlastnosti jen kódu:

| **Název vlastnosti** | **Typ** | **Popis** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService Manager | Získá informace o proxy služby ověřování ASP.NET, která odešle klientovi. |
| IsDebuggingEnabled | BOOL | Získá jestli skript a ladění kódu je povolena. |
| IsInAsyncPostback | BOOL | Zjistí, zda stránka je aktuálně v asynchronní požadavek post zpět. |
| ProfileService | ProfileService-Manager | Získá informace o proxy služby profilace ASP.NET, která odešle klientovi. |
| Skripty | Kolekce&lt;odkaz na skript&gt; | Získá kolekci odkazům na skript, které se budou odesílat do klienta. |
| Služby | Kolekce&lt;odkaz na službu&gt; | Získá kolekci odkazů na proxy webové služby, které budou odeslány do klienta. |
| SupportsPartialRendering | BOOL | Zjistí, zda aktuální klient podporuje částečné vykreslování. Pokud je tato vlastnost vrací **false**, pak všechny požadavky na stránku standardní postback. |

Veřejné kód metody:

| **Název metody** | **Typ** | **Popis** |
| --- | --- | --- |
| SetFocus(string) | Void | Nastaví fokus klienta určitý ovládací prvek v případě, že dokončení požadavku. |

Následníky značek:

| **Značka** | **Popis** |
| --- | --- |
| &lt;AuthenticationService&gt; | Poskytuje podrobnosti o proxy serveru k ověřovací službě ASP.NET. |
| &lt;ProfileService&gt; | Poskytuje podrobnosti o proxy serveru s technologií ASP.NET profilace služby. |
| &lt;Skripty&gt; | Poskytuje odkazy na další skripty. |
| &lt;asp:ScriptReference&gt; | Označuje odkaz specifického skriptu. |
| &lt;Service&gt; | Poskytuje další odkazy na webové služby, kteří budou mít generované třídy proxy serveru. |
| &lt;asp:ServiceReference&gt; | Označuje konkrétní odkaz webové služby. |

Ovládací prvek ScriptManager je nezbytné základní pro rozšíření ASP.NET AJAX. Poskytuje přístup ke knihovně skriptu (včetně systém typů rozsáhlé skript na straně klienta), podporuje částečné vykreslování a poskytuje rozsáhlou podporu pro další služby ASP.NET (například ověřování a profilace, ale také jiné webové služby). Ovládací prvek ScriptManager taky poskytuje podporu pro skripty klienta globalizace a lokalizace.

## <a name="providing-alterative-and-supplemental-scripts"></a>Poskytnutí V.42 a další skripty

Rozšíření Microsoft ASP.NET 2.0 AJAX zahrnují kód celý skript v obou ladění a vydání edice jako prostředky v odkazovaných sestaveních, vývojáři jsou volně přesměrování ScriptManager vlastní skripty, jakož i zaregistrovat Další potřebné skripty.

Pokud chcete přepsat výchozí vazby pro skripty typicky zahrnuté (jako jsou ty, které podporují názvů Sys.WebForms a vlastní zadáním systému), můžete si zaregistrovat `ResolveScriptReference` událostí třídy ScriptManager. Když tato metoda je volána, obslužné rutiny události má možnost změnit cestu k souboru skriptu, který je nejistá; Správce skriptu pak odešle kopii různé nebo vlastní skripty klientovi.

Kromě toho skriptu odkazy (reprezentována `ScriptReference` – třída) může být součástí prostřednictvím kódu programu, nebo prostřednictvím značek. Uděláte to tak, buď Programová změna `ScriptManager.Scripts` kolekce, nebo zahrnout `<asp:ScriptReference>` značky pod `<Scripts>` značku, která je první úrovně podřízený ovládací prvek ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>Vlastní zpracování chyb pro komponenty UpdatePanel

I když aktualizace jsou zpracovávány určeného ovládacích prvků UpdatePanel aktivačních událostí, podpora pro zpracování chyb a vlastní chybové zprávy se zpracovává souborem instance ovládací prvek ScriptManager na stránce. K tomu je potřeba vystavení události, `AsyncPostBackError`, na stránku, který lze potom zadejte vlastní logiky zpracování výjimek.

Ve využívání AsyncPostBackError událostí, můžete určit `AsyncPostBackErrorMessage` vlastností, která se pak způsobí, že výstrahy pole má být aktivována po dokončení zpětného volání.

Vlastní nastavení na straně klienta je také možné místo použití pole výchozí výstrah; Například můžete chtít zobrazit přizpůsobený `<div>` elementu, nikoli výchozí modální dialogové okno prohlížeče. V takovém případě můžete zpracovávat chyby ve skriptu klienta:

**Výpis 5: Skript na straně klienta pro zobrazení vlastní chyby**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Celkem jednoduše skript výše zaregistruje zpětné volání s modulem runtime AJAX na straně klienta pro při dokončení asynchronní požadavek. Následně kontroluje, zda se oznámila chyba a pokud ano, zpracuje podrobnosti, nakonec označující modulu runtime, chyba byla zpracována ve vlastních skriptů.

## <a name="globalization-and-localization-support"></a>Globalizace a lokalizace podpory

Ovládací prvek ScriptManager poskytuje rozsáhlou podporu pro lokalizace řetězce skriptu a komponenty uživatelského rozhraní; Toto téma však je mimo rozsah tohoto dokumentu. Další informace najdete v dokumentu White Paper, podpora globalizace v rozšíření ASP.NET AJAX.

## <a name="the-updatepanel-control"></a>Ovládací prvek UpdatePanel

## <a name="updatepanel-control-reference"></a>Řízení UpdatePanel – referenční informace

Povolená značka vlastnosti:

| **Název vlastnosti** | **Typ** | **Popis** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Určuje, zda podřízených ovládacích prvků vyvolání automaticky aktualizovat na zpětné volání. |
| RenderMode – | výčet (bloku, vložené) | Určuje, že vizuálně zobrazí způsob obsah. |
| UpdateMode | výčet (vždy, podmíněného) | Určuje, zda UpdatePanel vždy aktualizuje během částečné vykreslování nebo pokud je pouze aktualizován, když je dosaženo aktivační událost. |

Vlastnosti jen kódu:

| **Název vlastnosti** | **Typ** | **Popis** |
| --- | --- | --- |
| IsInPartialRendering | bool | Zjistí, zda UpdatePanel je podpora částečným vykreslením pro aktuální požadavek. |
| ContentTemplate | ITemplate | Získá šablonu značek pro žádost o aktualizaci. |
| ContentTemplateContainer | Ovládací prvek | Získá programový šablonu pro žádost o aktualizaci. |
| Aktivační procedury | UpdatePanel - TriggerCollection | Získá seznam aktivačních událostí přidružených aktuální UpdatePanel. |

Veřejné kód metody:

| **Název metody** | **Typ** | **Popis** |
| --- | --- | --- |
| Update() | Void | Aktualizuje zadaný UpdatePanel prostřednictvím kódu programu. Umožňuje žádost serveru pro aktivaci částečné vykreslení ovládacího prvku jinak untriggered UpdatePanel. |

Následníky značek:

| **Značka** | **Popis** |
| --- | --- |
| &lt;ContentTemplate&gt; | Určuje kód, který se má použít k vykreslení výsledek částečné vykreslování. Podřízená položka &lt;asp: UpdatePanel&gt;. |
| &lt;Triggery&gt; | Určuje kolekci *n* ovládací prvky přidružené k aktualizaci této UpdatePanel. Podřízená položka &lt;asp: UpdatePanel&gt;. |
| &lt;asp:AsyncPostBackTrigger&gt; | Určuje aktivační událost, která volá vykreslování částečná stránka pro danou UpdatePanel. To mohou nebo nemusí být ovládacího prvku jako následníka UpdatePanel nejistá. Podrobné název události. Podřízená položka &lt;aktivační události&gt;. |
| &lt;asp:PostBackTrigger&gt; | Určuje ovládací prvek, který způsobí, že celá stránka aktualizovat. To mohou nebo nemusí být ovládacího prvku jako následníka UpdatePanel nejistá. Podrobné k objektu. Podřízená položka &lt;aktivační události&gt;. |

`UpdatePanel` Ovládací prvek je ovládací prvek, který vymezuje serverové obsah, který se účastní funkci částečným vykreslením rozšíření AJAX. Neexistuje žádné omezení počtu UpdatePanel ovládací prvky, které mohou být na stránce, a mohou být použity. Každý UpdatePanel je izolovaný, tak, aby každý může nezávisle pracovat (může mít dvě komponenty UpdatePanel ve stejnou dobu, systémem vykreslování nezávisle na stránku postback různé části stránky).

UpdatePanel řídit především obchody s aktivační události ovládacího prvku – ve výchozím nastavení jsou všechny ovládací prvky obsažené v prvku UpdatePanel `ContentTemplate` vytvářející zpětné volání pro UpdatePanel registrován jako trigger. To znamená, že UpdatePanel můžete pracovat ovládací prvky vázané na data výchozí (například GridView), s uživatelské ovládací prvky a jejich lze naprogramovat ve skriptu.

Ve výchozím nastavení když se aktivuje vykreslování části stránky, se aktualizují všechny ovládací prvky UpdatePanel na stránce, určuje, zda UpdatePanel definované aktivačních událostí pro tyto akce. Například pokud jeden UpdatePanel definuje ovládacího prvku tlačítko a po kliknutí na tento ovládací prvek, všechny ovládací prvky UpdatePanel na této stránce se aktualizují ve výchozím nastavení. Důvodem je, že ve výchozím nastavení, `UpdateMode` vlastnost prvku UpdatePanel je nastavena na `Always`. Alternativně může nastavit vlastnost UpdateMode na `Conditional`, což znamená, že UpdatePanel bude obnovit jen v případě, že je dosáhl konkrétní aktivační událost.

## <a name="custom-control-notes"></a>Poznámky k vlastního ovládacího prvku

UpdatePanel mohou být přidány do jakékoli uživatelský ovládací prvek nebo vlastního ovládacího prvku; však musí obsahovat stránky, na kterém jsou tyto ovládací prvky zahrnuty také s vlastností EnablePartialRendering nastavenou na ovládací prvek ScriptManager **true**.

Jedním ze způsobů, ve kterém mohou započítat pro tento při použití vlastních ovládacích prvků je přepsat chráněného `CreateChildControls()` metodu `CompositeControl` třídy. Díky tomu můžete vložit UpdatePanel mezi podřízené položky ovládacího prvku a okolní uživatelé, pokud zjistíte, že stránka podporuje částečným vykreslením; jinak můžete jednoduše vrstvy podřízených ovládacích prvků do kontejneru `Control` instance.

## <a name="updatepanel-considerations"></a>Aspekty UpdatePanel

UpdatePanel funguje jako něco černé políčko, zabalení postback ASP.NET v kontextu JavaScript XMLHttpRequest. Existují však aspekty významně zvýšit výkon a berte v úvahu, jak z hlediska chování a rychlost. Chcete-li pochopit, jak funguje UpdatePanel, abyste mohli nejlépe rozhodnout, kdy je vhodné jeho použití, byste měli zkontrolovat AJAX exchange. Následující příklad používá stávající lokalitu a, Mozilla Firefox s příponou Firebug (Firebug zaznamená XMLHttpRequest data).

Vezměte v úvahu formulář, který mimo jiné, má PSČ textové pole, které by měl k naplnění Město a kraj pole ve formuláři nebo ovládací prvek. Tento formulář nakonec shromažďuje informace o členství, včetně názvu, adresy a kontaktní údaje uživatele. Existuje mnoho aspekty návrhu vzít v úvahu, v závislosti na požadavcích konkrétní projekt.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Kliknutím zobrazit obrázek v plné velikosti](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Kliknutím zobrazit obrázek v plné velikosti](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


V původní iteraci této aplikace bylo vytvořeno ovládacího prvku, který začlenit i metodu celého dat registrace uživatele, včetně PSČ, Město a kraj. Celý ovládací prvek byl uzavřen v rámci prvku UpdatePanel a do webového formuláře. Po zadání poštovnímu směrovacímu číslu uživatelem UpdatePanel zjistí událost (odpovídající TextChanged událost v back-end, zadáním aktivační události nebo pomocí ChildrenAsTriggers vlastností nastavenou na hodnotu true). AJAX odešle všechna pole v rámci UpdatePanel, protože zachycenou FireBug (podívejte se na diagram na pravé straně).

Jak uvádí, že snímek obrazovky, se dodávají hodnoty z každé ovládací prvek v rámci prvku UpdatePanel (v tomto případě jsou všechny prázdné), a také pole stavu zobrazení. Všechny told přes 9kb dat je odeslán, když ve skutečnosti měla pouze pět bajtů dat potřebné pro vytvoření této konkrétní žádosti. Odpovědí je i více opakovaném: celkem, 57kb se může odeslat klientovi, jednoduše se aktualizují textové pole a pole rozevíracího seznamu.

To může být také vás zajímá, zobrazíte jak prvku ASP.NET AJAX aktualizuje prezentaci. Část odpovědi UpdatePanel žádost o aktualizaci se zobrazí v zobrazení konzoly Firebug na levé straně; je speciálně formulovali oddělený kanálu řetězec, který je k rozdělení klientský skript a potom znovu sestaveny na stránce. Konkrétně nastaví prvku ASP.NET AJAX *innerHTML* vlastnost elementu HTML v klientovi, který představuje vaše UpdatePanel. Jako prohlížeč znovu generuje modelu DOM, neexistuje k mírnému zpoždění, v závislosti na množství informací, které je potřeba zpracovat.

Opětovné generování modelu DOM aktivuje počet další problémy:


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Kliknutím zobrazit obrázek v plné velikosti](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- Pokud cílených elementu HTML v rámci prvku UpdatePanel, ztratí fokus. Ano pro uživatele, kteří stisknutí klávesy Tab ukončíte textového pole PSČ, jejich další cílové by byl města textové pole. Ale po UpdatePanel aktualizovat zobrazení, formuláře by už neměly mít fokus a stisknutím klávesy Tab by byly spuštěny zvýraznění prvky výběru (například odkazy).
- Žádný druh vlastních skriptů na straně klienta se používá, není ve funkce uchováván přístupů DOM prvky, odkazy, může se stát nefunkční po částečné zpětné volání.

Komponenty UpdatePanel nejsou určeny jako řešení catch-all. Místo toho poskytují rychlé řešení pro určité situace, včetně vytváření prototypů, malé řízení aktualizace a poskytují obeznámeni rozhraní ASP.NET vývojářům, kteří mohou být obeznámeni s objektový model rozhraní .NET, ale tak méně s modelu DOM. Existuje několik možností, které může mít za následek vyšší výkon, v závislosti na scénáři aplikace:

- Zvažte použití PageMethods a JSON (JavaScript Object Notation) umožňuje vývojáři vyvolání statické metody na stránce, jako kdyby byla volaná volání webové služby. Protože tyto metody jsou statické, a bez stavu je nezbytné; volající skriptu poskytuje parametry a asynchronně vrácením výsledku.
- Zvažte použití webové služby a JSON, pokud jeden ovládací prvek se musí použít na několika místech v aplikaci. To znovu vyžaduje speciální uživatele příliš mnoho zásahů a pracuje asynchronně.

Zařadit funkce prostřednictvím webové služby nebo metody stránky má i nevýhody. Především vývojáře využívající technologii ASP.NET se obvykle mívají k sestavení malé součástí funkce do uživatelských ovládacích prvků (soubory .ascx). Metody stránky nemůže být hostovaná v těchto souborech; musí být hostované v rámci třídy stránky skutečné .aspx. Webové služby, musí být podobně hostované v rámci třídy .asmx. V závislosti na aplikaci tato architektura porušení jeden zásady odpovědnosti v tom, že dvě nebo více fyzických součástí, které mohou mít žádné nebo téměř žádné získá na ucelenosti ties teď rozloženy funkce pro jedinou komponentu.

Nakonec pokud aplikace vyžaduje, aby komponenty UpdatePanel se používají, podle následujících pokynů by měl pomůže při řešení potíží a údržby.

- **Vnořit komponenty UpdatePanel co nejméně, ne jenom v – jednotky, ale i mezi jednotky kódu.** S UpdatePanel na stránku, která zabalí ovládacího prvku, i když tento ovládací prvek také obsahuje UpdatePanel, který obsahuje další ovládací prvek, který obsahuje UpdatePanel, je třeba jednotky mezi vnoření. To pomáhá chránit zrušte prvky, které by měl být aktualizace a zabraňuje neočekávané aktualizuje podřízené komponenty UpdatePanel.
- **Zachovat *ChildrenAsTriggers* vlastnost nastavena na hodnotu false a explicitně nastavit spouštění událostí.** Použití `<Triggers>` kolekce je mnohem lepší způsob zpracování událostí a může zabránit neočekávanému chování, pomáhá s úlohy údržby a vynucení vývojář opt-in pro událost.
- **Použijte nejmenší možný jednotky k dosažení funkce.** Jak jsme uvedli v diskusi o službu PSČ, zabalení, že pouze minimální snižuje dobu na server, celkový počet zpracování a nároky na klienta a serveru Exchange, zlepší výkon.

## <a name="the-updateprogress-control"></a>Ovládací prvek UpdateProgress

## <a name="updateprogress-control-reference"></a>Řízení UpdateProgress – referenční informace

Povolená značka vlastnosti:

| **Název vlastnosti** | **Typ** | **Popis** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | String | Určuje ID UpdatePanel, který by měl tento UpdateProgress sestavy. |
| DisplayAfter | celá čísla | Určuje časový limit v milisekundách, než po zahájí Asynchronní požadavek, zobrazí se tento ovládací prvek. |
| DynamicLayout | bool | Určuje, zda je proces dynamicky vykreslen. |

Následníky značek:

| **Značka** | **Popis** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Obsahuje nastavení pro obsah, který se zobrazí, tento ovládací prvek šablonu ovládacího prvku. |

Ovládací prvek UpdateProgress vám dává míru zpětnou vazbu, aby vaši uživatelé zájmu přitom práce potřebné k přenosu na server. To může pomoct uživatelům vědět, že provádíte něco i když nemusí být viditelné, zvlášť, protože většina uživatelů se používají na stránku obnovení a zobrazuje na stavovém řádku zvýrazněte.

Jako poznámka může ovládacích prvků UpdateProgress vyskytovat kdekoli v hierarchii stránek. Však v případech, ve kterých částečné zpětného volání z podřízené UpdatePanel (kde UpdatePanel vnořená v rámci jiné UpdatePanel), zpětná vystavení této aktivační události podřízené UpdatePanel způsobí UpdateProgress šablony, který se má zobrazit pro podřízený objekt UpdatePanel stejně jako nadřazený UpdatePanel. Ale pokud aktivační událost je přímé podřízeného člena nadřazeného UpdatePanel, zobrazí se pouze šablony UpdateProgress přidružené k nadřazené.

## <a name="summary"></a>Souhrn

Rozšíření Microsoft ASP.NET AJAX jsou sofistikované produkty určené jako pomůcku při zpřístupnění webového obsahu a poskytují více možností uživatelů do webových aplikací. Jako součást rozšíření ASP.NET AJAX, ovládací prvky vykreslování částečné stránky, včetně ovládacích prvků ScriptManager, UpdatePanel a UpdateProgress jsou některé z nejvíc viditelné součástí sady nástrojů.

Součást ScriptManager integruje zřídit klienta JavaScript pro rozšíření, a také umožňuje různé součásti straně serveru a klienta, které fungují společně s minimálním vývoj investice.

Ovládací prvek UpdatePanel je zřejmá magic pole – značek v rámci prvku UpdatePanel může mít Codebehind na straně serveru a aktivovat aktualizaci stránky. Ovládací prvky UpdatePanel mohou být použity a může být závislá na ovládací prvky v jiné komponenty UpdatePanel. Ve výchozím nastavení komponenty UpdatePanel zpracovávat všechny postback vyvolané jejich následné ovládacích prvků, i když tuto funkci lze podrobně ladit, deklarativně nebo prostřednictvím kódu programu.

Při použití ovládacího prvku UpdatePanel, by měl být vědomi dopad na výkon, který může potenciálně způsobit vývojáři. Potenciální mezi ně patří webové služby a metody stránky, když je potřeba vzít v úvahu návrhu aplikace.

Provádění UpdateProgress řízení umožňuje uživatelům vědět, že uživatel nebo mu není ignorován, a že servisní požadavek se děje při stránce jinak není nic reagovat na vstup uživatele. Zahrnuje také možnost přerušit částečným vykreslením výsledky.

Tyto nástroje společně, usnadní vytváření bohatý a bezproblémové uživatelské prostředí tím, že server fungují méně zřejmé, uživatele a méně přerušení pracovního postupu.

## <a name="bio"></a>Bio

Scott Dikovat pracuje s technologií Microsoft Web od 1997 a je ředitel myKB.com ([www.myKB.com](http://www.myKB.com)) kde mu se specializuje na psaní ASP.NET na základě aplikací, které jsou zaměřené na řešení softwaru znalostní báze Knowledge Base. Scott nelze kontaktovat prostřednictvím e-mailu v [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) nebo jeho blog na [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Next](understanding-asp-net-ajax-updatepanel-triggers.md)
