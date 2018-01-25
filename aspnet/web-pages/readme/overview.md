---
uid: web-pages/readme/overview
title: "Služba WebMatrix Readme | Microsoft Docs"
author: rick-anderson
description: "Služba WebMatrix a ASP.NET Web Pages (Razor) verzi 1.0 – soubor Readme"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: b8402aa3db1b2566878c4d56212facbbb2925eec
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="webmatrix-readme"></a><span data-ttu-id="09c62-103">Soubor Readme pro službu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="09c62-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="09c62-104">13. ledna 2011</span><span class="sxs-lookup"><span data-stu-id="09c62-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="09c62-105">Obsah</span><span class="sxs-lookup"><span data-stu-id="09c62-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="09c62-106">Tento soubor readme platí pro verzi 1.0 nástroje WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="09c62-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="09c62-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="09c62-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="09c62-108">Instalace</span><span class="sxs-lookup"><span data-stu-id="09c62-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="09c62-109">Jak publikovat aplikace</span><span class="sxs-lookup"><span data-stu-id="09c62-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="09c62-110">Změny a problémy</span><span class="sxs-lookup"><span data-stu-id="09c62-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="09c62-111">Instalace služby WebMatrix 1.0</span><span class="sxs-lookup"><span data-stu-id="09c62-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="09c62-112">Webové stránky ASP.NET</span><span class="sxs-lookup"><span data-stu-id="09c62-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="09c62-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="09c62-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="09c62-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="09c62-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="09c62-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="09c62-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="09c62-116">Instalace aplikace</span><span class="sxs-lookup"><span data-stu-id="09c62-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="09c62-117">Publikování aplikací</span><span class="sxs-lookup"><span data-stu-id="09c62-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="09c62-118">Další informace</span><span class="sxs-lookup"><span data-stu-id="09c62-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="09c62-119">Přehled</span><span class="sxs-lookup"><span data-stu-id="09c62-119">Overview</span></span>

> <span data-ttu-id="09c62-120">Microsoft WebMatrix 1.0 je bezplatných webových zásobníku vývoj, který nainstaluje v minutách.</span><span class="sxs-lookup"><span data-stu-id="09c62-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="09c62-121">Webový server se integruje s databáze a programovací rozhraní pro vytvoření jednoho integrovaného rozhraní.</span><span class="sxs-lookup"><span data-stu-id="09c62-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="09c62-122">Služba WebMatrix můžete zjednodušit kód, testování a publikování vlastní web ASP.NET a PHP způsob nebo používáte službu WebMatrix ke spuštění nového webu pomocí Oblíbené open-source aplikace, jako je aplikace DotNetNuke, Umbraco, WordPress nebo Joomla.</span><span class="sxs-lookup"><span data-stu-id="09c62-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="09c62-123">Služba WebMatrix používá stejný výkonný webový server, databázový stroj a prostředí architektury, které se spustí svůj web na Internetu, které umožňuje přechod z vývojového do produkčního prostředí hladký a bezproblémový.</span><span class="sxs-lookup"><span data-stu-id="09c62-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="09c62-124">Instalace</span><span class="sxs-lookup"><span data-stu-id="09c62-124">Installation</span></span>

