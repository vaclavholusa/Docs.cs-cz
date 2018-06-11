---
title: Testování částí stránky Razor v ASP.NET Core
author: guardrex
description: Naučte se vytvářet testy částí pro stránky Razor aplikace.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: test/razor-pages-tests
ms.openlocfilehash: df74d8e44b2dff00e76139edba47fd8a30ce33ef
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252302"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a><span data-ttu-id="8b528-103">Testování částí stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b528-103">Razor Pages unit tests in ASP.NET Core</span></span>

<span data-ttu-id="8b528-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8b528-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8b528-105">Jádro ASP.NET podporuje testování částí aplikací pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="8b528-105">ASP.NET Core supports unit tests of Razor Pages apps.</span></span> <span data-ttu-id="8b528-106">Testy dat přístup layer (DAL) a zajistíte modely stránky:</span><span class="sxs-lookup"><span data-stu-id="8b528-106">Tests of the data access layer (DAL) and page models help ensure:</span></span>

* <span data-ttu-id="8b528-107">Části stránky Razor aplikace fungovat nezávisle a společně jako jednotku během vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="8b528-107">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="8b528-108">Třídy a metody mají omezenou obory zodpovědnosti.</span><span class="sxs-lookup"><span data-stu-id="8b528-108">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="8b528-109">Další dokumentaci existuje na chování aplikace.</span><span class="sxs-lookup"><span data-stu-id="8b528-109">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="8b528-110">Regresí, které jsou chyby způsobené aktualizace kód, nebyly nalezeny během automatického vytváření a nasazení.</span><span class="sxs-lookup"><span data-stu-id="8b528-110">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="8b528-111">Toto téma předpokládá, že máte základní znalosti a testování částí aplikací stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="8b528-111">This topic assumes that you have a basic understanding of Razor Pages apps and unit tests.</span></span> <span data-ttu-id="8b528-112">Pokud jste obeznámeni s stránky Razor aplikace nebo testovací koncepty, najdete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="8b528-112">If you're unfamiliar with Razor Pages apps or test concepts, see the following topics:</span></span>

