---
uid: web-pages/readme/overview
title: WebMatrix – soubor Readme | Dokumentace Microsoftu
author: rick-anderson
description: Služba WebMatrix a ASP.NET Web Pages (Razor) verze 1.0 – soubor Readme
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 924bf04772a1d73c7fdfb1168090daef388438ab
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398797"
---
<a name="webmatrix-readme"></a>WebMatrix – soubor Readme
====================
13. ledna 2011

## <a name="contents"></a>Obsah

> [!NOTE]
> Tento soubor readme platí pro verzi 1.0 služby WebMatrix.


- [Přehled](#Overview)
- [Instalace](#Installation_Notes)
- [Jak publikovat aplikace](#InstructionsForPublishingApplications)
- [Změny a problémy](#ChangesAndIssues)

    - [Instalace služby WebMatrix 1.0](#Known_Issues_Installation)
    - [Webové stránky ASP.NET](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [Služba IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Instalace aplikací](#Known_Issues_Installing_Applications)
    - [Publikování aplikací](#Known_Issues_Publishing_Applications)
- [Další informace](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Přehled

> Microsoft WebMatrix 1.0 je bezplatná webová vývoj zásobník, který nainstaluje během několika minut. Webový server se integruje s databází a programovací rozhraní pro vytvoření jednoho integrovaného rozhraní. Služba WebMatrix můžete použít ke zjednodušení způsobu kódu, testování a publikování vlastní web ASP.NET a PHP, nebo WebMatrix můžete použít ke spuštění nového webu pomocí oblíbených open source aplikace, jako jsou aplikace DotNetNuke, Umbraco, WordPress a Joomla. Služba WebMatrix používá stejný výkonný webový server, databázový stroj a prostředí architektury, které se spustí váš web na Internetu, takže hladký a bezproblémový přechod z vývojového do produkčního prostředí.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalace

> Instalace služby WebMatrix 1.0, je třeba nejprve nainstalovat [Microsoft webové platformy verze 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Po instalaci webové platformy, můžete k instalaci služby WebMatrix.
> 
> Pokud máte problémy během instalace, podívejte se na [Poradce při potížích se instalačního programu webové platformy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Jak publikovat aplikace

> Zobrazit [podrobné pokyny pro publikování aplikací](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Změny a problémy

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problémy 1.0 instalace služby WebMatrix

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problém: WebMatrix 1.0 je k dispozici pouze na platformách, které podporují rozhraní Microsoft .NET Framework 4

> Rozhraní .NET Framework verze 4 je požadované pro službu WebMatrix. V některých případech instalační program služby WebMatrix 1.0 vám umožní instalaci na platformě, která není součástí sady podporované konfigurace. Zejména Windows Vista bez aktualizace SP1 vám umožní začít instalace služby WebMatrix, ale selže a blokovat instalaci komponenty rozhraní .NET Framework 4.
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


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problém: Nelze nainstalovat WebMatrix 1.0, pokud Microsoft Visual Studio 2008 se nainstaluje bez Microsoft Visual Studio 2008 SP1

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

Tato část dokumentu popisuje nové funkce, změny a známé problémy s verzi 1.0 z rozhraní ASP.NET Web Pages se syntaxí Razor.

- [Nové funkce](#NewFeatures)
- [Změny](#Changes)
- [Problémy](#Issues)

#### <a id="NewFeatures"></a>  Nové funkce

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Novinka: Konfigurace nastavení přidá zakázat Správce balíčků

> Nový `asp:AdminManagerEnabled` klíč je k dispozici pro `<appSettings>` prvek *web.config* soubor, který vám umožní zcela zakázat Správce balíčků. Výchozí hodnota pro tento element je true, to znamená, že pokud není součástí *web.config* souboru, Správce balíčků je povolená. Zakázat Správce balíčků, přidejte následující prvek k *web.config* soubor v kořenové složce webu:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  Změny

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Změna: klíč "webPages:AdminFolderVirtualPath" přejmenovat "asp: AdminFolderVirtualPath"

> `webPages:AdminFolderVirtualPath` Klíč, který lze přidat do *web.config* použití byl přejmenován soubor k určení umístění správce balíčků `asp:` obor názvů místo `webPages` oboru názvů. Pokud jste použili tento element, je nutné ho přejmenovat v konfiguračním souboru.


#### <a id="Issues"></a>  Známé problémy

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problém: Hesla uživatelů se členstvím už nebude rozpoznán.

> Algoritmus pro vytváření a ukládání hesel členství (přihlášení) se změnil na zvýšení zabezpečení. V důsledku toho nebude rozpoznána hesla uložená pro členy (uživatele) vytvořené v Beta verzích ASP.NET Razor. 
> 
> **Alternativní řešení** Pokud web není ještě nebyla uvedena do produkčního prostředí, odstranit záznamy uživatelů z databáze členství. Pokud je databáze za provozu, prostřednictvím kódu programu obnovit stávající hesla v databázi členství.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problém: Neočekávané chování při použití vlastní tabulku členství

> K inicializaci zprostředkovatele členství pro web ASP.NET Razor, volání `WebSecurity.InitializeDatabaseConnection` metody. (V nástroji WebMatrix, šablona Starter Site zahrnuje volání této metody v  *\_AppStart.cshtml* souboru.) Pokud `autoCreateTables` parametr této metody je nastaven na hodnotu true (ve výchozím nastavení, je nastavena hodnotu true v šabloně Starter Site), a pokud názvu nerozpoznaný tabulky se předá metodě (druhý parametr), metoda nevyvolá chybu. Místo toho automaticky vytvoří v tabulce.
> 
> Pokud máte v úmyslu použít vlastní uživatelská tabulka pro členství, ale předejte název chybný tabulky pro to může být problém `WebSecurity.InitializeDatabaseConnection` metody. Protože metoda není ve výchozím nastavení vyvolá chybu, pokud neexistuje v tabulce, kterou zadáte, a místo toho vytvoří novou tabulku, můžete aplikace zobrazovat pracovat. Kód aplikace, která se spoléhá na vlastní uživatelská tabulka (a na pole v něm) můžete nakonec selhat s neočekávanými chybami.
> 
> **Alternativní řešení**  
> Ujistěte se, že název předaný `InitializeDatabaseConnection` metoda shody profilu uživatele v databázi členství tabulku nebo Ujistěte se, že `autoCreateTables` parametr je nastaven na hodnotu false.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Problém: Chybová zpráva "modul Správce vyžaduje přístup k ~/App\_dat."

> Za určitých okolností pokusu o vytvoření uživatelů nebo jinak pracovat s systém členství technologie ASP.NET může způsobit stránky zobrazí chybu *modulu Správce vyžaduje přístup k ~/App\_Data*. K tomu dojde, pokud účet, který služba IIS nebo IIS Express je spuštěný pod nemá oprávnění k vytvoření a zápis do *aplikace\_Data* složky v kořenové složky webu. 
> 
> **Alternativní řešení** ručně vytvořit *aplikace\_Data* složky webu. Ujistěte se, zda má účet Windows, na kterém aplikace běží pod (obvykle síťové služby) oprávnění pro čtení a zápisu pro kořenové složky aplikace a podsložky, jako je například aplikace\_Data. Podrobnější informace najdete v článku znalostní báze [problémy se službou SQL Server Express uživatele vytváření instancí a ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problém: "nepovedlo se vygenerovat uživatelskou instanci systému SQL Server" Chyba

> Pokud aplikace Web služby WebMatrix používá systém SQL Server Express a je spuštěna služba IIS 7.5 na Windows 7 nebo Windows Server 2008 R2, může se zobrazit chyba, která označuje, že SQL Server nemůže načíst cestu uživatele místní aplikace v době běhu.
> 
> **Alternativní řešení** Ujistěte se, zda má účet Windows, na kterém aplikace běží pod (obvykle síťové služby), jako oprávnění čtení/zápisu pro kořenové složky aplikace a podsložky *aplikace\_Data*. Podrobnější informace najdete v článku znalostní báze [problémy se službou SQL Server Express uživatele vytváření instancí a ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problém: Soubory, které obsahuje prostředky balíčku správce nebo Správce balíčků hesla jsou servable v rámci služby IIS 6.0 a starší

> Pokud nasazujete aplikaci rozhraní ASP.NET Web Pages (Razor), který byl vytvořen pomocí verze RC2, a pokud aplikace obsahuje soubor *password.txt* nebo *packagesources.txt* soubor */App\_ Data/admin*, IIS 6.0 bude sloužit soubor, pokud o to požádá potenciálně vystavení hesla pro vaši instanci správce balíčků. 
> 
> **Alternativní řešení** přejmenovat *password.txt* nebo *packagesources.txt* do souboru *password.config* nebo *packagesources.config*. Ve výchozím nastavení, služby IIS 6.0 neobsluhuje soubory, které mají *.config* rozšíření. (Ve službě IIS 7, žádné soubory v *aplikace\_Data* složky jsou poskytovány, takže není potřeba přejmenujte soubory.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problém: Probíhá odinstalace balíčků nainstalovat pomocí verze Beta 3 neodebere zcela balení komponent

> Pokud nainstalovali balíček pomocí Správce balíčků ve verzi Beta 3 a potom se pokuste odinstalujte ji pomocí aktuální verze balíčku není úplně odinstalovaná. Pomocí Správce balíčků **odinstalovat** tlačítko odebere některé součásti, ale ponechá kód knihovny balíčku a neaktualizuje *package.config* souboru.
> 
> **Alternativní řešení**   
> Proveďte tyto kroky:  
> 1. Odstranit *aplikace\_Data\packages* složky. Tato operace odebere všechny balíčky.   
> 2. Odstranit *souboru packages.config* soubor v kořenové složce webu.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problém: V sadě Visual Studio, vyvolání Správce balíčků webové trvá aplikace do offline režimu

> Pokud pracujete v sadě Visual Studio (nikoli WebMatrix) a použít  *\_správce* funkce spusťte Správce balíčků sady Visual Studio aplikaci převede do režimu offline a účtuje *aplikace\_ offline.htm* do kořenové složky webu, který naruší vaši schopnost používat Správce balíčků.
> 
> [!NOTE]
> I když obvykle uvidíte toto chování při použití rozhraní Správce balíčků založená na web, ke stejnému chování dochází-li přidat, odebrat nebo změnit libovolné soubory v *aplikace\_Data* složky.
> 
> **Alternativní řešení**   
> K práci s balíčky v sadě Visual Studio, použijte místo Správce balíčků webové rozšíření NuGet. Informace najdete v tématu [dokumentace pro NuGet](https://docs.microsoft.com/nuget/). Pokud pracujete s jinými soubory v *aplikace\_Data* složky, vezměte v úvahu zachovat soubory jinde chcete vyhnout tomuto problému. Pokud to není praktické, odstraňte *aplikace\_offline.htm* soubor ručně nebo počkejte, dokud lokalitu přejde do režimu online automaticky (ve výchozím nastavení po 30 sekundách).


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problém: Visual Studio IntelliSense projektu šablony a k dispozici pouze v architektuře ASP.NET MVC verze 3

> Instalace rozhraní ASP.NET Web Pages také neinstaluje nástroje pro Visual Studio jako je například technologie IntelliSense a projekt šablony pro aplikace ASP.NET Web Pages.
> 
> **Alternativní řešení** používat technologii IntelliSense a projekt šablony pro aplikace webových stránek ASP.NET v sadě Visual Studio, instalace technologie ASP.NET MVC 3 RC, buď prostřednictvím instalace webové platformy nebo [samostatný instalační program](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problém: Informační kanály pro čtení nebo jiných externích dat prostřednictvím serveru proxy

> Pokud serveru se systémem lokality je za proxy serverem, může být nutné nakonfigurovat informace o proxy serveru v *web.config* souboru, aby bylo možné číst informace, které pocházejí z mimo váš web. Například, pokud použijete `ReCaptcha` pomocné rutiny, Pomocník komunikuje se službou nástroje reCAPTCHA, ale mohou být blokovány váš proxy server. Podobně informační kanály, které se používají v ASP.NET Web Pages, jako je například informačního kanálu používat Správce balíčků může vyžadovat konfiguraci proxy serveru.
> 
> Pokud máte potíže v práci s externí služby nebo práce s balíčkem informačního kanálu, vložte následující prvky do vaší aplikační kořen *web.config* souboru:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Další informace o konfiguraci proxy serveru, naleznete v tématu [ &lt;proxy&gt; – Element (nastavení sítě)](https://msdn.microsoft.com/library/sa91de1e.aspx) na webové stránce MSDN.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problém: Odinstalování rozhraní .NET Framework verze 4 zakáže ASP.NET Web Pages se syntaxí Razor

> Pokud odinstalujete rozhraní .NET Framework verze 4 a pak ho znovu nainstalujte, ASP.NET Web Pages se syntaxí Razor je zakázaná. Stránky s *.cshtml* rozšíření správně spustit. Rozhraní ASP.NET Web Pages zaregistruje sestavení v kořenovém adresáři počítač *web.config* souboru a odebírá se rozhraní .NET Framework odebere tento soubor. Opětovná instalace rozhraní .NET Framework nainstalovat novou verzi konfiguračního souboru, ale nepřidá odkaz na sestavení rozhraní ASP.NET Web Pages.
> 
> **Alternativní řešení** po opětovné instalaci rozhraní .NET Framework, přeinstalujte rozhraní ASP.NET Web Pages se syntaxí Razor. Tím se přidá následující element na *web.config* souboru v kořenovém adresáři počítač, který je obvykle v následujícím umístění:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


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
> - Pokud nemáte kontrolu nad do počítače serveru (například nasazujete hostující web), přidejte následující na web *web.config* souboru: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problém: Nasazení aplikace do počítače, na kterém není SQL Server Compact nainstalovaná

> Aplikace, které zahrnují databáze systému SQL Server Compact můžete spustit na počítači, kde SQL Server Compact není nainstalovaná. Microsoft WebMatrix 1.0 automaticky za vás zkopíruje tyto binární soubory a provede příslušné *web.config* souborů transformace.
> 
> **Alternativní řešení** potřebujete zkopírujte tyto soubory a nastavte *web.config* změny v souboru ručně, postupujte takto:
> 
> 1. Kopírovat sestavení modulu databáze, které chcete *Bin* složku (a její podsložky) aplikace v cílovém počítači:  
> 
>    - Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **k** *\Bin*
>    - Kopírování <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>k</em></strong>\Bin\x86*
>    - Kopírování <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>k</strong><em>\Bin\amd64</em>
> 
> 2. V kořenové složce webové stránky, vytvořte nebo otevřete *web.config* souboru. (Ve službě WebMatrix 1.0 je k dispozici, pokud kliknete na tento typ souboru **všechny** v **vyberte typ souboru** dialogové okno.)
> 3. Přidejte následující prvek jako podřízený objekt `<configuration>` – element (ne uvnitř `<system.web>` element):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problém: "Databázi" a "WebGrid" Pomocníci nefungují na úrovni Medium Trust v jazyce Visual Basic

> Pokud používáte Visual Basic (vytváření *.vbhtml* soubory), `Database` a `WebGrid` pomocné rutiny nebude fungovat, pokud aplikace je nastaveno pro použití úrovni Medium Trust.
> 
> **Alternativní řešení**  
> Pokud používáte Visual Studio 2010, lze vyřešit tento problém instalací verze aktualizace Service Pack 1. Dokud je k dispozici konečná verze na verzi SP1, můžete stáhnout Beta verzi SP1 [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) stránky na webu Microsoft Download Center.   
>   
> Pokud je toto nepraktické, nebo pokud je velmi riskantní používat Visual Studio 2010, můžete dočasně nastavit aplikace používat plnou důvěryhodnost.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problém: "ApplicationPart" prostředky jsou externě dostupný.

> Pokud sestavení obsahuje objekty, které je odvozen od `ApplicationPart` třídy vystavené prostředků sestavení `ResourceRouteHandler` třídy. Představte si třeba následující adresu URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Tento požadavek stáhne všechny zdrojové řetězce v *System.Web.WebPages.Administration.dll* sestavení. Všechny vložené prostředky (včetně těch, které nejsou určené ke zpracování jako statický obsah), budou staženy. Pokud vložené prostředky obsahují citlivé informace, to může představovat bezpečnostní riziko. 
> 
> **Alternativní řešení**   
> Pokud vytvoříte **ApplicationPart** objektu, ujistěte se, že vložené prostředky spojené s, která **ApplicationPart** objektu sestavení neobsahují citlivé informace.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Informace o problémech s instalací pro službu WebMatrix najdete v tématu [problémy instalace služby WebMatrix](#Known_Issues_Installation) výše v tomto dokumentu.


Tato část dokumentu popisuje známé problémy pro vývojové prostředí WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problém: Změny v zadané uživatelské jméno nebo heslo připojovacího řetězce databáze v souboru web.config se neprojeví v pracovním prostoru databází

> **Alternativní řešení**  
> 
> 1. V *web.config* změňte název databáze v připojovacím řetězci (například přidat "1" do něj).
> 2. Uložit *web.config* souboru.
> 3. Klikněte na tlačítko **databází** a aktualizovat.
> 4. Změnit název databáze v připojovacím řetězci v *web.config* souboru zpět na původní název databáze.
> 5. Uložit *web.config* souboru.
> 6. Klikněte na tlačítko **databází** a aktualizovat.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problém: Nelze odstranit složky vytvořené pomocí služby WebMatrix

> Pokud služba WebMatrix, spuštěná pomocí zvýšenou úroveň oprávnění (to znamená, že jste spustili pomocí služby WebMatrix **spustit jako správce** možnost ve Windows), složky, které jsou vytvořeny pomocí služby WebMatrix nelze odstranit pomocí Průzkumníka Windows.
> 
> **Alternativní řešení**  
> Spusťte Windows Explorer pomocí zvýšenou úroveň oprávnění. Postupujte podle těchto kroků:  
> 
> 1. Ve Windows, klikněte na tlačítko **Start**.
> 2. Zadejte "Windows Explorer" a klikněte pravým tlačítkem na položku **Windows Explorer**.
> 3. Klikněte na tlačítko **spustit jako správce**. Odstraňte složky.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problém: Služba WebMatrix 1.0 není schopen provést určité úlohy, které vyžadují ke zvýšení úrovně oprávnění

> Služba WebMatrix 1.0 není schopen provádění určitých úloh vyžadujících zvýšená oprávnění, například při instalaci dalších komponent v následujících situacích:
> 
> - U Windows Vista nebo Windows 7 jsou přihlášeni pomocí účtu, který nemá oprávnění pro správu a řízení uživatelských účtů (UAC) je zakázán.
> - Používání systému Microsoft Windows XP nebo Microsoft Windows Server 2003.
> 
> **Alternativní řešení**  
> Většinu úkolů ve službě WebMatrix 1.0 se nevyžaduje oprávnění správce. Pro ty, které udělat můžete provádět operace jako správce nebo postupujte podle těchto kroků:
> 
> - U Windows Vista nebo Windows 7 povolení nástroje Řízení uživatelských účtů.
> - Windows XP přidejte uživatele do skupiny zabezpečení Administrators.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problém: "Webu z Galerie webových" je zakázaná.

> **Webu z Galerie webových** možnost je zakázaná, pokud není nainstalovaný 3.0 Instalační služby webové platformy.
> 
> **Alternativní řešení**  
> Nainstalujte [instalace webové platformy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


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


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problém: Technologie IntelliSense není k dispozici v nástroji WebMatrix pro Razor syntaxe jazyka C# a Visual Basic

> Technologie IntelliSense je podporována v nástroji WebMatrix pro HTML a CSS. To však není k dispozici pro jiné jazyky. 
> 
> **Alternativní řešení**   
> Žádné


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problém: IntelliSense pro HTML a CSS navrhuje prvky, které nejsou kontextově vhodné

> Technologie IntelliSense pro kód ve službě WebMatrix podporuje HTML pomocí [XHTML 1.0 přechodné schématu](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) a pomocí šablon stylů CSS [schématu CSS 2.1](http://www.w3.org/TR/CSS2/). Protože technologie IntelliSense je založena na tato konkrétní schémata, určité značek, atributy a vlastnosti může být určeno, které nejsou vhodné pro aktuální stránku nebo styl definice. Pro kód HTML může také vést k neočekávaným návrhů v obsahu, který může být interpretován jako poškozený XHTML (například, když nebyly uzavřeny značky). Tento problém může být více patrné, pokud je kurzor na místo uvnitř značku neúplné; v takovém případě může navrhnout nové počáteční značky technologie IntelliSense nebo tuto nabídku Další nesprávné návrhy. 
> 
> **Alternativní řešení**   
> Pro kód HTML Ujistěte se, že pracujete v rámci stránky XHTML ve správném formátu a kompletní. Šablony stylů CSS neexistuje žádné alternativní řešení.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problém: Technologie IntelliSense není vyvolána, když zadáte

> V některých případech technologie IntelliSense nemusí vyvolat při zadávání HTML a CSS v editoru. Konkrétně to může dojít, pokud je kurzor přímo vedle jiný element nebo na konci souboru. 
> 
> **Alternativní řešení**   
> Ujistěte se, že je prázdný znak kolem kurzoru a kurzor není na konci souboru. Technologie IntelliSense můžete také vyvolat ručně stisknutím kombinace kláves Ctrl + mezerník.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problém: Žádné uživatelské rozhraní je k dispozici pro zakázání technologie IntelliSense

> 1.0 pro službu WebMatrix poskytuje bez uživatelského rozhraní nebo gesta pro zakázání technologie IntelliSense. 
> 
> **Alternativní řešení**   
> Spusťte službu WebMatrix pomocí následujícího příkazu, která zahrnuje přepínač, který zakáže IntelliSense:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>Služba IIS Express

Služba IIS Express má svůj vlastní soubor readme, který je k dispozici na na následující adrese URL:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact má svůj vlastní soubor readme, který je k dispozici na na následující adrese URL:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Informace o problémech, které se týkají instalace systému SQL Server Compact jako součást nástroje WebMatrix, najdete v tématu [problémy instalace služby WebMatrix](#Known_Issues_Installation) výše v tomto dokumentu.

### <a id="Known_Issues_Installing_Applications"></a>  Instalace aplikací

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problém: Instalace aplikace může trvat dlouhou dobu Pokud složku Dokumenty uživatele přesměruje na sdílené síťové složky

> **Alternativní řešení**  
> Žádné Aplikace může nějakou dobu instalace, ale nainstaluje správně.


### <a id="Known_Issues_Publishing_Applications"></a>  Publikování aplikací

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problém: "požadovaná oprávnění nelze získat" chyby při publikování databáze SQL Compact

> Služba WebMatrix plně nepodporuje nasazování podpůrné binárních souborů pro SQL Server Compact do serveru, na kterém běží aplikace rozhraní .NET Framework verze 3.5 s konfigurací úrovni medium trust.
> 
> **Alternativní řešení**  
> Vhodnějším řešením je instalace rozhraní .NET Framework 4 na serveru. Případně postupujte takto:
> 
> 1. Přidejte následující prvky, které mají `SecurityClasses` tématu *webové\_MediumTrust.config* souboru:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Vytvořit novou sadu oprávnění *webové\_MediumTrust.config* soubor s následující požadovaná oprávnění:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Použití oprávnění nastaveno na SQL Server Compact tak, že vložíte následující prvky *webové\_MediumTrust.config* souboru:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problém: Galerie a systém PhpBB webové aplikace zobrazí chybu "Služba není k dispozici" po publikování

> Za určitých okolností publikování aplikace způsobí chybu "služba není k dispozici".
> 
> **Alternativní řešení**  
> V nástroji WebMatrix, přidat zpětné lomítko (\) na konec názvu serveru v **nastavení publikování** okno a potom aplikaci znovu publikovat.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problém: Moodle webu rozložení a propojení se přeruší po publikování

> Po publikování aplikace Moodle aplikace nebude fungovat správně.
> 
> **Alternativní řešení**  
> V nástroji WebMatrix, přidejte na konec lomítka (/) **název lokality** pole **nastavení publikování** okno a potom aplikaci znovu publikovat.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problém: Publikování nopCommerce selže s chybou databáze

> Publikování nopCommerce selže a oznámí chyby databáze jako "použít příkaz Insert nop\_tabulky protokolu se nezdařilo."
> 
> **Alternativní řešení**  
> 
> 1. V nástroji WebMatrix, klikněte na tlačítko **spustit** nopCommerce místně spustíte.
> 2. Přihlaste se ke stránce pro správu.
> 3. Klikněte na tlačítko **systému** nabídky.
> 4. Klikněte na tlačítko **protokolu** možnost.
> 5. Klikněte na tlačítko **vymazat protokol** tlačítko.
> 6. NopCommerce znovu publikujte.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problém: Silverstripe CMS zobrazí "HTTP 500 PHP FCGI chyba" při stažení publikovaného webu

> **Alternativní řešení**  
> Po kliknutí na **stahování publikování webu**, přeskočte `silverstripe-cache/manifest_main` v **publikování náhledu**. Tento soubor se používá pro ukládání do mezipaměti účely a je specifická pro každý počítač.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problém: Zobrazí tuto část získat "Chyba serveru v aplikaci"/"" při stahování publikovaného webu

> **Alternativní řešení**  
> Otevřete webu *web.config* souboru a nahraďte ID uživatele a heslo v připojovací řetězec databáze pomocí přihlašovacích údajů správce serveru SQL Server (přihlašovací údaje "sa").
> 
> Případně postupujte podle těchto kroků k uživatelskému účtu jste přihlášeni s `db_owner` oprávnění:
> 
> 1. Nainstalujte SQL Server Management Studio pomocí instalačního programu webové platformy.
> 2. Připojte se k místní instanci systému SQL Server Express (ve výchozím nastavení, `.\SQLEXPRESS`).
> 3. Klikněte na tlačítko **databází** &gt; *[localSubtextDatabase]* &gt; **zabezpečení** &gt; **uživatelé** &gt; *[localSubtextUser*] (výchozí hodnota je `subtextuser`], klikněte pravým tlačítkem a klikněte na tlačítko **vlastnosti**.
> 4. Vyberte **db\_vlastníka** v části role členství.


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


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problém: Některé odkazy nejsou viditelné v DotNetNuke po publikování nebo stahování webu

> Je-li publikovat nebo stáhnout aplikace DotNetNuke lokality, můžete potřebovat vymazat mezipaměť získat nové odkazy se zobrazí na webu.
> 
> **Alternativní řešení**
> 
> 1. Přihlaste se jako "Hostitel".
> 2. Přejděte do nabídky hostitele a vyberte **nastavení hostitele**.
> 3. Přejděte dolů a v části **Upřesnit nastavení**, rozbalte **nastavení výkonu**.
> 4. Klikněte na tlačítko **vymazat mezipaměť** odkaz pro stránky.
> 5. Přejděte do dolní části stránky a restartovat aplikaci.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problém: Některé odkazy v AtomSite se přeruší po stažení publikovaného webu

> **Alternativní řešení**  
> V *service.config* souboru *users.config* soubor a všechny *.xml* soubory, nahraďte řetězec adresy URL (například `http://myhost.com/atomsite`) s místní (například `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problém: Selhání aplikací založenou na MySQL, jako je WordPress k publikování a nahlaste chybu databáze

> Ve výchozím nastavení nainstaluje služba WebMatrix MySQL pomocí znakové sady UTF-8. Pokud instalace MySQL na vlastní a není znakové sady UTF-8 (například je Latin1), může selhat proces publikování databáze.
> 
> **Alternativní řešení**
> 
> 1. Změňte znakovou sadu pro MySQL na UTF-8. (Podrobnosti najdete v tématu [Server znakové sady a kolaci](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) na webu MySQL.)
> 2. Nainstalujte aplikaci znovu.
> 3. Publikování aplikace.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problém: "Download publikování webu" selže u aplikací, které mají nastavení založené na prohlížeči

> Některé aplikace (například Kentico CMS) vyžadují, abyste je spuštění v prohlížeči, aby bylo možné provést nastavení po instalaci, jako je vytvoření databáze. Pokud publikujete třeba aplikaci tímto způsobem bez dokončení instalace založené na prohlížeči, se nezdaří pokus o stažení stejného serveru ze vzdáleného serveru.
> 
> **Alternativní řešení**  
> Dokončete nastavení založené na prohlížeči před publikováním webu.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problém: "Download publikování webu" selže s chybou databáze DotNetNuke a Kooboo CMS

> Pokud pokusu o stažení aplikace ze serveru a mít pověření správce připojovacího řetězce databáze v **nastavení publikování** dialogového okna, může se zobrazit následující chybu v protokolu publikovat:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Alternativní řešení**  
> Pokud je to praktické, znovu publikovat webu (nebo jeho publikování) pomocí přihlašovacích údajů bez oprávnění správce pro databázi.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Další informace

Další informace o službě WebMatrix 1.0 naleznete na následujících webech:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Všechna práva vyhrazena. [Podmínky použití](https://msdn.microsoft.cos/cc300389.aspx).
