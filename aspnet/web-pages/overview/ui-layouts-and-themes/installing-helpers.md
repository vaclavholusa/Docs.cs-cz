---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalace pomocné rutiny v rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs
author: tfitzmac
description: Tento článek popisuje postup instalace pomocné rutiny na webu technologie ASP.NET Web Pages (Razor). Pomocné rutiny je opakovaně použitelné komponenty, která obsahuje kód a kód do za...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="97e51-104">Instalace pomocné rutiny v Web Pages (Razor) technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="97e51-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="97e51-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="97e51-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="97e51-106">Tento článek popisuje postup instalace pomocné rutiny na webu technologie ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="97e51-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="97e51-107">A *pomocná* je znovu použitelné komponentní, která obsahuje kód a značky provést úlohu, která může být zdlouhavé nebo komplexní.</span><span class="sxs-lookup"><span data-stu-id="97e51-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="97e51-108">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="97e51-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="97e51-109">Postup instalace pomocné rutiny na webové stránce vytvořené pomocí služby WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="97e51-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="97e51-110">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="97e51-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="97e51-111">Služba WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="97e51-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="97e51-112">Přehled pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="97e51-112">Overview of Helpers</span></span>

<span data-ttu-id="97e51-113">Některé úlohy, které uživatelé často chtějí provést na webových stránkách vyžadovat velké množství kódu nebo vyžadovat další znalostní báze.</span><span class="sxs-lookup"><span data-stu-id="97e51-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="97e51-114">Mezi příklady patří zobrazení grafu pro data; ukládání tlačítko "Následovat" Twitter na stránku; odesílání e-mailu z webu; oříznutí nebo změna velikosti obrázků; pomocí služby PayPal pro váš web.</span><span class="sxs-lookup"><span data-stu-id="97e51-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="97e51-115">Chcete-li snadné provést tyto druhy věcí, rozhraní ASP.NET Web Pages vám umožní používat *Pomocníci*.</span><span class="sxs-lookup"><span data-stu-id="97e51-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="97e51-116">Pomocné rutiny jsou součásti nainstalovat pro lokalitu a, ve kterých je typické úlohy provádět pomocí právě řádku nebo dva kódu Razor.</span><span class="sxs-lookup"><span data-stu-id="97e51-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="97e51-117">ASP.NET – webové stránky má několik Pomocníci součástí.</span><span class="sxs-lookup"><span data-stu-id="97e51-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="97e51-118">Mnoho pomocné rutiny jsou však k dispozici v balíčcích (doplňky), které jsou k dispozici pomocí Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="97e51-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="97e51-119">NuGet umožňuje vybrat balíček pro instalaci a pak se stará o všechny podrobnosti instalace.</span><span class="sxs-lookup"><span data-stu-id="97e51-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="97e51-120">Instalace pomocné rutiny ve službě WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="97e51-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="97e51-121">Ve službě WebMatrix 3, klikněte na **NuGet** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="97e51-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Dialogové okno Galerie NuGet ve službě WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="97e51-123">Tím se spustí Správce balíčků NuGet a zobrazí se dostupné balíčky.</span><span class="sxs-lookup"><span data-stu-id="97e51-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="97e51-124">Do vyhledávacího pole zadejte klíčové slovo pomocné rutiny, které chcete nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="97e51-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Dialogové okno Galerie NuGet ve službě WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="97e51-126">Vyberte balíček a pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="97e51-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="97e51-127">Klikněte na tlačítko **Ano** když se zobrazí dotaz, pokud chcete nainstalovat balíček a uvést, že souhlasíte s podmínkami.</span><span class="sxs-lookup"><span data-stu-id="97e51-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="97e51-128">Pokud je to první, kdy jste nainstalovali pomocné rutiny, vytvoří NuGet složek ve vašem webu pro kód, který tvoří pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="97e51-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="97e51-129">Chcete-li odinstalovat pomocné rutiny, klikněte na tlačítko **Galerie** tlačítko, klikněte na tlačítko **nainstalovaná** kartě a vyberte balíček, který chcete odinstalovat.</span><span class="sxs-lookup"><span data-stu-id="97e51-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="97e51-130">Instalace Pomocníka služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="97e51-130">Installing the Twitter helper</span></span>

<span data-ttu-id="97e51-131">Nejnovější verzi rozhraní API služby Twitter není kompatibilní s Pomocník Twitter, který nainstalujete prostřednictvím balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="97e51-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="97e51-132">Místo toho přejděte [Pomocník Twitter pomocí služby WebMatrix](twitter-helper.md) najdete informace o tom, jak nastavit Pomocník Twitter ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="97e51-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="97e51-133">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="97e51-133">Additional Resources</span></span>


[<span data-ttu-id="97e51-134">Představení ASP.NET Web Pages 2 – základy programování</span><span class="sxs-lookup"><span data-stu-id="97e51-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="97e51-135">Pomocník Twitter pomocí služby WebMatrix</span><span class="sxs-lookup"><span data-stu-id="97e51-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
