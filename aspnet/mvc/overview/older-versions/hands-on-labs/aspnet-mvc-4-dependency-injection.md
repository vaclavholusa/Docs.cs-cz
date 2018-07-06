---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Injektáž závislostí architektury ASP.NET MVC 4 | Dokumentace Microsoftu
author: rick-anderson
description: 'Poznámka: Tohoto praktického testovacího prostředí se předpokládá, že máte základní znalosti ASP.NET MVC a ASP.NET MVC 4 filtry. Pokud jste nepoužili ASP.NET MVC 4 filtry před jsme rec...'
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 44c8f2055fb62d589e874683cbf43eed87a8c447
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812338"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="b460c-104">Injektáž závislostí architektury ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b460c-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="b460c-105">podle [Campy Web týmu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b460c-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="b460c-106">Stáhněte si Web Campy školení Kit</span><span class="sxs-lookup"><span data-stu-id="b460c-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="b460c-107">Tohoto praktického testovacího prostředí předpokládá, že máte základní znalosti o **ASP.NET MVC** a **ASP.NET MVC 4 filtruje**.</span><span class="sxs-lookup"><span data-stu-id="b460c-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="b460c-108">Pokud jste ještě nepoužívali **ASP.NET MVC 4 filtry** před, doporučujeme si projít **ASP.NET MVC vlastní akce filtry** praktického testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b460c-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="b460c-109">Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="b460c-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="b460c-110">Projekt specifické pro toto testovací prostředí je k dispozici na [injektáž závislostí aplikace ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="b460c-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="b460c-111">V **orientované programování objekt** paradigma, objekty spolu fungují v modelu spolupráce tam, kde jsou přispěvatelé a spotřebitele.</span><span class="sxs-lookup"><span data-stu-id="b460c-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="b460c-112">Tento komunikační model přirozeně, generuje závislosti mezi objekty a jejich komponent, stávají obtížné spravovat, pokud se zvyšuje složitost.</span><span class="sxs-lookup"><span data-stu-id="b460c-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="b460c-113">![Třída závislosti a model složitost](aspnet-mvc-4-dependency-injection/_static/image1.png "třídy závislosti a složitosti modelu")</span><span class="sxs-lookup"><span data-stu-id="b460c-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="b460c-114">*Třída závislostí a složitosti modelu*</span><span class="sxs-lookup"><span data-stu-id="b460c-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="b460c-115">Pravděpodobně jste slyšeli o **Factory vzor** a oddělení mezi rozhraní a implementaci služby, ve kterém jsou objekty klienta často za umístění služeb.</span><span class="sxs-lookup"><span data-stu-id="b460c-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="b460c-116">Injektáž závislostí vzor je konkrétní implementaci ovládacího prvku inverzi.</span><span class="sxs-lookup"><span data-stu-id="b460c-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="b460c-117">**IOC (Inversion) ovládacího prvku** znamená, že objekty ne vytvořit další objekty, na kterých jsou závislé ke své práci.</span><span class="sxs-lookup"><span data-stu-id="b460c-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="b460c-118">Vývojáři získají objekty, které potřebují z vnějšího zdroje (například konfigurační soubor xml).</span><span class="sxs-lookup"><span data-stu-id="b460c-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="b460c-119">**Dependency Injection (DI)** znamená, že to se provádí bez zásahu objekt obvykle komponentou architektury, které předává parametry konstruktoru a nastavte vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b460c-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="b460c-120">Návrhový vzor závislostí vkládání (DI)</span><span class="sxs-lookup"><span data-stu-id="b460c-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="b460c-121">Na vysoké úrovni, který je cílem injektáž závislostí třída klienta (například *golfer*) potřebuje něco, co splňuje rozhraní (třeba *IClub*).</span><span class="sxs-lookup"><span data-stu-id="b460c-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="b460c-122">Není vás, co je konkrétní typ (například *WedgeClub WoodClub IronClub,* nebo *PutterClub*), někdo jiný, který chce (například dobré *caddy*).</span><span class="sxs-lookup"><span data-stu-id="b460c-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="b460c-123">Překladač závislostí v architektuře ASP.NET MVC můžete povolit registraci logiky závislost někde jinde (třeba kontejner nebo *kontejner kluby*).</span><span class="sxs-lookup"><span data-stu-id="b460c-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="b460c-124">![Diagram injektáž závislostí](aspnet-mvc-4-dependency-injection/_static/image2.png "obrázek injektáž závislostí")</span><span class="sxs-lookup"><span data-stu-id="b460c-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="b460c-125">*Injektáž závislostí - přirovnání Golf*</span><span class="sxs-lookup"><span data-stu-id="b460c-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="b460c-126">Výhody použití vzoru injektáž závislostí a IOC ovládacího prvku jsou následující:</span><span class="sxs-lookup"><span data-stu-id="b460c-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="b460c-127">Snižuje párování tříd</span><span class="sxs-lookup"><span data-stu-id="b460c-127">Reduces class coupling</span></span>
- <span data-ttu-id="b460c-128">Zvyšuje opětovné použití kódu</span><span class="sxs-lookup"><span data-stu-id="b460c-128">Increases code reusing</span></span>
- <span data-ttu-id="b460c-129">Zlepšuje udržovatelnosti kódu</span><span class="sxs-lookup"><span data-stu-id="b460c-129">Improves code maintainability</span></span>
- <span data-ttu-id="b460c-130">Zlepšuje testování aplikace</span><span class="sxs-lookup"><span data-stu-id="b460c-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="b460c-131">Injektáž závislostí někdy ve srovnání s abstraktní Factory návrhový vzor, ale není lehké rozdíl mezi oba přístupy.</span><span class="sxs-lookup"><span data-stu-id="b460c-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="b460c-132">DI má rámec práce za vyřešit závislosti voláním továren a registrovaných služeb.</span><span class="sxs-lookup"><span data-stu-id="b460c-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="b460c-133">Teď, když rozumíte vzor injektáž závislostí, se naučíte v celém tomto testovacím prostředí použít v architektuře ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b460c-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="b460c-134">Se spustí pomocí vkládání závislostí v **řadiče** zahrnout databázová služba.</span><span class="sxs-lookup"><span data-stu-id="b460c-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="b460c-135">V dalším kroku použijete injektáž závislostí do **zobrazení** využívání služby a zobrazit informace.</span><span class="sxs-lookup"><span data-stu-id="b460c-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="b460c-136">Nakonec rozšíříte DI na ASP.NET MVC 4 filtry, vkládání filtr vlastních akcí v řešení.</span><span class="sxs-lookup"><span data-stu-id="b460c-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="b460c-137">V tomto praktického testovacího prostředí se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="b460c-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="b460c-138">Integrace architektury ASP.NET MVC 4 s Unity pro vkládání závislostí pomocí balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="b460c-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="b460c-139">Pomocí vkládání závislostí uvnitř kontroler ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b460c-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="b460c-140">Pomocí vkládání závislostí uvnitř zobrazení ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b460c-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="b460c-141">Pomocí vkládání závislostí uvnitř filtr akce s ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b460c-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="b460c-142">Toto testovací prostředí používá Unity.Mvc3 balíček NuGet pro řešení závislostí, ale je možné přizpůsobit libovolné architektury injektáž závislostí pro práci s ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b460c-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b460c-143">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b460c-143">Prerequisites</span></span>

<span data-ttu-id="b460c-144">Musíte mít následující položky k dokončení tohoto testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="b460c-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b460c-145">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny k jeho instalaci).</span><span class="sxs-lookup"><span data-stu-id="b460c-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b460c-146">Instalace</span><span class="sxs-lookup"><span data-stu-id="b460c-146">Setup</span></span>

