---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: "Přidání dynamický obsah na stránku v mezipaměti (VB) | Microsoft Docs"
author: microsoft
description: "Zjistěte, jak kombinovat dynamické a uložené v mezipaměti obsahu na stejné stránce. Nahrazení po mezipaměti umožňuje zobrazit dynamický obsah, jako je například o banner oznámení o inzerovaném programu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: f07f4ecec36e71679dbc471b65f26d260349a07e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a><span data-ttu-id="27565-104">Přidání dynamický obsah na stránku v mezipaměti (VB)</span><span class="sxs-lookup"><span data-stu-id="27565-104">Adding Dynamic Content to a Cached Page (VB)</span></span>
====================
<span data-ttu-id="27565-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="27565-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="27565-106">Zjistěte, jak kombinovat dynamické a uložené v mezipaměti obsahu na stejné stránce.</span><span class="sxs-lookup"><span data-stu-id="27565-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="27565-107">Nahrazení po mezipaměti umožňuje zobrazit dynamický obsah, jako je například oznámení o inzerovaném programu informační zpráva nebo zprávy položky v rámci stránky, který se má byla výstupu do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="27565-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="27565-108">Využití výhod ukládání výstupu do mezipaměti, můžete výrazně zlepšit výkon aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="27565-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="27565-109">Místo obnovuje na stránce každé, když je zobrazení stránky vyžadováno, může generovat jednou stránky a uložené v mezipaměti v paměti pro více uživatelů.</span><span class="sxs-lookup"><span data-stu-id="27565-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="27565-110">Ale došlo k potížím.</span><span class="sxs-lookup"><span data-stu-id="27565-110">But there is a problem.</span></span> <span data-ttu-id="27565-111">Co když je třeba zobrazit dynamický obsah na stránce?</span><span class="sxs-lookup"><span data-stu-id="27565-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="27565-112">Představte si například, že chcete zobrazit oznámení informační zpráva na stránce.</span><span class="sxs-lookup"><span data-stu-id="27565-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="27565-113">Nechcete, aby inzerování banner ukládat do mezipaměti, aby každý uživatel vidí velmi stejné inzerování.</span><span class="sxs-lookup"><span data-stu-id="27565-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="27565-114">Tímto způsobem by nedávalo peníze!</span><span class="sxs-lookup"><span data-stu-id="27565-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="27565-115">Naštěstí je snadné řešení.</span><span class="sxs-lookup"><span data-stu-id="27565-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="27565-116">Můžete využít výhod funkce rozhraní ASP.NET volat *substituce mezipaměti*.</span><span class="sxs-lookup"><span data-stu-id="27565-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="27565-117">Nahrazení po mezipaměti umožňuje nahraďte dynamický obsah na stránce, který byl uložený do mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="27565-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="27565-118">Za normálních okolností při výstupu do mezipaměti na stránce pomocí &lt;OutputCache&gt; atribut, stránky se uloží do mezipaměti na serveru a klienta (webový prohlížeč).</span><span class="sxs-lookup"><span data-stu-id="27565-118">Normally, when you output cache a page by using the &lt;OutputCache&gt; attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="27565-119">Při použití mezipaměti po nahrazení stránky se uloží do mezipaměti pouze na serveru.</span><span class="sxs-lookup"><span data-stu-id="27565-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="27565-120">Použití mezipaměti po nahrazení</span><span class="sxs-lookup"><span data-stu-id="27565-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="27565-121">Použití mezipaměti po nahrazení vyžaduje dva kroky.</span><span class="sxs-lookup"><span data-stu-id="27565-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="27565-122">Nejprve musíte zadat metodu, která vrátí řetězec, který představuje dynamický obsah, který chcete zobrazit na stránce v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="27565-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="27565-123">Pak zavolejte metodu HttpResponse.WriteSubstitution() vložení dynamický obsah na stránku.</span><span class="sxs-lookup"><span data-stu-id="27565-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="27565-124">Představte si například, že chcete náhodně zobrazit různé příspěvků na stránce v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="27565-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="27565-125">Třída v výpis 1 zpřístupňuje jedinou metodu s názvem RenderNews(), který náhodně vrací jednu položku zpráv ze seznamu tři položky zprávy.</span><span class="sxs-lookup"><span data-stu-id="27565-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="27565-126">**Výpis 1 – Models\News.vb**</span><span class="sxs-lookup"><span data-stu-id="27565-126">**Listing 1 – Models\News.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

