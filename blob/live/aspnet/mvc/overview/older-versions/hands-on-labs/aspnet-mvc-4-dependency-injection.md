---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "Vkládání závislostí architektury ASP.NET MVC 4 | Microsoft Docs"
author: rick-anderson
description: "Poznámka: Toto testovací prostředí Hands-on předpokládá, že máte základní znalosti o architektuře ASP.NET MVC a ASP.NET MVC 4 filtry. Pokud jste nepoužili ASP.NET MVC 4 filtry před jsme rec..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: af4967f642ba4615f3392c0c404d2ec62edaaae8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="c8f56-104">Vkládání závislostí architektury ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="c8f56-104">ASP.NET MVC 4 Dependency Injection</span></span>
====================
<span data-ttu-id="c8f56-105">podle [webové táborech Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c8f56-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="c8f56-106">Toto testovací prostředí Hands-on předpokládá, že máte základní znalosti o **ASP.NET MVC** a **ASP.NET MVC 4 filtry**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="c8f56-107">Pokud jste nepoužili **ASP.NET MVC 4 filtry** před, doporučujeme si projít **ASP.NET MVC vlastní akce filtry** Hands-on testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8f56-107">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="c8f56-108">Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="c8f56-108">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<span data-ttu-id="c8f56-109">V **zaměřené na konkrétní objekt programování** zlepší, objekty spolupracují v modelu spolupráce tam, kde existují přispěvatelé a příjemce.</span><span class="sxs-lookup"><span data-stu-id="c8f56-109">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="c8f56-110">Tento model komunikace samozřejmě generuje závislosti mezi objekty a součásti, aby se aktivovala obtížné spravovat v případě, že zvyšuje složitost.</span><span class="sxs-lookup"><span data-stu-id="c8f56-110">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="c8f56-111">![Třídy závislosti a model složitost](aspnet-mvc-4-dependency-injection/_static/image1.png "třídy závislosti a model složitost")</span><span class="sxs-lookup"><span data-stu-id="c8f56-111">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="c8f56-112">*Třída závislosti a složitost modelu*</span><span class="sxs-lookup"><span data-stu-id="c8f56-112">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="c8f56-113">Pravděpodobně jste slyšeli o **vzor objektu pro vytváření** a oddělení mezi rozhraní a implementace pomocí služeb, kde jsou objekty klienta často zodpovědní za umístění služby.</span><span class="sxs-lookup"><span data-stu-id="c8f56-113">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="c8f56-114">Vzor vkládání závislostí je konkrétní implementace inverzi ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="c8f56-114">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="c8f56-115">**Inverze ovládacího prvku (IoC)** znamená, že objekty není vytvořit jiné objekty, které spoléhají při své práci.</span><span class="sxs-lookup"><span data-stu-id="c8f56-115">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="c8f56-116">Místo toho budou získat objekty, které potřebují z vnějšího zdroje (například konfiguračního souboru xml).</span><span class="sxs-lookup"><span data-stu-id="c8f56-116">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="c8f56-117">**Vkládání závislostí (DI)** znamená, že to je potřeba bez zásahu objekt obvykle součásti framework, který předává parametry konstruktor a nastavte vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c8f56-117">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="c8f56-118">Vzor návrhu závislostí vkládání (DI)</span><span class="sxs-lookup"><span data-stu-id="c8f56-118">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="c8f56-119">Na vysoké úrovni, který je cílem vkládání závislostí třída klienta (například *golfer*) vyžaduje něco, který splňuje rozhraní (například *IClub*).</span><span class="sxs-lookup"><span data-stu-id="c8f56-119">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="c8f56-120">Nebude vědět, co je konkrétní typ (například *WoodClub, IronClub, WedgeClub* nebo *PutterClub*), ho někdo jiný, který chce (například dobrou *caddy*).</span><span class="sxs-lookup"><span data-stu-id="c8f56-120">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="c8f56-121">Překladač závislostí v architektuře ASP.NET MVC lze umožňují registraci logika závislostí někde jinde (třeba kontejner nebo *balík kluby*).</span><span class="sxs-lookup"><span data-stu-id="c8f56-121">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="c8f56-122">![Diagram vkládání závislostí](aspnet-mvc-4-dependency-injection/_static/image2.png "vkládání závislostí obrázku")</span><span class="sxs-lookup"><span data-stu-id="c8f56-122">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="c8f56-123">*Vkládání závislostí - Golf analogie*</span><span class="sxs-lookup"><span data-stu-id="c8f56-123">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="c8f56-124">Výhody používání vzor vkládání závislostí a v inverzi řízení jsou následující:</span><span class="sxs-lookup"><span data-stu-id="c8f56-124">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="c8f56-125">Snižuje párování tříd</span><span class="sxs-lookup"><span data-stu-id="c8f56-125">Reduces class coupling</span></span>
- <span data-ttu-id="c8f56-126">Zvyšuje opětovné použití kódu</span><span class="sxs-lookup"><span data-stu-id="c8f56-126">Increases code reusing</span></span>
- <span data-ttu-id="c8f56-127">Zvyšuje udržovatelnost kódu</span><span class="sxs-lookup"><span data-stu-id="c8f56-127">Improves code maintainability</span></span>
- <span data-ttu-id="c8f56-128">Zlepšuje testování aplikací</span><span class="sxs-lookup"><span data-stu-id="c8f56-128">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="c8f56-129">Vkládání závislostí někdy ve srovnání s abstraktní vzoru návrhu objektu pro vytváření, ale je malého rozdílu mezi obou přístupů.</span><span class="sxs-lookup"><span data-stu-id="c8f56-129">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="c8f56-130">DI má rozhraní práce za k volání objekty pro vytváření a registrované služby vyřešit závislosti.</span><span class="sxs-lookup"><span data-stu-id="c8f56-130">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="c8f56-131">Teď, když znáte vzoru vkládání závislostí, se dozvíte v celém tomto testovacím prostředí jak se má použít v architektuře ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c8f56-131">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="c8f56-132">Se spustí pomocí vkládání závislostí v **řadiče** zahrnout služby přístupu k databázi.</span><span class="sxs-lookup"><span data-stu-id="c8f56-132">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="c8f56-133">V dalším kroku použijete vkládání závislostí **zobrazení** využívat služby a zobrazit informace.</span><span class="sxs-lookup"><span data-stu-id="c8f56-133">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="c8f56-134">Nakonec rozšíříte DI ASP.NET MVC 4 filtrů, vložení vlastní akce filtru v řešení.</span><span class="sxs-lookup"><span data-stu-id="c8f56-134">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="c8f56-135">V tomto testovacím prostředí Hands-on se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="c8f56-135">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="c8f56-136">Integrovat architektury ASP.NET MVC 4 s Unity vkládání závislostí pomocí balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="c8f56-136">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="c8f56-137">Pomocí vkládání závislostí uvnitř řadič rozhraní ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c8f56-137">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="c8f56-138">Pomocí vkládání závislostí uvnitř zobrazení ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c8f56-138">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="c8f56-139">Pomocí vkládání závislostí uvnitř filtr akce rozhraní ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c8f56-139">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="c8f56-140">Toto testovací prostředí používá Unity.Mvc3 balíček NuGet pro řešení závislostí, ale je možné upravit libovolnou architekturu vkládání závislostí pro práci s ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c8f56-140">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c8f56-141">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c8f56-141">Prerequisites</span></span>

<span data-ttu-id="c8f56-142">Musíte mít následující položky k dokončení tohoto testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="c8f56-142">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="c8f56-143">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat).</span><span class="sxs-lookup"><span data-stu-id="c8f56-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c8f56-144">Instalace</span><span class="sxs-lookup"><span data-stu-id="c8f56-144">Setup</span></span>

