---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Použití jazyka AJAX k dynamickým aktualizacím | Dokumentace Microsoftu
author: microsoft
description: Krok 10 implementuje podporu pro přihlášeného uživatele RSVP jejich zájmu o účast na akci dinner, pomocí přístupu na základě Ajax integrovaný v rámci podrobností dinner...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: e902881d3dab6a902cb747a197a32f317d199723
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752424"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="c9822-103">Použití jazyka AJAX k dynamickým aktualizacím</span><span class="sxs-lookup"><span data-stu-id="c9822-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="c9822-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c9822-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="c9822-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="c9822-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="c9822-106">Toto je krok 10 bezplatného [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="c9822-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="c9822-107">Krok 10 implementuje podporu pro přihlášeného uživatele RSVP jejich zájmu o účast na akci dinner, pomocí přístupu na základě Ajax integrovaný v rámci stránce s podrobnostmi o dinner.</span><span class="sxs-lookup"><span data-stu-id="c9822-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="c9822-108">Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.</span><span class="sxs-lookup"><span data-stu-id="c9822-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="c9822-109">NerdDinner krok 10: Přijímá AJAX povolení RSVPs</span><span class="sxs-lookup"><span data-stu-id="c9822-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="c9822-110">Teď můžeme implementovat podporu pro uživatele přihlášený potvrďte svou účast ještě jejich zájmu o účast webu dinner.</span><span class="sxs-lookup"><span data-stu-id="c9822-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="c9822-111">Jsme vám umožňují přístup na základě AJAX integrovaný v rámci stránce s podrobnostmi o dinner.</span><span class="sxs-lookup"><span data-stu-id="c9822-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="c9822-112">Označující, zda je uživatel RSVP'd</span><span class="sxs-lookup"><span data-stu-id="c9822-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="c9822-113">Uživatelé můžou navštívit */Dinners/podrobnosti / [id*] adresa URL, které chcete zobrazit podrobnosti o konkrétní společnosti dinner:</span><span class="sxs-lookup"><span data-stu-id="c9822-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="c9822-114">Details() implementované metody akce takto:</span><span class="sxs-lookup"><span data-stu-id="c9822-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="c9822-115">Naše prvním krokem k implementaci podpory RSVP budete moct přidat metodu helper "IsUserRegistered(username)" naše společnost Dinner objektu (v rámci Dinner.cs částečné třídy, které jsme vytvořili výše).</span><span class="sxs-lookup"><span data-stu-id="c9822-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="c9822-116">Tato pomocná metoda vrátí hodnotu PRAVDA nebo NEPRAVDA v závislosti na tom, jestli je uživatel aktuálně RSVP'd pro společnosti Dinner:</span><span class="sxs-lookup"><span data-stu-id="c9822-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="c9822-117">Naše Details.aspx zobrazit šablonu k zobrazení odpovídající zprávu označující, zda je uživatel zaregistrován nebo není pro událost jsme můžete přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="c9822-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="c9822-118">A teď když uživatel navštíví Dinner jsou registrované pro zobrazí tato zpráva:</span><span class="sxs-lookup"><span data-stu-id="c9822-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="c9822-119">A při návštěvě Dinner nejsou zaregistrovány pro zobrazí následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="c9822-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="c9822-120">Implementace Registrovací metoda akce</span><span class="sxs-lookup"><span data-stu-id="c9822-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="c9822-121">Přidejme funkci nutná pro povolení uživatelů ze stránky podrobností na reakce večeře nyní.</span><span class="sxs-lookup"><span data-stu-id="c9822-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="c9822-122">K provedení vytvoříme novou třídu "RSVPController" pravým tlačítkem myši na adresáři \Controllers a zvolením přidat -&gt;příkazu nabídky Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="c9822-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="c9822-123">Jsme budete implementovat metodu akce "Register" v rámci nové RSVPController třídu, která přijímá id večeře jako argument, načte příslušný objekt Dinner kontroluje, zda přihlášený uživatel je aktuálně v seznamu uživatelů, kteří zaregistrovali, a pokud není přidá objekt RSVP pro ně:</span><span class="sxs-lookup"><span data-stu-id="c9822-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="c9822-124">Všimněte si, že nad jak jednoduchým řetězcem se vrátí jako výstup metody akce.</span><span class="sxs-lookup"><span data-stu-id="c9822-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="c9822-125">Jsme může vložit, tato zpráva v rámci zobrazení šablony – ale protože je to small právě použijeme Content() pomocnou metodu na základní třídu kontroleru a vrátit zprávu řetězce, jako je výše.</span><span class="sxs-lookup"><span data-stu-id="c9822-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="c9822-126">Volání metody akce RSVPForEvent pomocí rozhraní AJAX</span><span class="sxs-lookup"><span data-stu-id="c9822-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="c9822-127">Použijeme AJAX k vyvolání metody akce registrace z našich zobrazení podrobností.</span><span class="sxs-lookup"><span data-stu-id="c9822-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="c9822-128">Implementace to je poměrně snadné.</span><span class="sxs-lookup"><span data-stu-id="c9822-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="c9822-129">Nejdřív přidáme dva odkazy na knihovnu skriptu:</span><span class="sxs-lookup"><span data-stu-id="c9822-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="c9822-130">První knihovny odkazuje na základní knihovny ASP.NET AJAX na straně klienta skriptů.</span><span class="sxs-lookup"><span data-stu-id="c9822-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="c9822-131">Tento soubor je přibližně 24 kb (komprimované) a obsahuje základní funkce jazyka AJAX na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c9822-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="c9822-132">Druhý knihovna obsahuje funkce nástrojů, které se integrují s ASP.NET MVC integrované AJAX pomocné metody (které použijeme za chvíli).</span><span class="sxs-lookup"><span data-stu-id="c9822-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="c9822-133">Můžeme pak aktualizace kód zobrazit šablonu, kterou jsme přidali dříve, tak, aby místo outputing zprávu "Jste se zaregistrovali pro tuto událost", můžeme místo vykreslení odkaz, který při vložení provede volání AJAX, která volá metodu naše RSVPForEvent akce v kontroleru reakce a RSVPs uživatele:</span><span class="sxs-lookup"><span data-stu-id="c9822-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="c9822-134">Pomocná metoda Ajax.ActionLink() využité nad je integrovaná do ASP.NET MVC a je podobný Html.ActionLink() pomocnou metodu s tím rozdílem, takže nemusíte provádět standardní navigace je volání AJAX na metodu akce po kliknutí na odkaz.</span><span class="sxs-lookup"><span data-stu-id="c9822-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="c9822-135">Výše jsme volání metody akce "Register" u "RSVP" kontroleru a předáním DinnerID jako parametr "id".</span><span class="sxs-lookup"><span data-stu-id="c9822-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="c9822-136">Poslední parametr AjaxOptions jsme prochází označuje, že má být obsah vrácené z metody akce a aktualizovat kód HTML &lt;div&gt; element na stránce, jehož id je "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="c9822-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="c9822-137">A teď když uživatel prochází večeři nejsou registrovány pro ještě zobrazí odkaz na RSVP pro něj:</span><span class="sxs-lookup"><span data-stu-id="c9822-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="c9822-138">Kliknou na odkaz "RSVP pro tuto událost", budete-li volání AJAX na metodu akce registru na řadiči RSVP a po dokončení se zobrazí aktualizovaná zpráva podobná níže uvedenému příkladu:</span><span class="sxs-lookup"><span data-stu-id="c9822-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="c9822-139">Šířka pásma sítě a provoz používané při kontrole toto volání AJAX je opravdu jednoduché.</span><span class="sxs-lookup"><span data-stu-id="c9822-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="c9822-140">Když uživatel klikne na odkaz "RSVP pro tuto událost", je provedené malé síťový požadavek HTTP POST */Dinners/Register/1* adresu URL, která vypadá jako níže na lince:</span><span class="sxs-lookup"><span data-stu-id="c9822-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="c9822-141">A jednoduše je odpověď na naše metoda akce registrace:</span><span class="sxs-lookup"><span data-stu-id="c9822-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="c9822-142">Toto jednoduché volání je rychlý a budou fungovat i v pomalé síti.</span><span class="sxs-lookup"><span data-stu-id="c9822-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="c9822-143">Přidání jQuery animace</span><span class="sxs-lookup"><span data-stu-id="c9822-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="c9822-144">Funkce AJAX, která jsme implementovali funguje dobře a rychlé.</span><span class="sxs-lookup"><span data-stu-id="c9822-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="c9822-145">V některých případech může se stát tak rychle, ale, že uživatel nemusí Všimněte si, že odkaz RSVP se nahradil novým textem.</span><span class="sxs-lookup"><span data-stu-id="c9822-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="c9822-146">Abyste viděli výsledek trochu zřejmé, můžeme přidat jednoduché animace k přitažení pozornosti ke zpráva aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c9822-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="c9822-147">Výchozí šablona projektu ASP.NET MVC zahrnuje jQuery – skvělé (a velmi populární) open source knihovna jazyka JavaScript, který je také podporován společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c9822-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="c9822-148">jQuery poskytuje řadu funkcí, včetně vylepšení vzhledu knihovny výběr a důsledky modelu DOM jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="c9822-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="c9822-149">Použití jQuery nejprve přidáme na ni odkaz skriptu.</span><span class="sxs-lookup"><span data-stu-id="c9822-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="c9822-150">Vzhledem k tomu, že budeme používat jQuery v rámci různých místech v rámci našeho webu, přidáme odkaz na skript v rámci naší soubor Site.master předlohové stránky tak, aby ho může použít všechny stránky.</span><span class="sxs-lookup"><span data-stu-id="c9822-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="c9822-151">*Tip: Ujistěte se, že je nainstalována oprava hotfix technologie intellisense jazyka JavaScript pro VS 2008 SP1, která umožňuje bohatší podporu technologie intellisense pro soubory JavaScriptu (včetně jQuery). Stáhněte si ho: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="c9822-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="c9822-152">Kód napsané s využitím JQuery často používá globální "$ ()" metodu JavaScript, který načte jeden nebo více elementů HTML pomocí selektor šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="c9822-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="c9822-153">Například <em>$("#rsvpmsg")</em> vybere libovolný prvek HTML s id rsvpmsg, zatímco <em>$(".something")</em> by vybrat všechny elementy s "něco co uživatel" šablon stylů CSS název třídy.</span><span class="sxs-lookup"><span data-stu-id="c9822-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="c9822-154">Můžete taky psát složitější dotazy jako "vrácení všech přepínačů checked" pomocí selektoru dotazu jako: <em>$("vstupu [@type= přepínač] [@checked]")</em>.</span><span class="sxs-lookup"><span data-stu-id="c9822-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="c9822-155">Jakmile vyberete prvky, může volat metody na nich provádět akce, jako je skrytí: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="c9822-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="c9822-156">Pro náš scénář RSVP budeme definovat jednoduchou funkci JavaScriptu s názvem "AnimateRSVPMessage", který vybere "rsvpmsg" &lt;div&gt; a animuje velikosti svého obsahu textu.</span><span class="sxs-lookup"><span data-stu-id="c9822-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="c9822-157">Níže uvedeného kódu spustí text malé a poté způsobí, že ho chcete zvýšit nad časový rámec 400 milisekund:</span><span class="sxs-lookup"><span data-stu-id="c9822-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="c9822-158">Jsme můžete pak navázání tato funkce JavaScriptu, která se má volat po úspěšném dokončení našich volání jazyka AJAX předáním názvu naše Ajax.ActionLink() pomocnou metodu (prostřednictvím AjaxOptions "OnSuccess" události vlastnost):</span><span class="sxs-lookup"><span data-stu-id="c9822-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="c9822-159">A teď při kliknutí na odkaz "RSVP pro tuto událost" a naše volání jazyka AJAX dokončí úspěšně, obsah zprávy zpět se animace a rozvíjet velké:</span><span class="sxs-lookup"><span data-stu-id="c9822-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="c9822-160">Kromě toho, že událost "OnSuccess" objekt AjaxOptions poskytuje OnBegin OnFailure a OnComplete události, které dokáže zpracovat (spolu s celou řadu dalších vlastností a užitečné možnosti).</span><span class="sxs-lookup"><span data-stu-id="c9822-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="c9822-161">Čištění – Refaktorujte si potvrďte svou účast ještě částečné zobrazení</span><span class="sxs-lookup"><span data-stu-id="c9822-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="c9822-162">Naše podrobnosti zobrazit šablonu spouští získat mírně dlouhý, které přesčas bude obtížnější trochu pochopit.</span><span class="sxs-lookup"><span data-stu-id="c9822-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="c9822-163">Chcete-li zlepšit čitelnost kódu, Pojďme nakonec vytvořit částečné zobrazení – RSVPStatus.ascx – zapouzdření veškerých RSVP zobrazit kód pro naší stránce s podrobnostmi o.</span><span class="sxs-lookup"><span data-stu-id="c9822-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="c9822-164">Můžeme to udělat tak, že pravým tlačítkem na složku \Views\Dinners a následným výběrem možnosti Přidat -&gt;zobrazit příkaz nabídky.</span><span class="sxs-lookup"><span data-stu-id="c9822-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="c9822-165">Musíme ji získat objekt Dinner jako jeho ViewModel silného typu.</span><span class="sxs-lookup"><span data-stu-id="c9822-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="c9822-166">Jsme pak zkopírovat a vložit obsah RSVP z našich Details.aspx nabízí pohled na to.</span><span class="sxs-lookup"><span data-stu-id="c9822-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="c9822-167">Po jsme provedli, který, můžeme také vytvořit další částečné zobrazení – EditAndDeleteLinks.ascx –, který zapouzdřuje naše úpravu a odstranění odkazu zobrazit kód.</span><span class="sxs-lookup"><span data-stu-id="c9822-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="c9822-168">Také budete moct získat objekt Dinner jako jeho ViewModel silného typu a upravit a odstranit logiku z našich Details.aspx nabízí pohled na to, kopírování a vkládání.</span><span class="sxs-lookup"><span data-stu-id="c9822-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="c9822-169">Naše podrobnosti zobrazení šablony může pak obsahuje pouze dva Html.RenderPartial() volání metody v dolní části:</span><span class="sxs-lookup"><span data-stu-id="c9822-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="c9822-170">Díky tomu kód čistější ke čtení a udržovat.</span><span class="sxs-lookup"><span data-stu-id="c9822-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="c9822-171">Dalším krokem</span><span class="sxs-lookup"><span data-stu-id="c9822-171">Next Step</span></span>

<span data-ttu-id="c9822-172">Nyní Podívejme se na jak můžeme ještě více použití jazyka AJAX a přidává interaktivní mapování na naši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c9822-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c9822-173">[Předchozí](secure-applications-using-authentication-and-authorization.md)
> [další](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="c9822-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
