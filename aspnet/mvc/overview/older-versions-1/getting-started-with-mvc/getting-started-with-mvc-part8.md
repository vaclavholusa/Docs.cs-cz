---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Přidání sloupce do modelu | Dokumentace Microsoftu
author: shanselman
description: Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvořte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 5dbda1280c073d5d4f2d71918ca31bc44475f64d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817198"
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="b31b2-104">Přidání sloupce do modelu</span><span class="sxs-lookup"><span data-stu-id="b31b2-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="b31b2-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="b31b2-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="b31b2-106">Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b31b2-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="b31b2-107">Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.</span><span class="sxs-lookup"><span data-stu-id="b31b2-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="b31b2-108">Přejděte [výukové centrum pro ASP.NET MVC](../../../index.md) najít další technologie ASP.NET MVC, kurzů a ukázek.</span><span class="sxs-lookup"><span data-stu-id="b31b2-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="b31b2-109">V této části jsme se chystáte projít jak můžeme provádět změny schématu databáze a zpracovat změny v rámci naší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b31b2-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="b31b2-110">Přidejme do tabulky Movie slo se "Hodnocení".</span><span class="sxs-lookup"><span data-stu-id="b31b2-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="b31b2-111">Přejděte zpět do integrovaného vývojového prostředí a kliknutím na Průzkumníka databáze.</span><span class="sxs-lookup"><span data-stu-id="b31b2-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="b31b2-112">Tabulky Movie klikněte pravým tlačítkem a vyberte Otevřít definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="b31b2-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="b31b2-113">Přidáte sloupec "Hodnocení", jak je vidět níže.</span><span class="sxs-lookup"><span data-stu-id="b31b2-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="b31b2-114">Vzhledem k tomu, že teď nemáme žádné hodnocení, sloupec můžete povolit hodnoty Null.</span><span class="sxs-lookup"><span data-stu-id="b31b2-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="b31b2-115">Klikněte na Uložit.</span><span class="sxs-lookup"><span data-stu-id="b31b2-115">Click Save.</span></span>

