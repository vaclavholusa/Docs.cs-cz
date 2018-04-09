---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Přidání sloupce do modelu | Microsoft Docs
author: shanselman
description: Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoření jednoduché webové aplikace, která čte a zapisuje z databáze.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="e21d4-104">Přidání sloupce do modelu</span><span class="sxs-lookup"><span data-stu-id="e21d4-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="e21d4-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="e21d4-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="e21d4-106">Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e21d4-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="e21d4-107">Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.</span><span class="sxs-lookup"><span data-stu-id="e21d4-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="e21d4-108">Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e21d4-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="e21d4-109">V této části přidáme provede jak můžeme provést změny schématu databáze a zpracovat změny v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e21d4-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="e21d4-110">Přidejme do tabulky film slo se "Hodnocení".</span><span class="sxs-lookup"><span data-stu-id="e21d4-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="e21d4-111">Vraťte se zpátky a integrovaného vývojového prostředí a kliknutím na Průzkumníka databáze.</span><span class="sxs-lookup"><span data-stu-id="e21d4-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="e21d4-112">Klikněte pravým tlačítkem v tabulce film a vyberte otevřete definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="e21d4-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="e21d4-113">Přidá sloupec "Hodnocení", jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="e21d4-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="e21d4-114">Vzhledem k tomu, že teď nemáme žádné hodnocení, sloupci povolit hodnoty Null.</span><span class="sxs-lookup"><span data-stu-id="e21d4-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="e21d4-115">Klikněte na tlačítko Uložit.</span><span class="sxs-lookup"><span data-stu-id="e21d4-115">Click Save.</span></span>

