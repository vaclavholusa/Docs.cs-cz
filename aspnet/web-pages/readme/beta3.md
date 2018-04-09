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
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="4f8d1-103">Web Matrix a rozhraní ASP.NET Web Pages (Razor) Beta 3 verze Readme</span><span class="sxs-lookup"><span data-stu-id="4f8d1-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="4f8d1-104">Web Matrix a rozhraní ASP.NET Web Pages (Razor) Beta 3 verze Readme</span><span class="sxs-lookup"><span data-stu-id="4f8d1-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="4f8d1-105">9 listopadu 2010</span><span class="sxs-lookup"><span data-stu-id="4f8d1-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="4f8d1-106">Obsah</span><span class="sxs-lookup"><span data-stu-id="4f8d1-106">Contents</span></span>

- [<span data-ttu-id="4f8d1-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="4f8d1-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="4f8d1-108">Instalace</span><span class="sxs-lookup"><span data-stu-id="4f8d1-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="4f8d1-109">Nové funkce, změny a známé problémy ve verzi Beta 3</span><span class="sxs-lookup"><span data-stu-id="4f8d1-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="4f8d1-110">Problémy instalace služby WebMatrix</span><span class="sxs-lookup"><span data-stu-id="4f8d1-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="4f8d1-111">Webové stránky ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4f8d1-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="4f8d1-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="4f8d1-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="4f8d1-113">Instalace aplikace</span><span class="sxs-lookup"><span data-stu-id="4f8d1-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="4f8d1-114">Publikování aplikací</span><span class="sxs-lookup"><span data-stu-id="4f8d1-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="4f8d1-115">Další problémy</span><span class="sxs-lookup"><span data-stu-id="4f8d1-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="4f8d1-116">Další informace</span><span class="sxs-lookup"><span data-stu-id="4f8d1-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="4f8d1-117">Přehled</span><span class="sxs-lookup"><span data-stu-id="4f8d1-117">Overview</span></span>

> <span data-ttu-id="4f8d1-118">Microsoft WebMatrix Beta je bezplatných webových zásobníku vývoj, který nainstaluje v minutách.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="4f8d1-119">Webový server se integruje s databáze a programovací rozhraní pro vytvoření jednoho integrovaného rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="4f8d1-120">Beta verze služby WebMatrix můžete zjednodušit kód, testování a publikování vlastní web ASP.NET a PHP způsob nebo používáte službu WebMatrix Beta ke spuštění nového webu pomocí Oblíbené open-source aplikace, jako je aplikace DotNetNuke, Umbraco, WordPress nebo Joomla.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="4f8d1-121">Beta verze služby WebMatrix používá stejný výkonný webový server, databázový stroj a prostředí architektury, které se spustí svůj web na Internetu, které umožňuje přechod z vývojového do produkčního prostředí hladký a bezproblémový.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="4f8d1-122">Instalace</span><span class="sxs-lookup"><span data-stu-id="4f8d1-122">Installation</span></span>

> <span data-ttu-id="4f8d1-123">Chcete-li nainstalovat službu WebMatrix Beta 3, použijete [Microsoft webové platformy verze 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="4f8d1-124">Po instalaci webové platformy, můžete k instalaci služby WebMatrix Beta 3.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="4f8d1-125">Pokud máte potíže s během instalace, podívejte se na [odstraňování potíží s instalačního programu webové platformy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="4f8d1-126">Pokyny k publikování aplikací</span><span class="sxs-lookup"><span data-stu-id="4f8d1-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="4f8d1-127">V tématu [podrobné pokyny k publikování aplikací](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="4f8d1-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="4f8d1-128">Nové funkce, změny, andKnown problémy</span><span class="sxs-lookup"><span data-stu-id="4f8d1-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="4f8d1-129">Instalace služby WebMatrix Beta 3</span><span class="sxs-lookup"><span data-stu-id="4f8d1-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="4f8d1-130">Problém: WebMatrix Beta 3 je k dispozici pouze na platformách, které podporují rozhraní Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="4f8d1-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="4f8d1-131">Rozhraní .NET Framework verze 4 je vyžadována pro beta verzi služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="4f8d1-132">V některých případech instalační program beta verzi služby WebMatrix vám umožní instalaci na platformu, která není součástí sady podporované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="4f8d1-133">Konkrétně Windows Vista bez aktualizace SP1 vám umožní zahájit instalaci služby WebMatrix Beta, ale součásti rozhraní .NET Framework 4 se nezdaří a blokovat instalaci.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="4f8d1-134">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-134">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-135">Nainstalujte na podporované platformě, která zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="4f8d1-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="4f8d1-136">Windows 7</span></span>
> - <span data-ttu-id="4f8d1-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="4f8d1-137">Windows Server 2008</span></span>
> - <span data-ttu-id="4f8d1-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="4f8d1-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="4f8d1-139">Windows Vista SP1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="4f8d1-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="4f8d1-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="4f8d1-140">Windows XP SP3</span></span>
> - <span data-ttu-id="4f8d1-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="4f8d1-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="4f8d1-142">Problém: Nelze nainstalovat službu WebMatrix Beta 3, když se nainstaluje Microsoft Visual Studio 2008 bez Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="4f8d1-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="4f8d1-143">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-143">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-144">Nainstalujte [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) z webu Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="4f8d1-145">Problém: V mezipaměti GAC nejsou nainstalovány některé sestavení pro SQL Server Compact 4.0</span><span class="sxs-lookup"><span data-stu-id="4f8d1-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="4f8d1-146">Spravovaná sestavení pro SQL Server Compact 4.0 nejsou umístěny v globální mezipaměti sestavení (GAC), při instalaci systému SQL Server Compact 4.0 na 64bitovém počítači a má počítač pouze profil rozhraní .NET Framework 3.5 SP1 klient nainstalován.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="4f8d1-147">Spravovaná sestavení, které nejsou nainstalované v mezipaměti GAC jsou:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="4f8d1-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span><span class="sxs-lookup"><span data-stu-id="4f8d1-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="4f8d1-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span><span class="sxs-lookup"><span data-stu-id="4f8d1-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="4f8d1-150">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-150">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-151">Odinstalujte systém SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="4f8d1-152">Stáhněte a nainstalujte plnou verzi rozhraní .NET Framework 3.5 SP1 z následujícího umístění:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="4f8d1-153">Microsoft .NET Framework 3.5 Service pack 1 (úplná balíček)</span><span class="sxs-lookup"><span data-stu-id="4f8d1-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="4f8d1-154">Znovu nainstalujte SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="4f8d1-155">Problém: Nelze odinstalovat, SQL Server Compact pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4f8d1-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="4f8d1-156">Odinstalace systému SQL Server Compact pomocí možnosti příkazového řádku v této verzi nefunguje.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="4f8d1-157">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-157">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-158">Použití *programy a funkce* v Ovládacích panelech odinstalujte Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="4f8d1-159">ASP.NET – webové stránky</span><span class="sxs-lookup"><span data-stu-id="4f8d1-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="4f8d1-160">Tato část dokumentu popisuje nové funkce, změny a známé problémy ve verzi Beta 3 z webových stránek ASP.NET se syntaxí Razor.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="4f8d1-161">Nové funkce</span><span class="sxs-lookup"><span data-stu-id="4f8d1-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="4f8d1-162">Změny</span><span class="sxs-lookup"><span data-stu-id="4f8d1-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="4f8d1-163">Problémy</span><span class="sxs-lookup"><span data-stu-id="4f8d1-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="4f8d1-164">Nové funkce ve verzi Beta 3 pro rozhraní ASP.NET Web Pages se syntaxí Razor</span><span class="sxs-lookup"><span data-stu-id="4f8d1-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="4f8d1-165">Nové: "Html.Raw" metoda vykresluje nezakódovaný kód</span><span class="sxs-lookup"><span data-stu-id="4f8d1-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="4f8d1-166">Nové `Html.Raw` metoda umožňuje vykreslení kódu HTML jako značka namísto vykreslování kódovaného výstupu.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="4f8d1-167">(Ve výchozím nastavení, ASP.NET Razor kóduje řetězce před jejich vykreslení.) Syntaxe je následující:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="4f8d1-168">Následující příklad ukazuje, jak používat `Html.Raw`:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="4f8d1-169">Změny ve verzi Beta 3 pro rozhraní ASP.NET Web Pages se syntaxí Razor</span><span class="sxs-lookup"><span data-stu-id="4f8d1-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="4f8d1-170">Změna: Metody "HrefAttribute" Odebrat</span><span class="sxs-lookup"><span data-stu-id="4f8d1-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="4f8d1-171">`HrefAttribute` Metodu `WebPage` třída byla odebrána.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="4f8d1-172">Tato pomocná byl použit ke kódování nebezpečné znaky v adresách URL.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="4f8d1-173">Se už nevyžaduje protože ASP.NET Razor automaticky kóduje řetězce.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="4f8d1-174">(Pomocí nové `Html.Raw` metoda k vykreslení nekódovaného řetězce.)</span><span class="sxs-lookup"><span data-stu-id="4f8d1-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="4f8d1-175">Změnu: Syntaxe deklarativní "@helper" Pomocníci změnit</span><span class="sxs-lookup"><span data-stu-id="4f8d1-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="4f8d1-176">Ve verzi Beta 3, ASP.NET změní, jak ho analyzuje pomocné rutiny, které jsou vytvořené pomocí `@helper` syntaxe.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="4f8d1-177">V podstatě `@helper` syntaxe je nyní analyzovat jako blok kódu místo jako blok kódu, který může obsahovat kód.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="4f8d1-178">Proto kód do pomocné rutiny nemusí být uzavřená do `@{ }` bloky.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="4f8d1-179">Naopak značky do pomocné rutiny musí být explicitně zahrnuta do elementů HTML nebo v prostředí ASP.NET Razor `<text></text>` značky.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="4f8d1-180">Například následující `@helper` syntaxe funguje ve verzi Beta 3:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="4f8d1-181">Ve verzi Beta 3 je třeba změnit tohoto pomocníka, aby vypadala jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="4f8d1-182">Všimněte si, že `@{ }` znaků kolem počáteční kód v pomocné rutiny se už používá.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="4f8d1-183">Je to proto, že obsah pomocníky jsou považovány za blok kódu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="4f8d1-184">Pomocné rutiny vykreslí značku, která začíná otevřením `<a>` značky.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="4f8d1-185">Pokud musí pomocné rutiny vykreslení jako prostý text ani značky, které neobsahují ukončovací značky (například `<meta>` značky), obsah k vykreslení musí být v `<text></text>` značky.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="4f8d1-186">Změn: Odebrat "WebPageContext.HttpContext"</span><span class="sxs-lookup"><span data-stu-id="4f8d1-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="4f8d1-187">`WebPageContext.HttpContext` Vlastnost byla odebrána.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="4f8d1-188">Místo nich se používá `HttpContext.Current`.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="4f8d1-189">( `WebPageContext.HttpContext` Tato vlastnost jednoduše zabalit.)</span><span class="sxs-lookup"><span data-stu-id="4f8d1-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="4f8d1-190">Změna: Pomocník "Facebook" přesunout do nového balíčku</span><span class="sxs-lookup"><span data-stu-id="4f8d1-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="4f8d1-191">`Facebook` Pomocné rutiny, byl přesunut do *Facebook.Helper* knihovny, která zahrnuje `Facebook` pomocné rutiny a další funkce.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="4f8d1-192">Je třeba nainstalovat tuto knihovnu jako samostatný balíček, jak je popsáno v "Instalace pomocné rutiny s balíček Manager" v tomto kurzu [Začínáme s ASP.NET stránky](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="4f8d1-193">Změna: Typy členství, Role a zabezpečení přesune do nového sestavení</span><span class="sxs-lookup"><span data-stu-id="4f8d1-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="4f8d1-194">Následující typy byly přesunuty do `WebMatrix.WebData` sestavení:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="4f8d1-195">Změna: Třída "TagBuilder" přesunout do System.Web.WebPages.dll sestavení</span><span class="sxs-lookup"><span data-stu-id="4f8d1-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="4f8d1-196">`TagBuilde` r třída byl přesunut do System.Web.WebPages.dll sestavení.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="4f8d1-197">Dřív šlo v sestavení, které bylo součástí ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="4f8d1-198">Tato změna znamená, že nemáte-li používat nainstalujte rozhraní ASP.NET MVC `TagBuilder` třídy.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="4f8d1-199">Třída je však stále v `System.Web.Mvc` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="4f8d1-200">Chcete-li použít `TagBuilder` – třída (například v vlastní ASP.NET Razor pomocníka), musí odkazovat obor názvů (například přidáním `@using System.Web.Mvc` kódu).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="4f8d1-201">Změnu: Žádost o ověření syntaxe změnit; Třída "Ověření" Odebrat</span><span class="sxs-lookup"><span data-stu-id="4f8d1-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="4f8d1-202">Ve verzi Beta 3 Pokud chcete zakázat ověřování pro jednotlivá pole nebo sady polí, můžete zavolat `Validation.Exclude` metody předávání v název nebo názvy polí, které chcete vyloučit z ověření.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="4f8d1-203">Nové syntaxe je k dispozici ve verzi Beta 3 pro vynechání ověřování.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="4f8d1-204">`Validation` Metodu použitou v Beta 3 byla odebrána.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="4f8d1-205">Pokud není zakážete ověření požadavku, pokud uživatelé se pokusí odeslat kód HTML (například pomocí editoru formátovaného textu na stránce), web zobrazovat chyby jako *hodnotu potenciálně nebezpečná Request.Form byla zjištěna z klienta*a uživatelský vstup nebyla přijata.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="4f8d1-206">Pokud zakážete ověření požadavku, je nutné ručně zkontrolovat vstup uživatele, abyste měli jistotu, že ho neobsahuje potenciálně nebezpečného kódu nebo skriptu pomocí něco podobného jako [skriptování V4.0 knihovny Microsoft proti mezi lokality](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="4f8d1-207">Chcete-li zakázat automatické ověření, zavolejte `Request.Unvalidated` metoda, předejte název pole nebo jiného objektu post, které chcete vynechat ověření žádosti pro.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="4f8d1-208">Tuto metodu můžete použít k ověření pro všechny položky v obejít `Form`, `QueryString`, `Cookies`, a `ServerVariables` kolekce.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="4f8d1-209">Následující příklady ukazují, jak používat `Unvalidated` metoda:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="4f8d1-210">Známé problémy pro ASP.NET Web Pages se syntaxí Razor</span><span class="sxs-lookup"><span data-stu-id="4f8d1-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="4f8d1-211">Problém: Neočekávané chování při použití vlastní uživatelská tabulka pro členství</span><span class="sxs-lookup"><span data-stu-id="4f8d1-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="4f8d1-212">K chybě při inicializaci zprostředkovatele členství pro web ASP.NET Razor, zavoláte `WebSecurity.InitializeDatabaseConnection` metoda.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="4f8d1-213">(Ve službě WebMatrix, zahrnuje šabloně Starter Site volání této metody v  *\_AppStart.cshtml* souboru.) Pokud `autoCreateTables` parametr této metody je nastaven na hodnotu true (ve výchozím nastavení je nastavena na hodnotu true v šabloně Starter Site), a pokud je název tabulky nerozpoznané předaný metodě (druhý parametr), metoda nevyvolá chybu.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="4f8d1-214">Místo toho automaticky vytvoří v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="4f8d1-215">To může být problém, pokud máte v úmyslu použít vlastní uživatelská tabulka pro členství, ale předat název nesprávné tabulky k `WebSecurity.InitializeDatabaseConnection` metoda.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="4f8d1-216">Vzhledem k tomu, že metoda není ve výchozím nastavení vyvolá chybu, pokud neexistuje v tabulce, které zadáte, a protože místo toho vytvoří novou tabulku, můžete aplikaci pravděpodobně fungovat.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="4f8d1-217">Aplikační kód, který závisí na vlastní uživatelská tabulka (a na pole v něm) můžete nakonec nezdaří s neočekávaným chybám.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="4f8d1-218">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-218">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-219">Ujistěte se, že název předaná `InitializeDatabaseConnection` metoda odpovídá uživatelský profil tabulky v databázi členství nebo zkontrolujte, zda `autoCreateTables` parametr je nastaven na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="4f8d1-220">Problém: "Nepodařilo se vygenerovat uživatelskou instanci systému SQL Server" Chyba</span><span class="sxs-lookup"><span data-stu-id="4f8d1-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="4f8d1-221">Pokud služba WebMatrix webové aplikace používá systém SQL Server Express a je spuštěna služba IIS 7.5 na Windows 7 nebo Windows Server 2008 R2, může se zobrazit chybu, která určuje, že systém SQL Server nelze načíst cestu k místní aplikace uživatele za běhu.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="4f8d1-222">**Alternativní řešení** Ujistěte se, že účet systému Windows, na kterém se aplikace spouští pod (obvykle síťová služba) má oprávnění ke čtení/zápisu pro kořenové složky aplikace a podsložky jako *aplikace\_dat*.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="4f8d1-223">Podrobnější informace najdete v článku znalostní báze Knowledge Base [problémy s SQL Server Express uživatele vytváření instancí a projekty webových aplikací ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="4f8d1-224">Problém: V sadě Visual Studio, obory názvů pro vlastní sestavení (DLL) nejsou importovány automaticky</span><span class="sxs-lookup"><span data-stu-id="4f8d1-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="4f8d1-225">Pokud používáte vlastní sestavení v projektu v sadě Visual Studio, obory názvů v těchto sestavení deklarována nejsou importovány automaticky v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="4f8d1-226">Odkazy na vlastní typy v důsledku toho nemusí rozpoznat v době návrhu a jsou označeny jako není platný v sadě Visual Studio (pomocí "vlnovku").</span><span class="sxs-lookup"><span data-stu-id="4f8d1-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="4f8d1-227">K tomuto problému dochází pouze v době návrhu v sadě Visual Studio; vlastní aplikace běží správně.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="4f8d1-228">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-228">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-229">Zahrnout `using` – příkaz (`imports` v jazyce Visual Basic), odkazuje na entity, které nejsou rozpoznány v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="4f8d1-230">Problém: Visual Studio IntelliSense a projekt šablony k dispozici pouze v architektuře ASP.NET MVC verze 3</span><span class="sxs-lookup"><span data-stu-id="4f8d1-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="4f8d1-231">Instalace technologie ASP.NET Web Pages také nenainstaluje nástroje pro sadu Visual Studio jako IntelliSense a projekt šablony pro aplikace ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="4f8d1-232">**Alternativní řešení** používáte IntelliSense a projekt šablony pro aplikace webových stránek ASP.NET v sadě Visual Studio, nainstalujte technologii ASP.NET MVC 3 RC buď prostřednictvím instalace webové platformy nebo [samostatný instalační program](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="4f8d1-233">Problém: "&lt;pomocná&gt; třída nebyla nalezena" Chyba</span><span class="sxs-lookup"><span data-stu-id="4f8d1-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="4f8d1-234">Po upgradu na verzi Beta 3, může se zobrazit chyba, pomocnou třídu (například `Facebook` třída) nelze najít.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="4f8d1-235">Od verze Beta 2 a budete pokračovat ve verzi Beta 3, byl přesunut pomocné rutiny do balíčků, které je třeba explicitně nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="4f8d1-236">Existující lokality nejsou upgradovány na zahrnují tyto balíčky; To zahrnuje lokality v *\My Documents\IISExpress* nebo *\My Documents\My weby* složek.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="4f8d1-237">Konkrétně se zobrazí tato chyba, pokud použijete výchozí web v *osobní weby* (web1), který obsahuje odkaz na `Twitter` pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="4f8d1-238">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-238">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-239">Komentář volání žádné pomocné rutiny v lokalitě, spusťte  *\_správce* stránky a nainstalujte balíček nebo balíčky, které zahrnují pomocné rutiny, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="4f8d1-240">Po instalaci balíčku, zrušte komentář u řádky, které odkazují pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="4f8d1-241">Problém: Nasazení Beta 3 ASP.NET Razor sestavení do složky Bin nemusí fungovat na hostování webů</span><span class="sxs-lookup"><span data-stu-id="4f8d1-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="4f8d1-242">Pokud nasadíte webu technologie ASP.NET Web Pages na hostitelském serveru, a Pokud nasazujete ASP.NET Razor Beta 3 sestavení do lokality *Bin* složky, může docházet k chybám, včetně následujících:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="4f8d1-243">To může dojít, pokud poskytovatel hostingu nainstaloval ASP.NET Web Pages Beta 1 sestavení do serveru globální mezipaměti (sestavení GAC).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="4f8d1-244">Sestavení v mezipaměti GAC získat přednost přes nainstalovány místně v sestavení *Bin* složky.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="4f8d1-245">**Alternativní řešení** obraťte se na svého poskytovatele hostingu, abyste se přesvědčili, že se zobrazuje chyby z důvodu konfliktu mezi verzemi poskytovatele sestavení a vaše.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="4f8d1-246">Pokud ano, požadavku, že poskytovatel hostingu aktualizovat sestavení v globální mezipaměti serveru.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="4f8d1-247">Problém: Čtení informačních kanálů nebo jiné externí data prostřednictvím serveru proxy</span><span class="sxs-lookup"><span data-stu-id="4f8d1-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="4f8d1-248">Serveru se systémem lokality je za proxy serverem, může být potřeba konfigurovat informace o proxy serveru v *Web.config* souboru, aby mohl číst informace, které pochází z mimo váš web.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="4f8d1-249">Například pokud použijete `ReCaptcha` pomocné rutiny, pomocné rutiny komunikuje se službou nástroje reCAPTCHA, ale mohou být blokovány proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="4f8d1-250">Podobně informační kanály, které se používají na webových stránkách ASP.NET, jako je například používá Správce balíčků informačního kanálu může vyžadovat konfiguraci proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="4f8d1-251">Pokud máte potíže v práci s externí službu nebo práce s tímto balíčkem kanálu, uveďte následující prvky do vaší aplikace kořenové *Web.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="4f8d1-252">Další informace o konfiguraci proxy serveru najdete v tématu [ &lt;proxy&gt; – Element (nastavení sítě)](https://msdn.microsoft.com/library/sa91de1e.aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="4f8d1-253">Problém: "Nelze načíst Microsoft.Web.Infrastructure.dll" Chyba</span><span class="sxs-lookup"><span data-stu-id="4f8d1-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="4f8d1-254">Pokud jste dříve nainstalovali verzi Beta 1 z webových stránek ASP.NET se syntaxí Razor a poté nainstalujte verzi Beta 3, nainstalují se všechny odpovídající sestavení v mezipaměti GAC s výjimkou *Microsoft.Web.Infrastructure.dll*.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="4f8d1-255">V důsledku toho, když spustíte stránky ASP.NET Razor, zobrazí chybu, která označuje, že *Microsoft.Web.Infrastructure.dll* nelze načíst.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="4f8d1-256">Tento problém neproběhne-li načíst verzi Beta 3 na čistém počítači.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="4f8d1-257">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-257">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-258">V Ovládacích panelech odinstalujte rozhraní ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="4f8d1-259">Znovu nainstalujte verzi Beta 3.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="4f8d1-260">Problém: Odinstalace rozhraní .NET Framework verze 4 zakáže technologie ASP.NET Web Pages se syntaxí Razor</span><span class="sxs-lookup"><span data-stu-id="4f8d1-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="4f8d1-261">Pokud odinstalujete rozhraní .NET Framework verze 4 a znovu ji nainstalovat, technologie ASP.NET Web Pages se syntaxí Razor je zakázané.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="4f8d1-262">Stránky s *.cshtml* rozšíření se nespustí správně.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="4f8d1-263">Rozhraní ASP.NET Web Pages zaregistruje sestavení v kořenu počítač *Web.config* soubor odebrán souboru a odebrání rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="4f8d1-264">Opětovné instalace rozhraní .NET Framework nainstaluje novou verzi konfiguračního souboru, ale nepřidá odkaz na sestavení pro ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="4f8d1-265">**Alternativní řešení** po opětovné instalaci rozhraní .NET Framework, přeinstalujte rozhraní ASP.NET Web Pages se syntaxí Razor.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="4f8d1-266">Tím se přidá následující elementu, který chcete *Web.config* soubor v kořenu počítač, který je obvykle v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="4f8d1-267">Problém: Aplikace dříve nasazené s ASP.NET sestavení do složky Bin dojde k chybě</span><span class="sxs-lookup"><span data-stu-id="4f8d1-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="4f8d1-268">Během nasazování kopií sestavení rozhraní ASP.NET Web Pages (například *Microsoft.WebPages.dll*) k *Bin* složky webu na serveru.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="4f8d1-269">(Tomu může dojít automaticky během nasazení nebo proto, že vývojář explicitně zkopírovat sestavení.) Ale pokud je nainstalován ve verzi Beta 3, chyby dochází k, jako je například chyby, které nebyly nalezeny některé typy.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="4f8d1-270">K tomu dochází, protože několik typů webových stránek ASP.NET se přesunuly do různých oborech názvů pro verzi Beta 3.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="4f8d1-271">**Alternativní řešení** </span><span class="sxs-lookup"><span data-stu-id="4f8d1-271">**Workaround** </span></span>  
> <span data-ttu-id="4f8d1-272">Vymazat *Bin* složku nasazené aplikace, zkopírujte nové sestavení do složky (nebo znovu nasadit aplikace) a restartujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="4f8d1-273">Problém: Adresy URL bez přípony nenašli.cshtml/.vbhtml soubory na službě IIS 7 nebo IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="4f8d1-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="4f8d1-274">Na službě IIS 7 nebo IIS 7.5, nejsou schopna najít stránky, které mají požadavky s adresou URL takto *.cshtml* nebo *.vbhtml* rozšíření:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="4f8d1-275">Tento problém nastane, protože přepisování adres URL není povoleno ve výchozím nastavení pro službu IIS 7 nebo IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="4f8d1-276">Nejpravděpodobnějším scénář je, že se nezobrazí problém při testování místně pomocí služby IIS Express, ale dojde při nasazení webu k hostování webu.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="4f8d1-277">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="4f8d1-278">Pokud budete mít kontrolu nad počítači serveru, na počítači serveru nainstalujte aktualizaci popsanou v [aktualizace je dostupná, že umožňuje určité služby IIS 7.0 nebo IIS 7.5 obslužné rutiny pro zpracování požadavků, jejichž adresy URL na konci období](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="4f8d1-279">Pokud nemáte kontrolu nad počítači serveru (například nasazujete k hostování webu), přidejte následující na web *Web.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="4f8d1-280">Problém: Pomocí projektu webové aplikace nebo stránky ASP.NET MVC a ASP.NET Web ve stejné aplikaci</span><span class="sxs-lookup"><span data-stu-id="4f8d1-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="4f8d1-281">Pokud v projektu webové aplikace nebo aplikace ASP.NET MVC byly pomocí webových stránek ASP.NET, může se zobrazit chyba, *WebPageHttpApplication* nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="4f8d1-282">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-282">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-283">Pokud se tato chyba, změňte základní třídu, ze kterého aplikace pochází.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="4f8d1-284">V *Global.asax* souboru, změňte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="4f8d1-285">k tomuto:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="4f8d1-286">To platí obrátí změnu, která byla zavedena ve verzi Beta 1 z webových stránek ASP.NET se syntaxí Razor.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="4f8d1-287">Problém: Nasazení aplikace do počítače, který nemá systém SQL Server Compact nainstalován</span><span class="sxs-lookup"><span data-stu-id="4f8d1-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="4f8d1-288">Aplikace, které obsahují systém SQL Server Compact databáze můžete spustit na počítači, kde systém SQL Server Compact není nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="4f8d1-289">Microsoft WebMatrix Beta 3 automaticky zkopíruje tyto binární soubory pro vás a provede odpovídající *Web.config* souboru transformace.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="4f8d1-290">**Alternativní řešení** Pokud potřebujete zkopírujte tyto soubory a ujistěte se, *Web.config* změny souborů ručně, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="4f8d1-291">Zkopírujte sestavení modulu databáze, které chcete *Bin* složky (a její podsložky) aplikace na cílovém počítači:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="4f8d1-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="4f8d1-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="4f8d1-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="4f8d1-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="4f8d1-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="4f8d1-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="4f8d1-295">V kořenové složce webové stránky, vytvořit nebo otevřít *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="4f8d1-296">(Ve službě WebMatrix Beta 3, je k dispozici, pokud kliknete na tento typ souboru **všechny** v **vyberte typ souboru** dialogové okno.)</span><span class="sxs-lookup"><span data-stu-id="4f8d1-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="4f8d1-297">Přidejte následující prvek jako podřízenou **&lt;konfigurace&gt;** – element (mimo **&lt;system.web&gt;** element):</span><span class="sxs-lookup"><span data-stu-id="4f8d1-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="4f8d1-298">Problém: Databáze a WebGrid Pomocníci nefungují na úrovni Medium Trust v jazyce Visual Basic</span><span class="sxs-lookup"><span data-stu-id="4f8d1-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="4f8d1-299">Pokud používáte Visual Basic (vytváření *.vbhtml* soubory), `Database` a `WebGrid` Pomocníci nebude fungovat, pokud je aplikace nastavena používat úrovni Medium Trust.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="4f8d1-300">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-300">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-301">Dočasně nastavte, že aplikace pro používání úplný vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="4f8d1-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="4f8d1-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="4f8d1-303">Problém: Vlastnost "Šifrovat" nebyl rozpoznán.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="4f8d1-304">SQL Server Compact 4.0 nerozpoznává `Encrypt` vlastnost `SqlCeConnection` třídy.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="4f8d1-305">Tato vlastnost byste neměli používat k šifrování souborů databáze.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="4f8d1-306">`Encrypt` Vlastnost byla zastaralé v systému SQL Server Compact 3.5 verzi a bylo zachováno pouze z důvodů zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="4f8d1-307">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-307">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-308">Použití `Encryption Mode` vlastnost `SqlCeConnection` třída k zašifrování souborů databáze systému SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="4f8d1-309">Následující příklad ukazuje, jak vytvořit k šifrované SQL Server Compact 4.0 databáze pomocí `Encryption Mode` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="4f8d1-310">Chcete-li změnit režim šifrování existující databázi SQL Server Compact 4.0, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="4f8d1-311">K šifrování nezašifrované databázi systému SQL Server Compact 4.0, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="4f8d1-312">Problém: Microsoft Visual C++ 2008 modulu runtime knihoven jsou požadovány</span><span class="sxs-lookup"><span data-stu-id="4f8d1-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="4f8d1-313">Nativní knihovny DLL systému SQL Server Compact 4.0 potřebovat Microsoft Visual C++ 2008 modulu Runtime knihoven (x 86, IA64 a x 64), aktualizace Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="4f8d1-314">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-314">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-315">Nainstalujte rozhraní .NET Framework 3.5 SP1.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="4f8d1-316">Tím se nainstaluje taky Visual C++ 2008 modulu Runtime knihoven SP1.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="4f8d1-317">Knihovny můžete stáhnout z následujícího umístění:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="4f8d1-318">Aktualizaci zabezpečení pro sadu Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL</span><span class="sxs-lookup"><span data-stu-id="4f8d1-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="4f8d1-319">Všimněte si, že instalace rozhraní .NET Framework 2.0, 3.0, nebo nemá 4 *není* nainstalovat Visual C++ 2008 modulu Runtime knihoven SP1.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="4f8d1-320">Problém: Pokud je systém SQL Server Compact nainstalovaný před instalací rozhraní .NET Framework na počítači, jeho neutrální název zprostředkovatele není registrován v souboru machine.config rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4f8d1-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="4f8d1-321">Na počítači, který nemá rozhraní .NET Framework nainstalovat, protože systém SQL Server Compact vyžadují rozhraní .NET framework lze nainstalovat systém SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="4f8d1-322">Pokud ani rozhraní .NET Framework verze 3.5 ani 4 je nainstalována před instalací systému SQL Server Compact, nastavení SQL Server Compact nezaregistruje neutrální název zprostředkovatele v jeho *machine.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="4f8d1-323">Všechny aplikace, které jsou závislé na položce SQL Server Compact v *machine.config* souboru se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="4f8d1-324">Neutrální název položka registrace v *machine.config* vypadá jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="4f8d1-325">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-325">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-326">Odinstalujte SQL Server Compact 4.0 CTP1.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="4f8d1-327">Stáhněte a nainstalujte plné verze rozhraní .NET Framework z následujícího umístění:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="4f8d1-328">Microsoft .NET Framework 3.5 Service pack 1 (úplná balíček)</span><span class="sxs-lookup"><span data-stu-id="4f8d1-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="4f8d1-329">Verze rozhraní Microsoft .NET Framework 4.0 (úplná balíček)</span><span class="sxs-lookup"><span data-stu-id="4f8d1-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="4f8d1-330">Potom přeinstalujte [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="4f8d1-331">Instalace aplikace</span><span class="sxs-lookup"><span data-stu-id="4f8d1-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="4f8d1-332">Problém: Instalace aplikace může trvat dlouhou dobu. Pokud je přesměrování složky Dokumenty uživatele do sdílené síťové složky</span><span class="sxs-lookup"><span data-stu-id="4f8d1-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="4f8d1-333">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-333">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-334">Žádné</span><span class="sxs-lookup"><span data-stu-id="4f8d1-334">None.</span></span> <span data-ttu-id="4f8d1-335">Aplikace může trvat, než k instalaci, ale nainstaluje správně.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="4f8d1-336">Publikování aplikací</span><span class="sxs-lookup"><span data-stu-id="4f8d1-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="4f8d1-337">Problém: Lokality nemusí fungovat po publikování Pokud pole "Cílová adresa URL" není s předponou http:// nebo https://.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="4f8d1-338">V **nastavení publikování** dialogové okno, pokud cílová adresa URL začíná `http://` nebo `https://`, web nemusí fungovat po nasazení.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="4f8d1-339">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-339">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-340">Ujistěte se, že před publikováním serveru, cílovou adresu URL v **nastavení publikování** dialogové okno začíná `http://` nebo `https://`.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="4f8d1-341">Problém: Publikování databáze MySQL selže s chybou "se nepodařilo publikovat databázi.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="4f8d1-342">To může dojít, pokud Vzdálená databáze nemůže spustit skript."</span><span class="sxs-lookup"><span data-stu-id="4f8d1-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="4f8d1-343">Chyba může mít několik důvodů.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="4f8d1-344">Jedním z důvodů, že se zobrazí tato chyba je pokud databázového skriptu obsahuje znak, jeden uvozovky (') a databáze MySQL cílové výchozí znakovou sadu není na UTF-8.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="4f8d1-345">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-345">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-346">Nastavte výchozí znakovou sadu pro vzdálené databáze MySQL na UTF-8.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="4f8d1-347">Další problémy</span><span class="sxs-lookup"><span data-stu-id="4f8d1-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="4f8d1-348">Problém: Hledání nebo Filtr nefunguje v sestavách Seskupit podle: typ problému</span><span class="sxs-lookup"><span data-stu-id="4f8d1-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="4f8d1-349">Při spuštění sestavy pro lokalitu, pokud zadáte text v *filtrovat podle adresy URL* pole a klikněte na tlačítko *vyhledávání*, nedojde k žádné akci.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="4f8d1-350">Důvodem je, že tento ovládací prvek není funkční i *Group By* je nastavena do stavu sestavy *typ problému*, který je výchozí.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="4f8d1-351">**Alternativní řešení** v *Group By* karta pásu karet klikněte na tlačítko *URL* k seskupení položek podle jejich adresu URL zdroje.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="4f8d1-352">Textové pole a tlačítko Filtrovat položky jsou funkční i v tomto stavu.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="4f8d1-353">Problém: Aplikací služby WCF se nepodaří spustit službou IIS Express</span><span class="sxs-lookup"><span data-stu-id="4f8d1-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="4f8d1-354">Procházení k aplikaci WCF výsledkem chyba stejný, jako je následující:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="4f8d1-355">*Nelze načíst soubor nebo sestavení ' Microsoft.Web.Administration, verze = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35, nebo jeden z jeho závislých. Systém nemůže najít zadaný soubor.*</span><span class="sxs-lookup"><span data-stu-id="4f8d1-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="4f8d1-356">K tomu dochází, protože služba IIS Express betaverze nepodporuje WCF ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="4f8d1-357">**Alternativní řešení** použijte některou z následujících náhradních postupů (alternativní řešení #2 vyžaduje systém Microsoft Windows Vista nebo novější):</span><span class="sxs-lookup"><span data-stu-id="4f8d1-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="4f8d1-358">Kopírování *Microsoft.Web.dll* a *Microsoft.Web.Administration.dll* sestavení z umístění instalace služby WebMatrix, které *bin* adresář WCF aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="4f8d1-359">Ve výchozím nastavení, služba WebMatrix je nainstalován v *Microsoft WebMatrix* podsložku systému *Program Files* složky.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="4f8d1-360">V systému Microsoft Windows Vista nebo novějším, vytvořit symlink k sestavení v *bin* adresáře pomocí následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="4f8d1-361">(Tento přístup má výhodu, nevytvoří kopii sestavení.)</span><span class="sxs-lookup"><span data-stu-id="4f8d1-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="4f8d1-362">Nainstalujte dvě sestavení v mezipaměti GAC.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="4f8d1-363">Řádku se zvýšenými oprávněními spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="4f8d1-364">Problém: WebMatrix Beta 3 se nepodařilo provést určité úlohy, které vyžadují zvýšení oprávnění</span><span class="sxs-lookup"><span data-stu-id="4f8d1-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="4f8d1-365">Služba WebMatrix Beta 3 se nepodařilo provést určité úlohy, které vyžadují zvýšení oprávnění, například při instalaci dalších součástí v následujících situacích:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="4f8d1-366">Windows Vista nebo Windows 7 jste přihlášeni pod účtem, který nemá oprávnění pro správu a řízení uživatelských účtů (UAC) je zakázáno.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="4f8d1-367">Používáte Microsoft Windows XP nebo Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="4f8d1-368">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-368">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-369">Většinu úloh ve službě WebMatrix Beta 3 nevyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="4f8d1-370">Pro ty, které provádějí můžete provést operaci jako správce nebo postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="4f8d1-371">V systému Windows Vista nebo Windows 7 povolte nástroj Řízení uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="4f8d1-372">V systému Windows XP přidejte uživatele do skupiny zabezpečení Administrators.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="4f8d1-373">Problém: "Lokality z Galerie webových" je zakázána.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="4f8d1-374">**Lokality z Galerie webových** možnost je zakázaná, pokud není nainstalovaná verze 3.0 webové platformy.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="4f8d1-375">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-375">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-376">Nainstalujte [webové platformy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="4f8d1-377">Problém: V systému Windows Server 2003, IIS Express se nespustí pro uživatele bez oprávnění správce</span><span class="sxs-lookup"><span data-stu-id="4f8d1-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="4f8d1-378">V systému Windows Server 2003 při spuštění stránky nebo spuštění služby IIS Express, služba IIS Express se nespustí.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="4f8d1-379">Pro webové stránky zobrazí se chybová zpráva označující, zda byla aplikace spuštěna uživatelem bez oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="4f8d1-380">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-380">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-381">Spusťte službu WebMatrix Beta 3 jako administrativní uživatel.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="4f8d1-382">Další podrobnosti najdete v následujícím článku Knowledge Base:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="4f8d1-383">Aplikace, která se spustí bez oprávnění správce uživatel nemůže naslouchat na HTTP provoz počítače, na kterém je aplikace spuštěna v systému Windows Vista, Windows Server 2003 nebo Windows XP.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="4f8d1-384">Problém: Google Chrome není k dispozici jako možnost spuštění</span><span class="sxs-lookup"><span data-stu-id="4f8d1-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="4f8d1-385">Google Chrome se nezobrazuje v seznamu prohlížečů v části **spustit** na **Domů** kartě.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="4f8d1-386">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-386">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-387">Některé verze Google Chrome nezaregistroval sami správně funkce výchozí programy systému Windows.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="4f8d1-388">Jako alternativní řešení, spustit Google Chrome, klikněte na *přizpůsobit a řízení Google Chrome* nabídky, klikněte na tlačítko *možnosti*a potom klikněte na *zkontrolujte Google Chrome Moje výchozí prohlížeč*.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="4f8d1-389">Problém: Dialogové okno "Cizí klíč" neumožňuje zadání primární klíč</span><span class="sxs-lookup"><span data-stu-id="4f8d1-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="4f8d1-390">**Cizí klíč** dialogové okno neumožňuje primární klíč tabulky zadejte název primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="4f8d1-391">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-391">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-392">To je úmyslné.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-392">This is intentional.</span></span> <span data-ttu-id="4f8d1-393">Není nutné k zadání názvu primární klíč tabulky primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="4f8d1-394">Problém: Tlačítko "Vztahů" zakázána.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="4f8d1-395">**Vztahy** tlačítko pod **tabulky** ve **databáze** pracovního prostoru je zakázán pro systém SQL Server Compact databáze.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="4f8d1-396">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-396">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-397">Žádné</span><span class="sxs-lookup"><span data-stu-id="4f8d1-397">None.</span></span> <span data-ttu-id="4f8d1-398">Systém SQL Server Compact nepodporuje relace mezi tabulkami.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="4f8d1-399">Problém: SQL parametrizované dotazy generování výjimek</span><span class="sxs-lookup"><span data-stu-id="4f8d1-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="4f8d1-400">V systému SQL Server Compact 4.0, pokud nezadáte, jako datový typ `SqlDbType` nebo `DbType` pro parametry v parametrizované dotazy, je vyvolána výjimka při spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="4f8d1-401">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="4f8d1-401">**Workaround**</span></span>  
> <span data-ttu-id="4f8d1-402">Explicitně nastavit na datový typ pro parametry, jako `SqlDbType` nebo `DbType`.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="4f8d1-403">Je to nezbytné v případě datové typy objektů BLOB (`image` a `ntext`).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="4f8d1-404">Použijte kód takto:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="4f8d1-405">Další informace</span><span class="sxs-lookup"><span data-stu-id="4f8d1-405">For More Information</span></span>

<span data-ttu-id="4f8d1-406">Další informace o službě WebMatrix Beta 3 najdete na následujících webech:</span><span class="sxs-lookup"><span data-stu-id="4f8d1-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="4f8d1-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="4f8d1-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="4f8d1-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4f8d1-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="4f8d1-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="4f8d1-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="4f8d1-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="4f8d1-411">Všechna práva vyhrazena.</span><span class="sxs-lookup"><span data-stu-id="4f8d1-411">All Rights Reserved.</span></span> <span data-ttu-id="4f8d1-412">[Podmínky použití](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f8d1-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
