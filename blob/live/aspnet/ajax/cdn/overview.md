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
ms.openlocfilehash: f69f707ba64d13fc372b7bc44718c9dcf8cec6e2
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="d2ddb-102">Sítě pro doručování obsahu Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="d2ddb-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="d2ddb-103">Poznámka: Microsoft Ajax CDN nemá žádné SLA vedle pomocí Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="d2ddb-104">Obsah</span><span class="sxs-lookup"><span data-stu-id="d2ddb-104">Table of Contents</span></span>

<span data-ttu-id="d2ddb-105">**[adresu AJAX.microsoft.com přejmenován na ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="d2ddb-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="d2ddb-106">**[Visual Studio .vsdoc podpora](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="d2ddb-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="d2ddb-107">**[Pomocí prvku ASP.NET Ajax od CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="d2ddb-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="d2ddb-108">**[Pomocí jQuery od CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="d2ddb-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="d2ddb-109">**[Pomocí jQuery uživatelského rozhraní od CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="d2ddb-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="d2ddb-110">**[Soubory třetích stran na CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="d2ddb-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="d2ddb-111">Verze jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="d2ddb-112">jQuery migrovat verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="d2ddb-113">jQuery verze uživatelského rozhraní na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="d2ddb-114">jQuery ověření verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="d2ddb-115">jQuery Mobile verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="d2ddb-116">jQuery šablony verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="d2ddb-117">jQuery cyklus verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="d2ddb-118">jQuery DataTables verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="d2ddb-119">Verze Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="d2ddb-120">Verze JSHint na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="d2ddb-121">Verze knockout na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="d2ddb-122">Globalizace verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="d2ddb-123">Odpověď verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="d2ddb-124">Zavedení verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="d2ddb-125">Zavedení TouchCarousel verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="d2ddb-126">Verze hammer.js na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="d2ddb-127">Webové formuláře ASP.NET a Ajax verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="d2ddb-128">ASP.NET MVC uvolní na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="d2ddb-129">Funkce SignalR technologie ASP.NET uvolní na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="d2ddb-130">Microsoft Ajax Content Delivery Network (CDN) hostuje knihoven jazyka JavaScript oblíbených třetích stran, například jQuery a umožňuje snadno přidat je do webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="d2ddb-131">Například můžete začít používat jQuery, který je hostován na tento CDN jednoduše přidáním &lt;skriptu&gt; značky na stránku, která odkazuje na ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="d2ddb-132">Využitím CDN, může výrazně zlepšit výkon aplikací Ajax.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="d2ddb-133">Obsah CDN ukládají do mezipaměti na servery umístěné po celém světě.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="d2ddb-134">Kromě toho CDN umožňuje prohlížečům soubory uložené v mezipaměti třetích stran JavaScript pro webové stránky, které se nacházejí v různých doménách.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="d2ddb-135">CDN podporuje protokol SSL (HTTPS), v případě, že budete muset poskytnout webovou stránku pomocí protokolu Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="d2ddb-136">CDN hostitelem následující knihovny skriptů třetích stran, které byly odeslány a licenci, vlastníky tyto knihovny:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="d2ddb-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="d2ddb-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="d2ddb-138">jQuery uživatelského rozhraní (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="d2ddb-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="d2ddb-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="d2ddb-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="d2ddb-140">jQuery ověření (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="d2ddb-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="d2ddb-141">jQuery cyklus (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="d2ddb-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="d2ddb-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="d2ddb-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="d2ddb-143">Microsoft Ajax CDN také zahrnuje následující knihovny, které byly odeslány společnosti Microsoft:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="d2ddb-144">Technologie ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="d2ddb-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="d2ddb-145">Soubory JavaScript rozhraní ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d2ddb-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="d2ddb-146">Soubory ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="d2ddb-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="d2ddb-147">Microsoft nenárokuje vlastnictví všech knihoven jiných výrobců hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="d2ddb-148">Tyto knihovny pro vás jsou licencování autorským vlastníky knihoven.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="d2ddb-149">Žádná práva, které možná budete muset stáhnout a použít tyto knihovny jsou udělit výhradně pomocí příslušných vlastníků autorských práv.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="d2ddb-150">Protože se nejedná knihovny Microsoft, společnost Microsoft poskytuje žádné záruky ani licence právy duševního vlastnictví (včetně žádné předpokládané patentová práva) pro hostované na této CDN knihovny třetích stran.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="d2ddb-151">Pokud chcete odeslat vaše knihovna JavaScript a vaše knihovna je jedním z hlavní knihovny jazyka JavaScript (jak je uvedeno v http://trends.builtwith.com) nebo rozšíření nebo moduly plug-in na tyto knihovny, které jsou (a) oblíbených; nebo (b) užitečné při použití technologie ASP.NET a obraťte se na AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="d2ddb-152">adresu AJAX.microsoft.com přejmenován na ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="d2ddb-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="d2ddb-153">CDN lze použít název domény webu microsoft.com a byl změněn na použít název domény aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="d2ddb-154">Tato změna byla provedená zvýšit výkon, protože když prohlížeč doménu microsoft.com by se odesílat žádné soubory cookie z této domény přes přenosu s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="d2ddb-155">Název domény. než microsoft.com přejmenováním možné zvýšit výkon co nejvíc 25 %.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="d2ddb-156">Poznamenejte si adresu ajax.microsoft.com budou nadále fungovat, ale doporučuje se ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="d2ddb-157">Starý formát: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="d2ddb-158">Nový formát: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="d2ddb-159">Visual Studio .vsdoc podpora</span><span class="sxs-lookup"><span data-stu-id="d2ddb-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="d2ddb-160">Použít soubory .vsdoc správně s Visual Studio 2008, musíte zajistit, že máte VS 2008 SP1 nainstalovány a opravy hotfix pro vsdoc soubory.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="d2ddb-161">Můžete získat z tohoto místa:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-161">You can get these from here:</span></span>

- [<span data-ttu-id="d2ddb-162">Stáhněte si Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Stáhnout Visual Studio 2008 SP1")
- [<span data-ttu-id="d2ddb-163">Stažení opravy hotfix .vsdoc pro Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "stažení opravy hotfix .vsdoc pro Visual Studio 2008 SP1")

<span data-ttu-id="d2ddb-164">Visual Studio 2010 podporuje .vsdoc soubory bez žádné další opravy.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="d2ddb-165">Pomocí prvku ASP.NET Ajax od CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="d2ddb-166">Pokud používáte technologii ASP.NET 4, můžete přesměrovat všechny požadavky pro skripty framework ASP.NET do CDN.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="d2ddb-167">Načítání skripty od CDN namísto místního webového serveru můžete výrazně zlepšit výkon veřejné weby technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="d2ddb-168">Pomocí vlastnosti ScriptManager EnableCDN přesměrovat všechny požadavky skriptu framework ASP.NET Ajax CDN společnosti Microsoft:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="d2ddb-169">Pomocí jQuery od CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="d2ddb-170">Můžete použít skripty jQuery hostované na CDN ve webové aplikaci přidáním následující skript element na stránce:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="d2ddb-171">CDN také obsahuje minifikovaný verzi jQuery skript, který můžete získat pomocí následující element:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="d2ddb-172">Pokud chcete, aby vaše stránky k záložnímu načítání jQuery z místní cestu na vlastní web, pokud není k dispozici CDN se stane, přidejte následující element ihned po element odkazující na CDN:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="d2ddb-173">Na následující stránce Ukázka používá CDN verzi knihovny jQuery (s přechod na místní kopie) pro zobrazení obsahu elementu div při kliknutí na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="d2ddb-174">Můžete získat další informace o jQuery a stáhnout místní kopii jQuery navštivte stránky [jQuery](http://jquery.com/) webu.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="d2ddb-175">Pomocí jQuery uživatelského rozhraní od CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="d2ddb-176">CDN také hostitelem knihovna uživatelského rozhraní jQuery.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="d2ddb-177">Knihovna uživatelského rozhraní jQuery zahrnuje širokou škálu pomůcek a účinky, které můžete použít v aplikacích ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="d2ddb-178">Například na následující stránce ukazuje, jak je možné používat jQuery ovládací prvek Datepicker uživatelského rozhraní v kontextu aplikace webových formulářů ASP.NET a zobrazit automaticky otevírané okno Kalendář:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="d2ddb-179">Pokud přesunete fokus do textového pole pomocí klávesnice, zobrazí se kalendáře:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Kalendářem vytvořené pomocí ovládací prvek Datepicker](overview/_static/image1.png)

<span data-ttu-id="d2ddb-181">Všimněte si, že musí obsahovat tři soubory od CDN ve výše uvedeném kódu:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="d2ddb-182">Knihovna jQuery &mdash; knihovna uživatelského rozhraní jQuery závisí na knihovny jQuery.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="d2ddb-183">Knihovna jQuery je nutné přidat na stránku předtím, než přidáte knihovna uživatelského rozhraní jQuery.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="d2ddb-184">Knihovna uživatelského rozhraní jQuery &mdash; knihovna uživatelského rozhraní jQuery obsahuje všechny důsledky uživatelského rozhraní jQuery a pomůcky, jako je například widgetu ovládací prvek Datepicker použít na stránce nahoře.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="d2ddb-185">Motiv uživatelského rozhraní jQuery &mdash; jQuery UI podporuje různé motivy.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="d2ddb-186">Výše uvedené stránka obsahuje odkaz na soubor k importu Redmond motiv šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="d2ddb-187">Všechny standardní jQuery UI motivy jsou hostované na CDN.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="d2ddb-188">[Navštivte tuto stránku](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 na Microsoft Ajax CDN") Chcete-li zobrazit miniatury pro každý motiv.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="d2ddb-189">Další informace o knihovna uživatelského rozhraní jQuery, najdete v oficiální [webu uživatelského rozhraní jQuery](http://jQueryUI.com "webu uživatelského rozhraní jQuery").</span><span class="sxs-lookup"><span data-stu-id="d2ddb-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="d2ddb-190">Soubory třetích stran na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="d2ddb-191">CDN hostitelem některé z nejčastěji používané knihovny JavaScript třetích stran.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="d2ddb-192">Microsoft nenárokuje vlastnictví všech knihoven jiných výrobců hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="d2ddb-193">Tyto knihovny pro vás jsou licencování autorským vlastníky knihoven.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="d2ddb-194">Žádná práva, které možná budete muset stáhnout a použít tyto knihovny jsou udělit výhradně pomocí příslušných vlastníků autorských práv.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="d2ddb-195">Protože se nejedná knihovny Microsoft, společnost Microsoft poskytuje žádné záruky ani licence právy duševního vlastnictví (včetně žádné předpokládané patentová práva) pro hostované na této CDN knihovny třetích stran.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-196">Verze jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-197">Následující verze jQuery hostovaných na CDN:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="d2ddb-198">verze jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-198">jQuery version 3.3.1</span></span>
- <span data-ttu-id="d2ddb-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js</span></span>
- <span data-ttu-id="d2ddb-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js</span></span>
- <span data-ttu-id="d2ddb-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map</span></span>
- <span data-ttu-id="d2ddb-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js</span></span>
- <span data-ttu-id="d2ddb-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js</span></span>
- <span data-ttu-id="d2ddb-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="d2ddb-205">verze jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-205">jQuery version 3.2.1</span></span>
- <span data-ttu-id="d2ddb-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="d2ddb-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="d2ddb-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="d2ddb-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="d2ddb-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="d2ddb-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="d2ddb-212">verze jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-212">jQuery version 3.2.0</span></span>

- <span data-ttu-id="d2ddb-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="d2ddb-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="d2ddb-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="d2ddb-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="d2ddb-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="d2ddb-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="d2ddb-219">verze jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-219">jQuery version 3.1.1</span></span>

- <span data-ttu-id="d2ddb-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="d2ddb-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="d2ddb-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="d2ddb-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="d2ddb-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="d2ddb-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="d2ddb-226">verze jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-226">jQuery version 3.1.0</span></span>

- <span data-ttu-id="d2ddb-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="d2ddb-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="d2ddb-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="d2ddb-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="d2ddb-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="d2ddb-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="d2ddb-233">verze jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-233">jQuery version 3.0.0</span></span>

- <span data-ttu-id="d2ddb-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="d2ddb-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="d2ddb-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="d2ddb-237">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-237">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="d2ddb-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="d2ddb-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="d2ddb-240">verze jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-240">jQuery version 2.2.4</span></span>

- <span data-ttu-id="d2ddb-241">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-241">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="d2ddb-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="d2ddb-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="d2ddb-244">verze jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-244">jQuery version 2.2.3</span></span>

- <span data-ttu-id="d2ddb-245">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-245">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="d2ddb-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="d2ddb-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="d2ddb-248">verze jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-248">jQuery version 2.2.2</span></span>

- <span data-ttu-id="d2ddb-249">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-249">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="d2ddb-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="d2ddb-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="d2ddb-252">verze jQuery 2.2.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-252">jQuery version 2.2.1</span></span>

- <span data-ttu-id="d2ddb-253">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-253">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="d2ddb-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="d2ddb-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="d2ddb-256">verze jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-256">jQuery version 2.2.0</span></span>

- <span data-ttu-id="d2ddb-257">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-257">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="d2ddb-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="d2ddb-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="d2ddb-260">verze jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-260">jQuery version 2.1.4</span></span>

- <span data-ttu-id="d2ddb-261">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-261">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="d2ddb-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="d2ddb-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="d2ddb-264">verze jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-264">jQuery version 2.1.3</span></span>

- <span data-ttu-id="d2ddb-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="d2ddb-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="d2ddb-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="d2ddb-268">verze jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-268">jQuery version 2.1.2</span></span>

- <span data-ttu-id="d2ddb-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="d2ddb-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="d2ddb-271">verze jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-271">jQuery version 2.1.1</span></span>

- <span data-ttu-id="d2ddb-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="d2ddb-273">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-273">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="d2ddb-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="d2ddb-275">verze jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-275">jQuery version 2.1.0</span></span>

- <span data-ttu-id="d2ddb-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="d2ddb-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="d2ddb-278">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-278">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="d2ddb-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="d2ddb-280">verze jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-280">jQuery version 2.0.3</span></span>

- <span data-ttu-id="d2ddb-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="d2ddb-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="d2ddb-283">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-283">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="d2ddb-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="d2ddb-285">jQuery verze bodu 2.0.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-285">jQuery version 2.0.2</span></span>

- <span data-ttu-id="d2ddb-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="d2ddb-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="d2ddb-288">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-288">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="d2ddb-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="d2ddb-290">jQuery verze 2.0.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-290">jQuery version 2.0.1</span></span>

- <span data-ttu-id="d2ddb-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="d2ddb-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="d2ddb-293">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-293">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="d2ddb-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="d2ddb-295">verze jQuery 2.0.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-295">jQuery version 2.0.0</span></span>

- <span data-ttu-id="d2ddb-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="d2ddb-297">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-297">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="d2ddb-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="d2ddb-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="d2ddb-300">verze jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-300">jQuery version 1.12.4</span></span>

- <span data-ttu-id="d2ddb-301">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-301">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="d2ddb-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="d2ddb-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="d2ddb-304">verze jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-304">jQuery version 1.12.3</span></span>

- <span data-ttu-id="d2ddb-305">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-305">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="d2ddb-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="d2ddb-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="d2ddb-308">verze jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-308">jQuery version 1.12.2</span></span>

- <span data-ttu-id="d2ddb-309">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-309">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="d2ddb-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="d2ddb-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="d2ddb-312">verze jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-312">jQuery version 1.12.1</span></span>

- <span data-ttu-id="d2ddb-313">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-313">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="d2ddb-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="d2ddb-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="d2ddb-316">verze jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-316">jQuery version 1.12.0</span></span>

- <span data-ttu-id="d2ddb-317">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-317">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="d2ddb-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="d2ddb-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="d2ddb-320">verze jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-320">jQuery version 1.11.3</span></span>

- <span data-ttu-id="d2ddb-321">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-321">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="d2ddb-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="d2ddb-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="d2ddb-324">verze jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-324">jQuery version 1.11.2</span></span>

- <span data-ttu-id="d2ddb-325">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-325">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="d2ddb-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="d2ddb-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="d2ddb-328">verze jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-328">jQuery version 1.11.1</span></span>

- <span data-ttu-id="d2ddb-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="d2ddb-330">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-330">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="d2ddb-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="d2ddb-332">verze jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-332">jQuery version 1.11.0</span></span>

- <span data-ttu-id="d2ddb-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="d2ddb-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="d2ddb-335">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-335">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="d2ddb-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="d2ddb-337">verze jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-337">jQuery version 1.10.2</span></span>

- <span data-ttu-id="d2ddb-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="d2ddb-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="d2ddb-340">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-340">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="d2ddb-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="d2ddb-342">verze jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-342">jQuery version 1.10.1</span></span>

- <span data-ttu-id="d2ddb-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="d2ddb-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="d2ddb-345">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-345">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="d2ddb-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="d2ddb-347">verze jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-347">jQuery version 1.10.0</span></span>

- <span data-ttu-id="d2ddb-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="d2ddb-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="d2ddb-350">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-350">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="d2ddb-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="d2ddb-352">verze jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-352">jQuery version 1.9.1</span></span>

- <span data-ttu-id="d2ddb-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="d2ddb-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="d2ddb-355">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-355">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="d2ddb-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="d2ddb-357">verze jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-357">jQuery version 1.9.0</span></span>

- <span data-ttu-id="d2ddb-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="d2ddb-359">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-359">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="d2ddb-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="d2ddb-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="d2ddb-362">verze jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-362">jQuery version 1.8.3</span></span>

- <span data-ttu-id="d2ddb-363">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-363">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="d2ddb-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="d2ddb-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="d2ddb-366">verze jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-366">jQuery version 1.8.2</span></span>

- <span data-ttu-id="d2ddb-367">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-367">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="d2ddb-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="d2ddb-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="d2ddb-370">verze jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-370">jQuery version 1.8.1</span></span>

- <span data-ttu-id="d2ddb-371">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-371">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="d2ddb-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="d2ddb-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="d2ddb-374">verze jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-374">jQuery version 1.8.0</span></span>

- <span data-ttu-id="d2ddb-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="d2ddb-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="d2ddb-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="d2ddb-378">verze jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-378">jQuery version 1.7.2</span></span>

- <span data-ttu-id="d2ddb-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="d2ddb-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="d2ddb-381">verze jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-381">jQuery version 1.7.1</span></span>

- <span data-ttu-id="d2ddb-382">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-382">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="d2ddb-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="d2ddb-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="d2ddb-385">verze jQuery 1.7</span><span class="sxs-lookup"><span data-stu-id="d2ddb-385">jQuery version 1.7</span></span>

- <span data-ttu-id="d2ddb-386">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-386">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="d2ddb-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="d2ddb-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="d2ddb-389">verze jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-389">jQuery version 1.6.4</span></span>

- <span data-ttu-id="d2ddb-390">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-390">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="d2ddb-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="d2ddb-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="d2ddb-393">verze jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-393">jQuery version 1.6.3</span></span>

- <span data-ttu-id="d2ddb-394">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-394">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="d2ddb-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="d2ddb-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="d2ddb-397">verze jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-397">jQuery version 1.6.2</span></span>

- <span data-ttu-id="d2ddb-398">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-398">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="d2ddb-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="d2ddb-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="d2ddb-401">jQuery verze 1.6.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-401">jQuery version 1.6.1</span></span>

- <span data-ttu-id="d2ddb-402">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-402">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="d2ddb-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="d2ddb-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="d2ddb-405">jQuery verzi 1.6</span><span class="sxs-lookup"><span data-stu-id="d2ddb-405">jQuery version 1.6</span></span>

- <span data-ttu-id="d2ddb-406">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-406">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="d2ddb-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="d2ddb-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="d2ddb-409">verze jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-409">jQuery version 1.5.2</span></span>

- <span data-ttu-id="d2ddb-410">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-410">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="d2ddb-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="d2ddb-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="d2ddb-413">verze jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-413">jQuery version 1.5.1</span></span>

- <span data-ttu-id="d2ddb-414">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-414">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="d2ddb-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="d2ddb-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="d2ddb-417">verze jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="d2ddb-417">jQuery version 1.5</span></span>

- <span data-ttu-id="d2ddb-418">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-418">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="d2ddb-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="d2ddb-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="d2ddb-421">verze jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-421">jQuery version 1.4.4</span></span>

- <span data-ttu-id="d2ddb-422">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-422">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="d2ddb-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="d2ddb-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="d2ddb-425">verze jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-425">jQuery version 1.4.3</span></span>

- <span data-ttu-id="d2ddb-426">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-426">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="d2ddb-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="d2ddb-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="d2ddb-429">jQuery verze 1.4.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-429">jQuery version 1.4.2</span></span>

- <span data-ttu-id="d2ddb-430">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-430">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="d2ddb-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="d2ddb-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="d2ddb-433">verze jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-433">jQuery version 1.4.1</span></span>

- <span data-ttu-id="d2ddb-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="d2ddb-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="d2ddb-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="d2ddb-437">jQuery verze 1.4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-437">jQuery version 1.4</span></span>

- <span data-ttu-id="d2ddb-438">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-438">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="d2ddb-439">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-439">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="d2ddb-440">jQuery verze 1.3.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-440">jQuery version 1.3.2</span></span>

- <span data-ttu-id="d2ddb-441">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-441">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="d2ddb-442">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-442">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="d2ddb-443">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-443">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="d2ddb-444">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-444">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-445">jQuery migrovat verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-445">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-446">Následující verze jQuery migrací jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-446">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="d2ddb-447">jQuery migrací verze 3.0.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-447">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="d2ddb-448">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="d2ddb-449">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-449">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="d2ddb-450">jQuery migrací verze 1.2.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-450">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="d2ddb-451">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="d2ddb-452">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-452">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="d2ddb-453">jQuery migrací verze 1.2.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-453">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="d2ddb-454">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="d2ddb-455">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-455">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="d2ddb-456">jQuery migrací verze 1.1.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-456">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="d2ddb-457">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="d2ddb-458">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-458">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="d2ddb-459">jQuery migrací verze 1.1.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-459">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="d2ddb-460">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-460">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="d2ddb-461">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-461">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="d2ddb-462">jQuery migrací verze 1.0.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-462">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="d2ddb-463">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-463">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="d2ddb-464">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-464">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-465">jQuery verze uživatelského rozhraní na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-465">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-466">Následující verze knihovny uživatelského rozhraní jQuery jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-466">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="d2ddb-467">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-467">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="d2ddb-468">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-468">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-469">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-469">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-470">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-470">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-471">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-471">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-472">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-472">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-473">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-473">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-474">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-474">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-475">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-475">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-476">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-476">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-477">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-477">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-478">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-478">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-479">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-479">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-480">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-480">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-481">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-481">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-482">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-482">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-483">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="d2ddb-483">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-484">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="d2ddb-484">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-485">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="d2ddb-485">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-486">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="d2ddb-486">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-487">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="d2ddb-487">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-488">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="d2ddb-488">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-489">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="d2ddb-489">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-490">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="d2ddb-490">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-491">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="d2ddb-491">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-492">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="d2ddb-492">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-493">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="d2ddb-493">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-494">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="d2ddb-494">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-495">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="d2ddb-495">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-496">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="d2ddb-496">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-497">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="d2ddb-497">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-498">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="d2ddb-498">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-499">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="d2ddb-499">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-500">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="d2ddb-500">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-501">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="d2ddb-501">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-502">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="d2ddb-502">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-503">jQuery ověření verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-503">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-504">Následující verze knihovny jQuery ověření jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-504">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="d2ddb-505">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-505">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="d2ddb-506">jQuery ověřením 1.17.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-506">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery ověření 1.17.0")
- [<span data-ttu-id="d2ddb-507">jQuery ověřením 1.16.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-507">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery ověření 1.16.0")
- [<span data-ttu-id="d2ddb-508">jQuery ověřením 1.15.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-508">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery ověření 1.15.1")
- [<span data-ttu-id="d2ddb-509">jQuery ověřením 1.15.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-509">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery ověření 1.15.0")
- [<span data-ttu-id="d2ddb-510">jQuery ověřením 1.14.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-510">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery ověření 1.14.0")
- [<span data-ttu-id="d2ddb-511">jQuery ověřením 1.13.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-511">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery ověření 1.13.1")
- [<span data-ttu-id="d2ddb-512">jQuery ověřením 1.13.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-512">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery ověření 1.13.0")
- [<span data-ttu-id="d2ddb-513">jQuery ověřením 1.12.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-513">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery ověření 1.12.0")
- [<span data-ttu-id="d2ddb-514">jQuery ověřením 1.11.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-514">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery ověření 1.11.1")
- [<span data-ttu-id="d2ddb-515">jQuery ověřením 1.11.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-515">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery ověření 1.11.0")
- [<span data-ttu-id="d2ddb-516">jQuery ověřením 1.10.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-516">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery ověření 1.10.0")
- [<span data-ttu-id="d2ddb-517">jQuery ověřením 1.9</span><span class="sxs-lookup"><span data-stu-id="d2ddb-517">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate verze 1.9")
- [<span data-ttu-id="d2ddb-518">jQuery ověřením 1.8.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-518">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate verze 1.8.1")
- [<span data-ttu-id="d2ddb-519">jQuery ověřením 1.8</span><span class="sxs-lookup"><span data-stu-id="d2ddb-519">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate verze 1.8")
- [<span data-ttu-id="d2ddb-520">jQuery ověřením 1.7</span><span class="sxs-lookup"><span data-stu-id="d2ddb-520">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate verze 1.7")
- [<span data-ttu-id="d2ddb-521">jQuery ověřením 1.6</span><span class="sxs-lookup"><span data-stu-id="d2ddb-521">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery ověřením 1.6")
- [<span data-ttu-id="d2ddb-522">jQuery ověřením 1.5.5</span><span class="sxs-lookup"><span data-stu-id="d2ddb-522">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery ověřením 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-523">jQuery Mobile verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-523">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-524">Následující verze knihovny jQuery Mobile jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-524">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="d2ddb-525">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-525">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="d2ddb-526">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="d2ddb-526">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-527">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-527">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-528">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-528">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-529">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-529">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-530">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-530">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-531">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-531">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-532">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-532">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-533">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-533">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-534">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-534">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-535">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-535">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-536">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-536">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-537">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-537">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-538">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-538">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-539">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-539">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-540">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-540">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-541">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-541">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 na Microsoft Ajax CDN")
- [<span data-ttu-id="d2ddb-542">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-542">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 na Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-543">jQuery šablony verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-543">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-544">Následující verze modulu plug-in jQuery šablony jsou hostovány na tento CDN.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-544">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="d2ddb-545">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-545">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="d2ddb-546">jQuery šablony Beta 1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-546">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery šablony Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-547">jQuery cyklus verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-547">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-548">Následující verze modulu plug-in jQuery cyklus jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-548">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="d2ddb-549">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-549">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="d2ddb-550">jQuery cyklus 2.99</span><span class="sxs-lookup"><span data-stu-id="d2ddb-550">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2.99 cyklu")
- [<span data-ttu-id="d2ddb-551">jQuery cyklus 2.94</span><span class="sxs-lookup"><span data-stu-id="d2ddb-551">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 cyklu")
- [<span data-ttu-id="d2ddb-552">jQuery cyklus 2,88</span><span class="sxs-lookup"><span data-stu-id="d2ddb-552">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 cyklu")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-553">jQuery DataTables verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-553">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-554">Následující verze modulu plug-in jQuery DataTables jsou hostované na této CDN.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-554">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="d2ddb-555">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-555">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="d2ddb-556">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="d2ddb-556">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="d2ddb-557">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-557">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="d2ddb-558">jQuery DataTables otázku 1.9.4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-558">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables otázku 1.9.4")
- [<span data-ttu-id="d2ddb-559">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-559">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="d2ddb-560">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-560">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="d2ddb-561">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-561">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="d2ddb-562">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-562">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="d2ddb-563">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-563">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-564">Verze Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-564">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-565">Následující verze systému [Modernizr](http://www.modernizr.com "Modernizr") jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-565">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="d2ddb-566">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-566">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="d2ddb-567">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-567">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="d2ddb-568">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-568">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="d2ddb-569">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-569">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="d2ddb-570">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-570">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="d2ddb-571">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-571">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-572">Verze JSHint na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-572">JSHint Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-573">Následující verze systému [JSHint](http://www.jshint.com "JSHint") jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-573">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="d2ddb-574">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-574">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-575">Verze knockout na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-575">Knockout Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-576">Následující verze systému [Knockout](http://www.knockoutjs.com "Knockout") jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-576">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="d2ddb-577">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="d2ddb-578">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="d2ddb-579">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="d2ddb-580">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="d2ddb-581">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="d2ddb-582">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="d2ddb-583">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="d2ddb-584">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="d2ddb-585">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="d2ddb-586">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="d2ddb-587">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="d2ddb-588">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="d2ddb-589">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="d2ddb-590">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-590">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="d2ddb-591">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-591">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="d2ddb-592">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-592">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="d2ddb-593">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-593">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="d2ddb-594">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-594">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="d2ddb-595">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-595">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="d2ddb-596">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-596">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-597">Globalizace verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-597">Globalize Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-598">Následující verze systému [Globalize](https://github.com/jquery/globalize "Globalize") jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-598">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="d2ddb-599">Globalizace verze 1.0.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-599">Globalize version 1.0.0</span></span>

- <span data-ttu-id="d2ddb-600">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="d2ddb-601">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/node-Main.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-601">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="d2ddb-602">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-602">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="d2ddb-603">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-603">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="d2ddb-604">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-604">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="d2ddb-605">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-605">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="d2ddb-606">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Plural.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-606">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="d2ddb-607">http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Relative-Time.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-607">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="d2ddb-608">Globalizace verze 0.1.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-608">Globalize version 0.1.1</span></span>

- <span data-ttu-id="d2ddb-609">http://AJAX.aspnetcdn.com/AJAX/Globalize/0.1.1/Globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-609">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="d2ddb-610">http://AJAX.aspnetcdn.com/AJAX/Globalize/0.1.1/Globalize.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-610">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="d2ddb-611">http://AJAX.aspnetcdn.com/AJAX/Globalize/0.1.1/cultures/Globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-611">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="d2ddb-612">všechny jazykové verze</span><span class="sxs-lookup"><span data-stu-id="d2ddb-612">all cultures</span></span>
- <span data-ttu-id="d2ddb-613">http://AJAX.aspnetcdn.com/AJAX/Globalize/0.1.1/cultures/Globalize.Culture. {jazyková verze kódu} .js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-613">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="d2ddb-614">"{Jazyková verze kódu}" nahraďte kód požadovanou jazykovou verzi, například Microsoft globalize.culture.en GB.js== soubory CDN == tyto knihovny byly odeslaný společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-614">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-615">Odpověď verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-615">Respond Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-616">Následující verze systému [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") reakce hostovaných na CDN:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-616">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="d2ddb-617">Verze 1.4.2 reagovat</span><span class="sxs-lookup"><span data-stu-id="d2ddb-617">Respond version 1.4.2</span></span>

- <span data-ttu-id="d2ddb-618">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="d2ddb-619">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="d2ddb-620">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-620">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="d2ddb-621">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="d2ddb-622">Verze 1.4.1 reagovat</span><span class="sxs-lookup"><span data-stu-id="d2ddb-622">Respond version 1.4.1</span></span>

- <span data-ttu-id="d2ddb-623">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="d2ddb-624">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="d2ddb-625">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-625">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="d2ddb-626">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-626">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="d2ddb-627">Verze 1.4.0 reagovat</span><span class="sxs-lookup"><span data-stu-id="d2ddb-627">Respond version 1.4.0</span></span>

- <span data-ttu-id="d2ddb-628">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-628">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="d2ddb-629">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-629">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="d2ddb-630">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-630">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="d2ddb-631">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-631">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="d2ddb-632">Verze 1.3.0 reagovat</span><span class="sxs-lookup"><span data-stu-id="d2ddb-632">Respond version 1.3.0</span></span>

- <span data-ttu-id="d2ddb-633">http://AJAX.aspnetcdn.com/AJAX/respond/1.3.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-633">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="d2ddb-634">Verze 1.2.0 reagovat</span><span class="sxs-lookup"><span data-stu-id="d2ddb-634">Respond version 1.2.0</span></span>

- <span data-ttu-id="d2ddb-635">http://AJAX.aspnetcdn.com/AJAX/respond/1.2.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-635">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-636">Zavedení verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-636">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-637">Následující verze systému [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap hostovaných na CDN:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-637">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="d2ddb-638">Zavedení verze 3.3.7</span><span class="sxs-lookup"><span data-stu-id="d2ddb-638">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="d2ddb-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="d2ddb-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-645">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-645">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="d2ddb-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="d2ddb-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="d2ddb-652">Zavedení verze 3.3.6</span><span class="sxs-lookup"><span data-stu-id="d2ddb-652">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="d2ddb-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="d2ddb-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-659">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-659">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="d2ddb-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="d2ddb-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="d2ddb-666">Zavedení verze 3.3.5</span><span class="sxs-lookup"><span data-stu-id="d2ddb-666">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="d2ddb-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="d2ddb-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-673">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-673">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="d2ddb-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="d2ddb-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="d2ddb-680">Zavedení verze 3.3.4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-680">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="d2ddb-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="d2ddb-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-687">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-687">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="d2ddb-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="d2ddb-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="d2ddb-694">Zavedení verze 3.3.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-694">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="d2ddb-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="d2ddb-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-701">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-701">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="d2ddb-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="d2ddb-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="d2ddb-708">Zavedení verze 3.3.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-708">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="d2ddb-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="d2ddb-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-714">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-714">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="d2ddb-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="d2ddb-721">Zavedení verze 3.3.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-721">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="d2ddb-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="d2ddb-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-727">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-727">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="d2ddb-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="d2ddb-734">Zavedení verze 3.2.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-734">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="d2ddb-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="d2ddb-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-740">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-740">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="d2ddb-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="d2ddb-747">Zavedení verze 3.1.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-747">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="d2ddb-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="d2ddb-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-753">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-753">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="d2ddb-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="d2ddb-760">Zavedení verze 3.1.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-760">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="d2ddb-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="d2ddb-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-766">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-766">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="d2ddb-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="d2ddb-773">Zavedení verze 3.0.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-773">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="d2ddb-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-777">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-777">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="d2ddb-784">Zavedení verze 3.0.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-784">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="d2ddb-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-788">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-788">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="d2ddb-795">Zavedení verze 3.0.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-795">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="d2ddb-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-799">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-799">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="d2ddb-806">Zavedení verze 3.0.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-806">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="d2ddb-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-810">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-810">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-811">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="d2ddb-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-812">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="d2ddb-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="d2ddb-813">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="d2ddb-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-814">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="d2ddb-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="d2ddb-815">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="d2ddb-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="d2ddb-816">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="d2ddb-817">Zavedení verze 2.3.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-817">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="d2ddb-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-819">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-819">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="d2ddb-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="d2ddb-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="d2ddb-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings-white.PNG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="d2ddb-826">Zavedení verze 2.3.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-826">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="d2ddb-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="d2ddb-828">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-828">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="d2ddb-829">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-829">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="d2ddb-830">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-830">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="d2ddb-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-831">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="d2ddb-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-832">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="d2ddb-833">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-833">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="d2ddb-834">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings-white.PNG</span><span class="sxs-lookup"><span data-stu-id="d2ddb-834">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-835">Zavedení TouchCarousel verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-835">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-836">Následující verze systému [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel verze jsou hostované na CDN :</span><span class="sxs-lookup"><span data-stu-id="d2ddb-836">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="d2ddb-837">Zavedení TouchCarousel verze 0.8.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-837">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="d2ddb-838">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-touch-carousel/0.8.0/CSS/Bootstrap-touch-carousel.CSS</span><span class="sxs-lookup"><span data-stu-id="d2ddb-838">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="d2ddb-839">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-touch-carousel/0.8.0/js/Bootstrap-touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-839">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-840">Verze hammer.js na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-840">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-841">Následující verze systému [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js verze jsou hostované na CDN:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-841">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="d2ddb-842">Verze hammer.js verze 2.0.4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-842">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="d2ddb-843">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-843">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="d2ddb-844">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-844">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="d2ddb-845">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.map</span><span class="sxs-lookup"><span data-stu-id="d2ddb-845">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-846">Webové formuláře ASP.NET a Ajax verze na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-846">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-847">Následující verze knihovny ASP.NET Ajax jsou hostované na CDN.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-847">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="d2ddb-848">Klepnutím na odkaz zobrazit skutečný seznam souborů.</span><span class="sxs-lookup"><span data-stu-id="d2ddb-848">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="d2ddb-849">Webové formuláře ASP.NET a Ajax verze 4.5.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-849">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "webových formulářů ASP.NET a Ajax 4.5.2")
- [<span data-ttu-id="d2ddb-850">Webové formuláře ASP.NET a Ajax verze 4</span><span class="sxs-lookup"><span data-stu-id="d2ddb-850">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "webových formulářů ASP.NET a Ajax 4")
- [<span data-ttu-id="d2ddb-851">Technologie ASP.NET Ajax verze 3.5</span><span class="sxs-lookup"><span data-stu-id="d2ddb-851">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "prvku ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-852">ASP.NET MVC uvolní na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-852">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-853">Následující soubory ASP.NET MVC JavaScript jsou hostované na této CDN:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-853">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="d2ddb-854">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-854">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="d2ddb-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-855">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="d2ddb-856">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-856">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="d2ddb-857">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-857">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="d2ddb-858">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-858">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="d2ddb-859">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-859">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="d2ddb-860">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-860">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="d2ddb-861">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-861">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="d2ddb-862">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-862">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="d2ddb-863">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-863">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="d2ddb-864">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-864">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="d2ddb-865">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-865">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="d2ddb-866">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-866">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="d2ddb-867">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-867">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="d2ddb-868">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-868">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="d2ddb-869">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-869">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="d2ddb-870">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-870">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="d2ddb-871">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-871">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="d2ddb-872">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-872">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="d2ddb-873">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-873">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="d2ddb-874">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-874">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="d2ddb-875">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-875">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="d2ddb-876">ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-876">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="d2ddb-877">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-877">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="d2ddb-878">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-878">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="d2ddb-879">Funkce SignalR technologie ASP.NET uvolní na CDN</span><span class="sxs-lookup"><span data-stu-id="d2ddb-879">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="d2ddb-880">Následující soubory ASP.NET SignalR JavaScript jsou hostované na této CDN:</span><span class="sxs-lookup"><span data-stu-id="d2ddb-880">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="d2ddb-881">Funkce SignalR technologie ASP.NET 2.2.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-881">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="d2ddb-882">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="d2ddb-883">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-883">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="d2ddb-884">Funkce SignalR technologie ASP.NET 2.2.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-884">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="d2ddb-885">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="d2ddb-886">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-886">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="d2ddb-887">Funkce SignalR technologie ASP.NET 2.2.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-887">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="d2ddb-888">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="d2ddb-889">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-889">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="d2ddb-890">Funkce SignalR technologie ASP.NET 2.1.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-890">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="d2ddb-891">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="d2ddb-892">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-892">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="d2ddb-893">Funkce SignalR technologie ASP.NET 2.0.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-893">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="d2ddb-894">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="d2ddb-895">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-895">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="d2ddb-896">Funkce SignalR technologie ASP.NET bodu 2.0.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-896">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="d2ddb-897">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="d2ddb-898">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-898">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="d2ddb-899">Funkce SignalR technologie ASP.NET 2.0.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-899">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="d2ddb-900">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="d2ddb-901">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-901">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="d2ddb-902">Funkce SignalR technologie ASP.NET 2.0.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-902">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="d2ddb-903">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="d2ddb-904">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-904">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="d2ddb-905">Funkce SignalR technologie ASP.NET 1.1.3</span><span class="sxs-lookup"><span data-stu-id="d2ddb-905">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="d2ddb-906">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="d2ddb-907">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-907">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="d2ddb-908">Funkce SignalR technologie ASP.NET 1.1.2</span><span class="sxs-lookup"><span data-stu-id="d2ddb-908">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="d2ddb-909">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="d2ddb-910">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-910">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="d2ddb-911">Funkce SignalR technologie ASP.NET 1.1.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-911">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="d2ddb-912">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="d2ddb-913">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-913">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="d2ddb-914">Funkce SignalR technologie ASP.NET 1.1.0</span><span class="sxs-lookup"><span data-stu-id="d2ddb-914">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="d2ddb-915">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-915">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="d2ddb-916">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-916">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="d2ddb-917">Funkce SignalR technologie ASP.NET 1.0.1</span><span class="sxs-lookup"><span data-stu-id="d2ddb-917">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="d2ddb-918">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-918">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="d2ddb-919">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="d2ddb-919">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="d2ddb-920">Informace o podmínky použití pro CDN najdete v tématu [Microsoft Ajax CDN podmínky použití](https://www.asp.net/terms-of-use "Microsoft Ajax CDN podmínky použití").</span><span class="sxs-lookup"><span data-stu-id="d2ddb-920">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
