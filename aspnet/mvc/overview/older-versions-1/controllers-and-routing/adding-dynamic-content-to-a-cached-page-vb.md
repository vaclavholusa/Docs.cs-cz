---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Přidání dynamického obsahu do stránky v mezipaměti (VB) | Dokumentace Microsoftu
author: microsoft
description: Zjistěte, jak kombinovat dynamická a uložená v mezipaměti obsahu na stejné stránce. Substituce mezipaměti po umožňuje zobrazit dynamický obsah, jako je například o oznámení o inzerovaných programech banner...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 121a3a35c8255f1423d7008930315f76bbb8e8f9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752626"
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a><span data-ttu-id="513b8-104">Přidání dynamického obsahu do stránky v mezipaměti (VB)</span><span class="sxs-lookup"><span data-stu-id="513b8-104">Adding Dynamic Content to a Cached Page (VB)</span></span>
====================
<span data-ttu-id="513b8-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="513b8-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="513b8-106">Zjistěte, jak kombinovat dynamická a uložená v mezipaměti obsahu na stejné stránce.</span><span class="sxs-lookup"><span data-stu-id="513b8-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="513b8-107">Substituce mezipaměti po umožňuje zobrazit dynamický obsah, jako je například reklamy nebo příspěvků v rámci stránky, který má výstup do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="513b8-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="513b8-108">Výhod ukládání výstupu do mezipaměti, může výrazně zlepšit výkon aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="513b8-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="513b8-109">Na stránce může místo obnovení na stránce každého, když je zobrazení stránky vyžadováno, generovat jednou a uložit do mezipaměti v paměti pro více uživatelů.</span><span class="sxs-lookup"><span data-stu-id="513b8-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="513b8-110">Ale dojde k nějakému problému.</span><span class="sxs-lookup"><span data-stu-id="513b8-110">But there is a problem.</span></span> <span data-ttu-id="513b8-111">Co když budete potřebovat zobrazit dynamický obsah na stránce?</span><span class="sxs-lookup"><span data-stu-id="513b8-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="513b8-112">Představte si například, že chcete zobrazit na stránce proužkové reklamy.</span><span class="sxs-lookup"><span data-stu-id="513b8-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="513b8-113">Nechcete, aby oznámení o inzerovaném programu banner ukládat do mezipaměti tak, aby každý uživatel uvidí stejné oznámení o inzerovaném programu.</span><span class="sxs-lookup"><span data-stu-id="513b8-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="513b8-114">Nebylo by si peníze za tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="513b8-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="513b8-115">Naštěstí je jednoduché řešení.</span><span class="sxs-lookup"><span data-stu-id="513b8-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="513b8-116">Můžete využít funkci ASP.NET Framework, volá *substituce mezipaměti*.</span><span class="sxs-lookup"><span data-stu-id="513b8-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="513b8-117">Substituce mezipaměti po umožňuje nahradit dynamický obsah na stránce, která se má uložit do mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="513b8-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="513b8-118">Za normálních okolností při výstupu do mezipaměti na stránce s použitím &lt;OutputCache&gt; atribut, na stránce se uloží do mezipaměti na serveru a klienta (webový prohlížeč).</span><span class="sxs-lookup"><span data-stu-id="513b8-118">Normally, when you output cache a page by using the &lt;OutputCache&gt; attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="513b8-119">Při použití mezipaměti po nahrazení stránka je uložit do mezipaměti pouze na serveru.</span><span class="sxs-lookup"><span data-stu-id="513b8-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="513b8-120">Použití mezipaměti po nahrazení</span><span class="sxs-lookup"><span data-stu-id="513b8-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="513b8-121">Použití mezipaměti po nahrazení sestává ze dvou kroků.</span><span class="sxs-lookup"><span data-stu-id="513b8-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="513b8-122">Nejprve budete muset definovat metodu, která vrací řetězec představující dynamický obsah, který chcete zobrazit stránky v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="513b8-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="513b8-123">Pak zavolejte metodu HttpResponse.WriteSubstitution() vkládat dynamického obsahu do stránky.</span><span class="sxs-lookup"><span data-stu-id="513b8-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="513b8-124">Představte si například, že chcete náhodně zobrazení položek různé informační v stránky v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="513b8-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="513b8-125">Třída v informacích 1 poskytuje jedinou metodu s názvem RenderNews(), který náhodně vrátí jednu položku zprávy ze seznamu položek zpráv tři.</span><span class="sxs-lookup"><span data-stu-id="513b8-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="513b8-126">**Výpis 1 – Models\News.vb**</span><span class="sxs-lookup"><span data-stu-id="513b8-126">**Listing 1 – Models\News.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