<span data-ttu-id="b31b2-116">[![Úpravy filmy tabulky](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b31b2-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="b31b2-117">V dalším kroku vrátit do okna Průzkumník řešení a otevře soubor Movies.edmx (která je ve složce \Models).</span><span class="sxs-lookup"><span data-stu-id="b31b2-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="b31b2-118">Klikněte pravým tlačítkem myši na návrhové ploše (bílé oblasti) a vyberte aktualizace modelů z databáze.</span><span class="sxs-lookup"><span data-stu-id="b31b2-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="b31b2-119">[![Videa – Microsoft Visual Web Developer Express 2010 (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b31b2-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="b31b2-120">Tím se spustí Průvodce aktualizací"".</span><span class="sxs-lookup"><span data-stu-id="b31b2-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="b31b2-121">Klikněte na kartu aktualizace v něm a klikněte na tlačítko Dokončit.</span><span class="sxs-lookup"><span data-stu-id="b31b2-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="b31b2-122">Naše třída modelu film pak aktualizuje s nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="b31b2-122">Our Movie model class will then be updated with the new column.</span></span>

![Průvodce aktualizací (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="b31b2-124">Po kliknutí na tlačítko Dokončit, uvidíte, že do Entity filmu v náš model se přidal nový sloupec hodnocení.</span><span class="sxs-lookup"><span data-stu-id="b31b2-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="b31b2-125">[![Video Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="b31b2-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="b31b2-126">Přidali jsme sloupec v modelu databáze, ale zobrazení nevíte o něm.</span><span class="sxs-lookup"><span data-stu-id="b31b2-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="b31b2-127">Aktualizace zobrazení s změny modelu</span><span class="sxs-lookup"><span data-stu-id="b31b2-127">Update Views with Model Changes</span></span>

<span data-ttu-id="b31b2-128">Existuje několik způsobů, jak může aktualizujeme naše šablony zobrazení tak, aby odrážely nový sloupec hodnocení.</span><span class="sxs-lookup"><span data-stu-id="b31b2-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="b31b2-129">Protože jsme vytvořili tato zobrazení vygenerováním přes dialogové okno Přidat zobrazení, jsme může je odstranit a znovu je vytvořte znovu.</span><span class="sxs-lookup"><span data-stu-id="b31b2-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="b31b2-130">Obvykle ale lidé budou už provedli změny k jejich zobrazení šablony z generování počáteční vygenerované a budete chtít přidat nebo odstranit pole ručně, stejně jako jsme to udělali s poli ID pro vytvoření.</span><span class="sxs-lookup"><span data-stu-id="b31b2-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="b31b2-131">Otevřete \Views\Movies\Index.aspx šablonu a přidejte &lt;th&gt;hodnocení&lt;/th&gt; k začátku tabulky Movie.</span><span class="sxs-lookup"><span data-stu-id="b31b2-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="b31b2-132">Po přidání dolovat po žánr.</span><span class="sxs-lookup"><span data-stu-id="b31b2-132">I added mine after Genre.</span></span> <span data-ttu-id="b31b2-133">Potom na stejné pozici sloupce, ale níže, přidejte řádek pro výstup naší nové hodnocení.</span><span class="sxs-lookup"><span data-stu-id="b31b2-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="b31b2-134">Naše finální šablona Index.aspx bude vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="b31b2-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="b31b2-135">Pojďme potom otevřete \Views\Movies\Create.aspx šablonu a přidejte popisek a textové pole pro naše nové vlastnosti hodnocení:</span><span class="sxs-lookup"><span data-stu-id="b31b2-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="b31b2-136">Naše finální šablona Create.aspx bude vypadat nějak takto a změňme náš prohlížeč title a sekundární &lt;h2&gt; název na něco jako "Vytvořit video" Když jsme tady!</span><span class="sxs-lookup"><span data-stu-id="b31b2-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="b31b2-137">Spusťte aplikaci a teď máte v databázi, který se přidal na stránku vytvořit nové pole.</span><span class="sxs-lookup"><span data-stu-id="b31b2-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="b31b2-138">Přidání nového videa – tentokrát s hodnocení – a klikněte na tlačítko vytvořit.</span><span class="sxs-lookup"><span data-stu-id="b31b2-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="b31b2-139">[![Vytvořit aplikaci Windows Internet Explorer film-](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="b31b2-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="b31b2-140">Po kliknutí na vytvořit přecházíte na indexovou stránku tam, kde jste nové video je uvedený s nového hodnocení sloupce v databázi</span><span class="sxs-lookup"><span data-stu-id="b31b2-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="b31b2-141">[![Seznam film – Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="b31b2-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="b31b2-142">V tomto kurzu základní teď vám pomůže začít vytváření Kontrolerů, jejich přidružení k zobrazení a předáním kolem údajů o pevně zakódované.</span><span class="sxs-lookup"><span data-stu-id="b31b2-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="b31b2-143">Potom jsme vytvořili určená databáze a vložte některá data v.</span><span class="sxs-lookup"><span data-stu-id="b31b2-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="b31b2-144">Jsme načíst data z databáze a zobrazí ho v tabulku HTML.</span><span class="sxs-lookup"><span data-stu-id="b31b2-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="b31b2-145">Potom jsme přidali vytvořit formulář, který umožní uživateli přidat data do databáze sami z v rámci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b31b2-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="b31b2-146">Jsme přidali ověřování a poté provedli ověření pomocí jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="b31b2-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="b31b2-147">Nakonec jsme změnit databáze, kterou chcete zahrnout nový sloupec dat a potom aktualizovat naše dvě stránky, které umožňuje vytvořit a zobrazit tato nová data.</span><span class="sxs-lookup"><span data-stu-id="b31b2-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="b31b2-148">Nyní neváhejte se přejít na náš kurz pro středně "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" a také mnoho videa a materiály v [ https://asp.net/mvc ](https://asp.net/mvc) získat i další informace o ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="b31b2-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="b31b2-149">Užijte si!</span><span class="sxs-lookup"><span data-stu-id="b31b2-149">Enjoy!</span></span>

- <span data-ttu-id="b31b2-150">Scott Hanselman – [ http://hanselman.com ](http://hanselman.com) a [ @shanselman ](http://twitter.com/shanselman) na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="b31b2-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b31b2-151">Předchozí</span><span class="sxs-lookup"><span data-stu-id="b31b2-151">Previous</span></span>](getting-started-with-mvc-part7.md)
