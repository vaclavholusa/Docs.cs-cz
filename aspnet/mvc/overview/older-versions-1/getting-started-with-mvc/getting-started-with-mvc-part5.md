---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Přístup k datům modelu z Kontroleru | Dokumentace Microsoftu
author: shanselman
description: Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvořte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: fe5111e2f103e9b20f27385421a1985062acaac1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371610"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="4cb8a-104">Přístup k datům modelu z Kontroleru</span><span class="sxs-lookup"><span data-stu-id="4cb8a-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="4cb8a-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="4cb8a-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="4cb8a-106">Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="4cb8a-107">Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="4cb8a-108">Přejděte [výukové centrum pro ASP.NET MVC](../../../index.md) najít další technologie ASP.NET MVC, kurzů a ukázek.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="4cb8a-109">V této části jsme se chystáte vytvořit novou třídu MoviesController a napsat kód, který načte naše data o filmech a zobrazí ji zpět do prohlížeče pomocí zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="4cb8a-110">Klikněte pravým tlačítkem na složku řadiče a proveďte nové MoviesController.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="4cb8a-111">[![Přidání Kontroleru](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4cb8a-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="4cb8a-112">Tím se vytvoří nový soubor "MoviesController.cs" pod naše \Controllers složky v rámci naší projektu.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="4cb8a-113">Umožňuje aktualizovat MovieController načíst seznam video z naší nově mají údaj vyplněný databáze.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="4cb8a-114">Tak, aby načteme filmy vydanou po léta 1984 jsme provádění dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="4cb8a-115">Budeme potřebovat zobrazit šablonu pro vykreslení tohoto seznamu filmy zpět, takže klikněte pravým tlačítkem na metodu a vyberte Přidat zobrazení k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="4cb8a-116">V dialogovém okně Přidat zobrazení budete Udáváme, že jsme prochází seznam&lt;Movies.Models.Movie&gt; do našich zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="4cb8a-117">Na rozdíl od předchozích časy použít dialogové okno Přidat zobrazení jsme se rozhodli vytvořit šablonu služby "Prázdný" tentokrát jsme budete označuje, že chceme, aby Visual Studio automaticky "scaffold" Zobrazit šablonu pro nás s nějakou výchozí obsah.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="4cb8a-118">Provedeme to tak, že vyberete "Seznam" položky v rámci "zobrazení obsahu rozevírací nabídky.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="4cb8a-119">Mějte na paměti, když jste vytvořili novou třídu, budete muset kompilovat aplikaci, aby se zobrazí v dialogovém okně Přidat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Přidání zobrazení](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="4cb8a-121">Klikněte na tlačítko Přidat a systém automaticky vygeneruje kód pro zobrazení pro USA, který zobrazuje náš seznam videa.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="4cb8a-122">Toto je změna vhodná doba &lt;h2&gt; přepravuje do něco jako "Moje film seznam", jako jsme to udělali dříve se zobrazením Hello World.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="4cb8a-123">[![Videa – Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4cb8a-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="4cb8a-124">Spusťte aplikaci a přejděte do adresního řádku /Movies.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="4cb8a-125">Jsme teď načíst data z databáze pomocí základní dotaz do třídy Kontroleru a vrátí data zobrazení, které ví o filmech.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="4cb8a-126">Toto zobrazení pak přede prostřednictvím seznamu filmy a vytvoří tabulky dat pro nás.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="4cb8a-127">[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="4cb8a-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="4cb8a-128">Jsme nesmí být implementace úpravy, podrobností a odstraněných funkcí s touto aplikací – proto nepotřebujeme výchozí odkazy, které šablona scaffold vytváří.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="4cb8a-129">Otevřete soubor /Movies/Index.aspx a neodeberete.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="4cb8a-130">Zde je zdrojový kód by měl vypadat naše aktualizovanou šablonu zobrazení po jsme tyto změny:</span><span class="sxs-lookup"><span data-stu-id="4cb8a-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="4cb8a-131">Odkazy, které jsme nebudete potřebovat, se vytváří, je v tomto příkladu odstraníme.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="4cb8a-132">Budeme udržovat naše vytvořit nové propojení, jako to je další!</span><span class="sxs-lookup"><span data-stu-id="4cb8a-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="4cb8a-133">Tady je vypadá naši aplikaci pomocí tohoto sloupce odebrány.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="4cb8a-134">[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="4cb8a-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="4cb8a-135">Nyní je k dispozici jednoduchý seznam naše data o filmech.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="4cb8a-136">Ale když kliknete na odkaz "Vytvořit nový", jsme chybu, protože získáte ji není připojili!</span><span class="sxs-lookup"><span data-stu-id="4cb8a-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="4cb8a-137">Můžeme implementovat metodu akce vytvoření a povolit tak uživateli zadat nové filmy v databázi.</span><span class="sxs-lookup"><span data-stu-id="4cb8a-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4cb8a-138">[Předchozí](getting-started-with-mvc-part4.md)
> [další](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="4cb8a-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
