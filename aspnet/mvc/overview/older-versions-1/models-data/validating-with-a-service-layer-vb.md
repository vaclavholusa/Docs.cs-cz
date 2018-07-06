---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Ověřování vrstvou služby (VB) | Dokumentace Microsoftu
author: StephenWalther
description: Další informace o přesunutí logiky ověřování mimo vaše akce kontroleru a do samostatné služby vrstvy. V tomto kurzu, Stephen Walther vysvětluje, jak budete...
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 673e9be46e37e9a805f1dae4944f69939b087dda
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836562"
---
<a name="validating-with-a-service-layer-vb"></a><span data-ttu-id="5cb2d-104">Ověřování vrstvou služby (VB)</span><span class="sxs-lookup"><span data-stu-id="5cb2d-104">Validating with a Service Layer (VB)</span></span>
====================
<span data-ttu-id="5cb2d-105">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="5cb2d-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="5cb2d-106">Další informace o přesunutí logiky ověřování mimo vaše akce kontroleru a do samostatné služby vrstvy.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="5cb2d-107">V tomto kurzu Stephen Walther vysvětluje, jak díky čemuž můžete udržovat sharp oddělené oblasti zájmu izolováním vrstvě služby mezi kontroleru.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="5cb2d-108">Cílem tohoto kurzu je k popisu jednu z metod ověřování v aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="5cb2d-109">V tomto kurzu se dozvíte, jak k přesunutí logiky ověřování mimo řadičů a do samostatné služby vrstvy.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="5cb2d-110">Oddělení otázky</span><span class="sxs-lookup"><span data-stu-id="5cb2d-110">Separating Concerns</span></span>

<span data-ttu-id="5cb2d-111">Když sestavíte aplikaci ASP.NET MVC, by neměla logiky databáze umístíte text do vaší akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="5cb2d-112">Kombinování logiky databáze a kontroler je aplikace obtížné udržovat v čase.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="5cb2d-113">Doporučujeme umísťovat celý logiky databáze ve vrstvě oddělené úložiště.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="5cb2d-114">Výpis 1 například obsahuje jednoduché úložiště s názvem ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="5cb2d-115">Úložiště produkt obsahuje všechny kód přístupu k datům pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="5cb2d-116">Seznam zahrnuje také rozhraní IProductRepository, který implementuje úložiště produktu.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="5cb2d-117">**Výpis 1 - Models\ProductRepository.vb**</span><span class="sxs-lookup"><span data-stu-id="5cb2d-117">**Listing 1 - Models\ProductRepository.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

<span data-ttu-id="5cb2d-118">Řadič v informacích 2 používá vrstvy úložiště v jeho Index() a Create() akce.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="5cb2d-119">Všimněte si, že tento kontroler neobsahuje žádnou logiku databáze.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="5cb2d-120">Vytvoření vrstvy úložiště umožňuje udržovat jasně oddělit oblasti zájmu.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="5cb2d-121">Kontrolery jsou zodpovědné za logiku řízení toku aplikace a úložiště je zodpovědný za logikou přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="5cb2d-122">**Výpis 2 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="5cb2d-122">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="5cb2d-123">Vytvoření vrstvy služby</span><span class="sxs-lookup"><span data-stu-id="5cb2d-123">Creating a Service Layer</span></span>

<span data-ttu-id="5cb2d-124">Tedy aplikace tok řízení logika patří kontroler a logiky přístupu k datům patří v úložišti.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="5cb2d-125">V takovém případě místo, kam dáte logiky ověřování?</span><span class="sxs-lookup"><span data-stu-id="5cb2d-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="5cb2d-126">Jednou z možností je umístit logiky ověřování v *vrstva služby*.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="5cb2d-127">Vrstva služby představuje další vrstvu v aplikaci ASP.NET MVC, která zprostředkovává komunikaci mezi řadičem a vrstvy úložiště.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="5cb2d-128">Vrstva služby obsahuje logiku.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-128">The service layer contains business logic.</span></span> <span data-ttu-id="5cb2d-129">Konkrétně obsahuje logiku ověřování.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="5cb2d-130">Například vrstva služby produkt ve verzi 3 výpis obsahuje metodu CreateProduct().</span><span class="sxs-lookup"><span data-stu-id="5cb2d-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="5cb2d-131">Metoda CreateProduct() volá ValidateProduct() metodu pro ověření nového produktu před předáním produktu do úložiště produktu.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="5cb2d-132">**Výpis 3 - Models\ProductService.vb**</span><span class="sxs-lookup"><span data-stu-id="5cb2d-132">**Listing 3 - Models\ProductService.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

