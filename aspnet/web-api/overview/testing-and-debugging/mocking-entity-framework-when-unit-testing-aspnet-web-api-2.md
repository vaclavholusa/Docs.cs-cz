---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Vytvoření modelu Entity Framework při testování rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: tfitzmac
description: Tento průvodce a aplikace ukazují, jak vytvořit testy jednotek pro aplikace webového rozhraní API 2, která používá Entity Framework. Ukazuje, jak změnit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f1ff2fda85a6d56a6bbb76b1bff740301ab0c70d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371866"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="fa908-104">Vytvoření modelu Entity Framework při testování rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="fa908-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="fa908-105">podle [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fa908-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="fa908-106">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="fa908-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="fa908-107">Tento průvodce a aplikace ukazují, jak vytvořit testy jednotek pro aplikace webového rozhraní API 2, která používá Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fa908-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="fa908-108">Ukazuje, jak změnit automaticky generovaný kontroleru k povolení předávání objekt kontextu pro testování a jak vytvořit testovací objekty, které pracují s Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fa908-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
> 
> <span data-ttu-id="fa908-109">Úvod do testování jednotek v rozhraní ASP.NET Web API najdete v tématu [jednotkové testování v ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="fa908-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
> 
> <span data-ttu-id="fa908-110">Tento kurz předpokládá, že jste obeznámeni se základními koncepcemi rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="fa908-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="fa908-111">Úvodní tutoriál naleznete v tématu [Začínáme s ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="fa908-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fa908-112">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="fa908-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="fa908-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="fa908-113">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="fa908-114">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="fa908-114">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="fa908-115">V tomto tématu</span><span class="sxs-lookup"><span data-stu-id="fa908-115">In this topic</span></span>

<span data-ttu-id="fa908-116">Toto téma obsahuje následující oddíly:</span><span class="sxs-lookup"><span data-stu-id="fa908-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="fa908-117">Požadované součásti</span><span class="sxs-lookup"><span data-stu-id="fa908-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="fa908-118">Stáhnout kód</span><span class="sxs-lookup"><span data-stu-id="fa908-118">Download code</span></span>](#download)
- [<span data-ttu-id="fa908-119">Vytvoření aplikace pomocí projektu testů jednotek</span><span class="sxs-lookup"><span data-stu-id="fa908-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="fa908-120">Vytvoření tříd modelu</span><span class="sxs-lookup"><span data-stu-id="fa908-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="fa908-121">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="fa908-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="fa908-122">Přidat injektáž závislostí</span><span class="sxs-lookup"><span data-stu-id="fa908-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="fa908-123">Instalace balíčků NuGet v projektu testů</span><span class="sxs-lookup"><span data-stu-id="fa908-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="fa908-124">Vytvoření kontextu testu</span><span class="sxs-lookup"><span data-stu-id="fa908-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="fa908-125">Vytváření testů</span><span class="sxs-lookup"><span data-stu-id="fa908-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="fa908-126">Spuštění testů</span><span class="sxs-lookup"><span data-stu-id="fa908-126">Run tests</span></span>](#runtests)

<span data-ttu-id="fa908-127">Pokud jste již dokončili kroky v [jednotkové testování v ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), můžete přeskočit k části [přidat kontroler](#controller).</span><span class="sxs-lookup"><span data-stu-id="fa908-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="fa908-128">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fa908-128">Prerequisites</span></span>

<span data-ttu-id="fa908-129">Visual Studio 2017 Community, Professional nebo Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="fa908-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="fa908-130">Stáhnout kód</span><span class="sxs-lookup"><span data-stu-id="fa908-130">Download code</span></span>

<span data-ttu-id="fa908-131">Stáhněte si [dokončený projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="fa908-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="fa908-132">Ke stažení projekt zahrnuje kód testu jednotek pro toto téma a [jednotky testování ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="fa908-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="fa908-133">Vytvoření aplikace pomocí projektu testů jednotek</span><span class="sxs-lookup"><span data-stu-id="fa908-133">Create application with unit test project</span></span>

<span data-ttu-id="fa908-134">Můžete buď při vytváření aplikace vytvořit projekt testování částí nebo přidat projekt testování částí do stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa908-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="fa908-135">Tento kurz ukazuje vytvoření projektu testů jednotek při vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa908-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="fa908-136">Vytvořit novou webovou aplikaci ASP.NET s názvem **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="fa908-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="fa908-137">V oknech Nový projekt ASP.NET, vyberte **prázdný** šablony a přidat složky a základní odkazy pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fa908-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="fa908-138">Vyberte **přidání jednotkových testů** možnost.</span><span class="sxs-lookup"><span data-stu-id="fa908-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="fa908-139">Automaticky s názvem projektu testování částí **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="fa908-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="fa908-140">Abyste mohli tento název.</span><span class="sxs-lookup"><span data-stu-id="fa908-140">You can keep this name.</span></span>

![Vytvořte projekt testu jednotek](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="fa908-142">Po vytvoření aplikace, zobrazí se obsahuje dva projekty – **StoreApp** a **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="fa908-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="fa908-143">Vytvoření tříd modelu</span><span class="sxs-lookup"><span data-stu-id="fa908-143">Create the model class</span></span>

<span data-ttu-id="fa908-144">V projektu StoreApp, přidejte soubor třídy do **modely** složku s názvem **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="fa908-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="fa908-145">Obsah souboru nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="fa908-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="fa908-146">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="fa908-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="fa908-147">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="fa908-147">Add the controller</span></span>

<span data-ttu-id="fa908-148">Klikněte pravým tlačítkem na složku řadiče a vyberte **přidat** a **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="fa908-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="fa908-149">Vyberte kontroler rozhraní Web API 2 s akcemi používající nástroj Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fa908-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Přidat nový kontroler](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="fa908-151">Nastavte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="fa908-151">Set the following values:</span></span>

- <span data-ttu-id="fa908-152">Název kontroleru: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="fa908-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="fa908-153">Třída modelu: **produktu**</span><span class="sxs-lookup"><span data-stu-id="fa908-153">Model class: **Product**</span></span>
- <span data-ttu-id="fa908-154">Třída kontextu dat: [vyberte **nový kontext dat** tlačítko, které vyplní hodnoty vidět níže]</span><span class="sxs-lookup"><span data-stu-id="fa908-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![Zadejte kontroler](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="fa908-156">Klikněte na tlačítko **přidat** vytvoření kontroleru s automaticky generovaným kódem.</span><span class="sxs-lookup"><span data-stu-id="fa908-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="fa908-157">Tento kód obsahuje metody pro vytváření, načítání, aktualizaci a odstranění instancí třídy produktu.</span><span class="sxs-lookup"><span data-stu-id="fa908-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="fa908-158">Následující kód ukazuje metody pro přidání produktu.</span><span class="sxs-lookup"><span data-stu-id="fa908-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="fa908-159">Všimněte si, že metoda vrací instanci **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="fa908-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="fa908-160">IHttpActionResult je jedním z nových funkcí ve webovém rozhraní API 2 a zjednodušuje vývoj jednotkových testů.</span><span class="sxs-lookup"><span data-stu-id="fa908-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="fa908-161">V další části, bude přizpůsobení generovaný kód pro usnadnění předávání objektů testu pro kontroler.</span><span class="sxs-lookup"><span data-stu-id="fa908-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="fa908-162">Přidat injektáž závislostí</span><span class="sxs-lookup"><span data-stu-id="fa908-162">Add dependency injection</span></span>

<span data-ttu-id="fa908-163">V současné době je třída ProductController pevně určený instance třídy StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="fa908-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="fa908-164">Ke změně aplikace a odebrat dané pevně zakódované závislosti použijete vzor volá vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="fa908-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="fa908-165">Porušením této závislosti, můžete předat objekt mock při testování.</span><span class="sxs-lookup"><span data-stu-id="fa908-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="fa908-166">Klikněte pravým tlačítkem myši **modely** složky a přidejte nové rozhraní s názvem **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="fa908-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="fa908-167">Nahraďte kód následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="fa908-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="fa908-168">Otevřete soubor StoreAppContext.cs a měnit následující zvýrazněný.</span><span class="sxs-lookup"><span data-stu-id="fa908-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="fa908-169">Všimněte si důležité změny jsou:</span><span class="sxs-lookup"><span data-stu-id="fa908-169">The important changes to note are:</span></span>

- <span data-ttu-id="fa908-170">StoreAppContext třída implementuje rozhraní IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="fa908-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="fa908-171">Metoda MarkAsModified je implementována.</span><span class="sxs-lookup"><span data-stu-id="fa908-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="fa908-172">Otevřete soubor ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="fa908-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="fa908-173">Změna existujícího kódu tak, aby odpovídaly zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="fa908-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="fa908-174">Tyto změny na StoreAppContext přerušit závislosti a jiné třídy a zajistěte tak předání jiný objekt třídy kontextu povolit.</span><span class="sxs-lookup"><span data-stu-id="fa908-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="fa908-175">Tato změna vám umožní předat během testování částí v kontextu testu.</span><span class="sxs-lookup"><span data-stu-id="fa908-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="fa908-176">Existuje jeden další změny, které musíte udělat v ProductController.</span><span class="sxs-lookup"><span data-stu-id="fa908-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="fa908-177">V **PutProduct** metody, nahraďte řádek, který nastaví stav entity upravit pomocí volání metody MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="fa908-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="fa908-178">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="fa908-178">Build the solution.</span></span>

<span data-ttu-id="fa908-179">Nyní jste připraveni nastavit testovacího projektu.</span><span class="sxs-lookup"><span data-stu-id="fa908-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="fa908-180">Instalace balíčků NuGet v projektu testů</span><span class="sxs-lookup"><span data-stu-id="fa908-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="fa908-181">Když použijete prázdnou šablonu k vytvoření aplikace, projekt testu jednotek (StoreApp.Tests) neobsahuje žádné nainstalované balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="fa908-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="fa908-182">Další šablony, jako je například šabloně webového rozhraní API patří některé balíčky NuGet v projektu testování částí.</span><span class="sxs-lookup"><span data-stu-id="fa908-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="fa908-183">Pro účely tohoto kurzu musíte zahrnout balíčku Entity Framework a balíček Microsoft ASP.NET Web API 2 Core do testovacího projektu.</span><span class="sxs-lookup"><span data-stu-id="fa908-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="fa908-184">Klikněte pravým tlačítkem na projekt StoreApp.Tests a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fa908-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="fa908-185">Je nutné vybrat projekt StoreApp.Tests přidat balíčky pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="fa908-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Správa balíčků](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="fa908-187">Z Online balíčků najít a nainstalovat balíček EntityFramework (verze 6.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="fa908-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="fa908-188">Pokud se zdá, že EntityFramework balíček je už nainstalovaný, možná jste vybrali StoreApp projektu namísto StoreApp.Tests projektu.</span><span class="sxs-lookup"><span data-stu-id="fa908-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Přidání Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="fa908-190">Najít a nainstalovat balíček Microsoft ASP.NET Web API 2 jádra.</span><span class="sxs-lookup"><span data-stu-id="fa908-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Nainstalujte balíček základního webového rozhraní api](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="fa908-192">Zavřete okno Spravovat balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="fa908-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="fa908-193">Vytvoření kontextu testu</span><span class="sxs-lookup"><span data-stu-id="fa908-193">Create test context</span></span>

<span data-ttu-id="fa908-194">Přidejte třídu pojmenovanou **TestDbSet** do testovacího projektu.</span><span class="sxs-lookup"><span data-stu-id="fa908-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="fa908-195">Tato třída slouží jako základní třída pro vaši datovou sadu testů.</span><span class="sxs-lookup"><span data-stu-id="fa908-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="fa908-196">Nahraďte kód následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="fa908-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="fa908-197">Přidejte třídu pojmenovanou **TestProductDbSet** do testovacího projektu, který obsahuje následující kód.</span><span class="sxs-lookup"><span data-stu-id="fa908-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="fa908-198">Přidejte třídu pojmenovanou **TestStoreAppContext** a nahraďte existující kód následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="fa908-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="fa908-199">Vytváření testů</span><span class="sxs-lookup"><span data-stu-id="fa908-199">Create tests</span></span>

<span data-ttu-id="fa908-200">Ve výchozím nastavení, testovací projekt obsahuje soubor prázdný test s názvem **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="fa908-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="fa908-201">Tento soubor obsahuje atributy, že pomocí kterých lze vytvářet testovací metody.</span><span class="sxs-lookup"><span data-stu-id="fa908-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="fa908-202">Pro účely tohoto kurzu můžete odstranit tento soubor, protože přidáte novou třídu testu.</span><span class="sxs-lookup"><span data-stu-id="fa908-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="fa908-203">Přidejte třídu pojmenovanou **TestProductController** do testovacího projektu.</span><span class="sxs-lookup"><span data-stu-id="fa908-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="fa908-204">Nahraďte kód následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="fa908-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="fa908-205">Spouštění testů</span><span class="sxs-lookup"><span data-stu-id="fa908-205">Run tests</span></span>

<span data-ttu-id="fa908-206">Nyní jste připraveni ke spuštění testů.</span><span class="sxs-lookup"><span data-stu-id="fa908-206">You are now ready to run the tests.</span></span> <span data-ttu-id="fa908-207">Všechny metody, která jsou označena **TestMethod** atribut bude ověřovat.</span><span class="sxs-lookup"><span data-stu-id="fa908-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="fa908-208">Z **Test** položku nabídky, spusťte testy.</span><span class="sxs-lookup"><span data-stu-id="fa908-208">From the **Test** menu item, run the tests.</span></span>

![Provádění testů](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="fa908-210">Otevřít **Průzkumník testů** okně a Všimněte si, že výsledky testů.</span><span class="sxs-lookup"><span data-stu-id="fa908-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![výsledky testu](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