<span data-ttu-id="27565-127">Abyste mohli využívat mezipaměti po nahrazení, zavolejte metodu HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="27565-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="27565-128">Metoda WriteSubstitution() nastaví kód tak, aby oblasti v mezipaměti stránku nahraďte dynamický obsah.</span><span class="sxs-lookup"><span data-stu-id="27565-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="27565-129">Metoda WriteSubstitution() slouží k zobrazení položky náhodných zpráv v zobrazení v výpis 2.</span><span class="sxs-lookup"><span data-stu-id="27565-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="27565-130">**Výpis 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="27565-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

<span data-ttu-id="27565-131">Metoda RenderNews je předaný metodě WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="27565-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="27565-132">Všimněte si, že RenderNews metoda není volána.</span><span class="sxs-lookup"><span data-stu-id="27565-132">Notice that the RenderNews method is not called.</span></span> <span data-ttu-id="27565-133">Místo toho odkaz na metodu je předán WriteSubstitution() pomocí operátoru AddressOf.</span><span class="sxs-lookup"><span data-stu-id="27565-133">Instead a reference to the method is passed to WriteSubstitution() with the help of the AddressOf operator.</span></span>

<span data-ttu-id="27565-134">Zobrazení Index se uloží do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="27565-134">The Index view is cached.</span></span> <span data-ttu-id="27565-135">Zobrazení je vrácený řadič v výpis 3.</span><span class="sxs-lookup"><span data-stu-id="27565-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="27565-136">Všimněte si, že akce Index() opatřen s &lt;OutputCache&gt; atribut, který způsobí, že zobrazení indexu ukládat do mezipaměti po dobu 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="27565-136">Notice that the Index() action is decorated with an &lt;OutputCache&gt; attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="27565-137">**Výpis 3 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="27565-137">**Listing 3 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

<span data-ttu-id="27565-138">Přestože zobrazení Index se uloží do mezipaměti, položky různé náhodné zprávy se zobrazují, když požádáte o indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="27565-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="27565-139">Pokud budete požadovat indexovou stránku, nezmění se zobrazuje stránkou po dobu 60 sekund (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="27565-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="27565-140">Skutečnost, že čas nezmění prokáže, že stránky se uloží do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="27565-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="27565-141">Ale obsah vloženy změny metoda – položku náhodných zpráv – WriteSubstitution() spolu s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="27565-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="27565-142">**Obrázek 1 – vložení dynamické příspěvků na stránce v mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="27565-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="27565-144">Použití mezipaměti po nahrazení v pomocné metody</span><span class="sxs-lookup"><span data-stu-id="27565-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="27565-145">Snadný způsob, jak využít výhod nahrazení po mezipaměti je zapouzdření volání metody WriteSubstitution() v rámci vlastní Pomocná metoda.</span><span class="sxs-lookup"><span data-stu-id="27565-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="27565-146">Tento přístup je zobrazená metodou helper výpis 4.</span><span class="sxs-lookup"><span data-stu-id="27565-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="27565-147">**Výpis 4 – Helpers\AdHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="27565-147">**Listing 4 – Helpers\AdHelper.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