<span data-ttu-id="513b8-127">Výhod substituce mezipaměti po volání metody HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="513b8-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="513b8-128">Metoda WriteSubstitution() nastaví kód k nahrazení oblast stránky v mezipaměti s dynamickým obsahem.</span><span class="sxs-lookup"><span data-stu-id="513b8-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="513b8-129">Metoda WriteSubstitution() slouží k zobrazení náhodných příspěvek v zobrazení na výpis 2.</span><span class="sxs-lookup"><span data-stu-id="513b8-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="513b8-130">**Listing 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="513b8-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

<span data-ttu-id="513b8-131">Metoda RenderNews se předá metodě WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="513b8-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="513b8-132">Všimněte si, že metoda RenderNews není volána.</span><span class="sxs-lookup"><span data-stu-id="513b8-132">Notice that the RenderNews method is not called.</span></span> <span data-ttu-id="513b8-133">Místo toho odkaz na metodu je předán WriteSubstitution() pomocí operátoru AddressOf.</span><span class="sxs-lookup"><span data-stu-id="513b8-133">Instead a reference to the method is passed to WriteSubstitution() with the help of the AddressOf operator.</span></span>

<span data-ttu-id="513b8-134">Zobrazení Index se uloží do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="513b8-134">The Index view is cached.</span></span> <span data-ttu-id="513b8-135">Zobrazení je vrácený řadič v informacích 3.</span><span class="sxs-lookup"><span data-stu-id="513b8-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="513b8-136">Všimněte si, že akce Index() je doplněn &lt;OutputCache&gt; atribut, který způsobí, že zobrazení indexu ukládat do mezipaměti po dobu 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="513b8-136">Notice that the Index() action is decorated with an &lt;OutputCache&gt; attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="513b8-137">**Výpis 3 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="513b8-137">**Listing 3 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

<span data-ttu-id="513b8-138">I v případě, že zobrazení Index se uloží do mezipaměti, se zobrazují různé náhodné informační položky při požadavku indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="513b8-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="513b8-139">Pokud budete požadovat indexovou stránku, času zobrazeného na stránce se nezmění po dobu 60 sekund (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="513b8-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="513b8-140">Skutečnost, že čas nezmění prokáže, že je do mezipaměti na stránce.</span><span class="sxs-lookup"><span data-stu-id="513b8-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="513b8-141">Nicméně obsah vloženy WriteSubstitution() změny metody – položky náhodné zpráv – s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="513b8-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="513b8-142">**Obrázek 1 – vkládá dynamické příspěvků v stránky v mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="513b8-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="513b8-144">Použití mezipaměti po nahrazení v pomocné metody</span><span class="sxs-lookup"><span data-stu-id="513b8-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="513b8-145">K zapouzdření volání metody WriteSubstitution() v rámci vlastní pomocné metody je snadný způsob, jak využít výhod mezipaměti po nahrazení.</span><span class="sxs-lookup"><span data-stu-id="513b8-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="513b8-146">Tento přístup je znázorněn ve pomocnou metodu v informacích 4.</span><span class="sxs-lookup"><span data-stu-id="513b8-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="513b8-147">**Část 4 – Helpers\AdHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="513b8-147">**Listing 4 – Helpers\AdHelper.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

