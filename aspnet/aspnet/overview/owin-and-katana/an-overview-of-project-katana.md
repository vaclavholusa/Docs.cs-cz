---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Přehled projektu Katana | Dokumentace Microsoftu
author: howarddierking
description: ASP.NET Framework je už více než deset let a platformu aktivoval vývoj aplikací webů a služeb. Jako webové aplikace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 212594b503bfc17d3faa6df03e01402d2862522f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390185"
---
<a name="an-overview-of-project-katana"></a>Přehled projektu Katana
====================
podle [Howard Dierking](https://github.com/howarddierking)

> ASP.NET Framework je už více než deset let a platformu aktivoval vývoj aplikací webů a služeb. Jak se vyvinula strategie vývoje webové aplikace, rozhraní bylo rozvoj v kroku s technologií, jako je ASP.NET MVC a ASP.NET Web API. Jako vývoj webových aplikací, dalším krokem evoluční bere do světa cloud computingu, Microsoft Office project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) obsahuje základní sadu komponent do aplikace ASP.NET, což jim umožňuje být flexibilní, přenosná, odlehčený a nabízí lepší výkon – jinými slovy, projektu [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) cloudu optimalizuje vaše aplikace ASP.NET.


## <a name="why-katana--why-now"></a>Proč Katana – proč teď?

 Bez ohledu na to, zda jeden vývojář rozhraní framework nebo koncového uživatele produktu diskutuje, je důležité pochopit základní motivace pro vytváření produktu – a součástí, která zahrnuje zjištěním produkt byl vytvořen pro. ASP.NET byl původně vytvořen se dva zákazníky v úvahu.   
  
**První skupina zákazníků se vývojáři klasické ASP.** V době ASP byl primární technologií pro vytváření dynamických, s daty weby a aplikace s propojují značky a skript na straně serveru. Modul runtime ASP zadaný skript na straně serveru se sadou objekty, které abstrahovaná základní aspekty základní protokolu HTTP a webový server a pokud takové správy stavu relace a aplikace, služby přístupu k dalším ukládat do mezipaměti atd. Zatímco výkonné, klasické aplikace ASP, se stávalo problematické pro správu podle jejich zvětšoval velikost a složitost. Důvodem byla z velké části vzhledem k absenci struktura v skriptování v prostředí, spolu s duplicity kódu vyplývající z prokládání kód a značek. Pokud chcete využít výhod klasické rozhraní ASP při řešení některých z jeho výzvy, ASP.NET využil organizace kód poskytuje objektově orientované jazycích rozhraní .NET Framework také zachováním model programování na straně serveru na jaké klasické rozhraní ASP rostl vývojáři zvyklí.

**Druhé skupině cílových zákazníků pro technologii ASP.NET byl vývojářům aplikací pro Windows business.** Na rozdíl od klasického ASP vývojáři, kteří byli zvyklí na psaní kódu HTML a kód pro vygenerování další značka jazyka HTML, WinForms vývojáře (např. vývojáři VB6 před sebou) byly zvyklí možnosti času návrhu, zahrnující plátna a širokou škálu uživatele ovládací prvky rozhraní. První verze technologie ASP.NET – označované také jako "Webových formulářů" poskytuje podobné možnosti času návrhu spolu s modelem událost na straně serveru pro součásti uživatelského rozhraní a sada funkcí infrastruktury (jako je například stav zobrazení) k vytvoření bezproblémové vývojářské prostředí mezi klientem a programování na straně serveru. Webové formuláře efektivně skryli bezstavové povahy webovým rozhraním v modelu stavové událost, která byla zkušenosti vývojáře WinForms.

### <a name="challenges-raised-by-the-historical-model"></a>Výzvy vyvolané historickém modelu

