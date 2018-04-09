---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Přístup k vašemu modelu datům z řadiče | Microsoft Docs
author: shanselman
description: Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoření jednoduché webové aplikace, která čte a zapisuje z databáze.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="6be9b-104">Přístup k vašemu modelu datům z řadiče</span><span class="sxs-lookup"><span data-stu-id="6be9b-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="6be9b-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="6be9b-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="6be9b-106">Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6be9b-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="6be9b-107">Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.</span><span class="sxs-lookup"><span data-stu-id="6be9b-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="6be9b-108">Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6be9b-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="6be9b-109">V této části jsme se chystáte vytvořit novou třídu MoviesController a napsat kód, naše film data načte a zobrazí zpět do prohlížeče pomocí šablony zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6be9b-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="6be9b-110">Klikněte pravým tlačítkem na složku řadiče a proveďte nové MoviesController.</span><span class="sxs-lookup"><span data-stu-id="6be9b-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="6be9b-111">[![Přidání Kontroleru](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6be9b-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="6be9b-112">Tím se vytvoří nový soubor "MoviesController.cs" pod naše \Controllers složku v rámci naší projektu.</span><span class="sxs-lookup"><span data-stu-id="6be9b-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="6be9b-113">Umožňuje aktualizovat MovieController načíst seznam filmy z našich nově vyplněná databáze.</span><span class="sxs-lookup"><span data-stu-id="6be9b-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="6be9b-114">Dotaz LINQ jsme se provádí tak, aby načteme filmy vydanou po letní 1984.</span><span class="sxs-lookup"><span data-stu-id="6be9b-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="6be9b-115">Budeme potřebovat šablonu zobrazení k vykreslení tento seznam filmy zpět, takže klikněte pravým tlačítkem myši v metodě a vyberte možnost Přidat zobrazení k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="6be9b-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="6be9b-116">V dialogovém okně Přidat zobrazení jsme budete znamenat předali seznam&lt;Movies.Models.Movie&gt; do našich zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="6be9b-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="6be9b-117">Na rozdíl od předchozích dobu jsme používá dialogové okno Přidat zobrazení a rozhodli se vytvořit šablonu "Prázdný" tentokrát jsme budete označuje, že chceme sady Visual Studio automaticky "vygenerovat" Zobrazit šablonu pro nám s některé výchozí obsah.</span><span class="sxs-lookup"><span data-stu-id="6be9b-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="6be9b-118">Provedeme to tak, že vyberete "Seznam" položky v rámci "obsahu rozevírací nabídce zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6be9b-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="6be9b-119">Mějte na paměti, když jste vytvořili novou třídu budete potřebovat kompilace aplikace se zobrazí v dialogovém okně Přidat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6be9b-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Přidání zobrazení](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="6be9b-121">Klikněte na tlačítko Přidat a systém automaticky vygeneruje kód pro zobrazení pro nás, které zobrazuje seznam filmy.</span><span class="sxs-lookup"><span data-stu-id="6be9b-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="6be9b-122">To je dobrý čas na přechod &lt;h2&gt; záhlaví něco jako "Moje film seznam", jako jsme provedli dříve s zobrazení Hello World.</span><span class="sxs-lookup"><span data-stu-id="6be9b-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="6be9b-123">[![Filmy - sadu Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="6be9b-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="6be9b-124">Spusťte aplikaci a navštivte /Movies na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="6be9b-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="6be9b-125">Jsme teď načíst data z databáze pomocí základního dotazu v Kontroleru a vrátí data zobrazení, které zná filmy.</span><span class="sxs-lookup"><span data-stu-id="6be9b-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="6be9b-126">Toto zobrazení potom otáčí prostřednictvím seznamu filmy a vytvoří tabulku dat pro nás.</span><span class="sxs-lookup"><span data-stu-id="6be9b-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="6be9b-127">[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="6be9b-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="6be9b-128">Jsme nebude být implementace úpravy, podrobnosti a odstranění funkce pomocí této aplikace - tak jsme nepotřebujete výchozí odkazy, které vytvořit šablonu vygenerované uživatelské rozhraní pro nás.</span><span class="sxs-lookup"><span data-stu-id="6be9b-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="6be9b-129">Otevřete soubor /Movies/Index.aspx a je odebrat.</span><span class="sxs-lookup"><span data-stu-id="6be9b-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="6be9b-130">Tady je zdrojový kód by měl vypadat naše aktualizovanou šablonu zobrazení po jsme proveďte tyto změny:</span><span class="sxs-lookup"><span data-stu-id="6be9b-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="6be9b-131">Vytváří odkazy, které nebudou potřebujeme, takže jsme budete odstranit v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="6be9b-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="6be9b-132">Ponecháme naše vytvořit nové propojení ale, jak se další!</span><span class="sxs-lookup"><span data-stu-id="6be9b-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="6be9b-133">Zde je, jak naše aplikace vypadá se tento sloupec odebrat.</span><span class="sxs-lookup"><span data-stu-id="6be9b-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="6be9b-134">[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="6be9b-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="6be9b-135">Nyní je k dispozici jednoduchý seznam naše film data.</span><span class="sxs-lookup"><span data-stu-id="6be9b-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="6be9b-136">Ale když kliknete na odkaz "Vytvořit nové", jsme budete dojde k chybě jako ji není připojili!</span><span class="sxs-lookup"><span data-stu-id="6be9b-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="6be9b-137">Umožňuje implementovat metodu vytvořit akce a umožní uživateli zadat nové filmy v naší databázi.</span><span class="sxs-lookup"><span data-stu-id="6be9b-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6be9b-138">[Předchozí](getting-started-with-mvc-part4.md)
> [další](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="6be9b-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
