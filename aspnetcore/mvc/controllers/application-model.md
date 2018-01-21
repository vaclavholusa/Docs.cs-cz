---
title: "Práce s modelem aplikace"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: c69dd1cfae713036ce0ee95f70acc162b1e82cb0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="working-with-the-application-model"></a><span data-ttu-id="69ffd-102">Práce s modelem aplikace</span><span class="sxs-lookup"><span data-stu-id="69ffd-102">Working with the Application Model</span></span>

<span data-ttu-id="69ffd-103">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="69ffd-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="69ffd-104">Definuje rozhraní ASP.NET MVC jádra *aplikačního modelu* představující součásti aplikace MVC.</span><span class="sxs-lookup"><span data-stu-id="69ffd-104">ASP.NET Core MVC defines an *application model* representing the components of an MVC app.</span></span> <span data-ttu-id="69ffd-105">Můžete číst a manipulaci s Tento model změnit chování prvky MVC.</span><span class="sxs-lookup"><span data-stu-id="69ffd-105">You can read and manipulate this model to modify how MVC elements behave.</span></span> <span data-ttu-id="69ffd-106">Ve výchozím nastavení MVC dodržovat určité konvence určit, které třídy jsou považovány za řadiče, které metody na tyto třídy jsou akce a chování parametry a směrování.</span><span class="sxs-lookup"><span data-stu-id="69ffd-106">By default, MVC follows certain conventions to determine which classes are considered to be controllers, which methods on those classes are actions, and how parameters and routing behave.</span></span> <span data-ttu-id="69ffd-107">Toto chování, aby odpovídaly potřebám vaší aplikace pomocí vytvoření vlastní konvence a jejich použití globálně, nebo jako atributy můžete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="69ffd-107">You can customize this behavior to suit your app's needs by creating your own conventions and applying them globally or as attributes.</span></span>

## <a name="models-and-providers"></a><span data-ttu-id="69ffd-108">Modely a zprostředkovatelů</span><span class="sxs-lookup"><span data-stu-id="69ffd-108">Models and Providers</span></span>

<span data-ttu-id="69ffd-109">Model aplikace ASP.NET MVC základní zahrnují abstraktní rozhraní a třídy konkrétní implementace, které popisují aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="69ffd-109">The ASP.NET Core MVC application model include both abstract interfaces and concrete implementation classes that describe an MVC application.</span></span> <span data-ttu-id="69ffd-110">Tento model je výsledkem MVC zjišťování řadičů aplikace, akce, parametrů akcí, trasy a filtry podle výchozích konvencí.</span><span class="sxs-lookup"><span data-stu-id="69ffd-110">This model is the result of MVC discovering the app's controllers, actions, action parameters, routes, and filters according to default conventions.</span></span> <span data-ttu-id="69ffd-111">Ve spolupráci s modelem aplikace, můžete upravit podle různých pravidel výchozí chování MVC v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="69ffd-111">By working with the application model, you can modify your app to follow different conventions from the default MVC behavior.</span></span> <span data-ttu-id="69ffd-112">Parametry, názvy, trasy a filtry se používají jako konfigurační data pro akce a kontrolery.</span><span class="sxs-lookup"><span data-stu-id="69ffd-112">The parameters, names, routes, and filters are all used as configuration data for actions and controllers.</span></span>

<span data-ttu-id="69ffd-113">Model ASP.NET MVC základní aplikace má následující strukturu:</span><span class="sxs-lookup"><span data-stu-id="69ffd-113">The ASP.NET Core MVC Application Model has the following structure:</span></span>

* <span data-ttu-id="69ffd-114">ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="69ffd-114">ApplicationModel</span></span>
    * <span data-ttu-id="69ffd-115">Řadiče (ControllerModel)</span><span class="sxs-lookup"><span data-stu-id="69ffd-115">Controllers (ControllerModel)</span></span>
        * <span data-ttu-id="69ffd-116">Akce (ActionModel)</span><span class="sxs-lookup"><span data-stu-id="69ffd-116">Actions (ActionModel)</span></span>
            * <span data-ttu-id="69ffd-117">Parametry (ParameterModel)</span><span class="sxs-lookup"><span data-stu-id="69ffd-117">Parameters (ParameterModel)</span></span>

