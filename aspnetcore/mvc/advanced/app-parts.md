---
title: "Částí aplikace v ASP.NET Core"
author: ardalis
description: "Další informace o použití částí aplikace, které jsou abstrations přes prostředky aplikace, ke konfiguraci vaší aplikace na zjišťování nebo předejít přetížení funkce ze sestavení."
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 702d7773374f331b25489060b18f752186d7acea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="b2302-103">Částí aplikace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2302-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="b2302-104">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b2302-104">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b2302-105">*Část aplikace* je abstrakci přes prostředky aplikace, ze kterého MVC funkce jako řadiče zobrazení součásti, nebo může být zjištěny značky pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="b2302-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="b2302-106">Příkladem je součástí aplikace je AssemblyPart, který zapouzdřuje odkaz na sestavení a zpřístupňuje typy a odkazy na kompilace.</span><span class="sxs-lookup"><span data-stu-id="b2302-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="b2302-107">*Funkce zprostředkovatelé* práce s částí aplikace k naplnění funkce aplikace ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="b2302-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="b2302-108">Nejčastěji se využívá případ částí aplikace je vám umožní nakonfigurovat aplikace zjistit (nebo vyloučit načítání) MVC funkce ze sestavení.</span><span class="sxs-lookup"><span data-stu-id="b2302-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="b2302-109">Představení částí aplikace</span><span class="sxs-lookup"><span data-stu-id="b2302-109">Introducing Application Parts</span></span>

