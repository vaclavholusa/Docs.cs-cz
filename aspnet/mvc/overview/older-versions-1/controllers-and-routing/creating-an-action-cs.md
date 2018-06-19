---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Vytváření akce (C#) | Microsoft Docs
author: microsoft
description: Zjistěte, jak přidat novou akci k řadiči ASP.NET MVC. Informace o požadavcích pro metodu jako akce.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c6145902db59b07e96a5563b138c1a6323946b2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867902"
---
<a name="creating-an-action-c"></a><span data-ttu-id="0ce59-104">Vytváření akce (C#)</span><span class="sxs-lookup"><span data-stu-id="0ce59-104">Creating an Action (C#)</span></span>
====================
<span data-ttu-id="0ce59-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0ce59-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="0ce59-106">Zjistěte, jak přidat novou akci k řadiči ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0ce59-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="0ce59-107">Informace o požadavcích pro metodu jako akce.</span><span class="sxs-lookup"><span data-stu-id="0ce59-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="0ce59-108">Cílem tohoto kurzu je vysvětlují, jak můžete vytvořit nové akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0ce59-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="0ce59-109">Můžete další informace o požadavcích metody akce.</span><span class="sxs-lookup"><span data-stu-id="0ce59-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="0ce59-110">Také zjistíte, jak zabránit metodu zasahování přímo jako akce.</span><span class="sxs-lookup"><span data-stu-id="0ce59-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="0ce59-111">Přidání akce do řadiče</span><span class="sxs-lookup"><span data-stu-id="0ce59-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="0ce59-112">Přidat novou akci k řadiči přidáním nové metody pro kontroler.</span><span class="sxs-lookup"><span data-stu-id="0ce59-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="0ce59-113">Například řadič v výpis 1 obsahuje akci s názvem Index() a akce s názvem SayHello().</span><span class="sxs-lookup"><span data-stu-id="0ce59-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="0ce59-114">Obě metody jsou zveřejněné jako akce.</span><span class="sxs-lookup"><span data-stu-id="0ce59-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="0ce59-115">**Výpis 1 - Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="0ce59-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="0ce59-116">Chcete-li být vystavený universe jako akce, metody musí splňovat určité požadavky:</span><span class="sxs-lookup"><span data-stu-id="0ce59-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="0ce59-117">Metoda musí být veřejné.</span><span class="sxs-lookup"><span data-stu-id="0ce59-117">The method must be public.</span></span>
- <span data-ttu-id="0ce59-118">Metodu nelze statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="0ce59-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="0ce59-119">Metodu nelze metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="0ce59-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="0ce59-120">Metodu nelze konstruktor, metody getter nebo setter.</span><span class="sxs-lookup"><span data-stu-id="0ce59-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="0ce59-121">Metoda nemůže mít otevřete obecné typy.</span><span class="sxs-lookup"><span data-stu-id="0ce59-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="0ce59-122">Metoda není metoda základní třídy controller.</span><span class="sxs-lookup"><span data-stu-id="0ce59-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="0ce59-123">Nemůže obsahovat metodu **ref** nebo **out** parametry.</span><span class="sxs-lookup"><span data-stu-id="0ce59-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="0ce59-124">Všimněte si, že neexistují žádná omezení návratový typ akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0ce59-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="0ce59-125">Akce kontroleru vrátit řetězec, DateTime, instance třídy náhodných nebo void.</span><span class="sxs-lookup"><span data-stu-id="0ce59-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="0ce59-126">Rozhraní ASP.NET MVC se převést žádný návratový typ, který není výsledek akce do řetězce a vykreslit řetězec do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0ce59-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="0ce59-127">Když přidáte libovolné metody, která není porušují tyto požadavky na řadič, je metoda zpřístupněná jako akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0ce59-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="0ce59-128">Dávejte pozor, zde.</span><span class="sxs-lookup"><span data-stu-id="0ce59-128">Be careful here.</span></span> <span data-ttu-id="0ce59-129">Akce kontroleru můžete vyvolat každý, kdo připojený k Internetu.</span><span class="sxs-lookup"><span data-stu-id="0ce59-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="0ce59-130">Například nevytvářejte DeleteMyWebsite() akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0ce59-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="0ce59-131">Volaná brání veřejná metoda</span><span class="sxs-lookup"><span data-stu-id="0ce59-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="0ce59-132">Pokud potřebujete vytvořit veřejnou metodu do třídy kontroleru a nechcete, aby ke zveřejnění metodu jako akce kontroleru můžete zabránit metody vyvolání pomocí atributu [NonAction].</span><span class="sxs-lookup"><span data-stu-id="0ce59-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="0ce59-133">Například řadič v výpis 2 obsahuje veřejnou metodu s názvem CompanySecrets(), který je upraven pomocí atributu [NonAction].</span><span class="sxs-lookup"><span data-stu-id="0ce59-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="0ce59-134">**Výpis 2 - Controllers\WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="0ce59-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="0ce59-135">Pokud se pokusíte vyvolání akce kontroleru CompanySecrets() zadáním /Work/CompanySecrets do adresního řádku prohlížeče budete získání chybové zprávy na obrázku 1.</span><span class="sxs-lookup"><span data-stu-id="0ce59-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="0ce59-136">[![Volání metody NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0ce59-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="0ce59-137">**Obrázek 01**: volání metody NonAction ([Kliknutím zobrazit obrázek v plné velikosti](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="0ce59-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0ce59-138">[Předchozí](creating-a-controller-cs.md)
> [další](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0ce59-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
