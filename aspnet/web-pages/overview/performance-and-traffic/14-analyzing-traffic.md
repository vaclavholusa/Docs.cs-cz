---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Sledování návštěvníka informace (Analytics) pro ASP.NET Web Pages lokality (Razor) | Microsoft Docs
author: tfitzmac
description: Poté, co jste, že jste podmínky webu přechodem, můžete analyzovat provoz vašeho webu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26572932"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="54284-103">Sledování návštěvníka informace (Analytics) pro stránku ASP.NET – webové stránky (Razor)</span><span class="sxs-lookup"><span data-stu-id="54284-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="54284-104">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="54284-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="54284-105">Tento článek popisuje, jak přidat web analytics na stránky na webu technologie ASP.NET Web Pages (Razor) pomocí pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="54284-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="54284-106">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="54284-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="54284-107">Postup odeslání informací o webu provozu do poskytovatele analytics.</span><span class="sxs-lookup"><span data-stu-id="54284-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="54284-108">Jsou to ASP.NET programování v článku nové funkce:</span><span class="sxs-lookup"><span data-stu-id="54284-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="54284-109">`Analytics` Pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="54284-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="54284-110">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="54284-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="54284-111">Rozhraní ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="54284-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="54284-112">Knihovnu ASP.NET Web Helpers (balíček NuGet)</span><span class="sxs-lookup"><span data-stu-id="54284-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="54284-113">Analýza je obecný termín pro technologie, která měří provoz na vašem webu můžete pochopit, jak lidé použít webu.</span><span class="sxs-lookup"><span data-stu-id="54284-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="54284-114">Mnoho analytické služby jsou k dispozici, včetně služeb z Google, Yahoo, StatCounter a dalších.</span><span class="sxs-lookup"><span data-stu-id="54284-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="54284-115">Způsob analýzy, které funguje je, že zaregistrujete k účtu pomocí zprostředkovatele datové analýzy, ve které registrujete lokality, které chcete sledovat. Zprostředkovatel odešle fragment kódu jazyka JavaScript, který obsahuje ID nebo sledování kód pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="54284-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="54284-116">Přidejte fragment kódu JavaScript do webové stránky v lokalitě, která chcete sledovat. (Obvykle přidáte fragmentu analytics zápatí nebo rozložení stránky nebo jiné jazyka HTML, který se zobrazí na každé stránce ve vaší lokalitě.) Pokud uživatelé požadují stránky, který obsahuje jednu z těchto fragmenty kódu jazyka JavaScript, fragmentu odesílá informace o aktuální stránku do zprostředkovatele datové analýzy, který zaznamenává různé podrobnosti o stránce.</span><span class="sxs-lookup"><span data-stu-id="54284-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="54284-117">Pokud chcete Podíváme se na vaší lokality statistiky, jste přihlášení k webu zprostředkovatele datové analýzy.</span><span class="sxs-lookup"><span data-stu-id="54284-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="54284-118">Potom můžete zobrazit nejrůznějším sestavy o vašem webu, jako jsou:</span><span class="sxs-lookup"><span data-stu-id="54284-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="54284-119">Počet zobrazení stránky pro jednotlivé stránky.</span><span class="sxs-lookup"><span data-stu-id="54284-119">The number of page views for individual pages.</span></span> <span data-ttu-id="54284-120">To odpovídá (přibližně) kolik lidí připojeni k serveru, a jsou nejoblíbenější stránky, které na váš web.</span><span class="sxs-lookup"><span data-stu-id="54284-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="54284-121">Jak dlouho uživatelé vynaložit na konkrétní stránky.</span><span class="sxs-lookup"><span data-stu-id="54284-121">How long people spend on specific pages.</span></span> <span data-ttu-id="54284-122">To vám sdělí takové věci, jako jestli je domovská stránka uchovávání zájmu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="54284-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="54284-123">Jaké weby uživatelé byly předtím, než se tak stalo vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="54284-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="54284-124">To vám pomůže pochopit zda provozu pochází z odkazy z hledání a tak dále.</span><span class="sxs-lookup"><span data-stu-id="54284-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="54284-125">Když uživatelé, kteří navštíví váš web a zůstanou jak dlouho.</span><span class="sxs-lookup"><span data-stu-id="54284-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="54284-126">Jaké země návštěvnících jsou z.</span><span class="sxs-lookup"><span data-stu-id="54284-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="54284-127">Jaké prohlížeče a operační systémy návštěvnících používají.</span><span class="sxs-lookup"><span data-stu-id="54284-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="54284-129">Přidat Analytics na stránku pomocí pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="54284-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="54284-130">Rozhraní ASP.NET Web Pages zahrnuje několik Pomocníci analytics (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, a `Analytics.GetStatCounterHtml`), můžete snadno spravovat fragmenty kódu JavaScript použít pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="54284-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="54284-131">Místo přijít na to, jak a kde uvést kód JavaScript, stačí je přidat na stránku pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="54284-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="54284-132">Veškeré informace, které budete muset zadat je váš název účtu, ID nebo sledování kód.</span><span class="sxs-lookup"><span data-stu-id="54284-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="54284-133">(Pro StatCounter, je také nutné zadat pár dalších hodnot.)</span><span class="sxs-lookup"><span data-stu-id="54284-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="54284-134">V tomto postupu vytvoříte rozložení stránky, která používá `GetGoogleHtml` pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="54284-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="54284-135">Pokud již máte účet s jedním z dalších zprostředkovatelů analýzy, můžete místo toho použijte tento účet a podle potřeby proveďte mírné úpravy.</span><span class="sxs-lookup"><span data-stu-id="54284-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="54284-136">Když vytvoříte účet analytics, je zaregistrovat adresu URL webu, který chcete sledovat.</span><span class="sxs-lookup"><span data-stu-id="54284-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="54284-137">Pokud testujete všechno, co v místním počítači, můžete nebude sledovat skutečná provoz (pouze provoz je můžete), takže nebudete moct záznamů a zobrazení statistiky lokality.</span><span class="sxs-lookup"><span data-stu-id="54284-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="54284-138">Ale tento postup ukazuje, jak přidat pomocná analytics na stránku.</span><span class="sxs-lookup"><span data-stu-id="54284-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="54284-139">Při publikování webu živý web odešle informace ke zprostředkovateli analytics.</span><span class="sxs-lookup"><span data-stu-id="54284-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="54284-140">Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste to ještě už přidali.</span><span class="sxs-lookup"><span data-stu-id="54284-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="54284-141">Vytvořte účet s Google Analytics a zaznamenejte název účtu.</span><span class="sxs-lookup"><span data-stu-id="54284-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="54284-142">Vytvoření rozložení stránky s názvem *Analytics.cshtml* a přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="54284-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="54284-143">Je nutné umístit volání `Analytics` pomocné rutiny v těle webové stránky (před `</body>` značka).</span><span class="sxs-lookup"><span data-stu-id="54284-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="54284-144">Prohlížeč, jinak nebude spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="54284-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="54284-145">Pokud používáte jiný analytics poskytovatele, použijte jednu z následujících pomocné místo:</span><span class="sxs-lookup"><span data-stu-id="54284-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="54284-146">(Yahoo)`@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="54284-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="54284-147">(StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="54284-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="54284-148">Nahraďte `myaccount` s názvem účtu, ID nebo sledování kód, který jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="54284-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="54284-149">Spusťte stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="54284-149">Run the page in the browser.</span></span> <span data-ttu-id="54284-150">(Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.)</span><span class="sxs-lookup"><span data-stu-id="54284-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="54284-151">V prohlížeči zobrazte zdrojový kód stránky.</span><span class="sxs-lookup"><span data-stu-id="54284-151">In the browser, view the page source.</span></span> <span data-ttu-id="54284-152">Budete moct najdete ve vykreslené analýzy kódu:</span><span class="sxs-lookup"><span data-stu-id="54284-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="54284-153">Přihlášení na webu Google Analytics a prozkoumat statistiku pro váš web.</span><span class="sxs-lookup"><span data-stu-id="54284-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="54284-154">Pokud používáte stránky na živý web, uvidíte položku protokoly najdete na stránku.</span><span class="sxs-lookup"><span data-stu-id="54284-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="54284-155">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="54284-155">Additional Resources</span></span>

- [<span data-ttu-id="54284-156">Google Analytics lokality</span><span class="sxs-lookup"><span data-stu-id="54284-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="54284-157">Yahoo! Analýza webu</span><span class="sxs-lookup"><span data-stu-id="54284-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="54284-158">StatCounter lokality</span><span class="sxs-lookup"><span data-stu-id="54284-158">StatCounter site</span></span>](http://statcounter.com/)
