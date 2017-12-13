---
uid: web-api/overview/advanced/dependency-injection
title: "Vkládání závislostí v rozhraní ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: "Tento kurz ukazuje, jak se do vašeho řadiče rozhraní ASP.NET Web API vložit závislosti. Použité v kurzu Unity aplikace bloku 2 rozhraní API webové verze softwaru..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: b4cf39c59ed257b0014dbdbecef3eb7bc48f410d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="f0e23-104">Vkládání závislostí v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="f0e23-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="f0e23-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f0e23-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f0e23-106">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="f0e23-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="f0e23-107">Tento kurz ukazuje, jak se do vašeho řadiče rozhraní ASP.NET Web API vložit závislosti.</span><span class="sxs-lookup"><span data-stu-id="f0e23-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f0e23-108">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="f0e23-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f0e23-109">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="f0e23-109">Web API 2</span></span>
> - [<span data-ttu-id="f0e23-110">Blok aplikace Unity</span><span class="sxs-lookup"><span data-stu-id="f0e23-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="f0e23-111">Entity Framework 6 (verze 5 také funguje)</span><span class="sxs-lookup"><span data-stu-id="f0e23-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="f0e23-112">Co je vkládání závislostí?</span><span class="sxs-lookup"><span data-stu-id="f0e23-112">What is Dependency Injection?</span></span>

<span data-ttu-id="f0e23-113">A *závislostí* je libovolný objekt, který vyžaduje jiný objekt.</span><span class="sxs-lookup"><span data-stu-id="f0e23-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="f0e23-114">Například je běžné k definování [úložiště](http://martinfowler.com/eaaCatalog/repository.html) přístup k datům, která zpracovává.</span><span class="sxs-lookup"><span data-stu-id="f0e23-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="f0e23-115">Umožňuje znázornění s příklad.</span><span class="sxs-lookup"><span data-stu-id="f0e23-115">Let's illustrate with an example.</span></span> <span data-ttu-id="f0e23-116">Nejprve budeme definovat modelu domény:</span><span class="sxs-lookup"><span data-stu-id="f0e23-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="f0e23-117">Tady je jednoduché úložiště třídu, která ukládá položky v databázi pomocí Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f0e23-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="f0e23-118">Nyní nastavíme kontroleru webového rozhraní API, který podporuje požadavky GET pro `Product` entity.</span><span class="sxs-lookup"><span data-stu-id="f0e23-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="f0e23-119">(I mě ponechat POST a další metody pro jednoduchost). Tady je první pokus o:</span><span class="sxs-lookup"><span data-stu-id="f0e23-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="f0e23-120">Všimněte si, že třídy kontroleru závisí na `ProductRepository`, a jsme jsou umožníte řadičem vytvořit `ProductRepository` instance.</span><span class="sxs-lookup"><span data-stu-id="f0e23-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="f0e23-121">Je však vhodné pevný kód závislost tímto způsobem z několika důvodů.</span><span class="sxs-lookup"><span data-stu-id="f0e23-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="f0e23-122">Pokud chcete nahradit `ProductRepository` s jinou implementaci, budete také muset upravit třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f0e23-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="f0e23-123">Pokud `ProductRepository` má závislosti, je nutné nakonfigurovat tyto uvnitř kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f0e23-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="f0e23-124">Pro velké projekt s více řadiči bude konfigurace kódu rozmístěny na projektu.</span><span class="sxs-lookup"><span data-stu-id="f0e23-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="f0e23-125">Je obtížné testů jednotek, protože kontroleru je pevně zakódovaná k dotazování databáze.</span><span class="sxs-lookup"><span data-stu-id="f0e23-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="f0e23-126">Testování částí měli byste použít model nebo se zakázaným inzerováním úložiště, které není možné pomocí currect návrhu.</span><span class="sxs-lookup"><span data-stu-id="f0e23-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="f0e23-127">Budeme moci reagovat na tyto problémy podle *vložení* úložiště do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f0e23-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="f0e23-128">Nejprve Refaktorovat `ProductRepository` třída do rozhraní:</span><span class="sxs-lookup"><span data-stu-id="f0e23-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="f0e23-129">Zadejte `IProductRepository` jako parametr konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="f0e23-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="f0e23-130">Tento příklad používá [konstruktor vkládání](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="f0e23-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="f0e23-131">Můžete také použít *setter vkládání*, kde můžete nastavit závislost prostřednictvím setter metody nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f0e23-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="f0e23-132">Ale nyní došlo k potížím, protože aplikace nemá vytvoření řadiče přímo.</span><span class="sxs-lookup"><span data-stu-id="f0e23-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="f0e23-133">Webové rozhraní API vytvoří řadiče, když ho přesměruje požadavek a webového rozhraní API neví nic `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="f0e23-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="f0e23-134">Toto je, kde odeslán překladač závislostí webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f0e23-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="f0e23-135">Překladač závislostí webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f0e23-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="f0e23-136">Definuje webové rozhraní API **IDependencyResolver** rozhraní pro řešení závislostí.</span><span class="sxs-lookup"><span data-stu-id="f0e23-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="f0e23-137">Tady je definici rozhraní:</span><span class="sxs-lookup"><span data-stu-id="f0e23-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="f0e23-138">**IDependencyScope** rozhraní má dvě metody:</span><span class="sxs-lookup"><span data-stu-id="f0e23-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="f0e23-139">**Metody GetService** vytvoří jednu instanci typu.</span><span class="sxs-lookup"><span data-stu-id="f0e23-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="f0e23-140">**Metodu GetServices** vytvoří kolekci objektů zadaného typu.</span><span class="sxs-lookup"><span data-stu-id="f0e23-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="f0e23-141">**IDependencyResolver** metoda dědí **IDependencyScope** a přidá **BeginScope** metoda.</span><span class="sxs-lookup"><span data-stu-id="f0e23-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="f0e23-142">O oborech I mluvit později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="f0e23-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="f0e23-143">Když webového rozhraní API vytvoří instanci řadiče, první volání **IDependencyResolver.GetService**a předejte typ kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f0e23-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="f0e23-144">Toto rozšíření háku můžete použít k vytvoření kontroleru, řešení všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="f0e23-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="f0e23-145">Pokud **metody GetService** vrátí hodnotu null, vypadá webového rozhraní API pro konstruktor bez parametrů na třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f0e23-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="f0e23-146">Zjištění závislosti, s kontejnerem Unity</span><span class="sxs-lookup"><span data-stu-id="f0e23-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="f0e23-147">I když můžete napsat úplná **IDependencyResolver** implementace od začátku, rozhraní je skutečně navržena tak, aby fungoval jako most mezi webového rozhraní API a existující IoC kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="f0e23-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="f0e23-148">Kontejner IoC je softwarová součást, která je zodpovědná za správu závislosti.</span><span class="sxs-lookup"><span data-stu-id="f0e23-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="f0e23-149">Zaregistrujte typy s kontejner a poté použít k vytvoření objektů kontejneru.</span><span class="sxs-lookup"><span data-stu-id="f0e23-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="f0e23-150">Kontejner se automaticky hodnoty se vztahy závislostí.</span><span class="sxs-lookup"><span data-stu-id="f0e23-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="f0e23-151">Mnoho IoC kontejnery umožňují řídit třeba životní cyklus objektů a oboru.</span><span class="sxs-lookup"><span data-stu-id="f0e23-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="f0e23-152">"IoC" znamená "inverzi řízení", což je obecný vzor, kde rozhraní volání do kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0e23-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="f0e23-153">Kontejner IoC vytvoří vašich objektů, které "Invertuje" obvyklý tok řízení.</span><span class="sxs-lookup"><span data-stu-id="f0e23-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="f0e23-154">V tomto kurzu použijeme [Unity](https://msdn.microsoft.com/en-us/library/ff647202.aspx) z Microsoft Patterns &amp; postupy.</span><span class="sxs-lookup"><span data-stu-id="f0e23-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/en-us/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="f0e23-155">(Zahrnout další oblíbené knihovny [rošády Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), a [StructureMap ](http://docs.structuremap.net/).) Správce balíčků NuGet můžete použít k instalaci Unity.</span><span class="sxs-lookup"><span data-stu-id="f0e23-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="f0e23-156">Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="f0e23-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f0e23-157">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f0e23-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="f0e23-158">Zde je implementací **IDependencyResolver** který zabalí kontejner Unity.</span><span class="sxs-lookup"><span data-stu-id="f0e23-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="f0e23-159">Pokud **metody GetService** metoda nelze přeložit typ, by měla vrátit **null**.</span><span class="sxs-lookup"><span data-stu-id="f0e23-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="f0e23-160">Pokud **metodu GetServices** metoda nelze přeložit typ, měla by vrátit objekt prázdnou kolekci.</span><span class="sxs-lookup"><span data-stu-id="f0e23-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="f0e23-161">Nemáte generování výjimek pro neznámé typy.</span><span class="sxs-lookup"><span data-stu-id="f0e23-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="f0e23-162">Konfigurace překladač závislostí</span><span class="sxs-lookup"><span data-stu-id="f0e23-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="f0e23-163">Nastavit překladač závislostí **překladače závislostí** vlastnost globální **HttpConfiguration** objektu.</span><span class="sxs-lookup"><span data-stu-id="f0e23-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="f0e23-164">Následující kód registruje `IProductRepository` rozhraní s Unity a poté vytvoří `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="f0e23-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="f0e23-165">Závislost oboru a dobu života řadiče</span><span class="sxs-lookup"><span data-stu-id="f0e23-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="f0e23-166">Řadiče se vytvářejí na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="f0e23-166">Controllers are created per request.</span></span> <span data-ttu-id="f0e23-167">Ke správě životnosti objektu **IDependencyResolver** používá koncept *oboru*.</span><span class="sxs-lookup"><span data-stu-id="f0e23-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="f0e23-168">Překladač závislostí, který je připojen k **HttpConfiguration** objekt má globální obor.</span><span class="sxs-lookup"><span data-stu-id="f0e23-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="f0e23-169">Webové rozhraní API, vytvoří řadič, zavolá **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="f0e23-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="f0e23-170">Tato metoda vrátí **IDependencyScope** představující podřízeném oboru.</span><span class="sxs-lookup"><span data-stu-id="f0e23-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="f0e23-171">Pak zavolá rozhraní Web API **metody GetService** v podřízeném oboru vytvoření kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f0e23-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="f0e23-172">Po dokončení žádosti o volání webového rozhraní API **Dispose** v podřízeném oboru.</span><span class="sxs-lookup"><span data-stu-id="f0e23-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="f0e23-173">Použití **Dispose** metodu dispose závislostí kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f0e23-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="f0e23-174">Jak implementovat **BeginScope** závisí na kontejner IoC.</span><span class="sxs-lookup"><span data-stu-id="f0e23-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="f0e23-175">Pro Unity odpovídá oboru kontejneru podřízené:</span><span class="sxs-lookup"><span data-stu-id="f0e23-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="f0e23-176">Většina kontejnery IoC mají podobné ekvivalenty.</span><span class="sxs-lookup"><span data-stu-id="f0e23-176">Most IoC containers have similar equivalents.</span></span>
