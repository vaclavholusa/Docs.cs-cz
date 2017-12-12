---
uid: whitepapers/aspnet4/overview
title: "ASP.NET 4 a Visual Studio 2010 Web Development přehled | Microsoft Docs"
author: rick-anderson
description: "Tento dokument obsahuje přehled mnoha nových funkcí pro technologii ASP.NET, které jsou zahrnuté v rozhraní.NET Framework 4 a Visual Studio 2010."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 226ef83f289b8fbe9a68f0d0741c7eca0d96ba94
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 a Visual Studio 2010 Web Development přehled
====================
> Tento dokument obsahuje přehled mnoha nových funkcí pro technologii ASP.NET, které jsou zahrnuté v rozhraní.NET Framework 4 a Visual Studio 2010.
> 
> [Stáhněte si tento dokument White Paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**Obsah**

**[Základní služby](#0.2__Toc253429238 "_Toc253429238")**  
[Soubor Web.config refaktoring](#0.2__Toc253429239 "_Toc253429239")  
[Ukládání do mezipaměti výstup pro Extensible](#0.2__Toc253429240 "_Toc253429240")  
[Automatické spuštění webové aplikace](#0.2__Toc253429241 "_Toc253429241")  
[Trvalé přesměrování stránky](#0.2__Toc253429242 "_Toc253429242")  
[Zmenšení stav relace](#0.2__Toc253429243 "_Toc253429243")  
[Rozšíření rozsahu povolených adres URL](#0.2__Toc253429244 "_Toc253429244")  
[Ověření Extensible žádosti](#0.2__Toc253429245 "_Toc253429245")  
[Objekt ukládání do mezipaměti a rozšiřitelnost ukládání objektů](#0.2__Toc253429246 "_Toc253429246")  
[Rozšiřitelné HTML, adresy URL a kódování hlaviček protokolu HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Monitorování výkonu pro jednotlivé aplikace v rámci jednoho pracovního procesu](#0.2__Toc253429248 "_Toc253429248")  
[Cílení na více](#0.2__Toc253429249 "_Toc253429249")

**[AJAX](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery zahrnuté s webovými formuláři a MVC](#0.2__Toc253429251 "_Toc253429251")  
[Podpora sítě doručování obsahu](#0.2__Toc253429252 "_Toc253429252")  
[Explicitní skripty ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Webové formuláře](#0.2__Toc253429256 "_Toc253429256")**  
[Nastavení značek Meta pomocí Page.MetaKeywords a vlastnosti Page.MetaDescription](#0.2__Toc253429257 "_Toc253429257")  
[Povolení zobrazení stavu jednotlivých ovládacích prvků](#0.2__Toc253429258 "_Toc253429258")  
[Změny možnosti prohlížeče](#0.2__Toc253429259 "_Toc253429259")  
[Směrování v technologii ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Nastavení ID klienta](#0.2__Toc253429261 "_Toc253429261")  
[Zachování výběru řádku v ovládacích prvcích dat](#0.2__Toc253429262 "_Toc253429262")  
[Ovládací prvek ASP.NET graf](#0.2__Toc253429263 "_Toc253429263")  
[Filtrování dat pomocí ovládacího prvku QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Kódovaný výrazy kódu HTML](#0.2__Toc253429265 "_Toc253429265")  
[Změny v šabloně projektu](#0.2__Toc253429266 "_Toc253429266")  
[Vylepšení šablon stylů CSS](#0.2__Toc253429267 "_Toc253429267")  
[Skrytí div elementy kolem skryté pole](#0.2__Toc253429268 "_Toc253429268")  
[Vykreslování vnější tabulky pro ovládací prvky podle šablony](#0.2__Toc253429269 "_Toc253429269")  
[ListView – ovládací prvek vylepšení](#0.2__Toc253429270 "_Toc253429270")  
[Třídy CheckBoxList a vylepšení ovládací prvek RadioButtonList](#0.2__Toc253429271 "_Toc253429271")  
[Vylepšení řízení nabídky](#0.2__Toc253429272 "_Toc253429272")  
[Průvodce a ovládací prvky CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Oblasti podpory](#0.2__Toc253429275 "_Toc253429275")  
[Podpora ověřování atributů datové poznámky](#0.2__Toc253429276 "_Toc253429276")  
[Objekty se šablonami za](#0.2__Toc253429277 "_Toc253429277")

**[Dynamická Data](#0.2__Toc253429278 "_Toc253429278")**  
[Povolení dynamické Data pro existující projekty](#0.2__Toc253429279 "_Toc253429279")  
[Syntaxe deklarativní ovládacího prvku DynamicDataManager](#0.2__Toc253429280 "_Toc253429280")  
[Šablony entit](#0.2__Toc253429281 "_Toc253429281")  
[Nové šablony polí pro adresy URL a e-mailové adresy](#0.2__Toc253429282 "_Toc253429282")  
[Vytváření odkazů pomocí ovládacího prvku DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Podpora pro dědičnosti v datovém modelu](#0.2__Toc253429284 "_Toc253429284")  
[Podpora pro relace m: N (pouze rozhraní Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Nové atributy pro řízení zobrazení a výčty podporu](#0.2__Toc253429286 "_Toc253429286")  
[Rozšířenou podporu pro filtry](#0.2__Toc253429287 "_Toc253429287")

**[Visual Studio 2010 Web Development vylepšení](#0.2__Toc253429288 "_Toc253429288")**  
[Vylepšené Kompatibilita šablon stylů CSS](#0.2__Toc253429289 "_Toc253429289")  
[Fragmenty HTML a JavaScript](#0.2__Toc253429290 "_Toc253429290")  
[Rozšíření JavaScript IntelliSense](#0.2__Toc253429291 "_Toc253429291")

**[Nasazení aplikací pomocí sady Visual Studio 2010 webové](#0.2__Toc253429292 "_Toc253429292")**  
[Webové balení](#0.2__Toc253429293 "_Toc253429293")  
[Transformace Web.config](#0.2__Toc253429294 "_Toc253429294")  
[Nasazení databáze](#0.2__Toc253429295 "_Toc253429295")  
[Publikování jedním kliknutím pro webové aplikace](#0.2__Toc253429296 "_Toc253429296")  
[Prostředky](#0.2__Toc253429297 "_Toc253429297")

**[Právní omezení](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Základní služby

ASP.NET 4 zavádí řadu funkcí, které vylepšují základní služby technologie ASP.NET, jako je například ukládání výstupu do mezipaměti a ukládání stavu relace.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Refaktoring souboru Web.config

`Web.config` Soubor, který obsahuje nastavení pro webové aplikace výrazně zvětšila posledních několika verzích rozhraní .NET Framework jako byly přidány nové funkce, jako je například Ajax, směrování a integrace se službou IIS 7. To má provedené těžší konfigurovat nebo spustit novou webovou aplikaci bez nástroje, jako je Visual Studio. V rozhraní NET Framework 4, byl přesunut do hlavní konfigurační prvky `machine.config` souborů a aplikací, které jsou nyní tato nastavení dědí. To umožňuje `Web.config` souboru v aplikacích ASP.NET 4 být prázdný nebo obsahovat jenom následující řádky, které určují pro sadu Visual Studio, jaká verze rozhraní cílení aplikace:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Ukládání Extensible výstupu do mezipaměti

Od doby, která byla vydána ASP.NET 1.0 ukládání výstupu do mezipaměti umožnilo vývojářům ukládat generovaný výstup stránky, ovládací prvky a odpovědi protokolu HTTP v paměti. Na následujících webových požadavků můžete rychleji poskytovat obsah načtením generovaný výstup z paměti místo znovu vygenerovat výstupní od začátku ASP.NET. Však tento přístup má omezenou – generovaného obsahu. vždycky musí být uložené v paměti, a na serverech, na kterých se vyskytují provozy, můžete počkat paměťové nároky ukládání výstupu do mezipaměti s požadavky na paměť z jiných částí webové aplikace.

ASP.NET 4 přidá bod rozšiřitelnosti pro ukládání výstupu do mezipaměti, který vám umožní nakonfigurovat jeden nebo více vlastních poskytovatelů výstupní mezipaměti. Výstupní mezipaměti mohou poskytovatelé mechanizmus pro ukládání zachovat obsah HTML. To umožňuje vytvářet vlastní poskytovatele výstupní mezipaměti pro různé trvalost mechanismy, mezi které patří místní nebo vzdálené disky, cloudového úložiště a distribuované mezipaměti.

Můžete vytvořit vlastního poskytovatele výstupní mezipaměti jako třídu, která pochází z nové *System.Web.Caching.OutputCacheProvider* typu. Potom můžete nakonfigurovat poskytovatele v `Web.config` soubor pomocí nové *zprostředkovatelé* pododdílu z *outputCache* elementu, jak je znázorněno v následujícím příkladu:

[!code-xml[Main](overview/samples/sample2.xml)]

Ve výchozím nastavení v rozhraní ASP.NET 4, všechny odpovědi protokolu HTTP, vykreslené stránky a ovládací prvky použít v paměti výstupní mezipaměti, jak je znázorněno v předchozím příkladu, kde *defaultProvider* je atribut nastaven na AspNetInternalProvider. Můžete změnit výchozí zprostředkovatel výstupní mezipaměti použít pro webovou aplikaci tak, že zadáte název jiného zprostředkovatele *defaultProvider*.

Kromě toho můžete vybrat různé zprostředkovatele výstupní mezipaměti na ovládací prvek a každý požadavek. Nejjednodušší způsob, jak vybrat různé zprostředkovatele výstupní mezipaměti pro různé webových uživatelských ovládacích prvků je provést to deklarativně pomocí nové *providerName* atribut v direktivě ovládacího prvku, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Určení poskytovatel různých výstupní mezipaměti pro požadavek HTTP vyžaduje trochu další práci. Místo zadání deklarativně zprostředkovatele, přepište novou *GetOuputCacheProviderName* metoda v `Global.asax` souboru pro programové určení poskytovatele, kterého chcete použít pro konkrétní žádost. Následující příklad ukazuje, jak to provést.

[!code-csharp[Main](overview/samples/sample4.cs)]

Po přidání rozšíření poskytovatel výstupní mezipaměti ASP.NET 4 můžete nyní vykonávat agresivnější a inteligentnější strategie ukládání výstupu do mezipaměti pro webové servery. Například je nyní možné pro ukládání do mezipaměti na stránkách "Top 10" webu v paměti, při ukládání do mezipaměti stránek, které získáte nižší provoz na disku. Alternativně můžete ukládat do mezipaměti každou kombinace vykreslované stránky, ale použít distribuované mezipaměti, aby spotřebu paměti přebírá z webových serverů front-end.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Automatické spuštění webové aplikace

Některých webových aplikací je nutné načíst velké objemy dat nebo provést nákladné inicializační zpracování ještě před obsluhou prvního požadavku. V předchozích verzích technologie ASP.NET pro tyto situace bylo navrhnout vlastní přístupy k "probuzení" aplikace ASP.NET a poté spusťte kódu inicializace během *aplikace\_zatížení* metoda v `Global.asax` soubor.

Nová funkce škálovatelnost s názvem *automatického spuštění* , přímo adresy tento scénář je k dispozici ASP.NET 4 spuštění na službu IIS 7.5 v systému Windows Server 2008 R2. Poskytuje funkci automatického spuštění řízený přístup pro spuštění fondu aplikací, inicializace aplikace ASP.NET a pak přijímá požadavky protokolu HTTP.

> [!NOTE] 
> 
> Modul Warm-Up aplikace IIS pro službu IIS 7.5
> 
> Tým služby IIS vydala první beta test verze modulu Application Warm-Up pro službu IIS 7.5. Díky zahájení práce s aplikací bylo ještě jednodušší, než se popisuje výš. Místo psaní vlastní kód, zadejte adresy URL prostředků k provedení před webová aplikace přijímá požadavky ze sítě. Tato warm-up dojde při spuštění služby IIS (Pokud jste nakonfigurovali fond aplikací služby IIS jako *AlwaysRunning*) a když recykluje pracovní proces služby IIS. V průběhu recyklace stará pracovní proces služby IIS dál provést požadavky, dokud se nově vytvořená pracovního procesu je plně jestli, tak, aby aplikací mít žádné přerušení nebo jiných problémů, z důvodu unprimed mezipamětí. Všimněte si, že tento modul funguje na libovolnou verzi technologie ASP.NET, od verze 2.0.
> 
> Další informace najdete v tématu [Application Warm-Up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) na webu IIS.net. Návod, která ukazuje, jak používat funkci warm-up najdete v tématu [Začínáme s modulem IIS 7.5 Application Warm-Up](https://www.iis.net/learn/manage) na webu IIS.net.


Pokud chcete používat funkci automatického spuštění, nastaví správce služby IIS fond aplikací služby IIS 7.5 na automatické spuštění pomocí následující konfigurace v `applicationHost.config` souboru:

[!code-xml[Main](overview/samples/sample5.xml)]

Protože jeden fond aplikací může obsahovat více aplikací, můžete zadat jednotlivé aplikace na automatické spuštění pomocí následující konfigurace v `applicationHost.config` souboru:

[!code-xml[Main](overview/samples/sample6.xml)]

Pokud je server služby IIS 7.5 studený started nebo po recyklaci fondu jednotlivých aplikací, IIS 7.5 používá informace v `applicationHost.config` souboru můžete zjistit, které vyžadují webové aplikace na automatické spuštění. Pro každou aplikaci, která jsou označena pro automatické spouštění IIS 7.5 odešle požadavek na technologii ASP.NET 4 a spusťte aplikaci ve stavu, během kterého aplikace dočasně nepřijímá požadavky HTTP. Pokud je v tomto stavu, ASP.NET vytvoří typ definované *serviceAutoStartProvider* atribut (jak je uvedeno v předchozím příkladu) a volání do jeho veřejné vstupní bod.

Vytvoření typu spravované automatického – spuštění nezbytné vstupnímu bodu implementací *IProcessHostPreloadClient* rozhraní, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](overview/samples/sample7.cs)]

Po vaší inicializaci spuštění kódu v *předběžného načítání* metoda a metoda vrátí, aplikace ASP.NET je připravena ke zpracování požadavků.

Po přidání automaticky spouštěná.5 služby IIS a ASP.NET 4 nyní máte dobře definovaný přístup pro provádění inicializaci nákladné aplikace před zpracováním první požadavek HTTP. Například můžete novou funkci Automatické spuštění pro inicializaci aplikace a pak signál Vyrovnávání zatížení, aby aplikace byla inicializována a připravena přijímat komunikaci protokolu HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Trvalé přesměrování stránky

Je obvyklé ve webových aplikacích přesunout stránky a další obsah kolem v čase, což může vést k nahromadění zastaralé odkazy na vyhledávacích webech. V technologii ASP.NET, vývojáři mají tradičně zpracovává požadavky na původní adresy URL pomocí pomocí *Response.Redirect* metoda pro předání požadavku na novou adresu URL. Ale *přesměrování* metoda vydá odpověď HTTP 302 nalezen (dočasné přesměrování), což vede k navíc HTTP odezvy při uživatelé pokusí přistoupit k původní adresy URL.

Přidá nový ASP.NET 4 *RedirectPermanent* Pomocná metoda, která usnadňuje problém protokolu HTTP 301 trvale přesunut odpovědi, jako v následujícím příkladu:

[!code-csharp[Main](overview/samples/sample8.cs)]

Vyhledávací weby a jiné agenty uživatele, které rozpoznají trvalé přesměrování uloží novou adresu URL, který je přidružený obsah, který eliminuje nepotřebné odezvy provedené v prohlížeči pro dočasné přesměrování.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Zmenšení stav relace

Technologie ASP.NET poskytuje dvě výchozí možnosti pro ukládání stavu relace mezi webové farmy: poskytovatele stavu relace, která volá serveru stav relace mimo proces a poskytovatele stavu relace, která ukládá data v databázi Microsoft SQL Server. Protože obě možnosti zahrnují ukládání informací o stavu mimo pracovní proces webové aplikace, musí být serializován před odesláním do vzdáleného úložiště stavu relace. V závislosti na tom, kolik informací vývojář ukládá stav relace můžou růst poměrně značnou velikost serializovaná data.

ASP.NET 4 zavádí novou možnost komprese pro oba typy poskytovatelů stav relace mimo proces. Když *compressionEnabled* konfigurace možnost vidět v následujícím příkladu je nastavena na *true*, ASP.NET zkomprimuje (a dekomprese) serializovaných relace stavu pomocí rozhraní .NET Framework  *System.IO.Compression.GZipStream* třídy.

[!code-xml[Main](overview/samples/sample9.xml)]

Jednoduché přidáním nového atributu tak, aby se `Web.config` souboru aplikací s k výměně za chodu cyklů procesoru na webových serverech můžou realizovat značné snížení velikosti serializovaná data stavu relace.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Rozšíření rozsahu povolených adres URL

ASP.NET 4 zavádí nové možnosti pro rozšíření velikost adresy URL aplikace. Předchozí verze technologie ASP.NET omezené délky cesty adresy URL až 260 znaků, na základě limitu cestu souboru systému souborů NTFS. V technologii ASP.NET 4, máte možnost zvýšení (nebo snížení) tento limit podle potřeby pro vaše aplikace používá dva nové *httpRuntime* konfigurační atributy. Následující příklad ukazuje tyto nové atributy.

[!code-xml[Main](overview/samples/sample10.xml)]

Chcete-li povolit delší nebo kratší cesty (část adresy URL, která neobsahuje protokol, název serveru a řetězce dotazu), změňte  *[maxUrlLength](https://msdn.microsoft.com/en-us/library/system.web.configuration.httpruntimesection.maxurllength.aspx)*  atribut. Povolit řetězce dotazu delší nebo kratší, upravte hodnotu  *[maxQueryStringLength](https://msdn.microsoft.com/en-us/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)*  atribut.

ASP.NET 4 můžete také nakonfigurovat znaky, které se používají znak kontrolou adresy URL. Když ASP.NET vyhledá neplatný znak v části cesty adresy URL, odmítne žádost a vyvolá chybu HTTP 400. V předchozích verzích technologie ASP.NET byla omezena na pevnou sadu znaků kontroly znaků adresy URL. V technologii ASP.NET 4, můžete přizpůsobit sadu platné znaky pomocí nové *requestPathInvalidChars* atribut *httpRuntime* konfigurační element, jak je znázorněno v následujícím příkladu:

[!code-xml[Main](overview/samples/sample11.xml)]

Ve výchozím nastavení *requestPathInvalidChars* atribut definuje osm znaků jako neplatný. (V řetězci, který je přiřazen k *requestPathInvalidChars* ve výchozím nastavení*,*menší než (&lt;), je větší než (&gt;) a ampersand (&amp;) se znaky kódování, protože `Web.config` soubor je soubor XML.) Podle potřeby můžete přizpůsobit sadu neplatné znaky.

> [!NOTE]
> Poznámka: ASP.NET 4 vždy odmítne cest URL, které obsahují znaky v rozsahu ASCII 0x00 až 0x1F, protože ty jsou neplatné znaky adresy URL, jak jsou definovány v dokumentu RFC 2396 sdružení IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). Ve verzích systému Windows Server, na kterých běží služby IIS 6 nebo vyšší, ovladač http.sys protokolu zařízení automaticky odmítne adresy URL se tyto znaky.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Ověření Extensible žádosti

Ověření požadavku ASP.NET vyhledá příchozí data požadavku HTTP pro řetězce, které běžně se používají při útoku skriptování (XSS). V případě nalezení potenciální řetězce XSS ověření žádosti označí podezřelý řetězec a vrátí chybu. Vestavěné ověření požadavku vrátí chybu, pouze pokud najde nejčastěji používá v útoky XSS řetězce. Předchozí pokusy o nastavení ověření XSS agresivnější výsledkem příliš mnoho chybných přijetí. Zákazníci však vhodné, ověření požadavku, který agresivnější, nebo naopak chtít záměrně uvolnit XSS kontroly pro konkrétní stránky nebo pro určité typy požadavků.

V technologii ASP.NET 4 funkcí ověření požadavku byla provedena extensible tak, že používáte logiku vlastní ověření žádosti. Pokud chcete rozšířit ověření žádosti, vytvořte třídu, která pochází z nové *System.Web.Util.RequestValidator* typ a nakonfigurovat aplikaci (v *httpRuntime* části `Web.config`souboru) chcete použít vlastní typ. Následující příklad ukazuje, jak nakonfigurovat vlastní ověření žádosti třídy:

[!code-xml[Main](overview/samples/sample12.xml)]

Nové *requestValidationType* atribut vyžaduje, aby standardní řetězec identifikátor typ rozhraní .NET Framework, který určuje třídu, která poskytuje vlastní žádosti o ověření. Technologie ASP.NET pro každý požadavek, vyvolá vlastní typ zpracovat každého jednotlivého příchozí data požadavku HTTP. Adresy URL příchozích, všechny hlavičky protokolu HTTP (soubory cookie a vlastní hlavičky) a obsahu entity jsou všechny dostupné kontrole třídu vlastní žádosti o ověření jako zobrazená v následujícím příkladu:

[!code-csharp[Main](overview/samples/sample13.cs)]

Pro případy, kdy nechcete kontrola část příchozích dat protokolu HTTP, ověření žádosti třída může vrátit zpět umožníte ASP.NET výchozího žádosti o ověření spustit jednoduše voláním *základní. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Objekt ukládání do mezipaměti a rozšiřitelnost ukládání objektů

Od prvního vydání, prostředí ASP.NET zahrnovalo mezipaměti výkonné objektů v paměti (*System.Web.Caching.Cache*). Implementace mezipaměti byla oblíbených tak, že byl použit v jiných webových aplikací. Je však nevhodných pro odkaz na aplikaci Windows Forms nebo WPF `System.Web.dll` stačí, abyste mohli použít objekt mezipaměti technologie ASP.NET.

Chcete-li ukládání do mezipaměti k dispozici pro všechny aplikace, rozhraní .NET Framework 4 zavádí nové sestavení, nový obor názvů, některé základní typy a konkrétní implementaci mezipaměti. Nové `System.Runtime.Caching.dll` sestavení obsahuje nové rozhraní API ukládání do mezipaměti v *System.Runtime.Caching –* oboru názvů. Obor názvů obsahuje dvě základní sady třídy:

- Abstraktní typy, které poskytují základ pro vytvoření libovolného typu implementace vlastních mezipaměti.
- Implementace mezipaměti konkrétní objekt v paměti ( *System.Runtime.Caching.MemoryCache* třídy).

Nové *MemoryCache* třída je modelována ve mezipaměti technologie ASP.NET a sdílí velkou část logiky stroje interní mezipaměti s technologií ASP.NET. I když veřejná rozhraní API ukládání do mezipaměti v *System.Runtime.Caching –* byly aktualizovány pro podporu vývoj vlastních mezipamětí, pokud jste použili ASP.NET *mezipaměti* objekt, zjistíte, koncepty seznámit v Nová rozhraní API.

Podrobné informace o nové *MemoryCache* třídy a podpůrné základní rozhraní API by vyžadovaly celý dokument. Ale v následujícím příkladu získáte představu o fungování novou mezipaměť rozhraní API. V příkladu byla zapsána pro aplikaci Windows Forms, bez jakékoli závislost na `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Rozšiřitelné HTML, adresy URL a kódování hlaviček protokolu HTTP

V technologii ASP.NET 4 můžete vytvořit vlastní rutiny kódování pro následující běžné úlohy kódování textu:

- Kódování HTML.
- Kódování URL.
- Kódování atributu HTML.
- Kódování odchozí hlavičky protokolu HTTP.

Můžete vytvořit vlastní kodér odvozování z nové *System.Web.Util.HttpEncoder* typu a potom nakonfigurovat technologii ASP.NET pomocí vlastního typu v *httpRuntime* části `Web.config` souboru, jako vidět v následujícím příkladu:

[!code-xml[Main](overview/samples/sample15.xml)]

Poté, co byl nakonfigurován vlastní kodér, ASP.NET automaticky volá vlastní implementaci kódování vždy, když veřejných metod kódování *System.Web.HttpUtility* nebo *System.Web.HttpServerUtility* se nazývají třídy. To umožňuje jedné části vývojový tým Web vytvořit vlastní kodér, který implementuje agresivní kódování znaků, zatímco zbytek webový vývojový tým používá veřejnou kódování rozhraní API technologie ASP.NET. Vlastní kodér v nakonfigurováním centrálně *httpRuntime* elementu, že je zaručeno, že jsou všechna volání kódování textu z veřejné ASP.NET kódování rozhraní API směrován přes vlastní kodér.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Monitorování výkonu pro jednotlivé aplikace v rámci jednoho pracovního procesu

Chcete-li zvýšit počet webů, které může být hostovaný na jednom serveru, spusťte mnoho hostitelé více aplikací prostředí ASP.NET v rámci jednoho pracovního procesu. Pokud se více aplikací používat jednoho sdílené pracovního procesu, je však správcům serverů umožňuje identifikovat konkrétní aplikaci, která má potíže s těžko.

ASP.NET 4 využívá nové funkce Sledování prostředků zaváděné modulu CLR. Chcete-li tuto funkci povolit, můžete přidat následující fragment kódu XML konfigurace k `aspnet.config` konfigurační soubor.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Poznámka: `aspnet.config` je soubor v adresáři, kde je nainstalované rozhraní .NET Framework. Není `Web.config` souboru.


Když *appdomainresourcemonitoring –* funkce povolena a dva nové čítače výkonu, které jsou dostupné v kategorii výkonu "Aplikace ASP.NET": *% času procesoru pro spravované* a  *Spravované paměti*. Obě tyto čítače výkonu použít nové funkce správy prostředků domény aplikace CLR ke sledování odhadovaný čas procesoru a využití spravované paměti jednotlivých aplikací ASP.NET. S ASP.NET 4, je v důsledku toho správci teď mají podrobnější pohled na spotřeby prostředků jednotlivých aplikací běžících v rámci jednoho pracovního procesu.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Cílení na více verzí

Můžete vytvořit aplikaci, která cílí na konkrétní verzi rozhraní .NET Framework. V technologii ASP.NET 4, nový atribut v *kompilace* element `Web.config` soubor umožňuje cílí na rozhraní .NET Framework 4 a novější. Pokud explicitně cílové rozhraní .NET Framework 4, a pokud zahrnete volitelné elementů v `Web.config` souboru, například položky pro *system.codedom*, tyto elementy musí být správné pro rozhraní .NET Framework 4. (Pokud rozhraní .NET Framework 4 není explicitně cíle, je z chybí položka v odvodit cílovém Frameworku, který `Web.config` souboru.)

Následující příklad ukazuje použití *targetFramework* atribut *kompilace* element `Web.config` souboru.

[!code-xml[Main](overview/samples/sample17.xml)]

Vezměte na vědomí následující skutečnosti související cílení na konkrétní verzi rozhraní .NET Framework:

- Ve fondu aplikací rozhraní .NET Framework 4, systém sestavení ASP.NET předpokládá rozhraní .NET Framework 4 jako cíl Pokud `Web.config` soubor neobsahuje *targetFramework* atribut nebo, pokud `Web.config` soubor nebyl nalezen. (Možná bude muset změnit kódování do vaší aplikace, aby ho spustit v rozhraní .NET Framework 4.)
- Pokud zahrnete *targetFramework* atribut a pokud *System.CodeDom –* element je definována v `Web.config` souboru, tento soubor musí obsahovat správné položky pro rozhraní .NET Framework 4.
- Pokud používáte *aspnet\_kompilátoru* příkaz předkompilovat vaší aplikace (například v prostředí pro sestavování), je nutné použít správnou verzi *aspnet\_kompilátoru* příkaz pro cílové rozhraní. Kompilátoru, která odeslaná pomocí rozhraní .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727) ke kompilaci pro rozhraní .NET Framework 3.5 a starší verze. Použijte kompilátoru, který se dodává s rozhraní .NET Framework 4 pro kompilaci aplikace vytvořené pomocí této framework nebo pomocí novější verze.
- Za běhu, kompilátor použije nejnovější framework sestavení, které jsou nainstalovány v počítači (a proto v mezipaměti GAC). Pokud aktualizace přišla později rozhraní (například hypotetický verze 4.1 je nainstalována), nebudete moci používat funkce v novější verzi rozhraní, i když *targetFramework* atribut cílí na nižší verzi (například 4.0). (Však v době návrhu v sadě Visual Studio 2010 nebo při použití *aspnet\_kompilátoru* příkaz, používá novější funkce rozhraní způsobí chyby kompilátoru).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>AJAX

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery zahrnuté s webovými formuláři a MVC

Šablony sady Visual Studio pro webové formuláře a MVC zahrnují knihovny jQuery open source. Při vytváření nového webu nebo projektu, se vytvoří skripty složku obsahující následující soubory 3:

- jQuery-1.4.1.js – čitelné, nezmenšená verze knihovny jQuery.
- jQuery-14.1.min.js – minifikovaný verzi knihovny jQuery.
- jQuery-1.4.1-vsdoc.js – soubor Intellisense dokumentace pro knihovny jQuery.

Zahrňte nezmenšenou verzi jQuery při vývoji aplikace. Zahrňte minifikovaný verzi jQuery aplikacích v produkčním prostředí.

Například následující stránky webových formulářů ukazuje, jak je možné používat jQuery změnit barvu pozadí ASP.NET TextBox – ovládací prvky na žlutou, pokud mají fokus.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Podpora sítě pro doručování obsahu

Microsoft Ajax Content Delivery Network (CDN) umožňuje snadno přidat na vaše webové aplikace ASP.NET Ajax a jQuery skripty. Například můžete začít používat knihovny jQuery jednoduše přidáním `<script>` značky na stránku, která odkazuje na adresu Ajax.microsoft.com takto:

[!code-html[Main](overview/samples/sample19.html)]

Využitím Microsoft Ajax CDN, může výrazně zlepšit výkon aplikací Ajax. Obsah Microsoft Ajax CDN ukládají do mezipaměti na servery umístěné po celém světě. Kromě toho Microsoft Ajax CDN umožňuje prohlížečům soubory uložené v mezipaměti JavaScript pro webové servery, které se nacházejí v různých doménách.

Microsoft Ajax Content Delivery Network podporuje protokol SSL (HTTPS), v případě, že budete muset poskytnout webovou stránku pomocí protokolu Secure Sockets Layer.

Další informace o Microsoft Ajax CDN, naleznete na následujícím webu:

[https://www.ASP.NET/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

ASP.NET ScriptManager podporuje Microsoft Ajax CDN. Jednoduše tak, že nastavení jednu vlastnost, vlastnost EnableCdn můžete načíst všechny soubory JavaScript framework ASP.NET od CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Jakmile nastavíte vlastnost EnableCdn na hodnotu true, rozhraní ASP.NET načte všechny soubory JavaScript framework ASP.NET z CDN, včetně všech JavaScript soubory používané pro ověřování a UpdatePanel. Nastavení této jednu vlastnost může mít výrazný dopad na výkon vaší webové aplikace.

Můžete nastavit cestu k CDN pro své vlastní soubory JavaScript pomocí atributu webového prostředku. Nové vlastnosti CdnPath Určuje cestu k CDN používá, když nastavíte vlastnost EnableCdn na hodnotu true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Explicitní skripty ScriptManager

V minulosti Pokud jste použili ASP.NET ScriptManger pak jste museli načíst celou knihovnu monolitický ASP.NET Ajax. Využitím nové vlastnosti ScriptManager.AjaxFrameworkMode, můžete řídit přesně součástí knihovny ASP.NET Ajax, které jsou načteny a načíst pouze komponenty knihovny ASP.NET Ajax, které potřebujete.

Vlastnost ScriptManager.AjaxFrameworkMode můžete nastavit následující hodnoty:

- Povolené – Určuje, že ovládací prvek ScriptManager automaticky přidá soubor skriptu MicrosoftAjax.js, což je soubor skriptu kombinované každého skriptu framework core (starší verze chování).
- Zakázané – Určuje, že všechny funkce skriptu Microsoft Ajax jsou vypnuté a že ovládací prvek ScriptManager neodkazuje všech skriptů, automaticky.
- Explicitní – Určuje, že explicitně zahrnete skriptu odkazy na soubor skriptu základní jednotlivých framework, který vyžaduje stránku a že bude obsahovat odkazy na závislosti, které vyžaduje každý soubor skriptu.

Například pokud nastavíte vlastnost AjaxFrameworkMode na hodnotu explicitní pak můžete určit konkrétní skripty součást prvku ASP.NET Ajax, které budete potřebovat:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>webové formuláře

Webové formuláře byl základní funkce technologie ASP.NET od verze ASP.NET 1.0. Mnoho vylepšení bylo provedeno v této oblasti pro technologii ASP.NET 4, včetně následujících:

- Možnost nastavit *meta* značky.
- Větší kontrolu nad stav zobrazení.
- Jednodušší způsoby, jak pracovat s možnosti prohlížeče.
- Podpora pro použití s webovými formuláři směrování ASP.NET.
- Větší kontrolu nad generovaných ID.
- Umožňuje zachovat vybrané řádky v ovládacích prvcích data.
- Větší kontrolu nad vykreslené HTML v *FormView* a *ListView* ovládací prvky.
- Podpora pro ovládací prvky zdroje dat filtrování.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Nastavení značek Meta pomocí Page.MetaKeywords a Page.MetaDescription vlastnosti

ASP.NET 4 přidá dvě vlastnosti do *stránky* třídy, *MetaKeywords* a *MetaDescription*. Tyto dvě vlastnosti představují odpovídající *meta* značky v stránku, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Tyto dvě vlastnosti fungovat stejně způsobem stránky *název* nemá vlastnost. Následují tato pravidla:

1. Pokud nejsou žádné *meta* značky v *head* element, který se shodovat s názvy vlastností (to znamená, název = "klíčová slova" pro *Page.MetaKeywords* a název = "Popis" pro  *Page.MetaDescription*, což znamená, že tyto vlastnosti nebyly nastaveny), *meta* značek přidá na stránku je vykreslen.
2. Pokud již existují *meta* značky s těmito názvy, tyto vlastnosti sloužit jako získání a nastavení metody pro obsah stávající značky.

Tyto vlastnosti lze nastavit v době běhu, která vám umožňuje získat obsah z databáze nebo jiného zdroje a která umožňuje nastavit značky do dynamicky popište, co je konkrétní stránka pro.

Můžete také nastavit *klíčová slova* a *popis* vlastnosti v *@ stránky* direktivy v horní části kódu stránky webových formulářů, jako v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Tím se přepíše *meta* značky obsah (pokud existuje), který již byl deklarován na stránce.

Obsah popis *meta* značky se používají pro zlepšení vyhledávání výpis verze Preview v Google. (Podrobnosti najdete v tématu [zlepšení fragmenty s makeover popis meta](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) na blogu k Google správce webového serveru centrální.) Google a Windows Live Search nepoužívejte obsah klíčová slova pro všechno, co, ale může jiných vyhledávacích webů. Další informace najdete v tématu [Meta klíčová slova Rady](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) na webu služby vyhledávání modul průvodce.

Tyto nové vlastnosti jsou jednoduché funkce, ale nebudou uložit, můžete z požadavek na přidat ručně nebo vytvoření vlastního kódu k vytvoření *meta* značky.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Povolení zobrazení stavu jednotlivých ovládacích prvků

Ve výchozím nastavení je povolen stav zobrazení pro stránku, s tím výsledkem, že každý ovládací prvek na stránce potenciálně ukládá stav zobrazení, i v případě, že není nutné pro aplikaci. Data o stavu zobrazení je součástí kód, že na stránce vygeneruje a zvyšuje dobu potřebnou k odeslání stránky klientovi a odeslána zpět. Ukládání více zobrazení stavu, než je nezbytné, může způsobit významného snížení výkonu. V předchozích verzích technologie ASP.NET vývojáři může zakázat zobrazení stavu jednotlivých ovládacích prvků za účelem snížení velikosti stránky, ale měl Uděláte to tak explicitně jednotlivých ovládacích prvků. V technologii ASP.NET 4, zahrnují ovládací prvky webového serveru *ViewStateMode* vlastnost, která umožňuje zakázat stav zobrazení ve výchozím nastavení a pak ho povolit jenom pro ovládací prvky, které vyžadují na stránce.

*ViewStateMode* vlastnost trvá výčet, který má tři hodnoty: *povoleno*, *zakázané*, a *zděděné*. *Povolit* umožňuje zobrazit stav pro ovládací prvek a pro všechny podřízené ovládací prvky, které jsou nastavené na *zděděné* nebo že nic nastavili. *Zakázané* zakáže zobrazení stavu, a *zděděné* Určuje, že pomocí ovládacího prvku *ViewStateMode* nastavení z nadřazeného ovládacího prvku.

Následující příklad ukazuje jak *ViewStateMode* vlastnost funguje. Značek a kódu pro ovládací prvky na následující stránce obsahuje hodnoty pro *ViewStateMode* vlastnost:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Jak vidíte, kód zakáže stav zobrazení ovládacího prvku PlaceHolder1. Podřízený ovládací prvek label1 dědí tuto hodnotu vlastnosti (*zděděné* je výchozí hodnota pro *ViewStateMode* pro ovládací prvky.) a proto uloží bez zobrazení stavu. V ovládacím prvku PlaceHolder2 *ViewStateMode* je nastaven na *povoleno*, tak, aby tuto vlastnost dědí label2 a uloží zobrazení stavu. Při prvním načtení stránky, *Text* vlastnost objektu i *popisek* ovládací prvky je nastaven na řetězec "[DynamicValue]".

Účinek těchto nastavení je, že při prvním načtení stránky, následující výstup se zobrazí v prohlížeči:

Zakázané`: [DynamicValue]`

Povoleno:`[DynamicValue]`

Po zpětné volání ale tento výstup se zobrazí:

Zakázané`: [DeclaredValue]`

Povoleno:`[DynamicValue]`

Ovládací prvek label1 (jehož *ViewStateMode* nastavena na hodnotu *zakázané*) nebyla zachována hodnotu, která byla nastavena na v kódu. Však label2 řízení (jehož *ViewStateMode* nastavena na hodnotu *povoleno*) je zachována jeho stav.

Můžete také nastavit *ViewStateMode* v *@ stránky* direktivy, jako v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample26.aspx)]

*Stránky* třída je jenom další ovládací prvek; funguje jako nadřazeného ovládacího prvku pro všechny ostatní ovládací prvky na stránce. Výchozí hodnota *ViewStateMode* je *povoleno* pro instance *stránky*. Protože ovládací prvky jako výchozí *zděděné*, zdědí ovládací prvky *povoleno* není-li nastavena hodnota vlastnosti *ViewStateMode* na úrovni stránka nebo ovládací prvek.

Hodnota *ViewStateMode* vlastnost určuje, pokud je stav zobrazení zachován pouze v případě *EnableViewState* je nastavena na *true*. Pokud *EnableViewState* je nastavena na *false*, nebude Udržovat stav zobrazení i v případě *ViewStateMode* je nastaven na *povoleno*.

Je vhodné využít pro tuto funkci s *ContentPlaceHolder* ovládacích prvků v hlavní stránky, kde můžete nastavit *ViewStateMode* k *zakázané* pro hlavní stránky a pak ji povolit jednotlivě pro *ContentPlaceHolder* ovládacích prvků, které obsahují ovládací prvky, které vyžadují zobrazení stavu.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Změny možnosti prohlížeče

ASP.NET určuje možnosti prohlížeče, který uživatel používá k procházení webu pomocí funkci *možnosti prohlížeče*. Možnosti prohlížeče jsou reprezentované pomocí *HttpBrowserCapabilities* objektu (vystavené *Request.Browser* vlastnost). Například můžete použít *HttpBrowserCapabilities* objektem pro určení, zda typu a verzi aktuální prohlížeč podporuje konkrétní verzi jazyka JavaScript. Nebo můžete použít *HttpBrowserCapabilities* objektem pro určení, jestli tato žádost pochází z mobilního zařízení.

*HttpBrowserCapabilities* objekt doprovází sadu souborů definic prohlížeče. Tyto soubory obsahují informace o možnostech konkrétní prohlížečů. V technologii ASP.NET 4 byly aktualizovány tyto soubory definice prohlížeč bude obsahovat informace o nedávno uvedených prohlížečích a zařízeních, jako jsou například Google Chrome, Research v smartphony BlackBerry pohybu a Apple iPhone.

Následující seznam obsahuje nový prohlížeč definiční soubory:

- *BlackBerry.Browser*
- *Chrome.Browser*
- *Default.Browser*
- *Firefox.Browser*
- *Gateway.Browser*
- *Generic.Browser*
- *IE.Browser*
- *iemobile.Browser*
- *iPhone.Browser*
- *Opera.Browser*
- *Safari.Browser*

#### <a name="using-browser-capabilities-providers"></a>Pomocí poskytovatelé možností prohlížeče

V technologii ASP.NET 3.5 Service Pack 1, můžete definovat možností, které má prohlížeč následujícími způsoby:

- Na úrovni počítače, můžete vytvořit nebo aktualizovat `.browser` souboru XML v následující složce:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Po definování schopností prohlížeče, spustíte následující příkaz z příkazový řádek sady Visual Studio, aby bylo možné znovu sestavit sestavení možností prohlížeče a přidejte ji do mezipaměti GAC:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Pro jednotlivé aplikace, můžete vytvořit `.browser` souboru do aplikace `App_Browsers` složky.

Tyto přístupy vyžadovat změnu souborů XML a změny v úrovni počítače, je nutné restartovat aplikaci po spuštění aspnet\_regbrowsers.exe procesu.

ASP.NET 4 obsahuje funkci označuje jako *poskytovatelé možností prohlížeče*. Jak již název naznačuje, díky tomu se vytvářet poskytovatele, který můžete použít vlastní kód k určení možnosti prohlížeče.

V praxi vývojáři často nedefinují možnosti vlastní prohlížeče. Prohlížeč souborů jsou těžko aktualizovat, proces pro aktualizaci je je poměrně složitá a syntaxe XML `.browser` souborů může být složité, používání a definovat. Co by jednodušší tento proces je kdyby běžné syntaxe definice prohlížeče, nebo databáze, která obsahovala definice aktuální prohlížeč nebo i webové služby pro takovou databázi. Nové funkce zprostředkovatelé možnosti prohlížeče usnadňuje tyto scénáře možné a praktické jiných společností.

Existují dva hlavní přístupy pro pomocí nové funkce poskytovatele funkce prohlížeč technologii ASP.NET 4: rozšířit možnosti prohlížeče ASP.NET definice funkce nebo zcela jeho nahrazení. Následující části popisují nejprve jak nahradit funkce a jak ho rozšířit.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Nahrazení funkce Možnosti prohlížeče ASP.NET

Nahradit funkci ASP.NET možnosti prohlížeče definice úplně, postupujte takto:

1. Vytvořte zprostředkovatele třídu odvozenou od *HttpCapabilitiesProvider* a který přepíše *GetBrowserCapabilities* metoda jako v následujícím příkladu: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Vytvoří nový kód v tomto příkladu *HttpBrowserCapabilities* objektu, zadání pouze možnost s názvem prohlížeče a nastavení této funkce na MyCustomBrowser.
2. Zaregistrujte zprostředkovatele s aplikací. 

    Chcete-li použít zprostředkovatele s aplikací, je nutné přidat *zprostředkovatele* atribut *browserCaps* tématu `Web.config` nebo `Machine.config` soubory. (Můžete také definovat atributů zprostředkovatele *umístění* element pro konkrétní adresáře v aplikaci, například do složky pro konkrétní mobilní zařízení.) Následující příklad ukazuje, jak nastavit *zprostředkovatele* atribut v konfiguračním souboru:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Jiný způsob, jak zaregistrovat novou definici schopností prohlížeče je použít kód, jak je znázorněno v následujícím příkladu:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Tento kód musí být spuštěný v *aplikace\_spustit* události `Global.asax` souboru. Jakékoli změny *BrowserCapabilitiesProvider* třída musí dojít před provedením jakékoli kód aplikace, pokud chcete mít jistotu, že mezipaměti zůstává v platném stavu pro vyřešen *HttpCapabilitiesBase* objektu.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Ukládání do mezipaměti HttpBrowserCapabilities objektu

V předchozím příkladu má jeden problém, který je, že kód by pokaždé, když vlastního zprostředkovatele je volána při získání *HttpBrowserCapabilities* objektu. Více než jednou. k tomu dochází při každé žádosti. V příkladu kód pro zprostředkovatele neprovádí mnohem. Ale pokud kód v vlastního poskytovatele provede významné práci v pořadí získat *HttpBrowserCapabilities* objektu, může to ovlivnit výkon. K tomu nedocházelo, mezipaměti *HttpBrowserCapabilities* objektu. Postupujte podle těchto kroků:

1. Vytvořte třídu, která je odvozena z *HttpCapabilitiesProvider*, jako v následujícím příkladu: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    V příkladu kód vygeneruje klíč mezipaměti při volání vlastní metody BuildCacheKey a získá dobu do mezipaměti při volání vlastní metody GetCacheTime. Kód pak přidá vyřešený *HttpBrowserCapabilities* objektů do mezipaměti. Objekt můžete načíst z mezipaměti a opakovaně na následné žádosti, které usnadňují použití vlastního zprostředkovatele.
2. Zaregistrujte zprostředkovatele s aplikací, jak je popsáno v předchozím postupu.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Rozšíření funkcí možnosti prohlížeče ASP.NET

Jak vytvořit nový popsané v předchozí části *HttpBrowserCapabilities* objekt v rozhraní ASP.NET 4. Funkce ASP.NET prohlížeče možnosti můžete také rozšířit přidáním nových definic možnosti prohlížeče na ty, které jsou již v technologii ASP.NET. Můžete provést bez použití prohlížeče definice XML. Následující postup ukazuje způsob.

1. Vytvořte třídu, která je odvozena z *HttpCapabilitiesEvaluator* a který přepíše *GetBrowserCapabilities* metoda, jak je znázorněno v následujícím příkladu: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Tento kód používá funkci ASP.NET prohlížeče možnosti nejprve pokusit se identifikovat v prohlížeči. Ale pokud se podaří identifikovat žádné prohlížeče na základě informací definované v požadavku (tj. Pokud *prohlížeče* vlastnost *HttpBrowserCapabilities* objektu je řetězec "Neznámý"), kód zavolá metodu vlastní zprostředkovatel (MyBrowserCapabilitiesEvaluator) k identifikaci prohlížeče.
2. Zaregistrujte zprostředkovatele s aplikací, jak je popsáno v předchozím příkladu.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Rozšíření prohlížeče možnosti funkce přidáním nových funkcí do existující definice funkcí

Kromě vytvoření vlastní prohlížeče definice zprostředkovatele a dynamicky vytváření nových definic prohlížeče můžete rozšířit stávající definice prohlížeče s další možnosti. To vám umožní používat definice, které je blízko co chcete použít, ale chybí pouze několik možností. Chcete-li to provést, použijte následující kroky.

1. Vytvořte třídu, která je odvozena z *HttpCapabilitiesEvaluator* a který přepíše *GetBrowserCapabilities* metoda, jak je znázorněno v následujícím příkladu: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Ukázkový kód rozšiřuje stávající ASP.NET *HttpCapabilitiesEvaluator* třídy a získá *HttpBrowserCapabilities* objekt, který odpovídá definici aktuální žádost pomocí následujícího kódu :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Kód pak můžete přidat nebo změnit funkce pro tento prohlížeč. Zadejte novou funkci prohlížeče dvěma způsoby:

    - Přidat dvojici klíč/hodnota do *IDictionary* objekt, který je zveřejněný prostřednictvím *možnosti* vlastnost *HttpCapabilitiesBase* objektu. V předchozím příkladu kód přidá funkce s názvem vícedotykové s hodnotou *true*.
    - Nastavit vlastnosti existující *HttpCapabilitiesBase* objektu. V předchozím příkladu, nastaví kód *rámce* vlastnost *true*. Tato vlastnost je jednoduše vytvoření přistupujícího *IDictionary* objekt, který je zveřejněný prostřednictvím *možnosti* vlastnost. 

        > [!NOTE]
        > Poznámka: Tento model se vztahuje na žádnou vlastnost *HttpBrowserCapabilities*, včetně adaptéry ovládacích prvků.
2. Zaregistrujte zprostředkovatele s aplikací, jak je popsáno v předchozím kroku.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Směrování v technologii ASP.NET 4

ASP.NET 4 přidá integrovanou podporu pro použití směrování s webovými formuláři. Směrování umožňuje nakonfigurovat aplikaci tak, aby přijímal žádosti adresy URL, které nelze namapovat na fyzické soubory. Místo toho můžete směrování k definování adresy URL, jsou srozumitelné pro uživatele a které mohou pomoci s optimalizací vyhledávání (vyhledávací weby SEO) pro vaši aplikaci. Adresu URL stránky, které se zobrazí produktové kategorie v existující aplikaci může například vypadat jako v následujícím příkladu:

[!code-console[Main](overview/samples/sample36.cmd)]

Pomocí směrování, můžete nakonfigurovat aplikaci tak, aby přijímal následující adresu URL k vykreslení stejné informace:

[!code-console[Main](overview/samples/sample37.cmd)]

Směrování byl k dispozici počínaje ASP.NET 3.5 SP1. (Příklad, jak pomocí směrování v technologii ASP.NET 3.5 SP1, naleznete v příspěvku [pomocí směrování s WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "název této položky.") na blogu Phila Haacka.) ASP.NET 4, ale zahrnuje některé funkce, které usnadňují použití směrování, včetně následujících:

- *PageRouteHandler* třída, která je jednoduchý obslužné rutiny HTTP, který používáte při definování tras. Třída předá data na stránku žádosti se směruje na.
- Nové vlastnosti *HttpRequest.RequestContext* a *Page.RouteData* (což je proxy server pro *HttpRequest.RequestContext.RouteData* objekt). Tyto vlastnosti usnadňují přístup k informacím, který je předán na základě trasy.
- Následující nové tvůrci výrazů, které jsou definovány v *System.Web.Compilation.RouteUrlExpressionBuilder* a *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, který poskytuje jednoduchý způsob, jak vytvořit adresu URL, která odpovídá adrese URL trasy v rámci ovládacího prvku ASP.NET.
- *RouteValue*, který poskytuje jednoduchý způsob, jak extrahovat informace z *RouteContext* objektu.
- *RouteParameter* třídy, která usnadňuje předat data obsažená v *RouteContext* objekt, který má dotaz pro ovládací prvek zdroje dat (podobně jako [ *FormParameter* ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Směrování pro stránky webových formulářů

Následující příklad ukazuje, jak definovat trasu webových formulářů pomocí nové *MapPageRoute* metodu *trasy* třídy:

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 zavádí *MapPageRoute* metoda. Následující příklad je ekvivalentní k definici SearchRoute ukazuje předchozí příklad, ale používá *PageRouteHandler* třídy.

[!code-csharp[Main](overview/samples/sample39.cs)]

Kód v ukázce mapuje trasu na fyzickou stránku (v první trasy k `~/search.aspx`). První definice trasy se také určuje, že by měl být parametr s názvem searchterm extrahovat z adresy URL a předána na stránku.

*MapPageRoute* metoda podporuje následující přetížení metody:

- *MapPageRoute (řetězec routeName, routeUrl řetězec, řetězec physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute (řetězec routeName, routeUrl řetězec, řetězec physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary výchozí nastavení)*
- *MapPageRoute (řetězec routeName, routeUrl řetězec, řetězec physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary výchozí hodnoty, omezení RouteValueDictionary)*

*CheckPhysicalUrlAccess* parametr určuje, zda trasy měli zkontrolovat oprávnění zabezpečení pro fyzickou stránku směrování (v tomto případě search.aspx) a oprávnění pro příchozí adresy URL (v tomto případě vyhledávání / {searchterm}). Pokud hodnota *checkPhysicalUrlAccess* je *false*, budou kontrolovat jenom oprávnění příchozí adresy URL. Tato oprávnění jsou definovány v `Web.config` souboru pomocí nastavení, jako například následující:

[!code-xml[Main](overview/samples/sample40.xml)]

V příkladu konfiguraci, byl odepřen přístup na fyzickou stránku `search.aspx` pro všechny uživatele kromě těch, kteří jsou v roli správce. Když *checkPhysicalUrlAccess* parametr je nastaven na *true* (což je výchozí hodnota), pouze správce můžou uživatelé pro přístup k adrese URL /search/ {searchterm}, protože je search.aspx fyzickou stránku omezen na uživatele v dané roli. Pokud *checkPhysicalUrlAccess* je nastaven na *false* a je lokalita nastavena jako v předchozím příkladu, jsou povolené všechny ověřené uživatele pro přístup k adrese URL /search/ {searchterm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Čtení informace o přesměrování stránky webových formulářů

V kódu fyzické stránky webových formulářů, dostanete informace, které směrování se extrahují z adresy URL (nebo Další informace, které do přidal jiného objektu *RouteData* objektu) pomocí dvou nových vlastností:  *HttpRequest.RequestContext* a *Page.RouteData*. (*Page.RouteData* zabalí *HttpRequest.RequestContext.RouteData*.) Následující příklad ukazuje, jak používat *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Vyextrahuje hodnotu, který byl předán pro parametr searchterm, jak jsou definovány v příkladu trasy, která dříve. Vezměte v úvahu následující adrese URL požadavku:

[!code-console[Main](overview/samples/sample42.cmd)]

Při této žádosti se slovo "scott" vykreslená v `search.aspx` stránky.

#### <a name="accessing-routing-information-in-markup"></a>Přístup k informacím o směrování v kódu

Metoda popsaná v předchozí části ukazuje, jak získat data trasy v kódu v stránky s webovými formuláři. Můžete také použít výrazy ve značkách, které umožňují přístup k stejné informace. Tvůrci výrazů představují výkonný a elegantní způsob, jak pracovat s deklarativní kódem. (Další informace naleznete v příspěvku [Express sami s vlastní tvůrci výrazů](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) na blogu Phila Haacka.)

Technologie ASP.NET 4 obsahuje dvě nové tvůrci výrazů pro směrování webových formulářů. Následující příklad ukazuje, jak je používat.

[!code-aspx[Main](overview/samples/sample43.aspx)]

V příkladu *RouteUrl* výraz se používá k definování adresy URL, která je založená na parametr trasy. To ušetří práci s zakódovat úplnou adresu URL do kódu a umožňuje později změnit strukturu adresy URL bez nutnosti všechny změny na tento odkaz.

Na základě trasy definované dříve, tento kód generuje následující adresu URL:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET automaticky funguje správné směrování (to znamená, vygeneruje správnou adresu URL) na základě vstupních parametrů. Název trasy můžete použít také ve výrazu, který umožňuje určit trasu k použití.

Následující příklad ukazuje, jak používat *RouteValue* výraz.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Při spuštění stránky, která obsahuje tento ovládací prvek, je hodnota "scott" zobrazena v popisku.

*RouteValue* výraz usnadňuje použít data trasy v značek a není pro práci s složitější Page.RouteData["x"] syntaxe značek.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Použití dat trasy pro parametry řízení zdroje dat

*RouteParameter* třída umožňuje určit data trasy jako hodnotu parametru pro dotazy v prvku zdroje dat. Ho [funguje podobně jako](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.formparameter.aspx) třídy, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample46.aspx)]

V takovém případě se hodnota searchterm parametru trasy se použije pro @companyname parametr ve *vyberte* příkaz.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Nastavení ID klienta

Nové *ClientIDMode* vlastnost adresy dlouhotrvající problém v technologii ASP.NET, a to, jak vytvořit ovládací prvky *id* atribut pro elementy, které vykresluje. Znalost *id* atribut u elementů vykreslovaných je důležité, pokud vaše aplikace obsahuje klientský skript, který odkazuje na těchto elementů.

*Id* atribut ve formátu HTML, který je vykreslen pro ovládací prvky webového serveru se generuje na základě *ClientID* vlastností ovládacího prvku. Dokud ASP.NET 4, algoritmus pro generování *id* atribut z *ClientID* vlastnost byla ke zřetězení názvový kontejner (pokud existuje) s ID a v případě opakovaných ovládacích prvků (jako v ovládací prvky datových), chcete-li přidat předponu a pořadové číslo. Když to má vždy zaručit, že jsou jedinečné ID ovládacích prvků na stránce, algoritmus má za následek řízení ID, které nebyly předvídatelný a byly proto obtížné odkaz v klientského skriptu.

Nové *ClientIDMode* vlastnost umožňuje určit, přesněji způsob generování ID klienta pro ovládací prvky. Můžete nastavit *ClientIDMode* vlastnost pro libovolný ovládací prvek, včetně pro stránku. Možná nastavení jsou následující:

- *AutoID* – jde o ekvivalent algoritmus pro generování *ClientID* hodnoty vlastností, které se používají v předchozích verzích technologie ASP.NET.
- *Statické* – to určuje, že *ClientID* hodnota bude stejné jako Identifikátor bez zřetězení ID nadřazeného objektu. názvy kontejnerů. To může být užitečné v webových uživatelských ovládacích prvků. Vzhledem k uživatelský ovládací prvek webu můžou být umístěné na různých stránkách a v ovládacích prvcích jiný kontejner, může být obtížné napsat klientský skript pro ovládací prvky, které používají *AutoID* algoritmus protože nemůžete vědět, co se tyto hodnoty ID .
- *Předvídatelný* – tato možnost je primárně pro použití v ovládacích prvcích dat, které používají šablony s opakováním. Zřetězuje vlastnosti ID ovládacího prvku pojmenování kontejnerů, ale vygenerovaný *ClientID* hodnoty neobsahují řetězce jako "ctlxxx". Toto nastavení funguje ve spojení s *ClientIDRowSuffix* vlastností ovládacího prvku. Můžete nastavit *ClientIDRowSuffix* vlastnost na název datového pole a hodnotu tohoto pole je použít jako přípona pro vygenerovaného *ClientID* hodnotu. Obvykle byste použili záznam dat jako primární klíč *ClientIDRowSuffix* hodnotu.
- *Zdědit* – toto nastavení je výchozí chování pro ovládací prvky; Určuje, že generování ID ovládacího prvku je stejný jako svůj nadřazený uzel.

Můžete nastavit *ClientIDMode* vlastnost na úrovni stránky. Definuje výchozí *ClientIDMode* hodnotu pro všechny ovládací prvky na aktuální stránce.

Výchozí hodnota *ClientIDMode* hodnota na úrovni stránky je *AutoID*a ve výchozím nastavení *ClientIDMode* hodnota na úrovni ovládací prvek je *zděděné*. Výsledkem je, pokud tuto vlastnost nastavit není kdekoli v kódu, všechny ovládací prvky bude jako výchozí *AutoID* algoritmus.

Nastavte hodnotu úrovně stránky v *@ stránky* – direktiva, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Můžete také nastavit *ClientIDMode* hodnota v konfiguračním souboru na úrovni počítače (počítače) nebo na úrovni aplikace. Definuje výchozí *ClientIDMode* nastavení pro všechny ovládací prvky v všechny stránky v aplikaci. Pokud nastavíte hodnotu na úrovni počítače, definuje výchozí *ClientIDMode* nastavení pro všechny weby na tomto počítači. Následující příklad ukazuje *ClientIDMode* nastavení v konfiguračním souboru:

[!code-xml[Main](overview/samples/sample48.xml)]

Jak jsme uvedli dříve, hodnota *ClientID* vlastnost je odvozený od pojmenování kontejneru pro nadřazeného ovládacího prvku. V některých případech, například při použití hlavní stránky ovládací prvky můžou skončit s ID stejně jako v následující vykreslení HTML:

[!code-html[Main](overview/samples/sample49.html)]

I když *vstupní* vidět ve značce elementu (z *TextBox* ovládací prvek) je jenom dva pojmenování kontejnerů hluboké na stránce (vnořeného *ContentPlaceholder* ovládací prvky), kvůli způsobu, kterým se zpracovávají stránky předlohy výsledek je ID ovládacího prvku takto:

[!code-console[Main](overview/samples/sample50.cmd)]

Toto ID je musí být jedinečný na stránce, ale je zbytečně dlouho pro většinu účelů. Představte si, že chcete zkrátit délku vykreslené ID a mít větší kontrolu nad způsob je ID generování. (Například chcete odstranit předpony "ctlxxx".) Nejjednodušší způsob, jak toho docílit, je nastavením *ClientIDMode* vlastnost, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample51.aspx)]

V této ukázce *ClientIDMode* je nastavena na *statické* pro krajních *NamingPanel* elementu a nastaven na *Predictable* pro vnitřní *NamingControl* element. Tato nastavení způsobí následující kód (zbývající stránky a stránky předlohy hodnota je stejná jako v předchozím příkladu):

[!code-html[Main](overview/samples/sample52.html)]

*Statické* nastavení má za následek resetování pojmenování hierarchie pro všechny ovládací prvky uvnitř krajních *NamingPanel* elementu a vyloučit *ContentPlaceHolder* a *MasterPage* ID z generovaného ID. ( *Název* atribut elementů vykreslovaných je poškozena, takže normální funkce ASP.NET se uchovávají pro události, zobrazení stavu a tak dále.) Vedlejším účinkem resetování hierarchii názvů je, že i když přesunete značku *NamingPanel* elementy na jiný *ContentPlaceholder* ID klient vykreslovaných ovládacích prvků zůstávají stejné.

> [!NOTE]
> Všimněte si, že je na vás, abyste měli jistotu, že jsou jedinečné identifikátory vykreslovaných ovládacích prvků. Pokud tomu tak není, ho můžete rozdělit žádné funkce, které vyžaduje jedinečný ID pro jednotlivé elementy HTML, jako je například klienta *document.getElementById* funkce.


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Vytvoření ID předvídatelný klienta v ovládací prvky vázané na Data

*ClientID* hodnoty, které jsou generovány pro ovládací prvky v ovládacím prvku seznamu vázané na data starší verze algoritmem může být dlouhé a nejsou skutečně předvídatelný. *ClientIDMode* funkce můžete mít větší kontrolu nad jak tyto identifikátory se generují.

Obsahuje kód v následujícím příkladu *ListView* ovládacího prvku:

[!code-aspx[Main](overview/samples/sample53.aspx)]

V předchozím příkladu *ClientIDMode* a *RowClientIDRowSuffix* vlastnosti jsou nastaveny v kódu. *ClientIDRowSuffix* vlastnost lze použít pouze v ovládacích prvcích vázaných na data a jeho chování se liší v závislosti na tom, které řídí, kterou používáte. Rozdíly jsou tyto:

- *Rutina GridView* ovládací prvek – můžete určit, název jednoho nebo více sloupců ve zdroji dat, které se zkombinují v době běhu k vytvoření ID klienta. Například pokud nastavíte *RowClientIDRowSuffix* k "ProductName, ProductId", řídit ID pro vykreslené prvky budou mít formát takto:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* ovládací prvek, je-li zadat jeden sloupec ve zdroji dat, který se připojí k ID klienta. Například pokud nastavíte *ClientIDRowSuffix* k "ProductName", ID vykreslovaných ovládacích prvků bude mít například následující formát:

- [!code-console[Main](overview/samples/sample55.cmd)]

- V takovém případě koncové 1 je odvozen od ID produktu z aktuální datové položky.

- *Opakovače* ovládací prvek – nepodporuje tento ovládací prvek *ClientIDRowSuffix* vlastnost. V *opakovače* slouží k řízení, index na aktuálním řádku. Při použití ClientIDMode = "Predictable" s *opakovače* řízení, klienta jsou generovány ID, který mají tento formát:

- [!code-console[Main](overview/samples/sample56.cmd)]

- Koncové 0 je index na aktuálním řádku.

*FormView* a *DetailsView* ovládací prvky, které nepodporují nezobrazovat více řádků *ClientIDRowSuffix* vlastnost.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Zachování výběru řádku v ovládacích prvcích dat.

*GridView* a *ListView* ovládací prvky můžete umožnit uživateli vybrat řádek. V předchozích verzích technologie ASP.NET výběr založena na index řádku na stránce. Například pokud vyberte třetí položku na stránce 1 a pak přejděte na stránku 2, je vybrána třetí položku na této stránce.

Trvalou výběr byl původně podporován jenom v projektech dynamických dat v rozhraní .NET Framework 3.5 SP1. Pokud je tato funkce povolena, je pro aktuálně vybranou položku podle data klíče pro položku. To znamená, že pokud vyberete třetí řádek na stránce 1 a přesunout do stránky 2, nevybere se na stránce 2. Když se vrátíte na stránku 1, vybere se stále třetí řádek. Trvalou výběr je nyní podporován pro *GridView* a *ListView* ovládacích prvků ve všech projektech pomocí *EnablePersistedSelection* vlastnost, jak je znázorněno Následující příklad:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Ovládací prvek ASP.NET grafu

Technologie ASP.NET *grafu* řízení rozšíří nabídky vizualizace dat v rozhraní .NET Framework. Pomocí *grafu* ovládací prvek, můžete vytvořit stránek ASP.NET, které mají intuitivní a vizuálně poutavé grafy pro komplexní statistické nebo finanční analýzu. Technologie ASP.NET *grafu* řízení bylo zavedeno jako doplněk verze rozhraní .NET Framework verze 3.5 SP1 a je součástí verze rozhraní .NET Framework 4.

Ovládací prvek obsahuje následující funkce:

- typy 35 odlišné grafu.
- Neomezený počet oblasti grafu, nadpisy, legendy a poznámky.
- Širokou škálu nastavení vzhledu pro všechny elementy grafu.
- 3D podpora u většiny typů grafů.
- Inteligentní data štítků, které lze automaticky přizpůsobit kolem datových bodů.
- Pruhů, oddělovacích čar měřítka osy a logaritmické měřítko.
- Více než 50 finanční a statistické vzorce pro analýzu dat a transformace.
- Jednoduchá vazba a manipulace dat grafu.
- Podpora pro běžné formáty dat, jako je například kalendářní data, času a měny.
- Podpora pro interaktivity a událostmi řízené přizpůsobení, včetně klienta klikněte na tlačítko události pomocí rozhraní Ajax.
- Stav správy.
- Binární streamování.

Následující obrázky ukazují příklady finanční grafů, které jsou vytvářeny pomocí ovládacího prvku grafu ASP.NET.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Obrázek 2: Příklady ovládacích prvků ASP.NET grafu

Pro další příklady, jak používat ovládací prvek ASP.NET grafu stažení ukázkového kódu na [ukázky prostředí pro Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) stránky na webu MSDN. Další ukázky komunity můžete najít v obsahu [fórum ovládacího prvku grafu](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Přidání ovládacího prvku grafu na stránku ASP.NET

Následující příklad ukazuje, jak přidat *grafu* ovládacího prvku na stránku ASP.NET pomocí značek. V příkladu *grafu* řízení vytvoří sloupcový graf pro statické datových bodů.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Používání 3D grafů

*Grafu* obsahuje ovládací prvek *ChartAreas* kolekce, která může obsahovat *ChartArea* objekty, které definují vlastnosti oblasti grafu. Například pokud chcete použít 3D pro oblast grafu, použijte *Area3DStyle* vlastnost jako v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample59.aspx)]

Následující obrázek znázorňuje 3D grafu s čtyři řady *panelu* typ grafu.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Obrázek 3: 3D pruhový graf

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Pomocí oddělovacích čar měřítka osy a logaritmická měřítka

Oddělovacích čar měřítka osy a logaritmická měřítka jsou dva další způsoby, jak přidat vyspělosti v grafu. Tyto funkce jsou specifické pro každý osy v oblasti grafu. Například pokud chcete použít tyto funkce na primární osu Y oblasti grafu, použijte *AxisY.IsLogarithmic* a *ScaleBreakStyle* vlastnosti v *ChartArea* objektu. Následující fragment kódu ukazuje, jak používat oddělovací čáry měřítka na primární osu Y.

[!code-aspx[Main](overview/samples/sample60.aspx)]

Následující obrázek znázorňuje s povoleno oddělovací čáry měřítka osy Y.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Obrázek 4: Oddělovacích čar měřítka osy

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtrování dat pomocí ovládacího prvku QueryExtender

K filtrování dat je velmi běžné úlohy pro vývojáře, kteří vytvářejí datové webové stránky. Tradičně provedení podle budovy *kde* klauzule v datech zdrojové ovládací prvky. Tento přístup může být složité a v některých případech *kde* syntaxe neumožňuje využít všechny funkce v původní databázi.

Pro provádění filtrování snadněji, nový *QueryExtender* ovládací prvek byl přidán v technologii ASP.NET 4. Tento ovládací prvek mohou být přidány do *EntityDataSource* nebo *LinqDataSource* ovládací prvky, chcete-li filtrovat data vrácená z těchto ovládacích prvků. Protože *QueryExtender* ovládací prvek využívá LINQ, se filtr použije na serveru databáze před odesláním dat na stránku, což vede k velmi efektivní operace.

*QueryExtender* řízení podporuje různé možnosti filtru. Následující části popisují tyto možnosti a obsahují příklady, jak je používat.

#### <a name="search"></a>Hledat

Pro možnost hledání *QueryExtender* ovládací prvek provádí hledání v zadaná pole. V následujícím příkladu používá ovládací prvek text, který je zadána v TextBoxSearch řízení a hledá jeho obsah v `ProductName` a `Supplier.CompanyName` sloupců dat, která je vrácena z *LinqDataSource* ovládací prvek.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Rozsah

Možnost rozsah je podobná možnost hledání, ale Určuje dvojici hodnot k definování rozsahu. V následujícím příkladu *QueryExtender* řízení hledání `UnitPrice` sloupec s daty vrácenými ze *LinqDataSource* ovládacího prvku. Rozsah je pro čtení z TextBoxFrom a TextBoxTo ovládacích prvků na stránce.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

Vlastnost výrazu možnost umožňuje definovat porovnání s hodnotou vlastnosti. Pokud je výsledkem výrazu *true*, je vrácena data, která je právě zkontrolován. V následujícím příkladu *QueryExtender* řízení filtruje data tak, že porovnáte data v `Discontinued` sloupce na hodnotu z ovládacího prvku CheckBoxDiscontinued na stránce.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Nakonec můžete zadat vlastní výraz pro použití s *QueryExtender* ovládacího prvku. Tato možnost umožňuje volání funkce na stránce, který definuje logiku vlastního filtru. Následující příklad ukazuje, jak deklarativně zadat vlastní výraz v *QueryExtender* ovládacího prvku.

[!code-aspx[Main](overview/samples/sample64.aspx)]

Následující příklad ukazuje vlastní funkce, které je vyvolána *QueryExtender* ovládacího prvku. V takovém případě místo použití dotaz na databázi, která zahrnuje *kde* klauzuli kód používá dotaz LINQ filtrovat data.

[!code-csharp[Main](overview/samples/sample65.cs)]

Tyto příklady ukazují jenom jeden výraz používá v *QueryExtender* řízení najednou. Však mohou obsahovat více výrazů uvnitř *QueryExtender* ovládacího prvku.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Kódovaný výrazy kódu HTML

Některé weby technologie ASP.NET (zejména s architekturou ASP.NET MVC) výraznou spoléhají na použití `<%` =  `expression %>` syntaxe (často říká "kódu útržky") pro zápis nějaký text do odpovědi. Pokud používáte kód výrazy, je snadné zapomněli ke kódování HTML, zadejte text, pokud text pochází od uživatele, ho můžete nechat stránky otevřete útoky XSS (skriptování mezi weby).

ASP.NET 4 zavádí následující nové syntaxe pro výrazy kódu:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Tuto syntaxi používá kódování HTML ve výchozím nastavení při zápisu do odpovědi. Tento nový výraz efektivně přeloží takto:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Například &lt;%: požadavek ["UserInput"] %&gt; provádí na základě hodnoty kódování HTML *požadavku ["UserInput"]*.

Cílem této funkce je umožnit nahraďte všechny výskyty staré syntaxe pomocí nové syntaxe tak, aby nebylo nutné se rozhodnout v každé fázi, kterého vystavitele si mají použít. Ale existují případy, ve kterých je výstup, měl by být ve formátu HTML nebo již s kódováním v takovém případě to může vést k dvojité kódování.

Pro případy, technologii ASP.NET 4 zavádí nové rozhraní *IHtmlString*, společně s konkrétní implementaci *HtmlString*. Instance tyto typy umožňují znamenat, že návratová hodnota je již správně kódovaný (nebo jinak zkontrolován) pro zobrazení jako kód HTML a že proto hodnota nesmí být kódovaný jazykem HTML znovu. Například následující by neměl být (a není) kódovaný jazykem HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

ASP.NET MVC 2 pomocné metody byly aktualizované pro práci s této nové syntaxe tak, aby nejsou double, kódování, ale pouze pokud používáte technologii ASP.NET 4. Toto nové syntaxe nefunguje při spuštění aplikace pomocí ASP.NET 3.5 SP1.

Uvědomte si, že to nezaručuje ochranu před útoky XSS. Kód HTML, který používá hodnoty atributu, které nejsou v uvozovkách může například obsahovat uživatelský vstup, přesto náchylné. Všimněte si, že výstup ovládacích prvků technologie ASP.NET a pomocníky ASP.NET MVC vždy obsahuje hodnoty atributu v uvozovkách, což je doporučený postup.

Tuto syntaxi, neprovádí kódování JavaScript, například při vytváření řetězec jazyka JavaScript v závislosti na vstup uživatele.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Změny v šabloně projektu

V předchozích verzích technologie ASP.NET, pokud používáte Visual Studio k vytvoření nového projektu webu nebo projektu webové aplikace, výsledné projekty obsahují pouze stránku Default.aspx, výchozí `Web.config` souboru a `App_Data` složky, jak je znázorněno v následující Obrázek:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio také podporuje typ projektu prázdný web, který neobsahuje žádné soubory vůbec, jak je znázorněno na následujícím obrázku:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Výsledkem je, že pro Začátečník, existuje jen velmi málo informací o tom, jak vytvořit produkční webové aplikace. Proto ASP.NET 4 zavádí tři nové šablony, jednu pro prázdný projekt webové aplikace a jeden pro projekt webové aplikace a webové stránky.

#### <a name="empty-web-application-template"></a>Šablona prázdné webové aplikace

Jak již název naznačuje, šablona prázdná webová aplikace je stripped-down projektu webové aplikace. Vyberte tuto šablonu projektu z dialogového okna Nový projekt sady Visual Studio, jak je znázorněno na následujícím obrázku:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Kliknutím zobrazit obrázek v plné velikosti](overview/_static/image8.png))

Když vytvoříte prázdnou webovou aplikaci ASP.NET, Visual Studio vytvoří rozložení následující složku:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Toto je podobná rozložení Prázdný web z dřívějších verzí technologie ASP.NET, s jednou výjimkou. V sadě Visual Studio 2010, projekty prázdný webové aplikace a prázdný web obsahují následující minimální `Web.config` soubor, který obsahuje informace, které slouží k identifikaci rozhraní, které cílí na projekt Visual Studio:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Bez *targetFramework* vlastnost, výchozí nastavení sady Visual Studio na cílení na rozhraní .NET Framework 2.0, aby byla zachována kompatibilita při otevírání starší aplikace.

#### <a name="web-application-and-web-site-project-templates"></a>Webové aplikace a webové stránky šablony projektů

Další dva nové šablony projektu dodané s Visual Studio 2010 obsahuje hlavní změny. Následující obrázek znázorňuje rozložení projektu, který se vytvoří při vytvoření nového projektu webové aplikace. (Rozložení pro webový projekt je téměř identický.)

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Projekt obsahuje několik souborů, které nebyly vytvořené v dřívějších verzích. Kromě toho nový projekt webové aplikace je nakonfigurovaný s základní členství funkce, které umožňuje rychle začít v zabezpečení přístupu k nové aplikace. Z důvodu této zahrnutí `Web.config` soubor pro nový projekt obsahuje položky, které se používají ke konfiguraci členství, role a profily. Následující příklad ukazuje `Web.config` soubor pro nový projekt webové aplikace. (V tomto případě *roleManager* je zakázána.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Kliknutím zobrazit obrázek v plné velikosti](overview/_static/image14.png))

Projekt obsahuje také druhý `Web.config` v soubor `Account` adresáře. Druhý soubor konfigurace poskytuje způsob, jak zabezpečit přístup k stránce ChangePassword.aspx pro jiný přihlášení uživatele. Následující příklad ukazuje obsah druhý `Web.config` souboru.

![](overview/_static/image15.png)

Stránky vytvořené ve výchozím nastavení v nové šablony projektu také obsahovat další obsah než v předchozích verzích. Projekt obsahuje soubor CSS a výchozí stránky předlohy a výchozí stránka (Default.aspx) je ve výchozím nastavení nakonfigurované na hlavní stránku. Výsledkem je, že při prvním spuštění webové aplikace nebo webové stránky, výchozí stránka (domácí) je již funkční. Ve skutečnosti je podobná výchozí stránky, které se zobrazí při spuštění nové aplikace MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Kliknutím zobrazit obrázek v plné velikosti](overview/_static/image18.png))

Mimo jiné tyto změny šablon projektu je k dispozici pokyny o tom, jak začít vytvářet nové webové aplikace. S sémanticky, striktní kompatibilní se standardem 1.0 kódu XHTML a rozložení, který je zadán pomocí šablon stylů CSS na stránkách v šablonách představují doporučené postupy pro vytváření aplikací ASP.NET 4 Web. Výchozí stránky také mít dva sloupce rozložení, který si můžete snadno přizpůsobit.

Představte si například, že pro novou webovou aplikaci chcete změnit některé barvy a vložte logo vaší společnosti, místo logo Moje aplikace technologie ASP.NET. Chcete-li to provést, vytvořte nový adresář pod `Content` k uložení obrázek loga:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Chcete-li přidat bitovou kopii na stránku, je pak otevřít `Site.Master` souboru, kde je definován text Moje aplikace technologie ASP.NET a nahraďte ho najít *bitové kopie* element jejichž *src* je atribut nastaven na nové logo Image, jako v následujícím příkladu:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Kliknutím zobrazit obrázek v plné velikosti](overview/_static/image22.png))

Potom můžete přejít do souboru Site.css a upravit definice tříd šablon stylů CSS, chcete-li změnit barvu pozadí stránky a také u záhlaví.

Výsledkem těchto změn je, že můžete zobrazit přizpůsobené Domovská stránka s velmi malé úsilí:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Kliknutím zobrazit obrázek v plné velikosti](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Vylepšení šablon stylů CSS

Mezi hlavními oblastmi práce v technologii ASP.NET 4 byl pomohou vykreslení HTML, který je kompatibilní s nejnovějších standardů HTML. To zahrnuje změny jak serverových ovládacích prvků ASP.NET pomocí stylů CSS.

#### <a name="compatibility-setting-for-rendering"></a>Nastavení kompatibility pro vykreslování

Ve výchozím nastavení, pokud webová aplikace nebo webu cílí na rozhraní .NET Framework 4 *controlRenderingCompatibilityVersion* atribut *stránky* element je nastaven na hodnotu "4.0". Tento element je definována v úrovni počítače `Web.config` souborů a ve výchozím nastavení platí pro všechny aplikace ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

Hodnota *controlRenderingCompatibility* je řetězec, který umožňuje potenciální nové verze definice v budoucích vezích se. V aktuální verzi jsou podporovány následující hodnoty pro tuto vlastnost:

- "3.5". Toto nastavení určuje starší vykreslování a značek. Ovládací prvky pro vykreslení značek je zpětně kompatibilní 100 % a nastavením *xhtmlConformance* je dodržení vlastnost.
- "4.0". Pokud toto nastavení má vlastnost, ovládací prvky ASP.NET webového serveru postupujte takto:
- *XhtmlConformance* vlastnost je vždy považován za "Strict". V důsledku toho ovládací prvky vykreslení kódu XHTML 1.0 Strict.
- Zakázat ovládacích prvků bez vstupu už vykreslí neplatný stylů.
- *div* elementy kolem skrytá pole jsou nyní navržen tak, aby nezpůsobují konflikt s uživatelem vytvořené pravidla šablon stylů CSS.
- Nabídky ovládací prvky vykreslovat kód, který je sémanticky správná a že jsou kompatibilní s pokyny pro usnadnění přístupu.
- Ovládací prvky ověření nevykreslují vložené styly.
- Ovládací prvky, které dříve vykresluje ohraničení = "0" (ovládací prvky, které jsou odvozeny od ASP.NET *tabulky* řízení a ASP.NET *Image* ovládací prvek) už nebudou zobrazovat tento atribut.

#### <a name="disabling-controls"></a>Zakázat ovládacích prvků

V technologii ASP.NET 3.5 SP1 a dřívějších verzích rozhraní vykreslí *zakázáno* atribut kód HTML pro všechny řídit, jehož *povoleno* vlastnost nastavena na hodnotu *false*. Však podle specifikace HTML 4.01, pouze *vstupní* elementy musí mít tento atribut.

V technologii ASP.NET 4, můžete nastavit *controlRenderingCompatabilityVersion* vlastnost na "3.5", jako v následujícím příkladu:

[!code-xml[Main](overview/samples/sample70.xml)]

Můžete vytvořit kód pro *popisek* řízení takto, což ho zakáže ovládacího prvku:

[!code-aspx[Main](overview/samples/sample71.aspx)]

*Popisek* ovládacího prvku by vykreslení HTML následující:

[!code-html[Main](overview/samples/sample72.html)]

V technologii ASP.NET 4, můžete nastavit *controlRenderingCompatabilityVersion* "4.0". V takovém případě má vliv pouze na tento vykreslení *vstupní* vykreslí elementy *zakázáno* atributu při ovládacího prvku *povoleno* je nastavena na *false* . Ovládací prvky, které není vykreslení HTML *vstupní* místo vykreslení elementů *třída* atribut, který odkazuje na třídu CSS, která můžete použít k definování zakázané vzhledu ovládacího prvku. Například *popisek* řízení starší příkladu by vygeneroval následující kód:

[!code-html[Main](overview/samples/sample73.html)]

Výchozí hodnota pro třídu, která zadaný pro tento ovládací prvek je "aspNetDisabled". Ale můžete změnit toto výchozí hodnota nastavením statických *DisabledCssClass* statické vlastnosti *WebControl* třídy. Pro vývojáře řízení chování pro určitý ovládací prvek lze také definovat pomocí *SupportsDisabledAttribute* vlastnost.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Skrytí div elementy kolem skryté pole

ASP.NET 2.0 a novějších verzích vykreslovat skrytá pole specifická pro systém (například *Skrytá* element používá k uložení informací o stavu zobrazení) uvnitř *div* element pro dosažení souladu se standardem XHTML. Ale mohly by způsobit problém při má vliv na pravidlo CSS *div* elementy na stránce. Například může vést k pixelů jeden řádek zobrazí na stránce přibližně skryté *div* elementy. V technologii ASP.NET 4 *div* prvky, které uzavřete skrytá pole ASP.NET generované přidání odkazu na třídu CSS jako v následujícím příkladu:

[!code-html[Main](overview/samples/sample74.html)]

Potom můžete definovat třídu CSS, která se vztahuje pouze *Skrytá* prvky, které jsou generovány technologií ASP.NET, jako v následujícím příkladu:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Vykreslování vnější tabulky pro ovládací prvky podle šablony

Ve výchozím nastavení jsou následující ovládacích prvků ASP.NET Web server, které nepodporují šablony automaticky uzavřen do vnější tabulky, který se používá k aplikování vložené styly:

- *FormView*
- *Přihlášení*
- *PasswordRecovery*
- *Změna hesla byla*
- *Průvodce*
- *CreateUserWizard*

Novou vlastnost s názvem *RenderOuterTable* byl přidán do těchto ovládacích prvků, které umožňuje vnější tabulka, která se odeberou z značku. Představte si třeba na následující příklad *FormView* ovládacího prvku:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Tento kód vykreslí na stránku, která zahrnuje tabulky HTML následující výstup:

[!code-html[Main](overview/samples/sample77.html)]

Chcete-li zabránit vykreslení v tabulce, můžete nastavit *FormView* ovládacího prvku *RenderOuterTable* vlastnosti, jako v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample78.aspx)]

Předchozí příklad vykreslí následující výstup bez *tabulky*, *tr*, a *td* prvky:

> Obsah


Toto vylepšení můžete usnadňují styl obsah ovládacího prvku s CSS, protože žádné neočekávané značky jsou vykreslovány pomocí ovládacího prvku.

> [!NOTE]
> Poznámka: Tato změna zakáže podporu pro funkci Automatické formátování v návrháři Visual Studio 2010, protože již není *tabulky* element, který může hostovat atributy stylu, které jsou generovány nástrojem možnost automatické formátování.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Rozšíření ovládacího prvku ListView

*ListView* řízení byly provedeny snazší použít v technologii ASP.NET 4. Starší verzi ovládacího prvku vyžaduje, abyste určili šablonu rozložení, která obsahovala ovládacího prvku serveru s ID známé. Následující kód ukazuje typický příklad použití *ListView* ovládací prvek v technologii ASP.NET 3.5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

V technologii ASP.NET 4 *ListView* řízení nevyžaduje šablonu rozložení. Kód zobrazí v předchozím příkladu lze nahradit následující kód:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>Třídy CheckBoxList a rozšíření RadioButtonList ovládacího prvku

V technologii ASP.NET 3.5, můžete zadat rozložení pro *CheckBoxList* a *RadioButtonList* pomocí následující dvě nastavení:

- *Tok*. Ovládací prvek vykreslí *span* elementy tak, aby obsahovala jeho obsah.
- *Tabulka*. Vykreslí ovládacího prvku *tabulky* element tak, aby obsahovala jeho obsah.

Následující příklad ukazuje kód pro každou z těchto ovládacích prvků.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Ve výchozím nastavení ovládací prvky vykreslení HTML podobný následujícímu:

[!code-html[Main](overview/samples/sample82.html)]

Protože tyto ovládací prvky obsahují seznam položek, k vykreslení sémanticky HTML vykreslovat jejich obsah pomocí seznamu HTML (*li*) elementy. To usnadňuje práci uživatele, kteří čtení webových stránek pomocí technologie usnadnění a usnadňuje ovládací prvky stylu pomocí šablon stylů CSS.

V technologii ASP.NET 4 *CheckBoxList* a *RadioButtonList* podporují následující nové hodnoty pro ovládací prvky *RepeatLayout* vlastnost:

- *OrderedList* – obsah je vykreslen jako *li* elementů v rámci *ol* elementu.
- *UnorderedList* – obsah je vykreslen jako *li* elementů v rámci *ul* elementu.

Následující příklad ukazuje, jak používat tyto nové hodnoty.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Předchozí kód generuje následující HTML:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Poznámka: Pokud nastavíte *RepeatLayout* k *OrderedList* nebo *UnorderedList*, *RepeatDirection* vlastnost může být dále používán a bude způsobí výjimku za běhu, pokud nastavena v rámci značek nebo kódu. Vlastnost by měla mít žádná hodnota, protože je definována visual rozložení tyto ovládací prvky, místo použití šablon stylů CSS.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Vylepšení nabídky ovládací prvek

Před ASP.NET 4 *nabídky* vykreslení ovládacího prvku řadě tabulek HTML. To provedené obtížnější používání stylů CSS mimo nastavení vlastností vložené a nebyl také kompatibilní se standardy usnadnění.

V technologii ASP.NET 4 ovládacího prvku nyní vykreslí HTML pomocí sémantického jazyka, který obsahuje neuspořádaný seznam a seznamu elementů. Následující příklad ukazuje kód na stránku ASP.NET pro *nabídky* ovládacího prvku.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Po vykreslení stránky, ovládací prvek vytváří následující HTML ( *onclick* byla vynechána kód pro přehlednost):

[!code-html[Main](overview/samples/sample86.html)]

Kromě vylepšení vykreslování klávesnice navigační nabídky vylepšila pomocí správy fokus. Když *nabídky* řízení získá fokus, můžete použít klávesy se šipkami přejděte elementy. *Nabídky* řízení nyní také připojí dostupné role bohaté aplikace (ARIA) internet a atributy násle[výskytů](http://www.w3.org/TR/wai-aria-practices/#menu "nabídky ARIA pokyny")pro vylepšené usnadnění přístupu.

Styly pro ovládací prvek nabídky jsou vykreslovány v bloku stylu v horní části stránky, nikoli souladu vykreslené elementů HTML. Pokud budete chtít využít plnou kontrolu nad styly pro ovládací prvek, můžete nastavit nový *IncludeStyleBlock* vlastnost *false*, v takovém případě není vygenerované blok stylu. Jedním ze způsobů použití této vlastnosti je použít funkci Automatické formátování v návrháři Visual Studio k nastavení vzhledu nabídky. Můžete pak spuštění stránky, otevřete stránku zdroje a pak zkopírujte blok vykreslené stylu na externí soubor CSS. V sadě Visual Studio, vrátit zpět do stylů a nastavte *IncludeStyleBlock* k *false*. Výsledkem je, že vzhled nabídky je definován pomocí stylů v externí šabloně stylů.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Průvodce a CreateUserWizard ovládací prvky

Technologie ASP.NET *průvodce* a *CreateUserWizard* ovládací prvky podporují šablony, které umožňují definovat HTML, který vykreslují. (*CreateUserWizard* je odvozena z *průvodce*.) Následující příklad ukazuje kód pro plně šablonované *CreateUserWizard* ovládacího prvku:

[!code-aspx[Main](overview/samples/sample87.aspx)]

Ovládací prvek vykreslí HTML podobný následujícímu:

[!code-html[Main](overview/samples/sample88.html)]

V technologii ASP.NET 3.5 SP1, i když změníte obsah šablony, pořád máte omezený počet kontrolu nad výstup *průvodce* ovládacího prvku. V technologii ASP.NET 4, můžete vytvořit *LayoutTemplate* šablony a vložení *zástupný symbol* ovládací prvky (použití vyhrazených názvů) Chcete-li určit, jakým způsobem chcete *řízení průvodce* k vykreslení. Následující příklad ukazuje toto:

[!code-aspx[Main](overview/samples/sample89.aspx)]

V příkladu obsahuje následující s názvem zástupné symboly v *LayoutTemplate* element:

- *headerPlaceholder* – v době běhu, to je nahrazena obsah *HeaderTemplate* elementu.
- *sideBarPlaceholder* – v době běhu, to je nahrazena obsah *třída SideBarTemplate* elementu.
- *wizardStepPlaceHolder* – v době běhu, to je nahrazena obsah *WizardStepTemplate* element.
- *navigationPlaceholder* – v době běhu, to je nahrazena žádné šablony navigace, která jste definovali.

Kód v příkladu, který používá zástupné symboly vykreslí následující HTML (bez obsahu ve skutečnosti definované v šablonách):

[!code-html[Main](overview/samples/sample90.html)]

Je pouze HTML, který není nyní uživatelem definované *span* elementu. (Očekáváme to v budoucích verzích, i na *span* prvek nebude vykreslen.) Tuto chybu získáte plnou kontrolu nad prakticky veškerý obsah, který je generován *průvodce* ovládacího prvku.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC byla zavedena jako představuje rozšíření rozhraní ASP.NET 3.5 SP1 v březnem 2009. Visual Studio 2010 obsahuje ASP.NET MVC 2, která zahrnuje nové funkce a možnosti.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Oblasti podpory

Oblasti umožňují skupiny kontrolery a zobrazení do oddílů velkého aplikace v relativní izolaci od ostatních oddílů. Jako samostatný projektu ASP.NET MVC, který může odkazovat potom hlavní aplikace se dá implementovat každou oblast. To pomáhá spravovat složitost při sestavování rozsáhlé aplikace a usnadňuje více týmů spolupracovat na jednu aplikaci.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Podpora ověřování atributů datové poznámky

*DataAnnotations* atributů umožňují připojit logiku ověření pro model pomocí metadat atributy. *DataAnnotations* atributy byly zavedeny v Dynamická Data technologie ASP.NET v technologii ASP.NET 3.5 SP1. Tyto atributy byly integrovány do výchozí vazač modelu a poskytují prostředky řízené metadata pro ověření vstupu uživatele.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Objekty se šablonami za

Objekty se šablonami za umožňují automaticky přidružit upravit a zobrazit šablony s datovými typy. Například šablony pomocné rutiny můžete použít k určení, že prvku uživatelského rozhraní pro výběr data je automaticky generován pro *System.DateTime* hodnotu. Toto je podobná šablony pole v Dynamická Data technologie ASP.NET.

*Html.EditorFor* a *Html.DisplayFor* pomocné metody mají integrovanou podporu pro vykreslení standardní data typy stejně jako komplexní objekty s více vlastnostmi. Vykreslování se taky přizpůsobit tím, že umožňuje použití datové poznámky atributů jako *DisplayName* a *ScaffoldColumn* k *ViewModel* objektu.

Často chcete přizpůsobit výstup i další pomocné rutiny uživatelského rozhraní a mít plnou kontrolu nad co je vygenerována. *Html.EditorFor* a *Html.DisplayFor* pomocné metody podporují pomocí ukázka mechanismus, který umožňuje definovat externí šablony, které můžete přepsat a řízení výstup vykreslen. Šablony lze vykreslit jednotlivě pro třídu.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dynamická Data

Dynamická Data byla zavedena ve verzi rozhraní .NET Framework 3.5 SP1 v polovině roku 2008. Tato funkce poskytuje mnoho vylepšení pro vytvoření datové aplikace, včetně následujících:

- RAD zkušenosti pro rychlé vytváření webu řízené daty.
- Automatické ověření, která je založená na omezení definovaná v datovém modelu.
- Schopnost snadno změnit kód, který je generována pro pole v *GridView* a *DetailsView* ovládacích prvků pomocí šablon pole, které jsou součástí projektu Dynamická Data.

> [!NOTE]
> Poznámka: Další informace naleznete [Dynamická Data dokumentaci](https://msdn.microsoft.com/en-us/library/cc488545.aspx) v knihovně MSDN.


Pro technologii ASP.NET 4 je vylepšená Dynamická Data umožňuje vývojářům více síly pro rychlé vytváření webů řízené daty.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Povolení dynamické Data pro existující projekty

Dynamické funkce dat, které poskytuje rozhraní .NET Framework 3.5 SP1 začlenění nové funkce, jako jsou následující:

- Šablony polí – zadejte tyto šablony na základě dat typu pro ovládací prvky vázané na data. Pole šablony poskytují jednodušší způsob, jak upravit vzhled prvků dat než při použití šablony polí pro každé pole.
- Ověření – Dynamická Data umožňuje použít atributy na datové třídy k určení ověření pro běžné scénáře, jako je povinná, kontrolu rozsahu, kontrola typu, vzor odpovídající pomocí regulárních výrazů a vlastní ověřování. Ověřování se vynucují ovládacími prvky data.

Ale tyto funkce má následující požadavky:

- Vrstva přístupu k datům se musel být založená na rozhraní Entity Framework nebo technologie LINQ to SQL.
- Ovládací prvky pro tyto funkce byly podporován zdroje pouze dat *EntityDataSource* nebo *LinqDataSource* ovládací prvky.
- Funkce vyžaduje webový projekt, který měl byl vytvořen pomocí Dynamická Data nebo Dynamická Data entity šablony aby bylo možné používat všechny soubory, které byly nezbytné k podpoře funkce.

Hlavní cíl podpory Dynamická Data technologie ASP.NET 4 je povolit nové funkce Dynamická Data pro všechny aplikace ASP.NET. Následující příklad ukazuje kód pro ovládací prvky, které můžete využít výhod funkce Dynamická Data v existující stránku.

[!code-aspx[Main](overview/samples/sample91.aspx)]

V kódu stránky musí být přidán následující kód chcete-li povolit podporu Dynamická Data pro tyto ovládací prvky:

[!code-csharp[Main](overview/samples/sample92.cs)]

Když *GridView* ovládací prvek je v režimu úprav, dynamická Data automaticky ověří, zda zadaná data je ve správném formátu. Pokud není, zobrazí se chybová zpráva.

Tato funkce taky poskytuje další výhody, jako je například moct zadat výchozí hodnoty pro vložení režimu. Bez Dynamická Data, k implementaci výchozí hodnota pro pole, musíte přiřadit události, najít ovládací prvek (pomocí *FindControl*) a nastavení jeho hodnoty. V technologii ASP.NET 4 *EnableDynamicData* volání podporuje druhý parametr, který umožňuje předat výchozí hodnoty pro každé pole v objektu, jak je uvedeno v následujícím příkladu:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Syntaxe deklarativní ovládacího prvku DynamicDataManager

*DynamicDataManager* ovládací prvek je vylepšená tak, aby ji můžete nakonfigurovat deklarativně, stejně jako u většiny ovládacích prvků technologie ASP.NET, místo pouze v kódu. Značku *DynamicDataManager* řízení vypadá jako v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Tato značka umožňuje chování dynamickými daty pro GridView1 ovládací prvek, který odkazuje *DataControls* části *DynamicDataManager* ovládacího prvku.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Šablony entit

Šablony entit nabízí nový způsob přizpůsobení rozložení dat bez nutnosti vytvořit vlastní stránky. Pomocí šablony *FormView* ovládací prvek (místo *DetailsView* řídit, v rámci stránky šablony v dřívějších verzích Dynamická Data) a *DynamicEntity* řízení k vykreslení šablon Entity. To vám dává větší kontrolu nad kód, který je vykreslen metodou Dynamická Data.

Následující seznam uvádí nové rozložení adresář projektu, které obsahuje entity šablony:

[!code-console[Main](overview/samples/sample95.cmd)]

`EntityTemplate` Adresář obsahuje šablony pro zobrazení dat modelu objektů. Ve výchozím nastavení, objekty jsou vykreslovány pomocí `Default.ascx` šablony, která poskytuje kód, který vypadá stejně jako kód vytvořené *DetailsView* ovládací prvek použitý dynamická data technologie ASP.NET 3.5 SP1. Následující příklad ukazuje kód pro `Default.ascx` ovládacího prvku:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Chcete-li změnit vzhled a chování pro celou lokalitu lze upravit výchozí šablony. Šablony pro zobrazení a úpravě operace vložení. Nové šablony mohou být přidány na základě názvu objektu dat aby bylo možné změnit vzhled a chování pouze jeden typ objektu. Můžete například přidat následující šablony:

[!code-console[Main](overview/samples/sample97.cmd)]

Šablona může obsahovat následující kód:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Nové šablony entity se zobrazí na stránce pomocí nové *DynamicEntity* ovládacího prvku. Tento ovládací prvek se v době běhu, nahradí obsah entity šablony. Následující kód ukazuje *FormView* řídit ve `Detail.aspx` stránku šablony, která používá šablony entity. Upozornění *DynamicEntity* elementu v kódu.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-e-mail-addresses"></a>Nové šablony polí pro adresy URL a e-mailové adresy

ASP.NET 4 zavádí dvě nové šablony předdefinované pole `EmailAddress.ascx` a `Url.ascx`. Tyto šablony jsou používány pro pole, které jsou označeny jako *EmailAddress* nebo *Url* s *datový typ* atribut. Pro *EmailAddress* objekty, poli se zobrazí jako hypertextový odkaz, který se vytvoří pomocí *mailto:* protokolu. Když uživatelé kliknou na odkaz, otevře uživatele e-mailového klienta a vytvoří "kostra" zprávu. Objekty typu *Url* se zobrazují jako běžné hypertextové odkazy.

Následující příklad ukazuje, jak by označené pole.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Vytváření odkazů pomocí ovládacího prvku DynamicHyperLink

Dynamická Data používá nová funkce směrování, která byla přidána v rozhraní .NET Framework 3.5 SP1 pro řízení adresy URL, které koncoví uživatelé uvidí, když přistupují k webu. Nové *DynamicHyperLink* ovládací prvek usnadňuje vytváření odkazů na stránky webu s dynamickými daty. Následující příklad ukazuje, jak používat *DynamicHyperLink* ovládacího prvku:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Tento kód vytvoří odkaz, který ukazuje na stránku seznamu `Products` tabulky podle tras, které jsou definovány v `Global.asax` souboru. Ovládací prvek automaticky použije výchozí název tabulky, založený na stránce Dynamická Data.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Podpora pro dědičnosti v datovém modelu

Rozhraní Entity Framework i technologie LINQ to SQL podporují dědičnost ve svých datových modelech. Příkladem může být databáze, která má `InsurancePolicy` tabulky. Může také obsahovat `CarPolicy` a `HousePolicy` tabulky, které mají stejná pole jako `InsurancePolicy` a poté přidejte další pole. Dynamická Data se změnilo, abyste pochopili zděděné objekty v datovém modelu a podporu generování uživatelského rozhraní pro zděděné tabulky.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Podpora pro relace m: N (pouze rozhraní Entity Framework)

Rozhraní Entity Framework má bohatou podporu pro relace m: n mezi tabulkami, která je implementována vystavením vztahu jako kolekce na *Entity* objektu. Nové `ManyToMany.ascx` a `ManyToMany_Edit.ascx` pole šablony byly přidány k zajištění podpory pro zobrazení a úpravy dat, která je součástí relace m: n.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Nové atributy pro řízení zobrazení a podporu výčtů

*DisplayAttribute* přidala tak, abyste získali další řízení zobrazení polí. *DisplayName* atribut v dřívějších verzích Dynamická Data, budete muset změnit název, který se používá jako popisek pro pole povolené. Nové *DisplayAttribute* třída umožňuje určit další možnosti pro zobrazení pole, jako je například pořadí, ve kterém se zobrazí pole a zda pole bude použito jako filtr. Atribut poskytuje také nezávislé ovládání název používaný pro popisky *GridView* řídit, jméno použité v *DetailsView* řídit, text nápovědy pro pole, a použít vodoznak pro pole (Pokud je toto pole přijímá textový vstup).

*EnumDataTypeAttribute* třída byla přidána k umožnění mapování polí pro výčty. Pokud použijete tento atribut pole, je třeba zadat typ výčtu. Dynamická Data používá nový `Enumeration.ascx` pole šablony k vytvoření uživatelského rozhraní pro zobrazení a úpravy hodnot výčtu. Šablona mapuje hodnoty z databáze, které mají názvy ve výčtu.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Vylepšená podpora pro filtry

Dynamická Data 1.0 dodávané s integrované filtry pro logická hodnota sloupce a sloupce cizího klíče. Filtry neumožňuje určit, zda byly zobrazeny nebo pořadí, v jakém byly zobrazeny. Nové *DisplayAttribute* atribut adresy obou těchto problémů tím, že můžete ovládat jestli se sloupce zobrazí jako filtr a v jakém pořadí se bude zobrazovat.

Další vylepšení je, že podpora filtrování byl znovu[zapsána do nové](#0.2__QueryExtender "_QueryExtender") funkce webových formulářů. To vám umožní vytvořit filtry bez nutnosti znalostní báze ovládacího prvku zdroje dat, který se použije filtry. Spolu s těmito příponami filtry také dostalo do šablon ovládacích prvků, které umožňuje přidat nové. Nakonec *DisplayAttribute* třídy již bylo zmíněno dříve umožňuje výchozí filtr k přepsání, stejným způsobem *UIHint* umožňuje pole výchozí šablonu pro sloupec k přepsání.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Visual Studio 2010 Web Development vylepšení

Vývoj webů v sadě Visual Studio 2010 je vylepšená pro větší Kompatibilita šablon stylů CSS, vyšší produktivitu prostřednictvím fragmenty kódu HTML a ASP.NET a nové dynamické IntelliSense JavaScript.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Lepší CSS kompatibilita

Návrhář Visual Web Developer v sadě Visual Studio 2010 je aktualizovaná ke zlepšení dodržování standardů 2.1 šablon stylů CSS. Návrhář lépe chrání integritu zdroji HTML a je větší odolnost než v předchozích verzích sady Visual Studio. Pod pokličkou architektury také vylepšily se bude lépe povolit budoucí vylepšení ve vykreslování, rozložení a použitelnost.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>Fragmenty HTML a JavaScript

V editoru HTML IntelliSense automaticky dokončuje názvy značek. IntelliSense – fragmenty funkce automaticky dokončuje celé značky a další. V sadě Visual Studio 2010 jsou podporovány fragmenty kódu technologie IntelliSense pro jazyk JavaScript, spolu s C# a Visual Basic, které byly podporovány v dřívějších verzích sady Visual Studio.

Visual Studio 2010 obsahuje více než 200 fragmentů, které vám pomůžou automatického dokončování společné značky ASP.NET a HTML, včetně povinné atributy (například runat = "server") a běžné atributy, které jsou specifické pro značku (například *ID*,  *DataSourceID*, *ControlToValidate*, a *Text*).

Další fragmenty si můžete stáhnout, nebo můžete napsat vlastní fragmenty kódu, které zapouzdřují bloky značek, které vy nebo váš tým použít pro běžné úlohy.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Rozšíření JavaScript IntelliSense

V sadě Visual Studio 2010 jsme přepracovali JavaScript IntelliSense zajistit i bohatší úpravy prostředí. Technologie IntelliSense nyní rozpoznává objekty, které byly vytvořeny dynamicky metodami, jako *registerNamespace* a podle podobných technik používaných jinými rámci jazyka JavaScript. Zvýšení výkonu, k analýze velkých knihoven skriptů a zobrazení IntelliSense s minimální nebo žádnou prodleva zpracování. Kompatibilita značně zvýšilo pro podporu téměř všechny knihovny třetích stran a pro podporu různých stylů psaní kódu. Dokumentační komentáře jsou nyní přeložena jako typ a okamžitě využity technologii IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Nasazení webové aplikace sadou Visual Studio 2010

Když vývojáře využívající technologii ASP.NET nasadit webovou aplikaci, často najdou se setkali s problémy, jako je následující:

- Nasazení do sdíleného hostingu lokality vyžaduje technologie jako FTP, který může být pomalé. Kromě toho je nutné ručně provést úlohy, jako je spouštění skriptů SQL pro konfiguraci databáze a je nutné změnit nastavení služby IIS, například konfiguraci virtuální adresář jako aplikace.
- V podnikovém prostředí, kromě nasazení souborů webové aplikace správci často musíte upravit konfigurační soubory technologie ASP.NET a nastavení služby IIS. Správce databáze musí běžet řadu skriptů SQL získat databázi aplikace spuštěna. Tato zařízení jsou celkové náročné, často trvat hodiny a musí být zdokumentována pečlivě.

Visual Studio 2010 obsahuje technologie, které se týkají těchto problémů a které umožňují bezproblémově nasazování webových aplikací. Jedním z těchto technologií je nástroj pro nasazení webu služby IIS (MsDeploy.exe).

Webové funkce nasazení v sadě Visual Studio 2010 následující hlavní oblasti:

- Balení Web
- Transformace Web.config
- Nasazení databáze
- Publikování jedním kliknutím pro webové aplikace

Následující části obsahují podrobnosti o těchto funkcích.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Balení Web

Visual Studio 2010 používá k vytvoření komprimovaný soubor (ZIP) pro vaši aplikaci, která se označuje jako nástroj MSDeploy *webového balíčku*. Soubor balíčku obsahuje metadata o aplikaci a na následující obsah:

- Nastavení služby IIS, která zahrnuje nastavení fondu aplikací, nastavení stránky chyba a tak dále.
- Skutečný obsah Web, který zahrnuje webové stránky, uživatelské ovládací prvky, statický obsah (bitových kopií a soubory HTML) a tak dále.
- Data a schémata databáze systému SQL Server.
- Certifikáty zabezpečení, součástí k instalaci v mezipaměti GAC, nastavení registru a tak dále.

Webový balíček můžete zkopírovat na libovolný server a pomocí Správce služby IIS nainstalovat ručně. Můžete taky pro automatické nasazení, balíček lze nainstalovat pomocí příkazy příkazového řádku nebo pomocí rozhraní API nasazení.

Visual Studio 2010 zajišťuje součástí úlohy nástroje MSBuild a cíle k vytvoření webových balíčků. Další informace najdete v tématu [Přehled nasazení projektu do aplikace ASP.NET webových aplikací](https://msdn.microsoft.com/en-us/library/dd394698%28VS.100%29.aspx) na webu MSDN a [10 + 20 důvodů, proč by měl vytvořit balíček webové](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) na Vishal Joshi blog.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Transformace Web.config

Pro nasazení webových aplikací Visual Studio 2010 zavádí [transformace dokumentů XML (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), což je funkce, která vám umožňuje transformovat `Web.config` soubor z nastavení pro vývoj výrobní nastavení. Nastavení transformace jsou určené v transformace souborů s názvem `web.debug.config`, `web.release.config`a tak dále. (Těchto souborů se názvy shodují s konfigurací nástroje MSBuild.) Transformační soubor obsahuje pouze změny, které musíte udělat na nasazený `Web.config` souboru. Zadáte změny pomocí jednoduché syntaxe.

Následující příklad ukazuje část `web.release.config` souboru, která může být vytvořena pro nasazení vaší verze konfigurace. Nahradit – klíčové slovo v příkladu určuje, že při nasazení *connectionString* uzel v `Web.config` souboru se nahradí hodnoty, které jsou uvedeny v příkladu.

[!code-xml[Main](overview/samples/sample102.xml)]

Další informace najdete v tématu [syntaxe transformace Web.config pro nasazení projektu webové aplikace](https://msdn.microsoft.com/en-us/library/dd465326%28VS.100%29.aspx) na webu MSDN <a id="0.2_a"> </a> webu a[nasazení webu: transformace Web.Config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)na Vishal Joshi blog.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Nasazení databáze

Balíček pro nasazení aplikace Visual Studio 2010 můžou obsahovat závislosti, databáze serveru SQL Server. Jako součást definice balíčku zadejte připojovací řetězec pro zdrojové databáze. Při vytváření balíčku webu Visual Studio 2010 vytvoří skripty SQL pro schéma databáze a volitelně pro data a pak přidá do balíčku. Můžete také zadat vlastní skripty SQL a určit pořadí, ve kterém by měly být spuštěny na serveru. Při nasazení zadejte připojovací řetězec, který je vhodný pro cílový server. proces nasazení ke spouštění skriptů, které vytvořit schéma databáze a přidejte data použije tento připojovací řetězec.

Kromě toho publikovat pomocí jedním kliknutím, můžete nakonfigurovat nasazení k publikování databáze přímo, pokud je publikovaná aplikace vzdálené sdílené hostování lokality. Další informace najdete v tématu [postupy: nasazení databáze s projekt webové aplikace](https://msdn.microsoft.com/en-us/library/dd465343%28VS.100%29.aspx) na webu MSDN a [nasazení databáze s VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) na Vishal Joshi blog.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Publikování jedním kliknutím pro webové aplikace

Visual Studio 2010 také vám umožní používat službu Vzdálená správa služby IIS pro publikování webové aplikace na vzdálený server. Můžete vytvořit profil publikování pro hostitelský účet nebo pro testování serverů nebo přípravu servery. Každý profil můžete bezpečně uložit příslušná pověření. Pak můžete nasadit do některé z cílových serverů s jedním kliknutím pomocí webu jedním kliknutím panel nástrojů publikování. S Visual Studio 2010 můžete také publikovat pomocí příkazového řádku nástroje MSBuild. To vám umožní nakonfigurovat prostředí sestavení team chcete zahrnout do modelu průběžnou integraci publikování.

Další informace najdete v tématu [postupy: nasazení webové aplikace projektu pomocí publikování jedním kliknutím a nasazení webu](https://msdn.microsoft.com/en-us/library/dd465337%28VS.100%29.aspx) na webu MSDN a [webové 1 klikněte na tlačítko Publikovat s VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) na Vishal Joshi blog. Video prezentace nasazení webových aplikací v sadě Visual Studio 2010 naleznete v tématu [VS 2010 pro Web Developer Preview](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) na Vishal Joshi blog.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Prostředky

Na následujících webech poskytují další informace o technologii ASP.NET 4 a Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/en-us/library/ee532866%28VS.100%29.aspx) – oficiální dokumentaci pro technologii ASP.NET 4 na webu MSDN.
- [https://www.ASP.NET/](https://www.asp.net/) – ASP.NET týmu vlastní webové stránky.
- [https://www.ASP.NET/DynamicData/](https://msdn.microsoft.com/en-us/library/cc488545.aspx) a [obsahu mapy Dynamická Data technologie ASP.NET](https://msdn.microsoft.com/en-us/library/cc488545%28VS.100%29.aspx) – prostředků Online na webu technologie ASP.NET a v oficiální dokumentaci pro dynamická Data technologie ASP.NET.
- [https://www.ASP.NET/AJAX/](../../ajax/index.md) – hlavní webové prostředky pro vývoj prvku ASP.NET Ajax.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) – blog The týmu Visual Web Developer, který obsahuje informace o funkcích v sadě Visual Studio 2010.
- [Skvělá webová sada ASP.NET](https://github.com/aspnet/AspNetWebStack) – hlavní webovým prostředkům pro verze preview technologie ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Právní omezení

Toto je předběžná dokument a mohou být změněny podstatně uvedením produktu softwaru popsaných v tomto dokumentu.

Informace obsažené v tomto dokumentu představují aktuální pohled společnosti Microsoft Corporation na uvedené problémy k datu publikování. Protože společnost Microsoft musí reagovat na měnící se podmínky na trhu, by neměl být interpretovat jako závazek společnosti Microsoft a společnost Microsoft nemůže zaručit přesnost informací jsou prezentovány po provedení datu publikování.

Tento dokument White Paper je pouze informativní charakter. MICROSOFT NEPOSKYTUJE ŽÁDNÉ ZÁRUKY, VÝSLOVNÉ, PŘEDPOKLÁDANÉ NEBO STATUTÁRNÍ INFORMACE V TOMTO DOKUMENTU.

Dodržovat všechny platné zákony o autorských právech zodpovídá uživatele. Bez omezení autorských práv, tento dokument může být opakuje, uložené v systému načtení nebo přenést jakýmkoli způsobem (ať už elektronicky, mechanických, včetně kopírování nebo) nebo pro jakýkoli účel bez předchozího písemného svolení společnosti Microsoft Corporation.

Společnost Microsoft může být patentů, patentová aplikací, ochranné známky, autorská práva nebo jiná duševní vlastnictví předmět uvedený v tomto dokumentu. Výslovně uvedeno v licenční smlouvě společnost Microsoft neposkytuje tento dokument žádnou licenci na tyto patenty, ochranné známky, autorská práva nebo jiné duševní vlastnictví.

Pokud není uvedeno jinak, příklady společností, organizací, produktů, názvů domén, e-mailové adresy, loga, osob, míst a událostí použité v ukázkách jsou smyšlené a spojení se žádné skutečnou společností, organizace, produktu, název domény, e-mailu adresu, logem, osoba, místní nebo událostí je určený nebo událostmi.

© 2009 Microsoft Corporation. Všechna práva vyhrazena.

Microsoft a Windows jsou registrované ochranné známky nebo ochranné známky společnosti Microsoft Corporation ve Spojených státech a dalších zemích.

Názvy společností a produktů může být ochranné známky příslušných vlastníků.
