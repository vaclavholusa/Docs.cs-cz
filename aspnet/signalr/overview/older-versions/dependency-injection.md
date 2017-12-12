---
uid: signalr/overview/older-versions/dependency-injection
title: "Vkládání závislostí v systému SignalR 1.x | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/15/2013
ms.topic: article
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 6fd155adc9a0aa71b66db7a51062a51fb0c1feca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="6447e-102">Vkládání závislostí v systému SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="6447e-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="6447e-103">podle [Karel Wasson](https://github.com/MikeWasson), [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="6447e-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="6447e-104">Vkládání závislostí je způsob, jak odebrat pevně zakódované závislosti mezi objekty, usnadnit nahradit objektu závislosti, buď pro testování (pomocí mock objektů), nebo chcete změnit chování.</span><span class="sxs-lookup"><span data-stu-id="6447e-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="6447e-105">Tento kurz ukazuje, jak provádět vkládání závislostí na rozbočovače SignalR.</span><span class="sxs-lookup"><span data-stu-id="6447e-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="6447e-106">Také ukazuje, jak používat technologie IoC kontejnery s SignalR.</span><span class="sxs-lookup"><span data-stu-id="6447e-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="6447e-107">Kontejner IoC je obecné rozhraní pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="6447e-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="6447e-108">Co je vkládání závislostí?</span><span class="sxs-lookup"><span data-stu-id="6447e-108">What is Dependency Injection?</span></span>

<span data-ttu-id="6447e-109">Tuto část přeskočte, pokud jste již obeznámeni s vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="6447e-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="6447e-110">*Vkládání závislostí* (DI) je vzor, kde objekty nejsou zodpovědný za vytváření vlastní závislosti.</span><span class="sxs-lookup"><span data-stu-id="6447e-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="6447e-111">Zde je jednoduchý příklad ke DI.</span><span class="sxs-lookup"><span data-stu-id="6447e-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="6447e-112">Předpokládejme, že máte objekt, který potřebuje k protokolování zpráv.</span><span class="sxs-lookup"><span data-stu-id="6447e-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="6447e-113">Můžete třeba definovat rozhraní protokolování:</span><span class="sxs-lookup"><span data-stu-id="6447e-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="6447e-114">V objektu, můžete vytvořit `ILogger` k protokolování zpráv:</span><span class="sxs-lookup"><span data-stu-id="6447e-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="6447e-115">Tento postup funguje, ale není optimální.</span><span class="sxs-lookup"><span data-stu-id="6447e-115">This works, but it's not the best design.</span></span> <span data-ttu-id="6447e-116">Pokud chcete nahradit `FileLogger` s jinou `ILogger` implementace, budete muset upravit `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="6447e-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="6447e-117">Předpokládající, že mnoho dalších objektů pomocí `FileLogger`, budete muset změnit všechny z nich.</span><span class="sxs-lookup"><span data-stu-id="6447e-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="6447e-118">Nebo pokud se rozhodnete, aby `FileLogger` typu singleton, budete také muset provést změny v celé aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6447e-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="6447e-119">Lepším řešením je "Vložit" `ILogger` do objektu – například pomocí argument konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="6447e-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="6447e-120">Teď objekt není zodpovědná za vybrat, které `ILogger` používat.</span><span class="sxs-lookup"><span data-stu-id="6447e-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="6447e-121">Můžete swich `ILogger` implementace, aniž byste museli měnit objekty, které jsou na ní závislé.</span><span class="sxs-lookup"><span data-stu-id="6447e-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="6447e-122">Tento vzor nazývá [konstruktor vkládání](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="6447e-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="6447e-123">Jiné vzor je setter vkládání, kde můžete nastavit závislost prostřednictvím setter metody nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6447e-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="6447e-124">Vkládání jednoduché závislostí v systému SignalR</span><span class="sxs-lookup"><span data-stu-id="6447e-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="6447e-125">Vezměte v úvahu chatovací aplikace z tohoto kurzu [Začínáme s SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="6447e-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="6447e-126">Tady je třídy rozbočovače z této aplikace:</span><span class="sxs-lookup"><span data-stu-id="6447e-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="6447e-127">Předpokládejme, že chcete ukládat chatovací zprávy na serveru před jejich odesláním.</span><span class="sxs-lookup"><span data-stu-id="6447e-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="6447e-128">Můžete například definovat rozhraní, které abstrahuje tuto funkci a pomocí DI vložení do rozhraní `ChatHub` třídy.</span><span class="sxs-lookup"><span data-stu-id="6447e-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="6447e-129">Jediným problémem je, že aplikace SignalR přímo nevytvoří centra; SignalR je automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="6447e-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="6447e-130">Ve výchozím nastavení očekává SignalR třídy rozbočovače mít konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="6447e-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="6447e-131">Však můžete snadno zaregistrovat funkci pro vytvoření instance hub a tuto funkci můžete používat k provedení DI.</span><span class="sxs-lookup"><span data-stu-id="6447e-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="6447e-132">Zaregistrovat funkci voláním **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="6447e-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="6447e-133">Teď SignalR bude volat tuto funkci anonymní, kdykoli bude potřeba vytvořit `ChatHub` instance.</span><span class="sxs-lookup"><span data-stu-id="6447e-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="6447e-134">Technologie IoC kontejnery</span><span class="sxs-lookup"><span data-stu-id="6447e-134">IoC Containers</span></span>