<span data-ttu-id="5cb2d-133">Kontroler produktu se aktualizoval v informacích 4 nahrazujícím vrstvu služby vrstvě úložiště.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="5cb2d-134">Vrstva kontroleru hovoří se vrstva služby.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="5cb2d-135">Přednášky vrstvu služby vrstvě úložiště.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="5cb2d-136">Každá vrstva má samostatné odpovědnost.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="5cb2d-137">**Část 4 – Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="5cb2d-137">**Listing 4 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

<span data-ttu-id="5cb2d-138">Všimněte si, že služby se vytvoří v konstruktoru kontroler produktu.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="5cb2d-139">Po vytvoření služby se předá slovníku stavů modelu ve službě.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="5cb2d-140">Služba produktu pomocí modelu stavu projít ověřením chybové zprávy zpět do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="5cb2d-141">Oddělení vrstva služby</span><span class="sxs-lookup"><span data-stu-id="5cb2d-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="5cb2d-142">Můžeme se nepodařilo izolovat kontroleru a vrstvy služby v jednom ohledu.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="5cb2d-143">Kontroler a vrstvy služby komunikují prostřednictvím ze stavů modelu.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="5cb2d-144">Jinými slovy vrstva služby obsahuje závislost na konkrétní funkce architektury ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="5cb2d-145">Chcete izolovat vrstva služby z našich řadič vrstvy co největší míře.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="5cb2d-146">Teoreticky jsme měl vrstvy služby pomocí libovolného typu aplikace a ne jenom aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="5cb2d-147">Například v budoucnu může chceme vytvořit front-endu pro naši aplikaci WPF.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="5cb2d-148">Jsme měli hledal způsob, jak odebrat závislost na rozhraní ASP.NET MVC stavu modelu z našich vrstvy služby.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="5cb2d-149">Výpis 5 vrstva služby byla aktualizována tak, aby již nepoužíval ze stavů modelu.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="5cb2d-150">Místo toho používá jakoukoli třídu, která implementuje rozhraní IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="5cb2d-151">**Výpis 5 - Models\ProductService.vb (oddělené)**</span><span class="sxs-lookup"><span data-stu-id="5cb2d-151">**Listing 5 - Models\ProductService.vb (decoupled)**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

<span data-ttu-id="5cb2d-152">IValidationDictionary rozhraní je definováno v informacích 6.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="5cb2d-153">Toto jednoduché rozhraní má jedné metody a vlastnosti jediné.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="5cb2d-154">**Výpis 6 - Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="5cb2d-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

<span data-ttu-id="5cb2d-155">Výpis 7, s názvem třídy ModelStateWrapper implementuje rozhraní IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="5cb2d-156">Můžete vytvořit instanci třídy ModelStateWrapper předáním slovníku stavů modelu do konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="5cb2d-157">**Výpis 7 - Models\ModelStateWrapper.vb**</span><span class="sxs-lookup"><span data-stu-id="5cb2d-157">**Listing 7 - Models\ModelStateWrapper.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

<span data-ttu-id="5cb2d-158">Nakonec aktualizované řadič v informacích 8 používá ModelStateWrapper při vytvoření vrstvy služby 've svém konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="5cb2d-159">**Výpis 8 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="5cb2d-159">**Listing 8 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

<span data-ttu-id="5cb2d-160">Použití IValidationDictionary rozhraní a třídy ModelStateWrapper nám umožňuje zcela izolovat naše vrstva služby z našich vrstvy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="5cb2d-161">Vrstva služby již není závislý na stavu modelu.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="5cb2d-162">Můžete předat jakoukoli třídu, která implementuje rozhraní IValidationDictionary pro vrstvu služby.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="5cb2d-163">Aplikace WPF může například implementovat rozhraní IValidationDictionary s třída jednoduchých kolekcí.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="5cb2d-164">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5cb2d-164">Summary</span></span>

<span data-ttu-id="5cb2d-165">Cílem tohoto kurzu byla fattica jedním z přístupů k provedení ověření v aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="5cb2d-166">V tomto kurzu jste zjistili, jak chcete přesunout všechny logiky ověřování mimo řadičů a do samostatné služby vrstvy.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="5cb2d-167">Také jste zjistili, jak izolovat vrstvě služby mezi řadiče vytvořením ModelStateWrapper třídy.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5cb2d-168">[Předchozí](validating-with-the-idataerrorinfo-interface-vb.md)
> [další](validation-with-the-data-annotation-validators-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5cb2d-168">[Previous](validating-with-the-idataerrorinfo-interface-vb.md)
[Next](validation-with-the-data-annotation-validators-vb.md)</span></span>