* [<span data-ttu-id="8b528-113">Úvod do stránky Razor</span><span class="sxs-lookup"><span data-stu-id="8b528-113">Introduction to Razor Pages</span></span>](xref:mvc/razor-pages/index)
* [<span data-ttu-id="8b528-114">Začínáme se stránkami Razor</span><span class="sxs-lookup"><span data-stu-id="8b528-114">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="8b528-115">Testování C# v .NET Core pomocí testovacích dotnet a xUnit částí</span><span class="sxs-lookup"><span data-stu-id="8b528-115">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

<span data-ttu-id="8b528-116">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8b528-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8b528-117">Ukázkový projekt se skládá ze dvou aplikací:</span><span class="sxs-lookup"><span data-stu-id="8b528-117">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="8b528-118">Aplikace</span><span class="sxs-lookup"><span data-stu-id="8b528-118">App</span></span>         | <span data-ttu-id="8b528-119">Složky projektu</span><span class="sxs-lookup"><span data-stu-id="8b528-119">Project folder</span></span>                        | <span data-ttu-id="8b528-120">Popis</span><span class="sxs-lookup"><span data-stu-id="8b528-120">Description</span></span> |
| ----------- | ------------------------------------- | ----------- |
| <span data-ttu-id="8b528-121">Zprávy aplikace</span><span class="sxs-lookup"><span data-stu-id="8b528-121">Message app</span></span> | <span data-ttu-id="8b528-122">*src/RazorPagesTestSample*</span><span class="sxs-lookup"><span data-stu-id="8b528-122">*src/RazorPagesTestSample*</span></span>            | <span data-ttu-id="8b528-123">Umožňuje uživateli přidat, odstraňte jednu, odstranit všechny a analyzovat zprávy.</span><span class="sxs-lookup"><span data-stu-id="8b528-123">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="8b528-124">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="8b528-124">Test app</span></span>    | <span data-ttu-id="8b528-125">*tests/RazorPagesTestSample.Tests*</span><span class="sxs-lookup"><span data-stu-id="8b528-125">*tests/RazorPagesTestSample.Tests*</span></span>    | <span data-ttu-id="8b528-126">Použít k testování částí aplikace zprávu: layer (DAL) a Index stránky model přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="8b528-126">Used to unit test the message app: Data access layer (DAL) and Index page model.</span></span> |

<span data-ttu-id="8b528-127">Testy můžete spustit pomocí funkce integrované testovací IDE, jako třeba [Visual Studio](https://www.visualstudio.com/vs/).</span><span class="sxs-lookup"><span data-stu-id="8b528-127">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://www.visualstudio.com/vs/).</span></span> <span data-ttu-id="8b528-128">Pokud používáte [Visual Studio Code](https://code.visualstudio.com/) nebo příkazového řádku, spusťte následující příkaz na příkazovém řádku v *tests/RazorPagesTestSample.Tests* složky:</span><span class="sxs-lookup"><span data-stu-id="8b528-128">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestSample.Tests* folder:</span></span>

```console
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="8b528-129">Zprávy aplikace organizace</span><span class="sxs-lookup"><span data-stu-id="8b528-129">Message app organization</span></span>

<span data-ttu-id="8b528-130">Zprávy aplikace je jednoduchý systém stránky Razor zpráv s následujícími charakteristikami:</span><span class="sxs-lookup"><span data-stu-id="8b528-130">The message app is a simple Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="8b528-131">Index stránky aplikace (*Pages/Index.cshtml* a *Pages/Index.cshtml.cs*) poskytuje uživatelské rozhraní a stránky model metody k řízení přidání, odstranění a analýzu zprávy (průměrné slov za zprávy) .</span><span class="sxs-lookup"><span data-stu-id="8b528-131">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="8b528-132">Zpráva je popsán `Message` – třída (*Data/Message.cs*) s dvě vlastnosti: `Id` (klíč) a `Text` (zprávy).</span><span class="sxs-lookup"><span data-stu-id="8b528-132">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="8b528-133">`Text` Vlastnost je nutné a omezena na 200 znaků.</span><span class="sxs-lookup"><span data-stu-id="8b528-133">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="8b528-134">Zprávy jsou uloženy pomocí [databáze v paměti rozhraní Entity Framework](/ef/core/providers/in-memory/)&#8224;.</span><span class="sxs-lookup"><span data-stu-id="8b528-134">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="8b528-135">Aplikace obsahuje vrstva přístupu k datům (DAL) v kontextu své databáze třídy `AppDbContext` (*Data/AppDbContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="8b528-135">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="8b528-136">DAL metody jsou označeny `virtual`, což umožňuje mocking metody pro použití v testech.</span><span class="sxs-lookup"><span data-stu-id="8b528-136">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="8b528-137">Pokud se databáze nachází prázdný při spuštění aplikace, se inicializuje tři zprávy úložiště zpráv.</span><span class="sxs-lookup"><span data-stu-id="8b528-137">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="8b528-138">Tyto *nasadí zprávy* se také používají v testech.</span><span class="sxs-lookup"><span data-stu-id="8b528-138">These *seeded messages* are also used in tests.</span></span>

<span data-ttu-id="8b528-139">&#8224;V tématu EF [testu s InMemory](/ef/core/miscellaneous/testing/in-memory), vysvětluje, jak používat databázi v paměti pro testování pomocí Mstestu.</span><span class="sxs-lookup"><span data-stu-id="8b528-139">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="8b528-140">Toto téma používá [xUnit](https://xunit.github.io/) test framework.</span><span class="sxs-lookup"><span data-stu-id="8b528-140">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="8b528-141">Test koncepty a testovací implementace napříč jiného testovacího architektury jsou podobné, ale nejsou identické.</span><span class="sxs-lookup"><span data-stu-id="8b528-141">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="8b528-142">I když se aplikace nepoužívá [použitému vzoru](http://martinfowler.com/eaaCatalog/repository.html) a není příklad efektivní [pracovní jednotky (UoW) vzor](https://martinfowler.com/eaaCatalog/unitOfWork.html), stránky Razor podporuje tyto vzory vývoj.</span><span class="sxs-lookup"><span data-stu-id="8b528-142">Although the app doesn't use the [repository pattern](http://martinfowler.com/eaaCatalog/repository.html) and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="8b528-143">Další informace najdete v tématu [navrhování vrstvu trvalosti infrastruktury](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [implementace úložiště a jednotky pracovních vzorů v aplikaci ASP.NET MVC](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), a [testovací kontroler Logika](/aspnet/core/mvc/controllers/testing) (ukázka implementuje vzor úložiště).</span><span class="sxs-lookup"><span data-stu-id="8b528-143">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="8b528-144">Testování aplikace organizace</span><span class="sxs-lookup"><span data-stu-id="8b528-144">Test app organization</span></span>

<span data-ttu-id="8b528-145">Testování aplikace je konzolovou aplikaci uvnitř *tests/RazorPagesTestSample.Tests* složky.</span><span class="sxs-lookup"><span data-stu-id="8b528-145">The test app is a console app inside the *tests/RazorPagesTestSample.Tests* folder.</span></span>

| <span data-ttu-id="8b528-146">Složky aplikace testu</span><span class="sxs-lookup"><span data-stu-id="8b528-146">Test app folder</span></span> | <span data-ttu-id="8b528-147">Popis</span><span class="sxs-lookup"><span data-stu-id="8b528-147">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="8b528-148">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="8b528-148">*UnitTests*</span></span>     | <ul><li><span data-ttu-id="8b528-149">*DataAccessLayerTest.cs* obsahuje testování částí pro DAL.</span><span class="sxs-lookup"><span data-stu-id="8b528-149">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="8b528-150">*IndexPageTests.cs* obsahuje testování částí modelu Index stránky.</span><span class="sxs-lookup"><span data-stu-id="8b528-150">*IndexPageTests.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="8b528-151">*Nástroje*</span><span class="sxs-lookup"><span data-stu-id="8b528-151">*Utilities*</span></span>     | <span data-ttu-id="8b528-152">Obsahuje `TestingDbContextOptions` metodu použitou k vytvoření nové databáze kontextu možnosti pro každou testování částí DAL tak, aby se obnovení databáze do stavu standardních hodnot pro každý test.</span><span class="sxs-lookup"><span data-stu-id="8b528-152">Contains the `TestingDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span> |

<span data-ttu-id="8b528-153">Testovací prostředí je [xUnit](https://xunit.github.io/).</span><span class="sxs-lookup"><span data-stu-id="8b528-153">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="8b528-154">Objekt mocking framework [Moq](https://github.com/moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="8b528-154">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span>

## <a name="unit-tests-of-the-data-access-layer-dal"></a><span data-ttu-id="8b528-155">Testování částí dat přístup layer (DAL)</span><span class="sxs-lookup"><span data-stu-id="8b528-155">Unit tests of the data access layer (DAL)</span></span>

<span data-ttu-id="8b528-156">Zpráva k němu má aplikace DAL s čtyři metody, které jsou součástí `AppDbContext` – třída (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="8b528-156">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="8b528-157">Každá z metod má jedno nebo dvě testování částí v testování aplikace.</span><span class="sxs-lookup"><span data-stu-id="8b528-157">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="8b528-158">DAL – metoda</span><span class="sxs-lookup"><span data-stu-id="8b528-158">DAL method</span></span>               | <span data-ttu-id="8b528-159">Funkce</span><span class="sxs-lookup"><span data-stu-id="8b528-159">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="8b528-160">Získá `List<Message>` z databáze, seřazené podle `Text` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8b528-160">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="8b528-161">Přidá `Message` do databáze.</span><span class="sxs-lookup"><span data-stu-id="8b528-161">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="8b528-162">Odstraní všechna `Message` záznamy z databáze.</span><span class="sxs-lookup"><span data-stu-id="8b528-162">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="8b528-163">Odstraní jeden `Message` z databáze pomocí `Id`.</span><span class="sxs-lookup"><span data-stu-id="8b528-163">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="8b528-164">Testy jednotek DAL vyžadují [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) při vytváření nového `AppDbContext` pro každý test.</span><span class="sxs-lookup"><span data-stu-id="8b528-164">Unit tests of the DAL require [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="8b528-165">Jeden ze způsobů vytvoření `DbContextOptions` pro každý test je použití [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span><span class="sxs-lookup"><span data-stu-id="8b528-165">One approach to creating the `DbContextOptions` for each test is to use a [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="8b528-166">Problém s tímto přístupem je, že každý test obdrží databáze ve stavu, předchozím testu nacházela.</span><span class="sxs-lookup"><span data-stu-id="8b528-166">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="8b528-167">To může být problém při pokusu o zápis testy atomic jednotek, které nekoliduje s navzájem.</span><span class="sxs-lookup"><span data-stu-id="8b528-167">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="8b528-168">Chcete-li vynutit `AppDbContext` Pokud chcete použít pro každý test nový kontext databáze, zadejte `DbContextOptions` instanci, která je založena na nového poskytovatele služby.</span><span class="sxs-lookup"><span data-stu-id="8b528-168">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="8b528-169">Testování aplikace ukazuje, jak to provést pomocí jeho `Utilities` třídy metoda `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="8b528-169">The test app shows how to do this using its `Utilities` class method `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="8b528-170">Pomocí `DbContextOptions` v jednotce DAL testy umožňuje spouštět atomicky s novou databází instance každého testu:</span><span class="sxs-lookup"><span data-stu-id="8b528-170">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="8b528-171">Každá metoda testu v `DataAccessLayerTest` – třída (*UnitTests/DataAccessLayerTest.cs*) pracuje podle vzoru uspořádat Act Assert:</span><span class="sxs-lookup"><span data-stu-id="8b528-171">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="8b528-172">Uspořádat: Databáze je nakonfigurovaná pro test nebo je definován očekávaný výsledek.</span><span class="sxs-lookup"><span data-stu-id="8b528-172">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="8b528-173">AKT: Test se spustí.</span><span class="sxs-lookup"><span data-stu-id="8b528-173">Act: The test is executed.</span></span>
1. <span data-ttu-id="8b528-174">Assert –: Kontrolní výrazy jsou vytvářeny k určení, pokud je výsledek testu úspěšné.</span><span class="sxs-lookup"><span data-stu-id="8b528-174">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="8b528-175">Například `DeleteMessageAsync` metoda odpovídá za odebrání do jedné zprávy identifikovaný jeho `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span><span class="sxs-lookup"><span data-stu-id="8b528-175">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="8b528-176">Existují dva testy pro tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="8b528-176">There are two tests for this method.</span></span> <span data-ttu-id="8b528-177">Jeden test kontroluje, že metoda po zprávy se nachází v databázi odstraní zprávu.</span><span class="sxs-lookup"><span data-stu-id="8b528-177">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="8b528-178">Ostatní metody testy, které databázi nezmění, pokud zpráva `Id` pro odstranění neexistuje.</span><span class="sxs-lookup"><span data-stu-id="8b528-178">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="8b528-179">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Metoda jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="8b528-179">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="8b528-180">Nejprve metoda provádí uspořádat krok, kde probíhá příprava pro krok akce.</span><span class="sxs-lookup"><span data-stu-id="8b528-180">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="8b528-181">Synchronizace replik indexů zprávy jsou získány a uchovávat v `seedMessages`.</span><span class="sxs-lookup"><span data-stu-id="8b528-181">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="8b528-182">Synchronizace replik indexů zprávy se uloží do databáze.</span><span class="sxs-lookup"><span data-stu-id="8b528-182">The seeding messages are saved into the database.</span></span> <span data-ttu-id="8b528-183">Zpráva s `Id` z `1` je nastaven pro odstranění.</span><span class="sxs-lookup"><span data-stu-id="8b528-183">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="8b528-184">Když `DeleteMessageAsync` spuštění metody, očekávané zprávy by měly mít všechny zprávy s výjimkou toho s `Id` z `1`.</span><span class="sxs-lookup"><span data-stu-id="8b528-184">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="8b528-185">`expectedMessages` Proměnná představuje tento očekávaný výsledek.</span><span class="sxs-lookup"><span data-stu-id="8b528-185">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="8b528-186">Metoda funguje: `DeleteMessageAsync` metoda spuštěna předávání v `recId` z `1`:</span><span class="sxs-lookup"><span data-stu-id="8b528-186">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="8b528-187">Nakonec získá metodu `Messages` z kontextu a porovná ho do `expectedMessages` potvrzující, zda jsou si rovny dva:</span><span class="sxs-lookup"><span data-stu-id="8b528-187">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="8b528-188">Chcete-li porovnat, dva `List<Message>` jsou stejné:</span><span class="sxs-lookup"><span data-stu-id="8b528-188">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="8b528-189">Zprávy jsou seřazené podle `Id`.</span><span class="sxs-lookup"><span data-stu-id="8b528-189">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="8b528-190">Zpráva páry jsou porovnávány na `Text` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8b528-190">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="8b528-191">Podobné metody test, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` zkontroluje výsledek pokusu o odstranění zprávu, která neexistuje.</span><span class="sxs-lookup"><span data-stu-id="8b528-191">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="8b528-192">V takovém případě musí být roven skutečné zprávy po očekávané zprávy v databázi `DeleteMessageAsync` metoda spuštěna.</span><span class="sxs-lookup"><span data-stu-id="8b528-192">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="8b528-193">Měla by existovat žádná změna databáze obsahu:</span><span class="sxs-lookup"><span data-stu-id="8b528-193">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a><span data-ttu-id="8b528-194">Testování částí modelu metod stránky</span><span class="sxs-lookup"><span data-stu-id="8b528-194">Unit tests of the page model methods</span></span>

<span data-ttu-id="8b528-195">Další sadu testů jednotek je zodpovědná za testy metody modelu stránky.</span><span class="sxs-lookup"><span data-stu-id="8b528-195">Another set of unit tests is responsible for tests of page model methods.</span></span> <span data-ttu-id="8b528-196">V aplikaci zprávy jsou součástí modely stránky indexu `IndexModel` třídy v *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="8b528-196">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="8b528-197">Metoda modelu stránky</span><span class="sxs-lookup"><span data-stu-id="8b528-197">Page model method</span></span> | <span data-ttu-id="8b528-198">Funkce</span><span class="sxs-lookup"><span data-stu-id="8b528-198">Function</span></span> |
| ----------------- | -------- |
| `OnGetAsync` | <span data-ttu-id="8b528-199">Získá zprávy z DAL pomocí uživatelského rozhraní `GetMessagesAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="8b528-199">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="8b528-200">Pokud `ModelState` je platný, vyvolá `AddMessageAsync` přidat zprávu do databáze.</span><span class="sxs-lookup"><span data-stu-id="8b528-200">If the `ModelState` is valid, calls `AddMessageAsync` to add a message to the database.</span></span> |
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="8b528-201">Volání `DeleteAllMessagesAsync` odstranit všechny zprávy v databázi.</span><span class="sxs-lookup"><span data-stu-id="8b528-201">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="8b528-202">Provede `DeleteMessageAsync` odstranění zprávy s `Id` zadaný.</span><span class="sxs-lookup"><span data-stu-id="8b528-202">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="8b528-203">Pokud jeden nebo více zpráv v databázi, vypočítá průměrné počty slov za zprávy.</span><span class="sxs-lookup"><span data-stu-id="8b528-203">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="8b528-204">Model metody stránky se zkoušejí sedm testy `IndexPageTests` – třída (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span><span class="sxs-lookup"><span data-stu-id="8b528-204">The page model methods are tested using seven tests in the `IndexPageTests` class (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span></span> <span data-ttu-id="8b528-205">Testy pomocí známých vzor uspořádat Assert akce.</span><span class="sxs-lookup"><span data-stu-id="8b528-205">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="8b528-206">Tyto testy zaměřit se na:</span><span class="sxs-lookup"><span data-stu-id="8b528-206">These tests focus on:</span></span>

* <span data-ttu-id="8b528-207">Určení, pokud metody, postupujte podle kroků správné chování při `ModelState` je neplatný.</span><span class="sxs-lookup"><span data-stu-id="8b528-207">Determining if the methods follow the correct behavior when the `ModelState` is invalid.</span></span>
* <span data-ttu-id="8b528-208">Potvrzení metody vytvoření správné `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="8b528-208">Confirming the methods produce the correct `IActionResult`.</span></span>
* <span data-ttu-id="8b528-209">Kontroluje, že jsou správně provedeny přiřazení hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8b528-209">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="8b528-210">Tato skupina testů často model metody vrstvy DAL k vytvoření očekávaná data pro Act krok, kde je stránka modelu metoda spuštěna.</span><span class="sxs-lookup"><span data-stu-id="8b528-210">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="8b528-211">Například `GetMessagesAsync` metodu `AppDbContext` je mocked k vytváření výstupu.</span><span class="sxs-lookup"><span data-stu-id="8b528-211">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="8b528-212">Když metoda modelu stránky provede tuto metodu, model vrátí výsledek.</span><span class="sxs-lookup"><span data-stu-id="8b528-212">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="8b528-213">Data nepřejde do stavu z databáze.</span><span class="sxs-lookup"><span data-stu-id="8b528-213">The data doesn't come from the database.</span></span> <span data-ttu-id="8b528-214">Tím se vytvoří testovací předvídatelný a spolehlivé podmínky pro použití DAL v testech modelu stránky.</span><span class="sxs-lookup"><span data-stu-id="8b528-214">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="8b528-215">`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` Testování ukazuje jak `GetMessagesAsync` metoda je mocked modelu stránky:</span><span class="sxs-lookup"><span data-stu-id="8b528-215">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="8b528-216">Když `OnGetAsync` metoda se spustí v kroku akce, volá model stránky `GetMessagesAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="8b528-216">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="8b528-217">Jednotkové testování krok Application Compatibility Toolkit (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span><span class="sxs-lookup"><span data-stu-id="8b528-217">Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="8b528-218">`IndexPage` model stránky `OnGetAsync` – metoda (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="8b528-218">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="8b528-219">`GetMessagesAsync` Metoda v DAL nevrací výsledek pro toto volání metody.</span><span class="sxs-lookup"><span data-stu-id="8b528-219">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="8b528-220">Mocked verzi metoda vrací výsledek.</span><span class="sxs-lookup"><span data-stu-id="8b528-220">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="8b528-221">V `Assert` krok, skutečný zprávy (`actualMessages`) jsou přiřazeny z `Messages` vlastnost modelu stránky.</span><span class="sxs-lookup"><span data-stu-id="8b528-221">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="8b528-222">Kontrola typu také provádí, když jsou přiřazeny zprávy.</span><span class="sxs-lookup"><span data-stu-id="8b528-222">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="8b528-223">Zprávy očekávaných a aktuálních jsou porovnávány na základě jejich `Text` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8b528-223">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="8b528-224">Vyhodnotí test, který dva `List<Message>` instance obsahují stejné zprávy.</span><span class="sxs-lookup"><span data-stu-id="8b528-224">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

<span data-ttu-id="8b528-225">Jiné testy v této skupině vytvořit stránku objekty modelu, které zahrnují `DefaultHttpContext`, `ModelStateDictionary`, `ActionContext` k navázání `PageContext`, `ViewDataDictionary`a `PageContext`.</span><span class="sxs-lookup"><span data-stu-id="8b528-225">Other tests in this group create page model objects that include the `DefaultHttpContext`, the `ModelStateDictionary`, an `ActionContext` to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="8b528-226">Tyto jsou užitečné při provádění testů.</span><span class="sxs-lookup"><span data-stu-id="8b528-226">These are useful in conducting tests.</span></span> <span data-ttu-id="8b528-227">Například zpráva aplikace vytváří `ModelState` chyba s `AddModelError` zkontroluje, jestli platná `PageResult` je vrácena, pokud `OnPostAddMessageAsync` se spustí:</span><span class="sxs-lookup"><span data-stu-id="8b528-227">For example, the message app establishes a `ModelState` error with `AddModelError` to check that a valid `PageResult` is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a><span data-ttu-id="8b528-228">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8b528-228">Additional resources</span></span>

* [<span data-ttu-id="8b528-229">Testování C# v .NET Core pomocí testovacích dotnet a xUnit částí</span><span class="sxs-lookup"><span data-stu-id="8b528-229">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="8b528-230">Testovací kontrolery</span><span class="sxs-lookup"><span data-stu-id="8b528-230">Test controllers</span></span>](xref:mvc/controllers/testing)
* <span data-ttu-id="8b528-231">[Testování částí kódu](/visualstudio/test/unit-test-your-code) (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="8b528-231">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* [<span data-ttu-id="8b528-232">Integrační testy</span><span class="sxs-lookup"><span data-stu-id="8b528-232">Integration tests</span></span>](xref:test/integration-tests)
* [<span data-ttu-id="8b528-233">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="8b528-233">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="8b528-234">Začínáme s xUnit.net (.NET Core/ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8b528-234">Getting started with xUnit.net (.NET Core/ASP.NET Core)</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="8b528-235">Moq</span><span class="sxs-lookup"><span data-stu-id="8b528-235">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="8b528-236">Moq rychlý start</span><span class="sxs-lookup"><span data-stu-id="8b528-236">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)
