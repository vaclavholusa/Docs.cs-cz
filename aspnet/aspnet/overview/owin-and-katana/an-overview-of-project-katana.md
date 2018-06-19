---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Přehled Katana projekt | Microsoft Docs
author: howarddierking
description: Rozhraní ASP.NET je k dispozici více než deset let a platformou aktivoval vývoj obrovském množství webů a služeb. Jako webové aplikace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 3c2bcbbc6e506af759f6d77af17d015278cc0bdf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878760"
---
<a name="an-overview-of-project-katana"></a>Přehled Katana projektu
====================
podle [Howard Dierking](https://github.com/howarddierking)

> Rozhraní ASP.NET je k dispozici více než deset let a platformou aktivoval vývoj obrovském množství webů a služeb. Jako webové aplikace vývoj strategie vyvinuly, byl rozhraní moct rozvoj v souladu s technologie, jako je technologie ASP.NET MVC a ASP.NET Web API. Jako vývoj webových aplikací, dalším krokem evolučním bere do světa cloud computing, projektu [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) poskytuje základní sada součástí aplikace ASP.NET, tím jim umožníte být flexibilní, přenosné, prosté a poskytují lepší výkon – jinými slovy, projektu [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) cloudu optimalizuje vaše aplikace ASP.NET.


## <a name="why-katana--why-now"></a>Proč Katana – proto nyní?

 Bez ohledu na to zda jeden diskutuje vývojáře framework nebo koncového uživatele produktu, je důležité si uvědomit, základní motivace pro vytvoření produktu – a součástí, která zahrnuje zjištěním produktu vytvořený pro. ASP.NET byl původně vytvořen se dvěma zákazníky v paměti.   
  
**První skupinu zákazníků se vývojáři classic ASP.** V době byl ASP jednu primární technologie pro vytváření dynamické a datové weby a aplikace podle propojují značek a skripty na straně serveru. Modul runtime ASP zadat skript na straně serveru sadu objektů, které abstrahované základní aspekty základního protokolu HTTP a webový server a zadaný přístup k další služby takové správy stavu relace a aplikace, do mezipaměti, atd. Při výkonné, stala klasické aplikace ASP může být obtížné spravovat jejich vzrostla v velikost a složitost. Důvodem byla z velké části chybí struktura nalezena ve vytváření skriptů prostředí, v kombinaci s duplikace kódu vyplývající z prokládání kódu a kódu. Aby bylo možné využít při adresování některé jeho problémy síly klasické ASP, ASP.NET trvalo výhod kód organizace poskytované objektově orientované jazyky rozhraní .NET Framework při zachování také programovací model na straně serveru do které klasické ASP měl zvětšil uzpůsobené vývojáři.

**Druhé skupině cílových zákazníků pro technologii ASP.NET se vývojáři obchodní aplikace systému Windows.** Na rozdíl od classic ASP vývojáři, kteří byli uzpůsobené pro zápis kódu HTML a kód ke generování další značka jazyka HTML, vývojáři WinForms (jako vývojáři VB6 před sebou) byly uzpůsobené pro prostředí pro dobu návrhu, které obsahovala na plátno a širokou škálu uživatele ovládací prvky rozhraní. První verzi technologie ASP.NET – taky známé jako "Webových formulářů" zadaný na podobném principu čas návrh spolu s modelem událost na straně serveru pro součásti uživatelského rozhraní a sada funkcí infrastruktury (třeba ViewState) k vytvoření prostředí bezproblémové vývojáře mezi klientem a programování na straně serveru. Webové formuláře efektivně hid bezstavové povaze Web v rámci modelu stavová událost, která byla pro vývojáře WinForms.

### <a name="challenges-raised-by-the-historical-model"></a>Výzvy aktivováno historickém modelu

**Net výsledek byl vyspělá, bohaté funkce runtime a vývojáře programovací model.** S která pocházejí funkce zvučnost několik významné problémy. Za prvé, rozhraní se **monolitický**, s logicky různorodých jednotky funkce je úzce párované ve stejném sestavení System.Web.dll (například objekty jádra protokolu HTTP pomocí rozhraní webových formulářů). Za druhé, ASP.NET byl zahrnut jako součást větší rozhraní .NET Framework, to znamená, že **doba mezi verzemi byla v řádu let.** Z tohoto důvodu bylo obtížné pro technologii ASP.NET držet krok se všechny změny děje v rychle vyvíjejí vývoj webů. Nakonec se System.Web.dll, samotné doplněná několika různými způsoby k určitým webovým možností hostování: Internetové informační služby (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Evolučním kroky: technologie ASP.NET MVC a ASP.NET Web API

A velké množství změn se děje v vývoj webů! Webové aplikace byly stále vyvíjených jako řadu malé, zaměřuje součásti spíše než velké architektury. Počet součásti a také četnost, se kterým byly vydané byl zvýšení někdy rychlejší tempem. Bylo naprosto jasné, že přizpůsobení se webu by vyžadovaly architektury získat menší, odpojeného a přesněji zaměřené místo větší a více funkcí, proto **ASP.NET team trvalo několik evolučním kroky k povolení technologie ASP.NET, jako je rodina modulární webové komponenty spíše než jeden framework**.

Zvýšení popularita dobře známé vzoru návrhu model-view-controller (MVC) díky architektury pro vývoj webových jako Ruby, na které byl jeden ze časná změny. Tento styl tvorbu webových aplikací Dal vývojář větší kontrolu nad jeho aplikace značek při zachování stále oddělení značek a obchodní logiku, která byla mezi počáteční body prodeji pro technologii ASP.NET. Pro splnění těchto požadavků pro tento styl vývoj webových aplikací, Microsoft trvalo možnost umístit samotné lepší do budoucna podle **vývoj ASP.NET MVC vzdáleně** (a bez zahrnutí v rozhraní .NET Framework). ASP.NET MVC byla vydána jako nezávislé soubor ke stažení. To dalo technického týmu flexibilitu k poskytování aktualizací mnohem častěji, než bylo dřív možné.

Další hlavní shift při vývoji webové aplikace byla posunu dynamické a generované serverem webové stránky statické počáteční značka s dynamické částech stránky generované komunikaci klientský skript **s back-end webovým rozhraním API prostřednictvím Požadavky AJAX**. Tato architektury shift pomohl pohonu zvýšení webové rozhraní API a vývoje rozhraní ASP.NET Web API. Jako v případě rozhraní ASP.NET MVC verzi rozhraní ASP.NET Web API zadat jinou možnost vyvíjí další jako více modulární rozhraní ASP.NET. Technický tým trvalo výhod možnost a **vytvořené rozhraní ASP.NET Web API tak, aby měl žádné závislosti na jakýkoli z typů framework core najít v System.Web.dll**. Tato možnost povolena dvě věci: nejdřív, který smyslem webového rozhraní API ASP.NET může vyvíjet zcela samostatné způsobem (a může pokračovat k iteraci rychle, protože je dodán prostřednictvím balíčku NuGet). Druhý protože neexistovaly žádné externí závislosti na System.Web.dll a proto žádné závislosti pro službu IIS, ASP.NET Web API zahrnuta možnost spustit v vlastního hostitele (například Konzolová aplikace služby systému Windows, atd.)

### <a name="the-future-a-nimble-framework"></a>Budoucí: Hbitější rozhraní

Oddělení součástí architektury od sebe navzájem a pak je uvolněné NuGet, architektury může nyní **iteraci více nezávisle a rychleji**. Kromě toho prokázat velmi přitažlivé pro vývojáře, kteří chtěli výkon a flexibilitu schopnosti samoobslužné hostování webového rozhraní API **malé, lightweight hostitele** pro své služby. Ukázalo se proto atraktivní, ve skutečnosti, aby další architektury také chtěli tato funkce a to prezentované nový ověřovací kód, že každý framework byla spuštěna ve svém vlastním procesu hostitele na vlastní základní adresu a potřebné ke správě (spuštěna, zastavena, atd.) nezávisle. Moderní webové aplikace podporuje obecně slouží statický soubor, generování dynamické stránky, webového rozhraní API a více nedávno real-bránu nebo nabízená oznámení. Očekává se, že každý z těchto služeb by měla spouštět a spravovat nezávisle nebyla jednoduše realistické.

Co je potřeba se jeden hostování abstrakce, který by umožňují vývojáři tvoří aplikace celou řadu různých komponent a rozhraní a spusťte tuto aplikaci podporující hostiteli.

## <a name="the-open-web-interface-for-net-owin"></a>Otevřete webové rozhraní pro platformu .NET (OWIN)

 INSPIROVANÉ výhody dosáhnout [Rack](http://rack.github.io/) v Ruby komunity několik členové komunity služby .NET stanoveným vytvořit abstrakci mezi webovými servery a součástí architektury. Dva cíle návrhu pro abstrakci OWIN byly bylo jednoduché a trvalo nejmenší počet možných závislostí na jiné typy framework. Tyto dva cíle zajistit:

- Nové komponenty může být snadněji vyvinuté a využívat.
- Aplikace může být více snadno přenášet mezi hostiteli a potenciálně celý platformy/operační systémy.

Výsledný abstrakci sestává ze dvou prvků jádra. První je slovník prostředí. Tato struktura dat je zodpovědná za uložení všech stavu, které jsou nezbytné pro zpracování požadavku HTTP a odpovědi, jakož i jakékoli stavu příslušného serveru. Slovník prostředí je definován následujícím způsobem:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Kompatibilní s OWIN webovém serveru je zodpovědná za naplnění slovník prostředí s daty, jako jsou datové proudy textu a kolekcemi hlaviček požadavku HTTP a odpovědi. Pak je zodpovědností aplikace nebo framework součásti, které naplnit nebo aktualizovat slovníku s další hodnoty a zápisu do proudu textu odpovědi.

Kromě určení typu pro slovník prostředí, specifikace OWIN definuje seznam párů klíčových hodnot základní slovník. Například následující tabulka uvádí požadované slovníku klíče pro požadavek HTTP:

| Název klíče | Hodnota Popis |
| --- | --- |
| `"owin.RequestBody"` | Datový proud s textu požadavku, pokud existuje. Stream.Null může použít jako zástupný znak, pokud neexistuje žádný text žádosti. V tématu [text žádosti](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>` Hlaviček požadavku. V tématu [hlavičky](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | A `string` obsahující metoda požadavku HTTP požadavku (například `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | A `string` obsahující cestu požadavku. Cesta musí být relativní vzhledem k "root" delegáta aplikace; v tématu [cesty](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | A `string` obsahující část odpovídající na "root" delegáta aplikace; Cesta požadavku najdete v části [cesty](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | A `string` obsahující protokolu název a verze (například `"HTTP/1.0"` nebo `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | A `string` komponenty řetězec dotazu bez úvodního požadavku HTTP identifikátor URI, který obsahuje "?" (například `"foo=bar&baz=quux"`). Hodnota může být prázdný řetězec. |
| `"owin.RequestScheme"` | A `string` obsahující schéma identifikátoru URI, která jsou použita pro požadavek (například `"http"`, `"https"`); najdete v části [schéma identifikátoru URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

Druhý klíče elementu OWIN je delegáta aplikace. Toto je funkce podpisu, který slouží jako primárním rozhraním mezi všemi komponentami v aplikaci OWIN. Definice delegáta aplikace je následující:

`Func<IDictionary<string, object>, Task>;`

Delegát aplikace je jednoduše implementace typu Func delegáta, kde funkce přijímá slovník prostředí jako vstup a vrátí úlohu. Tento návrh má několik důsledky pro vývojáře:

- Je velmi malý počet závislostí typu požadované za účelem zápisu komponenty OWIN. Tím se značně zvyšuje usnadnění OWIN pro vývojáře.
- Asynchronní návrh umožňuje abstrakci účinný s jeho zpracování výpočetních prostředků, zejména v další operace náročné na vstupně-výstupní operace.
- Protože delegát aplikace je atomic jednotkou provádění a protože slovník prostředí probíhá jako parametr na delegáta, komponenty OWIN možné snadno zřetězit dohromady a vytvoří komplexní HTTP zpracování kanály.

Z hlediska implementaci OWIN je specifikace ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Jeho cílem není potřeba další webové rozhraní, ale spíš specifikace pro interakci webové platformy a webové servery.

Pokud jste prozkoumat [OWIN](http://owin.org/) nebo [Katana](https://github.com/aspnet/AspNetKatana/wiki), může také dojít k tomu [balíček Owin NuGet](http://nuget.org/packages/Owin) a Owin.dll. Tato knihovna obsahuje jednotné rozhraní [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), který aplikaci umožňuje formalizovat a codifies sekvence spouštění popsané v [část 4](http://owin.org/html/owin.html#4-application-startup) specifikace OWIN. Není požadováno k sestavení OWIN servery, [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) rozhraní představuje konkrétní referenční bod a je používána komponenty Katana projektu.

## <a name="project-katana"></a>Projekt Katana

Zatímco jak [OWIN](http://owin.org/html/owin.html) specifikaci a *Owin.dll* jsou ve vlastnictví komunity a komunity spustit úsilí s otevřeným zdrojem, [Katana](https://github.com/aspnet/AspNetKatana/wiki) projektu představuje sadu OWIN součásti, které při stále open source jsou vytvořené a vydaných společností Microsoft. Tyto součásti zahrnují součásti infrastruktury, např. hostitelů a serverů, jak funkční součásti, jako je například ověřování součásti a vazby na rozhraní, jako [SignalR](../../../signalr/index.md) a [technologie ASP.NET Rozhraní API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Projekt má následující tři nejvyšších úrovní cíle: 

- **Přenosné** – komponenty by mohli být snadno nahrazena pro nové součásti, jakmile budou k dispozici. To zahrnuje všechny typy součástí z rozhraní serveru a hostitele. Z toho vyplývá tohoto cíle je, že rozhraní třetích stran můžete bezproblémově spustit na serverech Microsoft přitom architektury Microsoft může potenciálně běží na serverech třetích stran a hostitele.
- **Modulární, flexibilní**– na rozdíl od mnoha rozhraní, které zahrnují velkého počtu funkce, které jsou ve výchozím nastavení zapnuta, součásti projektu Katana by měly být malé a zaměřují, poskytuje kontrolu pro vývojáře aplikací při určení, které součásti použití v jeho aplikaci.
- **Lightweight/původce nebo škálovatelné** – porušením tradiční představu o rozhraní do sady malá, zaměřuje součásti, které jsou přidávány explicitně vývojář aplikací si výsledné Katana aplikace můžou využívat méně computing prostředky a v důsledku toho zpracovávat další zátěž, než s jinými typy serverů a rozhraní. Jako požadavky na aplikace potřebují další funkce od základní infrastruktury, ty mohou být přidány do kanálu OWIN, ale který by měl být rozhodnutí o explicitní ze strany vývojáře aplikace. Kromě toho možnost nahrazení součástí nižší úrovně znamená, jakmile budou k dispozici, nových serverů vysoký výkon bezproblémově zavedení zlepšit výkon aplikací OWIN bez poškození těchto aplikací.

## <a name="getting-started-with-katana-components"></a>Začínáme s komponentami Katana

Pokud bylo poprvé dostupné, jeden aspekt jejich [Node.js](http://nodejs.org/) rozhraní, které okamžitě upozornila lidově byl jednoduchost, pomocí kterého jeden může vytvořit a spustit webový server. Pokud byly Katana cílů s rámečkem v ohledem na [Node.js](http://nodejs.org/), může jedna shrnout vyslovením Katana přináší řadu výhod [Node.js](http://nodejs.org/) (a architektury, jako je) bez vynucení vývojáři zahození všechno, co uživatel ví o vývoji webových aplikací ASP.NET. Pro tento příkaz pro uložení true Začínáme s projektem Katana by měla být stejně jednoduché ve své podstatě k [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Vytváření "Hello, World!"

Jeden pozoruhodný rozdíl mezi JavaScript a .NET development je přítomnost (nebo absence) kompilátor. Výchozím bodem pro jednoduché Katana serveru jako takový je projekt sady Visual Studio. Ale můžeme začít s nejvíce minimální typy projektů: prázdný webové aplikace ASP.NET.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

V dalším kroku se nainstaluje [Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) balíček NuGet do projektu. Tento balíček poskytuje serveru OWIN, který je spuštěn v požadavku kanálu ASP.NET. Je možné najít na [Galerie NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) a může ji nainstalovat prostřednictvím dialogové okno Správce balíčku sady Visual Studio nebo konzoly Správce balíčků pomocí následujícího příkazu:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Instalace `Microsoft.Owin.Host.SystemWeb` balíček nainstaluje několik další balíčky jako závislosti. Jeden z těchto závislostí je `Microsoft.Owin`, jste knihovnu, která poskytuje několik podpůrné typy a metody pro vývoj aplikací OWIN. Tyto typy jsme můžete použít k rychlému zápisu následující server "hello, world".

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Tato velmi jednoduchý webový server můžete spustit nyní pomocí sady Visual Studio **F5** příkazů a nabízí i plnou podporu pro ladění.

## <a name="switching-hosts"></a>Přepínání hostitele

Ve výchozím nastavení spouští v předchozím příkladu "hello, world" v kanálu požadavku ASP.NET, který používá System.Web v rámci služby IIS. To můžete samostatně přidat strávíte hodnotu se nám těžit z flexibilitu a composability kanálu OWIN s možností správy a celkovou vyspělosti služby IIS umožňuje. Však může být případy, kdy nejsou vyžadovány výhody poskytované službou IIS a přání je menší, více lightweight hostitele. Co je potřeba, poté ke spuštění naše jednoduchý webový server mimo službu IIS a System.Web?

Pro ilustraci cílem přenositelnost, přesouvání z hostitele webového serveru na příkazovém řádku hostitele vyžaduje stačí přidat nové závislosti server a hostitelů do výstupní složky projektu a potom spustit hostitele. V tomto příkladu jsme budete hostovat naše webového serveru v hostiteli Katana názvem `OwinHost.exe` a použije serveru na základě Katana HttpListener. Podobně jako v ostatních komponentách Katana tyto bude možné získat z NuGet pomocí následujícího příkazu:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Z příkazového řádku, můžeme pak přejděte do kořenové složky projektu a jednoduše spusťte `OwinHost.exe` (který byl nainstalován do složky nástroje pro jeho odpovídající balíčku NuGet). Ve výchozím nastavení `OwinHost.exe` nastaven tak, aby vyhledejte serveru na základě HttpListener a proto je potřeba žádná další konfigurace. Navigace ve webovém prohlížeči a `http://localhost:5000/` zobrazuje aplikace teď běžící prostřednictvím konzoly.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Architektura Katana

 Architektura součástí Katana rozděluje aplikace do čtyř logické vrstev, jak je znázorněno níže: *hostitele, server, middleware,* a *aplikace*. Architektura součástí se započítá tak, že implementace tyto vrstvy můžou snadno nahradit, v mnoha případech bez nutnosti rekompilace aplikace.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Hostitel

 Hostitel je zodpovědná za:

- Správa v podkladovém procesu.
- Orchestrace pracovního postupu, který je výsledkem výběr serveru a konstrukce kanálu OWIN prostřednictvím které požadavky se budou zpracovávat.

  V současné době jsou 3 primární možnosti hostování aplikací založených na Katana:  
  
**IIS/ASP.NET**: pomocí standardní typy HTTP a httpHandlers, OWIN kanály můžete spustit ve službě IIS jako součást tok požadavku ASP.NET. Hostování podporu technologie ASP.NET je povoleno pomocí instalace balíčku Microsoft.AspNet.Host.SystemWeb NuGet do projektu webové aplikace. Kromě toho IIS funguje jako hostitel a server, je v tomto balíčku NuGet, což znamená, že pokud se používá hostitel SystemWeb, vývojář nelze nahradit implementace alternativní server conflated rozdíl hostitele nebo server OWIN.  
  
**Vlastní hostitel**: Sada komponent Katana dává vývojář možnost hostování aplikací v svoje vlastní proces, zda se konzolovou aplikaci, služba systému Windows, atd. Tato funkce bude vypadat podobně jako funkce hostování na vlastním poskytované pomocí webového rozhraní API. Následující příklad ukazuje vlastní hostitel kódu webového rozhraní API:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Hostování na vlastním nastavení pro aplikace Katana je podobný:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Jeden pozoruhodný rozdíl mezi webového rozhraní API a Katana hostování na vlastním příklady je chybí Katana hostování na vlastním příklad kódu webového rozhraní API konfigurace. Pokud chcete povolit přenositelnosti a composability, Katana odděluje kód, který spouští server z kódu, který konfiguruje zpracování kanálu požadavku. Kód, který nakonfiguruje webového rozhraní API, pak je obsažený ve třídě, spuštění, který je navíc určený jako parametr typu v WebApplication.Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

Třída při spuštění probereme podrobněji později v článku. Kód je však nezbytné ke spuštění Katana, hostování na vlastním procesu vypadá velmi podobně jako kód, který používáte dnes v aplikacích ASP.NET Web API hostování na vlastním serveru.

**OwinHost.exe**: při některé chcete napsat vlastní proces, který ke spuštění Katana webových aplikací, mnoho přejete jednoduše spustíte předdefinovaných spustitelný soubor, který můžete spustit server a spustit svých aplikacích. Pro tento scénář zahrnuje sada komponent Katana `OwinHost.exe`. Při spuštění z v kořenovém adresáři projektu, se tento spustitelný soubor spustí server (používá HttpListener server ve výchozím nastavení) a pomocí konvence můžete najít a spustit třída uživatele při spuštění. K podrobnějšímu řízení spustitelný soubor poskytuje řadu další parametry příkazového řádku.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Server

 Zatímco hostitel je zodpovědný za spouštění a udržování procesu, ve kterém je aplikace spuštěná, zodpovědnost serveru je otevřít soketu sítě, naslouchat žádostem a jejich odeslání v kanálu OWIN součásti zadanou uživatelem (jako je mohli jste už zaznamenat, tento kanál je zadána v třídě spuštění vývojář aplikace). V současné době projekt Katana zahrnuje dvě implementace serveru: 

- **Microsoft.Owin.Host.SystemWeb**: Jak už jsme zmínili, služba IIS v souladu s kanálu ASP.NET funguje jako hostitel i server. Proto při výběru to, který je hostitelem možnost, služba IIS spravuje hrozby úrovni hostitele, jako je proces aktivace i naslouchá požadavkům HTTP. Pro webové aplikace ASP.NET pak odešle požadavky do kanálu ASP.NET. Hostitel Katana SystemWeb zaregistruje modulu HTTP ASP.NET a httpHandlers za účelem zachycení požadavků, dokud budou procházet skrz kanál protokolu HTTP a odesílat je prostřednictvím kanálu OWIN zadaného uživatelem.
- **Microsoft.Owin.Host.HttpListener**: jak uvádí, že její název, tento server Katana používá třídu rozhraní .NET Framework HttpListener otevřít soket a odesílat požadavky do kanálu OWIN vývojáře zadaný. Toto je momentálně výchozí výběr serveru pro hostování na vlastním API Katana a OwinHost.exe.

## <a name="middlewareframework"></a>Middleware/framework

 Jak jsme uvedli když server přijímá žádosti od klienta, je zodpovědná za předání prostřednictvím kanálu OWIN komponent, které jsou určené pro vývojáře spuštění kódu. Tyto součásti kanálu se označují jako middleware.  
 Velmi základní úrovni komponentu OWIN middleware jednoduše musí implementovat delegáta aplikace OWIN, aby bylo možné volat.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Ale pro zjednodušení vývoj a složení komponenty middlewaru, podporuje Katana několik názvů a typy pomocné rutiny pro komponenty middlewaru. Mezi nejběžnější z nich je `OwinMiddleware` třídy. Součást vlastní middleware vytvořené pomocí této třídy by vypadat podobně jako následující: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Tato třída odvozená z `OwinMiddleware`, implementuje konstruktor, který přijímá instanci další middleware v kanálu jako jeden z jeho argumenty a předává je pro základní konstruktoru. Další argumenty, které se používá pro konfiguraci middlewaru jsou také deklarované jako parametry konstruktor za parametr další middleware.   
  
V době běhu je provést middleware prostřednictvím přepsané `Invoke` metoda. Tato metoda přebírá jediný argument typu `OwinContext`. Od tohoto objektu kontextu `Microsoft.Owin` balíček NuGet popsané výše a poskytuje silného typu přístup do slovníku požadavku a odpovědi a prostředí spolu s několika typy další pomocné.   
  
Třída middleware lze snadno přidat do kanálu OWIN v kódu aplikace spuštění následujícím způsobem:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Protože infrastruktury Katana jednoduše vytvoří kanálu OWIN middleware komponent a protože komponenty jednoduše potřebují podporu delegáta aplikace zapojit se do kanálu, komponenty middlewaru se může pohybovat složitost z jednoduchého Protokolovací nástroje pro celý architektury, jako je ASP.NET Web API, nebo [SignalR](../../../signalr/index.md). Například přidání webového rozhraní API ASP.NET do kanálu OWIN, který předchozí vyžaduje přidání následující spuštění kódu:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Katana infrastruktury budete stavět kanálu komponenty middlewaru podle pořadí, ve kterém byly přidány k objektu IAppBuilder v metodě konfigurace. V našem příkladu pak LoggerMiddleware dokáže zpracovat všechny požadavky, které procházet skrz kanál, bez ohledu na to, jak jsou tyto požadavky nakonec zpracovávány. To umožňuje efektivní scénáře, kde komponenta middlewaru (například komponenta ověřování) zpracovávat požadavky pro kanál, který obsahuje více komponent a rozhraní (například rozhraní ASP.NET Web API, SignalR a statických souborů serveru).
 
## <a name="applications"></a>Aplikace

Jak je znázorněno v předchozích příkladech, OWIN a Katana projekt by neměl představit jako nový model programování aplikací, ale spíše jako abstrakci oddělit programovací modely aplikace a rozhraní ze serveru a hostování infrastruktury. Například při vytváření aplikace webového rozhraní API, rozhraní framework developer bude nadále používat rozhraní ASP.NET Web API, bez ohledu na to, zda je aplikace spuštěná v kanálu OWIN pomocí součásti z Katana projektu. Jednom místě, kde budou viditelné pro vývojáře aplikací OWIN související kód bude spouštěcímu kódu aplikace, kde vývojář vytvoří kanálu OWIN. V kódu spuštění bude vývojář zaregistrovat řadu UseXx příkazy, obecně jeden pro každou součást middlewaru, který bude zpracovávat příchozí požadavky. Toto prostředí bude mít stejný účinek jako registrace modulů HTTP v aktuální System.Web world. Obvykle větší framework middleware, jako je například rozhraní ASP.NET Web API nebo [SignalR](../../../signalr/index.md) se zaregistruje na konci kanálu. Komponenty middlewaru mezi vyjímání, například ověřování nebo ukládání do mezipaměti, jsou obecně registrované směrem od kanálu tak, aby se bude zpracovávat požadavky pro všechny rozhraní a komponenty registrované později v kanálu. Toto rozdělení komponenty middlewaru od sebe navzájem a z základní součásti infrastruktury umožňuje součásti, které momentální v různých jsou rychlosti a zajistit, aby zůstala stabilní celého systému.

## <a name="components--nuget-packages"></a>Součástí – balíčků NuGet

Stejně jako mnoho aktuální knihoven a architektur jsou součástí projektu Katana dodávána jako sadu balíčků NuGet. Nadcházející verze 2.0, Katana graf závislostí balíček vypadá takto. (Klikněte na bitovou kopii pro větší zobrazení).

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Téměř každý balíček v projektu Katana závisí, přímo ani nepřímo, na balíček Owin. Můžete pamatovat, že toto je balíček, který obsahuje rozhraní IAppBuilder, který poskytuje konkrétní implementaci spouštění aplikace popsané v části 4 specifikace OWIN. Kromě toho řadu balíčky závisí na Microsoft.Owin, který obsahuje sadu podpůrné typy pro práci s požadavky a odpovědi HTTP. Zbývající část balíček můžou být klasifikované jako hostování balíčků infrastruktury (servery nebo hostitele) nebo middleware. Balíčky a závislosti, které jsou externí vzhledem k projektu Katana jsou zobrazeny v oranžová.

Hostování infrastrukturu pro Katana 2.0 obsahuje SystemWeb a na základě HttpListener servery, balíček OwinHost pro spouštění aplikací OWIN pomocí OwinHost.exe a balíček Microsoft.Owin.Hosting pro samoobslužné hostování aplikací OWIN v vlastní hostitel (například konzolové aplikace služby systému Windows, atd.)

Pro Katana 2.0 jsou komponenty middlewaru primárně zaměřuje na zajištění jiný způsob ověřování. Jedna součást další middleware pro diagnostiku jsou tu, která umožňuje podporu pro spuštění a chybové stránky. S růstem OWIN do fakticky hostování abstrakci ekosystému komponenty middlewaru, i ty vyvinuty společností Microsoft a třetích stran, se taky zvýší počet.

## <a name="conclusion"></a>Závěr

 Z jeho začátku cíle projektu Katana nebyla vytvoření a tím vynutit vývojáři další ještě jiné webové rozhraní. Místo toho cílem bylo vytvoření abstrakci pro vývojáře .NET webových aplikací poskytuje další možnost volby než bylo dřív možné. Porušením až logické vrstvy typické zásobník aplikací webové do sady replaceable součástí projektu Katana umožňuje součásti v zásobníku ke zlepšení v jakémkoli míra smysl pro tyto součásti. Ve všech součástí kolem jednoduché abstrakci OWIN, umožňuje Katana architektury a aplikacím postavená na nich být přenosná napříč celou řadu různých serverech a hostitele. Vložením vývojář v ovládacím prvku zásobníku Katana zajistí, že vývojář znamená, že ultimate výběr o tom, jak lightweight nebo jak bohaté funkce by měly být zásobníku svůj Web.  
  

## <a name="for-more-information-about-katana"></a>Další informace o Katana

- Katana projektu na Githubu: [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/).
- Video: [Katana projekt – OWIN pro technologii ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), podle Howard Dierking.

## <a name="acknowledgements"></a>Potvrzení

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick je senior programovací zapisovač pro Microsoft zaměřením na Azure a MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