<span data-ttu-id="69ffd-118">Každou úroveň modelu má přístup k společného `Properties` umožňuje přístup a přepsat vyšší úrovně v hierarchii, která nastavuje hodnoty vlastností kolekce a nižších úrovních.</span><span class="sxs-lookup"><span data-stu-id="69ffd-118">Each level of the model has access to a common `Properties` collection, and lower levels can access and overwrite property values set by higher levels in the hierarchy.</span></span> <span data-ttu-id="69ffd-119">Vlastnosti jsou nastavené jako trvalé k `ActionDescriptor.Properties` vytvoření akce.</span><span class="sxs-lookup"><span data-stu-id="69ffd-119">The properties are persisted to the `ActionDescriptor.Properties` when the actions are created.</span></span> <span data-ttu-id="69ffd-120">Pak pokud se žádost, všechny vlastnosti konvence přidat ani upravit lze přistupovat prostřednictvím `ActionContext.ActionDescriptor.Properties`.</span><span class="sxs-lookup"><span data-stu-id="69ffd-120">Then when a request is being handled, any properties a convention added or modified can be accessed through `ActionContext.ActionDescriptor.Properties`.</span></span> <span data-ttu-id="69ffd-121">Pomocí vlastnosti je skvělým způsobem, jak konfigurovat filtry, vazače modelů, atd. pro jednotlivé akce.</span><span class="sxs-lookup"><span data-stu-id="69ffd-121">Using properties is a great way to configure your filters, model binders, etc. on a per-action basis.</span></span>

> [!NOTE]
> <span data-ttu-id="69ffd-122">`ActionDescriptor.Properties` Kolekce není vláken (pro zápisy) po dokončení spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="69ffd-122">The `ActionDescriptor.Properties` collection is not thread safe (for writes) once app startup has finished.</span></span> <span data-ttu-id="69ffd-123">Konvence jsou nejlepší způsob, jak bezpečně přidat data do této kolekce.</span><span class="sxs-lookup"><span data-stu-id="69ffd-123">Conventions are the best way to safely add data to this collection.</span></span>

### <a name="iapplicationmodelprovider"></a><span data-ttu-id="69ffd-124">IApplicationModelProvider</span><span class="sxs-lookup"><span data-stu-id="69ffd-124">IApplicationModelProvider</span></span>

