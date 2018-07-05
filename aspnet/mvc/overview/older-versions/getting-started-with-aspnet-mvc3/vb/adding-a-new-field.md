---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Přidání nového pole do modelu filmů a databázové tabulky (VB) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 06d9f08ea3e1a85327083639adc6aa0f2cfbaa48
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382937"
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a><span data-ttu-id="06398-103">Přidání nového pole do modelu filmů a databázové tabulky (VB)</span><span class="sxs-lookup"><span data-stu-id="06398-103">Adding a New Field to the Movie Model and Database Table (VB)</span></span>
====================
<span data-ttu-id="06398-104">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="06398-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="06398-105">V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="06398-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="06398-106">Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="06398-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="06398-107">Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="06398-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="06398-108">Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="06398-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="06398-109">Visual Studio Web Developer Express SP1 požadavky</span><span class="sxs-lookup"><span data-stu-id="06398-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="06398-110">ASP.NET MVC 3 nástroje Update</span><span class="sxs-lookup"><span data-stu-id="06398-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="06398-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)</span><span class="sxs-lookup"><span data-stu-id="06398-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="06398-112">Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="06398-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="06398-113">Projekt aplikace Visual Web Developer se zdrojovým kódem VB.NET je k dispozici v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="06398-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="06398-114">[Stáhněte si verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="06398-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="06398-115">Pokud dáváte přednost C#, přejděte [verze jazyka C#](../cs/adding-a-new-field.md) tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="06398-115">If you prefer C#, switch to the [C# version](../cs/adding-a-new-field.md) of this tutorial.</span></span>


