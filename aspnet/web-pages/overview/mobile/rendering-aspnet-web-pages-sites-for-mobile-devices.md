---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Vykreslování v prostředí ASP.NET stránky (Razor) serverů pro mobilní zařízení | Dokumentace Microsoftu
author: tfitzmac
description: 'Tento článek popisuje, jak vytvářet stránky na webu rozhraní ASP.NET Web Pages (Razor), která se vykreslí správně na mobilních zařízeních. Co se dozvíte: jak budete...'
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: dcc44e0d78814ef9a8537b01efa17259a12e9c76
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816340"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="a1411-104">Vykreslení webů s ASP.NET webovými stránkami (Razor) pro mobilní zařízení</span><span class="sxs-lookup"><span data-stu-id="a1411-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="a1411-105">podle [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a1411-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a1411-106">Tento článek popisuje, jak vytvářet stránky na webu rozhraní ASP.NET Web Pages (Razor), která se vykreslí správně na mobilních zařízeních.</span><span class="sxs-lookup"><span data-stu-id="a1411-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="a1411-107">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="a1411-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="a1411-108">Jak používat zásady vytváření názvů určit, že na stránce je určený speciálně pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="a1411-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a1411-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="a1411-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a1411-110">Webové stránky ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="a1411-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="a1411-111">V tomto kurzu se také pracuje s ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="a1411-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="a1411-112">Webové stránky ASP.NET umožňuje vytvářet vlastní zobrazení pro vykreslení obsahu na mobilní telefon nebo jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="a1411-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="a1411-113">Nejjednodušší způsob, jak vytvořit stránku specifická pro zařízení na webu rozhraní ASP.NET Web Pages je pomocí vzoru pro pojmenovávání souborů následujícím způsobem: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="a1411-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: <em>FileName.</em><em>Mobile</em><em>.cshtml</em>.</span></span> <span data-ttu-id="a1411-114">Můžete vytvořit dvě verze stránky (například jeden s názvem <em>MyFile.cshtml</em> a jeden s názvem <em>MyFile.Mobile.cshtml</em>).</span><span class="sxs-lookup"><span data-stu-id="a1411-114">You can create two versions of a page (for example, one named <em>MyFile.cshtml</em> and one named <em>MyFile.Mobile.cshtml</em>).</span></span> <span data-ttu-id="a1411-115">V době, když mobilní zařízení požádá o spuštění <em>MyFile.cshtml</em>, rozhraní ASP.NET vykreslí obsah z <em>MyFile.Mobile.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="a1411-115">At run time, when a mobile device requests <em>MyFile.cshtml</em>, ASP.NET renders the content from <em>MyFile.Mobile.cshtml</em>.</span></span> <span data-ttu-id="a1411-116">V opačném případě <em>MyFile.cshtml</em> vykreslením.</span><span class="sxs-lookup"><span data-stu-id="a1411-116">Otherwise, <em>MyFile.cshtml</em> is rendered.</span></span>

<span data-ttu-id="a1411-117">Následující příklad ukazuje, jak povolit mobilní vykreslování přidáním stránku obsahu pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="a1411-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="a1411-118">*Page1.cshtml* obsahuje obsah a bočním panelu navigace.</span><span class="sxs-lookup"><span data-stu-id="a1411-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="a1411-119">*Page1.Mobile.cshtml* obsahuje stejný obsah, ale vynechá na bočním panelu.</span><span class="sxs-lookup"><span data-stu-id="a1411-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="a1411-120">Na webu rozhraní ASP.NET Web Pages, vytvořte soubor s názvem *Page1.cshtml* a nahraďte následující značky na aktuální obsah.</span><span class="sxs-lookup"><span data-stu-id="a1411-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="a1411-121">Vytvořte soubor s názvem *Page1.Mobile.cshtml* a nahraďte existující obsah následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="a1411-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="a1411-122">Všimněte si, že mobilní verzi stránce vynechá části navigace pro lepší vykreslování na obrazovce menší.</span><span class="sxs-lookup"><span data-stu-id="a1411-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="a1411-123">Spustit prohlížeč pro stolní počítač a přejděte do *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a1411-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="a1411-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a1411-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="a1411-125">Spustit mobilní prohlížeče (nebo emulátoru mobilního zařízení) a přejděte do *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a1411-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="a1411-126">(Všimněte si, že není zadána *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="a1411-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="a1411-127">jako část adresy URL.) I když je požadavek *Page1.cshtml*, rozhraní ASP.NET vykreslí *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a1411-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="a1411-129">K otestování mobilních stránek, můžete použít simulátor mobilní zařízení, na kterém běží na stolním počítači.</span><span class="sxs-lookup"><span data-stu-id="a1411-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="a1411-130">Tento nástroj umožňuje testování webových stránek, jak by vypadala na mobilních zařízeních (to znamená, obvykle s mnohem menší umožňuje zobrazit oblast).</span><span class="sxs-lookup"><span data-stu-id="a1411-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="a1411-131">Jedním z příkladů simulátoru je [doplňků uživatelského agenta přepínání](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) pro Mozilla Firefox, která vám umožňuje emulovat různá mobilní prohlížeče v desktopové verzi Firefox.</span><span class="sxs-lookup"><span data-stu-id="a1411-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="a1411-132">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="a1411-132">Additional Resources</span></span>


<span data-ttu-id="a1411-133">[Emulátor Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="a1411-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