<span data-ttu-id="c8f56-145">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="c8f56-145">**Installing Code Snippets**</span></span>

<span data-ttu-id="c8f56-146">Pro usnadnění práce většinu kódu, který bude spravovat podél toto testovací prostředí je k dispozici jako fragmenty kódu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8f56-146">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c8f56-147">K instalaci fragmenty kódu spustit **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="c8f56-147">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c8f56-148">Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha B:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c8f56-148">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c8f56-149">Cvičení</span><span class="sxs-lookup"><span data-stu-id="c8f56-149">Exercises</span></span>

<span data-ttu-id="c8f56-150">Toto testovací prostředí Hands-On se skládá ve cvičeních následující:</span><span class="sxs-lookup"><span data-stu-id="c8f56-150">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="c8f56-151">Cvičení 1: Vložení řadič</span><span class="sxs-lookup"><span data-stu-id="c8f56-151">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="c8f56-152">Cvičení 2: Vložení zobrazení</span><span class="sxs-lookup"><span data-stu-id="c8f56-152">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="c8f56-153">Cvičení 3: Vložení filtry</span><span class="sxs-lookup"><span data-stu-id="c8f56-153">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="c8f56-154">Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních.</span><span class="sxs-lookup"><span data-stu-id="c8f56-154">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="c8f56-155">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.</span><span class="sxs-lookup"><span data-stu-id="c8f56-155">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="c8f56-156">Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-156">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="c8f56-157">Cvičení 1: Vložení řadič</span><span class="sxs-lookup"><span data-stu-id="c8f56-157">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="c8f56-158">V tomto cvičení se dozvíte, jak používat vkládání závislostí v Kontrolery architektury ASP.NET MVC integrací Unity pomocí balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="c8f56-158">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="c8f56-159">Z tohoto důvodu bude obsahovat služby do řadičů MvcMusicStore oddělit logiku z přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="c8f56-159">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="c8f56-160">Služby se vytvoří nové závislosti v konstruktoru řadiče, které budou vyřešeny pomocí vkládání závislostí za pomoci **Unity**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-160">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="c8f56-161">Tento přístup vám ukáže, jak vygenerovat méně párované aplikace, které jsou větší flexibilitu a jednodušší pro údržbu a testování.</span><span class="sxs-lookup"><span data-stu-id="c8f56-161">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="c8f56-162">Taky se dozvíte, jak integrovat Unity ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c8f56-162">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="c8f56-163">O StoreManager služby</span><span class="sxs-lookup"><span data-stu-id="c8f56-163">About StoreManager Service</span></span>

<span data-ttu-id="c8f56-164">Úložišti Hudba MVC součástí řešení begin nyní zahrnuje službu, která spravuje data řadič úložiště s názvem **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-164">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="c8f56-165">Níže najdete implementace služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="c8f56-165">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="c8f56-166">Všimněte si, že všechny metody vrácení modelu entit.</span><span class="sxs-lookup"><span data-stu-id="c8f56-166">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="c8f56-167">**StoreController** z začátek řešení teď využívá **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-167">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="c8f56-168">Všechny odkazy na dat byly odebrány z **StoreController**a teď lze změnit aktuální poskytovatel dat přístupu beze změny libovolné metody, která využívá **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-168">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="c8f56-169">Zjistíte, pod kterou **StoreController** implementace má závislosti s **StoreService** uvnitř konstruktoru třídy.</span><span class="sxs-lookup"><span data-stu-id="c8f56-169">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="c8f56-170">Závislost byla zavedená v tomto cvičení souvisí s **inverzi řízení** (IoC).</span><span class="sxs-lookup"><span data-stu-id="c8f56-170">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="c8f56-171">**StoreController** obdrží konstruktoru třídy **IStoreService** parametr typu, který je nezbytné provést volání služby z uvnitř třídy.</span><span class="sxs-lookup"><span data-stu-id="c8f56-171">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="c8f56-172">Ale **StoreController** neimplementuje výchozí konstruktor (s žádné parametry), který musí mít všechny řadič pro práci s architekturou ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c8f56-172">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="c8f56-173">Vyřešit závislost, řadičem musí být vytvořený objekt factory abstraktní (třídu, která vrátí všechny objekt zadaného typu).</span><span class="sxs-lookup"><span data-stu-id="c8f56-173">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="c8f56-174">Když se pokusí vytvořit StoreController bez odeslání objektu služby, protože neexistuje žádný bezparametrový konstruktor deklarovat třídy bude dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="c8f56-174">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="c8f56-175">Úloha 1 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="c8f56-175">Task 1 - Running the Application</span></span>

