---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Přidání modelů a Kontrolerů | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 31e0fbf65c74a18677588cfa412dc34f7b25f78f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806355"
---
<a name="add-models-and-controllers"></a><span data-ttu-id="9e189-102">Přidání modelů a Kontrolerů</span><span class="sxs-lookup"><span data-stu-id="9e189-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="9e189-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9e189-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9e189-104">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="9e189-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="9e189-105">V této části přidáte tříd modelu, které definují entity databáze.</span><span class="sxs-lookup"><span data-stu-id="9e189-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="9e189-106">Potom přidáte kontrolerů rozhraní Web API, které provádějí operace CRUD s těmito entitami.</span><span class="sxs-lookup"><span data-stu-id="9e189-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="9e189-107">Přidání třídy modelu</span><span class="sxs-lookup"><span data-stu-id="9e189-107">Add Model Classes</span></span>

<span data-ttu-id="9e189-108">V tomto kurzu vytvoříme databázi s použitím "Code First" přístup do Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="9e189-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="9e189-109">S Code First třídy jazyka C#, které odpovídají zapíšete do databázové tabulky a EF vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="9e189-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="9e189-110">(Další informace najdete v tématu [vývojové přístupy s Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="9e189-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="9e189-111">Začneme tak, že definujete naše objektů domény jako POCOs (prostý staré CLR objekty).</span><span class="sxs-lookup"><span data-stu-id="9e189-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="9e189-112">Vytvoříme POCOs následující:</span><span class="sxs-lookup"><span data-stu-id="9e189-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="9e189-113">Autor</span><span class="sxs-lookup"><span data-stu-id="9e189-113">Author</span></span>
- <span data-ttu-id="9e189-114">Knihy</span><span class="sxs-lookup"><span data-stu-id="9e189-114">Book</span></span>

<span data-ttu-id="9e189-115">V Průzkumníku řešení klikněte pravým tlačítkem myši klikněte na složku modely.</span><span class="sxs-lookup"><span data-stu-id="9e189-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="9e189-116">Vyberte **přidat**a pak vyberte **třídy**.</span><span class="sxs-lookup"><span data-stu-id="9e189-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="9e189-117">Název třídy `Author`.</span><span class="sxs-lookup"><span data-stu-id="9e189-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="9e189-118">Často používaný kód v Author.cs nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="9e189-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="9e189-119">Přidejte další třídu s názvem `Book`, následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="9e189-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="9e189-120">Entity Framework pomocí těchto modelů vytvoření databázových tabulek.</span><span class="sxs-lookup"><span data-stu-id="9e189-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="9e189-121">Pro každý model `Id` vlastnost se stane sloupec primárního klíče tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="9e189-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="9e189-122">Ve třídě adresáře `AuthorId` definuje cizí klíč do `Author` tabulky.</span><span class="sxs-lookup"><span data-stu-id="9e189-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="9e189-123">(Pro jednoduchost, můžu jsem za předpokladu, že jednotlivé knihy má jeden Autor.) Kniha třída také obsahuje vlastnosti navigace použít k související `Author`.</span><span class="sxs-lookup"><span data-stu-id="9e189-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="9e189-124">Můžete použít navigační vlastnost pro přístup k související `Author` v kódu.</span><span class="sxs-lookup"><span data-stu-id="9e189-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="9e189-125">Další informace o vlastnosti navigace v části 4, řeknu [zpracování relací prvků](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="9e189-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="9e189-126">Přidat Kontrolerů webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9e189-126">Add Web API Controllers</span></span>

<span data-ttu-id="9e189-127">V této části přidáme kontrolerů rozhraní Web API, které podporují operace CRUD (vytváření, čtení, aktualizace a odstranění).</span><span class="sxs-lookup"><span data-stu-id="9e189-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="9e189-128">Kontrolery pomocí Entity Framework komunikovat s databázové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="9e189-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="9e189-129">Nejprve můžete odstranit soubor Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="9e189-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="9e189-130">Tento soubor obsahuje kontroler Web API příklad, ale nepotřebujete ho pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="9e189-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="9e189-131">V dalším kroku sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="9e189-131">Next, build the project.</span></span> <span data-ttu-id="9e189-132">Generování uživatelského rozhraní webového rozhraní API používá reflexi k nalezení tříd modelu, proto musí zkompilovaného sestavení.</span><span class="sxs-lookup"><span data-stu-id="9e189-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="9e189-133">V Průzkumníku řešení klikněte pravým tlačítkem myši na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="9e189-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="9e189-134">Vyberte **přidat**a pak vyberte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="9e189-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="9e189-135">V **přidat vygenerované uživatelské rozhraní** dialogového okna, vyberte možnost "webového rozhraní API 2 kontroler s akcemi používající nástroj Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="9e189-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="9e189-136">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9e189-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="9e189-137">V **přidat kontroler** dialogového okna, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="9e189-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="9e189-138">V **třída modelu** rozevírací seznam, vyberte `Author` třídy.</span><span class="sxs-lookup"><span data-stu-id="9e189-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="9e189-139">(Pokud se nezobrazuje v rozevíracím seznamu nezobrazí, ujistěte se, že jste sestavili projekt.)</span><span class="sxs-lookup"><span data-stu-id="9e189-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="9e189-140">Zkontrolujte "Použití asynchronní akce kontroleru".</span><span class="sxs-lookup"><span data-stu-id="9e189-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="9e189-141">Nechte název tak řadič jako &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="9e189-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="9e189-142">Klikněte na tlačítko plus (+) vedle tlačítka **třída kontextu dat**.</span><span class="sxs-lookup"><span data-stu-id="9e189-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="9e189-143">V **nový kontext dat.** dialogového okna, ponechte výchozí název a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9e189-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="9e189-144">Klikněte na tlačítko **přidat** Dokončit **přidat kontroler** dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="9e189-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="9e189-145">Dialog přidá dvě třídy do projektu:</span><span class="sxs-lookup"><span data-stu-id="9e189-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="9e189-146">`AuthorsController` Definuje kontroler Web API.</span><span class="sxs-lookup"><span data-stu-id="9e189-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="9e189-147">Kontroler implementuje rozhraní REST API, kterou klienti používají k provádění operací CRUD u seznamu autoři.</span><span class="sxs-lookup"><span data-stu-id="9e189-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="9e189-148">`BookServiceContext` Spravuje objekty entity za běhu, který obsahuje naplnění objekty s daty z databáze, řešení change tracking a zachovává data do databáze.</span><span class="sxs-lookup"><span data-stu-id="9e189-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="9e189-149">Dědí z `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9e189-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="9e189-150">V tomto okamžiku projekt znovu sestavte.</span><span class="sxs-lookup"><span data-stu-id="9e189-150">At this point, build the project again.</span></span> <span data-ttu-id="9e189-151">Nyní absolvovat stejný postup, chcete-li přidat kontroler API pro `Book` entity.</span><span class="sxs-lookup"><span data-stu-id="9e189-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="9e189-152">Tentokrát vyberte `Book` pro třídu modelu a vyberte existující `BookServiceContext` třída pro třídy datového kontextu.</span><span class="sxs-lookup"><span data-stu-id="9e189-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="9e189-153">(Nevytvářejte nový kontext dat.) Klikněte na tlačítko **přidat** přidat kontroler.</span><span class="sxs-lookup"><span data-stu-id="9e189-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="9e189-154">[Předchozí](part-1.md)
> [další](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="9e189-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
