---
uid: web-pages/readme/beta3
title: A ASP.NET Web Pages (Razor) verze Beta 3 verze – soubor Readme | Dokumentace Microsoftu
author: rick-anderson
description: Web Matrix a ASP.NET Web souboru Readme verze Beta 3 stránky (Razor)
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5aa609bf31499485dc7a1298fa689f3a7cee4774
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363530"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Web Matrix a ASP.NET Web souboru Readme verze Beta 3 stránky (Razor)
====================
> Web Matrix a ASP.NET Web souboru Readme verze Beta 3 stránky (Razor)

9. listopadu 2010

## <a name="contents"></a>Obsah

- [Přehled](#Overview)
- [Instalace](#Installation_Notes)
- [Nové funkce, změny a známé problémy ve verzi Beta 3](#Known_Issues)

    - [Problémy instalace služby WebMatrix](#Known_Issues_Installation)
    - [Webové stránky ASP.NET](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Instalace aplikací](#Known_Issues_Installing_Applications)
    - [Publikování aplikací](#Known_Issues_Publishing_Applications)
    - [Další problémy](#Known_Issues_Other_Issues)
- [Další informace](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Přehled

> Microsoft WebMatrix Beta je bezplatná webová vývoj zásobník, který nainstaluje během několika minut. Webový server se integruje s databází a programovací rozhraní pro vytvoření jednoho integrovaného rozhraní. Beta verze služby WebMatrix můžete použít ke zjednodušení způsobu kódu, testování a publikování vlastní web ASP.NET a PHP nebo beta verze služby WebMatrix můžete použít ke spuštění nového webu pomocí oblíbených open source aplikace, jako jsou aplikace DotNetNuke, Umbraco, WordPress a Joomla. Beta verze služby WebMatrix používá stejný výkonný webový server, databázový stroj a prostředí architektury, které se spustí váš web na Internetu, takže hladký a bezproblémový přechod z vývojového do produkčního prostředí.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalace

> Instalace služby WebMatrix Beta 3, použijete [Microsoft webové platformy verze 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Po instalaci webové platformy, můžete k instalaci služby WebMatrix Beta 3.
> 
> Pokud máte problémy během instalace, podívejte se na [Poradce při potížích se instalačního programu webové platformy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Pokyny k publikování aplikace

> Zobrazit [podrobné pokyny pro publikování aplikací](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Nové funkce, změny, andKnown problémy

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Instalace služby WebMatrix Beta 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problém: Beta verze služby WebMatrix 3 je k dispozici pouze na platformách, které podporují rozhraní Microsoft .NET Framework 4

> Rozhraní .NET Framework verze 4 je vyžadován pro beta verzi služby WebMatrix. V některých případech instalační program beta verzi služby WebMatrix vám umožní instalaci na platformě, která není součástí sady podporované konfigurace. Zejména Windows Vista bez aktualizace SP1 vám umožní začít instalace služby WebMatrix Beta, ale selže a blokovat instalaci komponenty rozhraní .NET Framework 4.
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problém: Nejde nainstalovat WebMatrix Beta 3, pokud Microsoft Visual Studio 2008 se nainstaluje bez Microsoft Visual Studio 2008 SP1

> **Alternativní řešení**  
> Nainstalujte [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) z webu Microsoft Download Center.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problém: V mezipaměti GAC nejsou nainstalovány některé sestavení pro SQL Server Compact 4.0

> Spravovaná sestavení pro SQL Server Compact 4.0 nejsou umístěny v globální mezipaměti sestavení (GAC), když nainstalujete SQL Server Compact 4.0 na 64bitovém počítači a že má počítač pouze .NET Framework 3.5 SP1 Client Profile nainstalované. Spravovaná sestavení, které nejsou nainstalovány v GAC jsou:
> 
> - *System.Data.SqlServerCe.dll* (poskytovatele ADO.NET)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )
> 
> **Alternativní řešení**  
> Odinstalujte systém SQL Server Compact 4.0. Stáhněte a nainstalujte plnou verzi rozhraní .NET Framework 3.5 SP1 v následujícím umístění:  
>   
> [Microsoft .NET Framework 3.5 Service pack 1 (úplný)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Potom znovu nainstalujte SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problém: Nelze odinstalovat, SQL Server Compact pomocí příkazového řádku

> Odinstalace systému SQL Server Compact prostřednictvím parametrů příkazového řádku v této verzi nefunguje.
> 
> **Alternativní řešení**  
> Použití *programy a funkce* v Ovládacích panelech Windows k odinstalaci serveru Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET – webové stránky

Tato část dokumentu popisuje nové funkce, změny a známé problémy s verzí Beta 3 z rozhraní ASP.NET Web Pages se syntaxí Razor.

- [Nové funkce](#NewFeatures)
- [Změny](#Changes)
- [Problémy](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Nové funkce ve verzi Beta 3 pro rozhraní ASP.NET Web Pages se syntaxí Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Novinka: "Html.Raw" metoda vykreslí nekódovaného kód

> Nové `Html.Raw` metoda vám umožňuje vykreslit jako značku namísto vykreslování kódovaného výstupu značka jazyka HTML. (Ve výchozím nastavení, ASP.NET Razor zakóduje řetězce před jejich vykreslením.) Syntaxe je následující:
> 
> `Html.Raw(value)`
> 
> Následující příklad ukazuje, jak používat `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Změny ve verzi Beta 3 pro rozhraní ASP.NET Web Pages se syntaxí Razor

#### <a name="change-hrefattribute-method-removed"></a>Změna: Odebrat metodu "HrefAttribute"

> `HrefAttribute` Metodu `WebPage` třída odebrala. Tato pomocná použitý ke kódování problematické znaky v adresách URL. To se už nevyžaduje, protože syntaxe Razor rozhraní ASP.NET Galerie automaticky kóduje řetězce. (Pomocí nové `Html.Raw` metoda k vykreslení nekódovaného řetězce.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Změna: Syntaxe deklarativní "@helper" změnit pomocné rutiny

> Ve verzi Beta 3, změní jak analyzuje pomocné rutiny, které jsou vytvořené pomocí technologie ASP.NET `@helper` syntaxe. V podstatě `@helper` syntaxe je nyní analyzovat jako blok kódu, nikoli jako blok kódu, který může obsahovat kód. Proto kód v Pomocníkovi nemusí být uzavřeny v `@{ }` bloky. Naopak značek v Pomocníkovi musí být explicitně zahrnuta do elementů HTML nebo v technologii ASP.NET Razor `<text></text>` značky.
> 
> Například následující `@helper` syntaxe funguje ve verzi Beta 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> Ve verzi Beta 3 musíte změnit této pomocné rutiny, aby vypadala jako v následujícím příkladu:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Všimněte si, že `@{ }` znaků kolem počáteční kód v pomocné rutiny se už nepoužívá. Je to proto, že obsah pomocné rutiny jsou považovány za blok kódu ve výchozím nastavení. Pomocná rutina vykreslí značku, která začíná otevřením `<a>` značky. Pokud musí pomocné rutiny vykreslení prostý text nebo značky, které neobsahují ukončovací značky (například `<meta>` značky), obsah vykreslený musí být v `<text></text>` značky.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Změna: Odebrat "WebPageContext.HttpContext"

> `WebPageContext.HttpContext` Se odebrala vlastnost. Místo nich se používá `HttpContext.Current`. ( `WebPageContext.HttpContext` Vlastnost jednoduše zabalené to.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Změna: Pomocník "Facebook" přesunout do nového balíčku

> `Facebook` Přesunula do pomocné rutiny *Facebook.Helper* knihovny, která zahrnuje `Facebook` pomocné rutiny a další funkce. Je nutné nainstalovat tuto knihovnu jako samostatný balíček, jak je popsáno v "Instalace pomocné rutiny pomocí Správce balíčků" v tomto kurzu [Začínáme se stránkami ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Změna: Typy členství, Role a zabezpečení přesune do nové sestavení

> Následující typy byly přesunuty do `WebMatrix.WebData` sestavení:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Změna: Přesunout do sestavení System.Web.WebPages.dll třída "TagBuilder"

> `TagBuilde` Třídy jazyka r byla přesunuta do System.Web.WebPages.dll sestavení. Dřív to bylo v sestavení, který byl součástí technologie ASP.NET MVC. Tato změna znamená, že není nutné instalovat ASP.NET MVC, aby bylo možné používat `TagBuilder` třídy.
> 
> Třída je však stále v `System.Web.Mvc` oboru názvů. Chcete-li použít `TagBuilder` třídy (například ve vlastní syntaxe Razor rozhraní ASP.NET pomocné rutiny), musí odkazovat obor názvů (například přidáním `@using System.Web.Mvc` kódu).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Změnu: Žádost o ověření syntaxe se změnily. Odebrat třídu "Ověřování"

> Ve verzi Beta 3, chcete-li zakázat ověřování pro jednotlivá pole nebo sadu polí, můžete volat `Validation.Exclude` předejte název nebo názvy polí, které chcete vyloučit z ověření. Nová syntaxe je k dispozici ve verzi Beta 3 pro vynechání ověřování. `Validation` Metodu použitou v Beta 3 se odebrala.
> 
> > [!NOTE]
> > Pokud není zakážete ověření požadavku, pokud se uživatelé pokusí odeslat kód HTML (například pomocí editor formátovaného textu na stránce), na webu ohlásí chybu jako *z klientabylazjištěnapotenciálněnebezpečnáhodnotaRequest.Form*a uživatelský vstup nebyla přijata. Pokud zakážete ověření požadavku, musíte ručně zkontrolovat vstupu uživatele, abyste měli jistotu, že ho neobsahuje potenciálně nebezpečného kódu nebo skript, pomocí příkazu podobného tomuto [skriptování V4.0 knihovny Microsoft mezi lokality](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Chcete-li zakázat automatické ověření, zavolejte `Request.Unvalidated` metoda, předejte název pole nebo jiný objekt příspěvku, který chcete obejít ověření žádosti pro. Tuto metodu můžete použít k ověření pro všechny položky v obejít `Form`, `QueryString`, `Cookies`, a `ServerVariables` kolekce. Následující příklady ukazují, jak používat `Unvalidated` metody:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Známé problémy pro ASP.NET Web Pages se syntaxí Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problém: Neočekávané chování při použití vlastní tabulku členství

> K inicializaci zprostředkovatele členství pro web ASP.NET Razor, volání `WebSecurity.InitializeDatabaseConnection` metody. (V nástroji WebMatrix, šablona Starter Site zahrnuje volání této metody v  *\_AppStart.cshtml* souboru.) Pokud `autoCreateTables` parametr této metody je nastaven na hodnotu true (ve výchozím nastavení, je nastavena hodnotu true v šabloně Starter Site), a pokud názvu nerozpoznaný tabulky se předá metodě (druhý parametr), metoda nevyvolá chybu. Místo toho automaticky vytvoří v tabulce.
> 
> Pokud máte v úmyslu použít vlastní uživatelská tabulka pro členství, ale předejte název chybný tabulky pro to může být problém `WebSecurity.InitializeDatabaseConnection` metody. Protože metoda není ve výchozím nastavení vyvolá chybu, pokud neexistuje v tabulce, kterou zadáte, a místo toho vytvoří novou tabulku, můžete aplikace zobrazovat pracovat. Kód aplikace, která se spoléhá na vlastní uživatelská tabulka (a na pole v něm) můžete nakonec selhat s neočekávanými chybami.
> 
> **Alternativní řešení**  
> Ujistěte se, že název předaný `InitializeDatabaseConnection` metoda shody profilu uživatele v databázi členství tabulku nebo Ujistěte se, že `autoCreateTables` parametr je nastaven na hodnotu false.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problém: "nepovedlo se vygenerovat uživatelskou instanci systému SQL Server" Chyba

> Pokud aplikace Web služby WebMatrix používá systém SQL Server Express a je spuštěna služba IIS 7.5 na Windows 7 nebo Windows Server 2008 R2, může se zobrazit chyba, která označuje, že SQL Server nemůže načíst cestu uživatele místní aplikace v době běhu.
> 
> **Alternativní řešení** Ujistěte se, zda má účet Windows, na kterém aplikace běží pod (obvykle síťové služby), jako oprávnění čtení/zápisu pro kořenové složky aplikace a podsložky *aplikace\_Data*. Podrobnější informace najdete v článku znalostní báze [problémy se službou SQL Server Express uživatele vytváření instancí a ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problém: V sadě Visual Studio, obory názvů pro (knihovny DLL) pro vlastní sestavení sady nejsou importovány automaticky

> Pokud používáte vlastní sestavení v projektu v sadě Visual Studio, obory názvů deklarovaný v těchto sestaveních sady nejsou importovány automaticky v době návrhu. Odkazy na vlastní typy v důsledku toho nemusí být rozpoznán v době návrhu a jsou označeny jako není rozpoznána v sadě Visual Studio (pomocí "vlnovku"). K tomuto problému dochází pouze v době návrhu v sadě Visual Studio; samotná aplikace běží správně.
> 
> **Alternativní řešení**  
> Zahrnout `using` – příkaz (`imports` v jazyce Visual Basic), která odkazuje na entity, které nejsou rozpoznány v době návrhu.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problém: Visual Studio IntelliSense projektu šablony a k dispozici pouze v architektuře ASP.NET MVC verze 3

> Instalace rozhraní ASP.NET Web Pages také neinstaluje nástroje pro Visual Studio jako je například technologie IntelliSense a projekt šablony pro aplikace ASP.NET Web Pages.
> 
> **Alternativní řešení** používat technologii IntelliSense a projekt šablony pro aplikace webových stránek ASP.NET v sadě Visual Studio, instalace technologie ASP.NET MVC 3 RC, buď prostřednictvím instalace webové platformy nebo [samostatný instalační program](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problém: "&lt;pomocné rutiny&gt; třída nebyla nalezena" Chyba

> Po upgradu na verzi Beta 3, může se zobrazit chyba, která pomocnou třídu (například `Facebook` třídy) nebyl nalezen. Od verze Beta 2 a pokračování ve verzi Beta 3, pomocné rutiny se přesunuly do balíčků, které je nutné explicitně nainstalovat. Existující lokality nejsou upgradováni na obsahovat tyto balíčky; Jedná se o lokalitách ve *\My Documents\IISExpress* nebo *\My Documents\My weby* složek. Zejména, uvidíte tuto chybu, pokud použijete výchozí web v rámci *části osobní weby* (web1), který obsahuje odkaz na `Twitter` pomocné rutiny.
> 
> **Alternativní řešení**  
> Odkomentujte volání jakékoli pomocné rutiny v lokalitě, spusťte  *\_správce* stránce a nainstalujte balíček nebo balíčky, které obsahují pomocné rutiny, které chcete použít. Po instalaci balíčku, zrušte komentář u řádků, které odkazují na pomocné rutiny.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problém: Nasazení syntaxe Razor rozhraní ASP.NET Beta 3 sestavení do složky Bin nemusí fungovat na hostování webů

> Pokud provádíte nasazení na webu rozhraní ASP.NET Web Pages na hostitelském serveru, a pokud provádíte nasazení na web ASP.NET Razor Beta 3 sestavení *Bin* složky, může docházet k chybám, včetně následujících:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> To může nastat, pokud poskytovatel hostingu nainstaloval ASP.NET Web Pages Beta 1 sestavení do serveru globální mezipaměti (sestavení GAC). Sestavení v GAC získat přednost před sestaveními nainstalovat místně do *Bin* složky.
> 
> **Alternativní řešení** obraťte se na svého poskytovatele hostingových služeb, abyste se přesvědčili, že se zobrazují chyby z důvodu konfliktu mezi verzí zprostředkovatele sestavení a ta vaše. Pokud ano, požádejte poskytovatele hostingu sestavení v GAC serveru.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problém: Informační kanály pro čtení nebo jiných externích dat prostřednictvím serveru proxy

> Pokud serveru se systémem lokality je za proxy serverem, může být nutné nakonfigurovat informace o proxy serveru v *Web.config* souboru, aby bylo možné číst informace, které pocházejí z mimo váš web. Například, pokud použijete `ReCaptcha` pomocné rutiny, Pomocník komunikuje se službou nástroje reCAPTCHA, ale mohou být blokovány váš proxy server. Podobně informační kanály, které se používají v ASP.NET Web Pages, jako je například informačního kanálu používat Správce balíčků může vyžadovat konfiguraci proxy serveru.
> 
> Pokud máte potíže v práci s externí služby nebo práce s balíčkem informačního kanálu, vložte následující prvky do vaší aplikační kořen *Web.config* souboru:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Další informace o konfiguraci proxy serveru, naleznete v tématu [ &lt;proxy&gt; – Element (nastavení sítě)](https://msdn.microsoft.com/library/sa91de1e.aspx) na webové stránce MSDN.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problém: Chyba "Microsoft.Web.Infrastructure.dll nelze načíst."

> Pokud dříve nainstalované verze Beta 1 z rozhraní ASP.NET Web Pages se syntaxí Razor a nainstalujte verze Beta 3, nainstaluje se všechny odpovídající sestavení v GAC s výjimkou *Microsoft.Web.Infrastructure.dll*. V důsledku toho při spuštění ASP.NET Razor pages, se zobrazí chyba, že *Microsoft.Web.Infrastructure.dll* nelze načíst.
> 
> Tomuto problému nedojde, pokud jste načetli na verzi Beta 3 na čistém počítači.
> 
> **Alternativní řešení**  
> V Ovládacích panelech odinstalujte rozhraní ASP.NET Web Pages. Znovu nainstalujte na verzi Beta 3.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problém: Odinstalování rozhraní .NET Framework verze 4 zakáže ASP.NET Web Pages se syntaxí Razor

> Pokud odinstalujete rozhraní .NET Framework verze 4 a pak ho znovu nainstalujte, ASP.NET Web Pages se syntaxí Razor je zakázaná. Stránky s *.cshtml* rozšíření správně spustit. Rozhraní ASP.NET Web Pages zaregistruje sestavení v kořenovém adresáři počítač *Web.config* souboru a odebírá se rozhraní .NET Framework odebere tento soubor. Opětovná instalace rozhraní .NET Framework nainstalovat novou verzi konfiguračního souboru, ale nepřidá odkaz na sestavení rozhraní ASP.NET Web Pages.
> 
> **Alternativní řešení** po opětovné instalaci rozhraní .NET Framework, přeinstalujte rozhraní ASP.NET Web Pages se syntaxí Razor. Tím se přidá následující element na *Web.config* souboru v kořenovém adresáři počítač, který je obvykle v následujícím umístění:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problém: Dříve nasazenými technologií ASP.NET sestavení ve složce Bin aplikace dochází k chybám

> Během nasazování, kopie sestavení rozhraní ASP.NET Web Pages (například *Microsoft.WebPages.dll*) k *Bin* složky na serveru webu. (To může dojít automaticky během nasazení nebo proto, že vývojáři explicitně zkopírovat sestavení.) Ale když je nainstalována verze Beta 3, chyby dojde k, jako jsou chyby, které určité typy nelze nalézt. K tomu dochází, protože počet typů rozhraní ASP.NET Web Pages se přesunuly do různých oborech názvů pro verzi Beta 3.
> 
> **Alternativní řešení**   
> Zrušte *Bin* složka nasazenou aplikaci, zkopírujte nové sestavení do složky (nebo znovu nasadit aplikaci) a poté restartujte aplikaci.


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problém: Adresy URL bez přípony nebyl nalezen.cshtml/.vbhtml souborů ve službě IIS 7 nebo IIS 7.5

> Na službu IIS 7.5 nebo IIS 7, nejsou schopna nalézt stránek, které mají požadavky s adresou URL takto *.cshtml* nebo *.vbhtml* rozšíření:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Tento problém nastane, protože přepisování adres URL není povolená ve výchozím nastavení pro službu IIS 7 nebo IIS 7.5. Nejpravděpodobnějším scénář je, že se nezobrazí problém při testování místně pomocí služby IIS Express, ale dochází při nasazení webu k hostování webu.
> 
> **Alternativní řešení**
> 
> - Pokud budete mít kontrolu nad do počítače serveru, na počítači serveru nainstalujte aktualizaci podle popisu v [aktualizace, která je dostupná, umožňuje některé služby IIS 7.0 a IIS 7.5 obslužné rutiny pro zpracování požadavků, jejichž adresy URL nesmí končit tečkou](https://support.microsoft.com/kb/980368).
> - Pokud nemáte kontrolu nad do počítače serveru (například nasazujete hostující web), přidejte následující na web *Web.config* souboru:
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problém: Pomocí projektu webové aplikace nebo technologie ASP.NET MVC a ASP.NET Web pages ve stejné aplikaci

> Pokud v projektu webové aplikace nebo aplikace ASP.NET MVC byly pomocí webových stránek ASP.NET, může se zobrazit chyba, která *WebPageHttpApplication* nebyl nalezen.
> 
> **Alternativní řešení**  
> Pokud se zobrazí tato chyba, změňte základní třídu, ze kterého aplikace pochází. V *Global.asax* změňte následující řádek:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> K tomuto:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Změnu, která byla zavedena v platnosti obrátí na vydání Beta 1 z rozhraní ASP.NET Web Pages se syntaxí Razor.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problém: Nasazení aplikace do počítače, na kterém není SQL Server Compact nainstalovaná

> Aplikace, které zahrnují databáze systému SQL Server Compact můžete spustit na počítači, kde SQL Server Compact není nainstalovaná. Microsoft WebMatrix Beta 3 automaticky za vás zkopíruje tyto binární soubory a provede příslušné *Web.config* souborů transformace.
> 
> **Alternativní řešení** potřebujete zkopírujte tyto soubory a nastavte *Web.config* změny v souboru ručně, postupujte takto:
> 
> 1. Kopírovat sestavení modulu databáze, které chcete *Bin* složku (a její podsložky) aplikace v cílovém počítači: 
> 
>     - Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*
>     - Kopírování *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **k** *\Bin\x86*
>     - Kopírování *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **k** *\Bin\amd64*
> 2. V kořenové složce webové stránky, vytvořte nebo otevřete *Web.config* souboru. (Ve službě WebMatrix Beta 3, je k dispozici, pokud kliknete na tento typ souboru **všechny** v **vyberte typ souboru** dialogové okno.)
> 3. Přidejte následující prvek jako podřízený objekt **&lt;konfigurace&gt;** – element (ne uvnitř **&lt;system.web&gt;** element):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problém: Databáze a WebGrid pomocné rutiny nefungují na úrovni Medium Trust v jazyce Visual Basic

> Pokud používáte Visual Basic (vytváření *.vbhtml* soubory), `Database` a `WebGrid` pomocné rutiny nebude fungovat, pokud aplikace je nastaveno pro použití úrovni Medium Trust.
> 
> **Alternativní řešení**  
> Dočasně nastaví aplikace používat plnou důvěryhodnost.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problém: Vlastnost "Šifrovat" nebyl rozpoznán.

> SQL Server Compact 4.0 nebylo rozpoznáno `Encrypt` vlastnost `SqlCeConnection` třídy. Tuto vlastnost byste neměli používat k šifrování souborů databáze. `Encrypt` Vlastnost se přestala nabízet v systému SQL Server Compact 3.5 vydání a bylo zachováno pouze z důvodu zpětné kompatibility. 
> 
> **Alternativní řešení**  
> Použití `Encryption Mode` vlastnost `SqlCeConnection` třídy k šifrování souborů databáze systému SQL Server Compact 4.0. Následující příklad ukazuje, jak vytvořit šifrovaný pomocí databáze systému SQL Server Compact 4.0 `Encryption Mode` vlastnost:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Chcete-li změnit režim šifrování stávající databázi systému SQL Server Compact 4.0, postupujte takto:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> K šifrování nešifrované databáze systému SQL Server Compact 4.0, postupujte takto:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problém: Jsou vyžadovány knihovny prostředí runtime Microsoft Visual C++ 2008

> Nativní knihovny DLL systému SQL Server Compact 4.0 potřebovat Microsoft Visual C++ 2008 Runtime Libraries (x 86, IA64 a x 64), aktualizace Service Pack 1.
> 
> **Alternativní řešení**  
> Instalace rozhraní .NET Framework 3.5 SP1. Tím se nainstaluje taky Visual C++ 2008 modulu Runtime knihoven SP1. Tyto knihovny si můžete stáhnout z následujícího umístění:   
>   
> [Aktualizaci zabezpečení pro Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Poznámka: instalace rozhraní .NET Framework 2.0, 3.0, nebo se 4 *není* instalaci Visual C++ 2008 modulu Runtime knihoven SP1.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problém: Pokud je SQL Server Compact nainstalovaný před instalací rozhraní .NET Framework v počítači, jeho výchozí název zprostředkovatele není registrován v souboru machine.config rozhraní .NET Framework

> SQL Server Compact můžete nainstalovat na počítač, který nemá rozhraní .NET Framework nainstalovat, protože SQL Server Compact vyžadují rozhraní .NET framework. Pokud je nainstalované rozhraní .NET Framework verze 3.5 ani 4 před instalací systému SQL Server Compact, SQL Server Compact nastavení nezaregistruje jeho výchozí název zprostředkovatele v *machine.config* souboru. Všechny aplikace, která závisí na SQL Server Compact položku v *machine.config* souboru se nezdaří. Výchozí název položky registrační záznam v *machine.config* vypadá podobně jako v následujícím příkladu:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Alternativní řešení**  
> Odinstalujte SQL Server Compact 4.0 CTP1. Stažení a instalace plné verze rozhraní .NET Framework z následujícího umístění:
> 
> [Microsoft .NET Framework 3.5 Service pack 1 (úplný)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Verze rozhraní Microsoft .NET Framework 4.0 (úplný)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Přeinstalujte [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Instalace aplikací

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problém: Instalace aplikace může trvat dlouhou dobu Pokud složku Dokumenty uživatele přesměruje na sdílené síťové složky

> **Alternativní řešení**  
> Žádné Aplikace může nějakou dobu instalace, ale nainstaluje správně.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Publikování aplikací

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problém: Web nemusí fungovat po publikování, pokud není pole "Cílovou adresu URL" předponou http:// nebo https://.

> V **nastavení publikování** dialogové okno, pokud cílová adresa URL nezačíná `http://` nebo `https://`, lokalitě nemusí fungovat po nasazení.
> 
> **Alternativní řešení**  
> Ujistěte se, že před publikováním serveru, cílovou adresu URL v **nastavení publikování** dialogové okno začíná `http://` nebo `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problém: Publikování databáze MySQL selže s chybou "se nepodařilo publikovat i databázi. To může dojít, pokud vzdálené databázi nepodaří spustit skript."

> Této chybě může dojít k z několika důvodů. Jedním z důvodů, že zobrazí se tato chyba je-li skript databáze obsahuje znak jednoduché uvozovky (') a není cílové databáze MySQL výchozí znakovou sadu UTF-8.
> 
> **Alternativní řešení**  
> Nastavte výchozí znakovou sadu pro vzdálenou databázi MySQL na UTF-8.


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Další problémy

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problém: Vyhledávání nebo filtru v sestavách pro nefunguje Seskupit podle: typ problému

> Při spuštění sestavy pro lokalitu, pokud zadáte text v *filtr podle adresy URL* pole a klikněte na tlačítko *hledání*, nic se nestane. Důvodem je, že tento ovládací prvek není funkční i *Group* stav sestavy je nastavený na *typ problému*, což je výchozí hodnota.
> 
> **Alternativní řešení** v *Group By* kartu pásu karet klikněte na tlačítko *URL* seskupit položky podle jejich zdroj adresy URL. Textové pole a tlačítko Filtrovat položky jsou funkční i v tomto stavu.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problém: Aplikací WCF neúspěšné spuštění službou IIS Express

> Procházení k aplikaci WCF způsobí chybu stejný, jako je následující:
> 
> *Nelze načíst soubor nebo sestavení "Microsoft.Web.Administration, verze = 7.0.0.0, jazyková verze = neutral, PublicKeyToken = 31bf3856ad364e35' nebo některou z jeho závislostí. Systém nemůže najít zadaný soubor.*
> 
> K tomu dochází, protože služba IIS Express Beta verzi nepodporuje WCF ve výchozím nastavení.
> 
> **Alternativní řešení** použijte některou z následujících náhradních postupů (alternativní řešení #2 vyžaduje systém Microsoft Windows Vista nebo novější):
> 
> 
> 1. Kopírovat *Microsoft.Web.dll* a *knihovna Microsoft.Web.Administration.dll* sestavení z umístění instalace služby WebMatrix se *bin* adresáři WCF aplikace. Ve výchozím nastavení, je nainstalována služba WebMatrix v *Microsoft WebMatrix* podsložku v systému *Program Files* složky.
> 2. V systému Microsoft Windows Vista nebo novější, vytvořte symlink k sestavením v *bin* adresář pomocí následujících příkazů. (Tento přístup má výhodu, že nevytváří kopii tohoto sestavení.)
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Nainstalujte dvě sestavení v mezipaměti GAC. Z řádku se zvýšenými oprávněními spusťte následující příkazy:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problém: Beta verze služby WebMatrix 3 není schopen provést určité úlohy, které vyžadují ke zvýšení úrovně oprávnění

> Služba WebMatrix Beta 3 není schopen provést určité úlohy, které vyžadují ke zvýšení úrovně oprávnění, například při instalaci dalších komponent v následujících situacích:
> 
> - U Windows Vista nebo Windows 7 jsou přihlášeni pomocí účtu, který nemá oprávnění pro správu a řízení uživatelských účtů (UAC) je zakázán.
> - Používání systému Microsoft Windows XP nebo Microsoft Windows Server 2003.
> 
> **Alternativní řešení**  
> Většinu úkolů ve službě WebMatrix Beta 3 nevyžaduje oprávnění správce. Pro ty, které udělat můžete provádět operace jako správce nebo postupujte podle těchto kroků:
> 
> - U Windows Vista nebo Windows 7 povolení nástroje Řízení uživatelských účtů.
> - Windows XP přidejte uživatele do skupiny zabezpečení Administrators.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problém: "Webu z Galerie webových" je zakázaná.

> **Webu z Galerie webových** možnost je zakázaná, pokud není nainstalovaný 3.0 Instalační služby webové platformy.
> 
> **Alternativní řešení**  
> Nainstalujte [instalace webové platformy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problém: V systému Windows Server 2003, službu IIS Express nelze spustit pro uživatele bez oprávnění správce

> V systému Windows Server 2003 při spuštění stránky nebo spuštění služby IIS Express, služby IIS Express se nespustí. Pro webové stránky, zobrazí se chyba, která označuje, že aplikace se spustila uživatel bez oprávnění správce.
> 
> **Alternativní řešení**  
> Služba WebMatrix Beta 3 Spusťte jako uživatel s oprávněním správce. Další podrobnosti najdete v následujícím článku znalostní báze:  
>   
> [Aplikace, která je tím, že uživatel bez oprávnění správce nemůže naslouchat na počítače, na kterém aplikace běží ve Windows Vista, Windows Server 2003 nebo Windows XP přenos pomocí protokolu HTTP.](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problém: Google Chrome není k dispozici jako možnost spuštění

> Google Chrome se nezobrazí v seznamu prohlížečů v rámci **spustit** na **Domů** kartu.
> 
> **Alternativní řešení**  
> Některé verze Google Chrome neregistrujte sami správně s výchozí programy funkcí ve Windows. Jako alternativní řešení, spusťte Google Chrome, klikněte na tlačítko *přizpůsobení a řízení Google Chrome* nabídky, klikněte na tlačítko *možnosti*a potom klikněte na tlačítko *zkontrolujte Google Chrome prohlížeči výchozí*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problém: V dialogovém okně "Cizí klíč" neumožňuje zadání primární klíč

> **Cizí klíč** dialogové okno k zadání název primárního klíče z primárního klíče tabulky nepovoluje.
> 
> **Alternativní řešení**  
> Je to záměr. Není nutné zadat název primárního klíče z primární klíč tabulky.


#### <a name="issue-the-relationships-button-is-disabled"></a>Problém: Tlačítko "Relace" je zakázané.

> **Vztahy** tlačítko **tabulky** kartu **databází** pracovní prostor je zakázaná pro databáze systému SQL Server Compact.
> 
> **Alternativní řešení**  
> Žádné SQL Server Compact nepodporuje relace mezi tabulkami.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problém: Parametrizovaného dotazu SQL dotazy vyvolat výjimky

> V systému SQL Server Compact 4.0, pokud nezadáte, jako datový typ `SqlDbType` nebo `DbType` pro parametry v parametrizované dotazy, dojde k výjimce při spuštění dotazu.
> 
> **Alternativní řešení**  
> Explicitně nastavte datový typ pro parametry, jako je třeba `SqlDbType` nebo `DbType`. Tato vlastnost je zásadní v případě typy dat objektů BLOB (`image` a `ntext`). Použít kód podobný tomuto:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Další informace

Další informace o službě WebMatrix Beta 3 naleznete na následujících webech:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© Microsoft 2010 Corporation. Všechna práva vyhrazena. [Podmínky použití](https://msdn.microsoft.cos/cc300389.aspx).
