---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: "Začínáme s Toolkitu AJAX (C#) | Microsoft Docs"
author: microsoft
description: "Přečtěte si vše, co potřebujete vědět, abyste mohli začít používat sadu ovládacích prvků AJAX."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d3f4dd26a9f82dce78b1c3665f9da6b54bdacba
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="59c14-103">Začínáme s Toolkitu AJAX (C#)</span><span class="sxs-lookup"><span data-stu-id="59c14-103">Get Started with the AJAX Control Toolkit (C#)</span></span>
====================
<span data-ttu-id="59c14-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="59c14-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="59c14-105">Přečtěte si vše, co potřebujete vědět, abyste mohli začít používat sadu ovládacích prvků AJAX.</span><span class="sxs-lookup"><span data-stu-id="59c14-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="59c14-106">Toolkitu AJAX obsahuje více než 30 volné ovládací prvky, které můžete použít v aplikacích ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="59c14-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="59c14-107">V tomto kurzu zjistěte, jak stáhnout sadu ovládacích prvků AJAX a přidání nástrojů ovládacích prvků do vaší sady nástrojů Visual Studio nebo Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="59c14-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="59c14-108">Stahování Toolkitu AJAX</span><span class="sxs-lookup"><span data-stu-id="59c14-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="59c14-109">[Sadu ovládacích prvků AJAX](http://devexpress.com/act) opensourcový projekt vyvinutý členy komunity ASP.NET a týmem ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="59c14-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 


<span data-ttu-id="59c14-110">[![Stahování Toolkitu AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="59c14-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span></span>

<span data-ttu-id="59c14-111">**Obrázek 01**: stahování Toolkitu AJAX ([Kliknutím zobrazit obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="59c14-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>


<span data-ttu-id="59c14-112">Po stažení souboru, budete muset odblokovat soubor.</span><span class="sxs-lookup"><span data-stu-id="59c14-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="59c14-113">Klikněte pravým tlačítkem na soubor, vyberte vlastnosti a klikněte **Odblokovat** tlačítko (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="59c14-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


<span data-ttu-id="59c14-114">[![Odblokování soubor ZIP Toolkit AJAX ovládací prvek](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="59c14-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span></span>

<span data-ttu-id="59c14-115">**Obrázek 02**: odblokování soubor ZIP Toolkit řízení AJAX ([Kliknutím zobrazit obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="59c14-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>


<span data-ttu-id="59c14-116">Po odblokování souboru můžete rozbalte soubor: klikněte pravým tlačítkem na soubor a vyberte **Extrahovat vše** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="59c14-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="59c14-117">Nyní jsme připraveni pro přidání do sady nástrojů Visual Studio nebo Visual Web Developer sady nástrojů.</span><span class="sxs-lookup"><span data-stu-id="59c14-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="59c14-118">Přidání Toolkitu AJAX do sady nástrojů</span><span class="sxs-lookup"><span data-stu-id="59c14-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="59c14-119">Nejjednodušší způsob, jak používat sadu ovládacích prvků AJAX je přidání sady nástrojů do vaší sady nástrojů Visual Studio nebo Visual Web Developer (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="59c14-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="59c14-120">Tímto způsobem můžete jednoduše přetáhnout toolkit ovládacího prvku na stránku Pokud chcete použít.</span><span class="sxs-lookup"><span data-stu-id="59c14-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


<span data-ttu-id="59c14-121">[![Sadu ovládacích prvků AJAX se zobrazí v sadě nástrojů](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="59c14-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span></span>

<span data-ttu-id="59c14-122">**Obrázek 03**: sadu ovládacích prvků AJAX se zobrazí v sadě nástrojů ([Kliknutím zobrazit obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="59c14-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>


<span data-ttu-id="59c14-123">Nejdřív musíte přidat kartě sadu ovládacích prvků AJAX do sady nástrojů.</span><span class="sxs-lookup"><span data-stu-id="59c14-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="59c14-124">Postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="59c14-124">Follow these steps.</span></span>

1. <span data-ttu-id="59c14-125">Vytvoření nového webu ASP.NET tak, že vyberete možnost nabídky soubor, nový web.</span><span class="sxs-lookup"><span data-stu-id="59c14-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="59c14-126">Dvakrát klikněte na Default.aspx v okně Průzkumníka řešení k otevření souboru v editoru.</span><span class="sxs-lookup"><span data-stu-id="59c14-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="59c14-127">Sada nástrojů pod na kartě Obecné klikněte pravým tlačítkem a vyberte možnost nabídky **přidat karta** (viz obrázek 4).</span><span class="sxs-lookup"><span data-stu-id="59c14-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="59c14-128">Zadejte novou kartu s názvem sadu ovládacích prvků AJAX.</span><span class="sxs-lookup"><span data-stu-id="59c14-128">Enter a new tab named AJAX Control Toolkit.</span></span>


<span data-ttu-id="59c14-129">[![Přidání novou kartu](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="59c14-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span></span>

<span data-ttu-id="59c14-130">**Obrázek 04**: Přidání novou kartu ([Kliknutím zobrazit obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="59c14-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>


<span data-ttu-id="59c14-131">Dále musíte přidat ovládací prvky sadu ovládacích prvků AJAX na novou kartu. Postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="59c14-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="59c14-132">Klikněte pravým tlačítkem na pod kartě sadu ovládacích prvků AJAX a vyberte možnost nabídky **zvolit položky (viz obrázek 5)**.</span><span class="sxs-lookup"><span data-stu-id="59c14-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="59c14-133">Přejděte do umístění, kde unzipped Toolkitu AJAX a vyberte AjaxControlToolkit.dll sestavení.</span><span class="sxs-lookup"><span data-stu-id="59c14-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


<span data-ttu-id="59c14-134">[![Zvolte položky, které chcete přidat do sady nástrojů](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="59c14-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span></span>

<span data-ttu-id="59c14-135">**Obrázek 05**: Zvolte položky, které chcete přidat do sady nástrojů ([Kliknutím zobrazit obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="59c14-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>


<span data-ttu-id="59c14-136">Po dokončení těchto kroků se ve vašem panelu nástrojů zobrazí všechny ovládací prvky toolkit.</span><span class="sxs-lookup"><span data-stu-id="59c14-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="59c14-137">Upgrade na novou verzi sady nástrojů</span><span class="sxs-lookup"><span data-stu-id="59c14-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="59c14-138">Pokud jste používali starší verze sady nástrojů a teď potřebujete přesunout do novější verze jsou doporučené kroky:</span><span class="sxs-lookup"><span data-stu-id="59c14-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="59c14-139">Binární soubory – odstranit stará verze sestavení AjaxControlToolkit.dll ze složky koše webu.</span><span class="sxs-lookup"><span data-stu-id="59c14-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="59c14-140">Položek sady nástrojů – odstranit kartě sadu ovládacích prvků AJAX a znovu vytvořit na kartě s novou verzi sestavení AjaxControlToolkit.dll výše uvedených kroků.</span><span class="sxs-lookup"><span data-stu-id="59c14-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="59c14-141">Další</span><span class="sxs-lookup"><span data-stu-id="59c14-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
