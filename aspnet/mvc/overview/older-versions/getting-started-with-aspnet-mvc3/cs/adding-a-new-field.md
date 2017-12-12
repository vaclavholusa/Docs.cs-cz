---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: "Přidání nové pole do modelu film a tabulky (C#) | Microsoft Docs"
author: Rick-Anderson
description: "V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: c10f3be30a92a605c34fa1c56fa3691389374beb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a><span data-ttu-id="eb0c4-103">Přidání nové pole do modelu film a tabulky (C#)</span><span class="sxs-lookup"><span data-stu-id="eb0c4-103">Adding a New Field to the Movie Model and Table (C#)</span></span>
====================
<span data-ttu-id="eb0c4-104">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="eb0c4-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="eb0c4-105">Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="eb0c4-106">Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="eb0c4-107">V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="eb0c4-108">Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="eb0c4-109">Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="eb0c4-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="eb0c4-110">Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="eb0c4-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="eb0c4-111">Visual Studio Web Developer Express SP1 požadavky</span><span class="sxs-lookup"><span data-stu-id="eb0c4-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="eb0c4-112">Aktualizace nástrojů rozhraní ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="eb0c4-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="eb0c4-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)</span><span class="sxs-lookup"><span data-stu-id="eb0c4-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="eb0c4-114">Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="eb0c4-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="eb0c4-115">Projekt Visual Web Developer se zdrojový kód C# je k dispozici v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="eb0c4-116">[Stáhnout verzi jazyka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="eb0c4-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="eb0c4-117">Pokud dáváte přednost jazyka Visual Basic, přepnout [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="eb0c4-118">V této části můžete provést některé změny třídy modelu a zjistěte, jak můžete aktualizovat schéma databáze tak, aby odpovídaly změny modelu.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-118">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="eb0c4-119">Přidání vlastnosti hodnocení filmu modelu</span><span class="sxs-lookup"><span data-stu-id="eb0c4-119">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="eb0c4-120">Začněte přidáním nové `Rating` vlastnost, která má existující `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-120">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="eb0c4-121">Otevřete *Movie.cs* souboru a přidejte `Rating` vlastnost podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="eb0c4-121">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="eb0c4-122">Kompletní `Movie` třídy nyní vypadá podobně jako následující kód:</span><span class="sxs-lookup"><span data-stu-id="eb0c4-122">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

<span data-ttu-id="eb0c4-123">Znovu zkompiluje aplikace pomocí **ladění** &gt; **sestavení film** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-123">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="eb0c4-124">Teď, když jste aktualizovali `Model` třídy, je také potřeba aktualizovat *\Views\Movies\Index.cshtml* a *\Views\Movies\Create.cshtml* zobrazí šablony za účelem podpory nové `Rating`vlastnost.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-124">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="eb0c4-125">Otevřete *\Views\Movies\Index.cshtml* souboru a přidejte `<th>Rating</th>` právě po záhlaví sloupce **cena** sloupce.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-125">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="eb0c4-126">Pak přidejte `<td>` sloupce u konce šablony k vykreslení `@item.Rating` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-126">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="eb0c4-127">Níže je co aktualizované *Index.cshtml* zobrazit šablonu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="eb0c4-127">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

