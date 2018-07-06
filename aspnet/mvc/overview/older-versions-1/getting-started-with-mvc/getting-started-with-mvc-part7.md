---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Přidání ověření do modelu | Dokumentace Microsoftu
author: shanselman
description: Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvořte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: f8172b63fcaa7599f5dd733271d9ea7570e3bcb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802783"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="21221-104">Přidání ověření do modelu</span><span class="sxs-lookup"><span data-stu-id="21221-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="21221-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="21221-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="21221-106">Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="21221-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="21221-107">Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.</span><span class="sxs-lookup"><span data-stu-id="21221-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="21221-108">Přejděte [výukové centrum pro ASP.NET MVC](../../../index.md) najít další technologie ASP.NET MVC, kurzů a ukázek.</span><span class="sxs-lookup"><span data-stu-id="21221-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="21221-109">V této části budeme implementovat podporu nutná pro povolení ověření vstupu v rámci naší aplikace.</span><span class="sxs-lookup"><span data-stu-id="21221-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="21221-110">Budete zajišťujeme, že náš obsah databáze je vždy správná a koncovým uživatelům poskytovat užitečné chybové zprávy, když se pokusí a zadejte data o filmech, která není platná.</span><span class="sxs-lookup"><span data-stu-id="21221-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="21221-111">Začneme budete přidáním trochu ověřovací logiku do třídy Video.</span><span class="sxs-lookup"><span data-stu-id="21221-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="21221-112">Klikněte pravým tlačítkem myši klikněte na složku, Model a vyberte Přidat třídu.</span><span class="sxs-lookup"><span data-stu-id="21221-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="21221-113">Zadejte název vaší třídy Video.</span><span class="sxs-lookup"><span data-stu-id="21221-113">Name your class Movie.</span></span>

<span data-ttu-id="21221-114">Jsme dříve při vytváření modelu Entity filmů, integrovaného vývojového prostředí vytvořit třídu video.</span><span class="sxs-lookup"><span data-stu-id="21221-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="21221-115">Ve skutečnosti součástí film třídy může být v jednom souboru a v další části.</span><span class="sxs-lookup"><span data-stu-id="21221-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="21221-116">Tomu se říká částečnou třídu.</span><span class="sxs-lookup"><span data-stu-id="21221-116">This is called a Partial Class.</span></span> <span data-ttu-id="21221-117">Teď ještě chvíli Zůstaneme rozšíření třídy Video z jiného souboru.</span><span class="sxs-lookup"><span data-stu-id="21221-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="21221-118">Vytvoříme film částečné třídy, na kterou odkazuje na třídu"kamarádské" některé atributy, které se mu ověření systému.</span><span class="sxs-lookup"><span data-stu-id="21221-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="21221-119">Vytvoříme označení názvu a cena podle potřeby a taky trvat, se cena v určitém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="21221-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="21221-120">Klikněte pravým tlačítkem na složku modely a vyberte Přidat třídu.</span><span class="sxs-lookup"><span data-stu-id="21221-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="21221-121">Název vaší třídy filmů a klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="21221-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="21221-122">Zde je, co naše částečné film třídy ve tvaru.</span><span class="sxs-lookup"><span data-stu-id="21221-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="21221-123">Znovu spusťte aplikaci a zkuste zadat videa s cenou více než 100.</span><span class="sxs-lookup"><span data-stu-id="21221-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="21221-124">Poté, co jste odeslání formuláře budete dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="21221-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="21221-125">Tato chyba je zachycena na straně serveru a vyvolá se po odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="21221-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="21221-126">Všimněte si, jak byly dostatečně inteligentní, aby zobrazení chybové zprávy a Udržovat hodnoty pro nás v rámci textového pole prvků technologie ASP.NET MVC integrovaných pomocných rutin HTML:</span><span class="sxs-lookup"><span data-stu-id="21221-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="21221-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="21221-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="21221-128">To spolupráce, ale bylo by dobré Pokud jsme mohli říct uživatelům, na straně klienta, hned, předtím, než získá podílejí na serveru.</span><span class="sxs-lookup"><span data-stu-id="21221-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="21221-129">Umožňuje povolit některé ověřování na straně klienta s použitím jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="21221-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="21221-130">Přidání ověřování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="21221-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="21221-131">Vzhledem k tomu, že naše třída Video už má několik atributů ověření, budete potřebujeme přidat několik souborů JavaScriptu do našich Create.aspx zobrazit šablonu a přidejte řádek kódu, které umožňují ověřování na straně klienta, aby proběhla.</span><span class="sxs-lookup"><span data-stu-id="21221-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="21221-132">V rámci VWD naše zobrazení/video složka go a otevřete Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="21221-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="21221-133">Otevření složky skriptů v Průzkumníku řešení a přetáhněte následující tři skripty pro v rámci &lt;head&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="21221-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="21221-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="21221-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="21221-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="21221-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="21221-136">Chcete tyto soubory skriptu se zobrazí v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="21221-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="21221-137">Přidejte také tento jeden řádek výše Html.BeginForm:</span><span class="sxs-lookup"><span data-stu-id="21221-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="21221-138">Tady je kód zobrazený v rámci rozhraní IDE.</span><span class="sxs-lookup"><span data-stu-id="21221-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="21221-139">[![Videa – Microsoft Visual Web Developer Express 2010 (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="21221-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="21221-140">Spusťte aplikaci a znovu navštívit /Movies/Create a klikněte na tlačítko vytvořit bez nutnosti zadávat žádná data.</span><span class="sxs-lookup"><span data-stu-id="21221-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="21221-141">Chybové zprávy se zobrazí okamžitě bez flash, přidružené k odesílání dat na stránce všechny způsob, jakým zpět na server.</span><span class="sxs-lookup"><span data-stu-id="21221-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="21221-142">Toto je vzhledem k tomu, že technologie ASP.NET MVC je nyní ověření vstupu u obou klienta (pomocí JavaScriptu) a na serveru.</span><span class="sxs-lookup"><span data-stu-id="21221-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="21221-143">[![Vytvoření – Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="21221-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="21221-144">To je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="21221-144">This is looking good!</span></span> <span data-ttu-id="21221-145">Pojďme nyní přidat jeden další sloupec databáze.</span><span class="sxs-lookup"><span data-stu-id="21221-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="21221-146">[Předchozí](getting-started-with-mvc-part6.md)
> [další](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="21221-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
