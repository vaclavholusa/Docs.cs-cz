---
uid: visual-studio/overview/2013/release-notes
title: Technologie ASP.NET a webové nástroje pro poznámky k verzi 2013 Visual Studio | Microsoft Docs
author: microsoft
description: Tento dokument popisuje verzi technologie ASP.NET a webové nástroje pro sadu Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: e9ddd96f186564834ff6bb2c30cf0ed5444cbf1b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877798"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a><span data-ttu-id="95b32-103">Technologie ASP.NET a webové nástroje pro poznámky k verzi 2013 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95b32-103">ASP.NET and Web Tools for Visual Studio 2013 Release Notes</span></span>
====================
<span data-ttu-id="95b32-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="95b32-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="95b32-105">Tento dokument popisuje verzi technologie ASP.NET a webové nástroje pro sadu Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="95b32-105">This document describes the release of ASP.NET and Web Tools for Visual Studio 2013.</span></span>


## <a name="contents"></a><span data-ttu-id="95b32-106">Obsah</span><span class="sxs-lookup"><span data-stu-id="95b32-106">Contents</span></span>

- [<span data-ttu-id="95b32-107">Poznámky k instalaci</span><span class="sxs-lookup"><span data-stu-id="95b32-107">Installation Notes</span></span>](#TOC1)
- [<span data-ttu-id="95b32-108">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="95b32-108">Documentation</span></span>](#TOC2)
- [<span data-ttu-id="95b32-109">Požadavky na software</span><span class="sxs-lookup"><span data-stu-id="95b32-109">Software Requirements</span></span>](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a><span data-ttu-id="95b32-110">Nové funkce v technologii ASP.NET a nástroje Web Tools pro sadu Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="95b32-110">New Features in ASP.NET and Web Tools for Visual Studio 2013</span></span>

- [<span data-ttu-id="95b32-111">One ASP.NET</span><span class="sxs-lookup"><span data-stu-id="95b32-111">One ASP.NET</span></span>](#TOC6)
- [<span data-ttu-id="95b32-112">Nové prostředí webového projektu</span><span class="sxs-lookup"><span data-stu-id="95b32-112">New Web Project Experience</span></span>](#newproj)
- [<span data-ttu-id="95b32-113">ASP.NET generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="95b32-113">ASP.NET Scaffolding</span></span>](#scaffold)
- [<span data-ttu-id="95b32-114">Browser Link</span><span class="sxs-lookup"><span data-stu-id="95b32-114">Browser Link</span></span>](#browser-link)
- [<span data-ttu-id="95b32-115">Vylepšení editoru webové Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95b32-115">Visual Studio Web Editor Enhancements</span></span>](#web-editor)
- [<span data-ttu-id="95b32-116">Podpora aplikací webové služby Azure App Service v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95b32-116">Azure App Service Web Apps Support in Visual Studio</span></span>](#waws)
- [<span data-ttu-id="95b32-117">Vylepšení publikování webu</span><span class="sxs-lookup"><span data-stu-id="95b32-117">Web Publish Enhancements</span></span>](#publish)
- [<span data-ttu-id="95b32-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="95b32-118">NuGet 2.7</span></span>](#nuget)
- [<span data-ttu-id="95b32-119">ASP.NET – webové formuláře</span><span class="sxs-lookup"><span data-stu-id="95b32-119">ASP.NET Web Forms</span></span>](#TOC9)
- [<span data-ttu-id="95b32-120">ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="95b32-120">ASP.NET MVC 5</span></span>](#TOC10)
- [<span data-ttu-id="95b32-121">Rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="95b32-121">ASP.NET Web API 2</span></span>](#TOC11)
- [<span data-ttu-id="95b32-122">Funkce SignalR technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="95b32-122">ASP.NET SignalR</span></span>](#TOC13)
- [<span data-ttu-id="95b32-123">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="95b32-123">ASP.NET Identity</span></span>](#TOC8)
- [<span data-ttu-id="95b32-124">Komponenty Microsoft OWIN</span><span class="sxs-lookup"><span data-stu-id="95b32-124">Microsoft OWIN Components</span></span>](#TOC7)
- [<span data-ttu-id="95b32-125">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="95b32-125">Entity Framework 6</span></span>](#ef6)
- [<span data-ttu-id="95b32-126">Syntaxe Razor rozhraní ASP.NET 3</span><span class="sxs-lookup"><span data-stu-id="95b32-126">ASP.NET Razor 3</span></span>](#TOC14)
- [<span data-ttu-id="95b32-127">Pozastavení aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="95b32-127">ASP.NET App Suspend</span></span>](#TOC15)
- [<span data-ttu-id="95b32-128">Známé problémy a nejnovější změny</span><span class="sxs-lookup"><span data-stu-id="95b32-128">Known Issues and Breaking Changes</span></span>](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a><span data-ttu-id="95b32-129">Poznámky k instalaci</span><span class="sxs-lookup"><span data-stu-id="95b32-129">Installation Notes</span></span>

<span data-ttu-id="95b32-130">Technologie ASP.NET a webové nástroje pro sadu Visual Studio 2013 jsou seskupeny v hlavní instalační program a můžete ho stáhnout [zde](https://www.asp.net/downloads).</span><span class="sxs-lookup"><span data-stu-id="95b32-130">ASP.NET and Web Tools for Visual Studio 2013 are bundled in the main installer and can be downloaded [here](https://www.asp.net/downloads).</span></span>

<a id="TOC2"></a>
## <a name="documentation"></a><span data-ttu-id="95b32-131">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="95b32-131">Documentation</span></span>

<span data-ttu-id="95b32-132">Kurzy a další informace o ASP.NET a webové nástroje pro sadu Visual Studio 2013 jsou dostupné na [webové stránky ASP.NET](https://www.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="95b32-132">Tutorials and other information about ASP.NET and Web Tools for Visual Studio 2013 are available from the [ASP.NET web site](https://www.asp.net/).</span></span>

<a id="TOC4"></a>
## <a name="software-requirements"></a><span data-ttu-id="95b32-133">Požadavky na software</span><span class="sxs-lookup"><span data-stu-id="95b32-133">Software Requirements</span></span>

<span data-ttu-id="95b32-134">ASP.NET a nástroje pro Web vyžaduje Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="95b32-134">ASP.NET and Web Tools requires Visual Studio 2013.</span></span>

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a><span data-ttu-id="95b32-135">Nové funkce v technologii ASP.NET a nástroje Web Tools pro sadu Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="95b32-135">New Features in ASP.NET and Web Tools for Visual Studio 2013</span></span>

<span data-ttu-id="95b32-136">Následující části popisují funkce, které byly zavedeny v verzi.</span><span class="sxs-lookup"><span data-stu-id="95b32-136">The following sections describe the features that have been introduced in the release.</span></span>

<a id="TOC6"></a>
## <a name="one-aspnet"></a><span data-ttu-id="95b32-137">Jeden ASP.NET</span><span class="sxs-lookup"><span data-stu-id="95b32-137">One ASP.NET</span></span>

<span data-ttu-id="95b32-138">S verzí sady Visual Studio 2013 jsme provedli krokem k sjednocujícího prostředí pomocí technologie ASP.NET, takže snadno můžete kombinovat a párovat ty, které chcete.</span><span class="sxs-lookup"><span data-stu-id="95b32-138">With the release of Visual Studio 2013, we have taken a step towards unifying the experience of using ASP.NET technologies, so that you can easily mix and match the ones you want.</span></span> <span data-ttu-id="95b32-139">Například můžete můžete spustit na projekt MVC pomocí a snadno do projektu přidejte stránky webových formulářů později nebo vygenerovat webovým rozhraním API v projektu webové formuláře.</span><span class="sxs-lookup"><span data-stu-id="95b32-139">For example, you can start a project using MVC and easily add Web Forms pages to the project later, or scaffold Web APIs in a Web Forms project.</span></span> <span data-ttu-id="95b32-140">Jeden ASP.NET se točí kolem usnadnit vám jako vývojář provádět akce, která nepatří technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="95b32-140">One ASP.NET is all about making it easier for you as a developer to do the things you love in ASP.NET.</span></span> <span data-ttu-id="95b32-141">Bez ohledu na to, jakou technologii zvolíte může mít jistotu, který vytváříte na důvěryhodné základní architektury jeden ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="95b32-141">No matter what technology you choose, you can have confidence that you are building on the trusted underlying framework of One ASP.NET.</span></span>

<a id="newproj"></a>
## <a name="new-web-project-experience"></a><span data-ttu-id="95b32-142">Nové prostředí webového projektu</span><span class="sxs-lookup"><span data-stu-id="95b32-142">New Web Project Experience</span></span>

<span data-ttu-id="95b32-143">Budeme mít rozšířené možnosti vytvoření nové webové projekty v sadě Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="95b32-143">We have enhanced the experience of creating new web projects in Visual Studio 2013.</span></span> <span data-ttu-id="95b32-144">V **nový webový projekt ASP.NET** dialogovém okně můžete vybrat typ projektu, nakonfigurovat libovolnou kombinaci technologie (webových formulářů, MVC, Web API), nakonfigurujte možnosti ověřování a přidejte projektu testování částí.</span><span class="sxs-lookup"><span data-stu-id="95b32-144">In the **New ASP.NET Web Project** dialog you can select the project type you want, configure any combination of technologies (Web Forms, MVC, Web API), configure authentication options, and add a unit test project.</span></span>

![Nový projekt ASP.NET](release-notes/_static/image1.png)

<span data-ttu-id="95b32-146">Dialogové okno Nový umožňuje změnit výchozí možnosti ověřování pro řadu šablony.</span><span class="sxs-lookup"><span data-stu-id="95b32-146">The new dialog enables you to change the default authentication options for many of the templates.</span></span> <span data-ttu-id="95b32-147">Například při vytváření projektu webových formulářů ASP.NET můžete vybrat některý z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="95b32-147">For example, when you create an ASP.NET Web Forms project you can select any of the following options:</span></span>

- <span data-ttu-id="95b32-148">Bez ověřování</span><span class="sxs-lookup"><span data-stu-id="95b32-148">No Authentication</span></span>
- <span data-ttu-id="95b32-149">Jednotlivých uživatelských účtů (členství technologie ASP.NET nebo sociální zprostředkovatele přihlášení)</span><span class="sxs-lookup"><span data-stu-id="95b32-149">Individual User Accounts (ASP.NET membership or social provider log in)</span></span>
- <span data-ttu-id="95b32-150">Účty organizace (Active Directory v aplikaci internet)</span><span class="sxs-lookup"><span data-stu-id="95b32-150">Organizational Accounts (Active Directory in an internet application)</span></span>
- <span data-ttu-id="95b32-151">Ověřování systému Windows (Active Directory v intranetu aplikace)</span><span class="sxs-lookup"><span data-stu-id="95b32-151">Windows Authentication (Active Directory in an intranet application)</span></span>

![Možnosti ověřování](release-notes/_static/image2.png)

<span data-ttu-id="95b32-153">Další informace o nový proces pro vytváření webových projektů najdete v tématu [vytváření webové projekty ASP.NET v sadě Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-153">For more information about the new process for creating web projects, see [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span></span> <span data-ttu-id="95b32-154">Další informace o nové možnosti ověřování najdete v tématu [ASP.NET Identity](#TOC8) dále v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="95b32-154">For more information about the new authentication options, see [ASP.NET Identity](#TOC8) later in this document.</span></span>

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a><span data-ttu-id="95b32-155">ASP.NET generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="95b32-155">ASP.NET Scaffolding</span></span>

<span data-ttu-id="95b32-156">Generování uživatelského rozhraní ASP.NET je architektura generování kódu pro webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="95b32-156">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="95b32-157">Usnadňuje přidat často používaný kód do projektu, který komunikuje s datovým modelem.</span><span class="sxs-lookup"><span data-stu-id="95b32-157">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="95b32-158">V předchozích verzích sady Visual Studio generování uživatelského rozhraní omezovala na projekty ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="95b32-158">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="95b32-159">Visual Studio 2013 můžete nyní pomocí generování uživatelského rozhraní pro všechny projekt ASP.NET, včetně webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="95b32-159">With Visual Studio 2013, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="95b32-160">Visual Studio 2013 pro projekt webové formuláře aktuálně nepodporuje generování stránek, ale když můžete nadále používat generování uživatelského rozhraní s webovými formuláři tak, že přidáte do projektu závislosti MVC.</span><span class="sxs-lookup"><span data-stu-id="95b32-160">Visual Studio 2013 does not currently support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="95b32-161">Podpora pro generování stránky pro webové formuláře bude přidána v budoucí aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="95b32-161">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="95b32-162">Při použití generování uživatelského rozhraní, je zajištěno, že všechny požadované závislosti jsou nainstalované v projektu.</span><span class="sxs-lookup"><span data-stu-id="95b32-162">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="95b32-163">Například když začít s projektem webových formulářů ASP.NET a přidání Kontroleru webového rozhraní API pomocí generování uživatelského rozhraní, požadované balíčky NuGet a odkazy se přidají do projektu automaticky.</span><span class="sxs-lookup"><span data-stu-id="95b32-163">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="95b32-164">Chcete-li přidat generování uživatelského rozhraní MVC do projektu webových formulářů, přidejte **novou vygenerovanou položku** a vyberte **závislosti MVC 5** v dialogovém okně.</span><span class="sxs-lookup"><span data-stu-id="95b32-164">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="95b32-165">Existují dvě možnosti pro generování uživatelského rozhraní MVC; Minimální a úplné.</span><span class="sxs-lookup"><span data-stu-id="95b32-165">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="95b32-166">Pokud vyberete minimální, jenom balíčky NuGet a odkazy pro architekturu ASP.NET MVC se přidají do projektu.</span><span class="sxs-lookup"><span data-stu-id="95b32-166">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="95b32-167">Pokud vyberete možnost Úplná minimální závislosti přidají, a také požadované soubory obsahu pro projekt MVC.</span><span class="sxs-lookup"><span data-stu-id="95b32-167">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="95b32-168">Podpora pro generování uživatelského rozhraní asynchronní řadiče využívá nové funkce asynchronní z Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="95b32-168">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="95b32-169">Další informace a podrobné pokyny najdete v tématu [generování uživatelského rozhraní ASP.NET: Přehled](aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-169">For more information and tutorials, see [ASP.NET Scaffolding Overview](aspnet-scaffolding-overview.md).</span></span>

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a><span data-ttu-id="95b32-170">Browser Link – SignalR kanál mezi prohlížečem a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95b32-170">Browser Link – SignalR channel between browser and Visual Studio</span></span>

<span data-ttu-id="95b32-171">Nové [Browser Link](using-browser-link.md) funkce umožňuje připojení více prohlížečů pro Visual Studio a aktualizujte je všechny kliknutím na tlačítko na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="95b32-171">The new [Browser Link](using-browser-link.md) feature lets you connect multiple browsers to Visual Studio and refresh them all by clicking a button in the toolbar.</span></span> <span data-ttu-id="95b32-172">Můžete se připojit více prohlížečů na vašem vývojovém webu, včetně emulátorů a všechny prohlížeče klikněte na tlačítko Aktualizovat na aktualizovat, všechny ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="95b32-172">You can connect multiple browsers to your development site, including mobile emulators, and click refresh to refresh all the browsers all at the same time.</span></span> <span data-ttu-id="95b32-173">Browser Link taky zpřístupňuje rozhraní API a umožňuje vývojářům psát rozšíření odkazů prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="95b32-173">Browser Link also exposes an API to enable developers to write Browser Link extensions.</span></span>

![](release-notes/_static/image3.png)

<span data-ttu-id="95b32-174">Když zapnete vývojáři využít rozhraní API odkazů prohlížeče, bude možné vytvořit velmi pokročilé scénáře, které protne hranice mezi Visual Studio a žádný prohlížeč, který je připojen.</span><span class="sxs-lookup"><span data-stu-id="95b32-174">By enabling developers to take advantage of the Browser Link API, it becomes possible to create very advanced scenarios that crosses boundaries between Visual Studio and any browser that's connected.</span></span> <span data-ttu-id="95b32-175">Web Essentials využívá rozhraní API pro vytvoření integrované možnosti mezi Visual Studio a prohlížeče nástrojů pro vývojáře, vzdáleného řízení emulátorů a mnoho dalších.</span><span class="sxs-lookup"><span data-stu-id="95b32-175">Web Essentials takes advantage of the API to create an integrated experience between Visual Studio and the browser's developer tools, remote controlling mobile emulators and a lot more.</span></span>

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a><span data-ttu-id="95b32-176">Vylepšení editoru webové Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95b32-176">Visual Studio Web Editor Enhancements</span></span>

<span data-ttu-id="95b32-177">Visual Studio 2013 zahrnuje nové editor HTML pro syntaxi Razor soubory a soubory ve formátu HTML v webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="95b32-177">Visual Studio 2013 includes a new HTML editor for Razor files and HTML files in web applications.</span></span> <span data-ttu-id="95b32-178">Nový editor HTML poskytuje jeden jednotná schéma založené na HTML5.</span><span class="sxs-lookup"><span data-stu-id="95b32-178">The new HTML editor provides a single unified schema based on HTML5.</span></span> <span data-ttu-id="95b32-179">Má závorek automatické doplňování, jQuery uživatelského rozhraní a AngularJS atributu IntelliSense, atribut seskupení IntelliSense, ID a název třídy Intellisense a dalších vylepšení, včetně lepší výkon, formátování a inteligentní značky.</span><span class="sxs-lookup"><span data-stu-id="95b32-179">It has automatic brace completion, jQuery UI and AngularJS attribute IntelliSense, attribute IntelliSense Grouping, ID and class name Intellisense, and other improvements including better performance, formatting and SmartTags.</span></span>

<span data-ttu-id="95b32-180">Následující snímek obrazovky ukazuje, jak pomocí Bootstrap atributu IntelliSense v editoru HTML.</span><span class="sxs-lookup"><span data-stu-id="95b32-180">The following screenshot demonstrates using Bootstrap attribute IntelliSense in the HTML editor.</span></span>

![IntelliSense v editoru HTML](release-notes/_static/image4.png)

<span data-ttu-id="95b32-182">Visual Studio 2013 také obsahuje oba CoffeeScript a méně editory součástí.</span><span class="sxs-lookup"><span data-stu-id="95b32-182">Visual Studio 2013 also comes with both CoffeeScript and LESS editors built in.</span></span> <span data-ttu-id="95b32-183">LESS editor pochází z šablon stylů CSS editor se skvělými funkcemi a má na všech menší dokumentech v konkrétní Intellisense pro proměnné a mixins @import řetězu.</span><span class="sxs-lookup"><span data-stu-id="95b32-183">The LESS editor comes with all the cool features from the CSS editor and has specific Intellisense for variables and mixins across all the LESS documents in the @import chain.</span></span>

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a><span data-ttu-id="95b32-184">Podpora aplikací webové služby Azure App Service v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95b32-184">Azure App Service Web Apps Support in Visual Studio</span></span>

<span data-ttu-id="95b32-185">Ve Visual Studiu 2013 se sadou Azure SDK pro .NET 2.2, můžete použít **Průzkumníka serveru** pracovat přímo s vzdálené webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="95b32-185">In Visual Studio 2013 with the Azure SDK for .NET 2.2, you can use **Server Explorer** to interact directly with your remote web apps.</span></span> <span data-ttu-id="95b32-186">Můžete se přihlásit k účtu Azure vytvářet nové webové aplikace, konfigurace aplikace a zobrazení protokolů v reálném čase a další.</span><span class="sxs-lookup"><span data-stu-id="95b32-186">You can sign in to your Azure account, create new web apps, configure apps, view real-time logs, and more.</span></span> <span data-ttu-id="95b32-187">Vydání Připravujeme krátce po SDK, 2.2, budete moct spustit v režimu ladění vzdáleně v Azure.</span><span class="sxs-lookup"><span data-stu-id="95b32-187">Coming soon after SDK 2.2 is released, you'll be able to run in debug mode remotely in Azure.</span></span> <span data-ttu-id="95b32-188">Většina nových funkcí pro Azure App Service Web Apps také fungovat v sadě Visual Studio 2012, když instalujete aktuální verzi sady Azure SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="95b32-188">Most of the new features for Azure App Service Web Apps also work in Visual Studio 2012 when you install the current release of the Azure SDK for .NET.</span></span>

<span data-ttu-id="95b32-189">Další informace naleznete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="95b32-189">For more information, see the following resources:</span></span>

- [<span data-ttu-id="95b32-190">Vytvoření webové aplikace ASP.NET ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="95b32-190">Create an ASP.NET web app in Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [<span data-ttu-id="95b32-191">Řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95b32-191">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a><span data-ttu-id="95b32-192">Vylepšení publikování webu</span><span class="sxs-lookup"><span data-stu-id="95b32-192">Web Publish Enhancements</span></span>

<span data-ttu-id="95b32-193">Visual Studio 2013 zahrnuje nových a vylepšených funkcí publikování webů.</span><span class="sxs-lookup"><span data-stu-id="95b32-193">Visual Studio 2013 includes new and enhanced Web Publish features.</span></span> <span data-ttu-id="95b32-194">Zde najdete několik z nich:</span><span class="sxs-lookup"><span data-stu-id="95b32-194">Here are a few of them:</span></span>

- <span data-ttu-id="95b32-195">Snadno [automatizovat šifrování souborů Web.config](https://go.microsoft.com/fwlink/?LinkId=325529).</span><span class="sxs-lookup"><span data-stu-id="95b32-195">Easily [automate Web.config file encryption](https://go.microsoft.com/fwlink/?LinkId=325529).</span></span> <span data-ttu-id="95b32-196">(Tento odkaz a následující dva bod dokumentaci na webu MSDN, která nemusí být dostupná, dokud nebude pozdní za den na 10/17.)</span><span class="sxs-lookup"><span data-stu-id="95b32-196">(This link and the following two point to documentation on MSDN that might not be available until late in the day on 10/17.)</span></span>
- <span data-ttu-id="95b32-197">Snadno [automatizovat trvá aplikace do režimu offline během nasazení](https://go.microsoft.com/fwlink/?LinkId=325530).</span><span class="sxs-lookup"><span data-stu-id="95b32-197">Easily [automate taking an application offline during deployment](https://go.microsoft.com/fwlink/?LinkId=325530).</span></span>
- <span data-ttu-id="95b32-198">Konfigurace nasazení webu pro [používat kontrolní součet souboru místo datum poslední změny](https://go.microsoft.com/fwlink/?LinkId=325531) týkajících se soubory, které by se měl zkopírovat na server.</span><span class="sxs-lookup"><span data-stu-id="95b32-198">Configure Web Deploy to [use file checksum instead of last-changed date](https://go.microsoft.com/fwlink/?LinkId=325531) for determining which files should be copied to the server.</span></span>
- <span data-ttu-id="95b32-199">Pokud používáte FTP nebo systém souborů publikovat metody i pomocí nástroje nasazení webu rychle publikujte jednotlivé vybrané soubory (včetně souboru Web.config).</span><span class="sxs-lookup"><span data-stu-id="95b32-199">Quickly publish individual selected files (including Web.config) when you're using the FTP or file system publish methods as well as with Web Deploy.</span></span>

<span data-ttu-id="95b32-200">Další informace o nasazení webu ASP.NET najdete v tématu [webu ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).</span><span class="sxs-lookup"><span data-stu-id="95b32-200">For more information about ASP.NET web deployment, see [the ASP.NET site](https://go.microsoft.com/fwlink/?LinkId=322027).</span></span>

<a id="nuget"></a>
## <a name="nuget-27"></a><span data-ttu-id="95b32-201">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="95b32-201">NuGet 2.7</span></span>

<span data-ttu-id="95b32-202">NuGet 2.7 zahrnuje celou řadu nových funkcí, které jsou popsány podrobně v [poznámky k verzi 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="95b32-202">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="95b32-203">Tato verze NuGet také eliminuje nutnost zajistit výslovný souhlas pro funkci obnovení balíčků NuGet stáhnout balíčky.</span><span class="sxs-lookup"><span data-stu-id="95b32-203">This version of NuGet also removes the need to provide explicit consent for NuGet's package restore feature to download packages.</span></span> <span data-ttu-id="95b32-204">Souhlasu (a související políčko v dialogovém okně Předvolby NuGet) je nyní uděleno nainstalováním NuGet.</span><span class="sxs-lookup"><span data-stu-id="95b32-204">Consent (and the associated checkbox in NuGet's preferences dialog) is now granted by installing NuGet.</span></span> <span data-ttu-id="95b32-205">Obnovení balíčku nyní jednoduše funguje ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="95b32-205">Now package restore simply works by default.</span></span>

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a><span data-ttu-id="95b32-206">ASP.NET – webové formuláře</span><span class="sxs-lookup"><span data-stu-id="95b32-206">ASP.NET Web Forms</span></span>

### <a name="one-aspnet"></a><span data-ttu-id="95b32-207">Jeden ASP.NET</span><span class="sxs-lookup"><span data-stu-id="95b32-207">One ASP.NET</span></span>

<span data-ttu-id="95b32-208">Šablony projektů webové formuláře bezproblémově integrovat nové prostředí jeden ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="95b32-208">The Web Forms project templates integrate seamlessly with the new One ASP.NET experience.</span></span> <span data-ttu-id="95b32-209">Můžete přidat MVC a webového rozhraní API podporují do projektu webových formulářů a ověřování pomocí Průvodce vytvořením projektu jeden ASP.NET můžete konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="95b32-209">You can add MVC and Web API support to your Web Forms project, and you can configure authentication using the One ASP.NET project creation wizard.</span></span> <span data-ttu-id="95b32-210">Další informace najdete v tématu [vytváření webové projekty ASP.NET v sadě Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-210">For more information, see [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="95b32-211">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="95b32-211">ASP.NET Identity</span></span>

<span data-ttu-id="95b32-212">Šablony projektů webové formuláře podporovat nové architektury ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="95b32-212">The Web Forms project templates support the new ASP.NET Identity framework.</span></span> <span data-ttu-id="95b32-213">Kromě toho šablony teď podporují vytvoření projektu intranetu webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="95b32-213">In addition, the templates now support creation of a Web Forms intranet project.</span></span> <span data-ttu-id="95b32-214">Další informace najdete v tématu [metody ověřování](creating-web-projects-in-visual-studio.md#auth) v **vytváření webové projekty ASP.NET v sadě Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="95b32-214">For more information, see [Authentication Methods](creating-web-projects-in-visual-studio.md#auth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="bootstrap"></a><span data-ttu-id="95b32-215">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="95b32-215">Bootstrap</span></span>

<span data-ttu-id="95b32-216">Pomocí šablony webových formulářů [Bootstrap](http://twitter.github.io/bootstrap/) k poskytování elegantní a dobře reagovaly vzhled a chování, můžete snadno přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="95b32-216">The Web Forms templates use [Bootstrap](http://twitter.github.io/bootstrap/) to provide a sleek and responsive look and feel that you can easily customize.</span></span> <span data-ttu-id="95b32-217">Další informace najdete v tématu [Bootstrap v šablony webových projektů Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).</span><span class="sxs-lookup"><span data-stu-id="95b32-217">For more information, see [Bootstrap in the Visual Studio 2013 web project templates](creating-web-projects-in-visual-studio.md#bootstrap).</span></span>

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a><span data-ttu-id="95b32-218">ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="95b32-218">ASP.NET MVC 5</span></span>

### <a name="one-aspnet"></a><span data-ttu-id="95b32-219">Jeden ASP.NET</span><span class="sxs-lookup"><span data-stu-id="95b32-219">One ASP.NET</span></span>

<span data-ttu-id="95b32-220">Šablon projektu MVC webové bezproblémově integrovat nové prostředí jeden ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="95b32-220">The Web MVC project templates integrate seamlessly with the new One ASP.NET experience.</span></span> <span data-ttu-id="95b32-221">Můžete přizpůsobit projektu MVC a nakonfigurovat ověřování pomocí Průvodce vytvořením projektu jeden ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="95b32-221">You can customize your MVC project and configure authentication using the One ASP.NET project creation wizard.</span></span> <span data-ttu-id="95b32-222">Úvodní kurz k ASP.NET MVC 5 naleznete na adrese [Začínáme s ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-222">An introductory tutorial to ASP.NET MVC 5 can be found at [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="95b32-223">Informace týkající se upgradu projekty MVC 4 až MVC 5 najdete v tématu [postup upgradu služby ASP.NET MVC 4 a projekt webového rozhraní API ASP.NET MVC 5 a webovém rozhraní API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-223">For information on upgrading MVC 4 projects to MVC 5, see [How to Upgrade an ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="95b32-224">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="95b32-224">ASP.NET Identity</span></span>

<span data-ttu-id="95b32-225">Šablon projektu MVC byly aktualizovány na používání technologie ASP.NET Identity pro ověřování a správu identit.</span><span class="sxs-lookup"><span data-stu-id="95b32-225">The MVC project templates have been updated to use ASP.NET Identity for authentication and identity management.</span></span> <span data-ttu-id="95b32-226">Kurz poskytuje funkci ověřování sítě Facebook a Google a členství v nové rozhraní API naleznete na adrese [vytvořit aplikaci ASP.NET MVC 5 s použitím Facebook a Google OAuth2 a přihlašování OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) a [vytvoření aplikace ASP.NET MVC pomocí ověřování a Databáze SQL a nasazení do Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="95b32-226">A tutorial featuring Facebook and Google authentication and the new membership API can be found at [Create an ASP.NET MVC 5 App with Facebook and Google OAuth2 and OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) and [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).</span></span>

### <a name="bootstrap"></a><span data-ttu-id="95b32-227">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="95b32-227">Bootstrap</span></span>

<span data-ttu-id="95b32-228">Šablona projektu MVC je aktualizovaná tak, aby použít [Bootstrap](http://getbootstrap.com/) k poskytování elegantní a dobře reagovaly vzhled a chování, můžete snadno přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="95b32-228">The MVC project template has been updated to use [Bootstrap](http://getbootstrap.com/) to provide a sleek and responsive look and feel that you can easily customize.</span></span> <span data-ttu-id="95b32-229">Další informace najdete v tématu [Bootstrap v šablony webových projektů Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).</span><span class="sxs-lookup"><span data-stu-id="95b32-229">For more information, see [Bootstrap in the Visual Studio 2013 web project templates](creating-web-projects-in-visual-studio.md#bootstrap).</span></span>

### <a name="authentication-filters"></a><span data-ttu-id="95b32-230">Filtry ověřování</span><span class="sxs-lookup"><span data-stu-id="95b32-230">Authentication filters</span></span>

<span data-ttu-id="95b32-231">Filtry ověřování je nový typ filtru v architektuře ASP.NET MVC, která spustí před filtry autorizace v architektuře ASP.NET MVC kanálu a umožňují určit ověřování logiku za akce, kontroleru, nebo globálně pro všechny řadiče.</span><span class="sxs-lookup"><span data-stu-id="95b32-231">Authentication filters are a new kind of filter in ASP.NET MVC that run prior to authorization filters in the ASP.NET MVC pipeline and allow you to specify authentication logic per-action, per-controller, or globally for all controllers.</span></span> <span data-ttu-id="95b32-232">Filtry ověřování zpracovat přihlašovací údaje v požadavku a zadejte odpovídající objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="95b32-232">Authentication filters process credentials in the request and provide a corresponding principal.</span></span> <span data-ttu-id="95b32-233">Filtry ověřování můžete také přidat výzev ověřování v reakci na neoprávněné žádosti.</span><span class="sxs-lookup"><span data-stu-id="95b32-233">Authentication filters can also add authentication challenges in response to unauthorized requests.</span></span>

### <a name="filter-overrides"></a><span data-ttu-id="95b32-234">Přepsání filtru</span><span class="sxs-lookup"><span data-stu-id="95b32-234">Filter overrides</span></span>

<span data-ttu-id="95b32-235">Nyní můžete přepsat filtry, které platí pro danou akci metodu nebo řadiče zadáním filtru přepsání.</span><span class="sxs-lookup"><span data-stu-id="95b32-235">You can now override which filters apply to a given action method or controller by specifying an override filter.</span></span> <span data-ttu-id="95b32-236">Zadejte filtry přepsání typy filtrů, které by neměl být spuštěn pro daný obor (akce nebo kontroleru).</span><span class="sxs-lookup"><span data-stu-id="95b32-236">Override filters specify a set of filter types that should not be run for a given scope (action or controller).</span></span> <span data-ttu-id="95b32-237">To umožňuje nastavit filtry, které platí globálně, ale pak vyloučit určité globální filtry z použití řadiče nebo konkrétní akce.</span><span class="sxs-lookup"><span data-stu-id="95b32-237">This allows you to configure filters that apply globally but then exclude certain global filters from applying to specific actions or controllers.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="95b32-238">Atribut směrování</span><span class="sxs-lookup"><span data-stu-id="95b32-238">Attribute routing</span></span>

<span data-ttu-id="95b32-239">ASP.NET MVC teď podporuje atribut směrování díky příspěvek ve Tim McCall, Autor [ http://attributerouting.net ](http://attributerouting.net).</span><span class="sxs-lookup"><span data-stu-id="95b32-239">ASP.NET MVC now supports attribute routing, thanks to a contribution by Tim McCall, the author of [http://attributerouting.net](http://attributerouting.net).</span></span> <span data-ttu-id="95b32-240">Se směrováním atributů můžete zadat trasy zadávání poznámek k akce a kontrolery.</span><span class="sxs-lookup"><span data-stu-id="95b32-240">With attribute routing you can specify your routes by annotating your actions and controllers.</span></span>

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a><span data-ttu-id="95b32-241">Rozhraní API pro ASP.NET Web 2</span><span class="sxs-lookup"><span data-stu-id="95b32-241">ASP.NET Web API 2</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="95b32-242">Atribut směrování</span><span class="sxs-lookup"><span data-stu-id="95b32-242">Attribute routing</span></span>

<span data-ttu-id="95b32-243">Rozhraní ASP.NET Web API nyní podporuje atribut směrování díky příspěvek ve Tim McCall, Autor [ http://attributerouting.net ](http://attributerouting.net).</span><span class="sxs-lookup"><span data-stu-id="95b32-243">ASP.NET Web API now supports attribute routing, thanks to a contribution by Tim McCall, the author of [http://attributerouting.net](http://attributerouting.net).</span></span> <span data-ttu-id="95b32-244">Se směrováním atributů můžete zadat trasy webového rozhraní API zadávání poznámek k vaše akce a kontrolery takto:</span><span class="sxs-lookup"><span data-stu-id="95b32-244">With attribute routing you can specify your Web API routes by annotating your actions and controllers like this:</span></span>

[!code-csharp[Main](release-notes/samples/sample1.cs)]

<span data-ttu-id="95b32-245">Atribut směrování vám dává větší kontrolu nad identifikátory URI ve vašem webovém rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="95b32-245">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="95b32-246">Můžete například snadno definovat hierarchie prostředků pomocí jediného kontroleru rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="95b32-246">For example, you can easily define a resource hierarchy using a single API controller:</span></span>

[!code-csharp[Main](release-notes/samples/sample2.cs)]

<span data-ttu-id="95b32-247">Atribut směrování také nabízí pohodlný syntaxi pro zadání volitelné parametry, výchozí hodnoty a omezení trasy:</span><span class="sxs-lookup"><span data-stu-id="95b32-247">Attribute routing also provides a convenient syntax for specifying optional parameters, default values, and route constraints:</span></span>

[!code-csharp[Main](release-notes/samples/sample3.cs)]

<span data-ttu-id="95b32-248">Další informace o směrováním atributů najdete v tématu [atribut směrování ve webovém rozhraní API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-248">For more information about attribute routing, see [Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).</span></span>

### <a name="oauth-20"></a><span data-ttu-id="95b32-249">OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="95b32-249">OAuth 2.0</span></span>

<span data-ttu-id="95b32-250">Šablony projektu webového rozhraní API a jednu stránku aplikaci teď podporují autorizace pomocí OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="95b32-250">The Web API and Single Page Application project templates now support authorization using OAuth 2.0.</span></span> <span data-ttu-id="95b32-251">OAuth 2.0 je rozhraní pro autorizaci přístupu klientů k chráněným prostředkům.</span><span class="sxs-lookup"><span data-stu-id="95b32-251">OAuth 2.0 is a framework for authorizing client access to protected resources.</span></span> <span data-ttu-id="95b32-252">Funguje pro celou řadu klientů včetně prohlížečů a mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="95b32-252">It works for a variety of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="95b32-253">Podpora pro OAuth 2.0 je založena na nové middleware zabezpečení poskytované komponenty Microsoft OWIN pro ověřování nosiče a implementace roli serveru autorizace.</span><span class="sxs-lookup"><span data-stu-id="95b32-253">Support for OAuth 2.0 is based on new security middleware provided by the Microsoft OWIN Components for bearer authentication and implementing the authorization server role.</span></span> <span data-ttu-id="95b32-254">Alternativně klientů může být ověřeny pomocí serveru organizační ověřování, jako je například Azure Active Directory nebo služba AD FS ve Windows serveru 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="95b32-254">Alternatively, clients can be authorized using an organizational authorization server, such as Azure Active Directory or ADFS in Windows Server 2012 R2.</span></span>

### <a name="odata-improvements"></a><span data-ttu-id="95b32-255">Vylepšení OData</span><span class="sxs-lookup"><span data-stu-id="95b32-255">OData Improvements</span></span>

<span data-ttu-id="95b32-256">**Podpora pro $select $rozšířit, $batch a $value**</span><span class="sxs-lookup"><span data-stu-id="95b32-256">**Support for $select, $expand, $batch, and $value**</span></span>

<span data-ttu-id="95b32-257">ASP.NET Web API OData teď má plnou podporu pro $select, $, rozbalte položku a $value.</span><span class="sxs-lookup"><span data-stu-id="95b32-257">ASP.NET Web API OData now has full support for $select, $expand, and $value.</span></span> <span data-ttu-id="95b32-258">Můžete taky $batch pro žádost o dávkové zpracování a zpracování sady změn.</span><span class="sxs-lookup"><span data-stu-id="95b32-258">You can also use $batch for request batching and processing of change sets.</span></span>

<span data-ttu-id="95b32-259">$Select a $expand umožňují změnit tvar data, která je vrácena z koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="95b32-259">The $select and $expand options let you change the shape of the data that is returned from an OData endpoint.</span></span> <span data-ttu-id="95b32-260">Další informace najdete v tématu [Introducing $select a $expand rozbalte podpory v Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-260">For more information, see [Introducing $select and $expand support in Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).</span></span>

<span data-ttu-id="95b32-261">**Vylepšené rozšíření**</span><span class="sxs-lookup"><span data-stu-id="95b32-261">**Improved extensibility**</span></span>

<span data-ttu-id="95b32-262">Rozšiřitelné jsou nyní formátovacích modulů OData.</span><span class="sxs-lookup"><span data-stu-id="95b32-262">The OData formatters are now extensible.</span></span> <span data-ttu-id="95b32-263">Můžete přidat položky metadat Atom, podporují záznamy s názvem datového proudu a média odkaz, přidejte poznámky instance a přizpůsobit způsob generování odkazů.</span><span class="sxs-lookup"><span data-stu-id="95b32-263">You can add Atom entry metadata, support named stream and media link entries, add instance annotations, and customize how links are generated.</span></span>

<span data-ttu-id="95b32-264">**Typ bez podpory**</span><span class="sxs-lookup"><span data-stu-id="95b32-264">**Type-less support**</span></span>

<span data-ttu-id="95b32-265">Teď můžete sestavit služby OData, aniž by bylo třeba definovat typy CLR pro vaše typy entit.</span><span class="sxs-lookup"><span data-stu-id="95b32-265">You can now build OData services without needing to define CLR types for your entity types.</span></span> <span data-ttu-id="95b32-266">Místo toho můžete řadičů OData trvat nebo vrátit instanci **IEdmObject**, které jsou formátovacích modulů OData serializace či deserializace.</span><span class="sxs-lookup"><span data-stu-id="95b32-266">Instead, your OData controllers can take or return instances of **IEdmObject**, which are the OData formatters serialize/deserialize.</span></span>

<span data-ttu-id="95b32-267">**Opakované použití existujícího modelu**</span><span class="sxs-lookup"><span data-stu-id="95b32-267">**Reuse an existing model**</span></span>

<span data-ttu-id="95b32-268">Pokud již máte existující model dat entity (EDM), můžete teď znovu použít ho přímo, aniž by museli vytvářet novou.</span><span class="sxs-lookup"><span data-stu-id="95b32-268">If you already have an existing entity data model (EDM), you can now reuse it directly, instead of having to build a new one.</span></span> <span data-ttu-id="95b32-269">Například pokud používáte rozhraní Entity Framework, můžete EDM, který sestaví EF za vás.</span><span class="sxs-lookup"><span data-stu-id="95b32-269">For example, if you are using Entity Framework, you can use the EDM that EF builds for you.</span></span>

### <a name="request-batching"></a><span data-ttu-id="95b32-270">Žádosti o dávkování</span><span class="sxs-lookup"><span data-stu-id="95b32-270">Request Batching</span></span>

<span data-ttu-id="95b32-271">Žádost o dávkování kombinuje více operací do jedné žádosti HTTP POST, omezit přenos v síti a zadejte hladší, méně chatty uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="95b32-271">Request batching combines multiple operations into a single HTTP POST request, to reduce network traffic and provide a smoother, less chatty user interface.</span></span> <span data-ttu-id="95b32-272">Rozhraní ASP.NET Web API nyní podporuje několik strategií pro dávkové zpracování žádosti:</span><span class="sxs-lookup"><span data-stu-id="95b32-272">ASP.NET Web API now supports several strategies for request batching:</span></span>

- <span data-ttu-id="95b32-273">Použijte koncový bod $batch služby OData.</span><span class="sxs-lookup"><span data-stu-id="95b32-273">Use the $batch endpoint of an OData service.</span></span>
- <span data-ttu-id="95b32-274">Balíček více požadavků do jedné žádosti vícedílné zprávy standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="95b32-274">Package multiple requests into a single MIME multipart request.</span></span>
- <span data-ttu-id="95b32-275">Použijte vlastní dávkování formát.</span><span class="sxs-lookup"><span data-stu-id="95b32-275">Use a custom batching format.</span></span>

<span data-ttu-id="95b32-276">Povolit žádosti o dávkování, jednoduše přidejte trasa se obslužnou rutinu dávkování webového rozhraní API konfigurace:</span><span class="sxs-lookup"><span data-stu-id="95b32-276">To enable request batching, simply add a route with a batching handler to your Web API configuration:</span></span>

[!code-csharp[Main](release-notes/samples/sample4.cs)]

<span data-ttu-id="95b32-277">Můžete taky řídit, jestli požadavky nebo provést sekvenčně a v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="95b32-277">You can also control whether requests or executed sequentially or in any order.</span></span>

### <a name="portable-aspnet-web-api-client"></a><span data-ttu-id="95b32-278">Přenosné ASP.NET Web API klienta</span><span class="sxs-lookup"><span data-stu-id="95b32-278">Portable ASP.NET Web API Client</span></span>

<span data-ttu-id="95b32-279">Klienta ASP.NET Web API nyní můžete vytvořit knihovny tříd portable, které fungují v rámci aplikace Windows Store a Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="95b32-279">You can now use the ASP.NET Web API Client to create portable class libraries that work across your Windows Store and Windows Phone 8 applications.</span></span> <span data-ttu-id="95b32-280">Můžete také vytvořit přenosné formátovací moduly, které můžete sdílet mezi klientem a serverem.</span><span class="sxs-lookup"><span data-stu-id="95b32-280">You can also create portable formatters that can be shared across client and server.</span></span>

### <a name="improved-testability"></a><span data-ttu-id="95b32-281">Vylepšené možnosti testování</span><span class="sxs-lookup"><span data-stu-id="95b32-281">Improved Testability</span></span>

<span data-ttu-id="95b32-282">Webové rozhraní API 2 díky je mnohem snazší jednotky testovací kontrolery vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="95b32-282">Web API 2 makes it much easier to unit test your API controllers.</span></span> <span data-ttu-id="95b32-283">Právě doložit vaší kontroler API s zprávu žádosti a konfigurace a potom volejte metodu akce, které chcete otestovat.</span><span class="sxs-lookup"><span data-stu-id="95b32-283">Just instantiate your API controller with your request message and configuration, and then call the action method you wish to test.</span></span> <span data-ttu-id="95b32-284">Je také snadno model **UrlHelper** třídu pro metody akce, které provádějí generování odkazů.</span><span class="sxs-lookup"><span data-stu-id="95b32-284">It is also easy to mock the **UrlHelper** class, for action methods that perform link generation.</span></span>

### <a name="ihttpactionresult"></a><span data-ttu-id="95b32-285">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="95b32-285">IHttpActionResult</span></span>

<span data-ttu-id="95b32-286">Nyní můžete implementovat IHttpActionResult k zapouzdření výsledek metody akce vašeho webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="95b32-286">You can now implement IHttpActionResult to encapsulate the result of your Web API action methods.</span></span> <span data-ttu-id="95b32-287">IHttpActionResult vrátila z metody akce webového rozhraní API se spustí modulem runtime ASP.NET Web API pro vytvoření zprávy výsledné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="95b32-287">An IHttpActionResult returned from a Web API action method is executed by the ASP.NET Web API runtime to produce the resultant response message.</span></span> <span data-ttu-id="95b32-288">IHttpActionResult lze vrátit ze všech akcí webového rozhraní API pro zjednodušení jednotky testování vaší implementace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="95b32-288">An IHttpActionResult can be returned from any Web API action to simplify unit testing of your Web API implementation.</span></span> <span data-ttu-id="95b32-289">Pro usnadnění práce, počet IHttpActionResult implementace jsou uvedeny předinstalované včetně výsledky pro vrácení určitých stavových kódů ve formátu obsahu nebo vyjednal obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="95b32-289">For convenience a number of IHttpActionResult implementations are provided out of the box including results for returning specific status codes, formatted content or content-negotiated responses.</span></span>

### <a name="httprequestcontext"></a><span data-ttu-id="95b32-290">HttpRequestContext</span><span class="sxs-lookup"><span data-stu-id="95b32-290">HttpRequestContext</span></span>

<span data-ttu-id="95b32-291">Nové **HttpRequestContext** sleduje nějaký stav, který je vázaný na žádost, ale nejsou ihned k dispozici z požadavku.</span><span class="sxs-lookup"><span data-stu-id="95b32-291">The new **HttpRequestContext** tracks any state that is tied to the request but is not immediately available from the request.</span></span> <span data-ttu-id="95b32-292">Například můžete použít **HttpRequestContext** získat data trasy, která objekt přidružený k požadavku na certifikát klienta, **UrlHelper** a kořen virtuální cesty.</span><span class="sxs-lookup"><span data-stu-id="95b32-292">For example, you can use the **HttpRequestContext** to get route data, the principal associated with the request, the client certificate, the **UrlHelper** and the virtual path root.</span></span> <span data-ttu-id="95b32-293">Můžete snadno vytvořit **HttpRequestContext** účely testování jednotky.</span><span class="sxs-lookup"><span data-stu-id="95b32-293">You can easily create an **HttpRequestContext** for unit testing purposes.</span></span>

<span data-ttu-id="95b32-294">Protože objekt zabezpečení pro daný požadavek je plynoucích s požadavkem, aniž byste museli spoléhat na **Thread.CurrentPrincipal**, objekt je nyní k dispozici po celou dobu požadavku zůstane v kanálu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="95b32-294">Because the principal for the request is flowed with the request instead of relying on **Thread.CurrentPrincipal**, the principal is now available throughout the lifetime of the request while it is in the Web API pipeline.</span></span>

### <a name="cors"></a><span data-ttu-id="95b32-295">CORS</span><span class="sxs-lookup"><span data-stu-id="95b32-295">CORS</span></span>

<span data-ttu-id="95b32-296">Díky jiné skvělý příspěvek ze společnosti Brock Allen ASP.NET nyní plně podporuje křížové požadavku sdílení zdroji (CORS).</span><span class="sxs-lookup"><span data-stu-id="95b32-296">Thanks to another great contribution from Brock Allen, ASP.NET now fully supports Cross Origin Request Sharing (CORS).</span></span>

<span data-ttu-id="95b32-297">Zabezpečení prohlížeče brání provedení požadavky AJAX do jiné domény na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="95b32-297">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="95b32-298">[CORS](http://www.w3.org/TR/cors/) je standard W3C, který umožňuje serveru zmírnit zásady stejného původu.</span><span class="sxs-lookup"><span data-stu-id="95b32-298">[CORS](http://www.w3.org/TR/cors/) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="95b32-299">Pomocí CORS, server explicitně povolit některé žádostí napříč zdroji při odmítnutí ostatní.</span><span class="sxs-lookup"><span data-stu-id="95b32-299">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span>

<span data-ttu-id="95b32-300">Webové rozhraní API 2 teď podporuje CORS, včetně automatického zpracování předběžných požadavků.</span><span class="sxs-lookup"><span data-stu-id="95b32-300">Web API 2 now supports CORS, including automatic handling of preflight requests.</span></span> <span data-ttu-id="95b32-301">Další informace najdete v tématu [povolení žádostí napříč zdroji v rozhraní ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-301">For more information, see [Enabling Cross-Origin Requests in ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).</span></span>

### <a name="authentication-filters"></a><span data-ttu-id="95b32-302">Filtry ověřování</span><span class="sxs-lookup"><span data-stu-id="95b32-302">Authentication Filters</span></span>

<span data-ttu-id="95b32-303">Filtry ověřování je nový typ filtru v rozhraní ASP.NET Web API, která spustí před filtry autorizace v rozhraní ASP.NET Web API kanálu a umožňují určit ověřování logiku za akce, kontroleru, nebo globálně pro všechny řadiče.</span><span class="sxs-lookup"><span data-stu-id="95b32-303">Authentication filters are a new kind of filter in ASP.NET Web API that run prior to authorization filters in the ASP.NET Web API pipeline and allow you to specify authentication logic per-action, per-controller, or globally for all controllers.</span></span> <span data-ttu-id="95b32-304">Filtry ověřování zpracovat přihlašovací údaje v požadavku a zadejte odpovídající objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="95b32-304">Authentication filters process credentials in the request and provide a corresponding principal.</span></span> <span data-ttu-id="95b32-305">Filtry ověřování můžete také přidat výzev ověřování v reakci na neoprávněné žádosti.</span><span class="sxs-lookup"><span data-stu-id="95b32-305">Authentication filters can also add authentication challenges in response to unauthorized requests.</span></span>

### <a name="filter-overrides"></a><span data-ttu-id="95b32-306">Přepsání filtru</span><span class="sxs-lookup"><span data-stu-id="95b32-306">Filter Overrides</span></span>

<span data-ttu-id="95b32-307">Nyní můžete přepsat filtry, které platí pro danou akci metodu nebo řadiče, zadáním filtru přepsání.</span><span class="sxs-lookup"><span data-stu-id="95b32-307">You can now override which filters apply to a given action method or controller, by specifying an override filter.</span></span> <span data-ttu-id="95b32-308">Zadejte filtry přepsání typy filtrů, které by neměla běžet pro daný obor (akce nebo kontroleru).</span><span class="sxs-lookup"><span data-stu-id="95b32-308">Override filters specify a set of filter types that should not run for a given scope (action or controller).</span></span> <span data-ttu-id="95b32-309">To umožňuje přidat globální filtry, ale pak vyloučit některé z řadiče nebo konkrétní akce.</span><span class="sxs-lookup"><span data-stu-id="95b32-309">This allows you to add global filters, but then exclude some from specific actions or controllers.</span></span>

### <a name="owin-integration"></a><span data-ttu-id="95b32-310">Integrace OWIN</span><span class="sxs-lookup"><span data-stu-id="95b32-310">OWIN Integration</span></span>

<span data-ttu-id="95b32-311">Rozhraní ASP.NET Web API nyní plně podporuje OWIN a dá se provozovat na jakéhokoli hostitele podporující OWIN.</span><span class="sxs-lookup"><span data-stu-id="95b32-311">ASP.NET Web API now fully supports OWIN and can be run on any OWIN capable host.</span></span> <span data-ttu-id="95b32-312">Další součástí je **HostAuthenticationFilter** integrace se sadou ověřování OWIN, který poskytuje.</span><span class="sxs-lookup"><span data-stu-id="95b32-312">Also included is a **HostAuthenticationFilter** that provides integration with the OWIN authentication system.</span></span>

<span data-ttu-id="95b32-313">Díky integraci OWIN můžete vlastní hostování webového rozhraní API v vlastní procesu spolu s další middleware OWIN, jako je například SignalR.</span><span class="sxs-lookup"><span data-stu-id="95b32-313">With OWIN integration, you can self-host Web API in your own process alongside other OWIN middleware, such as SignalR.</span></span> <span data-ttu-id="95b32-314">Další informace najdete v tématu [OWIN použijte k Self-Host ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-314">For more information, see [Use OWIN to Self-Host ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a><span data-ttu-id="95b32-315">Funkce SignalR technologie ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="95b32-315">ASP.NET SignalR 2.0</span></span>

<span data-ttu-id="95b32-316">Následující části popisují funkce SignalR 2.0.</span><span class="sxs-lookup"><span data-stu-id="95b32-316">The following sections describe features of SignalR 2.0.</span></span>

- [<span data-ttu-id="95b32-317">Založený na OWIN</span><span class="sxs-lookup"><span data-stu-id="95b32-317">Built on OWIN</span></span>](#builtonowin)
- [<span data-ttu-id="95b32-318">MapHubs a MapConnection jsou nyní MapSignalR</span><span class="sxs-lookup"><span data-stu-id="95b32-318">MapHubs and MapConnection are now MapSignalR</span></span>](#MapSignalR)
- [<span data-ttu-id="95b32-319">Cross-Domain Support</span><span class="sxs-lookup"><span data-stu-id="95b32-319">Cross-Domain Support</span></span>](#crossdomain)
- [<span data-ttu-id="95b32-320">iOS a Android, podporují prostřednictvím MonoTouch a MonoDroid</span><span class="sxs-lookup"><span data-stu-id="95b32-320">iOS and Android support via MonoTouch and MonoDroid</span></span>](#mobile)
- [<span data-ttu-id="95b32-321">Klient přenosné .NET</span><span class="sxs-lookup"><span data-stu-id="95b32-321">Portable .NET Client</span></span>](#portable)
- [<span data-ttu-id="95b32-322">Nový balíček pro hostování na vlastním serveru</span><span class="sxs-lookup"><span data-stu-id="95b32-322">New Self-Host Package</span></span>](#selfhost)
- [<span data-ttu-id="95b32-323">Podpora serveru zpětně kompatibilní</span><span class="sxs-lookup"><span data-stu-id="95b32-323">Backward-compatible server support</span></span>](#backwardcompat)
- [<span data-ttu-id="95b32-324">Odebrat server podpory pro rozhraní .NET 4.0</span><span class="sxs-lookup"><span data-stu-id="95b32-324">Removed server support for .NET 4.0</span></span>](#remove40)
- [<span data-ttu-id="95b32-325">Odesílání zprávy na seznam klientů a skupin</span><span class="sxs-lookup"><span data-stu-id="95b32-325">Sending a message to a list of clients and groups</span></span>](#messagelist)
- [<span data-ttu-id="95b32-326">Odesílání zprávy pro konkrétního uživatele</span><span class="sxs-lookup"><span data-stu-id="95b32-326">Sending a message to a specific user</span></span>](#sendtouser)
- [<span data-ttu-id="95b32-327">Lepší podporu zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="95b32-327">Better error handling support</span></span>](#errorhandling)
- [<span data-ttu-id="95b32-328">Jednodušší jednotky testování rozbočovače</span><span class="sxs-lookup"><span data-stu-id="95b32-328">Easier unit testing of hubs</span></span>](#unittesting)
- [<span data-ttu-id="95b32-329">Zpracování chyb jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="95b32-329">JavaScript error handling</span></span>](#javascripterror)

<span data-ttu-id="95b32-330">Příklad upgradu existujícího projektu 1.x SignalR 2.0, naleznete v části [upgrade SignalR 1.x projektu](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-330">For an example of how to upgrade an existing 1.x project to SignalR 2.0, see [Upgrading a SignalR 1.x Project](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<a id="builtonowin"></a>
### <a name="built-on-owin"></a><span data-ttu-id="95b32-331">Založený na OWIN</span><span class="sxs-lookup"><span data-stu-id="95b32-331">Built on OWIN</span></span>

<span data-ttu-id="95b32-332">SignalR 2.0 je založený na úplně [OWIN (otevřít webové rozhraní pro rozhraní .NET)](http://owin.org/).</span><span class="sxs-lookup"><span data-stu-id="95b32-332">SignalR 2.0 is built completely on [OWIN (the Open Web Interface for .NET)](http://owin.org/).</span></span> <span data-ttu-id="95b32-333">Tato změna zajistí procesu instalace pro SignalR mnohem víc konzistenci mezi hostované webové a vlastním hostováním aplikací SignalR, ale také vyžaduje počet změn, rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="95b32-333">This change makes the setup process for SignalR much more consistent between web-hosted and self-hosted SignalR applications, but has also required a number of API changes.</span></span>

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a><span data-ttu-id="95b32-334">MapHubs a MapConnection jsou nyní MapSignalR</span><span class="sxs-lookup"><span data-stu-id="95b32-334">MapHubs and MapConnection are now MapSignalR</span></span>

<span data-ttu-id="95b32-335">Pro kompatibilitu s normami OWIN, byla přejmenovaná tyto metody k `MapSignalR`.</span><span class="sxs-lookup"><span data-stu-id="95b32-335">For compatibility with OWIN standards, these methods have been renamed to `MapSignalR`.</span></span> <span data-ttu-id="95b32-336">`MapSignalR` Voláno bez parametrů se namapuje všechny rozbočovače (jako `MapHubs` nemá ve verzi 1.x); k mapování jednotlivých **připojení PersistentConnection** objekty, zadejte jako parametr typu a přípona adresy URL pro připojení jako typ připojení první argument.</span><span class="sxs-lookup"><span data-stu-id="95b32-336">`MapSignalR` called without parameters will map all hubs (as `MapHubs` does in version 1.x); to map individual **PersistentConnection** objects, specify the connection type as the type parameter, and the URL extension for the connection as the first argument.</span></span>

<span data-ttu-id="95b32-337">`MapSignalR` Metoda je volána v třídy pro spuštění Owin.</span><span class="sxs-lookup"><span data-stu-id="95b32-337">The `MapSignalR` method is called in an Owin startup class.</span></span> <span data-ttu-id="95b32-338">Visual Studio 2013 obsahuje novou šablonu pro třídy pro spuštění Owin; pro použití této šablony, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="95b32-338">Visual Studio 2013 contains a new template for an Owin startup class; to use this template, do the following:</span></span>

1. <span data-ttu-id="95b32-339">Klikněte pravým tlačítkem na projekt</span><span class="sxs-lookup"><span data-stu-id="95b32-339">Right-click on the project</span></span>
2. <span data-ttu-id="95b32-340">Vyberte **přidat**, **novou položku...**</span><span class="sxs-lookup"><span data-stu-id="95b32-340">Select **Add**, **New Item...**</span></span>
3. <span data-ttu-id="95b32-341">Vyberte **třídy pro spuštění Owin**.</span><span class="sxs-lookup"><span data-stu-id="95b32-341">Select **Owin Startup class**.</span></span> <span data-ttu-id="95b32-342">Pojmenujte novou třídu **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="95b32-342">Name the new class **Startup.cs**.</span></span>

<span data-ttu-id="95b32-343">V **webovou aplikaci,** Owin při spuštění třída obsahující `MapSignalR` metoda se pak přidá do procesu spouštění na Owin pomocí položky v uzlu nastavení aplikace v souboru Web.Config, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="95b32-343">In a **Web application,** the Owin startup class containing the `MapSignalR` method is then added to Owin's startup process using an entry in the application settings node of the Web.Config file, as shown below.</span></span>

<span data-ttu-id="95b32-344">V **samoobslužně hostované aplikace**, třída při spuštění se předá jako parametr typu `WebApp.Start` metoda.</span><span class="sxs-lookup"><span data-stu-id="95b32-344">In a **Self-hosted application**, the Startup class is passed as the type parameter of the `WebApp.Start` method.</span></span>

<span data-ttu-id="95b32-345">**Mapování rozbočovače a připojení v systému SignalR 1.x (ze souboru globální aplikací ve webové aplikaci):**</span><span class="sxs-lookup"><span data-stu-id="95b32-345">**Mapping hubs and connections in SignalR 1.x (from the global application file in a web application):**</span></span> 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

<span data-ttu-id="95b32-346">**Mapování rozbočovače a připojení v SignalR 2.0 (ze souboru třída Owin při spuštění):**</span><span class="sxs-lookup"><span data-stu-id="95b32-346">**Mapping hubs and connections in SignalR 2.0 (from an Owin Startup class file):**</span></span> 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

<span data-ttu-id="95b32-347">V **samoobslužně hostované aplikace**, třída při spuštění se předá jako parametr typu pro `WebApp.Start` metoda, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="95b32-347">In a **Self-hosted application**, the Startup class is passed as the type parameter for the `WebApp.Start` method, as shown below.</span></span>

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a><span data-ttu-id="95b32-348">Cross-Domain Support</span><span class="sxs-lookup"><span data-stu-id="95b32-348">Cross-Domain Support</span></span>

<span data-ttu-id="95b32-349">V systému SignalR 1.x napříč požadavky domény kontrolovala jeden příznak EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="95b32-349">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="95b32-350">Tento příznak řídí požadavky JSONP a CORS.</span><span class="sxs-lookup"><span data-stu-id="95b32-350">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="95b32-351">Pro větší flexibilitu, podpora všechny CORS byla odebrána z komponenty serveru SignalR (JavaScript lients i nadále používat CORS normálně když se zjistí, zda prohlížeč podporuje), a nové middlewaru OWIN, který má je k dispozici pro tyto scénáře podporovat.</span><span class="sxs-lookup"><span data-stu-id="95b32-351">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript lients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="95b32-352">V systému SignalR 2.0, pokud JSONP je v klientovi požadováno (pro podporu požadavků napříč doménami v prohlížečích starší), ji budou muset být explicitně povoleno nastavením `EnableJSONP` na `HubConfiguration` do objektu `true`, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="95b32-352">In SignalR 2.0, If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="95b32-353">JSONP je standardně zakázána, protože je to méně bezpečné než CORS.</span><span class="sxs-lookup"><span data-stu-id="95b32-353">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="95b32-354">Chcete-li přidat nový middleware CORS do SignalR 2.0, přidejte `Microsoft.Owin.Cors` knihovna k projektu a volání `UseCors` před SignalR middleware, jak je znázorněno v následující části.</span><span class="sxs-lookup"><span data-stu-id="95b32-354">To add the new CORS middleware in SignalR 2.0, add the `Microsoft.Owin.Cors` library to your project, and call `UseCors` before your SignalR middleware, as shown in the section below.</span></span>

<span data-ttu-id="95b32-355">**Přidání do projektu Microsoft.Owin.Cors**: K instalaci této knihovny, spusťte následující příkaz v konzole Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="95b32-355">**Adding Microsoft.Owin.Cors to your project**: To install this library, run the following command in the Package Manager Console:</span></span>

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

<span data-ttu-id="95b32-356">Tento příkaz přidá 2.0.0 verze balíčku do projektu.</span><span class="sxs-lookup"><span data-stu-id="95b32-356">This command will add the 2.0.0 version of the package to your project.</span></span>

<span data-ttu-id="95b32-357">**Volání metody UseCors**</span><span class="sxs-lookup"><span data-stu-id="95b32-357">**Calling UseCors**</span></span>

<span data-ttu-id="95b32-358">Následující fragmenty kódu ukazují, jak implementovat připojení mezi doménami v systému SignalR 1.x a 2.0.</span><span class="sxs-lookup"><span data-stu-id="95b32-358">The following code snippets demonstrate how to implement cross-domain connections in SignalR 1.x and 2.0.</span></span>

<span data-ttu-id="95b32-359">**Implementace žádosti napříč doménami v systému SignalR 1.x (ze souboru globální aplikace)**</span><span class="sxs-lookup"><span data-stu-id="95b32-359">**Implementing cross-domain requests in SignalR 1.x (from the global application file)**</span></span>

[!code-csharp[Main](release-notes/samples/sample9.cs)]

<span data-ttu-id="95b32-360">**Implementace žádosti napříč doménami v rámci SignalR 2.0 (ze souboru kódu C#)**</span><span class="sxs-lookup"><span data-stu-id="95b32-360">**Implementing cross-domain requests in SignalR 2.0 (from a C# code file)**</span></span>

<span data-ttu-id="95b32-361">Následující kód ukazuje, jak povolit CORS a JSONP v projektu SignalR 2.0.</span><span class="sxs-lookup"><span data-stu-id="95b32-361">The following code demonstrates how to enable CORS or JSONP in a SignalR 2.0 project.</span></span> <span data-ttu-id="95b32-362">Tento příklad používá `Map` a `RunSignalR` místo `MapSignalR`, aby se spouštěla CORS middleware jenom pro SignalR požadavky, které vyžadují podporu CORS (nikoli pro všechny přenosy v zadané cestě v `MapSignalR`.) `Map` lze také použít pro veškerý middleware, který je potřeba spustit pro konkrétní předponu adresy URL, nikoli pro celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="95b32-362">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) `Map` can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a><span data-ttu-id="95b32-363">iOS a Android, podporují prostřednictvím MonoTouch a MonoDroid</span><span class="sxs-lookup"><span data-stu-id="95b32-363">iOS and Android support via MonoTouch and MonoDroid</span></span>

<span data-ttu-id="95b32-364">Byla přidána podpora pro iOS a Android klienty, kteří používají MonoTouch a MonoDroid součásti z [Xamarin knihovny](https://xamarin.com/).</span><span class="sxs-lookup"><span data-stu-id="95b32-364">Support has been added for iOS and Android clients using MonoTouch and MonoDroid components from the [Xamarin library](https://xamarin.com/).</span></span> <span data-ttu-id="95b32-365">Další informace o tom, jak je používat najdete v tématu [pomocí součásti Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln).</span><span class="sxs-lookup"><span data-stu-id="95b32-365">For more information on how to use them, see [Using Xamarin Components](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln).</span></span> <span data-ttu-id="95b32-366">Tyto součásti bude k dispozici v [Xamarin Storu](https://store.xamarin.com/) při vydání SignalR RTW je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="95b32-366">These components will be available in the [Xamarin Store](https://store.xamarin.com/) when the SignalR RTW release is available.</span></span>

<a id="portable"></a> <span data-ttu-id="95b32-367">### Přenosný klient .NET</span><span class="sxs-lookup"><span data-stu-id="95b32-367">### Portable .NET client</span></span>

<span data-ttu-id="95b32-368">Pro lepší usnadňují vývoj pro různé platformy, Silverlight, WinRT a klienti Windows Phone nahradil jeden přenosný klient .NET, který podporuje tyto platformy:</span><span class="sxs-lookup"><span data-stu-id="95b32-368">To better facilitate cross-platform development, the Silverlight, WinRT and Windows Phone clients have been replaced with a single portable .NET client that supports the following platforms:</span></span>

- <span data-ttu-id="95b32-369">NET 4.5</span><span class="sxs-lookup"><span data-stu-id="95b32-369">NET 4.5</span></span>
- <span data-ttu-id="95b32-370">Silverlight 5</span><span class="sxs-lookup"><span data-stu-id="95b32-370">Silverlight 5</span></span>
- <span data-ttu-id="95b32-371">WinRT (.NET pro aplikace Windows Store)</span><span class="sxs-lookup"><span data-stu-id="95b32-371">WinRT (.NET for Windows Store Apps)</span></span>
- <span data-ttu-id="95b32-372">Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="95b32-372">Windows Phone 8</span></span>

<a id="selfhost"></a>

### <a name="new-self-host-package"></a><span data-ttu-id="95b32-373">Nový balíček pro hostování na vlastním serveru</span><span class="sxs-lookup"><span data-stu-id="95b32-373">New Self-Host Package</span></span>

<span data-ttu-id="95b32-374">Nyní je k balíčku NuGet, která usnadňují začátek práce s SignalR hostování na vlastním serveru (SignalR aplikace, které jsou hostované v procesu nebo jiná aplikace, nikoli hostované na webovém serveru).</span><span class="sxs-lookup"><span data-stu-id="95b32-374">There is now a NuGet package to make it easier to get started with SignalR Self-Host (SignalR applications that are hosted in a process or other application, rather than being hosted in a web server).</span></span> <span data-ttu-id="95b32-375">Upgrade projektu hostování na vlastním vytvořené s SignalR 1.x, odebrat balíček Microsoft.AspNet.SignalR.Owin a přidejte balíček Microsoft.AspNet.SignalR.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="95b32-375">To upgrade a self-host project built with SignalR 1.x, remove the Microsoft.AspNet.SignalR.Owin package, and add the Microsoft.AspNet.SignalR.SelfHost package.</span></span> <span data-ttu-id="95b32-376">Další informace o Začínáme s balíčkem hostování na vlastním serveru najdete v tématu [kurz: SignalR hostování na vlastním](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-376">For more information on getting started with the self-host package, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a><span data-ttu-id="95b32-377">Podpora serveru zpětně kompatibilní</span><span class="sxs-lookup"><span data-stu-id="95b32-377">Backward-compatible server support</span></span>

<span data-ttu-id="95b32-378">V předchozích verzích nástroje SignalR, verze balíčku SignalR v klientovi a serveru musela být identické.</span><span class="sxs-lookup"><span data-stu-id="95b32-378">In previous versions of SignalR, the versions of the SignalR package used in the client and the server needed to be identical.</span></span> <span data-ttu-id="95b32-379">Za účelem podpory silná klientských aplikací, které by bylo obtížné aktualizovat, SignalR 2.0 teď podporuje pomocí staršího klienta na novější verzi serveru.</span><span class="sxs-lookup"><span data-stu-id="95b32-379">In order to support thick-client applications that would be difficult to update, SignalR 2.0 now supports using a newer server version with an older client.</span></span> <span data-ttu-id="95b32-380">**Poznámka: SignalR 2.0 nepodporuje serverů se staršími verzemi vytvořené s nástroji novějších klientů.**</span><span class="sxs-lookup"><span data-stu-id="95b32-380">**Note: SignalR 2.0 does not support servers built with older versions with newer clients.**</span></span>

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a><span data-ttu-id="95b32-381">Odebrat server podpory pro rozhraní .NET 4.0</span><span class="sxs-lookup"><span data-stu-id="95b32-381">Removed server support for .NET 4.0</span></span>

<span data-ttu-id="95b32-382">SignalR 2.0 klesla podpory interoperability serveru pomocí rozhraní .NET 4.0.</span><span class="sxs-lookup"><span data-stu-id="95b32-382">SignalR 2.0 has dropped support for server interoperability with .NET 4.0.</span></span> <span data-ttu-id="95b32-383">Rozhraní .NET 4.5, musí být použit s servery SignalR 2.0.</span><span class="sxs-lookup"><span data-stu-id="95b32-383">.NET 4.5 must be used with SignalR 2.0 servers.</span></span> <span data-ttu-id="95b32-384">Je stále klienta rozhraní .NET 4.0 pro SignalR 2.0.</span><span class="sxs-lookup"><span data-stu-id="95b32-384">There is still a .NET 4.0 client for SignalR 2.0.</span></span>

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a><span data-ttu-id="95b32-385">Odesílání zprávy na seznam klientů a skupin</span><span class="sxs-lookup"><span data-stu-id="95b32-385">Sending a message to a list of clients and groups</span></span>

<span data-ttu-id="95b32-386">V systému SignalR 2.0 je možné odeslat zprávu pomocí seznamu klienta a ID skupin.</span><span class="sxs-lookup"><span data-stu-id="95b32-386">In SignalR 2.0, it's possible to send a message using a list of client and group IDs.</span></span> <span data-ttu-id="95b32-387">Následující fragmenty kódu ukazují, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="95b32-387">The following code snippets demonstrate how to do this.</span></span>

<span data-ttu-id="95b32-388">**Odesílání zprávy na seznam klientů a skupin pomocí připojení PersistentConnection**</span><span class="sxs-lookup"><span data-stu-id="95b32-388">**Sending a message to a list of clients and groups using PersistentConnection**</span></span>

[!code-csharp[Main](release-notes/samples/sample11.cs)]

<span data-ttu-id="95b32-389">**Odesílání zprávy na seznam klientů a skupin pomocí centra**</span><span class="sxs-lookup"><span data-stu-id="95b32-389">**Sending a message to a list of clients and groups using Hubs**</span></span>

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a><span data-ttu-id="95b32-390">Odesílání zprávy pro konkrétního uživatele</span><span class="sxs-lookup"><span data-stu-id="95b32-390">Sending a message to a specific user</span></span>

<span data-ttu-id="95b32-391">Tato funkce umožňuje uživatelům určit, co je ID uživatele podle IRequest prostřednictvím nové rozhraní IUserIdProvider:</span><span class="sxs-lookup"><span data-stu-id="95b32-391">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider:</span></span>

<span data-ttu-id="95b32-392">**Rozhraní IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="95b32-392">**The IUserIdProvider interface**</span></span>

[!code-csharp[Main](release-notes/samples/sample13.cs)]

<span data-ttu-id="95b32-393">Ve výchozím nastavení bude implementace, která používá IPrincipal.Identity.Name uživatele jako uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="95b32-393">By default there will be an implementation that uses the user's IPrincipal.Identity.Name as the user name.</span></span>

<span data-ttu-id="95b32-394">V rozbočovače budete moct odesílat zprávy pro tyto uživatele pomocí nového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="95b32-394">In hubs, you'll be able to send messages to these users via a new API:</span></span>

<span data-ttu-id="95b32-395">**Pomocí Clients.User rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="95b32-395">**Using the Clients.User API**</span></span>

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a><span data-ttu-id="95b32-396">Lepší podporu zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="95b32-396">Better Error Handling Support</span></span>

<span data-ttu-id="95b32-397">Uživatelé nyní můžete vyvolat **HubException** ze všech volání rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="95b32-397">Users can now throw **HubException** from any hub invocation.</span></span> <span data-ttu-id="95b32-398">Konstruktoru **HubException** může trvat zprávu řetězec a objekt dodatečné údaje o chybě.</span><span class="sxs-lookup"><span data-stu-id="95b32-398">The constructor of the **HubException** can take a string message and an object extra error data.</span></span> <span data-ttu-id="95b32-399">SignalR bude automaticky serializovat výjimku a odešlete ji do klienta kde se bude používat k odmítnutí nebo selhání volání metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="95b32-399">SignalR will auto-serialize the exception and send it to the client where it will be used to reject/fail the hub method invocation.</span></span>

<span data-ttu-id="95b32-400">**Zobrazit podrobné rozbočovače výjimky** nastavení nemá žádný vliv na **HubException** odesílány zpět do klienta, nebo ne; vždy odesláním.</span><span class="sxs-lookup"><span data-stu-id="95b32-400">The **show detailed hub exceptions** setting has no bearing on **HubException** being sent back to the client or not; it is always sent.</span></span>

<span data-ttu-id="95b32-401">**Kódu na straně serveru demonstraci odesílání HubException do klienta**</span><span class="sxs-lookup"><span data-stu-id="95b32-401">**Server-side code demonstrating sending a HubException to the client**</span></span>

[!code-csharp[Main](release-notes/samples/sample15.cs)]

<span data-ttu-id="95b32-402">**Kód jazyka JavaScript klienta demonstraci neodpovídá na požadavky HubException odeslaných ze serveru**</span><span class="sxs-lookup"><span data-stu-id="95b32-402">**JavaScript client code demonstrating responding to a HubException sent from the server**</span></span>

[!code-html[Main](release-notes/samples/sample16.html)]

<span data-ttu-id="95b32-403">**Ukázka neodpovídá na požadavky HubException odeslaných ze serveru kód klienta rozhraní .NET**</span><span class="sxs-lookup"><span data-stu-id="95b32-403">**.NET client code demonstrating responding to a HubException sent from the server**</span></span>

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a><span data-ttu-id="95b32-404">Jednodušší jednotky testování rozbočovače</span><span class="sxs-lookup"><span data-stu-id="95b32-404">Easier unit testing of hubs</span></span>

<span data-ttu-id="95b32-405">SignalR 2.0 obsahuje rozhraní nazvané `IHubCallerConnectionContext` na rozbočovačů, které usnadňuje vytvoření imitované klienta straně volání.</span><span class="sxs-lookup"><span data-stu-id="95b32-405">SignalR 2.0 includes an interface called `IHubCallerConnectionContext` on Hubs that makes it easier to create mock client side invocations.</span></span> <span data-ttu-id="95b32-406">Následující fragmenty kódu ukazují toto rozhraní pomocí Oblíbené testovací nevodivou [xUnit.net](https://github.com/xunit/xunit) a [moq](https://code.google.com/p/moq/).</span><span class="sxs-lookup"><span data-stu-id="95b32-406">The following code snippets demonstrate using this interface with popular test harnesses [xUnit.net](https://github.com/xunit/xunit) and [moq](https://code.google.com/p/moq/).</span></span>

<span data-ttu-id="95b32-407">**Testování částí SignalR pomocí xUnit.net**</span><span class="sxs-lookup"><span data-stu-id="95b32-407">**Unit testing SignalR with xUnit.net**</span></span>

[!code-csharp[Main](release-notes/samples/sample18.cs)]

<span data-ttu-id="95b32-408">**Testování částí SignalR pomocí moq**</span><span class="sxs-lookup"><span data-stu-id="95b32-408">**Unit testing SignalR with moq**</span></span>

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a><span data-ttu-id="95b32-409">Zpracování chyb jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="95b32-409">JavaScript error handling</span></span>

<span data-ttu-id="95b32-410">Všechny zpětná volání jazyka JavaScript zpracování chyb v SignalR 2.0 vrátit JavaScript – chyby objekty namísto nezpracovaná řetězců.</span><span class="sxs-lookup"><span data-stu-id="95b32-410">In SignalR 2.0, all JavaScript error handling callbacks return JavaScript error objects instead of raw strings.</span></span> <span data-ttu-id="95b32-411">To umožňuje SignalR přenést bohatší informace do vašeho chyba obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="95b32-411">This allows SignalR to flow richer information to your error handlers.</span></span> <span data-ttu-id="95b32-412">Můžete získat v popisu vnitřní výjimky z `source` vlastnost chyby.</span><span class="sxs-lookup"><span data-stu-id="95b32-412">You can get the inner exception from the `source` property of the error.</span></span>

<span data-ttu-id="95b32-413">**Kód JavaScript klienta, který zpracovává Start.Fail výjimky**</span><span class="sxs-lookup"><span data-stu-id="95b32-413">**JavaScript client code that handles the Start.Fail exception**</span></span>

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a><span data-ttu-id="95b32-414">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="95b32-414">ASP.NET Identity</span></span>

### <a name="new-aspnet-membership-system"></a><span data-ttu-id="95b32-415">Nový systém členství technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="95b32-415">New ASP.NET Membership System</span></span>

<span data-ttu-id="95b32-416">ASP.NET Identity je nový systém členství pro aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="95b32-416">ASP.NET Identity is the new membership system for ASP.NET applications.</span></span> <span data-ttu-id="95b32-417">ASP.NET Identity lze snadno integrovat data profilu uživatelská data aplikací.</span><span class="sxs-lookup"><span data-stu-id="95b32-417">ASP.NET Identity makes it easy to integrate user-specific profile data with application data.</span></span> <span data-ttu-id="95b32-418">ASP.NET Identity můžete také vyberte model trvalosti pro profily uživatelů ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="95b32-418">ASP.NET Identity also allows you to choose the persistence model for user profiles in your application.</span></span> <span data-ttu-id="95b32-419">Data můžete uložit do databáze systému SQL Server nebo jiného úložiště dat, včetně úložiště dat NoSQL, jako je například úložiště tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="95b32-419">You can store the data in a SQL Server database or another data store, including NoSQL data stores such as Azure Storage Tables.</span></span> <span data-ttu-id="95b32-420">Další informace najdete v tématu [jednotlivé uživatelské účty](creating-web-projects-in-visual-studio.md#indauth) v **vytváření webové projekty ASP.NET v sadě Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="95b32-420">For more information, see [Individual User Accounts](creating-web-projects-in-visual-studio.md#indauth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="claims-based-authentication"></a><span data-ttu-id="95b32-421">Ověřování na základě deklarací</span><span class="sxs-lookup"><span data-stu-id="95b32-421">Claims-based authentication</span></span>

<span data-ttu-id="95b32-422">ASP.NET teď podporuje ověřování na základě deklarace identity, kde je identita uživatele reprezentován jako sadu deklarací identity z důvěryhodných vystavitelů.</span><span class="sxs-lookup"><span data-stu-id="95b32-422">ASP.NET now supports claims-based authentication, where the user's identity is represented as a set of claims from a trusted issuer.</span></span> <span data-ttu-id="95b32-423">Uživatelé mohou být ověřené pomocí uživatelského jména a hesla udržované v databázi aplikace, nebo pomocí poskytovatelů identit sociálních (například: Accounts Microsoft, Facebook, Google, Twitter), nebo pomocí účty organizace prostřednictvím Azure Active Directory nebo Služba Active Directory Federation Services (ADFS).</span><span class="sxs-lookup"><span data-stu-id="95b32-423">Users can be authenticated using a username and password maintained in an application database, or using social identity providers (for example: Microsoft Accounts, Facebook, Google, Twitter), or using organizational accounts through Azure Active Directory or Active Directory Federation Services (ADFS).</span></span>

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a><span data-ttu-id="95b32-424">Integrace s Azure Active Directory a Windows Server Active Directory</span><span class="sxs-lookup"><span data-stu-id="95b32-424">Integration with Azure Active Directory and Windows Server Active Directory</span></span>

<span data-ttu-id="95b32-425">Nyní můžete vytvořit projekty ASP.NET, které používají Azure Active Directory nebo Windows Server Active Directory (AD) pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="95b32-425">You can now create ASP.NET projects that use Azure Active Directory or Windows Server Active Directory (AD) for authentication.</span></span> <span data-ttu-id="95b32-426">Další informace najdete v tématu [účty organizace](creating-web-projects-in-visual-studio.md#orgauth) v **vytváření webové projekty ASP.NET v sadě Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="95b32-426">For more information, see [Organizational Accounts](creating-web-projects-in-visual-studio.md#orgauth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="owin-integration"></a><span data-ttu-id="95b32-427">Integrace OWIN</span><span class="sxs-lookup"><span data-stu-id="95b32-427">OWIN Integration</span></span>

<span data-ttu-id="95b32-428">Ověřování pomocí technologie ASP.NET je nyní podle middleware OWIN, který lze použít na všechny hostitele na základě OWIN.</span><span class="sxs-lookup"><span data-stu-id="95b32-428">ASP.NET authentication is now based on OWIN middleware that can be used on any OWIN-based host.</span></span> <span data-ttu-id="95b32-429">Další informace o rozhraní OWIN, viz následující [komponenty Microsoft OWIN](#TOC7) části.</span><span class="sxs-lookup"><span data-stu-id="95b32-429">For more information about OWIN, see the following [Microsoft OWIN Components](#TOC7) section.</span></span>

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a><span data-ttu-id="95b32-430">Komponenty Microsoft OWIN</span><span class="sxs-lookup"><span data-stu-id="95b32-430">Microsoft OWIN Components</span></span>

<span data-ttu-id="95b32-431">[Otevřete Web Interface pro .NET](http://owin.org/) (OWIN) definuje abstrakci mezi .NET webové servery a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="95b32-431">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="95b32-432">OWIN oddělí webové aplikace ze serveru, provedení hostitele vázané na webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="95b32-432">OWIN decouples the web application from the server, making web applications host-agnostic.</span></span> <span data-ttu-id="95b32-433">Například můžete hostitele na základě OWIN webovou aplikaci ve službě IIS nebo vlastní hostování v vlastní proces.</span><span class="sxs-lookup"><span data-stu-id="95b32-433">For example, you can host an OWIN-based web application in IIS or self-host it in a custom process.</span></span>

<span data-ttu-id="95b32-434">Změny uváděné ve komponenty Microsoft OWIN (také označované jako projekt Katana) zahrnují nové komponenty serveru a hostitele, nové pomocné knihovny a middleware a nové middleware ověřování.</span><span class="sxs-lookup"><span data-stu-id="95b32-434">Changes introduced in the Microsoft OWIN components (also known as the Katana project) include new server and host components, new helper libraries and middleware, and new authentication middleware.</span></span>

<span data-ttu-id="95b32-435">Další informace o OWIN a Katana najdete v tématu [co je nového v OWIN a Katana](../../../aspnet/overview/owin-and-katana/index.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-435">For more information about OWIN and Katana, see [What's new in OWIN and Katana](../../../aspnet/overview/owin-and-katana/index.md).</span></span>

<span data-ttu-id="95b32-436">**Poznámka: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) aplikace nemohou být spuštěny v klasickém režimu služby IIS; musí být spuštěn v integrovaném režimu.**</span><span class="sxs-lookup"><span data-stu-id="95b32-436">**Note: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applications cannot run in IIS classic mode; they must be run in integrated mode.**</span></span>

<span data-ttu-id="95b32-437">**Poznámka: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) aplikace musí být spuštěny v režimu plné důvěryhodnosti.**</span><span class="sxs-lookup"><span data-stu-id="95b32-437">**Note: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applications must be run in full trust.**</span></span>

### <a name="new-servers-and-hosts"></a><span data-ttu-id="95b32-438">Nové servery a hostitele</span><span class="sxs-lookup"><span data-stu-id="95b32-438">New Servers and Hosts</span></span>

<span data-ttu-id="95b32-439">V této verzi se nebyly přidány nové komponenty povolte scénáře hostování na vlastním serveru.</span><span class="sxs-lookup"><span data-stu-id="95b32-439">With this release, new components were added to enable self-host scenarios.</span></span> <span data-ttu-id="95b32-440">Tyto součásti zahrnují následující balíčky NuGet:</span><span class="sxs-lookup"><span data-stu-id="95b32-440">These components include the following NuGet packages:</span></span>

- <span data-ttu-id="95b32-441">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="95b32-441">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="95b32-442">Poskytuje serveru OWIN, který používá **HttpListener** naslouchat požadavkům HTTP a směrovat je do kanálu OWIN.</span><span class="sxs-lookup"><span data-stu-id="95b32-442">Provides an OWIN server that uses **HttpListener** to listen for HTTP requests and direct them into the OWIN pipeline.</span></span>
- <span data-ttu-id="95b32-443">**Microsoft.Owin.Hosting** poskytuje knihovnu pro vývojáře, kteří chtějí hostování na vlastním kanál OWIN v vlastní proces, jako je konzolová aplikace nebo služba systému Windows.</span><span class="sxs-lookup"><span data-stu-id="95b32-443">**Microsoft.Owin.Hosting** Provides a library for developers who wish to self-host an OWIN pipeline in a custom process, such as a console application or Windows service.</span></span>
- <span data-ttu-id="95b32-444">**OwinHost**.</span><span class="sxs-lookup"><span data-stu-id="95b32-444">**OwinHost**.</span></span> <span data-ttu-id="95b32-445">Poskytuje samostatné spustitelný soubor, který zabalí `Microsoft.Owin.Hosting` a umožňuje hostování na vlastním kanálu OWIN bez nutnosti psaní vlastních hostitelskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="95b32-445">Provides a stand-alone executable that wraps `Microsoft.Owin.Hosting` and lets you self-host an OWIN pipeline without having to write a custom host application.</span></span>

<span data-ttu-id="95b32-446">Kromě toho `Microsoft.Owin.Host.SystemWeb` balíček teď umožňuje middleware zajistit pomocné parametry **SystemWeb** serveru, která určuje, že by měla být volána middleware během konkrétní fáze kanálu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="95b32-446">In addition, the `Microsoft.Owin.Host.SystemWeb` package now enables middleware to provide hints to the **SystemWeb** server, indicating that the middleware should be called during a specific ASP.NET pipeline stage.</span></span> <span data-ttu-id="95b32-447">Tato funkce je obzvláště užitečná pro middleware ověřování, který se má spustit již v rané fázi v kanálu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="95b32-447">This feature is particularly useful for authentication middleware, which should run early in the ASP.NET pipeline.</span></span>

### <a name="helper-libraries-and-middleware"></a><span data-ttu-id="95b32-448">Pomocné knihovny a middlewaru.</span><span class="sxs-lookup"><span data-stu-id="95b32-448">Helper Libraries and Middleware</span></span>

<span data-ttu-id="95b32-449">I když můžete napsat OWIN komponent pomocí pouze funkce a typ definice z specifikace OWIN nové `Microsoft.Owin` balíček poskytuje sadu abstrakce přívětivější.</span><span class="sxs-lookup"><span data-stu-id="95b32-449">Although you can write OWIN components using only the function and type definitions from the OWIN specification, the new `Microsoft.Owin` package provides a more user-friendly set of abstractions.</span></span> <span data-ttu-id="95b32-450">Tento balíček kombinuje několik starších balíčky (například `Owin.Extensions`, `Owin.Types`) do jedné, dobře strukturovaný objekt modelu, který je pak snadno použít ostatními komponentami OWIN.</span><span class="sxs-lookup"><span data-stu-id="95b32-450">This package combines several earlier packages (e.g., `Owin.Extensions`, `Owin.Types`) into a single, well-structured object model that can then be easily used by other OWIN components.</span></span> <span data-ttu-id="95b32-451">Ve skutečnosti většina komponenty Microsoft OWIN teď tento balíček použít.</span><span class="sxs-lookup"><span data-stu-id="95b32-451">In fact, the majority of Microsoft OWIN components now use this package.</span></span>

> [!NOTE]
> <span data-ttu-id="95b32-452">[OWIN](http://www.owin.org) aplikace nemohou být spuštěny v klasickém režimu služby IIS; musí být spuštěn v integrovaném režimu.</span><span class="sxs-lookup"><span data-stu-id="95b32-452">[OWIN](http://www.owin.org) applications cannot run in IIS classic mode; they must be run in integrated mode.</span></span>

> [!NOTE]
> <span data-ttu-id="95b32-453">[OWIN](http://www.owin.org) aplikace musí být spuštěny v režimu plné důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="95b32-453">[OWIN](http://www.owin.org) applications must be run in full trust.</span></span>

<span data-ttu-id="95b32-454">Tato verze rovněž obsahuje balíček Microsoft.Owin.Diagnostics, která zahrnuje ověření spuštěné aplikace OWIN middleware plus chybovou stránku middleware pomohou prozkoumat selhání.</span><span class="sxs-lookup"><span data-stu-id="95b32-454">This release also includes the Microsoft.Owin.Diagnostics package, which includes middleware to validate a running OWIN application, plus error-page middleware to help investigate failures.</span></span>

### <a name="authentication-components"></a><span data-ttu-id="95b32-455">Součásti ověření</span><span class="sxs-lookup"><span data-stu-id="95b32-455">Authentication Components</span></span>

<span data-ttu-id="95b32-456">Následující součásti ověřování jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="95b32-456">The following authentication components are available.</span></span>

- <span data-ttu-id="95b32-457">**Microsoft.Owin.Security.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="95b32-457">**Microsoft.Owin.Security.ActiveDirectory**.</span></span> <span data-ttu-id="95b32-458">Povoluje ověřování v místní nebo cloudové adresářové služby.</span><span class="sxs-lookup"><span data-stu-id="95b32-458">Enables authentication using on-premise or cloud-based directory services.</span></span>
- <span data-ttu-id="95b32-459">**Microsoft.Owin.Security.Cookies** umožňuje ověřování pomocí souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="95b32-459">**Microsoft.Owin.Security.Cookies** Enables authentication using cookies.</span></span> <span data-ttu-id="95b32-460">Tento balíček byl dříve s názvem `Microsoft.Owin.Security.Forms`.</span><span class="sxs-lookup"><span data-stu-id="95b32-460">This package was previously named `Microsoft.Owin.Security.Forms`.</span></span>
- <span data-ttu-id="95b32-461">**Microsoft.Owin.Security.Facebook** umožňuje ověřování pomocí služby na základě OAuth pro Facebook.</span><span class="sxs-lookup"><span data-stu-id="95b32-461">**Microsoft.Owin.Security.Facebook** Enables authentication using Facebook's OAuth-based service.</span></span>
- <span data-ttu-id="95b32-462">**Microsoft.Owin.Security.Google** umožňuje ověřování pomocí služby Google OpenID založené.</span><span class="sxs-lookup"><span data-stu-id="95b32-462">**Microsoft.Owin.Security.Google** Enables authentication using Google's OpenID-based service.</span></span>
- <span data-ttu-id="95b32-463">**Microsoft.Owin.Security.Jwt** umožňuje ověřování pomocí tokenů JWT.</span><span class="sxs-lookup"><span data-stu-id="95b32-463">**Microsoft.Owin.Security.Jwt** Enables authentication using JWT tokens.</span></span>
- <span data-ttu-id="95b32-464">**Microsoft.Owin.Security.MicrosoftAccount** ověřování umožňuje použití účtů Microsoft.</span><span class="sxs-lookup"><span data-stu-id="95b32-464">**Microsoft.Owin.Security.MicrosoftAccount** Enables authentication using Microsoft accounts.</span></span>
- <span data-ttu-id="95b32-465">**Microsoft.Owin.Security.OAuth**.</span><span class="sxs-lookup"><span data-stu-id="95b32-465">**Microsoft.Owin.Security.OAuth**.</span></span> <span data-ttu-id="95b32-466">Poskytuje serveru ověřování OAuth, jakož i middleware pro ověřování tokenů nosiče.</span><span class="sxs-lookup"><span data-stu-id="95b32-466">Provides an OAuth authorization server as well as middleware for authenticating bearer tokens.</span></span>
- <span data-ttu-id="95b32-467">**Microsoft.Owin.Security.Twitter** umožňuje ověřování pomocí služby na základě OAuth pro Twitter.</span><span class="sxs-lookup"><span data-stu-id="95b32-467">**Microsoft.Owin.Security.Twitter** Enables authentication using Twitter's OAuth-based service.</span></span>

<span data-ttu-id="95b32-468">Tato verze rovněž obsahuje `Microsoft.Owin.Cors` balíček, který obsahuje middleware pro zpracování požadavků HTTP mezi zdroji.</span><span class="sxs-lookup"><span data-stu-id="95b32-468">This release also includes the `Microsoft.Owin.Cors` package, which contains middleware for processing cross-origin HTTP requests.</span></span>

> [!NOTE]
> <span data-ttu-id="95b32-469">Odebrala se podpora pro podepisování tokenů JWT ve finální verzi Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="95b32-469">Support for JWT signing has been removed in the final version of Visual Studio 2013.</span></span>

<a id="ef6"></a>
## <a name="entity-framework-6"></a><span data-ttu-id="95b32-470">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="95b32-470">Entity Framework 6</span></span>

<span data-ttu-id="95b32-471">Seznam nových funkcí a jiné změny ve Entity Framework 6 najdete v tématu [Entity Framework verze historie](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="95b32-471">For a list of new features and other changes in Entity Framework 6, see [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a><span data-ttu-id="95b32-472">Syntaxe Razor rozhraní ASP.NET 3</span><span class="sxs-lookup"><span data-stu-id="95b32-472">ASP.NET Razor 3</span></span>

<span data-ttu-id="95b32-473">ASP.NET Razor 3 zahrnuje následující nové funkce:</span><span class="sxs-lookup"><span data-stu-id="95b32-473">ASP.NET Razor 3 includes the following new features:</span></span>

- <span data-ttu-id="95b32-474">Podpora pro úpravy kartě.</span><span class="sxs-lookup"><span data-stu-id="95b32-474">Support for Tab editing.</span></span> <span data-ttu-id="95b32-475">Preivously **formátovat dokument** příkaz, automaticky odsazení a automatické formátování v sadě Visual Studio nebude fungovat správně při použití **zachovat karty** možnost.</span><span class="sxs-lookup"><span data-stu-id="95b32-475">Preivously, the **Format Document** command, auto indenting, and auto formatting in Visual Studio did not work correctly when using the **Keep Tabs** option.</span></span> <span data-ttu-id="95b32-476">Tato změna opraví formátování pro kódu Razor pro karta formátování sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="95b32-476">This change corrects Visual Studio formatting for Razor code for tab formatting.</span></span>
- <span data-ttu-id="95b32-477">Podporu pravidel přepisování adres URL při generování odkazů.</span><span class="sxs-lookup"><span data-stu-id="95b32-477">Support for URL Rewrite rules when generating links.</span></span>
- <span data-ttu-id="95b32-478">Odebrání transparentní atributu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="95b32-478">Removal of security transparent attribute.</span></span>
  > [!NOTE]
  > <span data-ttu-id="95b32-479">Toto je narušující změně a umožňuje Razor 3 nekompatibilní s MVC4 a starší, zatímco Razor 2 není kompatibilní s MVC5 nebo sestavení zkompilovaných proti MVC5.</span><span class="sxs-lookup"><span data-stu-id="95b32-479">This is a breaking change, and makes Razor 3 incompatible with MVC4 and earlier, while Razor 2 is incompatible with MVC5 or assemblies compiled against MVC5.</span></span>

<span data-ttu-id="95b32-480">Chyby syntaxe Razor 3 v sadě Visual Studio 2013 z předběžných verzí naleznete [zde](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).</span><span class="sxs-lookup"><span data-stu-id="95b32-480">Razor 3 issues fixed in Visual Studio 2013 from pre-release versions can be found [here](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).</span></span>

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a><span data-ttu-id="95b32-481">Pozastavení aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="95b32-481">ASP.NET App Suspend</span></span>

<span data-ttu-id="95b32-482">Pozastavit aplikace ASP.NET je funkce Změna herní v rozhraní .NET Framework 4.5.1, které výrazně mění uživatelské rozhraní a hospodářského model pro hostování velkého počtu ASP.NET lokalit na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="95b32-482">ASP.NET App Suspend is a game-changing feature in the .NET Framework 4.5.1 that radically changes the user experience and economic model for hosting large numbers of ASP.NET sites on a single machine.</span></span> <span data-ttu-id="95b32-483">Další informace najdete v tématu [pozastavit aplikace ASP.NET – přizpůsobivý sdílené hostování webů .NET](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).</span><span class="sxs-lookup"><span data-stu-id="95b32-483">For more information, see [ASP.NET App Suspend – responsive shared .NET web hosting](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).</span></span>

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="95b32-484">Známé problémy a nejnovější změny</span><span class="sxs-lookup"><span data-stu-id="95b32-484">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="95b32-485">Tato část popisuje známé problémy a nejnovějších změn v ASP.NET a webové nástroje pro sadu Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="95b32-485">This section describes known issues and breaking changes in the ASP.NET and Web Tools for Visual Studio 2013.</span></span>

### <a name="nuget"></a><span data-ttu-id="95b32-486">NuGet</span><span class="sxs-lookup"><span data-stu-id="95b32-486">NuGet</span></span>

- <span data-ttu-id="95b32-487">[Nové obnovení balíčků nefunguje na Mono při použití souboru SLN –](https://nuget.codeplex.com/workitem/3596) – problém bude vyřešený v nadcházející nuget.exe stahování a [NuGet.CommandLine balíček](http://www.nuget.org/packages/NuGet.CommandLine/) aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="95b32-487">[New package restore doesn't work on Mono when using SLN file](https://nuget.codeplex.com/workitem/3596) – will be fixed in an upcoming nuget.exe download and [NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/) update.</span></span>
- <span data-ttu-id="95b32-488">[Nové obnovení balíčků nefunguje s projekty Wix](https://nuget.codeplex.com/workitem/3598) – problém bude vyřešený v nadcházející nuget.exe stahování a [NuGet.CommandLine balíček](http://www.nuget.org/packages/NuGet.CommandLine/) aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="95b32-488">[New package restore doesn't work with Wix projects](https://nuget.codeplex.com/workitem/3598) – will be fixed in an upcoming nuget.exe download and [NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/) update.</span></span>
- <span data-ttu-id="95b32-489">[Automatické obnovení balíčků nefunguje pro projekty v řešení složce](https://nuget.codeplex.com/workitem/3625) – problém bude vyřešený v NuGet 2.8.</span><span class="sxs-lookup"><span data-stu-id="95b32-489">[Automatic Package restore doesn't work for projects under a solution folder](https://nuget.codeplex.com/workitem/3625) – will be fixed in NuGet 2.8.</span></span>

### <a name="aspnet-web-api"></a><span data-ttu-id="95b32-490">Rozhraní API pro ASP.NET Web</span><span class="sxs-lookup"><span data-stu-id="95b32-490">ASP.NET Web API</span></span>

1. <span data-ttu-id="95b32-491">`ODataQueryOptions<T>.ApplyTo(IQueryable)` nevrací `IQueryable<T>` vždy, protože jsme doplnili podporu pro `$select` a `$expand`.</span><span class="sxs-lookup"><span data-stu-id="95b32-491">`ODataQueryOptions<T>.ApplyTo(IQueryable)` doesn't return `IQueryable<T>` always, as we added support for `$select` and `$expand`.</span></span>

    <span data-ttu-id="95b32-492">Naše starší ukázky pro `ODataQueryOptions<T>` vždy převedena návratový z hodnoty `ApplyTo` k `IQueryable<T>`.</span><span class="sxs-lookup"><span data-stu-id="95b32-492">Our earlier samples for `ODataQueryOptions<T>` always casted the return value from `ApplyTo` to `IQueryable<T>`.</span></span> <span data-ttu-id="95b32-493">To fungovala předtím, protože dotaz možnostech, které jsme podporováno dříve (`$filter`, `$orderby`, `$skip`, `$top`) neměňte tvaru dotazu.</span><span class="sxs-lookup"><span data-stu-id="95b32-493">This worked earlier because the query options that we supported earlier (`$filter`, `$orderby`, `$skip`, `$top`) do not change the shape of the query.</span></span> <span data-ttu-id="95b32-494">Teď, když podporujeme `$select` a `$expand` návratovou hodnotou z `ApplyTo` nebude `IQueryable<T>` vždy.</span><span class="sxs-lookup"><span data-stu-id="95b32-494">Now that we support `$select` and `$expand` the return value from `ApplyTo` will not be `IQueryable<T>` always.</span></span>

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    <span data-ttu-id="95b32-495">Pokud používáte ukázkový kód z dříve, pokud klient neodesílá bude pokračovat pracovní `$select` a `$expand`.</span><span class="sxs-lookup"><span data-stu-id="95b32-495">If you are using the sample code from earlier, it will continue working if the client does not send `$select` and `$expand`.</span></span> <span data-ttu-id="95b32-496">Ale pokud chcete podporovat `$select` a `$expand` máte k tomuto tento kód změnit.</span><span class="sxs-lookup"><span data-stu-id="95b32-496">However, if you wish to support `$select` and `$expand` you have to change that code to this.</span></span>

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. <span data-ttu-id="95b32-497">**Request.Url nebo RequestContext.Url má hodnotu null při dávkového požadavku**</span><span class="sxs-lookup"><span data-stu-id="95b32-497">**Request.Url or RequestContext.Url is null during a batch request**</span></span>

    <span data-ttu-id="95b32-498">Ve scénáři dávkování **UrlHelper** má hodnotu null, pokud k němu přistupovat z **Request.Url** nebo **RequestContext.Url**.</span><span class="sxs-lookup"><span data-stu-id="95b32-498">In a batching scenario, **UrlHelper** is null when accessed from **Request.Url** or **RequestContext.Url**.</span></span>

    <span data-ttu-id="95b32-499">Tento problém aktuálně sledovat tady: [BatchRequestContext.Url má hodnotu null pro dávkové zpracování požadavku](http://aspnetwebstack.codeplex.com/workitem/1301).</span><span class="sxs-lookup"><span data-stu-id="95b32-499">This issue is currently tracked here: [BatchRequestContext.Url is null for batching request](http://aspnetwebstack.codeplex.com/workitem/1301).</span></span>

    <span data-ttu-id="95b32-500">Alternativní řešení tohoto problému je vytvoření nové instance **UrlHelper**, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="95b32-500">The workaround for this issue is to create a new instance of **UrlHelper**, as in the following example:</span></span>

    <span data-ttu-id="95b32-501">**Vytvoření nové instance UrlHelper**</span><span class="sxs-lookup"><span data-stu-id="95b32-501">**Creating a new instance of UrlHelper**</span></span>

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a><span data-ttu-id="95b32-502">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="95b32-502">ASP.NET MVC</span></span>

1. <span data-ttu-id="95b32-503">Při použití MVC5 a OrgAuth, pokud máte zobrazení, které provést ověření AntiForgerToken, můžete narazit na k následující chybě při odesílání dat na zobrazení:</span><span class="sxs-lookup"><span data-stu-id="95b32-503">When using MVC5 and OrgAuth, if you have views which do AntiForgerToken validation, you might come across the following error when you post data to the view:</span></span>

    <span data-ttu-id="95b32-504">**Chyba**:</span><span class="sxs-lookup"><span data-stu-id="95b32-504">**Error**:</span></span>

    <span data-ttu-id="95b32-505">*Chyba serveru v aplikaci '/'.*</span><span class="sxs-lookup"><span data-stu-id="95b32-505">*Server Error in '/' Application.*</span></span>

    <span data-ttu-id="95b32-506"><em>Deklarace typu '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'nebo'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' nebyl v zadané ClaimsIdentity. Povolení podpory token proti padělání s ověřováním na základě deklarace, zkontrolujte, zda zprostředkovatele nakonfigurované deklarací identity je zajištění obě tyto deklarace na ClaimsIdentity instance, které generuje. Pokud na jiný typ deklarací zprostředkovatele deklarací identity nakonfigurované místo toho používá jako jedinečný identifikátor, lze nakonfigurovat pomocí statické vlastnosti AntiForgeryConfig.UniqueClaimTypeIdentifier.</em></span><span class="sxs-lookup"><span data-stu-id="95b32-506"><em>A claim of type '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>' or '<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' was not present on the provided ClaimsIdentity. To enable anti-forgery token support with claims-based authentication, please verify that the configured claims provider is providing both of these claims on the ClaimsIdentity instances it generates. If the configured claims provider instead uses a different claim type as a unique identifier, it can be configured by setting the static property AntiForgeryConfig.UniqueClaimTypeIdentifier.</em></span></span>

    <span data-ttu-id="95b32-507">**Alternativní řešení**:</span><span class="sxs-lookup"><span data-stu-id="95b32-507">**Workaround**:</span></span>

    <span data-ttu-id="95b32-508">V souboru Global.asax jeho řešení přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="95b32-508">Add the following line in Global.asax to fix it:</span></span>

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    <span data-ttu-id="95b32-509">Tento problém bude vyřešený na další vydání.</span><span class="sxs-lookup"><span data-stu-id="95b32-509">This will be fixed for the next release.</span></span>
2. <span data-ttu-id="95b32-510">Po upgradu aplikace MVC4 na MVC5, sestavte řešení a spusťte jej.</span><span class="sxs-lookup"><span data-stu-id="95b32-510">After upgrading an MVC4 app to MVC5, build the solution and launch it.</span></span> <span data-ttu-id="95b32-511">Byste měli vidět následující chybě:</span><span class="sxs-lookup"><span data-stu-id="95b32-511">You should see the following error:</span></span>

    <span data-ttu-id="95b32-512">[A] Nelze přetypovat System.Web.WebPages.Razor.Configuration.HostSection [B]System.Web.WebPages.Razor.Configuration.HostSection.</span><span class="sxs-lookup"><span data-stu-id="95b32-512">[A]System.Web.WebPages.Razor.Configuration.HostSection cannot be cast to [B]System.Web.WebPages.Razor.Configuration.HostSection.</span></span> <span data-ttu-id="95b32-513">Typu A pochází z ' System.Web.WebPages.Razor, verze = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' v kontextu "Default" v umístění ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ V4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'.</span><span class="sxs-lookup"><span data-stu-id="95b32-513">Type A originates from 'System.Web.WebPages.Razor, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'.</span></span> <span data-ttu-id="95b32-514">Typ B pochází z ' System.Web.WebPages.Razor, verze = 3.0.0.0, jazykovou verzi = neutral, PublicKeyToken = 31bf3856ad364e35' v kontextu "Default" v umístění ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.</span><span class="sxs-lookup"><span data-stu-id="95b32-514">Type B originates from 'System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.</span></span>

    <span data-ttu-id="95b32-515">Chcete-li opravit výše uvedené chyby, otevřete *všechny* (včetně těch v zobrazení složky) souborů Web.config v projektu a postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="95b32-515">To fix the above error, open *all* the Web.config files (including the ones in the Views folder) in your project and do the following:</span></span>

   1. <span data-ttu-id="95b32-516">Aktualizujte všechny výskyty verze "4.0.0.0" "System.Web.Mvc" na "5.0.0.0".</span><span class="sxs-lookup"><span data-stu-id="95b32-516">Update all occurrences of version "4.0.0.0" of "System.Web.Mvc" to "5.0.0.0".</span></span>
   2. <span data-ttu-id="95b32-517">Aktualizovat všechny výskyty verzi "System.Web.Helpers", "2.0.0.0" &quot;System.Web.WebPages&quot; a &quot;System.Web.WebPages.Razor&quot; k "3.0.0.0"</span><span class="sxs-lookup"><span data-stu-id="95b32-517">Update all occurrences of version "2.0.0.0" of "System.Web.Helpers", &quot;System.Web.WebPages&quot; and &quot;System.Web.WebPages.Razor&quot; to "3.0.0.0"</span></span>

      <span data-ttu-id="95b32-518">Například po provedení výše změny vazby sestavení by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="95b32-518">For example, after you make the above changes, the assembly bindings should look like this:</span></span>

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      <span data-ttu-id="95b32-519">Informace týkající se upgradu projekty MVC 4 až MVC 5 najdete v tématu [postup upgradu služby ASP.NET MVC 4 a projekt webového rozhraní API ASP.NET MVC 5 a webovém rozhraní API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="95b32-519">For information on upgrading MVC 4 projects to MVC 5, see [How to Upgrade an ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span></span>
3. <span data-ttu-id="95b32-520">Při použití ověřování na straně klienta s jQuery Nerušivý ověření, zobrazí se zpráva ověření je někdy nesprávný pro element HTML input s typem = "číslo".</span><span class="sxs-lookup"><span data-stu-id="95b32-520">When using client-side validation with jQuery Unobtrusive Validation, the validation message is sometimes incorrect for an HTML input element with type='number'.</span></span> <span data-ttu-id="95b32-521">Chyba ověřování je pro požadovaná hodnota ("stáří pole je povinné") se zobrazí kdy je zadáno neplatné číslo místo správné zpráva, že je požadováno platné číslo.</span><span class="sxs-lookup"><span data-stu-id="95b32-521">The validation error for a required value ("The Age field is required") is shown when an invalid number is entered instead of the correct message that a valid number is required.</span></span>

    <span data-ttu-id="95b32-522">Tento problém je často najít s automaticky generovaný kód pro model se ve vlastnosti integer. v zobrazení vytvořit a upravit.</span><span class="sxs-lookup"><span data-stu-id="95b32-522">This issue is commonly found with scaffolded code for a model with an integer property on the Create and Edit views.</span></span>

    <span data-ttu-id="95b32-523">Chcete-li tento problém obejít, změňte pomocný editor z:</span><span class="sxs-lookup"><span data-stu-id="95b32-523">To work around this issue, change the editor helper from:</span></span>

    `@Html.EditorFor(person => person.Age)`

    <span data-ttu-id="95b32-524">Do:</span><span class="sxs-lookup"><span data-stu-id="95b32-524">To:</span></span>

    `@Html.TextBoxFor(person => person.Age)`
4. <span data-ttu-id="95b32-525">ASP.NET MVC 5 nepodporuje částečnou důvěryhodností.</span><span class="sxs-lookup"><span data-stu-id="95b32-525">ASP.NET MVC 5 no longer supports partial trust.</span></span> <span data-ttu-id="95b32-526">Odstraňte projekty propojení s MVC nebo WebAPI binární soubory [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) atribut a [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) atribut.</span><span class="sxs-lookup"><span data-stu-id="95b32-526">Projects linking to the MVC or WebAPI binaries should remove the [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) attribute and the [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) attribute.</span></span> <span data-ttu-id="95b32-527">Odebrat tyto atributy se eliminovat chyby kompilátoru podobně jako tento.</span><span class="sxs-lookup"><span data-stu-id="95b32-527">Removing these attributes will eliminate compiler errors such as the following.</span></span>

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > <span data-ttu-id="95b32-528">Všimněte si, jako jste to vy vedlejším účinkem nelze použít 4.0 a 5.0 sestavení ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="95b32-528">Note, as a side effect of this you cannot use 4.0 and 5.0 assemblies in the same application.</span></span> <span data-ttu-id="95b32-529">Je potřeba aktualizovat všechny z nich k 5.0.</span><span class="sxs-lookup"><span data-stu-id="95b32-529">You need to update all of them to 5.0.</span></span>

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a><span data-ttu-id="95b32-530">SPA šablonu s Facebook autorizace může způsobit nestabilitu v aplikaci Internet Explorer, zatímco je hostovaná na webu do zóny sítě intranet</span><span class="sxs-lookup"><span data-stu-id="95b32-530">SPA Template with Facebook authorization may cause instability in IE while the web site is hosted in intranet zone</span></span>

<span data-ttu-id="95b32-531">Šablona SPA poskytuje externí přihlášení pomocí Facebooku.</span><span class="sxs-lookup"><span data-stu-id="95b32-531">The SPA template provides external log in with Facebook.</span></span> <span data-ttu-id="95b32-532">Když je vytvořen pomocí šablony projektu běží místně, přihlášení může způsobit IE došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="95b32-532">When the project created with the template is running locally, signing in may cause IE to crash.</span></span>

<span data-ttu-id="95b32-533">Řešení:</span><span class="sxs-lookup"><span data-stu-id="95b32-533">Solution:</span></span>

1. <span data-ttu-id="95b32-534">Hostování webu v Internetové zóně; nebo</span><span class="sxs-lookup"><span data-stu-id="95b32-534">Host the web site in internet zone; or</span></span>

2. <span data-ttu-id="95b32-535">V prohlížeči než IE otestování tohoto scénáře.</span><span class="sxs-lookup"><span data-stu-id="95b32-535">Test the scenario in a browser other than IE.</span></span>

### <a name="web-forms-scaffolding"></a><span data-ttu-id="95b32-536">Webové formuláře generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="95b32-536">Web Forms Scaffolding</span></span>

<span data-ttu-id="95b32-537">Webové formuláře generování uživatelského rozhraní byla odebrána z VS2013 a bude k dispozici v budoucí aktualizaci na Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="95b32-537">Web Forms Scaffolding has been removed from VS2013 and will be available in a future update to Visual Studio.</span></span> <span data-ttu-id="95b32-538">Generování uživatelského rozhraní v rámci projektu webové formuláře však můžete použít přidáním závislosti MVC a generování generování uživatelského rozhraní pro MVC.</span><span class="sxs-lookup"><span data-stu-id="95b32-538">However, you can still use scaffolding within a Web Forms project by adding MVC dependencies and generating scaffolding for MVC.</span></span> <span data-ttu-id="95b32-539">Projekt bude obsahovat kombinaci webových formulářů a MVC.</span><span class="sxs-lookup"><span data-stu-id="95b32-539">Your project will contain a combination of Web Forms and MVC.</span></span>

<span data-ttu-id="95b32-540">Pokud chcete přidat do projektu webové formuláře MVC, přidat novou položku vygenerované a vyberte **závislosti MVC 5**.</span><span class="sxs-lookup"><span data-stu-id="95b32-540">To add MVC to your Web Forms project, add a new scaffolded item and select **MVC 5 Dependencies**.</span></span> <span data-ttu-id="95b32-541">Vyberte minimální nebo úplné v závislosti na tom, jestli potřebujete všechny soubory obsahu, jako třeba skripty.</span><span class="sxs-lookup"><span data-stu-id="95b32-541">Select either Minimal or Full depending on whether you need all of the content files, such as scripts.</span></span> <span data-ttu-id="95b32-542">Potom přidáte vygenerované položky pro MVC, která vytvoří zobrazení a kontroler ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="95b32-542">Then, add a scaffolded item for MVC, which will create views and a controller in your project.</span></span>

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="95b32-543">MVC a webových rozhraní API generování uživatelského rozhraní – chyby HTTP 404, nebyla nalezena chyba</span><span class="sxs-lookup"><span data-stu-id="95b32-543">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="95b32-544">Pokud dojde k chybě při přidání vygenerované položky do projektu, je možné, že projekt bude ponechána v nekonzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="95b32-544">If an error is encountered when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="95b32-545">Některé změny provedené být generování uživatelského rozhraní se vrátí zpátky, ale ostatní změny, jako je například nainstalované balíčky NuGet, nebude vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="95b32-545">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="95b32-546">Směrování změny konfigurace se vrátí zpátky, uživatelé obdrží chybu HTTP 404 při navigaci na generované uživatelské rozhraní položky.</span><span class="sxs-lookup"><span data-stu-id="95b32-546">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="95b32-547">Alternativní řešení:</span><span class="sxs-lookup"><span data-stu-id="95b32-547">Workaround:</span></span>

- <span data-ttu-id="95b32-548">Chcete-li vyřešit tuto chybu pro MVC, přidat novou položku vygenerované a vyberte závislosti MVC 5 (minimální nebo úplná).</span><span class="sxs-lookup"><span data-stu-id="95b32-548">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="95b32-549">Tento proces bude do projektu přidejte všechny požadované změny.</span><span class="sxs-lookup"><span data-stu-id="95b32-549">This process will add all of the required changes to your project.</span></span>
- <span data-ttu-id="95b32-550">Odstranění této chyby pro webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="95b32-550">To fix this error for Web API:</span></span>

  1. <span data-ttu-id="95b32-551">Do projektu přidejte třídy WebApiConfig.</span><span class="sxs-lookup"><span data-stu-id="95b32-551">Add the WebApiConfig class to your project.</span></span>

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. <span data-ttu-id="95b32-552">Konfigurace WebApiConfig.Register v aplikaci\_Start – metoda v souboru Global.asax následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="95b32-552">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