<span data-ttu-id="513b8-148">Výpis 4 obsahuje modul jazyka Visual Basic, který poskytuje dvě metody: RenderBanner() a RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="513b8-148">Listing 4 contains a Visual Basic module that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="513b8-149">Metoda RenderBanner() představuje skutečný pomocnou metodu.</span><span class="sxs-lookup"><span data-stu-id="513b8-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="513b8-150">Tato metoda rozšiřuje standardní třídu ASP.NET MVC HtmlHelper, takže můžete volat Html.RenderBanner() v zobrazení stejně jako další metodu helper.</span><span class="sxs-lookup"><span data-stu-id="513b8-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="513b8-151">Metoda RenderBanner() volá metodu HttpResponse.WriteSubstitution() předávání metodu RenderBannerInternal() WriteSubsitution() metody.</span><span class="sxs-lookup"><span data-stu-id="513b8-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="513b8-152">Metoda RenderBannerInternal() je privátní metodu.</span><span class="sxs-lookup"><span data-stu-id="513b8-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="513b8-153">Tato metoda nebude vystavena jako metoda pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="513b8-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="513b8-154">Metoda RenderBannerInternal() náhodně vrátí jednu image banner oznámení o inzerovaném programu ze seznamu tři Image banner oznámení o inzerovaném programu.</span><span class="sxs-lookup"><span data-stu-id="513b8-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="513b8-155">Upravené zobrazení indexu v informacích 5 ukazuje, jak je možné používat RenderBanner() Pomocná metoda.</span><span class="sxs-lookup"><span data-stu-id="513b8-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="513b8-156">Všimněte si, že další &lt;% @ Import %&gt; direktiva je zahrnuté v horní části zobrazení pro import oboru názvů MvcApplication1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="513b8-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="513b8-157">Pokud opomenete importovat tento obor názvů, metoda RenderBanner() nezobrazí jako metoda na vlastnost ve formátu Html.</span><span class="sxs-lookup"><span data-stu-id="513b8-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="513b8-158">**Výpis 5 – Views\Home\Index.aspx (pomocí metody RenderBanner())**</span><span class="sxs-lookup"><span data-stu-id="513b8-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

<span data-ttu-id="513b8-159">Při žádosti o stránku zpracovanou zobrazení výpisu 5 různých banner oznámení se zobrazí spolu s každou žádostí (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="513b8-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="513b8-160">Na stránce se uloží do mezipaměti, ale inzerování banner se vloží dynamicky podle RenderBanner() Pomocná metoda.</span><span class="sxs-lookup"><span data-stu-id="513b8-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="513b8-161">**Obrázek 2 – Index zobrazení náhodných banner inzerování**</span><span class="sxs-lookup"><span data-stu-id="513b8-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="513b8-163">Souhrn</span><span class="sxs-lookup"><span data-stu-id="513b8-163">Summary</span></span>

<span data-ttu-id="513b8-164">Tento kurz vysvětluje, jak dynamicky aktualizovat obsah stránky v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="513b8-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="513b8-165">Jste zjistili, jak používat metodu HttpResponse.WriteSubstitution() umožňující dynamický obsah vložit do stránky v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="513b8-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="513b8-166">Také jste zjistili, jak k zapouzdření volání metody WriteSubstitution() v rámci metody pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="513b8-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="513b8-167">Využijte výhod ukládání do mezipaměti, kdykoli je to možné – může mít výrazný dopad na výkon webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="513b8-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="513b8-168">Jak je popsáno v tomto kurzu, můžete využít výhod ukládání do mezipaměti i v případě, že budete muset zobrazují dynamický obsah na stránkách.</span><span class="sxs-lookup"><span data-stu-id="513b8-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="513b8-169">[Předchozí](improving-performance-with-output-caching-vb.md)
> [další](creating-a-controller-vb.md)</span><span class="sxs-lookup"><span data-stu-id="513b8-169">[Previous](improving-performance-with-output-caching-vb.md)
[Next](creating-a-controller-vb.md)</span></span>
