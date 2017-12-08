---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: "Postup není v technologii ASP.NET a co dělat, místo toho | Microsoft Docs"
author: tfitzmac
description: "Toto téma popisuje několik běžných chyb, které uživatelé provést v rámci webové projekty ASP.NET. Poskytuje doporučení pro co dělat, aby se zabránilo tyto commo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 24c6a35a6b663ebb0f8d0e3e7988322fa5d9018c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="951cd-104">Postup není v technologii ASP.NET a co dělat, místo toho</span><span class="sxs-lookup"><span data-stu-id="951cd-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="951cd-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="951cd-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="951cd-106">Toto téma popisuje několik běžných chyb, které uživatelé provést v rámci webové projekty ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="951cd-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="951cd-107">Poskytuje doporučení pro co dělat, aby se zabránilo těchto běžných chyb.</span><span class="sxs-lookup"><span data-stu-id="951cd-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="951cd-108">Je založena na [prezentace](http://vimeo.com/68390507) podle **Damianu Edwards** na konferenci Norská vývojáři.</span><span class="sxs-lookup"><span data-stu-id="951cd-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="951cd-109">Právní omezení</span><span class="sxs-lookup"><span data-stu-id="951cd-109">Disclaimer</span></span>

<span data-ttu-id="951cd-110">V tomto tématu není určen jako kompletní příručka k zajištění, že aplikace je nejbezpečnější a nejúčinnější.</span><span class="sxs-lookup"><span data-stu-id="951cd-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="951cd-111">Potřebujete stále doporučené postupy pro zabezpečení a výkonu, které nejsou uvedené v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="951cd-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="951cd-112">Navrhne pouze tom, jak zamezit obvyklé chyby týkající se třídy rozhraní .NET a procesy.</span><span class="sxs-lookup"><span data-stu-id="951cd-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="951cd-113">Přehled</span><span class="sxs-lookup"><span data-stu-id="951cd-113">Overview</span></span>

<span data-ttu-id="951cd-114">Toto téma obsahuje následující oddíly:</span><span class="sxs-lookup"><span data-stu-id="951cd-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="951cd-115">Dodržování standardů</span><span class="sxs-lookup"><span data-stu-id="951cd-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="951cd-116">Adaptéry ovládacích prvků</span><span class="sxs-lookup"><span data-stu-id="951cd-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="951cd-117">Vlastnosti stylu na ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="951cd-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="951cd-118">Stránky a ovládací prvek zpětných volání</span><span class="sxs-lookup"><span data-stu-id="951cd-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="951cd-119">Zjišťování schopností prohlížeče</span><span class="sxs-lookup"><span data-stu-id="951cd-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="951cd-120">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="951cd-120">Security</span></span>](#security)

    - [<span data-ttu-id="951cd-121">Ověření žádosti</span><span class="sxs-lookup"><span data-stu-id="951cd-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="951cd-122">Ověřování pomocí formulářů bez souborů cookie a relace</span><span class="sxs-lookup"><span data-stu-id="951cd-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="951cd-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="951cd-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="951cd-124">Středním vztahem důvěryhodnosti</span><span class="sxs-lookup"><span data-stu-id="951cd-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="951cd-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="951cd-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="951cd-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="951cd-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="951cd-127">Spolehlivost a výkon</span><span class="sxs-lookup"><span data-stu-id="951cd-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="951cd-128">PreSendRequestHeaders a PreSendRequestContext</span><span class="sxs-lookup"><span data-stu-id="951cd-128">PreSendRequestHeaders and PreSendRequestContext</span></span>](#presend)
    - [<span data-ttu-id="951cd-129">Události asynchronní stránky s webovými formuláři</span><span class="sxs-lookup"><span data-stu-id="951cd-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="951cd-130">Ještě efektivněji a zapomněli pracovní</span><span class="sxs-lookup"><span data-stu-id="951cd-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="951cd-131">Obsah Entity žádosti</span><span class="sxs-lookup"><span data-stu-id="951cd-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="951cd-132">Metoda Response.Redirect a metody Response.End</span><span class="sxs-lookup"><span data-stu-id="951cd-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="951cd-133">EnableViewState a ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="951cd-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="951cd-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="951cd-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="951cd-135">Dlouhé žádosti systémem (> 110 sekund)</span><span class="sxs-lookup"><span data-stu-id="951cd-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="951cd-136">Dodržování standardů</span><span class="sxs-lookup"><span data-stu-id="951cd-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="951cd-137">Adaptéry ovládacích prvků</span><span class="sxs-lookup"><span data-stu-id="951cd-137">Control Adapters</span></span>

<span data-ttu-id="951cd-138">Doporučení: Přestat používat adaptéry ovládacích prvků pro adaptivního vykreslování a místo toho použít dotazy na média šablon stylů CSS a standardům HTML.</span><span class="sxs-lookup"><span data-stu-id="951cd-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="951cd-139">Ovládací prvky adaptéry byly zavedeny v rozhraní .NET 2.0 k vykreslení prezentace kód, který byl přizpůsobené pro prostředí a různých zařízení.</span><span class="sxs-lookup"><span data-stu-id="951cd-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="951cd-140">Tento adaptivního vykreslování nyní můžete provést pomocí šablon stylů CSS a HTML.</span><span class="sxs-lookup"><span data-stu-id="951cd-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="951cd-141">Měli přestat používat ovládací prvek adaptéry a převést žádné existující adaptéry a šablon stylů CSS, HTML.</span><span class="sxs-lookup"><span data-stu-id="951cd-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="951cd-142">Další informace najdete v tématu [dotazy na média](http://www.w3.org/TR/css3-mediaqueries/) a [postupy: Přidání mobilní stránky na vaše webové formuláře ASP.NET nebo aplikace MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="951cd-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="951cd-143">Vlastnosti stylu na ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="951cd-143">Style Properties on Controls</span></span>

<span data-ttu-id="951cd-144">Doporučení: Zastavte styl hodnoty nastavení ve značce ovládacího prvku a místo toho nastavit formátování hodnoty v šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="951cd-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="951cd-145">Ovládací prvky webového serveru obsahovat desítek vlastností, které můžete použít k nastavení vlastnosti stylu v řádku.</span><span class="sxs-lookup"><span data-stu-id="951cd-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="951cd-146">Například vlastnost ForeColor nastaví barvu textu pro ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="951cd-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="951cd-147">Můžete provést tento stejného efektu efektivněji prostřednictvím šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="951cd-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="951cd-148">Předlohy se styly umožňují centralizovat styl hodnoty a vyhnout se nastavení tyto hodnoty v celé vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="951cd-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="951cd-149">Následující příklad ukazuje třídu CSS nastaví text, který se red.</span><span class="sxs-lookup"><span data-stu-id="951cd-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="951cd-150">Další příklad ukazuje, jak dynamicky použít třídu CSS.</span><span class="sxs-lookup"><span data-stu-id="951cd-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="951cd-151">Stránky a ovládací prvek zpětných volání</span><span class="sxs-lookup"><span data-stu-id="951cd-151">Page and Control Callbacks</span></span>

<span data-ttu-id="951cd-152">Doporučení: Přestat používat stránky a ovládací prvek zpětná volání a místo toho použít některou z následujících: AJAX, UpdatePanel, metody akce MVC, webového rozhraní API nebo SignalR.</span><span class="sxs-lookup"><span data-stu-id="951cd-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="951cd-153">V předchozích verzích technologie ASP.NET metody zpětného volání stránky a ovládací prvek umožňují část webové stránky aktualizovat bez obnovení celou stránku.</span><span class="sxs-lookup"><span data-stu-id="951cd-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="951cd-154">Nyní můžete provést částečné aktualizace stránky prostřednictvím [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx), [MVC](../../../mvc/index.md), [webového rozhraní API](../../../web-api/index.md) nebo [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="951cd-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="951cd-155">Byste měli zastavit pomocí metody zpětného volání, protože se může způsobit problémy s přátelské adresy URL a směrování.</span><span class="sxs-lookup"><span data-stu-id="951cd-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="951cd-156">Ve výchozím nastavení ovládací prvky nepovolujte metody zpětného volání, ale pokud jste povolili tato funkce v ovládacím prvku, měli byste zakázat ho.</span><span class="sxs-lookup"><span data-stu-id="951cd-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="951cd-157">Zjišťování schopností prohlížeče</span><span class="sxs-lookup"><span data-stu-id="951cd-157">Browser Capability Detection</span></span>

<span data-ttu-id="951cd-158">Doporučení: Přestat používat zjišťování schopností statické prohlížeče a místo toho použít dynamické funkce zjišťování.</span><span class="sxs-lookup"><span data-stu-id="951cd-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="951cd-159">V předchozích verzích technologie ASP.NET podporované funkce pro každý prohlížeč byly uloženy v souboru XML.</span><span class="sxs-lookup"><span data-stu-id="951cd-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="951cd-160">Zjišťuje podpora funkce prostřednictvím statických vyhledávání není nejlepším postupem.</span><span class="sxs-lookup"><span data-stu-id="951cd-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="951cd-161">Nyní můžete dynamicky zjistit pomocí funkce zjišťování rozhraní, jako je funkce podporované prohlížeče [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="951cd-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="951cd-162">Funkce zjišťování Určuje podporu tak, že pokus o použití metody nebo vlastnosti a potom zkontrolujete, pokud v prohlížeči vytváří požadovaný výsledek.</span><span class="sxs-lookup"><span data-stu-id="951cd-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="951cd-163">Ve výchozím nastavení Modernizr je součástí šablony webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="951cd-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="951cd-164">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="951cd-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="951cd-165">Ověření žádosti</span><span class="sxs-lookup"><span data-stu-id="951cd-165">Request Validation</span></span>

<span data-ttu-id="951cd-166">Doporučení: Ověření vstupu uživatele a kódování výstup od uživatelů.</span><span class="sxs-lookup"><span data-stu-id="951cd-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="951cd-167">Ověření žádosti je funkce technologie ASP.NET, která kontroluje každý požadavek a zastaví požadavku, pokud je nalezena zjištěné hrozby.</span><span class="sxs-lookup"><span data-stu-id="951cd-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="951cd-168">Nezávisí na ověření žádosti pro zabezpečení aplikace před útoky skriptování mezi weby.</span><span class="sxs-lookup"><span data-stu-id="951cd-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="951cd-169">Místo toho ověřte veškerý vstup od uživatelů a kódování výstup.</span><span class="sxs-lookup"><span data-stu-id="951cd-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="951cd-170">V některých omezených případech můžete použití regulárních výrazů pro ověření vstupu, ale v složitější případech by měl ověřit vstup uživatele s použitím rozhraní .NET třídy, které určí, zda hodnota odpovídá povolené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="951cd-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="951cd-171">Následující příklad ukazuje, jak používat statickou metodu v třídě Uri k určení, zda je platný identifikátor Uri zadané uživatelem.</span><span class="sxs-lookup"><span data-stu-id="951cd-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="951cd-172">Ale dostatečně ověření identifikátor Uri, byste taky měli zkontrolovat a ujistěte se, určuje `http` nebo `https`.</span><span class="sxs-lookup"><span data-stu-id="951cd-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="951cd-173">Následující příklad používá instanci metody ověření, že je platný identifikátor Uri.</span><span class="sxs-lookup"><span data-stu-id="951cd-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="951cd-174">Před vykreslování uživatelský vstup ve formátu HTML nebo v dotazu SQL včetně uživatelský vstup, zakódujte hodnoty tak, aby zajistěte, aby byl škodlivý kód není zahrnutý.</span><span class="sxs-lookup"><span data-stu-id="951cd-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="951cd-175">Můžete HTML kódování hodnota ve značkách s &lt;%: %&gt; syntaxe, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="951cd-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="951cd-176">Nebo v syntaxi Razor můžete HTML kódování s @, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="951cd-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="951cd-177">Další příklad ukazuje způsob do formátu HTML kódování hodnotu v kódu.</span><span class="sxs-lookup"><span data-stu-id="951cd-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="951cd-178">Ke kódování bezpečně hodnotu pro příkazy SQL, použijte parametry příkazu, jako [SqlParameter](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="951cd-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="951cd-179">Ověřování pomocí formulářů bez souborů cookie a relace</span><span class="sxs-lookup"><span data-stu-id="951cd-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="951cd-180">Doporučení: Vyžadovat soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="951cd-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="951cd-181">Předávání informací o ověřování v řetězci dotazu není zabezpečený.</span><span class="sxs-lookup"><span data-stu-id="951cd-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="951cd-182">Pokud vaše aplikace obsahuje ověřování proto vyžadovat soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="951cd-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="951cd-183">Pokud váš soubor cookie obsahuje citlivé informace, zvažte vyžadování protokolu SSL pro soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="951cd-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="951cd-184">Následující příklad ukazuje, jak k určení v souboru Web.config, že ověřování pomocí formulářů vyžaduje souboru cookie, který se přenášejí přes protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="951cd-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="951cd-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="951cd-185">EnableViewStateMac</span></span>

<span data-ttu-id="951cd-186">Doporučení: Nikdy nastaven na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="951cd-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="951cd-187">Ve výchozím nastavení, EnbableViewStateMac nastavena na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="951cd-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="951cd-188">I když se aplikace nepoužívá stav zobrazení, nenastavujte EnableViewStateMac na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="951cd-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="951cd-189">Nastavení této hodnoty na hodnotu false bude ohrožení aplikace skriptování mezi servery.</span><span class="sxs-lookup"><span data-stu-id="951cd-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="951cd-190">Spouštění pomocí technologie ASP.NET 4.5.2, modul runtime vynucuje **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="951cd-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="951cd-191">I v případě, že je nastavena na hodnotu false, modul runtime ignoruje tuto hodnotu a pokračuje v hodnotu nastavenou na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="951cd-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="951cd-192">Další informace najdete v tématu [ASP.NET 4.5.2 a EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="951cd-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="951cd-193">Následující příklad ukazuje, jak nastavit EnableViewStateMac na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="951cd-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="951cd-194">Nemusíte ve skutečnosti, nastavte hodnotu na hodnotu true, protože je ve výchozím nastavení hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="951cd-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="951cd-195">Ale pokud nastavíte ho na hodnotu false na libovolné stránce v aplikaci, musíte okamžitě opravit tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="951cd-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="951cd-196">Středním vztahem důvěryhodnosti</span><span class="sxs-lookup"><span data-stu-id="951cd-196">Medium Trust</span></span>

<span data-ttu-id="951cd-197">Doporučení: Nezávisí na střední důvěryhodnosti (nebo jiné úroveň důvěryhodnosti) jako hranice zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="951cd-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="951cd-198">Částečná důvěryhodnost adekvátní nechrání vaší aplikace a by se neměla používat.</span><span class="sxs-lookup"><span data-stu-id="951cd-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="951cd-199">Místo toho použijte úplný vztah důvěryhodnosti a izolovat nedůvěryhodné aplikace v samostatných fondech aplikací.</span><span class="sxs-lookup"><span data-stu-id="951cd-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="951cd-200">Také spouštět každý fond aplikací v rámci jedinečnou identitu.</span><span class="sxs-lookup"><span data-stu-id="951cd-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="951cd-201">Další informace najdete v tématu [ASP.NET částečné důvěryhodnosti nezaručuje izolace aplikací](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="951cd-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="951cd-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="951cd-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="951cd-203">Doporučení: Nezakazujte nastavení zabezpečení v &lt;appSettings&gt; elementu.</span><span class="sxs-lookup"><span data-stu-id="951cd-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="951cd-204">AppSettings element obsahuje hodně hodnot, které jsou požadovány pro aktualizace zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="951cd-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="951cd-205">Nesmí změnit nebo zakázat tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="951cd-205">You should not change or disable these values.</span></span> <span data-ttu-id="951cd-206">Pokud při nasazení aktualizace, je nutné zakázat tyto hodnoty, okamžitě znovu povolte po dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="951cd-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="951cd-207">Podrobnosti najdete v tématu [ASP.NET appSettings Element](https://msdn.microsoft.com/en-us/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="951cd-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/en-us/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="951cd-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="951cd-208">UrlPathEncode</span></span>

<span data-ttu-id="951cd-209">Doporučení: Použijte [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx) místo.</span><span class="sxs-lookup"><span data-stu-id="951cd-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="951cd-210">Metoda UrlPathEncode byla přidána do rozhraní .NET Framework, chcete-li vyřešit potíže s kompatibilitou velmi konkrétní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="951cd-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="951cd-211">Neprovádí adekvátní kódování adresu URL a nechrání aplikace z skriptování mezi servery.</span><span class="sxs-lookup"><span data-stu-id="951cd-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="951cd-212">Nikdy používejte ji v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="951cd-212">You should never use it in your application.</span></span> <span data-ttu-id="951cd-213">Místo toho použijte [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="951cd-213">Instead, use [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="951cd-214">Následující příklad ukazuje, jak předat kódovaného adresu URL jako parametr řetězce dotazu pro ovládacího prvku hypertextový odkaz.</span><span class="sxs-lookup"><span data-stu-id="951cd-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="951cd-215">Spolehlivost a výkon</span><span class="sxs-lookup"><span data-stu-id="951cd-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontext"></a><span data-ttu-id="951cd-216">PreSendRequestHeaders a PreSendRequestContext</span><span class="sxs-lookup"><span data-stu-id="951cd-216">PreSendRequestHeaders and PreSendRequestContext</span></span>

<span data-ttu-id="951cd-217">Doporučení: Nepoužívejte tyto události s spravované moduly.</span><span class="sxs-lookup"><span data-stu-id="951cd-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="951cd-218">Místo toho zápisu nativní modul služby IIS k provedení požadované úlohy.</span><span class="sxs-lookup"><span data-stu-id="951cd-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="951cd-219">V tématu [vytváření modulů HTTP v nativním kódu](https://msdn.microsoft.com/en-us/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="951cd-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/en-us/library/ms693629.aspx).</span></span>

<span data-ttu-id="951cd-220">Můžete použít [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx) a [PreSendRequestContext](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx) události s nativní moduly služby IIS, ale nemohou používat s spravované moduly, které implementují IHttpModule.</span><span class="sxs-lookup"><span data-stu-id="951cd-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContext](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules, but do not use them with managed modules that implement IHttpModule.</span></span> <span data-ttu-id="951cd-221">Nastavení těchto vlastností může způsobit problémy s asynchronní požadavky.</span><span class="sxs-lookup"><span data-stu-id="951cd-221">Setting these properties can cause issues with asynchronous requests.</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="951cd-222">Události asynchronní stránky s webovými formuláři</span><span class="sxs-lookup"><span data-stu-id="951cd-222">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="951cd-223">Doporučení: V webových formulářů, vyhněte se asynchronní zápis void metod pro události životního cyklu stránky a použijte [Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx) pro asynchronní kód.</span><span class="sxs-lookup"><span data-stu-id="951cd-223">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="951cd-224">Pokud označíte událostí stránky s **asynchronní** a **void**, nelze určit dokončení asynchronní kódu.</span><span class="sxs-lookup"><span data-stu-id="951cd-224">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="951cd-225">Místo toho použijte Page.RegisterAsyncTask spustit kód asynchronní způsobem, který umožňuje sledovat její dokončení.</span><span class="sxs-lookup"><span data-stu-id="951cd-225">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="951cd-226">Následující příklad ukazuje a tlačítko klikněte na tlačítko obslužná rutina, která obsahuje asynchronní kód.</span><span class="sxs-lookup"><span data-stu-id="951cd-226">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="951cd-227">Tento příklad obsahuje čtení řetězcovou hodnotu asynchronně, který je k dispozici pouze jako zjednodušenou příklad asynchronní úlohu a ne jako doporučený postup.</span><span class="sxs-lookup"><span data-stu-id="951cd-227">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="951cd-228">Pokud používáte asynchronních úloh, nastavte Http runtime cílový framework 4.5 v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="951cd-228">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="951cd-229">Nastavení rozhraní target framework na oplátku 4.5 na nový kontext synchronizace, přidala se v rozhraní .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="951cd-229">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="951cd-230">Tato hodnota je nastavena ve výchozím nastavení v nové projekty v sadě Visual Studio 2012, ale je, není možné nastavit při práci s existujícího projektu.</span><span class="sxs-lookup"><span data-stu-id="951cd-230">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="951cd-231">Ještě efektivněji a zapomněli pracovní</span><span class="sxs-lookup"><span data-stu-id="951cd-231">Fire-and-Forget Work</span></span>

<span data-ttu-id="951cd-232">Doporučení: Při zpracování požadavku v prostředí ASP.NET, vyhněte se spouští pracovní ještě efektivněji a zapomněli (takové volání metody ThreadPool.QueueUserWorkItem nebo vytvoření časovače, která opakovaně volá delegáta).</span><span class="sxs-lookup"><span data-stu-id="951cd-232">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="951cd-233">Pokud aplikace obsahuje pracovní ještě efektivněji a zapomněli, který běží v prostředí ASP.NET, vaše aplikace mohou získat synchronizován. V každém okamžiku může být zničený doména aplikace, to znamená, že probíhající proces už neodpovídá aktuální stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="951cd-233">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="951cd-234">Měli byste přejít tento typ práce mimo prostředí ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="951cd-234">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="951cd-235">Můžete použít webové úlohy, služba systému Windows nebo role pracovního procesu v Azure pro probíhající práci a spusťte tento kód z jiného procesu.</span><span class="sxs-lookup"><span data-stu-id="951cd-235">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="951cd-236">Pokud je třeba provést činnost v prostředí ASP.NET, můžete přidat balíček Nuget názvem [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) ke spuštění kódu.</span><span class="sxs-lookup"><span data-stu-id="951cd-236">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="951cd-237">Obsah Entity žádosti</span><span class="sxs-lookup"><span data-stu-id="951cd-237">Request Entity Body</span></span>

<span data-ttu-id="951cd-238">Doporučení: Vyhněte se čtení Request.Form nebo Request.InputStream před spuštění obslužné rutiny událostí.</span><span class="sxs-lookup"><span data-stu-id="951cd-238">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="951cd-239">Nejdřívější, které byste si měli přečíst z Request.Form nebo Request.InputStream je během této obslužné rutiny spustit událostí.</span><span class="sxs-lookup"><span data-stu-id="951cd-239">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="951cd-240">V MVC Kontroleru je obslužná rutina a spouštění událostí je při spuštění metody akce.</span><span class="sxs-lookup"><span data-stu-id="951cd-240">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="951cd-241">V webových formulářů stránka je obslužná rutina a spouštění událostí je při aktivuje událost Page.Init.</span><span class="sxs-lookup"><span data-stu-id="951cd-241">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="951cd-242">Pokud číst obsah entity žádosti starší než spouštění událostí, narušovat zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="951cd-242">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="951cd-243">Pokud je třeba číst obsah entity žádosti před událostí spouštění, použijte buď [Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx) nebo [Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="951cd-243">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="951cd-244">Při použití GetBufferlessInputStream získat nezpracovaná datový proud z požadavku a převzít odpovědnost za celý požadavek na zpracování.</span><span class="sxs-lookup"><span data-stu-id="951cd-244">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="951cd-245">Po volání GetBufferlessInputStream, Request.Form a Request.InputStream nejsou k dispozici, protože nebyly vyplněna technologií ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="951cd-245">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="951cd-246">Při použití GetBufferedInputStream získat kopii datového proudu z požadavku.</span><span class="sxs-lookup"><span data-stu-id="951cd-246">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="951cd-247">Request.Form a Request.InputStream jsou stále k dispozici novější v žádosti, protože ASP.NET naplní jiné kopie.</span><span class="sxs-lookup"><span data-stu-id="951cd-247">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="951cd-248">Metoda Response.Redirect a metody Response.End</span><span class="sxs-lookup"><span data-stu-id="951cd-248">Response.Redirect and Response.End</span></span>

<span data-ttu-id="951cd-249">Doporučení: Znát rozdíly ve zpracování vláken po volání [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="951cd-249">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="951cd-250">[Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx) metoda volá metodu metody Response.End.</span><span class="sxs-lookup"><span data-stu-id="951cd-250">The [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="951cd-251">Probíhající synchronní volání Request.Redirect způsobí, že aktuální vlákno k okamžitě přerušení.</span><span class="sxs-lookup"><span data-stu-id="951cd-251">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="951cd-252">Ale v asynchronního procesu, volání Response.Redirect není přerušit aktuální vlákno, pokračuje v provádění kódu pro daný požadavek.</span><span class="sxs-lookup"><span data-stu-id="951cd-252">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="951cd-253">V asynchronní proces musí vrátit úlohu z metody zastavit provádění kódu.</span><span class="sxs-lookup"><span data-stu-id="951cd-253">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="951cd-254">V projektu MVC nesmí volání Response.Redirect.</span><span class="sxs-lookup"><span data-stu-id="951cd-254">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="951cd-255">Místo toho vrátí RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="951cd-255">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="951cd-256">EnableViewState a ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="951cd-256">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="951cd-257">Doporučení: ViewStateMode použijte místo EnableViewState poskytnout přesná kontrola nad niž používat ovládací prvky zobrazení stavu.</span><span class="sxs-lookup"><span data-stu-id="951cd-257">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="951cd-258">Když nastavíte na hodnotu false v direktivě stránky EnableViewState, stav zobrazení je zakázán pro všechny ovládací prvky v rámci dané stránky a nelze povolit.</span><span class="sxs-lookup"><span data-stu-id="951cd-258">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="951cd-259">Pokud chcete povolit stav zobrazení pro jenom některé ovládací prvky na stránce, nastavte pro stránku ViewStateMode na zakázáno.</span><span class="sxs-lookup"><span data-stu-id="951cd-259">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="951cd-260">Potom nastavte ViewStateMode na povoleno pouze ovládací prvky, které skutečně potřebují stav zobrazení.</span><span class="sxs-lookup"><span data-stu-id="951cd-260">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="951cd-261">Povolením stav zobrazení pro ovládací prvky, které je třeba ji můžete zmenšit velikost stav zobrazení pro webové stránky.</span><span class="sxs-lookup"><span data-stu-id="951cd-261">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="951cd-262">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="951cd-262">SqlMembershipProvider</span></span>

<span data-ttu-id="951cd-263">Doporučení: Použijte Universal Providers.</span><span class="sxs-lookup"><span data-stu-id="951cd-263">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="951cd-264">V aktuální šablony projektů SqlMembershipProvider nahradila [balíčku ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), která je k dispozici jako balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="951cd-264">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="951cd-265">Pokud používáte SqlMembershipProvider v projektu, který byl sestaven s dřívější verzi šablony, můžete přepnout do Universal Providers.</span><span class="sxs-lookup"><span data-stu-id="951cd-265">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="951cd-266">Universal Providers pracovat se všechny databáze, které podporuje rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="951cd-266">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="951cd-267">Další informace najdete v tématu [představení balíčku ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="951cd-267">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="951cd-268">Dlouhodobé požadavky (> 110 sekund)</span><span class="sxs-lookup"><span data-stu-id="951cd-268">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="951cd-269">Doporučení: Použijte [Websocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx) nebo [SignalR](../../../signalr/index.md) pro připojené klienty a použití asynchronní vstupně-výstupních operací.</span><span class="sxs-lookup"><span data-stu-id="951cd-269">Recommendation: Use [WebSockets](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="951cd-270">Dlouhodobé požadavky může způsobit nepředvídatelné výsledky a nízký výkon ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="951cd-270">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="951cd-271">Výchozí nastavení časového limitu pro žádost je 110 sekund.</span><span class="sxs-lookup"><span data-stu-id="951cd-271">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="951cd-272">Pokud používáte stav relace se dlouho běžící žádostí, ASP.NET se uvolní zámek na objektu Session po 110 sekund.</span><span class="sxs-lookup"><span data-stu-id="951cd-272">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="951cd-273">Aplikace však může být uprostřed operace na objektu relace, pokud se uvolní zámek, operace se nemusí dokončit úspěšně.</span><span class="sxs-lookup"><span data-stu-id="951cd-273">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="951cd-274">Pokud druhý žádost od uživatele je blokovaný, při prvním požadavku, může druhá žádost o přístup k objektu Session v nekonzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="951cd-274">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="951cd-275">Pokud vaše aplikace obsahuje blokování (nebo synchronní) vstupně-výstupních operací, aplikace bude reagovat.</span><span class="sxs-lookup"><span data-stu-id="951cd-275">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="951cd-276">Chcete-li zvýšit výkon, použijte asynchronní vstupně-výstupních operací v rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="951cd-276">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="951cd-277">Technologie WebSockets nebo SignalR taky použijte pro připojení klientů k serveru.</span><span class="sxs-lookup"><span data-stu-id="951cd-277">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="951cd-278">Tyto funkce jsou navrženy pro efektivní zpracování dlouho běžící požadavků.</span><span class="sxs-lookup"><span data-stu-id="951cd-278">These features are designed to efficiently handle long-running requests.</span></span>