<span data-ttu-id="e21d4-116">[![Úpravy filmy tabulky](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e21d4-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="e21d4-117">V dalším kroku se vrátí do Průzkumníka řešení a otevře soubor Movies.edmx (což je ve složce \Models).</span><span class="sxs-lookup"><span data-stu-id="e21d4-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="e21d4-118">Klikněte pravým tlačítkem na návrhovou plochu (bílé oblasti) a vyberte aktualizovat Model z databáze.</span><span class="sxs-lookup"><span data-stu-id="e21d4-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="e21d4-119">[![Filmy - sadu Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e21d4-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="e21d4-120">Tím se spustí Průvodce aktualizací"".</span><span class="sxs-lookup"><span data-stu-id="e21d4-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="e21d4-121">Klikněte na kartu aktualizace v něm a klikněte na tlačítko Dokončit.</span><span class="sxs-lookup"><span data-stu-id="e21d4-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="e21d4-122">Naše třída modelu film bude potom aktualizovány nového sloupce.</span><span class="sxs-lookup"><span data-stu-id="e21d4-122">Our Movie model class will then be updated with the new column.</span></span>

![Průvodce aktualizací (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="e21d4-124">Po kliknutí na tlačítko Dokončit, uvidíte, že nový sloupec hodnocení přidala film Entity v našem modelu.</span><span class="sxs-lookup"><span data-stu-id="e21d4-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="e21d4-125">[![Film Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="e21d4-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="e21d4-126">Přidali jsme sloupec ve model databáze, ale zobrazení neznáte o něm.</span><span class="sxs-lookup"><span data-stu-id="e21d4-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="e21d4-127">Aktualizace zobrazení s změny modelu</span><span class="sxs-lookup"><span data-stu-id="e21d4-127">Update Views with Model Changes</span></span>

<span data-ttu-id="e21d4-128">Aktualizujeme může našich šablon zobrazení tak, aby odrážela nový sloupec hodnocení několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="e21d4-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="e21d4-129">Vzhledem k tomu, že jsme vytvořili tato zobrazení vygenerováním pomocí dialogu přidat zobrazení, jsme může odstranit a znovu vytvořte je znovu.</span><span class="sxs-lookup"><span data-stu-id="e21d4-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="e21d4-130">Obvykle však osoby bude již provedli změny jejich zobrazení šablon z počáteční vygenerované generování a bude chcete přidat nebo odstranit pole ručně, stejně jako jsme to udělali s pole ID pro vytvoření.</span><span class="sxs-lookup"><span data-stu-id="e21d4-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="e21d4-131">Otevře \Views\Movies\Index.aspx šablony a přidat &lt;tý&gt;hodnocení&lt;/th&gt; k head film tabulky.</span><span class="sxs-lookup"><span data-stu-id="e21d4-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="e21d4-132">Po přidání dolování po Genre.</span><span class="sxs-lookup"><span data-stu-id="e21d4-132">I added mine after Genre.</span></span> <span data-ttu-id="e21d4-133">Potom ve stejné pozici sloupce, ale nižší dolů, přidejte řádek na výstup naší nové hodnocení.</span><span class="sxs-lookup"><span data-stu-id="e21d4-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="e21d4-134">Naše finální šablona Index.aspx bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="e21d4-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="e21d4-135">Umožňuje potom otevře \Views\Movies\Create.aspx šablony a přidat popisek a textové pole pro naše nové vlastnosti hodnocení:</span><span class="sxs-lookup"><span data-stu-id="e21d4-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="e21d4-136">Naše finální šablona Create.aspx bude vypadat takto a změňte název a sekundární naše prohlížeče &lt;h2&gt; nápis, který se něco podobného jako "Vytvořit film" při nám sem!</span><span class="sxs-lookup"><span data-stu-id="e21d4-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="e21d4-137">Spuštění aplikace a nyní máte k dispozici nové pole v databázi, který je přidán na stránku vytvořit.</span><span class="sxs-lookup"><span data-stu-id="e21d4-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="e21d4-138">Přidejte nové film - tentokrát s hodnocení - a klikněte na tlačítko vytvořit.</span><span class="sxs-lookup"><span data-stu-id="e21d4-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="e21d4-139">[![Vytvořit film – Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="e21d4-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="e21d4-140">Po kliknutí na tlačítko vytvořit, že posílá indexovou stránku tam, kde můžete nové film je označené nové hodnocení sloupec v databázi.</span><span class="sxs-lookup"><span data-stu-id="e21d4-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="e21d4-141">[![Seznam film – Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="e21d4-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="e21d4-142">V tomto kurzu základní tu můžete spustit provedením řadiče a jejich přidružení k zobrazení a předání kolem pevně data.</span><span class="sxs-lookup"><span data-stu-id="e21d4-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="e21d4-143">Potom jsme vytvořili a určená databáze a umístí některá data v.</span><span class="sxs-lookup"><span data-stu-id="e21d4-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="e21d4-144">Nemůžeme načíst data z databáze a zobrazí do tabulky HTML.</span><span class="sxs-lookup"><span data-stu-id="e21d4-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="e21d4-145">Potom jsme přidali vytvořit formulář, který umožní uživateli přidat data do databáze sami z webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e21d4-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="e21d4-146">Jsme přidali ověření a potom provedené ověření pomocí jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e21d4-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="e21d4-147">Nakonec jsme změnit databázi zahrnout nový sloupec dat a potom aktualizovat naše dvě stránky k vytváření a zobrazování tato nová data.</span><span class="sxs-lookup"><span data-stu-id="e21d4-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="e21d4-148">I nyní doporučujeme pokračovat v našem kurzu zprostředkující úrovni "[MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" a také mnoho videa a prostředkům v [ https://asp.net/mvc ](https://asp.net/mvc) i podrobné informace o architektuře ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="e21d4-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="e21d4-149">Užijte si ji!</span><span class="sxs-lookup"><span data-stu-id="e21d4-149">Enjoy!</span></span>

- <span data-ttu-id="e21d4-150">Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) a [ @shanselman ](http://twitter.com/shanselman) na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="e21d4-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e21d4-151">Předchozí</span><span class="sxs-lookup"><span data-stu-id="e21d4-151">Previous</span></span>](getting-started-with-mvc-part7.md)
