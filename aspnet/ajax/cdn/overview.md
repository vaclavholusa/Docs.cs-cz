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
ms.openlocfilehash: 2955888bc745fa3d49e40d364196283f16ad95ef
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="652d1-102">Sítě pro doručování obsahu Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="652d1-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="652d1-103">Poznámka: Microsoft Ajax CDN nemá žádné SLA vedle pomocí Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="652d1-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="652d1-104">Obsah</span><span class="sxs-lookup"><span data-stu-id="652d1-104">Table of Contents</span></span>

<span data-ttu-id="652d1-105">**[adresu AJAX.microsoft.com přejmenován na ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="652d1-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="652d1-106">**[Visual Studio .vsdoc podpora](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="652d1-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="652d1-107">**[Pomocí prvku ASP.NET Ajax od CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="652d1-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="652d1-108">**[Pomocí jQuery od CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="652d1-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="652d1-109">**[Pomocí jQuery uživatelského rozhraní od CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="652d1-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="652d1-110">**[Soubory třetích stran na CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="652d1-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="652d1-111">Verze jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="652d1-112">jQuery migrovat verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="652d1-113">jQuery verze uživatelského rozhraní na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="652d1-114">jQuery ověření verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="652d1-115">jQuery Mobile verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="652d1-116">jQuery šablony verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="652d1-117">jQuery cyklus verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="652d1-118">jQuery DataTables verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="652d1-119">Verze Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="652d1-120">Verze JSHint na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="652d1-121">Verze knockout na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="652d1-122">Globalizace verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="652d1-123">Odpověď verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="652d1-124">Zavedení verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="652d1-125">Zavedení TouchCarousel verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="652d1-126">Verze hammer.js na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="652d1-127">Webové formuláře ASP.NET a Ajax verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="652d1-128">ASP.NET MVC uvolní na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="652d1-129">Funkce SignalR technologie ASP.NET uvolní na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="652d1-130">Microsoft Ajax Content Delivery Network (CDN) hostuje knihoven jazyka JavaScript oblíbených třetích stran, například jQuery a umožňuje snadno přidat je do webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="652d1-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="652d1-131">Například můžete začít používat jQuery, který je hostován na tento CDN jednoduše přidáním &lt;skriptu&gt; značky na stránku, která odkazuje na ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="652d1-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="652d1-132">Využitím CDN, může výrazně zlepšit výkon aplikací Ajax.</span><span class="sxs-lookup"><span data-stu-id="652d1-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="652d1-133">Obsah CDN ukládají do mezipaměti na servery umístěné po celém světě.</span><span class="sxs-lookup"><span data-stu-id="652d1-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="652d1-134">Kromě toho CDN umožňuje prohlížečům soubory uložené v mezipaměti třetích stran JavaScript pro webové stránky, které se nacházejí v různých doménách.</span><span class="sxs-lookup"><span data-stu-id="652d1-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="652d1-135">CDN podporuje protokol SSL (HTTPS), v případě, že budete muset poskytnout webovou stránku pomocí protokolu Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="652d1-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="652d1-136">CDN hostitelem následující knihovny skriptů třetích stran, které byly odeslány a licenci, vlastníky tyto knihovny:</span><span class="sxs-lookup"><span data-stu-id="652d1-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="652d1-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="652d1-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="652d1-138">jQuery uživatelského rozhraní (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="652d1-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="652d1-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="652d1-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="652d1-140">jQuery ověření (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="652d1-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="652d1-141">jQuery cyklus (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="652d1-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="652d1-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="652d1-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="652d1-143">Microsoft Ajax CDN také zahrnuje následující knihovny, které byly odeslány společnosti Microsoft:</span><span class="sxs-lookup"><span data-stu-id="652d1-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="652d1-144">Technologie ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="652d1-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="652d1-145">Soubory JavaScript rozhraní ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="652d1-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="652d1-146">Soubory ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="652d1-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="652d1-147">Microsoft nenárokuje vlastnictví všech knihoven jiných výrobců hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="652d1-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="652d1-148">Tyto knihovny pro vás jsou licencování autorským vlastníky knihoven.</span><span class="sxs-lookup"><span data-stu-id="652d1-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="652d1-149">Žádná práva, které možná budete muset stáhnout a použít tyto knihovny jsou udělit výhradně pomocí příslušných vlastníků autorských práv.</span><span class="sxs-lookup"><span data-stu-id="652d1-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="652d1-150">Protože se nejedná knihovny Microsoft, společnost Microsoft poskytuje žádné záruky ani licence právy duševního vlastnictví (včetně žádné předpokládané patentová práva) pro hostované na této CDN knihovny třetích stran.</span><span class="sxs-lookup"><span data-stu-id="652d1-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="652d1-151">Pokud chcete odeslat vaše knihovna JavaScript a vaše knihovna je jedním z hlavní knihovny jazyka JavaScript (jak je uvedeno v http://trends.builtwith.com) nebo rozšíření nebo moduly plug-in na tyto knihovny, které jsou (a) oblíbených; nebo (b) užitečné při použití technologie ASP.NET a obraťte se na AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="652d1-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="652d1-152">adresu AJAX.microsoft.com přejmenován na ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="652d1-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="652d1-153">CDN lze použít název domény webu microsoft.com a byl změněn na použít název domény aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="652d1-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="652d1-154">Tato změna byla provedená zvýšit výkon, protože když prohlížeč doménu microsoft.com by se odesílat žádné soubory cookie z této domény přes přenosu s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="652d1-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="652d1-155">Název domény. než microsoft.com přejmenováním možné zvýšit výkon co nejvíc 25 %.</span><span class="sxs-lookup"><span data-stu-id="652d1-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="652d1-156">Poznamenejte si adresu ajax.microsoft.com budou nadále fungovat, ale doporučuje se ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="652d1-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="652d1-157">Starý formát: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="652d1-158">Nový formát: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="652d1-159">Visual Studio .vsdoc podpora</span><span class="sxs-lookup"><span data-stu-id="652d1-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="652d1-160">Použít soubory .vsdoc správně s Visual Studio 2008, musíte zajistit, že máte VS 2008 SP1 nainstalovány a opravy hotfix pro vsdoc soubory.</span><span class="sxs-lookup"><span data-stu-id="652d1-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="652d1-161">Můžete získat z tohoto místa:</span><span class="sxs-lookup"><span data-stu-id="652d1-161">You can get these from here:</span></span>

- [<span data-ttu-id="652d1-162">Stáhněte si Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="652d1-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Stáhnout Visual Studio 2008 SP1")
- [<span data-ttu-id="652d1-163">Stažení opravy hotfix .vsdoc pro Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="652d1-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "stažení opravy hotfix .vsdoc pro Visual Studio 2008 SP1")

<span data-ttu-id="652d1-164">Visual Studio 2010 podporuje .vsdoc soubory bez žádné další opravy.</span><span class="sxs-lookup"><span data-stu-id="652d1-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="652d1-165">Pomocí prvku ASP.NET Ajax od CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="652d1-166">Pokud používáte technologii ASP.NET 4, můžete přesměrovat všechny požadavky pro skripty framework ASP.NET do CDN.</span><span class="sxs-lookup"><span data-stu-id="652d1-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="652d1-167">Načítání skripty od CDN namísto místního webového serveru můžete výrazně zlepšit výkon veřejné weby technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="652d1-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="652d1-168">Pomocí vlastnosti ScriptManager EnableCDN přesměrovat všechny požadavky skriptu framework ASP.NET Ajax CDN společnosti Microsoft:</span><span class="sxs-lookup"><span data-stu-id="652d1-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="652d1-169">Pomocí jQuery od CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="652d1-170">Můžete použít skripty jQuery hostované na CDN ve webové aplikaci přidáním následující skript element na stránce:</span><span class="sxs-lookup"><span data-stu-id="652d1-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="652d1-171">CDN také obsahuje minifikovaný verzi jQuery skript, který můžete získat pomocí následující element:</span><span class="sxs-lookup"><span data-stu-id="652d1-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="652d1-172">Pokud chcete, aby vaše stránky k záložnímu načítání jQuery z místní cestu na vlastní web, pokud není k dispozici CDN se stane, přidejte následující element ihned po element odkazující na CDN:</span><span class="sxs-lookup"><span data-stu-id="652d1-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="652d1-173">Na následující stránce Ukázka používá CDN verzi knihovny jQuery (s přechod na místní kopie) pro zobrazení obsahu elementu div při kliknutí na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="652d1-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="652d1-174">Můžete získat další informace o jQuery a stáhnout místní kopii jQuery navštivte stránky [jQuery](http://jquery.com/) webu.</span><span class="sxs-lookup"><span data-stu-id="652d1-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="652d1-175">Pomocí jQuery uživatelského rozhraní od CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="652d1-176">CDN také hostitelem knihovna uživatelského rozhraní jQuery.</span><span class="sxs-lookup"><span data-stu-id="652d1-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="652d1-177">Knihovna uživatelského rozhraní jQuery zahrnuje širokou škálu pomůcek a účinky, které můžete použít v aplikacích ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="652d1-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="652d1-178">Například na následující stránce ukazuje, jak je možné používat jQuery ovládací prvek Datepicker uživatelského rozhraní v kontextu aplikace webových formulářů ASP.NET a zobrazit automaticky otevírané okno Kalendář:</span><span class="sxs-lookup"><span data-stu-id="652d1-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="652d1-179">Pokud přesunete fokus do textového pole pomocí klávesnice, zobrazí se kalendáře:</span><span class="sxs-lookup"><span data-stu-id="652d1-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Kalendářem vytvořené pomocí ovládací prvek Datepicker](overview/_static/image1.png)

<span data-ttu-id="652d1-181">Všimněte si, že musí obsahovat tři soubory od CDN ve výše uvedeném kódu:</span><span class="sxs-lookup"><span data-stu-id="652d1-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="652d1-182">Knihovna jQuery &mdash; knihovna uživatelského rozhraní jQuery závisí na knihovny jQuery.</span><span class="sxs-lookup"><span data-stu-id="652d1-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="652d1-183">Knihovna jQuery je nutné přidat na stránku předtím, než přidáte knihovna uživatelského rozhraní jQuery.</span><span class="sxs-lookup"><span data-stu-id="652d1-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="652d1-184">Knihovna uživatelského rozhraní jQuery &mdash; knihovna uživatelského rozhraní jQuery obsahuje všechny důsledky uživatelského rozhraní jQuery a pomůcky, jako je například widgetu ovládací prvek Datepicker použít na stránce nahoře.</span><span class="sxs-lookup"><span data-stu-id="652d1-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="652d1-185">Motiv uživatelského rozhraní jQuery &mdash; jQuery UI podporuje různé motivy.</span><span class="sxs-lookup"><span data-stu-id="652d1-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="652d1-186">Výše uvedené stránka obsahuje odkaz na soubor k importu Redmond motiv šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="652d1-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="652d1-187">Všechny standardní jQuery UI motivy jsou hostované na CDN.</span><span class="sxs-lookup"><span data-stu-id="652d1-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="652d1-188">[Navštivte tuto stránku](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 na Microsoft Ajax CDN") Chcete-li zobrazit miniatury pro každý motiv.</span><span class="sxs-lookup"><span data-stu-id="652d1-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="652d1-189">Další informace o knihovna uživatelského rozhraní jQuery, najdete v oficiální [webu uživatelského rozhraní jQuery](http://jQueryUI.com "webu uživatelského rozhraní jQuery").</span><span class="sxs-lookup"><span data-stu-id="652d1-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="652d1-190">Soubory třetích stran na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="652d1-191">CDN hostitelem některé z nejčastěji používané knihovny JavaScript třetích stran.</span><span class="sxs-lookup"><span data-stu-id="652d1-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="652d1-192">Microsoft nenárokuje vlastnictví všech knihoven jiných výrobců hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="652d1-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="652d1-193">Tyto knihovny pro vás jsou licencování autorským vlastníky knihoven.</span><span class="sxs-lookup"><span data-stu-id="652d1-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="652d1-194">Žádná práva, které možná budete muset stáhnout a použít tyto knihovny jsou udělit výhradně pomocí příslušných vlastníků autorských práv.</span><span class="sxs-lookup"><span data-stu-id="652d1-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="652d1-195">Protože se nejedná knihovny Microsoft, společnost Microsoft poskytuje žádné záruky ani licence právy duševního vlastnictví (včetně žádné předpokládané patentová práva) pro hostované na této CDN knihovny třetích stran.</span><span class="sxs-lookup"><span data-stu-id="652d1-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="652d1-196">Verze jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="652d1-197">Následující verze jQuery hostovaných na CDN:</span><span class="sxs-lookup"><span data-stu-id="652d1-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="652d1-198">verze jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="652d1-198">jQuery version 3.2.1</span></span>
- <span data-ttu-id="652d1-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="652d1-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="652d1-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="652d1-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="652d1-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="652d1-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="652d1-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="652d1-205">verze jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="652d1-205">jQuery version 3.2.0</span></span>

- <span data-ttu-id="652d1-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="652d1-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="652d1-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="652d1-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="652d1-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="652d1-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="652d1-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="652d1-212">verze jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="652d1-212">jQuery version 3.1.1</span></span>

- <span data-ttu-id="652d1-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="652d1-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="652d1-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="652d1-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="652d1-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="652d1-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="652d1-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="652d1-219">verze jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="652d1-219">jQuery version 3.1.0</span></span>

- <span data-ttu-id="652d1-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="652d1-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="652d1-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="652d1-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="652d1-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="652d1-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="652d1-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="652d1-226">verze jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="652d1-226">jQuery version 3.0.0</span></span>

- <span data-ttu-id="652d1-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="652d1-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="652d1-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="652d1-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="652d1-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="652d1-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="652d1-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="652d1-233">verze jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="652d1-233">jQuery version 2.2.4</span></span>

- <span data-ttu-id="652d1-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="652d1-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="652d1-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="652d1-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="652d1-237">verze jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="652d1-237">jQuery version 2.2.3</span></span>

- <span data-ttu-id="652d1-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="652d1-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="652d1-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="652d1-240">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-240">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="652d1-241">verze jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="652d1-241">jQuery version 2.2.2</span></span>

- <span data-ttu-id="652d1-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="652d1-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="652d1-244">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-244">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="652d1-245">verze jQuery 2.2.1</span><span class="sxs-lookup"><span data-stu-id="652d1-245">jQuery version 2.2.1</span></span>

- <span data-ttu-id="652d1-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="652d1-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="652d1-248">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-248">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="652d1-249">verze jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="652d1-249">jQuery version 2.2.0</span></span>

- <span data-ttu-id="652d1-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="652d1-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="652d1-252">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-252">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="652d1-253">verze jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="652d1-253">jQuery version 2.1.4</span></span>

- <span data-ttu-id="652d1-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="652d1-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="652d1-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="652d1-256">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-256">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="652d1-257">verze jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="652d1-257">jQuery version 2.1.3</span></span>

- <span data-ttu-id="652d1-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="652d1-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="652d1-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="652d1-260">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-260">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="652d1-261">verze jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="652d1-261">jQuery version 2.1.2</span></span>

- <span data-ttu-id="652d1-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="652d1-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="652d1-264">verze jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="652d1-264">jQuery version 2.1.1</span></span>

- <span data-ttu-id="652d1-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="652d1-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="652d1-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="652d1-268">verze jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="652d1-268">jQuery version 2.1.0</span></span>

- <span data-ttu-id="652d1-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="652d1-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="652d1-271">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-271">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="652d1-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="652d1-273">verze jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="652d1-273">jQuery version 2.0.3</span></span>

- <span data-ttu-id="652d1-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="652d1-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="652d1-275">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-275">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="652d1-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="652d1-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="652d1-278">jQuery verze bodu 2.0.2</span><span class="sxs-lookup"><span data-stu-id="652d1-278">jQuery version 2.0.2</span></span>

- <span data-ttu-id="652d1-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="652d1-280">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-280">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="652d1-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="652d1-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="652d1-283">jQuery verze 2.0.1</span><span class="sxs-lookup"><span data-stu-id="652d1-283">jQuery version 2.0.1</span></span>

- <span data-ttu-id="652d1-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="652d1-285">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-285">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="652d1-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="652d1-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="652d1-288">verze jQuery 2.0.0</span><span class="sxs-lookup"><span data-stu-id="652d1-288">jQuery version 2.0.0</span></span>

- <span data-ttu-id="652d1-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="652d1-290">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-290">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="652d1-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="652d1-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="652d1-293">verze jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="652d1-293">jQuery version 1.12.4</span></span>

- <span data-ttu-id="652d1-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="652d1-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="652d1-295">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-295">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="652d1-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="652d1-297">verze jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="652d1-297">jQuery version 1.12.3</span></span>

- <span data-ttu-id="652d1-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="652d1-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="652d1-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="652d1-300">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-300">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="652d1-301">verze jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="652d1-301">jQuery version 1.12.2</span></span>

- <span data-ttu-id="652d1-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="652d1-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="652d1-304">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-304">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="652d1-305">verze jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="652d1-305">jQuery version 1.12.1</span></span>

- <span data-ttu-id="652d1-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="652d1-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="652d1-308">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-308">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="652d1-309">verze jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="652d1-309">jQuery version 1.12.0</span></span>

- <span data-ttu-id="652d1-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="652d1-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="652d1-312">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-312">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="652d1-313">verze jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="652d1-313">jQuery version 1.11.3</span></span>

- <span data-ttu-id="652d1-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="652d1-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="652d1-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="652d1-316">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-316">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="652d1-317">verze jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="652d1-317">jQuery version 1.11.2</span></span>

- <span data-ttu-id="652d1-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="652d1-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="652d1-320">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-320">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="652d1-321">verze jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="652d1-321">jQuery version 1.11.1</span></span>

- <span data-ttu-id="652d1-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="652d1-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="652d1-324">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-324">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="652d1-325">verze jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="652d1-325">jQuery version 1.11.0</span></span>

- <span data-ttu-id="652d1-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="652d1-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="652d1-328">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-328">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="652d1-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="652d1-330">verze jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="652d1-330">jQuery version 1.10.2</span></span>

- <span data-ttu-id="652d1-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="652d1-332">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-332">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="652d1-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="652d1-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="652d1-335">verze jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="652d1-335">jQuery version 1.10.1</span></span>

- <span data-ttu-id="652d1-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="652d1-337">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-337">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="652d1-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="652d1-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="652d1-340">verze jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="652d1-340">jQuery version 1.10.0</span></span>

- <span data-ttu-id="652d1-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="652d1-342">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-342">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="652d1-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="652d1-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="652d1-345">verze jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="652d1-345">jQuery version 1.9.1</span></span>

- <span data-ttu-id="652d1-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="652d1-347">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-347">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="652d1-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="652d1-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="652d1-350">verze jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="652d1-350">jQuery version 1.9.0</span></span>

- <span data-ttu-id="652d1-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="652d1-352">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-352">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="652d1-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="652d1-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="652d1-355">verze jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="652d1-355">jQuery version 1.8.3</span></span>

- <span data-ttu-id="652d1-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="652d1-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="652d1-357">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-357">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="652d1-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="652d1-359">verze jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="652d1-359">jQuery version 1.8.2</span></span>

- <span data-ttu-id="652d1-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="652d1-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="652d1-362">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-362">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="652d1-363">verze jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="652d1-363">jQuery version 1.8.1</span></span>

- <span data-ttu-id="652d1-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="652d1-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="652d1-366">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-366">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="652d1-367">verze jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="652d1-367">jQuery version 1.8.0</span></span>

- <span data-ttu-id="652d1-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="652d1-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="652d1-370">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-370">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="652d1-371">verze jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="652d1-371">jQuery version 1.7.2</span></span>

- <span data-ttu-id="652d1-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="652d1-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="652d1-374">verze jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="652d1-374">jQuery version 1.7.1</span></span>

- <span data-ttu-id="652d1-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="652d1-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="652d1-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="652d1-378">verze jQuery 1.7</span><span class="sxs-lookup"><span data-stu-id="652d1-378">jQuery version 1.7</span></span>

- <span data-ttu-id="652d1-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="652d1-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="652d1-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="652d1-381">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-381">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="652d1-382">verze jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="652d1-382">jQuery version 1.6.4</span></span>

- <span data-ttu-id="652d1-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="652d1-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="652d1-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="652d1-385">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-385">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="652d1-386">verze jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="652d1-386">jQuery version 1.6.3</span></span>

- <span data-ttu-id="652d1-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="652d1-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="652d1-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="652d1-389">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-389">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="652d1-390">verze jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="652d1-390">jQuery version 1.6.2</span></span>

- <span data-ttu-id="652d1-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="652d1-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="652d1-393">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-393">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="652d1-394">jQuery verze 1.6.1</span><span class="sxs-lookup"><span data-stu-id="652d1-394">jQuery version 1.6.1</span></span>

- <span data-ttu-id="652d1-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="652d1-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="652d1-397">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-397">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="652d1-398">jQuery verzi 1.6</span><span class="sxs-lookup"><span data-stu-id="652d1-398">jQuery version 1.6</span></span>

- <span data-ttu-id="652d1-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="652d1-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="652d1-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="652d1-401">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-401">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="652d1-402">verze jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="652d1-402">jQuery version 1.5.2</span></span>

- <span data-ttu-id="652d1-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="652d1-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="652d1-405">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-405">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="652d1-406">verze jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="652d1-406">jQuery version 1.5.1</span></span>

- <span data-ttu-id="652d1-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="652d1-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="652d1-409">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-409">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="652d1-410">verze jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="652d1-410">jQuery version 1.5</span></span>

- <span data-ttu-id="652d1-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="652d1-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="652d1-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="652d1-413">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-413">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="652d1-414">verze jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="652d1-414">jQuery version 1.4.4</span></span>

- <span data-ttu-id="652d1-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="652d1-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="652d1-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="652d1-417">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-417">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="652d1-418">verze jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="652d1-418">jQuery version 1.4.3</span></span>

- <span data-ttu-id="652d1-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="652d1-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="652d1-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="652d1-421">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-421">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="652d1-422">jQuery verze 1.4.2</span><span class="sxs-lookup"><span data-stu-id="652d1-422">jQuery version 1.4.2</span></span>

- <span data-ttu-id="652d1-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="652d1-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="652d1-425">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-425">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="652d1-426">verze jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="652d1-426">jQuery version 1.4.1</span></span>

- <span data-ttu-id="652d1-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="652d1-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="652d1-429">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-429">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="652d1-430">jQuery verze 1.4</span><span class="sxs-lookup"><span data-stu-id="652d1-430">jQuery version 1.4</span></span>

- <span data-ttu-id="652d1-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="652d1-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="652d1-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="652d1-433">jQuery verze 1.3.2</span><span class="sxs-lookup"><span data-stu-id="652d1-433">jQuery version 1.3.2</span></span>

- <span data-ttu-id="652d1-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="652d1-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="652d1-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="652d1-437">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="652d1-437">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="652d1-438">jQuery migrovat verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-438">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="652d1-439">Následující verze jQuery migrací jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="652d1-439">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="652d1-440">jQuery migrací verze 3.0.0</span><span class="sxs-lookup"><span data-stu-id="652d1-440">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="652d1-441">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-441">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="652d1-442">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-442">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="652d1-443">jQuery migrací verze 1.2.1</span><span class="sxs-lookup"><span data-stu-id="652d1-443">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="652d1-444">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-444">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="652d1-445">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-445">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="652d1-446">jQuery migrací verze 1.2.0</span><span class="sxs-lookup"><span data-stu-id="652d1-446">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="652d1-447">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-447">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="652d1-448">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="652d1-449">jQuery migrací verze 1.1.1</span><span class="sxs-lookup"><span data-stu-id="652d1-449">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="652d1-450">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-450">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="652d1-451">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="652d1-452">jQuery migrací verze 1.1.0</span><span class="sxs-lookup"><span data-stu-id="652d1-452">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="652d1-453">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-453">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="652d1-454">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="652d1-455">jQuery migrací verze 1.0.0</span><span class="sxs-lookup"><span data-stu-id="652d1-455">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="652d1-456">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-456">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="652d1-457">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="652d1-458">jQuery verze uživatelského rozhraní na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-458">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="652d1-459">Následující verze knihovny uživatelského rozhraní jQuery jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="652d1-459">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="652d1-460">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="652d1-460">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="652d1-461">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="652d1-461">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-462">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="652d1-462">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-463">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="652d1-463">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-464">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="652d1-464">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-465">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="652d1-465">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-466">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="652d1-466">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-467">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="652d1-467">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-468">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="652d1-468">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-469">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="652d1-469">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-470">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="652d1-470">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-471">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="652d1-471">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-472">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="652d1-472">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-473">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="652d1-473">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-474">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="652d1-474">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-475">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="652d1-475">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-476">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="652d1-476">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-477">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="652d1-477">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-478">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="652d1-478">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-479">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="652d1-479">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-480">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="652d1-480">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-481">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="652d1-481">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-482">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="652d1-482">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-483">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="652d1-483">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-484">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="652d1-484">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-485">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="652d1-485">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-486">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="652d1-486">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-487">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="652d1-487">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-488">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="652d1-488">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-489">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="652d1-489">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-490">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="652d1-490">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-491">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="652d1-491">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-492">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="652d1-492">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-493">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="652d1-493">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-494">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="652d1-494">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-495">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="652d1-495">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="652d1-496">jQuery ověření verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-496">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="652d1-497">Následující verze knihovny jQuery ověření jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="652d1-497">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="652d1-498">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="652d1-498">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="652d1-499">jQuery ověřením 1.17.0</span><span class="sxs-lookup"><span data-stu-id="652d1-499">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery ověření 1.17.0")
- [<span data-ttu-id="652d1-500">jQuery ověřením 1.16.0</span><span class="sxs-lookup"><span data-stu-id="652d1-500">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery ověření 1.16.0")
- [<span data-ttu-id="652d1-501">jQuery ověřením 1.15.1</span><span class="sxs-lookup"><span data-stu-id="652d1-501">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery ověření 1.15.1")
- [<span data-ttu-id="652d1-502">jQuery ověřením 1.15.0</span><span class="sxs-lookup"><span data-stu-id="652d1-502">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery ověření 1.15.0")
- [<span data-ttu-id="652d1-503">jQuery ověřením 1.14.0</span><span class="sxs-lookup"><span data-stu-id="652d1-503">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery ověření 1.14.0")
- [<span data-ttu-id="652d1-504">jQuery ověřením 1.13.1</span><span class="sxs-lookup"><span data-stu-id="652d1-504">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery ověření 1.13.1")
- [<span data-ttu-id="652d1-505">jQuery ověřením 1.13.0</span><span class="sxs-lookup"><span data-stu-id="652d1-505">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery ověření 1.13.0")
- [<span data-ttu-id="652d1-506">jQuery ověřením 1.12.0</span><span class="sxs-lookup"><span data-stu-id="652d1-506">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery ověření 1.12.0")
- [<span data-ttu-id="652d1-507">jQuery ověřením 1.11.1</span><span class="sxs-lookup"><span data-stu-id="652d1-507">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery ověření 1.11.1")
- [<span data-ttu-id="652d1-508">jQuery ověřením 1.11.0</span><span class="sxs-lookup"><span data-stu-id="652d1-508">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery ověření 1.11.0")
- [<span data-ttu-id="652d1-509">jQuery ověřením 1.10.0</span><span class="sxs-lookup"><span data-stu-id="652d1-509">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery ověření 1.10.0")
- [<span data-ttu-id="652d1-510">jQuery ověřením 1.9</span><span class="sxs-lookup"><span data-stu-id="652d1-510">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate verze 1.9")
- [<span data-ttu-id="652d1-511">jQuery ověřením 1.8.1</span><span class="sxs-lookup"><span data-stu-id="652d1-511">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate verze 1.8.1")
- [<span data-ttu-id="652d1-512">jQuery ověřením 1.8</span><span class="sxs-lookup"><span data-stu-id="652d1-512">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate verze 1.8")
- [<span data-ttu-id="652d1-513">jQuery ověřením 1.7</span><span class="sxs-lookup"><span data-stu-id="652d1-513">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate verze 1.7")
- [<span data-ttu-id="652d1-514">jQuery ověřením 1.6</span><span class="sxs-lookup"><span data-stu-id="652d1-514">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery ověřením 1.6")
- [<span data-ttu-id="652d1-515">jQuery ověřením 1.5.5</span><span class="sxs-lookup"><span data-stu-id="652d1-515">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery ověřením 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="652d1-516">jQuery Mobile verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-516">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="652d1-517">Následující verze knihovny jQuery Mobile jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="652d1-517">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="652d1-518">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="652d1-518">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="652d1-519">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="652d1-519">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-520">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="652d1-520">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-521">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="652d1-521">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-522">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="652d1-522">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-523">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="652d1-523">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-524">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="652d1-524">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-525">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="652d1-525">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-526">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="652d1-526">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-527">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="652d1-527">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-528">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="652d1-528">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-529">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="652d1-529">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-530">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="652d1-530">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-531">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="652d1-531">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-532">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="652d1-532">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-533">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="652d1-533">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-534">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="652d1-534">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 na Microsoft Ajax CDN")
- [<span data-ttu-id="652d1-535">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="652d1-535">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 na Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="652d1-536">jQuery šablony verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-536">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="652d1-537">Následující verze modulu plug-in jQuery šablony jsou hostovány na tento CDN.</span><span class="sxs-lookup"><span data-stu-id="652d1-537">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="652d1-538">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="652d1-538">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="652d1-539">jQuery šablony Beta 1</span><span class="sxs-lookup"><span data-stu-id="652d1-539">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery šablony Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="652d1-540">jQuery cyklus verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-540">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="652d1-541">Následující verze modulu plug-in jQuery cyklus jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="652d1-541">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="652d1-542">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="652d1-542">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="652d1-543">jQuery cyklus 2.99</span><span class="sxs-lookup"><span data-stu-id="652d1-543">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2.99 cyklu")
- [<span data-ttu-id="652d1-544">jQuery cyklus 2.94</span><span class="sxs-lookup"><span data-stu-id="652d1-544">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 cyklu")
- [<span data-ttu-id="652d1-545">jQuery cyklus 2,88</span><span class="sxs-lookup"><span data-stu-id="652d1-545">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 cyklu")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="652d1-546">jQuery DataTables verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-546">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="652d1-547">Následující verze modulu plug-in jQuery DataTables jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="652d1-547">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="652d1-548">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="652d1-548">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="652d1-549">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="652d1-549">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="652d1-550">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="652d1-550">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="652d1-551">jQuery DataTables otázku 1.9.4</span><span class="sxs-lookup"><span data-stu-id="652d1-551">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables otázku 1.9.4")
- [<span data-ttu-id="652d1-552">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="652d1-552">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="652d1-553">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="652d1-553">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="652d1-554">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="652d1-554">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="652d1-555">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="652d1-555">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="652d1-556">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="652d1-556">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="652d1-557">Verze Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-557">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="652d1-558">Následující verze systému [Modernizr](http://www.modernizr.com "Modernizr") jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="652d1-558">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="652d1-559">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="652d1-559">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="652d1-560">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-560">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="652d1-561">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-561">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="652d1-562">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-562">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="652d1-563">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="652d1-563">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="652d1-564">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="652d1-564">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="652d1-565">Verze JSHint na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-565">JSHint Releases on the CDN</span></span>

<span data-ttu-id="652d1-566">Následující verze systému [JSHint](http://www.jshint.com "JSHint") jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="652d1-566">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="652d1-567">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="652d1-567">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="652d1-568">Verze knockout na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-568">Knockout Releases on the CDN</span></span>

<span data-ttu-id="652d1-569">Následující verze systému [Knockout](http://www.knockoutjs.com "Knockout") jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="652d1-569">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="652d1-570">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-570">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="652d1-571">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-571">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="652d1-572">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-572">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="652d1-573">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-573">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="652d1-574">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-574">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="652d1-575">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-575">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="652d1-576">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-576">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="652d1-577">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="652d1-578">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="652d1-579">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="652d1-580">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="652d1-581">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="652d1-582">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="652d1-583">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="652d1-584">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="652d1-585">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="652d1-586">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="652d1-587">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="652d1-588">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="652d1-589">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="652d1-590">Globalizace verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-590">Globalize Releases on the CDN</span></span>

<span data-ttu-id="652d1-591">Následující verze systému [Globalize](https://github.com/jquery/globalize "Globalize") jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="652d1-591">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="652d1-592">Globalizace verze 1.0.0</span><span class="sxs-lookup"><span data-stu-id="652d1-592">Globalize version 1.0.0</span></span>

- <span data-ttu-id="652d1-593">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize.js</span><span class="sxs-lookup"><span data-stu-id="652d1-593">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="652d1-594">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/node-Main.js</span><span class="sxs-lookup"><span data-stu-id="652d1-594">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="652d1-595">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="652d1-595">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="652d1-596">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="652d1-596">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="652d1-597">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="652d1-597">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="652d1-598">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="652d1-598">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="652d1-599">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Plural.js</span><span class="sxs-lookup"><span data-stu-id="652d1-599">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="652d1-600">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Relative-Time.js</span><span class="sxs-lookup"><span data-stu-id="652d1-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="652d1-601">Globalizace verze 0.1.1</span><span class="sxs-lookup"><span data-stu-id="652d1-601">Globalize version 0.1.1</span></span>

- <span data-ttu-id="652d1-602">http://AJAX.aspnetcdn.com/AJAX/Globalize/0.1.1/Globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-602">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="652d1-603">http://AJAX.aspnetcdn.com/AJAX/Globalize/0.1.1/Globalize.js</span><span class="sxs-lookup"><span data-stu-id="652d1-603">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="652d1-604">http://AJAX.aspnetcdn.com/AJAX/Globalize/0.1.1/cultures/Globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="652d1-604">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="652d1-605">všechny jazykové verze</span><span class="sxs-lookup"><span data-stu-id="652d1-605">all cultures</span></span>
- <span data-ttu-id="652d1-606">http://AJAX.aspnetcdn.com/AJAX/Globalize/0.1.1/cultures/Globalize.Culture. {jazyková verze kódu} .js</span><span class="sxs-lookup"><span data-stu-id="652d1-606">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="652d1-607">"{Jazyková verze kódu}" nahraďte kód požadovanou jazykovou verzi, například Microsoft globalize.culture.en GB.js== soubory CDN == tyto knihovny byly odeslaný společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="652d1-607">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="652d1-608">Odpověď verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-608">Respond Releases on the CDN</span></span>

<span data-ttu-id="652d1-609">Následující verze systému [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") reakce hostovaných na CDN:</span><span class="sxs-lookup"><span data-stu-id="652d1-609">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="652d1-610">Verze 1.4.2 reagovat</span><span class="sxs-lookup"><span data-stu-id="652d1-610">Respond version 1.4.2</span></span>

- <span data-ttu-id="652d1-611">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.js</span><span class="sxs-lookup"><span data-stu-id="652d1-611">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="652d1-612">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-612">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="652d1-613">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="652d1-613">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="652d1-614">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-614">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="652d1-615">Verze 1.4.1 reagovat</span><span class="sxs-lookup"><span data-stu-id="652d1-615">Respond version 1.4.1</span></span>

- <span data-ttu-id="652d1-616">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.js</span><span class="sxs-lookup"><span data-stu-id="652d1-616">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="652d1-617">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-617">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="652d1-618">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="652d1-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="652d1-619">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="652d1-620">Verze 1.4.0 reagovat</span><span class="sxs-lookup"><span data-stu-id="652d1-620">Respond version 1.4.0</span></span>

- <span data-ttu-id="652d1-621">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="652d1-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="652d1-622">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-622">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="652d1-623">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="652d1-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="652d1-624">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="652d1-625">Verze 1.3.0 reagovat</span><span class="sxs-lookup"><span data-stu-id="652d1-625">Respond version 1.3.0</span></span>

- <span data-ttu-id="652d1-626">http://AJAX.aspnetcdn.com/AJAX/respond/1.3.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="652d1-626">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="652d1-627">Verze 1.2.0 reagovat</span><span class="sxs-lookup"><span data-stu-id="652d1-627">Respond version 1.2.0</span></span>

- <span data-ttu-id="652d1-628">http://AJAX.aspnetcdn.com/AJAX/respond/1.2.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="652d1-628">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="652d1-629">Zavedení verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-629">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="652d1-630">Následující verze systému [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap hostovaných na CDN:</span><span class="sxs-lookup"><span data-stu-id="652d1-630">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="652d1-631">Zavedení verze 3.3.7</span><span class="sxs-lookup"><span data-stu-id="652d1-631">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="652d1-632">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-632">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="652d1-633">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-633">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-634">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-634">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-635">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-635">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="652d1-636">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-636">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-637">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-637">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-638">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-638">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="652d1-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="652d1-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="652d1-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="652d1-645">Zavedení verze 3.3.6</span><span class="sxs-lookup"><span data-stu-id="652d1-645">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="652d1-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="652d1-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="652d1-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-652">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-652">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="652d1-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="652d1-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="652d1-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="652d1-659">Zavedení verze 3.3.5</span><span class="sxs-lookup"><span data-stu-id="652d1-659">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="652d1-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="652d1-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="652d1-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-666">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-666">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="652d1-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="652d1-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="652d1-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="652d1-673">Zavedení verze 3.3.4</span><span class="sxs-lookup"><span data-stu-id="652d1-673">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="652d1-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="652d1-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="652d1-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-680">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-680">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="652d1-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="652d1-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="652d1-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="652d1-687">Zavedení verze 3.3.2</span><span class="sxs-lookup"><span data-stu-id="652d1-687">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="652d1-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="652d1-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="652d1-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-694">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-694">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="652d1-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="652d1-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="652d1-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="652d1-701">Zavedení verze 3.3.1</span><span class="sxs-lookup"><span data-stu-id="652d1-701">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="652d1-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="652d1-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="652d1-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-708">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-708">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="652d1-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="652d1-714">Zavedení verze 3.3.0</span><span class="sxs-lookup"><span data-stu-id="652d1-714">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="652d1-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="652d1-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="652d1-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-721">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-721">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="652d1-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="652d1-727">Zavedení verze 3.2.0</span><span class="sxs-lookup"><span data-stu-id="652d1-727">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="652d1-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="652d1-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="652d1-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-734">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-734">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="652d1-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="652d1-740">Zavedení verze 3.1.1</span><span class="sxs-lookup"><span data-stu-id="652d1-740">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="652d1-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="652d1-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="652d1-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-747">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-747">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="652d1-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="652d1-753">Zavedení verze 3.1.0</span><span class="sxs-lookup"><span data-stu-id="652d1-753">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="652d1-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="652d1-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="652d1-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-760">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="652d1-760">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="652d1-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="652d1-766">Zavedení verze 3.0.3</span><span class="sxs-lookup"><span data-stu-id="652d1-766">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="652d1-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="652d1-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-773">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-773">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="652d1-777">Zavedení verze 3.0.2</span><span class="sxs-lookup"><span data-stu-id="652d1-777">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="652d1-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="652d1-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-784">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-784">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="652d1-788">Zavedení verze 3.0.1</span><span class="sxs-lookup"><span data-stu-id="652d1-788">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="652d1-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="652d1-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-795">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-795">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="652d1-799">Zavedení verze 3.0.0</span><span class="sxs-lookup"><span data-stu-id="652d1-799">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="652d1-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="652d1-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="652d1-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="652d1-806">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="652d1-806">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="652d1-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="652d1-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="652d1-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="652d1-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="652d1-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="652d1-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="652d1-810">Zavedení verze 2.3.2</span><span class="sxs-lookup"><span data-stu-id="652d1-810">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="652d1-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-811">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="652d1-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-812">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-813">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-814">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-815">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="652d1-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-816">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="652d1-817">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="652d1-817">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="652d1-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings-white.PNG</span><span class="sxs-lookup"><span data-stu-id="652d1-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="652d1-819">Zavedení verze 2.3.1</span><span class="sxs-lookup"><span data-stu-id="652d1-819">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="652d1-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="652d1-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="652d1-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="652d1-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="652d1-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="652d1-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="652d1-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="652d1-826">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="652d1-826">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="652d1-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings-white.PNG</span><span class="sxs-lookup"><span data-stu-id="652d1-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="652d1-828">Zavedení TouchCarousel verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-828">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="652d1-829">Následující verze systému [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel verze jsou hostované na CDN :</span><span class="sxs-lookup"><span data-stu-id="652d1-829">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="652d1-830">Zavedení TouchCarousel verze 0.8.0</span><span class="sxs-lookup"><span data-stu-id="652d1-830">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="652d1-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-touch-carousel/0.8.0/CSS/Bootstrap-touch-carousel.CSS</span><span class="sxs-lookup"><span data-stu-id="652d1-831">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="652d1-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-touch-carousel/0.8.0/js/Bootstrap-touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="652d1-832">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="652d1-833">Verze hammer.js na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-833">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="652d1-834">Následující verze systému [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js verze jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="652d1-834">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="652d1-835">Verze hammer.js verze 2.0.4</span><span class="sxs-lookup"><span data-stu-id="652d1-835">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="652d1-836">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="652d1-836">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="652d1-837">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-837">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="652d1-838">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.map</span><span class="sxs-lookup"><span data-stu-id="652d1-838">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="652d1-839">Webové formuláře ASP.NET a Ajax verze na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-839">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="652d1-840">Následující verze knihovny ASP.NET Ajax jsou hostované na CDN.</span><span class="sxs-lookup"><span data-stu-id="652d1-840">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="652d1-841">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="652d1-841">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="652d1-842">Webové formuláře ASP.NET a Ajax verze 4.5.2</span><span class="sxs-lookup"><span data-stu-id="652d1-842">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "webových formulářů ASP.NET a Ajax 4.5.2")
- [<span data-ttu-id="652d1-843">Webové formuláře ASP.NET a Ajax verze 4</span><span class="sxs-lookup"><span data-stu-id="652d1-843">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "webových formulářů ASP.NET a Ajax 4")
- [<span data-ttu-id="652d1-844">Technologie ASP.NET Ajax verze 3.5</span><span class="sxs-lookup"><span data-stu-id="652d1-844">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "prvku ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="652d1-845">ASP.NET MVC uvolní na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-845">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="652d1-846">Následující soubory ASP.NET MVC JavaScript jsou hostované na této CDN:</span><span class="sxs-lookup"><span data-stu-id="652d1-846">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="652d1-847">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="652d1-847">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="652d1-848">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="652d1-848">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="652d1-849">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-849">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="652d1-850">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="652d1-850">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="652d1-851">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="652d1-851">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="652d1-852">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-852">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="652d1-853">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="652d1-853">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="652d1-854">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="652d1-854">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="652d1-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-855">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="652d1-856">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="652d1-856">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="652d1-857">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="652d1-857">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="652d1-858">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-858">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="652d1-859">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="652d1-859">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="652d1-860">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="652d1-860">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="652d1-861">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-861">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="652d1-862">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="652d1-862">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="652d1-863">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-863">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="652d1-864">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="652d1-864">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="652d1-865">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-865">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="652d1-866">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="652d1-866">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="652d1-867">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="652d1-867">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="652d1-868">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-868">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="652d1-869">ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="652d1-869">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="652d1-870">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="652d1-870">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="652d1-871">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="652d1-871">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="652d1-872">Funkce SignalR technologie ASP.NET uvolní na CDN</span><span class="sxs-lookup"><span data-stu-id="652d1-872">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="652d1-873">Následující soubory ASP.NET SignalR JavaScript jsou hostované na této CDN:</span><span class="sxs-lookup"><span data-stu-id="652d1-873">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="652d1-874">Funkce SignalR technologie ASP.NET 2.2.2</span><span class="sxs-lookup"><span data-stu-id="652d1-874">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="652d1-875">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-875">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="652d1-876">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-876">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="652d1-877">Funkce SignalR technologie ASP.NET 2.2.1</span><span class="sxs-lookup"><span data-stu-id="652d1-877">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="652d1-878">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-878">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="652d1-879">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-879">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="652d1-880">Funkce SignalR technologie ASP.NET 2.2.0</span><span class="sxs-lookup"><span data-stu-id="652d1-880">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="652d1-881">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-881">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="652d1-882">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="652d1-883">Funkce SignalR technologie ASP.NET 2.1.0</span><span class="sxs-lookup"><span data-stu-id="652d1-883">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="652d1-884">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-884">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="652d1-885">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="652d1-886">Funkce SignalR technologie ASP.NET 2.0.3</span><span class="sxs-lookup"><span data-stu-id="652d1-886">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="652d1-887">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-887">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="652d1-888">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="652d1-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="652d1-889">Funkce SignalR technologie ASP.NET bodu 2.0.2</span><span class="sxs-lookup"><span data-stu-id="652d1-889">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="652d1-890">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-890">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="652d1-891">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="652d1-892">Funkce SignalR technologie ASP.NET 2.0.1</span><span class="sxs-lookup"><span data-stu-id="652d1-892">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="652d1-893">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-893">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="652d1-894">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="652d1-895">Funkce SignalR technologie ASP.NET 2.0.0</span><span class="sxs-lookup"><span data-stu-id="652d1-895">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="652d1-896">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-896">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="652d1-897">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="652d1-898">Funkce SignalR technologie ASP.NET 1.1.3</span><span class="sxs-lookup"><span data-stu-id="652d1-898">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="652d1-899">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-899">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="652d1-900">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="652d1-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="652d1-901">Funkce SignalR technologie ASP.NET 1.1.2</span><span class="sxs-lookup"><span data-stu-id="652d1-901">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="652d1-902">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-902">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="652d1-903">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="652d1-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="652d1-904">Funkce SignalR technologie ASP.NET 1.1.1</span><span class="sxs-lookup"><span data-stu-id="652d1-904">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="652d1-905">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-905">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="652d1-906">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="652d1-907">Funkce SignalR technologie ASP.NET 1.1.0</span><span class="sxs-lookup"><span data-stu-id="652d1-907">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="652d1-908">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-908">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="652d1-909">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="652d1-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="652d1-910">Funkce SignalR technologie ASP.NET 1.0.1</span><span class="sxs-lookup"><span data-stu-id="652d1-910">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="652d1-911">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="652d1-911">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="652d1-912">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="652d1-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="652d1-913">Informace o podmínky použití pro CDN najdete v tématu [Microsoft Ajax CDN podmínky použití](https://www.asp.net/terms-of-use "Microsoft Ajax CDN podmínky použití").</span><span class="sxs-lookup"><span data-stu-id="652d1-913">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
