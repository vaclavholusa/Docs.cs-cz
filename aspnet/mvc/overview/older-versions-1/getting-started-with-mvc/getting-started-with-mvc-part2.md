---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Přidávání řadiče | Microsoft Docs
author: shanselman
description: Aktualizovaná verze, pokud v tomto kurzu je k dispozici zde pomocí sady Visual Studio 2013. Nový kurz používá ASP.NET MVC 5, který nabízí mnoho vylepšení v porovnání s t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: c6ecd1ffdd53a629d0079d57b85c7f6db2f316ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869020"
---
<a name="adding-a-controller"></a><span data-ttu-id="cd7c0-104">Přidání Kontroleru</span><span class="sxs-lookup"><span data-stu-id="cd7c0-104">Adding a Controller</span></span>
====================
<span data-ttu-id="cd7c0-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="cd7c0-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="cd7c0-106">Pokud v tomto kurzu je k dispozici aktualizovaná verze [sem](../../getting-started/introduction/getting-started.md) pomocí [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span><span class="sxs-lookup"><span data-stu-id="cd7c0-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="cd7c0-107">Nový kurz používá ASP.NET MVC 5, který obsahuje mnoho vylepšení v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="cd7c0-108">Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="cd7c0-109">Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="cd7c0-110">Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="cd7c0-111">MVC je zkratka pro Model, zobrazení, řadiče.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-111">MVC stands for Model, View, Controller.</span></span> <span data-ttu-id="cd7c0-112">MVC je vzor pro vývoj aplikací tak, aby měl každý část odpovědnost, které se liší od jiného.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-112">MVC is a pattern for developing applications such that each part has a responsibility that is different from another.</span></span>

- <span data-ttu-id="cd7c0-113">Model: Data aplikace</span><span class="sxs-lookup"><span data-stu-id="cd7c0-113">Model: The data of your application</span></span>
- <span data-ttu-id="cd7c0-114">Zobrazení: Soubory šablon vaše aplikace bude používat k dynamickému generování odpovědi HTML.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-114">Views: The template files your application will use to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="cd7c0-115">Řadiče: Třídy, které zpracovávat příchozí požadavky na adresu URL pro aplikaci, načíst data modelu a potom zadejte zobrazit šablony, které vykreslení odpověď zpět do klienta</span><span class="sxs-lookup"><span data-stu-id="cd7c0-115">Controllers: Classes that handle incoming URL requests to the application, retrieve model data, and then specify view templates that render a response back to the client</span></span>

<span data-ttu-id="cd7c0-116">Budete se o tyto koncepty v tomto kurzu jsme ukazují, jak je používat k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-116">We'll be covering all these concepts in this tutorial and show you how to use them to build an application.</span></span>

<span data-ttu-id="cd7c0-117">Umožňuje vytvořit nový řadič kliknutím pravým tlačítkem na složku řadiče v Průzkumníku řešení a výběrem možnosti Přidat kontroler.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-117">Let's create a new controller by right-clicking the controllers folder in the solution Explorer and selecting Add Controller.</span></span>

<span data-ttu-id="cd7c0-118">[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cd7c0-118">[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)</span></span>

<span data-ttu-id="cd7c0-119">Pojmenujte nový kontroler "HelloWorldController" a klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-119">Name your new controller "HelloWorldController" and click Add.</span></span>

<span data-ttu-id="cd7c0-120">[![Dialogové okno řadiče přidání](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="cd7c0-120">[![Add Controller Dialog](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)</span></span>

<span data-ttu-id="cd7c0-121">Všimněte si, v Průzkumníku řešení na pravé straně, který byl vytvořen nový soubor můžete volat HelloWorldController.cs a tento soubor je nyní otevřít v **IDE**.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-121">Notice in the Solution Explorer on the right that a new file has been created for you called HelloWorldController.cs and that file is now opened in the **IDE**.</span></span>

<span data-ttu-id="cd7c0-122">[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="cd7c0-122">[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)</span></span>

<span data-ttu-id="cd7c0-123">Vytvořte dvě nové metody, které vypadají takto uvnitř novou veřejnou třídu HelloWorldController.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-123">Create two new methods that look like this inside of your new public class HelloWorldController.</span></span> <span data-ttu-id="cd7c0-124">Vrátí řetězec HTML přímo z našich řadiče jako příklad.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-124">We'll return a string of HTML directly from our controller as an example.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

<span data-ttu-id="cd7c0-125">Řadiči jmenuje HelloWorldController a nová metoda je volána Index.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-125">Your Controller is named HelloWorldController and your new Method is called Index.</span></span> <span data-ttu-id="cd7c0-126">Spusťte aplikaci znovu, stejně jako předtím (klikněte na tlačítko Přehrát akci nebo stiskněte klávesu F5 k tomu).</span><span class="sxs-lookup"><span data-stu-id="cd7c0-126">Run your application again, just as before (click the play button or press F5 to do this).</span></span> <span data-ttu-id="cd7c0-127">Jakmile je zahájen prohlížeč, změnit cestu v panelu Adresa `http://localhost:xx/HelloWorld` zvolil kde xx je ať číslo počítače.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-127">Once your browser has started up, change the path in the address bar to `http://localhost:xx/HelloWorld` where xx is whatever number your computer has chosen.</span></span> <span data-ttu-id="cd7c0-128">Váš prohlížeč by měl nyní vypadat jako na následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-128">Now your browser should look like the screenshot below.</span></span> <span data-ttu-id="cd7c0-129">V našem metody popsané výše jsme vrátí řetězec předaný do metodu s názvem "Obsahu."</span><span class="sxs-lookup"><span data-stu-id="cd7c0-129">In our method above we returned a string passed into a method called "Content."</span></span> <span data-ttu-id="cd7c0-130">Můžeme vás vyzval systém právě vrátí kód HTML, a to!</span><span class="sxs-lookup"><span data-stu-id="cd7c0-130">We told the system just returns some HTML, and it did!</span></span>

<span data-ttu-id="cd7c0-131">ASP.NET MVC volá různé třídy Controller (a různé metody akce v nich), v závislosti na adresy URL příchozích.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-131">ASP.NET MVC invokes different Controller classes (and different Action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="cd7c0-132">Logika výchozí mapování používá ASP.NET MVC používá k řízení, jaký kód běží tento formát:</span><span class="sxs-lookup"><span data-stu-id="cd7c0-132">The default mapping logic used by ASP.NET MVC uses a format like this to control what code is run:</span></span>

<span data-ttu-id="cd7c0-133">/ [Controller] / [název akce] nebo [parametry]</span><span class="sxs-lookup"><span data-stu-id="cd7c0-133">/[Controller]/[ActionName]/[Parameters]</span></span>

<span data-ttu-id="cd7c0-134">První část adresy URL určuje třídy Kontroleru provést.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-134">The first part of the URL determines the Controller class to execute.</span></span> <span data-ttu-id="cd7c0-135">Proto /HelloWorld mapuje k třídě HelloWorldController.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-135">So /HelloWorld maps to the HelloWorldController class.</span></span> <span data-ttu-id="cd7c0-136">Druhá část adresy URL určuje metody akce v třídě provést.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-136">The second part of the URL determines the Action method on the class to execute.</span></span> <span data-ttu-id="cd7c0-137">Proto /HelloWorld/Index by způsobilo metodu Index() třídy HelloWorldcontroller provést.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-137">So /HelloWorld/Index would cause the Index() method of the HelloWorldcontroller class to execute.</span></span> <span data-ttu-id="cd7c0-138">Všimněte si, že jsme pouze museli navštívit /HelloWorld výše a metodu, kterou byla implicitní Index.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-138">Notice that we only had to visit /HelloWorld above and the method Index was implied.</span></span> <span data-ttu-id="cd7c0-139">Je to proto, že je metoda s názvem "Index" výchozí metoda, která bude volána na řadiči, pokud není explicitně určen.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-139">This is because a method named "Index" is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="cd7c0-140">[![Toto je můj výchozí akci.](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="cd7c0-140">[![This is my default action](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)</span></span>

<span data-ttu-id="cd7c0-141">Nyní Pojďme navštivte `http://localhost:xx/HelloWorld/Welcome.` teď naše úvodní metoda má provést a vrátí jeho řetězec HTML.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-141">Now, let's visit `http://localhost:xx/HelloWorld/Welcome.` Now our Welcome Method has executed and returned its HTML string.</span></span>

<span data-ttu-id="cd7c0-142">Znovu nebo [Controller] / [název akce] nebo [parametry] tak, aby je řadič HelloWorld a Vítá v tomto případě je metoda.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-142">Again, /[Controller]/[ActionName]/[Parameters] so Controller is HelloWorld and Welcome is the Method in this case.</span></span> <span data-ttu-id="cd7c0-143">Zatím jsme dosud neučinili parametry.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-143">We haven't done Parameters yet.</span></span>

<span data-ttu-id="cd7c0-144">[![Toto je metoda úvodní akce](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="cd7c0-144">[![This is the Welcome action method](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)</span></span>

<span data-ttu-id="cd7c0-145">Pojďme upravit naše ukázka mírně tak, aby některé informace v z adresy URL můžete předat do kontroleru, například takto: / HelloWorld/Vítá? name = Scott&amp;numtimes = 4.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-145">Let's modify our sample slightly so that we can pass some information in from the URL to our controller, for example like this: /HelloWorld/Welcome?name=Scott&amp;numtimes=4.</span></span> <span data-ttu-id="cd7c0-146">Změňte metodu úvodní zahrnuje dva parametry a aktualizace je třeba níže.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-146">Change your Welcome method to include two parameters and update it like below.</span></span> <span data-ttu-id="cd7c0-147">Upozorňujeme, že jsme si volitelný parametr funkcí jazyka C# k označení, že numTimes parametr by měl výchozí na 1, pokud není předán v.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-147">Note that we've used the C# optional parameter feature to indicate that the parameter numTimes should default to 1 if it's not passed in.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

<span data-ttu-id="cd7c0-148">Spusťte aplikaci a navštivte `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` změna hodnoty názvu a numtimes, jak se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-148">Run your application and visit `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` changing the value of name and numtimes as you like.</span></span> <span data-ttu-id="cd7c0-149">Systém automaticky mapují pojmenované parametry z řetězec vašeho dotazu na panelu Adresa parametry ve své metodě.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-149">The system automatically mapped the named parameters from your query string in the address bar to parameters in your method.</span></span>

<span data-ttu-id="cd7c0-150">V obou těchto příkladech řadičem byl všechny pracuje a má byla vrácení HTML přímo.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-150">In both these examples the controller has been doing all the work, and has been returning HTML directly.</span></span> <span data-ttu-id="cd7c0-151">Normálně Neradi bychom naše řadiče vrácení HTML přímo - vzhledem k tomu, který bude mít je velmi náročná kódu.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-151">Ordinarily we don't want our Controllers returning HTML directly - since that ends up being very cumbersome to code.</span></span> <span data-ttu-id="cd7c0-152">Místo toho obvykle použijeme samostatný soubor šablony zobrazení ke generování odpovědi HTML.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-152">Instead we'll typically use a separate View template file to help generate the HTML response.</span></span> <span data-ttu-id="cd7c0-153">Podíváme, jak jsme to můžete provést.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-153">Let's look at how we can do this.</span></span> <span data-ttu-id="cd7c0-154">Zavřete prohlížeč a vraťte se k prostředí IDE.</span><span class="sxs-lookup"><span data-stu-id="cd7c0-154">Close your browser and return to the IDE.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cd7c0-155">[Předchozí](getting-started-with-mvc-part1.md)
> [další](getting-started-with-mvc-part3.md)</span><span class="sxs-lookup"><span data-stu-id="cd7c0-155">[Previous](getting-started-with-mvc-part1.md)
[Next](getting-started-with-mvc-part3.md)</span></span>
