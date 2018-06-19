---
uid: signalr/overview/advanced/dependency-injection
title: Vkládání závislostí v systému SignalR | Microsoft Docs
author: MikeWasson
description: Verze softwaru použitým v tomto tématu Visual Studio 2013 .NET 4.5 SignalR verze 2 předchozí verze tohoto tématu informace o předchozích verzí aplikace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26565558"
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="46dee-103">Vkládání závislostí v systému SignalR</span><span class="sxs-lookup"><span data-stu-id="46dee-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="46dee-104">podle [Karel Wasson](https://github.com/MikeWasson), [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="46dee-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="46dee-105">Verze softwaru použitým v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="46dee-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="46dee-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="46dee-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="46dee-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="46dee-107">.NET 4.5</span></span>
> - <span data-ttu-id="46dee-108">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="46dee-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="46dee-109">Předchozí verze tohoto tématu</span><span class="sxs-lookup"><span data-stu-id="46dee-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="46dee-110">Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="46dee-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="46dee-111">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="46dee-111">Questions and comments</span></span>
> 
> <span data-ttu-id="46dee-112">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="46dee-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="46dee-113">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="46dee-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="46dee-114">Vkládání závislostí je způsob, jak odebrat pevně zakódované závislosti mezi objekty, usnadnit nahradit objektu závislosti, buď pro testování (pomocí mock objektů), nebo chcete změnit chování.</span><span class="sxs-lookup"><span data-stu-id="46dee-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="46dee-115">Tento kurz ukazuje, jak provádět vkládání závislostí na rozbočovače SignalR.</span><span class="sxs-lookup"><span data-stu-id="46dee-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="46dee-116">Také ukazuje, jak používat technologie IoC kontejnery s SignalR.</span><span class="sxs-lookup"><span data-stu-id="46dee-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="46dee-117">Kontejner IoC je obecné rozhraní pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="46dee-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="46dee-118">Co je vkládání závislostí?</span><span class="sxs-lookup"><span data-stu-id="46dee-118">What is Dependency Injection?</span></span>

<span data-ttu-id="46dee-119">Tuto část přeskočte, pokud jste již obeznámeni s vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="46dee-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="46dee-120">*Vkládání závislostí* (DI) je vzor, kde objekty nejsou zodpovědný za vytváření vlastní závislosti.</span><span class="sxs-lookup"><span data-stu-id="46dee-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="46dee-121">Zde je jednoduchý příklad ke DI.</span><span class="sxs-lookup"><span data-stu-id="46dee-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="46dee-122">Předpokládejme, že máte objekt, který potřebuje k protokolování zpráv.</span><span class="sxs-lookup"><span data-stu-id="46dee-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="46dee-123">Můžete třeba definovat rozhraní protokolování:</span><span class="sxs-lookup"><span data-stu-id="46dee-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="46dee-124">V objektu, můžete vytvořit `ILogger` k protokolování zpráv:</span><span class="sxs-lookup"><span data-stu-id="46dee-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="46dee-125">Tento postup funguje, ale není optimální.</span><span class="sxs-lookup"><span data-stu-id="46dee-125">This works, but it's not the best design.</span></span> <span data-ttu-id="46dee-126">Pokud chcete nahradit `FileLogger` s jinou `ILogger` implementace, budete muset upravit `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="46dee-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="46dee-127">Předpokládající, že mnoho dalších objektů pomocí `FileLogger`, budete muset změnit všechny z nich.</span><span class="sxs-lookup"><span data-stu-id="46dee-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="46dee-128">Nebo pokud se rozhodnete, aby `FileLogger` typu singleton, budete také muset provést změny v celé aplikaci.</span><span class="sxs-lookup"><span data-stu-id="46dee-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="46dee-129">Lepším řešením je "Vložit" `ILogger` do objektu – například pomocí argument konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="46dee-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="46dee-130">Teď objekt není zodpovědná za vybrat, které `ILogger` používat.</span><span class="sxs-lookup"><span data-stu-id="46dee-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="46dee-131">Můžete swich `ILogger` implementace, aniž byste museli měnit objekty, které jsou na ní závislé.</span><span class="sxs-lookup"><span data-stu-id="46dee-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="46dee-132">Tento vzor nazývá [konstruktor vkládání](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="46dee-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="46dee-133">Jiné vzor je setter vkládání, kde můžete nastavit závislost prostřednictvím setter metody nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="46dee-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="46dee-134">Vkládání jednoduché závislostí v systému SignalR</span><span class="sxs-lookup"><span data-stu-id="46dee-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="46dee-135">Vezměte v úvahu chatovací aplikace z tohoto kurzu [Začínáme s SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="46dee-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="46dee-136">Tady je třídy rozbočovače z této aplikace:</span><span class="sxs-lookup"><span data-stu-id="46dee-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="46dee-137">Předpokládejme, že chcete ukládat chatovací zprávy na serveru před jejich odesláním.</span><span class="sxs-lookup"><span data-stu-id="46dee-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="46dee-138">Můžete například definovat rozhraní, které abstrahuje tuto funkci a pomocí DI vložení do rozhraní `ChatHub` třídy.</span><span class="sxs-lookup"><span data-stu-id="46dee-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="46dee-139">Jediným problémem je, že aplikace SignalR přímo nevytvoří centra; SignalR je automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="46dee-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="46dee-140">Ve výchozím nastavení očekává SignalR třídy rozbočovače mít konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="46dee-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="46dee-141">Však můžete snadno zaregistrovat funkci pro vytvoření instance hub a tuto funkci můžete používat k provedení DI.</span><span class="sxs-lookup"><span data-stu-id="46dee-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="46dee-142">Zaregistrovat funkci voláním **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="46dee-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="46dee-143">Teď SignalR bude volat tuto funkci anonymní, kdykoli bude potřeba vytvořit `ChatHub` instance.</span><span class="sxs-lookup"><span data-stu-id="46dee-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="46dee-144">Technologie IoC kontejnery</span><span class="sxs-lookup"><span data-stu-id="46dee-144">IoC Containers</span></span>