<span data-ttu-id="06398-116">V této části budete provádět některé změny tříd modelu a zjistěte, jak můžete aktualizovat schéma databáze tak, aby odpovídaly změny modelu.</span><span class="sxs-lookup"><span data-stu-id="06398-116">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="06398-117">Přidání vlastnosti do hodnocení filmů modelu</span><span class="sxs-lookup"><span data-stu-id="06398-117">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="06398-118">Začněte přidáním nového `Rating` vlastnost ke stávající `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="06398-118">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="06398-119">Otevřít *Movie.cs* a přidejte `Rating` vlastnost podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="06398-119">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

<span data-ttu-id="06398-120">Kompletní `Movie` třídy teď vypadá jako v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="06398-120">The complete `Movie` class now looks like the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

<span data-ttu-id="06398-121">Znovu zkompilovat aplikaci pomocí **ladění** &gt; **sestavení film** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="06398-121">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="06398-122">Teď, když jste aktualizovali `Model` třídy, je také potřeba aktualizovat *\Views\Movies\Index.vbhtml* a *\Views\Movies\Create.vbhtml* zobrazení šablon pro podporu nového `Rating`vlastnost.</span><span class="sxs-lookup"><span data-stu-id="06398-122">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.vbhtml* and *\Views\Movies\Create.vbhtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="06398-123">Otevřít<em>\Views\Movies\Index.vbhtml</em> a přidejte `<th>Rating</th>` hned za záhlaví sloupce <strong>cena</strong> sloupec.</span><span class="sxs-lookup"><span data-stu-id="06398-123">Open the<em>\Views\Movies\Index.vbhtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="06398-124">Pak přidejte `<td>` sloupec blíží ke konci šablonu k vykreslení `@item.Rating` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="06398-124">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="06398-125">Níže je co aktualizované <em>Index.vbhtml</em> zobrazit šablonu vypadá jako:</span><span class="sxs-lookup"><span data-stu-id="06398-125">Below is what the updated <em>Index.vbhtml</em> view template looks like:</span></span>

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

<span data-ttu-id="06398-126">Dále otevřete *\Views\Movies\Create.vbhtml* a přidejte následující kód na konci formuláře.</span><span class="sxs-lookup"><span data-stu-id="06398-126">Next, open the *\Views\Movies\Create.vbhtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="06398-127">Tím zkopírujete textové pole tak, aby hodnocení můžete zadat, když se vytvoří nová videa.</span><span class="sxs-lookup"><span data-stu-id="06398-127">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="06398-128">Správa modelů a rozdíly ve schématu databáze</span><span class="sxs-lookup"><span data-stu-id="06398-128">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="06398-129">Teď když jste aktualizovali kód aplikace pro podporu nového `Rating` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="06398-129">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="06398-130">Nyní spusťte aplikaci a přejděte */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="06398-130">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="06398-131">Když toto provedete, se však zobrazí chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="06398-131">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="06398-132">Tato chyba se zobrazuje, protože aktualizovaný `Movie` třídy modelu v aplikaci je nyní liší od schématu `Movie` tabulky existující databáze.</span><span class="sxs-lookup"><span data-stu-id="06398-132">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="06398-133">(Neexistuje žádný `Rating` sloupec v tabulce databáze.)</span><span class="sxs-lookup"><span data-stu-id="06398-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="06398-134">Ve výchozím nastavení při použití platformy Entity Framework Code First automaticky vytvořit databázi, jako jste to udělali dříve v tomto kurzu Code First přidá tabulku do databáze pro sledování, zda je synchronizovaný s tříd modelu, které byly vygenerovány z schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="06398-134">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="06398-135">Pokud nejsou synchronizované, Entity Framework vyvolá chybu.</span><span class="sxs-lookup"><span data-stu-id="06398-135">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="06398-136">Díky tomu je snadněji sledovat problémy při vývoji, který může jinak pouze pro vás (pomocí skrytého chyby) v době běhu.</span><span class="sxs-lookup"><span data-stu-id="06398-136">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="06398-137">Tato funkce kontroluje se synchronizace je, co způsobí, že chybová zpráva, který se má zobrazit, které jste viděli.</span><span class="sxs-lookup"><span data-stu-id="06398-137">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="06398-138">Existují dva přístupy k vyřešení chyby:</span><span class="sxs-lookup"><span data-stu-id="06398-138">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="06398-139">Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi založené na nové schéma třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="06398-139">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="06398-140">Tento přístup je příliš pohodlné při aktivním vývoji v testovací databázi, protože umožňuje rychlý rozvoj schématu modelu a databáze společně.</span><span class="sxs-lookup"><span data-stu-id="06398-140">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="06398-141">Nevýhodou, je však dojít ke ztrátě existujících dat v databázi, tak můžete *není* chcete použít tento postup u provozní databáze!</span><span class="sxs-lookup"><span data-stu-id="06398-141">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="06398-142">Explicitně upravte schéma stávající databázi tak, aby odpovídalo tříd modelu.</span><span class="sxs-lookup"><span data-stu-id="06398-142">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="06398-143">Výhodou tohoto přístupu je, že zachováte vaše data.</span><span class="sxs-lookup"><span data-stu-id="06398-143">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="06398-144">Můžete tuto změnu provést buď ručně, nebo tak, že vytvoříte databázi změnit skript.</span><span class="sxs-lookup"><span data-stu-id="06398-144">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="06398-145">V tomto kurzu použijeme první přístup, budete mít Entity Framework Code First automaticky znovu vytvořit databázi kdykoli změny modelu.</span><span class="sxs-lookup"><span data-stu-id="06398-145">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="06398-146">Automatické opětovné vytvoření databáze na změny modelu</span><span class="sxs-lookup"><span data-stu-id="06398-146">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="06398-147">Umožňuje aktualizovat aplikaci tak, aby Code First automaticky sníží a znovu vytvoří databázi, můžete kdykoli změnit model pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="06398-147">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="06398-148">**Upozornění** byste měli povolit tento přístup automaticky vyřadit a znovu vytvořit databázi pouze při použití databázi vývoj nebo testování a *nikdy* u provozní databáze, která obsahuje reálná data.</span><span class="sxs-lookup"><span data-stu-id="06398-148">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="06398-149">Použití na provozním serveru může způsobit ztrátu dat.</span><span class="sxs-lookup"><span data-stu-id="06398-149">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="06398-150">V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *modely* složky, vyberte **přidat**a pak vyberte **třídy**.</span><span class="sxs-lookup"><span data-stu-id="06398-150">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="06398-151">Název třídy &quot;MovieInitializer&quot;.</span><span class="sxs-lookup"><span data-stu-id="06398-151">Name the class &quot;MovieInitializer&quot;.</span></span> <span data-ttu-id="06398-152">Aktualizace `MovieInitializer` třídy tak, aby obsahovala následující kód:</span><span class="sxs-lookup"><span data-stu-id="06398-152">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

<span data-ttu-id="06398-153">`MovieInitializer` Třída určuje, zda by měla být databáze používá model vyřadit a automaticky znovu vytvořena Pokud nikdy změnit tříd modelu.</span><span class="sxs-lookup"><span data-stu-id="06398-153">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="06398-154">Tento kód obsahuje `Seed` metoda zadat některá data výchozí automaticky přidat až do databáze, když vytvořili (nebo opětovném vytváření).</span><span class="sxs-lookup"><span data-stu-id="06398-154">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="06398-155">To poskytuje vhodný způsob, jak naplnit databázi s ukázkovými daty, aniž by bylo potřeba ručně přidejte do ní pokaždé, když provedete modelu změnit.</span><span class="sxs-lookup"><span data-stu-id="06398-155">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="06398-156">Teď, když jste definovali `MovieInitializer` třídy, je vhodné nastavit tak, aby pokaždé, když je aplikace spuštěná, zkontroluje, jestli se liší od schématu databáze třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="06398-156">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="06398-157">Pokud ano, můžete spustit inicializátor znovu vytvořit databázi podle modelu a naplnit databázi s ukázkovými daty.</span><span class="sxs-lookup"><span data-stu-id="06398-157">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="06398-158">Otevřít *Global.asax* soubor, který je v kořenovém adresáři `MvcMovies` projektu:</span><span class="sxs-lookup"><span data-stu-id="06398-158">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

<span data-ttu-id="06398-159">*Global.asax* soubor obsahuje třídu, která definuje celé aplikace pro projekt a obsahuje `Application_Start` obslužná rutina události, která se spouští při prvním spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="06398-159">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="06398-160">Najít `Application_Start` metoda a přidejte volání do `Database.SetInitializer` na začátku metody, jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="06398-160">Find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

<span data-ttu-id="06398-161">`Database.SetInitializer` Jste právě přidali označuje, že databáze používané `MovieDBContext` instance by měl automaticky odstranit a znovu vytvořen, pokud se schéma a databáze se neshodují.</span><span class="sxs-lookup"><span data-stu-id="06398-161">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="06398-162">A protože jste viděli, bude také naplnit databázi s ukázkovými daty, která je zadána v `MovieInitializer` třídy.</span><span class="sxs-lookup"><span data-stu-id="06398-162">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="06398-163">Zavřít *Global.asax* souboru.</span><span class="sxs-lookup"><span data-stu-id="06398-163">Close the *Global.asax* file.</span></span>

<span data-ttu-id="06398-164">Znovu spusťte aplikaci a přejděte */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="06398-164">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="06398-165">Při spuštění aplikace zjistí, zda jejich struktura model už odpovídá schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="06398-165">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="06398-166">Automaticky znovu vytvoří databázi tak, aby odpovídaly struktuře nový model a naplní databázi s ukázková videa:</span><span class="sxs-lookup"><span data-stu-id="06398-166">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

<span data-ttu-id="06398-168">Klikněte na tlačítko **vytvořit nový** odkaz na přidání nového videa.</span><span class="sxs-lookup"><span data-stu-id="06398-168">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="06398-169">Všimněte si, že můžete přidat hodnocení.</span><span class="sxs-lookup"><span data-stu-id="06398-169">Note that you can add a rating.</span></span>

<span data-ttu-id="06398-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="06398-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span></span>

<span data-ttu-id="06398-171">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="06398-171">Click **Create**.</span></span> <span data-ttu-id="06398-172">Tento nový film, včetně hodnocení, zobrazí se nově ve výpisu:</span><span class="sxs-lookup"><span data-stu-id="06398-172">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

<span data-ttu-id="06398-174">V této části jste viděli, jak můžete upravit objekty modelu a udržovat synchronizované s změny databáze.</span><span class="sxs-lookup"><span data-stu-id="06398-174">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="06398-175">Také jste se naučili způsob, jak naplnit nově vytvořenou databázi s ukázkovými daty, takže si můžete vyzkoušet scénáře.</span><span class="sxs-lookup"><span data-stu-id="06398-175">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="06398-176">V dalším kroku Podívejme se na jak můžete přidat bohatší logiku ověřování na třídy modelu a povolit některé obchodní pravidla, která budou vynucena.</span><span class="sxs-lookup"><span data-stu-id="06398-176">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="06398-177">[Předchozí](examining-the-edit-methods-and-edit-view.md)
> [další](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="06398-177">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
