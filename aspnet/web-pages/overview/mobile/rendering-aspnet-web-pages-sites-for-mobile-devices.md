---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Vykreslování rozhraní ASP.NET Web Pages (Razor) lokality pro mobilní zařízení | Microsoft Docs
author: tfitzmac
description: 'Tento článek popisuje postup vytváření stránek v serveru technologie ASP.NET Web Pages (Razor), který vykreslí správně na mobilních zařízeních. Co se dozvíte: jak můžete...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 641798c5b835be959d02dd0d854b61ca21d83016
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897316"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="93cb2-104">Vykreslování ASP.NET – webové stránky (Razor) servery pro mobilní zařízení</span><span class="sxs-lookup"><span data-stu-id="93cb2-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="93cb2-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="93cb2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="93cb2-106">Tento článek popisuje postup vytváření stránek v serveru technologie ASP.NET Web Pages (Razor), který vykreslí správně na mobilních zařízeních.</span><span class="sxs-lookup"><span data-stu-id="93cb2-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="93cb2-107">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="93cb2-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="93cb2-108">Jak používat zásady vytváření názvů určíte, že na stránce je určený speciálně pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="93cb2-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="93cb2-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="93cb2-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="93cb2-110">Rozhraní ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="93cb2-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="93cb2-111">V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="93cb2-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="93cb2-112">ASP.NET – webové stránky umožňuje vytvářet vlastní zobrazení pro vykreslování obsah na mobilní telefon nebo jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="93cb2-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="93cb2-113">Nejjednodušší způsob, jak vytvořit stránku specifické pro zařízení v stránku webové stránky ASP.NET je pomocí vzor pojmenovávání souborů takto: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="93cb2-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: <em>FileName.</em><em>Mobile</em><em>.cshtml</em>.</span></span> <span data-ttu-id="93cb2-114">Můžete vytvořit dvě verze stránky (například jednu s názvem <em>MyFile.cshtml</em> a jednu s názvem <em>MyFile.Mobile.cshtml</em>).</span><span class="sxs-lookup"><span data-stu-id="93cb2-114">You can create two versions of a page (for example, one named <em>MyFile.cshtml</em> and one named <em>MyFile.Mobile.cshtml</em>).</span></span> <span data-ttu-id="93cb2-115">V době, kdy požadavky na mobilní zařízení spuštění <em>MyFile.cshtml</em>, ASP.NET vykreslí obsah z <em>MyFile.Mobile.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="93cb2-115">At run time, when a mobile device requests <em>MyFile.cshtml</em>, ASP.NET renders the content from <em>MyFile.Mobile.cshtml</em>.</span></span> <span data-ttu-id="93cb2-116">V opačném <em>MyFile.cshtml</em> je vykreslen.</span><span class="sxs-lookup"><span data-stu-id="93cb2-116">Otherwise, <em>MyFile.cshtml</em> is rendered.</span></span>

<span data-ttu-id="93cb2-117">Následující příklad ukazuje, jak můžete povolit mobilní vykreslování přidáním stránky obsahu pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="93cb2-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="93cb2-118">*Page1.cshtml* obsahuje obsah a bočním panelu navigace.</span><span class="sxs-lookup"><span data-stu-id="93cb2-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="93cb2-119">*Page1.Mobile.cshtml* obsahuje stejný obsah, ale vynechá na bočním panelu.</span><span class="sxs-lookup"><span data-stu-id="93cb2-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="93cb2-120">V lokalitě služby ASP.NET Web Pages, vytvořte soubor s názvem *Page1.cshtml* a nahraďte aktuální obsah následující kód.</span><span class="sxs-lookup"><span data-stu-id="93cb2-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="93cb2-121">Vytvořte soubor s názvem *Page1.Mobile.cshtml* a nahraďte následující kód existujícího obsahu.</span><span class="sxs-lookup"><span data-stu-id="93cb2-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="93cb2-122">Všimněte si, že mobilní verzi stránce vynechá části navigace pro lepší vykreslování na menší obrazovce.</span><span class="sxs-lookup"><span data-stu-id="93cb2-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="93cb2-123">Spustit prohlížeč pro stolní počítač a přejděte do *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="93cb2-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="93cb2-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="93cb2-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="93cb2-125">Spustit prohlížeč pro mobilní zařízení (nebo emulátoru mobilního zařízení) a přejděte do *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="93cb2-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="93cb2-126">(Všimněte si, že neuvedete *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="93cb2-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="93cb2-127">jako součást adresy URL.) I když požadavek je pro *Page1.cshtml*, rozhraní ASP.NET vykreslí *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="93cb2-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="93cb2-129">K testování mobilních stránek, můžete použít simulátoru mobilních zařízení, která běží na stolním počítači.</span><span class="sxs-lookup"><span data-stu-id="93cb2-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="93cb2-130">Tento nástroj umožňuje testovací webové stránky, jako by vypadal na mobilních zařízeních (to znamená, obvykle s mnohem menšími umožňuje zobrazit oblast).</span><span class="sxs-lookup"><span data-stu-id="93cb2-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="93cb2-131">Jedním z příkladů simulátoru je [uživatele agenta přepínači rozšíření](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) pro aplikaci Mozilla Firefox, která vám umožňuje emulovat různé mobilní prohlížeče z plochy verzi Firefox.</span><span class="sxs-lookup"><span data-stu-id="93cb2-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="93cb2-132">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="93cb2-132">Additional Resources</span></span>


<span data-ttu-id="93cb2-133">[Emulátor Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="93cb2-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
