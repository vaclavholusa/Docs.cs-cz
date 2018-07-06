---
uid: web-pages/readme/overview
title: WebMatrix – soubor Readme | Dokumentace Microsoftu
author: rick-anderson
description: Služba WebMatrix a ASP.NET Web Pages (Razor) verze 1.0 – soubor Readme
ms.author: aspnetcontent
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 8aa0eedb3156ca33905c106b72d5fa553a917b16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822859"
---
<a name="webmatrix-readme"></a><span data-ttu-id="fac86-103">WebMatrix – soubor Readme</span><span class="sxs-lookup"><span data-stu-id="fac86-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="fac86-104">13. ledna 2011</span><span class="sxs-lookup"><span data-stu-id="fac86-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="fac86-105">Obsah</span><span class="sxs-lookup"><span data-stu-id="fac86-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="fac86-106">Tento soubor readme platí pro verzi 1.0 služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="fac86-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="fac86-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="fac86-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="fac86-108">Instalace</span><span class="sxs-lookup"><span data-stu-id="fac86-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="fac86-109">Jak publikovat aplikace</span><span class="sxs-lookup"><span data-stu-id="fac86-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="fac86-110">Změny a problémy</span><span class="sxs-lookup"><span data-stu-id="fac86-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="fac86-111">Instalace služby WebMatrix 1.0</span><span class="sxs-lookup"><span data-stu-id="fac86-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="fac86-112">Webové stránky ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fac86-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="fac86-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="fac86-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="fac86-114">Služba IIS Express</span><span class="sxs-lookup"><span data-stu-id="fac86-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="fac86-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="fac86-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="fac86-116">Instalace aplikací</span><span class="sxs-lookup"><span data-stu-id="fac86-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="fac86-117">Publikování aplikací</span><span class="sxs-lookup"><span data-stu-id="fac86-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="fac86-118">Další informace</span><span class="sxs-lookup"><span data-stu-id="fac86-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="fac86-119">Přehled</span><span class="sxs-lookup"><span data-stu-id="fac86-119">Overview</span></span>

> <span data-ttu-id="fac86-120">Microsoft WebMatrix 1.0 je bezplatná webová vývoj zásobník, který nainstaluje během několika minut.</span><span class="sxs-lookup"><span data-stu-id="fac86-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="fac86-121">Webový server se integruje s databází a programovací rozhraní pro vytvoření jednoho integrovaného rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fac86-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="fac86-122">Služba WebMatrix můžete použít ke zjednodušení způsobu kódu, testování a publikování vlastní web ASP.NET a PHP, nebo WebMatrix můžete použít ke spuštění nového webu pomocí oblíbených open source aplikace, jako jsou aplikace DotNetNuke, Umbraco, WordPress a Joomla.</span><span class="sxs-lookup"><span data-stu-id="fac86-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="fac86-123">Služba WebMatrix používá stejný výkonný webový server, databázový stroj a prostředí architektury, které se spustí váš web na Internetu, takže hladký a bezproblémový přechod z vývojového do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="fac86-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="fac86-124">Instalace</span><span class="sxs-lookup"><span data-stu-id="fac86-124">Installation</span></span>

