---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: "Používat k poskytování dynamické aktualizace AJAX | Microsoft Docs"
author: microsoft
description: "Krok 10 implementuje podporu pro přihlášeného uživatele k zasílání zpráv rysy jejich zájmu účastí večeři, pomocí integrované v rámci podrobností večeři přístup na základě Ajax na..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7b75f8c6cf08112eb77d1a9a40222ed1425ef3a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="100f8-103">Používat k poskytování dynamické aktualizace AJAX</span><span class="sxs-lookup"><span data-stu-id="100f8-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="100f8-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="100f8-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="100f8-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="100f8-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="100f8-106">Toto je krok 10 bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="100f8-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="100f8-107">Krok 10 implementuje podporu pro přihlášeného uživatele k zasílání zpráv rysy jejich zájmu účastí večeři, pomocí integrované v rámci stránce s podrobnostmi o večeři přístup na základě Ajax na.</span><span class="sxs-lookup"><span data-stu-id="100f8-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="100f8-108">Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.</span><span class="sxs-lookup"><span data-stu-id="100f8-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="100f8-109">NerdDinner krok 10: Přijímá AJAX povolení RSVPs</span><span class="sxs-lookup"><span data-stu-id="100f8-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="100f8-110">Teď umožňuje implementovat podporu pro uživatele přihlášeného k RSVP jejich zájmu účastí večeři na.</span><span class="sxs-lookup"><span data-stu-id="100f8-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="100f8-111">Jsme budete to umožňují přístup na základě AJAX integrovaná v rámci dané stránky večeři podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="100f8-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="100f8-112">Označující, zda je na které odpověděl uživatele</span><span class="sxs-lookup"><span data-stu-id="100f8-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="100f8-113">Uživatelé můžou navštívit */Dinners/podrobnosti / [id*] zobrazit podrobnosti o konkrétní večeři adresa URL:</span><span class="sxs-lookup"><span data-stu-id="100f8-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="100f8-114">Details() je implementována metoda akce takto:</span><span class="sxs-lookup"><span data-stu-id="100f8-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="100f8-115">Naše prvním krokem k implementaci podpory zasílání zpráv rysy bude přidat metodu helper "IsUserRegistered(username)" naše objekt večeři (v rámci Dinner.cs třídu, kterou jsme vytvořili výše).</span><span class="sxs-lookup"><span data-stu-id="100f8-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="100f8-116">Tato pomocná metoda vrátí hodnotu PRAVDA nebo NEPRAVDA v závislosti na tom, zda uživatel je aktuálně na které odpověděl večeře:</span><span class="sxs-lookup"><span data-stu-id="100f8-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="100f8-117">Naše Details.aspx zobrazit šablonu k zobrazení odpovídající zprávu, která udává, zda je uživatel zaregistrován nebo není pro událost jsme můžete přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="100f8-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="100f8-118">A teď Pokud uživatel navštíví večeři, jsou registrované pro se zobrazí tato zpráva:</span><span class="sxs-lookup"><span data-stu-id="100f8-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="100f8-119">A při návštěvě večeři, že nejsou registrované budete najdete v tématu následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="100f8-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="100f8-120">Implementace metody akce registrace</span><span class="sxs-lookup"><span data-stu-id="100f8-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="100f8-121">Nyní Pojďme přidat funkce, které jsou nutné k povolení uživatelům k zasílání zpráv rysy večeře na stránce podrobností.</span><span class="sxs-lookup"><span data-stu-id="100f8-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="100f8-122">Chcete-li tuto funkci implementovat, vytvoříme novou třídu "RSVPController" pravým tlačítkem myši na adresář \Controllers a zvolením Add -&gt;příkazu nabídky řadiče.</span><span class="sxs-lookup"><span data-stu-id="100f8-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="100f8-123">Jsme budete implementovat metodu akce "Register" v rámci novou třídu RSVPController, která přebírá id večeře jako argument, načte příslušný objekt večeři, zkontroluje, pokud právě přihlášeného uživatele v seznamu uživatelů, kteří zaregistrovali pro něj a pokud není přidá objekt zasílání zpráv rysy pro ně:</span><span class="sxs-lookup"><span data-stu-id="100f8-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="100f8-124">Všimněte si výše jak jednoduchým řetězcem se vrátí jako výstup metody akce.</span><span class="sxs-lookup"><span data-stu-id="100f8-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="100f8-125">Jsme může vložených, tato zpráva v šabloně na zobrazení – ale vzhledem k tomu, že je proto malá právě použijeme pomocnou metodu Content() na základní třídy kontroleru a vrátí zprávu řetězec jako výše.</span><span class="sxs-lookup"><span data-stu-id="100f8-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="100f8-126">Volání metody akce RSVPForEvent pomocí rozhraní AJAX</span><span class="sxs-lookup"><span data-stu-id="100f8-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="100f8-127">Použijeme AJAX k vyvolání metody akce registrace z našich zobrazení podrobností.</span><span class="sxs-lookup"><span data-stu-id="100f8-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="100f8-128">Implementace to je velmi snadné.</span><span class="sxs-lookup"><span data-stu-id="100f8-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="100f8-129">Nejprve přidáme dvě reference knihovny skriptu:</span><span class="sxs-lookup"><span data-stu-id="100f8-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="100f8-130">První knihovny odkazuje na základní knihovny ASP.NET AJAX klientský skript.</span><span class="sxs-lookup"><span data-stu-id="100f8-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="100f8-131">Tento soubor obsahuje základní funkce AJAX na straně klienta a je přibližně 24 kb velikostí (komprimované).</span><span class="sxs-lookup"><span data-stu-id="100f8-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="100f8-132">Druhý knihovna obsahuje nástroj funkce, které integrovat s ASP.NET MVC předdefinované AJAX pomocné metody (které použijeme krátce).</span><span class="sxs-lookup"><span data-stu-id="100f8-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="100f8-133">Můžeme pak aktualizace zobrazení Kód šablony, kterou jsme přidali dříve tak, aby místo outputing zprávu "Je nejsou registrované pro tuto událost" jsme místo vykreslit odkaz při stisknutí. provádí volání AJAX, která volá metodu akce naše RSVPForEvent na zasílání zpráv rysy kontroleru a RSVPs uživatele:</span><span class="sxs-lookup"><span data-stu-id="100f8-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="100f8-134">Pomocnou metodu Ajax.ActionLink() používá výše je integrovaný do technologie ASP.NET MVC a je podobný Html.ActionLink() pomocnou metodu s tím rozdílem, že namísto provádění standardní navigační umožňuje volání AJAX na metodu akce při kliknutí na odkaz.</span><span class="sxs-lookup"><span data-stu-id="100f8-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="100f8-135">Vyšší jsme se volání metody akce "Register" na "Zasílání zpráv rysy" řadiči a předáním DinnerID jako parametr "id".</span><span class="sxs-lookup"><span data-stu-id="100f8-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="100f8-136">Konečný parametr AjaxOptions jsme předávání označuje, že chceme trvat obsah vrátila z metody akce a aktualizovat HTML &lt;div&gt; elementu na stránce, jejíž id je "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="100f8-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="100f8-137">A teď když uživatel prochází večeři nejsou registrované pro ještě se zobrazí odkaz na zasílání zpráv rysy pro ni:</span><span class="sxs-lookup"><span data-stu-id="100f8-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="100f8-138">Pokud kliknou na odkaz "Pro tuto událost zasílání zpráv rysy" budete provádění volání AJAX na metodu akce registrace na řadiči zasílání zpráv rysy, a po dokončení se zobrazí zprávu aktualizované jako níže:</span><span class="sxs-lookup"><span data-stu-id="100f8-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="100f8-139">Je skutečně lightweight šířku pásma sítě a provoz související se situací při provádění tohoto volání AJAX.</span><span class="sxs-lookup"><span data-stu-id="100f8-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="100f8-140">Když uživatel klikne na odkaz "Pro tuto událost zasílání zpráv rysy", malé síťový požadavek HTTP POST přišla */Dinners/Register/1* adresa URL, která vypadá jako níže v drátové síti:</span><span class="sxs-lookup"><span data-stu-id="100f8-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="100f8-141">A odpověď z našich metoda akce registrace je jednoduše:</span><span class="sxs-lookup"><span data-stu-id="100f8-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="100f8-142">Toto volání lightweight je rychlá a bude fungovat i přes pomalé síti.</span><span class="sxs-lookup"><span data-stu-id="100f8-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="100f8-143">Přidání jQuery animace</span><span class="sxs-lookup"><span data-stu-id="100f8-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="100f8-144">Funkce AJAX jsme implementováno funguje dobře a rychlé.</span><span class="sxs-lookup"><span data-stu-id="100f8-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="100f8-145">Někdy tomu může dojít tak rychle, ale, že uživatel nemusí Všimněte si, že odkaz zasílání zpráv rysy nahrazen novým textem.</span><span class="sxs-lookup"><span data-stu-id="100f8-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="100f8-146">Chcete-li výsledek trochu zřejmější jsme přidat jednoduchou animaci k upozornění zpráva aktualizace.</span><span class="sxs-lookup"><span data-stu-id="100f8-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="100f8-147">Výchozí šablona projektu ASP.NET MVC zahrnuje jQuery – knihovna JavaScript vynikající (a velmi oblíbených) s otevřeným zdrojem, který taky podporuje Microsoft.</span><span class="sxs-lookup"><span data-stu-id="100f8-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="100f8-148">jQuery poskytuje celou řadu funkcí, včetně dobrý model HTML DOM výběr a efekty knihovny.</span><span class="sxs-lookup"><span data-stu-id="100f8-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="100f8-149">Použít jQuery nejprve přidáme odkaz na skript k němu.</span><span class="sxs-lookup"><span data-stu-id="100f8-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="100f8-150">Vzhledem k tomu, že budeme používat jQuery v rámci různých místech v rámci našeho webu, přidáme odkaz na skript v rámci naší soubor Site.master hlavní stránky tak, aby všechny stránky můžete používat.</span><span class="sxs-lookup"><span data-stu-id="100f8-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="100f8-151">*Tip: Ujistěte se, že jste nainstalovali opravu hotfix JavaScript intellisense pro VS 2008 SP1, která umožňuje bohatší podporu technologie intellisense pro soubory JavaScript (včetně jQuery). Si můžete stáhnout z: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="100f8-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="100f8-152">Kód napsaný pomocí JQuery často používá globální $ ()"metodu JavaScript, který načte jeden nebo více elementů HTML pomocí selektor šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="100f8-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="100f8-153">Například *$("#rsvpmsg")* vybere libovolný prvek HTML s id rsvpmsg, zatímco *$(".something")* by vybrat všechny elementy s "něco co" šablon stylů CSS název třídy.</span><span class="sxs-lookup"><span data-stu-id="100f8-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="100f8-154">Je také možné zapsat složitější dotazy jako "vrátí všechny zaškrtnuté přepínačů" pomocí dotazu pro výběr jako: *$("vstup [@type= přepínač] [@checked]")*.</span><span class="sxs-lookup"><span data-stu-id="100f8-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="100f8-155">Po dokončení výběru prvky, můžete volat metody na jejich provedení akce, jako je skrytí: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="100f8-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="100f8-156">Pro náš scénář zasílání zpráv rysy budeme definovat jednoduchý funkce jazyka JavaScript s názvem "AnimateRSVPMessage", která vybere položku "rsvpmsg" &lt;div&gt; a animuje velikost jeho textového obsahu.</span><span class="sxs-lookup"><span data-stu-id="100f8-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="100f8-157">Níže uvedeného kódu spustí malé text a pak příčiny pro zvýšení přes 400 milisekund období:</span><span class="sxs-lookup"><span data-stu-id="100f8-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="100f8-158">Jsme můžete pak navázání této funkce JavaScript, která má být volána po naše volání AJAX úspěšně dokončí pomocí předání názvu naše Pomocná metoda Ajax.ActionLink() (prostřednictvím AjaxOptions "OnSuccess" události vlastnost):</span><span class="sxs-lookup"><span data-stu-id="100f8-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="100f8-159">A teď až po kliknutí na odkaz "Pro tuto událost zasílání zpráv rysy" a naše volání AJAX dokončí úspěšně, obsah zprávy odeslané zpět bude animace růst velké:</span><span class="sxs-lookup"><span data-stu-id="100f8-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="100f8-160">Kromě událost "OnSuccess", zpřístupní objekt AjaxOptions OnBegin OnFailure – a onComplete – události, které může zpracovat (spolu s celou řadu dalších vlastností a užitečné možnosti).</span><span class="sxs-lookup"><span data-stu-id="100f8-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="100f8-161">Vyčištění - Refaktorovat out RSVP částečné zobrazení</span><span class="sxs-lookup"><span data-stu-id="100f8-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="100f8-162">Naše podrobnosti zobrazit šablonu spouští získat trochu dlouhý, které přesčasová znamená, že trochu těžší pochopit.</span><span class="sxs-lookup"><span data-stu-id="100f8-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="100f8-163">K vylepšení čitelnosti kódu, můžeme nakonec vytvořením částečné zobrazení – RSVPStatus.ascx – zapouzdřující všechny kód zasílání zpráv rysy zobrazení pro naší stránce s podrobnostmi o.</span><span class="sxs-lookup"><span data-stu-id="100f8-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="100f8-164">Jsme to můžete udělat pravým tlačítkem na složku \Views\Dinners a pak vyberete Add -&gt;příkazu v nabídce zobrazení.</span><span class="sxs-lookup"><span data-stu-id="100f8-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="100f8-165">Jsme si ho trvat objekt večeři jako jeho ViewModel silného typu.</span><span class="sxs-lookup"><span data-stu-id="100f8-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="100f8-166">Jsme můžete pak zkopírujte a vložte obsah zasílání zpráv rysy z našich zobrazením Details.aspx do ní.</span><span class="sxs-lookup"><span data-stu-id="100f8-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="100f8-167">Když jsme tento krok, umožňuje taky vytvořit jinou částečné zobrazení – EditAndDeleteLinks.ascx -, který zapouzdřuje kód naše upravit a odstranit zobrazení odkaz.</span><span class="sxs-lookup"><span data-stu-id="100f8-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="100f8-168">Také jsme budete mít trvat objekt večeři jako jeho ViewModel silného typu a zkopírujte a vložte logiky upravit a odstranit z našich zobrazením Details.aspx do ní.</span><span class="sxs-lookup"><span data-stu-id="100f8-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="100f8-169">Naše podrobnosti a zobrazit šablonu poté zahrnout pouze dvě volání metod Html.RenderPartial() v dolní části:</span><span class="sxs-lookup"><span data-stu-id="100f8-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="100f8-170">Díky tomu kód čisticí ke čtení a údržbu.</span><span class="sxs-lookup"><span data-stu-id="100f8-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="100f8-171">Další krok</span><span class="sxs-lookup"><span data-stu-id="100f8-171">Next Step</span></span>

<span data-ttu-id="100f8-172">Nyní podíváme, jak jsme můžete použít i další AJAX a přidat podporu interaktivní mapování do naší aplikace.</span><span class="sxs-lookup"><span data-stu-id="100f8-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="100f8-173">[Předchozí](secure-applications-using-authentication-and-authorization.md)
[další](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="100f8-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
