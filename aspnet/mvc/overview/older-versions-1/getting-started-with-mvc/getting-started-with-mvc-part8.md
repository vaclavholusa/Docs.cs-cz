---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: "Přidání sloupce do modelu | Microsoft Docs"
author: shanselman
description: "Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 4a4fc144dcbed8ad14411332df7acccc032ddf46
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="aef6b-104">Přidání sloupce do modelu</span><span class="sxs-lookup"><span data-stu-id="aef6b-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="aef6b-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="aef6b-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="aef6b-106">Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="aef6b-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="aef6b-107">Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.</span><span class="sxs-lookup"><span data-stu-id="aef6b-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="aef6b-108">Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="aef6b-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="aef6b-109">V této části přidáme provede jak můžeme provést změny schématu databáze a zpracovat změny v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aef6b-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="aef6b-110">Přidejme do tabulky film slo se "Hodnocení".</span><span class="sxs-lookup"><span data-stu-id="aef6b-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="aef6b-111">Vraťte se zpátky a integrovaného vývojového prostředí a kliknutím na Průzkumníka databáze.</span><span class="sxs-lookup"><span data-stu-id="aef6b-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="aef6b-112">Klikněte pravým tlačítkem v tabulce film a vyberte otevřete definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="aef6b-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="aef6b-113">Přidá sloupec "Hodnocení", jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="aef6b-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="aef6b-114">Vzhledem k tomu, že teď nemáme žádné hodnocení, sloupci povolit hodnoty Null.</span><span class="sxs-lookup"><span data-stu-id="aef6b-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="aef6b-115">Klikněte na tlačítko Uložit.</span><span class="sxs-lookup"><span data-stu-id="aef6b-115">Click Save.</span></span>

