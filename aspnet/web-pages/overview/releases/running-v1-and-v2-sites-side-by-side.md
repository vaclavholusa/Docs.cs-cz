---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: "S různými verzemi technologie ASP.NET Web Pages (Razor) vedle sebe | Microsoft Docs"
author: tfitzmac
description: "Tento článek vysvětluje, jak spustit weby technologie ASP.NET Web Pages (Razor) na stejném počítači nebo serveru, když k webům, které jsou nakonfigurovány pro použití různých verzí..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: c11399b0bde59d18fa378ed48c15844454c1f956
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="5b207-103">S různými verzemi ASP.NET Web Pages (Razor) vedle sebe</span><span class="sxs-lookup"><span data-stu-id="5b207-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="5b207-104">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5b207-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5b207-105">Tento článek vysvětluje, jak spustit weby technologie ASP.NET Web Pages (Razor) na stejném počítači nebo serveru, když k webům, které jsou nakonfigurovány pro použití různých verzí z webové stránky ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5b207-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="5b207-106">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="5b207-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="5b207-107">Výchozí chování Novinky v technologii ASP.NET Pokud máte webů vytvořených pomocí technologie ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="5b207-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="5b207-108">Postup konfigurace nové lokality ke spuštění ze starší verze z technologie ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="5b207-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="5b207-109">Toto je funkce technologie ASP.NET byla zavedená v článku:</span><span class="sxs-lookup"><span data-stu-id="5b207-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="5b207-110">`webPages:Version` Nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b207-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="5b207-111">Verze softwaru</span><span class="sxs-lookup"><span data-stu-id="5b207-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="5b207-112">Rozhraní ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5b207-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5b207-113">V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2 a ASP.NET Web Pages 1.0.</span><span class="sxs-lookup"><span data-stu-id="5b207-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="5b207-114">ASP.NET – webové stránky podporuje možnost spouštění weby vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="5b207-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="5b207-115">Díky tomu můžete i nadále spouštět starší aplikace technologie ASP.NET Web Pages, vytvářet nové aplikace ASP.NET Web Pages a spustit všechny z nich na stejném počítači.</span><span class="sxs-lookup"><span data-stu-id="5b207-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="5b207-116">Zde jsou některé věci pamatovat při instalaci webových stránek pomocí služby WebMatrix:</span><span class="sxs-lookup"><span data-stu-id="5b207-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="5b207-117">Ve výchozím nastavení bude existující webové stránky aplikace v počítači spustit jako na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="5b207-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="5b207-118">(Sestavení jsou nainstalované v globální mezipaměti sestavení (GAC) a používají automaticky.)</span><span class="sxs-lookup"><span data-stu-id="5b207-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="5b207-119">Pokud chcete spustit lokalitě v jiné verzi z webových stránek ASP.NET, můžete nakonfigurovat k tomuto webu.</span><span class="sxs-lookup"><span data-stu-id="5b207-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="5b207-120">Pokud váš web již nemá *web.config* souboru v kořenovém adresáři serveru, vytvořte novou a zkopírujte následující kód XML do ní, přepsání existujícího obsahu.</span><span class="sxs-lookup"><span data-stu-id="5b207-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="5b207-121">Pokud lokalita již obsahuje *web.config* soubor, přidejte `<appSettings>` element stejný, jako je následující k `<configuration>` oddílu.</span><span class="sxs-lookup"><span data-stu-id="5b207-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
<span data-ttu-id="5b207-122">' – Pokud nezadáte verzi v *web.config* soubor, lokalitu je nasadit jako na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="5b207-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="5b207-123">(Sestavení se zkopírují do *bin* složku v nasazené lokalitě.)</span><span class="sxs-lookup"><span data-stu-id="5b207-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="5b207-124">Nové aplikace, které vytvoříte pomocí šablony webů v matici webové zahrnují verze sestavení webové stránky na webu *bin* složky.</span><span class="sxs-lookup"><span data-stu-id="5b207-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="5b207-125">Obecně platí, můžete vždy řídit kterou verzi webové stránky pro použití s vaší lokality pomocí NuGet pro instalaci příslušné sestavení do lokality *bin* složky.</span><span class="sxs-lookup"><span data-stu-id="5b207-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="5b207-126">Balíčky naleznete [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="5b207-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5b207-127">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="5b207-127">Additional Resources</span></span>

[<span data-ttu-id="5b207-128">Hlavní funkce v rozhraní ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="5b207-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