<span data-ttu-id="b460c-147">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="b460c-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="b460c-148">Pro usnadnění práce velkou část kódu, které budete spravovat podél tohoto testovacího prostředí je k dispozici jako fragmenty kódu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b460c-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b460c-149">K instalaci spustit fragmenty kódu **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="b460c-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b460c-150">Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha B:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b460c-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b460c-151">Cvičení</span><span class="sxs-lookup"><span data-stu-id="b460c-151">Exercises</span></span>

<span data-ttu-id="b460c-152">Podle následující praktická cvičení se skládá tohoto praktického testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="b460c-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="b460c-153">Cvičení 1: Injektáž Kontroleru</span><span class="sxs-lookup"><span data-stu-id="b460c-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="b460c-154">Cvičení 2: Vložení zobrazení</span><span class="sxs-lookup"><span data-stu-id="b460c-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="b460c-155">Cvičení 3: Injektáž filtry</span><span class="sxs-lookup"><span data-stu-id="b460c-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="b460c-156">Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení.</span><span class="sxs-lookup"><span data-stu-id="b460c-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b460c-157">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.</span><span class="sxs-lookup"><span data-stu-id="b460c-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="b460c-158">Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**.</span><span class="sxs-lookup"><span data-stu-id="b460c-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="b460c-159">Cvičení 1: Injektáž Kontroleru</span><span class="sxs-lookup"><span data-stu-id="b460c-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="b460c-160">V tomto cvičení se dozvíte, jak používat injektáž závislostí v Kontrolery architektury ASP.NET MVC integrací Unity pomocí balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="b460c-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="b460c-161">Z tohoto důvodu bude obsahovat služby řadičům MvcMusicStore oddělení logiky od přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="b460c-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="b460c-162">Služby se vytvoří novou závislost v kontroleru konstruktor, který bude vyřešen pomocí vkládání závislostí pomocí **Unity**.</span><span class="sxs-lookup"><span data-stu-id="b460c-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="b460c-163">Tento přístup se ukazují, jak generovat méně provázané aplikace, které jsou větší flexibilitu a jednodušší údržbu a testování.</span><span class="sxs-lookup"><span data-stu-id="b460c-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="b460c-164">Také se dozvíte, jak integrovat technologie ASP.NET MVC pomocí Unity.</span><span class="sxs-lookup"><span data-stu-id="b460c-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="b460c-165">O službě StoreManager</span><span class="sxs-lookup"><span data-stu-id="b460c-165">About StoreManager Service</span></span>

