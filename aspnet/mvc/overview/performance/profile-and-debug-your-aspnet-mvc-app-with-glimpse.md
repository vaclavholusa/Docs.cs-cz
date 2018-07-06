---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilování a ladění aplikace ASP.NET MVC pomocí balíčku Glimpse | Dokumentace Microsoftu
author: Rick-Anderson
description: Balíčku glimpse je neúspěchu a rostoucí řadu opensourcových balíčků NuGet, která poskytuje podrobné výkonu, ladění a diagnostických informací pro ASP.NET...
ms.author: aspnetcontent
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 4d044217d6c33594b39747a765165b8702338027
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808538"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="10027-103">Profilování a ladění aplikace ASP.NET MVC pomocí balíčku Glimpse</span><span class="sxs-lookup"><span data-stu-id="10027-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="10027-104">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="10027-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="10027-105">Balíčku glimpse je neúspěchu a rostoucí řadu opensourcových balíčků NuGet, která poskytuje podrobné výkonu, ladění a diagnostických informací pro aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="10027-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="10027-106">Je triviální k instalaci, zjednodušené, ultrarychlých a klíčové metriky výkonu se zobrazí v dolní části každé stránky.</span><span class="sxs-lookup"><span data-stu-id="10027-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="10027-107">Umožňuje vám a přejít k podrobnostem do vaší aplikace, když budete chtít zjistit, co se děje na serveru.</span><span class="sxs-lookup"><span data-stu-id="10027-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="10027-108">Balíčku glimpse poskytuje mnohem cenné informace, které doporučujeme že použít v celém cyklu vývoje, včetně Azure testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="10027-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="10027-109">Zatímco [Fiddler](http://www.telerik.com/fiddler) a [F-12 vývojových nástrojů](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) poskytují na straně klienta zobrazení, balíčku Glimpse poskytuje podrobné zobrazení ze serveru.</span><span class="sxs-lookup"><span data-stu-id="10027-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="10027-110">Tento kurz se zaměřuje na pomocí balíčku Glimpse ASP.NET MVC a balíčky EF, ale jsou k dispozici řada dalších balíčků.</span><span class="sxs-lookup"><span data-stu-id="10027-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="10027-111">Kde je to možné nemohu propojit na příslušné [Nakoukněte dokumentace](http://getglimpse.com/Docs/) které můžu pomáhají udržovat.</span><span class="sxs-lookup"><span data-stu-id="10027-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="10027-112">Balíčku glimpse je opensourcový projekt, příliš se může přispívat do zdrojového kódu a dokumenty.</span><span class="sxs-lookup"><span data-stu-id="10027-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="10027-113">Instalace balíčku Glimpse</span><span class="sxs-lookup"><span data-stu-id="10027-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="10027-114">Povolit balíčku Glimpse pro místního hostitele</span><span class="sxs-lookup"><span data-stu-id="10027-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="10027-115">Na kartě časové osy</span><span class="sxs-lookup"><span data-stu-id="10027-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="10027-116">Vazby modelu</span><span class="sxs-lookup"><span data-stu-id="10027-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="10027-117">Trasy</span><span class="sxs-lookup"><span data-stu-id="10027-117">Routes</span></span>](#route)
- [<span data-ttu-id="10027-118">Pomocí balíčku Glimpse v Azure</span><span class="sxs-lookup"><span data-stu-id="10027-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="10027-119">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="10027-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="10027-120">Instalace balíčku Glimpse</span><span class="sxs-lookup"><span data-stu-id="10027-120">Installing Glimpse</span></span>

<span data-ttu-id="10027-121">Balíčku Glimpse můžete nainstalovat z konzoly Správce balíčků NuGet nebo **spravovat balíčky NuGet** konzoly.</span><span class="sxs-lookup"><span data-stu-id="10027-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="10027-122">Pro tuto ukázku nainstaluji Mvc5 a EF6 balíčky:</span><span class="sxs-lookup"><span data-stu-id="10027-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![instalace balíčku Glimpse z NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="10027-124">Vyhledejte *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="10027-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF z dlg instalace NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="10027-126">Výběrem **nainstalované balíčky**, uvidíte balíčku Glimpse závislé moduly, které jsou nainstalovány:</span><span class="sxs-lookup"><span data-stu-id="10027-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Nainstalované balíčky balíčku Glimpse z DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="10027-128">Následující příkazy instalace balíčku Glimpse MVC5 a EF6 modulů z konzoly Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="10027-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="10027-129">Povolit balíčku Glimpse pro místního hostitele</span><span class="sxs-lookup"><span data-stu-id="10027-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="10027-130">Přejděte do http://localhost:&lt; port #&gt;/glimpse.axd a kliknutím <strong>balíčku Glimpse zapnout</strong> tlačítko.</span><span class="sxs-lookup"><span data-stu-id="10027-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Stránka axd balíčku glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="10027-132">Pokud máte panel Oblíbené zobrazí, lze přetáhnout a rozevírací tlačítka balíčku Glimpse a přidejte je jako bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="10027-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Aplikace Internet Explorer s boookmarklets balíčku Glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="10027-134">Nyní se můžete dostat vaše aplikace a **vedoucí nahoru zobrazení** (HUD) se zobrazí v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="10027-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Správce kontaktů stránka s HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="10027-136">[Stránky balíčku Glimpse HUD](http://getglimpse.com/Docs/Heads-up-Display) podrobnosti výše uvedené informace o časování.</span><span class="sxs-lookup"><span data-stu-id="10027-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="10027-137">Zobrazí data HUD nerušivý výkonu může upozornit na problém okamžitě – než se ponoříte do testovacího cyklu.</span><span class="sxs-lookup"><span data-stu-id="10027-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="10027-138">Kliknutím na &quot;g&quot; zobrazí v pravém dolním rohu panelu balíčku Glimpse:</span><span class="sxs-lookup"><span data-stu-id="10027-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Panel balíčku glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="10027-140">Na obrázku výše [karta spuštění](http://getglimpse.com/Docs/Execution-Tab) zaškrtnuto, zobrazí podrobnosti časování akce a filtry v kanálu.</span><span class="sxs-lookup"><span data-stu-id="10027-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="10027-141">Můžete zobrazit Moje [zastavit sledování filtru časovače](http://www.nuget.org/packages/StopWatch/) začínají na fázi 6 kanálu.</span><span class="sxs-lookup"><span data-stu-id="10027-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="10027-142">Moje lehký časovače můžete nabízejí užitečné profilu a data časování, ho výpadky celou dobu trvání autorizace a vykreslení zobrazení.</span><span class="sxs-lookup"><span data-stu-id="10027-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="10027-143">Informace o mé časovač v [profilu a času vaše aplikace ASP.NET MVC úplně přenést do Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="10027-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="10027-144">[Karty](http://getglimpse.com/Docs/Tabs) stránka obsahuje odkazy na podrobné informace na každé kartě.</span><span class="sxs-lookup"><span data-stu-id="10027-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="10027-145">Na kartě časové osy</span><span class="sxs-lookup"><span data-stu-id="10027-145">The Timeline tab</span></span>

<span data-ttu-id="10027-146">Upravili Petr Dykstra nezpracovaných [kurz EF 6 a MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) následujícím kódem změnit Instruktoři kontroleru:</span><span class="sxs-lookup"><span data-stu-id="10027-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="10027-147">Výše uvedený kód umožňuje předat řetězec dotazu (`eager`) do správy dočkat nebo explicitní načtení dat.</span><span class="sxs-lookup"><span data-stu-id="10027-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="10027-148">Na následujícím obrázku se používá explicitní načtení a časování stránka zobrazuje každou registrace načten v `Index` metody akce:</span><span class="sxs-lookup"><span data-stu-id="10027-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![explicitní načtení](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="10027-150">V následujícím kódu je zadán eager a načte se po každé registraci `Index` se nazývá zobrazení:</span><span class="sxs-lookup"><span data-stu-id="10027-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![je zadán eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="10027-152">Při najetí myší nad segment času na získání podrobných informací o časování:</span><span class="sxs-lookup"><span data-stu-id="10027-152">You can hover over a time segment to get detailed timing information:</span></span>

![najetím myši zobrazíte podrobné časování](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="10027-154">Vazby modelu</span><span class="sxs-lookup"><span data-stu-id="10027-154">Model Binding</span></span>

<span data-ttu-id="10027-155">[Kartu vazby modelu](http://getglimpse.com/Docs/Model-Binding-Tab) nabízí celou řadu informace, které vám pomohou pochopit, jak jsou svázány proměnných formuláře a proč některé nejsou vázány podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="10027-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="10027-156">Obrázek níže ukazuje **?**</span><span class="sxs-lookup"><span data-stu-id="10027-156">The image below shows the **?**</span></span> <span data-ttu-id="10027-157">ikonu, která můžete kliknout na zobrazíte na stránce nápovědy balíčku glimpse této funkce.</span><span class="sxs-lookup"><span data-stu-id="10027-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![Nakoukněte zobrazení vazby modelu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="10027-159">Trasy</span><span class="sxs-lookup"><span data-stu-id="10027-159">Routes</span></span>

 <span data-ttu-id="10027-160">Na kartě balíčku Glimpse trasy můžete vám pomůže ladit a lepší pochopení směrování.</span><span class="sxs-lookup"><span data-stu-id="10027-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="10027-161">Na následujícím obrázku produktu trasa se vybere (a zobrazí zeleně, vytváření balíčku Glimpse).</span><span class="sxs-lookup"><span data-stu-id="10027-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="10027-162">![Vybraný název produktu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) trasy tokeny omezení, oblastí a data se zobrazují.</span><span class="sxs-lookup"><span data-stu-id="10027-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="10027-163">Zobrazit [trasy balíčku Glimpse](http://getglimpse.com/Docs/Routes-Tab) a [směrováním atributů v ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="10027-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="10027-164">Pomocí balíčku Glimpse v Azure</span><span class="sxs-lookup"><span data-stu-id="10027-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="10027-165">Výchozí zásady zabezpečení balíčku Glimpse povoluje jenom data balíčku Glimpse zobrazený z místního hostitele.</span><span class="sxs-lookup"><span data-stu-id="10027-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="10027-166">Tyto zásady zabezpečení můžete změnit, abyste mohli zobrazit tato data na vzdáleném serveru (například webové aplikace v Azure).</span><span class="sxs-lookup"><span data-stu-id="10027-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="10027-167">Pro testovací prostředí v Azure, přidejte značku zvýrazněné až do dolní části *web.confg* soubor balíčku Glimpse:</span><span class="sxs-lookup"><span data-stu-id="10027-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="10027-168">Díky této změně samostatně, každý uživatel svá data můžete zobrazit balíčku Glimpse na vzdálený web.</span><span class="sxs-lookup"><span data-stu-id="10027-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="10027-169">Zvažte přidání značky nad profil publikování, obsahuje pouze nasazené použité při použití tohoto profilu publikování (například váš test na platformě Azure proifle.) Pokud chcete omezit data balíčku Glimpse, přidáme `canViewGlimpseData` role a povolit pouze uživatelé v této roli k zobrazení dat balíčku Glimpse.</span><span class="sxs-lookup"><span data-stu-id="10027-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="10027-170">Odebrat komentáře z *GlimpseSecurityPolicy.cs* soubor a změňte [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) volání z `Administrator` k `canViewGlimpseData` role:</span><span class="sxs-lookup"><span data-stu-id="10027-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="10027-171">Zabezpečení – bohaté údaje poskytnuté balíčku Glimpse může zpřístupnit zabezpečení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="10027-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="10027-172">Microsoft neprovedl auditu zabezpečení balíčku glimpse pro použití v produkční aplikace.</span><span class="sxs-lookup"><span data-stu-id="10027-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="10027-173">Informace o přidání rolí, najdete v části Moje [nasazení webové aplikace zabezpečené aplikace ASP.NET MVC 5 s Membership, OAuth a SQL Database do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) kurzu.</span><span class="sxs-lookup"><span data-stu-id="10027-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="10027-174">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="10027-174">Additional Resources</span></span>

- [<span data-ttu-id="10027-175">Nasaďte zabezpečené aplikace ASP.NET MVC 5 s Membership, OAuth a SQL Database do Azure</span><span class="sxs-lookup"><span data-stu-id="10027-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="10027-176">[Nakoukněte konfigurace](http://getglimpse.com/Docs/Configuration) -Doc stránky na karty, zásady modulu runtime, protokolování a další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="10027-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
