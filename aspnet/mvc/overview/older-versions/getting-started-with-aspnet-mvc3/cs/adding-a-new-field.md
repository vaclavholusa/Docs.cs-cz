---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Přidání nového pole do modelu a tabulky Movie (C#) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 91b02f9991b714f8da2aa736c9ba5e58a7228350
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829069"
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a><span data-ttu-id="d5dd9-103">Přidání nového pole do modelu a tabulky Movie (C#)</span><span class="sxs-lookup"><span data-stu-id="d5dd9-103">Adding a New Field to the Movie Model and Table (C#)</span></span>
====================
<span data-ttu-id="d5dd9-104">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d5dd9-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="d5dd9-105">Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="d5dd9-106">Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="d5dd9-107">V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="d5dd9-108">Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="d5dd9-109">Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="d5dd9-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="d5dd9-110">Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="d5dd9-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="d5dd9-111">Visual Studio Web Developer Express SP1 požadavky</span><span class="sxs-lookup"><span data-stu-id="d5dd9-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="d5dd9-112">ASP.NET MVC 3 nástroje Update</span><span class="sxs-lookup"><span data-stu-id="d5dd9-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="d5dd9-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)</span><span class="sxs-lookup"><span data-stu-id="d5dd9-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="d5dd9-114">Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="d5dd9-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="d5dd9-115">Projekt aplikace Visual Web Developer se zdrojovým kódem jazyka C# je k dispozici v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="d5dd9-116">[Stáhněte si verzi C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="d5dd9-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="d5dd9-117">Pokud dáváte přednost jazyka Visual Basic, přejděte [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="d5dd9-118">V této části budete provádět některé změny tříd modelu a zjistěte, jak můžete aktualizovat schéma databáze tak, aby odpovídaly změny modelu.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-118">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="d5dd9-119">Přidání vlastnosti do hodnocení filmů modelu</span><span class="sxs-lookup"><span data-stu-id="d5dd9-119">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="d5dd9-120">Začněte přidáním nového `Rating` vlastnost ke stávající `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-120">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="d5dd9-121">Otevřít *Movie.cs* a přidejte `Rating` vlastnost podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="d5dd9-121">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="d5dd9-122">Kompletní `Movie` třídy teď vypadá jako v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="d5dd9-122">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

<span data-ttu-id="d5dd9-123">Znovu zkompilovat aplikaci pomocí **ladění** &gt; **sestavení film** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-123">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="d5dd9-124">Teď, když jste aktualizovali `Model` třídy, je také potřeba aktualizovat *\Views\Movies\Index.cshtml* a *\Views\Movies\Create.cshtml* zobrazení šablon pro podporu nového `Rating`vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-124">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="d5dd9-125">Otevřít *\Views\Movies\Index.cshtml* a přidejte `<th>Rating</th>` hned za záhlaví sloupce **cena** sloupec.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-125">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="d5dd9-126">Pak přidejte `<td>` sloupec blíží ke konci šablonu k vykreslení `@item.Rating` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-126">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="d5dd9-127">Níže je co aktualizované *Index.cshtml* zobrazit šablonu vypadá jako:</span><span class="sxs-lookup"><span data-stu-id="d5dd9-127">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

<span data-ttu-id="d5dd9-128">Dále otevřete *\Views\Movies\Create.cshtml* a přidejte následující kód na konci formuláře.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-128">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="d5dd9-129">Tím zkopírujete textové pole tak, aby hodnocení můžete zadat, když se vytvoří nová videa.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-129">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="d5dd9-130">Správa modelů a rozdíly ve schématu databáze</span><span class="sxs-lookup"><span data-stu-id="d5dd9-130">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="d5dd9-131">Teď když jste aktualizovali kód aplikace pro podporu nového `Rating` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-131">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="d5dd9-132">Nyní spusťte aplikaci a přejděte */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-132">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="d5dd9-133">Když toto provedete, se však zobrazí chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="d5dd9-133">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="d5dd9-134">Tato chyba se zobrazuje, protože aktualizovaný `Movie` třídy modelu v aplikaci je nyní liší od schématu `Movie` tabulky existující databáze.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-134">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="d5dd9-135">(Neexistuje žádný `Rating` sloupec v tabulce databáze.)</span><span class="sxs-lookup"><span data-stu-id="d5dd9-135">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="d5dd9-136">Ve výchozím nastavení při použití platformy Entity Framework Code First automaticky vytvořit databázi, jako jste to udělali dříve v tomto kurzu Code First přidá tabulku do databáze pro sledování, zda je synchronizovaný s tříd modelu, které byly vygenerovány z schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-136">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="d5dd9-137">Pokud nejsou synchronizované, Entity Framework vyvolá chybu.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-137">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="d5dd9-138">Díky tomu je snadněji sledovat problémy při vývoji, který může jinak pouze pro vás (pomocí skrytého chyby) v době běhu.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-138">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="d5dd9-139">Tato funkce kontroluje se synchronizace je, co způsobí, že chybová zpráva, který se má zobrazit, které jste viděli.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-139">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="d5dd9-140">Existují dva přístupy k vyřešení chyby:</span><span class="sxs-lookup"><span data-stu-id="d5dd9-140">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="d5dd9-141">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi založené na nové schéma třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-141">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="d5dd9-142">Tento přístup je příliš pohodlné při aktivním vývoji v testovací databázi, protože umožňuje rychlý rozvoj schématu modelu a databáze společně.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-142">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="d5dd9-143">Nevýhodou, je však dojít ke ztrátě existujících dat v databázi, tak můžete *není* chcete použít tento postup u provozní databáze!</span><span class="sxs-lookup"><span data-stu-id="d5dd9-143">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="d5dd9-144">Explicitně upravte schéma stávající databázi tak, aby odpovídalo tříd modelu.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-144">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="d5dd9-145">Výhodou tohoto přístupu je, že zachováte vaše data.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-145">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="d5dd9-146">Můžete tuto změnu provést buď ručně, nebo tak, že vytvoříte databázi změnit skript.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-146">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="d5dd9-147">V tomto kurzu použijeme první přístup, budete mít Entity Framework Code First automaticky znovu vytvořit databázi kdykoli změny modelu.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-147">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="d5dd9-148">Automatické opětovné vytvoření databáze na změny modelu</span><span class="sxs-lookup"><span data-stu-id="d5dd9-148">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="d5dd9-149">Umožňuje aktualizovat aplikaci tak, aby Code First automaticky sníží a znovu vytvoří databázi, můžete kdykoli změnit model pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-149">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d5dd9-150">**Upozornění** byste měli povolit tento přístup automaticky vyřadit a znovu vytvořit databázi pouze při použití databázi vývoj nebo testování a *nikdy* u provozní databáze, která obsahuje reálná data.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-150">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="d5dd9-151">Použití na provozním serveru může způsobit ztrátu dat.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-151">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="d5dd9-152">V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *modely* složky, vyberte **přidat**a pak vyberte **třídy**.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-152">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="d5dd9-153">Název třídy "MovieInitializer".</span><span class="sxs-lookup"><span data-stu-id="d5dd9-153">Name the class "MovieInitializer".</span></span> <span data-ttu-id="d5dd9-154">Aktualizace `MovieInitializer` třídy tak, aby obsahovala následující kód:</span><span class="sxs-lookup"><span data-stu-id="d5dd9-154">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="d5dd9-155">`MovieInitializer` Třída určuje, zda by měla být databáze používá model vyřadit a automaticky znovu vytvořena Pokud nikdy změnit tříd modelu.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-155">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="d5dd9-156">Tento kód obsahuje `Seed` metoda zadat některá data výchozí automaticky přidat až do databáze, když vytvořili (nebo opětovném vytváření).</span><span class="sxs-lookup"><span data-stu-id="d5dd9-156">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="d5dd9-157">To poskytuje vhodný způsob, jak naplnit databázi s ukázkovými daty, aniž by bylo potřeba ručně přidejte do ní pokaždé, když provedete modelu změnit.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-157">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="d5dd9-158">Teď, když jste definovali `MovieInitializer` třídy, je vhodné nastavit tak, aby pokaždé, když je aplikace spuštěná, zkontroluje, jestli se liší od schématu databáze třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-158">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="d5dd9-159">Pokud ano, můžete spustit inicializátor znovu vytvořit databázi podle modelu a naplnit databázi s ukázkovými daty.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-159">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="d5dd9-160">Otevřít *Global.asax* soubor, který je v kořenovém adresáři `MvcMovies` projektu:</span><span class="sxs-lookup"><span data-stu-id="d5dd9-160">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

<span data-ttu-id="d5dd9-161">*Global.asax* soubor obsahuje třídu, která definuje celé aplikace pro projekt a obsahuje `Application_Start` obslužná rutina události, která se spouští při prvním spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-161">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="d5dd9-162">Přidejme dva příkazy using do horní části souboru.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-162">Let's add two using statements to the top of the file.</span></span> <span data-ttu-id="d5dd9-163">První odkazuje na obor názvů Entity Framework, a druhý odkazuje na obor názvů kde naše `MovieInitializer` třídy životy:</span><span class="sxs-lookup"><span data-stu-id="d5dd9-163">The first references the Entity Framework namespace, and the second references the namespace where our `MovieInitializer` class lives:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

<span data-ttu-id="d5dd9-164">Vyhledejte `Application_Start` metoda a přidejte volání do `Database.SetInitializer` na začátku metody, jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="d5dd9-164">Then find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

<span data-ttu-id="d5dd9-165">`Database.SetInitializer` Jste právě přidali označuje, že databáze používané `MovieDBContext` instance by měl automaticky odstranit a znovu vytvořen, pokud se schéma a databáze se neshodují.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-165">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="d5dd9-166">A protože jste viděli, bude také naplnit databázi s ukázkovými daty, která je zadána v `MovieInitializer` třídy.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-166">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="d5dd9-167">Zavřít *Global.asax* souboru.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-167">Close the *Global.asax* file.</span></span>

<span data-ttu-id="d5dd9-168">Znovu spusťte aplikaci a přejděte */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-168">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="d5dd9-169">Při spuštění aplikace zjistí, zda jejich struktura model už odpovídá schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-169">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="d5dd9-170">Automaticky znovu vytvoří databázi tak, aby odpovídaly struktuře nový model a naplní databázi s ukázková videa:</span><span class="sxs-lookup"><span data-stu-id="d5dd9-170">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

<span data-ttu-id="d5dd9-172">Klikněte na tlačítko **vytvořit nový** odkaz na přidání nového videa.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-172">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="d5dd9-173">Všimněte si, že můžete přidat hodnocení.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-173">Note that you can add a rating.</span></span>

<span data-ttu-id="d5dd9-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="d5dd9-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span></span>

<span data-ttu-id="d5dd9-175">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-175">Click **Create**.</span></span> <span data-ttu-id="d5dd9-176">Tento nový film, včetně hodnocení, zobrazí se nově ve výpisu:</span><span class="sxs-lookup"><span data-stu-id="d5dd9-176">The new movie, including the rating, now shows up in the movies listing:</span></span>

<span data-ttu-id="d5dd9-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="d5dd9-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span></span>

<span data-ttu-id="d5dd9-178">V této části jste viděli, jak můžete upravit objekty modelu a udržovat synchronizované s změny databáze.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-178">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="d5dd9-179">Také jste se naučili způsob, jak naplnit nově vytvořenou databázi s ukázkovými daty, takže si můžete vyzkoušet scénáře.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-179">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="d5dd9-180">V dalším kroku Podívejme se na jak můžete přidat bohatší logiku ověřování na třídy modelu a povolit některé obchodní pravidla, která budou vynucena.</span><span class="sxs-lookup"><span data-stu-id="d5dd9-180">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d5dd9-181">[Předchozí](examining-the-edit-methods-and-edit-view.md)
> [další](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="d5dd9-181">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
