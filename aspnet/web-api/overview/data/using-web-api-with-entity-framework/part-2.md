---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Přidat modely a řadičů | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 015bb9698d81387d03ea8f9629316fb53232e708
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="add-models-and-controllers"></a><span data-ttu-id="b77a8-102">Přidání řadiče a modely</span><span class="sxs-lookup"><span data-stu-id="b77a8-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="b77a8-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b77a8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b77a8-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="b77a8-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="b77a8-105">V této části přidáte třídy modelu, které definují entity databáze.</span><span class="sxs-lookup"><span data-stu-id="b77a8-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="b77a8-106">Potom přidáte řadiče webového rozhraní API, které provádění operací CRUD tyto entity.</span><span class="sxs-lookup"><span data-stu-id="b77a8-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="b77a8-107">Přidání třídy modelu</span><span class="sxs-lookup"><span data-stu-id="b77a8-107">Add Model Classes</span></span>

<span data-ttu-id="b77a8-108">V tomto kurzu vytvoříme databázi pomocí "Code First" přístup k Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="b77a8-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="b77a8-109">S Code First zápisu třídy jazyka C#, které odpovídají tabulkách databáze a EF vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="b77a8-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="b77a8-110">(Další informace najdete v tématu [Entity Framework vývoj přístupy](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="b77a8-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="b77a8-111">Začneme definováním naše objektů domény jako POCOs (prostý starý CLR objekty).</span><span class="sxs-lookup"><span data-stu-id="b77a8-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="b77a8-112">Vytvoříme POCOs následující:</span><span class="sxs-lookup"><span data-stu-id="b77a8-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="b77a8-113">Autor</span><span class="sxs-lookup"><span data-stu-id="b77a8-113">Author</span></span>
- <span data-ttu-id="b77a8-114">Adresáře</span><span class="sxs-lookup"><span data-stu-id="b77a8-114">Book</span></span>

<span data-ttu-id="b77a8-115">V Průzkumníku řešení klikněte pravým tlačítkem složku modely.</span><span class="sxs-lookup"><span data-stu-id="b77a8-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="b77a8-116">Vyberte **přidat**, pak vyberte **třída**.</span><span class="sxs-lookup"><span data-stu-id="b77a8-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="b77a8-117">Název třídy `Author`.</span><span class="sxs-lookup"><span data-stu-id="b77a8-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="b77a8-118">Všechny standardní kód Author.cs nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="b77a8-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="b77a8-119">Přidejte jinou třídu s názvem `Book`, s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="b77a8-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="b77a8-120">Rozhraní Entity Framework použije tyto modely k vytvoření databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="b77a8-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="b77a8-121">Pro každý model `Id` vlastnost bude sloupec primárního klíče tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="b77a8-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="b77a8-122">Ve třídě adresáře `AuthorId` definuje cizí klíč do `Author` tabulky.</span><span class="sxs-lookup"><span data-stu-id="b77a8-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="b77a8-123">(Pro jednoduchost, I mě za předpokladu, že každý seznam má jeden Autor.) Třída kniha také obsahuje vlastnost navigace, která má související `Author`.</span><span class="sxs-lookup"><span data-stu-id="b77a8-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="b77a8-124">Navigační vlastnost lze použít pro přístup k související `Author` v kódu.</span><span class="sxs-lookup"><span data-stu-id="b77a8-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="b77a8-125">Další informace o navigačních vlastností v rámci 4, říci [zpracování vztahy entit](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="b77a8-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="b77a8-126">Přidat Kontrolerů webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="b77a8-126">Add Web API Controllers</span></span>

<span data-ttu-id="b77a8-127">V této části přidáme řadiče webového rozhraní API, které podporují operace CRUD (vytvářet, číst, aktualizovat a odstranit).</span><span class="sxs-lookup"><span data-stu-id="b77a8-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="b77a8-128">V řadičích použije ke komunikaci s vrstva databáze Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b77a8-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="b77a8-129">Nejprve můžete odstranit soubor Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="b77a8-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="b77a8-130">Tento soubor obsahuje řadič příklad webového rozhraní API, ale nebudete ho potřebovat pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b77a8-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="b77a8-131">V dalším kroku sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="b77a8-131">Next, build the project.</span></span> <span data-ttu-id="b77a8-132">Generování uživatelského rozhraní Web API používá k nalezení třídy modelu, proto potřebuje kompilované sestavení reflexe.</span><span class="sxs-lookup"><span data-stu-id="b77a8-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="b77a8-133">V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="b77a8-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="b77a8-134">Vyberte **přidat**, pak vyberte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="b77a8-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="b77a8-135">V **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte možnost "Web API 2 řadiče s akcemi používající rozhraní Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="b77a8-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="b77a8-136">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b77a8-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="b77a8-137">V **přidat kontroler** dialogové okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b77a8-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="b77a8-138">V **třída modelu** rozevíracího seznamu, vyberte `Author` třídy.</span><span class="sxs-lookup"><span data-stu-id="b77a8-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="b77a8-139">(Pokud ho uvedené v rozevírací nabídce nevidíte, ujistěte se, že jste vytvořili projekt.)</span><span class="sxs-lookup"><span data-stu-id="b77a8-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="b77a8-140">Zkontrolujte "Použít asynchronní akce kontroleru".</span><span class="sxs-lookup"><span data-stu-id="b77a8-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="b77a8-141">Ponechejte pole název řadiče jako &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="b77a8-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="b77a8-142">Klikněte na tlačítko plus (+) vedle tlačítko **třída kontextu dat**.</span><span class="sxs-lookup"><span data-stu-id="b77a8-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="b77a8-143">V **nový kontext dat** dialogové okno, ponechte výchozí název a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b77a8-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="b77a8-144">Klikněte na tlačítko **přidat** k dokončení **přidat kontroler** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b77a8-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="b77a8-145">Dialogové okno přidá dvě třídy do projektu:</span><span class="sxs-lookup"><span data-stu-id="b77a8-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="b77a8-146">`AuthorsController` Definuje kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b77a8-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="b77a8-147">Řadičem implementuje rozhraní REST API, který klienti používat k provádění operací CRUD v seznamu autoři.</span><span class="sxs-lookup"><span data-stu-id="b77a8-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="b77a8-148">`BookServiceContext` Spravuje objekty entity za běhu, včetně vyplnění objekty s daty z databáze, sledování změn a zachování dat pro databázi.</span><span class="sxs-lookup"><span data-stu-id="b77a8-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="b77a8-149">Dědí z `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="b77a8-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="b77a8-150">V tomto okamžiku znovu sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="b77a8-150">At this point, build the project again.</span></span> <span data-ttu-id="b77a8-151">Nyní absolvovat stejný postup pro přidání řadič rozhraní API pro `Book` entity.</span><span class="sxs-lookup"><span data-stu-id="b77a8-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="b77a8-152">Tentokrát vyberte `Book` pro třídu modelu, vyberte existující `BookServiceContext` třída pro třídy kontextu dat.</span><span class="sxs-lookup"><span data-stu-id="b77a8-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="b77a8-153">(Nevytvářejte nový kontext data.) Klikněte na tlačítko **přidat** pro přidání řadiče.</span><span class="sxs-lookup"><span data-stu-id="b77a8-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="b77a8-154">[Předchozí](part-1.md)
> [další](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="b77a8-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
