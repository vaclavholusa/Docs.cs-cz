---
title: Použitému vzoru úložišť s ASP.NET Core
author: ardalis
description: Zjistěte, jak implementovat vzor návrhu úložiště aplikace v aplikaci ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342740"
---
# <a name="repository-pattern-with-aspnet-core"></a><span data-ttu-id="c14c7-103">Použitému vzoru úložišť s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c14c7-103">Repository pattern with ASP.NET Core</span></span>

<span data-ttu-id="c14c7-104">Podle [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c14c7-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c14c7-105">*Použitému vzoru úložišť* je návrhový vzor, který izoluje přístup k datům za abstrakce rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c14c7-105">The *repository pattern* is a design pattern that isolates data access behind interface abstractions.</span></span> <span data-ttu-id="c14c7-106">Připojení k databázi a manipulace s objekty úložiště dat se provádí prostřednictvím metody poskytované objektem implementace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c14c7-106">Connecting to the database and manipulating data storage objects is performed through methods provided by the interface's implementation.</span></span> <span data-ttu-id="c14c7-107">V důsledku toho není nutné pro volání kódu k řešení databázi aspekty, jako je připojení, příkazy a čtenáři.</span><span class="sxs-lookup"><span data-stu-id="c14c7-107">Consequently, there's no need for calling code to deal with database concerns, such as connections, commands, and readers.</span></span>

<span data-ttu-id="c14c7-108">Implementace vzoru úložiště pomocí ASP.NET Core má následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c14c7-108">Implementation of the repository pattern with ASP.NET Core has the following benefits:</span></span>

* <span data-ttu-id="c14c7-109">Organizace aplikace není tak složitý s žádné přímé závislost mezi firmy a vrstvy přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="c14c7-109">Organization of the app is less complex with no direct interdependency between the business and data access layers.</span></span>
* <span data-ttu-id="c14c7-110">Je snazší znovu použít kód pro přístup k databázi, protože kód je centrálně spravované pomocí jednoho nebo více úložišť.</span><span class="sxs-lookup"><span data-stu-id="c14c7-110">It's easier to reuse database access code because the code is centrally managed by one or more repositories.</span></span>
* <span data-ttu-id="c14c7-111">Obchodní domény může být nezávisle na sobě jednotky testování z databázové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="c14c7-111">The business domain can be independently unit tested from the database layer.</span></span>

<span data-ttu-id="c14c7-112">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c14c7-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c14c7-113">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) používá vzor úložiště k inicializaci a zobrazit seznam názvů film znak.</span><span class="sxs-lookup"><span data-stu-id="c14c7-113">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) uses the repository pattern to initialize and display a list of movie character names.</span></span> <span data-ttu-id="c14c7-114">Tato aplikace používá [Entity Framework Core](/ef/core/) a `ApplicationDbContext` třídy pro jeho trvalosti dat, ale infrastruktury databáze není zřejmé tam, kde je k datům přistupuje.</span><span class="sxs-lookup"><span data-stu-id="c14c7-114">The app uses [Entity Framework Core](/ef/core/) and an `ApplicationDbContext` class for its data persistence, but the database infrastructure isn't apparent where the data is accessed.</span></span> <span data-ttu-id="c14c7-115">Data access a databázových objektů jsou abstrahované za [úložiště](https://martinfowler.com/eaaCatalog/repository.html).</span><span class="sxs-lookup"><span data-stu-id="c14c7-115">Data access and database objects are abstracted behind a [repository](https://martinfowler.com/eaaCatalog/repository.html).</span></span>

## <a name="repository-interface"></a><span data-ttu-id="c14c7-116">Rozhraní úložiště</span><span class="sxs-lookup"><span data-stu-id="c14c7-116">Repository interface</span></span>

<span data-ttu-id="c14c7-117">Úložiště rozhraní definuje vlastnosti a metody pro implementaci.</span><span class="sxs-lookup"><span data-stu-id="c14c7-117">A repository interface defines the properties and methods for implementation.</span></span> <span data-ttu-id="c14c7-118">V ukázkové aplikaci rozhraní úložiště pro data o filmech znak je `ICharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="c14c7-118">In the sample app, the repository interface for movie character data is `ICharacterRepository`.</span></span> <span data-ttu-id="c14c7-119">`ICharacterRepository` definuje `ListAll` a `Add` metod požadovaných pro práci s `Character` instance v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="c14c7-119">`ICharacterRepository` defines the `ListAll` and `Add` methods required to work with `Character` instances in the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c14c7-120">`Character` je definována takto:</span><span class="sxs-lookup"><span data-stu-id="c14c7-120">`Character` is defined as:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a><span data-ttu-id="c14c7-121">Konkrétní typ úložiště</span><span class="sxs-lookup"><span data-stu-id="c14c7-121">Repository concrete type</span></span>

<span data-ttu-id="c14c7-122">Rozhraní je implementováno konkrétního typu implementujícího typ.</span><span class="sxs-lookup"><span data-stu-id="c14c7-122">The interface is implemented by a concrete type.</span></span> <span data-ttu-id="c14c7-123">V ukázkové aplikaci `CharacterRepository` spravuje kontext databáze a implementuje `ListAll` a `Add` metody:</span><span class="sxs-lookup"><span data-stu-id="c14c7-123">In the sample app, `CharacterRepository` manages the database context and implements the `ListAll` and `Add` methods:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a><span data-ttu-id="c14c7-124">Registrace služby úložiště</span><span class="sxs-lookup"><span data-stu-id="c14c7-124">Register the repository service</span></span>

<span data-ttu-id="c14c7-125">Místní úložiště a databáze jsou zaregistrované v kontejneru služby `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c14c7-125">The repository and database context are registered with the service container in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c14c7-126">V ukázkové aplikaci `ApplicationDbContext` je nakonfigurovaný pomocí volání metody rozšíření [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span><span class="sxs-lookup"><span data-stu-id="c14c7-126">In the sample app, `ApplicationDbContext` is configured with the call to the extension method [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span> <span data-ttu-id="c14c7-127">`ICharacterRepository` je zaregistrovaný jako vymezené služby:</span><span class="sxs-lookup"><span data-stu-id="c14c7-127">`ICharacterRepository` is registered as a scoped service:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a><span data-ttu-id="c14c7-128">Vloží instanci úložiště</span><span class="sxs-lookup"><span data-stu-id="c14c7-128">Inject an instance of the repository</span></span>

<span data-ttu-id="c14c7-129">Ve třídě, ve kterém jsou vyžadována přístup k databázi je instance úložiště požadované prostřednictvím konstruktoru a přiřadit soukromé pole pro použití metody třídy.</span><span class="sxs-lookup"><span data-stu-id="c14c7-129">In a class where database access is required, an instance of the repository is requested via the constructor and assigned to a private field for use by class methods.</span></span> <span data-ttu-id="c14c7-130">V ukázkové aplikaci `ICharacterRepository` se používá ke:</span><span class="sxs-lookup"><span data-stu-id="c14c7-130">In the sample app, `ICharacterRepository` is used to:</span></span>

* <span data-ttu-id="c14c7-131">Naplnění databáze, pokud neexistuje žádné znaky.</span><span class="sxs-lookup"><span data-stu-id="c14c7-131">Populate the database if no characters exist.</span></span>
* <span data-ttu-id="c14c7-132">Získání seznamu sad znaků pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c14c7-132">Obtain a list of the characters for display.</span></span>

<span data-ttu-id="c14c7-133">Všimněte si, jak volající kód pouze komunikuje s implementací rozhraní `CharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="c14c7-133">Notice how the calling code only interacts with the interface's implementation, `CharacterRepository`.</span></span> <span data-ttu-id="c14c7-134">Volání kódu nepoužívá `ApplicationDbContext` přímo:</span><span class="sxs-lookup"><span data-stu-id="c14c7-134">Calling code doesn't use the `ApplicationDbContext` directly:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a><span data-ttu-id="c14c7-135">Rozhraní obecného úložiště</span><span class="sxs-lookup"><span data-stu-id="c14c7-135">Generic repository interface</span></span>

<span data-ttu-id="c14c7-136">Toto téma a jeho ukázkovou aplikaci ukazují nejjednodušší implementace tohoto modelu úložiště, kde se vytvoří jedno úložiště pro každý objekt firmy.</span><span class="sxs-lookup"><span data-stu-id="c14c7-136">This topic and its sample app demonstrate the simplest implementation of the repository pattern, where one repository is created for each business object.</span></span> <span data-ttu-id="c14c7-137">Pokud aplikace překročí několik objektů *obecného úložiště rozhraní* může snížit množství kódu vyžadovaného pro implementaci vzoru úložiště.</span><span class="sxs-lookup"><span data-stu-id="c14c7-137">If the app grows beyond a few objects, a *generic repository interface* may reduce the amount of code required to implement the repository pattern.</span></span> <span data-ttu-id="c14c7-138">Další informace najdete v tématu [DevIQ: použitému vzoru úložišť: Obecné rozhraní úložiště](http://deviq.com/repository-pattern/).</span><span class="sxs-lookup"><span data-stu-id="c14c7-138">For more information, see [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c14c7-139">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c14c7-139">Additional resources</span></span>

* [<span data-ttu-id="c14c7-140">DevIQ: Použitému vzoru úložišť</span><span class="sxs-lookup"><span data-stu-id="c14c7-140">DevIQ: Repository Pattern</span></span>](https://deviq.com/repository-pattern/)
* [<span data-ttu-id="c14c7-141">Injektáž závislostí</span><span class="sxs-lookup"><span data-stu-id="c14c7-141">Dependency injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="c14c7-142">Injektáž závislostí do zobrazení</span><span class="sxs-lookup"><span data-stu-id="c14c7-142">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
* [<span data-ttu-id="c14c7-143">Injektáž závislostí do kontrolerů</span><span class="sxs-lookup"><span data-stu-id="c14c7-143">Dependency injection into controllers</span></span>](xref:mvc/controllers/dependency-injection)
* [<span data-ttu-id="c14c7-144">Injektáž závislostí v obslužných rutinách požadavků</span><span class="sxs-lookup"><span data-stu-id="c14c7-144">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
* [<span data-ttu-id="c14c7-145">Inverze – kontejnery ovládacích prvků a vzor injektáž závislostí</span><span class="sxs-lookup"><span data-stu-id="c14c7-145">Inversion of Control Containers and the Dependency Injection Pattern</span></span>](https://www.martinfowler.com/articles/injection.html)
