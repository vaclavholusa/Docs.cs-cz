---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: Seznámení s filtry akcí (C#) | Microsoft Docs
author: microsoft
description: Cílem tohoto kurzu je vysvětlit, filtrů akce. Filtr akce je atribut, který můžete použít pro akce kontroleru--nebo celý řadiče...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: d68933297329370e227f524c4b96ed7e259ef833
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="understanding-action-filters-c"></a><span data-ttu-id="1ff41-104">Seznámení s filtry akcí (C#)</span><span class="sxs-lookup"><span data-stu-id="1ff41-104">Understanding Action Filters (C#)</span></span>
====================
<span data-ttu-id="1ff41-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1ff41-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="1ff41-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="1ff41-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> <span data-ttu-id="1ff41-107">Cílem tohoto kurzu je vysvětlit, filtrů akce.</span><span class="sxs-lookup"><span data-stu-id="1ff41-107">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="1ff41-108">Filtr akce je atribut, který můžete použít pro akce kontroleru--nebo celý řadiče – které mění způsob, ve kterém se spustí akce.</span><span class="sxs-lookup"><span data-stu-id="1ff41-108">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span>


## <a name="understanding-action-filters"></a><span data-ttu-id="1ff41-109">Seznámení s filtry akcí</span><span class="sxs-lookup"><span data-stu-id="1ff41-109">Understanding Action Filters</span></span>

<span data-ttu-id="1ff41-110">Cílem tohoto kurzu je vysvětlit, filtrů akce.</span><span class="sxs-lookup"><span data-stu-id="1ff41-110">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="1ff41-111">Filtr akce je atribut, který můžete použít pro akce kontroleru--nebo celý řadiče – které mění způsob, ve kterém se spustí akce.</span><span class="sxs-lookup"><span data-stu-id="1ff41-111">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span> <span data-ttu-id="1ff41-112">Rozhraní ASP.NET MVC zahrnuje několik filtrů akce:</span><span class="sxs-lookup"><span data-stu-id="1ff41-112">The ASP.NET MVC framework includes several action filters:</span></span>

- <span data-ttu-id="1ff41-113">OutputCache – tento filtr akce ukládá do mezipaměti výstup akce kontroleru pro zadanou dobu.</span><span class="sxs-lookup"><span data-stu-id="1ff41-113">OutputCache – This action filter caches the output of a controller action for a specified amount of time.</span></span>
- <span data-ttu-id="1ff41-114">HandleError – tento filtr akce zpracovává chyby vyvolá, když provádí akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1ff41-114">HandleError – This action filter handles errors raised when a controller action executes.</span></span>
- <span data-ttu-id="1ff41-115">Autorizovat – tento filtr akce umožňuje omezení přístupu ke konkrétní uživatel nebo role.</span><span class="sxs-lookup"><span data-stu-id="1ff41-115">Authorize – This action filter enables you to restrict access to a particular user or role.</span></span>

<span data-ttu-id="1ff41-116">Také můžete vytvořit vlastní akce filtry.</span><span class="sxs-lookup"><span data-stu-id="1ff41-116">You also can create your own custom action filters.</span></span> <span data-ttu-id="1ff41-117">Můžete například vytvořit vlastní akce filtru kvůli implementaci vlastního ověřování systému.</span><span class="sxs-lookup"><span data-stu-id="1ff41-117">For example, you might want to create a custom action filter in order to implement a custom authentication system.</span></span> <span data-ttu-id="1ff41-118">Nebo můžete chtít vytvořit filtr akce, která upraví zobrazení dat vrácených akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1ff41-118">Or, you might want to create an action filter that modifies the view data returned by a controller action.</span></span>

<span data-ttu-id="1ff41-119">V tomto kurzu zjistěte, jak vytvořit filtr akce od základů.</span><span class="sxs-lookup"><span data-stu-id="1ff41-119">In this tutorial, you learn how to build an action filter from the ground up.</span></span> <span data-ttu-id="1ff41-120">Vytvoříme filtr akce protokol, který protokoluje různé fáze zpracování akce do okna výstupu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ff41-120">We create a Log action filter that logs different stages of the processing of an action to the Visual Studio Output window.</span></span>

### <a name="using-an-action-filter"></a><span data-ttu-id="1ff41-121">Pomocí filtru akce</span><span class="sxs-lookup"><span data-stu-id="1ff41-121">Using an Action Filter</span></span>

<span data-ttu-id="1ff41-122">Filtr akce je atribut.</span><span class="sxs-lookup"><span data-stu-id="1ff41-122">An action filter is an attribute.</span></span> <span data-ttu-id="1ff41-123">Většina filtrů akce můžete použít pro akce jednotlivých řadiče nebo celý kontroler.</span><span class="sxs-lookup"><span data-stu-id="1ff41-123">You can apply most action filters to either an individual controller action or an entire controller.</span></span>

<span data-ttu-id="1ff41-124">Například řadič Data v výpis 1 zpřístupní akci s názvem `Index()` , který vrací aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="1ff41-124">For example, the Data controller in Listing 1 exposes an action named `Index()` that returns the current time.</span></span> <span data-ttu-id="1ff41-125">Tato akce je upraven pomocí `OutputCache` filtru akce.</span><span class="sxs-lookup"><span data-stu-id="1ff41-125">This action is decorated with the `OutputCache` action filter.</span></span> <span data-ttu-id="1ff41-126">Tento filtr způsobí, že hodnoty vrácené akce ukládat do mezipaměti 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="1ff41-126">This filter causes the value returned by the action to be cached for 10 seconds.</span></span>

<span data-ttu-id="1ff41-127">**Výpis 1 – `Controllers\DataController.cs`**</span><span class="sxs-lookup"><span data-stu-id="1ff41-127">**Listing 1 – `Controllers\DataController.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

<span data-ttu-id="1ff41-128">Pokud opakovaně vyvolání `Index()` akce zadáním adresy URL/Data/Index do adresního řádku prohlížeče a stiskne aktualizace tlačítko vícekrát, zobrazí se ve stejnou dobu 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="1ff41-128">If you repeatedly invoke the `Index()` action by entering the URL /Data/Index into the address bar of your browser and hitting the Refresh button multiple times, then you will see the same time for 10 seconds.</span></span> <span data-ttu-id="1ff41-129">Výstup `Index()` akce se uloží do mezipaměti 10 sekund (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="1ff41-129">The output of the `Index()` action is cached for 10 seconds (see Figure 1).</span></span>


<span data-ttu-id="1ff41-130">[![Čas v mezipaměti](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1ff41-130">[![Cached time](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)</span></span>

<span data-ttu-id="1ff41-131">**Obrázek 01**: v mezipaměti Doba ([Kliknutím zobrazit obrázek v plné velikosti](understanding-action-filters-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1ff41-131">**Figure 01**: Cached time ([Click to view full-size image](understanding-action-filters-cs/_static/image3.png))</span></span>


<span data-ttu-id="1ff41-132">V jedné akce filtru – výpis 1 `OutputCache` filtru akce – u `Index()` metoda.</span><span class="sxs-lookup"><span data-stu-id="1ff41-132">In Listing 1, a single action filter – the `OutputCache` action filter – is applied to the `Index()` method.</span></span> <span data-ttu-id="1ff41-133">Pokud potřebujete, můžete použít více filtrů Akce na stejné akce.</span><span class="sxs-lookup"><span data-stu-id="1ff41-133">If you need, you can apply multiple action filters to the same action.</span></span> <span data-ttu-id="1ff41-134">Například můžete chtít použít i `OutputCache` a `HandleError` filtry akcí na stejnou akci.</span><span class="sxs-lookup"><span data-stu-id="1ff41-134">For example, you might want to apply both the `OutputCache` and `HandleError` action filters to the same action.</span></span>

<span data-ttu-id="1ff41-135">Výpis 1 `OutputCache` akce filtr je použit `Index()` akce.</span><span class="sxs-lookup"><span data-stu-id="1ff41-135">In Listing 1, the `OutputCache` action filter is applied to the `Index()` action.</span></span> <span data-ttu-id="1ff41-136">Také může použít pro tento atribut `DataController` třídy sám sebe.</span><span class="sxs-lookup"><span data-stu-id="1ff41-136">You also could apply this attribute to the `DataController` class itself.</span></span> <span data-ttu-id="1ff41-137">V takovém případě výsledek vrácený žádnou akci vystavené řadičem by do mezipaměti 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="1ff41-137">In that case, the result returned by any action exposed by the controller would be cached for 10 seconds.</span></span>

### <a name="the-different-types-of-filters"></a><span data-ttu-id="1ff41-138">Různé typy filtrů</span><span class="sxs-lookup"><span data-stu-id="1ff41-138">The Different Types of Filters</span></span>

<span data-ttu-id="1ff41-139">Rozhraní ASP.NET MVC podporuje čtyři typy filtrů:</span><span class="sxs-lookup"><span data-stu-id="1ff41-139">The ASP.NET MVC framework supports four different types of filters:</span></span>

1. <span data-ttu-id="1ff41-140">Filtry autorizace – implementuje `IAuthorizationFilter` atribut.</span><span class="sxs-lookup"><span data-stu-id="1ff41-140">Authorization filters – Implements the `IAuthorizationFilter` attribute.</span></span>
2. <span data-ttu-id="1ff41-141">Filtry akce – implementuje `IActionFilter` atribut.</span><span class="sxs-lookup"><span data-stu-id="1ff41-141">Action filters – Implements the `IActionFilter` attribute.</span></span>
3. <span data-ttu-id="1ff41-142">Vést filtry – implementuje `IResultFilter` atribut.</span><span class="sxs-lookup"><span data-stu-id="1ff41-142">Result filters – Implements the `IResultFilter` attribute.</span></span>
4. <span data-ttu-id="1ff41-143">Filtry výjimek – implementuje `IExceptionFilter` atribut.</span><span class="sxs-lookup"><span data-stu-id="1ff41-143">Exception filters – Implements the `IExceptionFilter` attribute.</span></span>

<span data-ttu-id="1ff41-144">Filtry jsou spouštěny v pořadí uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="1ff41-144">Filters are executed in the order listed above.</span></span> <span data-ttu-id="1ff41-145">Například filtry autorizace se vždy spuštěny před filtry akcí a filtry výjimek jsou vždy provést po každý jiný typ filtru.</span><span class="sxs-lookup"><span data-stu-id="1ff41-145">For example, authorization filters are always executed before action filters and exception filters are always executed after every other type of filter.</span></span>

<span data-ttu-id="1ff41-146">Filtry autorizace se používají k implementace ověřování a autorizace pro akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1ff41-146">Authorization filters are used to implement authentication and authorization for controller actions.</span></span> <span data-ttu-id="1ff41-147">Například filtr Authorize je příkladem filtr autorizace.</span><span class="sxs-lookup"><span data-stu-id="1ff41-147">For example, the Authorize filter is an example of an Authorization filter.</span></span>

<span data-ttu-id="1ff41-148">Filtrů akce obsahují logiku, která se provedla před a po provedení akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1ff41-148">Action filters contain logic that is executed before and after a controller action executes.</span></span> <span data-ttu-id="1ff41-149">Filtr akce můžete použít například k úpravě zobrazení data, která vrátí akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1ff41-149">You can use an action filter, for instance, to modify the view data that a controller action returns.</span></span>

<span data-ttu-id="1ff41-150">Filtry výsledků obsahují logiku, která se provedla před a po provedení výsledku zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1ff41-150">Result filters contain logic that is executed before and after a view result is executed.</span></span> <span data-ttu-id="1ff41-151">Například můžete chtít změnit zobrazení výsledků pravým předtím, než je zobrazení vykresleno do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1ff41-151">For example, you might want to modify a view result right before the view is rendered to the browser.</span></span>

<span data-ttu-id="1ff41-152">Filtry výjimek jsou poslední typ spustit filtr.</span><span class="sxs-lookup"><span data-stu-id="1ff41-152">Exception filters are the last type of filter to run.</span></span> <span data-ttu-id="1ff41-153">Zpracování chyb, vyvolá akce kontroleru nebo výsledky akce kontroleru můžete použít filtr výjimek.</span><span class="sxs-lookup"><span data-stu-id="1ff41-153">You can use an exception filter to handle errors raised by either your controller actions or controller action results.</span></span> <span data-ttu-id="1ff41-154">Můžete také použít filtry výjimek chyb do protokolu.</span><span class="sxs-lookup"><span data-stu-id="1ff41-154">You also can use exception filters to log errors.</span></span>

<span data-ttu-id="1ff41-155">Každý jiný typ filtru je provést v určitém pořadí.</span><span class="sxs-lookup"><span data-stu-id="1ff41-155">Each different type of filter is executed in a particular order.</span></span> <span data-ttu-id="1ff41-156">Pokud chcete určit pořadí, ve kterém jsou prováděny filtry stejného typu, můžete nastavit vlastnost pořadí filtru.</span><span class="sxs-lookup"><span data-stu-id="1ff41-156">If you want to control the order in which filters of the same type are executed then you can set a filter's Order property.</span></span>

<span data-ttu-id="1ff41-157">Základní třída pro všechny filtry akce je `System.Web.Mvc.FilterAttribute` třídy.</span><span class="sxs-lookup"><span data-stu-id="1ff41-157">The base class for all action filters is the `System.Web.Mvc.FilterAttribute` class.</span></span> <span data-ttu-id="1ff41-158">Pokud chcete implementovat konkrétní typ filtru, pak je nutné vytvořit třídu, která dědí vlastnosti ze základní třídy filtru a implementuje jeden nebo více `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, nebo `IExceptionFilter` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1ff41-158">If you want to implement a particular type of filter, then you need to create a class that inherits from the base Filter class and implements one or more of the `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, or `IExceptionFilter` interfaces.</span></span>

### <a name="the-base-actionfilterattribute-class"></a><span data-ttu-id="1ff41-159">Základní třídy ActionFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="1ff41-159">The Base ActionFilterAttribute Class</span></span>

<span data-ttu-id="1ff41-160">Abyste snadněji implementovat vlastní akce filtru, rozhraní ASP.NET MVC zahrnuje na základní `ActionFilterAttribute` třídy.</span><span class="sxs-lookup"><span data-stu-id="1ff41-160">In order to make it easier for you to implement a custom action filter, the ASP.NET MVC framework includes a base `ActionFilterAttribute` class.</span></span> <span data-ttu-id="1ff41-161">Tato třída implementuje i `IActionFilter` a `IResultFilter` rozhraní a dědí z `Filter` třídy.</span><span class="sxs-lookup"><span data-stu-id="1ff41-161">This class implements both the `IActionFilter` and `IResultFilter` interfaces and inherits from the `Filter` class.</span></span>

<span data-ttu-id="1ff41-162">Terminologie zde není zcela v souladu.</span><span class="sxs-lookup"><span data-stu-id="1ff41-162">The terminology here is not entirely consistent.</span></span> <span data-ttu-id="1ff41-163">Technicky je třídu, která dědí z třídy ActionFilterAttribute filtr akce a filtr výsledků.</span><span class="sxs-lookup"><span data-stu-id="1ff41-163">Technically, a class that inherits from the ActionFilterAttribute class is both an action filter and a result filter.</span></span> <span data-ttu-id="1ff41-164">V tom smyslu přijít, však filtr akcí aplikace word se používá k odkazování na jakýkoli typ filtru v rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1ff41-164">However, in the loose sense, the word action filter is used to refer to any type of filter in the ASP.NET MVC framework.</span></span>

<span data-ttu-id="1ff41-165">Základní `ActionFilterAttribute` třída má následující metody, které můžete přepsat:</span><span class="sxs-lookup"><span data-stu-id="1ff41-165">The base `ActionFilterAttribute` class has the following methods that you can override:</span></span>

- <span data-ttu-id="1ff41-166">OnActionExecuting – tato metoda je volána před provedením akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1ff41-166">OnActionExecuting – This method is called before a controller action is executed.</span></span>
- <span data-ttu-id="1ff41-167">OnActionExecuted – tato metoda je volána po provedení akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1ff41-167">OnActionExecuted – This method is called after a controller action is executed.</span></span>
- <span data-ttu-id="1ff41-168">OnResultExecuting – tato metoda je volána před provedením výsledku akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1ff41-168">OnResultExecuting – This method is called before a controller action result is executed.</span></span>
- <span data-ttu-id="1ff41-169">OnResultExecuted – tato metoda je volána po provedení výsledku akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1ff41-169">OnResultExecuted – This method is called after a controller action result is executed.</span></span>

<span data-ttu-id="1ff41-170">V další části uvidíme, jak můžete implementovat, každá z těchto různých metod.</span><span class="sxs-lookup"><span data-stu-id="1ff41-170">In the next section, we'll see how you can implement each of these different methods.</span></span>

### <a name="creating-a-log-action-filter"></a><span data-ttu-id="1ff41-171">Vytváření akce filtru protokolu</span><span class="sxs-lookup"><span data-stu-id="1ff41-171">Creating a Log Action Filter</span></span>

<span data-ttu-id="1ff41-172">Chcete-li ilustrují, jak můžete vytvořit vlastní akce filtru, vytvoříme vlastní akce filtr, který protokoluje fází zpracování akce kontroleru do okna výstupu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ff41-172">In order to illustrate how you can build a custom action filter, we'll create a custom action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span> <span data-ttu-id="1ff41-173">Naše `LogActionFilter` je součástí výpis 2.</span><span class="sxs-lookup"><span data-stu-id="1ff41-173">Our `LogActionFilter` is contained in Listing 2.</span></span>

<span data-ttu-id="1ff41-174">**Výpis 2 – `ActionFilters\LogActionFilter.cs`**</span><span class="sxs-lookup"><span data-stu-id="1ff41-174">**Listing 2 – `ActionFilters\LogActionFilter.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

<span data-ttu-id="1ff41-175">Výpis 2 `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, a `OnResultExecuted()` všechny metody volání `Log()` metoda.</span><span class="sxs-lookup"><span data-stu-id="1ff41-175">In Listing 2, the `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, and `OnResultExecuted()` methods all call the `Log()` method.</span></span> <span data-ttu-id="1ff41-176">Název metody a aktuální data trasy, která je předána `Log()` metoda.</span><span class="sxs-lookup"><span data-stu-id="1ff41-176">The name of the method and the current route data is passed to the `Log()` method.</span></span> <span data-ttu-id="1ff41-177">`Log()` Metoda zapíše zprávu do okna výstupu Visual Studio (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="1ff41-177">The `Log()` method writes a message to the Visual Studio Output window (see Figure 2).</span></span>


<span data-ttu-id="1ff41-178">[![Zápis do okna výstupu Visual Studio](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="1ff41-178">[![Writing to the Visual Studio Output window](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)</span></span>

<span data-ttu-id="1ff41-179">**Obrázek 02**: zápis do okna výstupu Visual Studio ([Kliknutím zobrazit obrázek v plné velikosti](understanding-action-filters-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1ff41-179">**Figure 02**: Writing to the Visual Studio Output window ([Click to view full-size image](understanding-action-filters-cs/_static/image6.png))</span></span>


<span data-ttu-id="1ff41-180">Domovské řadiče v výpis 3 znázorňuje, jak můžete použít filtr akce protokolu celý ovladač třídy.</span><span class="sxs-lookup"><span data-stu-id="1ff41-180">The Home controller in Listing 3 illustrates how you can apply the Log action filter to an entire controller class.</span></span> <span data-ttu-id="1ff41-181">Vždy, když jednu z akcí, které jsou vystavené domovské řadiče jsou vyvolány – buď `Index()` metoda nebo `About()` metoda – fáze zpracování akce se zaznamenávají do okna výstupu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ff41-181">Whenever any of the actions exposed by the Home controller are invoked – either the `Index()` method or the `About()` method – the stages of processing the action are logged to the Visual Studio Output window.</span></span>

<span data-ttu-id="1ff41-182">**Výpis 3 – `Controllers\HomeController.cs`**</span><span class="sxs-lookup"><span data-stu-id="1ff41-182">**Listing 3 – `Controllers\HomeController.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a><span data-ttu-id="1ff41-183">Souhrn</span><span class="sxs-lookup"><span data-stu-id="1ff41-183">Summary</span></span>

<span data-ttu-id="1ff41-184">V tomto kurzu jste se seznámili s ASP.NET MVC filtrů akce.</span><span class="sxs-lookup"><span data-stu-id="1ff41-184">In this tutorial, you were introduced to ASP.NET MVC action filters.</span></span> <span data-ttu-id="1ff41-185">Seznámili jste se čtyři různé typy filtrů: filtry autorizace, filtry akce, filtry výsledků a filtry výjimek.</span><span class="sxs-lookup"><span data-stu-id="1ff41-185">You learned about the four different types of filters: authorization filters, action filters, result filters, and exception filters.</span></span> <span data-ttu-id="1ff41-186">Seznámili jste se také základní `ActionFilterAttribute` třídy.</span><span class="sxs-lookup"><span data-stu-id="1ff41-186">You also learned about the base `ActionFilterAttribute` class.</span></span>

<span data-ttu-id="1ff41-187">Nakonec jste zjistili, jak implementovat jednoduchého filtru akce.</span><span class="sxs-lookup"><span data-stu-id="1ff41-187">Finally, you learned how to implement a simple action filter.</span></span> <span data-ttu-id="1ff41-188">Jsme vytvořili filtr akce protokol, který protokoluje fází zpracování akce kontroleru do okna výstupu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ff41-188">We created a Log action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1ff41-189">[Předchozí](asp-net-mvc-routing-overview-cs.md)
> [další](improving-performance-with-output-caching-cs.md)</span><span class="sxs-lookup"><span data-stu-id="1ff41-189">[Previous](asp-net-mvc-routing-overview-cs.md)
[Next](improving-performance-with-output-caching-cs.md)</span></span>
