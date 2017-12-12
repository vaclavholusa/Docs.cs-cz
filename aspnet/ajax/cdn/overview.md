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
<a name="microsoft-ajax-content-delivery-network"></a>Sítě pro doručování obsahu Microsoft Ajax
====================
Poznámka: Microsoft Ajax CDN nemá žádné SLA vedle pomocí Azure CDN.

## <a name="table-of-contents"></a>Obsah

**[adresu AJAX.microsoft.com přejmenován na ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**  
**[Visual Studio .vsdoc podpora](#Visual_Studio_vsdoc_Support_19)**  
**[Pomocí prvku ASP.NET Ajax od CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**  
**[Pomocí jQuery od CDN](#Using_jQuery_from_the_CDN_21)**  
**[Pomocí jQuery uživatelského rozhraní od CDN](#Using_jQuery_UI_from_the_CDN_22)**  
**[Soubory třetích stran na CDN](#Third-Party_Files_on_the_CDN_23)**  
  
 [Verze jQuery na CDN](#jQuery_Releases_on_the_CDN_0)  
 [jQuery migrovat verze na CDN](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [jQuery verze uživatelského rozhraní na CDN](#jQuery_UI_Releases_on_the_CDN_2)  
 [jQuery ověření verze na CDN](#jQuery_Validation_Releases_on_the_CDN_3)  
 [jQuery Mobile verze na CDN](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [jQuery šablony verze na CDN](#jQuery_Templates_Releases_on_the_CDN_5)  
 [jQuery cyklus verze na CDN](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [jQuery DataTables verze na CDN](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [Verze Modernizr na CDN](#Modernizr_Releases_on_the_CDN_8)  
 [Verze JSHint na CDN](#JSHint_Releases_on_the_CDN_10)  
 [Verze knockout na CDN](#Knockout_Releases_on_the_CDN_11)  
 [Globalizace verze na CDN](#Globalize_Releases_on_the_CDN_12)  
 [Odpověď verze na CDN](#Respond_Releases_on_the_CDN_13)  
 [Zavedení verze na CDN](#Bootstrap_Releases_on_the_CDN_14)  
 [Zavedení TouchCarousel verze na CDN](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [Verze hammer.js na CDN](#Hammerjs_Releases_on_the_CDN_19)  
 [Webové formuláře ASP.NET a Ajax verze na CDN](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [ASP.NET MVC uvolní na CDN](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [Funkce SignalR technologie ASP.NET uvolní na CDN](#ASPNET_SignalR_Releases_on_the_CDN_17)

Microsoft Ajax Content Delivery Network (CDN) hostuje knihoven jazyka JavaScript oblíbených třetích stran, například jQuery a umožňuje snadno přidat je do webových aplikací. Například můžete začít používat jQuery, který je hostován na tento CDN jednoduše přidáním &lt;skriptu&gt; značky na stránku, která odkazuje na ajax.aspnetcdn.com.

Využitím CDN, může výrazně zlepšit výkon aplikací Ajax. Obsah CDN ukládají do mezipaměti na servery umístěné po celém světě. Kromě toho CDN umožňuje prohlížečům soubory uložené v mezipaměti třetích stran JavaScript pro webové stránky, které se nacházejí v různých doménách.

CDN podporuje protokol SSL (HTTPS), v případě, že budete muset poskytnout webovou stránku pomocí protokolu Secure Sockets Layer.

CDN hostitelem následující knihovny skriptů třetích stran, které byly odeslány a licenci, vlastníky tyto knihovny:

- jQuery (www.jquery.com)
- jQuery uživatelského rozhraní (www.jqueryui.com)
- jQuery Mobile (www.jquerymobile.com)
- jQuery ověření (www.jquery.com)
- jQuery cyklus (www.malsup.com/jquery/cycle/)
- jQuery DataTables (http://datatables.net/)

Microsoft Ajax CDN také zahrnuje následující knihovny, které byly odeslány společnosti Microsoft:

- Technologie ASP.NET Ajax
- Soubory JavaScript rozhraní ASP.NET MVC
- Soubory ASP.NET SignalR JavaScript

Microsoft nenárokuje vlastnictví všech knihoven jiných výrobců hostované na této CDN. Tyto knihovny pro vás jsou licencování autorským vlastníky knihoven. Žádná práva, které možná budete muset stáhnout a použít tyto knihovny jsou udělit výhradně pomocí příslušných vlastníků autorských práv. Protože se nejedná knihovny Microsoft, společnost Microsoft poskytuje žádné záruky ani licence právy duševního vlastnictví (včetně žádné předpokládané patentová práva) pro hostované na této CDN knihovny třetích stran.

Pokud chcete odeslat vaše knihovna JavaScript a vaše knihovna je jedním z hlavní knihovny jazyka JavaScript (jak je uvedeno v http://trends.builtwith.com) nebo rozšíření nebo moduly plug-in na tyto knihovny, které jsou (a) oblíbených; nebo (b) užitečné při použití technologie ASP.NET a obraťte se na AjaxCDNSubmission@Microsoft.com.

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a>adresu AJAX.microsoft.com přejmenován na ajax.aspnetcdn.com

CDN lze použít název domény webu microsoft.com a byl změněn na použít název domény aspnetcdn.com. Tato změna byla provedená zvýšit výkon, protože když prohlížeč doménu microsoft.com by se odesílat žádné soubory cookie z této domény přes přenosu s každou žádostí. Název domény. než microsoft.com přejmenováním možné zvýšit výkon co nejvíc 25 %. Poznamenejte si adresu ajax.microsoft.com budou nadále fungovat, ale doporučuje se ajax.aspnetcdn.com.

- Starý formát: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js
- Nový formát: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a>Visual Studio .vsdoc podpora

Použít soubory .vsdoc správně s Visual Studio 2008, musíte zajistit, že máte VS 2008 SP1 nainstalovány a opravy hotfix pro vsdoc soubory. Můžete získat z tohoto místa:

- [Stáhněte si Visual Studio 2008 SP1](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Stáhnout Visual Studio 2008 SP1")
- [Stažení opravy hotfix .vsdoc pro Visual Studio 2008 SP1](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "stažení opravy hotfix .vsdoc pro Visual Studio 2008 SP1")

Visual Studio 2010 podporuje .vsdoc soubory bez žádné další opravy.

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a>Pomocí prvku ASP.NET Ajax od CDN

Pokud používáte technologii ASP.NET 4, můžete přesměrovat všechny požadavky pro skripty framework ASP.NET do CDN. Načítání skripty od CDN namísto místního webového serveru můžete výrazně zlepšit výkon veřejné weby technologie ASP.NET.

Pomocí vlastnosti ScriptManager EnableCDN přesměrovat všechny požadavky skriptu framework ASP.NET Ajax CDN společnosti Microsoft:

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a>Pomocí jQuery od CDN

Můžete použít skripty jQuery hostované na CDN ve webové aplikaci přidáním následující skript element na stránce:

[!code-html[Main](overview/samples/sample2.html)]

CDN také obsahuje minifikovaný verzi jQuery skript, který můžete získat pomocí následující element:

[!code-html[Main](overview/samples/sample3.html)]

Pokud chcete, aby vaše stránky k záložnímu načítání jQuery z místní cestu na vlastní web, pokud není k dispozici CDN se stane, přidejte následující element ihned po element odkazující na CDN:

[!code-html[Main](overview/samples/sample4.html)]

Na následující stránce Ukázka používá CDN verzi knihovny jQuery (s přechod na místní kopie) pro zobrazení obsahu elementu div při kliknutí na tlačítko.

[!code-html[Main](overview/samples/sample5.html)]

Můžete získat další informace o jQuery a stáhnout místní kopii jQuery navštivte stránky [jQuery](http://jquery.com/) webu.

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a>Pomocí jQuery uživatelského rozhraní od CDN

CDN také hostitelem knihovna uživatelského rozhraní jQuery. Knihovna uživatelského rozhraní jQuery zahrnuje širokou škálu pomůcek a účinky, které můžete použít v aplikacích ASP.NET. Například na následující stránce ukazuje, jak je možné používat jQuery ovládací prvek Datepicker uživatelského rozhraní v kontextu aplikace webových formulářů ASP.NET a zobrazit automaticky otevírané okno Kalendář:

[!code-aspx[Main](overview/samples/sample6.aspx)]

Pokud přesunete fokus do textového pole pomocí klávesnice, zobrazí se kalendáře:

![Kalendářem vytvořené pomocí ovládací prvek Datepicker](overview/_static/image1.png)

Všimněte si, že musí obsahovat tři soubory od CDN ve výše uvedeném kódu:

- Knihovna jQuery &mdash; knihovna uživatelského rozhraní jQuery závisí na knihovny jQuery. Knihovna jQuery je nutné přidat na stránku předtím, než přidáte knihovna uživatelského rozhraní jQuery.
- Knihovna uživatelského rozhraní jQuery &mdash; knihovna uživatelského rozhraní jQuery obsahuje všechny důsledky uživatelského rozhraní jQuery a pomůcky, jako je například widgetu ovládací prvek Datepicker použít na stránce nahoře.
- Motiv uživatelského rozhraní jQuery &mdash; jQuery UI podporuje různé motivy. Výše uvedené stránka obsahuje odkaz na soubor k importu Redmond motiv šablony stylů CSS.

Všechny standardní jQuery UI motivy jsou hostované na CDN. [Navštivte tuto stránku](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 na Microsoft Ajax CDN") Chcete-li zobrazit miniatury pro každý motiv.

Další informace o knihovna uživatelského rozhraní jQuery, najdete v oficiální [webu uživatelského rozhraní jQuery](http://jQueryUI.com "webu uživatelského rozhraní jQuery").

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a>Soubory třetích stran na CDN

CDN hostitelem některé z nejčastěji používané knihovny JavaScript třetích stran. Microsoft nenárokuje vlastnictví všech knihoven jiných výrobců hostované na této CDN. Tyto knihovny pro vás jsou licencování autorským vlastníky knihoven. Žádná práva, které možná budete muset stáhnout a použít tyto knihovny jsou udělit výhradně pomocí příslušných vlastníků autorských práv. Protože se nejedná knihovny Microsoft, společnost Microsoft poskytuje žádné záruky ani licence právy duševního vlastnictví (včetně žádné předpokládané patentová práva) pro hostované na této CDN knihovny třetích stran.

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a>Verze jQuery na CDN

Následující verze jQuery hostovaných na CDN:

#### <a name="jquery-version-321"></a>verze jQuery 3.2.1
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.map
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.SLIM.min.map

#### <a name="jquery-version-320"></a>verze jQuery 3.2.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.map
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.SLIM.min.map

#### <a name="jquery-version-311"></a>verze jQuery 3.1.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.map
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.SLIM.min.map

#### <a name="jquery-version-310"></a>verze jQuery 3.1.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.map
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.SLIM.min.map

#### <a name="jquery-version-300"></a>verze jQuery 3.0.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.map
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.SLIM.min.map

#### <a name="jquery-version-224"></a>verze jQuery 2.2.4

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.map

#### <a name="jquery-version-223"></a>verze jQuery 2.2.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.map

#### <a name="jquery-version-222"></a>verze jQuery 2.2.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.map

#### <a name="jquery-version-221"></a>verze jQuery 2.2.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.map

#### <a name="jquery-version-220"></a>verze jQuery 2.2.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.map

#### <a name="jquery-version-214"></a>verze jQuery 2.1.4

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.map

#### <a name="jquery-version-213"></a>verze jQuery 2.1.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.map

#### <a name="jquery-version-212"></a>verze jQuery 2.1.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.min.js

#### <a name="jquery-version-211"></a>verze jQuery 2.1.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.map

#### <a name="jquery-version-210"></a>verze jQuery 2.1.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.map

#### <a name="jquery-version-203"></a>verze jQuery 2.0.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.map

#### <a name="jquery-version-202"></a>jQuery verze bodu 2.0.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.map

#### <a name="jquery-version-201"></a>jQuery verze 2.0.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.map

#### <a name="jquery-version-200"></a>verze jQuery 2.0.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.map

#### <a name="jquery-version-1124"></a>verze jQuery 1.12.4

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.map

#### <a name="jquery-version-1123"></a>verze jQuery 1.12.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.map

#### <a name="jquery-version-1122"></a>verze jQuery 1.12.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.map

#### <a name="jquery-version-1121"></a>verze jQuery 1.12.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.map

#### <a name="jquery-version-1120"></a>verze jQuery 1.12.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.map

#### <a name="jquery-version-1113"></a>verze jQuery 1.11.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.map

#### <a name="jquery-version-1112"></a>verze jQuery 1.11.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.map

#### <a name="jquery-version-1111"></a>verze jQuery 1.11.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.map

#### <a name="jquery-version-1110"></a>verze jQuery 1.11.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.map

#### <a name="jquery-version-1102"></a>verze jQuery 1.10.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.map

#### <a name="jquery-version-1101"></a>verze jQuery 1.10.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.map

#### <a name="jquery-version-1100"></a>verze jQuery 1.10.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.map

#### <a name="jquery-version-191"></a>verze jQuery 1.9.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.map

#### <a name="jquery-version-190"></a>verze jQuery 1.9.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.map

#### <a name="jquery-version-183"></a>verze jQuery 1.8.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a>verze jQuery 1.8.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a>verze jQuery 1.8.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a>verze jQuery 1.8.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a>verze jQuery 1.7.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.min.js

#### <a name="jquery-version-171"></a>verze jQuery 1.7.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a>verze jQuery 1.7

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a>verze jQuery 1.6.4

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a>verze jQuery 1.6.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a>verze jQuery 1.6.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a>jQuery verze 1.6.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a>jQuery verzi 1.6

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a>verze jQuery 1.5.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a>verze jQuery 1.5.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a>verze jQuery 1.5

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a>verze jQuery 1.4.4

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a>verze jQuery 1.4.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a>jQuery verze 1.4.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a>verze jQuery 1.4.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a>jQuery verze 1.4

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.min.js

#### <a name="jquery-version-132"></a>jQuery verze 1.3.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a>jQuery migrovat verze na CDN

Následující verze jQuery migrací jsou hostované na CDN:

#### <a name="jquery-migrate-version-300"></a>jQuery migrací verze 3.0.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-3.0.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a>jQuery migrací verze 1.2.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.2.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.2.1.min.js

jQuery migrací verze 1.2.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.2.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a>jQuery migrací verze 1.1.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.1.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a>jQuery migrací verze 1.1.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.1.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a>jQuery migrací verze 1.0.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.0.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-Migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a>jQuery verze uživatelského rozhraní na CDN

Následující verze knihovny uživatelského rozhraní jQuery jsou hostované na této CDN. Klepnutím na odkaz zobrazit skutečný seznam souborů.

- [jQuery UI 1.12.1](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 na Microsoft Ajax CDN")
- [jQuery UI 1.12.0](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 na Microsoft Ajax CDN")
- [jQuery UI 1.11.4](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 na Microsoft Ajax CDN")
- [jQuery UI 1.11.3](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 na Microsoft Ajax CDN")
- [jQuery UI 1.11.2](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 na Microsoft Ajax CDN")
- [jQuery UI 1.11.1](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 na Microsoft Ajax CDN")
- [jQuery UI 1.11.0](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 na Microsoft Ajax CDN")
- [jQuery UI 1.10.4](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 na Microsoft Ajax CDN")
- [jQuery UI 1.10.3](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 na Microsoft Ajax CDN")
- [jQuery UI 1.10.2](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 na Microsoft Ajax CDN")
- [jQuery UI 1.10.1](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 na Microsoft Ajax CDN")
- [jQuery UI 1.10.0](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 na Microsoft Ajax CDN")
- [jQuery UI 1.9.2](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 na Microsoft Ajax CDN")
- [jQuery UI 1.9.1](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 na Microsoft Ajax CDN")
- [jQuery UI 1.9.0](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 na Microsoft Ajax CDN")
- [jQuery UI 1.8.24](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 na Microsoft Ajax CDN")
- [jQuery UI 1.8.23](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 na Microsoft Ajax CDN")
- [jQuery UI 1.8.22](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 na Microsoft Ajax CDN")
- [jQuery UI 1.8.21](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 na Microsoft Ajax CDN")
- [jQuery UI 1.8.20](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 na Microsoft Ajax CDN")
- [jQuery UI 1.8.19](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 na Microsoft Ajax CDN")
- [jQuery UI 1.8.18](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 na Microsoft Ajax CDN")
- [jQuery UI 1.8.17](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 na Microsoft Ajax CDN")
- [jQuery UI 1.8.16](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 na Microsoft Ajax CDN")
- [jQuery UI 1.8.15](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 na Microsoft Ajax CDN")
- [jQuery UI 1.8.14](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 na Microsoft Ajax CDN")
- [jQuery UI 1.8.13](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 na Microsoft Ajax CDN")
- [jQuery UI 1.8.12](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 na Microsoft Ajax CDN")
- [jQuery UI 1.8.11](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 na Microsoft Ajax CDN")
- [jQuery UI 1.8.10](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 na Microsoft Ajax CDN")
- [jQuery UI 1.8.9](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 na Microsoft Ajax CDN")
- [jQuery UI 1.8.8](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 na Microsoft Ajax CDN")
- [jQuery UI 1.8.7](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 na Microsoft Ajax CDN")
- [jQuery UI 1.8.6](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 na Microsoft Ajax CDN")
- [jQuery UI 1.8.5](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a>jQuery ověření verze na CDN

Následující verze knihovny jQuery ověření jsou hostované na této CDN. Klepnutím na odkaz zobrazit skutečný seznam souborů.

- [jQuery ověřením 1.17.0](jquery-validate/cdnjqueryvalidate1170.md "jQuery ověření 1.17.0")
- [jQuery ověřením 1.16.0](jquery-validate/cdnjqueryvalidate1160.md "jQuery ověření 1.16.0")
- [jQuery ověřením 1.15.1](jquery-validate/cdnjqueryvalidate1151.md "jQuery ověření 1.15.1")
- [jQuery ověřením 1.15.0](jquery-validate/cdnjqueryvalidate1150.md "jQuery ověření 1.15.0")
- [jQuery ověřením 1.14.0](jquery-validate/cdnjqueryvalidate1140.md "jQuery ověření 1.14.0")
- [jQuery ověřením 1.13.1](jquery-validate/cdnjqueryvalidate1131.md "jQuery ověření 1.13.1")
- [jQuery ověřením 1.13.0](jquery-validate/cdnjqueryvalidate1130.md "jQuery ověření 1.13.0")
- [jQuery ověřením 1.12.0](jquery-validate/cdnjqueryvalidate1120.md "jQuery ověření 1.12.0")
- [jQuery ověřením 1.11.1](jquery-validate/cdnjqueryvalidate1111.md "jQuery ověření 1.11.1")
- [jQuery ověřením 1.11.0](jquery-validate/cdnjqueryvalidate111.md "jQuery ověření 1.11.0")
- [jQuery ověřením 1.10.0](jquery-validate/cdnjqueryvalidate110.md "jQuery ověření 1.10.0")
- [jQuery ověřením 1.9](jquery-validate/cdnjqueryvalidate19.md "jquery.validate verze 1.9")
- [jQuery ověřením 1.8.1](jquery-validate/cdnjqueryvalidate181.md "jquery.validate verze 1.8.1")
- [jQuery ověřením 1.8](jquery-validate/cdnjqueryvalidate18.md "jquery.validate verze 1.8")
- [jQuery ověřením 1.7](jquery-validate/cdnjqueryvalidate17.md "jquery.validate verze 1.7")
- [jQuery ověřením 1.6](jquery-validate/cdnjqueryvalidate16.md "jQuery ověřením 1.6")
- [jQuery ověřením 1.5.5](jquery-validate/cdnjqueryvalidate155.md "jQuery ověřením 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a>jQuery Mobile verze na CDN

Následující verze knihovny jQuery Mobile jsou hostované na této CDN. Klepnutím na odkaz zobrazit skutečný seznam souborů.

- [jQuery Mobile 1.4.5](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 na Microsoft Ajax CDN")
- [jQuery Mobile 1.4.2](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 na Microsoft Ajax CDN")
- [jQuery Mobile 1.4.1](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 na Microsoft Ajax CDN")
- [jQuery Mobile 1.4.0](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 na Microsoft Ajax CDN")
- [jQuery Mobile 1.3.2](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 na Microsoft Ajax CDN")
- [jQuery Mobile 1.3.1](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 na Microsoft Ajax CDN")
- [jQuery Mobile 1.3.0](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 na Microsoft Ajax CDN")
- [jQuery Mobile 1.2.0](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 na Microsoft Ajax CDN")
- [jQuery Mobile 1.1.2](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 na Microsoft Ajax CDN")
- [jQuery Mobile 1.1.1](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 na Microsoft Ajax CDN")
- [jQuery Mobile 1.1.0](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 na Microsoft Ajax CDN")
- [jQuery Mobile 1.1.0 RC 2](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 na Microsoft Ajax CDN")
- [jQuery Mobile 1.0.1](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 na Microsoft Ajax CDN")
- [jQuery Mobile 1.0](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 na Microsoft Ajax CDN")
- [jQuery Mobile 1.0 RC 2](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 na Microsoft Ajax CDN")
- [jQuery Mobile 1.0 RC 1](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 na Microsoft Ajax CDN")
- [jQuery Mobile 1.0 beta 3](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 na Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a>jQuery šablony verze na CDN

Následující verze modulu plug-in jQuery šablony jsou hostovány na tento CDN. Klepnutím na odkaz zobrazit skutečný seznam souborů.

- [jQuery šablony Beta 1](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery šablony Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a>jQuery cyklus verze na CDN

Následující verze modulu plug-in jQuery cyklus jsou hostované na této CDN. Klepnutím na odkaz zobrazit skutečný seznam souborů.

- [jQuery cyklus 2.99](jquery-cycle/cdnjquerycycle299.md "jQuery 2.99 cyklu")
- [jQuery cyklus 2.94](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 cyklu")
- [jQuery cyklus 2,88](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 cyklu")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a>jQuery DataTables verze na CDN

Následující verze modulu plug-in jQuery DataTables jsou hostované na této CDN. Klepnutím na odkaz zobrazit skutečný seznam souborů.

- [jQuery DataTables 1.10.5](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [jQuery DataTables 1.10.4](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [jQuery DataTables otázku 1.9.4](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables otázku 1.9.4")
- [jQuery DataTables 1.9.3](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [jQuery DataTables 1.9.2](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [jQuery DataTables 1.9.1](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [jQuery DataTables 1.9.0](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [jQuery DataTables 1.8.2](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a>Verze Modernizr na CDN

Následující verze systému [Modernizr](http://www.modernizr.com "Modernizr") jsou hostované na CDN:

- http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js
- http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js
- http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js
- http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js
- http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js
- http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a>Verze JSHint na CDN

Následující verze systému [JSHint](http://www.jshint.com "JSHint") jsou hostované na CDN:

- http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a>Verze knockout na CDN

Následující verze systému [Knockout](http://www.knockoutjs.com "Knockout") jsou hostované na CDN:

- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.Debug.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.Debug.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.Debug.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.Debug.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.Debug.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.Debug.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.Debug.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.Debug.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.Debug.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.js
- http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.Debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a>Globalizace verze na CDN

Následující verze systému [Globalize](https://github.com/jquery/globalize "Globalize") jsou hostované na CDN:

#### <a name="globalize-version-100"></a>Globalizace verze 1.0.0

- http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize.js
- http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/node-Main.js
- http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Currency.js
- http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Date.js
- http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Message.js
- http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Number.js
- http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Plural.js
- http://AJAX.aspnetcdn.com/AJAX/Globalize/1.0.0/Globalize/Relative-Time.js

#### <a name="globalize-version-011"></a>Globalizace verze 0.1.1

- http://AJAX.aspnetcdn.com/AJAX/Globalize/0.1.1/Globalize.min.js
- http://AJAX.aspnetcdn.com/AJAX/Globalize/0.1.1/Globalize.js
- http://AJAX.aspnetcdn.com/AJAX/Globalize/0.1.1/cultures/Globalize.cultures.js

    - všechny jazykové verze
- http://AJAX.aspnetcdn.com/AJAX/Globalize/0.1.1/cultures/Globalize.Culture. {jazyková verze kódu} .js

    - "{Jazyková verze kódu}" nahraďte kód požadovanou jazykovou verzi, například Microsoft globalize.culture.en GB.js== soubory CDN == tyto knihovny byly odeslaný společnosti Microsoft.

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a>Odpověď verze na CDN

Následující verze systému [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") reakce hostovaných na CDN:

#### <a name="respond-version-142"></a>Verze 1.4.2 reagovat

- http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.js
- http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.min.js
- http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.matchmedia.addListener.js
- http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a>Verze 1.4.1 reagovat

- http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.js
- http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.min.js
- http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.matchmedia.addListener.js
- http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a>Verze 1.4.0 reagovat

- http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.js
- http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.min.js
- http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.matchmedia.addListener.js
- http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a>Verze 1.3.0 reagovat

- http://AJAX.aspnetcdn.com/AJAX/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a>Verze 1.2.0 reagovat

- http://AJAX.aspnetcdn.com/AJAX/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a>Zavedení verze na CDN

Následující verze systému [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap hostovaných na CDN:

#### <a name="bootstrap-version-337"></a>Zavedení verze 3.3.7

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2

#### <a name="bootstrap-version-336"></a>Zavedení verze 3.3.6

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2

#### <a name="bootstrap-version-335"></a>Zavedení verze 3.3.5

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2

#### <a name="bootstrap-version-334"></a>Zavedení verze 3.3.4

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2

#### <a name="bootstrap-version-332"></a>Zavedení verze 3.3.2

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2

#### <a name="bootstrap-version-331"></a>Zavedení verze 3.3.1

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-330"></a>Zavedení verze 3.3.0

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-320"></a>Zavedení verze 3.2.0

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-311"></a>Zavedení verze 3.1.1

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-310"></a>Zavedení verze 3.1.0

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS.map
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-303"></a>Zavedení verze 3.0.3

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-302"></a>Zavedení verze 3.0.2

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-301"></a>Zavedení verze 3.0.1

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-300"></a>Zavedení verze 3.0.0

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.eot
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-232"></a>Zavedení verze 2.3.2

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings.PNG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings-white.PNG

#### <a name="bootstrap-version-231"></a>Zavedení verze 2.3.1

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings.PNG
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings-white.PNG

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a>Zavedení TouchCarousel verze na CDN

Následující verze systému [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel verze jsou hostované na CDN :

#### <a name="bootstrap-touchcarousel-version-080"></a>Zavedení TouchCarousel verze 0.8.0

- http://AJAX.aspnetcdn.com/AJAX/Bootstrap-touch-carousel/0.8.0/CSS/Bootstrap-touch-carousel.CSS
- http://AJAX.aspnetcdn.com/AJAX/Bootstrap-touch-carousel/0.8.0/js/Bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a>Verze hammer.js na CDN

Následující verze systému [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js verze jsou hostované na CDN:

#### <a name="hammerjs-version-204"></a>Verze hammer.js verze 2.0.4

- http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js
- http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.js
- http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a>Webové formuláře ASP.NET a Ajax verze na CDN

Následující verze knihovny ASP.NET Ajax jsou hostované na CDN. Klepnutím na odkaz zobrazit skutečný seznam souborů.

- [Webové formuláře ASP.NET a Ajax verze 4.5.2](cdnajax452.md "webových formulářů ASP.NET a Ajax 4.5.2")
- [Webové formuláře ASP.NET a Ajax verze 4](cdnajax4.md "webových formulářů ASP.NET a Ajax 4")
- [Technologie ASP.NET Ajax verze 3.5](cdnajax35.md "prvku ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a>ASP.NET MVC uvolní na CDN

Následující soubory ASP.NET MVC JavaScript jsou hostované na této CDN:

#### <a name="aspnet-mvc-523"></a>ASP.NET MVC 5.2.3

- http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a>ASP.NET MVC 5.1

- http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a>ASP.NET MVC 5.0

- http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a>ASP.NET MVC 4.0

- http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a>ASP.NET MVC 3.0

- http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.min.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.min.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js

#### <a name="aspnet-mvc-20"></a>ASP.NET MVC 2.0

- http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js

#### <a name="aspnet-mvc-10"></a>ASP.NET MVC 1,0

- http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a>Funkce SignalR technologie ASP.NET uvolní na CDN

Následující soubory ASP.NET SignalR JavaScript jsou hostované na této CDN:

#### <a name="aspnet-signalr-222"></a>Funkce SignalR technologie ASP.NET 2.2.2

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a>Funkce SignalR technologie ASP.NET 2.2.1

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a>Funkce SignalR technologie ASP.NET 2.2.0

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a>Funkce SignalR technologie ASP.NET 2.1.0

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.1.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a>Funkce SignalR technologie ASP.NET 2.0.3

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a>Funkce SignalR technologie ASP.NET bodu 2.0.2

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a>Funkce SignalR technologie ASP.NET 2.0.1

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a>Funkce SignalR technologie ASP.NET 2.0.0

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a>Funkce SignalR technologie ASP.NET 1.1.3

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a>Funkce SignalR technologie ASP.NET 1.1.2

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a>Funkce SignalR technologie ASP.NET 1.1.1

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a>Funkce SignalR technologie ASP.NET 1.1.0

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a>Funkce SignalR technologie ASP.NET 1.0.1

- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.0.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.0.1.js

Informace o podmínky použití pro CDN najdete v tématu [Microsoft Ajax CDN podmínky použití](https://www.asp.net/terms-of-use "Microsoft Ajax CDN podmínky použití").