<span data-ttu-id="6447e-135">Předchozí kód je vhodná pro jednoduché případy.</span><span class="sxs-lookup"><span data-stu-id="6447e-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="6447e-136">Ale stále museli zapsat toto:</span><span class="sxs-lookup"><span data-stu-id="6447e-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="6447e-137">V komplexní aplikace s velký počet závislostí může být nutné zapsat spoustu tento kód "kabeláž".</span><span class="sxs-lookup"><span data-stu-id="6447e-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="6447e-138">Tento kód může být obtížné spravovat, zejména v případě, že jsou vnořeny závislosti.</span><span class="sxs-lookup"><span data-stu-id="6447e-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="6447e-139">Také je obtížné testování částí.</span><span class="sxs-lookup"><span data-stu-id="6447e-139">It is also hard to unit test.</span></span>

<span data-ttu-id="6447e-140">Jeden řešením je použití kontejner IoC.</span><span class="sxs-lookup"><span data-stu-id="6447e-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="6447e-141">Kontejner IoC je softwarová součást, která je zodpovědná za správu závislosti. Zaregistrujte typy s kontejner a poté použít k vytvoření objektů kontejneru.</span><span class="sxs-lookup"><span data-stu-id="6447e-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="6447e-142">Kontejner se automaticky hodnoty se vztahy závislostí.</span><span class="sxs-lookup"><span data-stu-id="6447e-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="6447e-143">Mnoho IoC kontejnery umožňují řídit třeba životní cyklus objektů a oboru.</span><span class="sxs-lookup"><span data-stu-id="6447e-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="6447e-144">"IoC" znamená "inverzi řízení", což je obecný vzor, kde rozhraní volání do kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="6447e-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="6447e-145">Kontejner IoC vytvoří vašich objektů, které "Invertuje" obvyklý tok řízení.</span><span class="sxs-lookup"><span data-stu-id="6447e-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="6447e-146">Použití technologie IoC kontejnerů v systému SignalR</span><span class="sxs-lookup"><span data-stu-id="6447e-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="6447e-147">Chatovací aplikace je pravděpodobně příliš jednoduchý, abyste mohli využívat výhod kontejner IoC.</span><span class="sxs-lookup"><span data-stu-id="6447e-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="6447e-148">Místo toho Podíváme se na [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) ukázka.</span><span class="sxs-lookup"><span data-stu-id="6447e-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="6447e-149">Ukázka StockTicker definuje dvě hlavní třídy:</span><span class="sxs-lookup"><span data-stu-id="6447e-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="6447e-150">`StockTickerHub`Třída: rozbočovače, která spravuje připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="6447e-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="6447e-151">`StockTicker`: Typu singleton, která obsahuje uložených ceny a je pravidelně aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="6447e-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="6447e-152">`StockTickerHub`obsahuje odkaz na `StockTicker` typu singleton, zatímco `StockTicker` obsahuje odkaz na **IHubConnectionContext** pro `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="6447e-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="6447e-153">Toto rozhraní se používá ke komunikaci s `StockTickerHub` instance.</span><span class="sxs-lookup"><span data-stu-id="6447e-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="6447e-154">(Další informace najdete v tématu [Server všesměrového vysílání pomocí funkce SignalR technologie ASP.NET](index.md).)</span><span class="sxs-lookup"><span data-stu-id="6447e-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="6447e-155">Kontejner IoC jsme vám pomůže trochu untangle tyto závislosti.</span><span class="sxs-lookup"><span data-stu-id="6447e-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="6447e-156">Nejprve umožňuje zjednodušit `StockTickerHub` a `StockTicker` třídy.</span><span class="sxs-lookup"><span data-stu-id="6447e-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="6447e-157">V následujícím kódu I jste komentované části jsme nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="6447e-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="6447e-158">Odebrat konstruktor bez parametrů z `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="6447e-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="6447e-159">Místo toho jsme vždy použije DI k vytvoření rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="6447e-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="6447e-160">U StockTicker odeberte instanci typu singleton.</span><span class="sxs-lookup"><span data-stu-id="6447e-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="6447e-161">Později použijeme kontejner IoC k řízení životního cyklu StockTicker.</span><span class="sxs-lookup"><span data-stu-id="6447e-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="6447e-162">Také zveřejněte konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="6447e-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="6447e-163">V dalším kroku jsme Refaktorovat kód tak, že vytvoříte rozhraní pro `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="6447e-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="6447e-164">Toto rozhraní použijeme oddělit `StockTickerHub` z `StockTicker` třídy.</span><span class="sxs-lookup"><span data-stu-id="6447e-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="6447e-165">Sada Visual Studio provádí tento druh refaktoringu easy přístup.</span><span class="sxs-lookup"><span data-stu-id="6447e-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="6447e-166">Otevřete soubor StockTicker.cs, klikněte pravým tlačítkem na `StockTicker` deklarace třídy a vyberte **Refaktorovat** ... **Extrahování rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="6447e-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="6447e-167">V **extrahování rozhraní** dialogové okno, klikněte na tlačítko **Vybrat vše**.</span><span class="sxs-lookup"><span data-stu-id="6447e-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="6447e-168">Nechte ostatní výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6447e-168">Leave the other defaults.</span></span> <span data-ttu-id="6447e-169">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="6447e-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="6447e-170">Visual Studio vytvoří nové rozhraní s názvem `IStockTicker`a také změní `StockTicker` k odvozování z `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="6447e-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="6447e-171">Otevřete soubor IStockTicker.cs a změňte rozhraní **veřejné**.</span><span class="sxs-lookup"><span data-stu-id="6447e-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="6447e-172">V `StockTickerHub` třídy, změňte dvě instance `StockTicker` k `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="6447e-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="6447e-173">Vytvoření `IStockTicker` rozhraní není nezbytně nutné, ale vy jste chtěli zobrazit, jak DI může přispět ke snížení párování mezi komponentami ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6447e-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="6447e-174">Přidání knihovny Ninject</span><span class="sxs-lookup"><span data-stu-id="6447e-174">Add the Ninject Library</span></span>