<span data-ttu-id="aef6b-116">[![Úpravy filmy tabulky](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="aef6b-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="aef6b-117">V dalším kroku se vrátí do Průzkumníka řešení a otevře soubor Movies.edmx (což je ve složce \Models).</span><span class="sxs-lookup"><span data-stu-id="aef6b-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="aef6b-118">Klikněte pravým tlačítkem na návrhovou plochu (bílé oblasti) a vyberte aktualizovat Model z databáze.</span><span class="sxs-lookup"><span data-stu-id="aef6b-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="aef6b-119">[![Filmy - sadu Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="aef6b-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="aef6b-120">Tím se spustí Průvodce aktualizací"".</span><span class="sxs-lookup"><span data-stu-id="aef6b-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="aef6b-121">Klikněte na kartu aktualizace v něm a klikněte na tlačítko Dokončit.</span><span class="sxs-lookup"><span data-stu-id="aef6b-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="aef6b-122">Naše třída modelu film bude potom aktualizovány nového sloupce.</span><span class="sxs-lookup"><span data-stu-id="aef6b-122">Our Movie model class will then be updated with the new column.</span></span>

![Průvodce aktualizací (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="aef6b-124">Po kliknutí na tlačítko Dokončit, uvidíte, že nový sloupec hodnocení přidala film Entity v našem modelu.</span><span class="sxs-lookup"><span data-stu-id="aef6b-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="aef6b-125">[![Film Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="aef6b-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="aef6b-126">Přidali jsme sloupec ve model databáze, ale zobrazení neznáte o něm.</span><span class="sxs-lookup"><span data-stu-id="aef6b-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="aef6b-127">Aktualizace zobrazení s změny modelu</span><span class="sxs-lookup"><span data-stu-id="aef6b-127">Update Views with Model Changes</span></span>

<span data-ttu-id="aef6b-128">Aktualizujeme může našich šablon zobrazení tak, aby odrážela nový sloupec hodnocení několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="aef6b-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="aef6b-129">Vzhledem k tomu, že jsme vytvořili tato zobrazení vygenerováním pomocí dialogu přidat zobrazení, jsme může odstranit a znovu vytvořte je znovu.</span><span class="sxs-lookup"><span data-stu-id="aef6b-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="aef6b-130">Obvykle však osoby bude již provedli změny jejich zobrazení šablon z počáteční vygenerované generování a bude chcete přidat nebo odstranit pole ručně, stejně jako jsme to udělali s pole ID pro vytvoření.</span><span class="sxs-lookup"><span data-stu-id="aef6b-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="aef6b-131">Otevře \Views\Movies\Index.aspx šablony a přidat &lt;tý&gt;hodnocení&lt;/th&gt; k head film tabulky.</span><span class="sxs-lookup"><span data-stu-id="aef6b-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="aef6b-132">Po přidání dolování po Genre.</span><span class="sxs-lookup"><span data-stu-id="aef6b-132">I added mine after Genre.</span></span> <span data-ttu-id="aef6b-133">Potom ve stejné pozici sloupce, ale nižší dolů, přidejte řádek na výstup naší nové hodnocení.</span><span class="sxs-lookup"><span data-stu-id="aef6b-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="aef6b-134">Naše finální šablona Index.aspx bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="aef6b-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="aef6b-135">Umožňuje potom otevře \Views\Movies\Create.aspx šablony a přidat popisek a textové pole pro naše nové vlastnosti hodnocení:</span><span class="sxs-lookup"><span data-stu-id="aef6b-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="aef6b-136">Naše finální šablona Create.aspx bude vypadat takto a změňte název a sekundární naše prohlížeče &lt;h2&gt; nápis, který se něco podobného jako "Vytvořit film" při nám sem!</span><span class="sxs-lookup"><span data-stu-id="aef6b-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="aef6b-137">Spuštění aplikace a nyní máte k dispozici nové pole v databázi, který je přidán na stránku vytvořit.</span><span class="sxs-lookup"><span data-stu-id="aef6b-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="aef6b-138">Přidejte nové film - tentokrát s hodnocení - a klikněte na tlačítko vytvořit.</span><span class="sxs-lookup"><span data-stu-id="aef6b-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="aef6b-139">[![Vytvořit film – Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="aef6b-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="aef6b-140">Po kliknutí na tlačítko vytvořit, že posílá indexovou stránku tam, kde můžete nové film je označené nové hodnocení sloupec v databázi.</span><span class="sxs-lookup"><span data-stu-id="aef6b-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="aef6b-141">[![Seznam film – Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="aef6b-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="aef6b-142">V tomto kurzu základní tu můžete spustit provedením řadiče a jejich přidružení k zobrazení a předání kolem pevně data.</span><span class="sxs-lookup"><span data-stu-id="aef6b-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="aef6b-143">Potom jsme vytvořili a určená databáze a umístí některá data v.</span><span class="sxs-lookup"><span data-stu-id="aef6b-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="aef6b-144">Nemůžeme načíst data z databáze a zobrazí do tabulky HTML.</span><span class="sxs-lookup"><span data-stu-id="aef6b-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="aef6b-145">Potom jsme přidali vytvořit formulář, který umožní uživateli přidat data do databáze sami z webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="aef6b-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="aef6b-146">Jsme přidali ověření a potom provedené ověření pomocí jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="aef6b-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="aef6b-147">Nakonec jsme změnit databázi zahrnout nový sloupec dat a potom aktualizovat naše dvě stránky k vytváření a zobrazování tato nová data.</span><span class="sxs-lookup"><span data-stu-id="aef6b-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="aef6b-148">I nyní doporučujeme pokračovat v našem kurzu zprostředkující úrovni "[MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" a také mnoho videa a prostředkům v [https://asp.net/mvc](https://asp.net/mvc) i podrobné informace o architektuře ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="aef6b-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="aef6b-149">Užijte si ji!</span><span class="sxs-lookup"><span data-stu-id="aef6b-149">Enjoy!</span></span>

- <span data-ttu-id="aef6b-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) a [ @shanselman ](http://twitter.com/shanselman) na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="aef6b-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="aef6b-151">Předchozí</span><span class="sxs-lookup"><span data-stu-id="aef6b-151">Previous</span></span>](getting-started-with-mvc-part7.md)
