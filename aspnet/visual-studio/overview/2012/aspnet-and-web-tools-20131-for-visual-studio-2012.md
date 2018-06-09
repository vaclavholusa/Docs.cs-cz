---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Poznámky k verzi pro ASP.NET a nástroje pro Web 2013.1 pro sadu Visual Studio 2012 | Microsoft Docs
author: microsoft
description: Tento dokument popisuje verzi technologie ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036424"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="d2168-103">Poznámky k verzi pro ASP.NET a nástroje pro Web 2013.1 pro sadu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d2168-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="d2168-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d2168-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d2168-105">Tento dokument popisuje verzi technologie ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d2168-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="d2168-106">Obsah</span><span class="sxs-lookup"><span data-stu-id="d2168-106">Contents</span></span>

- [<span data-ttu-id="d2168-107">Poznámky k instalaci</span><span class="sxs-lookup"><span data-stu-id="d2168-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="d2168-108">Požadavky na software</span><span class="sxs-lookup"><span data-stu-id="d2168-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="d2168-109">Nové funkce v technologii ASP.NET a nástroje pro Web 2013.1 pro sadu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d2168-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="d2168-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="d2168-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="d2168-111">Šablony</span><span class="sxs-lookup"><span data-stu-id="d2168-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="d2168-112">Šablony ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="d2168-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="d2168-113">Šablony ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="d2168-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="d2168-114">Šablony položek</span><span class="sxs-lookup"><span data-stu-id="d2168-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="d2168-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="d2168-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="d2168-116">ASP.NET generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d2168-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="d2168-117">Razor Editor</span><span class="sxs-lookup"><span data-stu-id="d2168-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="d2168-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="d2168-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="d2168-119">Známé problémy a nejnovější změny</span><span class="sxs-lookup"><span data-stu-id="d2168-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="d2168-120">ASP.NET generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d2168-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="d2168-121">MVC a webových rozhraní API generování uživatelského rozhraní – chyby HTTP 404, nebyla nalezena chyba</span><span class="sxs-lookup"><span data-stu-id="d2168-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="d2168-122">Visual Studio Express 2012 pro Web přestane pracovat po přidání vygenerované položky</span><span class="sxs-lookup"><span data-stu-id="d2168-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="d2168-123">Syntaxe Razor rozhraní ASP.NET 3</span><span class="sxs-lookup"><span data-stu-id="d2168-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="d2168-124">Prohlížení souboru cshtml s procházet s nebo F5 způsobí chybu serveru</span><span class="sxs-lookup"><span data-stu-id="d2168-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="d2168-125">Přepisování adres URL a Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="d2168-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="d2168-126">Šablony</span><span class="sxs-lookup"><span data-stu-id="d2168-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="d2168-127">Poznámky k instalaci</span><span class="sxs-lookup"><span data-stu-id="d2168-127">Installation Notes</span></span>

<span data-ttu-id="d2168-128">[Nainstalujte](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d2168-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="d2168-129">Požadavky na software</span><span class="sxs-lookup"><span data-stu-id="d2168-129">Software Requirements</span></span>

<span data-ttu-id="d2168-130">Musíte mít Visual Studio 2012 nebo Visual Studio Express 2012 pro Web.</span><span class="sxs-lookup"><span data-stu-id="d2168-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="d2168-131">Nové funkce v technologii ASP.NET a nástroje pro Web 2013.1 pro sadu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d2168-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="d2168-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="d2168-132">Bootstrap</span></span>

<span data-ttu-id="d2168-133">Když generujete řadiče MVC 5 a zobrazení, kód pro zobrazení používá [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="d2168-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="d2168-134">Šablony</span><span class="sxs-lookup"><span data-stu-id="d2168-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="d2168-135">Šablony ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="d2168-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="d2168-136">Jsme přidali novou šablonu MVC 5.</span><span class="sxs-lookup"><span data-stu-id="d2168-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="d2168-137">Odkazuje na nejnovější balíčky MVC 5 NuGet a generování uživatelského rozhraní můžete použít k přidání řadiče a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d2168-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="d2168-138">Šablony ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="d2168-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="d2168-139">Jsme přidali novou šablonu webovém rozhraní API 2.</span><span class="sxs-lookup"><span data-stu-id="d2168-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="d2168-140">Odkazuje na nejnovější balíčky NuGet 2 webové rozhraní API a generování uživatelského rozhraní můžete použít k přidání řadiče a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d2168-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="d2168-141">Šablony položek</span><span class="sxs-lookup"><span data-stu-id="d2168-141">Item Templates</span></span>

<span data-ttu-id="d2168-142">Jsme přidali nové šablony položek pro zobrazení MVC 5, webové stránky (Razor 3) a řadiče webovém rozhraní API 2.</span><span class="sxs-lookup"><span data-stu-id="d2168-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="d2168-143">Při přidávání nové položky instalaci související balíčky NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="d2168-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="d2168-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="d2168-144">Entity Framework 6</span></span>

<span data-ttu-id="d2168-145">Když generujete řadič MVC nebo webového rozhraní API pomocí rozhraní Entity Framework, použijeme Framework 6.</span><span class="sxs-lookup"><span data-stu-id="d2168-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="d2168-146">Další informace o rozhraní Entity Framework najdete v tématu [Entity Framework verze historie](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="d2168-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="d2168-147">Můžete také stáhnout a nainstalovat nástroje Entity Framework 6 pro sadu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d2168-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="d2168-148">Najdete v článku [získat rozhraní Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="d2168-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="d2168-149">ASP.NET generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d2168-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="d2168-150">Generování uživatelského rozhraní ASP.NET je architektura generování kódu pro webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d2168-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="d2168-151">Usnadňuje přidat často používaný kód do projektu, který komunikuje s datovým modelem.</span><span class="sxs-lookup"><span data-stu-id="d2168-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="d2168-152">V předchozích verzích sady Visual Studio generování uživatelského rozhraní omezovala na projekty ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d2168-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="d2168-153">S touto aktualizací teď můžete generování uživatelského rozhraní pro všechny projekt ASP.NET, včetně webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="d2168-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="d2168-154">Tato aktualizace nepodporuje generování stránky pro projekt webové formuláře, ale když můžete nadále používat generování uživatelského rozhraní s webovými formuláři tak, že přidáte do projektu závislosti MVC.</span><span class="sxs-lookup"><span data-stu-id="d2168-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="d2168-155">Podpora pro generování stránky pro webové formuláře bude přidána v budoucí aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="d2168-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="d2168-156">Při použití generování uživatelského rozhraní, je zajištěno, že všechny požadované závislosti jsou nainstalované v projektu.</span><span class="sxs-lookup"><span data-stu-id="d2168-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="d2168-157">Například když začít s projektem webových formulářů ASP.NET a přidání Kontroleru webového rozhraní API pomocí generování uživatelského rozhraní, požadované balíčky NuGet a odkazy se přidají do projektu automaticky.</span><span class="sxs-lookup"><span data-stu-id="d2168-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="d2168-158">Chcete-li přidat generování uživatelského rozhraní MVC do projektu webových formulářů, přidejte **novou vygenerovanou položku** a vyberte **závislosti MVC 5** v dialogovém okně.</span><span class="sxs-lookup"><span data-stu-id="d2168-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="d2168-159">Existují dvě možnosti pro generování uživatelského rozhraní MVC; Minimální a úplné.</span><span class="sxs-lookup"><span data-stu-id="d2168-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="d2168-160">Pokud vyberete minimální, jenom balíčky NuGet a odkazy pro architekturu ASP.NET MVC se přidají do projektu.</span><span class="sxs-lookup"><span data-stu-id="d2168-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="d2168-161">Pokud vyberete možnost Úplná minimální závislosti přidají, a také požadované soubory obsahu pro projekt MVC.</span><span class="sxs-lookup"><span data-stu-id="d2168-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="d2168-162">Podpora pro generování uživatelského rozhraní asynchronní řadiče využívá nové funkce asynchronní z Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="d2168-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="d2168-163">Další informace a podrobné pokyny najdete v tématu [generování uživatelského rozhraní ASP.NET: Přehled](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d2168-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="d2168-164">Tyto kurzy zobrazit generování uživatelského rozhraní s Visual Studio 2013, ale jsou i pro technologii ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d2168-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="d2168-165">Razor Editor</span><span class="sxs-lookup"><span data-stu-id="d2168-165">Razor Editor</span></span>

<span data-ttu-id="d2168-166">Pomocí této aktualizace Visual Studio 2012 teď podporuje nástrojů nebo úpravách Razor 3.</span><span class="sxs-lookup"><span data-stu-id="d2168-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="d2168-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="d2168-167">NuGet 2.7</span></span>

<span data-ttu-id="d2168-168">NuGet 2.7 zahrnuje celou řadu nových funkcí, které jsou popsány podrobně v [poznámky k verzi 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="d2168-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="d2168-169">Tato verze NuGet odebírá potřebu uživatelům výslovně povolit NuGet, chcete-li obnovit chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="d2168-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="d2168-170">Při instalaci NuGet 2.7, uživatelé implicitně svůj souhlas automaticky obnovují se balíčky chybí.</span><span class="sxs-lookup"><span data-stu-id="d2168-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="d2168-171">Uživatelé mohou explicitně vyjádření výslovného nesouhlasu balíček obnovení prostřednictvím nastavení NuGet v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d2168-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="d2168-172">Tato změna zjednodušuje, jak funguje obnovení balíčku.</span><span class="sxs-lookup"><span data-stu-id="d2168-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="d2168-173">Známé problémy a nejnovější změny</span><span class="sxs-lookup"><span data-stu-id="d2168-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="d2168-174">ASP.NET generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d2168-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="d2168-175">MVC a webových rozhraní API generování uživatelského rozhraní – chyby HTTP 404, nebyla nalezena chyba</span><span class="sxs-lookup"><span data-stu-id="d2168-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="d2168-176">Pokud dojde k chybě při přidání vygenerované položky do projektu, je možné, že projekt bude ponechána v nekonzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="d2168-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="d2168-177">Některé změny provedené být generování uživatelského rozhraní se vrátí zpátky, ale ostatní změny, jako je například nainstalované balíčky NuGet, nebude vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="d2168-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="d2168-178">Směrování změny konfigurace se vrátí zpátky, uživatelé obdrží chybu HTTP 404 při navigaci na generované uživatelské rozhraní položky.</span><span class="sxs-lookup"><span data-stu-id="d2168-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="d2168-179">Chcete-li vyřešit tuto chybu pro MVC, přidat novou položku vygenerované a vyberte závislosti MVC 5 (minimální nebo úplná).</span><span class="sxs-lookup"><span data-stu-id="d2168-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="d2168-180">Tento proces bude do projektu přidejte všechny požadované změny.</span><span class="sxs-lookup"><span data-stu-id="d2168-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="d2168-181">Odstranění této chyby pro webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="d2168-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="d2168-182">Do projektu přidejte následující třídy WebApiConfig.</span><span class="sxs-lookup"><span data-stu-id="d2168-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="d2168-183">Konfigurace WebApiConfig.Register v aplikaci\_Start – metoda v souboru Global.asax následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d2168-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="d2168-184">Visual Studio Express 2012 pro Web přestane pracovat po přidání vygenerované položky</span><span class="sxs-lookup"><span data-stu-id="d2168-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="d2168-185">Pokud sadu Visual Studio Express 2012 pro Web přestane pracovat po přidání vygenerované položky s platformou Entity Framework (například webové 2 kontroler API s akcemi používající rozhraní Entity Framework), je možné, že Visual Studio Express Nepodařilo se načíst nativní bitové kopie sestavení závisí na System.Web.Extensions.</span><span class="sxs-lookup"><span data-stu-id="d2168-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="d2168-186">Chcete-li tento problém, nakonfigurujte Visual Studio Express pro práci s MSIL bitové kopie System.Web.Extensions:</span><span class="sxs-lookup"><span data-stu-id="d2168-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="d2168-187">Otevřete příkazový řádek v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="d2168-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="d2168-188">Přejděte na 11.0\Common7\IDE %ProgramFiles%\Microsoft Visual Studio nebo % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (pro 64bitové Windows).</span><span class="sxs-lookup"><span data-stu-id="d2168-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="d2168-189">V textovém editoru otevřete VWDExpress.exe.config.</span><span class="sxs-lookup"><span data-stu-id="d2168-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="d2168-190">Přidejte následující řádek v části &lt;konfigurace&gt;/&lt;runtime&gt; element:</span><span class="sxs-lookup"><span data-stu-id="d2168-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="d2168-191">Restartování sady Visual Studio Express 2012 pro Web.</span><span class="sxs-lookup"><span data-stu-id="d2168-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="d2168-192">Syntaxe Razor rozhraní ASP.NET 3</span><span class="sxs-lookup"><span data-stu-id="d2168-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="d2168-193">Zobrazení withBrowse soubor cshtml WithorF5causes chyby serveru</span><span class="sxs-lookup"><span data-stu-id="d2168-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="d2168-194">Při vytváření projektu MVC 5 v sadě Visual Studio 2012 (nebo otevřete v projektu sady Visual Studio 2012 MVC 5, který byl vytvořen v sadě Visual Studio 2013) a pokus o zobrazení souboru cshtml pomocí procházet s nebo F5, se zobrazí chyba s oznámením - **chyba serveru v Aplikace '/'**.</span><span class="sxs-lookup"><span data-stu-id="d2168-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="d2168-195">Server se pokusí přejděte na `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="d2168-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="d2168-196">Chcete-li problém vyřešit, změňte **spustit akci** nastavení ve vašem projektu a **konkrétní stránky**.</span><span class="sxs-lookup"><span data-stu-id="d2168-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="d2168-197">Není nutné zadat hodnotu pro stránku.</span><span class="sxs-lookup"><span data-stu-id="d2168-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="d2168-198">Po provedení této změny, výběr F5 přejde do kořenového adresáře aplikace (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="d2168-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="d2168-199">Toto chování není stejný jako chování pro projekty MVC 5 v sadě Visual Studio 2013, kde **aktuální stránku** nastavení spustí otevřete stránku.</span><span class="sxs-lookup"><span data-stu-id="d2168-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="d2168-200">Přepisování adres URL a Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="d2168-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="d2168-201">Po upgradu na ASP.NET Razor 3 nebo ASP.NET MVC 5, zápis tilde(~) může již nebude fungovat správně, pokud používáte přepisů adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d2168-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="d2168-202">Přepisování adres URL ovlivňuje tilde(~) zápis v elementů HTML jako &lt;A /&gt;, &lt;skriptu nebo&gt;, &lt;odkaz nebo&gt;, a v důsledku tilda už mapuje na kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="d2168-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="d2168-203">Například, pokud přepsání žádosti o **asp.net/content** k **asp.net**, atribut href v &lt;A href = "~/content/" /&gt; přeloží na **/content/ obsah nebo** místo **/**.</span><span class="sxs-lookup"><span data-stu-id="d2168-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="d2168-204">Chcete-li potlačit tuto změnu, můžete nastavit **IIS\_WasUrlRewritten** kontext, který má hodnotu false na každé webové stránce nebo v **aplikace\_BeginRequest** v souboru Global.asax.</span><span class="sxs-lookup"><span data-stu-id="d2168-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="d2168-205">Šablony</span><span class="sxs-lookup"><span data-stu-id="d2168-205">Templates</span></span>

<span data-ttu-id="d2168-206">Při vytváření rozhraní ASP.NET MVC projektů pomocí sady Visual Studio 2012 na Windows 8.1 nebo Windows Server 2012 R2, Visual Studio zobrazí chybovou zprávu s oznámením "Web konfigurace [url] pro technologii ASP.NET 4.5 se nezdařilo."</span><span class="sxs-lookup"><span data-stu-id="d2168-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![Chyba konfigurace.](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="d2168-208">Tato chyba se může zobrazit, protože Visual Studio 2012 není funkce technologie ASP.NET 4.5 při instalaci v těchto verzích Windows.</span><span class="sxs-lookup"><span data-stu-id="d2168-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="d2168-209">Pokud chcete povolit technologii ASP.NET 4.5, proveďte kroky popsané v [Windows zapnout nebo vypnout funkce](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="d2168-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![zapnout nebo vypnout funkce systému Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="d2168-211">Alternativně můžete povolit technologii ASP.NET 4.5 pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d2168-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="d2168-212">Otevřete příkazový řádek v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="d2168-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="d2168-213">Spusťte následující příkaz k povolení technologie ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="d2168-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