<span data-ttu-id="46dee-145">Předchozí kód je vhodná pro jednoduché případy.</span><span class="sxs-lookup"><span data-stu-id="46dee-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="46dee-146">Ale stále museli zapsat toto:</span><span class="sxs-lookup"><span data-stu-id="46dee-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="46dee-147">V komplexní aplikace s velký počet závislostí může být nutné zapsat spoustu tento kód "kabeláž".</span><span class="sxs-lookup"><span data-stu-id="46dee-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="46dee-148">Tento kód může být obtížné spravovat, zejména v případě, že jsou vnořeny závislosti.</span><span class="sxs-lookup"><span data-stu-id="46dee-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="46dee-149">Také je obtížné testování částí.</span><span class="sxs-lookup"><span data-stu-id="46dee-149">It is also hard to unit test.</span></span>

<span data-ttu-id="46dee-150">Jeden řešením je použití kontejner IoC.</span><span class="sxs-lookup"><span data-stu-id="46dee-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="46dee-151">Kontejner IoC je softwarová součást, která je zodpovědná za správu závislosti. Zaregistrujte typy s kontejner a poté použít k vytvoření objektů kontejneru.</span><span class="sxs-lookup"><span data-stu-id="46dee-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="46dee-152">Kontejner se automaticky hodnoty se vztahy závislostí.</span><span class="sxs-lookup"><span data-stu-id="46dee-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="46dee-153">Mnoho IoC kontejnery umožňují řídit třeba životní cyklus objektů a oboru.</span><span class="sxs-lookup"><span data-stu-id="46dee-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="46dee-154">"IoC" znamená "inverzi řízení", což je obecný vzor, kde rozhraní volání do kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="46dee-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="46dee-155">Kontejner IoC vytvoří vašich objektů, které "Invertuje" obvyklý tok řízení.</span><span class="sxs-lookup"><span data-stu-id="46dee-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="46dee-156">Použití technologie IoC kontejnerů v systému SignalR</span><span class="sxs-lookup"><span data-stu-id="46dee-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="46dee-157">Chatovací aplikace je pravděpodobně příliš jednoduchý, abyste mohli využívat výhod kontejner IoC.</span><span class="sxs-lookup"><span data-stu-id="46dee-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="46dee-158">Místo toho Podíváme se na [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) ukázka.</span><span class="sxs-lookup"><span data-stu-id="46dee-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="46dee-159">Ukázka StockTicker definuje dvě hlavní třídy:</span><span class="sxs-lookup"><span data-stu-id="46dee-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="46dee-160">`StockTickerHub`Třída: rozbočovače, která spravuje připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="46dee-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="46dee-161">`StockTicker`: Typu singleton, která obsahuje uložených ceny a je pravidelně aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="46dee-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="46dee-162">`StockTickerHub`obsahuje odkaz na `StockTicker` typu singleton, zatímco `StockTicker` obsahuje odkaz na **IHubConnectionContext** pro `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="46dee-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="46dee-163">Toto rozhraní se používá ke komunikaci s `StockTickerHub` instance.</span><span class="sxs-lookup"><span data-stu-id="46dee-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="46dee-164">(Další informace najdete v tématu [Server všesměrového vysílání pomocí funkce SignalR technologie ASP.NET](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="46dee-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="46dee-165">Kontejner IoC jsme vám pomůže trochu untangle tyto závislosti.</span><span class="sxs-lookup"><span data-stu-id="46dee-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="46dee-166">Nejprve umožňuje zjednodušit `StockTickerHub` a `StockTicker` třídy.</span><span class="sxs-lookup"><span data-stu-id="46dee-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="46dee-167">V následujícím kódu I jste komentované části jsme nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="46dee-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="46dee-168">Odebrat konstruktor bez parametrů z `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="46dee-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="46dee-169">Místo toho jsme vždy použije DI k vytvoření rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="46dee-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="46dee-170">U StockTicker odeberte instanci typu singleton.</span><span class="sxs-lookup"><span data-stu-id="46dee-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="46dee-171">Později použijeme kontejner IoC k řízení životního cyklu StockTicker.</span><span class="sxs-lookup"><span data-stu-id="46dee-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="46dee-172">Také zveřejněte konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="46dee-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="46dee-173">V dalším kroku jsme Refaktorovat kód tak, že vytvoříte rozhraní pro `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="46dee-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="46dee-174">Toto rozhraní použijeme oddělit `StockTickerHub` z `StockTicker` třídy.</span><span class="sxs-lookup"><span data-stu-id="46dee-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="46dee-175">Sada Visual Studio provádí tento druh refaktoringu easy přístup.</span><span class="sxs-lookup"><span data-stu-id="46dee-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="46dee-176">Otevřete soubor StockTicker.cs, klikněte pravým tlačítkem na `StockTicker` deklarace třídy a vyberte **Refaktorovat** ... **Extrahování rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="46dee-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="46dee-177">V **extrahování rozhraní** dialogové okno, klikněte na tlačítko **Vybrat vše**.</span><span class="sxs-lookup"><span data-stu-id="46dee-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="46dee-178">Nechte ostatní výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="46dee-178">Leave the other defaults.</span></span> <span data-ttu-id="46dee-179">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="46dee-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="46dee-180">Visual Studio vytvoří nové rozhraní s názvem `IStockTicker`a také změní `StockTicker` k odvozování z `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="46dee-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="46dee-181">Otevřete soubor IStockTicker.cs a změňte rozhraní **veřejné**.</span><span class="sxs-lookup"><span data-stu-id="46dee-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="46dee-182">V `StockTickerHub` třídy, změňte dvě instance `StockTicker` k `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="46dee-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="46dee-183">Vytvoření `IStockTicker` rozhraní není nezbytně nutné, ale vy jste chtěli zobrazit, jak DI může přispět ke snížení párování mezi komponentami ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="46dee-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="46dee-184">Přidání knihovny Ninject</span><span class="sxs-lookup"><span data-stu-id="46dee-184">Add the Ninject Library</span></span>