<span data-ttu-id="eb0c4-128">Dále otevřete *\Views\Movies\Create.cshtml* souboru a přidejte následující značku u konce formuláře.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-128">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="eb0c4-129">To vykresluje textové pole tak, aby při vytváření nové film můžete zadat hodnocení.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-129">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="eb0c4-130">Správa modelu a rozdíly schématu databáze</span><span class="sxs-lookup"><span data-stu-id="eb0c4-130">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="eb0c4-131">Nyní po aktualizaci kódu aplikace, které podporují nové `Rating` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-131">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="eb0c4-132">Nyní spusťte aplikaci a přejděte do */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-132">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="eb0c4-133">Když to uděláte, se ale zobrazí následující chyba:</span><span class="sxs-lookup"><span data-stu-id="eb0c4-133">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="eb0c4-134">Protože se tato chyba zobrazuje aktualizovaný `Movie` třídy modelu v aplikaci je nyní liší od schématu `Movie` tabulky existující databáze.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-134">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="eb0c4-135">(Není žádná `Rating` sloupec v tabulce databáze.)</span><span class="sxs-lookup"><span data-stu-id="eb0c4-135">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="eb0c4-136">Ve výchozím nastavení při použití Entity Framework Code First automaticky vytvořit databázi, stejně jako dříve v tomto kurzu Code First přidá tabulku k databázi chcete-li sledovat, jestli je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-136">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="eb0c4-137">Pokud nejsou synchronizované, rozhraní Entity Framework vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-137">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="eb0c4-138">To usnadňuje sledovat problémy v čase vývoj, který může jinak pouze pro vás (podle skrytého chyby) v době běhu.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-138">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="eb0c4-139">Funkci Kontrola synchronizace je co způsobí, že se chybová zpráva, který se má zobrazit, jste viděli.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-139">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="eb0c4-140">Řešení chyby dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="eb0c4-140">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="eb0c4-141">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi na základě nové třídy schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-141">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="eb0c4-142">Tento přístup je velmi vhodné při provádění active vývoj v testovací databázi, protože umožňuje rychle společně momentální schéma modelu a databáze.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-142">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="eb0c4-143">Nevýhodou, ale je, že přijdete o stávající data v databázi – tak můžete *nemáte* chcete použít tuto metodu na produkční databázi!</span><span class="sxs-lookup"><span data-stu-id="eb0c4-143">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="eb0c4-144">Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-144">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="eb0c4-145">Výhodou tohoto přístupu je, že zachováte data.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-145">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="eb0c4-146">Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-146">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="eb0c4-147">V tomto kurzu použijeme prvním přístupem – budete mít Entity Framework Code First automaticky znovu vytvořit databázi kdykoliv změny modelu.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-147">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="eb0c4-148">Automaticky znovu vytvořit databázi na změny modelu</span><span class="sxs-lookup"><span data-stu-id="eb0c4-148">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="eb0c4-149">Umožňuje aktualizovat aplikaci tak, aby Code First automaticky zahodí a znovu vytvoří databázi kdykoliv změnit modelu pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-149">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="eb0c4-150">**Upozornění** byste měli povolit tento přístup automaticky vyřadit a znovu vytvořit databázi pouze v případě, že používáte vývoj nebo testování databáze, a *nikdy* na provozní databázi, která obsahuje skutečná data.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-150">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="eb0c4-151">Pomocí na provozním serveru může způsobit ztrátu dat.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-151">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="eb0c4-152">V **Průzkumníku řešení**, klikněte pravým tlačítkem *modely* složky, vyberte **přidat**a potom vyberte **třída**.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-152">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="eb0c4-153">Název třídy "MovieInitializer".</span><span class="sxs-lookup"><span data-stu-id="eb0c4-153">Name the class "MovieInitializer".</span></span> <span data-ttu-id="eb0c4-154">Aktualizace `MovieInitializer` třída obsahuje následující kód:</span><span class="sxs-lookup"><span data-stu-id="eb0c4-154">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="eb0c4-155">`MovieInitializer` Třída určuje, že by měl být databáze používá model vyřadit a automaticky znovu vytvoří Pokud někdy změnit třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-155">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="eb0c4-156">Tento kód obsahuje `Seed` metoda zadat některá data výchozí automaticky přidat do databáze žádný čas ji vytvořit (nebo znovu vytvořit).</span><span class="sxs-lookup"><span data-stu-id="eb0c4-156">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="eb0c4-157">To poskytuje vhodný způsob, jak naplnění databáze ukázková data, aniž by bylo potřeba ručně přidejte do ní pokaždé, když provedete model změnit.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-157">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="eb0c4-158">Teď, když jste definovali `MovieInitializer` třídy, budete se muset propojit se tak, aby pokaždé, když je aplikace spuštěná, se ověří, zda třídy modelu jsou odlišné od schématu v databázi.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-158">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="eb0c4-159">Pokud ano, můžete spustit inicializátoru znovu vytvořit databázi na model a pak naplnit databázi s ukázkovými daty.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-159">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="eb0c4-160">Otevřete *Global.asax* soubor, který je v kořenovém adresáři `MvcMovies` projektu:</span><span class="sxs-lookup"><span data-stu-id="eb0c4-160">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