<span data-ttu-id="b2302-110">Aplikace MVC načíst jejich funkce z [částí aplikace](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="b2302-110">MVC apps load their features from [application parts](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="b2302-111">Konkrétně [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) třída reprezentuje z části aplikace, kterou je zajištěna sestavení.</span><span class="sxs-lookup"><span data-stu-id="b2302-111">In particular, the [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="b2302-112">Tyto třídy můžete použít ke zjištění a načíst MVC funkcí, jako jsou řadiče, zobrazení součásti, pomocné rutiny značky a razor kompilace zdrojů.</span><span class="sxs-lookup"><span data-stu-id="b2302-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="b2302-113">[ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) zodpovídá za sledování částí aplikace a poskytovatelů funkcí, které jsou k dispozici v aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="b2302-113">The [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="b2302-114">Můžete pracovat s `ApplicationPartManager` v `Startup` při konfiguraci MVC:</span><span class="sxs-lookup"><span data-stu-id="b2302-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

<span data-ttu-id="b2302-115">Ve výchozím nastavení se MVC vyhledávání strom závislosti a vyhledat řadiče (i v jiných sestavení).</span><span class="sxs-lookup"><span data-stu-id="b2302-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="b2302-116">Načíst libovolný sestavení (například z modulu plug-in, který se odkazuje v době kompilace), můžete použít jako součást aplikace.</span><span class="sxs-lookup"><span data-stu-id="b2302-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="b2302-117">Můžete použít částí aplikace k *vyhnout* hledá řadičů v konkrétní sestavení nebo umístění.</span><span class="sxs-lookup"><span data-stu-id="b2302-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="b2302-118">Můžete řídit, které části (nebo sestavení) jsou k dispozici pro aplikaci změnou `ApplicationParts` kolekce `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="b2302-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="b2302-119">Pořadí položek v `ApplicationParts` kolekce není důležité.</span><span class="sxs-lookup"><span data-stu-id="b2302-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="b2302-120">Je důležité plně nakonfigurovat `ApplicationPartManager` před jeho použitím v kontejneru konfigurace služby.</span><span class="sxs-lookup"><span data-stu-id="b2302-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="b2302-121">Například byste měli plně nakonfigurovat `ApplicationPartManager` před vyvoláním `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="b2302-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="b2302-122">Selhání Uděláte to tak, bude znamenat, že řadiče v částí aplikace přidán po, nebude mít vliv volání metody (nebude získat registrován jako služby) může být důsledkem toho nesprávné bevavior vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b2302-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="b2302-123">Pokud je sestavení obsahující řadiče nechcete používat, odeberte jej z `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="b2302-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="b2302-124">Kromě sestavení projektu a jeho závislá sestavení `ApplicationPartManager` bude obsahovat části pro `Microsoft.AspNetCore.Mvc.TagHelpers` a `Microsoft.AspNetCore.Mvc.Razor` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b2302-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="b2302-125">Zprostředkovatele funkce aplikace</span><span class="sxs-lookup"><span data-stu-id="b2302-125">Application Feature Providers</span></span>

<span data-ttu-id="b2302-126">Zprostředkovatele funkce aplikace zkontrolujte částí aplikace a poskytují funkce pro ty části.</span><span class="sxs-lookup"><span data-stu-id="b2302-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="b2302-127">Existuje poskytovatelů integrované funkce pro následující funkce MVC:</span><span class="sxs-lookup"><span data-stu-id="b2302-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="b2302-128">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="b2302-128">Controllers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="b2302-129">Odkaz na metadata</span><span class="sxs-lookup"><span data-stu-id="b2302-129">Metadata Reference</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="b2302-130">Pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="b2302-130">Tag Helpers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="b2302-131">Zobrazení součásti</span><span class="sxs-lookup"><span data-stu-id="b2302-131">View Components</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="b2302-132">Zprostředkovatelé funkce dědit z `IApplicationFeatureProvider<T>`, kde `T` je typ funkce.</span><span class="sxs-lookup"><span data-stu-id="b2302-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="b2302-133">Můžete implementovat vlastní funkce, kterou zprostředkovatele pro jakýkoli z typů funkce MVC uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="b2302-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="b2302-134">Pořadí poskytovatelů funkce v `ApplicationPartManager.FeatureProviders` kolekce může být důležité, protože novější zprostředkovatelé může reagovat na akce provedené předchozí poskytovatelů.</span><span class="sxs-lookup"><span data-stu-id="b2302-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="b2302-135">Ukázka: Funkce obecné adaptér</span><span class="sxs-lookup"><span data-stu-id="b2302-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="b2302-136">Ve výchozím nastavení, ASP.NET MVC základní ignoruje obecné řadiče (například `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="b2302-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="b2302-137">Tato ukázka používá zprostředkovatele funkce řadiče, který spustí po výchozím zprostředkovatelem a přidá instance obecné řadiče pro zadaný seznam typů (definované v `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="b2302-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="b2302-138">Typy entit:</span><span class="sxs-lookup"><span data-stu-id="b2302-138">The entity types:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="b2302-139">Zprostředkovatele funkce je přidaný do `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b2302-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="b2302-140">Ve výchozím nastavení, Obecný řadič názvy používaných pro směrování by být ve formátu *GenericController 1 [pomůcky]* místo *pomůcky*.</span><span class="sxs-lookup"><span data-stu-id="b2302-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="b2302-141">Upravit název tak, aby odpovídaly obecného typu používané řadičem se používá následující atribut:</span><span class="sxs-lookup"><span data-stu-id="b2302-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="b2302-142">`GenericController` Třídy:</span><span class="sxs-lookup"><span data-stu-id="b2302-142">The `GenericController` class:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="b2302-143">Výsledek, pokud se požaduje odpovídající trasy:</span><span class="sxs-lookup"><span data-stu-id="b2302-143">The result, when a matching route is requested:</span></span>

![Příklad výstup z ukázkové aplikace načte "Hello z obecné Sproket řadiče."](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="b2302-145">Ukázka: Zobrazení dostupných funkcí</span><span class="sxs-lookup"><span data-stu-id="b2302-145">Sample: Display available features</span></span>

<span data-ttu-id="b2302-146">Můžete iterovat vyplněná funkce dostupné pro vaši aplikaci tím, že požádá `ApplicationPartManager` prostřednictvím [vkládání závislostí](../../fundamentals/dependency-injection.md) a jeho použití k naplnění instancí příslušné funkce:</span><span class="sxs-lookup"><span data-stu-id="b2302-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="b2302-147">Příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="b2302-147">Example output:</span></span>

![Příklad výstupu z ukázkové aplikace](app-parts/_static/available-features.png)