<span data-ttu-id="c8f56-176">V této úloze budete spouštět aplikaci Begin, které zahrnuje službu do řadiče úložiště, který odděluje přístup k datům z aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="c8f56-176">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="c8f56-177">Při spuštění aplikace, zobrazí se výjimku, jako služba řadiče není jako parametr předaný ve výchozím nastavení:</span><span class="sxs-lookup"><span data-stu-id="c8f56-177">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="c8f56-178">Otevřete **začít** řešení umístěný v **vložení Source\Ex01 Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-178">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

    1. <span data-ttu-id="c8f56-179">Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c8f56-179">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c8f56-180">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-180">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="c8f56-181">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="c8f56-181">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="c8f56-182">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-182">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8f56-183">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-183">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c8f56-184">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-184">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c8f56-185">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8f56-185">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="c8f56-186">Stiskněte klávesu **kombinaci kláves Ctrl + F5** ke spuštění aplikace bez ladění.</span><span class="sxs-lookup"><span data-stu-id="c8f56-186">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="c8f56-187">Zobrazí se chybová zpráva &quot; **žádný konstruktor bez parametrů definované pro tento objekt**&quot;:</span><span class="sxs-lookup"><span data-stu-id="c8f56-187">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="c8f56-188">![Chyba při spouštění aplikace začít ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "Chyba při spouštění aplikace začít ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="c8f56-188">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="c8f56-189">*Chyba při spouštění aplikace začít ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="c8f56-189">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="c8f56-190">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="c8f56-190">Close the browser.</span></span>

<span data-ttu-id="c8f56-191">V následujících krocích budete pracovat v řešení úložiště Hudba vložení závislostí, které tento řadič potřebuje.</span><span class="sxs-lookup"><span data-stu-id="c8f56-191">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="c8f56-192">Úloha 2 – včetně Unity do MvcMusicStore řešení</span><span class="sxs-lookup"><span data-stu-id="c8f56-192">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="c8f56-193">V této úloze bude obsahovat **Unity.Mvc3** balíček NuGet pro řešení.</span><span class="sxs-lookup"><span data-stu-id="c8f56-193">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="c8f56-194">Balíček Unity.Mvc3 byl navržený pro ASP.NET MVC 3, ale je plně kompatibilní s ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c8f56-194">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="c8f56-195">Unity je kontejner vkládání závislostí lightweight, extensible s volitelnou podporu pro instanci a zadejte zachycení.</span><span class="sxs-lookup"><span data-stu-id="c8f56-195">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="c8f56-196">Je kontejner pro obecné účely pro použití v jakéhokoli typu aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="c8f56-196">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="c8f56-197">Poskytuje společné funkce najít v mechanismy vkládání závislostí, včetně: vytvoření objektu, abstrakce požadavky zadáním závislosti na modulu runtime a flexibilitu, rozlišením konfigurace komponent do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c8f56-197">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="c8f56-198">Nainstalujte **Unity.Mvc3** balíček NuGet v **MvcMusicStore** projektu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-198">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="c8f56-199">Chcete-li to provést, otevřete **Konzola správce balíčků** z **zobrazení** | **ostatní okna**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-199">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="c8f56-200">Spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="c8f56-200">Run the following command.</span></span>

    <span data-ttu-id="c8f56-201">POMOCÍ PMC</span><span class="sxs-lookup"><span data-stu-id="c8f56-201">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="c8f56-202">![Instalace balíčku NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "instalace balíčku NuGet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="c8f56-202">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="c8f56-203">*Instalace balíčku NuGet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="c8f56-203">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="c8f56-204">Jednou **Unity.Mvc3** balíček nainstalován, prozkoumejte soubory a složky, se automaticky přidá, aby bylo možné zjednodušit konfiguraci Unity.</span><span class="sxs-lookup"><span data-stu-id="c8f56-204">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="c8f56-205">![Unity.Mvc3 balíček nainstalován](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 balíček nainstalován")</span><span class="sxs-lookup"><span data-stu-id="c8f56-205">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="c8f56-206">*Unity.Mvc3 balíček nainstalován*</span><span class="sxs-lookup"><span data-stu-id="c8f56-206">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="c8f56-207">Úloha 3 – registrace Unity v aplikaci Global.asax.cs\_Start</span><span class="sxs-lookup"><span data-stu-id="c8f56-207">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="c8f56-208">V této úloze budete aktualizovat **aplikace\_spustit** metoda umístěný v **Global.asax.cs** volání inicializátoru Unity zaváděcího nástroje a potom aktualizovat registraci soubor zaváděcího nástroje Služba a řadič bude používat pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="c8f56-208">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="c8f56-209">Teď můžete spojit zaváděcí nástroj, který je soubor, který inicializuje kontejneru Unity a překladače závislostí.</span><span class="sxs-lookup"><span data-stu-id="c8f56-209">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="c8f56-210">Chcete-li to provést, otevřete **Global.asax.cs** a přidejte následující zvýrazněný kód v rámci **aplikace\_spustit** metoda.</span><span class="sxs-lookup"><span data-stu-id="c8f56-210">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="c8f56-211">(Code fragment kódu - *testovacím vkládání závislostí ASP.NET - Ex01 - inicializovat Unity*)</span><span class="sxs-lookup"><span data-stu-id="c8f56-211">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="c8f56-212">Otevřete **Bootstrapper.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="c8f56-212">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="c8f56-213">Zahrnout následujících oborů názvů: **MvcMusicStore.Services** a **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-213">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="c8f56-214">(Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex01 - zaváděcího nástroje Přidání obory názvů*)</span><span class="sxs-lookup"><span data-stu-id="c8f56-214">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="c8f56-215">Nahraďte **BuildUnityContainer** metoda je obsah s následující kód, který registruje řadič úložiště a služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="c8f56-215">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="c8f56-216">(Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex01 – registrace úložiště řadič a služby*)</span><span class="sxs-lookup"><span data-stu-id="c8f56-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="c8f56-217">Úloha 4 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="c8f56-217">Task 4 - Running the Application</span></span>

<span data-ttu-id="c8f56-218">V této úloze budete spouštět aplikace k ověření, že je lze načíst teď po včetně Unity.</span><span class="sxs-lookup"><span data-stu-id="c8f56-218">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="c8f56-219">Stiskněte klávesu **F5** ke spuštění aplikace, aplikace by se měly načíst teď bez zobrazení všech chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="c8f56-219">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="c8f56-220">![Aplikace s vkládání závislostí](aspnet-mvc-4-dependency-injection/_static/image6.png "s vkládání závislostí aplikací")</span><span class="sxs-lookup"><span data-stu-id="c8f56-220">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="c8f56-221">*Aplikace s vkládání závislostí*</span><span class="sxs-lookup"><span data-stu-id="c8f56-221">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="c8f56-222">Přejděte do **/úložiště**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-222">Browse to **/Store**.</span></span> <span data-ttu-id="c8f56-223">To bude vyvolání **StoreController**, který je teď vytvořený pomocí **Unity**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-223">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="c8f56-224">![Úložiště Hudba MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Hudba úložiště")</span><span class="sxs-lookup"><span data-stu-id="c8f56-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="c8f56-225">*Úložiště Hudba MVC*</span><span class="sxs-lookup"><span data-stu-id="c8f56-225">*MVC Music Store*</span></span>
3. <span data-ttu-id="c8f56-226">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="c8f56-226">Close the browser.</span></span>

<span data-ttu-id="c8f56-227">V následujícím cvičení se dozvíte, jak rozšířit oboru vkládání závislostí pro použití v zobrazení ASP.NET MVC a filtrů akce.</span><span class="sxs-lookup"><span data-stu-id="c8f56-227">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="c8f56-228">Cvičení 2: Vložení zobrazení</span><span class="sxs-lookup"><span data-stu-id="c8f56-228">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="c8f56-229">V tomto cvičení se dozvíte, jak používat vkládání závislostí v zobrazení s nových funkcí technologie ASP.NET MVC 4 pro integraci Unity.</span><span class="sxs-lookup"><span data-stu-id="c8f56-229">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="c8f56-230">Aby bylo možné provést, bude volat vlastní služba uvnitř zobrazení procházet úložiště, které se zobrazí zprávu a bitovou kopii níže.</span><span class="sxs-lookup"><span data-stu-id="c8f56-230">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="c8f56-231">Potom bude projekt integrovat Unity a vytvořte překladač závislostí Vlastní vložení závislosti.</span><span class="sxs-lookup"><span data-stu-id="c8f56-231">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="c8f56-232">Úloha 1 – Vytvoření zobrazení, který využívá službu</span><span class="sxs-lookup"><span data-stu-id="c8f56-232">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="c8f56-233">V této úloze vytvoříte zobrazení, která provádí volání služby generovat nové závislosti.</span><span class="sxs-lookup"><span data-stu-id="c8f56-233">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="c8f56-234">V jednoduchých službou zasílání zpráv zahrnuté v tomto řešení se skládá službu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-234">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="c8f56-235">Otevřete **začít** řešení umístěný v **vložení Source\Ex02 View\Begin** složky.</span><span class="sxs-lookup"><span data-stu-id="c8f56-235">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="c8f56-236">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="c8f56-236">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="c8f56-237">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c8f56-237">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c8f56-238">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-238">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="c8f56-239">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="c8f56-239">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="c8f56-240">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-240">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8f56-241">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-241">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c8f56-242">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-242">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c8f56-243">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8f56-243">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="c8f56-244">Další informace najdete v tomto článku: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="c8f56-244">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="c8f56-245">Zahrnout **MessageService.cs** a **IMessageService.cs** třídy umístěny ve **zdroje \Assets** složky v **/služeb**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-245">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="c8f56-246">Chcete-li to provést, klikněte pravým tlačítkem **služby** složky a vyberte **přidat existující položku**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-246">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="c8f56-247">Přejděte do umístění tyto soubory a zahrnout je.</span><span class="sxs-lookup"><span data-stu-id="c8f56-247">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="c8f56-248">![Přidání služba zpráv a rozhraní služby](aspnet-mvc-4-dependency-injection/_static/image8.png "přidání služba zpráv a rozhraní služby")</span><span class="sxs-lookup"><span data-stu-id="c8f56-248">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="c8f56-249">*Přidání služba zpráv a rozhraní služby*</span><span class="sxs-lookup"><span data-stu-id="c8f56-249">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8f56-250">**IMessageService** rozhraní definuje dvě vlastnosti implementované **MessageService** třídy.</span><span class="sxs-lookup"><span data-stu-id="c8f56-250">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="c8f56-251">Tyto vlastnosti -**zpráva** a **ImageUrl**-ukládat zprávy a adresa URL obrázku, který se má zobrazit.</span><span class="sxs-lookup"><span data-stu-id="c8f56-251">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="c8f56-252">Vytvořit složku **/stránky** v projektu kořenová složka a poté přidejte existující třída **MyBasePage.cs** z **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-252">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="c8f56-253">Základní stránka, kterou bude dědit ze má následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-253">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="c8f56-254">![Složka stránek](aspnet-mvc-4-dependency-injection/_static/image9.png "složky stránky")</span><span class="sxs-lookup"><span data-stu-id="c8f56-254">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="c8f56-255">Otevřete **Browse.cshtml** zobrazit z **/zobrazení/úložiště** složky a nastavit jej dědí **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-255">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="c8f56-256">V **Procházet** zobrazit, že přidáte volání **MessageService** zobrazíte bitovou kopii a zprávu načíst službou.</span><span class="sxs-lookup"><span data-stu-id="c8f56-256">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
<span data-ttu-id="c8f56-257">(C#)</span><span class="sxs-lookup"><span data-stu-id="c8f56-257">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="c8f56-258">Úloha 2 – včetně překladač závislostí vlastní a aktivátoru stránky vlastní zobrazení</span><span class="sxs-lookup"><span data-stu-id="c8f56-258">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="c8f56-259">V předchozí úloze vložit nové závislosti uvnitř zobrazení volání služby uvnitř ho provést.</span><span class="sxs-lookup"><span data-stu-id="c8f56-259">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="c8f56-260">Nyní, bude vyřešení této závislosti implementací rozhraní vkládání závislostí ASP.NET MVC **IViewPageActivator** a **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-260">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="c8f56-261">V řešení bude obsahovat implementace **IDependencyResolver** který se bude zabývat načtení služby pomocí Unity.</span><span class="sxs-lookup"><span data-stu-id="c8f56-261">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="c8f56-262">Pak bude obsahovat jiné vlastní implementaci **IViewPageActivator** rozhraní, které se řešení s vytvořením zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c8f56-262">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="c8f56-263">Od verze ASP.NET MVC 3 měl implementaci pro vkládání závislostí zjednodušené rozhraní k registraci služeb.</span><span class="sxs-lookup"><span data-stu-id="c8f56-263">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="c8f56-264">**IDependencyResolver** a **IViewPageActivator** jsou součástí ASP.NET MVC 3 funkcí pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="c8f56-264">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="c8f56-265">**-IDependencyResolver** rozhraní nahrazuje předchozí IMvcServiceLocator.</span><span class="sxs-lookup"><span data-stu-id="c8f56-265">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="c8f56-266">Implementátory IDependencyResolver musí vrátit instanci služby nebo služby kolekce.</span><span class="sxs-lookup"><span data-stu-id="c8f56-266">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="c8f56-267">**-IViewPageActivator** rozhraní poskytuje jemně odstupňovanou kontrolu nad jak instance stránky zobrazení pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="c8f56-267">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="c8f56-268">Třídy, které implementují **IViewPageActivator** rozhraní můžete vytvořit zobrazení instancí pomocí informace o kontextu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-268">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="c8f56-269">Vytvořte nebo**objekty Factory** složky v kořenové složce projektu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-269">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="c8f56-270">Zahrnout **CustomViewPageActivator.cs** do řešení z **/zdroje/prostředky/** k **objekty Factory** složky.</span><span class="sxs-lookup"><span data-stu-id="c8f56-270">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="c8f56-271">Chcete-li provést, klikněte pravým tlačítkem **/Factories** složky, vyberte **přidat | Existující položka** a pak vyberte **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-271">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="c8f56-272">Tato třída implementuje **IViewPageActivator** rozhraní pro uložení kontejneru Unity.</span><span class="sxs-lookup"><span data-stu-id="c8f56-272">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="c8f56-273">**CustomViewPageActivator** je zodpovědný za správu vytvoření zobrazení pomocí kontejner Unity.</span><span class="sxs-lookup"><span data-stu-id="c8f56-273">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="c8f56-274">Zahrnout **UnityDependencyResolver.cs** souboru z **/zdroje/prostředky** k **/Factories** složky.</span><span class="sxs-lookup"><span data-stu-id="c8f56-274">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="c8f56-275">Chcete-li provést, klikněte pravým tlačítkem **/Factories** složky, vyberte **přidat | Existující položka** a pak vyberte **UnityDependencyResolver.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="c8f56-275">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="c8f56-276">**UnityDependencyResolver** třída je vlastní překladače závislostí pro Unity.</span><span class="sxs-lookup"><span data-stu-id="c8f56-276">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="c8f56-277">Pokud služba nebyla nalezena uvnitř kontejneru Unity, je invocated základní překladač.</span><span class="sxs-lookup"><span data-stu-id="c8f56-277">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="c8f56-278">V následující úlohu obou implementace registrována umožníte modelu vědět, umístění služeb a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c8f56-278">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="c8f56-279">Úloha 3 – registrace pro vkládání závislostí v rámci kontejneru Unity</span><span class="sxs-lookup"><span data-stu-id="c8f56-279">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="c8f56-280">V této úloze umístíte všechny předchozí věci a vytvořit tak pracovat vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="c8f56-280">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="c8f56-281">Až teď vaše řešení obsahuje následující prvky:</span><span class="sxs-lookup"><span data-stu-id="c8f56-281">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="c8f56-282">A **Procházet** zobrazení, která dědí z **MyBaseClass** a odebírá **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-282">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="c8f56-283">Třídu zprostředkující -**MyBaseClass**-má deklarovat pro rozhraní služby vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="c8f56-283">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="c8f56-284">Může služba - **MessageService** - a jeho rozhraní **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-284">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="c8f56-285">Překladač závislostí vlastní for Unity - **UnityDependencyResolver** – který se zabývá načítání služby.</span><span class="sxs-lookup"><span data-stu-id="c8f56-285">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="c8f56-286">Aktivátor stránky zobrazení - **CustomViewPageActivator** -vytvářející stránky.</span><span class="sxs-lookup"><span data-stu-id="c8f56-286">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="c8f56-287">Vložení **Procházet** zobrazení, se nyní zaregistrujete překladač závislostí vlastní v kontejneru Unity.</span><span class="sxs-lookup"><span data-stu-id="c8f56-287">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="c8f56-288">Otevřete **Bootstrapper.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="c8f56-288">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="c8f56-289">Registrace instance **MessageService** do kontejneru Unity k chybě při inicializaci služby:</span><span class="sxs-lookup"><span data-stu-id="c8f56-289">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="c8f56-290">(Code fragment kódu - *služba ASP.NET závislostí vkládání laboratoř - Ex02 – registrace zpráv*)</span><span class="sxs-lookup"><span data-stu-id="c8f56-290">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="c8f56-291">Přidat odkaz na **MvcMusicStore.Factories** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="c8f56-291">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="c8f56-292">(Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex02 - továrny Namespace*)</span><span class="sxs-lookup"><span data-stu-id="c8f56-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="c8f56-293">Zaregistrovat **CustomViewPageActivator** jako aktivátoru stránky zobrazení do kontejneru Unity:</span><span class="sxs-lookup"><span data-stu-id="c8f56-293">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="c8f56-294">(Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex02 – registrace CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="c8f56-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="c8f56-295">Nahraďte překladač závislostí ASP.NET MVC 4 výchozí instanci **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-295">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="c8f56-296">Chcete-li to provést, nahraďte **inicializační** metoda obsahu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c8f56-296">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="c8f56-297">(Code fragment kódu - *překladač závislostí ASP.NET závislostí vkládání laboratoř - Ex02 - aktualizace*)</span><span class="sxs-lookup"><span data-stu-id="c8f56-297">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="c8f56-298">ASP.NET MVC poskytuje výchozí třídu překladače závislostí.</span><span class="sxs-lookup"><span data-stu-id="c8f56-298">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="c8f56-299">Pro práci s překladače závislostí vlastní jako ten, který jsme vytvořili pro unity, má tento překladač vyměnit.</span><span class="sxs-lookup"><span data-stu-id="c8f56-299">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="c8f56-300">Úloha 4 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="c8f56-300">Task 4 - Running the Application</span></span>

<span data-ttu-id="c8f56-301">V této úloze budete spouštět aplikaci ověřte, zda prohlížeč úložiště využívá službu a ukazuje bitovou kopii a načíst zprávu:</span><span class="sxs-lookup"><span data-stu-id="c8f56-301">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="c8f56-302">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8f56-302">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c8f56-303">Klikněte na tlačítko **Rock** v nabídce žánry a najdete v tématu Jak **MessageService** byl aplikován na zobrazení a načíst zobrazení uvítací zprávy a bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="c8f56-303">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="c8f56-304">V tomto příkladu jsme k zadávání &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="c8f56-304">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="c8f56-305">![Úložiště Hudba MVC – zobrazení vkládání](aspnet-mvc-4-dependency-injection/_static/image10.png "úložiště Hudba MVC – zobrazení vkládání")</span><span class="sxs-lookup"><span data-stu-id="c8f56-305">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="c8f56-306">*Úložiště Hudba MVC – zobrazení vkládání*</span><span class="sxs-lookup"><span data-stu-id="c8f56-306">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="c8f56-307">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="c8f56-307">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="c8f56-308">Cvičení 3: Vložení filtrů Akce</span><span class="sxs-lookup"><span data-stu-id="c8f56-308">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="c8f56-309">V testovacím prostředí předchozí praktických **vlastní filtry akce** jste pracovali s filtry přizpůsobení a vkládání.</span><span class="sxs-lookup"><span data-stu-id="c8f56-309">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="c8f56-310">V tomto cvičení se dozvíte, jak se pomocí kontejneru Unity vložit filtry pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="c8f56-310">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="c8f56-311">K tomu, přidáte do řešení úložiště Hudba vlastní akce filtr, který bude trasování aktivity lokality.</span><span class="sxs-lookup"><span data-stu-id="c8f56-311">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="c8f56-312">Úloha 1 – včetně sledování filtru v řešení</span><span class="sxs-lookup"><span data-stu-id="c8f56-312">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="c8f56-313">V této úloze bude v úložišti Hudba obsahovat vlastní akce filtru k události trasování.</span><span class="sxs-lookup"><span data-stu-id="c8f56-313">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="c8f56-314">Jako vlastní akce filtru koncepty již zpracovány v testovacím prostředí předchozí &quot;vlastní filtry akce&quot;, bude jenom obsahují třídu filtru ve složce prostředky tohoto testovacího prostředí a pak vytvořte zprostředkovatele filtrů pro Unity:</span><span class="sxs-lookup"><span data-stu-id="c8f56-314">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="c8f56-315">Otevřete **začít** řešení umístěný v **Source\Ex03 - vložení Filter\Begin akce** složky.</span><span class="sxs-lookup"><span data-stu-id="c8f56-315">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="c8f56-316">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="c8f56-316">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="c8f56-317">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c8f56-317">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c8f56-318">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-318">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="c8f56-319">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="c8f56-319">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="c8f56-320">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-320">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8f56-321">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-321">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c8f56-322">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-322">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c8f56-323">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8f56-323">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="c8f56-324">Další informace najdete v tomto článku: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="c8f56-324">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="c8f56-325">Zahrnout **TraceActionFilter.cs** souboru z **/zdroje/prostředky** k **/filtry** složky.</span><span class="sxs-lookup"><span data-stu-id="c8f56-325">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="c8f56-326">Tento filtr vlastní akce se provádí trasování rozhraní ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c8f56-326">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="c8f56-327">Můžete zkontrolovat &quot;místní ASP.NET MVC 4 a dynamické filtry akce&quot; testovacího prostředí pro odkaz na další.</span><span class="sxs-lookup"><span data-stu-id="c8f56-327">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="c8f56-328">Přidání prázdné třídy **FilterProvider.cs** na projekt ve složce   **/filtry.**</span><span class="sxs-lookup"><span data-stu-id="c8f56-328">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="c8f56-329">Přidat **System.Web.Mvc** a **Microsoft.Practices.Unity** obory názvů v **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-329">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="c8f56-330">(Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex03 - zprostředkovatele filtrů přidání obory názvů*)</span><span class="sxs-lookup"><span data-stu-id="c8f56-330">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="c8f56-331">Ujistěte se, dědí z třídy **IFilterProvider** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c8f56-331">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="c8f56-332">Přidat **IUnityContainer** vlastnost **FilterProvider** třídy a pak vytvořte konstruktoru třídy přiřadit kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c8f56-332">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="c8f56-333">(Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex03 - filtru zprostředkovatele konstruktor*)</span><span class="sxs-lookup"><span data-stu-id="c8f56-333">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="c8f56-334">Konstruktoru třídy zprostředkovatele filtru není vytváření **nové** objektu uvnitř.</span><span class="sxs-lookup"><span data-stu-id="c8f56-334">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="c8f56-335">Kontejner je předán jako parametr a závislost je vyřešeno Unity.</span><span class="sxs-lookup"><span data-stu-id="c8f56-335">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="c8f56-336">V **FilterProvider** třídy, implementovat metodu **GetFilters** z **IFilterProvider** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c8f56-336">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="c8f56-337">(Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex03 - filtru zprostředkovatele GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="c8f56-337">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="c8f56-338">Úloha 2 – registrace a povolení filtru</span><span class="sxs-lookup"><span data-stu-id="c8f56-338">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="c8f56-339">V této úloze povolíte sledování lokality.</span><span class="sxs-lookup"><span data-stu-id="c8f56-339">In this task, you will enable site tracking.</span></span> <span data-ttu-id="c8f56-340">Kvůli tomu zaregistrujete filtru v **Bootstrapper.cs BuildUnityContainer** metoda spusťte trasování:</span><span class="sxs-lookup"><span data-stu-id="c8f56-340">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="c8f56-341">Otevřete **Web.config** nachází v kořenu projektu a povolit trasování sledování na skupinu System.Web.</span><span class="sxs-lookup"><span data-stu-id="c8f56-341">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="c8f56-342">Otevřete **Bootstrapper.cs** v kořenu projektu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-342">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="c8f56-343">Přidat odkaz na **MvcMusicStore.Filters** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="c8f56-343">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="c8f56-344">(Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex03 - zaváděcího nástroje Přidání obory názvů*)</span><span class="sxs-lookup"><span data-stu-id="c8f56-344">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="c8f56-345">Vyberte **BuildUnityContainer** metoda a zaregistrujte filtru v kontejneru Unity.</span><span class="sxs-lookup"><span data-stu-id="c8f56-345">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="c8f56-346">Budete mít k registraci zprostředkovatele filtrů, jakož i filtr akce.</span><span class="sxs-lookup"><span data-stu-id="c8f56-346">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="c8f56-347">(Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex03 – registrace FilterProvider a ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="c8f56-347">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="c8f56-348">Úloha 3 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="c8f56-348">Task 3 - Running the Application</span></span>

<span data-ttu-id="c8f56-349">V této úloze bude aplikaci spustit a otestovat, zda filtr vlastní akce je trasování aktivity:</span><span class="sxs-lookup"><span data-stu-id="c8f56-349">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="c8f56-350">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8f56-350">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c8f56-351">Klikněte na tlačítko **Rock** v nabídce žánry.</span><span class="sxs-lookup"><span data-stu-id="c8f56-351">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="c8f56-352">Pokud chcete, můžete procházet na další žánry.</span><span class="sxs-lookup"><span data-stu-id="c8f56-352">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="c8f56-353">![Hudba úložiště](aspnet-mvc-4-dependency-injection/_static/image11.png "Hudba úložiště")</span><span class="sxs-lookup"><span data-stu-id="c8f56-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="c8f56-354">*Hudba úložiště*</span><span class="sxs-lookup"><span data-stu-id="c8f56-354">*Music Store*</span></span>
3. <span data-ttu-id="c8f56-355">Přejděte do **/Trace.axd** zobrazíte trasování aplikací a pak klikněte na tlačítko **zobrazit podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-355">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="c8f56-356">![Protokol trasování aplikace](aspnet-mvc-4-dependency-injection/_static/image12.png "protokolu trasování aplikace")</span><span class="sxs-lookup"><span data-stu-id="c8f56-356">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="c8f56-357">*Trasování protokolu aplikace*</span><span class="sxs-lookup"><span data-stu-id="c8f56-357">*Application Trace Log*</span></span>

    <span data-ttu-id="c8f56-358">![Trasování aplikací - podrobnosti požadavku](aspnet-mvc-4-dependency-injection/_static/image13.png "trasování aplikací - podrobnosti požadavku")</span><span class="sxs-lookup"><span data-stu-id="c8f56-358">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="c8f56-359">*Trasování aplikací - podrobnosti požadavku*</span><span class="sxs-lookup"><span data-stu-id="c8f56-359">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="c8f56-360">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="c8f56-360">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c8f56-361">Souhrn</span><span class="sxs-lookup"><span data-stu-id="c8f56-361">Summary</span></span>

<span data-ttu-id="c8f56-362">Provedením tohoto testovacího prostředí Hands-On jste se naučili, jak pomocí vkládání závislostí v architektuře ASP.NET MVC 4 integrací Unity pomocí balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="c8f56-362">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="c8f56-363">K dosažení, která se mají použít vkládání závislostí uvnitř řadiče, zobrazení a filtrů akce.</span><span class="sxs-lookup"><span data-stu-id="c8f56-363">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="c8f56-364">Následující koncepty byly zahrnuté:</span><span class="sxs-lookup"><span data-stu-id="c8f56-364">The following concepts were covered:</span></span>

- <span data-ttu-id="c8f56-365">Funkce vkládání závislostí aplikace ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="c8f56-365">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="c8f56-366">Pomocí balíčku NuGet Unity.Mvc3 integrace Unity</span><span class="sxs-lookup"><span data-stu-id="c8f56-366">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="c8f56-367">Vkládání závislostí v řadiče</span><span class="sxs-lookup"><span data-stu-id="c8f56-367">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="c8f56-368">Vkládání závislostí v zobrazeních.</span><span class="sxs-lookup"><span data-stu-id="c8f56-368">Dependency Injection in Views</span></span>
- <span data-ttu-id="c8f56-369">Vkládání závislostí filtrů Akce</span><span class="sxs-lookup"><span data-stu-id="c8f56-369">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c8f56-370">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="c8f56-370">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c8f56-371">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze  **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="c8f56-371">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c8f56-372">Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="c8f56-372">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c8f56-373">Přejděte na [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="c8f56-373">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c8f56-374">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; *Visual Studio Express 2012 pro Web se sadou Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="c8f56-374">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="c8f56-375">Klikněte na **nyní nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-375">Click on **Install Now**.</span></span> <span data-ttu-id="c8f56-376">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="c8f56-376">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c8f56-377">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="c8f56-377">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c8f56-378">![Nainstalovat Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "nainstalovat Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c8f56-378">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c8f56-379">*Nainstalovat Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="c8f56-379">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c8f56-380">Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="c8f56-380">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Vyjádření souhlasu s podmínkami licence](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="c8f56-382">*Vyjádření souhlasu s podmínkami licence*</span><span class="sxs-lookup"><span data-stu-id="c8f56-382">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c8f56-383">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="c8f56-383">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="c8f56-385">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="c8f56-385">*Installation progress*</span></span>
6. <span data-ttu-id="c8f56-386">Po dokončení instalace, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-386">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="c8f56-388">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="c8f56-388">*Installation completed*</span></span>
7. <span data-ttu-id="c8f56-389">Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.</span><span class="sxs-lookup"><span data-stu-id="c8f56-389">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c8f56-390">Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="c8f56-390">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pro Web dlaždice](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="c8f56-392">*VS Express pro Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="c8f56-392">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="c8f56-393">Příloha B: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="c8f56-393">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="c8f56-394">S fragmenty kódu máte všechny kód, který je nutné na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="c8f56-394">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c8f56-395">Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="c8f56-395">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c8f56-396">![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-dependency-injection/_static/image19.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="c8f56-396">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="c8f56-397">*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="c8f56-397">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="c8f56-398">***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***</span><span class="sxs-lookup"><span data-stu-id="c8f56-398">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="c8f56-399">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="c8f56-399">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c8f56-400">Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).</span><span class="sxs-lookup"><span data-stu-id="c8f56-400">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c8f56-401">Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.</span><span class="sxs-lookup"><span data-stu-id="c8f56-401">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c8f56-402">Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).</span><span class="sxs-lookup"><span data-stu-id="c8f56-402">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c8f56-403">Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="c8f56-403">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="c8f56-404">![Začněte psát název fragmentu](aspnet-mvc-4-dependency-injection/_static/image20.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="c8f56-404">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="c8f56-405">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="c8f56-405">*Start typing the snippet name*</span></span>

<span data-ttu-id="c8f56-406">![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](aspnet-mvc-4-dependency-injection/_static/image21.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="c8f56-406">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="c8f56-407">*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="c8f56-407">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="c8f56-408">![Stisknutím klávesy Tab znovu a fragmentu rozšíří](aspnet-mvc-4-dependency-injection/_static/image22.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")</span><span class="sxs-lookup"><span data-stu-id="c8f56-408">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="c8f56-409">*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*</span><span class="sxs-lookup"><span data-stu-id="c8f56-409">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="c8f56-410">***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="c8f56-410">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="c8f56-411">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="c8f56-411">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="c8f56-412">Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-412">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="c8f56-413">Vyberte relevantní fragment kódu ze seznamu, kliknutím na.</span><span class="sxs-lookup"><span data-stu-id="c8f56-413">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="c8f56-414">![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-dependency-injection/_static/image23.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="c8f56-414">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="c8f56-415">*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="c8f56-415">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="c8f56-416">![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-dependency-injection/_static/image24.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="c8f56-416">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="c8f56-417">*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="c8f56-417">*Pick the relevant snippet from the list, by clicking on it*</span></span>