<span data-ttu-id="eb0c4-161">*Global.asax* soubor obsahuje třídu, která definuje bude celá aplikace pro projekt a obsahuje `Application_Start` obslužné rutiny události, která se spouští při prvním spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-161">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="eb0c4-162">Přidejme dva příkazy using do horní části souboru.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-162">Let's add two using statements to the top of the file.</span></span> <span data-ttu-id="eb0c4-163">První odkazuje na obor názvů Entity Framework, a druhá odkazuje na obor názvů kde naše `MovieInitializer` třídy život:</span><span class="sxs-lookup"><span data-stu-id="eb0c4-163">The first references the Entity Framework namespace, and the second references the namespace where our `MovieInitializer` class lives:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

<span data-ttu-id="eb0c4-164">Vyhledejte `Application_Start` metoda a přidejte volání `Database.SetInitializer` na začátku metodu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="eb0c4-164">Then find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

<span data-ttu-id="eb0c4-165">`Database.SetInitializer` Příkaz jste právě přidali označuje, že databáze použít pomocí `MovieDBContext` instance by měl být automaticky odstraní a znovu vytvoří Pokud schéma a databáze se neshodují.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-165">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="eb0c4-166">A protože jste viděli, bude také naplnit databázi s ukázkovými daty, která je zadána v `MovieInitializer` třídy.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-166">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="eb0c4-167">Zavřít *Global.asax* souboru.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-167">Close the *Global.asax* file.</span></span>

<span data-ttu-id="eb0c4-168">Znovu spusťte aplikaci a přejděte do */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-168">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="eb0c4-169">Při spuštění aplikace zjistí, zda jejich struktura model už odpovídá schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-169">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="eb0c4-170">Automaticky znovu vytvoří databázi tak, aby odpovídala nové modelovou strukturu a naplní databázi s filmy ukázka:</span><span class="sxs-lookup"><span data-stu-id="eb0c4-170">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

<span data-ttu-id="eb0c4-172">Klikněte **vytvořit nový** odkaz na přidání nové videa.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-172">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="eb0c4-173">Všimněte si, že můžete přidat hodnocení.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-173">Note that you can add a rating.</span></span>

<span data-ttu-id="eb0c4-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="eb0c4-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span></span>

<span data-ttu-id="eb0c4-175">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-175">Click **Create**.</span></span> <span data-ttu-id="eb0c4-176">Nové film, včetně hodnocení, se zobrazí v filmy výpis:</span><span class="sxs-lookup"><span data-stu-id="eb0c4-176">The new movie, including the rating, now shows up in the movies listing:</span></span>

<span data-ttu-id="eb0c4-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="eb0c4-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span></span>

<span data-ttu-id="eb0c4-178">V této části jste viděli, jak můžete upravit objekty modelu a synchronizujte databázi s změny.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-178">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="eb0c4-179">Také jste zjistili, způsob, jak naplnit nově vytvořenou databázi s ukázkovými daty, můžete zkusit scénáře.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-179">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="eb0c4-180">V dalším kroku podíváme, jak můžete přidat do třídy modelu bohatší logiku ověření a povolit některé obchodní pravidla, která budou vynucena.</span><span class="sxs-lookup"><span data-stu-id="eb0c4-180">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="eb0c4-181">[Předchozí](examining-the-edit-methods-and-edit-view.md)
[další](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="eb0c4-181">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
