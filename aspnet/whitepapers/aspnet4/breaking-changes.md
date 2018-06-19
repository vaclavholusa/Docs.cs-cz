---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 nejnovější změny | Microsoft Docs
author: rick-anderson
description: Tento dokument popisuje změny, které byly provedeny pro verzi rozhraní .NET Framework verze 4, který může potenciálně ovlivnit aplikace, které byly vytvořené pomocí...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 7eea51add6b05684357314e3d6aa5087383c6408
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899113"
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 nejnovější změny
====================
> Tento dokument popisuje změny, které byly provedeny pro verzi rozhraní .NET Framework verze 4, který může potenciálně ovlivnit aplikace, které byly vytvořené pomocí dřívější verze, včetně ASP.NET 4 Beta 1 a Beta 2 verze.
> 
> [Stáhněte si tento dokument White Paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Obsah

[ControlRenderingCompatibilityVersion nastavení v souboru Web.config](#0.1__Toc256770141 "_Toc256770141")  
[Změny ClientIDMode](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode a UrlEncode nyní kódování apostrofech](#0.1__Toc256770143 "_Toc256770143")  
[Stránka ASP.NET (.aspx) Analyzátor je Stricter](#0.1__Toc256770144 "_Toc256770144")  
[Browser Definition Files Updated](#0.1__Toc256770145 "_Toc256770145")  
[System.Web.Mobile.dll Removed from Root Web Configuration File](#0.1__Toc256770146 "_Toc256770146")  
[ASP.NET Request Validation](#0.1__Toc256770147 "_Toc256770147")  
[Výchozí algoritmus hash je nyní HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Konfigurace chyb souvisejících s novou konfiguraci ASP.NET 4 kořenové](#0.1__Toc256770149 "_Toc256770149")  
[Aplikace ASP.NET 4 podřízené nezdaří spuštění pod ASP.NET 2.0 nebo v technologii ASP.NET 3.5 aplikace](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 weby se nepodaří spustit v počítačích, kde je nainstalován SharePoint](#0.1__Toc256770151 "_Toc256770151")  
[Vlastnost HttpRequest.FilePath už obsahuje hodnoty PathInfo](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 aplikace může způsobit chyby HttpException, odkazující na soubor eurl.axd](#0.1__Toc256770153 "_Toc256770153")  
[Obslužné rutiny událostí může není není vyvolána v výchozí dokument v IIS 7 nebo IIS 7.5 integrovaný režim](#0.1__Toc256770154 "_Toc256770154")  
[Změny v kódu ASP.NET přístup zabezpečení (CA) implementaci](#0.1__Toc256770155 "_Toc256770155")  
[Byly přesunuty MembershipUser a dalších typů v Namespace System.Web.Security](#0.1__Toc256770156 "_Toc256770156")  
[Výstup ukládání do mezipaměti změny k odlišení \* hlavičky protokolu HTTP](#0.1__Toc256770157 "_Toc256770157")  
[System.Web.Security Types for Passport are Obsolete](#0.1__Toc256770158 "_Toc256770158")  
[Vlastnost MenuItem.PopOutImageUrl nepodaří Vykreslit obrázek v technologii ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl a Menu.DynamicPopOutImageUrl selhání vykreslování obrázků, pokud cesty obsahují zpětná lomítka](#0.1__Toc256770160 "_Toc256770160")  
[Disclaimer](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>ControlRenderingCompatibilityVersion nastavení v souboru Web.config

Ovládací prvky ASP.NET bylo upraveno v rozhraní .NET Framework verze 4 k umožňují určit, jak přesněji vykreslovaly značky. V předchozích verzích rozhraní .NET Framework některé ovládací prvky vytvářely značky, které jste měli žádný způsob, jak zakázat. Ve výchozím nastavení technologii ASP.NET 4 Tento typ značky nebude vygenerována.

Pokud používáte Visual Studio 2010 k upgradu vaší aplikace z technologie ASP.NET 2.0 nebo ASP.NET 3.5, nástroj automaticky přidá nastavení, pro které `Web.config` soubor, který zachovává starší vykreslování. Ale pokud provádíte upgrade aplikace můžete změnit fond aplikací ve službě IIS pro rozhraní .NET Framework 4, ASP.NET používá nový režim vykreslování ve výchozím nastavení. Pokud chcete zakázat nový režim vykreslování, přidejte následující nastavení v `Web.config` souboru:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Hlavní vykreslování změny, které přináší nové chování jsou následující:

- **Image** a **ImageButton** už vykreslení ovládacích prvků `border="0"` atribut.
- **BaseValidator** třídy a ověření ovládací prvky, které jsou odvozeny od ho už nebudou zobrazovat červený text ve výchozím nastavení.
- **HtmlForm** prvek nezobrazuje **název** atribut.
- **Tabulky** řízení už vykreslí `border="0"` atribut.
- Ovládací prvky, které nejsou určeny pro vstup uživatele (například **popisek** ovládací prvek) už nebudou zobrazovat `disabled="disabled"` atribut, pokud jejich **povoleno** je nastavena na **false**(nebo pokud toto nastavení dědí z ovládacího prvku kontejner).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode změny

**ClientIDMode** nastavení v rozhraní ASP.NET 4 umožňuje určit, jak technologie ASP.NET vygeneruje **id** atribut pro elementů HTML. V předchozích verzích technologie ASP.NET byla ekvivalentní výchozí chování **AutoID** nastavení **ClientIDMode**. Výchozí nastavení je však nyní **Predictable**.

Pokud používáte Visual Studio 2010 k upgradu vaší aplikace z technologie ASP.NET 2.0 nebo ASP.NET 3.5, nástroj automaticky přidá nastavení, pro které `Web.config` soubor, který zachovává chování starší verze rozhraní .NET Framework. Ale pokud provádíte upgrade aplikace můžete změnit fond aplikací ve službě IIS pro rozhraní .NET Framework 4, ASP.NET používá nový režim ve výchozím nastavení. Pokud chcete zakázat nový režim ID klienta, přidejte následující nastavení v `Web.config` souboru:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode a UrlEncode nyní kódování jednoduchých uvozovek:

V technologii ASP.NET 4 **HtmlEncode** a **UrlEncode** metody **HttpUtility** a **HttpServerUtility** byly aktualizovány třídy kódování znak jednoduché uvozovky (') následujícím způsobem:

- **HtmlEncode** metoda kóduje instance jednoduché uvozovky jako.
- **UrlEncode** metoda kóduje instance jednoduché uvozovky jako % 27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>Stránka ASP.NET (.aspx) Analyzátor je Stricter

Analyzátor stránky pro stránky ASP.NET (`.aspx` soubory) a uživatelské ovládací prvky (`.ascx` souborů) je v technologii ASP.NET 4 přísnější a odmítnou více instancí neplatné značky. Například následující dva fragmenty kódu by úspěšně analyzovat ve starších verzích technologie ASP.NET, ale bude nyní vyvolat chyby analyzátoru v technologii ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Všimněte si neplatný středník na konci **HiddenField** značky.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Všimněte si uzavřené **styl** atribut, který spouští do **CssClass** atribut.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Soubory definice prohlížeče aktualizovat

Informace o nových a aktualizovaných prohlížečích a zařízeních byly aktualizovány s definicemi prohlížeče. Starší prohlížečích a zařízeních, jako je například Netscape Navigátor byly odstraněny a novější prohlížeče a přidané zařízení, jako například Google Chrome a Apple iPhone.

Pokud vaše aplikace obsahuje definice vlastní prohlížečů, které dědí z jednoho z definice prohlížeče, které byly odebrány, zobrazí se chyba. Například pokud `App_Browsers` složka obsahuje definici prohlížeče, který dědí z definice IE2 prohlížeče, zobrazí se následující chybová zpráva konfigurace:

- Element prohlížeče nebo brány s ID 'IE2' nebyl nalezen.

> [!NOTE]
> **HttpBrowserCapabilities** objektu (který je zveřejněný prostřednictvím stránky **Request.Browser** vlastnost) vycházejí z definice soubory prohlížeče. Informace vrácené přístupu k vlastnosti tohoto objektu v technologii ASP.NET 4 proto může být jiná než informace vrácené ve starší verzi technologie ASP.NET.


Můžete obnovit původní definičních souborech prohlížeče pomocí kopírování souborů definice prohlížeče z následující složky:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Zkopírujte soubory do odpovídajícího `\CONFIG\Browsers` složku pro technologii ASP.NET 4. Po zkopírování souborů, spusťte Aspnet\_regbrowsers.exe nástroj příkazového řádku.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System.Web.Mobile.dll odebrána z kořenové soubor webové konfigurace

V předchozích verzích technologie ASP.NET, odkaz na sestavení System.Web.Mobile.dll byl zahrnut v kořenovém `Web.config` v soubor **sestavení** tématu v části. Chcete-li zvýšit výkon, byl odebrán odkaz na toto sestavení.

Sestavení System.Web.Mobile.dll je zahrnuta v technologii ASP.NET 4, ale je zastaralý. Pokud chcete používat typy ze sestavení System.Web.Mobile.dll, přidejte odkaz na toto sestavení buď kořenové `Web.config` souborů nebo k aplikaci `Web.config` souboru. Například pokud chcete použít některou z (nepoužívané) mobilní ovládací prvky ASP.NET, musíte přidat odkaz na sestavení System.Web.Mobile.dll k `Web.config` souboru.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Ověření žádosti technologie ASP.NET

Funkce ověření žádosti technologie ASP.NET poskytuje určitou úroveň výchozí ochranu proti útokům na skriptování (XSS). V předchozích verzích technologie ASP.NET se ve výchozím nastavení povoleno ověření žádosti. Však použít pouze pro stránky ASP.NET (`.aspx` soubory a jejich třída soubory) a pouze když byly provádění tyto stránek.

V technologii ASP.NET 4 ve výchozím nastavení, žádost je povoleno ověření pro všechny požadavky, protože je povolená před **BeginRequest** fáze požadavku HTTP. Žádost o ověření v důsledku toho se vztahuje na požadavky na všechny prostředky, ne jenom žádosti o stránky .aspx. To zahrnuje požadavků například volání webové služby a vlastní obslužné rutiny HTTP. Ověření žádosti je aktivní taky při vytváření vlastních modulů HTTP jsou čtení obsahu požadavku HTTP.

V důsledku toho žádosti o ověření může nyní dojít k chybám pro požadavky, které dříve není aktivovat chyby. Chcete-li vrátit k chování funkcí technologie ASP.NET 2.0 žádosti o ověření, přidejte následující nastavení v `Web.config` souboru:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Doporučujeme však analýzy všechny chyby ověření požadavku k určení, zda stávající obslužné rutiny, moduly nebo další vlastní kód přistupuje k potenciálně nebezpečného vstupy HTTP, které by mohly být XSS útokům vektory.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Výchozí algoritmus hash je nyní HMACSHA256

Technologie ASP.NET používá šifrování a algoritmy hash k zabezpečení dat, jako jsou soubory cookie ověřování formulářů a stavu zobrazení. Ve výchozím nastavení technologii ASP.NET 4 teď používá algoritmus HMACSHA256 pro hodnoty hash operací se soubory cookie a stav zobrazení. Starší verze technologie ASP.NET používá starší algoritmus HMACSHA1.

Vaše aplikace může mít vliv, pokud spustíte smíšený 2.0/ASP.NET ASP.NET 4 prostředích, kde data, jako jsou soubory cookie ověřování formulářů musí fungovat across.NET Framework verze. Pro konfiguraci aplikace ASP.NET 4 Web starší algoritmus HMACSHA1, přidejte následující nastavení v `Web.config` souboru:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Chyby konfigurace související s novou technologií ASP.NET 4 Konfigurace kořenového adresáře

Konfigurační soubory kořenové ( `machine.config` soubor a kořenové `Web.config` souboru) pro rozhraní .NET Framework 4 (a proto ASP.NET 4) byly aktualizovány zahrnout většinu standardní konfiguračních informací v technologii ASP.NET 3.5 byl nalezen v aplikace `Web.config` soubory. Z důvodu složitosti spravovaných systémech konfigurace služby IIS 7 a IIS 7.5 spouštění aplikací využívajících technologii ASP.NET 3.5 v rozhraní ASP.NET 4 a v rámci služby IIS 7 a IIS 7.5 může mít za následek technologie ASP.NET nebo služby IIS chyby konfigurace.

Doporučujeme aktualizovat aplikace technologie ASP.NET 3.5 na technologii ASP.NET 4 pomocí nástroje pro upgrade projektu v sadě Visual Studio 2010, pokud je to možné. Visual Studio 2010 automaticky upraví aplikace technologie ASP.NET 3.5 `Web.config` soubor, aby obsahoval odpovídající nastavení pro technologii ASP.NET 4.

Je však podporovaném scénáři ke spouštění aplikací využívajících technologii ASP.NET 3.5 pomocí rozhraní .NET Framework 4 bez opětovnou kompilaci. V takovém případě budete muset ručně upravit aplikace `Web.config` soubor předtím, než spustíte aplikaci v rozhraní .NET Framework 4 a IIS 7 nebo IIS 7.5.

V následujících dvou částech popisují změny, které budete možná muset udělat pro různé kombinace softwaru.

**Windows Vista SP1 nebo Windows Server 2008 SP1, kde jsou nainstalovány ani opravy hotfix KB958854 ani SP2.** V této konfiguraci systému konfigurace služby IIS 7 nesprávně sloučí konfigurace spravované aplikace tak, že porovnáte úrovni aplikace `Web.config` souboru na technologii ASP.NET 2.0 `machine.config` soubory. Z důvodu se aplikace úrovni `Web.config` musí mít soubory z rozhraní .NET Framework 3.5 nebo novější **system.web.extensions** definice oddíl konfigurace (element) s cílem způsobit chybu ověření služby IIS 7.

Ale ručně změnit úroveň aplikace `Web.config` položky souboru, které neodpovídají přesněji původní definice standardní konfigurace části, které byly zavedeny v sadě Visual Studio 2008 způsobí chyby konfigurace technologie ASP.NET. (Výchozí položky konfigurace, které jsou generovány nástrojem Visual Studio 2008 fungovat správně.) Častých problémů je, že ručně upravit `Web.config` vynechte soubory **allowDefinition** a **requirePermission** konfigurační atributy, které se nacházejí v různých konfiguračním oddílu definice. To způsobí neshodu mezi zkrácené konfigurační oddíl na úrovni aplikace `Web.config` soubory a dokončení definice v technologii ASP.NET 4 `machine.config` souboru. V důsledku toho za běhu, konfigurační systém ASP.NET 4 vyvolá chybu konfigurace.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2 a také systém Windows Vista SP1 a Windows Server 2008 SP1, kde je nainstalována oprava hotfix KB958854.**

V tomto scénáři nativní konfiguračního systému služby IIS 7 a IIS 7.5 vrátí chyby v konfiguraci, protože vykonává porovnání text na **typ** atribut, který je definovaný pro obslužnou rutinu spravovaného konfigurace oddílu. Protože všechny `Web.config` mít "3.5" v řetězci typ pro soubory, které jsou generovány nástrojem Visual Studio 2008 a Visual Studio 2008 SP1 **system.web.extensions** (a související) rutiny oddílu konfigurace a protože technologie ASP.NET 4 `machine.config` soubor má "4.0" **typ** atribut pro stejnou konfiguraci oddílu obslužných rutin, aplikace, které jsou generovány v sadě Visual Studio 2008 nebo Visual Studio 2008 SP1, vždy selže ověření konfigurace ve službě IIS 7 a SLUŽBU IIS 7.5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Řešení těchto problémů

Alternativní řešení pro první scénář je aktualizovat na úroveň aplikace `Web.config` zahrnutím standardní konfigurace text ze souboru `Web.config` soubor, který byl automaticky vytvořen systémem Visual Studio 2008.

Alternativní řešení pro první scénář je pro instalaci aktualizací Service Pack 2 pro Vista nebo Windows Server 2008 ve vašem počítači nebo nainstalujte opravu hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) opravit nesprávné konfigurace sloučení chování Systému konfigurace služby IIS. Však po provedení některé z těchto akcí, aplikace bude pravděpodobně dojde k chybě konfigurace z důvodu problému pro druhý scénář popisuje.

Alternativní řešení pro druhý scénář je odstranit nebo komentář všechny **system.web.extensions** definice oddílu konfigurace a konfigurační oddíl skupinu definice z úrovni aplikace `Web.config` souboru. V horní části úrovni aplikace jsou obvykle tyto definice `Web.config` souborů a lze identifikovat podle **configSections** elementu a jeho podřízených položek.

Pro oba scénáře, se doporučuje, aby také ručně odstranit **system.codedom** část, i když to není potřeba.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>Aplikace ASP.NET 4 podřízené nezdaří spuštění pod ASP.NET 2.0 nebo v technologii ASP.NET 3.5 aplikace

ASP.NET 4 aplikace, které jsou nakonfigurovány jako podřízené objekty aplikace, které běží starší verze technologie ASP.NET se pravděpodobně nezdaří z důvodu chyby konfigurace nebo kompilace. Následující příklad ukazuje do struktury adresářů pro ovlivněné aplikaci.

`/parentwebapp` (nakonfigurované na používání technologie ASP.NET 2.0 nebo ASP.NET 3.5)  
`/childwebapp` (nakonfigurován pro použití technologie ASP.NET 4)

Aplikace `childwebapp` složky nebude možné spustit na službě IIS 7 nebo IIS 7.5 a sestava bude k chybě konfigurace. Text chyby bude zahrnovat zpráva podobná následující:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

Službu IIS 6, aplikace `childwebapp` složky také nebude možné spustit, ale bude hlášené jiné chybě. Text chyby může stavu například následující:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Tyto scénáře dojít, protože informace o konfiguraci z nadřazené aplikace `parentwebapp` složky je součástí hierarchie informace o konfiguraci, která určuje konečné sloučené konfiguraci nastavení, které jsou používány podřízené webové v aplikaci `childwebapp` složky. V závislosti na tom, jestli ASP.NET 4 webová aplikace běží na IIS 7 (nebo IIS 7.5) nebo na služby IIS 6 systému konfigurace služby IIS nebo systému kompilace ASP.NET 4 vrátí chybu.

Kroky, které je potřeba provést k vyřešení tohoto problému a získat podřízená aplikace ASP.NET 4 závisí na tom, jestli aplikace ASP.NET 4 běží služby IIS 6 nebo na IIS 7 (nebo IIS 7.5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Krok 1 (IIS 7 nebo IIS 7.5 pouze)

Tento krok je nutný pouze v operačních systémech, které spustit službu IIS 7 nebo IIS 7.5, který zahrnuje Windows Vista, Windows Server 2008, Windows 7 a Windows Server 2008 R2.

Přesunout **configSections** v definici `Web.config` soubor s nadřazenou aplikací (aplikace, který používá technologii ASP.NET 2.0 nebo ASP.NET 3.5) do kořenové `Web.config` souboru pro rozhraní.NET Framework 2.0. Nativní konfiguračního systému služby IIS 7 a IIS 7.5 kontrol **configSections** prvku, když se sloučí hierarchii konfigurační soubory. Přesun **configSections** definice z nadřazené webové aplikace `Web.config` souboru do kořenového adresáře `Web.config` souboru efektivně skryje element ze proces sloučení konfigurace, ke kterému dochází pro podřízené ASP.NET 4 aplikace.

Na 32bitové verzi operačního systému nebo pro fondy aplikací 32-bit, kořenové `Web.config` soubor pro technologii ASP.NET 2.0 a technologii ASP.NET 3.5 je normálně umístěný v následující složce:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

Na 64bitový operační systém nebo pro fondy aplikací 64-bit, kořenové `Web.config` soubor pro technologii ASP.NET 2.0 a technologii ASP.NET 3.5 je normálně umístěný v následující složce:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Pokud spustíte 32bitové a 64bitové verze webových aplikací na 64bitovém počítači, musíte přesunout **configSections** element až do kořenové `Web.config` soubory 32bitové i 64bitové systémy.

Když vložíte **configSections** element v kořenu `Web.config` souboru, vložte části ihned po **konfigurace** element. Následující příklad ukazuje, jaké horní části kořenu `Web.config` soubor by měl vypadat jako když dokončíte přesouvání elementů.

> [!NOTE]
> V následujícím příkladu řádky zabalené čitelnější.


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Krok 2 (všechny verze služby IIS)

Tento krok je vyžadován podřízené webové aplikace ASP.NET 4, zda je spuštěný na služby IIS 6 nebo IIS 7 (nebo IIS 7.5).

V `Web.config` přidat soubor nadřazené webové aplikace, která běží ASP.NET 2 nebo ASP.NET 3.5 **umístění** značky, které explicitně určuje (pro systémy konfiguraci IIS a ASP.NET), pouze položky konfigurace platí pro nadřazené webové aplikace. Následující příklad ukazuje syntaxi **umístění** elementu, který chcete přidat:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

Následující příklad ukazuje jak **umístění** značky slouží k zabalení všech konfiguračních oddílů, počínaje **appSettings** části a konče **system.webServer**části.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Po dokončení kroků 1 a 2 podřízené 4 webových aplikací ASP.NET se spustí bez chyb.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 weby se nepodaří spustit v počítačích, kde je nainstalován služby SharePoint

Mít webové servery, které používají službu SharePoint `Web.config` soubor, který je nasazen v kořenovém adresáři web služby SharePoint (například `c:\inetpub\wwwroot\web.config` pro Default Web Site). V tomto `Web.config` souboru, SharePoint nastaví vlastní částečným vztahem důvěryhodnosti úrovně s názvem WSS\_minimální.

Pokud se pokusíte spustit ASP.NET 4 webovou stránku, která je nasazena jako podřízená položka tohoto typu web služby SharePoint, zobrazí se chybová zpráva:

`Could not find permission set named 'ASP.NET'.`

K této chybě dojde, protože vypadá infrastrukturu zabezpečení (CA) přístup kódu ASP.NET 4 pro sadu oprávnění s názvem ASP.NET. Však částečné důvěryhodnosti konfiguračního souboru, který je odkazován objektem WSS\_minimální neobsahuje žádné sady oprávnění s tímto názvem.

Aktuálně nejsou na verzi SharePoint, který je kompatibilní s technologií ASP.NET. V důsledku toho byste neměli spouštět webu technologie ASP.NET 4 jako podřízené lokality pod webu SharePoint lokalit.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>Vlastnost HttpRequest.FilePath už obsahuje PathInfo hodnoty

Předchozí verze technologie ASP.NET, které jsou zahrnuté **PathInfo** v hodnotě vrácená z různých související s cestou vlastnosti souboru, včetně **HttpRequest.FilePath**,  **HttpRequest.AppRelativeCurrentExecutionFilePath**, a **HttpRequest.CurrentExecutionFilePath**. Technologie ASP.NET 4 už obsahuje **PathInfo** hodnotu ve vrácené hodnoty z těchto vlastností. Místo toho **PathInfo** informace jsou k dispozici v **HttpRequest.PathInfo**. Představte si například následující fragment adresy URL:

`/testapp/Action.mvc/SomeAction`

V předchozích verzích technologie ASP.NET **požadavku HTTP** vlastnosti mít následující hodnoty:

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (prázdný)

V technologii ASP.NET 4 **požadavku HTTP** vlastnosti místo toho mít následující hodnoty:

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 aplikace může způsobit chyby HttpException, odkazující na soubor eurl.axd

Po ASP.NET 4 byla zapnuta služby IIS 6, aplikace ASP.NET 2.0, které běží na služby IIS 6 (v systému Windows Server 2003 nebo Windows Server 2003 R2) může způsobit chyby, například následující:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

K této chybě dojde, protože když ASP.NET zjistí, že na web konfigurován pro použití technologie ASP.NET 4, nativní součást ASP.NET 4 předá adresu URL bez přípony pro spravované část technologie ASP.NET pro další zpracování. Pokud jsou virtuální adresáře, které jsou pod webu technologie ASP.NET 4 nakonfigurované pro použití technologie ASP.NET 2.0, ale zpracování URL bez přípony ve výsledcích tento způsob upravené adresu URL, která obsahuje řetězec "soubor eurl.axd". Tato adresa URL změny se pak posílají do aplikace ASP.NET 2.0. ASP.NET 2.0 nemůže rozpoznat formát "soubor eurl.axd". Proto se pokusí vyhledat soubor s názvem technologii ASP.NET 2.0 `eurl.axd` a spusťte ho. Protože žádný takový soubor neexistuje, žádost skončí s chybou s **HttpException** výjimka.

Můžete alternativně vyřešit tento problém pomocí jedné z následujících možností.

### <a name="option-1"></a>možnost 1

Pokud ASP.NET 4 není potřeba, aby bylo možné spustit na webu, přemapujte webu místo toho použít technologii ASP.NET 2.0.

### <a name="option-2"></a>Možnost 2

Pokud technologie ASP.NET 4 je potřeba ke spuštění webové stránky, přesuňte všechny virtuální adresáře podřízené technologii ASP.NET 2.0 na jiný web, který je namapovaný na technologii ASP.NET 2.0.

### <a name="option-3"></a>Možnost 3

Pokud není praktické přemapování webu na technologii ASP.NET 2.0 nebo chcete změnit umístění virtuální adresář, explicitně zakážete zpracování v rozhraní ASP.NET 4 URL bez přípony. Použijte následující postup:

1. V registru systému Windows otevřete následující uzly:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Vytvořte novou **DWORD** hodnotu s názvem **EnableExtensionlessUrls**.
2. Nastavit **EnableExtensionlessUrls** na hodnotu 0. To zakáže bez přípony chování adresy URL.
3. Hodnota registru uložte a zavřete editor registru.
4. Spustit **iisreset** nástroj příkazového řádku, což způsobí, že služba IIS ke čtení novou hodnotu registru.

> [!NOTE]
> Nastavení **EnableExtensionlessUrls** 1 umožňuje bez přípony chování adresy URL. Toto je výchozí nastavení, pokud není zadaná žádná hodnota.


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Obslužné rutiny událostí může není není vyvolána v výchozí dokument v IIS 7 nebo IIS 7.5 integrovaný režim

Technologie ASP.NET 4 obsahuje změny, které mění jak **akce** atribut HTML **formuláře** prvek je vykreslovaný při přeloží adresu URL bez přípony na výchozí dokument. Jedná se například adresy URL bez přípony přeložit na výchozí dokument [ http://contoso.com/ ](http://contoso.com/), což v požadavku na [ http://contoso.com/Default.aspx ](http://contoso.com/Default.aspx).

Rozhraní ASP.NET 4 nyní vykreslí HTML **formuláře** elementu **akce** při požadavku na adresu URL bez přípony, který má výchozí dokument k němu mapována hodnota atributu jako prázdný řetězec. Například ve starších verzích technologie ASP.NET, požadavek na [ http://contoso.com ](http://contoso.com) by způsobilo požadavek na `Default.aspx`. V tomto dokumentu, otevření **formuláře** vykreslená značka jako v následujícím příkladu:

`<form action="Default.aspx" />`

V technologii ASP.NET 4, požadavek na [ http://contoso.com ](http://contoso.com) žádost, aby výsledkem také `Default.aspx`. Ale nyní ASP.NET vykreslí otevírání HTML **formuláře** značky jako v následujícím příkladu:

`<form action="" />`

Tento rozdíl ve způsobu **akce** atribut je vykreslen může způsobit malých změn v zpracování post formuláře pomocí služby IIS a ASP.NET. Když **akce** atribut je řetězec prázdný, služby IIS **DefaultDocumentModule** objektu bude vytvořit podřízenou žádost pro `Default.aspx`. Ve většině případů, je tento podřízený požadavek transparentní do kódu aplikace a `Default.aspx` stránka se spustí normálně.

Potenciální interakce mezi spravovaného kódu a IIS 7 nebo IIS 7.5 integrovaného režimu však může způsobit spravované stránky .aspx přestane fungovat správně při podřízený požadavek. Pokud nastanou následující podmínky, podřízené požadavku `Default.aspx` dokumentu bude mít za následek chybu nebo neočekávané chování:

1. Stránky ASPX je odesláno prohlížeči s **formuláře** elementu **akce** atribut nastaven na "".
2. Formuláře je odeslána zpět do technologie ASP.NET.
3. Spravovaný modul HTTP čte určitou část obsahu entity. Například modul čte **Request.Form** nebo **Request.Params**. To způsobí, že obsah entity požadavku POST čtení do spravované paměti. Obsah entity je v důsledku toho již nebude k dispozici pro všechny moduly nativního kódu, které běží ve službě IIS 7 nebo IIS 7.5 integrovaného režimu.
4. Služby IIS **DefaultDocumentModule** objekt nakonec spustí a vytvoří podřízený požadavek `Default.aspx` dokumentu. Protože obsah entity je již přečtena část spravovaného kódu, neexistuje však žádný obsah entity k dispozici k odeslání do podřízený požadavek.
5. Spuštění kanál protokolu HTTP pro podřízenou žádost o obslužnou rutinu pro `.aspx` soubory spouští během fáze spuštění obslužné rutiny.
6. Protože neexistuje žádný obsah entity, nejsou žádné proměnné formuláře a bez zobrazení stavu, a proto nejsou dostupné pro obslužnou rutinu stránky .aspx, chcete-li zjistit, jaké události (pokud existuje) by měla být aktivována žádné informace. V důsledku toho žádné zpětného volání obslužné rutiny pro stránky .aspx ovlivněných spustit.

Tento problém můžete vyřešit následujícími způsoby:

- Identifikujte modul HTTP, který přistupuje k obsahu entity žádosti během požadavků dokumentu výchozí a určit, zda může být nakonfigurován pro spouštění jenom pro spravované požadavky. V integrovaném režimu služby IIS 7 a IIS 7.5, může být označen vytváření modulů HTTP v spouštět jenom pro spravované požadavky přidáním následující atribut v modulu **system.webServer/modules** položku:

- `precondition="managedHandler"`

- Toto nastavení zakáže modul pro požadavky, že služba IIS 7 a IIS 7.5 určit jako nebude spravovat požadavky. Pro výchozí dokument požadavky první požadavek je pro adresu URL bez přípony. Proto služby IIS neběží žádné spravované moduly, které jsou označené s podmínkou pro spravované obslužné rutiny během zpracování požadavku počáteční. V důsledku toho spravované moduly nebudou neúmyslně číst obsah entity a proto obsahu entity je stále k dispozici a je předají podřízený požadavek a výchozí dokument.

- Pokud problematické modulů HTTP nutné spustit pro všechny požadavky (statické soubory, pro adresy URL bez přípony, které odkazují na **DefaultDocumentModule** objekt, pro spravované žádosti, atd.), upravovat stránky .aspx ovlivněných podle explicitně nastavení **akce** vlastnost stránky **System.Web.UI.HtmlControls.HtmlForm** ovládacího prvku na prázdný řetězec. Například, pokud je výchozí dokument `Default.aspx`, upravit kód stránky explicitně nastavit **HtmlForm** ovládacího prvku **akce** vlastnost "Default.aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Změny pro implementaci kódu ASP.NET přístup zabezpečení (CA)

ASP.NET 2.0, a rozšíření použít funkce ASP.NET, které byly přidány v 3.5, .NET Framework 1.1 a model zabezpečení (CA) 2.0 kód přístupu. Implementace certifikační Autority v technologii ASP.NET 4 však byl podstatně overhauled. Částečně důvěryhodné aplikace ASP.NET, které spoléhat na důvěryhodný kód spuštěný v globální mezipaměti sestavení (GAC) může dojít v důsledku toho různé výjimky zabezpečení. Částečně důvěryhodné aplikace, které závisí na rozsáhlých úpravách zásady počítače CAS může selhat také s výjimkami zabezpečení.

Můžete obnovit částečně důvěryhodné aplikace ASP.NET 4 pro chování ASP.NET 1.1 a 2.0 pomocí nové **legacyCasModel** atribut **důvěryhodnosti** konfigurační element, jak je znázorněno v následujícím příkladu :

`<trust level= "Medium" legacyCasModel="true" />`

Při navrácení starší verze modelu certifikační Autority, jsou povolené následující chování staré certifikační Autority:

- Zásady počítače certifikační Autority je omezení dodržena.
- Několik sad různých oprávnění v jediné doméně aplikace jsou povoleny.
- Kontrolní výrazy výslovná oprávnění nejsou požadována pro sestavení v mezipaměti GAC, které jsou vyvolány, pokud je v zásobníku pouze ASP.NET nebo jiný kód, rozhraní .NET Framework.

Jeden scénář nelze vrátit zpět v rozhraní .NET Framework 4: jiných webových částečně důvěryhodné aplikace můžou zavolat už určitých rozhraní API v System.Web.dll a System.Web.Extensions.dll. V předchozích verzích rozhraní .NET Framework bylo možné pro jiné webové částečným vztahem důvěryhodnosti aplikace explicitně udělit oprávnění <strong>AspNetHostingPermission</strong> oprávnění. Tyto aplikace pak může použít <strong>System.Web.HttpUtility</strong>, typy, které do <strong>System.Web.ClientServices.\< / strong > * obory názvů a typy související s členství, role a profily. Tyto typy volání z aplikace s částečnou důvěryhodností mimo Web není podporován v rozhraní .NET Framework 4.

> [!NOTE]
> **HtmlEncode** a **HtmlDecode** funkce **System.Web.HttpUtility** třída byla přesunuta do nové rozhraní .NET Framework 4  **System.Net.WebUtility** třídy. Pokud pouze funkce ASP.NET, která se používá, který byl, upravit kód aplikace s novým **WebUtility** třídy místo.


Toto je shrnutí změn v implementaci výchozí certifikační Autority v technologii ASP.NET 4:

- ASP.NET aplikační domény jsou teď homogenní aplikační domény. Pouze částečným vztahem důvěryhodnosti a plné důvěryhodnosti grant sady jsou k dispozici v doméně aplikace.
- Prostředí ASP.NET nastaví udělení částečné důvěryhodnosti nezávisí žádné zásady CAS podnikové úrovni, počítače nebo uživatele.
- Použít model rozhraní .NET Framework 4 průhlednost byla převedena sestavení ASP.NET, která poskytuje 3.5 a 3.5 SP1.
- Použití technologie ASP.NET **AspNetHostingPermission** podstatně snížit atribut. Většina instance tohoto atributu byly odebrány z veřejné rozhraní API ASP.NET.
- Dynamicky kompilované sestavení, které jsou vytvořené pomocí poskytovatele sestavení pro ASP.NET byly aktualizovány explicitně označit sestavení jako transparentní.
- Ve všech sestaveních ASP.NET jsou nyní označeny tak, že je atribut APTCA dodržení pouze v webových hostitelských prostředích. Částečně důvěryhodné jiných webových hostitelských prostředích jako ClickOnce nebude možné provést volání do sestavení ASP.NET.

Další informace o modelu zabezpečení přístupu kódu novou technologii ASP.NET 4 najdete v tématu [pomocí zabezpečení přístupu kódu v aplikacích ASP.NET](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) na webu MSDN.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>Byly přesunuty MembershipUser a dalších typů v Namespace System.Web.Security

Některé typy, které se používají v členství technologie ASP.NET byly přesunuté z `System.Web.dll` do nové sestavení System.Web.ApplicationServices.dll. Typy byly přesunuty za účelem vyřešení závislostí architektonických vrstev mezi typy v klientovi a v rozšířených SKU rozhraní .NET Framework.

Webové projekty nemají problémy v důsledku přesunutí těchto typů, protože System.Web.ApplicationServices.dll byl přidán do seznamu odkazovaná sestavení, který se používá ve výchozím nastavení systém kompilace technologie ASP.NET. Pokud provádíte upgrade webový projekt, který byl vytvořen pomocí dřívější verze ASP.NET na ASP.NET 4 tak, že otevřete v sadě Visual Studio 2010, projekt bude kompilovat bez chyb.

Podobně pokud provádíte upgrade projektu webové aplikace, která byla vytvořena ve starší verzi technologie ASP.NET na ASP.NET 4 tak, že otevřete v sadě Visual Studio 2010, proces upgradu přidá odkaz na soubor System.Web.ApplicationServices.dll do projektu. Proto upgrade webové projekty aplikací bude také kompilovat bez chyb.

Zkompilované soubory (binární), které byly vytvořené pomocí dřívějších verzí technologie ASP.NET se také spustí bez chyb na technologii ASP.NET 4, i když typy členství se přesunuly na jiném sestavení. Zadejte informace o předávání přidala na technologii ASP.NET 4 verzi `System.Web.dll` , automaticky směruje běhu odkazy pro tyto typy do nového umístění pro typy.

Knihovny tříd, které používají konkrétní členství typy a které byly upgradovány z dřívějších verzí technologie ASP.NET se však nezdaří kompilovat při použití v projektu ASP.NET 4. Například projektu knihovny tříd se nemusí podařit zkompilování a zobrazovat chyby například následující:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Tento problém můžete vyřešit přidáním odkazu ve vašem projektu knihovny tříd System.Web.ApplicationServices.dll.

Následující seznam ukazuje *System.Web.Security* typy, které byly přesunuty z `System.Web.dll` soubor System.Web.ApplicationServices.dll:

- *System.Web.Security.MembershipCreateStatus*
- *System.Web.Security.Membership.CreateUserException*
- *System.Web.Security.MembershipPasswordException*
- *System.Web.Security.MembershipPasswordFormat*
- *System.Web.Security.MembershipProvider*
- *System.Web.Security.MembershipProviderCollection*
- *System.Web.Security.MembershipUser*
- *System.Web.Security.MembershipUserCollection*
- *System.Web.Security.MembershipValidatePasswordEventHandler*
- *System.Web.Security.ValidatePasswordEventArgs*
- *System.Web.Security.RoleProvider*
- <a id="0.1_a"></a>*System.Web.Configuration.MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>Výstup ukládání do mezipaměti změny k odlišení \* hlavičky protokolu HTTP

V technologii ASP.NET 1.0 nezdařila z důvodu chyby v mezipaměti stránek, které zadané `Location="ServerAndClient"` jako nastavení výstupní mezipaměti pro vydávání `Vary:*` hlavičky protokolu HTTP v odpovědi. To mělo za následek předání klientský prohlížeč pro ukládání do mezipaměti nikdy stránce místně.

V technologii ASP.NET 1.1 **System.Web.HttpCachePolicy.SetOmitVaryStar** metoda jste přidali, může volat pro potlačení `Vary:*` záhlaví. Tato metoda je vybraná, protože změna emitovaného záhlaví HTTP byla považována za potenciálně nejnovější změny v době. Ale vývojáři mít potíže s chování technologie ASP.NET a sestavy chyb naznačují, že vývojáři neberou v existující **SetOmitVaryStar** chování.

V technologii ASP.NET 4 bylo rozhodnuto opravit problém nevyřeší. `Vary:*` Hlavičky protokolu HTTP už nevydává odpovědi, které zadat direktivu následující:

`<%@OutputCache Location="ServerAndClient" %>`

V důsledku toho **SetOmitVaryStar** k potlačení, už je potřeba `Vary:*` záhlaví.

V aplikacích, které určují `Location="ServerAndClient"` v **@ OutputCache** direktivy na stránce, uvidíte nyní chování implicitní název **umístění** hodnotu atributu – tj, bude stránky ukládat do mezipaměti v prohlížeči bez nutnosti zavoláte **SetOmitVaryStar** metoda.

Pokud stránky v aplikaci musí emitování `Vary:*`, volání **AppendHeader** metoda jako v následujícím příkladu:

`HttpResponse.AppendHeader("Vary","*");`

Alternativně můžete změnit hodnotu ukládání výstupu do mezipaměti **umístění** atribut "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Typy System.Web.Security pro Passport jsou zastaralé

Podpora služby Passport integrovaný do technologie ASP.NET 2.0 je už zastaralé a nepodporované několik let z důvodu změn v Passport (nyní LiveID). V důsledku toho pět typů související se Passport v **System.Web.Security** jsou nyní označeny **ObsoleteAttribute** atribut.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>Vlastnost MenuItem.PopOutImageUrl nepodaří Vykreslit obrázek v technologii ASP.NET 4

V technologii ASP.NET 3.5 *MenuItem.PopOutImageUrl* vlastnost umožňuje zadat adresu URL pro obrázek, který se zobrazí v položku nabídky k označení, že položky nabídky má dynamické podnabídky. Následující příklad ukazuje, jak tuto vlastnost nezadáte v značek v technologii ASP.NET 3.5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

V důsledku změnu návrhu v technologii ASP.NET 4, je vykreslen žádný výstup *PopOutImageUrl* Pokud je nastavena pro *MenuItem* třídy. Místo toho musíte zadat adresu URL obrázku přímo v *nabídky* řízení buď pomocí *StaticPopOutImageUrl* vlastnost nebo *DynamicPopOutImageUrl* vlastnost. Při práci s statickou nabídku, *Menu.StaticPopOutImageUrl* vlastnost určuje adresu URL pro obrázek, který se zobrazí pro indikaci, zda má položku nabídky statické podnabídky, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Pokud pracujete s dynamické nabídky, můžete použít *Menu.DynamicPopOutImageUrl* vlastnosti a určit adresu URL pro image, která určuje, zda má dynamické položky nabídky podnabídky. Následující příklad je podobný předchozímu, ale ukazuje, jak nastavit *DynamicPopOutImageUrl* vlastnost pro dynamické nabídky.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Pokud *Menu.DynamicPopOutImageUrl* není nastavena vlastnost a *Menu.DynamicEnableDefaultPopOutImage* je nastavena na *false*, zobrazí se žádný obrázek. Podobně pokud *StaticPopOutImageUrl* není nastavena vlastnost a *StaticEnableDefaultPopOutImage* je nastavena na *false*, se zobrazí žádný obrázek.

Když nastavíte cesty pro tyto vlastnosti, použijte lomítkem (/) namísto zpětné lomítko (\). Další informace najdete v tématu [Menu.StaticPopOutImageUrl a Menu.DynamicPopOutImageUrl se nepodařilo vykreslit bitové kopie při cesty obsahovat zpětná lomítka](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.") jinde v tomto dokumentu.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl a Menu.DynamicPopOutImageUrl nepodaří při cesty obsahovat zpětná lomítka vykreslení obrázků

V technologii ASP.NET 4, bitové kopie, které zadáte pomocí *Menu.StaticPopOutImageUrl* a *Menu.DynamicPopOutImageUrl* vlastnosti se nezobrazí, pokud cesta obsahuje backlashes (\). Jedná se o změnu z dřívějších verzí technologie ASP.NET.

Následující příklad *nabídky* řídit kód ukazuje *StaticPopOutImageUrl* vlastností nastavenou pomocí cesty, které obsahuje zpětné lomítko. V technologii ASP.NET 4 nebude vykreslovat image zadaná ve vlastnosti.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Chcete-li tento problém, změňte cestu hodnoty, které jsou určené v *StaticPopOutImageUrl* a *DynamicPopOutImageUrl* vlastnosti, které chcete použít lomítka (/). Následující příklad ukazuje, tato změna:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Pamatujte, že aplikace, které byly migrovány z předchozích verzí aplikace ASP.NET na ASP.NET 4 také může mít vliv, protože *MenuItem.PopOutImageUrl* vlastnost byl změněn. Další informace najdete v tématu [vlastnost MenuItem.PopOutImageUrl selže k vykreslení bitovou kopii v technologii ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") jinde v tomto dokumentu.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Právní omezení

Toto je předběžná dokument a mohou být změněny podstatně uvedením produktu softwaru popsaných v tomto dokumentu.

Informace obsažené v tomto dokumentu představují aktuální pohled společnosti Microsoft Corporation na uvedené problémy k datu publikování. Protože společnost Microsoft musí reagovat na měnící se podmínky na trhu, by neměl být interpretovat jako závazek společnosti Microsoft a společnost Microsoft nemůže zaručit přesnost informací jsou prezentovány po provedení datu publikování.

Tento dokument White Paper je pouze informativní charakter. MICROSOFT NEPOSKYTUJE ŽÁDNÉ ZÁRUKY, VÝSLOVNÉ, PŘEDPOKLÁDANÉ NEBO STATUTÁRNÍ INFORMACE V TOMTO DOKUMENTU.

Dodržovat všechny platné zákony o autorských právech zodpovídá uživatele. Bez omezení autorských práv, tento dokument může být opakuje, uložené v systému načtení nebo přenést jakýmkoli způsobem (ať už elektronicky, mechanických, včetně kopírování nebo) nebo pro jakýkoli účel bez předchozího písemného svolení společnosti Microsoft Corporation.

Společnost Microsoft může být patentů, patentová aplikací, ochranné známky, autorská práva nebo jiná duševní vlastnictví předmět uvedený v tomto dokumentu. Výslovně uvedeno v licenční smlouvě společnost Microsoft neposkytuje tento dokument žádnou licenci na tyto patenty, ochranné známky, autorská práva nebo jiné duševní vlastnictví.

Pokud není uvedeno jinak, příklady společností, organizací, produktů, názvů domén, e-mailové adresy, loga, osob, míst a událostí použité v ukázkách jsou smyšlené a spojení se žádné skutečnou společností, organizace, produktu, název domény, e-mailu adresu, logem, osoba, místní nebo událostí je určený nebo událostmi.

© 2010 Microsoft Corporation. Všechna práva vyhrazena.

Microsoft a Windows jsou registrované ochranné známky nebo ochranné známky společnosti Microsoft Corporation ve Spojených státech a dalších zemích.

Názvy společností a produktů může být ochranné známky příslušných vlastníků.
