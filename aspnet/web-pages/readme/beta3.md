---
uid: web-pages/readme/beta3
title: Webový matice a ASP.NET Web Pages (Razor) Beta 3 verze Readme | Microsoft Docs
author: rick-anderson
description: Web Matrix a rozhraní ASP.NET Web Pages (Razor) Beta 3 verze Readme
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5ef7a6f44758cf94fc19d6fbab3cc4b7bce8e8e5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Web Matrix a rozhraní ASP.NET Web Pages (Razor) Beta 3 verze Readme
====================
> Web Matrix a rozhraní ASP.NET Web Pages (Razor) Beta 3 verze Readme

9 listopadu 2010

## <a name="contents"></a>Obsah

- [Přehled](#Overview)
- [Instalace](#Installation_Notes)
- [Nové funkce, změny a známé problémy ve verzi Beta 3](#Known_Issues)

    - [Problémy instalace služby WebMatrix](#Known_Issues_Installation)
    - [Webové stránky ASP.NET](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Instalace aplikace](#Known_Issues_Installing_Applications)
    - [Publikování aplikací](#Known_Issues_Publishing_Applications)
    - [Další problémy](#Known_Issues_Other_Issues)
- [Další informace](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Přehled

> Microsoft WebMatrix Beta je bezplatných webových zásobníku vývoj, který nainstaluje v minutách. Webový server se integruje s databáze a programovací rozhraní pro vytvoření jednoho integrovaného rozhraní. Beta verze služby WebMatrix můžete zjednodušit kód, testování a publikování vlastní web ASP.NET a PHP způsob nebo používáte službu WebMatrix Beta ke spuštění nového webu pomocí Oblíbené open-source aplikace, jako je aplikace DotNetNuke, Umbraco, WordPress nebo Joomla. Beta verze služby WebMatrix používá stejný výkonný webový server, databázový stroj a prostředí architektury, které se spustí svůj web na Internetu, které umožňuje přechod z vývojového do produkčního prostředí hladký a bezproblémový.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalace

> Chcete-li nainstalovat službu WebMatrix Beta 3, použijete [Microsoft webové platformy verze 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Po instalaci webové platformy, můžete k instalaci služby WebMatrix Beta 3.
> 
> Pokud máte potíže s během instalace, podívejte se na [odstraňování potíží s instalačního programu webové platformy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Pokyny k publikování aplikací

> V tématu [podrobné pokyny k publikování aplikací](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Nové funkce, změny, andKnown problémy

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Instalace služby WebMatrix Beta 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problém: WebMatrix Beta 3 je k dispozici pouze na platformách, které podporují rozhraní Microsoft .NET Framework 4

> Rozhraní .NET Framework verze 4 je vyžadována pro beta verzi služby WebMatrix. V některých případech instalační program beta verzi služby WebMatrix vám umožní instalaci na platformu, která není součástí sady podporované konfigurace. Konkrétně Windows Vista bez aktualizace SP1 vám umožní zahájit instalaci služby WebMatrix Beta, ale součásti rozhraní .NET Framework 4 se nezdaří a blokovat instalaci.
> 
> **Alternativní řešení**  
> Nainstalujte na podporované platformě, která zahrnuje:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 nebo novější
> - Windows XP SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problém: Nelze nainstalovat službu WebMatrix Beta 3, když se nainstaluje Microsoft Visual Studio 2008 bez Microsoft Visual Studio 2008 SP1

> **Alternativní řešení**  
> Nainstalujte [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) z webu Microsoft Download Center.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problém: V mezipaměti GAC nejsou nainstalovány některé sestavení pro SQL Server Compact 4.0

> Spravovaná sestavení pro SQL Server Compact 4.0 nejsou umístěny v globální mezipaměti sestavení (GAC), při instalaci systému SQL Server Compact 4.0 na 64bitovém počítači a má počítač pouze profil rozhraní .NET Framework 3.5 SP1 klient nainstalován. Spravovaná sestavení, které nejsou nainstalované v mezipaměti GAC jsou:
> 
> - *System.Data.SqlServerCe.dll* (ADO.NET provider)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )
> 
> **Alternativní řešení**  
> Odinstalujte systém SQL Server Compact 4.0. Stáhněte a nainstalujte plnou verzi rozhraní .NET Framework 3.5 SP1 z následujícího umístění:  
>   
> [Microsoft .NET Framework 3.5 Service pack 1 (úplná balíček)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Znovu nainstalujte SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problém: Nelze odinstalovat, SQL Server Compact pomocí příkazového řádku

> Odinstalace systému SQL Server Compact pomocí možnosti příkazového řádku v této verzi nefunguje.
> 
> **Alternativní řešení**  
> Použití *programy a funkce* v Ovládacích panelech odinstalujte Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET – webové stránky

Tato část dokumentu popisuje nové funkce, změny a známé problémy ve verzi Beta 3 z webových stránek ASP.NET se syntaxí Razor.

- [Nové funkce](#NewFeatures)
- [Změny](#Changes)
- [Problémy](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Nové funkce ve verzi Beta 3 pro rozhraní ASP.NET Web Pages se syntaxí Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Nové: "Html.Raw" metoda vykresluje nezakódovaný kód

> Nové `Html.Raw` metoda umožňuje vykreslení kódu HTML jako značka namísto vykreslování kódovaného výstupu. (Ve výchozím nastavení, ASP.NET Razor kóduje řetězce před jejich vykreslení.) Syntaxe je následující:
> 
> `Html.Raw(value)`
> 
> Následující příklad ukazuje, jak používat `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Změny ve verzi Beta 3 pro rozhraní ASP.NET Web Pages se syntaxí Razor

#### <a name="change-hrefattribute-method-removed"></a>Změna: Metody "HrefAttribute" Odebrat

> `HrefAttribute` Metodu `WebPage` třída byla odebrána. Tato pomocná byl použit ke kódování nebezpečné znaky v adresách URL. Se už nevyžaduje protože ASP.NET Razor automaticky kóduje řetězce. (Pomocí nové `Html.Raw` metoda k vykreslení nekódovaného řetězce.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Změnu: Syntaxe deklarativní "@helper" Pomocníci změnit

> Ve verzi Beta 3, ASP.NET změní, jak ho analyzuje pomocné rutiny, které jsou vytvořené pomocí `@helper` syntaxe. V podstatě `@helper` syntaxe je nyní analyzovat jako blok kódu místo jako blok kódu, který může obsahovat kód. Proto kód do pomocné rutiny nemusí být uzavřená do `@{ }` bloky. Naopak značky do pomocné rutiny musí být explicitně zahrnuta do elementů HTML nebo v prostředí ASP.NET Razor `<text></text>` značky.
> 
> Například následující `@helper` syntaxe funguje ve verzi Beta 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> Ve verzi Beta 3 je třeba změnit tohoto pomocníka, aby vypadala jako v následujícím příkladu:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Všimněte si, že `@{ }` znaků kolem počáteční kód v pomocné rutiny se už používá. Je to proto, že obsah pomocníky jsou považovány za blok kódu ve výchozím nastavení. Pomocné rutiny vykreslí značku, která začíná otevřením `<a>` značky. Pokud musí pomocné rutiny vykreslení jako prostý text ani značky, které neobsahují ukončovací značky (například `<meta>` značky), obsah k vykreslení musí být v `<text></text>` značky.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Změn: Odebrat "WebPageContext.HttpContext"

> `WebPageContext.HttpContext` Vlastnost byla odebrána. Místo nich se používá `HttpContext.Current`. ( `WebPageContext.HttpContext` Tato vlastnost jednoduše zabalit.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Změna: Pomocník "Facebook" přesunout do nového balíčku

> `Facebook` Pomocné rutiny, byl přesunut do *Facebook.Helper* knihovny, která zahrnuje `Facebook` pomocné rutiny a další funkce. Je třeba nainstalovat tuto knihovnu jako samostatný balíček, jak je popsáno v "Instalace pomocné rutiny s balíček Manager" v tomto kurzu [Začínáme s ASP.NET stránky](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Změna: Typy členství, Role a zabezpečení přesune do nového sestavení

> Následující typy byly přesunuty do `WebMatrix.WebData` sestavení:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Změna: Třída "TagBuilder" přesunout do System.Web.WebPages.dll sestavení

> `TagBuilde` r třída byl přesunut do System.Web.WebPages.dll sestavení. Dřív šlo v sestavení, které bylo součástí ASP.NET MVC. Tato změna znamená, že nemáte-li používat nainstalujte rozhraní ASP.NET MVC `TagBuilder` třídy.
> 
> Třída je však stále v `System.Web.Mvc` oboru názvů. Chcete-li použít `TagBuilder` – třída (například v vlastní ASP.NET Razor pomocníka), musí odkazovat obor názvů (například přidáním `@using System.Web.Mvc` kódu).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Změnu: Žádost o ověření syntaxe změnit; Třída "Ověření" Odebrat

> Ve verzi Beta 3 Pokud chcete zakázat ověřování pro jednotlivá pole nebo sady polí, můžete zavolat `Validation.Exclude` metody předávání v název nebo názvy polí, které chcete vyloučit z ověření. Nové syntaxe je k dispozici ve verzi Beta 3 pro vynechání ověřování. `Validation` Metodu použitou v Beta 3 byla odebrána.
> 
> > [!NOTE]
> > Pokud není zakážete ověření požadavku, pokud uživatelé se pokusí odeslat kód HTML (například pomocí editoru formátovaného textu na stránce), web zobrazovat chyby jako *hodnotu potenciálně nebezpečná Request.Form byla zjištěna z klienta*a uživatelský vstup nebyla přijata. Pokud zakážete ověření požadavku, je nutné ručně zkontrolovat vstup uživatele, abyste měli jistotu, že ho neobsahuje potenciálně nebezpečného kódu nebo skriptu pomocí něco podobného jako [skriptování V4.0 knihovny Microsoft proti mezi lokality](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Chcete-li zakázat automatické ověření, zavolejte `Request.Unvalidated` metoda, předejte název pole nebo jiného objektu post, které chcete vynechat ověření žádosti pro. Tuto metodu můžete použít k ověření pro všechny položky v obejít `Form`, `QueryString`, `Cookies`, a `ServerVariables` kolekce. Následující příklady ukazují, jak používat `Unvalidated` metoda:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Známé problémy pro ASP.NET Web Pages se syntaxí Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problém: Neočekávané chování při použití vlastní uživatelská tabulka pro členství

> K chybě při inicializaci zprostředkovatele členství pro web ASP.NET Razor, zavoláte `WebSecurity.InitializeDatabaseConnection` metoda. (Ve službě WebMatrix, zahrnuje šabloně Starter Site volání této metody v  *\_AppStart.cshtml* souboru.) Pokud `autoCreateTables` parametr této metody je nastaven na hodnotu true (ve výchozím nastavení je nastavena na hodnotu true v šabloně Starter Site), a pokud je název tabulky nerozpoznané předaný metodě (druhý parametr), metoda nevyvolá chybu. Místo toho automaticky vytvoří v tabulce.
> 
> To může být problém, pokud máte v úmyslu použít vlastní uživatelská tabulka pro členství, ale předat název nesprávné tabulky k `WebSecurity.InitializeDatabaseConnection` metoda. Vzhledem k tomu, že metoda není ve výchozím nastavení vyvolá chybu, pokud neexistuje v tabulce, které zadáte, a protože místo toho vytvoří novou tabulku, můžete aplikaci pravděpodobně fungovat. Aplikační kód, který závisí na vlastní uživatelská tabulka (a na pole v něm) můžete nakonec nezdaří s neočekávaným chybám.
> 
> **Alternativní řešení**  
> Ujistěte se, že název předaná `InitializeDatabaseConnection` metoda odpovídá uživatelský profil tabulky v databázi členství nebo zkontrolujte, zda `autoCreateTables` parametr je nastaven na hodnotu false.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problém: "Nepodařilo se vygenerovat uživatelskou instanci systému SQL Server" Chyba

> Pokud služba WebMatrix webové aplikace používá systém SQL Server Express a je spuštěna služba IIS 7.5 na Windows 7 nebo Windows Server 2008 R2, může se zobrazit chybu, která určuje, že systém SQL Server nelze načíst cestu k místní aplikace uživatele za běhu.
> 
> **Alternativní řešení** Ujistěte se, že účet systému Windows, na kterém se aplikace spouští pod (obvykle síťová služba) má oprávnění ke čtení/zápisu pro kořenové složky aplikace a podsložky jako *aplikace\_dat*. Podrobnější informace najdete v článku znalostní báze Knowledge Base [problémy s SQL Server Express uživatele vytváření instancí a projekty webových aplikací ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problém: V sadě Visual Studio, obory názvů pro vlastní sestavení (DLL) nejsou importovány automaticky

> Pokud používáte vlastní sestavení v projektu v sadě Visual Studio, obory názvů v těchto sestavení deklarována nejsou importovány automaticky v době návrhu. Odkazy na vlastní typy v důsledku toho nemusí rozpoznat v době návrhu a jsou označeny jako není platný v sadě Visual Studio (pomocí "vlnovku"). K tomuto problému dochází pouze v době návrhu v sadě Visual Studio; vlastní aplikace běží správně.
> 
> **Alternativní řešení**  
> Zahrnout `using` – příkaz (`imports` v jazyce Visual Basic), odkazuje na entity, které nejsou rozpoznány v době návrhu.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problém: Visual Studio IntelliSense a projekt šablony k dispozici pouze v architektuře ASP.NET MVC verze 3

> Instalace technologie ASP.NET Web Pages také nenainstaluje nástroje pro sadu Visual Studio jako IntelliSense a projekt šablony pro aplikace ASP.NET Web Pages.
> 
> **Alternativní řešení** používáte IntelliSense a projekt šablony pro aplikace webových stránek ASP.NET v sadě Visual Studio, nainstalujte technologii ASP.NET MVC 3 RC buď prostřednictvím instalace webové platformy nebo [samostatný instalační program](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problém: "&lt;pomocná&gt; třída nebyla nalezena" Chyba

> Po upgradu na verzi Beta 3, může se zobrazit chyba, pomocnou třídu (například `Facebook` třída) nelze najít. Od verze Beta 2 a budete pokračovat ve verzi Beta 3, byl přesunut pomocné rutiny do balíčků, které je třeba explicitně nainstalovat. Existující lokality nejsou upgradovány na zahrnují tyto balíčky; To zahrnuje lokality v *\My Documents\IISExpress* nebo *\My Documents\My weby* složek. Konkrétně se zobrazí tato chyba, pokud použijete výchozí web v *osobní weby* (web1), který obsahuje odkaz na `Twitter` pomocné rutiny.
> 
> **Alternativní řešení**  
> Komentář volání žádné pomocné rutiny v lokalitě, spusťte  *\_správce* stránky a nainstalujte balíček nebo balíčky, které zahrnují pomocné rutiny, které chcete použít. Po instalaci balíčku, zrušte komentář u řádky, které odkazují pomocné rutiny.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problém: Nasazení Beta 3 ASP.NET Razor sestavení do složky Bin nemusí fungovat na hostování webů

> Pokud nasadíte webu technologie ASP.NET Web Pages na hostitelském serveru, a Pokud nasazujete ASP.NET Razor Beta 3 sestavení do lokality *Bin* složky, může docházet k chybám, včetně následujících:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> To může dojít, pokud poskytovatel hostingu nainstaloval ASP.NET Web Pages Beta 1 sestavení do serveru globální mezipaměti (sestavení GAC). Sestavení v mezipaměti GAC získat přednost přes nainstalovány místně v sestavení *Bin* složky.
> 
> **Alternativní řešení** obraťte se na svého poskytovatele hostingu, abyste se přesvědčili, že se zobrazuje chyby z důvodu konfliktu mezi verzemi poskytovatele sestavení a vaše. Pokud ano, požadavku, že poskytovatel hostingu aktualizovat sestavení v globální mezipaměti serveru.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problém: Čtení informačních kanálů nebo jiné externí data prostřednictvím serveru proxy

> Serveru se systémem lokality je za proxy serverem, může být potřeba konfigurovat informace o proxy serveru v *Web.config* souboru, aby mohl číst informace, které pochází z mimo váš web. Například pokud použijete `ReCaptcha` pomocné rutiny, pomocné rutiny komunikuje se službou nástroje reCAPTCHA, ale mohou být blokovány proxy serveru. Podobně informační kanály, které se používají na webových stránkách ASP.NET, jako je například používá Správce balíčků informačního kanálu může vyžadovat konfiguraci proxy serveru.
> 
> Pokud máte potíže v práci s externí službu nebo práce s tímto balíčkem kanálu, uveďte následující prvky do vaší aplikace kořenové *Web.config* souboru:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Další informace o konfiguraci proxy serveru najdete v tématu [ &lt;proxy&gt; – Element (nastavení sítě)](https://msdn.microsoft.com/library/sa91de1e.aspx) na webu MSDN.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problém: "Nelze načíst Microsoft.Web.Infrastructure.dll" Chyba

> Pokud jste dříve nainstalovali verzi Beta 1 z webových stránek ASP.NET se syntaxí Razor a poté nainstalujte verzi Beta 3, nainstalují se všechny odpovídající sestavení v mezipaměti GAC s výjimkou *Microsoft.Web.Infrastructure.dll*. V důsledku toho, když spustíte stránky ASP.NET Razor, zobrazí chybu, která označuje, že *Microsoft.Web.Infrastructure.dll* nelze načíst.
> 
> Tento problém neproběhne-li načíst verzi Beta 3 na čistém počítači.
> 
> **Alternativní řešení**  
> V Ovládacích panelech odinstalujte rozhraní ASP.NET Web Pages. Znovu nainstalujte verzi Beta 3.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problém: Odinstalace rozhraní .NET Framework verze 4 zakáže technologie ASP.NET Web Pages se syntaxí Razor

> Pokud odinstalujete rozhraní .NET Framework verze 4 a znovu ji nainstalovat, technologie ASP.NET Web Pages se syntaxí Razor je zakázané. Stránky s *.cshtml* rozšíření se nespustí správně. Rozhraní ASP.NET Web Pages zaregistruje sestavení v kořenu počítač *Web.config* soubor odebrán souboru a odebrání rozhraní .NET Framework. Opětovné instalace rozhraní .NET Framework nainstaluje novou verzi konfiguračního souboru, ale nepřidá odkaz na sestavení pro ASP.NET Web Pages.
> 
> **Alternativní řešení** po opětovné instalaci rozhraní .NET Framework, přeinstalujte rozhraní ASP.NET Web Pages se syntaxí Razor. Tím se přidá následující elementu, který chcete *Web.config* soubor v kořenu počítač, který je obvykle v následujícím umístění:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problém: Aplikace dříve nasazené s ASP.NET sestavení do složky Bin dojde k chybě

> Během nasazování kopií sestavení rozhraní ASP.NET Web Pages (například *Microsoft.WebPages.dll*) k *Bin* složky webu na serveru. (Tomu může dojít automaticky během nasazení nebo proto, že vývojář explicitně zkopírovat sestavení.) Ale pokud je nainstalován ve verzi Beta 3, chyby dochází k, jako je například chyby, které nebyly nalezeny některé typy. K tomu dochází, protože několik typů webových stránek ASP.NET se přesunuly do různých oborech názvů pro verzi Beta 3.
> 
> **Alternativní řešení**   
> Vymazat *Bin* složku nasazené aplikace, zkopírujte nové sestavení do složky (nebo znovu nasadit aplikace) a restartujte aplikaci.


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problém: Adresy URL bez přípony nenašli.cshtml/.vbhtml soubory na službě IIS 7 nebo IIS 7.5

> Na službě IIS 7 nebo IIS 7.5, nejsou schopna najít stránky, které mají požadavky s adresou URL takto *.cshtml* nebo *.vbhtml* rozšíření:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Tento problém nastane, protože přepisování adres URL není povoleno ve výchozím nastavení pro službu IIS 7 nebo IIS 7.5. Nejpravděpodobnějším scénář je, že se nezobrazí problém při testování místně pomocí služby IIS Express, ale dojde při nasazení webu k hostování webu.
> 
> **Alternativní řešení**
> 
> - Pokud budete mít kontrolu nad počítači serveru, na počítači serveru nainstalujte aktualizaci popsanou v [aktualizace je dostupná, že umožňuje určité služby IIS 7.0 nebo IIS 7.5 obslužné rutiny pro zpracování požadavků, jejichž adresy URL na konci období](https://support.microsoft.com/kb/980368).
> - Pokud nemáte kontrolu nad počítači serveru (například nasazujete k hostování webu), přidejte následující na web *Web.config* souboru:
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problém: Pomocí projektu webové aplikace nebo stránky ASP.NET MVC a ASP.NET Web ve stejné aplikaci

> Pokud v projektu webové aplikace nebo aplikace ASP.NET MVC byly pomocí webových stránek ASP.NET, může se zobrazit chyba, *WebPageHttpApplication* nebyl nalezen.
> 
> **Alternativní řešení**  
> Pokud se tato chyba, změňte základní třídu, ze kterého aplikace pochází. V *Global.asax* souboru, změňte následující řádek:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> k tomuto:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> To platí obrátí změnu, která byla zavedena ve verzi Beta 1 z webových stránek ASP.NET se syntaxí Razor.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problém: Nasazení aplikace do počítače, který nemá systém SQL Server Compact nainstalován

> Aplikace, které obsahují systém SQL Server Compact databáze můžete spustit na počítači, kde systém SQL Server Compact není nainstalovaná. Microsoft WebMatrix Beta 3 automaticky zkopíruje tyto binární soubory pro vás a provede odpovídající *Web.config* souboru transformace.
> 
> **Alternativní řešení** Pokud potřebujete zkopírujte tyto soubory a ujistěte se, *Web.config* změny souborů ručně, postupujte takto:
> 
> 1. Zkopírujte sestavení modulu databáze, které chcete *Bin* složky (a její podsložky) aplikace na cílovém počítači: 
> 
>     - Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*
>     - Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **to** *\Bin\x86*
>     - Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **to** *\Bin\amd64*
> 2. V kořenové složce webové stránky, vytvořit nebo otevřít *Web.config* souboru. (Ve službě WebMatrix Beta 3, je k dispozici, pokud kliknete na tento typ souboru **všechny** v **vyberte typ souboru** dialogové okno.)
> 3. Přidejte následující prvek jako podřízenou **&lt;konfigurace&gt;** – element (mimo **&lt;system.web&gt;** element):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problém: Databáze a WebGrid Pomocníci nefungují na úrovni Medium Trust v jazyce Visual Basic

> Pokud používáte Visual Basic (vytváření *.vbhtml* soubory), `Database` a `WebGrid` Pomocníci nebude fungovat, pokud je aplikace nastavena používat úrovni Medium Trust.
> 
> **Alternativní řešení**  
> Dočasně nastavte, že aplikace pro používání úplný vztah důvěryhodnosti.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problém: Vlastnost "Šifrovat" nebyl rozpoznán.

> SQL Server Compact 4.0 nerozpoznává `Encrypt` vlastnost `SqlCeConnection` třídy. Tato vlastnost byste neměli používat k šifrování souborů databáze. `Encrypt` Vlastnost byla zastaralé v systému SQL Server Compact 3.5 verzi a bylo zachováno pouze z důvodů zpětné kompatibility. 
> 
> **Alternativní řešení**  
> Použití `Encryption Mode` vlastnost `SqlCeConnection` třída k zašifrování souborů databáze systému SQL Server Compact 4.0. Následující příklad ukazuje, jak vytvořit k šifrované SQL Server Compact 4.0 databáze pomocí `Encryption Mode` vlastnost:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Chcete-li změnit režim šifrování existující databázi SQL Server Compact 4.0, postupujte takto:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> K šifrování nezašifrované databázi systému SQL Server Compact 4.0, postupujte takto:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problém: Microsoft Visual C++ 2008 modulu runtime knihoven jsou požadovány

> Nativní knihovny DLL systému SQL Server Compact 4.0 potřebovat Microsoft Visual C++ 2008 modulu Runtime knihoven (x 86, IA64 a x 64), aktualizace Service Pack 1.
> 
> **Alternativní řešení**  
> Nainstalujte rozhraní .NET Framework 3.5 SP1. Tím se nainstaluje taky Visual C++ 2008 modulu Runtime knihoven SP1. Knihovny můžete stáhnout z následujícího umístění:   
>   
> [Aktualizaci zabezpečení pro sadu Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Všimněte si, že instalace rozhraní .NET Framework 2.0, 3.0, nebo nemá 4 *není* nainstalovat Visual C++ 2008 modulu Runtime knihoven SP1.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problém: Pokud je systém SQL Server Compact nainstalovaný před instalací rozhraní .NET Framework na počítači, jeho neutrální název zprostředkovatele není registrován v souboru machine.config rozhraní .NET Framework

> Na počítači, který nemá rozhraní .NET Framework nainstalovat, protože systém SQL Server Compact vyžadují rozhraní .NET framework lze nainstalovat systém SQL Server Compact. Pokud ani rozhraní .NET Framework verze 3.5 ani 4 je nainstalována před instalací systému SQL Server Compact, nastavení SQL Server Compact nezaregistruje neutrální název zprostředkovatele v jeho *machine.config* souboru. Všechny aplikace, které jsou závislé na položce SQL Server Compact v *machine.config* souboru se nezdaří. Neutrální název položka registrace v *machine.config* vypadá jako v následujícím příkladu:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Alternativní řešení**  
> Odinstalujte SQL Server Compact 4.0 CTP1. Stáhněte a nainstalujte plné verze rozhraní .NET Framework z následujícího umístění:
> 
> [Microsoft .NET Framework 3.5 Service pack 1 (úplná balíček)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Verze rozhraní Microsoft .NET Framework 4.0 (úplná balíček)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Potom přeinstalujte [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Instalace aplikace

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problém: Instalace aplikace může trvat dlouhou dobu. Pokud je přesměrování složky Dokumenty uživatele do sdílené síťové složky

> **Alternativní řešení**  
> Žádné Aplikace může trvat, než k instalaci, ale nainstaluje správně.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Publikování aplikací

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problém: Lokality nemusí fungovat po publikování Pokud pole "Cílová adresa URL" není s předponou http:// nebo https://.

> V **nastavení publikování** dialogové okno, pokud cílová adresa URL začíná `http://` nebo `https://`, web nemusí fungovat po nasazení.
> 
> **Alternativní řešení**  
> Ujistěte se, že před publikováním serveru, cílovou adresu URL v **nastavení publikování** dialogové okno začíná `http://` nebo `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problém: Publikování databáze MySQL selže s chybou "se nepodařilo publikovat databázi. To může dojít, pokud Vzdálená databáze nemůže spustit skript."

> Chyba může mít několik důvodů. Jedním z důvodů, že se zobrazí tato chyba je pokud databázového skriptu obsahuje znak, jeden uvozovky (') a databáze MySQL cílové výchozí znakovou sadu není na UTF-8.
> 
> **Alternativní řešení**  
> Nastavte výchozí znakovou sadu pro vzdálené databáze MySQL na UTF-8.


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Další problémy

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problém: Hledání nebo Filtr nefunguje v sestavách Seskupit podle: typ problému

> Při spuštění sestavy pro lokalitu, pokud zadáte text v *filtrovat podle adresy URL* pole a klikněte na tlačítko *vyhledávání*, nedojde k žádné akci. Důvodem je, že tento ovládací prvek není funkční i *Group By* je nastavena do stavu sestavy *typ problému*, který je výchozí.
> 
> **Alternativní řešení** v *Group By* karta pásu karet klikněte na tlačítko *URL* k seskupení položek podle jejich adresu URL zdroje. Textové pole a tlačítko Filtrovat položky jsou funkční i v tomto stavu.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problém: Aplikací služby WCF se nepodaří spustit službou IIS Express

> Procházení k aplikaci WCF výsledkem chyba stejný, jako je následující:
> 
> *Nelze načíst soubor nebo sestavení ' Microsoft.Web.Administration, verze = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35, nebo jeden z jeho závislých. Systém nemůže najít zadaný soubor.*
> 
> K tomu dochází, protože služba IIS Express betaverze nepodporuje WCF ve výchozím nastavení.
> 
> **Alternativní řešení** použijte některou z následujících náhradních postupů (alternativní řešení #2 vyžaduje systém Microsoft Windows Vista nebo novější):
> 
> 
> 1. Kopírování *Microsoft.Web.dll* a *Microsoft.Web.Administration.dll* sestavení z umístění instalace služby WebMatrix, které *bin* adresář WCF aplikace. Ve výchozím nastavení, služba WebMatrix je nainstalován v *Microsoft WebMatrix* podsložku systému *Program Files* složky.
> 2. V systému Microsoft Windows Vista nebo novějším, vytvořit symlink k sestavení v *bin* adresáře pomocí následujících příkazů. (Tento přístup má výhodu, nevytvoří kopii sestavení.)
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Nainstalujte dvě sestavení v mezipaměti GAC. Řádku se zvýšenými oprávněními spusťte následující příkazy:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problém: WebMatrix Beta 3 se nepodařilo provést určité úlohy, které vyžadují zvýšení oprávnění

> Služba WebMatrix Beta 3 se nepodařilo provést určité úlohy, které vyžadují zvýšení oprávnění, například při instalaci dalších součástí v následujících situacích:
> 
> - Windows Vista nebo Windows 7 jste přihlášeni pod účtem, který nemá oprávnění pro správu a řízení uživatelských účtů (UAC) je zakázáno.
> - Používáte Microsoft Windows XP nebo Microsoft Windows Server 2003.
> 
> **Alternativní řešení**  
> Většinu úloh ve službě WebMatrix Beta 3 nevyžaduje oprávnění správce. Pro ty, které provádějí můžete provést operaci jako správce nebo postupujte podle těchto kroků:
> 
> - V systému Windows Vista nebo Windows 7 povolte nástroj Řízení uživatelských účtů.
> - V systému Windows XP přidejte uživatele do skupiny zabezpečení Administrators.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problém: "Lokality z Galerie webových" je zakázána.

> **Lokality z Galerie webových** možnost je zakázaná, pokud není nainstalovaná verze 3.0 webové platformy.
> 
> **Alternativní řešení**  
> Nainstalujte [webové platformy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problém: V systému Windows Server 2003, IIS Express se nespustí pro uživatele bez oprávnění správce

> V systému Windows Server 2003 při spuštění stránky nebo spuštění služby IIS Express, služba IIS Express se nespustí. Pro webové stránky zobrazí se chybová zpráva označující, zda byla aplikace spuštěna uživatelem bez oprávnění správce.
> 
> **Alternativní řešení**  
> Spusťte službu WebMatrix Beta 3 jako administrativní uživatel. Další podrobnosti najdete v následujícím článku Knowledge Base:  
>   
> [Aplikace, která se spustí bez oprávnění správce uživatel nemůže naslouchat na HTTP provoz počítače, na kterém je aplikace spuštěna v systému Windows Vista, Windows Server 2003 nebo Windows XP.](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problém: Google Chrome není k dispozici jako možnost spuštění

> Google Chrome se nezobrazuje v seznamu prohlížečů v části **spustit** na **Domů** kartě.
> 
> **Alternativní řešení**  
> Některé verze Google Chrome nezaregistroval sami správně funkce výchozí programy systému Windows. Jako alternativní řešení, spustit Google Chrome, klikněte na *přizpůsobit a řízení Google Chrome* nabídky, klikněte na tlačítko *možnosti*a potom klikněte na *zkontrolujte Google Chrome Moje výchozí prohlížeč*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problém: Dialogové okno "Cizí klíč" neumožňuje zadání primární klíč

> **Cizí klíč** dialogové okno neumožňuje primární klíč tabulky zadejte název primárního klíče.
> 
> **Alternativní řešení**  
> To je úmyslné. Není nutné k zadání názvu primární klíč tabulky primárního klíče.


#### <a name="issue-the-relationships-button-is-disabled"></a>Problém: Tlačítko "Vztahů" zakázána.

> **Vztahy** tlačítko pod **tabulky** ve **databáze** pracovního prostoru je zakázán pro systém SQL Server Compact databáze.
> 
> **Alternativní řešení**  
> Žádné Systém SQL Server Compact nepodporuje relace mezi tabulkami.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problém: SQL parametrizované dotazy generování výjimek

> V systému SQL Server Compact 4.0, pokud nezadáte, jako datový typ `SqlDbType` nebo `DbType` pro parametry v parametrizované dotazy, je vyvolána výjimka při spuštění dotazu.
> 
> **Alternativní řešení**  
> Explicitně nastavit na datový typ pro parametry, jako `SqlDbType` nebo `DbType`. Je to nezbytné v případě datové typy objektů BLOB (`image` a `ntext`). Použijte kód takto:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Další informace

Další informace o službě WebMatrix Beta 3 najdete na následujících webech:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. Všechna práva vyhrazena. [Podmínky použití](https://msdn.microsoft.cos/cc300389.aspx).