<span data-ttu-id="27565-148">Výpis 4 obsahuje modul jazyka Visual Basic, který zveřejňuje dvě metody: RenderBanner() a RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="27565-148">Listing 4 contains a Visual Basic module that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="27565-149">Metoda RenderBanner() představuje skutečný pomocnou metodu.</span><span class="sxs-lookup"><span data-stu-id="27565-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="27565-150">Tato metoda rozšiřuje standardní třída ASP.NET MVC HtmlHelper, tak, aby Html.RenderBanner() můžete volat v zobrazení stejně jako další metodu helper.</span><span class="sxs-lookup"><span data-stu-id="27565-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="27565-151">RenderBanner() metoda volá metodu HttpResponse.WriteSubstitution() předávání metodu RenderBannerInternal() metodu WriteSubsitution().</span><span class="sxs-lookup"><span data-stu-id="27565-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="27565-152">Metoda RenderBannerInternal() je soukromá metoda.</span><span class="sxs-lookup"><span data-stu-id="27565-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="27565-153">Tato metoda nebude zpřístupněná jako metodu helper.</span><span class="sxs-lookup"><span data-stu-id="27565-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="27565-154">Metoda RenderBannerInternal() náhodně vrátí jeden image banner oznámení o inzerovaném programu ze seznamu obrázků oznámení o inzerovaném programu tři hlavičky.</span><span class="sxs-lookup"><span data-stu-id="27565-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="27565-155">Upravené zobrazení indexu v výpis 5 ukazuje, jak je možné používat RenderBanner() pomocnou metodu.</span><span class="sxs-lookup"><span data-stu-id="27565-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="27565-156">Všimněte si, že další &lt;% @ Import %&gt; – direktiva je zahrnuta v horní části zobrazení pro import MvcApplication1.Helpers obor názvů.</span><span class="sxs-lookup"><span data-stu-id="27565-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="27565-157">Pokud neprovedete importovat tento obor názvů, metoda RenderBanner() nezobrazí jako metodu pro vlastnost Html.</span><span class="sxs-lookup"><span data-stu-id="27565-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="27565-158">**Výpis 5 – Views\Home\Index.aspx (s RenderBanner() metoda)**</span><span class="sxs-lookup"><span data-stu-id="27565-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

<span data-ttu-id="27565-159">Při žádosti o stránku pro vykreslení zobrazení v výpis 5 inzerování různých hlavička se zobrazí spolu s každou žádostí (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="27565-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="27565-160">Stránky se uloží do mezipaměti, ale hlavička oznámení je vloženy dynamicky RenderBanner() pomocnou metodu.</span><span class="sxs-lookup"><span data-stu-id="27565-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="27565-161">**Obrázek 2 – zobrazení Index zobrazení náhodných banner inzerování**</span><span class="sxs-lookup"><span data-stu-id="27565-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="27565-163">Souhrn</span><span class="sxs-lookup"><span data-stu-id="27565-163">Summary</span></span>

<span data-ttu-id="27565-164">V tomto kurzu vysvětlení, jak můžete dynamicky aktualizovat obsah v mezipaměti stránky.</span><span class="sxs-lookup"><span data-stu-id="27565-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="27565-165">Jste zjistili, jak lze pomocí této metody HttpResponse.WriteSubstitution() Povolit dynamický obsah vložit v stránky v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="27565-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="27565-166">Také jste zjistili, jak má být zapouzdřena volání metody WriteSubstitution() v rámci metodu helper HTML.</span><span class="sxs-lookup"><span data-stu-id="27565-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="27565-167">Využít výhod ukládání do mezipaměti, kdykoli je to možné – ho může mít výrazný dopad na výkon webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="27565-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="27565-168">Jak je popsáno v tomto kurzu, můžete využít výhod ukládání do mezipaměti i v případě, že je třeba zobrazit dynamický obsah na stránkách.</span><span class="sxs-lookup"><span data-stu-id="27565-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="27565-169">[Předchozí](improving-performance-with-output-caching-vb.md)
[další](creating-a-controller-vb.md)</span><span class="sxs-lookup"><span data-stu-id="27565-169">[Previous](improving-performance-with-output-caching-vb.md)
[Next](creating-a-controller-vb.md)</span></span>
