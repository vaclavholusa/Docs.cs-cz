---
title: "Vkládání závislostí do řadiče"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 21cffcd285879fdca81cb7d92d0f079d4bf7756c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-into-controllers"></a><span data-ttu-id="ba8db-102">Vkládání závislostí do řadiče</span><span class="sxs-lookup"><span data-stu-id="ba8db-102">Dependency injection into controllers</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="ba8db-103">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ba8db-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ba8db-104">Kontrolery MVC ASP.NET Core měli požádat o jejich závislosti explicitně prostřednictvím jejich konstruktory.</span><span class="sxs-lookup"><span data-stu-id="ba8db-104">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="ba8db-105">V některých případech akce jednotlivých kontroleru může vyžadovat služba a nemusí mít smysl k požadavku na úrovni kontroleru.</span><span class="sxs-lookup"><span data-stu-id="ba8db-105">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="ba8db-106">V takovém případě je také možné vložit služby jako parametr v metodě akce.</span><span class="sxs-lookup"><span data-stu-id="ba8db-106">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="ba8db-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ba8db-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="ba8db-108">Vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="ba8db-108">Dependency Injection</span></span>

<span data-ttu-id="ba8db-109">Vkládání závislostí je technika, který následuje [závislostí inverzi Princip](http://deviq.com/dependency-inversion-principle/), povolení pro aplikace, musí se skládat z volně párované moduly.</span><span class="sxs-lookup"><span data-stu-id="ba8db-109">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="ba8db-110">Má integrovanou podporu pro ASP.NET Core [vkládání závislostí](../../fundamentals/dependency-injection.md), což usnadňuje aplikace pro testování a údržbu.</span><span class="sxs-lookup"><span data-stu-id="ba8db-110">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="ba8db-111">Vkládání – konstruktor</span><span class="sxs-lookup"><span data-stu-id="ba8db-111">Constructor Injection</span></span>

<span data-ttu-id="ba8db-112">ASP.NET Core integrovanou podporu pro vkládání závislostí na základě konstruktor rozšiřuje na řadiče MVC.</span><span class="sxs-lookup"><span data-stu-id="ba8db-112">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="ba8db-113">Stačí přidat typ služby k řadiči jako parametr konstruktoru, se pokusí přeložit typu pomocí jeho vytvořené v kontejneru služby ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ba8db-113">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="ba8db-114">Služby jsou obvykle, ale ne vždy definovány, pomocí rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ba8db-114">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="ba8db-115">Například pokud aplikace obsahuje obchodní logiky, která závisí na aktuální čas, můžete vložit službu, která načte dobu (nikoli nezakódovávejte ho), který by umožnil testy předávat implementace, které používají nastavit čas.</span><span class="sxs-lookup"><span data-stu-id="ba8db-115">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="ba8db-116">Implementace rozhraní tohoto typu tak, aby používala systémové hodiny v době běhu je jednoduchá:</span><span class="sxs-lookup"><span data-stu-id="ba8db-116">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="ba8db-117">S tímto na místě můžete použít službu v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="ba8db-117">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="ba8db-118">V takovém případě jsme přidali některé logiku `HomeController` `Index` metodu pro zobrazení pohlednice uživateli podle denní dobu.</span><span class="sxs-lookup"><span data-stu-id="ba8db-118">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="ba8db-119">Pokud jsme spustit nyní aplikaci, jsme pravděpodobně dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="ba8db-119">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="ba8db-120">K této chybě dojde, když jsme nenakonfigurovali služby v `ConfigureServices` metoda v našich `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="ba8db-120">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="ba8db-121">Chcete-li určit, který požaduje pro `IDateTime` by se měly vyřešit pomocí instance `SystemDateTime`, přidejte zvýrazněný řádek v seznamu níže na vaše `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="ba8db-121">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="ba8db-122">Tato konkrétní službu může být implementovaná pomocí některé z možností několik různých doba platnosti (`Transient`, `Scoped`, nebo `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="ba8db-122">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="ba8db-123">V tématu [vkládání závislostí](../../fundamentals/dependency-injection.md) pochopit, jak každá z těchto možností oboru ovlivní chování služby.</span><span class="sxs-lookup"><span data-stu-id="ba8db-123">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="ba8db-124">Jakmile služba byla nakonfigurována, spuštění aplikace a přechodu na domovskou stránku měli zobrazení založené na čase zprávy podle očekávání:</span><span class="sxs-lookup"><span data-stu-id="ba8db-124">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Server pozdravu](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="ba8db-126">V tématu [testování logiku řadič](testing.md) Další informace o explicitní žádost o závislosti [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) v řadiče usnadňuje kód pro testování.</span><span class="sxs-lookup"><span data-stu-id="ba8db-126">See [Testing Controller Logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="ba8db-127">Vkládání předdefinovaných závislostí ASP.NET Core podporuje mít jenom jeden konstruktor pro třídy, které žádají o služby.</span><span class="sxs-lookup"><span data-stu-id="ba8db-127">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="ba8db-128">Pokud máte více než jeden konstruktor, může dojít, s oznámením o výjimce:</span><span class="sxs-lookup"><span data-stu-id="ba8db-128">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="ba8db-129">Jako zobrazí se chybová zpráva, můžete vyřešit potíže s právě jeden konstruktor.</span><span class="sxs-lookup"><span data-stu-id="ba8db-129">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="ba8db-130">Můžete také [nahraďte podpoře vkládání závislostí výchozí implementace třetích stran](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), řadu které budou podporovat několik konstruktorů.</span><span class="sxs-lookup"><span data-stu-id="ba8db-130">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="ba8db-131">Vkládání akce s FromServices</span><span class="sxs-lookup"><span data-stu-id="ba8db-131">Action Injection with FromServices</span></span>

<span data-ttu-id="ba8db-132">Někdy nepotřebujete služby pro více než jednu akci v rámci vašeho řadiče.</span><span class="sxs-lookup"><span data-stu-id="ba8db-132">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="ba8db-133">V takovém případě má smysl vložení služby jako parametr pro metodu akce.</span><span class="sxs-lookup"><span data-stu-id="ba8db-133">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="ba8db-134">K tomu je potřeba označení parametr s atributem `[FromServices]` jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="ba8db-134">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="ba8db-135">Přístup k nastavení z řadiče</span><span class="sxs-lookup"><span data-stu-id="ba8db-135">Accessing Settings from a Controller</span></span>

<span data-ttu-id="ba8db-136">Přístup k aplikaci nebo konfigurace nastavení z v kontroleru je běžný vzor.</span><span class="sxs-lookup"><span data-stu-id="ba8db-136">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="ba8db-137">Tento přístup má použít vzor možnosti popsané v [konfigurace](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="ba8db-137">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="ba8db-138">Nastavení obecně by neměl žádosti přímo z vašeho řadiče pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="ba8db-138">You generally should not request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="ba8db-139">Lepším řešením je požadavek `IOptions<T>` instance, kde `T` je třída konfigurace budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="ba8db-139">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="ba8db-140">Chcete-li pracovat s vzoru možnosti, vytvořte třídu, která představuje možnosti, jako je tato:</span><span class="sxs-lookup"><span data-stu-id="ba8db-140">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="ba8db-141">Pak budete muset nakonfigurovat aplikaci, aby používá možnosti model a přidat vaší třídě konfigurace ke kolekci služby v `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ba8db-141">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="ba8db-142">V seznamu nahoře, jsme konfigurujete aplikaci číst nastavení ze souboru formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ba8db-142">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="ba8db-143">Zcela v kódu, můžete také nakonfigurovat nastavení, které se zobrazí ve výše uvedeném komentáři kódu.</span><span class="sxs-lookup"><span data-stu-id="ba8db-143">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="ba8db-144">V tématu [konfigurace](xref:fundamentals/configuration/index) pro další možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ba8db-144">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="ba8db-145">Jakmile jste určili objekt silného typu konfigurace (v tomto případě `SampleWebSettings`) a jeho přidání ke kolekci služby můžete žádosti ji ze všech Kontroleru nebo metodě akce tím, že požádá o instanci `IOptions<T>` (v tomto případě `IOptions<SampleWebSettings>`) .</span><span class="sxs-lookup"><span data-stu-id="ba8db-145">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="ba8db-146">Následující kód ukazuje, jak jeden vyžadují nastavení z řadiče:</span><span class="sxs-lookup"><span data-stu-id="ba8db-146">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="ba8db-147">Následující možnosti vzor umožňuje nastavení a konfiguraci, chcete-li být odděleno od sebe navzájem a zajišťuje kontroleru je následující [oddělené oblasti zájmu](http://deviq.com/separation-of-concerns/), protože nepotřebuje vědět, jak a kde najít nastavení informace.</span><span class="sxs-lookup"><span data-stu-id="ba8db-147">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="ba8db-148">Také umožňuje kontroleru usnadňují testování částí [testování logiku řadiče](testing.md), protože neexistuje žádné [statické plevami](http://deviq.com/static-cling/) nebo přímé vytváření instancí třídy nastavení v rámci třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="ba8db-148">It also makes the controller easier to unit test [Testing Controller Logic](testing.md), since there is no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