<span data-ttu-id="b460c-166">Zahrnuje službu, která spravuje data Store řadiče s názvem Music Store MVC, nyní k dispozici v řešení begin **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="b460c-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="b460c-167">Níže najdete implementace služby Store.</span><span class="sxs-lookup"><span data-stu-id="b460c-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="b460c-168">Všimněte si, že všechny metody vrátí modelu entity.</span><span class="sxs-lookup"><span data-stu-id="b460c-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="b460c-169">**StoreController** z zahájit řešení teď využívá **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="b460c-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="b460c-170">Byly odebrány všechny odkazy na data z **StoreController**a je nyní možné změnit aktuální přístup poskytovatele dat beze změny libovolné metody, která využívá **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="b460c-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="b460c-171">Vás bude nižší, než **StoreController** implementace má závislost s **StoreService** uvnitř konstruktoru třídy.</span><span class="sxs-lookup"><span data-stu-id="b460c-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="b460c-172">Závislost zavedené v tomto cvičení souvisí s **ovládacího prvku inverzi** (Inversion).</span><span class="sxs-lookup"><span data-stu-id="b460c-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="b460c-173">**StoreController** obdrží konstruktoru třídy **IStoreService** parametr typu, což je velmi důležité provést volání služby z uvnitř třídy.</span><span class="sxs-lookup"><span data-stu-id="b460c-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="b460c-174">Ale **StoreController** neimplementuje výchozí konstruktor (bez parametrů), žádné řadiče, musí mít pro práci s architekturou ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b460c-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="b460c-175">Chcete-li vyřešit závislost, kontroleru je potřeba vytvořit abstraktní výrobou (třída, která vrací libovolný objekt zadaného typu).</span><span class="sxs-lookup"><span data-stu-id="b460c-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="b460c-176">Obdržíte chybu třídy se pokusí vytvořit StoreController bez odeslání objekt služby, protože neexistuje žádný konstruktor deklarovat.</span><span class="sxs-lookup"><span data-stu-id="b460c-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="b460c-177">Úloha 1 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b460c-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="b460c-178">V této úloze budete spouštět aplikaci Begin, které zahrnují službu do Kontroleru Store, který odděluje přístup k datům z aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="b460c-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="b460c-179">Při spuštění aplikace, zobrazí se výjimka, jak službu řadiče se předá jako parametr ve výchozím nastavení:</span><span class="sxs-lookup"><span data-stu-id="b460c-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="b460c-180">Otevřít **začít** řešení nachází v **vkládá Source\Ex01 Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="b460c-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="b460c-181">Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b460c-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b460c-182">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b460c-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b460c-183">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="b460c-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b460c-184">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="b460c-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b460c-185">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="b460c-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b460c-186">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="b460c-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b460c-187">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b460c-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b460c-188">Stisknutím klávesy **Ctrl + F5** ke spuštění aplikace bez ladění.</span><span class="sxs-lookup"><span data-stu-id="b460c-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="b460c-189">Zobrazí se chybová zpráva &quot; **žádný konstruktor bez parametrů definovaných pro tento objekt**&quot;:</span><span class="sxs-lookup"><span data-stu-id="b460c-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="b460c-190">![Chyba při spouštění aplikace začít ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "Chyba při spouštění aplikace začít ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="b460c-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="b460c-191">*Chyba při spouštění aplikace začít ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="b460c-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="b460c-192">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="b460c-192">Close the browser.</span></span>

<span data-ttu-id="b460c-193">V následujícím postupu bude fungovat u řešení Music Store vložení závislosti, musí tento kontroler.</span><span class="sxs-lookup"><span data-stu-id="b460c-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="b460c-194">Úloha 2 – včetně Unity do MvcMusicStore řešení</span><span class="sxs-lookup"><span data-stu-id="b460c-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="b460c-195">V této úloze bude obsahovat **Unity.Mvc3** balíček NuGet do řešení.</span><span class="sxs-lookup"><span data-stu-id="b460c-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="b460c-196">Balíček Unity.Mvc3 je navržená pro ASP.NET MVC 3, ale je plně kompatibilní s ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b460c-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="b460c-197">Unity pro instanci je kontejner vkládání závislostí jednoduchých, rozšiřitelných s volitelnou podporou a zadejte zachycení.</span><span class="sxs-lookup"><span data-stu-id="b460c-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="b460c-198">Je kontejner pro obecné účely pro použití v jakéhokoli typu aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="b460c-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="b460c-199">Poskytuje společné funkce v včetně mechanismy injektáž závislostí: vytvoření objektu, abstrakce požadavků podle určení závislostí v modulu runtime, flexibilitu a odložením Konfigurace komponent do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b460c-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="b460c-200">Nainstalujte **Unity.Mvc3** balíček NuGet v **MvcMusicStore** projektu.</span><span class="sxs-lookup"><span data-stu-id="b460c-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="b460c-201">Chcete-li to provést, otevřete **Konzola správce balíčků** z **zobrazení** | **ostatní Windows**.</span><span class="sxs-lookup"><span data-stu-id="b460c-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="b460c-202">Spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="b460c-202">Run the following command.</span></span>

    <span data-ttu-id="b460c-203">PMC</span><span class="sxs-lookup"><span data-stu-id="b460c-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="b460c-204">![Instaluje se balíček NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "instaluje se balíček NuGet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="b460c-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="b460c-205">*Instaluje se balíček NuGet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="b460c-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="b460c-206">Jakmile **Unity.Mvc3** balíček nainstalován, prozkoumejte soubory a složky, aby bylo možné zjednodušit konfiguraci Unity automaticky přidá.</span><span class="sxs-lookup"><span data-stu-id="b460c-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="b460c-207">![Nainstalovaný balíček Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "nainstalovaný balíček Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="b460c-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="b460c-208">*Nainstalovaný balíček Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="b460c-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="b460c-209">Úloha 3 – registrace Unity v aplikaci Global.asax.cs\_Start</span><span class="sxs-lookup"><span data-stu-id="b460c-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="b460c-210">V této úloze budete aktualizovat **aplikace\_Start** metoda umístěný v **Global.asax.cs** volání inicializátor zaváděcího nástroje Unity a potom aktualizujte registraci souboru zaváděcího nástroje Služba a řadič bude používat pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="b460c-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="b460c-211">Teď jste připojili zaváděcí nástroj, který je soubor, který inicializuje kontejneru Unity a překladač závislostí.</span><span class="sxs-lookup"><span data-stu-id="b460c-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="b460c-212">Chcete-li to provést, otevřete **Global.asax.cs** a přidejte následující zvýrazněný kód v rámci **aplikace\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="b460c-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="b460c-213">(Fragment - kódu *Lab vkládání závislostí ASP.NET - Ex01 - inicializace Unity*)</span><span class="sxs-lookup"><span data-stu-id="b460c-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="b460c-214">Otevřít **Bootstrapper.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="b460c-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="b460c-215">Následující obory názvů: **MvcMusicStore.Services** a **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="b460c-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="b460c-216">(Fragment - kódu *ASP.NET závislost vkládání Lab – Ex01 - Bootstrapperu přidání oborů názvů*)</span><span class="sxs-lookup"><span data-stu-id="b460c-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="b460c-217">Nahraďte **BuildUnityContainer** obsah následujícím kódem, který registruje Store Kontroleru a službu Store vašeho metody.</span><span class="sxs-lookup"><span data-stu-id="b460c-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="b460c-218">(Fragment - kódu *ASP.NET závislost vkládání Lab – Ex01 - Store Register řadič a služby*)</span><span class="sxs-lookup"><span data-stu-id="b460c-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b460c-219">Úloha 4 – spouštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b460c-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="b460c-220">V této úloze budete spouštět aplikace kvůli ověření, že jej lze načíst teď po zahrnutí Unity.</span><span class="sxs-lookup"><span data-stu-id="b460c-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="b460c-221">Stisknutím klávesy **F5** ke spuštění aplikace, aplikace by se měly načíst nyní bez toho, abych jakákoli chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="b460c-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="b460c-222">![Spuštění aplikace pomocí vkládání závislostí](aspnet-mvc-4-dependency-injection/_static/image6.png "spuštění aplikace pomocí vkládání závislostí")</span><span class="sxs-lookup"><span data-stu-id="b460c-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="b460c-223">*Spuštění aplikace pomocí vkládání závislostí*</span><span class="sxs-lookup"><span data-stu-id="b460c-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="b460c-224">Přejděte do **/Store**.</span><span class="sxs-lookup"><span data-stu-id="b460c-224">Browse to **/Store**.</span></span> <span data-ttu-id="b460c-225">Tím se vyvolá **StoreController**, který je vytvořený pomocí **Unity**.</span><span class="sxs-lookup"><span data-stu-id="b460c-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="b460c-226">![Aplikace MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span><span class="sxs-lookup"><span data-stu-id="b460c-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="b460c-227">*Aplikace MVC Music Store*</span><span class="sxs-lookup"><span data-stu-id="b460c-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="b460c-228">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="b460c-228">Close the browser.</span></span>

<span data-ttu-id="b460c-229">V následujícím cvičení se dozvíte, jak rozšířit rozsah injektáž závislostí ji používat uvnitř zobrazení ASP.NET MVC a filtrů akce.</span><span class="sxs-lookup"><span data-stu-id="b460c-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="b460c-230">Cvičení 2: Vložení zobrazení</span><span class="sxs-lookup"><span data-stu-id="b460c-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="b460c-231">V tomto cvičení se dozvíte, jak pomocí vkládání závislostí v zobrazení s novými funkcemi technologie ASP.NET MVC 4 pro integraci Unity.</span><span class="sxs-lookup"><span data-stu-id="b460c-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="b460c-232">Aby bylo možné provést, se volání vlastní služby uvnitř zobrazení procházet Store, které se zobrazí zpráva a bitovou kopii níže.</span><span class="sxs-lookup"><span data-stu-id="b460c-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="b460c-233">Pak bude integrovat projektu pomocí Unity a vytvořit překladače vlastní závislost při vložení závislosti.</span><span class="sxs-lookup"><span data-stu-id="b460c-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="b460c-234">Úloha 1 – vytváření zobrazení, které využívá službu</span><span class="sxs-lookup"><span data-stu-id="b460c-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="b460c-235">V této úloze vytvoříte zobrazení, která provádí volání služby ke generování novou závislost.</span><span class="sxs-lookup"><span data-stu-id="b460c-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="b460c-236">Služba se skládá z jednoduchého zasílání zpráv služby zahrnuté v tomto řešení.</span><span class="sxs-lookup"><span data-stu-id="b460c-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="b460c-237">Otevřít **začít** řešení nachází v **vkládá Source\Ex02 View\Begin** složky.</span><span class="sxs-lookup"><span data-stu-id="b460c-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="b460c-238">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="b460c-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b460c-239">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b460c-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b460c-240">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b460c-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b460c-241">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="b460c-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b460c-242">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="b460c-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b460c-243">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="b460c-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b460c-244">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="b460c-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b460c-245">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b460c-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="b460c-246">Další informace najdete v tomto článku: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="b460c-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="b460c-247">Zahrnout **MessageService.cs** a **IMessageService.cs** třídy umístěny v **zdroje \Assets** složky v **/Services**.</span><span class="sxs-lookup"><span data-stu-id="b460c-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="b460c-248">Chcete-li to provést, klikněte pravým tlačítkem na **služby** a pak zvolte položku **přidat existující položku**.</span><span class="sxs-lookup"><span data-stu-id="b460c-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="b460c-249">Přejděte do umístění souborů a zahrnout je.</span><span class="sxs-lookup"><span data-stu-id="b460c-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="b460c-250">![Přidání zprávy služby a služby rozhraní](aspnet-mvc-4-dependency-injection/_static/image8.png "přidání zprávy služby a služby rozhraní")</span><span class="sxs-lookup"><span data-stu-id="b460c-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="b460c-251">*Přidání zprávy služby a služby rozhraní*</span><span class="sxs-lookup"><span data-stu-id="b460c-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b460c-252">**IMessageService** rozhraní definuje dvě vlastnosti, které jsou implementované **MessageService** třídy.</span><span class="sxs-lookup"><span data-stu-id="b460c-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="b460c-253">Tyto vlastnosti -**zpráva** a **ImageUrl**– ukládání zprávu a adresu URL obrázku, který se má zobrazit.</span><span class="sxs-lookup"><span data-stu-id="b460c-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="b460c-254">Vytvořit složku **/stránky** v projektu kořenová složka a pak přidejte existující třídu **MyBasePage.cs** z **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="b460c-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="b460c-255">Základní stránka, kterou bude dědit z má následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="b460c-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="b460c-256">![Složka stránek](aspnet-mvc-4-dependency-injection/_static/image9.png "složce stránky")</span><span class="sxs-lookup"><span data-stu-id="b460c-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="b460c-257">Otevřít **Browse.cshtml** zobrazení z **/zobrazení/Store** složky a nastavte ji zdědit **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="b460c-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="b460c-258">V **Procházet** zobrazovat, přidávat volání **MessageService** zobrazíte bitovou kopii a napište zprávu, nenačte služba.</span><span class="sxs-lookup"><span data-stu-id="b460c-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="b460c-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="b460c-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="b460c-260">Úloha 2 – včetně překladače vlastní závislost a aktivátoru stránky vlastního zobrazení</span><span class="sxs-lookup"><span data-stu-id="b460c-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="b460c-261">V předchozí úloze vloží novou závislost uvnitř zobrazení provádět volání služby dovnitř.</span><span class="sxs-lookup"><span data-stu-id="b460c-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="b460c-262">Nyní, vyřeší dané závislosti implementací rozhraní injektáž závislostí ASP.NET MVC **IViewPageActivator** a **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="b460c-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="b460c-263">V řešení bude obsahovat implementace **IDependencyResolver** , který se bude zabývat načítání služby pomocí Unity.</span><span class="sxs-lookup"><span data-stu-id="b460c-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="b460c-264">Potom bude obsahovat jiné vlastní implementaci **IViewPageActivator** rozhraní, které se vyřeší vytváření zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b460c-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="b460c-265">Od verze technologie ASP.NET MVC 3 měla implementace pro vkládání závislostí zjednodušené rozhraní pro registraci služby.</span><span class="sxs-lookup"><span data-stu-id="b460c-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="b460c-266">**IDependencyResolver** a **IViewPageActivator** jsou součástí sady funkcí technologie ASP.NET MVC 3 pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="b460c-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="b460c-267">**-IDependencyResolver** rozhraní nahradí předchozí IMvcServiceLocator.</span><span class="sxs-lookup"><span data-stu-id="b460c-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="b460c-268">Implementátoři IDependencyResolver musí vracet instanci služby nebo služby kolekce.</span><span class="sxs-lookup"><span data-stu-id="b460c-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="b460c-269">**-IViewPageActivator** rozhraní poskytuje jemně odstupňovanou kontrolu nad jak zobrazit stránky jsou vytvořeny pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="b460c-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="b460c-270">Třídy, které implementují **IViewPageActivator** rozhraní můžete vytvořit zobrazení instance pomocí informací o kontextu.</span><span class="sxs-lookup"><span data-stu-id="b460c-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="b460c-271">Vytvořte /**továren** složky v kořenové složce projektu.</span><span class="sxs-lookup"><span data-stu-id="b460c-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="b460c-272">Zahrnout **CustomViewPageActivator.cs** do svého řešení z **/zdroje/prostředky/** k **továren** složky.</span><span class="sxs-lookup"><span data-stu-id="b460c-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="b460c-273">Chcete-li to mohli udělat, klikněte pravým tlačítkem **/Factories** složky, vyberte **přidat | Existující položka** a pak vyberte **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="b460c-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="b460c-274">Tato třída implementuje **IViewPageActivator** rozhraní pro uložení kontejneru Unity.</span><span class="sxs-lookup"><span data-stu-id="b460c-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="b460c-275">**CustomViewPageActivator** zodpovídá za správu vytváření zobrazení pomocí Unity kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b460c-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="b460c-276">Zahrnout **UnityDependencyResolver.cs** souboru z **/zdroje/prostředky** k **/Factories** složky.</span><span class="sxs-lookup"><span data-stu-id="b460c-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="b460c-277">Chcete-li to mohli udělat, klikněte pravým tlačítkem **/Factories** složky, vyberte **přidat | Existující položka** a pak vyberte **UnityDependencyResolver.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="b460c-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="b460c-278">**UnityDependencyResolver** třída je vlastní překladače závislostí pro Unity.</span><span class="sxs-lookup"><span data-stu-id="b460c-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="b460c-279">Když služba nebyla nalezena uvnitř kontejneru Unity, je invocated základní překladač.</span><span class="sxs-lookup"><span data-stu-id="b460c-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="b460c-280">V následující úlohy se zaregistruje obou implementacích Pokud chcete, aby model znát umístění služby a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b460c-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="b460c-281">Úloha 3 – registrace pro injektáž závislostí v rámci kontejneru Unity</span><span class="sxs-lookup"><span data-stu-id="b460c-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="b460c-282">V této úloze zařadí všechny předchozí věci dohromady a ujistěte se, vkládání závislostí fungovat.</span><span class="sxs-lookup"><span data-stu-id="b460c-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="b460c-283">Až teď vaše řešení obsahuje následující prvky:</span><span class="sxs-lookup"><span data-stu-id="b460c-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="b460c-284">A **Procházet** zobrazení, která dědí z **MyBaseClass** a využívá **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="b460c-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="b460c-285">Zprostředkující třída -**MyBaseClass**–, který má deklarován pro rozhraní služby vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="b460c-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="b460c-286">Služby - **MessageService** - a jeho rozhraní **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="b460c-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="b460c-287">Překladače vlastní závislost pro Unity – **UnityDependencyResolver** –, který se zabývá načítání služby.</span><span class="sxs-lookup"><span data-stu-id="b460c-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="b460c-288">Aktivátor stránky zobrazení - **CustomViewPageActivator** –, který vytvoří danou stránku.</span><span class="sxs-lookup"><span data-stu-id="b460c-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="b460c-289">Chcete-li vložit **Procházet** zobrazení, se nyní zaregistrujete překladače vlastní závislost v kontejneru Unity.</span><span class="sxs-lookup"><span data-stu-id="b460c-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="b460c-290">Otevřít **Bootstrapper.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="b460c-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="b460c-291">Zaregistrovat instanci **MessageService** do kontejneru Unity na inicializaci služby:</span><span class="sxs-lookup"><span data-stu-id="b460c-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="b460c-292">(Fragment - kódu *zpráva služba testovacího prostředí – Ex02 – registrace technologie ASP.NET závislost vkládání*)</span><span class="sxs-lookup"><span data-stu-id="b460c-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="b460c-293">Přidejte odkaz na **MvcMusicStore.Factories** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="b460c-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="b460c-294">(Fragment - kódu *ASP.NET závislost vkládání Lab – Ex02 – objekty pro vytváření Namespace*)</span><span class="sxs-lookup"><span data-stu-id="b460c-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="b460c-295">Zaregistrujte **CustomViewPageActivator** jako aktivátoru stránky zobrazení do kontejneru Unity:</span><span class="sxs-lookup"><span data-stu-id="b460c-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="b460c-296">(Fragment - kódu *ASP.NET závislost vkládání Lab – Ex02 - Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="b460c-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="b460c-297">ASP.NET MVC 4 výchozího překladače závislostí provede nahraďte instance **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="b460c-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="b460c-298">Chcete-li to provést, nahraďte **inicializační** metoda obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="b460c-298">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="b460c-299">(Fragment - kódu *překladač závislostí ASP.NET závislost vkládání Lab – Ex02 – aktualizace*)</span><span class="sxs-lookup"><span data-stu-id="b460c-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="b460c-300">ASP.NET MVC poskytuje výchozí třídu překladače závislostí.</span><span class="sxs-lookup"><span data-stu-id="b460c-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="b460c-301">Pro práci s překladače vlastní závislostí, které jsme vytvořili pro unity, má tento překladač se nahradí.</span><span class="sxs-lookup"><span data-stu-id="b460c-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b460c-302">Úloha 4 – spouštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b460c-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="b460c-303">V této úloze budete spouštět aplikace kvůli ověření, že prohlížeč Store využívá službu a zobrazí obrázek a zprávy načtené:</span><span class="sxs-lookup"><span data-stu-id="b460c-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="b460c-304">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b460c-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b460c-305">Klikněte na tlačítko **Rock** v rámci nabídky žánry a najdete v tématu Jak **MessageService** se vloží do zobrazení a načíst zobrazení uvítací zprávy a image.</span><span class="sxs-lookup"><span data-stu-id="b460c-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="b460c-306">V tomto příkladu jsme zadáváte &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="b460c-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="b460c-307">![MVC Music Store – zobrazení vkládání](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - vkládání zobrazení")</span><span class="sxs-lookup"><span data-stu-id="b460c-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="b460c-308">*MVC Music Store - vkládání zobrazení*</span><span class="sxs-lookup"><span data-stu-id="b460c-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="b460c-309">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="b460c-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="b460c-310">Cvičení 3: Injektáž filtrů Akce</span><span class="sxs-lookup"><span data-stu-id="b460c-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="b460c-311">V předchozím praktických cvičení **vlastní filtry akce** jste už pracovali s filtry přizpůsobení a vkládání.</span><span class="sxs-lookup"><span data-stu-id="b460c-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="b460c-312">V tomto cvičení se dozvíte, jak vkládat pomocí Unity kontejneru filtrů pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="b460c-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="b460c-313">K tomu, které přidáte do řešení Music Store filtr vlastní akce, který bude sledovat aktivitu webu.</span><span class="sxs-lookup"><span data-stu-id="b460c-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="b460c-314">Úloha 1 – včetně sledování filtru v řešení</span><span class="sxs-lookup"><span data-stu-id="b460c-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="b460c-315">V této úloze zahrnete do Music Store vlastní akce filtru na události trasování.</span><span class="sxs-lookup"><span data-stu-id="b460c-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="b460c-316">Jako filtr vlastních akcí jsou již považovány koncepty v předchozím prostředí &quot;vlastní filtry akce&quot;, se jednoduše uvést třídy filtru ve složce prostředky tohoto testovacího prostředí a pak vytvořte zprostředkovatele filtrů pro Unity:</span><span class="sxs-lookup"><span data-stu-id="b460c-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="b460c-317">Otevřít **začít** řešení nachází v **Source\Ex03 - vkládá Filter\Begin akce** složky.</span><span class="sxs-lookup"><span data-stu-id="b460c-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="b460c-318">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="b460c-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b460c-319">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b460c-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b460c-320">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b460c-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b460c-321">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="b460c-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b460c-322">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="b460c-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b460c-323">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="b460c-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b460c-324">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="b460c-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b460c-325">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b460c-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="b460c-326">Další informace najdete v tomto článku: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="b460c-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="b460c-327">Zahrnout **TraceActionFilter.cs** souboru z **/zdroje/prostředky** k **/filtruje** složky.</span><span class="sxs-lookup"><span data-stu-id="b460c-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="b460c-328">Tento filtr vlastních akcí provádí trasování rozhraní ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b460c-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="b460c-329">Můžete zkontrolovat &quot;místní ASP.NET MVC 4 a dynamické filtry akce&quot; testovacího prostředí pro odkaz na další.</span><span class="sxs-lookup"><span data-stu-id="b460c-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="b460c-330">Přidat prázdnou třídu **FilterProvider.cs** do projektu ve složce   **/filtry.**</span><span class="sxs-lookup"><span data-stu-id="b460c-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="b460c-331">Přidat **System.Web.Mvc** a **Microsoft.Practices.Unity** obory názvů v **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="b460c-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="b460c-332">(Fragment - kódu *ASP.NET závislost vkládání Lab – Ex03 - zprostředkovatele filtrů přidání oborů názvů*)</span><span class="sxs-lookup"><span data-stu-id="b460c-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="b460c-333">Ujistěte se, třída dědila z **IFilterProvider** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b460c-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="b460c-334">Přidat **IUnityContainer** vlastnost **FilterProvider** třídy a pak vytvořte konstruktor třídy přiřadit kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b460c-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="b460c-335">(Fragment - kódu *ASP.NET závislost vkládání Lab – Ex03 - filtru poskytovatele konstruktor*)</span><span class="sxs-lookup"><span data-stu-id="b460c-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="b460c-336">Konstruktor třídy zprostředkovatele filtru není vytváření **nové** objektu.</span><span class="sxs-lookup"><span data-stu-id="b460c-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="b460c-337">Kontejner je předán jako parametr a závislost je vyřešen Unity.</span><span class="sxs-lookup"><span data-stu-id="b460c-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="b460c-338">V **FilterProvider** třídy, implementovat metodu **GetFilters** z **IFilterProvider** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b460c-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="b460c-339">(Fragment - kódu *ASP.NET závislost vkládání Lab – Ex03 - filtru poskytovatele GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="b460c-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="b460c-340">Úloha 2 – registrace a povolení filtr</span><span class="sxs-lookup"><span data-stu-id="b460c-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="b460c-341">V této úloze povolíte sledování webu.</span><span class="sxs-lookup"><span data-stu-id="b460c-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="b460c-342">K tomuto účelu se zaregistrovat tohoto filtru v **Bootstrapper.cs BuildUnityContainer** metodu spustit trasování:</span><span class="sxs-lookup"><span data-stu-id="b460c-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="b460c-343">Otevřít **Web.config** v kořenu projektu a povolit trasování sledování na skupinu System.Web.</span><span class="sxs-lookup"><span data-stu-id="b460c-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="b460c-344">Otevřít **Bootstrapper.cs** v kořenu projektu.</span><span class="sxs-lookup"><span data-stu-id="b460c-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="b460c-345">Přidejte odkaz na **MvcMusicStore.Filters** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="b460c-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="b460c-346">(Fragment - kódu *ASP.NET závislost vkládání Lab – Ex03 - Bootstrapperu přidání oborů názvů*)</span><span class="sxs-lookup"><span data-stu-id="b460c-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="b460c-347">Vyberte **BuildUnityContainer** metoda a zaregistrujte filtru v kontejneru Unity.</span><span class="sxs-lookup"><span data-stu-id="b460c-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="b460c-348">Budete muset zaregistrovat zprostředkovatele filtrů, stejně jako filtr akce.</span><span class="sxs-lookup"><span data-stu-id="b460c-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="b460c-349">(Fragment - kódu *ASP.NET závislost vkládání Lab – Ex03 - Register FilterProvider a ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="b460c-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b460c-350">Úloha 3 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b460c-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="b460c-351">V této úloze se spustit aplikaci a otestovat, zda filtr vlastních akcí je trasování aktivity:</span><span class="sxs-lookup"><span data-stu-id="b460c-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="b460c-352">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b460c-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b460c-353">Klikněte na tlačítko **Rock** v rámci nabídky žánrů.</span><span class="sxs-lookup"><span data-stu-id="b460c-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="b460c-354">Můžete vyhledat další žánry Pokud budete chtít.</span><span class="sxs-lookup"><span data-stu-id="b460c-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="b460c-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="b460c-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="b460c-356">*Aplikace Music Store*</span><span class="sxs-lookup"><span data-stu-id="b460c-356">*Music Store*</span></span>
3. <span data-ttu-id="b460c-357">Přejděte do **/Trace.axd** zobrazíte trasování aplikace stránce a potom klikněte na tlačítko **zobrazit podrobnosti o**.</span><span class="sxs-lookup"><span data-stu-id="b460c-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="b460c-358">![Protokol trasování aplikace](aspnet-mvc-4-dependency-injection/_static/image12.png "protokolu trasování aplikace")</span><span class="sxs-lookup"><span data-stu-id="b460c-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="b460c-359">*Protokol trasování aplikace*</span><span class="sxs-lookup"><span data-stu-id="b460c-359">*Application Trace Log*</span></span>

    <span data-ttu-id="b460c-360">![Trasování aplikace – podrobnosti o žádosti](aspnet-mvc-4-dependency-injection/_static/image13.png "trasování aplikace – podrobnosti o požadavku")</span><span class="sxs-lookup"><span data-stu-id="b460c-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="b460c-361">*Trasování aplikace – podrobnosti o žádosti*</span><span class="sxs-lookup"><span data-stu-id="b460c-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="b460c-362">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="b460c-362">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b460c-363">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b460c-363">Summary</span></span>

<span data-ttu-id="b460c-364">Po dokončení tohoto praktického testovacího prostředí jste se dozvěděli, jak pomocí vkládání závislostí v architektuře ASP.NET MVC 4 integrací Unity pomocí balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="b460c-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="b460c-365">Abychom toho dosáhli, používáte injektáž závislostí do kontrolerů, zobrazení a filtrů akce.</span><span class="sxs-lookup"><span data-stu-id="b460c-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="b460c-366">Následující koncepty byly pokryty:</span><span class="sxs-lookup"><span data-stu-id="b460c-366">The following concepts were covered:</span></span>

- <span data-ttu-id="b460c-367">Funkce vkládání závislostí aplikace ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b460c-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="b460c-368">Pomocí balíčku NuGet Unity.Mvc3 integrace Unity</span><span class="sxs-lookup"><span data-stu-id="b460c-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="b460c-369">Injektáž závislostí do Kontrolerů</span><span class="sxs-lookup"><span data-stu-id="b460c-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="b460c-370">Injektáž závislostí v zobrazeních.</span><span class="sxs-lookup"><span data-stu-id="b460c-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="b460c-371">Injektáž závislostí filtrů Akce</span><span class="sxs-lookup"><span data-stu-id="b460c-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b460c-372">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="b460c-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b460c-373">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="b460c-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b460c-374">Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="b460c-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b460c-375">Přejděte na [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b460c-375">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b460c-376">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="b460c-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="b460c-377">Klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="b460c-377">Click on **Install Now**.</span></span> <span data-ttu-id="b460c-378">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="b460c-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b460c-379">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.</span><span class="sxs-lookup"><span data-stu-id="b460c-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b460c-380">![Instalace sady Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "instalace sady Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b460c-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b460c-381">*Instalace sady Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b460c-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b460c-382">Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="b460c-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Přijetí podmínek licence](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="b460c-384">*Přijetí podmínek licence*</span><span class="sxs-lookup"><span data-stu-id="b460c-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b460c-385">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="b460c-385">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="b460c-387">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="b460c-387">*Installation progress*</span></span>
6. <span data-ttu-id="b460c-388">Až instalace skončí, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="b460c-388">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="b460c-390">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="b460c-390">*Installation completed*</span></span>
7. <span data-ttu-id="b460c-391">Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="b460c-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b460c-392">Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="b460c-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web dlaždice](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="b460c-394">*VS Express for Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="b460c-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="b460c-395">Příloha B: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="b460c-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="b460c-396">Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="b460c-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b460c-397">Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="b460c-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b460c-398">![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-dependency-injection/_static/image19.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="b460c-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b460c-399">*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="b460c-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b460c-400">***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***</span><span class="sxs-lookup"><span data-stu-id="b460c-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b460c-401">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="b460c-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b460c-402">Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).</span><span class="sxs-lookup"><span data-stu-id="b460c-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b460c-403">Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="b460c-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b460c-404">Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).</span><span class="sxs-lookup"><span data-stu-id="b460c-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b460c-405">Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="b460c-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b460c-406">![Začněte psát název fragmentu kódu](aspnet-mvc-4-dependency-injection/_static/image20.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="b460c-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b460c-407">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="b460c-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="b460c-408">![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](aspnet-mvc-4-dependency-injection/_static/image21.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")</span><span class="sxs-lookup"><span data-stu-id="b460c-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b460c-409">*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*</span><span class="sxs-lookup"><span data-stu-id="b460c-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b460c-410">![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](aspnet-mvc-4-dependency-injection/_static/image22.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")</span><span class="sxs-lookup"><span data-stu-id="b460c-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b460c-411">*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*</span><span class="sxs-lookup"><span data-stu-id="b460c-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b460c-412">***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="b460c-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b460c-413">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="b460c-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b460c-414">Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="b460c-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b460c-415">Kliknutím na vyberte relevantní fragment kódu ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="b460c-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b460c-416">![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-dependency-injection/_static/image23.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="b460c-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b460c-417">*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="b460c-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b460c-418">![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-dependency-injection/_static/image24.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="b460c-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b460c-419">*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="b460c-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
