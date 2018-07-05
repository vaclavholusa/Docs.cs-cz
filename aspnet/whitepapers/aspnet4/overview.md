---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 a Visual Studio 2010 – přehled vývoje webu | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument poskytuje přehled o řadu nových funkcí pro technologii ASP.NET, které jsou zahrnuty v rozhraní.NET Framework 4 a v sadě Visual Studio 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 7a75b0c39c923bb500368dbb2304b534d8ed994d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380776"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 a Visual Studio 2010 – přehled vývoje webu
====================
> Tento dokument poskytuje přehled o řadu nových funkcí pro technologii ASP.NET, které jsou zahrnuty v rozhraní.NET Framework 4 a v sadě Visual Studio 2010.
> 
> [Stáhněte si tento dokument White Paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**Obsah**

**[Core Services](#0.2__Toc253429238 "_Toc253429238")**  
[Soubor Web.config refaktoring](#0.2__Toc253429239 "_Toc253429239")  
[Ukládání do mezipaměti Extensible výstup](#0.2__Toc253429240 "_Toc253429240")  
[Automatické spuštění webové aplikace](#0.2__Toc253429241 "_Toc253429241")  
[Trvalé přesměrování stránky](#0.2__Toc253429242 "_Toc253429242")  
[Zmenšení stav relace](#0.2__Toc253429243 "_Toc253429243")  
[Zvětšení rozsahu povolené adresy URL](#0.2__Toc253429244 "_Toc253429244")  
[Ověření požadavku Extensible](#0.2__Toc253429245 "_Toc253429245")  
[Objekt ukládání do mezipaměti a ukládání do mezipaměti rozšiřitelnosti objektu](#0.2__Toc253429246 "_Toc253429246")  
[Rozšiřitelné HTML, URL a kódování hlaviček protokolu HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Monitorování výkonu pro jednotlivé aplikace v jedné pracovní proces](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery zahrnuté s webovými formuláři a MVC](#0.2__Toc253429251 "_Toc253429251")  
[Podpora sítě pro doručování obsahu](#0.2__Toc253429252 "_Toc253429252")  
[ScriptManager Explicit Scripts](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Nastavení metaznaček Page.MetaKeywords a vlastnosti Page.MetaDescription](#0.2__Toc253429257 "_Toc253429257")  
[Umožňuje zobrazit stav jednotlivých ovládacích prvků](#0.2__Toc253429258 "_Toc253429258")  
[Změny možností prohlížeče](#0.2__Toc253429259 "_Toc253429259")  
[Směrování v technologii ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Nastavení ID klienta](#0.2__Toc253429261 "_Toc253429261")  
[Trvalý výběr řádku v ovládacích prvcích dat](#0.2__Toc253429262 "_Toc253429262")  
[ASP.NET Chart Control](#0.2__Toc253429263 "_Toc253429263")  
[Filtrování dat pomocí ovládacího prvku QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Kódovaný výrazů v kódu HTML](#0.2__Toc253429265 "_Toc253429265")  
[Změny v šabloně projektu](#0.2__Toc253429266 "_Toc253429266")  
[Vylepšení šablon stylů CSS](#0.2__Toc253429267 "_Toc253429267")  
[Skrytí div prvky kolem skryté pole](#0.2__Toc253429268 "_Toc253429268")  
[Vykreslování vnější tabulky pro ovládací prvky bez vizuálního vzhledu](#0.2__Toc253429269 "_Toc253429269")  
[Vylepšení ovládacího prvku ListView](#0.2__Toc253429270 "_Toc253429270")  
[CheckBoxList a vylepšení řízení RadioButtonList](#0.2__Toc253429271 "_Toc253429271")  
[Vylepšení správy nabídky](#0.2__Toc253429272 "_Toc253429272")  
[Průvodce a ovládací prvky CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Oblasti podpory](#0.2__Toc253429275 "_Toc253429275")  
[Podpora ověřování dat poznámky atributu](#0.2__Toc253429276 "_Toc253429276")  
[Pomocnými objekty](#0.2__Toc253429277 "_Toc253429277")

**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**  
[Povolení dynamických dat u existujících projektů](#0.2__Toc253429279 "_Toc253429279")  
[Syntaxe deklarativní ovládacího prvku DynamicDataManager](#0.2__Toc253429280 "_Toc253429280")  
[Šablony entit](#0.2__Toc253429281 "_Toc253429281")  
[Nové šablony polí pro adresy URL a e-mailové adresy](#0.2__Toc253429282 "_Toc253429282")  
[Vytváření odkazů pomocí ovládacího prvku DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Podpora dědičnosti v datovém modelu](#0.2__Toc253429284 "_Toc253429284")  
[Podpora pro relace m: N (pouze Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Nové atributy pro ovládací prvek zobrazení a podporu výčtů](#0.2__Toc253429286 "_Toc253429286")  
[Vylepšená podpora pro filtry](#0.2__Toc253429287 "_Toc253429287")

**[Vylepšení Visual Studio 2010 webového vývoje](#0.2__Toc253429288 "_Toc253429288")**  
[Vylepšená Kompatibilita šablon stylů CSS](#0.2__Toc253429289 "_Toc253429289")  
[HTML a JavaScript fragmenty](#0.2__Toc253429290 "_Toc253429290")  
[JavaScript IntelliSense Enhancements](#0.2__Toc253429291 "_Toc253429291")

**[Webové nasazení aplikací pomocí sady Visual Studio 2010](#0.2__Toc253429292 "_Toc253429292")**  
[Web Packaging](#0.2__Toc253429293 "_Toc253429293")  
[Transformace souboru Web.config](#0.2__Toc253429294 "_Toc253429294")  
[Nasazení databáze](#0.2__Toc253429295 "_Toc253429295")  
[Publikování jedním kliknutím pro webové aplikace](#0.2__Toc253429296 "_Toc253429296")  
[Resources](#0.2__Toc253429297 "_Toc253429297")

**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Základní služby

ASP.NET 4 zavádí řadu funkcí, které zlepšují služby ASP.NET core jako je například ukládání výstupu do mezipaměti a ukládání stavu relace.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Soubor Web.config refaktoring

`Web.config` Soubor, který obsahuje konfiguraci pro webovou aplikaci zvětšila výrazně za posledních několik verzí rozhraní .NET Framework byly přidány nové funkce, jako je například Ajax, směrování a integrace se službou IIS 7. To bylo obtížnější konfigurace nebo spuštění nové webové aplikace bez nástroje, jako je Visual Studio. V rozhraní .NET Framework 4, hlavní konfigurační prvky byly přesunuty do `machine.config` souborů a aplikací, které jsou nyní dědí nastavení. Díky tomu `Web.config` soubor v aplikacích ASP.NET 4 prázdný nebo obsahovat jenom následující řádky, které určují verzi rozhraní Framework aplikace je cílen na verzi pro Visual Studio:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Ukládání Extensible výstupu do mezipaměti

Od doby, která byla vydána ASP.NET 1.0 má ukládání výstupu do mezipaměti umožňuje vývojářům ukládat generovaný výstup stránky, ovládací prvky a odpovědi protokolu HTTP v paměti. V následující webové požadavky ASP.NET obsluhoval obsah rychleji načtením generovaný výstup z paměti namísto znova se generuje výstup úplně od začátku. Ale tento přístup má omezení – vygenerovaný obsah má vždy být uloženy v paměti a na serverech, na kterých dochází k velkému provozu se mohou utkat paměti používané ukládání výstupu do mezipaměti s nároky na paměť z jiných částí webové aplikace.

ASP.NET 4 přidá bod rozšiřitelnosti pro ukládání výstupu do mezipaměti, která vám umožní nakonfigurovat jeden nebo více vlastních poskytovatelů výstupní mezipaměti. Výstupní mezipaměť mohou poskytovatelé každý použitý mechanizmus úložiště zachovat obsah HTML. Díky tomu je možné vytvořit vlastního zprostředkovatele mezipaměti výstupu pro různé trvalost mechanismy, které mohou zahrnovat místních nebo vzdálených disků, cloudového úložiště a distribuované mezipaměti.

Vytvoření vlastního zprostředkovatele mezipaměti výstupu jako třída, která je odvozena z nové *System.Web.Caching.OutputCacheProvider* typu. Potom můžete nakonfigurovat poskytovatele v `Web.config` soubor pomocí nových *poskytovatelé* podsekce *outputCache* elementu, jak je znázorněno v následujícím příkladu:

[!code-xml[Main](overview/samples/sample2.xml)]

Ve výchozím nastavení v technologii ASP.NET 4, všechny odpovědi protokolu HTTP, vykreslené stránky a ovládací prvky pomocí mezipaměti v paměti, jak je znázorněno v předchozím příkladu, kde *defaultProvider* atribut je nastaven na AspNetInternalProvider. Můžete změnit výchozí zprostředkovatel výstupní mezipaměti použít pro webovou aplikaci tak, že zadáte název jiného zprostředkovatele *defaultProvider*.

Kromě toho můžete vybrat různé zprostředkovatele výstupní mezipaměti pro ovládací prvek a každý požadavek. Nejjednodušší způsob, jak vybrat různé zprostředkovatele výstupní mezipaměti pro jiné webové uživatelské ovládací prvky je provést to deklarativně pomocí nových *providerName* atribut v direktivě ovládacího prvku, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Určení poskytovatele různé výstupní mezipaměti pro požadavek HTTP vyžaduje trochu více práce. Místo určení deklarativně poskytovatele, přepíšete novou *GetOuputCacheProviderName* metodu `Global.asax` souboru programově zadat poskytovatele pro konkrétní žádost. Následující příklad ukazuje, jak to provést.

[!code-csharp[Main](overview/samples/sample4.cs)]

Přidání rozšíření poskytovatel výstupní mezipaměti ASP.NET 4 můžete nyní fungujícího agresivnější a inteligentnější strategie ukládání výstupu do mezipaměti pro webové servery. Například je nyní možné pro ukládání do mezipaměti na stránkách "Top 10" lokality v paměti, při ukládání do mezipaměti stránek, které získáte nižší provoz na disku. Alternativně můžete ukládat do mezipaměti každou kombinaci se liší podle vykreslované stránky, ale pomocí distribuované mezipaměti tak, aby spotřebu paměti se sníženou zátěží z front-endové webové servery.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Automatické spuštění webové aplikace

Některé webové aplikace potřebovat k načtení velkého objemu dat nebo provádět nákladné inicializace ještě před obsluhou prvního požadavku na zpracování. V předchozích verzích technologie ASP.NET pro tyto situace, museli jste navrhnout vlastní přístupy k "probuzení" aplikace ASP.NET a potom spusťte kód inicializace během *aplikace\_zatížení* metodu `Global.asax` soubor.

Nová funkce škálovatelnost s názvem *automatického spuštění* , že přímo adresy tento scénář je k dispozici ASP.NET 4 spuštění ve službě IIS 7.5 na Windows Server 2008 R2. Funkce automatického spuštění poskytuje řízený přístup pro spuštění fondu aplikací, inicializace aplikace ASP.NET a potom přijímá žádosti protokolu HTTP.

> [!NOTE] 
> 
> Modul zahřívání aplikace služby IIS pro službu IIS 7.5
> 
> Tým služby IIS vydala první verzi beta testování zahřívání modulu aplikace pro službu IIS 7.5. Díky tomu je zahájení práce s vaší aplikací ještě jednodušší než dříve popsané. Místo psaní vlastního kódu, zadejte adresy URL prostředků ke spuštění před webová aplikace přijímá žádosti od sítě. Tato zahřívání nastává při spuštění služby IIS (Pokud jste nakonfigurovali se fond aplikací IIS jako *AlwaysRunning*) a kdy se recykluje pracovní proces služby IIS. Během recyklace i nadále spouštět požadavky, dokud se nově vytvořená pracovního procesu je plně provozní teplotu, tak, aby aplikace prostředí bez přerušení nebo jiné problémy způsobené mezipamětí unprimed původní pracovní proces služby IIS. Všimněte si, že tento modul funguje s všech verzí technologie ASP.NET, počínaje verzí 2.0.
> 
> Další informace najdete v tématu [Application warm-up 1.0](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) na webu IIS.net. Názorný postup ukazuje, jak použít funkci zahřívání, naleznete v tématu [Začínáme s modulem IIS 7.5 aplikace zahřívání](https://www.iis.net/learn/manage) na webu IIS.net.


Použít funkci automatického spuštění, nastaví správce služby IIS ve službě IIS 7.5 s použitím následující konfigurace v automaticky spustit fond aplikací `applicationHost.config` souboru:

[!code-xml[Main](overview/samples/sample5.xml)]

Protože jediného fondu aplikací může obsahovat více aplikací, můžete zadat jednotlivé aplikace automaticky spustit s použitím následující konfigurace v `applicationHost.config` souboru:

[!code-xml[Main](overview/samples/sample6.xml)]

Pokud je server služby IIS 7.5 spouštěná studeného nebo samostatný fond aplikací recykluje, IIS 7.5 pomocí informací v `applicationHost.config` souboru můžete zjistit, které vyžadují webovou aplikací automaticky spustit. Pro každou aplikaci, která je označena pro automatické spouštění IIS 7.5 odešle požadavek na technologii ASP.NET 4 a spusťte tak aplikaci ve stavu, během které aplikaci dočasně nepřijímá požadavky HTTP. Pokud je v tomto stavu, ASP.NET vytvoří instanci typu definovaného *serviceAutoStartProvider* atribut (jak je znázorněno v předchozím příkladu) a zavolá jeho veřejné vstupní bod.

Vytvoření spravované automaticky spouštěná typu s nezbytné vstupní bod implementací *IProcessHostPreloadClient* rozhraní, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](overview/samples/sample7.cs)]

Po inicializaci váš kód běží v *přednačtení* metodou a metodou vrátí, aplikace ASP.NET je připravena ke zpracování požadavků.

Přidání automatické spouštění služby IIS.5 a technologii ASP.NET 4 Teď máte jasně definované přístup k provedení inicializace náročné aplikace před zpracováním první žádosti protokolu HTTP. Například můžete použít novou funkci automatického spuštění pro inicializaci aplikace a pak signál pro vyrovnávání zatížení, že aplikace byla inicializována a připravená přijmout provoz protokolu HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Trvalé přesměrování stránky

To je běžný postup ve webových aplikacích pro přesun stránek a další obsah kolem v čase, což může vést ke kumulaci zastaralé odkazů na vyhledávací weby. V technologii ASP.NET, vývojáři obvykle zpracovat požadavky na původní adresy URL pomocí *Response.Redirect* metodu pro předání požadavku na novou adresu URL. Ale *přesměrování* metoda problémy odpověď HTTP 302 nalezen (dočasné přesměrování), což vede k další HTTP odezvy při uživatelé pokusí přistoupit k původní adresy URL.

ASP.NET 4 přidá nový *RedirectPermanent* Pomocná metoda, která usnadňuje problém HTTP 301 trvale přesunuto odpovědi, jako v následujícím příkladu:

[!code-csharp[Main](overview/samples/sample8.cs)]

Vyhledávací weby a jiní agenti pro uživatele, které rozpoznají trvalé přesměrování uloží novou adresu URL, která souvisí s obsahem, který odstraňuje nepotřebné odezvy provedené v prohlížeči pro dočasné přesměrování.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Zmenšení stav relace

Technologie ASP.NET poskytuje dvě výchozí možnosti pro ukládání stavu relace ve webové farmě: Zprostředkovatel stavu relace, která vyvolá serveru stavu relace mimo proces a zprostředkovatel stavu relace, který ukládá data v databázi Microsoft SQL Server. Vzhledem k tomu, že obě možnosti zahrnují ukládání informací o stavu mimo pracovní proces webové aplikace, musí být serializován, před odesláním do vzdáleného úložiště stavu relace. V závislosti na tom, kolik informací vývojář ukládá stav relace můžou růst poměrně značnou velikost serializovaná data.

ASP.NET 4 zavádí nové možnosti komprese pro oba typy zprostředkovatele stavu relací mimo proces. Když *compressionEnabled* možnost konfigurace je znázorněno v následujícím příkladu je nastavena na *true*, ASP.NET se komprimovat (a dekomprimovat) serializovaný stav relace s použitím rozhraní .NET Framework  *System.IO.Compression.GZipStream* třídy.

[!code-xml[Main](overview/samples/sample9.xml)]

Jednoduché přidání nového atributu `Web.config` souboru aplikací s náhradní cyklů procesoru na webových serverech můžou realizovat značné snížení velikosti serializovaná data stavu relace.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Zvětšení rozsahu povolené adresy URL

ASP.NET 4 zavádí nové možnosti pro rozšíření velikost adresy URL aplikace. Předchozí verze technologie ASP.NET omezením délky cesty adresy URL do 260 znaků podle cesty k souboru omezení systému souborů NTFS. V technologii ASP.NET 4, máte možnost zvětšit (nebo zmenšit) tento limit, v závislosti na vaší aplikace pomocí dvou nových *httpRuntime* atributů konfigurace. Následující příklad ukazuje tyto nové atributy.

[!code-xml[Main](overview/samples/sample10.xml)]

Chcete-li povolit delší nebo kratší cesty (část adresy URL, která nezahrnuje protokol, název serveru a řetězce dotazu), upravte *[parametru maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* atribut. Povolit řetězce dotazu delší nebo kratší, upravte hodnotu *[parametru maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* atribut.

ASP.NET 4 můžete také nakonfigurovat znaky, které jsou používány znak zaškrtnutí adresy URL. Když ASP.NET vyhledá neplatný znak v část cesty adresy URL, zamítne žádost a vyvolá chybu HTTP 400. V předchozích verzích technologie ASP.NET byly omezené na pevnou sadu znaků kontroly znaků adresy URL. V technologii ASP.NET 4, můžete přizpůsobit sadu platné znaky, pomocí nové *requestPathInvalidChars* atribut *httpRuntime* prvek konfigurace, jak je znázorněno v následujícím příkladu:

[!code-xml[Main](overview/samples/sample11.xml)]

Ve výchozím nastavení <em>requestPathInvalidChars</em> atribut definuje osm znaků jako neplatný. (V řetězci, který je přiřazen k <em>requestPathInvalidChars</em> ve výchozím nastavení<em>,</em>menší než (&lt;), je větší než (&gt;) a znak ampersand (&amp;) znaky jsou kódování, protože `Web.config` soubor je soubor XML.) Podle potřeby můžete přizpůsobit sadu neplatné znaky.

> [!NOTE]
> Poznámka: ASP.NET 4 vždy odmítne cestami URL, které obsahují znaky v rozsahu ASCII od 0x00 do 0x1F, protože ty jsou neplatné znaky adresy URL, jak jsou definovány v dokumentu RFC 2396 sdružení IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). Ve verzích Windows serveru, na kterých běží služby IIS 6 nebo vyšší, ovladač http.sys protokolu zařízení automaticky odmítne adresy URL se tyto znaky.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Ověření Extensible žádosti

Ověření požadavku ASP.NET prohledá příchozí data požadavku HTTP pro řetězce, které se běžně používají v útoky skriptování napříč weby (XSS). Pokud se nenajdou potenciální řetězce XSS, žádost o ověření označí podezřelý řetězec a vrátí chybu. Ověření integrované žádosti chybovou zprávu pouze v případě, že nalezne nejběžnější řetězců používané v útoky XSS. Předchozí pokusy o nastavení ověření XSS agresivnější výsledkem příliš mnoho falešných poplachů. Zákazníci však může být vhodné, žádost o ověření, který je agresivnější, nebo naopak může být vhodné záměrně zmírnit XSS kontroly pro konkrétní stránky, nebo pro konkrétní typy žádostí.

V technologii ASP.NET 4 žádosti o ověření funkce byl proveden extensible tak, aby můžete použít logiku vlastního ověření žádosti. Pokud chcete rozšířit ověření žádosti, vytvořte třídu, která je odvozena z nové *System.Web.Util.RequestValidator* typu a nakonfigurovat aplikaci (v *httpRuntime* část `Web.config`souboru) pro použití vlastního typu. Následující příklad ukazuje, jak nakonfigurovat vlastní ověření žádosti třídy:

[!code-xml[Main](overview/samples/sample12.xml)]

Nové *requestValidationType* atribut vyžaduje standardní řetězec identifikátor typ rozhraní .NET Framework, který určuje třídu, která poskytuje vlastní žádosti o ověření. Technologie ASP.NET pro každý požadavek, vyvolá vlastní typ zpracovat každou část příchozí data požadavku HTTP. Adresy URL příchozích, všechny hlavičky protokolu HTTP (soubory cookie a vlastní hlavičky) a obsahu entity jsou všechny dostupné pro kontrolu třídou vlastní žádosti o ověření, je znázorněno v následujícím příkladu:

[!code-csharp[Main](overview/samples/sample13.cs)]

V případech, kdy nechcete ke kontrole část příchozích dat protokolu HTTP, třída ověření žádosti může vrátit zpět k umožní ověření žádosti ASP.NET výchozí spuštění prostým voláním *základní. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Objekt, ukládání do mezipaměti a rozšiřitelnost ukládání objektů

Od své první verze technologie ASP.NET je součástí výkonné objektů v paměti cache (*System.Web.Caching.Cache*). Implementace mezipaměti byla oblíbených tak, že byl použit v jiných webových aplikací. Je ale není vhodný pro aplikaci Windows Forms a WPF zahrnout odkaz na `System.Web.dll` chci mít možnost použití objektu mezipaměti ASP.NET.

Chcete-li ukládání do mezipaměti k dispozici pro všechny aplikace, rozhraní .NET Framework 4 zavádí nové sestavení, nový obor názvů, některé základní typy a konkrétní implementaci mezipaměti. Nové `System.Runtime.Caching.dll` sestavení obsahuje nové rozhraní API ukládání do mezipaměti v *System.Runtime.Caching* oboru názvů. Obor názvů obsahuje dvě základní sady tříd:

- Abstraktní typy, které poskytují základ pro vytváření jakéhokoli typu implementace vlastní mezipaměti.
- Implementace mezipaměti konkrétních objektů v paměti do mezipaměti ( *System.Runtime.Caching.MemoryCache* třídy).

Nové *MemoryCache* třídy je modelována ve vyrovnávací paměti ASP.NET a velkou část logiky modul vnitřní mezipaměti sdílí s technologií ASP.NET. I když veřejné rozhraní API ukládání do mezipaměti v *System.Runtime.Caching* byla aktualizována a podporuje vývoj vlastních mezipamětí, pokud jste už použili technologie ASP.NET *mezipaměti* objektu, najdete známé koncepty Nová rozhraní API.

Podrobné informace o novém *MemoryCache* třídy a podpora základního rozhraní API by vyžadovaly celý dokument. Ale následující příklad dává představu o fungování nové rozhraní API mezipaměti. V příkladu napsané pro aplikace Windows Forms, bez jakékoli závislosti `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Rozšiřitelné HTML, URL a kódování hlaviček protokolu HTTP

V technologii ASP.NET 4 můžete vytvořit vlastní kódování rutiny pro následující běžné úlohy kódování textu:

- Kódování HTML.
- Kódování URL.
- Kódování atributu HTML.
- Kódování odchozí záhlaví HTTP.

Můžete vytvořit vlastní kodér odvozením z nové *System.Web.Util.HttpEncoder* typu a potom konfiguraci technologie ASP.NET použijte vlastní typ v *httpRuntime* část `Web.config` soubor jako můžete vidět v následujícím příkladu:

[!code-xml[Main](overview/samples/sample15.xml)]

Po dokončení konfigurace vlastní kodér, ASP.NET automaticky volá vlastní implementaci kódování vždy, když veřejné metody z kódování *System.Web.HttpUtility* nebo *System.Web.HttpServerUtility* třídy se nazývají. To umožní jedné části webové vývojový tým vytvořit vlastní kodér, který implementuje agresivní kódování znaků, zatímco zbytek týmu vývoje webu používá veřejnou kódování rozhraní API technologie ASP.NET. Vlastní kodér v nakonfigurováním centrálně *httpRuntime* elementu zaručuje, že všechna volání kódování textu z veřejné kódování rozhraní API technologie ASP.NET jsou směrovány vlastní kodér.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Monitorování výkonu pro jednotlivé aplikace v jedné pracovní proces

Pokud chcete zvýšit počet webů, které je možné hostovat na jednom serveru, mnoho hostitelům spustit více aplikací prostředí ASP.NET v jeden pracovní proces. Je-li používat více aplikací jednoho sdíleného pracovního procesu, je ale obtížné pro správce serveru k identifikaci jednotlivých aplikace, ke které dochází k potížím.

ASP.NET 4 využívá nové funkce Sledování prostředků zavedených v modulu CLR. Pokud chcete povolit tuto funkci, můžete přidat následující fragment kódu konfigurace XML pro `aspnet.config` konfigurační soubor.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Poznámka: `aspnet.config` je soubor v adresáři, ve kterém je nainstalováno rozhraní .NET Framework. Není `Web.config` souboru.


Když *appdomainresourcemonitoring –* funkce povolená, dvě nové čítače výkonu jsou k dispozici v kategorii "Aplikací technologie ASP.NET" výkonu: *% času procesoru spravované* a  *Spravované paměti používá*. Obě tyto čítače výkonu použít nové funkce správy prostředků domény aplikace CLR ke sledování odhadovaný čas procesoru a využití spravované paměti jednotlivých aplikací ASP.NET. V důsledku toho v technologii ASP.NET 4 Správci mít podrobnější přehled o spotřebě prostředků jednotlivých aplikací spuštěných v jedné pracovní proces.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Cílení na více verzí

Můžete vytvořit aplikaci, která cílí na konkrétní verzi rozhraní .NET Framework. V technologii ASP.NET 4, nový atribut v *kompilace* elementu `Web.config` souboru umožňuje cílit na rozhraní .NET Framework 4 a novější. Pokud explicitně je cílem rozhraní .NET Framework 4 a zadáte-li volitelné prvky v `Web.config` souboru, například položky pro *system.codedom*, tyto prvky musí být správná pro rozhraní .NET Framework 4. (Pokud sdělení nebudete cílit na explicitně rozhraní .NET Framework 4, Cílová architektura, která je odvozen z chybějící položky v `Web.config` souboru.)

Následující příklad ukazuje použití *targetFramework* atribut *kompilace* elementu `Web.config` souboru.

[!code-xml[Main](overview/samples/sample17.xml)]

Mějte na paměti následující skutečnosti související cílení na určitou verzi rozhraní .NET Framework:

- Ve fondu aplikací rozhraní .NET Framework 4, systém sestavení ASP.NET se předpokládá .NET Framework 4 jako cíl Pokud `Web.config` soubor neobsahuje *targetFramework* atribut nebo, pokud `Web.config` soubor nebyl nalezen. (Může mít kódování měnit vaší aplikace, aby byla spouštěna rozhraní .NET Framework 4.)
- Zadáte-li *targetFramework* atribut a pokud *system.codeDom* element je definován v `Web.config` soubor, tento soubor musí obsahovat správné položky pro rozhraní .NET Framework 4.
- Pokud používáte *aspnet\_kompilátoru* příkazu pro předkompilaci aplikace (například v prostředí sestavení), je nutné použít správnou verzi *aspnet\_kompilátoru* příkaz pro cílovou architekturu. Použijte kompilátor dodávané s rozhraním .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727) ke kompilaci pro rozhraní .NET Framework 3.5 a starší verze. Kompilaci aplikací vytvořených pomocí dané rozhraní nebo pomocí novější verze pomocí kompilátoru, která se dodává s rozhraní .NET Framework 4.
- V době běhu, kterou kompilátor používá nejnovější sestavení rozhraní, které jsou nainstalovány v počítači (a tedy v mezipaměti GAC). Pokud aktualizace je později provedené v rámci (například hypotetické verze 4.1 je nainstalována), budou moct používat funkce v novější verzi rozhraní framework, i když *targetFramework* atribut cílí na starší verzi (například 4.0). (Ale v době návrhu v sadě Visual Studio 2010 nebo při použití *aspnet\_kompilátoru* příkazu, použití novějších funkcí rozhraní Framework způsobí chyby kompilátoru).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>AJAX

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery zahrnuté s webovými formuláři a MVC

Šablony sady Visual Studio pro webové formuláře a MVC zahrnout knihovny jQuery open source. Při vytváření nového webu nebo projekt je vytvořen skripty složku obsahující následující soubory 3:

- jQuery-1.4.1.js – lidsky čitelném, nezmenšená verzi knihovny jQuery.
- jQuery-14.1.min.js – minifikovaný verzi knihovny jQuery.
- jQuery-1.4.1-vsdoc.js – soubor dokumentace technologie Intellisense pro knihovnu jQuery.

Zahrnují nezmenšenou verzi jQuery při vývoji aplikace. Zahrňte minifikovaný verzi jQuery aplikacích v produkčním prostředí.

Například následující stránky webových formulářů ukazuje, jak je možné používat jQuery a změňte barvu pozadí ovládacích prvků technologie ASP.NET textového pole na žlutou, pokud mají fokus.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Podpora služby Content Delivery Network

Microsoft Ajax Content Delivery Network (CDN) umožňuje snadno přidat k vaší webové aplikace technologie ASP.NET Ajax a jQuery skripty. Například můžete začít používat knihovny jQuery jednoduše tak, že přidáte `<script>` značky na stránku, která odkazuje na adresu Ajax.microsoft.com takto:

[!code-html[Main](overview/samples/sample19.html)]

S využitím Microsoft Ajax CDN může výrazně zlepšit výkon aplikací Ajax. Obsah Microsoft Ajax CDN jsou ukládány do mezipaměti na serverech umístěných po celém světě. Kromě toho Microsoft Ajax CDN umožňuje prohlížečům uložené v mezipaměti soubory jazyka JavaScript pro webové stránky, které se nacházejí v různých doménách.

Microsoft Ajax Content Delivery Network podporuje protokol SSL (HTTPS), v případě, že budete muset poskytnout webové stránky s použitím Secure Sockets Layer.

Implementujte záložní CDN není k dispozici. Testování na náhradní řešení.

Další informace o Microsoft Ajax CDN, naleznete na následujícím webu:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

Správce skriptů ASP.NET podporuje Microsoft Ajax CDN. Jednoduše tak, že nastavení jednu vlastnost, vlastnost EnableCdn můžete načíst všechny soubory jazyka JavaScript technologie ASP.NET framework z CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Jakmile nastavíte vlastnost EnableCdn na hodnotu true, rozhraní ASP.NET se načtou všechny soubory jazyka JavaScript technologie ASP.NET framework CDN, včetně všech souborů JavaScript používá pro ověřování polí a prvku UpdatePanel. Nastavení této jednu vlastnost může mít výrazný dopad na výkon webové aplikace.

Můžete nastavit cestu k CDN pro soubory JavaScriptu pomocí atributu webového prostředku. Vlastnosti nového CdnPath Určuje cestu k CDN používá, když nastavíte vlastnost EnableCdn na hodnotu true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Explicitní skripty správce skriptů

V minulosti Pokud jste použili ASP.NET ScriptManger pak jste museli podstupovat načíst celý monolitické knihovna ASP.NET Ajax. S využitím nové vlastnosti ScriptManager.AjaxFrameworkMode, můžete nastavit přesně načíst součásti knihovna ASP.NET Ajax a načíst pouze součásti knihovna ASP.NET Ajax, které potřebujete.

Vlastnost ScriptManager.AjaxFrameworkMode můžete nastavit následující hodnoty:

- Povolené – Určuje, zda ovládací prvek ScriptManager automaticky obsahuje MicrosoftAjax.js souboru skriptu, což je soubor skriptu kombinované každého skriptu framework core (starší chování).
- Zakázané – Určuje, že všechny funkce skripty Microsoft Ajax vypnuté a, že ovládací prvek ScriptManager neodkazuje na žádné skripty automaticky.
- Explicitní – Určuje, že bude obsahovat explicitně skriptových odkazů do jednotlivých framework core skript, který vyžaduje vaše stránka a, že bude obsahovat odkazy na závislosti, které vyžaduje každý soubor skriptu.

Například pokud nastavíte vlastnost AjaxFrameworkMode explicitní hodnotu pak můžete zadat konkrétní skripty komponent technologie ASP.NET Ajax, které potřebujete:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>webové formuláře

Webové formuláře byla funkce jádra ASP.NET oproti verzi 1.0 technologie ASP.NET. Mnoho vylepšení byly v této oblasti pro technologii ASP.NET 4, včetně následujících:

- Možnost nastavit *meta* značky.
- Větší kontrolu nad stavu zobrazení.
- Jednodušší způsoby, jak pracovat s možností prohlížeče.
- Podpora použití směrování s webovými formuláři ASP.NET.
- Větší kontrolu nad generované identifikátory.
- Umožňuje zachovat vybranými řádky v ovládacích prvcích dat.
- Větší kontrolu nad zobrazený HTML v *FormView* a *ListView* ovládacích prvků.
- Podpora pro ovládací prvky zdroje dat filtrování.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Nastavení metaznaček Page.MetaKeywords a Page.MetaDescription vlastnosti

ASP.NET 4 přidá dvě vlastnosti *stránky* třídy, *MetaKeywords* a *MetaDescription*. Tyto dvě vlastnosti představují odpovídající *meta* značky na stránce, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Tyto dvě vlastnosti fungovat stejně způsobu, jakým na stránce *název* nemá vlastnost. Jsou-li postupovat podle těchto pravidel:

1. Pokud existují žádné *meta* značek v *head* element, který se shodovat s názvy vlastností (to znamená, název = "klíčová slova" pro *Page.MetaKeywords* a název = "Popis"  *Page.MetaDescription*, což znamená, že tyto vlastnosti nebyly nastaveny), *meta* značky se přidají do stránky při vykreslení.
2. Pokud už *meta* značky s těmito názvy těchto vlastností představovat get a set metod pro obsah existující značky.

Tyto vlastnosti můžete nastavit v době běhu, který umožňuje získat obsah z databáze nebo jiného zdroje, a který umožňuje nastavit značky dynamicky popište, co je pro konkrétní stránka.

Můžete také nastavit *klíčová slova* a *popis* vlastnosti *@ Page* direktiv v horní části kódu stránky webových formulářů, jako v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Tím se přepíše *meta* obsah (pokud existuje) na stránce už deklarovaný.

Obsah popisu *meta* značky se používají pro vylepšení hledání výpis náhledy v Googlu. (Podrobnosti najdete v tématu [zlepšit fragmenty kódu se jednotlivé popis meta](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) na blogu Google správce webového serveru centrální.) Google a Windows Live Search nepoužívejte obsah klíčových slov pro všechno, co, ale může další vyhledávací weby. Další informace najdete v tématu [Meta klíčová slova Rady](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) na webu vyhledávací modul průvodce.

Tyto nové vlastnosti jsou jednoduchá funkce, ale se ušetřit z požadavku přidejte ručně nebo psaní vlastního kódu k vytvoření *meta* značky.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Umožňuje zobrazit stav jednotlivých ovládacích prvků

Ve výchozím nastavení je povolen stav zobrazení stránky, což má za následek, že každý ovládací prvek na stránce potenciálně uloží stav zobrazení, i v případě, že není nutné pro aplikaci. Data stavu zobrazení je součástí kódu, že na stránce generuje a zvyšuje množství čas potřebný k odeslání stránky klientovi a znovu ji publikovat. Ukládání více zobrazení stavu, než je nezbytné, může způsobit významné snížení výkonu. V předchozích verzích technologie ASP.NET vývojáři může zakázat zobrazit stav jednotlivých ovládacích prvků za účelem snížení velikosti stránky, ale museli dělat to explicitně pro jednotlivé ovládací prvky. V technologii ASP.NET 4, zahrnují ovládací prvky webového serveru *ViewStateMode* vlastnost, která vám umožní zakázat zobrazení stavu ve výchozím nastavení a potom ji povolit pouze pro ovládací prvky, které vyžadují na stránce.

*ViewStateMode* vlastnost přijímá výčet, který má tři hodnoty: *povoleno*, *zakázané*, a *Zdědit*. *Povolené* umožňuje zobrazit stav pro tento ovládací prvek a pro všechny podřízené ovládací prvky, které jsou nastaveny na *Zdědit* nebo mít nic nastaveno. *Zakázané* zakáže zobrazení stavu, a *Zdědit* Určuje, že používá ovládací prvek *ViewStateMode* nastavení z nadřazeného ovládacího prvku.

Následující příklad ukazuje způsob, jakým *ViewStateMode* vlastnost funguje. Hodnoty pro zahrnuje značek a kódu pro ovládací prvky na následující stránce *ViewStateMode* vlastnost:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Jak je vidět, kód zakazuje stav zobrazení ovládacího prvku PlaceHolder1. Podřízený ovládací prvek label1 zdědí tuto hodnotu vlastnosti (*Zdědit* je výchozí hodnota pro *ViewStateMode* pro ovládací prvky.) a proto uloží stav žádné zobrazení. V ovládacím prvku PlaceHolder2 *ViewStateMode* je nastavena na *povoleno*, takže tuto vlastnost dědí label2 a uloží stav zobrazení. Při prvním načtení stránky, *Text* vlastnost objektu i *popisek* ovládacích prvků je nastaven na řetězec "[třídu DynamicValue]".

Efekt z těchto nastavení je, že při prvním načtení stránky, se zobrazí následující výstup v prohlížeči:

Zakázané `: [DynamicValue]`

Povoleno:`[DynamicValue]`

Po zpětné volání, ale se zobrazí následující výstup:

Zakázané `: [DeclaredValue]`

Povoleno:`[DynamicValue]`

Ovládací prvek label1 (jehož *ViewStateMode* nastavena na hodnotu *zakázané*) nebyla zachována hodnotu, která byla nastavena na v kódu. Však určit, label2 (jehož *ViewStateMode* nastavena na hodnotu *povoleno*) je zachována jeho stav.

Můžete také nastavit *ViewStateMode* v *@ Page* direktivy, jako v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample26.aspx)]

*Stránky* třída je právě jiný ovládací prvek; funguje jako nadřazený ovládací prvek pro všechny ostatní ovládací prvky na stránce. Výchozí hodnota *ViewStateMode* je *povoleno* pro instance *stránky*. Protože ovládacích prvků ve výchozím nastavení *Zdědit*, zdědí ovládacích prvků *povoleno* hodnota vlastnosti, pokud nenastavíte *ViewStateMode* na úrovni stránky nebo ovládací prvek.

Hodnota *ViewStateMode* určuje vlastnost, pokud stav zobrazení je zachován pouze v případě *EnableViewState* je nastavena na *true*. Pokud *EnableViewState* je nastavena na *false*, nebude Udržovat stav zobrazení i v případě *ViewStateMode* je nastavena na *povoleno*.

Je vhodné využít k použití této funkce s *ContentPlaceHolder* ovládacích prvků v hlavní stránky, kde můžete nastavit *ViewStateMode* k *zakázané* pro hlavní stránky a pak povolte jednotlivě pro *ContentPlaceHolder* ovládací prvky, které pak obsahovat ovládací prvky, které vyžadují zobrazení stavu.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Změny možnosti prohlížeče

Určuje možnosti prohlížeče, které uživatel používá k procházení webu pomocí funkci s názvem ASP.NET *možnosti prohlížeče*. Možnosti prohlížeče jsou reprezentovány *HttpBrowserCapabilities* objektu (vystavené *Request.Browser* vlastnost). Například můžete použít *HttpBrowserCapabilities* objektem pro určení, zda typu a verzi aktuální prohlížeč podporuje konkrétní verzi jazyka JavaScript. Nebo můžete použít *HttpBrowserCapabilities* objektem pro určení, zda požadavek pochází z mobilního zařízení.

*HttpBrowserCapabilities* objekt doprovází sadu souborů definic prohlížeče. Tyto soubory obsahují informace o možnostech konkrétní prohlížečů. V technologii ASP.NET 4 se aktualizovaly těchto souborů definic prohlížeč bude obsahovat informace o naposledy zavedení prohlížeče a zařízení, jako jsou Google Chrome, výzkumu v pohybu BlackBerry smartphony a Apple iPhone.

Následující seznam obsahuje nový prohlížeč souborů definic:

- *blackberry.browser*
- *chrome.browser*
- *Default.Browser*
- *firefox.browser*
- *gateway.browser*
- *generic.browser*
- *ie.browser*
- *iemobile.browser*
- *iPhone.Browser*
- *opera.browser*
- *safari.browser*

#### <a name="using-browser-capabilities-providers"></a>Použití poskytovatelů možnosti prohlížeče

V technologii ASP.NET 3.5 Service Pack 1, můžete definovat možnosti, které má prohlížeč následujícími způsoby:

- Na úrovni počítače, vytvořit nebo aktualizovat `.browser` soubor XML v následující složce:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Po definování schopnostech prohlížeče spustíte následující příkaz z Visual Studio příkazového řádku Pokud chcete znovu vytvořit sestavení možností prohlížeče a přidejte ji do mezipaměti GAC:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Pro jednotlivé aplikace, můžete vytvořit `.browser` souboru v aplikačním `App_Browsers` složky.

Tyto přístupy vyžadovat změnu souborů XML a změn na úrovni počítače, je nutné restartovat aplikaci po spuštění aspnet\_regbrowsers.exe procesu.

ASP.NET 4 obsahuje funkci označovanou jako *poskytovatelé možností prohlížeče*. Jak název napovídá, díky tomu se vytvořit zprostředkovatele, který můžete použít k určení schopností prohlížeče váš vlastní kód.

V praxi vývojáři často nemá definován vlastní prohlížeč. Prohlížeč souborů jsou těžko aktualizovat, proces aktualizuje je poměrně složité a syntaxe XML pro `.browser` soubory můžou být složité a definovat. Co by tento proces značně zjednodušují je, pokud byly nějaké běžné definice syntaxe prohlížeče, nebo databáze, který obsahoval definice aktuální prohlížeč nebo dokonce i webovou službu pro tyto databáze. Nové funkce poskytovatele možnosti prohlížeče díky scénářů je to možné a praktické pro vývojáře třetích stran.

Existují dva hlavní přístupy pro pomocí nové funkce poskytovatele funkce prohlížeče technologie ASP.NET 4: definice funkce rozšíření schopností prohlížeče technologie ASP.NET nebo úplného nahrazení. Následující části popisují nejprve jak nahradit funkci a jak ji rozšířit.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Nahrazení funkce možností prohlížeče technologie ASP.NET

Pro nahrazení definice funkcí technologie ASP.NET prohlížeče možnosti zcela, postupujte takto:

1. Vytvořte třídu poskytovatele, který je odvozen z *HttpCapabilitiesProvider* a, která přepíše *GetBrowserCapabilities* metody, jako v následujícím příkladu: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Kód v tomto příkladě vytvoří novou *HttpBrowserCapabilities* objektu, zadáním pouze funkci s názvem prohlížeče a nastavíte tuto možnost na MyCustomBrowser.
2. Zaregistrujte poskytovatele s aplikací. 

    Chcete-li použít zprostředkovatele s aplikací, je nutné přidat *poskytovatele* atribut *browserCaps* tématu `Web.config` nebo `Machine.config` soubory. (Můžete také definovat atributů zprostředkovatele *umístění* – element pro konkrétní adresáře v aplikaci, jako je složka pro konkrétní mobilní zařízení.) Následující příklad ukazuje, jak nastavit *poskytovatele* atribut v konfiguračním souboru:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Dalším způsobem, jak registrovat nová definice funkce prohlížeč je při použití kódu, jak je znázorněno v následujícím příkladu:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Tento kód musí být spuštěn *aplikace\_Start* událost `Global.asax` souboru. Jakékoli změny *BrowserCapabilitiesProvider* třídy se musí vyskytovat před provedením žádný kód v aplikaci, pokud chcete mít jistotu, že mezipaměť zůstane v platném stavu pro příkaz vyřešený *HttpCapabilitiesBase* objektu.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Ukládání do mezipaměti HttpBrowserCapabilities objektu

V předchozím příkladu má jeden problém, který je, že by kód spustit pokaždé, když vlastního zprostředkovatele je vyvolána, pokud chcete získat *HttpBrowserCapabilities* objektu. Více než jednou. to může nastat při každém požadavku. V příkladu kódu pro zprostředkovatele není nutné velká. Nicméně pokud kód ve zprostředkovateli vlastní provádí pracné v pořadí zobrazíte *HttpBrowserCapabilities* objektu, to může ovlivnit výkon. K tomu nedocházelo, můžete ukládat do mezipaměti *HttpBrowserCapabilities* objektu. Postupujte podle těchto kroků:

1. Vytvořte třídu, která je odvozena z *HttpCapabilitiesProvider*, například v následujícím příkladu: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    V příkladu se kód vygeneruje klíč mezipaměti pomocí volání vlastní metody BuildCacheKey a získá dobu do mezipaměti pomocí volání vlastní metody GetCacheTime. Kód poté přidá vyřešený *HttpBrowserCapabilities* objektů do mezipaměti. Objekt je možné načíst z mezipaměti a opakovaně použít na následné žádosti, které usnadňují použití vlastního zprostředkovatele.
2. Zaregistrujte poskytovatele s aplikací, jak je popsáno v předchozím postupu.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Rozšíření funkcí možnosti prohlížeče technologie ASP.NET

Předchozí část popisuje, jak vytvořit nový *HttpBrowserCapabilities* objektu v technologii ASP.NET 4. Funkce Možnosti prohlížeče technologie ASP.NET můžete také rozšířit přidáním nových definic možnosti prohlížeče na ty, které už jsou v technologii ASP.NET. Můžete to provést bez použití prohlížeče definice XML. Následující postup ukazuje jak.

1. Vytvořte třídu, která je odvozena z *HttpCapabilitiesEvaluator* a, která přepíše *GetBrowserCapabilities* způsob, jak je znázorněno v následujícím příkladu: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Tento kód používá funkci ASP.NET prohlížeče možnosti nejprve pokusit se identifikovat prohlížeč. Ale pokud se podaří identifikovat žádný prohlížeč na základě informací definované v požadavku (tj. Pokud *prohlížeče* vlastnost *HttpBrowserCapabilities* objektu je řetězec "Neznámý"), kód volá vlastní zprostředkovatel (MyBrowserCapabilitiesEvaluator) k identifikaci prohlížeče.
2. Zaregistrujte poskytovatele s aplikací, jak je popsáno v předchozím příkladu.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Rozšíření prohlížeče možnosti funkce přidáním nových funkcí do existující definice funkce

Kromě vytvoření vlastní prohlížeč definice zprostředkovatele a dynamicky se vytvoří nová definicí prohlížeče můžete rozšířit stávající definicí prohlížeče doplněná o funkce. To vám umožní používat definice, která je blízko co chcete, ale nemá pouze několik možností. Chcete-li to provést, postupujte následovně.

1. Vytvořte třídu, která je odvozena z *HttpCapabilitiesEvaluator* a, která přepíše *GetBrowserCapabilities* způsob, jak je znázorněno v následujícím příkladu: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Ukázkový kód rozšiřuje existující ASP.NET *HttpCapabilitiesEvaluator* třídy a získá *HttpBrowserCapabilities* objekt, který odpovídá aktuální definice požadavku s použitím následujícího kódu :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Kód poté můžete přidat nebo upravit možnosti tohoto prohlížeče. Existují dva způsoby, jak zadat novou funkci prohlížeče:

    - Přidat dvojici klíč/hodnota do *IDictionary* objekt, který je zveřejněný prostřednictvím *možnosti* vlastnost *HttpCapabilitiesBase* objektu. V předchozím příkladu kód přidá možnost s názvem vícedotykové s hodnotou *true*.
    - Nastavit vlastnosti existující *HttpCapabilitiesBase* objektu. V předchozím příkladu kód nastaví *snímků* vlastnost *true*. Tato vlastnost je jednoduše přistupujícího *IDictionary* objekt, který je zveřejněný prostřednictvím *možnosti* vlastnost. 

        > [!NOTE]
        > Poznámka: Tento model se vztahuje na jakoukoli vlastnost *HttpBrowserCapabilities*, včetně adaptérů ovládacích prvků.
2. Zaregistrujte poskytovatele s aplikací, jak je popsáno v předchozím kroku.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Směrování v technologii ASP.NET 4

ASP.NET 4 přidá integrovanou podporu pro použití směrování s webovými formuláři. Směrování umožňuje nakonfigurovat aplikaci tak, aby přijímal žádosti adresy URL, které nelze namapovat na fyzické soubory. Místo toho můžete směrování k definování adresy URL, která jsou srozumitelné pro uživatele a, které mohou pomoci s optimalizací vyhledávače (SEO) pro vaši aplikaci. Adresa URL pro stránku, která zobrazí produktové kategorie v existující aplikaci může například vypadat jako v následujícím příkladu:

[!code-console[Main](overview/samples/sample36.cmd)]

Pomocí směrování, můžete nakonfigurovat aplikaci tak, aby přijímal následující adresu URL k vykreslení tyto informace:

[!code-console[Main](overview/samples/sample37.cmd)]

Směrování byl k dispozici od verze technologie ASP.NET 3.5 SP1. (Příklad toho, jak pomocí směrování v technologii ASP.NET 3.5 SP1, naleznete v příspěvku [pomocí směrování s webových formulářů](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "název této položky.") v blogu phila Haacka.) ASP.NET 4 však zahrnuje některé funkce, které usnadňují použití směrování, včetně následujících:

- *PageRouteHandler* třídy, která je jednoduchý obslužnou rutinu HTTP, které používáte při definování tras. Třída předá data, požadavek se přesměruje na stránku.
- Nové vlastnosti *HttpRequest.RequestContext* a *Page.RouteData* (což je proxy serverem, který *HttpRequest.RequestContext.RouteData* objekt). Tyto vlastnosti usnadňují přístup k informacím, který je předán z trasy.
- Následující nového Tvůrce výrazů, které jsou definovány v *System.Web.Compilation.RouteUrlExpressionBuilder* a *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, která poskytuje jednoduchý způsob, jak vytvořit adresu URL, která odpovídá adrese URL trasy v rámci serverový ovládací prvek ASP.NET.
- *RouteValue*, která poskytuje jednoduchý způsob, jak extrahovat informace z *RouteContext* objektu.
- *RouteParameter* třídu, která je snazší předat data obsažená v *RouteContext* objektu do dotazu pro ovládací prvek zdroje dat (podobně jako [ *FormParameter* ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Směrování pro stránky webových formulářů

Následující příklad ukazuje, jak definovat trasu webových formulářů pomocí nových *MapPageRoute* metodu *trasy* třídy:

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 zavádí *MapPageRoute* metody. Následující příklad je ekvivalentní k definici SearchRoute je znázorněno v předchozím příkladu, ale používá *PageRouteHandler* třídy.

[!code-csharp[Main](overview/samples/sample39.cs)]

Kódem v příkladu mapuje trasu na fyzickou stránku (v prvním postupu k `~/search.aspx`). První definice trasy také určuje, že by měl parametr s názvem searchterm extrahovat z adresy URL a předána na stránku.

*MapPageRoute* metoda podporuje následující přetížení metody:

- *MapPageRoute (routeName řetězec, řetězec routeUrl, řetězec %{physicalfile/, bool checkPhysicalUrlAccess)*
- *MapPageRoute (routeName řetězec, řetězec routeUrl, řetězec %{physicalfile/, bool checkPhysicalUrlAccess, RouteValueDictionary výchozí nastavení)*
- *MapPageRoute (routeName řetězec, řetězec routeUrl, řetězec %{physicalfile/, bool checkPhysicalUrlAccess, RouteValueDictionary výchozí hodnoty, omezení RouteValueDictionary)*

*CheckPhysicalUrlAccess* parametr určuje, zda trasa by měla kontrolovat oprávnění zabezpečení pro fyzickou stránku směrování (v tomto případě search.aspx) a oprávnění na příchozí adrese URL (v tomto případě do vyhledávacího pole / {searchterm}). Pokud hodnota *checkPhysicalUrlAccess* je *false*, zkontroluje pouze oprávnění příchozí adrese URL. Tato oprávnění jsou definovány v `Web.config` soubor pomocí nastavení, jako je následující:

[!code-xml[Main](overview/samples/sample40.xml)]

Příklad konfigurace přístup byl odepřen na fyzickou stránku `search.aspx` pro všechny uživatele kromě těch, kteří jsou v roli správce. Když *checkPhysicalUrlAccess* parametr je nastaven na *true* (což je výchozí hodnota), pouze správci jsou povolena pro přístup k adrese URL /search/ {searchterm}, protože je search.aspx fyzickou stránku omezeno na uživatele v této roli. Pokud *checkPhysicalUrlAccess* je nastavena na *false* a lokalita je nakonfigurovaná, jak je znázorněno v předchozím příkladu, jsou povoleny všechny ověřené uživatele pro přístup k adrese URL /search/ {searchterm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Čtení směrování informace na webové stránce formulářů

V kódu fyzické stránky webových formulářů, dostanete informace, které směrování se extrahují z adresy URL (nebo jiné informace, který má jiný objekt přidán do *RouteData* objektů) pomocí dvou nových vlastností:  *HttpRequest.RequestContext* a *Page.RouteData*. (*Page.RouteData* zabalí *HttpRequest.RequestContext.RouteData*.) Následující příklad ukazuje, jak používat *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Kód extrahuje hodnotu, která byla předána parametru searchterm definované v trase příklad výše. Vezměte v úvahu následující adresu URL požadavku:

[!code-console[Main](overview/samples/sample42.cmd)]

Při této žádosti se slova "scott" by být vykreslen v `search.aspx` stránky.

#### <a name="accessing-routing-information-in-markup"></a>Přístup k informacím o směrování v kódu

Metody popsané v předchozí části ukazuje, jak získat data trasy v kódu v stránky s webovými formuláři. Můžete také použít výrazy ve značkách, které poskytují přístup ke stejným informacím. Tvůrce výrazů jsou výkonné a elegantní způsob, jak pracovat s deklarativního kódu. (Další informace naleznete v příspěvku [Express sami pomocí vlastního tvůrce výrazů](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) v blogu phila Haacka.)

ASP.NET 4 obsahuje dvě nové Tvůrce výrazů pro směrování webových formulářů. Následující příklad ukazuje způsob jejich použití.

[!code-aspx[Main](overview/samples/sample43.aspx)]

V tomto příkladu *RouteUrl* výrazu se používá k definování adresy URL, která je založena na parametru trasy. To vám ušetří pevně zakódovat s úplnou adresu URL do kódu a můžete později změnit strukturu adresy URL bez nutnosti změny na tomto odkazu.

Na základě trasy definovali dříve, tento kód generuje následující adresu URL:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET automaticky funguje správné směrování (to znamená, generuje správnou adresu URL) na základě vstupních parametrů. Můžete použít také název trasy ve výrazu, který umožňuje určit trasu k použití.

Následující příklad ukazuje způsob použití *RouteValue* výrazu.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Při spuštění stránky, která obsahuje tento ovládací prvek je hodnota "scott" zobrazený v popisku.

*RouteValue* výraz umožňují snadno používat data trasy v kódu a jeho se vyhnete nutnosti pracovat s mnohem složitější Page.RouteData["x"] syntaxe v kódu.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Použití dat trasy pro parametrů ovládací prvek zdroje dat

*RouteParameter* třída umožňuje zadat jako hodnotu parametru pro dotazy v ovládacím prvku zdroje dat data trasy. To [funguje podobně jako](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) třídy, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample46.aspx)]

V takovém případě se hodnota searchterm parametr trasa se použije pro @companyname parametr <em>vyberte</em> příkazu.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Nastavení ID klienta

Nové *ClientIDMode* vlastnost řeší dlouhotrvající problém v technologii ASP.NET, a to, jak vytvořit ovládací prvky *id* atribut pro elementy, které vykreslují. Vědět, *id* atribut pro elementy vykreslované je důležité, pokud vaše aplikace obsahuje klientský skript, který odkazuje na tyto prvky.

*Id* atribut v jazyce HTML, který je generován pro ovládací prvky webového serveru je generován a základě *ClientID* vlastnost ovládacího prvku. Až do technologie ASP.NET 4, algoritmus pro generování *id* atribut z *ClientID* vlastnosti se mají zřetězit názvový kontejner (pokud existuje) s ID a v případě opakovaných ovládací prvky (jako v ovládací prvky dat), chcete-li přidat předponu a pořadové číslo. Když to má vždycky zaručeno, že jsou jedinečné identifikátory ovládacích prvků na stránce, algoritmus umožňují řídit ID, které nebyly předvídatelný a byly proto obtížné odkaz v klientského skriptu.

Nové *ClientIDMode* vlastnost umožňuje určit přesnější způsob, jakým vygeneruje ID klienta pro ovládací prvky. Můžete nastavit *ClientIDMode* vlastnost pro libovolný ovládací prvek, včetně stránky. Je to možné nastavení jsou následující:

- *AutoID* – jedná se o ekvivalent algoritmus pro generování *ClientID* hodnoty vlastností použitých v předchozích verzích technologie ASP.NET.
- *Statické* – Určuje, který *ClientID* hodnota bude stejné jako ID bez zřetězení ID nadřazeného objektu pojmenování kontejnerů. To může být užitečné ve webových ovládacích prvcích uživatele. Vzhledem k tomu, že uživatelský ovládací prvek webu mohou být umístěny na různých stránkách a ovládacích prvků jiný kontejner, může být obtížné je napsat skript pro ovládací prvky, které používají klienta *AutoID* algoritmus protože nelze předvídat, co se bude hodnoty ID .
- *Předvídatelný* – tato možnost je především pro použití v ovládacích prvcích dat, které používají opakující se šablony. Zřetězí vlastnosti ID ovládacího prvku pojmenování kontejnerů, ale vygenerovaný *ClientID* hodnoty neobsahují řetězci, například "ctlxxx". Toto nastavení funguje ve spojení s *ClientIDRowSuffix* vlastnost ovládacího prvku. Můžete nastavit *ClientIDRowSuffix* vlastnost na název datového pole a hodnotu daného pole se používá jako přípona pro generované *ClientID* hodnotu. Obvykle byste použili primární klíč datový záznam jako *ClientIDRowSuffix* hodnotu.
- *Dědit* – toto nastavení je výchozí chování pro ovládací prvky, určuje, že generování ID ovládacího prvku je stejný jako jeho nadřazený objekt.

Můžete nastavit *ClientIDMode* vlastností na úrovni stránky. Definuje výchozí *ClientIDMode* hodnotu pro všechny ovládací prvky na aktuální stránce.

Výchozí hodnota *ClientIDMode* hodnota na úrovni stránky je *AutoID*a ve výchozím nastavení *ClientIDMode* hodnotu na úrovni řízení *Zdědit*. V důsledku toho pokud nenastavíte tato vlastnost kdekoli ve vašem kódu, všechny ovládací prvky budou ve výchozím nastavení *AutoID* algoritmus.

Nastavte hodnotu úrovni stránky v *@ Page* směrnice, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Můžete také nastavit *ClientIDMode* hodnoty v konfiguračním souboru na úrovni počítače (počítače) nebo na úrovni aplikace. Definuje výchozí *ClientIDMode* nastavení pro všechny ovládací prvky na všech stránkách v aplikaci. Pokud hodnotu nastavíte na úrovni počítače, definuje výchozí *ClientIDMode* nastavení pro všechny weby na tomto počítači. Následující příklad ukazuje *ClientIDMode* nastavení v konfiguračním souboru:

[!code-xml[Main](overview/samples/sample48.xml)]

Jak bylo uvedeno dříve, hodnota *ClientID* vlastnost je odvozen z názvový kontejner pro nadřazený ovládací prvek. V některých případech, například při použití stránky předlohy ovládací prvky můžete skončit s ID jako v následujícím vykreslení HTML:

[!code-html[Main](overview/samples/sample49.html)]

I v případě, *vstupní* uvedené ve značce elementu (z *textového pole* ovládací prvek) je jenom dva pojmenování kontejnerů hluboké na stránce (vnořeného *ContentPlaceholder* ovládací prvky), kvůli způsobu, jakým jsou zpracovány stránky předlohy konečný výsledek je ID ovládacího prvku, jako je následující:

[!code-console[Main](overview/samples/sample50.cmd)]

Toto ID se musí být jedinečný ve stránce, ale je zbytečně dlouho pro většinu účelů. Představte si, že chcete zkrátit délku vygenerované ID a mít větší kontrolu nad způsob ID generování. (Například chcete odstranit předpony "ctlxxx".) Nejjednodušší způsob, jak toho dosáhnout, je nastavení *ClientIDMode* vlastnost, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample51.aspx)]

V této ukázce *ClientIDMode* je nastavena na *statické* pro nejkrajnější *NamingPanel* elementu a nastaven na *Predictable* pro vnitřní *NamingControl* elementu. Toto nastavení má za následek následující značky (zbytek stránky a stránky předlohy se předpokládá, že stejná jako v předchozím příkladu):

[!code-html[Main](overview/samples/sample52.html)]

*Statické* nastavení má vliv na resetování pojmenování hierarchie pro všechny ovládací prvky uvnitř nejkrajnější *NamingPanel* elementu a vyloučit *ContentPlaceHolder* a *MasterPage* ID z vygenerované ID. ( *Název* atribut vykreslené prvků je poškozena, takže normální funkce technologie ASP.NET je zachován z důvodu události, zobrazení stavu a tak dále.) Vedlejším účinkem resetování hierarchii názvů je, že i v případě, že přesunete značky *NamingPanel* prvků, které mají jiný *ContentPlaceholder* ovládací prvek vykreslené client ID zůstávají stejné.

> [!NOTE]
> Všimněte si, že je na vás, abyste měli jistotu, že jsou jedinečné identifikátory vykreslovaných ovládacích prvků. Pokud nejsou, může dojít k narušení žádné funkce, které vyžaduje jedinečný ID pro jednotlivé elementy HTML, jako je například klient *document.getElementById* funkce.


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Vytváření předvídatelné ID klienta v ovládacích prvcích vázaných na Data

*ClientID* hodnoty, které jsou generovány pro ovládací prvky v ovládacím prvku seznamu vázaného na data pomocí starší verze algoritmu mohou být dlouhé a nejsou ve skutečnosti předvídatelné. *ClientIDMode* funkce vám umožňují mít větší kontrolu nad jak tyto identifikátory jsou generovány.

Obsahuje kód v následujícím příkladu *ListView* ovládacího prvku:

[!code-aspx[Main](overview/samples/sample53.aspx)]

V předchozím příkladu *ClientIDMode* a *RowClientIDRowSuffix* vlastnosti nastavené v kódu. *ClientIDRowSuffix* vlastnost lze použít pouze v ovládacích prvcích vázaných na data, a její chování se liší v závislosti na tom, který ovládací prvek, kterou používáte. Rozdíly jsou tyto:

- *GridView* ovládacího prvku – můžete určit, název jednoho nebo více sloupců ve zdroji dat, které jsou zkombinované v době běhu k vytvoření ID klienta. Například pokud nastavíte *RowClientIDRowSuffix* na "ProductName; ProductId", určit ID pro elementy vykreslované bude mít formát podobný tomuto:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* ovládacího prvku – můžete zadat jeden sloupec ve zdroji dat, která se připojuje k ID klienta. Například pokud nastavíte *ClientIDRowSuffix* na "ProductName"; ID vykreslovaných ovládacích prvků bude mít formát podobný tomuto:

- [!code-console[Main](overview/samples/sample55.cmd)]

- V tomto případě koncové 1 je odvozen od ID produktu z aktuální datové položky.

- *Repeater* ovládacího prvku – tento ovládací prvek není podporován *ClientIDRowSuffix* vlastnost. V *Repeater* slouží ovládací prvek, index aktuálního řádku. Při použití ClientIDMode = "Predictable" with *Repeater* řízení, klienta jsou generovány ID, které mají tento formát:

- [!code-console[Main](overview/samples/sample56.cmd)]

- Koncové 0 je index aktuálního řádku.

*FormView* a *DetailsView* ovládací prvky, které nepodporují nezobrazují více řádků *ClientIDRowSuffix* vlastnost.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Trvalý výběr řádku v ovládacích prvcích dat

*GridView* a *ListView* ovládací prvky můžete umožnit uživatelům vybrat řádek. V předchozích verzích technologie ASP.NET byly výběr podle index řádku na stránce. Například pokud vyberte třetí položka na stránce 1 a potom přejít na stránku 2, třetí položka na této stránce vybrali.

Trvalý výběr byl zpočátku podporována pouze v projektech dynamických dat v rozhraní .NET Framework 3.5 SP1. Pokud je tato funkce povolena, aktuální vybrané položky podle klíče dat pro položku. To znamená, že pokud vyberete třetí řádek na stránce 1 a přejít na stránku 2, nevybere se na stránce 2. Když se vrátíte na stránku 1, třetí řádek je stále vybrán. Trvalý výběr se teď podporuje pro *GridView* a *ListView* ovládacích prvků ve všech projektech pomocí *EnablePersistedSelection* vlastnost, jak je znázorněno Následující příklad:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Ovládací prvek ASP.NET grafu

Technologie ASP.NET *grafu* rozšíří ovládací prvek nabídky vizualizace dat v rozhraní .NET Framework. Použití *grafu* ovládacího prvku, můžete vytvořit stránky technologie ASP.NET, které mají intuitivní a vizuálně působivé grafy pro komplexní statistické nebo finanční analýzu. Technologie ASP.NET *grafu* ovládací prvek byl zaveden jako doplněk k vydání verze rozhraní .NET Framework verze 3.5 SP1 a je součástí verze rozhraní .NET Framework 4.

Ovládací prvek obsahuje následující funkce:

- 35 typy různých grafů.
- Neomezený počet oblasti grafu, názvy, legendy a poznámky.
- Celou řadu nastavení vzhledu pro všechny prvky grafu.
- 3D podpory u většiny typů grafů.
- Inteligentní popisky, které může automaticky přizpůsobit kolem datových bodů.
- Čáry pruhů, oddělovacích čar měřítka osy a logaritmické měřítko.
- Více než 50 finanční a statistické vzorce pro analýzu dat a transformace.
- Jednoduchá vazba a manipulace dat grafu.
- Podpora pro běžné formáty dat, jako je například data, času a měny.
- Podpora přizpůsobení založený na událostech a interaktivitě, včetně klienta klikněte na tlačítko události pomocí rozhraní Ajax.
- Správa stavu.
- Binární datové proudy.

Následující obrázky znázorňují příklady finanční grafy, které vytváří ovládací prvek ASP.NET grafu.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Obrázek 2: Příklady ovládacích prvků technologie ASP.NET grafu

Pro další příklady toho, jak pomocí ovládacího prvku grafu ASP.NET stáhnout ukázkový kód [ukázky prostředí pro Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) stránky na webu MSDN. Další ukázky komunity můžete najít v obsahu [fórum ovládacího prvku grafu](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Přidání ovládacího prvku grafu na stránku ASP.NET

Následující příklad ukazuje, jak přidat *grafu* ovládacího prvku pro stránku ASP.NET pomocí značek. V tomto příkladu *grafu* ovládací prvek vytvoří sloupcový graf na základě statických datových bodů.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Používání 3D grafů

*Grafu* obsahuje ovládací prvek *ChartAreas* kolekce, která může obsahovat *ChartArea* objekty, které definují vlastnosti oblasti grafu. Například pokud chcete použít 3D pro oblasti grafu, použijte *Area3DStyle* vlastnost jako v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample59.aspx)]

Následující obrázek znázorňuje 3D grafem s čtyři řady *panelu* typ grafu.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Obrázek 3: 3D pruhový graf

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Pomocí oddělovacích čar měřítka osy a logaritmické stupnice

Jsou dvě další způsoby, jak do grafu přidat sofistikovanější oddělovacích čar měřítka osy a logaritmických stupnicí. Tyto funkce jsou specifické pro každou osu v oblasti grafu. Například pokud chcete použít tyto funkce na primární osy Y oblasti grafu, použijte *AxisY.IsLogarithmic* a *ScaleBreakStyle* vlastnosti v *ChartArea* objektu. Následující fragment kódu ukazuje, jak pomocí oddělovacích čar měřítka osy na primární ose Y.

[!code-aspx[Main](overview/samples/sample60.aspx)]

Obrázek níže ukazuje osu Y pomocí oddělovacích čar měřítka osy povolena.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Obrázek 4: Oddělovacích čar měřítka osy

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtrování dat pomocí ovládacího prvku QueryExtender

K filtrování dat je velmi běžné úlohy pro vývojáře, kteří vytvářejí datově řízených webových stránek. Tradičně provedení vytvořením *kde* ovládací prvky zdroje klauzule v datech. Tento přístup může být složité a v některých případech *kde* syntaxe nebude využívat všechny funkce podkladové databáze.

Aby filtrování snadněji, nový *QueryExtender* ovládací prvek byl přidán v technologii ASP.NET 4. Tento ovládací prvek lze přidat do *EntityDataSource* nebo *LinqDataSource* ovládací prvky, chcete-li filtrovat data vrácená z těchto ovládacích prvků. Vzhledem k tomu, *QueryExtender* ovládací prvek závisí na LINQ, filtr je použit na databázovém serveru předtím, než se odešlou na stránku, která má za následek velmi efektivní operace.

*QueryExtender* ovládací prvek podporuje širokou škálu možností filtrování. Následující části popisují tyto možnosti a poskytnout příklady, jak je používat.

#### <a name="search"></a>Hledat

Pro možnost Hledat *QueryExtender* ovládání provede vyhledávání v zadaná pole. V následujícím příkladu používá ovládací prvek text, který je zadán v TextBoxSearch ovládacího prvku a hledání obsahu v `ProductName` a `Supplier.CompanyName` sloupce v datech, která je vrácena z *LinqDataSource* ovládací prvek.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Rozsah

Možnost rozsah je podobné možnosti vyhledávání, ale Určuje dvojici hodnot k definování rozsahu. V následujícím příkladu *QueryExtender* ovládací prvek vyhledávání `UnitPrice` sloupec data vrácená z *zdroje dat LinqDataSource* ovládacího prvku. Rozsah je od TextBoxFrom a TextBoxTo ovládacích prvků na stránce pro čtení.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

Možnost Vlastnosti výrazu umožňuje definovat porovnání s hodnotou vlastnosti. Pokud je výraz vyhodnocen *true*, je vrácena data, která je zkoumají. V následujícím příkladu *QueryExtender* ovládací prvek filtruje data pomocí dat v porovnání `Discontinued` sloupce na hodnotu z ovládacího prvku CheckBoxDiscontinued na stránce.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Nakonec můžete určit pomocí vlastního výrazu *QueryExtender* ovládacího prvku. Tato možnost umožňuje volání funkce stránky, které definuje vlastní filtr logiku. Následující příklad ukazuje, jak deklarativně specifikovat vlastního výrazu v *QueryExtender* ovládacího prvku.

[!code-aspx[Main](overview/samples/sample64.aspx)]

Následující příklad ukazuje vlastní funkce, která je volána *QueryExtender* ovládacího prvku. V tomto případě namísto použití databázového dotazu, který zahrnuje *kde* klauzule, tento kód použije dotaz LINQ filtrovat data.

[!code-csharp[Main](overview/samples/sample65.cs)]

Tyto příklady ukazují jenom jeden výraz používá *QueryExtender* ovládací prvek v čase. Však může obsahovat několik výrazů uvnitř *QueryExtender* ovládacího prvku.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Kódovaný výrazů v kódu HTML

Některé weby technologie ASP.NET (zejména s ASP.NET MVC) výrazně spoléhají na použití `<%` =  `expression %>` syntaxe (často označované jako "code útržky") pro zápis textu do odpovědi. Při použití výrazů v kódu, je snadné zapomenout určený ke kódování HTML, text, pokud text pochází od uživatele zadání, ho můžete nechat stránky otevřené útoky XSS (skriptování mezi).

ASP.NET 4 zavádí následující nové syntaxe pro výrazy kódu:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Tato syntaxe používá kódování HTML ve výchozím nastavení při zápisu do odpovědi. Tento nový výraz se přeloží efektivně takto:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Například &lt;%: % žádost o ["UserInput"]&gt; provádí na základě hodnoty kódování HTML *žádost o ["UserInput"]*.

Cílem této funkce je možné nahradit všechny výskyty stará syntaxe novou syntaxi tak, aby se muset rozhodnout při každém kroku, který z nich použít. Existují však případy, ve kterých je výstup má být ve formátu HTML nebo je již kódovat, v takovém případě to může vést k double kódování.

Pro případy, technologii ASP.NET 4 zavádí nové rozhraní *IHtmlString*, spolu s konkrétní implementaci *HtmlString*. Instance těchto typů umožňují značí, že návratová hodnota je již správně kódovaný (nebo jinak prozkoumat) pro zobrazení ve formátu HTML a, proto hodnota by neměla být kódovaný jazykem HTML znovu. Například následující by neměl být (a není) kódovaný jazykem HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

ASP.NET MVC 2 pomocné metody bylo aktualizováno, aby tuto novou syntaxi pracovat tak, aby nebyly double, kódování, ale pouze při spouštění technologie ASP.NET 4. Tato nová syntaxe nefunguje při spuštění aplikace pomocí technologie ASP.NET 3.5 SP1.

Uvědomte si, že nejsou tím však zaručena ochranu před útoky XSS. Kód HTML, který používá hodnoty atributů, které nejsou v uvozovkách může například obsahovat uživatelský vstup, přesto náchylné. Všimněte si, výstup ovládací prvky technologie ASP.NET a ASP.NET MVC pomocné rutiny vždy obsahuje hodnoty atributů do uvozovek, což je doporučený postup.

Tato syntaxe, neprovádí kódování, JavaScript, například při vytváření řetězec jazyka JavaScript v závislosti na vstup uživatele.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Změny v šabloně projektu

V předchozích verzích technologie ASP.NET, když pomocí sady Visual Studio vytvořte nový projekt webu nebo projekt webové aplikace a výsledné projekty obsahují jenom stránku Default.aspx, výchozí `Web.config` souboru a `App_Data` složky, jak je znázorněno v následujícím Obrázek:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio také podporuje typ projektu prázdný web, který neobsahuje žádné soubory vůbec, jak je znázorněno na následujícím obrázku:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Výsledkem je, že pro začátečníky, je velmi málo pokyny o tom, jak vytvářet produkční webové aplikace. ASP.NET 4 proto zavádí tři nové šablony, jeden pro prázdný projekt webové aplikace a jeden pro projekt webové aplikace a webové stránky.

#### <a name="empty-web-application-template"></a>Šablona prázdná webové aplikace

Jak název napovídá, šablona prázdná webová aplikace je stripped-down projektu webové aplikace. Vyberte tuto šablonu projektu v dialogovém okně Nový projekt sady Visual Studio, jak je znázorněno na následujícím obrázku:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](overview/_static/image8.png))

Když vytvoříte prázdný webové aplikace ASP.NET, sada Visual Studio vytvoří následující složky rozložení:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

To se podobá rozložení Prázdný web ze starších verzí prostředí ASP.NET, s jednou výjimkou. V sadě Visual Studio 2010, prázdná webová aplikace a prázdného webu projekty obsahují následující minimální `Web.config` soubor, který obsahuje informace, které slouží k identifikaci rozhraní framework, pro kterou projekt cílí pomocí sady Visual Studio:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Bez něj *targetFramework* vlastnost, výchozí hodnota je Visual Studio k zachování kompatibility při otevírání starší aplikace cílí na rozhraní .NET Framework 2.0.

#### <a name="web-application-and-web-site-project-templates"></a>Webová aplikace a šablony projektu webového serveru

Další dvě nové šablony projektů, které jsou součástí sady Visual Studio 2010 obsahují důležité změny. Následující obrázek znázorňuje rozložení projektu, který je vytvořen při vytvoření nového projektu webové aplikace. (Rozložení pro webový projekt je prakticky totožný.)

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Projekt obsahuje několik souborů, které nebyly vytvořeny v dřívějších verzích. Kromě toho nový projekt webové aplikace se nakonfigurují funkce základního členství, která umožňuje rychle začít pracovat v zabezpečení přístupu k nové aplikaci. Z důvodu této zařazení `Web.config` soubor pro nový projekt obsahuje položky, které se používají ke konfiguraci členství, role a profily. Následující příklad ukazuje `Web.config` soubor pro nový projekt webové aplikace. (V tomto případě *roleManager* je zakázaná.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](overview/_static/image14.png))

Projekt obsahuje také druhý `Web.config` soubor `Account` adresáře. Druhý soubor konfigurace zajišťuje zabezpečený přístup ke stránce ChangePassword.aspx nezaprotokolované u uživatelů. Následující příklad ukazuje obsah druhého `Web.config` souboru.

![](overview/_static/image15.png)

Stránky vytvořené ve výchozím nastavení nové projektové šablony také obsahovat více obsahu, než v předchozích verzích. Projekt obsahuje výchozí stránky předlohy a souborů šablon stylů CSS a výchozí stránka (Default.aspx) je nakonfigurován na použití stránky předlohy ve výchozím nastavení. Výsledkem je, že při spuštění webové aplikace nebo webu poprvé, výchozí stránka (domů) je již funkční. Ve skutečnosti je podobná výchozí stránky, které se zobrazí po spuštění nové aplikace MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](overview/_static/image18.png))

Záměrem těchto změn šablon projektu je s pokyny o tom, jak můžete začít sestavovat novou webovou aplikaci. S sémanticky, striktní XHTML 1.0 kompatibilní značek a rozložení, který je určen pomocí šablon stylů CSS stránek v šablonách představují doporučené postupy pro vytváření aplikací v prostředí ASP.NET 4. Výchozí stránky mají navíc rozložení se dvěma sloupci, který můžete snadno přizpůsobit.

Představte si například, že pro nové webové aplikace chcete změnit některé barvy a vložit loga společnosti místo logo Moje aplikace technologie ASP.NET. K tomuto účelu vytvořte nový adresář v rámci `Content` k uložení obrázek loga:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Chcete-li přidat bitovou kopii na stránku, poté otevřete `Site.Master` souboru, kde je definován text My ASP.NET Application a nahraďte ho hodnotou *image* elementu jehož *src* atribut je nastaven na nové logo Image, jako v následujícím příkladu:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](overview/_static/image22.png))

Můžete přejít do souboru Site.css a změnit definice třídy šablony stylů CSS změnit barvu pozadí stránky stejně jako u záhlaví.

Výsledkem těchto změn je, že můžete zobrazit vlastní domovskou stránku s velmi málo úsilí:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Vylepšení šablon stylů CSS

Jedna z hlavních oblastech práce v technologii ASP.NET 4 se stále k pomáhají vykreslovat kód HTML, který je v souladu s nejnovějšími standardy HTML. To zahrnuje změny jak serverových ovládacích prvků ASP.NET pomocí stylů CSS.

#### <a name="compatibility-setting-for-rendering"></a>Nastavení kompatibility pro vykreslování

Ve výchozím nastavení, když webová aplikace nebo webu cílí na rozhraní .NET Framework 4 *controlRenderingCompatibilityVersion* atribut *stránky* prvek je nastaven na "4.0". Tento element je definována v úrovni počítače `Web.config` souboru a ve výchozím nastavení se vztahuje na všechny aplikace ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

Hodnota pro *controlRenderingCompatibility* je řetězec, který umožňuje potenciální nové definice verze v budoucích vezích se. V aktuální verzi jsou podporovány následující hodnoty pro tuto vlastnost:

- "3.5". Toto nastavení označuje starší vykreslování a značky. Kód pro vykreslení ovládacích prvků je zpětně kompatibilní 100 % a nastavení *xhtmlConformance* vlastnost zachovaný.
- "4.0". Pokud toto nastavení má vlastnost, ovládací prvky ASP.NET webového serveru postupujte takto:
- *XhtmlConformance* vlastnost je vždy považován za "Strict". V důsledku toho ovládací prvky vykreslení kódu XHTML 1.0 Strict.
- Zakázání ovládací prvky bez zadání už vykreslí neplatný styly.
- *div* prvky kolem skrytá pole jsou nyní ve stylu tak, že není konfliktu s uživatelsky vytvořených pravidel šablon stylů CSS.
- Ovládací prvky nabídky vykreslení značky, které jsou sémanticky správné a jestli splňují přístupnosti.
- Ovládací prvky ověřování nezobrazují vložené styly.
- Ovládací prvky, které dříve vykresluje ohraničení = "0" (ovládací prvky, které jsou odvozeny z technologie ASP.NET *tabulky* ovládacího prvku a technologie ASP.NET *Image* ovládací prvek) už zobrazovat tento atribut.

#### <a name="disabling-controls"></a>Zakázání ovládacích prvků

V technologii ASP.NET 3.5 SP1 a předchozí verze rozhraní framework vykreslí *zakázané* atribut v kódu HTML pro všechny ovládací prvek, jehož *povoleno* vlastnost nastavena na hodnotu *false*. Nicméně podle specifikace HTML 4.01, pouze *vstupní* prvky musí mít tento atribut.

V technologii ASP.NET 4, můžete nastavit *controlRenderingCompatabilityVersion* vlastnost "3.5", jako v následujícím příkladu:

[!code-xml[Main](overview/samples/sample70.xml)]

Můžete například vytvořit značku *popisek* ovládacího prvku následujícím postupem, který zakáže ovládací prvek:

[!code-aspx[Main](overview/samples/sample71.aspx)]

*Popisek* vykreslení ovládacího prvku HTML následující:

[!code-html[Main](overview/samples/sample72.html)]

V technologii ASP.NET 4, můžete nastavit *controlRenderingCompatabilityVersion* "4.0". V takovém případě je řízeno jen tento vykreslení *vstupní* prvky vykreslí *zakázané* atribut při ovládacího prvku *povoleno* je nastavena na *false* . Ovládací prvky, které nezobrazují HTML *vstupní* místo vykreslení elementů *třídy* atribut, který odkazuje na třídu šablony stylů CSS, která můžete použít k definování zakázané vzhled ovládacího prvku. Například *popisek* ovládací prvek je znázorněno v předchozím příkladu by generují následující kód:

[!code-html[Main](overview/samples/sample73.html)]

Výchozí hodnota pro třídu, která zadané pro tento ovládací prvek je "aspNetDisabled". Však můžete změnit výchozí hodnotu nastavením statické *DisabledCssClass* statická vlastnost *WebControl* třídy. Pro vývojáře řízení chování pro konkrétní ovládací prvek lze také definovat pomocí *SupportsDisabledAttribute* vlastnost.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Skrytí div prvky kolem skrytá pole

ASP.NET 2.0 a novějších verzích vykreslovat skrytá pole specifické pro systém (například *skryté* element sloužící k ukládání informací o stavu zobrazení) uvnitř *div* – element pro dosažení souladu se standardem XHTML. Nicméně to může způsobit problém při má vliv na pravidla šablony stylů CSS *div* elementů na stránce. Například to může vést k řádku jeden pixel povolí, na stránce kolem skryté *div* elementy. V technologii ASP.NET 4 *div* prvky, které uzavřete skrytých polí generovaných ASP.NET přidejte odkaz na třídu šablony stylů CSS jako v následujícím příkladu:

[!code-html[Main](overview/samples/sample74.html)]

Potom můžete definovat třídu CSS, která se vztahuje pouze na *skryté* prvky, které jsou generovány pomocí technologie ASP.NET, jako v následujícím příkladu:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Vykreslování vnější tabulky pro ovládací prvky bez vizuálního vzhledu

Ve výchozím nastavení jsou následující ovládací prvky serveru v prostředí ASP.NET, které nepodporují šablony automaticky zabaleny ve vnější tabulky, která se používá k aplikování vložené styly:

- *FormView*
- *přihlášení*
- *PasswordRecovery*
- *Metodu ChangePassword*
- *Průvodce*
- *CreateUserWizard*

Novou vlastnost s názvem *vlastnost RenderOuterTable* byl přidán do těchto ovládacích prvků, které umožňuje vnější tabulka, která má být odebrán z kódu. Zvažte například následující příklad *FormView* ovládacího prvku:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Tento kód vykreslí na stránku, která obsahuje tabulku HTML následující výstup:

[!code-html[Main](overview/samples/sample77.html)]

Pokud chcete zabránit vykreslení v tabulce, můžete nastavit *FormView* ovládacího prvku *vlastnost RenderOuterTable* vlastnosti, jako v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample78.aspx)]

Předchozí příklad vykreslí následující výstup, aniž by *tabulky*, *tr*, a *td* prvky:

> Obsah


Toto vylepšení můžete usnadňují styl obsah ovládacího prvku pomocí šablon stylů CSS, protože žádné neočekávané značky jsou vykreslovány pomocí ovládacího prvku.

> [!NOTE]
> Poznámka: Tato změna zakáže podporu pro funkci Automatické formátování v návrháři aplikace Visual Studio 2010, protože už není *tabulky* element, který může hostovat atributy stylu, které jsou generovány možnost automaticky formátovat.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Vylepšení ovládacího prvku ListView

*ListView* ovládacího prvku provedl usnadňuje používání v technologii ASP.NET 4. Starší verze ovládacího prvku vyžaduje, abyste určili šablony rozložení, který obsahoval serverový ovládací prvek s ID známé. Následující kód ukazuje, jak používat Typickým příkladem *ListView* ovládacího prvku v technologii ASP.NET 3.5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

V technologii ASP.NET 4 *ListView* ovládací prvek nevyžaduje, aby šablona rozložení. Značek je znázorněno v předchozím příkladu je možné nahradit následující kód:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>CheckBoxList a rozšíření RadioButtonList ovládacího prvku

V technologii ASP.NET 3.5, můžete zadat rozložení *CheckBoxList* a *RadioButtonList* pomocí následující dvě nastavení:

- *Tok*. Ovládací prvek vykreslí *span* prvky tak, aby obsahovala jeho obsah.
- *Tabulka*. Ovládací prvek vykreslí *tabulky* element tak, aby obsahovala jeho obsah.

Následující příklad ukazuje značky pro každý z těchto ovládacích prvků.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Ve výchozím nastavení ovládací prvky vykreslí ve formátu HTML podobný následujícímu:

[!code-html[Main](overview/samples/sample82.html)]

Protože tyto ovládací prvky obsahují seznamy položek k vykreslení sémanticky HTML vykreslovat jejich obsah pomocí seznamu HTML (*li*) elementy. To usnadňuje pro uživatele, kteří webových stránek pomocí technologie pro usnadnění čtení a usnadňuje ovládací prvky stylu pomocí šablon stylů CSS.

V technologii ASP.NET 4 *CheckBoxList* a *RadioButtonList* řídí podporu pro následující nové hodnoty *RepeatLayout* vlastnost:

- *OrderedList* – obsah se vykreslí jako *li* elementů v rámci *ol* elementu.
- *Rozložení UnorderedList* – obsah se vykreslí jako *li* elementů v rámci *ul* elementu.

Následující příklad ukazuje, jak použít tyto nové hodnoty.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Předchozí kód generuje následující kód HTML:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Poznámka: Pokud nastavíte *RepeatLayout* k *OrderedList* nebo *rozložení UnorderedList*, *RepeatDirection* vlastnost již nelze použít a bude Vyvolejte výjimku za běhu, pokud byla nastavena v rámci značek nebo kódu. Vlastnost by nemají žádnou hodnotu, protože je definována rozložení vizuálu z těchto ovládacích prvků, místo použití šablon stylů CSS.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Vylepšení ovládacího prvku nabídka

Před technologii ASP.NET 4 *nabídky* ovládací prvek vykreslen řadě tabulek HTML. To udělali obtížnější použít styly CSS mimo nastavení vložených vlastností a nebyl také kompatibilní se standardy usnadnění.

V technologii ASP.NET 4 vykreslí ovládací prvek nyní pomocí sémantické značky, který obsahuje Neseřazený seznam a seznam prvků jazyka HTML. Následující příklad ukazuje značky na stránce ASP.NET pro *nabídky* ovládacího prvku.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Při vykreslení stránky, ovládací prvek vytvoří následující kód HTML ( *onclick* byl vynechán kód pro přehlednost):

[!code-html[Main](overview/samples/sample86.html)]

Kromě vylepšení vykreslování navigaci pomocí klávesnice nabídky je vylepšená správa fokus. Když *nabídky* ovládací prvek získá fokus, můžete použít klávesy se šipkami přejít elementy. *Nabídky* ovládací prvek nyní také připojí dostupné bohaté rolí internet aplikace (ARIA) a pro atributy ke splnění[výskytů](http://www.w3.org/TR/wai-aria-practices/#menu "nabídky ARIA pokyny")lepší usnadnění přístupu.

Styly pro ovládací prvek nabídky se zobrazují v styl bloku v horní části stránky, nikoli podle vykreslené elementů HTML. Pokud chcete využít plnou kontrolu nad používání stylů pro ovládací prvek, můžete nastavit nový *IncludeStyleBlock* vlastnost *false*, v takovém případě není aktivováno, blok stylu. Jeden ze způsobů použití této vlastnosti je použití funkce automaticky formátovat v návrháři aplikace Visual Studio k nastavení vzhledu nabídky. Můžete pak spuštění stránky, otevřete zdroj stránky a zkopírujte blok vykreslené stylu na externí soubor šablony stylů CSS. V sadě Visual Studio zrušit jeho styl a sada *IncludeStyleBlock* k *false*. Výsledkem je, že vzhled nabídky je definován pomocí stylů v externí šabloně stylů.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Průvodce a CreateUserWizard ovládacích prvků

Technologie ASP.NET *průvodce* a *CreateUserWizard* ovládacích prvků nepodporují šablony, které umožňují definovat HTML, které vykreslují. (*CreateUserWizard* je odvozena z *průvodce*.) Následující příklad ukazuje kód pro plně šablony *CreateUserWizard* ovládacího prvku:

[!code-aspx[Main](overview/samples/sample87.aspx)]

Vykreslí ovládací prvek HTML podobný následujícímu:

[!code-html[Main](overview/samples/sample88.html)]

V technologii ASP.NET 3.5 SP1, i když změníte obsah šablony, pořád máte omezenou kontrolu nad výstup *průvodce* ovládacího prvku. V technologii ASP.NET 4, můžete vytvořit *LayoutTemplate* šablony a vložit *zástupný symbol* ovládací prvky (pomocí vyhrazené názvy) k určení způsobu *ovládacího prvku průvodce* k vykreslení. Následující příklad ukazuje toto:

[!code-aspx[Main](overview/samples/sample89.aspx)]

Tento příklad obsahuje následující zástupné symboly v pojmenované *LayoutTemplate* element:

- *headerPlaceholder* – v době běhu, to je nahrazena obsah *HeaderTemplate* elementu.
- *sideBarPlaceholder* – v době běhu, to je nahrazena obsah *třída SideBarTemplate* elementu.
- *wizardStepPlaceHolder* – v době běhu, to je nahrazena obsah *WizardStepTemplate* elementu.
- *navigationPlaceholder* – v době běhu, to je nahrazena žádné navigační šablony, které jste definovali.

Značky v příkladu, který používá zástupné symboly vykreslí následující kód HTML (bez obsahu ve skutečnosti definovány v šablonách):

[!code-html[Main](overview/samples/sample90.html)]

Je pouze kód HTML, který není nyní uživatelem definované *span* elementu. (Očekáváme, který v budoucích verzích, dokonce i pomocí *span* element se nevykreslí.) Tuto chybu vám plnou kontrolu nad prakticky veškerý obsah, který je generován *průvodce* ovládacího prvku.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC byla zavedená jako rozšiřovatelnou platformu pro doplněk pro technologie ASP.NET 3.5 SP1 v března 2009. Visual Studio 2010 obsahuje 2 technologie ASP.NET MVC, která zahrnuje nové funkce a možnosti.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Oblasti podpory

Oblasti umožňují skupiny kontrolerů a zobrazení do části rozsáhlé aplikace v relativní izolaci od ostatních oddílů. Každou oblast, kterou je možné implementovat jako samostatné projektu ASP.NET MVC, která může poté odkazovat hlavní aplikace. To pomáhá se správou složitosti, když vytváříte rozsáhlé aplikace a usnadňuje tak více týmy pracovat společně na jedné aplikace.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Podpora ověřování atributů dat – Poznámka

*DataAnnotations* atributy umožňují logiku ověřování k modelu připojit pomocí atributy metadat. *DataAnnotations* atributy byly zavedeny v Dynamická Data technologie ASP.NET v technologii ASP.NET 3.5 SP1. Tyto atributy jsou integrované do výchozí vazač modelu a poskytují způsob metadaty řízenou ověření vstupu uživatele.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Pomocnými objekty

Pomocnými objekty se vám umožní automaticky přidružit úpravy a zobrazení šablony s datovými typy. Pomocné šablony můžete použít například k určení, že je prvek uživatelského rozhraní pro výběr data automaticky generován pro *System.DateTime* hodnotu. To se podobá šablony polí v dynamických dat ASP.NET.

*Html.EditorFor* a *Html.DisplayFor* pomocné metody mají integrovanou podporu pro vykreslení standardních datových typů i složité objekty s více vlastnostmi. Jsou také přizpůsobit vykreslení tím, že umožní použít atributy dat. Poznámka jako *DisplayName* a *ScaffoldColumn* k *ViewModel* objektu.

Často chcete přizpůsobit výstup z pomocné rutiny uživatelského rozhraní ještě dál a získejte úplnou kontrolu nad co je vygenerována. *Html.EditorFor* a *Html.DisplayFor* pomocné metody podporují použití šablon mechanismus, který umožňuje definovat externí šablony, které můžete přepsat a ovládací prvek vykreslen výstup. Šablony lze vykreslit jednotlivě pro třídu.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dynamická Data

Dynamická Data byla zavedena ve verzi rozhraní .NET Framework 3.5 SP1 v polovině roku 2008. Tato funkce poskytuje mnoho vylepšení pro vytváření aplikací řízených daty, včetně následujících:

- RAD prostředí rychle vytvářet webové stránky řízené daty.
- Automatické ověřování, který je založen na omezení definovaná v datovém modelu.
- Schopnost snadno změnit kód generovaný pro pole v *GridView* a *DetailsView* ovládacích prvků pomocí šablon pole, které jsou součástí projektu Dynamická Data.

> [!NOTE]
> Poznámka: Další informace naleznete [dynamických dat dokumentaci](https://msdn.microsoft.com/library/cc488545.aspx) v knihovně MSDN.


Pro technologii ASP.NET 4 dynamických dat je vylepšená poskytnout vývojáři získají vyšší výkon pro rychlé vytváření datově řízených webových serverů.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Povolení dynamických dat u existujících projektů

Dynamické funkce Data, která poskytuje rozhraní .NET Framework 3.5 SP1 přinesl nové funkce, jako je následující:

- Pole šablony – poskytují tyto na základě dat typu šablony pro ovládací prvky vázané na data. Pole šablony poskytují jednodušší způsob, jak přizpůsobit vzhled ovládacích prvcích dat než při použití šablony polí pro každé pole.
- Ověřování – Dynamická Data vám umožní používat atributy u datových tříd k určení ověření pro běžné scénáře, jako jsou povinná pole, kontrolu rozsahu, kontrolu typu, porovnávání vzorů pomocí regulárních výrazů a vlastní ověřování. Ověřování se vynucují ovládacími prvky dat.

Tyto funkce však má následující požadavky:

- Vrstva přístupu k datům měl být založený na rozhraní Entity Framework a LINQ to SQL.
- Pouze data source ovládací prvky pro tyto funkce se nepodporuje *EntityDataSource* nebo *LinqDataSource* ovládacích prvků.
- Funkce vyžaduje webový projekt, který měl nebyl vytvořen pomocí dynamických dat nebo entity dynamických dat šablony mají všechny soubory, které jsou vyžadovány k podpoře funkce.

Hlavním cílem podporu dynamických dat v technologii ASP.NET 4 je povolení nových funkcí dynamických dat pro každou aplikaci ASP.NET. Následující příklad ukazuje značky pro ovládací prvky, které můžete využít výhod funkce Dynamická Data v existující stránky.

[!code-aspx[Main](overview/samples/sample91.aspx)]

V kódu stránky musíte přidat následující kód chcete-li povolit podporu dynamických dat pro tyto ovládací prvky:

[!code-csharp[Main](overview/samples/sample92.cs)]

Když *GridView* ovládací prvek je v režimu úprav, dynamickými daty automaticky ověří, zda zadaná data ve správném formátu. Pokud není, zobrazí se chybová zpráva.

Tato funkce také přináší i další výhody, jako je například schopnost určit výchozí hodnoty pro režimu vkládání. Bez Dynamická Data, implementovat výchozí hodnotu pro pole, musíte přiřadit události, vyhledejte ovládací prvek (pomocí *FindControl*) a nastavení jeho hodnoty. V technologii ASP.NET 4 *EnableDynamicData* volání podporuje druhý parametr, který umožňuje předat výchozí hodnoty pro všechna pole objektu, jak je znázorněno v tomto příkladu:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Syntaxe deklarativní ovládacího prvku DynamicDataManager

*DynamicDataManager* ovládací prvek je vylepšená tak, aby ji můžete nakonfigurovat deklarativně, stejně jako u většiny ovládacích prvků v ASP.NET, namísto pouze v kódu. Zápis *DynamicDataManager* ovládací prvek vypadat jako v následujícím příkladu:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Tato značka umožňuje chování dynamických dat pro ovládací prvek GridView1, na který odkazuje *DataControls* část *DynamicDataManager* ovládacího prvku.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Šablony entit

Šablony entit nabízí nový způsob přizpůsobení rozložení dat, aniž by bylo potřeba vytvořit vlastní stránky. Stránce použití šablon *FormView* ovládacího prvku (místo *DetailsView* řídit, jak použít v šablonách stránky v dřívějších verzích dynamických dat) a *DynamicEntity* ovládací prvek vykreslovat šablony entit. To dává větší kontrolu nad značkami, které je vykresleno dynamickými daty.

Následující seznam uvádí nové rozložení adresáře projektu, který obsahuje šablony entity:

[!code-console[Main](overview/samples/sample95.cmd)]

`EntityTemplate` Adresář obsahuje šablony pro objekty modelu dat zobrazení. Ve výchozím nastavení, objekty jsou vykreslovány pomocí `Default.ascx` šablonu, která obsahuje kód, který vypadá stejně jako kód vytvořil *DetailsView* ovládací prvek používat Dynamická Data technologie ASP.NET 3.5 SP1. Následující příklad ukazuje kód pro `Default.ascx` ovládacího prvku:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Chcete-li změnit vzhled a chování pro celou lokalitu se dá upravit výchozí šablony. Šablony pro zobrazení a úpravě operací vložení. Nové šablony mohou být přidány na základě názvu objektu dat. Chcete-li změnit vzhled a chování pouze jeden typ objektu. Můžete například přidat následující šablony:

[!code-console[Main](overview/samples/sample97.cmd)]

Šablona může obsahovat následující kód:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Nové entity šablony se zobrazí na stránce s použitím nového *DynamicEntity* ovládacího prvku. V době běhu tento ovládací prvek nahrazena obsahu entity šablony. Následující kód ukazuje *FormView* v ovládacím prvku `Detail.aspx` stránku šablony, který používá šablonu entity. Všimněte si, že *DynamicEntity* elementu v kódu.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Nové šablony polí pro adresy URL a e-mailové adresy

ASP.NET 4 zavádí dvě nové šablony předdefinované pole `EmailAddress.ascx` a `Url.ascx`. Tyto šablony jsou používány pro pole, které jsou označeny jako *EmailAddress* nebo *Url* s *datový typ* atribut. Pro *EmailAddress* objekty, pole se zobrazí jako hypertextový odkaz, který je vytvořen pomocí *mailto:* protokolu. Když uživatelé kliknou na odkaz, otevře uživatele e-mailového klienta a vytvoří kostru zprávu. Objekty typu *Url* se zobrazí jako hypertextové odkazy, běžný.

Následující příklad ukazuje, jak by pole označená.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Vytváření odkazů pomocí ovládacího prvku DynamicHyperLink

Dynamická Data používají novou funkci směrování, která byla přidána v rozhraní .NET Framework 3.5 SP1 pro řízení adresy URL, které koncovým uživatelům zobrazí při přístupu k webové stránce. Nové *DynamicHyperLink* ovládací prvek umožňuje snadno vytvářet odkazy na stránky webu s dynamickými daty. Následující příklad ukazuje způsob použití *DynamicHyperLink* ovládacího prvku:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Tento kód vytvoří odkaz, který odkazuje na stránce seznamu `Products` tabulky podle trasy, které jsou definovány v `Global.asax` souboru. Ovládací prvek automaticky používá výchozí název tabulky založenou na stránce Dynamická Data.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Podpora dědičnosti v datovém modelu

Jak Entity Framework a LINQ to SQL v jejich datové modely podporují dědičnost. Příkladem může být databázi, která má `InsurancePolicy` tabulky. Může také obsahovat `CarPolicy` a `HousePolicy` tabulky, které mají stejná pole jako `InsurancePolicy` a pak přidejte další pole. Dynamická Data byla změněna pochopit zděděných objektů v datovém modelu a podporu generování uživatelského rozhraní pro zděděné tabulky.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Podpora pro relace m: N (pouze Entity Framework)

Má bohatou podporu pro many-to-many relace mezi tabulkami, které je implementované vystavení vztahu jako kolekce v Entity Framework *Entity* objektu. Nové `ManyToMany.ascx` a `ManyToMany_Edit.ascx` šablony pole byly přidány k poskytování podpory pro zobrazování a upravování dat, který je součástí relace many-to-many.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Nové atributy pro ovládací prvek zobrazení a podporu výčtů

*DisplayAttribute* byl přidán do vám poskytnou další řízení zobrazení polí. *DisplayName* atribut v dřívějších verzích dynamických dat je možné změnit název, který se používá jako popisek pro pole povolená. Nové *DisplayAttribute* třída umožňuje určit další možnosti pro zobrazení pole, jako je například pořadí, ve které se zobrazí pole a určuje, zda pole se použije jako filtr. Atribut také poskytuje nezávislé kontrolu nad název použitý pro popisky *GridView* řídit, název použitý v *prvku DetailsView* řídit, text nápovědy pro pole, a použít vodoznak pro pole (Pokud je toto pole přijímá textový vstup).

*EnumDataTypeAttribute* třídy je přidaný do umožňují mapování polí na výčty. Při použití tohoto atributu na pole, zadejte typ výčtu. Dynamická Data používají nové `Enumeration.ascx` pole šablony k vytvoření uživatelského rozhraní pro zobrazení a úpravy hodnot výčtu. Šablona mapuje hodnoty z databáze, které mají názvy ve výčtu.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Vylepšená podpora pro filtry

Dynamická Data 1.0 součástí integrované filtry pro logickou sloupce a sloupce cizího klíče. Filtry nepovolil určete, zda se zobrazí, nebo v jakém byly zobrazeny. Nové *DisplayAttribute* atribut adresy obou těchto problémů tím, že můžete řídit, jestli se sloupce zobrazí jako filtr a v jakém pořadí se bude zobrazovat.

Další vylepšení je, že podpora filtrování byl znovu[zapsána do pomocí nové](#0.2__QueryExtender "_QueryExtender") funkce webových formulářů. To vám umožní vytvořit filtry bez nutnosti znalost ovládací prvek zdroje dat, který se použije filtry. Spolu s těmito příponami filtry také dostalo do šablon ovládacích prvků, které vám umožní přidávat nové značky. Nakonec *DisplayAttribute* třídy již bylo zmíněno dříve umožňuje výchozí filtr k přepsání ve stejném způsobu, jakým *UIHint* umožňuje pole výchozí šablonu pro sloupec k přepsání.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Vylepšení webového vývoje Visual Studio 2010

Vývoj pro web v sadě Visual Studio 2010 bylo vylepšeno pro lepší Kompatibilita šablon stylů CSS, vyšší produktivitu prostřednictvím fragmentů kódu HTML a ASP.NET a nové dynamické technologie IntelliSense jazyka JavaScript.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Kompatibilita vylepšené šablony stylů CSS

Návrhář Visual Web Developer v sadě Visual Studio 2010 je aktualizovaná na striktnější dodržování pravidel šablon stylů CSS 2.1 standardy. Návrhář lépe zachová integrita zdrojový kód HTML a je výkonnější než v předchozích verzích sady Visual Studio. Pod pokličkou architektury také byla vylepšena se bude lépe povolit budoucí vylepšení v vykreslování, rozložení a použitelnost.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>Fragmenty HTML a JavaScript

V editoru HTML IntelliSense automaticky dokončuje názvy značek. Funkce fragmenty kódu technologie IntelliSense automaticky dokončuje celý značek a dalších. V sadě Visual Studio 2010 jsou podporovány fragmenty kódu technologie IntelliSense pro JavaScript, C# a Visual Basic, které byly podporovány v dřívějších verzích sady Visual Studio.

Visual Studio 2010 obsahuje více než 200 fragmenty kódu, které vám pomohou automatické dokončování společné značky technologie ASP.NET a HTML, včetně požadovaných atributů (jako je například runat = "server") a společné atributy, které jsou specifické pro značky (například *ID*,  *DataSourceID*, *ControlToValidate*, a *Text*).

Další fragmenty kódu si můžete stáhnout, nebo můžete napsat vlastní fragmenty kódu, které provádí zapouzdření bloky kódu, které vy nebo váš tým používat pro běžné úlohy.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Vylepšení technologie IntelliSense jazyka JavaScript

V sadě Visual Studio 2010 jsme přepracovali technologie IntelliSense jazyka JavaScript poskytovat ještě bohatší možnosti úprav. Technologie IntelliSense nyní rozpoznává objekty, které byly vytvořeny dynamicky metodami, jako například *registerNamespace* a podobné techniky, které používají jiné architektury JavaScriptu. Bylo vylepšeno výkonu k analýze velkých knihovny skriptů a k zobrazení technologie IntelliSense s žádné nebo téměř žádné zpoždění zpracování. Kompatibilita se výrazně zvýšil na podporu téměř všechny knihovny třetích stran a pro podporu různých stylů psaní kódu. Dokumentační komentáře jsou analyzovány teď hned při psaní a okamžitě se využívají technologii IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Nasazení webové aplikace sadou Visual Studio 2010

Když vývojáře využívající technologii ASP.NET nasazení webové aplikace, často zjistí, že se narazí problémy, jako je následující:

- Nasazení do sdíleného hostingu lokality vyžaduje technologie, jako je FTP, který může být pomalé. Kromě toho je nutné ručně provést úlohy, jako je spouštění skriptů SQL, pokud chcete nakonfigurovat databázi a je nutné změnit nastavení služby IIS, jako je například konfigurace virtuálního adresáře složky jako aplikace.
- V podnikovém prostředí, kromě nasazení souborů webové aplikace správců často třeba upravit konfiguračních souborů ASP.NET a nastavení služby IIS. Správce databáze musí spustit sérii skripty SQL zobrazíte databáze aplikace spuštěná. Tato zařízení jsou náročné na práci, obvykle trvat hodiny a musí být zdokumentována pečlivě.

Visual Studio 2010 obsahuje technologie, které řeší tyto problémy a které umožňují bez problémů nasazování webových aplikací. Jednu z těchto technologií je nástroj pro nasazení webu služby IIS (MsDeploy.exe).

Webové nasazení funkce v sadě Visual Studio 2010 následující hlavní oblasti:

- Vytváření webových balíčků
- Transformace souboru Web.config
- Nasazení databáze
- Publikování jedním kliknutím pro webové aplikace

Následující části obsahují podrobnosti o těchto funkcích.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Vytváření webových balíčků

K vytvoření komprimovaný soubor (ZIP) pro vaši aplikaci, která se označuje jako Visual Studio 2010 používá nástroj MSDeploy *webového balíčku*. Soubor balíčku obsahuje metadata o vaší aplikace spolu s následujícím obsahem:

- Nastavení služby IIS, která zahrnuje nastavení fondu aplikací, nastavení chyb stránky a tak dále.
- Skutečné webový obsah, včetně webových stránek, uživatelské ovládací prvky, statický obsah (obrázky a soubory HTML) a tak dále.
- Schémata databáze systému SQL Server a data.
- Certifikáty zabezpečení, součástí k instalaci v mezipaměti GAC, nastavení registru a tak dále.

Webový balíček můžete zkopírovat u libovolného serveru a pomocí Správce služby IIS nainstalovat ručně. Další možností pro automatické nasazení, balíček nainstalujete pomocí příkazů příkazového řádku nebo pomocí rozhraní API nasazení.

Visual Studio 2010 poskytuje integrované úlohy MSBuild a cíle k vytvoření webových balíčků. Další informace najdete v tématu [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) na webu MSDN a [10 + 20 důvody, proč by měl vytvořit webový balíček](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) na blogu Vishal Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Transformace souboru Web.config

Pro nasazení webové aplikace Visual Studio 2010 zavádí [transformaci dokumentů XML (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), což je funkce, který umožňuje transformovat `Web.config` soubor z nastavení pro vývoj na výrobní nastavení. Nastavení transformace jsou určené v transformační soubory s názvem `web.debug.config`, `web.release.config`, a tak dále. (Názvy tyto soubory odpovídají MSBuild konfigurace). Transformační soubor obsahuje pouze změny, které je třeba provést pro nasazený `Web.config` souboru. Určete změny pomocí jednoduché syntaxe.

Následující příklad ukazuje část `web.release.config` souboru, který může být vytvořen pro nasazení vaší verze konfigurace. Nahradit – klíčové slovo v příkladu, který určuje během nasazení *připojovací řetězec* uzlu `Web.config` soubor se nahradí hodnotami, které jsou uvedené v příkladu.

[!code-xml[Main](overview/samples/sample102.xml)]

Další informace najdete v tématu [syntaxe transformace souboru Web.config pro nasazení projektu webové aplikace](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) na MSDN <a id="0.2_a"> </a> webu a[nasazení webu: transformace Web.Config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)na blogu Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Nasazení databáze

Balíček pro nasazení sady Visual Studio 2010 může obsahovat závislosti na databáze SQL serveru. Jako součást definice balíčku zadejte připojovací řetězec pro zdrojovou databázi. Při vytváření webového balíčku Visual Studio 2010 vytvoří skripty SQL pro schéma databáze a volitelně pro data a pak přidá do balíčku. Můžete také zadat vlastní skripty SQL a určete pořadí, ve kterém by měl spustit na serveru. V době nasazení můžete zadat připojovací řetězec, který je vhodný pro cílový server; proces nasazení ke spouštění skriptů, které vytvoří schéma databáze a přidejte data použije tento připojovací řetězec.

Kromě toho pomocí jedním kliknutím publikovat, můžete nakonfigurovat nasazení publikování databáze přímo, když je aplikace publikována na vzdálený web sdíleného hostingu. Další informace najdete v tématu [postupy: nasazení databáze se projekt webové aplikace](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) na webu MSDN a [nasazení databáze s VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) na blogu Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Publikování jedním kliknutím pro webové aplikace

Visual Studio 2010 také vám umožní používat vzdálenou správu služby IIS pro publikování webových aplikací na vzdálený server. Vytvoříte profil publikování pro hostitelský účet nebo servery testů nebo přípravných servery. Každý profil můžete bezpečně uložit příslušná pověření. Pak můžete nasadit do některé z cílových serverů s jedním kliknutím na webu jedním kliknutím pomocí nástrojů pro publikování. S Visual Studio 2010 můžete také publikovat pomocí příkazového řádku MSBuild. To vám umožní nakonfigurovat prostředí sestavení týmu, aby zahrnovalo publikování v modelu průběžné integrace.

Další informace najdete v tématu [postupy: nasazení webové aplikace projektu pomocí publikování jedním kliknutím a nasazení webu](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) na webu MSDN a [Web publikovat kliknutím 1 s VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) na blogu Vishal Joshi. Chcete-li zobrazit videa prezentace o nasazení webové aplikace v sadě Visual Studio 2010, naleznete v tématu [VS 2010 pro webové vývojáře náhledy](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) na blogu Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Prostředky

Na následujících webech poskytují další informace o technologii ASP.NET 4 a Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) – oficiální dokumentaci pro technologii ASP.NET 4 na webové stránce MSDN.
- [https://www.asp.net/](https://www.asp.net/) – ASP.NET týmu vlastní webové stránky.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) a [obsahu mapy Dynamická Data technologie ASP.NET](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) – Online prostředků na webu technologie ASP.NET a v oficiální dokumentaci pro dynamická Data technologie ASP.NET.
- [https://www.asp.net/ajax/](../../ajax/index.md) – Hlavní webový zdroj pro vývoj pro ASP.NET Ajax.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) – V týmu Visual Web Developer blogu, který obsahuje informace o funkcích v sadě Visual Studio 2010.
- [Skvělá webová sada ASP.NET](https://github.com/aspnet/AspNetWebStack) – hlavní webový prostředek pro verzi preview verze technologie ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Právní omezení

Toto je předběžná dokumentu a může podstatně změnit před finální komerční verzi softwaru, které jsou popsány zde.

Informace obsažené v tomto dokumentu představují aktuální pohled společnosti Microsoft Corporation na tyto problémy k datu publikování. Protože Microsoft reagovat na měnící se podmínky na trhu, neměly by být vykládány jako závazek Microsoftu a společnost Microsoft nemůže zaručit přesnost jakýchkoli informací, které jsou prezentovány po provedení datu publikování.

Tento dokument White Paper se pouze k informačním účelům. MICROSOFT NEPOSKYTUJE ŽÁDNÉ ZÁRUKY, VÝSLOVNÝCH, ODVOZENÝCH NEBO ZÁKONNÝCH, INFORMACE V TOMTO DOKUMENTU.

V souladu s příslušnými zákony o autorských právech je zodpovědný uživatel. Bez omezení autorská práva, žádná část tohoto dokumentu může být reprodukovat, uložené v zavedené rozšiřován nebo jakýmkoli způsobem (electronic, mechanickým, mechanicky, záznam nebo jinak) nebo pro libovolný účel, bez výslovného písemného povolení společnosti Microsoft Corporation.

Společnost Microsoft může vlastnit patenty, patentové přihlášky, ochranné známky, autorská práva nebo další práva na duševní vlastnictví přihlášky v tomto dokumentu. Výslovně uvedeno v písemné licenční smlouvě se společností Microsoft, poskytnutím tohoto dokumentu vám není udělena licence k těmto patentům, ochranné známky, autorská práva nebo jinému duševnímu vlastnictví.

Pokud není uvedeno jinak, společnosti, organizace, produkty, názvy domén, e-mailové adresy, loga, osoby, místa a události použité v ukázkách jsou smyšlené. proto žádné jejich spojení se žádné skutečnou společností, organizací, produktu, název domény, e-mailu adresy, loga, osoby, místa nebo událostí je určena ji vyvozovat.

© 2009 Microsoft Corporation. Všechna práva vyhrazena.

Microsoft a Windows jsou registrované ochranné známky nebo ochranné známky společnosti Microsoft Corporation ve Spojených státech amerických a dalších zemích.

Názvy skutečných společností a produktů mohou být ochrannými známkami příslušných vlastníků.