> <span data-ttu-id="09c62-125">Pokud chcete nainstalovat službu WebMatrix 1.0, musíte nejprve nainstalovat [Microsoft webové platformy verze 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="09c62-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="09c62-126">Po instalaci webové platformy, můžete k instalaci služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="09c62-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="09c62-127">Pokud máte potíže s během instalace, podívejte se na [odstraňování potíží s instalačního programu webové platformy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="09c62-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="09c62-128">Jak publikovat aplikace</span><span class="sxs-lookup"><span data-stu-id="09c62-128">How to Publish Applications</span></span>

> <span data-ttu-id="09c62-129">V tématu [podrobné pokyny k publikování aplikací](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="09c62-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="09c62-130">Změny a problémy</span><span class="sxs-lookup"><span data-stu-id="09c62-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="09c62-131">Problémy 1.0 instalace služby WebMatrix</span><span class="sxs-lookup"><span data-stu-id="09c62-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="09c62-132">Problém: WebMatrix 1.0 je k dispozici pouze na platformách, které podporují rozhraní Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="09c62-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="09c62-133">Rozhraní .NET Framework verze 4 je požadované pro službu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="09c62-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="09c62-134">V některých případech instalační program služby WebMatrix 1.0, budete moct znovu nainstalujte na platformě, která není součástí sady podporované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="09c62-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="09c62-135">Konkrétně Windows Vista bez aktualizace SP1 vám umožní zahájit instalaci služby WebMatrix, ale součásti rozhraní .NET Framework 4 se nezdaří a blokovat instalaci.</span><span class="sxs-lookup"><span data-stu-id="09c62-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="09c62-136">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-136">**Workaround**</span></span>  
> <span data-ttu-id="09c62-137">Nainstalujte na podporované platformě, která zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="09c62-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="09c62-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="09c62-138">Windows 7</span></span>
> - <span data-ttu-id="09c62-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="09c62-139">Windows Server 2008</span></span>
> - <span data-ttu-id="09c62-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="09c62-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="09c62-141">Windows Vista SP1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="09c62-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="09c62-142">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="09c62-142">Windows XP SP3</span></span>
> - <span data-ttu-id="09c62-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="09c62-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="09c62-144">Problém: Nelze nainstalovat službu WebMatrix 1.0, když se nainstaluje Microsoft Visual Studio 2008 bez Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="09c62-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="09c62-145">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-145">**Workaround**</span></span>  
> <span data-ttu-id="09c62-146">Nainstalujte [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) z webu Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="09c62-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="09c62-147">Problém: V mezipaměti GAC nejsou nainstalovány některé sestavení pro SQL Server Compact 4.0</span><span class="sxs-lookup"><span data-stu-id="09c62-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="09c62-148">Spravovaná sestavení pro SQL Server Compact 4.0 nejsou umístěny v globální mezipaměti sestavení (GAC), při instalaci systému SQL Server Compact 4.0 na 64bitovém počítači a má počítač pouze profil rozhraní .NET Framework 3.5 SP1 klient nainstalován.</span><span class="sxs-lookup"><span data-stu-id="09c62-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="09c62-149">Spravovaná sestavení, které nejsou nainstalované v mezipaměti GAC jsou:</span><span class="sxs-lookup"><span data-stu-id="09c62-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="09c62-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span><span class="sxs-lookup"><span data-stu-id="09c62-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="09c62-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span><span class="sxs-lookup"><span data-stu-id="09c62-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="09c62-152">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-152">**Workaround**</span></span>  
> <span data-ttu-id="09c62-153">Odinstalujte systém SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="09c62-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="09c62-154">Stáhněte a nainstalujte plnou verzi rozhraní .NET Framework 3.5 SP1 z následujícího umístění:</span><span class="sxs-lookup"><span data-stu-id="09c62-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="09c62-155">Microsoft .NET Framework 3.5 Service pack 1 (úplná balíček)</span><span class="sxs-lookup"><span data-stu-id="09c62-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="09c62-156">Znovu nainstalujte SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="09c62-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="09c62-157">Problém: Nelze odinstalovat, SQL Server Compact pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="09c62-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="09c62-158">Odinstalace systému SQL Server Compact pomocí možnosti příkazového řádku v této verzi nefunguje.</span><span class="sxs-lookup"><span data-stu-id="09c62-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="09c62-159">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-159">**Workaround**</span></span>  
> <span data-ttu-id="09c62-160">Použití *programy a funkce* v Ovládacích panelech odinstalujte Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="09c62-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="09c62-161">ASP.NET – webové stránky</span><span class="sxs-lookup"><span data-stu-id="09c62-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="09c62-162">Tato část dokumentu popisuje nové funkce, změny a známé problémy s verzi 1.0 z webových stránek ASP.NET se syntaxí Razor.</span><span class="sxs-lookup"><span data-stu-id="09c62-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="09c62-163">Nové funkce</span><span class="sxs-lookup"><span data-stu-id="09c62-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="09c62-164">Změny</span><span class="sxs-lookup"><span data-stu-id="09c62-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="09c62-165">Problémy</span><span class="sxs-lookup"><span data-stu-id="09c62-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a><span data-ttu-id="09c62-166">Nové funkce</span><span class="sxs-lookup"><span data-stu-id="09c62-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="09c62-167">Nové: Konfigurace nastavení přidáno zakázat Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="09c62-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="09c62-168">Nový `asp:AdminManagerEnabled` klíč je k dispozici pro `<appSettings>` element v *web.config* souboru, který umožňuje úplně vypnout správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="09c62-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="09c62-169">Výchozí hodnota pro tento element je nastavena hodnota true, znamená to, že pokud není součástí *web.config* souboru, Správce balíčků je povoleno.</span><span class="sxs-lookup"><span data-stu-id="09c62-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="09c62-170">Zakázat Správce balíčků, přidat následující prvek *web.config* soubor v kořenovém adresáři webu:</span><span class="sxs-lookup"><span data-stu-id="09c62-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a><span data-ttu-id="09c62-171">Změny</span><span class="sxs-lookup"><span data-stu-id="09c62-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="09c62-172">Změnit: "webPages:AdminFolderVirtualPath" klíč přejmenován na "asp: AdminFolderVirtualPath"</span><span class="sxs-lookup"><span data-stu-id="09c62-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="09c62-173">`webPages:AdminFolderVirtualPath` Klíč, který lze přidat do *web.config* souboru k zadání umístění správce balíčků byl přejmenován na použití `asp:` obor názvů místo `webPages` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="09c62-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="09c62-174">Pokud jste použili tento prvek, je třeba přejmenovat v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="09c62-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a><span data-ttu-id="09c62-175">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="09c62-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="09c62-176">Problém: Hesla uživatelů se členstvím už rozpoznána</span><span class="sxs-lookup"><span data-stu-id="09c62-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="09c62-177">Algoritmus pro vytváření a ukládání hesel členství (přihlášení) se změnil za bezpečnější.</span><span class="sxs-lookup"><span data-stu-id="09c62-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="09c62-178">V důsledku toho nebude rozpoznána hesel uložených pro členy (uživatelé) z verze Beta verzí aplikace ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="09c62-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="09c62-179">**Alternativní řešení** Pokud webu nebyla dosud byl uvedení do produkčního prostředí, odeberte záznamů uživatele z databáze členství.</span><span class="sxs-lookup"><span data-stu-id="09c62-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="09c62-180">Pokud je databáze za provozu, prostřednictvím kódu programu obnovit existující hesla v databázi členství.</span><span class="sxs-lookup"><span data-stu-id="09c62-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="09c62-181">Problém: Neočekávané chování při použití vlastní uživatelská tabulka pro členství</span><span class="sxs-lookup"><span data-stu-id="09c62-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="09c62-182">K chybě při inicializaci zprostředkovatele členství pro web ASP.NET Razor, zavoláte `WebSecurity.InitializeDatabaseConnection` metoda.</span><span class="sxs-lookup"><span data-stu-id="09c62-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="09c62-183">(Ve službě WebMatrix, zahrnuje šabloně Starter Site volání této metody v  *\_AppStart.cshtml* souboru.) Pokud `autoCreateTables` parametr této metody je nastaven na hodnotu true (ve výchozím nastavení je nastavena na hodnotu true v šabloně Starter Site), a pokud je název tabulky nerozpoznané předaný metodě (druhý parametr), metoda nevyvolá chybu.</span><span class="sxs-lookup"><span data-stu-id="09c62-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="09c62-184">Místo toho automaticky vytvoří v tabulce.</span><span class="sxs-lookup"><span data-stu-id="09c62-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="09c62-185">To může být problém, pokud máte v úmyslu použít vlastní uživatelská tabulka pro členství, ale předat název nesprávné tabulky k `WebSecurity.InitializeDatabaseConnection` metoda.</span><span class="sxs-lookup"><span data-stu-id="09c62-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="09c62-186">Vzhledem k tomu, že metoda není ve výchozím nastavení vyvolá chybu, pokud neexistuje v tabulce, které zadáte, a protože místo toho vytvoří novou tabulku, můžete aplikaci pravděpodobně fungovat.</span><span class="sxs-lookup"><span data-stu-id="09c62-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="09c62-187">Aplikační kód, který závisí na vlastní uživatelská tabulka (a na pole v něm) můžete nakonec nezdaří s neočekávaným chybám.</span><span class="sxs-lookup"><span data-stu-id="09c62-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="09c62-188">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-188">**Workaround**</span></span>  
> <span data-ttu-id="09c62-189">Ujistěte se, že název předaná `InitializeDatabaseConnection` metoda odpovídá uživatelský profil tabulky v databázi členství nebo zkontrolujte, zda `autoCreateTables` parametr je nastaven na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="09c62-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="09c62-190">Problém: Chybová zpráva "v modulu Správce vyžaduje přístup k ~/App\_Data"</span><span class="sxs-lookup"><span data-stu-id="09c62-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="09c62-191">Za určitých okolností pokusu o vytvoření uživatelů nebo jinak pracovat s systém členství technologie ASP.NET může způsobit na stránce zobrazit chyba *modulu Správce vyžaduje přístup k ~/App\_Data*.</span><span class="sxs-lookup"><span data-stu-id="09c62-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="09c62-192">K tomu dojde, pokud účet, který služba IIS nebo IIS Express je spuštěná s pověřeními nemá oprávnění k vytvoření a zápis do *aplikace\_Data* složku v kořenovém adresáři webu.</span><span class="sxs-lookup"><span data-stu-id="09c62-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="09c62-193">**Alternativní řešení** ručně vytvořit *aplikace\_Data* složku pro web.</span><span class="sxs-lookup"><span data-stu-id="09c62-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="09c62-194">Ujistěte se, že účet systému Windows, na kterém se aplikace spouští pod (obvykle síťová služba) má oprávnění ke čtení/zápisu pro kořenové složky aplikace a podsložky, jako je aplikace\_Data.</span><span class="sxs-lookup"><span data-stu-id="09c62-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="09c62-195">Podrobnější informace najdete v článku znalostní báze Knowledge Base [problémy s SQL Server Express uživatele vytváření instancí a projekty webových aplikací ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="09c62-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="09c62-196">Problém: "Nepodařilo se vygenerovat uživatelskou instanci systému SQL Server" Chyba</span><span class="sxs-lookup"><span data-stu-id="09c62-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="09c62-197">Pokud služba WebMatrix webové aplikace používá systém SQL Server Express a je spuštěna služba IIS 7.5 na Windows 7 nebo Windows Server 2008 R2, může se zobrazit chybu, která určuje, že systém SQL Server nelze načíst cestu k místní aplikace uživatele za běhu.</span><span class="sxs-lookup"><span data-stu-id="09c62-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="09c62-198">**Alternativní řešení** Ujistěte se, že účet systému Windows, na kterém se aplikace spouští pod (obvykle síťová služba) má oprávnění ke čtení/zápisu pro kořenové složky aplikace a podsložky jako *aplikace\_dat*.</span><span class="sxs-lookup"><span data-stu-id="09c62-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="09c62-199">Podrobnější informace najdete v článku znalostní báze Knowledge Base [problémy s SQL Server Express uživatele vytváření instancí a projekty webových aplikací ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="09c62-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="09c62-200">Problém: Soubory, které obsahuje Správce balíčků prostředků nebo hesla správce balíčků jsou servable v rámci služby IIS 6.0 a starší</span><span class="sxs-lookup"><span data-stu-id="09c62-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="09c62-201">Pokud nasadíte aplikaci ASP.NET Web Pages (Razor), která byla vytvořená pomocí verze RC2 a pokud aplikace obsahuje *password.txt* nebo *packagesources.txt* souboru pod */App\_ Data/admin*, IIS 6.0 bude sloužit souboru, pokud je požadována, potenciálně vystavení hesla pro vaše instance správce balíčku.</span><span class="sxs-lookup"><span data-stu-id="09c62-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="09c62-202">**Alternativní řešení** přejmenovat *password.txt* nebo *packagesources.txt* do souboru *password.config* nebo *packagesources.config*. Ve výchozím nastavení, IIS 6.0 neobsluhuje soubory, které mají *.config* rozšíření.</span><span class="sxs-lookup"><span data-stu-id="09c62-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="09c62-203">(Ve službě IIS 7, žádné soubory v *aplikace\_Data* složky se zpracovávají, takže není nutné přejmenovat soubory.)</span><span class="sxs-lookup"><span data-stu-id="09c62-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="09c62-204">Problém: Odinstalace balíčky nainstalované používání verze Beta 3 neodebere úplně součásti balíčku</span><span class="sxs-lookup"><span data-stu-id="09c62-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="09c62-205">Pokud jste nainstalovali balíček pomocí Správce balíčků ve verzi Beta 3 a poté odinstalujte jej pomocí aktuální verze, balíček není zcela odinstalována.</span><span class="sxs-lookup"><span data-stu-id="09c62-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="09c62-206">Pomocí Správce balíčků **odinstalace** tlačítko odebere některé součásti, ale ponechá kód knihovny balíčku a neaktualizuje *package.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="09c62-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="09c62-207">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="09c62-207">**Workaround** </span></span>  
> <span data-ttu-id="09c62-208">Proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="09c62-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="09c62-209">Odstranit *aplikace\_Data\packages* složky.</span><span class="sxs-lookup"><span data-stu-id="09c62-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="09c62-210">Tím se odeberou všechny balíčky.</span><span class="sxs-lookup"><span data-stu-id="09c62-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="09c62-211">Odstranit *packages.config* soubor v kořenovém adresáři webu.</span><span class="sxs-lookup"><span data-stu-id="09c62-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="09c62-212">Problém: V sadě Visual Studio, vyvolání správce webových balíčků převede aplikace do režimu offline</span><span class="sxs-lookup"><span data-stu-id="09c62-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="09c62-213">Pokud pracujete v sadě Visual Studio (ne WebMatrix) a použít  *\_správce* funkce spustit Správce balíčků, Visual Studio převede aplikace do režimu offline a požadavky *aplikace\_ offline.htm* do kořenové složky webu, který naruší budete moci použít Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="09c62-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="09c62-214">Přestože by obvykle uvidíte toto chování při použití rozhraní založené na webu balíček správce, je-li přidat, odebrat nebo změnit všechny soubory v dojde k stejné chování *aplikace\_Data* složky.</span><span class="sxs-lookup"><span data-stu-id="09c62-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="09c62-215">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="09c62-215">**Workaround** </span></span>  
> <span data-ttu-id="09c62-216">K práci s balíčky v sadě Visual Studio, použijte namísto správce webových balíčků rozšíření NuGet.</span><span class="sxs-lookup"><span data-stu-id="09c62-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="09c62-217">Informace najdete v tématu [NuGet dokumentaci](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="09c62-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="09c62-218">Pokud pracujete s ostatními soubory v *aplikace\_Data* složky, můžete si uložit soubory, jinde k tomuto problému vyhnout.</span><span class="sxs-lookup"><span data-stu-id="09c62-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="09c62-219">Pokud není praktické, odstraňte *aplikace\_offline.htm* soubor ručně, nebo počkejte, až lokalitu přejde do režimu online automaticky (ve výchozím nastavení po 30 sekund).</span><span class="sxs-lookup"><span data-stu-id="09c62-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="09c62-220">Problém: Visual Studio IntelliSense a projekt šablony k dispozici pouze v architektuře ASP.NET MVC verze 3</span><span class="sxs-lookup"><span data-stu-id="09c62-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="09c62-221">Instalace technologie ASP.NET Web Pages také nenainstaluje nástroje pro sadu Visual Studio jako IntelliSense a projekt šablony pro aplikace ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="09c62-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="09c62-222">**Alternativní řešení** používáte IntelliSense a projekt šablony pro aplikace webových stránek ASP.NET v sadě Visual Studio, nainstalujte technologii ASP.NET MVC 3 RC buď prostřednictvím instalace webové platformy nebo [samostatný instalační program](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="09c62-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="09c62-223">Problém: Čtení informačních kanálů nebo jiné externí data prostřednictvím serveru proxy</span><span class="sxs-lookup"><span data-stu-id="09c62-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="09c62-224">Serveru se systémem lokality je za proxy serverem, může být potřeba konfigurovat informace o proxy serveru v *web.config* souboru, aby mohl číst informace, které pochází z mimo váš web.</span><span class="sxs-lookup"><span data-stu-id="09c62-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="09c62-225">Například pokud použijete `ReCaptcha` pomocné rutiny, pomocné rutiny komunikuje se službou nástroje reCAPTCHA, ale mohou být blokovány proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="09c62-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="09c62-226">Podobně informační kanály, které se používají na webových stránkách ASP.NET, jako je například používá Správce balíčků informačního kanálu může vyžadovat konfiguraci proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="09c62-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="09c62-227">Pokud máte potíže v práci s externí službu nebo práce s tímto balíčkem kanálu, uveďte následující prvky do vaší aplikace kořenové *web.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="09c62-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="09c62-228">Další informace o konfiguraci proxy serveru najdete v tématu [ &lt;proxy&gt; – Element (nastavení sítě)](https://msdn.microsoft.com/library/sa91de1e.aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="09c62-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="09c62-229">Problém: Odinstalace rozhraní .NET Framework verze 4 zakáže technologie ASP.NET Web Pages se syntaxí Razor</span><span class="sxs-lookup"><span data-stu-id="09c62-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="09c62-230">Pokud odinstalujete rozhraní .NET Framework verze 4 a znovu ji nainstalovat, technologie ASP.NET Web Pages se syntaxí Razor je zakázané.</span><span class="sxs-lookup"><span data-stu-id="09c62-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="09c62-231">Stránky s *.cshtml* rozšíření se nespustí správně.</span><span class="sxs-lookup"><span data-stu-id="09c62-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="09c62-232">Rozhraní ASP.NET Web Pages zaregistruje sestavení v kořenu počítač *web.config* soubor odebrán souboru a odebrání rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="09c62-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="09c62-233">Opětovné instalace rozhraní .NET Framework nainstaluje novou verzi konfiguračního souboru, ale nepřidá odkaz na sestavení pro ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="09c62-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="09c62-234">**Alternativní řešení** po opětovné instalaci rozhraní .NET Framework, přeinstalujte rozhraní ASP.NET Web Pages se syntaxí Razor.</span><span class="sxs-lookup"><span data-stu-id="09c62-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="09c62-235">Tím se přidá následující elementu, který chcete *web.config* soubor v kořenu počítač, který je obvykle v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="09c62-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="09c62-236">Problém: Adresy URL bez přípony nenašli.cshtml/.vbhtml soubory na službě IIS 7 nebo IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="09c62-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="09c62-237">Na službě IIS 7 nebo IIS 7.5, nejsou schopna najít stránky, které mají požadavky s adresou URL takto *.cshtml* nebo *.vbhtml* rozšíření:</span><span class="sxs-lookup"><span data-stu-id="09c62-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
>   
> `http://www.example.com/ExampleSite/ExampleFile`  
>   
> <span data-ttu-id="09c62-238">Tento problém nastane, protože přepisování adres URL není povoleno ve výchozím nastavení pro službu IIS 7 nebo IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="09c62-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="09c62-239">Nejpravděpodobnějším scénář je, že se nezobrazí problém při testování místně pomocí služby IIS Express, ale dojde při nasazení webu k hostování webu.</span><span class="sxs-lookup"><span data-stu-id="09c62-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="09c62-240">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="09c62-241">Pokud budete mít kontrolu nad počítači serveru, na počítači serveru nainstalujte aktualizaci popsanou v [aktualizace je dostupná, že umožňuje určité služby IIS 7.0 nebo IIS 7.5 obslužné rutiny pro zpracování požadavků, jejichž adresy URL na konci období](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="09c62-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="09c62-242">Pokud nemáte kontrolu nad počítači serveru (například nasazujete k hostování webu), přidejte následující na web *web.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="09c62-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="09c62-243">Problém: Nasazení aplikace do počítače, který nemá systém SQL Server Compact nainstalován</span><span class="sxs-lookup"><span data-stu-id="09c62-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="09c62-244">Aplikace, které obsahují systém SQL Server Compact databáze můžete spustit na počítači, kde systém SQL Server Compact není nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="09c62-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="09c62-245">Microsoft WebMatrix 1.0 automaticky zkopíruje tyto binární soubory pro vás a provede odpovídající *web.config* souboru transformace.</span><span class="sxs-lookup"><span data-stu-id="09c62-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="09c62-246">**Alternativní řešení** Pokud potřebujete zkopírujte tyto soubory a ujistěte se, *web.config* změny souborů ručně, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="09c62-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="09c62-247">Zkopírujte sestavení modulu databáze, které chcete *Bin* složky (a její podsložky) aplikace na cílovém počítači:</span><span class="sxs-lookup"><span data-stu-id="09c62-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>     - <span data-ttu-id="09c62-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="09c62-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>         <span data-ttu-id="09c62-249">**k** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="09c62-249">**to** *\Bin*</span></span>
>     - <span data-ttu-id="09c62-250">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\*\*\*to\*\*\*\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="09c62-250">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\*\*\*to\*\*\*\Bin\x86*</span></span>
>     - <span data-ttu-id="09c62-251">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* \**to\*\*\*\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="09c62-251">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* \**to\*\*\*\Bin\amd64*</span></span>
> 2. <span data-ttu-id="09c62-252">V kořenové složce webové stránky, vytvořit nebo otevřít *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="09c62-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="09c62-253">(Ve službě WebMatrix 1.0 je k dispozici, pokud kliknete na tento typ souboru **všechny** v **vyberte typ souboru** dialogové okno.)</span><span class="sxs-lookup"><span data-stu-id="09c62-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="09c62-254">Přidejte následující prvek jako podřízenou `<configuration>` – element (mimo `<system.web>` element):</span><span class="sxs-lookup"><span data-stu-id="09c62-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="09c62-255">Problém: "Databáze" a "WebGrid" Pomocníci nefungují na úrovni Medium Trust v jazyce Visual Basic</span><span class="sxs-lookup"><span data-stu-id="09c62-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="09c62-256">Pokud používáte Visual Basic (vytváření *.vbhtml* soubory), `Database` a `WebGrid` Pomocníci nebude fungovat, pokud je aplikace nastavena používat úrovni Medium Trust.</span><span class="sxs-lookup"><span data-stu-id="09c62-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="09c62-257">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-257">**Workaround**</span></span>  
> <span data-ttu-id="09c62-258">Pokud používáte Visual Studio 2010, můžete vyřešit tento problém instalací verze aktualizace Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="09c62-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="09c62-259">Dokud finální verzi verze s aktualizací SP1 je k dispozici, můžete stáhnout verzi SP1 Beta [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) stránky na webu Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="09c62-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="09c62-260">Pokud toto není praktické, nebo pokud používáte Visual Studio 2010, můžete dočasně nastavit aplikaci, aby používala úplný vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="09c62-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="09c62-261">Problém: "ApplicationPart" prostředky jsou externě dostupný.</span><span class="sxs-lookup"><span data-stu-id="09c62-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="09c62-262">Pokud sestavení obsahuje objekty, které je odvozena z `ApplicationPart` třídy, vystavené sestavení prostředky `ResourceRouteHandler` třídy.</span><span class="sxs-lookup"><span data-stu-id="09c62-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="09c62-263">Zvažte například následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="09c62-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="09c62-264">Tento požadavek stáhne všechny řetězce prostředků v *System.Web.WebPages.Administration.dll* sestavení.</span><span class="sxs-lookup"><span data-stu-id="09c62-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="09c62-265">Všechny vložené prostředky (i ty, které nejsou určeny k zpracovat jako statický obsah), se stáhnou.</span><span class="sxs-lookup"><span data-stu-id="09c62-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="09c62-266">Pokud vložené prostředky obsahují citlivé informace, to může představovat bezpečnostní riziko.</span><span class="sxs-lookup"><span data-stu-id="09c62-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="09c62-267">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="09c62-267">**Workaround** </span></span>  
> <span data-ttu-id="09c62-268">Pokud vytvoříte **ApplicationPart** objektu, ujistěte se, že vložené prostředky přidružené který **ApplicationPart** sestavení objektu neobsahují citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="09c62-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="09c62-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="09c62-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="09c62-270">Informace o problémech s instalací pro službu WebMatrix najdete v tématu [problémy instalace služby WebMatrix](#Known_Issues_Installation) dříve v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="09c62-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="09c62-271">Tato část dokumentu popisuje známé problémy pro vývojové prostředí WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="09c62-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="09c62-272">Problém: Změny v uživatelské jméno nebo heslo připojovacího řetězce databáze v souboru web.config se neprojeví v pracovní prostor databáze</span><span class="sxs-lookup"><span data-stu-id="09c62-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="09c62-273">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="09c62-274">V *web.config* souboru, změňte název databáze v připojovacím řetězci (například přidat "1" do něj).</span><span class="sxs-lookup"><span data-stu-id="09c62-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="09c62-275">Uložit *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="09c62-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="09c62-276">Klikněte na tlačítko **databáze** a aktualizujte.</span><span class="sxs-lookup"><span data-stu-id="09c62-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="09c62-277">Změňte název databáze v připojovacím řetězci v *web.config* souboru zpět na původní název databáze.</span><span class="sxs-lookup"><span data-stu-id="09c62-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="09c62-278">Uložit *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="09c62-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="09c62-279">Klikněte na tlačítko **databáze** a aktualizujte.</span><span class="sxs-lookup"><span data-stu-id="09c62-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="09c62-280">Problém: Složky vytvořené WebMatrix nelze odstranit.</span><span class="sxs-lookup"><span data-stu-id="09c62-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="09c62-281">Pokud služba WebMatrix používá zvýšenými oprávněními (to znamená, že jste spustili pomocí služby WebMatrix **spustit jako správce** možnost v systému Windows), složky, které jsou vytvořené pomocí služby WebMatrix nelze odstranit pomocí Průzkumníka Windows.</span><span class="sxs-lookup"><span data-stu-id="09c62-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="09c62-282">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-282">**Workaround**</span></span>  
> <span data-ttu-id="09c62-283">Spusťte program Průzkumník Windows zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="09c62-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="09c62-284">Postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="09c62-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="09c62-285">V systému Windows, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="09c62-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="09c62-286">Zadejte "Windows Explorer" a klikněte pravým tlačítkem na položku **Průzkumníka Windows**.</span><span class="sxs-lookup"><span data-stu-id="09c62-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="09c62-287">Klikněte na tlačítko **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="09c62-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="09c62-288">Potom můžete odstranit složky.</span><span class="sxs-lookup"><span data-stu-id="09c62-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="09c62-289">Problém: WebMatrix 1.0 nelze provést určité úlohy, které vyžadují zvýšení oprávnění</span><span class="sxs-lookup"><span data-stu-id="09c62-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="09c62-290">Služba WebMatrix 1.0 se nepodařilo provést určité úlohy, které vyžadují zvýšení oprávnění, například při instalaci dalších součástí v následujících situacích:</span><span class="sxs-lookup"><span data-stu-id="09c62-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="09c62-291">Windows Vista nebo Windows 7 jste přihlášeni pod účtem, který nemá oprávnění pro správu a řízení uživatelských účtů (UAC) je zakázáno.</span><span class="sxs-lookup"><span data-stu-id="09c62-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="09c62-292">Používáte Microsoft Windows XP nebo Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="09c62-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="09c62-293">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-293">**Workaround**</span></span>  
> <span data-ttu-id="09c62-294">Většinu úloh ve službě WebMatrix 1.0 nevyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="09c62-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="09c62-295">Pro ty, které provádějí můžete provést operaci jako správce nebo postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="09c62-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="09c62-296">V systému Windows Vista nebo Windows 7 povolte nástroj Řízení uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="09c62-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="09c62-297">V systému Windows XP přidejte uživatele do skupiny zabezpečení Administrators.</span><span class="sxs-lookup"><span data-stu-id="09c62-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="09c62-298">Problém: "Lokality z Galerie webových" je zakázána.</span><span class="sxs-lookup"><span data-stu-id="09c62-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="09c62-299">**Lokality z Galerie webových** možnost je zakázaná, pokud není nainstalovaná verze 3.0 webové platformy.</span><span class="sxs-lookup"><span data-stu-id="09c62-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="09c62-300">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-300">**Workaround**</span></span>  
> <span data-ttu-id="09c62-301">Nainstalujte [webové platformy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="09c62-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="09c62-302">Problém: Google Chrome není k dispozici jako možnost spuštění</span><span class="sxs-lookup"><span data-stu-id="09c62-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="09c62-303">Google Chrome se nezobrazuje v seznamu prohlížečů v části **spustit** na **Domů** kartě.</span><span class="sxs-lookup"><span data-stu-id="09c62-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="09c62-304">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-304">**Workaround**</span></span>  
> <span data-ttu-id="09c62-305">Některé verze Google Chrome nezaregistroval sami správně funkce výchozí programy systému Windows.</span><span class="sxs-lookup"><span data-stu-id="09c62-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="09c62-306">Jako alternativní řešení, spustit Google Chrome, klikněte na *přizpůsobit a řízení Google Chrome* nabídky, klikněte na tlačítko *možnosti*a potom klikněte na *zkontrolujte Google Chrome Moje výchozí prohlížeč*.</span><span class="sxs-lookup"><span data-stu-id="09c62-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="09c62-307">Problém: Dialogové okno "Cizí klíč" neumožňuje zadání primární klíč</span><span class="sxs-lookup"><span data-stu-id="09c62-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="09c62-308">**Cizí klíč** dialogové okno neumožňuje primární klíč tabulky zadejte název primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="09c62-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="09c62-309">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-309">**Workaround**</span></span>  
> <span data-ttu-id="09c62-310">To je úmyslné.</span><span class="sxs-lookup"><span data-stu-id="09c62-310">This is intentional.</span></span> <span data-ttu-id="09c62-311">Není nutné k zadání názvu primární klíč tabulky primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="09c62-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="09c62-312">Problém: IntelliSense není k dispozici ve službě WebMatrix pro syntaxi Razor syntaxe jazyka C# a Visual Basic</span><span class="sxs-lookup"><span data-stu-id="09c62-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="09c62-313">IntelliSense je podporováno ve službě WebMatrix pro HTML a CSS.</span><span class="sxs-lookup"><span data-stu-id="09c62-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="09c62-314">Však není k dispozici pro další jazyky.</span><span class="sxs-lookup"><span data-stu-id="09c62-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="09c62-315">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="09c62-315">**Workaround** </span></span>  
> <span data-ttu-id="09c62-316">Žádné</span><span class="sxs-lookup"><span data-stu-id="09c62-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="09c62-317">Problém: IntelliSense pro HTML a CSS navrhuje prvky, které nejsou kontextově vhodné</span><span class="sxs-lookup"><span data-stu-id="09c62-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="09c62-318">IntelliSense pro kód ve službě WebMatrix podporuje používání HTML [XHTML 1.0 přechodném schématu](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) a používání šablon stylů CSS [schématu CSS 2.1](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="09c62-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="09c62-319">Protože tyto konkrétní schémata vychází IntelliSense, určité značky, atributy a vlastnosti může navrhované, které nejsou vhodné pro aktuální stránku nebo styl definice.</span><span class="sxs-lookup"><span data-stu-id="09c62-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="09c62-320">Pro kód HTML může také dojít k neočekávané návrhů v obsah, který může být vyhodnocen jako poškozený XHTML (například když značky nejsou uzavřeny).</span><span class="sxs-lookup"><span data-stu-id="09c62-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="09c62-321">Tento problém může být více patrné, pokud je kurzor uvnitř neúplné značky; v takovém případě může IntelliSense Navrhněte nové počáteční značky nebo nabízí další nesprávný návrhy.</span><span class="sxs-lookup"><span data-stu-id="09c62-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="09c62-322">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="09c62-322">**Workaround** </span></span>  
> <span data-ttu-id="09c62-323">Pro kód HTML Ujistěte se, že pracujete v kódu XHTML stránky ve správném formátu, dokončení.</span><span class="sxs-lookup"><span data-stu-id="09c62-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="09c62-324">U šablon stylů CSS neexistuje žádné alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="09c62-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="09c62-325">Problém: IntelliSense není vyvolána, když zadáte</span><span class="sxs-lookup"><span data-stu-id="09c62-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="09c62-326">IntelliSense v některých případech nemusí být volána při zadávání HTML nebo šablon stylů CSS v editoru.</span><span class="sxs-lookup"><span data-stu-id="09c62-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="09c62-327">Konkrétně to může dojít, když je kurzor přímo vedle jiný element nebo na konci souboru.</span><span class="sxs-lookup"><span data-stu-id="09c62-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="09c62-328">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="09c62-328">**Workaround** </span></span>  
> <span data-ttu-id="09c62-329">Ujistěte se, že je prázdný znak kolem bodu vložení a kurzor není na konci souboru.</span><span class="sxs-lookup"><span data-stu-id="09c62-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="09c62-330">Můžete také vyvolat IntelliSense ručně stisknutím kombinace kláves Ctrl + mezerník.</span><span class="sxs-lookup"><span data-stu-id="09c62-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="09c62-331">Problém: Žádné uživatelské rozhraní je k dispozici pro zakázání IntelliSense</span><span class="sxs-lookup"><span data-stu-id="09c62-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="09c62-332">Služba WebMatrix 1.0 poskytuje bez uživatelského rozhraní nebo gesto pro zakázání IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="09c62-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="09c62-333">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="09c62-333">**Workaround** </span></span>  
> <span data-ttu-id="09c62-334">Spusťte následující příkaz, který zahrnuje přepínač, který zakazuje IntelliSense pomocí služby WebMatrix:</span><span class="sxs-lookup"><span data-stu-id="09c62-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="09c62-335">Služby IIS Express</span><span class="sxs-lookup"><span data-stu-id="09c62-335">IIS Express</span></span>

<span data-ttu-id="09c62-336">Služba IIS Express má svůj vlastní soubor readme, který je k dispozici na následující adrese URL:</span><span class="sxs-lookup"><span data-stu-id="09c62-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="09c62-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="09c62-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="09c62-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="09c62-338">SQL Server Compact</span></span>

<span data-ttu-id="09c62-339">Systém SQL Server Compact má svůj vlastní soubor readme, který je k dispozici na následující adrese URL:</span><span class="sxs-lookup"><span data-stu-id="09c62-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="09c62-340">https://go.microsoft.com/fwlink/?LinkID=208545</span><span class="sxs-lookup"><span data-stu-id="09c62-340">https://go.microsoft.com/fwlink/?LinkID=208545</span></span>](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="09c62-341">Informace o problémech, které zahrnují instalaci systému SQL Server Compact jako součást nástroje WebMatrix, najdete v tématu [problémy instalace služby WebMatrix](#Known_Issues_Installation) dříve v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="09c62-341">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a><span data-ttu-id="09c62-342">Instalace aplikace</span><span class="sxs-lookup"><span data-stu-id="09c62-342">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="09c62-343">Problém: Instalace aplikace může trvat dlouhou dobu. Pokud je přesměrování složky Dokumenty uživatele do sdílené síťové složky</span><span class="sxs-lookup"><span data-stu-id="09c62-343">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="09c62-344">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-344">**Workaround**</span></span>  
> <span data-ttu-id="09c62-345">Žádné</span><span class="sxs-lookup"><span data-stu-id="09c62-345">None.</span></span> <span data-ttu-id="09c62-346">Aplikace může trvat, než k instalaci, ale nainstaluje správně.</span><span class="sxs-lookup"><span data-stu-id="09c62-346">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a><span data-ttu-id="09c62-347">Publikování aplikací</span><span class="sxs-lookup"><span data-stu-id="09c62-347">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="09c62-348">Problém: "požadované oprávnění nelze získat" Chyba při publikování databáze SQL Compact</span><span class="sxs-lookup"><span data-stu-id="09c62-348">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="09c62-349">Služba WebMatrix nepodporuje plně nasazení podpůrné binární soubory pro systém SQL Server Compact na server, který běží s konfigurací středním vztahem důvěryhodnosti rozhraní .NET Framework verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="09c62-349">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="09c62-350">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-350">**Workaround**</span></span>  
> <span data-ttu-id="09c62-351">Vhodnějším řešením je instalace rozhraní .NET Framework 4 na serveru.</span><span class="sxs-lookup"><span data-stu-id="09c62-351">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="09c62-352">Případně postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="09c62-352">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="09c62-353">Přidejte následující prvky k `SecurityClasses` kapitoly *webové\_MediumTrust.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="09c62-353">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="09c62-354">Vytvořit novou sadu oprávnění *webové\_MediumTrust.config* soubor s požadovanými oprávněními následující:</span><span class="sxs-lookup"><span data-stu-id="09c62-354">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="09c62-355">Použití oprávnění nastavena na systém SQL Server Compact umístěním následující prvky *webové\_MediumTrust.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="09c62-355">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="09c62-356">Problém: Galerie a PhpBB webové aplikace zobrazí chybu "Služba není k dispozici" po publikování</span><span class="sxs-lookup"><span data-stu-id="09c62-356">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="09c62-357">Za určitých okolností publikování aplikace způsobí chybu "služba není k dispozici".</span><span class="sxs-lookup"><span data-stu-id="09c62-357">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="09c62-358">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-358">**Workaround**</span></span>  
> <span data-ttu-id="09c62-359">Ve službě WebMatrix, přidejte zpětné lomítko (\) na konec názvu serveru v **nastavení publikování** okna a potom aplikaci znovu publikovat.</span><span class="sxs-lookup"><span data-stu-id="09c62-359">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="09c62-360">Problém: Rozložení webu Moodle a odkazy jsou přerušená po publikování</span><span class="sxs-lookup"><span data-stu-id="09c62-360">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="09c62-361">Po publikování aplikace Moodle aplikace nebude fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="09c62-361">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="09c62-362">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-362">**Workaround**</span></span>  
> <span data-ttu-id="09c62-363">Ve službě WebMatrix, přidejte na konec lomítko (/) **název lokality** pole **nastavení publikování** okna a potom aplikaci znovu publikovat.</span><span class="sxs-lookup"><span data-stu-id="09c62-363">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="09c62-364">Problém: Publikování nopCommerce selže s chybou databáze</span><span class="sxs-lookup"><span data-stu-id="09c62-364">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="09c62-365">Publikování nopCommerce selže a ohlásí chybu databáze jako "Vložit do nop\_tabulky protokolu se nezdařilo."</span><span class="sxs-lookup"><span data-stu-id="09c62-365">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="09c62-366">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-366">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="09c62-367">Ve službě WebMatrix, klikněte na tlačítko **spustit** nopCommerce místně spustíte.</span><span class="sxs-lookup"><span data-stu-id="09c62-367">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="09c62-368">Přihlaste se na stránku pro správu.</span><span class="sxs-lookup"><span data-stu-id="09c62-368">Log into the administration page.</span></span>
> 3. <span data-ttu-id="09c62-369">Klikněte **systému** nabídky.</span><span class="sxs-lookup"><span data-stu-id="09c62-369">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="09c62-370">Klikněte **protokolu** možnost.</span><span class="sxs-lookup"><span data-stu-id="09c62-370">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="09c62-371">Klikněte **vymazat protokol** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="09c62-371">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="09c62-372">NopCommerce znovu publikujte.</span><span class="sxs-lookup"><span data-stu-id="09c62-372">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="09c62-373">Problém: Silverstripe CMS zobrazí "HTTP 500 PHP FCGI chyba" při stažení publikované lokality</span><span class="sxs-lookup"><span data-stu-id="09c62-373">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="09c62-374">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-374">**Workaround**</span></span>  
> <span data-ttu-id="09c62-375">Po kliknutí na tlačítko **stažení publikování serveru**, přeskočte `silverstripe-cache/manifest_main` v **Náhled publikování**.</span><span class="sxs-lookup"><span data-stu-id="09c62-375">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="09c62-376">Tento soubor se používá pro ukládání do mezipaměti pro účely a je specifická pro každý počítač.</span><span class="sxs-lookup"><span data-stu-id="09c62-376">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="09c62-377">Problém: Subtext zobrazí "Error Server v aplikaci '/'" při stahování publikované lokality</span><span class="sxs-lookup"><span data-stu-id="09c62-377">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="09c62-378">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-378">**Workaround**</span></span>  
> <span data-ttu-id="09c62-379">Otevřete v lokalitě *web.config* souboru a nahraďte ID uživatele a heslo v připojovací řetězec databáze s přihlašovacími údaji správce systému SQL Server ("sa" přihlašovací údaje).</span><span class="sxs-lookup"><span data-stu-id="09c62-379">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="09c62-380">Alternativně postupujte podle těchto kroků aby nového uživatelského účtu, který jste se přihlásili `db_owner` oprávnění:</span><span class="sxs-lookup"><span data-stu-id="09c62-380">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="09c62-381">Nainstalujte SQL Server Management Studio pomocí služby instalace webové platformy.</span><span class="sxs-lookup"><span data-stu-id="09c62-381">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="09c62-382">Připojte se k místní instanci systému SQL Server Express (ve výchozím nastavení, `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="09c62-382">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="09c62-383">Klikněte na tlačítko **databáze** &gt; *[localSubtextDatabase]* &gt; **zabezpečení** &gt; **uživatelé** &gt; *[localSubtextUser*] (výchozí hodnota je `subtextuser`], klikněte pravým tlačítkem a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="09c62-383">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="09c62-384">Vyberte **db\_vlastníka** v části role členství.</span><span class="sxs-lookup"><span data-stu-id="09c62-384">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="09c62-385">Problém: Lokality nemusí fungovat po publikování Pokud pole "Cílová adresa URL" není s předponou http:// nebo https://.</span><span class="sxs-lookup"><span data-stu-id="09c62-385">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="09c62-386">V **nastavení publikování** dialogové okno, pokud cílová adresa URL začíná `http://` nebo `https://`, web nemusí fungovat po nasazení.</span><span class="sxs-lookup"><span data-stu-id="09c62-386">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="09c62-387">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-387">**Workaround**</span></span>  
> <span data-ttu-id="09c62-388">Ujistěte se, že před publikováním serveru, cílovou adresu URL v **nastavení publikování** dialogové okno začíná `http://` nebo `https://`.</span><span class="sxs-lookup"><span data-stu-id="09c62-388">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="09c62-389">Problém: Publikování databáze MySQL selže s chybou "se nepodařilo publikovat databázi.</span><span class="sxs-lookup"><span data-stu-id="09c62-389">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="09c62-390">To může dojít, pokud Vzdálená databáze nemůže spustit skript."</span><span class="sxs-lookup"><span data-stu-id="09c62-390">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="09c62-391">Chyba může mít několik důvodů.</span><span class="sxs-lookup"><span data-stu-id="09c62-391">The error can occur for a number of reasons.</span></span> <span data-ttu-id="09c62-392">Jedním z důvodů, že se zobrazí tato chyba je pokud databázového skriptu obsahuje znak, jeden uvozovky (') a databáze MySQL cílové výchozí znakovou sadu není na UTF-8.</span><span class="sxs-lookup"><span data-stu-id="09c62-392">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="09c62-393">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-393">**Workaround**</span></span>  
> <span data-ttu-id="09c62-394">Nastavte výchozí znakovou sadu pro vzdálené databáze MySQL na UTF-8.</span><span class="sxs-lookup"><span data-stu-id="09c62-394">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="09c62-395">Problém: Některé odkazy nejsou viditelné v DotNetNuke po publikování nebo stahování webu</span><span class="sxs-lookup"><span data-stu-id="09c62-395">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="09c62-396">Pokud publikujete nebo stáhnout aplikace DotNetNuke lokality, musíte může a vymažte mezipaměť získat nové odkazy na webu.</span><span class="sxs-lookup"><span data-stu-id="09c62-396">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="09c62-397">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-397">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="09c62-398">Přihlaste se jako "Hostitel".</span><span class="sxs-lookup"><span data-stu-id="09c62-398">Log in as "Host".</span></span>
> 2. <span data-ttu-id="09c62-399">Přejděte do nabídky hostitele a vyberte možnost **nastavení hostitele**.</span><span class="sxs-lookup"><span data-stu-id="09c62-399">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="09c62-400">Přejděte dolů a v části **Upřesnit nastavení**, rozbalte položku **nastavení výkonu**.</span><span class="sxs-lookup"><span data-stu-id="09c62-400">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="09c62-401">Klikněte **vymazat mezipaměť** odkaz pro stránky.</span><span class="sxs-lookup"><span data-stu-id="09c62-401">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="09c62-402">Přejděte do dolní části stránky a restartování aplikace.</span><span class="sxs-lookup"><span data-stu-id="09c62-402">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="09c62-403">Problém: V AtomSite některé odkazy jsou přerušená po stažení publikované lokality</span><span class="sxs-lookup"><span data-stu-id="09c62-403">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="09c62-404">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-404">**Workaround**</span></span>  
> <span data-ttu-id="09c62-405">V *service.config* souboru *users.config* soubor a všechny *.xml* soubory, nahraďte řetězce adresy URL (například `http://myhost.com/atomsite`) s místní (například `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="09c62-405">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="09c62-406">Problém: Založenou na MySQL aplikace, jako je WordPress nezdaří publikování a ohlásí chybu databáze</span><span class="sxs-lookup"><span data-stu-id="09c62-406">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="09c62-407">Ve výchozím nastavení nainstaluje služba WebMatrix MySQL s znakové sady UTF-8.</span><span class="sxs-lookup"><span data-stu-id="09c62-407">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="09c62-408">Pokud instalujete MySQL sami a není znakovou sadu UTF-8 (například je Latin1), se nemusí podařit proces publikování databází.</span><span class="sxs-lookup"><span data-stu-id="09c62-408">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="09c62-409">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-409">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="09c62-410">Změňte sadu pro databázi MySQL na UTF-8 znaků.</span><span class="sxs-lookup"><span data-stu-id="09c62-410">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="09c62-411">(Podrobnosti najdete v tématu [Server znaková sada a kolace](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) na webu MySQL.)</span><span class="sxs-lookup"><span data-stu-id="09c62-411">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="09c62-412">Nainstalujte aplikaci znovu.</span><span class="sxs-lookup"><span data-stu-id="09c62-412">Reinstall the application.</span></span>
> 3. <span data-ttu-id="09c62-413">Aplikaci znovu publikujte.</span><span class="sxs-lookup"><span data-stu-id="09c62-413">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="09c62-414">Problém: "Stažení publikování serveru" selže pro aplikace, které instalační program založené na prohlížeči</span><span class="sxs-lookup"><span data-stu-id="09c62-414">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="09c62-415">Některé aplikace (například Kentico CMS) vyžadují, abyste je spustit v prohlížeči, aby bylo možné provést po instalaci instalační program, jako je například vytváření databáze.</span><span class="sxs-lookup"><span data-stu-id="09c62-415">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="09c62-416">Pokud publikujete aplikaci jako to bez dokončení instalace založené na prohlížeči, pokus o stažení stejné síti ze vzdáleného serveru se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="09c62-416">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="09c62-417">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-417">**Workaround**</span></span>  
> <span data-ttu-id="09c62-418">Dokončení instalace založené na prohlížeči před publikováním webu.</span><span class="sxs-lookup"><span data-stu-id="09c62-418">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="09c62-419">Problém: "Stažení publikování serveru" selže s chybou databáze pro DotNetNuke a Kooboo CMS</span><span class="sxs-lookup"><span data-stu-id="09c62-419">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="09c62-420">Pokud pokusu o stažení aplikace ze serveru a mít pověření správce v připojovací řetězec databáze **nastavení publikování** dialogové okno, může se zobrazit následující chyby v protokolu publikovat:</span><span class="sxs-lookup"><span data-stu-id="09c62-420">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="09c62-421">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="09c62-421">**Workaround**</span></span>  
> <span data-ttu-id="09c62-422">Pokud je to možné, znovu publikovat webu (nebo její zveřejnění) pomocí přihlašovacích údajů bez oprávnění správce pro databázi.</span><span class="sxs-lookup"><span data-stu-id="09c62-422">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="09c62-423">Další informace</span><span class="sxs-lookup"><span data-stu-id="09c62-423">For More Information</span></span>

<span data-ttu-id="09c62-424">Další informace o službě WebMatrix 1.0 najdete na následujících webech:</span><span class="sxs-lookup"><span data-stu-id="09c62-424">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="09c62-425">IIS.net</span><span class="sxs-lookup"><span data-stu-id="09c62-425">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="09c62-426">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="09c62-426">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="09c62-427">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="09c62-427">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="09c62-428">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="09c62-428">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="09c62-429">Všechna práva vyhrazena.</span><span class="sxs-lookup"><span data-stu-id="09c62-429">All Rights Reserved.</span></span> <span data-ttu-id="09c62-430">[Podmínky použití](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="09c62-430">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
