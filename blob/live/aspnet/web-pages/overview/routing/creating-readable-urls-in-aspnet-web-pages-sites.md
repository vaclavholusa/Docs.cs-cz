---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: "Vytváření čitelné adresy URL v rozhraní ASP.NET Web Pages lokalit (Razor) | Microsoft Docs"
author: tfitzmac
description: "Tento článek popisuje směrování v webu technologie ASP.NET Web Pages (Razor) a jak to vám umožní používat adresy URL, které jsou čitelnější a lepší pro SEO. Co budete..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="8f400-104">Vytváření čitelné adresy URL v lokalitách rozhraní ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="8f400-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="8f400-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8f400-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8f400-106">Tento článek popisuje směrování v webu technologie ASP.NET Web Pages (Razor) a jak to vám umožní používat adresy URL, které jsou čitelnější a lepší pro SEO.</span><span class="sxs-lookup"><span data-stu-id="8f400-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="8f400-107">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="8f400-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="8f400-108">Jak technologie ASP.NET používá směrování vám umožňují používat více čitelný a dohledatelné adresy URL.</span><span class="sxs-lookup"><span data-stu-id="8f400-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8f400-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="8f400-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8f400-110">Rozhraní ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="8f400-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="8f400-111">V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="8f400-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="8f400-112">O směrování</span><span class="sxs-lookup"><span data-stu-id="8f400-112">About Routing</span></span>

<span data-ttu-id="8f400-113">Adresy URL pro stránky ve vaší lokalitě může mít dopad na tom, jak dobře funguje webu.</span><span class="sxs-lookup"><span data-stu-id="8f400-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="8f400-114">Adresa URL to je &quot;popisný&quot; můžete bylo snazší pro osoby využití serveru.</span><span class="sxs-lookup"><span data-stu-id="8f400-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="8f400-115">Může také pomoct s optimalizací vyhledávání (vyhledávací weby SEO) pro lokalitu.</span><span class="sxs-lookup"><span data-stu-id="8f400-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="8f400-116">Weby technologie ASP.NET, zahrnují možnost automaticky používat přátelské adresy URL.</span><span class="sxs-lookup"><span data-stu-id="8f400-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="8f400-117">Technologie ASP.NET umožňuje vytvářet smysluplný adresy URL, které popisují akce uživatelů místo právě odkazující na soubor na serveru.</span><span class="sxs-lookup"><span data-stu-id="8f400-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="8f400-118">Zvažte tyto adresy URL pro fiktivní blogu:</span><span class="sxs-lookup"><span data-stu-id="8f400-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="8f400-119">Porovnejte tyto adresy URL k těm, které jsou následující:</span><span class="sxs-lookup"><span data-stu-id="8f400-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="8f400-120">V první pár uživatel být vědět, že se zobrazí na blogu pomocí *blog.cshtml* stránky a by bylo nutné vytvořit řetězec dotazu, který získá správné kategorie nebo rozsah.</span><span class="sxs-lookup"><span data-stu-id="8f400-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="8f400-121">Druhá sada příklady je mnohem snazší pochopit a vytvářet.</span><span class="sxs-lookup"><span data-stu-id="8f400-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="8f400-122">Adresy URL v prvním příkladu také přejděte přímo na konkrétní soubor (*blog.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="8f400-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="8f400-123">Pokud z nějakého důvodu blogu byly přesunuty do jiné složky na serveru, nebo pokud byly na blogu přepsaná na jinou stránku, odkazy by být nesprávné.</span><span class="sxs-lookup"><span data-stu-id="8f400-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="8f400-124">Druhá sada adresy URL neodkazuje na konkrétní stránky, tak i v případě, že změní implementace blogu nebo umístění, adresy URL by nadále platné.</span><span class="sxs-lookup"><span data-stu-id="8f400-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="8f400-125">Na webových stránkách ASP.NET, můžete vytvořit příjemnější adresy URL stejně jako v předchozích příkladech vzhledem k tomu, že technologie ASP.NET používá *směrování*.</span><span class="sxs-lookup"><span data-stu-id="8f400-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="8f400-126">Směrování vytvoří logické mapování z adresy URL stránky (nebo stránky), která může splnění požadavku.</span><span class="sxs-lookup"><span data-stu-id="8f400-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="8f400-127">Vzhledem k tomu, že je mapování logické (ne fyzických, do určitého souboru), směrování poskytuje flexibilitu v tom, jak definovat adresy URL pro váš web.</span><span class="sxs-lookup"><span data-stu-id="8f400-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="8f400-128">Jak funguje směrování</span><span class="sxs-lookup"><span data-stu-id="8f400-128">How Routing Works</span></span>

