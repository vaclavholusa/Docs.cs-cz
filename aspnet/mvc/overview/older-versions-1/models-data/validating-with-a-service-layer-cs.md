---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Ověřování pomocí služby vrstvu (C#) | Microsoft Docs
author: StephenWalther
description: Další informace o přesunutí logika ověřování z vaší akce kontroleru a do vrstvy samostatné služby. V tomto kurzu Stephen Walther vysvětluje, jak můžete...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 06042ac197cc54da767a94a44c57eb09bb3db9fa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869033"
---
<a name="validating-with-a-service-layer-c"></a><span data-ttu-id="1917c-104">Ověřování pomocí služby vrstvu (C#)</span><span class="sxs-lookup"><span data-stu-id="1917c-104">Validating with a Service Layer (C#)</span></span>
====================
<span data-ttu-id="1917c-105">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="1917c-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="1917c-106">Další informace o přesunutí logika ověřování z vaší akce kontroleru a do vrstvy samostatné služby.</span><span class="sxs-lookup"><span data-stu-id="1917c-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="1917c-107">V tomto kurzu Stephen Walther vysvětluje, jak je možné uchovávat sharp oddělené oblasti zájmu tak, že izoluje vrstvě služby z vašeho řadiče vrstvy.</span><span class="sxs-lookup"><span data-stu-id="1917c-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="1917c-108">Cílem tohoto kurzu je popisují jednu z metod ověřování v aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1917c-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="1917c-109">V tomto kurzu zjistěte, jak přesunout logika ověřování z řadičů a do vrstvy samostatné služby.</span><span class="sxs-lookup"><span data-stu-id="1917c-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="1917c-110">Oddělení otázky</span><span class="sxs-lookup"><span data-stu-id="1917c-110">Separating Concerns</span></span>

<span data-ttu-id="1917c-111">Když vytvoříte aplikaci ASP.NET MVC, by neměla umístit logika databáze uvnitř vaší akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1917c-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="1917c-112">Kombinování logika databáze a řadič díky aplikaci obtížnější Údržba v průběhu času.</span><span class="sxs-lookup"><span data-stu-id="1917c-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="1917c-113">Doporučuje se, umístěte všechny logika databáze ve vrstvě samostatné úložiště.</span><span class="sxs-lookup"><span data-stu-id="1917c-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="1917c-114">Například výpis 1 obsahuje jednoduché úložiště s názvem ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="1917c-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="1917c-115">Produkt úložiště obsahuje všechny data přístupový kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="1917c-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="1917c-116">Seznam také zahrnuje rozhraní IProductRepository, který implementuje úložiště produktu.</span><span class="sxs-lookup"><span data-stu-id="1917c-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="1917c-117">**Listing 1 -- Models\ProductRepository.cs**</span><span class="sxs-lookup"><span data-stu-id="1917c-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="1917c-118">Řadič v výpis 2 používá vrstvy úložiště v jeho Index() a Create() akce.</span><span class="sxs-lookup"><span data-stu-id="1917c-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="1917c-119">Všimněte si, že tento řadič neobsahuje žádné databáze logiku.</span><span class="sxs-lookup"><span data-stu-id="1917c-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="1917c-120">Vytvoření vrstvy úložiště umožňuje udržovat čistou oddělené oblasti zájmu.</span><span class="sxs-lookup"><span data-stu-id="1917c-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="1917c-121">Řadiče jsou zodpovědní za logiku aplikace toku řízení a úložiště je zodpovědná za data přístup logiku.</span><span class="sxs-lookup"><span data-stu-id="1917c-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="1917c-122">**Výpis 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="1917c-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="1917c-123">Vytvoření vrstvy služby</span><span class="sxs-lookup"><span data-stu-id="1917c-123">Creating a Service Layer</span></span>

<span data-ttu-id="1917c-124">Ano aplikace toku řízení logika patří řadič a data přístup logika patří v úložišti.</span><span class="sxs-lookup"><span data-stu-id="1917c-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="1917c-125">V takovém případě, kde vložíte logika ověřování?</span><span class="sxs-lookup"><span data-stu-id="1917c-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="1917c-126">Jednou z možností je logika ověřování v umístit *vrstvy služby*.</span><span class="sxs-lookup"><span data-stu-id="1917c-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="1917c-127">Vrstva služby je další vrstvy v aplikaci ASP.NET MVC, která zprostředkovává komunikaci mezi řadiče a vrstvy úložiště.</span><span class="sxs-lookup"><span data-stu-id="1917c-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="1917c-128">Vrstva služby obsahuje obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="1917c-128">The service layer contains business logic.</span></span> <span data-ttu-id="1917c-129">Konkrétně obsahuje logiku ověření.</span><span class="sxs-lookup"><span data-stu-id="1917c-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="1917c-130">Například vrstvy služby produktu ve výpisu 3 má CreateProduct() metodu.</span><span class="sxs-lookup"><span data-stu-id="1917c-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="1917c-131">CreateProduct() metoda volá metodu ValidateProduct() k ověření nového produktu před předáním produktu do úložiště produktu.</span><span class="sxs-lookup"><span data-stu-id="1917c-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="1917c-132">**Výpis 3 - Models\ProductService.cs**</span><span class="sxs-lookup"><span data-stu-id="1917c-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="1917c-133">Kontroler produktu se aktualizovalo v výpis 4 používat vrstvu služby namísto vrstvy úložiště.</span><span class="sxs-lookup"><span data-stu-id="1917c-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="1917c-134">Vrstva řadiče komunikuje se vrstvě služby.</span><span class="sxs-lookup"><span data-stu-id="1917c-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="1917c-135">Vrstva služby komunikuje se vrstvy úložiště.</span><span class="sxs-lookup"><span data-stu-id="1917c-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="1917c-136">Jednotlivé úrovně má samostatnou zodpovědnost.</span><span class="sxs-lookup"><span data-stu-id="1917c-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="1917c-137">**Výpis 4 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="1917c-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="1917c-138">Všimněte si, vytvořený službu produktu v konstruktoru řadiče produktu.</span><span class="sxs-lookup"><span data-stu-id="1917c-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="1917c-139">Při vytváření služby ze slovníku stavu modelu je předán do služby.</span><span class="sxs-lookup"><span data-stu-id="1917c-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="1917c-140">Služba produktu používá předávání chybových zpráv ověření zpátky na řadič stavu modelu.</span><span class="sxs-lookup"><span data-stu-id="1917c-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="1917c-141">Oddělení vrstvě služby</span><span class="sxs-lookup"><span data-stu-id="1917c-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="1917c-142">Nemůžeme se nepodařilo izolovat řadiče a vrstvy služby v jednom ohledu.</span><span class="sxs-lookup"><span data-stu-id="1917c-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="1917c-143">Řadiče a vrstvy služby komunikují prostřednictvím stavu modelu.</span><span class="sxs-lookup"><span data-stu-id="1917c-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="1917c-144">Jinými slovy vrstvě služby má závislost na konkrétní funkce rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1917c-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="1917c-145">Chceme izolovat vrstvě služby z našich řadiče vrstvy co nejvíc.</span><span class="sxs-lookup"><span data-stu-id="1917c-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="1917c-146">Teoreticky jsme byste měli mít vrstvy služby pomocí libovolného typu aplikace a ne jenom aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1917c-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="1917c-147">Například v budoucnu může chceme sestavení front-end pro naši aplikaci WPF.</span><span class="sxs-lookup"><span data-stu-id="1917c-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="1917c-148">Jsme by měl zjistit způsob, jak odebrat závislost na rozhraní ASP.NET MVC stav modelu z našich vrstvy služby.</span><span class="sxs-lookup"><span data-stu-id="1917c-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="1917c-149">Ve výpisu 5 vrstvě služby byla aktualizována tak, aby již nepoužíval stavu modelu.</span><span class="sxs-lookup"><span data-stu-id="1917c-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="1917c-150">Místo toho použije všechny třídy, která implementuje rozhraní IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="1917c-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="1917c-151">**Výpis 5 - Models\ProductService.cs (odpojené)**</span><span class="sxs-lookup"><span data-stu-id="1917c-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="1917c-152">Rozhraní IValidationDictionary je definována v výpis 6.</span><span class="sxs-lookup"><span data-stu-id="1917c-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="1917c-153">Toto jednoduché rozhraní má jedné metody a vlastnosti jediné.</span><span class="sxs-lookup"><span data-stu-id="1917c-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="1917c-154">**Výpis 6 - Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="1917c-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="1917c-155">Třídy ve výpisu 7, s názvem třídy ModelStateWrapper implementuje rozhraní IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="1917c-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="1917c-156">Můžete vytvořit instanci třídy ModelStateWrapper předáním slovník stavu modelu konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="1917c-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="1917c-157">**Výpis 7 – Models\ModelStateWrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="1917c-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="1917c-158">Nakonec aktualizované řadiče v výpis 8 používá ModelStateWrapper při vytváření vrstvě služby v jeho konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="1917c-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="1917c-159">**Výpis 8 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="1917c-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="1917c-160">Použití IValidationDictionary rozhraní a třídy ModelStateWrapper umožňuje nám zcela izolovat naše vrstvy služby z našich řadiče vrstvy.</span><span class="sxs-lookup"><span data-stu-id="1917c-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="1917c-161">Už není závislá na stav modelu vrstvě služby.</span><span class="sxs-lookup"><span data-stu-id="1917c-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="1917c-162">Můžete předat všechny třídy, která implementuje rozhraní IValidationDictionary k vrstvě služby.</span><span class="sxs-lookup"><span data-stu-id="1917c-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="1917c-163">Například může aplikace WPF implementovat rozhraní IValidationDictionary s třída jednoduchých kolekcí.</span><span class="sxs-lookup"><span data-stu-id="1917c-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="1917c-164">Souhrn</span><span class="sxs-lookup"><span data-stu-id="1917c-164">Summary</span></span>

<span data-ttu-id="1917c-165">Cílem tohoto kurzu bylo popisují jeden z přístupů k provádění ověření v aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1917c-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="1917c-166">V tomto kurzu jste zjistili, jak přesunout všechny logika ověřování z řadičů a do vrstvy samostatné služby.</span><span class="sxs-lookup"><span data-stu-id="1917c-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="1917c-167">Také jste zjistili, jak izolovat vrstvě služby z vašeho řadiče vrstvy ModelStateWrapper třídu.</span><span class="sxs-lookup"><span data-stu-id="1917c-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1917c-168">[Předchozí](validating-with-the-idataerrorinfo-interface-cs.md)
> [další](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="1917c-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
