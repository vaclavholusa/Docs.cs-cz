---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: "Vylepšení výkonu pomocí výstupní mezipaměti (VB) | Microsoft Docs"
author: microsoft
description: "V tomto kurzu zjistíte, jak může výrazně zvýšit výkon webových aplikací ASP.NET MVC a využívají k ukládání výstupu do mezipaměti. Můžete..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: 3bd4b6c3ac52577cbee451d2986f1167e441f0e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="improving-performance-with-output-caching-vb"></a><span data-ttu-id="68488-104">Vylepšení výkonu pomocí ukládání výstupu do mezipaměti (VB)</span><span class="sxs-lookup"><span data-stu-id="68488-104">Improving Performance with Output Caching (VB)</span></span>
====================
<span data-ttu-id="68488-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="68488-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="68488-106">V tomto kurzu zjistíte, jak může výrazně zvýšit výkon webových aplikací ASP.NET MVC a využívají k ukládání výstupu do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="68488-106">In this tutorial, you learn how you can dramatically improve the performance of your ASP.NET MVC web applications by taking advantage of output caching.</span></span> <span data-ttu-id="68488-107">Zjistíte, jak pro ukládání do mezipaměti výsledek vrácený z akce kontroleru, takže není nutné vytvořit každém nového uživatele vyvolá akci stejný obsah.</span><span class="sxs-lookup"><span data-stu-id="68488-107">You learn how to cache the result returned from a controller action so that the same content does not need to be created each and every time a new user invokes the action.</span></span>


<span data-ttu-id="68488-108">Cílem tohoto kurzu je vysvětlují, jak může výrazně zlepšit výkon aplikace ASP.NET MVC využitím výstupní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="68488-108">The goal of this tutorial is to explain how you can dramatically improve the performance of an ASP.NET MVC application by taking advantage of the output cache.</span></span> <span data-ttu-id="68488-109">Výstupní mezipaměti umožňuje ukládat do mezipaměti obsah vrácený akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="68488-109">The output cache enables you to cache the content returned by a controller action.</span></span> <span data-ttu-id="68488-110">Tímto způsobem stejný obsah se nemusí generovat každé, když je volána stejné akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="68488-110">That way, the same content does not need to be generated each and every time the same controller action is invoked.</span></span>

<span data-ttu-id="68488-111">Představte si například, že vaše aplikace ASP.NET MVC zobrazí seznam záznamů databáze v zobrazení s názvem Index.</span><span class="sxs-lookup"><span data-stu-id="68488-111">Imagine, for example, that your ASP.NET MVC application displays a list of database records in a view named Index.</span></span> <span data-ttu-id="68488-112">Za normálních okolností každé dobu, po kterou uživatel vyvolá akce kontroleru, který vrátí Index zobrazení, sadu záznamů databáze musí být načtena z databáze spuštěním dotazu na databázi.</span><span class="sxs-lookup"><span data-stu-id="68488-112">Normally, each and every time that a user invokes the controller action that returns the Index view, the set of database records must be retrieved from the database by executing a database query.</span></span>

<span data-ttu-id="68488-113">Pokud na druhé straně využít výhod výstupní mezipaměti můžete vyhnout, provádění dotazu na databázi pokaždé, když každý uživatel, vyvolá stejné akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="68488-113">If, on the other hand, you take advantage of the output cache then you can avoid executing a database query every time any user invokes the same controller action.</span></span> <span data-ttu-id="68488-114">Zobrazení mohou být načteny z mezipaměti namísto znovu generovány z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="68488-114">The view can be retrieved from the cache instead of being regenerated from the controller action.</span></span> <span data-ttu-id="68488-115">Ukládání do mezipaměti umožňuje můžete vyhnout provádění redundantní fungovat na serveru.</span><span class="sxs-lookup"><span data-stu-id="68488-115">Caching enables you to avoid performing redundant work on the server.</span></span>

#### <a name="enabling-output-caching"></a><span data-ttu-id="68488-116">Povolení ukládání výstupu do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="68488-116">Enabling Output Caching</span></span>

