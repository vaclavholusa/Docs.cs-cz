---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profil a ladění aplikace ASP.NET MVC pomocí balíčku Glimpse | Microsoft Docs
author: Rick-Anderson
description: Balíčku glimpse je neúspěchu a rozšiřujících se řadu balíčky NuGet s otevřeným zdrojem, který poskytuje podrobné výkon, ladění a diagnostické informace pro technologii ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 6ac23256c57116de81c7bf690d5ce743301c75ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872972"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="3ec8c-103">Profil a ladění aplikace ASP.NET MVC pomocí balíčku Glimpse</span><span class="sxs-lookup"><span data-stu-id="3ec8c-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="3ec8c-104">podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="3ec8c-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="3ec8c-105">Balíčku glimpse je neúspěchu a rozšiřujících se řadu balíčky NuGet s otevřeným zdrojem, který poskytuje podrobné výkon, ladění a diagnostické informace pro aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="3ec8c-106">Je trivial k instalaci, lightweight, ultrarychlého a zobrazí klíčové metriky výkonu v dolní části každé stránky.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="3ec8c-107">Umožňuje rozbalit vaší aplikace, když potřebujete zjistit, co se děje na serveru.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="3ec8c-108">Balíčku glimpse poskytuje mnohem cenné informace, doporučujeme, že můžete ji použít v celé vaší cyklu vývoje, včetně Azure testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="3ec8c-109">Při [Fiddler](http://www.telerik.com/fiddler) a [nástroje pro vývoj F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) poskytují straně klienta zobrazení balíčku Glimpse poskytuje podrobný pohled ze serveru.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="3ec8c-110">V tomto kurzu se soustředí na pomocí balíčku Glimpse ASP.NET MVC a EF balíčků, ale jsou k dispozici mnoho dalších balíčků.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="3ec8c-111">Kde to bude možné I odkaz na příslušné [balíčku Glimpse dokumentace](http://getglimpse.com/Docs/) který I pomáhají udržovat.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="3ec8c-112">Balíčku glimpse je opensourcový projekt, příliš se může přispívat k zdrojový kód a dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="3ec8c-113">Instalace balíčku Glimpse</span><span class="sxs-lookup"><span data-stu-id="3ec8c-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="3ec8c-114">Povolit balíčku Glimpse pro místního hostitele</span><span class="sxs-lookup"><span data-stu-id="3ec8c-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="3ec8c-115">Na kartě časové osy</span><span class="sxs-lookup"><span data-stu-id="3ec8c-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="3ec8c-116">Vazby modelu</span><span class="sxs-lookup"><span data-stu-id="3ec8c-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="3ec8c-117">Trasy</span><span class="sxs-lookup"><span data-stu-id="3ec8c-117">Routes</span></span>](#route)
- [<span data-ttu-id="3ec8c-118">Pomocí balíčku Glimpse na Azure</span><span class="sxs-lookup"><span data-stu-id="3ec8c-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="3ec8c-119">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="3ec8c-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="3ec8c-120">Instalace balíčku Glimpse</span><span class="sxs-lookup"><span data-stu-id="3ec8c-120">Installing Glimpse</span></span>

<span data-ttu-id="3ec8c-121">Balíčku Glimpse můžete nainstalovat z konzoly Správce balíčků NuGet nebo **spravovat balíčky NuGet** konzoly.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="3ec8c-122">V této ukázce nainstaluji Mvc5 a EF6 balíčky:</span><span class="sxs-lookup"><span data-stu-id="3ec8c-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![instalace balíčku Glimpse z NuGet dialogové okno](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="3ec8c-124">Vyhledejte *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="3ec8c-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF z dialogové okno instalace NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="3ec8c-126">Výběrem **nainstalované balíky**, můžete zobrazit závislé moduly balíčku Glimpse nainstalován:</span><span class="sxs-lookup"><span data-stu-id="3ec8c-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Instalovat balíčky balíčku Glimpse z dialogové okno](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="3ec8c-128">Následující příkazy instalace balíčku Glimpse MVC5 a EF6 modulů z konzoly Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="3ec8c-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="3ec8c-129">Povolit balíčku Glimpse pro místního hostitele</span><span class="sxs-lookup"><span data-stu-id="3ec8c-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="3ec8c-130">Přejděte na http://localhost: &lt;portu #&gt;/glimpse.axd a klikněte na <strong>balíčku Glimpse zapnout</strong> tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Stránka axd balíčku glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="3ec8c-132">Pokud máte panelu Oblíbené položky zobrazí, vám může přetahování tlačítka balíčku Glimpse a přidejte je jako bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="3ec8c-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Aplikace Internet Explorer s balíčku Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="3ec8c-134">Teď můžete Navigovat vaší aplikace a **hlav si zobrazení** (HUD) se zobrazí v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Obraťte se na správce stránka s HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="3ec8c-136">[Stránky balíčku Glimpse HUD](http://getglimpse.com/Docs/Heads-up-Display) časování podrobnosti uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="3ec8c-137">Zobrazí data HUD nerušivý výkonu vás může upozornit na problém okamžitě – předtím, než získáte cyklus testu.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="3ec8c-138">Kliknutím na &quot;g&quot; zobrazí v pravém dolním panelu balíčku Glimpse:</span><span class="sxs-lookup"><span data-stu-id="3ec8c-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Panel balíčku glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="3ec8c-140">Na obrázku výše [kartě provádění](http://getglimpse.com/Docs/Execution-Tab) je vybraná, který obsahuje podrobnosti časování akcí a filtrů v kanálu.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="3ec8c-141">Můžete zobrazit Moje [zastavit sledování filtru časovače](http://www.nuget.org/packages/StopWatch/) spustit ve fázi 6 kanálu.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="3ec8c-142">Při Moje lehká časovače může poskytnout užitečné profil/časování data, jeho neúspěšných přístupů do všech čas strávený v autorizace a vykreslování zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="3ec8c-143">Další informace o Moje časovače v [profilu a času Azure vaše aplikace ASP.NET MVC úplně](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ec8c-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="3ec8c-144">[Karty](http://getglimpse.com/Docs/Tabs) stránka obsahuje odkazy na podrobné informace na každé kartě.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="3ec8c-145">Na kartě časové osy</span><span class="sxs-lookup"><span data-stu-id="3ec8c-145">The Timeline tab</span></span>

<span data-ttu-id="3ec8c-146">Došlo tní Dykstra nezpracovaných [kurz EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) změnit řadiče vyučující následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="3ec8c-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="3ec8c-147">Výše uvedený kód umožňuje mi předat v řetězci dotazu (`eager`) na ovládací prvek přes nebo explicitní načítání dat.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="3ec8c-148">Na následujícím obrázku se používá explicitní načítání a časování stránka zobrazuje každou registrace načten do `Index` metodu akce:</span><span class="sxs-lookup"><span data-stu-id="3ec8c-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![explicitní načítání](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="3ec8c-150">V následujícím kódu, je zadána eager a načte jsou po každém zápisu `Index` se nazývá zobrazení:</span><span class="sxs-lookup"><span data-stu-id="3ec8c-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![je zadán eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="3ec8c-152">Můžete podržet přes segment čas časování podrobné informace:</span><span class="sxs-lookup"><span data-stu-id="3ec8c-152">You can hover over a time segment to get detailed timing information:</span></span>

![Počkejte, až se najdete v podrobné časování](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="3ec8c-154">Vazby modelu</span><span class="sxs-lookup"><span data-stu-id="3ec8c-154">Model Binding</span></span>

<span data-ttu-id="3ec8c-155">[Kartě vazby modelu](http://getglimpse.com/Docs/Model-Binding-Tab) poskytuje širokou řadu informace, které vám pomohou pochopit, jak jsou svázané s proměnných formuláře a proč některé nejsou vázán, jako byste očekávali.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="3ec8c-156">Obrázek níže znázorňuje **?**</span><span class="sxs-lookup"><span data-stu-id="3ec8c-156">The image below shows the **?**</span></span> <span data-ttu-id="3ec8c-157">ikonu, která můžete kliknutím na se zprovoznit stránku nápovědy balíčku glimpse této funkce.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![balíčku glimpse zobrazení vazby modelu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="3ec8c-159">Trasy</span><span class="sxs-lookup"><span data-stu-id="3ec8c-159">Routes</span></span>

 <span data-ttu-id="3ec8c-160">Na kartě balíčku Glimpse trasy se vám může pomoct ladění a pochopit směrování.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="3ec8c-161">Na obrázku níže produktu trasa se vybere (a zobrazuje zeleně balíčku Glimpse convention).</span><span class="sxs-lookup"><span data-stu-id="3ec8c-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="3ec8c-162">![Vybraný název produktu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) tokeny omezení, oblasti a data trasy se také zobrazí.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="3ec8c-163">V tématu [trasy balíčku Glimpse](http://getglimpse.com/Docs/Routes-Tab) a [atribut směrování v ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="3ec8c-164">Pomocí balíčku Glimpse na Azure</span><span class="sxs-lookup"><span data-stu-id="3ec8c-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="3ec8c-165">Výchozí zásady zabezpečení balíčku Glimpse umožňuje pouze data balíčku Glimpse zobrazit z místního hostitele.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="3ec8c-166">Tuto zásadu zabezpečení můžete změnit, aby se dala zobrazit tato data na vzdáleném serveru (například webové aplikace v Azure).</span><span class="sxs-lookup"><span data-stu-id="3ec8c-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="3ec8c-167">Pro testovací prostředí v Azure, přidejte zvýrazněná značka až do dolní části *Web.config* souboru balíčku Glimpse:</span><span class="sxs-lookup"><span data-stu-id="3ec8c-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="3ec8c-168">V této změně samostatně může každý uživatel zobrazit vaše data balíčku Glimpse na vzdálený web.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="3ec8c-169">Zvažte přidání značka výše k profil publikování, je nasazena pouze použité při použití tohoto profilu publikování (například vaše Azure testovací proifle.) Chcete-li omezit data balíčku Glimpse, přidáme `canViewGlimpseData` role a povolit pouze uživatelé v této roli k zobrazení dat balíčku Glimpse.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="3ec8c-170">Odebere komentáře z *GlimpseSecurityPolicy.cs* soubor a změňte [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) volat z `Administrator` k `canViewGlimpseData` role:</span><span class="sxs-lookup"><span data-stu-id="3ec8c-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="3ec8c-171">Zabezpečení – bohaté údaje poskytnuté balíčku Glimpse mohla vystavit zabezpečení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="3ec8c-172">Microsoft neprovedl auditu zabezpečení balíčku glimpse pro použití v aplikacích výroby.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="3ec8c-173">Informace o přidávání rolí, najdete v části Moje [nasazení webové aplikace zabezpečené ASP.NET MVC 5 s členství, OAuth a databáze SQL Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) kurzu.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="3ec8c-174">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="3ec8c-174">Additional Resources</span></span>

- [<span data-ttu-id="3ec8c-175">Nasazení aplikace zabezpečené ASP.NET MVC 5 s členství, OAuth a databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="3ec8c-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="3ec8c-176">[Konfigurace balíčku glimpse](http://getglimpse.com/Docs/Configuration) -stránka dokumentace týkající se konfigurace karty, zásady modulu runtime, protokolování a další.</span><span class="sxs-lookup"><span data-stu-id="3ec8c-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