> <span data-ttu-id="fac86-125">Instalace služby WebMatrix 1.0, je třeba nejprve nainstalovat [Microsoft webové platformy verze 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="fac86-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="fac86-126">Po instalaci webové platformy, můžete k instalaci služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="fac86-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="fac86-127">Pokud máte problémy během instalace, podívejte se na [Poradce při potížích se instalačního programu webové platformy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="fac86-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="fac86-128">Jak publikovat aplikace</span><span class="sxs-lookup"><span data-stu-id="fac86-128">How to Publish Applications</span></span>

> <span data-ttu-id="fac86-129">Zobrazit [podrobné pokyny pro publikování aplikací](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="fac86-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="fac86-130">Změny a problémy</span><span class="sxs-lookup"><span data-stu-id="fac86-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="fac86-131">Problémy 1.0 instalace služby WebMatrix</span><span class="sxs-lookup"><span data-stu-id="fac86-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="fac86-132">Problém: WebMatrix 1.0 je k dispozici pouze na platformách, které podporují rozhraní Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="fac86-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="fac86-133">Rozhraní .NET Framework verze 4 je požadované pro službu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="fac86-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="fac86-134">V některých případech instalační program služby WebMatrix 1.0 vám umožní instalaci na platformě, která není součástí sady podporované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fac86-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="fac86-135">Zejména Windows Vista bez aktualizace SP1 vám umožní začít instalace služby WebMatrix, ale selže a blokovat instalaci komponenty rozhraní .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="fac86-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="fac86-136">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-136">**Workaround**</span></span>  
> <span data-ttu-id="fac86-137">Nainstalujte na podporované platformě, která zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="fac86-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="fac86-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="fac86-138">Windows 7</span></span>
> - <span data-ttu-id="fac86-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="fac86-139">Windows Server 2008</span></span>
> - <span data-ttu-id="fac86-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="fac86-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="fac86-141">Windows Vista SP1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="fac86-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="fac86-142">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="fac86-142">Windows XP SP3</span></span>
> - <span data-ttu-id="fac86-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="fac86-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="fac86-144">Problém: Nelze nainstalovat WebMatrix 1.0, pokud Microsoft Visual Studio 2008 se nainstaluje bez Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="fac86-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="fac86-145">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-145">**Workaround**</span></span>  
> <span data-ttu-id="fac86-146">Nainstalujte [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) z webu Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="fac86-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="fac86-147">Problém: V mezipaměti GAC nejsou nainstalovány některé sestavení pro SQL Server Compact 4.0</span><span class="sxs-lookup"><span data-stu-id="fac86-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="fac86-148">Spravovaná sestavení pro SQL Server Compact 4.0 nejsou umístěny v globální mezipaměti sestavení (GAC), když nainstalujete SQL Server Compact 4.0 na 64bitovém počítači a že má počítač pouze .NET Framework 3.5 SP1 Client Profile nainstalované.</span><span class="sxs-lookup"><span data-stu-id="fac86-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="fac86-149">Spravovaná sestavení, které nejsou nainstalovány v GAC jsou:</span><span class="sxs-lookup"><span data-stu-id="fac86-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="fac86-150">*System.Data.SqlServerCe.dll* (poskytovatele ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="fac86-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="fac86-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span><span class="sxs-lookup"><span data-stu-id="fac86-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="fac86-152">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-152">**Workaround**</span></span>  
> <span data-ttu-id="fac86-153">Odinstalujte systém SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="fac86-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="fac86-154">Stáhněte a nainstalujte plnou verzi rozhraní .NET Framework 3.5 SP1 v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="fac86-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="fac86-155">Microsoft .NET Framework 3.5 Service pack 1 (úplný)</span><span class="sxs-lookup"><span data-stu-id="fac86-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="fac86-156">Potom znovu nainstalujte SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="fac86-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="fac86-157">Problém: Nelze odinstalovat, SQL Server Compact pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="fac86-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="fac86-158">Odinstalace systému SQL Server Compact prostřednictvím parametrů příkazového řádku v této verzi nefunguje.</span><span class="sxs-lookup"><span data-stu-id="fac86-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="fac86-159">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-159">**Workaround**</span></span>  
> <span data-ttu-id="fac86-160">Použití *programy a funkce* v Ovládacích panelech Windows k odinstalaci serveru Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="fac86-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="fac86-161">ASP.NET – webové stránky</span><span class="sxs-lookup"><span data-stu-id="fac86-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="fac86-162">Tato část dokumentu popisuje nové funkce, změny a známé problémy s verzi 1.0 z rozhraní ASP.NET Web Pages se syntaxí Razor.</span><span class="sxs-lookup"><span data-stu-id="fac86-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="fac86-163">Nové funkce</span><span class="sxs-lookup"><span data-stu-id="fac86-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="fac86-164">Změny</span><span class="sxs-lookup"><span data-stu-id="fac86-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="fac86-165">Problémy</span><span class="sxs-lookup"><span data-stu-id="fac86-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a>  <span data-ttu-id="fac86-166">Nové funkce</span><span class="sxs-lookup"><span data-stu-id="fac86-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="fac86-167">Novinka: Konfigurace nastavení přidá zakázat Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="fac86-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="fac86-168">Nový `asp:AdminManagerEnabled` klíč je k dispozici pro `<appSettings>` prvek *web.config* soubor, který vám umožní zcela zakázat Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="fac86-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="fac86-169">Výchozí hodnota pro tento element je true, to znamená, že pokud není součástí *web.config* souboru, Správce balíčků je povolená.</span><span class="sxs-lookup"><span data-stu-id="fac86-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="fac86-170">Zakázat Správce balíčků, přidejte následující prvek k *web.config* soubor v kořenové složce webu:</span><span class="sxs-lookup"><span data-stu-id="fac86-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  <span data-ttu-id="fac86-171">Změny</span><span class="sxs-lookup"><span data-stu-id="fac86-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="fac86-172">Změna: klíč "webPages:AdminFolderVirtualPath" přejmenovat "asp: AdminFolderVirtualPath"</span><span class="sxs-lookup"><span data-stu-id="fac86-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="fac86-173">`webPages:AdminFolderVirtualPath` Klíč, který lze přidat do *web.config* použití byl přejmenován soubor k určení umístění správce balíčků `asp:` obor názvů místo `webPages` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="fac86-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="fac86-174">Pokud jste použili tento element, je nutné ho přejmenovat v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="fac86-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a>  <span data-ttu-id="fac86-175">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="fac86-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="fac86-176">Problém: Hesla uživatelů se členstvím už nebude rozpoznán.</span><span class="sxs-lookup"><span data-stu-id="fac86-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="fac86-177">Algoritmus pro vytváření a ukládání hesel členství (přihlášení) se změnil na zvýšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fac86-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="fac86-178">V důsledku toho nebude rozpoznána hesla uložená pro členy (uživatele) vytvořené v Beta verzích ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="fac86-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="fac86-179">**Alternativní řešení** Pokud web není ještě nebyla uvedena do produkčního prostředí, odstranit záznamy uživatelů z databáze členství.</span><span class="sxs-lookup"><span data-stu-id="fac86-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="fac86-180">Pokud je databáze za provozu, prostřednictvím kódu programu obnovit stávající hesla v databázi členství.</span><span class="sxs-lookup"><span data-stu-id="fac86-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="fac86-181">Problém: Neočekávané chování při použití vlastní tabulku členství</span><span class="sxs-lookup"><span data-stu-id="fac86-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="fac86-182">K inicializaci zprostředkovatele členství pro web ASP.NET Razor, volání `WebSecurity.InitializeDatabaseConnection` metody.</span><span class="sxs-lookup"><span data-stu-id="fac86-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="fac86-183">(V nástroji WebMatrix, šablona Starter Site zahrnuje volání této metody v  *\_AppStart.cshtml* souboru.) Pokud `autoCreateTables` parametr této metody je nastaven na hodnotu true (ve výchozím nastavení, je nastavena hodnotu true v šabloně Starter Site), a pokud názvu nerozpoznaný tabulky se předá metodě (druhý parametr), metoda nevyvolá chybu.</span><span class="sxs-lookup"><span data-stu-id="fac86-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="fac86-184">Místo toho automaticky vytvoří v tabulce.</span><span class="sxs-lookup"><span data-stu-id="fac86-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="fac86-185">Pokud máte v úmyslu použít vlastní uživatelská tabulka pro členství, ale předejte název chybný tabulky pro to může být problém `WebSecurity.InitializeDatabaseConnection` metody.</span><span class="sxs-lookup"><span data-stu-id="fac86-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="fac86-186">Protože metoda není ve výchozím nastavení vyvolá chybu, pokud neexistuje v tabulce, kterou zadáte, a místo toho vytvoří novou tabulku, můžete aplikace zobrazovat pracovat.</span><span class="sxs-lookup"><span data-stu-id="fac86-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="fac86-187">Kód aplikace, která se spoléhá na vlastní uživatelská tabulka (a na pole v něm) můžete nakonec selhat s neočekávanými chybami.</span><span class="sxs-lookup"><span data-stu-id="fac86-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="fac86-188">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-188">**Workaround**</span></span>  
> <span data-ttu-id="fac86-189">Ujistěte se, že název předaný `InitializeDatabaseConnection` metoda shody profilu uživatele v databázi členství tabulku nebo Ujistěte se, že `autoCreateTables` parametr je nastaven na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="fac86-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="fac86-190">Problém: Chybová zpráva "modul Správce vyžaduje přístup k ~/App\_dat."</span><span class="sxs-lookup"><span data-stu-id="fac86-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="fac86-191">Za určitých okolností pokusu o vytvoření uživatelů nebo jinak pracovat s systém členství technologie ASP.NET může způsobit stránky zobrazí chybu *modulu Správce vyžaduje přístup k ~/App\_Data*.</span><span class="sxs-lookup"><span data-stu-id="fac86-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="fac86-192">K tomu dojde, pokud účet, který služba IIS nebo IIS Express je spuštěný pod nemá oprávnění k vytvoření a zápis do *aplikace\_Data* složky v kořenové složky webu.</span><span class="sxs-lookup"><span data-stu-id="fac86-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="fac86-193">**Alternativní řešení** ručně vytvořit *aplikace\_Data* složky webu.</span><span class="sxs-lookup"><span data-stu-id="fac86-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="fac86-194">Ujistěte se, zda má účet Windows, na kterém aplikace běží pod (obvykle síťové služby) oprávnění pro čtení a zápisu pro kořenové složky aplikace a podsložky, jako je například aplikace\_Data.</span><span class="sxs-lookup"><span data-stu-id="fac86-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="fac86-195">Podrobnější informace najdete v článku znalostní báze [problémy se službou SQL Server Express uživatele vytváření instancí a ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="fac86-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="fac86-196">Problém: "nepovedlo se vygenerovat uživatelskou instanci systému SQL Server" Chyba</span><span class="sxs-lookup"><span data-stu-id="fac86-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="fac86-197">Pokud aplikace Web služby WebMatrix používá systém SQL Server Express a je spuštěna služba IIS 7.5 na Windows 7 nebo Windows Server 2008 R2, může se zobrazit chyba, která označuje, že SQL Server nemůže načíst cestu uživatele místní aplikace v době běhu.</span><span class="sxs-lookup"><span data-stu-id="fac86-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="fac86-198">**Alternativní řešení** Ujistěte se, zda má účet Windows, na kterém aplikace běží pod (obvykle síťové služby), jako oprávnění čtení/zápisu pro kořenové složky aplikace a podsložky *aplikace\_Data*.</span><span class="sxs-lookup"><span data-stu-id="fac86-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="fac86-199">Podrobnější informace najdete v článku znalostní báze [problémy se službou SQL Server Express uživatele vytváření instancí a ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="fac86-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="fac86-200">Problém: Soubory, které obsahuje prostředky balíčku správce nebo Správce balíčků hesla jsou servable v rámci služby IIS 6.0 a starší</span><span class="sxs-lookup"><span data-stu-id="fac86-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="fac86-201">Pokud nasazujete aplikaci rozhraní ASP.NET Web Pages (Razor), který byl vytvořen pomocí verze RC2, a pokud aplikace obsahuje soubor *password.txt* nebo *packagesources.txt* soubor */App\_ Data/admin*, IIS 6.0 bude sloužit soubor, pokud o to požádá potenciálně vystavení hesla pro vaši instanci správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="fac86-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="fac86-202">**Alternativní řešení** přejmenovat *password.txt* nebo *packagesources.txt* do souboru *password.config* nebo *packagesources.config*. Ve výchozím nastavení, služby IIS 6.0 neobsluhuje soubory, které mají *.config* rozšíření.</span><span class="sxs-lookup"><span data-stu-id="fac86-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="fac86-203">(Ve službě IIS 7, žádné soubory v *aplikace\_Data* složky jsou poskytovány, takže není potřeba přejmenujte soubory.)</span><span class="sxs-lookup"><span data-stu-id="fac86-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="fac86-204">Problém: Probíhá odinstalace balíčků nainstalovat pomocí verze Beta 3 neodebere zcela balení komponent</span><span class="sxs-lookup"><span data-stu-id="fac86-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="fac86-205">Pokud nainstalovali balíček pomocí Správce balíčků ve verzi Beta 3 a potom se pokuste odinstalujte ji pomocí aktuální verze balíčku není úplně odinstalovaná.</span><span class="sxs-lookup"><span data-stu-id="fac86-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="fac86-206">Pomocí Správce balíčků **odinstalovat** tlačítko odebere některé součásti, ale ponechá kód knihovny balíčku a neaktualizuje *package.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="fac86-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="fac86-207">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="fac86-207">**Workaround** </span></span>  
> <span data-ttu-id="fac86-208">Proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="fac86-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="fac86-209">Odstranit *aplikace\_Data\packages* složky.</span><span class="sxs-lookup"><span data-stu-id="fac86-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="fac86-210">Tato operace odebere všechny balíčky.</span><span class="sxs-lookup"><span data-stu-id="fac86-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="fac86-211">Odstranit *souboru packages.config* soubor v kořenové složce webu.</span><span class="sxs-lookup"><span data-stu-id="fac86-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="fac86-212">Problém: V sadě Visual Studio, vyvolání Správce balíčků webové trvá aplikace do offline režimu</span><span class="sxs-lookup"><span data-stu-id="fac86-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="fac86-213">Pokud pracujete v sadě Visual Studio (nikoli WebMatrix) a použít  *\_správce* funkce spusťte Správce balíčků sady Visual Studio aplikaci převede do režimu offline a účtuje *aplikace\_ offline.htm* do kořenové složky webu, který naruší vaši schopnost používat Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="fac86-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="fac86-214">I když obvykle uvidíte toto chování při použití rozhraní Správce balíčků založená na web, ke stejnému chování dochází-li přidat, odebrat nebo změnit libovolné soubory v *aplikace\_Data* složky.</span><span class="sxs-lookup"><span data-stu-id="fac86-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="fac86-215">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="fac86-215">**Workaround** </span></span>  
> <span data-ttu-id="fac86-216">K práci s balíčky v sadě Visual Studio, použijte místo Správce balíčků webové rozšíření NuGet.</span><span class="sxs-lookup"><span data-stu-id="fac86-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="fac86-217">Informace najdete v tématu [dokumentace pro NuGet](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="fac86-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="fac86-218">Pokud pracujete s jinými soubory v *aplikace\_Data* složky, vezměte v úvahu zachovat soubory jinde chcete vyhnout tomuto problému.</span><span class="sxs-lookup"><span data-stu-id="fac86-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="fac86-219">Pokud to není praktické, odstraňte *aplikace\_offline.htm* soubor ručně nebo počkejte, dokud lokalitu přejde do režimu online automaticky (ve výchozím nastavení po 30 sekundách).</span><span class="sxs-lookup"><span data-stu-id="fac86-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="fac86-220">Problém: Visual Studio IntelliSense projektu šablony a k dispozici pouze v architektuře ASP.NET MVC verze 3</span><span class="sxs-lookup"><span data-stu-id="fac86-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="fac86-221">Instalace rozhraní ASP.NET Web Pages také neinstaluje nástroje pro Visual Studio jako je například technologie IntelliSense a projekt šablony pro aplikace ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="fac86-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="fac86-222">**Alternativní řešení** používat technologii IntelliSense a projekt šablony pro aplikace webových stránek ASP.NET v sadě Visual Studio, instalace technologie ASP.NET MVC 3 RC, buď prostřednictvím instalace webové platformy nebo [samostatný instalační program](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="fac86-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="fac86-223">Problém: Informační kanály pro čtení nebo jiných externích dat prostřednictvím serveru proxy</span><span class="sxs-lookup"><span data-stu-id="fac86-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="fac86-224">Pokud serveru se systémem lokality je za proxy serverem, může být nutné nakonfigurovat informace o proxy serveru v *web.config* souboru, aby bylo možné číst informace, které pocházejí z mimo váš web.</span><span class="sxs-lookup"><span data-stu-id="fac86-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="fac86-225">Například, pokud použijete `ReCaptcha` pomocné rutiny, Pomocník komunikuje se službou nástroje reCAPTCHA, ale mohou být blokovány váš proxy server.</span><span class="sxs-lookup"><span data-stu-id="fac86-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="fac86-226">Podobně informační kanály, které se používají v ASP.NET Web Pages, jako je například informačního kanálu používat Správce balíčků může vyžadovat konfiguraci proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="fac86-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="fac86-227">Pokud máte potíže v práci s externí služby nebo práce s balíčkem informačního kanálu, vložte následující prvky do vaší aplikační kořen *web.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="fac86-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="fac86-228">Další informace o konfiguraci proxy serveru, naleznete v tématu [ &lt;proxy&gt; – Element (nastavení sítě)](https://msdn.microsoft.com/library/sa91de1e.aspx) na webové stránce MSDN.</span><span class="sxs-lookup"><span data-stu-id="fac86-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="fac86-229">Problém: Odinstalování rozhraní .NET Framework verze 4 zakáže ASP.NET Web Pages se syntaxí Razor</span><span class="sxs-lookup"><span data-stu-id="fac86-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="fac86-230">Pokud odinstalujete rozhraní .NET Framework verze 4 a pak ho znovu nainstalujte, ASP.NET Web Pages se syntaxí Razor je zakázaná.</span><span class="sxs-lookup"><span data-stu-id="fac86-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="fac86-231">Stránky s *.cshtml* rozšíření správně spustit.</span><span class="sxs-lookup"><span data-stu-id="fac86-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="fac86-232">Rozhraní ASP.NET Web Pages zaregistruje sestavení v kořenovém adresáři počítač *web.config* souboru a odebírá se rozhraní .NET Framework odebere tento soubor.</span><span class="sxs-lookup"><span data-stu-id="fac86-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="fac86-233">Opětovná instalace rozhraní .NET Framework nainstalovat novou verzi konfiguračního souboru, ale nepřidá odkaz na sestavení rozhraní ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="fac86-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="fac86-234">**Alternativní řešení** po opětovné instalaci rozhraní .NET Framework, přeinstalujte rozhraní ASP.NET Web Pages se syntaxí Razor.</span><span class="sxs-lookup"><span data-stu-id="fac86-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="fac86-235">Tím se přidá následující element na *web.config* souboru v kořenovém adresáři počítač, který je obvykle v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="fac86-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="fac86-236">Problém: Adresy URL bez přípony nebyl nalezen.cshtml/.vbhtml souborů ve službě IIS 7 nebo IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="fac86-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="fac86-237">Na službu IIS 7.5 nebo IIS 7, nejsou schopna nalézt stránek, které mají požadavky s adresou URL takto *.cshtml* nebo *.vbhtml* rozšíření:</span><span class="sxs-lookup"><span data-stu-id="fac86-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="fac86-238">Tento problém nastane, protože přepisování adres URL není povolená ve výchozím nastavení pro službu IIS 7 nebo IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="fac86-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="fac86-239">Nejpravděpodobnějším scénář je, že se nezobrazí problém při testování místně pomocí služby IIS Express, ale dochází při nasazení webu k hostování webu.</span><span class="sxs-lookup"><span data-stu-id="fac86-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="fac86-240">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="fac86-241">Pokud budete mít kontrolu nad do počítače serveru, na počítači serveru nainstalujte aktualizaci podle popisu v [aktualizace, která je dostupná, umožňuje některé služby IIS 7.0 a IIS 7.5 obslužné rutiny pro zpracování požadavků, jejichž adresy URL nesmí končit tečkou](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="fac86-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="fac86-242">Pokud nemáte kontrolu nad do počítače serveru (například nasazujete hostující web), přidejte následující na web *web.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="fac86-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="fac86-243">Problém: Nasazení aplikace do počítače, na kterém není SQL Server Compact nainstalovaná</span><span class="sxs-lookup"><span data-stu-id="fac86-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="fac86-244">Aplikace, které zahrnují databáze systému SQL Server Compact můžete spustit na počítači, kde SQL Server Compact není nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="fac86-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="fac86-245">Microsoft WebMatrix 1.0 automaticky za vás zkopíruje tyto binární soubory a provede příslušné *web.config* souborů transformace.</span><span class="sxs-lookup"><span data-stu-id="fac86-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="fac86-246">**Alternativní řešení** potřebujete zkopírujte tyto soubory a nastavte *web.config* změny v souboru ručně, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="fac86-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="fac86-247">Kopírovat sestavení modulu databáze, které chcete *Bin* složku (a její podsložky) aplikace v cílovém počítači:</span><span class="sxs-lookup"><span data-stu-id="fac86-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="fac86-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="fac86-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>        <span data-ttu-id="fac86-249">**k** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="fac86-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="fac86-250">Kopírování <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>k</em></strong>\Bin\x86\*</span><span class="sxs-lookup"><span data-stu-id="fac86-250">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86\*</span></span>
>    - <span data-ttu-id="fac86-251">Kopírování <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>k</strong><em>\Bin\amd64</em></span><span class="sxs-lookup"><span data-stu-id="fac86-251">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>to</strong><em>\Bin\amd64</em></span></span>
> 
> 2. <span data-ttu-id="fac86-252">V kořenové složce webové stránky, vytvořte nebo otevřete *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="fac86-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="fac86-253">(Ve službě WebMatrix 1.0 je k dispozici, pokud kliknete na tento typ souboru **všechny** v **vyberte typ souboru** dialogové okno.)</span><span class="sxs-lookup"><span data-stu-id="fac86-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="fac86-254">Přidejte následující prvek jako podřízený objekt `<configuration>` – element (ne uvnitř `<system.web>` element):</span><span class="sxs-lookup"><span data-stu-id="fac86-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="fac86-255">Problém: "Databázi" a "WebGrid" Pomocníci nefungují na úrovni Medium Trust v jazyce Visual Basic</span><span class="sxs-lookup"><span data-stu-id="fac86-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="fac86-256">Pokud používáte Visual Basic (vytváření *.vbhtml* soubory), `Database` a `WebGrid` pomocné rutiny nebude fungovat, pokud aplikace je nastaveno pro použití úrovni Medium Trust.</span><span class="sxs-lookup"><span data-stu-id="fac86-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="fac86-257">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-257">**Workaround**</span></span>  
> <span data-ttu-id="fac86-258">Pokud používáte Visual Studio 2010, lze vyřešit tento problém instalací verze aktualizace Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="fac86-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="fac86-259">Dokud je k dispozici konečná verze na verzi SP1, můžete stáhnout Beta verzi SP1 [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) stránky na webu Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="fac86-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="fac86-260">Pokud je toto nepraktické, nebo pokud je velmi riskantní používat Visual Studio 2010, můžete dočasně nastavit aplikace používat plnou důvěryhodnost.</span><span class="sxs-lookup"><span data-stu-id="fac86-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="fac86-261">Problém: "ApplicationPart" prostředky jsou externě dostupný.</span><span class="sxs-lookup"><span data-stu-id="fac86-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="fac86-262">Pokud sestavení obsahuje objekty, které je odvozen od `ApplicationPart` třídy vystavené prostředků sestavení `ResourceRouteHandler` třídy.</span><span class="sxs-lookup"><span data-stu-id="fac86-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="fac86-263">Představte si třeba následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="fac86-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="fac86-264">Tento požadavek stáhne všechny zdrojové řetězce v *System.Web.WebPages.Administration.dll* sestavení.</span><span class="sxs-lookup"><span data-stu-id="fac86-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="fac86-265">Všechny vložené prostředky (včetně těch, které nejsou určené ke zpracování jako statický obsah), budou staženy.</span><span class="sxs-lookup"><span data-stu-id="fac86-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="fac86-266">Pokud vložené prostředky obsahují citlivé informace, to může představovat bezpečnostní riziko.</span><span class="sxs-lookup"><span data-stu-id="fac86-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="fac86-267">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="fac86-267">**Workaround** </span></span>  
> <span data-ttu-id="fac86-268">Pokud vytvoříte **ApplicationPart** objektu, ujistěte se, že vložené prostředky spojené s, která **ApplicationPart** objektu sestavení neobsahují citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="fac86-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="fac86-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="fac86-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="fac86-270">Informace o problémech s instalací pro službu WebMatrix najdete v tématu [problémy instalace služby WebMatrix](#Known_Issues_Installation) výše v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="fac86-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="fac86-271">Tato část dokumentu popisuje známé problémy pro vývojové prostředí WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="fac86-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="fac86-272">Problém: Změny v zadané uživatelské jméno nebo heslo připojovacího řetězce databáze v souboru web.config se neprojeví v pracovním prostoru databází</span><span class="sxs-lookup"><span data-stu-id="fac86-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="fac86-273">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="fac86-274">V *web.config* změňte název databáze v připojovacím řetězci (například přidat "1" do něj).</span><span class="sxs-lookup"><span data-stu-id="fac86-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="fac86-275">Uložit *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="fac86-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="fac86-276">Klikněte na tlačítko **databází** a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="fac86-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="fac86-277">Změnit název databáze v připojovacím řetězci v *web.config* souboru zpět na původní název databáze.</span><span class="sxs-lookup"><span data-stu-id="fac86-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="fac86-278">Uložit *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="fac86-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="fac86-279">Klikněte na tlačítko **databází** a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="fac86-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="fac86-280">Problém: Nelze odstranit složky vytvořené pomocí služby WebMatrix</span><span class="sxs-lookup"><span data-stu-id="fac86-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="fac86-281">Pokud služba WebMatrix, spuštěná pomocí zvýšenou úroveň oprávnění (to znamená, že jste spustili pomocí služby WebMatrix **spustit jako správce** možnost ve Windows), složky, které jsou vytvořeny pomocí služby WebMatrix nelze odstranit pomocí Průzkumníka Windows.</span><span class="sxs-lookup"><span data-stu-id="fac86-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="fac86-282">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-282">**Workaround**</span></span>  
> <span data-ttu-id="fac86-283">Spusťte Windows Explorer pomocí zvýšenou úroveň oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fac86-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="fac86-284">Postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="fac86-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="fac86-285">Ve Windows, klikněte na tlačítko **Start**.</span><span class="sxs-lookup"><span data-stu-id="fac86-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="fac86-286">Zadejte "Windows Explorer" a klikněte pravým tlačítkem na položku **Windows Explorer**.</span><span class="sxs-lookup"><span data-stu-id="fac86-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="fac86-287">Klikněte na tlačítko **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="fac86-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="fac86-288">Odstraňte složky.</span><span class="sxs-lookup"><span data-stu-id="fac86-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="fac86-289">Problém: Služba WebMatrix 1.0 není schopen provést určité úlohy, které vyžadují ke zvýšení úrovně oprávnění</span><span class="sxs-lookup"><span data-stu-id="fac86-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="fac86-290">Služba WebMatrix 1.0 není schopen provádění určitých úloh vyžadujících zvýšená oprávnění, například při instalaci dalších komponent v následujících situacích:</span><span class="sxs-lookup"><span data-stu-id="fac86-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="fac86-291">U Windows Vista nebo Windows 7 jsou přihlášeni pomocí účtu, který nemá oprávnění pro správu a řízení uživatelských účtů (UAC) je zakázán.</span><span class="sxs-lookup"><span data-stu-id="fac86-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="fac86-292">Používání systému Microsoft Windows XP nebo Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="fac86-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="fac86-293">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-293">**Workaround**</span></span>  
> <span data-ttu-id="fac86-294">Většinu úkolů ve službě WebMatrix 1.0 se nevyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="fac86-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="fac86-295">Pro ty, které udělat můžete provádět operace jako správce nebo postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="fac86-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="fac86-296">U Windows Vista nebo Windows 7 povolení nástroje Řízení uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="fac86-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="fac86-297">Windows XP přidejte uživatele do skupiny zabezpečení Administrators.</span><span class="sxs-lookup"><span data-stu-id="fac86-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="fac86-298">Problém: "Webu z Galerie webových" je zakázaná.</span><span class="sxs-lookup"><span data-stu-id="fac86-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="fac86-299">**Webu z Galerie webových** možnost je zakázaná, pokud není nainstalovaný 3.0 Instalační služby webové platformy.</span><span class="sxs-lookup"><span data-stu-id="fac86-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="fac86-300">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-300">**Workaround**</span></span>  
> <span data-ttu-id="fac86-301">Nainstalujte [instalace webové platformy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="fac86-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="fac86-302">Problém: Google Chrome není k dispozici jako možnost spuštění</span><span class="sxs-lookup"><span data-stu-id="fac86-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="fac86-303">Google Chrome se nezobrazí v seznamu prohlížečů v rámci **spustit** na **Domů** kartu.</span><span class="sxs-lookup"><span data-stu-id="fac86-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="fac86-304">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-304">**Workaround**</span></span>  
> <span data-ttu-id="fac86-305">Některé verze Google Chrome neregistrujte sami správně s výchozí programy funkcí ve Windows.</span><span class="sxs-lookup"><span data-stu-id="fac86-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="fac86-306">Jako alternativní řešení, spusťte Google Chrome, klikněte na tlačítko *přizpůsobení a řízení Google Chrome* nabídky, klikněte na tlačítko *možnosti*a potom klikněte na tlačítko *zkontrolujte Google Chrome prohlížeči výchozí*.</span><span class="sxs-lookup"><span data-stu-id="fac86-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="fac86-307">Problém: V dialogovém okně "Cizí klíč" neumožňuje zadání primární klíč</span><span class="sxs-lookup"><span data-stu-id="fac86-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="fac86-308">**Cizí klíč** dialogové okno k zadání název primárního klíče z primárního klíče tabulky nepovoluje.</span><span class="sxs-lookup"><span data-stu-id="fac86-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="fac86-309">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-309">**Workaround**</span></span>  
> <span data-ttu-id="fac86-310">Je to záměr.</span><span class="sxs-lookup"><span data-stu-id="fac86-310">This is intentional.</span></span> <span data-ttu-id="fac86-311">Není nutné zadat název primárního klíče z primární klíč tabulky.</span><span class="sxs-lookup"><span data-stu-id="fac86-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="fac86-312">Problém: Technologie IntelliSense není k dispozici v nástroji WebMatrix pro Razor syntaxe jazyka C# a Visual Basic</span><span class="sxs-lookup"><span data-stu-id="fac86-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="fac86-313">Technologie IntelliSense je podporována v nástroji WebMatrix pro HTML a CSS.</span><span class="sxs-lookup"><span data-stu-id="fac86-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="fac86-314">To však není k dispozici pro jiné jazyky.</span><span class="sxs-lookup"><span data-stu-id="fac86-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="fac86-315">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="fac86-315">**Workaround** </span></span>  
> <span data-ttu-id="fac86-316">Žádné</span><span class="sxs-lookup"><span data-stu-id="fac86-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="fac86-317">Problém: IntelliSense pro HTML a CSS navrhuje prvky, které nejsou kontextově vhodné</span><span class="sxs-lookup"><span data-stu-id="fac86-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="fac86-318">Technologie IntelliSense pro kód ve službě WebMatrix podporuje HTML pomocí [XHTML 1.0 přechodné schématu](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) a pomocí šablon stylů CSS [schématu CSS 2.1](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="fac86-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="fac86-319">Protože technologie IntelliSense je založena na tato konkrétní schémata, určité značek, atributy a vlastnosti může být určeno, které nejsou vhodné pro aktuální stránku nebo styl definice.</span><span class="sxs-lookup"><span data-stu-id="fac86-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="fac86-320">Pro kód HTML může také vést k neočekávaným návrhů v obsahu, který může být interpretován jako poškozený XHTML (například, když nebyly uzavřeny značky).</span><span class="sxs-lookup"><span data-stu-id="fac86-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="fac86-321">Tento problém může být více patrné, pokud je kurzor na místo uvnitř značku neúplné; v takovém případě může navrhnout nové počáteční značky technologie IntelliSense nebo tuto nabídku Další nesprávné návrhy.</span><span class="sxs-lookup"><span data-stu-id="fac86-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="fac86-322">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="fac86-322">**Workaround** </span></span>  
> <span data-ttu-id="fac86-323">Pro kód HTML Ujistěte se, že pracujete v rámci stránky XHTML ve správném formátu a kompletní.</span><span class="sxs-lookup"><span data-stu-id="fac86-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="fac86-324">Šablony stylů CSS neexistuje žádné alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="fac86-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="fac86-325">Problém: Technologie IntelliSense není vyvolána, když zadáte</span><span class="sxs-lookup"><span data-stu-id="fac86-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="fac86-326">V některých případech technologie IntelliSense nemusí vyvolat při zadávání HTML a CSS v editoru.</span><span class="sxs-lookup"><span data-stu-id="fac86-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="fac86-327">Konkrétně to může dojít, pokud je kurzor přímo vedle jiný element nebo na konci souboru.</span><span class="sxs-lookup"><span data-stu-id="fac86-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="fac86-328">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="fac86-328">**Workaround** </span></span>  
> <span data-ttu-id="fac86-329">Ujistěte se, že je prázdný znak kolem kurzoru a kurzor není na konci souboru.</span><span class="sxs-lookup"><span data-stu-id="fac86-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="fac86-330">Technologie IntelliSense můžete také vyvolat ručně stisknutím kombinace kláves Ctrl + mezerník.</span><span class="sxs-lookup"><span data-stu-id="fac86-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="fac86-331">Problém: Žádné uživatelské rozhraní je k dispozici pro zakázání technologie IntelliSense</span><span class="sxs-lookup"><span data-stu-id="fac86-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="fac86-332">1.0 pro službu WebMatrix poskytuje bez uživatelského rozhraní nebo gesta pro zakázání technologie IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="fac86-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="fac86-333">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="fac86-333">**Workaround** </span></span>  
> <span data-ttu-id="fac86-334">Spusťte službu WebMatrix pomocí následujícího příkazu, která zahrnuje přepínač, který zakáže IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="fac86-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="fac86-335">Služba IIS Express</span><span class="sxs-lookup"><span data-stu-id="fac86-335">IIS Express</span></span>

<span data-ttu-id="fac86-336">Služba IIS Express má svůj vlastní soubor readme, který je k dispozici na na následující adrese URL:</span><span class="sxs-lookup"><span data-stu-id="fac86-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="fac86-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x409</span><span class="sxs-lookup"><span data-stu-id="fac86-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="fac86-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="fac86-338">SQL Server Compact</span></span>

<span data-ttu-id="fac86-339">SQL Server Compact má svůj vlastní soubor readme, který je k dispozici na na následující adrese URL:</span><span class="sxs-lookup"><span data-stu-id="fac86-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="fac86-340">Informace o problémech, které se týkají instalace systému SQL Server Compact jako součást nástroje WebMatrix, najdete v tématu [problémy instalace služby WebMatrix](#Known_Issues_Installation) výše v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="fac86-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a>  <span data-ttu-id="fac86-341">Instalace aplikací</span><span class="sxs-lookup"><span data-stu-id="fac86-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="fac86-342">Problém: Instalace aplikace může trvat dlouhou dobu Pokud složku Dokumenty uživatele přesměruje na sdílené síťové složky</span><span class="sxs-lookup"><span data-stu-id="fac86-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="fac86-343">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-343">**Workaround**</span></span>  
> <span data-ttu-id="fac86-344">Žádné</span><span class="sxs-lookup"><span data-stu-id="fac86-344">None.</span></span> <span data-ttu-id="fac86-345">Aplikace může nějakou dobu instalace, ale nainstaluje správně.</span><span class="sxs-lookup"><span data-stu-id="fac86-345">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a>  <span data-ttu-id="fac86-346">Publikování aplikací</span><span class="sxs-lookup"><span data-stu-id="fac86-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="fac86-347">Problém: "požadovaná oprávnění nelze získat" chyby při publikování databáze SQL Compact</span><span class="sxs-lookup"><span data-stu-id="fac86-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="fac86-348">Služba WebMatrix plně nepodporuje nasazování podpůrné binárních souborů pro SQL Server Compact do serveru, na kterém běží aplikace rozhraní .NET Framework verze 3.5 s konfigurací úrovni medium trust.</span><span class="sxs-lookup"><span data-stu-id="fac86-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="fac86-349">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-349">**Workaround**</span></span>  
> <span data-ttu-id="fac86-350">Vhodnějším řešením je instalace rozhraní .NET Framework 4 na serveru.</span><span class="sxs-lookup"><span data-stu-id="fac86-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="fac86-351">Případně postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="fac86-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="fac86-352">Přidejte následující prvky, které mají `SecurityClasses` tématu *webové\_MediumTrust.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="fac86-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="fac86-353">Vytvořit novou sadu oprávnění *webové\_MediumTrust.config* soubor s následující požadovaná oprávnění:</span><span class="sxs-lookup"><span data-stu-id="fac86-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="fac86-354">Použití oprávnění nastaveno na SQL Server Compact tak, že vložíte následující prvky *webové\_MediumTrust.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="fac86-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="fac86-355">Problém: Galerie a systém PhpBB webové aplikace zobrazí chybu "Služba není k dispozici" po publikování</span><span class="sxs-lookup"><span data-stu-id="fac86-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="fac86-356">Za určitých okolností publikování aplikace způsobí chybu "služba není k dispozici".</span><span class="sxs-lookup"><span data-stu-id="fac86-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="fac86-357">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-357">**Workaround**</span></span>  
> <span data-ttu-id="fac86-358">V nástroji WebMatrix, přidat zpětné lomítko (\) na konec názvu serveru v **nastavení publikování** okno a potom aplikaci znovu publikovat.</span><span class="sxs-lookup"><span data-stu-id="fac86-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="fac86-359">Problém: Moodle webu rozložení a propojení se přeruší po publikování</span><span class="sxs-lookup"><span data-stu-id="fac86-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="fac86-360">Po publikování aplikace Moodle aplikace nebude fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="fac86-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="fac86-361">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-361">**Workaround**</span></span>  
> <span data-ttu-id="fac86-362">V nástroji WebMatrix, přidejte na konec lomítka (/) **název lokality** pole **nastavení publikování** okno a potom aplikaci znovu publikovat.</span><span class="sxs-lookup"><span data-stu-id="fac86-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="fac86-363">Problém: Publikování nopCommerce selže s chybou databáze</span><span class="sxs-lookup"><span data-stu-id="fac86-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="fac86-364">Publikování nopCommerce selže a oznámí chyby databáze jako "použít příkaz Insert nop\_tabulky protokolu se nezdařilo."</span><span class="sxs-lookup"><span data-stu-id="fac86-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="fac86-365">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="fac86-366">V nástroji WebMatrix, klikněte na tlačítko **spustit** nopCommerce místně spustíte.</span><span class="sxs-lookup"><span data-stu-id="fac86-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="fac86-367">Přihlaste se ke stránce pro správu.</span><span class="sxs-lookup"><span data-stu-id="fac86-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="fac86-368">Klikněte na tlačítko **systému** nabídky.</span><span class="sxs-lookup"><span data-stu-id="fac86-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="fac86-369">Klikněte na tlačítko **protokolu** možnost.</span><span class="sxs-lookup"><span data-stu-id="fac86-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="fac86-370">Klikněte na tlačítko **vymazat protokol** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fac86-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="fac86-371">NopCommerce znovu publikujte.</span><span class="sxs-lookup"><span data-stu-id="fac86-371">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="fac86-372">Problém: Silverstripe CMS zobrazí "HTTP 500 PHP FCGI chyba" při stažení publikovaného webu</span><span class="sxs-lookup"><span data-stu-id="fac86-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="fac86-373">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-373">**Workaround**</span></span>  
> <span data-ttu-id="fac86-374">Po kliknutí na **stahování publikování webu**, přeskočte `silverstripe-cache/manifest_main` v **publikování náhledu**.</span><span class="sxs-lookup"><span data-stu-id="fac86-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="fac86-375">Tento soubor se používá pro ukládání do mezipaměti účely a je specifická pro každý počítač.</span><span class="sxs-lookup"><span data-stu-id="fac86-375">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="fac86-376">Problém: Zobrazí tuto část získat "Chyba serveru v aplikaci"/"" při stahování publikovaného webu</span><span class="sxs-lookup"><span data-stu-id="fac86-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="fac86-377">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-377">**Workaround**</span></span>  
> <span data-ttu-id="fac86-378">Otevřete webu *web.config* souboru a nahraďte ID uživatele a heslo v připojovací řetězec databáze pomocí přihlašovacích údajů správce serveru SQL Server (přihlašovací údaje "sa").</span><span class="sxs-lookup"><span data-stu-id="fac86-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="fac86-379">Případně postupujte podle těchto kroků k uživatelskému účtu jste přihlášeni s `db_owner` oprávnění:</span><span class="sxs-lookup"><span data-stu-id="fac86-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="fac86-380">Nainstalujte SQL Server Management Studio pomocí instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="fac86-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="fac86-381">Připojte se k místní instanci systému SQL Server Express (ve výchozím nastavení, `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="fac86-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="fac86-382">Klikněte na tlačítko **databází** &gt; *[localSubtextDatabase]* &gt; **zabezpečení** &gt; **uživatelé** &gt; *[localSubtextUser*] (výchozí hodnota je `subtextuser`], klikněte pravým tlačítkem a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="fac86-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="fac86-383">Vyberte **db\_vlastníka** v části role členství.</span><span class="sxs-lookup"><span data-stu-id="fac86-383">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="fac86-384">Problém: Web nemusí fungovat po publikování, pokud není pole "Cílovou adresu URL" předponou http:// nebo https://.</span><span class="sxs-lookup"><span data-stu-id="fac86-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="fac86-385">V **nastavení publikování** dialogové okno, pokud cílová adresa URL nezačíná `http://` nebo `https://`, lokalitě nemusí fungovat po nasazení.</span><span class="sxs-lookup"><span data-stu-id="fac86-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="fac86-386">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-386">**Workaround**</span></span>  
> <span data-ttu-id="fac86-387">Ujistěte se, že před publikováním serveru, cílovou adresu URL v **nastavení publikování** dialogové okno začíná `http://` nebo `https://`.</span><span class="sxs-lookup"><span data-stu-id="fac86-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="fac86-388">Problém: Publikování databáze MySQL selže s chybou "se nepodařilo publikovat i databázi.</span><span class="sxs-lookup"><span data-stu-id="fac86-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="fac86-389">To může dojít, pokud vzdálené databázi nepodaří spustit skript."</span><span class="sxs-lookup"><span data-stu-id="fac86-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="fac86-390">Této chybě může dojít k z několika důvodů.</span><span class="sxs-lookup"><span data-stu-id="fac86-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="fac86-391">Jedním z důvodů, že zobrazí se tato chyba je-li skript databáze obsahuje znak jednoduché uvozovky (') a není cílové databáze MySQL výchozí znakovou sadu UTF-8.</span><span class="sxs-lookup"><span data-stu-id="fac86-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="fac86-392">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-392">**Workaround**</span></span>  
> <span data-ttu-id="fac86-393">Nastavte výchozí znakovou sadu pro vzdálenou databázi MySQL na UTF-8.</span><span class="sxs-lookup"><span data-stu-id="fac86-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="fac86-394">Problém: Některé odkazy nejsou viditelné v DotNetNuke po publikování nebo stahování webu</span><span class="sxs-lookup"><span data-stu-id="fac86-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="fac86-395">Je-li publikovat nebo stáhnout aplikace DotNetNuke lokality, můžete potřebovat vymazat mezipaměť získat nové odkazy se zobrazí na webu.</span><span class="sxs-lookup"><span data-stu-id="fac86-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="fac86-396">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="fac86-397">Přihlaste se jako "Hostitel".</span><span class="sxs-lookup"><span data-stu-id="fac86-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="fac86-398">Přejděte do nabídky hostitele a vyberte **nastavení hostitele**.</span><span class="sxs-lookup"><span data-stu-id="fac86-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="fac86-399">Přejděte dolů a v části **Upřesnit nastavení**, rozbalte **nastavení výkonu**.</span><span class="sxs-lookup"><span data-stu-id="fac86-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="fac86-400">Klikněte na tlačítko **vymazat mezipaměť** odkaz pro stránky.</span><span class="sxs-lookup"><span data-stu-id="fac86-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="fac86-401">Přejděte do dolní části stránky a restartovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fac86-401">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="fac86-402">Problém: Některé odkazy v AtomSite se přeruší po stažení publikovaného webu</span><span class="sxs-lookup"><span data-stu-id="fac86-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="fac86-403">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-403">**Workaround**</span></span>  
> <span data-ttu-id="fac86-404">V *service.config* souboru *users.config* soubor a všechny *.xml* soubory, nahraďte řetězec adresy URL (například `http://myhost.com/atomsite`) s místní (například `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="fac86-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="fac86-405">Problém: Selhání aplikací založenou na MySQL, jako je WordPress k publikování a nahlaste chybu databáze</span><span class="sxs-lookup"><span data-stu-id="fac86-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="fac86-406">Ve výchozím nastavení nainstaluje služba WebMatrix MySQL pomocí znakové sady UTF-8.</span><span class="sxs-lookup"><span data-stu-id="fac86-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="fac86-407">Pokud instalace MySQL na vlastní a není znakové sady UTF-8 (například je Latin1), může selhat proces publikování databáze.</span><span class="sxs-lookup"><span data-stu-id="fac86-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="fac86-408">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="fac86-409">Změňte znakovou sadu pro MySQL na UTF-8.</span><span class="sxs-lookup"><span data-stu-id="fac86-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="fac86-410">(Podrobnosti najdete v tématu [Server znakové sady a kolaci](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) na webu MySQL.)</span><span class="sxs-lookup"><span data-stu-id="fac86-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="fac86-411">Nainstalujte aplikaci znovu.</span><span class="sxs-lookup"><span data-stu-id="fac86-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="fac86-412">Publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="fac86-412">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="fac86-413">Problém: "Download publikování webu" selže u aplikací, které mají nastavení založené na prohlížeči</span><span class="sxs-lookup"><span data-stu-id="fac86-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="fac86-414">Některé aplikace (například Kentico CMS) vyžadují, abyste je spuštění v prohlížeči, aby bylo možné provést nastavení po instalaci, jako je vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="fac86-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="fac86-415">Pokud publikujete třeba aplikaci tímto způsobem bez dokončení instalace založené na prohlížeči, se nezdaří pokus o stažení stejného serveru ze vzdáleného serveru.</span><span class="sxs-lookup"><span data-stu-id="fac86-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="fac86-416">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-416">**Workaround**</span></span>  
> <span data-ttu-id="fac86-417">Dokončete nastavení založené na prohlížeči před publikováním webu.</span><span class="sxs-lookup"><span data-stu-id="fac86-417">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="fac86-418">Problém: "Download publikování webu" selže s chybou databáze DotNetNuke a Kooboo CMS</span><span class="sxs-lookup"><span data-stu-id="fac86-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="fac86-419">Pokud pokusu o stažení aplikace ze serveru a mít pověření správce připojovacího řetězce databáze v **nastavení publikování** dialogového okna, může se zobrazit následující chybu v protokolu publikovat:</span><span class="sxs-lookup"><span data-stu-id="fac86-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="fac86-420">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="fac86-420">**Workaround**</span></span>  
> <span data-ttu-id="fac86-421">Pokud je to praktické, znovu publikovat webu (nebo jeho publikování) pomocí přihlašovacích údajů bez oprávnění správce pro databázi.</span><span class="sxs-lookup"><span data-stu-id="fac86-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="fac86-422">Další informace</span><span class="sxs-lookup"><span data-stu-id="fac86-422">For More Information</span></span>

<span data-ttu-id="fac86-423">Další informace o službě WebMatrix 1.0 naleznete na následujících webech:</span><span class="sxs-lookup"><span data-stu-id="fac86-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="fac86-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="fac86-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="fac86-425">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fac86-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="fac86-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="fac86-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="fac86-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="fac86-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="fac86-428">Všechna práva vyhrazena.</span><span class="sxs-lookup"><span data-stu-id="fac86-428">All Rights Reserved.</span></span> <span data-ttu-id="fac86-429">[Podmínky použití](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="fac86-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
