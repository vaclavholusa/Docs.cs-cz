---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 – nejnovější změny | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument popisuje změny, které se provedly pro verzi rozhraní .NET Framework 4. Tato verze může potenciálně ovlivnit aplikace, které byly vytvořeny pomocí...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 112483abdd920649fb530959a538b1d5ed6064d7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756630"
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 – nejnovější změny
====================
> Tento dokument popisuje změny, které se provedly pro verzi rozhraní .NET Framework 4. Tato verze může potenciálně ovlivnit aplikace, které byly vytvořeny pomocí dřívějších verzích, včetně verzí technologie ASP.NET 4 Beta 1 a beta verze 2.
> 
> [Stáhněte si tento dokument White Paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Obsah

[ControlRenderingCompatibilityVersion nastavení v souboru Web.config](#0.1__Toc256770141 "_Toc256770141")  
[Změny ClientIDMode](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode a nyní UrlEncode kódování jednoduchých uvozovek](#0.1__Toc256770143 "_Toc256770143")  
[Stránka technologie ASP.NET (.aspx) Analyzátor je Stricter](#0.1__Toc256770144 "_Toc256770144")  
[Browser Definition Files Updated](#0.1__Toc256770145 "_Toc256770145")  
[System.Web.Mobile.dll odebrán ze souboru konfigurace kořenového webu](#0.1__Toc256770146 "_Toc256770146")  
[Ověření požadavku ASP.NET](#0.1__Toc256770147 "_Toc256770147")  
[Výchozí algoritmus hash je nyní HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Chyby konfigurace související s novou konfiguraci technologie ASP.NET 4 kořenové](#0.1__Toc256770149 "_Toc256770149")  
[Aplikace ASP.NET 4 podřízené nezdaří spustit v technologii ASP.NET 2.0 nebo technologie ASP.NET aplikací verze 3.5](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 weby se nemůže spustit na počítačích, kde je nainstalován SharePoint](#0.1__Toc256770151 "_Toc256770151")  
[Vlastnost HttpRequest.FilePath již obsahuje hodnoty PathInfo](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 může aplikace generovat HttpException chyby, které odkazují na soubor eurl.axd](#0.1__Toc256770153 "_Toc256770153")  
[Obslužné rutiny událostí může není není vyvolána ve výchozí dokument v IIS 7 nebo IIS 7.5 integrovaném režimu](#0.1__Toc256770154 "_Toc256770154")  
[Změny v kódu ASP.NET Access Security (CAS) implementace](#0.1__Toc256770155 "_Toc256770155")  
[Byly přesunuty MembershipUser a další typy v Namespace System.Web.Security](#0.1__Toc256770156 "_Toc256770156")  
[Výstup ukládání do mezipaměti změny se liší \* hlavičky protokolu HTTP](#0.1__Toc256770157 "_Toc256770157")  
[Typy System.Web.Security pro Passportu jsou zastaralé](#0.1__Toc256770158 "_Toc256770158")  
[Vlastnost MenuItem.PopOutImageUrl selže, aby se vykreslil obraz v technologii ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl a navrátit služby po Menu.DynamicPopOutImageUrl k vykreslování obrázků, když cesty obsahovat obrácená lomítka](#0.1__Toc256770160 "_Toc256770160")  
[Disclaimer](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>ControlRenderingCompatibilityVersion nastavení v souboru Web.config

Ovládací prvky ASP.NET byly upraveny v rozhraní .NET Framework verze 4 k vám umožní zadat, jak přesněji vykreslují značek. V předchozích verzích rozhraní .NET Framework, protože některé ovládací prvky ho kód, který jste měli žádný způsob, jak zakázat. Ve výchozím nastavení technologii ASP.NET 4 tohoto typu značky již vygenerována.

Pokud používáte Visual Studio 2010 k upgradu vaší aplikace v ASP.NET 2.0 nebo technologie ASP.NET 3.5, nástroj automaticky přidá nastavení, které `Web.config` soubor, který uchovává starší vykreslování. Nicméně pokud upgradujete aplikaci tak, že změníte fond aplikací ve službě IIS cílit na rozhraní .NET Framework 4, ASP.NET používá nový režim vykreslování ve výchozím nastavení. Pokud chcete zakázat nový režim vykreslování, přidejte následující nastavení v `Web.config` souboru:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Hlavní vykreslování změny, které přináší nové chování jsou následující:

- **Image** a **ImageButton** už vykreslení ovládacích prvků `border="0"` atribut.
- **BaseValidator** třídy a ověřování ovládacích prvků, které jsou odvozeny z něj už vykreslit červený text ve výchozím nastavení.
- **HtmlForm** ovládacího prvku nezobrazuje **název** atribut.
- **Tabulky** ovládací prvek již vykreslení `border="0"` atribut.
- Ovládací prvky, které nejsou určeny pro uživatelský vstup (například **popisek** ovládací prvek) už zobrazovat `disabled="disabled"` atribut, pokud jejich **povoleno** je nastavena na **false**(nebo pokud toto nastavení dědí z ovládacího prvku kontejneru).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode změny

**ClientIDMode** nastavení v technologii ASP.NET 4 umožňuje určit, jak technologie ASP.NET generuje **id** atributů elementů HTML. V předchozích verzích technologie ASP.NET, je ekvivalentní výchozí chování **AutoID** nastavení **ClientIDMode**. Ve výchozím nastavení je teď ale **Predictable**.

Pokud používáte Visual Studio 2010 k upgradu vaší aplikace v ASP.NET 2.0 nebo technologie ASP.NET 3.5, nástroj automaticky přidá nastavení, které `Web.config` soubor, který uchovává chování starších verzích rozhraní .NET Framework. Ale pokud upgradujete aplikaci tak, že změníte fond aplikací ve službě IIS cílit na rozhraní .NET Framework 4, ASP.NET používá nový režim ve výchozím nastavení. Chcete-li zakázat režim nové ID klienta, přidejte následující nastavení v `Web.config` souboru:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode a nyní UrlEncode kódovat jednoduché uvozovky

V technologii ASP.NET 4 **HtmlEncode** a **UrlEncode** metody **HttpUtility** a **HttpServerUtility** třídy byla aktualizována, aby kódování znak jednoduché uvozovky (') následujícím způsobem:

- **HtmlEncode** metoda kóduje výskyty jednoduché uvozovky jako.
- **UrlEncode** metoda kóduje jako % 27 instance jednoduché uvozovky.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>Stránka technologie ASP.NET (.aspx) Analyzátor je Stricter

Analyzátor stránky pro stránky ASP.NET (`.aspx` soubory) a uživatelských ovládacích prvků (`.ascx` souborů) je přísnější v technologii ASP.NET 4 a odmítne další výskyty neplatný kód. Například následující dva fragmenty kódu by úspěšně analyzovat ve starších verzích technologie ASP.NET, ale bude nyní vyvolat chyby analyzátoru v technologii ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Všimněte si, že neplatné středníky na konci **HiddenField** značky.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Všimněte si, uzavřené **styl** atribut, který se spustí do **CssClass** atribut.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Soubory definice prohlížeče aktualizovat

Soubory definice prohlížeče se aktualizovaly informace o nových a aktualizovaných prohlížečích a zařízeních. Starší prohlížeče a zařízení, jako jsou Netscape Navigator byly odebrány a novějších prohlížečů a zařízení, jako jsou Google Chrome a Apple iPhone se přidaly.

Pokud vaše aplikace obsahuje definice vlastních prohlížečů, které dědí z jednoho z definicí prohlížeče, které byly odebrány, zobrazí se chyba. Například pokud `App_Browsers` složka obsahuje definici prohlížeče, která dědí z definice IE2 prohlížeče, zobrazí se následující chybová zpráva konfigurace:

- Nebyl nalezen element prohlížeče nebo brány s ID "IE2".

> [!NOTE]
> **HttpBrowserCapabilities** objektu (který je zveřejněný na stránce **Request.Browser** vlastnost) je řízenou soubory definice prohlížeče. Proto informace vrácené přístupem k vlastnosti tohoto objektu v technologii ASP.NET 4, může být jiný než informace vrácené v dřívější verzi technologie ASP.NET.


Můžete se vrátit k původní definice soubory prohlížeče tak, že zkopírujete soubory definice prohlížeče z následující složky:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Zkopírujte soubory do odpovídajícího `\CONFIG\Browsers` složku pro technologii ASP.NET 4. Po zkopírování souborů, spuštění Aspnet\_regbrowsers.exe nástroj příkazového řádku.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System.Web.Mobile.dll odebrán ze souboru konfigurace kořenového webu

V předchozích verzích technologie ASP.NET, odkaz na sestavení System.Web.Mobile.dll byla zahrnuta v kořenovém adresáři `Web.config` soubor **sestavení** části. Za účelem zlepšení výkonu byl odebrán odkaz na toto sestavení.

Sestavení System.Web.Mobile.dll je zahrnuta v technologii ASP.NET 4, ale je zastaralé. Pokud chcete používat typy ze sestavení System.Web.Mobile.dll, přidejte odkaz na toto sestavení buď do kořenového adresáře `Web.config` souboru nebo k aplikaci `Web.config` souboru. Například pokud chcete použít některou z (nepoužívané) mobilní ovládací prvky ASP.NET, musíte přidat odkaz na sestavení System.Web.Mobile.dll `Web.config` souboru.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Ověření požadavku ASP.NET

Žádosti o ověření funkce v technologii ASP.NET poskytuje určitou úroveň výchozí ochranu před útoky skriptování napříč weby (XSS). V předchozích verzích technologie ASP.NET se ve výchozím nastavení povoleno ověření žádosti. Ale použijí jenom u stránky ASP.NET (`.aspx` soubory a jejich třídy) a pouze pokud byly provádění těchto stránek.

V technologii ASP.NET 4, ve výchozím nastavení, žádost o ověření je povolena pro všechny požadavky, protože je povolená před **BeginRequest** fáze požadavek HTTP. V důsledku toho žádost o ověření platí pro požadavky pro všechny prostředky technologie ASP.NET, nikoli pouze požadavky na stránku .aspx. Patří sem požadavky, jako je například volání webové služby a vlastní obslužné rutiny HTTP. Žádost o ověření je aktivní také v případě vlastních modulů HTTP čtete obsah požadavku HTTP.

V důsledku toho žádost o ověření může nyní dojít k chybám pro požadavky, které dříve není vyvolávat chyby. Chcete-li vrátit k chování funkcí technologie ASP.NET 2.0 žádost o ověření, přidejte následující nastavení v `Web.config` souboru:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Doporučujeme však, že analýza všech chyb ověření požadavku k určení, zda existující obslužné rutiny, moduly nebo jiný vlastní kód přistupuje k potenciálně nebezpečné vstupy HTTP, které mohou být XSS útoku vektory.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Výchozí algoritmus hash je nyní HMACSHA256

Technologie ASP.NET používá k zabezpečení dat, jako jsou soubory cookie pro ověřování formulářů a stavu zobrazení šifrování a algoritmy hash. Ve výchozím nastavení technologii ASP.NET 4 teď používá HMACSHA256 algoritmus hash operací na soubory cookie a stav zobrazení. Starší verze technologie ASP.NET použita starší HMACSHA1 algoritmus.

Vaše aplikace může ovlivnit, pokud spustíte 2.0/ASP.NET smíšené technologie ASP.NET 4 prostředí místo, kde data, jako jsou soubory cookie pro ověřování formuláře musí pracovat across.NET verze rozhraní Framework. Pro konfiguraci aplikace ASP.NET 4 Web používat starší HMACSHA1 algoritmus, přidejte následující nastavení v `Web.config` souboru:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Chyby konfigurace související s novou technologií ASP.NET 4 Konfigurace kořenového adresáře

Konfigurační soubory root ( `machine.config` souboru a kořenové `Web.config` souboru) pro rozhraní .NET Framework 4 (a tedy ASP.NET 4) byly aktualizovány zahrnout většinu informací konfigurace často používaný text, který v technologii ASP.NET 3.5 byl nalezen v aplikace `Web.config` soubory. Z důvodu složitosti spravovaných systémech konfigurace služby IIS 7 a IIS 7.5 spouštění aplikací technologie ASP.NET 3.5 v technologii ASP.NET 4 a v rámci služby IIS 7 a IIS 7.5 může způsobit technologie ASP.NET nebo IIS chyby konfigurace.

Doporučujeme, abyste upgrade aplikací technologie ASP.NET 3.5 na ASP.NET 4 pomocí nástroje pro upgrade projektu v sadě Visual Studio 2010, pokud je to možné. Visual Studio 2010 automaticky upraví aplikace technologie ASP.NET 3.5 `Web.config` souboru tak, aby obsahovala odpovídající nastavení pro technologii ASP.NET 4.

Je však podporovaný scénář pro spuštění pomocí rozhraní .NET Framework 4 bez opětovnou kompilaci aplikací technologie ASP.NET 3.5. V takovém případě budete muset ručně upravit aplikace `Web.config` souborů před spuštěním aplikace v rozhraní .NET Framework 4 a IIS 7 nebo IIS 7.5.

Změny, které možná budete muset vytvořit pro různé kombinace softwaru naleznete v následujících dvou částech.

**Windows Vista SP1 nebo Windows Server 2008 SP1, kde jsou instalovány opravu hotfix KB958854 ani SP2.** V této konfiguraci systému konfigurace služby IIS 7 nesprávně sloučí konfigurace spravované aplikace porovnáním na úrovni aplikace `Web.config` souboru na technologii ASP.NET 2.0 `machine.config` soubory. Z důvodu toho úrovni aplikace `Web.config` soubory z rozhraní .NET Framework 3.5 nebo novější musí mít **system.web.extensions** definice oddílu konfigurace (elementu) aby způsobit selhání ověřování IIS 7.

Ale ručně změnit úroveň aplikace `Web.config` souboru položky, které nejsou přesně shodné původní definice často používaný text konfigurace části, které byly představeny s nástrojem Visual Studio 2008 způsobí chyby konfigurace technologie ASP.NET. (Výchozí položky konfigurace, které jsou generovány nástrojem Visual Studio 2008 fungovat správně.) Běžný problém je, že ručně upravit `Web.config` vynechte soubory **allowDefinition** a **requirePermission** konfigurační atributy, které se nacházejí v různých konfiguračního oddílu definice. To způsobí neshodu mezi zkrácený konfigurační oddíl v úrovni aplikace `Web.config` soubory a dokončení definice v technologii ASP.NET 4 `machine.config` souboru. V důsledku toho systém konfigurace technologie ASP.NET 4 za běhu, vyvolá chybu konfigurace.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2 a také na Windows Vista SP1 a Windows Server 2008 SP1, kde je nainstalována oprava hotfix KB958854.**

V tomto scénáři, systém nativní konfigurace služby IIS 7 a IIS 7.5 vrátí chyby v konfiguraci, protože provádí porovnání textu na **typ** atribut, který je definován pro obslužné rutiny spravovaného konfiguračního oddílu. Protože všechny `Web.config` soubory, které jsou generovány pomocí sady Visual Studio 2008 a Visual Studio 2008 SP1 mají "3,5" v řetězci typ pro **system.web.extensions** (a související) obslužné rutiny konfiguračního oddílu a protože technologie ASP.NET 4 `machine.config` soubor má "4.0" **typ** atribut pro stejném obslužné rutiny konfiguračního oddílu, aplikace, které jsou generovány v sadě Visual Studio 2008 nebo Visual Studio 2008 SP1, vždy neúspěšné ověření konfigurace ve službě IIS 7 a IIS 7.5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Řešení těchto problémů

Alternativní řešení pro první scénář je aktualizovat na úrovni aplikace `Web.config` zahrnutím často používaný text konfigurace ze souboru `Web.config` soubor, který se vygeneroval automaticky pomocí sady Visual Studio 2008.

Alternativní řešení pro první scénář je k instalaci aktualizací Service Pack 2 pro Vista nebo Windows Server 2008 ve vašem počítači nebo k instalaci oprav hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) Chcete-li vyřešit chování nesprávné konfiguraci a slučování Systému konfigurace služby IIS. Však po provedení některé z těchto akcí, aplikace bude pravděpodobně dojde k chyby v konfiguraci z důvodu popsané pro druhý scénář.

Alternativním řešením pro druhý scénář je odstranit nebo okomentovat všechny **system.web.extensions** definice konfigurace oddíl a oddíl konfigurace skupiny definic z úrovni aplikace `Web.config` souboru. V horní části na úrovni aplikace jsou obvykle tyto definice `Web.config` souboru a lze je identifikovat podle **configSections** element a jeho podřízené položky.

Pro oba scénáře, doporučujeme vám také ručně odstranit **system.codedom** oddíl, i když tento krok není povinný.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>Aplikace ASP.NET 4 podřízené nezdaří spustit v technologii ASP.NET 2.0 nebo technologie ASP.NET aplikací verze 3.5

ASP.NET 4 aplikací, které jsou nakonfigurovány jako podřízené prvky aplikace, které běží starší verze technologie ASP.NET se nemusí podařit spustit kvůli chybám při konfiguraci nebo kompilaci. Následující příklad ukazuje strukturu adresářů pro ovlivněné aplikaci.

`/parentwebapp` (nakonfigurován pro použití technologie ASP.NET 2.0 nebo technologie ASP.NET 3.5)  
`/childwebapp` (nakonfigurován pro použití technologie ASP.NET 4)

Aplikace `childwebapp` složky se nepodaří spustit ve službě IIS 7 nebo IIS 7.5 a bude sestava chyby v konfiguraci. Text chyby bude obsahovat zpráva podobná následující:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

Ve službě IIS 6, aplikace v `childwebapp` složky také nebude možné spustit, ale ten nahlásí chybu jiný. Text chyby může stát například následující:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Tyto scénáře způsobeny tím, že informace o konfiguraci z nadřazené aplikaci `parentwebapp` složka je součástí hierarchie konfigurační informace, které určuje konečný sloučené konfiguraci nastavení, které jsou používány podřízeného webu v aplikaci `childwebapp` složky. V závislosti na tom, zda aplikace technologie ASP.NET 4 běží na IIS 7 (nebo IIS 7.5) nebo na IIS 6 systému konfigurace služby IIS nebo systému kompilace ASP.NET 4, vrátí se chyba.

Kroky, které je třeba dodržovat tento problém můžete vyřešit a získat podřízené aplikace ASP.NET 4 závisí na aplikaci technologie ASP.NET 4, jestli běží na IIS 6 nebo na IIS 7 (nebo IIS 7.5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Krok 1 (IIS 7 nebo IIS 7.5 jenom)

Tento krok je nutný pouze v operačních systémů, na kterých běží služby IIS 7 a 7.5 služby IIS, která zahrnuje Windows Vista, Windows Server 2008, Windows 7 a Windows Server 2008 R2.

Přesunout **configSections** definice v `Web.config` souboru nadřazená aplikace (aplikace, na kterém běží technologie ASP.NET 2.0 nebo technologie ASP.NET 3.5) do kořenové `Web.config` souboru pro rozhraní.NET Framework 2.0. Prohledá systém nativní konfigurace služby IIS 7 a IIS 7.5 **configSections** element při sloučení hierarchie konfigurační soubory. Přechod **configSections** definice z nadřazeného webovou aplikaci `Web.config` souboru do kořenového adresáře `Web.config` soubor efektivně skryje element z sloučení procesu konfigurace, ke které dochází pro dítě ASP.NET 4 aplikace.

V 32bitové verzi operačního systému nebo pro fondy aplikací 32-bit, kořenové `Web.config` soubor pro technologii ASP.NET 2.0 a technologii ASP.NET 3.5 se obvykle nachází v následující složce:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

64bitová verze operačního systému nebo pro fondy aplikací 64-bit, kořenové `Web.config` soubor pro technologii ASP.NET 2.0 a technologii ASP.NET 3.5 se obvykle nachází v následující složce:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

32bitová verze a 64bitová verze webové aplikace při spuštění v 64bitovém počítači, je nutné přesunout **configSections** elementu až do kořenového `Web.config` soubory pro 32bitové a 64bitové systémy.

Při vložení **configSections** element v kořenovém adresáři `Web.config` soubor, vložte části ihned po **konfigurace** elementu. Následující příklad ukazuje, jaké horní část kořenové `Web.config` soubor by měl vypadat po dokončení přesunutí prvků.

> [!NOTE]
> V následujícím příkladu jsou zabalené řádky pro lepší čitelnost.


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Krok 2 (všechny verze služby IIS)

Tento krok je nutný podřízené webové aplikace ASP.NET 4 určuje, zda běží na IIS 6 nebo IIS 7 (nebo IIS 7.5).

V `Web.config` přidat soubor nadřazené webové aplikace, na kterém běží 2 technologie ASP.NET nebo ASP.NET 3.5 **umístění** značky, které explicitně určuje (pro systémy konfiguraci IIS a ASP.NET), který pouze položky konfigurace platí pro nadřazené webové aplikace. Následující příklad ukazuje syntaxi **umístění** prvek, který chcete přidat:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

Následující příklad ukazuje způsob, jakým **umístění** značka se používá k zabalení všechny konfigurační oddíly funkce začínající **appSettings** části a konče **system.webServer**oddílu.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Po dokončení kroků 1 a 2 podřízené ASP.NET 4 – webové aplikace se spustí bez chyb.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 weby se nemůže spustit na počítačích, kde je nainstalován SharePoint

Máte webové servery, které používají službu SharePoint `Web.config` soubor, který je nasazen do kořenové webu služby SharePoint (například `c:\inetpub\wwwroot\web.config` pro výchozí web). V tomto `Web.config` souboru SharePoint nastaví vlastní částečným vztahem důvěryhodnosti úroveň s názvem WSS\_minimální.

Pokud se pokusíte spustit webu technologie ASP.NET 4, který je nasazen jako podřízený objekt tohoto typu web služby SharePoint, zobrazí se následující chyba:

`Could not find permission set named 'ASP.NET'.`

K této chybě dochází, protože infrastruktury security (CAS) technologie ASP.NET 4 kód přístupu hledá sadu oprávnění s názvem ASP.NET. Ale částečné důvěryhodnosti konfigurační soubor, který je odkazován WSS\_minimální neobsahuje žádné sady oprávnění s tímto názvem.

Aktuálně není verzi Sharepointu, který je kompatibilní s technologií ASP.NET. V důsledku toho by se neměly pokoušet spuštění webu technologie ASP.NET 4 jako podřízené lokality pod Sharepointovými webovými servery.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>Vlastnost HttpRequest.FilePath již obsahuje PathInfo hodnoty

Předchozí verze technologie ASP.NET, které jsou zahrnuté **PathInfo** hodnotu v hodnota vrácená z různých souvisejícím s umístěním vlastnosti souboru, včetně **HttpRequest.FilePath**,  **HttpRequest.AppRelativeCurrentExecutionFilePath**, a **HttpRequest.CurrentExecutionFilePath**. ASP.NET 4 už obsahuje **PathInfo** hodnotu ve vrácené hodnoty z těchto vlastností. Místo toho **PathInfo** informace jsou k dispozici v **HttpRequest.PathInfo**. Představte si například následující fragment adresy URL:

`/testapp/Action.mvc/SomeAction`

V předchozích verzích technologie ASP.NET **HttpRequest** vlastnosti mít následující hodnoty:

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (prázdné)

V technologii ASP.NET 4 **HttpRequest** vlastnosti místo toho mít následující hodnoty:

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 může aplikace generovat HttpException chyby, které odkazují na soubor eurl.axd

Po povolení ASP.NET 4 na IIS 6, aplikace ASP.NET 2.0, které běží na IIS 6 (v systému Windows Server 2003 nebo Windows Server 2003 R2) může způsobit chyby jako je následující:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

K této chybě dochází, protože když ASP.NET zjistí, že na web konfigurován pro použití technologie ASP.NET 4, nativní součást ASP.NET 4 předá adresu URL bez přípony spravované část ASP.NET k dalšímu zpracování. Nicméně pokud virtuálním adresářům, které jsou pod webu technologie ASP.NET 4 jsou nakonfigurovány pro použití technologie ASP.NET 2.0, zpracování URL bez přípony ve výsledcích tento způsob upravené adresy URL, která obsahuje řetězec "soubor eurl.axd". Tuto adresu URL změny se pak posílají do aplikace ASP.NET 2.0. ASP.NET 2.0 nemůže rozpoznat formát "soubor eurl.axd". Proto se pokusí vyhledat soubor s názvem ASP.NET 2.0 `eurl.axd` a spustit ho. Protože neexistuje žádný takový soubor, požadavek selže s **HttpException** výjimky.

Můžete alternativně vyřešit tento problém pomocí jedné z následujících možností.

### <a name="option-1"></a>Možnost 1

Pokud ASP.NET 4 není potřeba, abyste mohli spustit na webovém serveru, přemapování lokality tak, aby místo toho použijte technologii ASP.NET 2.0.

### <a name="option-2"></a>Možnost 2

Pokud ASP.NET 4 je potřeba ke spuštění webové stránky, přesuňte všechny virtuální adresáře technologie ASP.NET 2.0 podřízený jiný web, který je namapovaný na technologii ASP.NET 2.0.

### <a name="option-3"></a>Možnost 3

Pokud není praktické přemapovat na webu technologie ASP.NET 2.0 nebo chcete změnit umístění virtuální adresář, explicitně zakážete zpracování v technologii ASP.NET 4 URL bez přípony. Pomocí následujícího postupu:

1. V registru Windows otevřete následující uzel:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Vytvořte nový **DWORD** hodnotu s názvem **EnableExtensionlessUrls**.
2. Nastavte **EnableExtensionlessUrls** na hodnotu 0. Zakáže bez přípony chování adresy URL.
3. Hodnota registru uložte a zavřete editor registru.
4. Spustit **iisreset** nástroj příkazového řádku, což způsobí, že služba IIS ke čtení novou hodnotu registru.

> [!NOTE]
> Nastavení **EnableExtensionlessUrls** 1 povolí bez přípony chování adresy URL. Toto je výchozí nastavení, pokud není zadána žádná hodnota.


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Obslužné rutiny událostí může není není vyvolána ve výchozí dokument v IIS 7 nebo IIS 7.5 integrovaném režimu

ASP.NET 4 obsahuje změny, které se mění způsob, jakým **akce** atributu HTML **formuláře** prvek je vykreslovaný při bez přípony adresa URL přeloží na výchozí dokument. Příkladem adresy URL bez přípony, překládá na výchozí dokument může být [ http://contoso.com/ ](http://contoso.com/)výsledkem požadavek na [ http://contoso.com/Default.aspx ](http://contoso.com/Default.aspx).

ASP.NET 4 nyní vykreslí HTML **formuláře** elementu **akce** hodnotu atributu jako prázdný řetězec, když se požadavek na adresu URL bez přípony, který má výchozí dokument k němu mapována. Například v dřívějších verzích technologie ASP.NET, požadavek na [ http://contoso.com ](http://contoso.com) výsledkem by byla žádost o `Default.aspx`. V tomto dokumentu, otevírání **formuláře** by být vykreslen jako v následujícím příkladu:

`<form action="Default.aspx" />`

V technologii ASP.NET 4, požadavek na [ http://contoso.com ](http://contoso.com) výsledkem také požadavek na `Default.aspx`. Ale ASP.NET nyní vykreslí otevírání HTML **formuláře** značky jako v následujícím příkladu:

`<form action="" />`

Tento rozdíl v tom, jak **akce** atribut je vykreslen může způsobit drobné změny ve zpracování odeslaného formuláře služby IIS a ASP.NET. Když **akce** atributu je prázdný řetězec, IIS **DefaultDocumentModule** objektu bude vytvořit podřízenou žádost pro `Default.aspx`. Ve většině případů, je tento podřízený požadavek transparentní kód aplikace a `Default.aspx` stránka běží normálně.

Však může způsobit potenciální interakce mezi spravovaným kódem a IIS 7 nebo IIS 7.5 integrovaného režimu spravované stránky .aspx přestane fungovat správně při podřízeného požadavku. Pokud nastanou následující podmínky, podřízený požadavek `Default.aspx` dokumentu bude mít za následek chybu nebo neočekávané chování:

1. Stránky .aspx je odesláno prohlížeči s **formuláře** elementu **akce** atribut nastaven na "".
2. Odeslání formuláře zpět do technologie ASP.NET.
3. Spravovaný modul HTTP načte některá část obsahu entity. Například modul čte **Request.Form** nebo **Request.Params**. To způsobí, že obsah entity požadavku POST k načtení do spravované paměti. V důsledku toho obsahu entity už nejsou k dispozici pro všechny moduly nativní kód, na kterých běží v IIS 7 nebo IIS 7.5 integrovaného režimu.
4. IIS **DefaultDocumentModule** objekt nakonec spustí a vytvoří podřízený požadavek `Default.aspx` dokumentu. Protože obsah entity je již přečtena zasažené spravovaného kódu, neexistuje však žádný k dispozici k odeslání podřízeného požadavku obsah entity.
5. Při spuštění kanálu HTTP pro podřízeného požadavku, obslužné rutiny pro `.aspx` soubory spustí ve fázi spuštění obslužné rutiny.
6. Protože neexistuje žádný obsah entity, neexistují žádné proměnné formuláře a bez zobrazení stavu, a proto nejsou dostupné pro obslužnou rutinu stránky ASPX určit, jaká událost (pokud existuje) by měla být aktivovaná žádné informace. V důsledku toho žádné obslužné rutiny události postback stránky ASPX ovlivněné spustit.

Tento problém můžete vyřešit následujícími způsoby:

- Modul HTTP, který přistupuje k obsahu entity požadavku během požadavků dokumentu výchozí identifikovat a zjistit, zda může být nakonfigurován pro spouštění jenom pro spravované požadavky. V integrovaném režimu služby IIS 7 a IIS 7.5, může být označený z modulů HTTP spustit pouze pro spravované požadavky tak, že přidáte následující atribut modulu **oddílu system.webServer/modules** položky:

- `precondition="managedHandler"`

- Toto nastavení zakáže modul pro požadavky, že služba IIS 7 a IIS 7.5 určit jako nebude spravovat požadavky. Pro výchozí dokument požadavky je první žádost o adresu URL bez přípony. Proto služby IIS neběží žádné spravované moduly, které jsou označeny s podmínkou pro spravovanou obslužnou rutinu během zpracování požadavku. počáteční. V důsledku toho spravované moduly nebudou neúmyslně číst obsah entity a proto obsah entity je stále k dispozici a předává podřízeného požadavku a výchozí dokument.

- Pokud problematické modulů HTTP se ke spuštění pro všechny požadavky (u statických souborů pro adresy URL bez přípony, které odkazují na **DefaultDocumentModule** objekt pro požadavky na spravované, atd.), explicitně modifikovat stránky ASPX ovlivněné podle nastavení **akce** vlastnost na stránce **System.Web.UI.HtmlControls.HtmlForm** ovládacího prvku na neprázdný řetězec. Například, pokud je výchozí dokument `Default.aspx`, upravte kód na stránce explicitně nastavit **HtmlForm** ovládacího prvku **akce** vlastnost "Default.aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Změny pro implementaci kódu ASP.NET Access Security (CAS)

ASP.NET 2.0, a při rozšíření i pro použití funkcí technologie ASP.NET, které byly přidány v 3.5, .NET Framework 1.1 a 2.0 kód access security (CAS) modelu. Implementace certifikační Autority v technologii ASP.NET 4 však byla podstatně overhauled. V důsledku toho částečným vztahem důvěryhodnosti aplikace technologie ASP.NET, které spoléhají na důvěryhodný kód spuštěný v globální mezipaměti sestavení (GAC) může selhat s různými bezpečnostním výjimkám. Částečným vztahem důvěryhodnosti aplikace, které spoléhají na rozsáhlé změny do počítače certifikační Autority zásad může také selhat s výjimkami zabezpečení.

Můžete se vrátit částečným vztahem důvěryhodnosti aplikace ASP.NET 4 pro chování ASP.NET 1.1 a 2.0 s použitím nového **legacyCasModel** atribut **vztahu důvěryhodnosti** prvek konfigurace, jak je znázorněno v následujícím příkladu :

`<trust level= "Medium" legacyCasModel="true" />`

Po obnovení do starší verze CAS modelu jsou povoleny následující chování staré certifikační Autority:

- Zásady počítače certifikační Autority je omezení dodržena.
- Několik sad oprávnění v jediné doméně aplikace jsou povoleny.
- Kontrolní výrazy explicitní oprávnění nejsou požadována pro sestavení v mezipaměti GAC, které jsou vyvolány pouze technologie ASP.NET nebo jiný kód rozhraní .NET Framework je na zásobníku.

Jeden scénář nelze vrátit zpět v rozhraní .NET Framework 4: mimo Web částečným vztahem důvěryhodnosti aplikace můžou zavolat už určitých rozhraní API v System.Web.dll a System.Web.Extensions.dll. V předchozích verzích rozhraní .NET Framework bylo možné mimo Web částečným vztahem důvěryhodnosti aplikací se explicitně udělí oprávnění <strong>AspNetHostingPermission</strong> oprávnění. Tyto aplikace pak může použít <strong>System.Web.HttpUtility</strong>, napíše <strong>System.Web.ClientServices.\< / strong > * obory názvů a typy související s členství, role a profily. Volání těchto typů z aplikace s částečnou důvěryhodností mimo Web je již nejsou podporovány v rozhraní .NET Framework 4.

> [!NOTE]
> **HtmlEncode** a **HtmlDecode** funkce **System.Web.HttpUtility** třídy se přesunul do nového rozhraní .NET Framework 4  **System.Net.WebUtility** třídy. Pokud jste pouze funkcí technologie ASP.NET, který se používal, upravit kód aplikace k používání nového **WebUtility** namísto třídy.


Tady je shrnutí změn pro výchozí implementace certifikačních Autorit v technologii ASP.NET 4:

- Domény aplikace technologie ASP.NET jsou nyní homogenní aplikační domény. V doméně aplikace jsou k dispozici pouze sady udělení částečným vztahem důvěryhodnosti a úplného vztahu důvěryhodnosti.
- Sady udělení částečným vztahem důvěryhodnosti ASP.NET jsou nezávislá na libovolné úrovni rozlehlé sítě, počítače nebo uživatele zásad CAS.
- Sestavení technologie ASP.NET, která poskytuje 3.5 a 3.5 SP1 se převedly tak použít model transparentnosti rozhraní .NET Framework 4.
- Použití technologie ASP.NET **AspNetHostingPermission** podstatně snížit atribut. Největším množstvím instancí tohoto atributu se odstranily z veřejných rozhraní API technologie ASP.NET.
- Dynamicky kompilovaných sestavení, které jsou vytvořeny pomocí poskytovatelé sestavení ASP.NET byly aktualizovány explicitně označit sestavení jako průhledná.
- Všechna sestavení ASP.NET jsou nyní označeny tak, že je atribut APTCA zachované pouze webové hostitelské prostředí. Hostitelská prostředí částečně důvěryhodných mimo Web jako ClickOnce nebude možné provést volání do sestavení ASP.NET.

Další informace o modelu zabezpečení přístupu kódu novou technologii ASP.NET 4 najdete v tématu [pomocí zabezpečení přístupu kódu v aplikacích ASP.NET](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) na webové stránce MSDN.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>Byly přesunuty MembershipUser a další typy v System.Web.Security Namespace

Některé typy, které se používají v členství technologie ASP.NET se přesunuly z `System.Web.dll` do nového sestavení System.Web.ApplicationServices.dll. Aby bylo možné vyřešit závislosti architektury vrstvení mezi typy v klientovi a SKU rozšířené rozhraní .NET Framework se přesunuly typy.

Webové projekty nemají problémy v důsledku přesunutí těchto typů, protože System.Web.ApplicationServices.dll byl přidán do seznamu odkazovaných sestavení, který se používá ve výchozím nastavení systém kompilace technologie ASP.NET. Pokud provádíte upgrade projektu webové stránky, která byla vytvořena pomocí starší verze technologie ASP.NET na ASP.NET 4 tak, že otevřete v sadě Visual Studio 2010, se projekt zkompiluje bez chyb.

Podobně pokud upgradujete projekt webové aplikace, který byl vytvořen v dřívější verzi technologie ASP.NET na ASP.NET 4 tak, že otevřete v sadě Visual Studio 2010, proces upgradu přidá odkaz na System.Web.ApplicationServices.dll do projektu. Proto upgrade webové projekty aplikací také zkompiluje bez chyb.

Kompilované (binární) soubory, které byly vytvořeny pomocí starší verze technologie ASP.NET se také spustí bez chyb na technologii ASP.NET 4, přestože typy členství se přesunuly na jiné sestavení. Informace o posloupnosti typů je přidaný do verze technologie ASP.NET 4 `System.Web.dll` odkazy za běhu pro tyto typy, které automaticky směruje do nového umístění pro typy.

Knihovny tříd, které používají typy konkrétní členství a upgradu ze starší verze technologie ASP.NET se však nezdaří kompilace při použití v projektu aplikace ASP.NET 4. Projekt knihovny tříd může například selhat ke kompilaci a ohlaste chybu, například následující:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Tento problém můžete vyřešit přidáním odkazu v projektu knihovny tříd System.Web.ApplicationServices.dll.

Následující seznam obsahuje *System.Web.Security* typy, které byly přesunuty z `System.Web.dll` soubor System.Web.ApplicationServices.dll:

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

## <a name="output-caching-changes-to-vary--http-header"></a>Výstup ukládání do mezipaměti změny se liší \* hlavičky protokolu HTTP

V technologii ASP.NET 1.0 chybu způsobila stránky v mezipaměti, zadané `Location="ServerAndClient"` jako nastavení výstupní mezipaměti a vygenerovat `Vary:*` hlavičku protokolu HTTP v odpovědi. To mělo vliv na sděluje prohlížeči klientů pro ukládání do mezipaměti nikdy stránce místně.

V technologii ASP.NET 1.1 **System.Web.HttpCachePolicy.SetOmitVaryStar** byla přidána metoda, která lze volat pro potlačení `Vary:*` záhlaví. Tato metoda byla vybrána, protože změna emitovaný hlavičku protokolu HTTP byla považována za potenciálně rozbíjející změny v době. Ale vývojáři mají bylo matoucí chování v technologii ASP.NET a zpráv o chybách naznačují, že vývojáři si nejsou vědomi existující **SetOmitVaryStar** chování.

V technologii ASP.NET 4 Společnost se rozhodla vyřešit problém nevyřeší. `Vary:*` Hlavičky protokolu HTTP je již vygenerován z odpovědí, které určují následující direktivy:

`<%@OutputCache Location="ServerAndClient" %>`

V důsledku toho **SetOmitVaryStar** už je nepotřebujete k potlačení `Vary:*` záhlaví.

V aplikacích, které určují `Location="ServerAndClient"` v **@ OutputCache** direktiv na stránku, uvidíte teď chování odvozené od názvu **umístění** hodnotu atributu – to je, budou stránky ukládat do mezipaměti v prohlížeči bez nutnosti volání **SetOmitVaryStar** metody.

Pokud stránky v aplikaci musíte vygenerovat `Vary:*`, zavolejte **AppendHeader** metody, jako v následujícím příkladu:

`HttpResponse.AppendHeader("Vary","*");`

Alternativně můžete změnit hodnotu ukládání výstupu do mezipaměti **umístění** atribut "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Typy System.Web.Security pro Passportu jsou zastaralé

Podpora služby Passport, integrované do ASP.NET 2.0 je už zastaralá a nepodporované několika let z důvodu změn v účtu služby Passport (nyní LiveID). V důsledku toho pět typů související se Passport v **System.Web.Security** jsou nyní označeny **ObsoleteAttribute** atribut.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>Vlastnost MenuItem.PopOutImageUrl selže, aby se vykreslil obraz v technologii ASP.NET 4

V technologii ASP.NET 3.5 *MenuItem.PopOutImageUrl* vlastnost umožňuje zadat adresu URL pro bitovou kopii, která se zobrazí v položce nabídky k označení, že položka nabídky obsahuje podnabídku dynamické. Následující příklad ukazuje, jak tuto vlastnost zadat v kódu v technologii ASP.NET 3.5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

V důsledku změny návrhu v technologii ASP.NET 4, je vykreslen žádný výstup pro *PopOutImageUrl* Pokud je nastavena pro *MenuItem* třídy. Místo toho musíte zadat adresu URL obrázku přímo v *nabídky* ovládat buď pomocí *StaticPopOutImageUrl* vlastnost nebo *DynamicPopOutImageUrl* vlastnost. Při práci s statickou nabídku *Menu.StaticPopOutImageUrl* vlastnost určuje adresu URL obrázku, který se zobrazí indikaci, že statickou položkou nabídky obsahuje podnabídku, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Pokud pracujete s dynamickou nabídku, můžete použít *Menu.DynamicPopOutImageUrl* vlastnosti a určit adresu URL pro bitovou kopii, která označuje, že má dynamické položky nabídky podnabídky. Následující příklad je podobný předchozímu, ale ukazuje, jak nastavit *DynamicPopOutImageUrl* vlastnost pro dynamickou nabídku.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Pokud *Menu.DynamicPopOutImageUrl* není nastavena vlastnost a *Menu.DynamicEnableDefaultPopOutImage* je nastavena na *false*, zobrazí se žádné image. Podobně pokud *StaticPopOutImageUrl* není nastavena vlastnost a *StaticEnableDefaultPopOutImage* je nastavena na *false*, zobrazen žádný obrázek.

Když nastavíte cesty pro tyto vlastnosti, použijte lomítkem (/) namísto zpětné lomítko (\). Další informace najdete v tématu [Menu.StaticPopOutImageUrl a Menu.DynamicPopOutImageUrl se nepodařilo vykreslit obrázky při cesty obsahovat obrácená lomítka](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.") jinde v tomto dokumentu.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl a navrátit služby po Menu.DynamicPopOutImageUrl k vykreslování obrázků, když cesty obsahovat obrácená lomítka

V technologii ASP.NET 4, obrázků, které zadáte pomocí *Menu.StaticPopOutImageUrl* a *Menu.DynamicPopOutImageUrl* vlastnosti se nezobrazí, pokud cesta obsahuje backlashes (\). Jedná se o změnu z předchozích verzí technologie ASP.NET.

Následující příklad *nabídky* zobrazí značky *StaticPopOutImageUrl* nastavenou pomocí cesty, které obsahuje zpětné lomítko. V technologii ASP.NET 4 nebude vykreslovat obrázek určený ve vlastnosti.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Chcete-li tento problém vyřešit, změňte hodnoty cest, které jsou určené v *StaticPopOutImageUrl* a *DynamicPopOutImageUrl* vlastnosti, které chcete používat lomítka (/). Následující příklad ukazuje tuto změnu:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Všimněte si, že aplikace, které se migrovaly z předchozích verzí technologie ASP.NET na ASP.NET 4 může také být ovlivněno, protože *MenuItem.PopOutImageUrl* změně vlastnosti. Další informace najdete v tématu [vlastnost MenuItem.PopOutImageUrl selže aby se vykreslil obraz v technologii ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") jinde v tomto dokumentu.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Právní omezení

Toto je předběžná dokumentu a může podstatně změnit před finální komerční verzi softwaru, které jsou popsány zde.

Informace obsažené v tomto dokumentu představují aktuální pohled společnosti Microsoft Corporation na tyto problémy k datu publikování. Protože Microsoft reagovat na měnící se podmínky na trhu, neměly by být vykládány jako závazek Microsoftu a společnost Microsoft nemůže zaručit přesnost jakýchkoli informací, které jsou prezentovány po provedení datu publikování.

Tento dokument White Paper se pouze k informačním účelům. MICROSOFT NEPOSKYTUJE ŽÁDNÉ ZÁRUKY, VÝSLOVNÝCH, ODVOZENÝCH NEBO ZÁKONNÝCH, INFORMACE V TOMTO DOKUMENTU.

V souladu s příslušnými zákony o autorských právech je zodpovědný uživatel. Bez omezení autorská práva, žádná část tohoto dokumentu může být reprodukovat, uložené v zavedené rozšiřován nebo jakýmkoli způsobem (electronic, mechanickým, mechanicky, záznam nebo jinak) nebo pro libovolný účel, bez výslovného písemného povolení společnosti Microsoft Corporation.

Společnost Microsoft může vlastnit patenty, patentové přihlášky, ochranné známky, autorská práva nebo další práva na duševní vlastnictví přihlášky v tomto dokumentu. Výslovně uvedeno v písemné licenční smlouvě se společností Microsoft, poskytnutím tohoto dokumentu vám není udělena licence k těmto patentům, ochranné známky, autorská práva nebo jinému duševnímu vlastnictví.

Pokud není uvedeno jinak, společnosti, organizace, produkty, názvy domén, e-mailové adresy, loga, osoby, místa a události použité v ukázkách jsou smyšlené. proto žádné jejich spojení se žádné skutečnou společností, organizací, produktu, název domény, e-mailu adresy, loga, osoby, místa nebo událostí je určena ji vyvozovat.

© Microsoft 2010 Corporation. Všechna práva vyhrazena.

Microsoft a Windows jsou registrované ochranné známky nebo ochranné známky společnosti Microsoft Corporation ve Spojených státech amerických a dalších zemích.

Názvy skutečných společností a produktů mohou být ochrannými známkami příslušných vlastníků.