<span data-ttu-id="8f400-129">Když ASP.NET zpracovává žádost, načte adresu URL určit, jak ho směrovat.</span><span class="sxs-lookup"><span data-stu-id="8f400-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="8f400-130">Technologie ASP.NET pokusí vyhledat jednotlivých segmentů adresy URL pro soubory na disku, budete zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="8f400-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="8f400-131">Pokud je nalezena shoda, nic zbývající v adrese URL je předána na stránku jako *informace o cestě*.</span><span class="sxs-lookup"><span data-stu-id="8f400-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="8f400-132">Představte si, že někdo požádá tuto adresu URL:</span><span class="sxs-lookup"><span data-stu-id="8f400-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="8f400-133">Hledání přejde takto:</span><span class="sxs-lookup"><span data-stu-id="8f400-133">The search goes like this:</span></span>

1. <span data-ttu-id="8f400-134">Je k dispozici soubor s cesta a název */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="8f400-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="8f400-135">Pokud ano, spusťte této stránce a předat žádné informace.</span><span class="sxs-lookup"><span data-stu-id="8f400-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="8f400-136">V opačném případě...</span><span class="sxs-lookup"><span data-stu-id="8f400-136">Otherwise ...</span></span>
2. <span data-ttu-id="8f400-137">Je k dispozici soubor s cesta a název */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="8f400-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="8f400-138">Pokud ano, spusťte tuto stránku a předejte hodnotu `c` k němu.</span><span class="sxs-lookup"><span data-stu-id="8f400-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="8f400-139">V opačném případě...</span><span class="sxs-lookup"><span data-stu-id="8f400-139">Otherwise …</span></span>
3. <span data-ttu-id="8f400-140">Je k dispozici soubor s cesta a název */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="8f400-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="8f400-141">Pokud ano, spusťte tuto stránku a předejte hodnotu `b/c` k němu.</span><span class="sxs-lookup"><span data-stu-id="8f400-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="8f400-142">Pokud hledání najít žádné přesně odpovídá pro *.cshtml* soubory v jejich zadané složky ASP.NET pokračuje v hledání pro tyto soubory pak:</span><span class="sxs-lookup"><span data-stu-id="8f400-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="8f400-143">*/a/b/c/default.cshtml* (žádné informace o cestě).</span><span class="sxs-lookup"><span data-stu-id="8f400-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="8f400-144">*/a/b/c/index.cshtml* (žádné informace o cestě).</span><span class="sxs-lookup"><span data-stu-id="8f400-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="8f400-145">Být jasné, požadavky na konkrétní stránky (to znamená, požadavků, které zahrnují *.cshtml* příponu názvu souboru) pracovat stejně, jako byste očekávali.</span><span class="sxs-lookup"><span data-stu-id="8f400-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="8f400-146">Žádost o jako `http://www.contoso.com/a/b.cshtml` spustí stránce *b.cshtml* stejně dobře.</span><span class="sxs-lookup"><span data-stu-id="8f400-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="8f400-147">Uvnitř stránky, můžete získat informace o cestě prostřednictvím stránky `UrlData` vlastnost, která je slovník.</span><span class="sxs-lookup"><span data-stu-id="8f400-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="8f400-148">Představte si, že máte soubor s názvem *ViewCustomers.cshtml* a váš web získá tuto žádost:</span><span class="sxs-lookup"><span data-stu-id="8f400-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="8f400-149">Jak je popsáno v výše uvedených pravidel, požadavek bude přejděte na stránku.</span><span class="sxs-lookup"><span data-stu-id="8f400-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="8f400-150">Uvnitř stránky, můžete použít k získání a zobrazení informací o cestě kód jako následující (v tomto případě hodnota &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="8f400-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="8f400-151">Protože směrování nebude zahrnovat názvy dokončení souborů, může být nejednoznačnosti Pokud máte stránky, které mají stejný název přípony názvu souborů, ale odlišným (například *MyPage.cshtml* a *MyPage.html*) .</span><span class="sxs-lookup"><span data-stu-id="8f400-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="8f400-152">Aby se zabránilo problémům s směrování, je nejvhodnější zajistit, že nemáte stránky ve vaší lokalitě, jejichž názvy se liší pouze v jejich rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8f400-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="8f400-153">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="8f400-153">Additional Resources</span></span>

<span data-ttu-id="8f400-154">[Služba WebMatrix - adresy URL, UrlData a směrování pro SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="8f400-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="8f400-155">Tuto položku blogu podle Karel Brind poskytuje některé další podrobnosti o tom, jak směrování funguje na webových stránkách ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8f400-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
