---
title: "Vkládání závislostí do zobrazení"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/dependency-injection
ms.openlocfilehash: e5703bd5b3b2340b4ef170a81550237ffe2c3e3d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="dependency-injection-into-views"></a><span data-ttu-id="a3b8f-102">Vkládání závislostí do zobrazení</span><span class="sxs-lookup"><span data-stu-id="a3b8f-102">Dependency injection into views</span></span>

<span data-ttu-id="a3b8f-103">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a3b8f-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a3b8f-104">Jádro ASP.NET podporuje [vkládání závislostí](xref:fundamentals/dependency-injection) do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-104">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="a3b8f-105">To může být užitečné pro zobrazení konkrétní služby, jako je lokalizace nebo data, vyžaduje se jenom pro naplnění zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-105">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="a3b8f-106">Snažte se zachovat [oddělené oblasti zájmu](http://deviq.com/separation-of-concerns/) mezi kontrolery a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-106">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="a3b8f-107">Většinu dat, která se zobrazí zobrazení by měl být předána z řadiče.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-107">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="a3b8f-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a3b8f-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="a3b8f-109">Jednoduchý příklad</span><span class="sxs-lookup"><span data-stu-id="a3b8f-109">A Simple Example</span></span>

<span data-ttu-id="a3b8f-110">Službu můžete vložit do zobrazení pomocí `@inject` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-110">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="a3b8f-111">Si můžete představit `@inject` jako přidání vlastnosti do zobrazení a naplnění danou vlastnost pomocí DI.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-111">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="a3b8f-112">Syntaxe `@inject`: `@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="a3b8f-112">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="a3b8f-113">Příklad `@inject` v akci:</span><span class="sxs-lookup"><span data-stu-id="a3b8f-113">An example of `@inject` in action:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="a3b8f-114">Toto zobrazení ukazuje seznam `ToDoItem` instance, společně s souhrn zobrazující celkové statistiky.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-114">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="a3b8f-115">Souhrn se naplní ze vloženého `StatisticsService`.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-115">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="a3b8f-116">Tato služba je zaregistrovaný pro vkládání závislostí v `ConfigureServices` v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a3b8f-116">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="a3b8f-117">`StatisticsService` Provede pár výpočtů na sadu `ToDoItem` instance, které se přistupuje prostřednictvím úložiště:</span><span class="sxs-lookup"><span data-stu-id="a3b8f-117">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

<span data-ttu-id="a3b8f-118">Ukázka úložiště používá kolekci v paměti.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-118">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="a3b8f-119">Implementace výše uvedeném (který funguje na všechna data v paměti) se nedoporučuje pro velké, vzdáleně používaná datové sady.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-119">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="a3b8f-120">Ukázka zobrazuje data z model vázán k zobrazení a službu vloženy do zobrazení:</span><span class="sxs-lookup"><span data-stu-id="a3b8f-120">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Chcete-li zobrazit výpis celkový počet položek, Dokončit položky, průměrná priority a seznam úloh s jejich úroveň priority a logické hodnoty, která určuje dokončení.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="a3b8f-122">Naplnění vyhledávání dat</span><span class="sxs-lookup"><span data-stu-id="a3b8f-122">Populating Lookup Data</span></span>

<span data-ttu-id="a3b8f-123">Vkládání zobrazení může být užitečné k naplnění možnosti v prvky uživatelského rozhraní, jako je například rozevíracích seznamů.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-123">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="a3b8f-124">Vezměte v úvahu formuláře profilu uživatele, který zahrnuje možnosti pro zadání pohlaví, stav a další předvolby.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-124">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="a3b8f-125">Vykreslování formuláře, pomocí standardní přístup MVC by vyžadovaly kontroleru k žádosti o služby přístup dat pro každou z těchto sad možností a poté načíst model nebo `ViewBag` s každou sadu možností svázat.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-125">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="a3b8f-126">Alternativní způsob vloží přímo do zobrazení získat možnosti služby.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-126">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="a3b8f-127">To minimalizuje množství vyžaduje adaptérem přesun tato zobrazení element konstrukce logiku do samotného zobrazení kód.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-127">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="a3b8f-128">Akce kontroleru zobrazit formulář Úpravy profilu stačí předat formuláře instance profil:</span><span class="sxs-lookup"><span data-stu-id="a3b8f-128">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="a3b8f-129">Formuláře HTML, který používá k aktualizaci těchto předvolby zahrnuje rozevíracích seznamů pro tři vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="a3b8f-129">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Aktualizujte profil zobrazení s formuláři povolení položka názvu, pohlaví, stavu a Oblíbené barev.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="a3b8f-131">Tyto seznamy jsou naplněny službu, která je prováděno do zobrazení:</span><span class="sxs-lookup"><span data-stu-id="a3b8f-131">These lists are populated by a service that has been injected into the view:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="a3b8f-132">`ProfileOptionsService` Je úrovni uživatelského rozhraní služba navržená k poskytování právě data potřebná pro tento formát:</span><span class="sxs-lookup"><span data-stu-id="a3b8f-132">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> <span data-ttu-id="a3b8f-133">Nezapomeňte si zaregistrovat typy bude požadovat pomocí vkládání závislostí v `ConfigureServices` metoda v *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-133">Don't forget to register types you will request through dependency injection in the  `ConfigureServices` method in *Startup.cs*.</span></span>

## <a name="overriding-services"></a><span data-ttu-id="a3b8f-134">Přepsání služby</span><span class="sxs-lookup"><span data-stu-id="a3b8f-134">Overriding Services</span></span>

<span data-ttu-id="a3b8f-135">Kromě vložení nové služby, můžete tento postup použít také k přepsání dříve vloženého services na stránce.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-135">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="a3b8f-136">Následující obrázek znázorňuje všechna pole, které jsou k dispozici na stránce použít v prvním příkladu:</span><span class="sxs-lookup"><span data-stu-id="a3b8f-136">The figure below shows all of the fields available on the page used in the first example:</span></span>

![IntelliSense kontextové nabídky na zadaný seznam Html, součást, StatsService a adresu Url pole symbol @](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="a3b8f-138">Jak vidíte, pole výchozí obsahovat `Html`, `Component`, a `Url` (a taky `StatsService` , jsme vložit).</span><span class="sxs-lookup"><span data-stu-id="a3b8f-138">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="a3b8f-139">Pokud například chcete nahradit vlastní pomocné rutiny HTML výchozí, by mohl snadno provedete použitím `@inject`:</span><span class="sxs-lookup"><span data-stu-id="a3b8f-139">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="a3b8f-140">Pokud chcete rozšířit stávající služby, můžete použít tento postup jednoduše při dědění z nebo zabalení stávající implementaci vlastními.</span><span class="sxs-lookup"><span data-stu-id="a3b8f-140">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="a3b8f-141">Viz také</span><span class="sxs-lookup"><span data-stu-id="a3b8f-141">See Also</span></span>

* <span data-ttu-id="a3b8f-142">Simon Timms Blog: [získávání vyhledávání dat do zobrazení](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="a3b8f-142">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
