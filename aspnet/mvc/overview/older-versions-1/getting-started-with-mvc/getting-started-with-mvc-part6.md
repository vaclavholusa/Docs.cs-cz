---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: "Přidávání vytváření metoda a vytvořit zobrazení | Microsoft Docs"
author: shanselman
description: "Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 4792689087ab85be25fe186b2ec97915af448ef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-create-method-and-create-view"></a><span data-ttu-id="8da8c-104">Přidávání vytváření metoda a vytvořit zobrazení</span><span class="sxs-lookup"><span data-stu-id="8da8c-104">Adding a Create Method and Create View</span></span>
====================
<span data-ttu-id="8da8c-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="8da8c-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="8da8c-106">Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8da8c-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="8da8c-107">Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.</span><span class="sxs-lookup"><span data-stu-id="8da8c-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="8da8c-108">Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8da8c-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="8da8c-109">V této části přidáme implementovat podporu nutná pro povolení uživatelům vytvářet nové filmy v naší databázi.</span><span class="sxs-lookup"><span data-stu-id="8da8c-109">In this section we are going to implement the support necessary to enable users to create new movies in our database.</span></span> <span data-ttu-id="8da8c-110">Provedeme to implementací filmy nebo vytvořit adresu URL akce.</span><span class="sxs-lookup"><span data-stu-id="8da8c-110">We'll do this by implementing the /Movies/Create URL action.</span></span>

<span data-ttu-id="8da8c-111">Implementace filmy nebo vytvoření adresy URL je dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="8da8c-111">Implementing the /Movies/Create URL is a two step process.</span></span> <span data-ttu-id="8da8c-112">Pokud uživatel navštíví první adresa URL filmy nebo vytvořit chceme je zobrazovat formuláře HTML, který může vyplnit k zadání nového film.</span><span class="sxs-lookup"><span data-stu-id="8da8c-112">When a user first visits the /Movies/Create URL we want to show them an HTML form that they can fill out to enter a new movie.</span></span> <span data-ttu-id="8da8c-113">Pak když uživatel odešle formulář a příspěvcích zpět na server, chceme načtení odeslaných obsah a jeho uložení do databáze.</span><span class="sxs-lookup"><span data-stu-id="8da8c-113">Then, when the user submits the form and posts the data back to the server, we want to retrieve the posted contents and save it into our database.</span></span>

<span data-ttu-id="8da8c-114">Tyto dva kroky v rámci dvou metod Create() jsme budete implementovat v rámci naší MoviesController třídy.</span><span class="sxs-lookup"><span data-stu-id="8da8c-114">We'll implement these two steps within two Create() methods within our MoviesController class.</span></span> <span data-ttu-id="8da8c-115">Zobrazí jednu metodu &lt;formuláře&gt; , uživatel by měl vyplnit k vytvoření nové film.</span><span class="sxs-lookup"><span data-stu-id="8da8c-115">One method will show the &lt;form&gt; that the user should fill out to create a new movie.</span></span> <span data-ttu-id="8da8c-116">Druhá metoda zpracovává zpracování odeslaných dat, když uživatel odešle &lt;formuláře&gt; zpět na server a uložte nový film v rámci naší databáze.</span><span class="sxs-lookup"><span data-stu-id="8da8c-116">The second method will handle processing the posted data when the user submits the &lt;form&gt; back to the server, and save a new Movie within our database.</span></span>

<span data-ttu-id="8da8c-117">Níže je kód přidáme do našich MoviesController třídu pro implementaci toto:</span><span class="sxs-lookup"><span data-stu-id="8da8c-117">Below is the code we'll add to our MoviesController class to implement this:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

<span data-ttu-id="8da8c-118">Výše uvedený kód obsahuje všechny kód, který budeme potřebovat v rámci Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="8da8c-118">The above code contains all of the code that we'll need within our Controller.</span></span>

<span data-ttu-id="8da8c-119">Teď umožňuje implementovat šablony vytvořit zobrazení, která použijeme k zobrazení formuláře pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="8da8c-119">Let's now implement the Create View template that we'll use to display a form to the user.</span></span> <span data-ttu-id="8da8c-120">Jsme budete klikněte pravým tlačítkem v první metodu Create a vyberte možnost "Přidat zobrazení" pro vytvoření šablony zobrazení pro naše film formulář.</span><span class="sxs-lookup"><span data-stu-id="8da8c-120">We'll right click in the first Create method and select "Add View" to create the view template for our Movie form.</span></span>

<span data-ttu-id="8da8c-121">Jsme vyberete, které jsme se chystáte předat zobrazit šablonu "Film" jako třída jeho zobrazení dat a znamenat, že má "vygenerovat" šablonu "Vytvořit".</span><span class="sxs-lookup"><span data-stu-id="8da8c-121">We'll select that we are going to pass the view template a "Movie" as its view data class, and indicate that we want to "scaffold" a "Create" template.</span></span>

