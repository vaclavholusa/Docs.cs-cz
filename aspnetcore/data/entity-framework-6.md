---
title: "Začínáme s ASP.NET Core a Entity Framework 6"
author: tdykstra
description: "Tento článek ukazuje, jak používat Entity Framework 6 v aplikaci ASP.NET Core."
keywords: "Jádro ASP.NET, rozhraní Entity Framework EF 6"
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.assetid: 016cc836-4c43-45a4-b9a7-9efaf53350df
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 8abec95c591f20069e20eec55fd21503e74f8606
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="675d9-104">Začínáme s ASP.NET Core a Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="675d9-104">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="675d9-105">Podle [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), a [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="675d9-105">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="675d9-106">Tento článek ukazuje, jak používat Entity Framework 6 v aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="675d9-106">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="675d9-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="675d9-107">Overview</span></span>

<span data-ttu-id="675d9-108">Pokud chcete používat Entity Framework 6, váš projekt má zkompilovat pro rozhraní .NET Framework jako Entity Framework 6 nepodporuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="675d9-108">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="675d9-109">Pokud potřebujete více platforem funkce budete muset upgradovat na [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="675d9-109">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="675d9-110">Doporučeným způsobem, jak používat Entity Framework 6 v aplikaci ASP.NET Core je uvést kontext EF6 a třídy modelu v knihovně tříd rozhraní úplné projektu s cílem.</span><span class="sxs-lookup"><span data-stu-id="675d9-110">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="675d9-111">Přidáte odkaz na knihovnu tříd z projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="675d9-111">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="675d9-112">Viz ukázka [řešení sady Visual Studio s projekty EF6 a ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="675d9-112">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="675d9-113">Nelze blokovat kontextu EF6 v projektu ASP.NET Core, protože .NET Core projekty nepodporují všechny funkce, která EF6 příkazy, jako *Enable-Migrations* vyžadují.</span><span class="sxs-lookup"><span data-stu-id="675d9-113">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="675d9-114">Bez ohledu na typ projektu, ve kterém můžete najít váš kontext EF6 s kontextu EF6 pracovat pouze EF6 nástroje příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="675d9-114">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="675d9-115">Například `Scaffold-DbContext` je dostupný jenom v Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="675d9-115">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="675d9-116">Pokud potřebujete zpětná analýza databáze do EF6 model, přečtěte si téma [Code First k existující databázi](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="675d9-116">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="675d9-117">Odkaz na úplné framework a EF6 v projektu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="675d9-117">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="675d9-118">Projekt ASP.NET Core musí odkazovat na rozhraní .NET framework a EF6.</span><span class="sxs-lookup"><span data-stu-id="675d9-118">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="675d9-119">Například *.csproj* souboru projektu ASP.NET Core bude vypadat podobně jako v následujícím příkladu (jsou zobrazeny pouze odpovídající části souboru).</span><span class="sxs-lookup"><span data-stu-id="675d9-119">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="675d9-120">Pokud vytváříte nový projekt, použijte **webové aplikace ASP.NET Core (rozhraní .NET Framework)** šablony.</span><span class="sxs-lookup"><span data-stu-id="675d9-120">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="675d9-121">Popisovač připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="675d9-121">Handle connection strings</span></span>

<span data-ttu-id="675d9-122">EF6 nástroje příkazového řádku, které budete používat v projektu knihovny tříd EF6 vyžadují výchozí konstruktor, takže se můžete vytvořit instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="675d9-122">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="675d9-123">Ale budete pravděpodobně chtít zadejte připojovací řetězec k použití v projektu ASP.NET Core, v takovém případě váš kontext konstruktor musí mít parametr, který umožňuje předat v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="675d9-123">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="675d9-124">Tady je příklad.</span><span class="sxs-lookup"><span data-stu-id="675d9-124">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="675d9-125">Vzhledem k tomu, že váš kontext EF6 nemá konstruktor bez parametrů, musí poskytnout implementaci projektu EF6 [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="675d9-125">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="675d9-126">Nástroje příkazového řádku EF6 vyhledá a použít tuto implementaci, takže se můžete vytvořit instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="675d9-126">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="675d9-127">Tady je příklad.</span><span class="sxs-lookup"><span data-stu-id="675d9-127">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="675d9-128">V tento ukázkový kód `IDbContextFactory` implementace předá v pevně připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="675d9-128">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="675d9-129">Toto je připojovací řetězec, který bude používat nástroje příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="675d9-129">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="675d9-130">Budete chtít provádět strategii zajistit, že knihovna tříd používá stejný připojovací řetězec, který používá volající aplikace.</span><span class="sxs-lookup"><span data-stu-id="675d9-130">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="675d9-131">Například může získat hodnotu z proměnné prostředí v obou projektů.</span><span class="sxs-lookup"><span data-stu-id="675d9-131">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="675d9-132">Nastavit vkládání závislostí v projektu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="675d9-132">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="675d9-133">V projektu základní *Startup.cs* souboru, nastavte kontext EF6 pro vkládání závislostí (DI) v `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="675d9-133">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="675d9-134">Objektů kontextu EF by měl určené pro jednotlivé požadavky životnost.</span><span class="sxs-lookup"><span data-stu-id="675d9-134">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="675d9-135">Pak můžete získat instance kontextu v řadičích pomocí DI.</span><span class="sxs-lookup"><span data-stu-id="675d9-135">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="675d9-136">Kód je podobná by zápisu pro kontextu EF jádra:</span><span class="sxs-lookup"><span data-stu-id="675d9-136">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="675d9-137">Ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="675d9-137">Sample application</span></span>

<span data-ttu-id="675d9-138">Ukázkovou aplikaci, práce, najdete v článku [ukázkové řešení sady Visual Studio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) doprovodný v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="675d9-138">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="675d9-139">Tato ukázka lze vytvořit od začátku pomocí následujících kroků v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="675d9-139">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="675d9-140">Vytvoření řešení.</span><span class="sxs-lookup"><span data-stu-id="675d9-140">Create a solution.</span></span>

* <span data-ttu-id="675d9-141">**Přidat nový projekt > Web > ASP.NET Core webové aplikace (rozhraní .NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="675d9-141">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="675d9-142">**Přidat nový projekt > klasický desktopový Windows > třídy knihovny (rozhraní .NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="675d9-142">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="675d9-143">V **Konzola správce balíčků** (pomocí PMC) pro oba projekty, spusťte příkaz `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="675d9-143">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="675d9-144">V projektu knihovny tříd vytvořit datové třídy modelu a třídu kontextu a implementaci `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="675d9-144">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="675d9-145">Pomocí PMC pro projektu knihovny tříd, spusťte příkazy `Enable-Migrations` a `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="675d9-145">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="675d9-146">Pokud jste nastavili ASP.NET Core projekt jako spouštěný projekt, přidejte `-StartupProjectName EF6` tyto příkazy.</span><span class="sxs-lookup"><span data-stu-id="675d9-146">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="675d9-147">V projektu základní přidáte odkaz na projekt na projekt knihovny tříd.</span><span class="sxs-lookup"><span data-stu-id="675d9-147">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="675d9-148">V projektu jádra v *Startup.cs*, zaregistrovat kontext pro DI.</span><span class="sxs-lookup"><span data-stu-id="675d9-148">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="675d9-149">V projektu jádra v *appSettings.JSON určený*, přidejte připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="675d9-149">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="675d9-150">V projektu jádra přidejte řadiče a zobrazení, chcete-li ověřit, že můžete číst a zapisovat data.</span><span class="sxs-lookup"><span data-stu-id="675d9-150">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="675d9-151">(Všimněte si, že generování uživatelského rozhraní ASP.NET MVC základní nebudou fungovat s kontextem EF6 na něj odkazovat z knihovny tříd.)</span><span class="sxs-lookup"><span data-stu-id="675d9-151">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="675d9-152">Souhrn</span><span class="sxs-lookup"><span data-stu-id="675d9-152">Summary</span></span>

<span data-ttu-id="675d9-153">Tento článek poskytl základní pokyny pro používání Entity Framework 6 v aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="675d9-153">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="675d9-154">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="675d9-154">Additional Resources</span></span>

* [<span data-ttu-id="675d9-155">Rozhraní Entity Framework - konfigurace založené na kódu</span><span class="sxs-lookup"><span data-stu-id="675d9-155">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