<span data-ttu-id="68488-117">Povolit ukládání výstupu do mezipaměti přidáním &lt;OutputCache&gt; atribut akce jednotlivých řadiče nebo třídu celý kontroler.</span><span class="sxs-lookup"><span data-stu-id="68488-117">You enable output caching by adding an &lt;OutputCache&gt; attribute to either an individual controller action or an entire controller class.</span></span> <span data-ttu-id="68488-118">Například řadič v výpis 1 zpřístupní akci s názvem Index().</span><span class="sxs-lookup"><span data-stu-id="68488-118">For example, the controller in Listing 1 exposes an action named Index().</span></span> <span data-ttu-id="68488-119">Výstup Index() akce se uloží do mezipaměti 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="68488-119">The output of the Index() action is cached for 10 seconds.</span></span>

<span data-ttu-id="68488-120">**Výpis 1 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="68488-120">**Listing 1 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]


<span data-ttu-id="68488-121">V Beta verzím rozhraní ASP.NET MVC, ukládání výstupu do mezipaměti nefunguje pro adresu URL podobnou [http://www.MySite.com/](http://www.mysite.com/).</span><span class="sxs-lookup"><span data-stu-id="68488-121">In the Beta versions of ASP.NET MVC, output caching does not work for a URL like [http://www.MySite.com/](http://www.mysite.com/).</span></span> <span data-ttu-id="68488-122">Místo toho musíte zadat adresu URL jako [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index).</span><span class="sxs-lookup"><span data-stu-id="68488-122">Instead, you must enter a URL like [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index).</span></span>


<span data-ttu-id="68488-123">Výpis 1 výstup Index() akce je do mezipaměti 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="68488-123">In Listing 1, the output of the Index() action is cached for 10 seconds.</span></span> <span data-ttu-id="68488-124">Pokud dáváte přednost, můžete zadat mnohem delší dobu mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="68488-124">If you prefer, you can specify a much longer cache duration.</span></span> <span data-ttu-id="68488-125">Například pokud chcete pro ukládání do mezipaměti výstup akce kontroleru pro jeden den je můžete zadat dobu trvání mezipaměti 86400 sekund (60 sekund \* 60 minut \* 24 hodin).</span><span class="sxs-lookup"><span data-stu-id="68488-125">For example, if you want to cache the output of a controller action for one day then you can specify a cache duration of 86400 seconds (60 seconds \* 60 minutes \* 24 hours).</span></span>

<span data-ttu-id="68488-126">Neexistuje žádná záruka, že obsahu bude do mezipaměti pro množství času, který určíte.</span><span class="sxs-lookup"><span data-stu-id="68488-126">There is no guarantee that content will be cached for the amount of time that you specify.</span></span> <span data-ttu-id="68488-127">Při paměťových prostředků začnou nízkou, čištění obsah v mezipaměti spustí automaticky.</span><span class="sxs-lookup"><span data-stu-id="68488-127">When memory resources become low, the cache starts evicting content automatically.</span></span>

<span data-ttu-id="68488-128">Domovské řadiče v výpis 1 vrátí zobrazení indexu v výpis 2.</span><span class="sxs-lookup"><span data-stu-id="68488-128">The Home controller in Listing 1 returns the Index view in Listing 2.</span></span> <span data-ttu-id="68488-129">Není co speciální o tomto zobrazení.</span><span class="sxs-lookup"><span data-stu-id="68488-129">There is nothing special about this view.</span></span> <span data-ttu-id="68488-130">Zobrazení indexu jednoduše zobrazí aktuální čas (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="68488-130">The Index view simply displays the current time (see Figure 1).</span></span>

<span data-ttu-id="68488-131">**Výpis 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="68488-131">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

<span data-ttu-id="68488-132">**Obrázek 1 – zobrazení indexu ukládaná do mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="68488-132">**Figure 1 – Cached Index view**</span></span>

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

<span data-ttu-id="68488-134">Pokud vícekrát vyvolání akce Index() zadáním adresy URL/Home nebo indexu na panelu Adresa svého prohlížeče a opakovaně klepnutím na tlačítko Aktualizovat nebo načtěte v prohlížeči, pak času zobrazeného v indexu zobrazení nedojde ke změně 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="68488-134">If you invoke the Index() action multiple times by entering the URL /Home/Index in the address bar of your browser and hitting the Refresh/Reload button in your browser repeatedly, then the time displayed by the Index view won't change for 10 seconds.</span></span> <span data-ttu-id="68488-135">Současně je zobrazit, protože zobrazení je uložený v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="68488-135">The same time is displayed because the view is cached.</span></span>

<span data-ttu-id="68488-136">Je důležité si uvědomit, že stejné zobrazení, se uloží do mezipaměti pro všechny uživatele, kteří navštíví vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="68488-136">It is important to understand that the same view is cached for everyone who visits your application.</span></span> <span data-ttu-id="68488-137">Každý, kdo vyvolá akci Index() získají uložené v mezipaměti stejnou verzi zobrazení indexu.</span><span class="sxs-lookup"><span data-stu-id="68488-137">Anyone who invokes the Index() action will get the same cached version of the Index view.</span></span> <span data-ttu-id="68488-138">To znamená, že je výrazně snížit množství práce, který webový server musíte provést k obsluze zobrazení indexu.</span><span class="sxs-lookup"><span data-stu-id="68488-138">This means that the amount of work that the web server must perform to serve the Index view is dramatically reduced.</span></span>

<span data-ttu-id="68488-139">Zobrazení v výpis 2 se stane něco skutečně jednoduché to.</span><span class="sxs-lookup"><span data-stu-id="68488-139">The view in Listing 2 happens to be doing something really simple.</span></span> <span data-ttu-id="68488-140">Zobrazení pouze zobrazuje aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="68488-140">The view just displays the current time.</span></span> <span data-ttu-id="68488-141">Však může stejně jako se snadno mezipaměti zobrazení, které se zobrazí sada záznamů databáze.</span><span class="sxs-lookup"><span data-stu-id="68488-141">However, you could just as easily cache a view that displays a set of database records.</span></span> <span data-ttu-id="68488-142">Sada záznamů databáze v takovém případě nemusí být načtena z databáze každé době vyvolání akce kontroleru, který vrací zobrazení.</span><span class="sxs-lookup"><span data-stu-id="68488-142">In that case, the set of database records would not need to be retrieved from the database each and every time the controller action that returns the view is invoked.</span></span> <span data-ttu-id="68488-143">Ukládání do mezipaměti může snížit množství práce, kterou musí provést webový server a databázový server.</span><span class="sxs-lookup"><span data-stu-id="68488-143">Caching can reduce the amount of work that both your web server and database server must perform.</span></span>

<span data-ttu-id="68488-144">Nepoužívejte stránce &lt;% @ OutputCache %&gt; direktivy v zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="68488-144">Don't use the page &lt;%@ OutputCache %&gt; directive in an MVC view.</span></span> <span data-ttu-id="68488-145">Tato direktiva je vykrvení přes ze světa webových formulářů a nesmí být použita v aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="68488-145">This directive is bleeding over from the Web Forms world and should not be used in an ASP.NET MVC application.</span></span> 

#### <a name="where-content-is-cached"></a><span data-ttu-id="68488-146">Kde je obsah uložený v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="68488-146">Where Content is Cached</span></span>

<span data-ttu-id="68488-147">Ve výchozím nastavení se při použití &lt;OutputCache&gt; atribut, obsah se uloží do mezipaměti ve třech umístěních: webový server, všechny proxy servery a webový prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="68488-147">By default, when you use the &lt;OutputCache&gt; attribute, content is cached in three locations: the web server, any proxy servers, and the web browser.</span></span> <span data-ttu-id="68488-148">Můžete řídit přesně, kde je obsah mezipaměti změnou vlastnost umístění &lt;OutputCache&gt; atribut.</span><span class="sxs-lookup"><span data-stu-id="68488-148">You can control exactly where content is cached by modifying the Location property of the &lt;OutputCache&gt; attribute.</span></span>

<span data-ttu-id="68488-149">Můžete nastavit vlastnosti umístění na některý z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="68488-149">You can set the Location property to any one of the following values:</span></span>

> <span data-ttu-id="68488-150">· Všechny</span><span class="sxs-lookup"><span data-stu-id="68488-150">· Any</span></span>
> 
> <span data-ttu-id="68488-151">· Klienta</span><span class="sxs-lookup"><span data-stu-id="68488-151">· Client</span></span>
> 
> <span data-ttu-id="68488-152">· Příjem dat</span><span class="sxs-lookup"><span data-stu-id="68488-152">· Downstream</span></span>
> 
> <span data-ttu-id="68488-153">· Server</span><span class="sxs-lookup"><span data-stu-id="68488-153">· Server</span></span>
> 
> <span data-ttu-id="68488-154">· None</span><span class="sxs-lookup"><span data-stu-id="68488-154">· None</span></span>
> 
> <span data-ttu-id="68488-155">· ServerAndClient</span><span class="sxs-lookup"><span data-stu-id="68488-155">· ServerAndClient</span></span>


<span data-ttu-id="68488-156">Ve výchozím umístění vlastnost má hodnotu žádné.</span><span class="sxs-lookup"><span data-stu-id="68488-156">By default, the Location property has the value Any.</span></span> <span data-ttu-id="68488-157">Existují však situace, ve kterých je vhodné do mezipaměti jenom v prohlížeči nebo pouze na serveru.</span><span class="sxs-lookup"><span data-stu-id="68488-157">However, there are situations in which you might want to cache only on the browser or only on the server.</span></span> <span data-ttu-id="68488-158">Například pokud jsou ukládání do mezipaměti informace, které je přizpůsobené pro každého uživatele, pak mezipaměti byste neměli ukládat informace na serveru.</span><span class="sxs-lookup"><span data-stu-id="68488-158">For example, if you are caching information that is personalized for each user then you should not cache the information on the server.</span></span> <span data-ttu-id="68488-159">Pokud jsou zobrazení různé informace pro různé uživatele by měla mezipaměti informace pouze na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="68488-159">If you are displaying different information to different users then you should cache the information only on the client.</span></span>

<span data-ttu-id="68488-160">Například řadič v výpis 3 zpřístupní akci s názvem GetName(), který vrací aktuální uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="68488-160">For example, the controller in Listing 3 exposes an action named GetName() that returns the current user name.</span></span> <span data-ttu-id="68488-161">Pokud konektor protokolů k webu a vyvolá akci GetName() akce vrátí řetězec "Hi konektoru".</span><span class="sxs-lookup"><span data-stu-id="68488-161">If Jack logs into the website and invokes the GetName() action then the action returns the string "Hi Jack".</span></span> <span data-ttu-id="68488-162">Pokud, následně Jill protokolů k webu a vyvolá akci GetName() pak uživatel taky dostane řetězec "Hi konektoru".</span><span class="sxs-lookup"><span data-stu-id="68488-162">If, subsequently, Jill logs into the website and invokes the GetName() action then she also will get the string "Hi Jack".</span></span> <span data-ttu-id="68488-163">Řetězec se po zkomprimování uloží na webovém serveru pro všechny uživatele konektoru původně vyvolá akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="68488-163">The string is cached on the web server for all users after Jack initially invokes the controller action.</span></span>

<span data-ttu-id="68488-164">**Výpis 3 – Controllers\BadUserController.vb**</span><span class="sxs-lookup"><span data-stu-id="68488-164">**Listing 3 – Controllers\BadUserController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

<span data-ttu-id="68488-165">S největší pravděpodobností řadiče v výpis 3 nefunguje požadovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="68488-165">Most likely, the controller in Listing 3 does not work the way that you want.</span></span> <span data-ttu-id="68488-166">Zobrazí se zpráva "Konektor Hi" k Jill nechcete.</span><span class="sxs-lookup"><span data-stu-id="68488-166">You don't want to display the message "Hi Jack" to Jill.</span></span>

<span data-ttu-id="68488-167">By nikdy mezipaměti přizpůsobený obsah v mezipaměti na serveru.</span><span class="sxs-lookup"><span data-stu-id="68488-167">You should never cache personalized content in the server cache.</span></span> <span data-ttu-id="68488-168">Můžete ale chtít mezipaměti přizpůsobený obsah do mezipaměti prohlížeče ke zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="68488-168">However, you might want to cache the personalized content in the browser cache to improve performance.</span></span> <span data-ttu-id="68488-169">Pokud mezipaměti obsahu v prohlížeči, a uživatel vyvolá stejné akce kontroleru vícekrát, můžete obsah načten z mezipaměti prohlížeče. namísto serveru.</span><span class="sxs-lookup"><span data-stu-id="68488-169">If you cache content in the browser, and a user invokes the same controller action multiple times, then the content can be retrieved from the browser cache instead of the server.</span></span>

<span data-ttu-id="68488-170">Upravené řadiče v výpis 4 ukládá do mezipaměti výstup GetName() akce.</span><span class="sxs-lookup"><span data-stu-id="68488-170">The modified controller in Listing 4 caches the output of the GetName() action.</span></span> <span data-ttu-id="68488-171">Ale obsah se uloží do mezipaměti jen v prohlížeči a není na serveru.</span><span class="sxs-lookup"><span data-stu-id="68488-171">However, the content is cached only on the browser and not on the server.</span></span> <span data-ttu-id="68488-172">Tímto způsobem, pokud více uživatelů vyvolání metody GetName(), každý uživatel získá své vlastní uživatelské jméno a uživatelské jméno není jiného uživatele.</span><span class="sxs-lookup"><span data-stu-id="68488-172">That way, when multiple users invoke the GetName() method, each person gets their own user name and not another person's user name.</span></span>

<span data-ttu-id="68488-173">**Výpis 4 – Controllers\UserController.vb**</span><span class="sxs-lookup"><span data-stu-id="68488-173">**Listing 4 – Controllers\UserController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

<span data-ttu-id="68488-174">Všimněte si, že &lt;OutputCache&gt; atribut v výpis 4 obsahuje vlastnost umístění nastaven na hodnotu OutputCacheLocation.Client.</span><span class="sxs-lookup"><span data-stu-id="68488-174">Notice that the &lt;OutputCache&gt; attribute in Listing 4 includes a Location property set to the value OutputCacheLocation.Client.</span></span> <span data-ttu-id="68488-175">&lt;OutputCache&gt; atribut také obsahuje vlastnost NoStore.</span><span class="sxs-lookup"><span data-stu-id="68488-175">The &lt;OutputCache&gt; attribute also includes a NoStore property.</span></span> <span data-ttu-id="68488-176">Vlastnost NoStore slouží k informování proxy servery a prohlížeče, by neměl uložených trvalá kopie obsahu v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="68488-176">The NoStore property is used to inform proxy servers and browsers that they should not store a permanent copy of the cached content.</span></span>

#### <a name="varying-the-output-cache"></a><span data-ttu-id="68488-177">Různých výstupní mezipaměti</span><span class="sxs-lookup"><span data-stu-id="68488-177">Varying the Output Cache</span></span>

<span data-ttu-id="68488-178">V některých případech můžete chtít různé verze velmi stejný obsah v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="68488-178">In some situations, you might want different cached versions of the very same content.</span></span> <span data-ttu-id="68488-179">Představte si například, že vytváříte stránky a podrobností.</span><span class="sxs-lookup"><span data-stu-id="68488-179">Imagine, for example, that you are creating a master/detail page.</span></span> <span data-ttu-id="68488-180">Stránky předlohy zobrazí seznam názvů film.</span><span class="sxs-lookup"><span data-stu-id="68488-180">The master page displays a list of movie titles.</span></span> <span data-ttu-id="68488-181">Když kliknete na název, zobrazí podrobnosti pro vybraného videa.</span><span class="sxs-lookup"><span data-stu-id="68488-181">When you click a title, you get details for the selected movie.</span></span>

<span data-ttu-id="68488-182">Můžete ukládat do mezipaměti stránce s podrobnostmi o, bude se zobrazí podrobnosti pro stejné film bez ohledu na to, které film kliknutí.</span><span class="sxs-lookup"><span data-stu-id="68488-182">If you cache the details page, then the details for the same movie will be displayed no matter which movie you click.</span></span> <span data-ttu-id="68488-183">Zobrazí se první film první uživatel vybírat pro všechny budoucí uživatele.</span><span class="sxs-lookup"><span data-stu-id="68488-183">The first movie selected by the first user will be displayed to all future users.</span></span>

<span data-ttu-id="68488-184">Tento problém lze vyřešit využívat výhod vlastnost VaryByParam &lt;OutputCache&gt; atribut.</span><span class="sxs-lookup"><span data-stu-id="68488-184">You can fix this problem by taking advantage of the VaryByParam property of the &lt;OutputCache&gt; attribute.</span></span> <span data-ttu-id="68488-185">Tato vlastnost umožňuje vytvářet různé verze uložené v mezipaměti velmi stejný obsah, pokud parametr formuláře nebo parametr řetězce dotazu se liší.</span><span class="sxs-lookup"><span data-stu-id="68488-185">This property enables you to create different cached versions of the very same content when a form parameter or query string parameter varies.</span></span>

<span data-ttu-id="68488-186">Například řadič v výpis 5 zpřístupní dvě akce s názvem Master() a Details().</span><span class="sxs-lookup"><span data-stu-id="68488-186">For example, the controller in Listing 5 exposes two actions named Master() and Details().</span></span> <span data-ttu-id="68488-187">Akce Master() vrátí seznam názvů film a Details() akce vrátí podrobnosti vybraného videa.</span><span class="sxs-lookup"><span data-stu-id="68488-187">The Master() action returns a list of movie titles and the Details() action returns the details for the selected movie.</span></span>

<span data-ttu-id="68488-188">**Výpis 5 – Controllers\MoviesController.vb**</span><span class="sxs-lookup"><span data-stu-id="68488-188">**Listing 5 – Controllers\MoviesController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

<span data-ttu-id="68488-189">Akce Master() zahrnuje vlastnost VaryByParam s hodnotou "žádný".</span><span class="sxs-lookup"><span data-stu-id="68488-189">The Master() action includes a VaryByParam property with the value "none".</span></span> <span data-ttu-id="68488-190">Vrátí se při zobrazení Master() akce je volána, stejnou verzi v mezipaměti hlavního serveru.</span><span class="sxs-lookup"><span data-stu-id="68488-190">When the Master() action is invoked, the same cached version of the Master view is returned.</span></span> <span data-ttu-id="68488-191">Parametrů formuláře nebo řetězce dotazu, které parametry jsou ignorovány (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="68488-191">Any form parameters or query string parameters are ignored (see Figure 2).</span></span>

<span data-ttu-id="68488-192">**Obrázek 2 – /Movies/Master zobrazení**</span><span class="sxs-lookup"><span data-stu-id="68488-192">**Figure 2 – The /Movies/Master view**</span></span>

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

<span data-ttu-id="68488-194">**Obrázek 3: zobrazení nebo filmy/podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="68488-194">**Figure 3 – The /Movies/Details view**</span></span>

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

<span data-ttu-id="68488-196">Akce Details() zahrnuje vlastnost VaryByParam s hodnotou "Id".</span><span class="sxs-lookup"><span data-stu-id="68488-196">The Details() action includes a VaryByParam property with the value "Id".</span></span> <span data-ttu-id="68488-197">Když různé hodnoty parametru Id jsou předána akce kontroleru, jsou generovány v mezipaměti různé verze zobrazení podrobností.</span><span class="sxs-lookup"><span data-stu-id="68488-197">When different values of the Id parameter are passed to the controller action, different cached versions of the Details view are generated.</span></span>

<span data-ttu-id="68488-198">Je důležité pochopit, že pomocí výsledky vlastnost VaryByParam v další ukládání do mezipaměti a není menší.</span><span class="sxs-lookup"><span data-stu-id="68488-198">It is important to understand that using the VaryByParam property results in more caching and not less.</span></span> <span data-ttu-id="68488-199">V mezipaměti jinou verzi zobrazení podrobností se vytvoří pro každou odlišnou verzi parametr Id.</span><span class="sxs-lookup"><span data-stu-id="68488-199">A different cached version of the Details view is created for each different version of the Id parameter.</span></span>

<span data-ttu-id="68488-200">Vlastnost VaryByParam můžete nastavit následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="68488-200">You can set the VaryByParam property to the following values:</span></span>

> <span data-ttu-id="68488-201">\*= Vytvořte jinou verzi v mezipaměti vždy, když se liší parametr řetězce formátu nebo dotazu.</span><span class="sxs-lookup"><span data-stu-id="68488-201">\* = Create a different cached version whenever a form or query string parameter varies.</span></span>
> 
> <span data-ttu-id="68488-202">žádné = nikdy vytvořit různé verze uložené v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="68488-202">none = Never create different cached versions</span></span>
> 
> <span data-ttu-id="68488-203">Středník seznam parametrů = vytvořit různé verze uložené v mezipaměti vždy, když některý z parametrů formuláře nebo dotaz, řetězec v seznamu je to různé.</span><span class="sxs-lookup"><span data-stu-id="68488-203">Semicolon list of parameters = Create different cached versions whenever any of the form or query string parameters in the list varies</span></span>


#### <a name="creating-a-cache-profile"></a><span data-ttu-id="68488-204">Vytvoření profilu mezipaměti</span><span class="sxs-lookup"><span data-stu-id="68488-204">Creating a Cache Profile</span></span>

<span data-ttu-id="68488-205">Jako alternativu ke konfiguraci vlastnosti výstupní mezipaměti a úprava vlastností &lt;OutputCache&gt; atribut, můžete vytvořit profil mezipaměti v souboru webové konfigurace (web.config).</span><span class="sxs-lookup"><span data-stu-id="68488-205">As an alternative to configuring output cache properties by modifying properties of the &lt;OutputCache&gt; attribute, you can create a cache profile in the web configuration (web.config) file.</span></span> <span data-ttu-id="68488-206">Vytvoření profilu mezipaměti v konfiguračním souboru webu nabízí několik výhod důležité.</span><span class="sxs-lookup"><span data-stu-id="68488-206">Creating a cache profile in the web configuration file offers a couple of important advantages.</span></span>

<span data-ttu-id="68488-207">Nejprve nakonfigurováním ukládání výstupu do mezipaměti v konfiguračním souboru webu můžete řídit, jak akce kontroleru ukládat obsah do mezipaměti v jednom centrálním místě.</span><span class="sxs-lookup"><span data-stu-id="68488-207">First, by configuring output caching in the web configuration file, you can control how controller actions cache content in one central location.</span></span> <span data-ttu-id="68488-208">Můžete vytvořit jeden profil mezipaměti a použít profil pro několik řadiče nebo akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="68488-208">You can create one cache profile and apply the profile to several controllers or controller actions.</span></span>

<span data-ttu-id="68488-209">Druhý můžete upravit konfigurační soubor webu bez nutnosti rekompilace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="68488-209">Second, you can modify the web configuration file without recompiling your application.</span></span> <span data-ttu-id="68488-210">Pokud je nutné zakázat ukládání do mezipaměti pro aplikaci, která již byla nasazena do produkčního prostředí, můžete jednoduše upravit profily mezipaměti definována v konfiguračním souboru webu.</span><span class="sxs-lookup"><span data-stu-id="68488-210">If you need to disable caching for an application that has already been deployed to production, then you can simply modify the cache profiles defined in the web configuration file.</span></span> <span data-ttu-id="68488-211">Všechny změny konfigurační soubor webu bude automaticky zjistil a použita.</span><span class="sxs-lookup"><span data-stu-id="68488-211">Any changes to the web configuration file will be detected automatically and applied.</span></span>

<span data-ttu-id="68488-212">Například &lt;ukládání do mezipaměti&gt; oddílu konfigurace webu na výpis 6 definuje profil mezipaměti s názvem Cache1Hour.</span><span class="sxs-lookup"><span data-stu-id="68488-212">For example, the &lt;caching&gt; web configuration section in Listing 6 defines a cache profile named Cache1Hour.</span></span> <span data-ttu-id="68488-213">&lt;Ukládání do mezipaměti&gt; část musí být v rámci &lt;system.web&gt; části souboru webové konfigurace.</span><span class="sxs-lookup"><span data-stu-id="68488-213">The &lt;caching&gt; section must appear within the &lt;system.web&gt; section of a web configuration file.</span></span>

<span data-ttu-id="68488-214">**Výpis 6 – část ukládání do mezipaměti pro soubor web.config**</span><span class="sxs-lookup"><span data-stu-id="68488-214">**Listing 6 – Caching section for web.config**</span></span>

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

<span data-ttu-id="68488-215">Řadič v výpis 7 znázorňuje, jak můžete použít profil Cache1Hour pro akce kontroleru s &lt;OutputCache&gt; atribut.</span><span class="sxs-lookup"><span data-stu-id="68488-215">The controller in Listing 7 illustrates how you can apply the Cache1Hour profile to a controller action with the &lt;OutputCache&gt; attribute.</span></span>

<span data-ttu-id="68488-216">**Výpis 7 – Controllers\ProfileController.vb**</span><span class="sxs-lookup"><span data-stu-id="68488-216">**Listing 7 – Controllers\ProfileController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

<span data-ttu-id="68488-217">Pokud vyvolání akce Index() vystavené řadiče v výpis 7 pak ve stejnou dobu, bude vrácen jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="68488-217">If you invoke the Index() action exposed by the controller in Listing 7 then the same time will be returned for 1 hour.</span></span>

#### <a name="summary"></a><span data-ttu-id="68488-218">Souhrn</span><span class="sxs-lookup"><span data-stu-id="68488-218">Summary</span></span>

<span data-ttu-id="68488-219">Ukládání výstupu do mezipaměti umožňuje velmi snadno metoda výrazně zlepšit výkon aplikací ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="68488-219">Output caching provides you with a very easy method of dramatically improving the performance of your ASP.NET MVC applications.</span></span> <span data-ttu-id="68488-220">V tomto kurzu jste zjistili, jak používat &lt;OutputCache&gt; atribut pro ukládání do mezipaměti výstup akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="68488-220">In this tutorial, you learned how to use the &lt;OutputCache&gt; attribute to cache the output of controller actions.</span></span> <span data-ttu-id="68488-221">Také jste zjistili, jak upravit vlastnosti &lt;OutputCache&gt; atribut třeba doba trvání a VaryByParam vlastnosti, které chcete upravit, jak získá obsah uložený v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="68488-221">You also learned how to modify properties of the &lt;OutputCache&gt; attribute such as the Duration and VaryByParam properties to modify how content gets cached.</span></span> <span data-ttu-id="68488-222">Nakonec jste zjistili, jak definovat profily mezipaměti v konfiguračním souboru webu.</span><span class="sxs-lookup"><span data-stu-id="68488-222">Finally, you learned how to define cache profiles in the web configuration file.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="68488-223">[Předchozí](understanding-action-filters-vb.md)
[další](adding-dynamic-content-to-a-cached-page-vb.md)</span><span class="sxs-lookup"><span data-stu-id="68488-223">[Previous](understanding-action-filters-vb.md)
[Next](adding-dynamic-content-to-a-cached-page-vb.md)</span></span>