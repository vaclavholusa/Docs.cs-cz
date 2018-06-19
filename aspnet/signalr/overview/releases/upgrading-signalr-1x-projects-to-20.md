---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: Upgrade projektů 1.x SignalR na verzi 2 | Microsoft Docs
author: pfletcher
description: Toto téma popisuje postup upgradu existující projekt 1.x SignalR na SignalR 2.x a řešení potíží, které mohou nastat během procesu upgradu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566026"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="f583a-103">Upgrade projektů 1.x SignalR na verze 2</span><span class="sxs-lookup"><span data-stu-id="f583a-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="f583a-104">podle [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f583a-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="f583a-105">Toto téma popisuje postup upgradu existující projekt 1.x SignalR na SignalR 2.x a řešení potíží, které mohou nastat během procesu upgradu.</span><span class="sxs-lookup"><span data-stu-id="f583a-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f583a-106">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="f583a-106">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f583a-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f583a-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="f583a-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f583a-108">.NET 4.5</span></span>
> - <span data-ttu-id="f583a-109">SignalR verze 1 a 2</span><span class="sxs-lookup"><span data-stu-id="f583a-109">SignalR versions 1 and 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="f583a-110">Tento kurz pomocí sady Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f583a-110">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="f583a-111">Pokud chcete používat Visual Studio 2012 v tomto kurzu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="f583a-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="f583a-112">Aktualizace vašeho [Správce balíčků](http://docs.nuget.org/docs/start-here/installing-nuget) na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="f583a-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="f583a-113">Nainstalujte [webové platformy](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="f583a-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="f583a-114">Ve webové platformy, vyhledání a instalace **ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="f583a-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="f583a-115">Šablony sady Visual Studio pro třídy SignalR dojde k instalaci, jako **rozbočovače**.</span><span class="sxs-lookup"><span data-stu-id="f583a-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="f583a-116">Některé šablony (jako například **třídy pro spuštění OWIN**) nebudou k dispozici; pro tyto třídy soubor použít místo.</span><span class="sxs-lookup"><span data-stu-id="f583a-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="f583a-117">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="f583a-117">Questions and comments</span></span>
> 
> <span data-ttu-id="f583a-118">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="f583a-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f583a-119">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="f583a-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="f583a-120">SignalR 2 nabízí konzistentní vývojového prostředí napříč platformami serveru pomocí [OWIN](http://owin.org).</span><span class="sxs-lookup"><span data-stu-id="f583a-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="f583a-121">Tento článek popisuje několik kroků, které jsou potřebné k aktualizaci 1.x SignalR na verze 2.</span><span class="sxs-lookup"><span data-stu-id="f583a-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="f583a-122">Když toto je vhodné k upgradu aplikace SignalR 2, SignalR 1.x, i nadále podporována.</span><span class="sxs-lookup"><span data-stu-id="f583a-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="f583a-123">Tento kurz popisuje, jak upgradovat hostované webové aplikace SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="f583a-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="f583a-124">Samoobslužné hostovaných aplikací (ty, které v konzolové aplikaci, služba systému Windows nebo jiný proces serveru hostitele) jsou nyní podporovány pod SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="f583a-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="f583a-125">Informace o tom, jak začít vytvářet vlastním hostováním aplikací s SignalR 2 najdete v tématu [kurz: SignalR hostování na vlastním](../deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="f583a-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="f583a-126">Obsah</span><span class="sxs-lookup"><span data-stu-id="f583a-126">Contents</span></span>

<span data-ttu-id="f583a-127">Následující části popisují úlohy, které jsou spojené s upgrade SignalR projekty a řešení potíží, které může způsobit.</span><span class="sxs-lookup"><span data-stu-id="f583a-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="f583a-128">Příklad: Upgrade kurzu Začínáme SignalR 2</span><span class="sxs-lookup"><span data-stu-id="f583a-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="f583a-129">Řešení potíží s chybami došlo během upgradu</span><span class="sxs-lookup"><span data-stu-id="f583a-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="f583a-130">Příklad: Upgrade Začínáme kurz aplikace pro SignalR 2</span><span class="sxs-lookup"><span data-stu-id="f583a-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="f583a-131">V této části budete aktualizovat vytvořené v aplikaci [SignalR 1.x verzi kurzu Začínáme](../older-versions/index.md) používat SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="f583a-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="f583a-132">Po dokončení tohoto kurzu Začínáme, klikněte pravým tlačítkem na projekt a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="f583a-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="f583a-133">Ověřte, zda **cílové rozhraní** je nastaven na **rozhraní .NET Framework 4.5.**</span><span class="sxs-lookup"><span data-stu-id="f583a-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="f583a-134">Otevřete konzolu Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="f583a-134">Open the Package Manager Console.</span></span> <span data-ttu-id="f583a-135">Odebrat SignalR 1.x z projektu pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="f583a-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="f583a-136">Nainstalujte SignalR 2 pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="f583a-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="f583a-137">Na stránce HTML aktualizujte odkaz na skript pro SignalR tak, aby odpovídala verzi skriptů teď zahrnutý v projektu.</span><span class="sxs-lookup"><span data-stu-id="f583a-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="f583a-138">Ve třídě globální aplikaci odeberte volání MapHubs.</span><span class="sxs-lookup"><span data-stu-id="f583a-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="f583a-139">Klikněte pravým tlačítkem na řešení a vyberte **přidat**, **novou položku...** . V dialogovém okně vyberte **třídy pro spuštění Owin**.</span><span class="sxs-lookup"><span data-stu-id="f583a-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="f583a-140">Pojmenujte novou třídu **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="f583a-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="f583a-141">Nahraďte obsah souboru Startup.cs následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f583a-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="f583a-142">Atribut sestavení přidá třídu do procesu spuštění na Owin, který provede `Configuration` metoda při spuštění Owin.</span><span class="sxs-lookup"><span data-stu-id="f583a-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="f583a-143">Naopak volá `MapSignalR` metodu, která vytvoří trasy pro všechny rozbočovače SignalR v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f583a-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="f583a-144">Spusťte projekt a zkopírujte adresu URL na hlavní stránku do jiné prohlížeče nebo podokně prohlížeče jako předtím.</span><span class="sxs-lookup"><span data-stu-id="f583a-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="f583a-145">Každé stránce výzvou k zadání uživatelského jména a zpráv odeslaných z každé stránce musí být viditelná v obou podokna prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f583a-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="f583a-146">Řešení potíží s chybami došlo během upgradu</span><span class="sxs-lookup"><span data-stu-id="f583a-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="f583a-147">Tato část popisuje problémy, které mohou nastat během upgradu.</span><span class="sxs-lookup"><span data-stu-id="f583a-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="f583a-148">Komplexnější seznam chyb a problémů, které mohou nastat s aplikací SignalR naleznete v části [řešení potíží s SignalR](../testing-and-debugging/troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f583a-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="f583a-149">'Volání je nejednoznačný mezi následující metody nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f583a-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="f583a-150">K této chybě dojde, pokud odkaz na `Microsoft.AspNet.SignalR.Owin` se neodebere.</span><span class="sxs-lookup"><span data-stu-id="f583a-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="f583a-151">Tento balíček je zastaralý; odkaz musí být odebrány a verze 1.x SelfHost balíčku musí být odinstalovány.</span><span class="sxs-lookup"><span data-stu-id="f583a-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="f583a-152">Bezobslužná selžou metody rozbočovače</span><span class="sxs-lookup"><span data-stu-id="f583a-152">Hub methods fail silently</span></span>

<span data-ttu-id="f583a-153">Ověřte, zda odkazům na skript v vašeho klienta jsou aktuální, a který `OwinStartup` atribut pro třídy pro spuštění má správné třídy a názvy sestavení pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="f583a-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="f583a-154">Zkuste také otevřením centra adresa (/ signalr/hubs) ve vašem prohlížeči; všechny chyby, který se zobrazí nabídne další informace o příčinu problému.</span><span class="sxs-lookup"><span data-stu-id="f583a-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>
