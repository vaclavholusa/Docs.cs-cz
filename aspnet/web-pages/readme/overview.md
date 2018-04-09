---
uid: web-pages/readme/overview
title: Služba WebMatrix Readme | Microsoft Docs
author: rick-anderson
description: Služba WebMatrix a ASP.NET Web Pages (Razor) verzi 1.0 – soubor Readme
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: c65ee58b8c13b0b4acb6e7c9b631c8235e791506
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="webmatrix-readme"></a>Soubor Readme pro službu WebMatrix
====================
13. ledna 2011

## <a name="contents"></a>Obsah

> [!NOTE]
> Tento soubor readme platí pro verzi 1.0 nástroje WebMatrix.


- [Přehled](#Overview)
- [Instalace](#Installation_Notes)
- [Jak publikovat aplikace](#InstructionsForPublishingApplications)
- [Změny a problémy](#ChangesAndIssues)

    - [Instalace služby WebMatrix 1.0](#Known_Issues_Installation)
    - [Webové stránky ASP.NET](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Instalace aplikace](#Known_Issues_Installing_Applications)
    - [Publikování aplikací](#Known_Issues_Publishing_Applications)
- [Další informace](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Přehled

> Microsoft WebMatrix 1.0 je bezplatných webových zásobníku vývoj, který nainstaluje v minutách. Webový server se integruje s databáze a programovací rozhraní pro vytvoření jednoho integrovaného rozhraní. Služba WebMatrix můžete zjednodušit kód, testování a publikování vlastní web ASP.NET a PHP způsob nebo používáte službu WebMatrix ke spuštění nového webu pomocí Oblíbené open-source aplikace, jako je aplikace DotNetNuke, Umbraco, WordPress nebo Joomla. Služba WebMatrix používá stejný výkonný webový server, databázový stroj a prostředí architektury, které se spustí svůj web na Internetu, které umožňuje přechod z vývojového do produkčního prostředí hladký a bezproblémový.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalace

> Pokud chcete nainstalovat službu WebMatrix 1.0, musíte nejprve nainstalovat [Microsoft webové platformy verze 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Po instalaci webové platformy, můžete k instalaci služby WebMatrix.
> 
> Pokud máte potíže s během instalace, podívejte se na [odstraňování potíží s instalačního programu webové platformy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Jak publikovat aplikace

> V tématu [podrobné pokyny k publikování aplikací](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Změny a problémy

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problémy 1.0 instalace služby WebMatrix

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problém: WebMatrix 1.0 je k dispozici pouze na platformách, které podporují rozhraní Microsoft .NET Framework 4

> Rozhraní .NET Framework verze 4 je požadované pro službu WebMatrix. V některých případech instalační program služby WebMatrix 1.0, budete moct znovu nainstalujte na platformě, která není součástí sady podporované konfigurace. Konkrétně Windows Vista bez aktualizace SP1 vám umožní zahájit instalaci služby WebMatrix, ale součásti rozhraní .NET Framework 4 se nezdaří a blokovat instalaci.
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


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problém: Nelze nainstalovat službu WebMatrix 1.0, když se nainstaluje Microsoft Visual Studio 2008 bez Microsoft Visual Studio 2008 SP1

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

Tato část dokumentu popisuje nové funkce, změny a známé problémy s verzi 1.0 z webových stránek ASP.NET se syntaxí Razor.

- [Nové funkce](#NewFeatures)
- [Změny](#Changes)
- [Problémy](#Issues)

#### <a id="NewFeatures"></a>  Nové funkce

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Nové: Konfigurace nastavení přidáno zakázat Správce balíčků

> Nový `asp:AdminManagerEnabled` klíč je k dispozici pro `<appSettings>` element v *web.config* souboru, který umožňuje úplně vypnout správce balíčků. Výchozí hodnota pro tento element je nastavena hodnota true, znamená to, že pokud není součástí *web.config* souboru, Správce balíčků je povoleno. Zakázat Správce balíčků, přidat následující prvek *web.config* soubor v kořenovém adresáři webu:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  Změny

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Změnit: "webPages:AdminFolderVirtualPath" klíč přejmenován na "asp: AdminFolderVirtualPath"

> `webPages:AdminFolderVirtualPath` Klíč, který lze přidat do *web.config* souboru k zadání umístění správce balíčků byl přejmenován na použití `asp:` obor názvů místo `webPages` oboru názvů. Pokud jste použili tento prvek, je třeba přejmenovat v konfiguračním souboru.


#### <a id="Issues"></a>  Známé problémy

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problém: Hesla uživatelů se členstvím už rozpoznána

> Algoritmus pro vytváření a ukládání hesel členství (přihlášení) se změnil za bezpečnější. V důsledku toho nebude rozpoznána hesel uložených pro členy (uživatelé) z verze Beta verzí aplikace ASP.NET Razor. 
> 
> **Alternativní řešení** Pokud webu nebyla dosud byl uvedení do produkčního prostředí, odeberte záznamů uživatele z databáze členství. Pokud je databáze za provozu, prostřednictvím kódu programu obnovit existující hesla v databázi členství.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problém: Neočekávané chování při použití vlastní uživatelská tabulka pro členství

> K chybě při inicializaci zprostředkovatele členství pro web ASP.NET Razor, zavoláte `WebSecurity.InitializeDatabaseConnection` metoda. (Ve službě WebMatrix, zahrnuje šabloně Starter Site volání této metody v  *\_AppStart.cshtml* souboru.) Pokud `autoCreateTables` parametr této metody je nastaven na hodnotu true (ve výchozím nastavení je nastavena na hodnotu true v šabloně Starter Site), a pokud je název tabulky nerozpoznané předaný metodě (druhý parametr), metoda nevyvolá chybu. Místo toho automaticky vytvoří v tabulce.
> 
> To může být problém, pokud máte v úmyslu použít vlastní uživatelská tabulka pro členství, ale předat název nesprávné tabulky k `WebSecurity.InitializeDatabaseConnection` metoda. Vzhledem k tomu, že metoda není ve výchozím nastavení vyvolá chybu, pokud neexistuje v tabulce, které zadáte, a protože místo toho vytvoří novou tabulku, můžete aplikaci pravděpodobně fungovat. Aplikační kód, který závisí na vlastní uživatelská tabulka (a na pole v něm) můžete nakonec nezdaří s neočekávaným chybám.
> 
> **Alternativní řešení**  
> Ujistěte se, že název předaná `InitializeDatabaseConnection` metoda odpovídá uživatelský profil tabulky v databázi členství nebo zkontrolujte, zda `autoCreateTables` parametr je nastaven na hodnotu false.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Problém: Chybová zpráva "v modulu Správce vyžaduje přístup k ~/App\_Data"

> Za určitých okolností pokusu o vytvoření uživatelů nebo jinak pracovat s systém členství technologie ASP.NET může způsobit na stránce zobrazit chyba *modulu Správce vyžaduje přístup k ~/App\_Data*. K tomu dojde, pokud účet, který služba IIS nebo IIS Express je spuštěná s pověřeními nemá oprávnění k vytvoření a zápis do *aplikace\_Data* složku v kořenovém adresáři webu. 
> 
> **Alternativní řešení** ručně vytvořit *aplikace\_Data* složku pro web. Ujistěte se, že účet systému Windows, na kterém se aplikace spouští pod (obvykle síťová služba) má oprávnění ke čtení/zápisu pro kořenové složky aplikace a podsložky, jako je aplikace\_Data. Podrobnější informace najdete v článku znalostní báze Knowledge Base [problémy s SQL Server Express uživatele vytváření instancí a projekty webových aplikací ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problém: "Nepodařilo se vygenerovat uživatelskou instanci systému SQL Server" Chyba

> Pokud služba WebMatrix webové aplikace používá systém SQL Server Express a je spuštěna služba IIS 7.5 na Windows 7 nebo Windows Server 2008 R2, může se zobrazit chybu, která určuje, že systém SQL Server nelze načíst cestu k místní aplikace uživatele za běhu.
> 
> **Alternativní řešení** Ujistěte se, že účet systému Windows, na kterém se aplikace spouští pod (obvykle síťová služba) má oprávnění ke čtení/zápisu pro kořenové složky aplikace a podsložky jako *aplikace\_dat*. Podrobnější informace najdete v článku znalostní báze Knowledge Base [problémy s SQL Server Express uživatele vytváření instancí a projekty webových aplikací ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problém: Soubory, které obsahuje Správce balíčků prostředků nebo hesla správce balíčků jsou servable v rámci služby IIS 6.0 a starší

> Pokud nasadíte aplikaci ASP.NET Web Pages (Razor), která byla vytvořená pomocí verze RC2 a pokud aplikace obsahuje *password.txt* nebo *packagesources.txt* souboru pod */App\_ Data/admin*, IIS 6.0 bude sloužit souboru, pokud je požadována, potenciálně vystavení hesla pro vaše instance správce balíčku. 
> 
> **Alternativní řešení** přejmenovat *password.txt* nebo *packagesources.txt* do souboru *password.config* nebo *packagesources.config*. Ve výchozím nastavení, IIS 6.0 neobsluhuje soubory, které mají *.config* rozšíření. (Ve službě IIS 7, žádné soubory v *aplikace\_Data* složky se zpracovávají, takže není nutné přejmenovat soubory.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problém: Odinstalace balíčky nainstalované používání verze Beta 3 neodebere úplně součásti balíčku

> Pokud jste nainstalovali balíček pomocí Správce balíčků ve verzi Beta 3 a poté odinstalujte jej pomocí aktuální verze, balíček není zcela odinstalována. Pomocí Správce balíčků **odinstalace** tlačítko odebere některé součásti, ale ponechá kód knihovny balíčku a neaktualizuje *package.config* souboru.
> 
> **Alternativní řešení**   
> Proveďte tyto kroky:  
> 1. Odstranit *aplikace\_Data\packages* složky. Tím se odeberou všechny balíčky.   
> 2. Odstranit *packages.config* soubor v kořenovém adresáři webu.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problém: V sadě Visual Studio, vyvolání správce webových balíčků převede aplikace do režimu offline

> Pokud pracujete v sadě Visual Studio (ne WebMatrix) a použít  *\_správce* funkce spustit Správce balíčků, Visual Studio převede aplikace do režimu offline a požadavky *aplikace\_ offline.htm* do kořenové složky webu, který naruší budete moci použít Správce balíčků.
> 
> [!NOTE]
> Přestože by obvykle uvidíte toto chování při použití rozhraní založené na webu balíček správce, je-li přidat, odebrat nebo změnit všechny soubory v dojde k stejné chování *aplikace\_Data* složky.
> 
> **Alternativní řešení**   
> K práci s balíčky v sadě Visual Studio, použijte namísto správce webových balíčků rozšíření NuGet. Informace najdete v tématu [NuGet dokumentaci](https://docs.microsoft.com/nuget/). Pokud pracujete s ostatními soubory v *aplikace\_Data* složky, můžete si uložit soubory, jinde k tomuto problému vyhnout. Pokud není praktické, odstraňte *aplikace\_offline.htm* soubor ručně, nebo počkejte, až lokalitu přejde do režimu online automaticky (ve výchozím nastavení po 30 sekund).


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problém: Visual Studio IntelliSense a projekt šablony k dispozici pouze v architektuře ASP.NET MVC verze 3

> Instalace technologie ASP.NET Web Pages také nenainstaluje nástroje pro sadu Visual Studio jako IntelliSense a projekt šablony pro aplikace ASP.NET Web Pages.
> 
> **Alternativní řešení** používáte IntelliSense a projekt šablony pro aplikace webových stránek ASP.NET v sadě Visual Studio, nainstalujte technologii ASP.NET MVC 3 RC buď prostřednictvím instalace webové platformy nebo [samostatný instalační program](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problém: Čtení informačních kanálů nebo jiné externí data prostřednictvím serveru proxy

> Serveru se systémem lokality je za proxy serverem, může být potřeba konfigurovat informace o proxy serveru v *web.config* souboru, aby mohl číst informace, které pochází z mimo váš web. Například pokud použijete `ReCaptcha` pomocné rutiny, pomocné rutiny komunikuje se službou nástroje reCAPTCHA, ale mohou být blokovány proxy serveru. Podobně informační kanály, které se používají na webových stránkách ASP.NET, jako je například používá Správce balíčků informačního kanálu může vyžadovat konfiguraci proxy serveru.
> 
> Pokud máte potíže v práci s externí službu nebo práce s tímto balíčkem kanálu, uveďte následující prvky do vaší aplikace kořenové *web.config* souboru:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Další informace o konfiguraci proxy serveru najdete v tématu [ &lt;proxy&gt; – Element (nastavení sítě)](https://msdn.microsoft.com/library/sa91de1e.aspx) na webu MSDN.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problém: Odinstalace rozhraní .NET Framework verze 4 zakáže technologie ASP.NET Web Pages se syntaxí Razor

> Pokud odinstalujete rozhraní .NET Framework verze 4 a znovu ji nainstalovat, technologie ASP.NET Web Pages se syntaxí Razor je zakázané. Stránky s *.cshtml* rozšíření se nespustí správně. Rozhraní ASP.NET Web Pages zaregistruje sestavení v kořenu počítač *web.config* soubor odebrán souboru a odebrání rozhraní .NET Framework. Opětovné instalace rozhraní .NET Framework nainstaluje novou verzi konfiguračního souboru, ale nepřidá odkaz na sestavení pro ASP.NET Web Pages.
> 
> **Alternativní řešení** po opětovné instalaci rozhraní .NET Framework, přeinstalujte rozhraní ASP.NET Web Pages se syntaxí Razor. Tím se přidá následující elementu, který chcete *web.config* soubor v kořenu počítač, který je obvykle v následujícím umístění:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


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
> - Pokud nemáte kontrolu nad počítači serveru (například nasazujete k hostování webu), přidejte následující na web *web.config* souboru: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problém: Nasazení aplikace do počítače, který nemá systém SQL Server Compact nainstalován

> Aplikace, které obsahují systém SQL Server Compact databáze můžete spustit na počítači, kde systém SQL Server Compact není nainstalovaná. Microsoft WebMatrix 1.0 automaticky zkopíruje tyto binární soubory pro vás a provede odpovídající *web.config* souboru transformace.
> 
> **Alternativní řešení** Pokud potřebujete zkopírujte tyto soubory a ujistěte se, *web.config* změny souborů ručně, postupujte takto:
> 
> 1. Zkopírujte sestavení modulu databáze, které chcete *Bin* složky (a její podsložky) aplikace na cílovém počítači:  
> 
>    - Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **k** *\Bin*
>    - Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86*
>    - Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>to</strong><em>\Bin\amd64</em>
> 
> 2. V kořenové složce webové stránky, vytvořit nebo otevřít *web.config* souboru. (Ve službě WebMatrix 1.0 je k dispozici, pokud kliknete na tento typ souboru **všechny** v **vyberte typ souboru** dialogové okno.)
> 3. Přidejte následující prvek jako podřízenou `<configuration>` – element (mimo `<system.web>` element):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problém: "Databáze" a "WebGrid" Pomocníci nefungují na úrovni Medium Trust v jazyce Visual Basic

> Pokud používáte Visual Basic (vytváření *.vbhtml* soubory), `Database` a `WebGrid` Pomocníci nebude fungovat, pokud je aplikace nastavena používat úrovni Medium Trust.
> 
> **Alternativní řešení**  
> Pokud používáte Visual Studio 2010, můžete vyřešit tento problém instalací verze aktualizace Service Pack 1. Dokud finální verzi verze s aktualizací SP1 je k dispozici, můžete stáhnout verzi SP1 Beta [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) stránky na webu Microsoft Download Center.   
>   
> Pokud toto není praktické, nebo pokud používáte Visual Studio 2010, můžete dočasně nastavit aplikaci, aby používala úplný vztah důvěryhodnosti.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problém: "ApplicationPart" prostředky jsou externě dostupný.

> Pokud sestavení obsahuje objekty, které je odvozena z `ApplicationPart` třídy, vystavené sestavení prostředky `ResourceRouteHandler` třídy. Zvažte například následující adresu URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Tento požadavek stáhne všechny řetězce prostředků v *System.Web.WebPages.Administration.dll* sestavení. Všechny vložené prostředky (i ty, které nejsou určeny k zpracovat jako statický obsah), se stáhnou. Pokud vložené prostředky obsahují citlivé informace, to může představovat bezpečnostní riziko. 
> 
> **Alternativní řešení**   
> Pokud vytvoříte **ApplicationPart** objektu, ujistěte se, že vložené prostředky přidružené který **ApplicationPart** sestavení objektu neobsahují citlivé informace.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Informace o problémech s instalací pro službu WebMatrix najdete v tématu [problémy instalace služby WebMatrix](#Known_Issues_Installation) dříve v tomto dokumentu.


Tato část dokumentu popisuje známé problémy pro vývojové prostředí WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problém: Změny v uživatelské jméno nebo heslo připojovacího řetězce databáze v souboru web.config se neprojeví v pracovní prostor databáze

> **Alternativní řešení**  
> 
> 1. V *web.config* souboru, změňte název databáze v připojovacím řetězci (například přidat "1" do něj).
> 2. Uložit *web.config* souboru.
> 3. Klikněte na tlačítko **databáze** a aktualizujte.
> 4. Změňte název databáze v připojovacím řetězci v *web.config* souboru zpět na původní název databáze.
> 5. Uložit *web.config* souboru.
> 6. Klikněte na tlačítko **databáze** a aktualizujte.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problém: Složky vytvořené WebMatrix nelze odstranit.

> Pokud služba WebMatrix používá zvýšenými oprávněními (to znamená, že jste spustili pomocí služby WebMatrix **spustit jako správce** možnost v systému Windows), složky, které jsou vytvořené pomocí služby WebMatrix nelze odstranit pomocí Průzkumníka Windows.
> 
> **Alternativní řešení**  
> Spusťte program Průzkumník Windows zvýšenými oprávněními. Postupujte podle těchto kroků:  
> 
> 1. V systému Windows, klikněte na tlačítko **spustit**.
> 2. Zadejte "Windows Explorer" a klikněte pravým tlačítkem na položku **Průzkumníka Windows**.
> 3. Klikněte na tlačítko **spustit jako správce**. Potom můžete odstranit složky.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problém: WebMatrix 1.0 nelze provést určité úlohy, které vyžadují zvýšení oprávnění

> Služba WebMatrix 1.0 se nepodařilo provést určité úlohy, které vyžadují zvýšení oprávnění, například při instalaci dalších součástí v následujících situacích:
> 
> - Windows Vista nebo Windows 7 jste přihlášeni pod účtem, který nemá oprávnění pro správu a řízení uživatelských účtů (UAC) je zakázáno.
> - Používáte Microsoft Windows XP nebo Microsoft Windows Server 2003.
> 
> **Alternativní řešení**  
> Většinu úloh ve službě WebMatrix 1.0 nevyžaduje oprávnění správce. Pro ty, které provádějí můžete provést operaci jako správce nebo postupujte podle těchto kroků:
> 
> - V systému Windows Vista nebo Windows 7 povolte nástroj Řízení uživatelských účtů.
> - V systému Windows XP přidejte uživatele do skupiny zabezpečení Administrators.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problém: "Lokality z Galerie webových" je zakázána.

> **Lokality z Galerie webových** možnost je zakázaná, pokud není nainstalovaná verze 3.0 webové platformy.
> 
> **Alternativní řešení**  
> Nainstalujte [webové platformy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


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


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problém: IntelliSense není k dispozici ve službě WebMatrix pro syntaxi Razor syntaxe jazyka C# a Visual Basic

> IntelliSense je podporováno ve službě WebMatrix pro HTML a CSS. Však není k dispozici pro další jazyky. 
> 
> **Alternativní řešení**   
> Žádné


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problém: IntelliSense pro HTML a CSS navrhuje prvky, které nejsou kontextově vhodné

> IntelliSense pro kód ve službě WebMatrix podporuje používání HTML [XHTML 1.0 přechodném schématu](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) a používání šablon stylů CSS [schématu CSS 2.1](http://www.w3.org/TR/CSS2/). Protože tyto konkrétní schémata vychází IntelliSense, určité značky, atributy a vlastnosti může navrhované, které nejsou vhodné pro aktuální stránku nebo styl definice. Pro kód HTML může také dojít k neočekávané návrhů v obsah, který může být vyhodnocen jako poškozený XHTML (například když značky nejsou uzavřeny). Tento problém může být více patrné, pokud je kurzor uvnitř neúplné značky; v takovém případě může IntelliSense Navrhněte nové počáteční značky nebo nabízí další nesprávný návrhy. 
> 
> **Alternativní řešení**   
> Pro kód HTML Ujistěte se, že pracujete v kódu XHTML stránky ve správném formátu, dokončení. U šablon stylů CSS neexistuje žádné alternativní řešení.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problém: IntelliSense není vyvolána, když zadáte

> IntelliSense v některých případech nemusí být volána při zadávání HTML nebo šablon stylů CSS v editoru. Konkrétně to může dojít, když je kurzor přímo vedle jiný element nebo na konci souboru. 
> 
> **Alternativní řešení**   
> Ujistěte se, že je prázdný znak kolem bodu vložení a kurzor není na konci souboru. Můžete také vyvolat IntelliSense ručně stisknutím kombinace kláves Ctrl + mezerník.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problém: Žádné uživatelské rozhraní je k dispozici pro zakázání IntelliSense

> Služba WebMatrix 1.0 poskytuje bez uživatelského rozhraní nebo gesto pro zakázání IntelliSense. 
> 
> **Alternativní řešení**   
> Spusťte následující příkaz, který zahrnuje přepínač, který zakazuje IntelliSense pomocí služby WebMatrix:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>Služby IIS Express

Služba IIS Express má svůj vlastní soubor readme, který je k dispozici na následující adrese URL:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

Systém SQL Server Compact má svůj vlastní soubor readme, který je k dispozici na následující adrese URL:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Informace o problémech, které zahrnují instalaci systému SQL Server Compact jako součást nástroje WebMatrix, najdete v tématu [problémy instalace služby WebMatrix](#Known_Issues_Installation) dříve v tomto dokumentu.

### <a id="Known_Issues_Installing_Applications"></a>  Instalace aplikace

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problém: Instalace aplikace může trvat dlouhou dobu. Pokud je přesměrování složky Dokumenty uživatele do sdílené síťové složky

> **Alternativní řešení**  
> Žádné Aplikace může trvat, než k instalaci, ale nainstaluje správně.


### <a id="Known_Issues_Publishing_Applications"></a>  Publikování aplikací

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problém: "požadované oprávnění nelze získat" Chyba při publikování databáze SQL Compact

> Služba WebMatrix nepodporuje plně nasazení podpůrné binární soubory pro systém SQL Server Compact na server, který běží s konfigurací středním vztahem důvěryhodnosti rozhraní .NET Framework verze 3.5.
> 
> **Alternativní řešení**  
> Vhodnějším řešením je instalace rozhraní .NET Framework 4 na serveru. Případně postupujte takto:
> 
> 1. Přidejte následující prvky k `SecurityClasses` kapitoly *webové\_MediumTrust.config* souboru:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Vytvořit novou sadu oprávnění *webové\_MediumTrust.config* soubor s požadovanými oprávněními následující:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Použití oprávnění nastavena na systém SQL Server Compact umístěním následující prvky *webové\_MediumTrust.config* souboru:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problém: Galerie a PhpBB webové aplikace zobrazí chybu "Služba není k dispozici" po publikování

> Za určitých okolností publikování aplikace způsobí chybu "služba není k dispozici".
> 
> **Alternativní řešení**  
> Ve službě WebMatrix, přidejte zpětné lomítko (\) na konec názvu serveru v **nastavení publikování** okna a potom aplikaci znovu publikovat.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problém: Rozložení webu Moodle a odkazy jsou přerušená po publikování

> Po publikování aplikace Moodle aplikace nebude fungovat správně.
> 
> **Alternativní řešení**  
> Ve službě WebMatrix, přidejte na konec lomítko (/) **název lokality** pole **nastavení publikování** okna a potom aplikaci znovu publikovat.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problém: Publikování nopCommerce selže s chybou databáze

> Publikování nopCommerce selže a ohlásí chybu databáze jako "Vložit do nop\_tabulky protokolu se nezdařilo."
> 
> **Alternativní řešení**  
> 
> 1. Ve službě WebMatrix, klikněte na tlačítko **spustit** nopCommerce místně spustíte.
> 2. Přihlaste se na stránku pro správu.
> 3. Klikněte **systému** nabídky.
> 4. Klikněte **protokolu** možnost.
> 5. Klikněte **vymazat protokol** tlačítko.
> 6. NopCommerce znovu publikujte.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problém: Silverstripe CMS zobrazí "HTTP 500 PHP FCGI chyba" při stažení publikované lokality

> **Alternativní řešení**  
> Po kliknutí na tlačítko **stažení publikování serveru**, přeskočte `silverstripe-cache/manifest_main` v **Náhled publikování**. Tento soubor se používá pro ukládání do mezipaměti pro účely a je specifická pro každý počítač.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problém: Subtext zobrazí "Error Server v aplikaci '/'" při stahování publikované lokality

> **Alternativní řešení**  
> Otevřete v lokalitě *web.config* souboru a nahraďte ID uživatele a heslo v připojovací řetězec databáze s přihlašovacími údaji správce systému SQL Server ("sa" přihlašovací údaje).
> 
> Alternativně postupujte podle těchto kroků aby nového uživatelského účtu, který jste se přihlásili `db_owner` oprávnění:
> 
> 1. Nainstalujte SQL Server Management Studio pomocí služby instalace webové platformy.
> 2. Připojte se k místní instanci systému SQL Server Express (ve výchozím nastavení, `.\SQLEXPRESS`).
> 3. Klikněte na tlačítko **databáze** &gt; *[localSubtextDatabase]* &gt; **zabezpečení** &gt; **uživatelé** &gt; *[localSubtextUser*] (výchozí hodnota je `subtextuser`], klikněte pravým tlačítkem a klikněte na tlačítko **vlastnosti**.
> 4. Vyberte **db\_vlastníka** v části role členství.


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


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problém: Některé odkazy nejsou viditelné v DotNetNuke po publikování nebo stahování webu

> Pokud publikujete nebo stáhnout aplikace DotNetNuke lokality, musíte může a vymažte mezipaměť získat nové odkazy na webu.
> 
> **Alternativní řešení**
> 
> 1. Přihlaste se jako "Hostitel".
> 2. Přejděte do nabídky hostitele a vyberte možnost **nastavení hostitele**.
> 3. Přejděte dolů a v části **Upřesnit nastavení**, rozbalte položku **nastavení výkonu**.
> 4. Klikněte **vymazat mezipaměť** odkaz pro stránky.
> 5. Přejděte do dolní části stránky a restartování aplikace.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problém: V AtomSite některé odkazy jsou přerušená po stažení publikované lokality

> **Alternativní řešení**  
> V *service.config* souboru *users.config* soubor a všechny *.xml* soubory, nahraďte řetězce adresy URL (například `http://myhost.com/atomsite`) s místní (například `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problém: Založenou na MySQL aplikace, jako je WordPress nezdaří publikování a ohlásí chybu databáze

> Ve výchozím nastavení nainstaluje služba WebMatrix MySQL s znakové sady UTF-8. Pokud instalujete MySQL sami a není znakovou sadu UTF-8 (například je Latin1), se nemusí podařit proces publikování databází.
> 
> **Alternativní řešení**
> 
> 1. Změňte sadu pro databázi MySQL na UTF-8 znaků. (Podrobnosti najdete v tématu [Server znaková sada a kolace](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) na webu MySQL.)
> 2. Nainstalujte aplikaci znovu.
> 3. Aplikaci znovu publikujte.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problém: "Stažení publikování serveru" selže pro aplikace, které instalační program založené na prohlížeči

> Některé aplikace (například Kentico CMS) vyžadují, abyste je spustit v prohlížeči, aby bylo možné provést po instalaci instalační program, jako je například vytváření databáze. Pokud publikujete aplikaci jako to bez dokončení instalace založené na prohlížeči, pokus o stažení stejné síti ze vzdáleného serveru se nezdaří.
> 
> **Alternativní řešení**  
> Dokončení instalace založené na prohlížeči před publikováním webu.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problém: "Stažení publikování serveru" selže s chybou databáze pro DotNetNuke a Kooboo CMS

> Pokud pokusu o stažení aplikace ze serveru a mít pověření správce v připojovací řetězec databáze **nastavení publikování** dialogové okno, může se zobrazit následující chyby v protokolu publikovat:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Alternativní řešení**  
> Pokud je to možné, znovu publikovat webu (nebo její zveřejnění) pomocí přihlašovacích údajů bez oprávnění správce pro databázi.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Další informace

Další informace o službě WebMatrix 1.0 najdete na následujících webech:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Všechna práva vyhrazena. [Podmínky použití](https://msdn.microsoft.cos/cc300389.aspx).