<span data-ttu-id="6447e-175">Existuje mnoho kontejnerů IoC open source pro .NET.</span><span class="sxs-lookup"><span data-stu-id="6447e-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="6447e-176">V tomto kurzu budete použít [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="6447e-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="6447e-177">(Zahrnout další oblíbené knihovny [rošády Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), a [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="6447e-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="6447e-178">Použití Správce balíčků NuGet k instalaci [Ninject knihovny](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="6447e-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="6447e-179">V sadě Visual Studio z **nástroje** nabídky vyberte možnost **Správce balíčků knihoven** | **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="6447e-179">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="6447e-180">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6447e-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="6447e-181">Nahraďte překladač závislostí pro SignalR</span><span class="sxs-lookup"><span data-stu-id="6447e-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="6447e-182">Pokud chcete použít Ninject v rámci SignalR, vytvořte třídu, která je odvozena z **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="6447e-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="6447e-183">Tato třída přepsání **metody GetService** a **metodu GetServices** metody **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="6447e-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="6447e-184">SignalR volá tyto metody vytvořit různé objekty za běhu, včetně instance hub, jakož i různé služby, které se používá interně SignalR.</span><span class="sxs-lookup"><span data-stu-id="6447e-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="6447e-185">**Metody GetService** metoda vytvoří jednu instanci typu.</span><span class="sxs-lookup"><span data-stu-id="6447e-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="6447e-186">Potlačí tuto metodu volat jádra Ninject **TryGet** metoda.</span><span class="sxs-lookup"><span data-stu-id="6447e-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="6447e-187">Pokud tato metoda vrátí hodnotu null, přejít k výchozí překladač.</span><span class="sxs-lookup"><span data-stu-id="6447e-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="6447e-188">**Metodu GetServices** metoda vytvoří kolekci objektů zadaného typu.</span><span class="sxs-lookup"><span data-stu-id="6447e-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="6447e-189">Potlačí tuto metodu ke zřetězení výsledků Ninject s výsledky z výchozí překladač.</span><span class="sxs-lookup"><span data-stu-id="6447e-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="6447e-190">Konfigurovat Ninject vazby</span><span class="sxs-lookup"><span data-stu-id="6447e-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="6447e-191">Nyní použijeme Ninject deklarovat typ vazby.</span><span class="sxs-lookup"><span data-stu-id="6447e-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="6447e-192">Otevřete soubor RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="6447e-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="6447e-193">V `RegisterHubs.Start` metoda, vytvoření kontejneru Ninject, který volá Ninject *jádra*.</span><span class="sxs-lookup"><span data-stu-id="6447e-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="6447e-194">Vytvoření instance překladače naše vlastní závislosti:</span><span class="sxs-lookup"><span data-stu-id="6447e-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="6447e-195">Vytvoření vazby pro `IStockTicker` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6447e-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="6447e-196">Tento kód vlastně říká dvě věci.</span><span class="sxs-lookup"><span data-stu-id="6447e-196">This code is saying two things.</span></span> <span data-ttu-id="6447e-197">První, vždy, když aplikace potřebuje `IStockTicker`, jádra by měl vytvořit instanci `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="6447e-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="6447e-198">Druhý, `StockTicker` třída by měla být vytvořená jako objekt typu singleton.</span><span class="sxs-lookup"><span data-stu-id="6447e-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="6447e-199">Ninject vytvoří jednu instanci objektu a vrátit stejnou instanci pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="6447e-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="6447e-200">Vytvoření vazby pro **IHubConnectionContext** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6447e-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="6447e-201">Tento kód creatres anonymní funkce, která vrací **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="6447e-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="6447e-202">**WhenInjectedInto** metoda informuje Ninject tuto funkci použít, pouze při vytváření `IStockTicker` instance.</span><span class="sxs-lookup"><span data-stu-id="6447e-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="6447e-203">Důvodem je skutečnost, že se vytvoří SignalR **IHubConnectionContext** instance interně, a nechceme přepsat, jak je vytvořila SignalR.</span><span class="sxs-lookup"><span data-stu-id="6447e-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="6447e-204">Tato funkce se vztahuje pouze na našem `StockTicker` třídy.</span><span class="sxs-lookup"><span data-stu-id="6447e-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="6447e-205">Předat do překladače závislostí **MapHubs** metoda:</span><span class="sxs-lookup"><span data-stu-id="6447e-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="6447e-206">Teď SignalR použije překladač zadaný v **MapHubs**, namísto výchozí překladač.</span><span class="sxs-lookup"><span data-stu-id="6447e-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="6447e-207">Zde je kód úplný výpis pro `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="6447e-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="6447e-208">Ke spuštění aplikace StockTicker v sadě Visual Studio, stisknutím klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="6447e-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="6447e-209">V okně prohlížeče, přejděte na `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="6447e-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="6447e-210">Aplikace má přesně stejné funkce jako před.</span><span class="sxs-lookup"><span data-stu-id="6447e-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="6447e-211">(Popis najdete v tématu [Server všesměrového vysílání pomocí funkce SignalR technologie ASP.NET](index.md).) Jsme nedošlo ke změně chování; právě snazší kód pro testování, údržbu a momentální.</span><span class="sxs-lookup"><span data-stu-id="6447e-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
