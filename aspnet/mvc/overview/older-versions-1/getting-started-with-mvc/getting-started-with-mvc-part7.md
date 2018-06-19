---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Přidání ověřování do modelu | Microsoft Docs
author: shanselman
description: Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoření jednoduché webové aplikace, která čte a zapisuje z databáze.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 78dd6bdd81fcb51a3a21a8f1ee12b4b2bfc37db5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871945"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="5195b-104">Přidání ověřování do modelu</span><span class="sxs-lookup"><span data-stu-id="5195b-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="5195b-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="5195b-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="5195b-106">Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5195b-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="5195b-107">Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.</span><span class="sxs-lookup"><span data-stu-id="5195b-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="5195b-108">Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5195b-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="5195b-109">V této části přidáme implementovat podporu nutná pro povolení ověření vstupu v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5195b-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="5195b-110">Jsme budete zajistěte, aby náš obsah databáze je vždy správný a koncovým uživatelům poskytovat užitečné chybové zprávy, když se nepovede, zadejte film data, která není platná.</span><span class="sxs-lookup"><span data-stu-id="5195b-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="5195b-111">Začneme budete přidáním trochu logiku ověření pro třídu film.</span><span class="sxs-lookup"><span data-stu-id="5195b-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="5195b-112">Klikněte pravým tlačítkem na složku, Model a vyberte Přidat třídu.</span><span class="sxs-lookup"><span data-stu-id="5195b-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="5195b-113">Název vaší třídy film.</span><span class="sxs-lookup"><span data-stu-id="5195b-113">Name your class Movie.</span></span>

<span data-ttu-id="5195b-114">Pokud jsme vytvořili Model Entity film předtím rozhraní IDE vytvořit třídu film.</span><span class="sxs-lookup"><span data-stu-id="5195b-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="5195b-115">Ve skutečnosti součástí třídy film může být v jednom souboru a část v jiném.</span><span class="sxs-lookup"><span data-stu-id="5195b-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="5195b-116">Tomu se říká konkrétní třídu.</span><span class="sxs-lookup"><span data-stu-id="5195b-116">This is called a Partial Class.</span></span> <span data-ttu-id="5195b-117">Vytvoříme rozšíření třídy film z jiného souboru.</span><span class="sxs-lookup"><span data-stu-id="5195b-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="5195b-118">Vytvoříme třídu částečné film který odkazuje na třídu"kamarád" s některé atributy, které se mu dávat pokyny ověření do systému.</span><span class="sxs-lookup"><span data-stu-id="5195b-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="5195b-119">Jsme budete označit nadpis a cenu podle požadavku a taky trvat, být do určitého rozsahu.</span><span class="sxs-lookup"><span data-stu-id="5195b-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="5195b-120">Klikněte pravým tlačítkem na složku modely a vyberte Přidat třídu.</span><span class="sxs-lookup"><span data-stu-id="5195b-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="5195b-121">Název vaší třídy film a klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="5195b-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="5195b-122">Zde je co naše částečné třídy vypadá film jako.</span><span class="sxs-lookup"><span data-stu-id="5195b-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="5195b-123">Znovu spusťte aplikaci a zkuste zadejte film s cenou více než 100.</span><span class="sxs-lookup"><span data-stu-id="5195b-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="5195b-124">Poté, co jste odeslání formuláře budete dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="5195b-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="5195b-125">Chyba je zachycena na straně serveru a vyskytne se po odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="5195b-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="5195b-126">Všimněte si, jak byly ASP.NET MVC předdefinované pomocné rutiny HTML dostatečně inteligentní, zobrazí se chybová zpráva a Udržovat hodnoty pro nám v rámci prvků textového pole:</span><span class="sxs-lookup"><span data-stu-id="5195b-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="5195b-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5195b-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="5195b-128">To vyhovující postup, ale bude dobrý, jsme může informace pro uživatele na straně klienta, hned, než získá podílejí na server.</span><span class="sxs-lookup"><span data-stu-id="5195b-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="5195b-129">Umožňuje povolit některé ověřování na straně klienta v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5195b-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="5195b-130">Přidání ověřování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="5195b-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="5195b-131">Vzhledem k tomu, že naše třída film již některých atributů ověření, budete potřebujeme přidejte několik souborů JavaScript pro naše Create.aspx zobrazit šablonu a přidejte řádek kódu povolit ověřování na straně klienta proběhla.</span><span class="sxs-lookup"><span data-stu-id="5195b-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="5195b-132">Z přejděte v rámci VWD naše složky zobrazení/film a otevře Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="5195b-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="5195b-133">Otevře složky skriptů v Průzkumníku řešení a přetáhněte ji následující tři skriptů v rámci &lt;head&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="5195b-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="5195b-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="5195b-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="5195b-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="5195b-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="5195b-136">Chcete tyto soubory skriptu, než se objeví v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="5195b-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="5195b-137">Navíc přidejte tento jeden řádek výše Html.BeginForm:</span><span class="sxs-lookup"><span data-stu-id="5195b-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="5195b-138">Tady je kód uvedené v rámci rozhraní IDE.</span><span class="sxs-lookup"><span data-stu-id="5195b-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="5195b-139">[![Filmy - sadu Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="5195b-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="5195b-140">Spusťte aplikaci a znovu přejděte /Movies/Create a klikněte na tlačítko vytvořit bez nutnosti zadávat žádná data.</span><span class="sxs-lookup"><span data-stu-id="5195b-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="5195b-141">Chybové zprávy, které se zobrazí okamžitě bez stránce flash, že jsme přidružit k odesílání dat všechny způsob zpět na server.</span><span class="sxs-lookup"><span data-stu-id="5195b-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="5195b-142">Toto je, protože rozhraní ASP.NET MVC je nyní ověření vstupu na obou klienta (pomocí jazyka JavaScript) a na serveru.</span><span class="sxs-lookup"><span data-stu-id="5195b-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="5195b-143">[![Vytvoření – Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="5195b-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="5195b-144">To je vyhledávání dobrý!</span><span class="sxs-lookup"><span data-stu-id="5195b-144">This is looking good!</span></span> <span data-ttu-id="5195b-145">Přidejme jeden další sloupec teď do databáze.</span><span class="sxs-lookup"><span data-stu-id="5195b-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5195b-146">[Předchozí](getting-started-with-mvc-part6.md)
> [další](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="5195b-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
