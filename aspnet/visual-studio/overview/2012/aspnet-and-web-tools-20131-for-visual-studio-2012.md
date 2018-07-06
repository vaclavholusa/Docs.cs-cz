---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Poznámky k verzi pro ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012 | Dokumentace Microsoftu
author: microsoft
description: Tento dokument popisuje verzi technologie ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012.
ms.author: aspnetcontent
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 33577dec7278694f932ac7eded84359baacf65bc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829098"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="dcc21-103">Zpráva k vydání verze pro ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="dcc21-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="dcc21-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="dcc21-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="dcc21-105">Tento dokument popisuje verzi technologie ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="dcc21-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="dcc21-106">Obsah</span><span class="sxs-lookup"><span data-stu-id="dcc21-106">Contents</span></span>

- [<span data-ttu-id="dcc21-107">Poznámky k instalaci</span><span class="sxs-lookup"><span data-stu-id="dcc21-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="dcc21-108">Požadavky na software</span><span class="sxs-lookup"><span data-stu-id="dcc21-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="dcc21-109">Novinky v ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="dcc21-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="dcc21-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="dcc21-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="dcc21-111">Šablony</span><span class="sxs-lookup"><span data-stu-id="dcc21-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="dcc21-112">Šablony ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="dcc21-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="dcc21-113">Šablony ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="dcc21-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="dcc21-114">Šablony položek</span><span class="sxs-lookup"><span data-stu-id="dcc21-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="dcc21-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="dcc21-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="dcc21-116">ASP.NET generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="dcc21-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="dcc21-117">Razor Editor</span><span class="sxs-lookup"><span data-stu-id="dcc21-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="dcc21-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="dcc21-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="dcc21-119">Známé problémy a změny způsobující chyby</span><span class="sxs-lookup"><span data-stu-id="dcc21-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="dcc21-120">ASP.NET generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="dcc21-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="dcc21-121">MVC a generování rozhraní Web API – HTTP 404, nebyla nalezena chyba</span><span class="sxs-lookup"><span data-stu-id="dcc21-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="dcc21-122">Visual Studio Express 2012 pro Web přestane pracovat po přidání vygenerované položky</span><span class="sxs-lookup"><span data-stu-id="dcc21-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="dcc21-123">Syntaxe Razor rozhraní ASP.NET 3</span><span class="sxs-lookup"><span data-stu-id="dcc21-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="dcc21-124">Zobrazení souboru cshtml procházet s nebo F5 způsobí chybu serveru</span><span class="sxs-lookup"><span data-stu-id="dcc21-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="dcc21-125">Přepsání adresy URL a Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="dcc21-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="dcc21-126">Šablony</span><span class="sxs-lookup"><span data-stu-id="dcc21-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="dcc21-127">Poznámky k instalaci</span><span class="sxs-lookup"><span data-stu-id="dcc21-127">Installation Notes</span></span>

<span data-ttu-id="dcc21-128">[Nainstalujte](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) technologie ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="dcc21-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="dcc21-129">Požadavky na software</span><span class="sxs-lookup"><span data-stu-id="dcc21-129">Software Requirements</span></span>

<span data-ttu-id="dcc21-130">Musíte mít Visual Studio 2012 nebo Visual Studio Express 2012 pro Web.</span><span class="sxs-lookup"><span data-stu-id="dcc21-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="dcc21-131">Novinky v ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="dcc21-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="dcc21-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="dcc21-132">Bootstrap</span></span>

<span data-ttu-id="dcc21-133">Při generování uživatelského rozhraní zobrazení a kontrolerů MVC 5, používá značky pro zobrazení [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="dcc21-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="dcc21-134">Šablony</span><span class="sxs-lookup"><span data-stu-id="dcc21-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="dcc21-135">Šablony ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="dcc21-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="dcc21-136">Přidali jsme nové šablony MVC 5.</span><span class="sxs-lookup"><span data-stu-id="dcc21-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="dcc21-137">Odkazuje na nejnovější balíčky MVC 5 NuGet a generování uživatelského rozhraní můžete použít k přidání kontrolerů a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dcc21-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="dcc21-138">Šablony ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="dcc21-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="dcc21-139">Přidali jsme nové šablony webové rozhraní API 2.</span><span class="sxs-lookup"><span data-stu-id="dcc21-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="dcc21-140">Odkazuje na nejnovější balíčky NuGet 2 webové rozhraní API a generování uživatelského rozhraní můžete použít k přidání kontrolerů a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="dcc21-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="dcc21-141">Šablony položek</span><span class="sxs-lookup"><span data-stu-id="dcc21-141">Item Templates</span></span>

<span data-ttu-id="dcc21-142">Přidali jsme nové šablony položek pro zobrazení MVC 5, Web Pages (Razor 3) a webovým rozhraním API 2 řadiče.</span><span class="sxs-lookup"><span data-stu-id="dcc21-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="dcc21-143">Při přidání nové položky nainstalují související balíčků NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="dcc21-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="dcc21-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="dcc21-144">Entity Framework 6</span></span>

<span data-ttu-id="dcc21-145">Při generování uživatelského rozhraní řadič MVC nebo webového rozhraní API pomocí Entity Frameworku používáme Framework 6.</span><span class="sxs-lookup"><span data-stu-id="dcc21-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="dcc21-146">Další informace o rozhraní Entity Framework naleznete v tématu [historie verzí Entity Framework](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="dcc21-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="dcc21-147">Můžete také stáhnout a nainstalovat Entity Framework 6 Tools for Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="dcc21-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="dcc21-148">Zobrazit [získat Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="dcc21-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="dcc21-149">ASP.NET generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="dcc21-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="dcc21-150">ASP.NET generování uživatelského rozhraní je architektura generování kódu pro webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="dcc21-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="dcc21-151">To umožňuje snadno přidat do projektu, který komunikuje s datovým modelem často používaný kód.</span><span class="sxs-lookup"><span data-stu-id="dcc21-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="dcc21-152">V předchozích verzích sady Visual Studio generování uživatelského rozhraní omezovala na projekty ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="dcc21-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="dcc21-153">S touto aktualizací teď můžete generování uživatelského rozhraní pro libovolný projekt ASP.NET, včetně webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="dcc21-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="dcc21-154">Tato aktualizace nepodporuje generování stránek pro projekt webových formulářů, ale můžete pořád používat generování uživatelského rozhraní s webovými formuláři tak, že přidáte k projektu závislosti MVC.</span><span class="sxs-lookup"><span data-stu-id="dcc21-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="dcc21-155">Podpora pro generování stránky pro webové formuláře bude přidána v budoucí aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="dcc21-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="dcc21-156">Při používání generování uživatelského rozhraní, zajišťujeme, že všechny požadované závislosti jsou nainstalovány v projektu.</span><span class="sxs-lookup"><span data-stu-id="dcc21-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="dcc21-157">Například pokud začínat projekt webových formulářů ASP.NET a poté použijte generování uživatelského rozhraní pro přidání Kontroleru webového rozhraní API, požadované balíčky NuGet a odkazy jsou přidány do projektu automaticky.</span><span class="sxs-lookup"><span data-stu-id="dcc21-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="dcc21-158">Chcete-li přidat generování uživatelského rozhraní MVC do projektu webových formulářů, přidejte **novou vygenerovanou položku** a vyberte **závislosti MVC 5** v dialogovém okně.</span><span class="sxs-lookup"><span data-stu-id="dcc21-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="dcc21-159">Existují dvě možnosti pro generování uživatelského rozhraní MVC; Minimální a úplné.</span><span class="sxs-lookup"><span data-stu-id="dcc21-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="dcc21-160">Pokud vyberete minimální, pouze balíčky NuGet a odkazy pro architekturu ASP.NET MVC se přidají do vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="dcc21-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="dcc21-161">Pokud vyberete možnost Úplná minimální závislosti jsou přidány, a také požadované soubory obsahu pro projekt MVC.</span><span class="sxs-lookup"><span data-stu-id="dcc21-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="dcc21-162">Podpora pro generování kontrolerů asynchronní používá nové asynchronní funkce z Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="dcc21-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="dcc21-163">Další informace a podrobné pokyny najdete v tématu [generování uživatelského rozhraní ASP.NET: Přehled](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dcc21-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="dcc21-164">Tyto kurzy vám ukážou generování uživatelského rozhraní pomocí sady Visual Studio 2013, ale navíc se vztahuje na ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="dcc21-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="dcc21-165">Razor Editor</span><span class="sxs-lookup"><span data-stu-id="dcc21-165">Razor Editor</span></span>

<span data-ttu-id="dcc21-166">V této aktualizaci Visual Studio 2012 teď podporuje nástroje a úpravy Razor 3.</span><span class="sxs-lookup"><span data-stu-id="dcc21-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="dcc21-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="dcc21-167">NuGet 2.7</span></span>

<span data-ttu-id="dcc21-168">NuGet 2.7 zahrnuje bohatou sadu nových funkcí, které jsou popsány podrobně v [zpráva k vydání verze NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="dcc21-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="dcc21-169">Tato verze NuGet eliminuje potřebu uživatelům výslovně povolit NuGet Chcete-li obnovit chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="dcc21-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="dcc21-170">Při instalaci NuGet 2.7, uživatelé se implicitně souhlasit s automaticky obnovit chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="dcc21-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="dcc21-171">Uživatele můžete výslovně nesouhlasit obnovení balíčku NuGet nastavení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcc21-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="dcc21-172">Tato změna zjednodušuje, jak funguje obnovení balíčku.</span><span class="sxs-lookup"><span data-stu-id="dcc21-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="dcc21-173">Známé problémy a změny způsobující chyby</span><span class="sxs-lookup"><span data-stu-id="dcc21-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="dcc21-174">ASP.NET generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="dcc21-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="dcc21-175">MVC a generování rozhraní Web API – HTTP 404, nebyla nalezena chyba</span><span class="sxs-lookup"><span data-stu-id="dcc21-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="dcc21-176">Pokud narazíte na chybu při přidání vygenerované položky do projektu, je možné, že váš projekt bude ponechána v nekonzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="dcc21-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="dcc21-177">Některé změny se generování uživatelského rozhraní bude vrácena zpět, ale jiné změny, jako je například nainstalované balíčky NuGet, nebude vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="dcc21-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="dcc21-178">Pokud se vrátí zpět změny konfigurace směrování, uživatelům se zobrazí chyba HTTP 404 při přechodu na automaticky generovaný položky.</span><span class="sxs-lookup"><span data-stu-id="dcc21-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="dcc21-179">Chcete-li vyřešit tuto chybu pro MVC, přidejte nová vygenerovaná položka a vybrat závislosti MVC 5 (minimální nebo úplná).</span><span class="sxs-lookup"><span data-stu-id="dcc21-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="dcc21-180">Tento proces se všechny požadované změny, přidejte do projektu.</span><span class="sxs-lookup"><span data-stu-id="dcc21-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="dcc21-181">Chcete-li vyřešit tuto chybu pro webové rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="dcc21-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="dcc21-182">Do projektu přidejte následující třídy WebApiConfig.</span><span class="sxs-lookup"><span data-stu-id="dcc21-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="dcc21-183">V aplikaci nakonfigurovat WebApiConfig.Register\_začátek metody v souboru Global.asax následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dcc21-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="dcc21-184">Visual Studio Express 2012 pro Web přestane pracovat po přidání vygenerované položky</span><span class="sxs-lookup"><span data-stu-id="dcc21-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="dcc21-185">Pokud sadu Visual Studio Express 2012 pro Web přestane pracovat po přidání vygenerované položky s Entity Framework (například Kontroleru webového rozhraní API 2 s akcemi používající nástroj Entity Framework), je možné, že Visual Studio Express se nepodařilo načíst nativní bitové kopie sestavení závisí na System.Web.Extensions.</span><span class="sxs-lookup"><span data-stu-id="dcc21-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="dcc21-186">Chcete-li tento problém, nakonfigurujte Visual Studio Express pro práci s MSIL obrázek System.Web.Extensions:</span><span class="sxs-lookup"><span data-stu-id="dcc21-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="dcc21-187">Otevřete příkazový řádek v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="dcc21-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="dcc21-188">Přejděte na %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE nebo % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (pro 64bitová verze Windows).</span><span class="sxs-lookup"><span data-stu-id="dcc21-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="dcc21-189">V textovém editoru otevřete VWDExpress.exe.config.</span><span class="sxs-lookup"><span data-stu-id="dcc21-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="dcc21-190">Přidejte následující řádek pod &lt;konfigurace&gt;/&lt;runtime&gt; element:</span><span class="sxs-lookup"><span data-stu-id="dcc21-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="dcc21-191">Restartování sady Visual Studio Express 2012 pro Web.</span><span class="sxs-lookup"><span data-stu-id="dcc21-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="dcc21-192">Syntaxe Razor rozhraní ASP.NET 3</span><span class="sxs-lookup"><span data-stu-id="dcc21-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="dcc21-193">Zobrazení withBrowse souboru cshtml WithorF5causes chyba serveru</span><span class="sxs-lookup"><span data-stu-id="dcc21-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="dcc21-194">Při vytvoření projektu aplikace MVC 5 v sadě Visual Studio 2012 (nebo otevřít v sadě Visual Studio 2012 MVC 5 projekt, který byl vytvořen v sadě Visual Studio 2013) a pokusíte se k zobrazení souboru cshtml pomocí procházet s nebo F5, zobrazí se chybové zprávy, - **chyba serveru Aplikace '/'**.</span><span class="sxs-lookup"><span data-stu-id="dcc21-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="dcc21-195">Server se pokusí přejít na `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="dcc21-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="dcc21-196">Chcete-li tento problém vyřešit, změňte **spustit akci** nastavení ve vašem projektu a **konkrétní stránka**.</span><span class="sxs-lookup"><span data-stu-id="dcc21-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="dcc21-197">Není potřeba zadat hodnotu pro stránku.</span><span class="sxs-lookup"><span data-stu-id="dcc21-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="dcc21-198">Po provedení této změny výběru F5 přejde do kořenového adresáře aplikace (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="dcc21-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="dcc21-199">Toto chování není stejný jako chování pro projekty MVC 5 v sadě Visual Studio 2013, kde **aktuální stránku** spustí otevřete stránku nastavení.</span><span class="sxs-lookup"><span data-stu-id="dcc21-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="dcc21-200">Přepsání adresy URL a Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="dcc21-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="dcc21-201">Po upgradu na 3 Razor technologie ASP.NET nebo ASP.NET MVC 5, zápis tilde(~) už nemusí fungovat správně Pokud používáte adresu URL přepisů.</span><span class="sxs-lookup"><span data-stu-id="dcc21-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="dcc21-202">Přepsání adresy URL ovlivňuje zápis tilde(~) prvků HTML, jako &lt;A /&gt;, &lt;skript /&gt;, &lt;odkaz /&gt;, a v důsledku tilda už mapuje na kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="dcc21-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="dcc21-203">Například, pokud přepsání požadavků pro **asp.net/content** k **asp.net**, atribut href v &lt;A href = "~/content/" /&gt; přeloží na **/content/ obsah /** místo **/**.</span><span class="sxs-lookup"><span data-stu-id="dcc21-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="dcc21-204">Chcete-li potlačit tuto změnu, můžete nastavit **IIS\_WasUrlRewritten** na hodnotu false v jednotlivých webových stránkách nebo v kontextu **aplikace\_BeginRequest** v souboru Global.asax.</span><span class="sxs-lookup"><span data-stu-id="dcc21-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="dcc21-205">Šablony</span><span class="sxs-lookup"><span data-stu-id="dcc21-205">Templates</span></span>

<span data-ttu-id="dcc21-206">Při vytváření technologie ASP.NET MVC projektů s Visual Studio 2012 ve Windows 8.1 nebo Windows Server 2012 R2, Visual Studio zobrazí chybovou zprávu s oznámením "Webové konfigurace [url] pro technologii ASP.NET 4.5 se nezdařilo."</span><span class="sxs-lookup"><span data-stu-id="dcc21-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![Chyba konfigurace](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="dcc21-208">Tato chyba se může zobrazit, protože Visual Studio 2012 není funkce technologie ASP.NET 4.5 nainstalovaném v těchto verzích Windows.</span><span class="sxs-lookup"><span data-stu-id="dcc21-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="dcc21-209">Pokud chcete povolit technologii ASP.NET 4.5, proveďte kroky popsané v [Windows zapnout nebo vypnout funkce](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="dcc21-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![zapnout nebo vypnout funkce Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="dcc21-211">Alternativně můžete povolit technologii ASP.NET 4.5 prostřednictvím příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="dcc21-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="dcc21-212">Otevřete příkazový řádek v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="dcc21-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="dcc21-213">Spuštěním následujícího příkazu povolte ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="dcc21-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