<span data-ttu-id="8da8c-122">[![Přidání zobrazení](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8da8c-122">[![Add View](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span></span>

<span data-ttu-id="8da8c-123">Po kliknutí na tlačítko Přidat, vytvoří se pro vás \Movies\Create.aspx zobrazit šablonu.</span><span class="sxs-lookup"><span data-stu-id="8da8c-123">After you click the Add button, \Movies\Create.aspx View template will be created for you.</span></span> <span data-ttu-id="8da8c-124">Vzhledem k tomu, že jsme vybrali "Vytvořit" z rozevíracího seznamu "zobrazení obsahu", dialogové okno Přidat zobrazení automaticky "vygeneroval" některé výchozí obsah pro nás.</span><span class="sxs-lookup"><span data-stu-id="8da8c-124">Because we selected "Create" from the "view content" dropdown, the Add View dialog automatically "scaffolded" some default content for us.</span></span> <span data-ttu-id="8da8c-125">Generování uživatelského rozhraní vytvořit HTML &lt;formuláře&gt;, místo pro chybu ověřování zpráv přejdete a vzhledem k tomu, že generování uživatelského rozhraní ví o filmy, vytvoření popisku a pole pro každou vlastnost naše třídy.</span><span class="sxs-lookup"><span data-stu-id="8da8c-125">The scaffolding created an HTML &lt;form&gt;, a place for validation error messages to go, and since scaffolding knows about Movies, it created Label and Fields for each property of our class.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

<span data-ttu-id="8da8c-126">Vzhledem k tomu, že naše databáze automaticky poskytuje film ID, odeberte umožňuje těchto polí tohoto referenčního modelu. ID z našich zobrazení pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="8da8c-126">Since our database automatically gives a Movie an ID, let's remove those fields that reference model.Id from our Create View.</span></span> <span data-ttu-id="8da8c-127">Odeberte 7 řádky po &lt;legendy&gt;pole&lt;/legend&gt; jak ukazují pole ID jsme nechcete.</span><span class="sxs-lookup"><span data-stu-id="8da8c-127">Remove the 7 lines after &lt;legend&gt;Fields&lt;/legend&gt; as they show the ID field that we don't want.</span></span>

<span data-ttu-id="8da8c-128">Umožňuje nyní vytvořte nové film a přidejte ho do databáze.</span><span class="sxs-lookup"><span data-stu-id="8da8c-128">Let's now create a new movie and add it to the database.</span></span> <span data-ttu-id="8da8c-129">Jsme budete Uděláte to tak, že spustíte aplikaci znovu a přejděte "nebo filmy" adresa URL a klikněte na tlačítko "Vytvořit" propojit přidání nové videa.</span><span class="sxs-lookup"><span data-stu-id="8da8c-129">We'll do this by running the application again and visit the "/Movies" URL and click the "Create" link to add a new Movie.</span></span>

<span data-ttu-id="8da8c-130">[![Vytvoření – Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="8da8c-130">[![Create - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span></span>

<span data-ttu-id="8da8c-131">Když kliknete na tlačítko pro vytvoření, jsme budete mít publikování zpět (přes HTTP POST) data na tomto formuláři metodu /Movies/Create, který jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="8da8c-131">When we click the Create button, we'll be posting back (via HTTP POST) the data on this form to the /Movies/Create method that we just created.</span></span> <span data-ttu-id="8da8c-132">Stejně jako při systém automaticky trvalo parametr "numTimes" a "název" mimo adresu URL a mapovat je na parametry pro metodu dříve bude systém automaticky trvat pole formuláře POST a jejich namapování na objekt.</span><span class="sxs-lookup"><span data-stu-id="8da8c-132">Just like when the system automatically took the "numTimes" and "name " parameter out of the URL and mapped them to parameters on a method earlier, the system will automatically take the Form Fields from a POST and map them to an object.</span></span> <span data-ttu-id="8da8c-133">V takovém případě hodnot polí ve formátu HTML jako "ReleaseDate" a "Title" bude automaticky přepnut do správné vlastnosti novou instanci třídy film.</span><span class="sxs-lookup"><span data-stu-id="8da8c-133">In this case, values from fields in HTML like "ReleaseDate" and "Title" will automatically be put into the correct properties of a new instance of a Movie.</span></span>

<span data-ttu-id="8da8c-134">Podívejme se na druhé metody vytvořit z našich MoviesController znovu.</span><span class="sxs-lookup"><span data-stu-id="8da8c-134">Let's look at the second Create method from our MoviesController again.</span></span> <span data-ttu-id="8da8c-135">Všimněte si, jak dlouho trvá objekt "Film" jako argument:</span><span class="sxs-lookup"><span data-stu-id="8da8c-135">Notice how it takes a "Movie" object as an argument:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

<span data-ttu-id="8da8c-136">Tento objekt film byla předána do verze [HttpPost] naše vytvořit metody akce a jsme uloženy v databázi a potom přesměruje uživatele zpět na Index() metody akce, které se zobrazí uložený výsledek v seznamu video:</span><span class="sxs-lookup"><span data-stu-id="8da8c-136">This Movie object was then passed to the [HttpPost] version of our Create action method, and we saved it in the database and then redirected the user back to the Index() action method which will show the saved result in the movie list:</span></span>

<span data-ttu-id="8da8c-137">[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="8da8c-137">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span></span>

<span data-ttu-id="8da8c-138">Jsme nejsou kontrolu, pokud naše filmy, když jsou správné, a databáze nebude povolovat nám uložit film s bez názvu.</span><span class="sxs-lookup"><span data-stu-id="8da8c-138">We aren't checking if our movies are correct, though, and the database won't allow us to save a movie with no Title.</span></span> <span data-ttu-id="8da8c-139">Je dobrý Pokud jsme může informace pro uživatele, který před databáze došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="8da8c-139">It'd be nice if we could tell the user that before the database threw an error.</span></span> <span data-ttu-id="8da8c-140">Tato další jsme to udělat tak, že přidáte podporu ověřování do aplikace.</span><span class="sxs-lookup"><span data-stu-id="8da8c-140">We'll do this next by adding validation support to our application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8da8c-141">[Předchozí](getting-started-with-mvc-part5.md)
[další](getting-started-with-mvc-part7.md)</span><span class="sxs-lookup"><span data-stu-id="8da8c-141">[Previous](getting-started-with-mvc-part5.md)
[Next](getting-started-with-mvc-part7.md)</span></span>
