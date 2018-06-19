---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Přidání nové pole film modelu a databázové tabulky (VB) | Microsoft Docs
author: Rick-Anderson
description: V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5927b7d977e375881fe618b4b844cbd708023ba1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877317"
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a><span data-ttu-id="7646f-103">Přidání nové pole film modelu a databázové tabulky (VB)</span><span class="sxs-lookup"><span data-stu-id="7646f-103">Adding a New Field to the Movie Model and Database Table (VB)</span></span>
====================
<span data-ttu-id="7646f-104">podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7646f-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="7646f-105">V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7646f-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="7646f-106">Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="7646f-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="7646f-107">Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="7646f-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="7646f-108">Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="7646f-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="7646f-109">Visual Studio Web Developer Express SP1 požadavky</span><span class="sxs-lookup"><span data-stu-id="7646f-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="7646f-110">Aktualizace nástrojů rozhraní ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="7646f-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="7646f-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)</span><span class="sxs-lookup"><span data-stu-id="7646f-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="7646f-112">Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="7646f-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="7646f-113">Projekt Visual Web Developer se VB.NET zdrojový kód je k dispozici v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="7646f-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="7646f-114">[Stáhnout verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="7646f-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="7646f-115">Pokud dáváte přednost C#, přepnout [C# verze](../cs/adding-a-new-field.md) tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="7646f-115">If you prefer C#, switch to the [C# version](../cs/adding-a-new-field.md) of this tutorial.</span></span>


