---
uid: ajax/cdn/overview
title: "Sítě pro doručování obsahu Microsoft Ajax | Microsoft Docs"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: f1225f06e5218d893e3f49b2ccc67d56365b30e5
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="8c318-102">Sítě pro doručování obsahu Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="8c318-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="8c318-103">Poznámka: Microsoft Ajax CDN nemá žádné SLA vedle pomocí Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="8c318-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="8c318-104">Obsah</span><span class="sxs-lookup"><span data-stu-id="8c318-104">Table of Contents</span></span>

<span data-ttu-id="8c318-105">**[adresu AJAX.microsoft.com přejmenován na ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="8c318-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="8c318-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="8c318-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="8c318-107">**[Pomocí prvku ASP.NET Ajax od CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="8c318-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="8c318-108">**[Pomocí jQuery od CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="8c318-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="8c318-109">**[Pomocí jQuery uživatelského rozhraní od CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="8c318-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="8c318-110">**[Soubory třetích stran na CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="8c318-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="8c318-111">Verze jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="8c318-112">jQuery migrovat verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="8c318-113">jQuery verze uživatelského rozhraní na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="8c318-114">jQuery ověření verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="8c318-115">jQuery Mobile verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="8c318-116">jQuery šablony verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="8c318-117">jQuery cyklus verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="8c318-118">jQuery DataTables verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="8c318-119">Verze Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="8c318-120">Verze JSHint na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="8c318-121">Verze knockout na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="8c318-122">Globalizace verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="8c318-123">Odpověď verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="8c318-124">Zavedení verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="8c318-125">Zavedení TouchCarousel verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="8c318-126">Verze hammer.js na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="8c318-127">Webové formuláře ASP.NET a Ajax verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="8c318-128">ASP.NET MVC uvolní na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="8c318-129">Funkce SignalR technologie ASP.NET uvolní na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="8c318-130">Microsoft Ajax Content Delivery Network (CDN) hostuje knihoven jazyka JavaScript oblíbených třetích stran, například jQuery a umožňuje snadno přidat je do webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="8c318-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="8c318-131">Například můžete začít používat jQuery, který je hostován na tento CDN jednoduše přidáním &lt;skriptu&gt; značky na stránku, která odkazuje na ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="8c318-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="8c318-132">Využitím CDN, může výrazně zlepšit výkon aplikací Ajax.</span><span class="sxs-lookup"><span data-stu-id="8c318-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="8c318-133">Obsah CDN ukládají do mezipaměti na servery umístěné po celém světě.</span><span class="sxs-lookup"><span data-stu-id="8c318-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="8c318-134">Kromě toho CDN umožňuje prohlížečům soubory uložené v mezipaměti třetích stran JavaScript pro webové stránky, které se nacházejí v různých doménách.</span><span class="sxs-lookup"><span data-stu-id="8c318-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="8c318-135">CDN podporuje protokol SSL (HTTPS), v případě, že budete muset poskytnout webovou stránku pomocí protokolu Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="8c318-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="8c318-136">CDN hostitelem následující knihovny skriptů třetích stran, které byly odeslány a licenci, vlastníky tyto knihovny:</span><span class="sxs-lookup"><span data-stu-id="8c318-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="8c318-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="8c318-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="8c318-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="8c318-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="8c318-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="8c318-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="8c318-140">jQuery ověření (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="8c318-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="8c318-141">jQuery cyklus (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="8c318-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="8c318-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="8c318-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="8c318-143">Microsoft Ajax CDN také zahrnuje následující knihovny, které byly odeslány společnosti Microsoft:</span><span class="sxs-lookup"><span data-stu-id="8c318-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="8c318-144">Technologie ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="8c318-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="8c318-145">Soubory JavaScript rozhraní ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8c318-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="8c318-146">Soubory ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="8c318-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="8c318-147">Microsoft nenárokuje vlastnictví všech knihoven jiných výrobců hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="8c318-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="8c318-148">Tyto knihovny pro vás jsou licencování autorským vlastníky knihoven.</span><span class="sxs-lookup"><span data-stu-id="8c318-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="8c318-149">Žádná práva, které možná budete muset stáhnout a použít tyto knihovny jsou udělit výhradně pomocí příslušných vlastníků autorských práv.</span><span class="sxs-lookup"><span data-stu-id="8c318-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="8c318-150">Protože se nejedná knihovny Microsoft, společnost Microsoft poskytuje žádné záruky ani licence právy duševního vlastnictví (včetně žádné předpokládané patentová práva) pro hostované na této CDN knihovny třetích stran.</span><span class="sxs-lookup"><span data-stu-id="8c318-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="8c318-151">Pokud chcete odeslat vaše knihovna JavaScript a vaše knihovna je jedním z hlavní knihovny JavaScript (jak je uvedeno v http://trends.builtwith.com) nebo rozšíření nebo moduly plug-in na tyto knihovny, které jsou (a) oblíbených; nebo (b) užitečné při použití technologie ASP.NET a obraťte se prosím na AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="8c318-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="8c318-152">adresu AJAX.microsoft.com přejmenován na ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="8c318-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="8c318-153">CDN lze použít název domény webu microsoft.com a byl změněn na použít název domény aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="8c318-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="8c318-154">Tato změna byla provedená zvýšit výkon, protože když prohlížeč doménu microsoft.com by se odesílat žádné soubory cookie z této domény přes přenosu s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="8c318-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="8c318-155">Název domény. než microsoft.com přejmenováním možné zvýšit výkon co nejvíc 25 %.</span><span class="sxs-lookup"><span data-stu-id="8c318-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="8c318-156">Poznamenejte si adresu ajax.microsoft.com budou nadále fungovat, ale doporučuje se ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="8c318-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="8c318-157">Starý formát: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="8c318-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="8c318-158">Nový formát: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="8c318-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="8c318-159">Visual Studio .vsdoc podpora</span><span class="sxs-lookup"><span data-stu-id="8c318-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="8c318-160">Použít soubory .vsdoc správně s Visual Studio 2008, musíte zajistit, že máte VS 2008 SP1 nainstalovány a opravy hotfix pro vsdoc soubory.</span><span class="sxs-lookup"><span data-stu-id="8c318-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="8c318-161">Můžete získat z tohoto místa:</span><span class="sxs-lookup"><span data-stu-id="8c318-161">You can get these from here:</span></span>

- [<span data-ttu-id="8c318-162">Stáhněte si Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="8c318-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Stáhnout Visual Studio 2008 SP1")
- [<span data-ttu-id="8c318-163">Stažení opravy hotfix .vsdoc pro Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="8c318-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "stažení opravy hotfix .vsdoc pro Visual Studio 2008 SP1")

<span data-ttu-id="8c318-164">Visual Studio 2010 podporuje .vsdoc soubory bez žádné další opravy.</span><span class="sxs-lookup"><span data-stu-id="8c318-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="8c318-165">Pomocí prvku ASP.NET Ajax od CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="8c318-166">Pokud používáte technologii ASP.NET 4, můžete přesměrovat všechny požadavky pro skripty framework ASP.NET do CDN.</span><span class="sxs-lookup"><span data-stu-id="8c318-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="8c318-167">Načítání skripty od CDN namísto místního webového serveru můžete výrazně zlepšit výkon veřejné weby technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8c318-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="8c318-168">Pomocí vlastnosti ScriptManager EnableCDN přesměrovat všechny požadavky skriptu framework ASP.NET Ajax CDN společnosti Microsoft:</span><span class="sxs-lookup"><span data-stu-id="8c318-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="8c318-169">Pomocí jQuery od CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="8c318-170">Můžete použít skripty jQuery hostované na CDN ve webové aplikaci přidáním následující skript element na stránce:</span><span class="sxs-lookup"><span data-stu-id="8c318-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="8c318-171">CDN také obsahuje minifikovaný verzi jQuery skript, který můžete získat pomocí následující element:</span><span class="sxs-lookup"><span data-stu-id="8c318-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="8c318-172">Pokud chcete, aby vaše stránky k záložnímu načítání jQuery z místní cestu na vlastní web, pokud není k dispozici CDN se stane, přidejte následující element ihned po element odkazující na CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="8c318-173">Na následující stránce Ukázka používá CDN verzi knihovny jQuery (s přechod na místní kopie) pro zobrazení obsahu elementu div při kliknutí na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8c318-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="8c318-174">Můžete získat další informace o jQuery a stáhnout místní kopii jQuery navštivte stránky [jQuery](http://jquery.com/) webu.</span><span class="sxs-lookup"><span data-stu-id="8c318-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="8c318-175">Pomocí jQuery uživatelského rozhraní od CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="8c318-176">CDN také hostitelem knihovna uživatelského rozhraní jQuery.</span><span class="sxs-lookup"><span data-stu-id="8c318-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="8c318-177">Knihovna uživatelského rozhraní jQuery zahrnuje širokou škálu pomůcek a účinky, které můžete použít v aplikacích ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8c318-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="8c318-178">Například na následující stránce ukazuje, jak je možné používat jQuery ovládací prvek Datepicker uživatelského rozhraní v kontextu aplikace webových formulářů ASP.NET a zobrazit automaticky otevírané okno Kalendář:</span><span class="sxs-lookup"><span data-stu-id="8c318-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="8c318-179">Pokud přesunete fokus do textového pole pomocí klávesnice, zobrazí se kalendáře:</span><span class="sxs-lookup"><span data-stu-id="8c318-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Kalendářem vytvořené pomocí ovládací prvek Datepicker](overview/_static/image1.png)

<span data-ttu-id="8c318-181">Všimněte si, že musí obsahovat tři soubory od CDN ve výše uvedeném kódu:</span><span class="sxs-lookup"><span data-stu-id="8c318-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="8c318-182">Knihovna jQuery &mdash; knihovna uživatelského rozhraní jQuery závisí na knihovny jQuery.</span><span class="sxs-lookup"><span data-stu-id="8c318-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="8c318-183">Knihovna jQuery je nutné přidat na stránku předtím, než přidáte knihovna uživatelského rozhraní jQuery.</span><span class="sxs-lookup"><span data-stu-id="8c318-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="8c318-184">Knihovna uživatelského rozhraní jQuery &mdash; knihovna uživatelského rozhraní jQuery obsahuje všechny důsledky uživatelského rozhraní jQuery a pomůcky, jako je například widgetu ovládací prvek Datepicker použít na stránce nahoře.</span><span class="sxs-lookup"><span data-stu-id="8c318-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="8c318-185">Motiv uživatelského rozhraní jQuery &mdash; jQuery UI podporuje různé motivy.</span><span class="sxs-lookup"><span data-stu-id="8c318-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="8c318-186">Výše uvedené stránka obsahuje odkaz na soubor k importu Redmond motiv šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="8c318-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="8c318-187">Všechny standardní jQuery UI motivy jsou hostované na CDN.</span><span class="sxs-lookup"><span data-stu-id="8c318-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="8c318-188">[Navštivte tuto stránku](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 na Microsoft Ajax CDN") Chcete-li zobrazit miniatury pro každý motiv.</span><span class="sxs-lookup"><span data-stu-id="8c318-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="8c318-189">Další informace o knihovna uživatelského rozhraní jQuery, najdete v oficiální [webu uživatelského rozhraní jQuery](http://jQueryUI.com "webu uživatelského rozhraní jQuery").</span><span class="sxs-lookup"><span data-stu-id="8c318-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="8c318-190">Soubory třetích stran na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="8c318-191">CDN hostitelem některé z nejčastěji používané knihovny JavaScript třetích stran.</span><span class="sxs-lookup"><span data-stu-id="8c318-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="8c318-192">Microsoft nenárokuje vlastnictví všech knihoven jiných výrobců hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="8c318-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="8c318-193">Tyto knihovny pro vás jsou licencování autorským vlastníky knihoven.</span><span class="sxs-lookup"><span data-stu-id="8c318-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="8c318-194">Žádná práva, které možná budete muset stáhnout a použít tyto knihovny jsou udělit výhradně pomocí příslušných vlastníků autorských práv.</span><span class="sxs-lookup"><span data-stu-id="8c318-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="8c318-195">Protože se nejedná knihovny Microsoft, společnost Microsoft poskytuje žádné záruky ani licence právy duševního vlastnictví (včetně žádné předpokládané patentová práva) pro hostované na této CDN knihovny třetích stran.</span><span class="sxs-lookup"><span data-stu-id="8c318-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="8c318-196">Verze jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="8c318-197">Následující verze jQuery hostovaných na CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="8c318-198">verze jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="8c318-198">jQuery version 3.3.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="8c318-199">verze jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="8c318-199">jQuery version 3.2.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="8c318-200">verze jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="8c318-200">jQuery version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="8c318-201">verze jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="8c318-201">jQuery version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="8c318-202">verze jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="8c318-202">jQuery version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="8c318-203">verze jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="8c318-203">jQuery version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="8c318-204">verze jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="8c318-204">jQuery version 2.2.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="8c318-205">verze jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="8c318-205">jQuery version 2.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="8c318-206">verze jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="8c318-206">jQuery version 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="8c318-207">verze jQuery 2.2.1</span><span class="sxs-lookup"><span data-stu-id="8c318-207">jQuery version 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="8c318-208">verze jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="8c318-208">jQuery version 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="8c318-209">verze jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="8c318-209">jQuery version 2.1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="8c318-210">verze jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="8c318-210">jQuery version 2.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="8c318-211">verze jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="8c318-211">jQuery version 2.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="8c318-212">verze jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="8c318-212">jQuery version 2.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="8c318-213">verze jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="8c318-213">jQuery version 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="8c318-214">verze jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="8c318-214">jQuery version 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="8c318-215">jQuery verze bodu 2.0.2</span><span class="sxs-lookup"><span data-stu-id="8c318-215">jQuery version 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="8c318-216">jQuery verze 2.0.1</span><span class="sxs-lookup"><span data-stu-id="8c318-216">jQuery version 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="8c318-217">verze jQuery 2.0.0</span><span class="sxs-lookup"><span data-stu-id="8c318-217">jQuery version 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="8c318-218">verze jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="8c318-218">jQuery version 1.12.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="8c318-219">verze jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="8c318-219">jQuery version 1.12.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="8c318-220">verze jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="8c318-220">jQuery version 1.12.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="8c318-221">verze jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="8c318-221">jQuery version 1.12.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="8c318-222">verze jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="8c318-222">jQuery version 1.12.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="8c318-223">verze jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="8c318-223">jQuery version 1.11.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="8c318-224">verze jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="8c318-224">jQuery version 1.11.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="8c318-225">verze jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="8c318-225">jQuery version 1.11.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="8c318-226">verze jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="8c318-226">jQuery version 1.11.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="8c318-227">verze jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="8c318-227">jQuery version 1.10.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="8c318-228">verze jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="8c318-228">jQuery version 1.10.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="8c318-229">verze jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="8c318-229">jQuery version 1.10.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="8c318-230">verze jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="8c318-230">jQuery version 1.9.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="8c318-231">verze jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="8c318-231">jQuery version 1.9.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="8c318-232">verze jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="8c318-232">jQuery version 1.8.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="8c318-233">verze jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="8c318-233">jQuery version 1.8.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="8c318-234">verze jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="8c318-234">jQuery version 1.8.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="8c318-235">verze jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="8c318-235">jQuery version 1.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="8c318-236">verze jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="8c318-236">jQuery version 1.7.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="8c318-237">verze jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="8c318-237">jQuery version 1.7.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="8c318-238">verze jQuery 1.7</span><span class="sxs-lookup"><span data-stu-id="8c318-238">jQuery version 1.7</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="8c318-239">verze jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="8c318-239">jQuery version 1.6.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="8c318-240">verze jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="8c318-240">jQuery version 1.6.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="8c318-241">verze jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="8c318-241">jQuery version 1.6.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="8c318-242">jQuery verze 1.6.1</span><span class="sxs-lookup"><span data-stu-id="8c318-242">jQuery version 1.6.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="8c318-243">jQuery verzi 1.6</span><span class="sxs-lookup"><span data-stu-id="8c318-243">jQuery version 1.6</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="8c318-244">verze jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="8c318-244">jQuery version 1.5.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="8c318-245">verze jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="8c318-245">jQuery version 1.5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="8c318-246">verze jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="8c318-246">jQuery version 1.5</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="8c318-247">verze jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="8c318-247">jQuery version 1.4.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="8c318-248">verze jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="8c318-248">jQuery version 1.4.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="8c318-249">jQuery verze 1.4.2</span><span class="sxs-lookup"><span data-stu-id="8c318-249">jQuery version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="8c318-250">verze jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="8c318-250">jQuery version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="8c318-251">jQuery verze 1.4</span><span class="sxs-lookup"><span data-stu-id="8c318-251">jQuery version 1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="8c318-252">jQuery verze 1.3.2</span><span class="sxs-lookup"><span data-stu-id="8c318-252">jQuery version 1.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="8c318-253">jQuery migrovat verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-253">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="8c318-254">Následující verze jQuery migrací jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-254">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="8c318-255">jQuery migrací verze 3.0.0</span><span class="sxs-lookup"><span data-stu-id="8c318-255">jQuery Migrate version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="8c318-256">jQuery migrací verze 1.2.1</span><span class="sxs-lookup"><span data-stu-id="8c318-256">jQuery Migrate version 1.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="8c318-257">jQuery migrací verze 1.2.0</span><span class="sxs-lookup"><span data-stu-id="8c318-257">jQuery Migrate version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="8c318-258">jQuery migrací verze 1.1.1</span><span class="sxs-lookup"><span data-stu-id="8c318-258">jQuery Migrate version 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="8c318-259">jQuery migrací verze 1.1.0</span><span class="sxs-lookup"><span data-stu-id="8c318-259">jQuery Migrate version 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="8c318-260">jQuery migrací verze 1.0.0</span><span class="sxs-lookup"><span data-stu-id="8c318-260">jQuery Migrate version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="8c318-261">jQuery verze uživatelského rozhraní na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-261">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="8c318-262">Následující verze knihovny uživatelského rozhraní jQuery jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="8c318-262">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="8c318-263">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="8c318-263">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="8c318-264">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="8c318-264">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-265">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="8c318-265">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-266">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="8c318-266">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-267">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="8c318-267">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-268">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="8c318-268">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-269">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="8c318-269">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-270">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="8c318-270">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-271">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="8c318-271">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-272">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="8c318-272">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-273">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="8c318-273">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-274">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="8c318-274">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-275">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="8c318-275">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-276">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="8c318-276">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-277">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="8c318-277">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-278">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="8c318-278">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-279">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="8c318-279">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-280">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="8c318-280">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-281">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="8c318-281">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-282">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="8c318-282">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-283">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="8c318-283">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-284">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="8c318-284">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-285">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="8c318-285">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-286">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="8c318-286">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-287">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="8c318-287">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-288">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="8c318-288">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-289">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="8c318-289">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-290">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="8c318-290">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-291">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="8c318-291">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-292">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="8c318-292">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-293">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="8c318-293">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-294">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="8c318-294">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-295">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="8c318-295">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-296">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="8c318-296">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-297">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="8c318-297">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-298">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="8c318-298">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="8c318-299">jQuery ověření verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-299">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="8c318-300">Následující verze knihovny jQuery ověření jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="8c318-300">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="8c318-301">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="8c318-301">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="8c318-302">jQuery ověřením 1.17.0</span><span class="sxs-lookup"><span data-stu-id="8c318-302">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery ověření 1.17.0")
- [<span data-ttu-id="8c318-303">jQuery ověřením 1.16.0</span><span class="sxs-lookup"><span data-stu-id="8c318-303">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery ověření 1.16.0")
- [<span data-ttu-id="8c318-304">jQuery ověřením 1.15.1</span><span class="sxs-lookup"><span data-stu-id="8c318-304">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery ověření 1.15.1")
- [<span data-ttu-id="8c318-305">jQuery ověřením 1.15.0</span><span class="sxs-lookup"><span data-stu-id="8c318-305">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery ověření 1.15.0")
- [<span data-ttu-id="8c318-306">jQuery ověřením 1.14.0</span><span class="sxs-lookup"><span data-stu-id="8c318-306">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery ověření 1.14.0")
- [<span data-ttu-id="8c318-307">jQuery ověřením 1.13.1</span><span class="sxs-lookup"><span data-stu-id="8c318-307">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery ověření 1.13.1")
- [<span data-ttu-id="8c318-308">jQuery ověřením 1.13.0</span><span class="sxs-lookup"><span data-stu-id="8c318-308">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery ověření 1.13.0")
- [<span data-ttu-id="8c318-309">jQuery ověřením 1.12.0</span><span class="sxs-lookup"><span data-stu-id="8c318-309">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery ověření 1.12.0")
- [<span data-ttu-id="8c318-310">jQuery ověřením 1.11.1</span><span class="sxs-lookup"><span data-stu-id="8c318-310">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery ověření 1.11.1")
- [<span data-ttu-id="8c318-311">jQuery ověřením 1.11.0</span><span class="sxs-lookup"><span data-stu-id="8c318-311">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery ověření 1.11.0")
- [<span data-ttu-id="8c318-312">jQuery ověřením 1.10.0</span><span class="sxs-lookup"><span data-stu-id="8c318-312">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery ověření 1.10.0")
- [<span data-ttu-id="8c318-313">jQuery Validate 1.9</span><span class="sxs-lookup"><span data-stu-id="8c318-313">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate version 1.9")
- [<span data-ttu-id="8c318-314">jQuery Validate 1.8.1</span><span class="sxs-lookup"><span data-stu-id="8c318-314">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate version 1.8.1")
- [<span data-ttu-id="8c318-315">jQuery ověřením 1.8</span><span class="sxs-lookup"><span data-stu-id="8c318-315">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate verze 1.8")
- [<span data-ttu-id="8c318-316">jQuery Validate 1.7</span><span class="sxs-lookup"><span data-stu-id="8c318-316">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate version 1.7")
- [<span data-ttu-id="8c318-317">jQuery ověřením 1.6</span><span class="sxs-lookup"><span data-stu-id="8c318-317">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery ověřením 1.6")
- [<span data-ttu-id="8c318-318">jQuery ověřením 1.5.5</span><span class="sxs-lookup"><span data-stu-id="8c318-318">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery ověřením 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="8c318-319">jQuery Mobile verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-319">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="8c318-320">Následující verze knihovny jQuery Mobile jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="8c318-320">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="8c318-321">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="8c318-321">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="8c318-322">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="8c318-322">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-323">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="8c318-323">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-324">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="8c318-324">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-325">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="8c318-325">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-326">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="8c318-326">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-327">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="8c318-327">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-328">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="8c318-328">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-329">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="8c318-329">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-330">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="8c318-330">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-331">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="8c318-331">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-332">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="8c318-332">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-333">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="8c318-333">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-334">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="8c318-334">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-335">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="8c318-335">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-336">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="8c318-336">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-337">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="8c318-337">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 na Microsoft Ajax CDN")
- [<span data-ttu-id="8c318-338">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="8c318-338">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 na Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="8c318-339">jQuery šablony verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-339">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="8c318-340">Následující verze modulu plug-in jQuery šablony jsou hostovány na tento CDN.</span><span class="sxs-lookup"><span data-stu-id="8c318-340">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="8c318-341">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="8c318-341">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="8c318-342">jQuery šablony Beta 1</span><span class="sxs-lookup"><span data-stu-id="8c318-342">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery šablony Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="8c318-343">jQuery cyklus verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-343">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="8c318-344">Následující verze modulu plug-in jQuery cyklus jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="8c318-344">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="8c318-345">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="8c318-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="8c318-346">jQuery cyklus 2.99</span><span class="sxs-lookup"><span data-stu-id="8c318-346">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2.99 cyklu")
- [<span data-ttu-id="8c318-347">jQuery cyklus 2.94</span><span class="sxs-lookup"><span data-stu-id="8c318-347">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 cyklu")
- [<span data-ttu-id="8c318-348">jQuery cyklus 2,88</span><span class="sxs-lookup"><span data-stu-id="8c318-348">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 cyklu")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="8c318-349">jQuery DataTables verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-349">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="8c318-350">Následující verze modulu plug-in jQuery DataTables jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="8c318-350">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="8c318-351">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="8c318-351">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="8c318-352">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="8c318-352">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="8c318-353">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="8c318-353">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="8c318-354">jQuery DataTables otázku 1.9.4</span><span class="sxs-lookup"><span data-stu-id="8c318-354">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables otázku 1.9.4")
- [<span data-ttu-id="8c318-355">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="8c318-355">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="8c318-356">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="8c318-356">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="8c318-357">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="8c318-357">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="8c318-358">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="8c318-358">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="8c318-359">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="8c318-359">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="8c318-360">Verze Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-360">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="8c318-361">Následující verze systému [Modernizr](http://www.modernizr.com "Modernizr") jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-361">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="8c318-362">Verze JSHint na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-362">JSHint Releases on the CDN</span></span>

<span data-ttu-id="8c318-363">Následující verze systému [JSHint](http://www.jshint.com "JSHint") jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-363">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="8c318-364">Verze knockout na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-364">Knockout Releases on the CDN</span></span>

<span data-ttu-id="8c318-365">Následující verze systému [Knockout](http://www.knockoutjs.com "Knockout") jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-365">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="8c318-366">Globalizace verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-366">Globalize Releases on the CDN</span></span>

<span data-ttu-id="8c318-367">Následující verze systému [Globalize](https://github.com/jquery/globalize "Globalize") jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-367">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="8c318-368">Globalizace verze 1.0.0</span><span class="sxs-lookup"><span data-stu-id="8c318-368">Globalize version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="8c318-369">Globalizace verze 0.1.1</span><span class="sxs-lookup"><span data-stu-id="8c318-369">Globalize version 0.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="8c318-370">všechny jazykové verze</span><span class="sxs-lookup"><span data-stu-id="8c318-370">all cultures</span></span>
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="8c318-371">"{Jazyková verze kódu}" nahraďte kód požadovanou jazykovou verzi, například Microsoft globalize.culture.en GB.js== soubory CDN == tyto knihovny byly odeslaný společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8c318-371">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="8c318-372">Odpověď verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-372">Respond Releases on the CDN</span></span>

<span data-ttu-id="8c318-373">Následující verze systému [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") reakce hostovaných na CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-373">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="8c318-374">Verze 1.4.2 reagovat</span><span class="sxs-lookup"><span data-stu-id="8c318-374">Respond version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="8c318-375">Verze 1.4.1 reagovat</span><span class="sxs-lookup"><span data-stu-id="8c318-375">Respond version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="8c318-376">Verze 1.4.0 reagovat</span><span class="sxs-lookup"><span data-stu-id="8c318-376">Respond version 1.4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="8c318-377">Verze 1.3.0 reagovat</span><span class="sxs-lookup"><span data-stu-id="8c318-377">Respond version 1.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="8c318-378">Verze 1.2.0 reagovat</span><span class="sxs-lookup"><span data-stu-id="8c318-378">Respond version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="8c318-379">Zavedení verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-379">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="8c318-380">Následující verze systému [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap hostovaných na CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-380">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-400"></a><span data-ttu-id="8c318-381">Zavedení verze 4.0.0</span><span class="sxs-lookup"><span data-stu-id="8c318-381">Bootstrap version 4.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="8c318-382">Zavedení verze 3.3.7</span><span class="sxs-lookup"><span data-stu-id="8c318-382">Bootstrap version 3.3.7</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="8c318-383">Zavedení verze 3.3.6</span><span class="sxs-lookup"><span data-stu-id="8c318-383">Bootstrap version 3.3.6</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="8c318-384">Zavedení verze 3.3.5</span><span class="sxs-lookup"><span data-stu-id="8c318-384">Bootstrap version 3.3.5</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="8c318-385">Zavedení verze 3.3.4</span><span class="sxs-lookup"><span data-stu-id="8c318-385">Bootstrap version 3.3.4</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="8c318-386">Zavedení verze 3.3.2</span><span class="sxs-lookup"><span data-stu-id="8c318-386">Bootstrap version 3.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="8c318-387">Zavedení verze 3.3.1</span><span class="sxs-lookup"><span data-stu-id="8c318-387">Bootstrap version 3.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="8c318-388">Zavedení verze 3.3.0</span><span class="sxs-lookup"><span data-stu-id="8c318-388">Bootstrap version 3.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="8c318-389">Zavedení verze 3.2.0</span><span class="sxs-lookup"><span data-stu-id="8c318-389">Bootstrap version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="8c318-390">Zavedení verze 3.1.1</span><span class="sxs-lookup"><span data-stu-id="8c318-390">Bootstrap version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="8c318-391">Zavedení verze 3.1.0</span><span class="sxs-lookup"><span data-stu-id="8c318-391">Bootstrap version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="8c318-392">Zavedení verze 3.0.3</span><span class="sxs-lookup"><span data-stu-id="8c318-392">Bootstrap version 3.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="8c318-393">Zavedení verze 3.0.2</span><span class="sxs-lookup"><span data-stu-id="8c318-393">Bootstrap version 3.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="8c318-394">Zavedení verze 3.0.1</span><span class="sxs-lookup"><span data-stu-id="8c318-394">Bootstrap version 3.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="8c318-395">Zavedení verze 3.0.0</span><span class="sxs-lookup"><span data-stu-id="8c318-395">Bootstrap version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="8c318-396">Zavedení verze 2.3.2</span><span class="sxs-lookup"><span data-stu-id="8c318-396">Bootstrap version 2.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="8c318-397">Zavedení verze 2.3.1</span><span class="sxs-lookup"><span data-stu-id="8c318-397">Bootstrap version 2.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="8c318-398">Zavedení TouchCarousel verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-398">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="8c318-399">Následující verze systému [ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ") Bootstrap TouchCarousel verze jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-399">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="8c318-400">Zavedení TouchCarousel verze 0.8.0</span><span class="sxs-lookup"><span data-stu-id="8c318-400">Bootstrap TouchCarousel version 0.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="8c318-401">Verze hammer.js na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-401">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="8c318-402">Následující verze systému [ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js verze jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-402">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="8c318-403">Hammer.js version 2.0.4</span><span class="sxs-lookup"><span data-stu-id="8c318-403">Hammer.js version 2.0.4</span></span>

- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="8c318-404">Webové formuláře ASP.NET a Ajax verze na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-404">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="8c318-405">Následující verze knihovny ASP.NET Ajax jsou hostované na CDN.</span><span class="sxs-lookup"><span data-stu-id="8c318-405">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="8c318-406">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="8c318-406">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="8c318-407">Webové formuláře ASP.NET a Ajax verze 4.5.2</span><span class="sxs-lookup"><span data-stu-id="8c318-407">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "webových formulářů ASP.NET a Ajax 4.5.2")
- [<span data-ttu-id="8c318-408">Webové formuláře ASP.NET a Ajax verze 4</span><span class="sxs-lookup"><span data-stu-id="8c318-408">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "webových formulářů ASP.NET a Ajax 4")
- [<span data-ttu-id="8c318-409">Technologie ASP.NET Ajax verze 3.5</span><span class="sxs-lookup"><span data-stu-id="8c318-409">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "prvku ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="8c318-410">ASP.NET MVC uvolní na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-410">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="8c318-411">Následující soubory ASP.NET MVC JavaScript jsou hostované na této CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-411">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="8c318-412">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="8c318-412">ASP.NET MVC 5.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="8c318-413">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="8c318-413">ASP.NET MVC 5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="8c318-414">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="8c318-414">ASP.NET MVC 5.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="8c318-415">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="8c318-415">ASP.NET MVC 4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="8c318-416">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="8c318-416">ASP.NET MVC 3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="8c318-417">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="8c318-417">ASP.NET MVC 2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="8c318-418">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="8c318-418">ASP.NET MVC 1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="8c318-419">Funkce SignalR technologie ASP.NET uvolní na CDN</span><span class="sxs-lookup"><span data-stu-id="8c318-419">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="8c318-420">Následující soubory ASP.NET SignalR JavaScript jsou hostované na této CDN:</span><span class="sxs-lookup"><span data-stu-id="8c318-420">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="8c318-421">Funkce SignalR technologie ASP.NET 2.2.2</span><span class="sxs-lookup"><span data-stu-id="8c318-421">ASP.NET SignalR 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="8c318-422">Funkce SignalR technologie ASP.NET 2.2.1</span><span class="sxs-lookup"><span data-stu-id="8c318-422">ASP.NET SignalR 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="8c318-423">Funkce SignalR technologie ASP.NET 2.2.0</span><span class="sxs-lookup"><span data-stu-id="8c318-423">ASP.NET SignalR 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="8c318-424">Funkce SignalR technologie ASP.NET 2.1.0</span><span class="sxs-lookup"><span data-stu-id="8c318-424">ASP.NET SignalR 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="8c318-425">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="8c318-425">ASP.NET SignalR 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="8c318-426">Funkce SignalR technologie ASP.NET bodu 2.0.2</span><span class="sxs-lookup"><span data-stu-id="8c318-426">ASP.NET SignalR 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="8c318-427">Funkce SignalR technologie ASP.NET 2.0.1</span><span class="sxs-lookup"><span data-stu-id="8c318-427">ASP.NET SignalR 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="8c318-428">Funkce SignalR technologie ASP.NET 2.0.0</span><span class="sxs-lookup"><span data-stu-id="8c318-428">ASP.NET SignalR 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="8c318-429">Funkce SignalR technologie ASP.NET 1.1.3</span><span class="sxs-lookup"><span data-stu-id="8c318-429">ASP.NET SignalR 1.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="8c318-430">Funkce SignalR technologie ASP.NET 1.1.2</span><span class="sxs-lookup"><span data-stu-id="8c318-430">ASP.NET SignalR 1.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="8c318-431">Funkce SignalR technologie ASP.NET 1.1.1</span><span class="sxs-lookup"><span data-stu-id="8c318-431">ASP.NET SignalR 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="8c318-432">Funkce SignalR technologie ASP.NET 1.1.0</span><span class="sxs-lookup"><span data-stu-id="8c318-432">ASP.NET SignalR 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="8c318-433">Funkce SignalR technologie ASP.NET 1.0.1</span><span class="sxs-lookup"><span data-stu-id="8c318-433">ASP.NET SignalR 1.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="8c318-434">Informace o podmínky použití pro CDN najdete v tématu [Microsoft Ajax CDN podmínky použití](https://www.asp.net/terms-of-use "Microsoft Ajax CDN podmínky použití").</span><span class="sxs-lookup"><span data-stu-id="8c318-434">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