**Net výsledkem bylo runtime vyspělá, plně funkční a programovací model pro vývojáře.** S, která pochází kombinujícím funkce několik významné problémy. Za prvé, rozhraní bylo **monolitické**, s logicky různorodých jednotkami funkce je úzce párované ve stejném sestavení System.Web.dll (například objekty jádra protokolu HTTP s použitím rozhraní framework webové formuláře). Za druhé, byl zahrnut jako součást větší .NET Framework, která je určena, který ASP.NET **dobu mezi aktualizacemi byl v řádu let.** Kvůli tomu bylo obtížné pro technologii ASP.NET drží neustále krok s všechny změny v rychle se vyvíjejícími vývoj pro Web. Nakonec System.Web.dll, samotné se s velkou provázaností, několika různými způsoby pro konkrétní webové možnost hostování: Internetové informační služby (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Evoluční kroky: technologie ASP.NET MVC a ASP.NET Web API

A velké množství změn se dělo ve vývoji webů! Webové aplikace se stále vyvíjejí jako řadu malých, zaměřuje součásti spíše než velké architektury. Počet komponent, jakož i frekvence, které byly vydány bylo zvýšení stále vyšší rychlostí. Bylo naprosto jasné, že by vyžadoval přizpůsobení pomocí webového rozhraní můžete získat menší, oddělení a přesněji zaměřené namísto větší a funkčně bohatšího, proto **tým ASP.NET trvalo několik evoluční kroky k povolení technologie ASP.NET jako řadu modulární webové součásti spíše než jeden rámec**.

Jednou z dřívější změny bylo nárůst oblíbenost dobře známé vzoru návrhu model-view-controller (MVC) díky webové vývojářské platformy, jako jsou Ruby on Rails. Tento styl vytváření webových aplikací daly vývojář větší kontrolu nad značkami své aplikace stále zachováním oddělení kódu a obchodní logiky, který byl jeden z prvních bodů prodeje pro technologii ASP.NET. Pro splnění požadavků pro tento styl vývoj webové aplikace, Microsoft trvalo příležitost k umístění samotný lepší v budoucnosti podle **vývoj ASP.NET MVC mimo pásmo** (a ne včetně v rozhraní .NET Framework). ASP.NET MVC byla vydána jako nezávislé ke stažení. Technický tým to zajistila flexibilitu, kterou pro doručení aktualizací mnohem častěji, než byly dřív možné.

Další hlavní shift při vývoji webové aplikace byl přesun z dynamického na generovaný serverem webové stránky ke statické počáteční kód pomocí dynamických oddílů stránce vygenerovat skript na straně klienta komunikaci **s back-endem webového rozhraní API prostřednictvím Odesílání požadavků AJAX**. Tento posun architektury pomohla pohonu vzestup webová rozhraní API a vývoj rozhraní ASP.NET Web API. Třeba v případě technologie ASP.NET MVC na verzi ASP.NET Web API poskytuje další možnost, jak vyvíjet další jako modulárnější rozhraní ASP.NET. Technický tým využil příležitosti a **vytvořené rozhraní ASP.NET Web API tak, aby měl žádné závislosti na jakýkoli z typů rozhraní framework core součástí System.Web.dll**. Tato možnost povolena dvě věci: nejprve určena, který rozhraní ASP.NET Web API může vyvíjet zcela samostatné způsobem (a může nadále rychle iterovat, protože se doručují prostřednictvím NuGet). Za druhé protože neexistovaly žádné externí závislosti pro System.Web.dll a proto žádné závislosti do služby IIS, ASP.NET Web API zahrnuta možnost spouštět v vlastního hostitele (například konzolové aplikace, služby Windows atd.)

### <a name="the-future-a-nimble-framework"></a>Do budoucna: Pohotové rozhraní

Oddělení součásti framework jeden z druhého a uvolňuje je v NuGet, může rozhraní nyní **iterovat nezávisleji a rychleji**. Kromě toho výkonná a flexibilní možnosti samoobslužné hostování webového rozhraní API, ukázalo jako velmi atraktivní pro vývojáře, kteří chtěli **malé, zjednodušené hostitele** pro služby. Ukázalo se proto atraktivní, ve skutečnosti další architektury také vám chtěli tato funkce, a to prezentované nový ověřovací kód spustili ve svém vlastním procesu hostitele základní adresy každé rozhraní, a potřebné pro správu (spuštěno, zastaveno a tak dále) nezávisle na sobě. Moderní webové aplikace obecně podporuje poskytování obsahu statického souboru, generování dynamických stránky, webové rozhraní API a další nedávno reálném-čase nebo nabízená oznámení. Očekává se, že každá z těchto služeb by měl spustit a spravovat nezávisle nebyla jednoduše reálné.

Toho, co bylo potřeba byl jeden hostování úrovní abstrakce, jakou by povolit pro vývojáře k vytvoření aplikace z celé řady různých komponent a architektury a poté spusťte tuto aplikaci na podpůrné hostitele.

## <a name="the-open-web-interface-for-net-owin"></a>Rozhraní Owin pro platformu .NET (OWIN)

 Inspirovat výhody dosahuje [Rack](http://rack.github.io/) v Ruby komunitě několik členů komunity .NET nedodá vytvořit abstrakci mezi webovými servery a součásti framework. Dva cíle návrhu pro abstrakci OWIN byly, aby byl jednoduchý a trvalo nejmíň možných závislostí na jiné typy rozhraní. Tyto dva cíle pomáhají zajistit:

- Nové součásti může snadněji vyvinuli a využívat.
- Aplikace může být více snadno přenést mezi hostiteli a potenciálně celý platformy/operační systémy.

Výsledný abstrakce sestává ze dvou prvků core. První je slovník prostředí. Tuto datovou strukturu je zodpovědná za uložení všech stavu, které jsou nezbytné pro zpracování požadavku HTTP a odpovědí, jakož i všechny stavu příslušného serveru. Slovník prostředí je definovaná následujícím způsobem:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Kompatibilní s OWIN webového serveru je zodpovědná za naplnění slovník prostředí s daty, jako jsou datové proudy textu a kolekcemi hlaviček požadavku HTTP a odpovědí. Odpovídá pak součásti aplikace nebo architekturu pro naplnění nebo aktualizujte další hodnoty do slovníku a zápisu do proudu textu odpovědi.

Kromě určení typu pro slovník prostředí, specifikace OWIN definuje seznam core páry klíč-hodnota slovníku. Například následující tabulka uvádí požadované slovník klíčů pro požadavek protokolu HTTP:

| Název klíče | Hodnota Popis |
| --- | --- |
| `"owin.RequestBody"` | Stream s tělo požadavku, pokud existuje. Stream.Null může sloužit jako zástupný symbol, pokud není k dispozici není datová část požadavku. Zobrazit [text žádosti](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>` Hlaviček požadavků. Zobrazit [záhlaví](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | A `string` obsahující metoda požadavku HTTP požadavku (například `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | A `string` obsahující cestu požadavku. Cesta musí být relativní vzhledem k "root" delegáta aplikace. Zobrazit [cesty](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | A `string` obsahující část cestu požadavku odpovídající na "root" delegáta aplikace; viz [cesty](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | A `string` obsahující název protokolu a verze (například `"HTTP/1.0"` nebo `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | A `string` obsahující komponenty řetězec dotazu bez úvodního požadavku HTTP identifikátor URI, "?" (například `"foo=bar&baz=quux"`). Hodnota může být prázdný řetězec. |
| `"owin.RequestScheme"` | A `string` obsahující schéma identifikátoru URI použitá u žádosti (například `"http"`, `"https"`); viz [schéma identifikátoru URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

Druhý klíčovým prvkem OWIN je delegát aplikace. Toto je signatura funkce, která slouží jako primární rozhraní mezi všemi komponentami v aplikaci OWIN. Definice delegáta aplikace vypadá takto:

`Func<IDictionary<string, object>, Task>;`

Delegát aplikace potom je jednoduše implementace typ delegáta Func, ve kterém funkce přijímá slovník prostředí jako vstup a vrátí úkol. Tento návrh obsahuje několik důsledky pro vývojáře:

- Existuje velmi malý počet závislostí typu, abyste mohli napsat komponenty OWIN. Tím se značně zvyšuje usnadnění OWIN pro vývojáře.
- Asynchronní návrh umožňuje abstrakce účinný s jeho zpracování výpočetních prostředků, zejména v další operace náročné na vstupně-výstupních operací.
- Protože aplikace delegát je atomickou jednotku provádění a protože slovník prostředí provádí jako parametr delegátu, komponenty OWIN dají se snadno dohromady a vytvoří komplexní zpracování kanály HTTP.

Z hlediska implementace, je OWIN specifikace ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Jeho cílem je neměl by se další webovou architekturu, ale spíše specifikace pro interakci webové servery a webové platformy.

Pokud jste prozkoumat [OWIN](http://owin.org/) nebo [Katana](https://github.com/aspnet/AspNetKatana/wiki), můžete také všimnout [balíček Owin NuGet](http://nuget.org/packages/Owin) a Owin.dll. Tato knihovna obsahuje jedno rozhraní, [objekt IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), která aplikaci umožňuje formalizovat a kodifikuje pořadí spouštění je popsáno v [části 4](http://owin.org/html/owin.html#4-application-startup) specifikace OWIN. Přestože se nevyžaduje, aby bylo možné sestavovacích serverů OWIN, [objekt IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) rozhraní poskytuje odkaz na konkrétní bod a je používána součásti projektu Katana.

## <a name="project-katana"></a>Projektu Katana

Vzhledem k tomu i [OWIN](http://owin.org/html/owin.html) specifikaci a *Owin.dll* jsou vlastněné community a spustit úsilí opensourcové komunity [Katana](https://github.com/aspnet/AspNetKatana/wiki) projektu představuje sadu OWIN součásti, které při stále jako software open source jsou integrované a vydaných společností Microsoft. Tyto součásti zahrnují jak komponent infrastruktury, jako je například hostitelů a serverů, jakož i funkční součásti, jako jsou komponenty ověřování a vazby pro platformy, jako [SignalR](../../../signalr/index.md) a [rozhraní ASP.NET Web Rozhraní API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Projekt má následující tři základní cíle: 

- **Přenosné** – komponenty by měli být schopni jednoduše nahradí nových komponent, jakmile budou k dispozici. To zahrnuje všechny typy komponent z rozhraní pro server a hostitelů. Důsledkem tohoto cíle je, že rozhraní třetích stran je bez obav spouštět na servery Microsoftu při rozhraní Microsoft může potenciálně spustit na servery třetích stran a hostitele.
- **Flexibilní a modulární**– na rozdíl od mnoha architektur, mezi které patří velkého počtu funkcí, které jsou ve výchozím nastavení zapnutá, by měla být součástí projektu Katana malém rozsahu a zaměřeně, poskytuje kontrolu vývojáři aplikace při určování, které součásti použijte ve své aplikaci.
- **Zjednodušené/výkonné a škálovatelné** – tím, že rozkládají tradiční pojem rozhraní do sady malých, zaměřuje komponenty, které jsou explicitně přidána vývojář aplikace výsledné aplikace Katana může využívat míň computingu prostředky a proto zpracovávat větší zatížení než s jinými typy serverů a architekturu. Jako požadavky aplikace vyžadují další funkce ze základní infrastruktury, ty lze přidat do kanálu OWIN, ale, který by měl být explicitní rozhodnutí ze strany vývojáře aplikace. Kromě toho nabídky nižší úrovně součásti znamená, že, jakmile budou k dispozici, nové servery vysoký výkon můžete bez problémů zavést ke zlepšení výkonu aplikací OWIN bez narušení těchto aplikací.

## <a name="getting-started-with-katana-components"></a>Začínáme s komponentami Katana

Kdy bylo poprvé dostupné, jedním z aspektů [Node.js](http://nodejs.org/) architektura, která okamžitě upozornila lidově byl jednoduchost, pomocí kterého jeden může vytvořit a spustit webový server. Pokud byly uvedeny Katana cíle ohledem na [Node.js](http://nodejs.org/), může jedna shrnout chci říct, že Katana přináší řadu výhod [Node.js](http://nodejs.org/) (a architektur, jako je třeba) bez vynucení pro vývojáře k zahození všechno, co ví o vývoji webových aplikací ASP.NET. Pro tento příkaz pro uložení true Začínáme se službou projektu Katana by měl být stejně jednoduchá ze své podstaty k [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Vytváření "Hello World!"

Jeden pozoruhodný rozdíl mezi vývojem v jazyce JavaScript a .NET je přítomnost (nebo nepřítomnost) kompilátoru. V důsledku toho je výchozím bodem pro jednoduchý server Katana projektu sady Visual Studio. Ale můžeme začít s nejvíce minimální typů projektů: Prázdná webová aplikace ASP.NET.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

V dalším kroku se nainstaluje [Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) balíček NuGet do projektu. Tento balíček poskytuje serveru OWIN, který běží v kanálu požadavku ASP.NET. Můžete najít na [Galerie NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) a můžete ho nainstalovat pomocí dialogové okno Správce balíčků sady Visual Studio nebo konzoly Správce balíčků pomocí následujícího příkazu:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Instalace `Microsoft.Owin.Host.SystemWeb` balíčku se nainstaluje několik dalších balíčků jako závislosti. Jedním z těchto závislostí je `Microsoft.Owin`, knihovnu, která poskytuje několik pomocné rutiny typy a metody pro vývoj aplikací OWIN. Můžeme použít tyto typy rychle psát následující server "hello world".

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Tato velmi jednoduchý webový server se teď dají spouštět pomocí sady Visual Studio **F5** příkazů a obsahuje plnou podporu pro ladění.

## <a name="switching-hosts"></a>Přepínání hostitele

Ve výchozím nastavení spouští v předchozím příkladu "hello world" v požadavku kanálu ASP.NET, která používá System.Web v rámci služby IIS. To může sám přidat skvělé možnosti jako umožňuje nám zvýšit výhody flexibility a skládání kanál OWIN s možností správy a celkovou vyspělosti služby IIS. Může však být případech, kdy výhody poskytované službou IIS se nevyžadují a že je požadováno je menší, odlehčenější hostitele. Co je potřeba, pak ke spuštění náš jednoduchý webový server mimo službu IIS a System.Web?

Pro ilustraci cílem přenositelnost, přesouvání z hostitele webového serveru na příkazovém řádku hostitele vyžaduje pouhým přidáním nové závislosti serveru a hostitele do výstupní složky projektu a pak spuštěním hostitele. V tomto příkladu budeme hostovat zde náš webový server na hostiteli Katana volá `OwinHost.exe` a použije na základě Katana HttpListener server. Podobně do jiných komponent Katana tyto bude možné získat z NuGet pomocí následujícího příkazu:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Z příkazového řádku, můžeme pak přejděte do kořenové složky projektu a spusťte `OwinHost.exe` (který byl nainstalován ve složce Nástroje, jeho odpovídající balíček NuGet). Ve výchozím nastavení `OwinHost.exe` je nakonfigurován k vyhledání serveru na základě HttpListener a proto je potřeba žádná další konfigurace. Navigace ve webovém prohlížeči na `http://localhost:5000/` ukazuje prostřednictvím konzoly teď spuštěná.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Architektura Katana

 Architektura součástí Katana rozděluje aplikaci na čtyři logické vrstvy, jak je znázorněno níže: *hostitele serveru, middleware,* a *aplikace*. Architektura součástí dostaneme tak, že implementace z těchto úrovní se můžou snadno nahradit, v mnoha případech bez nové kompilace aplikace.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Hostitel

 Hostitel je zodpovědný za:

- Správa základního procesu.
- Orchestrace pracovních postupů, jehož výsledkem výběr serveru a vytváření kanálu OWIN prostřednictvím které požadavky se budou zpracovávat.

  V současné době jsou 3 primární možnosti hostování pro aplikace založené na Katana:  
  
**IIS/ASP.NET**: použití standardních typů HttpModule a httpHandlers OWIN kanály můžete spustit ve službě IIS jako součást požadavku tok, který technologie ASP.NET. Po instalaci balíčku Microsoft.AspNet.Host.SystemWeb NuGet do projektu webové aplikace je povolena podpora hostování technologie ASP.NET. Kromě toho IIS funguje jako hostitel a server, je v tomto balíčku NuGet, což znamená, že pokud se používá hostitel SystemWeb, vývojář nelze nahradit implementace alternativní server conflated rozlišení serveru/hostitele OWIN.  
  
**Vlastní hostitel**: Sada komponent The Katana umožňuje vývojáři k hostování aplikací ve své vlastní proces, zda je konzolová aplikace, služby Windows, atd. Tato funkce bude vypadat nějak hostování na vlastním serveru funkce poskytované službou webového rozhraní API. Následující příklad ukazuje vlastní hostitel kódu webového rozhraní API:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Hostování na vlastním nastavení Katana aplikace se podobá:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Jeden pozoruhodný rozdíl mezi webovým rozhraním API a sadou Katana hostování na vlastním serveru příklady je chybí konfigurační kód webového rozhraní API z příkladu Katana hostování na vlastním serveru. Chcete-li povolit přenositelnost a skládání, Katana odděluje kód, který se spustí na serveru z kódu, který nakonfiguruje kanál zpracování požadavku. Kód, který nakonfiguruje webové rozhraní API a pak je obsažen ve třídě spuštění, který je také zadán jako parametr typu v WebApplication.Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

Třída při spuštění bude popsány podrobněji dále v tomto článku. Kód však potřebná ke spuštění Katana hostování na vlastním procesu vypadá velmi podobají kódu, který budete používat ještě dnes v aplikacích ASP.NET Web API hostování na vlastním serveru.

**OwinHost.exe**: zatímco některé bude zapotřebí vytvořit vlastní proces, který ke spouštění Katana webových aplikací, mnoho přejete jednoduše spustit předem připravené spustitelný soubor, který můžete spustit server a spustit své aplikace. Pro tento scénář zahrnuje sada komponent Katana `OwinHost.exe`. Při spuštění z v kořenovém adresáři projektu, tento spustitelný soubor spustit server (používá HttpListener server ve výchozím nastavení), který se používají konvence pro vyhledání a spuštění třídy pro spuštění uživatele. Spustitelný soubor pro podrobnější řízení, poskytuje řadu další parametry příkazového řádku.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Server

 Hostitel je zodpovědný za spouštění a údržbu procesu, ve kterém je aplikace spuštěná, odpovědnost serveru je otevřít soket sítě naslouchat požadavkům a odeslání přes kanál OWIN součásti zadanou uživatelem (jako je jste si už všimnout, je zadán tento kanál ve třídě vývojář aplikace po spuštění). V současné době projektu Katana zahrnuje dvě implementace serveru: 

- **Microsoft.Owin.Host.SystemWeb**: Jak už jsme zmínili, služba IIS v souladu s kanálu ASP.NET funguje jako hostitel a server. Proto při výběru to možnost hostování, služba IIS jak spravuje podrobnostmi na úrovni hostitele jako je aktivace procesu a čeká na požadavky HTTP. Pro webové aplikace ASP.NET pak odešle požadavky do kanálu ASP.NET. Hostitel Katana SystemWeb zaregistruje HttpModule technologie ASP.NET a httpHandlers účelem zachycení požadavků tak, jak probíhá přes kanál protokolu HTTP a odesílat je prostřednictvím kanálu OWIN zadané uživatelem.
- **Microsoft.Owin.Host.HttpListener**: jak název naznačuje, tento server Katana používá třídy rozhraní .NET Framework HttpListener otevřít soket a odesílat požadavky do kanálu OWIN určených pro vývojáře. Toto je momentálně výchozí výběr serveru pro hostování na vlastním serveru API Katana i OwinHost.exe.

## <a name="middlewareframework"></a>Middleware a rozhraní

 Jak už jsme zmínili Pokud server přijímá žádosti od klientů, je pověřený procházejících skrz kanál OWIN komponent, které jsou určeny pro vývojáře při spuštění kódu. Tyto komponenty kanálu se označuje jako middlewaru.  
 Na nejzákladnější úrovni komponentu OWIN middleware jednoduše musí implementovat delegáta aplikace OWIN tak, aby se volat.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Však zjednodušit vývoj a skládání middlewarových komponent Katana podporuje několik typů pomocné rutiny a konvence middlewarových komponent. Nejběžnější z nich je `OwinMiddleware` třídy. Komponentu vlastní middleware vytvořené pomocí této třídy by vypadalo podobně jako následující: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Tato třída je odvozena z `OwinMiddleware`, implementuje konstruktor, který přijímá instanci další middleware v kanálu jako jeden z jejích argumentů a předá ho do konstruktor základní třídy. Další argumenty použité pro konfiguraci middlewaru jsou také deklarovány jako parametry konstruktoru za parametr další middleware.   
  
Za běhu, middleware provádí prostřednictvím přepsané `Invoke` metody. Tato metoda přebírá jeden argument typu `OwinContext`. Tento objekt kontextu pochází od `Microsoft.Owin` balíček NuGet popsané výše a poskytuje přístup silného typu slovníku požadavku a odpovědi a prostředí, spolu s několika typy další pomocné rutiny.   
  
Třída middlewaru můžete snadno přidat do kanálu OWIN v kódu při spuštění aplikace následujícím způsobem:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Protože infrastruktury Katana jednoduše vytvoří kanálu OWIN middleware komponenty a komponenty stačí pro podporu delegáta aplikace k účasti v kanálu, middlewarových komponent může být v rozsahu od jednoduché Protokolovací nástroje, které celý architektury, jako je ASP.NET, webové rozhraní API, nebo [SignalR](../../../signalr/index.md). Přidání webového rozhraní API ASP.NET do kanálu OWIN předchozí například vyžaduje, přidáním následujícího kódu po spuštění:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Infrastruktura Katana sestaví kanálu middlewarových komponent v pořadí, ve kterém byly přidány do objektu IAppBuilder v metodě konfigurace. V našem příkladu pak můžete LoggerMiddleware zpracování všech požadavků, které budou plout prostřednictvím kanálu bez ohledu na to, takže v konečném důsledku zpracování těchto požadavků. To umožňuje využívat výkonné scénáře, ve kterém komponenta middlewaru (například komponenta ověřování) zpracovávat požadavky pro kanál, který obsahuje více komponent a architektury (například webového rozhraní API ASP.NET SignalR a statický souborový server).
 
## <a name="applications"></a>Aplikace

Jak je znázorněno v předchozích příkladech, OWIN a Katana projektu by neměl být představit jako nový model programování aplikací, ale spíše jako o abstrakci oddělit aplikačních programovacích modelů a architektur ze serveru a infrastruktury hostování. Například při sestavování aplikace webového rozhraní API, rozhraní pro vývojáře bude nadále používat rozhraní ASP.NET Web API, bez ohledu na to, jestli je aplikace spuštěná v používání komponent z projektu Katana kanálu OWIN. Na jednom místě, kde budou viditelné pro vývojáře aplikací OWIN související kód bude spouštěcímu kódu aplikace, kde vývojáři kontextovému kanálu OWIN. V kódu při spuštění k registraci vývojáře řadu UseXx příkazů, obvykle jednu pro každou komponentu middlewaru, který bude zpracovávat příchozí požadavky. Toto prostředí budou mít stejný účinek jako registrace modulů HTTP v aktuální světě System.Web. Obvykle větší framework middleware, jako je například rozhraní ASP.NET Web API nebo [SignalR](../../../signalr/index.md) se zaregistruje na konci kanálu. Společné middlewarových komponent, třeba kroky týkající se ověřování nebo ukládání do mezipaměti, jsou obecně registrované spíš na začátku kanálu tak, aby se budou zpracovávat požadavky pro všechny architektury a komponenty zaregistrovaný později v kanálu. Toto oddělení middlewarových komponent od sebe navzájem a ze základní komponenty infrastruktury umožňuje součásti neustálý vývoj na různých pestrosti při zajištění stabilní celého systému.

## <a name="components--nuget-packages"></a>Komponenty – balíčky NuGet

Stejně jako mnoho aktuální knihoven a architektur jsou součástí projektu Katana dodávány jako sadu balíčků NuGet. Graf závislosti balíčku Katana pro nadcházející verzi 2.0, vypadá takto. (Kliknutím na bitovou kopii pro větší zobrazení.)

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Téměř všechny balíčky v projektu Katana závisí, přímo nebo nepřímo, balíček Owin. Možná si Vzpomínáte, že toto je balíček, který obsahuje objekt IAppBuilder rozhraní, které poskytuje konkrétní implementaci spouštění aplikace je popsáno v části 4 specifikace OWIN. Kromě toho mnoho balíčků závisí na Microsoft.Owin, který obsahuje sadu typů pomocné rutiny pro práci s požadavky a odpovědi HTTP. Zbývající část balíček dají považovat za hostování balíčků infrastruktury (servery nebo hostitele) nebo middlewaru. Balíčky a závislosti, které jsou externí vzhledem k projektu Katana se zobrazí oranžově.

Infrastruktury hostování Katana 2.0 obsahuje SystemWeb a servery s HttpListener, OwinHost balíčku pro spouštění aplikace OWIN pomocí OwinHost.exe a balíček Microsoft.Owin.Hosting aplikace OWIN v hostování na vlastním vlastního hostitele (například konzolové aplikace, služby Windows, atd.)

2.0 Katana middlewarových komponent primárně zaměřen na poskytovali různých způsob ověřování. Jedna komponenta další middleware k diagnostice je k dispozici, které přinášejí podporu pro spuštění a chybové stránky. Růstem do de facto stane abstrakce hostování OWIN ekosystému middlewarových komponent, i těch vyvinutá společností Microsoft a třetích stran, se také zvýší v čísle.

## <a name="conclusion"></a>Závěr

 Od jeho začátku cíle projektu Katana nebyla k vytvoření a tím vynutit vývojáře a zjistěte další webové rozhraní. Místo toho cílem bylo vytvořit abstrakci pro vývojáře .NET webových aplikací poskytují více možností než dříve bylo možné. Pomocí rozdělení logické vrstvy typické zásobníku webové aplikace do sady replaceable komponenty, umožňuje projektu Katana komponenty v rámci zásobníku ke zlepšení v libovolné míra dává smysl pro tyto součásti. Vytvořením všechny součásti kolem jednoduché abstrakce OWIN Katana umožňuje rozhraní a aplikací vybudovaných nad nich jeho přenositelnost napříč celou řadu různých serverů a hostitelů. Vložením vývojář v ovládacím prvku zásobníku Katana zajistí, že vývojář znamená, že ultimate výběr o tom, jak jednoduché nebo jak bohaté na funkce by měla být zásobníku svůj Web.  
  

## <a name="for-more-information-about-katana"></a>Další informace o Katana

- Katana projektu na Githubu: [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/).
- Video: [projektu Katana - OWIN pro technologii ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), podle Howard Dierking.

## <a name="acknowledgements"></a>Potvrzení

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick je vedoucí pro programování zápis pro společnost Microsoft, zaměřuje se na Azure a MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
