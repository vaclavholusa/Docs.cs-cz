---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Vytvoření akce (VB) | Dokumentace Microsoftu
author: microsoft
description: Zjistěte, jak přidat novou akci kontroler ASP.NET MVC. Další informace o požadavcích pro metodu na akci.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 070dd4c9d68327eec52fe385000b9ca3907eaa9f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756411"
---
<a name="creating-an-action-vb"></a><span data-ttu-id="f96c4-104">Vytvoření akce (VB)</span><span class="sxs-lookup"><span data-stu-id="f96c4-104">Creating an Action (VB)</span></span>
====================
<span data-ttu-id="f96c4-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f96c4-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f96c4-106">Zjistěte, jak přidat novou akci kontroler ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f96c4-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="f96c4-107">Další informace o požadavcích pro metodu na akci.</span><span class="sxs-lookup"><span data-stu-id="f96c4-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="f96c4-108">Cílem tohoto kurzu je vysvětlují, jak můžete vytvořit nové akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f96c4-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="f96c4-109">Informace o požadavky na metodu akce.</span><span class="sxs-lookup"><span data-stu-id="f96c4-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="f96c4-110">Také se dozvíte, jak zabránit metodu vystaven jako akci.</span><span class="sxs-lookup"><span data-stu-id="f96c4-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="f96c4-111">Přidání akce Kontroleru</span><span class="sxs-lookup"><span data-stu-id="f96c4-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="f96c4-112">Přidejte novou akci k řadiči tak, že přidáte nové metody pro kontroler.</span><span class="sxs-lookup"><span data-stu-id="f96c4-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="f96c4-113">Například řadič v informacích 1 obsahuje akce s názvem Index() a akce s názvem SayHello().</span><span class="sxs-lookup"><span data-stu-id="f96c4-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="f96c4-114">Obě metody jsou vystaveny jako akce.</span><span class="sxs-lookup"><span data-stu-id="f96c4-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="f96c4-115">**Výpis 1 - Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="f96c4-115">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

<span data-ttu-id="f96c4-116">Aby bylo možné vystavit rozhraní universe jako akci, metoda musí splňovat určité požadavky:</span><span class="sxs-lookup"><span data-stu-id="f96c4-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="f96c4-117">Metoda musí být veřejné.</span><span class="sxs-lookup"><span data-stu-id="f96c4-117">The method must be public.</span></span>
- <span data-ttu-id="f96c4-118">Metoda nemůže být statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="f96c4-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="f96c4-119">Metoda nemůže být metodou rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f96c4-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="f96c4-120">Metoda nemůže být konstruktor, metoda getter nebo setter.</span><span class="sxs-lookup"><span data-stu-id="f96c4-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="f96c4-121">Metoda nemůže mít otevřených obecných typů.</span><span class="sxs-lookup"><span data-stu-id="f96c4-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="f96c4-122">Metoda není metodu základní třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f96c4-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="f96c4-123">Metoda nemůže obsahovat **ref** nebo **si** parametry.</span><span class="sxs-lookup"><span data-stu-id="f96c4-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="f96c4-124">Všimněte si, že neexistují žádná omezení u návratového typu akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f96c4-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="f96c4-125">Akce kontroleru vrátit řetězec, DateTime, instance třídy Random nebo void.</span><span class="sxs-lookup"><span data-stu-id="f96c4-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="f96c4-126">Architektura ASP.NET MVC se převést návratový typ, který není výsledek akce do řetězce a vykreslit řetězec, který se v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f96c4-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="f96c4-127">Když přidáte jakoukoli metodu, která nebudou porušovat tyto požadavky na řadič, metoda je vystavena jako akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f96c4-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="f96c4-128">Buďte opatrní tady.</span><span class="sxs-lookup"><span data-stu-id="f96c4-128">Be careful here.</span></span> <span data-ttu-id="f96c4-129">Akce kontroleru můžete vyvolat každý připojený k Internetu.</span><span class="sxs-lookup"><span data-stu-id="f96c4-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="f96c4-130">Ne, například vytvořit DeleteMyWebsite() akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f96c4-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="f96c4-131">Brání veřejnou metodu volanou</span><span class="sxs-lookup"><span data-stu-id="f96c4-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="f96c4-132">Pokud je potřeba vytvořit veřejnou metodu do třídy kontroleru a nechcete vystavit metodu akce kontroleru, pak můžete zabránit metodu volanou pomocí &lt;NonAction&gt; atribut.</span><span class="sxs-lookup"><span data-stu-id="f96c4-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the &lt;NonAction&gt; attribute.</span></span> <span data-ttu-id="f96c4-133">Například kontroler v informacích 2 obsahuje veřejnou metodu s názvem CompanySecrets(), která je upravena pomocí &lt;NonAction&gt; atribut.</span><span class="sxs-lookup"><span data-stu-id="f96c4-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the &lt;NonAction&gt; attribute.</span></span>

<span data-ttu-id="f96c4-134">**Výpis 2 - Controllers\WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="f96c4-134">**Listing 2 - Controllers\WorkController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

<span data-ttu-id="f96c4-135">Pokud při pokusu o vyvolání akce kontroleru CompanySecrets() zadáním /Work/CompanySecrets do adresního řádku prohlížeče zobrazí se zpráva chyby na obrázku 1.</span><span class="sxs-lookup"><span data-stu-id="f96c4-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="f96c4-136">[![Volání metody NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f96c4-136">[![Invoking a NonAction method](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span></span>

<span data-ttu-id="f96c4-137">**Obrázek 01**: volání metody NonAction ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-an-action-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f96c4-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-vb/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f96c4-138">[Předchozí](creating-a-controller-vb.md)
> [další](aspnet-mvc-controllers-overview-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f96c4-138">[Previous](creating-a-controller-vb.md)
[Next](aspnet-mvc-controllers-overview-cs.md)</span></span>