<span data-ttu-id="46dee-185">Existuje mnoho kontejnerů IoC open source pro .NET.</span><span class="sxs-lookup"><span data-stu-id="46dee-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="46dee-186">V tomto kurzu budete použít [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="46dee-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="46dee-187">(Zahrnout další oblíbené knihovny [rošády Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), a [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="46dee-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="46dee-188">Použití Správce balíčků NuGet k instalaci [Ninject knihovny](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="46dee-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="46dee-189">V sadě Visual Studio z **nástroje** nabídky vyberte možnost **Správce balíčků knihoven** | **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="46dee-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="46dee-190">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="46dee-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="46dee-191">Nahraďte překladač závislostí pro SignalR</span><span class="sxs-lookup"><span data-stu-id="46dee-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="46dee-192">Pokud chcete použít Ninject v rámci SignalR, vytvořte třídu, která je odvozena z **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="46dee-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="46dee-193">Tato třída přepsání **metody GetService** a **metodu GetServices** metody **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="46dee-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="46dee-194">SignalR volá tyto metody vytvořit různé objekty za běhu, včetně instance hub, jakož i různé služby, které se používá interně SignalR.</span><span class="sxs-lookup"><span data-stu-id="46dee-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="46dee-195">**Metody GetService** metoda vytvoří jednu instanci typu.</span><span class="sxs-lookup"><span data-stu-id="46dee-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="46dee-196">Potlačí tuto metodu volat jádra Ninject **TryGet** metoda.</span><span class="sxs-lookup"><span data-stu-id="46dee-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="46dee-197">Pokud tato metoda vrátí hodnotu null, přejít k výchozí překladač.</span><span class="sxs-lookup"><span data-stu-id="46dee-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="46dee-198">**Metodu GetServices** metoda vytvoří kolekci objektů zadaného typu.</span><span class="sxs-lookup"><span data-stu-id="46dee-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="46dee-199">Potlačí tuto metodu ke zřetězení výsledků Ninject s výsledky z výchozí překladač.</span><span class="sxs-lookup"><span data-stu-id="46dee-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="46dee-200">Konfigurovat Ninject vazby</span><span class="sxs-lookup"><span data-stu-id="46dee-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="46dee-201">Nyní použijeme Ninject deklarovat typ vazby.</span><span class="sxs-lookup"><span data-stu-id="46dee-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="46dee-202">Otevřete třídy Startup.cs vaší aplikace (které jste buď vytvořili ručně podle pokynů balíčku v `readme.txt`, nebo který byl vytvořen přidání ověřování do projektu).</span><span class="sxs-lookup"><span data-stu-id="46dee-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="46dee-203">V `Startup.Configuration` metoda, vytvoření kontejneru Ninject, který volá Ninject *jádra*.</span><span class="sxs-lookup"><span data-stu-id="46dee-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="46dee-204">Vytvoření instance překladače naše vlastní závislosti:</span><span class="sxs-lookup"><span data-stu-id="46dee-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="46dee-205">Vytvoření vazby pro `IStockTicker` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="46dee-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="46dee-206">Tento kód vlastně říká dvě věci.</span><span class="sxs-lookup"><span data-stu-id="46dee-206">This code is saying two things.</span></span> <span data-ttu-id="46dee-207">První, vždy, když aplikace potřebuje `IStockTicker`, jádra by měl vytvořit instanci `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="46dee-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="46dee-208">Druhý, `StockTicker` třída by měla být vytvořená jako objekt typu singleton.</span><span class="sxs-lookup"><span data-stu-id="46dee-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="46dee-209">Ninject vytvoří jednu instanci objektu a vrátit stejnou instanci pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="46dee-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="46dee-210">Vytvoření vazby pro **IHubConnectionContext** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="46dee-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="46dee-211">Tento kód creatres anonymní funkce, která vrací **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="46dee-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="46dee-212">**WhenInjectedInto** metoda informuje Ninject tuto funkci použít, pouze při vytváření `IStockTicker` instance.</span><span class="sxs-lookup"><span data-stu-id="46dee-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="46dee-213">Důvodem je skutečnost, že se vytvoří SignalR **IHubConnectionContext** instance interně, a nechceme přepsat, jak je vytvořila SignalR.</span><span class="sxs-lookup"><span data-stu-id="46dee-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="46dee-214">Tato funkce se vztahuje pouze na našem `StockTicker` třídy.</span><span class="sxs-lookup"><span data-stu-id="46dee-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="46dee-215">Předat do překladače závislostí **MapSignalR** metoda přidáním konfiguraci rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="46dee-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="46dee-216">Aktualizujte metodu Startup.ConfigureSignalR v třída při spuštění tohoto příkladu nový parametr:</span><span class="sxs-lookup"><span data-stu-id="46dee-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="46dee-217">Teď SignalR použije překladač zadaný v **MapSignalR**, namísto výchozí překladač.</span><span class="sxs-lookup"><span data-stu-id="46dee-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="46dee-218">Zde je kód úplný výpis pro `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="46dee-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="46dee-219">Ke spuštění aplikace StockTicker v sadě Visual Studio, stisknutím klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="46dee-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="46dee-220">V okně prohlížeče, přejděte na `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="46dee-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="46dee-221">Aplikace má přesně stejné funkce jako před.</span><span class="sxs-lookup"><span data-stu-id="46dee-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="46dee-222">(Popis najdete v tématu [Server všesměrového vysílání pomocí funkce SignalR technologie ASP.NET](../getting-started/tutorial-server-broadcast-with-signalr.md).) Jsme nedošlo ke změně chování; právě snazší kód pro testování, údržbu a momentální.</span><span class="sxs-lookup"><span data-stu-id="46dee-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