<span data-ttu-id="69ffd-125">Jádro ASP.NET MVC načte model aplikace pomocí zprostředkovatele vzoru, definovaného [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="69ffd-125">ASP.NET Core MVC loads the application model using a provider pattern, defined by the [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interface.</span></span> <span data-ttu-id="69ffd-126">Tato část popisuje některé z interní implementace podrobnosti o tom, tato funkce zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="69ffd-126">This section covers some of the internal implementation details of how this provider functions.</span></span> <span data-ttu-id="69ffd-127">To je rozšířená – většinu aplikací, které využívají model aplikace by to udělat ve spolupráci s konvence.</span><span class="sxs-lookup"><span data-stu-id="69ffd-127">This is an advanced topic - most apps that leverage the application model should do so by working with conventions.</span></span>

<span data-ttu-id="69ffd-128">Implementace `IApplicationModelProvider` rozhraní "wrap" jiné, s každou implementaci volání `OnProvidersExecuting` ve vzestupném pořadí podle jeho `Order` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="69ffd-128">Implementations of the `IApplicationModelProvider` interface "wrap" one another, with each implementation calling `OnProvidersExecuting` in ascending order based on its `Order` property.</span></span> <span data-ttu-id="69ffd-129">`OnProvidersExecuted` Metoda je volána poté v obráceném pořadí.</span><span class="sxs-lookup"><span data-stu-id="69ffd-129">The `OnProvidersExecuted` method is then called in reverse order.</span></span> <span data-ttu-id="69ffd-130">Rozhraní framework definuje několik poskytovatelů:</span><span class="sxs-lookup"><span data-stu-id="69ffd-130">The framework defines several providers:</span></span>

<span data-ttu-id="69ffd-131">První (`Order=-1000`):</span><span class="sxs-lookup"><span data-stu-id="69ffd-131">First (`Order=-1000`):</span></span>

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

<span data-ttu-id="69ffd-132">Potom (`Order=-990`):</span><span class="sxs-lookup"><span data-stu-id="69ffd-132">Then (`Order=-990`):</span></span>

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> <span data-ttu-id="69ffd-133">Pořadí, ve které dva poskytovatelé se stejnou hodnotou pro `Order` se nazývají není definován a proto se spolehnout.</span><span class="sxs-lookup"><span data-stu-id="69ffd-133">The order in which two providers with the same value for `Order` are called is undefined, and therefore should not be relied upon.</span></span>

> [!NOTE]
> <span data-ttu-id="69ffd-134">`IApplicationModelProvider`je rozšířené koncept pro nástroj framework autorům rozšíření.</span><span class="sxs-lookup"><span data-stu-id="69ffd-134">`IApplicationModelProvider` is an advanced concept for framework authors to extend.</span></span> <span data-ttu-id="69ffd-135">Obecně platí aplikace by měl použít konvence a rozhraní by měl používat zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="69ffd-135">In general, apps should use conventions and frameworks should use providers.</span></span> <span data-ttu-id="69ffd-136">Klíče rozdíl je, že zprostředkovatelé vždy před konvence.</span><span class="sxs-lookup"><span data-stu-id="69ffd-136">The key distinction is that providers always run before conventions.</span></span>

<span data-ttu-id="69ffd-137">`DefaultApplicationModelProvider` Vytváří řadu výchozí chování používá ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="69ffd-137">The `DefaultApplicationModelProvider` establishes many of the default behaviors used by ASP.NET Core MVC.</span></span> <span data-ttu-id="69ffd-138">Jeho zodpovědnosti patří:</span><span class="sxs-lookup"><span data-stu-id="69ffd-138">Its responsibilities include:</span></span>

* <span data-ttu-id="69ffd-139">Přidávání globálních filtrů pro daný kontext</span><span class="sxs-lookup"><span data-stu-id="69ffd-139">Adding global filters to the context</span></span>
* <span data-ttu-id="69ffd-140">Přidání řadiče do kontextu</span><span class="sxs-lookup"><span data-stu-id="69ffd-140">Adding controllers to the context</span></span>
* <span data-ttu-id="69ffd-141">Přidání metody veřejné kontroleru jako akce</span><span class="sxs-lookup"><span data-stu-id="69ffd-141">Adding public controller methods as actions</span></span>
* <span data-ttu-id="69ffd-142">Přidání parametrů metody akce v kontextu</span><span class="sxs-lookup"><span data-stu-id="69ffd-142">Adding action method parameters to the context</span></span>
* <span data-ttu-id="69ffd-143">Použití trasy a další atributy</span><span class="sxs-lookup"><span data-stu-id="69ffd-143">Applying route and other attributes</span></span>

<span data-ttu-id="69ffd-144">Některé integrované chování jsou implementované `DefaultApplicationModelProvider`.</span><span class="sxs-lookup"><span data-stu-id="69ffd-144">Some built-in behaviors are implemented by the `DefaultApplicationModelProvider`.</span></span> <span data-ttu-id="69ffd-145">Tento zprostředkovatel je zodpovědný za vytváření [ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), která zase odkazuje [ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), a [ `ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) instance.</span><span class="sxs-lookup"><span data-stu-id="69ffd-145">This provider is responsible for constructing the [`ControllerModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), which in turn references [`ActionModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [`PropertyModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), and [`ParameterModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) instances.</span></span> <span data-ttu-id="69ffd-146">`DefaultApplicationModelProvider` Třída je podrobnosti implementace interní framework, která může a bude v budoucnu změnit.</span><span class="sxs-lookup"><span data-stu-id="69ffd-146">The `DefaultApplicationModelProvider` class is an internal framework implementation detail that can and will change in the future.</span></span> 

<span data-ttu-id="69ffd-147">`AuthorizationApplicationModelProvider` Zodpovídá za použití chování přidružené `AuthorizeFilter` a `AllowAnonymousFilter` atributy.</span><span class="sxs-lookup"><span data-stu-id="69ffd-147">The `AuthorizationApplicationModelProvider` is responsible for applying the behavior associated with the `AuthorizeFilter` and `AllowAnonymousFilter` attributes.</span></span> <span data-ttu-id="69ffd-148">[Další informace o těchto atributů](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="69ffd-148">[Learn more about these attributes](xref:security/authorization/simple).</span></span>

<span data-ttu-id="69ffd-149">`CorsApplicationModelProvider` Implementuje chování přidružené `IEnableCorsAttribute` a `IDisableCorsAttribute`a `DisableCorsAuthorizationFilter`.</span><span class="sxs-lookup"><span data-stu-id="69ffd-149">The `CorsApplicationModelProvider` implements behavior associated with the `IEnableCorsAttribute` and `IDisableCorsAttribute`, and the `DisableCorsAuthorizationFilter`.</span></span> <span data-ttu-id="69ffd-150">[Další informace o CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="69ffd-150">[Learn more about CORS](xref:security/cors).</span></span>

## <a name="conventions"></a><span data-ttu-id="69ffd-151">Konvence</span><span class="sxs-lookup"><span data-stu-id="69ffd-151">Conventions</span></span>

<span data-ttu-id="69ffd-152">Aplikační model definuje abstrakce konvence, které poskytují jednodušší způsob, jak přizpůsobit chování modelů než přepsání celý model nebo zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="69ffd-152">The application model defines convention abstractions that provide a simpler way to customize the behavior of the models than overriding the entire model or provider.</span></span> <span data-ttu-id="69ffd-153">Tato abstrakce jsou doporučeným způsobem, jak upravit chování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="69ffd-153">These abstractions are the recommended way to modify your app's behavior.</span></span> <span data-ttu-id="69ffd-154">Konvence poskytují způsob, jak napsat kód, který bude dynamicky použít vlastní nastavení.</span><span class="sxs-lookup"><span data-stu-id="69ffd-154">Conventions provide a way for you to write code that will dynamically apply customizations.</span></span> <span data-ttu-id="69ffd-155">Při [filtry](xref:mvc/controllers/filters) úprava chování rozhraní framework pro zajištění, přizpůsobení umožňují řídit, jak společně drátové celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="69ffd-155">While [filters](xref:mvc/controllers/filters) provide a means of modifying the framework's behavior, customizations let you control how the whole app is wired together.</span></span>

<span data-ttu-id="69ffd-156">K dispozici jsou následující konvence:</span><span class="sxs-lookup"><span data-stu-id="69ffd-156">The following conventions are available:</span></span>

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

<span data-ttu-id="69ffd-157">Konvence použijí přidáním možnosti MVC nebo implementací `Attribute`s a jejich použití k řadiče, akcí nebo parametrů akcí (podobně jako [ `Filters` ](xref:mvc/controllers/filters)).</span><span class="sxs-lookup"><span data-stu-id="69ffd-157">Conventions are applied by adding them to MVC options or by implementing `Attribute`s and applying them to controllers, actions, or action parameters (similar to [`Filters`](xref:mvc/controllers/filters)).</span></span> <span data-ttu-id="69ffd-158">Na rozdíl od filtrů jsou konvence spustit pouze při spouštění aplikace, nikoli jako součást každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="69ffd-158">Unlike filters, conventions are only executed when the app is starting, not as part of each request.</span></span>

### <a name="sample-modifying-the-applicationmodel"></a><span data-ttu-id="69ffd-159">Ukázka: Úprava ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="69ffd-159">Sample: Modifying the ApplicationModel</span></span>

<span data-ttu-id="69ffd-160">Následující konvence se používá k přidání vlastnosti do aplikačního modelu.</span><span class="sxs-lookup"><span data-stu-id="69ffd-160">The following convention is used to add a property to the application model.</span></span> 

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

<span data-ttu-id="69ffd-161">Konvence modelu aplikace se použijí jako možnosti, když MVC je přidaný do `ConfigureServices` v `Startup`.</span><span class="sxs-lookup"><span data-stu-id="69ffd-161">Application model conventions are applied as options when MVC is added in `ConfigureServices` in `Startup`.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

<span data-ttu-id="69ffd-162">Vlastnosti jsou k dispozici `ActionDescriptor` vlastnosti kolekce v rámci akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="69ffd-162">Properties are accessible from the `ActionDescriptor` properties collection within controller actions:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a><span data-ttu-id="69ffd-163">Ukázka: Úprava ControllerModel popis</span><span class="sxs-lookup"><span data-stu-id="69ffd-163">Sample: Modifying the ControllerModel Description</span></span>

<span data-ttu-id="69ffd-164">Jako v předchozím příkladu řadiče modelu můžete také upravit, aby obsahovat vlastní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="69ffd-164">As in the previous example, the controller model can also be modified to include custom properties.</span></span> <span data-ttu-id="69ffd-165">Tyto přepíše existující vlastnosti se stejným názvem zadané v modelu aplikace.</span><span class="sxs-lookup"><span data-stu-id="69ffd-165">These will override existing properties with the same name specified in the application model.</span></span> <span data-ttu-id="69ffd-166">Následující konvence atribut přidá popis na úrovni kontroleru:</span><span class="sxs-lookup"><span data-stu-id="69ffd-166">The following convention attribute adds a description at the controller level:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

<span data-ttu-id="69ffd-167">Touto konvencí se použije jako atribut na řadiči.</span><span class="sxs-lookup"><span data-stu-id="69ffd-167">This convention is applied as an attribute on a controller.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

<span data-ttu-id="69ffd-168">Vlastnost "Popis" přistupuje stejným způsobem jako v předchozích příkladech.</span><span class="sxs-lookup"><span data-stu-id="69ffd-168">The "description" property is accessed in the same manner as in previous examples.</span></span>

### <a name="sample-modifying-the-actionmodel-description"></a><span data-ttu-id="69ffd-169">Ukázka: Úprava ActionModel popis</span><span class="sxs-lookup"><span data-stu-id="69ffd-169">Sample: Modifying the ActionModel Description</span></span>

<span data-ttu-id="69ffd-170">Konvence samostatné atribut lze použít k jednotlivým akcím, přepsání nastavení na úrovni aplikace nebo řadič se už používá.</span><span class="sxs-lookup"><span data-stu-id="69ffd-170">A separate attribute convention can be applied to individual actions, overriding behavior already applied at the application or controller level.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

<span data-ttu-id="69ffd-171">Použití tato akce v kontroleru předchozí příklad ukazuje, jak přepíše úrovni kontroleru konvence:</span><span class="sxs-lookup"><span data-stu-id="69ffd-171">Applying this to an action within the previous example's controller demonstrates how it overrides the controller-level convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a><span data-ttu-id="69ffd-172">Ukázka: Úprava ParameterModel</span><span class="sxs-lookup"><span data-stu-id="69ffd-172">Sample: Modifying the ParameterModel</span></span>

<span data-ttu-id="69ffd-173">Následující konvence lze použít pro parametry akce k úpravě jejich `BindingInfo`.</span><span class="sxs-lookup"><span data-stu-id="69ffd-173">The following convention can be applied to action parameters to modify their `BindingInfo`.</span></span> <span data-ttu-id="69ffd-174">Následující konvenci vyžaduje, aby parametr parametr trasy; Další potenciální vazby zdroje (např. hodnoty řetězce dotazu) se ignorují.</span><span class="sxs-lookup"><span data-stu-id="69ffd-174">The following convention requires that the parameter be a route parameter; other potential binding sources (such as query string values) are ignored.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

<span data-ttu-id="69ffd-175">Atribut může použít jakékoli parametr akce:</span><span class="sxs-lookup"><span data-stu-id="69ffd-175">The attribute may be applied to any action parameter:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a><span data-ttu-id="69ffd-176">Ukázka: Úprava názvu ActionModel</span><span class="sxs-lookup"><span data-stu-id="69ffd-176">Sample: Modifying the ActionModel Name</span></span>

<span data-ttu-id="69ffd-177">Upraví následující konvence `ActionModel` aktualizovat *název* akce, které je použito.</span><span class="sxs-lookup"><span data-stu-id="69ffd-177">The following convention modifies the `ActionModel` to update the *name* of the action to which it is applied.</span></span> <span data-ttu-id="69ffd-178">Nový název je zadat jako parametr do atribut.</span><span class="sxs-lookup"><span data-stu-id="69ffd-178">The new name is provided as a parameter to the attribute.</span></span> <span data-ttu-id="69ffd-179">Tento nový název se používá ve směrování, takže ovlivní trasy použité k dosažení této metodě akce.</span><span class="sxs-lookup"><span data-stu-id="69ffd-179">This new name is used by routing, so it will affect the route used to reach this action method.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

<span data-ttu-id="69ffd-180">Tento atribut se používá pro metodu akce v `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="69ffd-180">This attribute is applied to an action method in the `HomeController`:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

<span data-ttu-id="69ffd-181">I když je název metody `SomeName`, atribut přepsání konvence MVC pomocí názvu metody a nahradí název akce s `MyCoolAction`.</span><span class="sxs-lookup"><span data-stu-id="69ffd-181">Even though the method name is `SomeName`, the attribute overrides the MVC convention of using the method name and replaces the action name with `MyCoolAction`.</span></span> <span data-ttu-id="69ffd-182">Proto trasy použité k dosažení této akce je `/Home/MyCoolAction`.</span><span class="sxs-lookup"><span data-stu-id="69ffd-182">Thus, the route used to reach this action is `/Home/MyCoolAction`.</span></span>

> [!NOTE]
> <span data-ttu-id="69ffd-183">V tomto příkladu je v podstatě stejný jako pomocí integrovaných [název akce](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="69ffd-183">This example is essentially the same as using the built-in [ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) attribute.</span></span>

### <a name="sample-custom-routing-convention"></a><span data-ttu-id="69ffd-184">Ukázka: Vlastní směrování konvence</span><span class="sxs-lookup"><span data-stu-id="69ffd-184">Sample: Custom Routing Convention</span></span>

<span data-ttu-id="69ffd-185">Můžete použít `IApplicationModelConvention` přizpůsobit funguje jak směrování.</span><span class="sxs-lookup"><span data-stu-id="69ffd-185">You can use an `IApplicationModelConvention` to customize how routing works.</span></span> <span data-ttu-id="69ffd-186">Například následující konvence bude začlenit řadiče obory názvů do jejich trasy, nahraďte `.` v oboru názvů s `/` v trasy:</span><span class="sxs-lookup"><span data-stu-id="69ffd-186">For example, the following convention will incorporate Controllers' namespaces into their routes, replacing `.` in the namespace with `/` in the route:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

<span data-ttu-id="69ffd-187">Konvence je přidán jako možnost v spuštění.</span><span class="sxs-lookup"><span data-stu-id="69ffd-187">The convention is added as an option in Startup.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> <span data-ttu-id="69ffd-188">Můžete přidat konvence pro vaše [middleware](xref:fundamentals/middleware) díky přístupu k `MvcOptions` pomocí`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span><span class="sxs-lookup"><span data-stu-id="69ffd-188">You can add conventions to your [middleware](xref:fundamentals/middleware) by accessing `MvcOptions` using `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span></span>

<span data-ttu-id="69ffd-189">Tato ukázka platí tato konvence pro tras, které nepoužívají atribut směrování, pokud je řadič má "Namespace" v názvu.</span><span class="sxs-lookup"><span data-stu-id="69ffd-189">This sample applies this convention to routes that are not using attribute routing where the controller has  "Namespace" in its name.</span></span> <span data-ttu-id="69ffd-190">Následující řadiče ukazuje touto konvencí:</span><span class="sxs-lookup"><span data-stu-id="69ffd-190">The following controller demonstrates this convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a><span data-ttu-id="69ffd-191">Používání modelu aplikací v WebApiCompatShim</span><span class="sxs-lookup"><span data-stu-id="69ffd-191">Application Model Usage in WebApiCompatShim</span></span>

<span data-ttu-id="69ffd-192">Jádro ASP.NET MVC používá jinou sadu konvence z technologie ASP.NET Web API 2.</span><span class="sxs-lookup"><span data-stu-id="69ffd-192">ASP.NET Core MVC uses a different set of conventions from ASP.NET Web API 2.</span></span> <span data-ttu-id="69ffd-193">Pomocí vlastních názvů, můžete upravit aplikaci ASP.NET MVC základní chování, aby byla konzistentní se u aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="69ffd-193">Using custom conventions, you can modify an ASP.NET Core MVC app's behavior to be consistent with that of a Web API app.</span></span> <span data-ttu-id="69ffd-194">Microsoft se dodává [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) speciálně pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="69ffd-194">Microsoft ships the [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specifically for this purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="69ffd-195">Další informace o [migrace z rozhraní ASP.NET Web API](xref:migration/webapi).</span><span class="sxs-lookup"><span data-stu-id="69ffd-195">Learn more about [migrating from ASP.NET Web API](xref:migration/webapi).</span></span>

<span data-ttu-id="69ffd-196">Pokud chcete použít Shimu Web API kompatibility, budete muset do projektu přidejte balíček a pak přidejte se názvů do MVC voláním `AddWebApiConventions` v `Startup`:</span><span class="sxs-lookup"><span data-stu-id="69ffd-196">To use the Web API Compatibility Shim, you need to add the package to your project and then add the conventions to MVC by calling `AddWebApiConventions` in `Startup`:</span></span>

```c#
services.AddMvc().AddWebApiConventions();
```

<span data-ttu-id="69ffd-197">Konvence poskytované shimu se použije pouze na části aplikace, které předtím určité atributy použité k nim.</span><span class="sxs-lookup"><span data-stu-id="69ffd-197">The conventions provided by the shim are only applied to parts of the app that have had certain attributes applied to them.</span></span> <span data-ttu-id="69ffd-198">Následující čtyři atributy se používají řídit, která řadiče by měl mít jejich konvence upraveném shim konvence:</span><span class="sxs-lookup"><span data-stu-id="69ffd-198">The following four attributes are used to control which controllers should have their conventions modified by the shim's conventions:</span></span>

* [<span data-ttu-id="69ffd-199">UseWebApiActionConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="69ffd-199">UseWebApiActionConventionsAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [<span data-ttu-id="69ffd-200">UseWebApiOverloadingAttribute</span><span class="sxs-lookup"><span data-stu-id="69ffd-200">UseWebApiOverloadingAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [<span data-ttu-id="69ffd-201">UseWebApiParameterConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="69ffd-201">UseWebApiParameterConventionsAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [<span data-ttu-id="69ffd-202">UseWebApiRoutesAttribute</span><span class="sxs-lookup"><span data-stu-id="69ffd-202">UseWebApiRoutesAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a><span data-ttu-id="69ffd-203">Konvence akce</span><span class="sxs-lookup"><span data-stu-id="69ffd-203">Action Conventions</span></span>

<span data-ttu-id="69ffd-204">`UseWebApiActionConventionsAttribute` Se používá k mapování metoda HTTP pro akce na základě jejich názvu (například `Get` by mapovat `HttpGet`).</span><span class="sxs-lookup"><span data-stu-id="69ffd-204">The `UseWebApiActionConventionsAttribute` is used to map the HTTP method to actions based on their name (for instance, `Get` would map to `HttpGet`).</span></span> <span data-ttu-id="69ffd-205">Platí jenom pro akce, které nepoužívají směrováním atributů.</span><span class="sxs-lookup"><span data-stu-id="69ffd-205">It only applies to actions that do not use attribute routing.</span></span>

### <a name="overloading"></a><span data-ttu-id="69ffd-206">Přetížení</span><span class="sxs-lookup"><span data-stu-id="69ffd-206">Overloading</span></span>

<span data-ttu-id="69ffd-207">`UseWebApiOverloadingAttribute` Se používá k aplikování `WebApiOverloadingApplicationModelConvention` konvence.</span><span class="sxs-lookup"><span data-stu-id="69ffd-207">The `UseWebApiOverloadingAttribute` is used to apply the `WebApiOverloadingApplicationModelConvention` convention.</span></span> <span data-ttu-id="69ffd-208">Touto konvencí přidá `OverloadActionConstraint` do procesu výběr akce, což omezuje akce kandidáta na ty, pro které žádost, splňuje všechny-volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="69ffd-208">This convention adds an `OverloadActionConstraint` to the action selection process, which limits candidate actions to those for which the request satisfies all non-optional parameters.</span></span>

### <a name="parameter-conventions"></a><span data-ttu-id="69ffd-209">Parametr konvence</span><span class="sxs-lookup"><span data-stu-id="69ffd-209">Parameter Conventions</span></span>

<span data-ttu-id="69ffd-210">`UseWebApiParameterConventionsAttribute` Se používá k aplikování `WebApiParameterConventionsApplicationModelConvention` akci convention.</span><span class="sxs-lookup"><span data-stu-id="69ffd-210">The `UseWebApiParameterConventionsAttribute` is used to apply the `WebApiParameterConventionsApplicationModelConvention` action convention.</span></span> <span data-ttu-id="69ffd-211">Touto konvencí Určuje, že jednoduché typy používat jako akce parametry jsou vázány z identifikátoru URI ve výchozím nastavení, při komplexní typy jsou svázány z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="69ffd-211">This convention specifies that simple types used as action parameters are bound from the URI by default, while complex types are bound from the request body.</span></span>

### <a name="routes"></a><span data-ttu-id="69ffd-212">Trasy</span><span class="sxs-lookup"><span data-stu-id="69ffd-212">Routes</span></span>

<span data-ttu-id="69ffd-213">`UseWebApiRoutesAttribute` Ovládací prvky jestli `WebApiApplicationModelConvention` řadiče konvence platí.</span><span class="sxs-lookup"><span data-stu-id="69ffd-213">The `UseWebApiRoutesAttribute` controls whether the `WebApiApplicationModelConvention` controller convention is applied.</span></span> <span data-ttu-id="69ffd-214">Když je povolené, touto konvencí se používá k přidání podpory pro [oblasti](xref:mvc/controllers/areas) na trasu.</span><span class="sxs-lookup"><span data-stu-id="69ffd-214">When enabled, this convention is used to add support for [areas](xref:mvc/controllers/areas) to the route.</span></span>

<span data-ttu-id="69ffd-215">Kromě sadu konvence, balíček kompatibility obsahuje `System.Web.Http.ApiController` základní třídy, který nahrazuje zadanému pomocí webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="69ffd-215">In addition to a set of conventions, the compatibility package includes a `System.Web.Http.ApiController` base class that replaces the one provided by Web API.</span></span> <span data-ttu-id="69ffd-216">To umožňuje řadičů zapsán pro webového rozhraní API a dědění z jeho `ApiController` postup, jak byly navrženy, když jsou spuštěné na ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="69ffd-216">This allows your controllers written for Web API and inheriting from its `ApiController` to work as they were designed, while running on ASP.NET Core MVC.</span></span> <span data-ttu-id="69ffd-217">Tato třída základní řadič opatřen se všemi `UseWebApi*` atributy uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="69ffd-217">This base controller class is decorated with all of the `UseWebApi*` attributes listed above.</span></span> <span data-ttu-id="69ffd-218">`ApiController` Zpřístupní vlastnosti, metod a typy výsledků, které jsou kompatibilní s výstrahám nacházejícím se v rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="69ffd-218">The `ApiController` exposes properties, methods, and result types that are compatible with those found in Web API.</span></span>

## <a name="using-apiexplorer-to-document-your-app"></a><span data-ttu-id="69ffd-219">Pomocí ApiExplorer do dokumentů aplikace</span><span class="sxs-lookup"><span data-stu-id="69ffd-219">Using ApiExplorer to Document Your App</span></span>

<span data-ttu-id="69ffd-220">Zpřístupní aplikačního modelu [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) vlastnost na každé úrovni, který slouží k procházení struktury aplikace.</span><span class="sxs-lookup"><span data-stu-id="69ffd-220">The application model exposes an [`ApiExplorer`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) property at each level that can be used to traverse the app's structure.</span></span> <span data-ttu-id="69ffd-221">To může být slouží jako [generování stránky nápovědy pro vaše webové rozhraní API pomocí nástroje, například Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="69ffd-221">This can be used to [generate help pages for your Web APIs using tools like Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="69ffd-222">`ApiExplorer` Zpřístupňuje vlastnost `IsVisible` vlastnost, která můžete nastavit k určení, které části modelu vaše aplikace by měly být vystaveny.</span><span class="sxs-lookup"><span data-stu-id="69ffd-222">The `ApiExplorer` property exposes an `IsVisible` property that can be set to specify which parts of your app's model should be exposed.</span></span> <span data-ttu-id="69ffd-223">Můžete nakonfigurovat toto nastavení používá konvence:</span><span class="sxs-lookup"><span data-stu-id="69ffd-223">You can configure this setting using a convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

<span data-ttu-id="69ffd-224">Pomocí tohoto přístupu (a další konvence v případě potřeby), můžete povolit nebo zakázat rozhraní API viditelnost na všechny úrovně v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="69ffd-224">Using this approach (and additional conventions if required), you can enable or disable API visibility at any level within your app.</span></span> 