<span data-ttu-id="7646f-116">V této části můžete provést některé změny třídy modelu a zjistěte, jak můžete aktualizovat schéma databáze tak, aby odpovídaly změny modelu.</span><span class="sxs-lookup"><span data-stu-id="7646f-116">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="7646f-117">Přidání vlastnosti hodnocení filmu modelu</span><span class="sxs-lookup"><span data-stu-id="7646f-117">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="7646f-118">Začněte přidáním nové `Rating` vlastnost, která má existující `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="7646f-118">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="7646f-119">Otevřete *Movie.cs* souboru a přidejte `Rating` vlastnost podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="7646f-119">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

<span data-ttu-id="7646f-120">Kompletní `Movie` třídy nyní vypadá podobně jako následující kód:</span><span class="sxs-lookup"><span data-stu-id="7646f-120">The complete `Movie` class now looks like the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

<span data-ttu-id="7646f-121">Znovu zkompiluje aplikace pomocí **ladění** &gt; **sestavení film** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="7646f-121">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="7646f-122">Teď, když jste aktualizovali `Model` třídy, je také potřeba aktualizovat *\Views\Movies\Index.vbhtml* a *\Views\Movies\Create.vbhtml* zobrazí šablony za účelem podpory nové `Rating`vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7646f-122">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.vbhtml* and *\Views\Movies\Create.vbhtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="7646f-123">Otevřete<em>\Views\Movies\Index.vbhtml</em> souboru a přidejte `<th>Rating</th>` právě po záhlaví sloupce <strong>cena</strong> sloupce.</span><span class="sxs-lookup"><span data-stu-id="7646f-123">Open the<em>\Views\Movies\Index.vbhtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="7646f-124">Pak přidejte `<td>` sloupce u konce šablony k vykreslení `@item.Rating` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7646f-124">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="7646f-125">Níže je co aktualizované <em>Index.vbhtml</em> zobrazit šablonu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="7646f-125">Below is what the updated <em>Index.vbhtml</em> view template looks like:</span></span>

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

<span data-ttu-id="7646f-126">Dále otevřete *\Views\Movies\Create.vbhtml* souboru a přidejte následující značku u konce formuláře.</span><span class="sxs-lookup"><span data-stu-id="7646f-126">Next, open the *\Views\Movies\Create.vbhtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="7646f-127">To vykresluje textové pole tak, aby při vytváření nové film můžete zadat hodnocení.</span><span class="sxs-lookup"><span data-stu-id="7646f-127">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="7646f-128">Správa modelu a rozdíly schématu databáze</span><span class="sxs-lookup"><span data-stu-id="7646f-128">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="7646f-129">Nyní po aktualizaci kódu aplikace, které podporují nové `Rating` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7646f-129">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="7646f-130">Nyní spusťte aplikaci a přejděte do */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="7646f-130">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="7646f-131">Když to uděláte, se ale zobrazí následující chyba:</span><span class="sxs-lookup"><span data-stu-id="7646f-131">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="7646f-132">Protože se tato chyba zobrazuje aktualizovaný `Movie` třídy modelu v aplikaci je nyní liší od schématu `Movie` tabulky existující databáze.</span><span class="sxs-lookup"><span data-stu-id="7646f-132">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="7646f-133">(Není žádná `Rating` sloupec v tabulce databáze.)</span><span class="sxs-lookup"><span data-stu-id="7646f-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="7646f-134">Ve výchozím nastavení při použití Entity Framework Code First automaticky vytvořit databázi, stejně jako dříve v tomto kurzu Code First přidá tabulku k databázi chcete-li sledovat, jestli je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="7646f-134">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="7646f-135">Pokud nejsou synchronizované, rozhraní Entity Framework vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="7646f-135">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="7646f-136">To usnadňuje sledovat problémy v čase vývoj, který může jinak pouze pro vás (podle skrytého chyby) v době běhu.</span><span class="sxs-lookup"><span data-stu-id="7646f-136">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="7646f-137">Funkci Kontrola synchronizace je co způsobí, že se chybová zpráva, který se má zobrazit, jste viděli.</span><span class="sxs-lookup"><span data-stu-id="7646f-137">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="7646f-138">Řešení chyby dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="7646f-138">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="7646f-139">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi na základě nové třídy schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="7646f-139">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="7646f-140">Tento přístup je velmi vhodné při provádění active vývoj v testovací databázi, protože umožňuje rychle společně momentální schéma modelu a databáze.</span><span class="sxs-lookup"><span data-stu-id="7646f-140">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="7646f-141">Nevýhodou, ale je, že přijdete o stávající data v databázi – tak můžete *nemáte* chcete použít tuto metodu na produkční databázi!</span><span class="sxs-lookup"><span data-stu-id="7646f-141">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="7646f-142">Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="7646f-142">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="7646f-143">Výhodou tohoto přístupu je, že zachováte data.</span><span class="sxs-lookup"><span data-stu-id="7646f-143">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="7646f-144">Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.</span><span class="sxs-lookup"><span data-stu-id="7646f-144">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="7646f-145">V tomto kurzu použijeme prvním přístupem – budete mít Entity Framework Code First automaticky znovu vytvořit databázi kdykoliv změny modelu.</span><span class="sxs-lookup"><span data-stu-id="7646f-145">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="7646f-146">Automaticky znovu vytvořit databázi na změny modelu</span><span class="sxs-lookup"><span data-stu-id="7646f-146">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="7646f-147">Umožňuje aktualizovat aplikaci tak, aby Code First automaticky zahodí a znovu vytvoří databázi kdykoliv změnit modelu pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="7646f-147">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="7646f-148">**Upozornění** byste měli povolit tento přístup automaticky vyřadit a znovu vytvořit databázi pouze v případě, že používáte vývoj nebo testování databáze, a *nikdy* na provozní databázi, která obsahuje skutečná data.</span><span class="sxs-lookup"><span data-stu-id="7646f-148">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="7646f-149">Pomocí na provozním serveru může způsobit ztrátu dat.</span><span class="sxs-lookup"><span data-stu-id="7646f-149">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="7646f-150">V **Průzkumníku řešení**, klikněte pravým tlačítkem *modely* složky, vyberte **přidat**a potom vyberte **třída**.</span><span class="sxs-lookup"><span data-stu-id="7646f-150">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="7646f-151">Název třídy &quot;MovieInitializer&quot;.</span><span class="sxs-lookup"><span data-stu-id="7646f-151">Name the class &quot;MovieInitializer&quot;.</span></span> <span data-ttu-id="7646f-152">Aktualizace `MovieInitializer` třída obsahuje následující kód:</span><span class="sxs-lookup"><span data-stu-id="7646f-152">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

<span data-ttu-id="7646f-153">`MovieInitializer` Třída určuje, že by měl být databáze používá model vyřadit a automaticky znovu vytvoří Pokud někdy změnit třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="7646f-153">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="7646f-154">Tento kód obsahuje `Seed` metoda zadat některá data výchozí automaticky přidat do databáze žádný čas ji vytvořit (nebo znovu vytvořit).</span><span class="sxs-lookup"><span data-stu-id="7646f-154">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="7646f-155">To poskytuje vhodný způsob, jak naplnění databáze ukázková data, aniž by bylo potřeba ručně přidejte do ní pokaždé, když provedete model změnit.</span><span class="sxs-lookup"><span data-stu-id="7646f-155">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="7646f-156">Teď, když jste definovali `MovieInitializer` třídy, budete se muset propojit se tak, aby pokaždé, když je aplikace spuštěná, se ověří, zda třídy modelu jsou odlišné od schématu v databázi.</span><span class="sxs-lookup"><span data-stu-id="7646f-156">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="7646f-157">Pokud ano, můžete spustit inicializátoru znovu vytvořit databázi na model a pak naplnit databázi s ukázkovými daty.</span><span class="sxs-lookup"><span data-stu-id="7646f-157">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="7646f-158">Otevřete *Global.asax* soubor, který je v kořenovém adresáři `MvcMovies` projektu:</span><span class="sxs-lookup"><span data-stu-id="7646f-158">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

<span data-ttu-id="7646f-159">*Global.asax* soubor obsahuje třídu, která definuje bude celá aplikace pro projekt a obsahuje `Application_Start` obslužné rutiny události, která se spouští při prvním spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="7646f-159">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="7646f-160">Najít `Application_Start` metoda a přidejte volání `Database.SetInitializer` na začátku metodu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="7646f-160">Find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

<span data-ttu-id="7646f-161">`Database.SetInitializer` Příkaz jste právě přidali označuje, že databáze použít pomocí `MovieDBContext` instance by měl být automaticky odstraní a znovu vytvoří Pokud schéma a databáze se neshodují.</span><span class="sxs-lookup"><span data-stu-id="7646f-161">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="7646f-162">A protože jste viděli, bude také naplnit databázi s ukázkovými daty, která je zadána v `MovieInitializer` třídy.</span><span class="sxs-lookup"><span data-stu-id="7646f-162">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="7646f-163">Zavřít *Global.asax* souboru.</span><span class="sxs-lookup"><span data-stu-id="7646f-163">Close the *Global.asax* file.</span></span>

<span data-ttu-id="7646f-164">Znovu spusťte aplikaci a přejděte do */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="7646f-164">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="7646f-165">Při spuštění aplikace zjistí, zda jejich struktura model už odpovídá schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="7646f-165">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="7646f-166">Automaticky znovu vytvoří databázi tak, aby odpovídala nové modelovou strukturu a naplní databázi s filmy ukázka:</span><span class="sxs-lookup"><span data-stu-id="7646f-166">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

<span data-ttu-id="7646f-168">Klikněte **vytvořit nový** odkaz na přidání nové videa.</span><span class="sxs-lookup"><span data-stu-id="7646f-168">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="7646f-169">Všimněte si, že můžete přidat hodnocení.</span><span class="sxs-lookup"><span data-stu-id="7646f-169">Note that you can add a rating.</span></span>

<span data-ttu-id="7646f-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="7646f-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span></span>

<span data-ttu-id="7646f-171">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7646f-171">Click **Create**.</span></span> <span data-ttu-id="7646f-172">Nové film, včetně hodnocení, se zobrazí v filmy výpis:</span><span class="sxs-lookup"><span data-stu-id="7646f-172">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

<span data-ttu-id="7646f-174">V této části jste viděli, jak můžete upravit objekty modelu a synchronizujte databázi s změny.</span><span class="sxs-lookup"><span data-stu-id="7646f-174">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="7646f-175">Také jste zjistili, způsob, jak naplnit nově vytvořenou databázi s ukázkovými daty, můžete zkusit scénáře.</span><span class="sxs-lookup"><span data-stu-id="7646f-175">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="7646f-176">V dalším kroku podíváme, jak můžete přidat do třídy modelu bohatší logiku ověření a povolit některé obchodní pravidla, která budou vynucena.</span><span class="sxs-lookup"><span data-stu-id="7646f-176">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7646f-177">[Předchozí](examining-the-edit-methods-and-edit-view.md)
> [další](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="7646f-177">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
