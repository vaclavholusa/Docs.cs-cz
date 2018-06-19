---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 modely a přístup k datům | Microsoft Docs
author: rick-anderson
description: 'Poznámka: Toto testovací prostředí Hands-on předpokládá, že máte základní znalosti o architektuře ASP.NET MVC. Pokud jste nepoužili ASP.NET MVC před, doporučujeme si projít ASP.NET MVC 4...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 88b3316b116962dd35031f4b971dbfe31ed0e010
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306735"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="e15ae-104">ASP.NET MVC 4 modely a přístup k datům</span><span class="sxs-lookup"><span data-stu-id="e15ae-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="e15ae-105">Podle [webové táborech Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e15ae-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e15ae-106">Stažení webové táborech cvičení Kit</span><span class="sxs-lookup"><span data-stu-id="e15ae-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="e15ae-107">Toto testovací prostředí Hands-on předpokládá, že máte základní znalosti o **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="e15ae-108">Pokud jste nepoužili **ASP.NET MVC** před, doporučujeme si projít **ASP.NET MVC 4 Základy** Hands-on testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e15ae-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="e15ae-109">Tato laboratoř vás provede procesem vylepšení a nových funkcí popsaných výše použitím malých změn na ukázkové webové aplikaci ve zdrojové složce zadané.</span><span class="sxs-lookup"><span data-stu-id="e15ae-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="e15ae-110">Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [verze Microsoft-webové/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="e15ae-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="e15ae-111">Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 modely a přístup k datům](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="e15ae-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="e15ae-112">V **ASP.NET MVC Základy** Hands-on testovací prostředí, můžete mít byla předávání pevně dat z řadičů šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e15ae-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="e15ae-113">Ale, aby bylo možné vytvořit skutečné webové aplikace, můžete chtít použít skutečné databázi.</span><span class="sxs-lookup"><span data-stu-id="e15ae-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="e15ae-114">Toto testovací prostředí Hands-on vám ukáže, jak používat databázový stroj, aby bylo možné ukládají a načítají data potřebná pro aplikaci služby obchodu Hudba.</span><span class="sxs-lookup"><span data-stu-id="e15ae-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="e15ae-115">Provést tuto akci, bude začínat existující databázi a z něj vytvořit datového modelu Entity.</span><span class="sxs-lookup"><span data-stu-id="e15ae-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="e15ae-116">V tomto testovacím prostředí bude vyhovovat **Database First** přístup a taky **Code First** přístup.</span><span class="sxs-lookup"><span data-stu-id="e15ae-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="e15ae-117">Ale můžete také použít **Model First** přístupu, vytvořte stejný model pomocí nástroje a potom z něj generovat databázi.</span><span class="sxs-lookup"><span data-stu-id="e15ae-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="e15ae-118">![První vs databáze. Model první](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Nejprve modelu")</span><span class="sxs-lookup"><span data-stu-id="e15ae-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="e15ae-119">*První vs databáze. Nejprve modelu*</span><span class="sxs-lookup"><span data-stu-id="e15ae-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="e15ae-120">Po generování modelu, budou správné úpravy v StoreController poskytnout zobrazení úložiště dat získaných z databáze, místo použití pevně data.</span><span class="sxs-lookup"><span data-stu-id="e15ae-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="e15ae-121">Nebudete muset provádět všechny změny šablony zobrazení protože StoreController se vrátí stejné ViewModels zobrazení šablony, ale tentokrát data budou pocházet z databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="e15ae-122">**Přístupu Code First**</span><span class="sxs-lookup"><span data-stu-id="e15ae-122">**The Code First Approach**</span></span>

<span data-ttu-id="e15ae-123">Code First přístup umožňuje definovat model z kódu bez generování tříd, které jsou obecně kombinaci s rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e15ae-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="e15ae-124">V kódu nejprve objekty modelu jsou definovány s POCOs, &quot;prostý staré objekty CLR&quot;.</span><span class="sxs-lookup"><span data-stu-id="e15ae-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="e15ae-125">POCOs jsou jednoduché prostý třídy, které mají žádné dědičnosti a neimplementuje rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e15ae-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="e15ae-126">Jsme může automaticky generovat databázi z nich nebo můžeme použít existující databázi a generovat mapování třídy z kódu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="e15ae-127">Výhody použití tohoto přístupu je, aby zůstala modelu nezávisle trvalost framework (v tomto případě Entity Framework), jak POCOs třídy nejsou kombinaci s rozhraní mapování.</span><span class="sxs-lookup"><span data-stu-id="e15ae-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="e15ae-128">Toto testovací prostředí je založena na technologii ASP.NET MVC 4 a verzi Hudba úložiště ukázkovou aplikaci přizpůsobit a minimalizuje podle pouze funkce uvedené v tomto testovacím prostředí Hands-On.</span><span class="sxs-lookup"><span data-stu-id="e15ae-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="e15ae-129">Pokud chcete prozkoumat celek **Hudba úložiště** aplikace najdete ji v [MVC. Hudba úložiště](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="e15ae-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e15ae-130">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e15ae-130">Prerequisites</span></span>

<span data-ttu-id="e15ae-131">Musíte mít následující položky k dokončení tohoto testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="e15ae-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="e15ae-132">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat).</span><span class="sxs-lookup"><span data-stu-id="e15ae-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e15ae-133">Instalace</span><span class="sxs-lookup"><span data-stu-id="e15ae-133">Setup</span></span>

<span data-ttu-id="e15ae-134">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="e15ae-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="e15ae-135">Pro usnadnění práce většinu kódu, který bude spravovat podél toto testovací prostředí je k dispozici jako fragmenty kódu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e15ae-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="e15ae-136">K instalaci fragmenty kódu spustit **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="e15ae-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="e15ae-137">Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="e15ae-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e15ae-138">Cvičení</span><span class="sxs-lookup"><span data-stu-id="e15ae-138">Exercises</span></span>

<span data-ttu-id="e15ae-139">Toto testovací prostředí Hands-on se skládá ve cvičeních následující:</span><span class="sxs-lookup"><span data-stu-id="e15ae-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="e15ae-140">Cvičení 1: Přidání databáze</span><span class="sxs-lookup"><span data-stu-id="e15ae-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="e15ae-141">Cvičení 2: Vytvoření databáze pomocí Code First</span><span class="sxs-lookup"><span data-stu-id="e15ae-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="e15ae-142">Cvičení 3: Dotaz na databázi s parametry</span><span class="sxs-lookup"><span data-stu-id="e15ae-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="e15ae-143">Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních.</span><span class="sxs-lookup"><span data-stu-id="e15ae-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="e15ae-144">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.</span><span class="sxs-lookup"><span data-stu-id="e15ae-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="e15ae-145">Odhadovaný čas dokončení tohoto testovacího prostředí: **35 minut**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="e15ae-146">Cvičení 1: Přidání databáze</span><span class="sxs-lookup"><span data-stu-id="e15ae-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="e15ae-147">V tomto cvičení se dozvíte, jak přidat databáze s tabulkami aplikace MusicStore k řešení, aby bylo možné využívat jeho data.</span><span class="sxs-lookup"><span data-stu-id="e15ae-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="e15ae-148">Jakmile databáze je generována s modelem a k řešení přidat, upraví třídě StoreController zobrazit šablonu poskytnout dat získaných z databáze, místo pevně definovaných hodnot.</span><span class="sxs-lookup"><span data-stu-id="e15ae-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="e15ae-149">Úloha 1 – přidání databáze</span><span class="sxs-lookup"><span data-stu-id="e15ae-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="e15ae-150">V této úloze budete přidávat již vytvořené databáze s hlavní tabulky MusicStore aplikace k řešení.</span><span class="sxs-lookup"><span data-stu-id="e15ae-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="e15ae-151">Otevřete **začít** řešení nacházející se v **zdroj/Ex1-AddingADatabaseDBFirst/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="e15ae-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="e15ae-152">Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e15ae-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e15ae-153">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e15ae-154">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="e15ae-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e15ae-155">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e15ae-156">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e15ae-157">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e15ae-158">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e15ae-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e15ae-159">Přidat **MvcMusicStore** soubor databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="e15ae-160">V tomto testovacím prostředí Hands-on budete používat již vytvořené databáze názvem **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="e15ae-161">To lze provést, klikněte pravým tlačítkem na **aplikace\_Data** složku, přejděte na příkaz **přidat** a pak klikněte na **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="e15ae-162">Přejděte do **\Source\Assets** a vyberte **MvcMusicStore.mdf** souboru.</span><span class="sxs-lookup"><span data-stu-id="e15ae-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="e15ae-163">![Přidat existující položku](aspnet-mvc-4-models-and-data-access/_static/image2.png "přidání existující položky")</span><span class="sxs-lookup"><span data-stu-id="e15ae-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="e15ae-164">*Přidání existující položky*</span><span class="sxs-lookup"><span data-stu-id="e15ae-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="e15ae-165">![Soubor databáze MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf databázový soubor")</span><span class="sxs-lookup"><span data-stu-id="e15ae-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="e15ae-166">*Soubor databáze MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="e15ae-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="e15ae-167">Databáze byla přidána do projektu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-167">The database has been added to the project.</span></span> <span data-ttu-id="e15ae-168">I v případě, že databáze se nachází uvnitř řešení, se můžete dotazovat a aktualizovat ji jako byla hostované v jiné databázi serveru.</span><span class="sxs-lookup"><span data-stu-id="e15ae-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="e15ae-169">![MvcMusicStore databáze v Průzkumníku řešení](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore databáze v Průzkumníku řešení")</span><span class="sxs-lookup"><span data-stu-id="e15ae-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="e15ae-170">*MvcMusicStore databáze v Průzkumníku řešení*</span><span class="sxs-lookup"><span data-stu-id="e15ae-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="e15ae-171">Ověřte připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="e15ae-171">Verify the connection to the database.</span></span> <span data-ttu-id="e15ae-172">Chcete-li to provést, dvakrát klikněte na **MvcMusicStore.mdf** k navázání připojení.</span><span class="sxs-lookup"><span data-stu-id="e15ae-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="e15ae-173">![Připojení k MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "připojení k MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="e15ae-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="e15ae-174">*Připojení k MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="e15ae-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="e15ae-175">Úloha 2 – vytváření datového modelu</span><span class="sxs-lookup"><span data-stu-id="e15ae-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="e15ae-176">V této úloze vytvoří datový model pro interakci s databází přidali v předchozí úloze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="e15ae-177">Vytvoření datového modelu, která bude představovat databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="e15ae-178">Chcete-li to provést, v Průzkumníku řešení klikněte pravým tlačítkem **modely** složku, přejděte na příkaz **přidat** a pak klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="e15ae-179">V **přidat novou položku** dialogovém okně, vyberte **Data** šablony a potom **ADO.NET Entity Data Model** položky.</span><span class="sxs-lookup"><span data-stu-id="e15ae-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="e15ae-180">Změňte název modelu dat **StoreDB.edmx** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="e15ae-181">![Přidání StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "přidání StoreDB ADO.NET Entity Data Model")</span><span class="sxs-lookup"><span data-stu-id="e15ae-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="e15ae-182">*Přidání StoreDB ADO.NET Entity Data Model*</span><span class="sxs-lookup"><span data-stu-id="e15ae-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="e15ae-183">**Entity Data Model Wizard** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="e15ae-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="e15ae-184">Tento průvodce vás provede vytvoření vrstvy modelu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="e15ae-185">Vzhledem k tomu, že model by měl být na základě vytvořit existující databáze recentyl přidat, vyberte **generování z databáze** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="e15ae-186">![Výběr obsah modelu](aspnet-mvc-4-models-and-data-access/_static/image7.png "výběr obsah modelu")</span><span class="sxs-lookup"><span data-stu-id="e15ae-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="e15ae-187">*Výběr obsah modelu*</span><span class="sxs-lookup"><span data-stu-id="e15ae-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="e15ae-188">Vzhledem k tomu, že chcete generovat model z databáze, musíte zadat připojení používat.</span><span class="sxs-lookup"><span data-stu-id="e15ae-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="e15ae-189">Klikněte na tlačítko **nové připojení**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="e15ae-190">Vyberte **soubor databáze Microsoft SQL Server** a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="e15ae-191">![Vyberte zdroj dat](aspnet-mvc-4-models-and-data-access/_static/image8.png "vybrat zdroj dat")</span><span class="sxs-lookup"><span data-stu-id="e15ae-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="e15ae-192">*Vyberte zdroj dat dialogové okno*</span><span class="sxs-lookup"><span data-stu-id="e15ae-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="e15ae-193">Klikněte na tlačítko **Procházet** a vyberte databázi **MvcMusicStore.mdf** jste vyhledali v **aplikace\_Data** složky a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="e15ae-194">![Vlastnosti připojení](aspnet-mvc-4-models-and-data-access/_static/image9.png "vlastnosti připojení")</span><span class="sxs-lookup"><span data-stu-id="e15ae-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="e15ae-195">*Vlastnosti připojení*</span><span class="sxs-lookup"><span data-stu-id="e15ae-195">*Connection properties*</span></span>
6. <span data-ttu-id="e15ae-196">Generovaná třída by měla mít stejný název jako připojovací řetězec entity, takže změňte její název **MusicStoreEntities** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="e15ae-197">![Výběr datové připojení](aspnet-mvc-4-models-and-data-access/_static/image10.png "vybrat datové připojení")</span><span class="sxs-lookup"><span data-stu-id="e15ae-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="e15ae-198">*Výběr datové připojení*</span><span class="sxs-lookup"><span data-stu-id="e15ae-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="e15ae-199">Vyberte objekty databáze, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="e15ae-199">Choose the database objects to use.</span></span> <span data-ttu-id="e15ae-200">Entity Model používat jenom databázové tabulky, vyberte **tabulky** možnost a ujistěte se, že **zahrnout do modelu sloupce cizích klíčů** a **množně nebo singularizovat generované názvy objektů** jsou vybrané možnosti.</span><span class="sxs-lookup"><span data-stu-id="e15ae-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="e15ae-201">Změňte Model Namespace k **MvcMusicStore.Model** a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="e15ae-202">![Výběr databázové objekty](aspnet-mvc-4-models-and-data-access/_static/image11.png "výběr databázové objekty")</span><span class="sxs-lookup"><span data-stu-id="e15ae-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="e15ae-203">*Výběr databázové objekty*</span><span class="sxs-lookup"><span data-stu-id="e15ae-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e15ae-204">Pokud se zobrazí dialogové okno upozornění zabezpečení, klikněte na tlačítko **OK** spustit šablonu a vygenerovat třídy pro model entity.</span><span class="sxs-lookup"><span data-stu-id="e15ae-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="e15ae-205">Diagramu entity pro databázi se zobrazí, když se vytvoří samostatnou třídu, která mapuje každou tabulku do databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="e15ae-206">Například **alb** budou odpovídat tabulky **Album** třídy, kde bude každý sloupec v tabulce mapovat vlastnosti třídy.</span><span class="sxs-lookup"><span data-stu-id="e15ae-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="e15ae-207">To vám umožní pro dotazování a práce s objekty, které představují řádků v databázi.</span><span class="sxs-lookup"><span data-stu-id="e15ae-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="e15ae-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagramu Entity")</span><span class="sxs-lookup"><span data-stu-id="e15ae-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="e15ae-209">*Entity diagram*</span><span class="sxs-lookup"><span data-stu-id="e15ae-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e15ae-210">Šablony T4 (.tt) spustit kód vygenerovat třídy entity a přepíše existující třídy se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="e15ae-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="e15ae-211">V tomto příkladu třídy &quot;Album&quot;, &quot;Genre&quot; a &quot;umělcem&quot; měla přepsat generovaného kódu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="e15ae-212">Úloha 3 – vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="e15ae-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="e15ae-213">V této úloze bude zaškrtnete, i když generování modelu odebraly **Album**, **Genre** a **umělcem** třídy modelu, sestavení projektu úspěšně pomocí nové třídy datového modelu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="e15ae-214">Sestavení projektu výběrem **sestavení** položky nabídky a pak **sestavení MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="e15ae-215">![Sestavení projektu](aspnet-mvc-4-models-and-data-access/_static/image13.png "sestavení projektu")</span><span class="sxs-lookup"><span data-stu-id="e15ae-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="e15ae-216">*Sestavení projektu*</span><span class="sxs-lookup"><span data-stu-id="e15ae-216">*Building the project*</span></span>
2. <span data-ttu-id="e15ae-217">Sestavení projektu úspěšně.</span><span class="sxs-lookup"><span data-stu-id="e15ae-217">The project builds successfully.</span></span> <span data-ttu-id="e15ae-218">Proč to stále funguje?</span><span class="sxs-lookup"><span data-stu-id="e15ae-218">Why does it still work?</span></span> <span data-ttu-id="e15ae-219">Funguje, protože tabulky databáze, které mají pole obsahující vlastnosti, které jste používali v odstraněné třídy **Album** a **Genre**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="e15ae-220">![Sestavení bylo úspěšné](aspnet-mvc-4-models-and-data-access/_static/image14.png "sestavení bylo úspěšné")</span><span class="sxs-lookup"><span data-stu-id="e15ae-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="e15ae-221">*Sestavení bylo úspěšné*</span><span class="sxs-lookup"><span data-stu-id="e15ae-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="e15ae-222">Při návrháře zobrazí entity ve formátu diagramu, jsou skutečně třídy jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="e15ae-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="e15ae-223">Rozbalte **StoreDB.edmx** uzlu v Průzkumníku řešení a potom **StoreDB.tt**, zobrazí se nové generovaného entity.</span><span class="sxs-lookup"><span data-stu-id="e15ae-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="e15ae-224">![Generované soubory](aspnet-mvc-4-models-and-data-access/_static/image15.png "generované soubory")</span><span class="sxs-lookup"><span data-stu-id="e15ae-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="e15ae-225">*Generované soubory*</span><span class="sxs-lookup"><span data-stu-id="e15ae-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="e15ae-226">Úloha 4 – dotazování databáze</span><span class="sxs-lookup"><span data-stu-id="e15ae-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="e15ae-227">V této úloze tak, aby místo použití pevně zakódované data, se bude dotazovat databáze načíst informace o aktualizujte StoreController třídy.</span><span class="sxs-lookup"><span data-stu-id="e15ae-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="e15ae-228">Otevřete **Controllers\StoreController.cs** a přidejte následující pole do třídy pro uložení instance **MusicStoreEntities** třídy s názvem **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="e15ae-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="e15ae-229">(Code fragment kódu - *modely a přístup k datům - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="e15ae-230">**MusicStoreEntities** třída zpřístupňuje vlastnost kolekce pro každou tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="e15ae-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="e15ae-231">Aktualizace **Procházet** metody akce k načtení Genre se všemi **alb**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="e15ae-232">(Code fragment kódu - *modely a přístup k datům - Ex1 úložiště Procházet*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="e15ae-233">Používáte funkce .NET názvem **LINQ** (language-integrated query) pro zápis výrazy silného typu dotazů vůči tyto kolekce – které bude spouštění kódu v databázi a vrátit objekty, které můžete naprogramovat proti.</span><span class="sxs-lookup"><span data-stu-id="e15ae-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="e15ae-234">Další informace o LINQ, naleznete [webu msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="e15ae-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="e15ae-235">Aktualizace **Index** metody akce k načtení všech žánry.</span><span class="sxs-lookup"><span data-stu-id="e15ae-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="e15ae-236">(Code fragment kódu - *modely a Data Access – Index úložiště Ex1*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="e15ae-237">Aktualizace **Index** metody akce k načtení všech žánry a transformace kolekce do seznamu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="e15ae-238">(Code fragment kódu - *modely a přístup k datům - Ex1 úložiště GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="e15ae-239">Úloha 5: spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e15ae-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="e15ae-240">V této úloze zkontroluje se, že na stránce Index úložiště se nyní zobrazí žánry uloženy v databázi místo pevně zakódované ty, které jsou.</span><span class="sxs-lookup"><span data-stu-id="e15ae-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="e15ae-241">Není třeba měnit zobrazit šablonu, protože **StoreController** vrací stejné entity jako předtím, ale tentokrát data budou pocházet z databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="e15ae-242">Znovu sestavte řešení a stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="e15ae-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e15ae-243">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="e15ae-243">The project starts in the Home page.</span></span> <span data-ttu-id="e15ae-244">Ověřte, že v nabídce **žánry** již není seznamu pevně zakódované a přímo načtena data z databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="e15ae-246">*Procházení žánry z databáze*</span><span class="sxs-lookup"><span data-stu-id="e15ae-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="e15ae-247">Nyní přejděte do jakékoli genre a ověřte, zda že alb budou naplněny z databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="e15ae-248">![Procházení alb z databáze](aspnet-mvc-4-models-and-data-access/_static/image17.png "procházení alb z databáze")</span><span class="sxs-lookup"><span data-stu-id="e15ae-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="e15ae-249">*Procházení alb z databáze*</span><span class="sxs-lookup"><span data-stu-id="e15ae-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="e15ae-250">Cvičení 2: Vytvoření databáze nejdřív pomocí kódu</span><span class="sxs-lookup"><span data-stu-id="e15ae-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="e15ae-251">V tomto cvičení se dozvíte, jak použít Code First přístup k vytvoření databáze s tabulkami aplikace MusicStore a jak pro přístup k jeho data.</span><span class="sxs-lookup"><span data-stu-id="e15ae-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="e15ae-252">Po vygenerování modelu upravíte StoreController poskytnout dat získaných z databáze, místo použití hodnot pevně zakódované zobrazit šablonu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="e15ae-253">Pokud jste dokončili cvičení 1 a už pracovali s databáze prvním přístupem, se teď Další informace o získání stejné výsledky s jiným procesem.</span><span class="sxs-lookup"><span data-stu-id="e15ae-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="e15ae-254">Úlohy, které jsou společné s cvičení 1 bylo označeno k usnadnění vaší čtení.</span><span class="sxs-lookup"><span data-stu-id="e15ae-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="e15ae-255">Pokud jste nedokončili prověření 1, ale chtěli dozvědět Code First přístup, můžete spustit z tohoto cvičení a získat úplné vysvětlení tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="e15ae-256">Úloha 1 - naplnění ukázková Data</span><span class="sxs-lookup"><span data-stu-id="e15ae-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="e15ae-257">V této úloze bude naplnit databázi s ukázkovými daty při výchozímu vytvoření pomocí Code First.</span><span class="sxs-lookup"><span data-stu-id="e15ae-257">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="e15ae-258">Otevřete **začít** řešení nacházející se v **zdroj/Ex2-CreatingADatabaseCodeFirst/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="e15ae-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="e15ae-259">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="e15ae-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="e15ae-260">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e15ae-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e15ae-261">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e15ae-262">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="e15ae-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e15ae-263">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e15ae-264">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e15ae-265">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e15ae-266">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e15ae-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e15ae-267">Přidat **SampleData.cs** do souboru **modely** složky.</span><span class="sxs-lookup"><span data-stu-id="e15ae-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="e15ae-268">To lze provést, klikněte pravým tlačítkem na **modely** složku, přejděte na příkaz **přidat** a pak klikněte na **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="e15ae-269">Přejděte do **\Source\Assets** a vyberte **SampleData.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="e15ae-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="e15ae-270">![Ukázková data naplnit kódu](aspnet-mvc-4-models-and-data-access/_static/image18.png "ukázkových dat naplnit kódu")</span><span class="sxs-lookup"><span data-stu-id="e15ae-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="e15ae-271">*Ukázková data naplnit kódu*</span><span class="sxs-lookup"><span data-stu-id="e15ae-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="e15ae-272">Otevřete **Global.asax.cs** souboru a přidejte následující *pomocí* příkazy.</span><span class="sxs-lookup"><span data-stu-id="e15ae-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="e15ae-273">(Code fragment kódu - *modely a přístup k datům - Ex2 globální direktiv Using Asax*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="e15ae-274">V **aplikace\_Start()** metoda přidejte následující řádek k nastavení inicializátoru databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="e15ae-275">(Code fragment kódu - *modely a přístup k datům - Ex2 globální Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="e15ae-276">Úloha 2 – konfigurace připojení k databázi</span><span class="sxs-lookup"><span data-stu-id="e15ae-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="e15ae-277">Teď, když databáze jste už přidali do našich projektu, budete psát **Web.config** souboru připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="e15ae-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="e15ae-278">Přidat připojovací řetězec v **Web.config**. Uděláte to tak, že otevřete **Web.config** v kořenu projektu a nahraďte připojovací řetězec s názvem objekt DefaultConnection se tento řádek ve **&lt;connectionStrings&gt;** části:</span><span class="sxs-lookup"><span data-stu-id="e15ae-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="e15ae-279">![Umístění souboru Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "umístění souboru Web.config")</span><span class="sxs-lookup"><span data-stu-id="e15ae-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="e15ae-280">*umístění souboru Web.config*</span><span class="sxs-lookup"><span data-stu-id="e15ae-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="e15ae-281">Úloha 3 – práci s modelem</span><span class="sxs-lookup"><span data-stu-id="e15ae-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="e15ae-282">Teď, když už jste nakonfigurovali připojení k databázi, propojíte model s databázových tabulek.</span><span class="sxs-lookup"><span data-stu-id="e15ae-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="e15ae-283">V této úloze vytvoříte třídu, která propojí do databáze s Code First.</span><span class="sxs-lookup"><span data-stu-id="e15ae-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="e15ae-284">Mějte na paměti, že je existující třídy modelu objektů POCO, které by měl být upraven.</span><span class="sxs-lookup"><span data-stu-id="e15ae-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="e15ae-285">Pokud jste dokončili cvičení 1, Všimněte si, že se tento krok provést pomocí průvodce.</span><span class="sxs-lookup"><span data-stu-id="e15ae-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="e15ae-286">Pomocí tohoto postupu Code First, ručně vytvoříte třídy, které budou propojené s dat entity.</span><span class="sxs-lookup"><span data-stu-id="e15ae-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="e15ae-287">Otevřete třídu modelu objektů POCO **Genre** z **modely** projektu složky a obsahovat identifikátor.</span><span class="sxs-lookup"><span data-stu-id="e15ae-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="e15ae-288">Použít interní vlastnost s názvem **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="e15ae-289">(Code fragment kódu - *modely a přístup k datům - Ex2 kód první Genre*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="e15ae-290">Pro práci s Code First názvů, třídy Genre musí mít vlastnost primárního klíče, který bude automaticky zjistit.</span><span class="sxs-lookup"><span data-stu-id="e15ae-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="e15ae-291">Další informace o první pravidla týkající se kódu v tomto [článku na webu msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="e15ae-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="e15ae-292">Nyní otevřete třídu modelu objektů POCO **Album** z **modely** projektu složky a zahrnují cizí klíče, vytvoření vlastností s názvy **GenreId** a  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="e15ae-293">Tato třída už máte **GenreId** pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="e15ae-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="e15ae-294">(Code fragment kódu - *modely a přístup k datům - Ex2 kód prvního alba*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="e15ae-295">Otevřete třídu modelu objektů POCO **umělcem** a zahrnout **ArtistId** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e15ae-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="e15ae-296">(Code fragment kódu - *modely a přístup k datům - Ex2 kód první umělcem*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="e15ae-297">Klikněte pravým tlačítkem myši **modely** složky projektu a vyberte **přidat | Třída**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="e15ae-298">Název souboru **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="e15ae-299">Potom klikněte na **přidat.**</span><span class="sxs-lookup"><span data-stu-id="e15ae-299">Then, click **Add.**</span></span>

    <span data-ttu-id="e15ae-300">![Přidání třídy](aspnet-mvc-4-models-and-data-access/_static/image20.png "přidání třídy")</span><span class="sxs-lookup"><span data-stu-id="e15ae-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="e15ae-301">*Přidání nové položky*</span><span class="sxs-lookup"><span data-stu-id="e15ae-301">*Adding a new item*</span></span>

    <span data-ttu-id="e15ae-302">![Přidání class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "přidávání class2")</span><span class="sxs-lookup"><span data-stu-id="e15ae-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="e15ae-303">*Přidání třídy*</span><span class="sxs-lookup"><span data-stu-id="e15ae-303">*Adding a class*</span></span>
5. <span data-ttu-id="e15ae-304">Třída jste právě vytvořili, otevřete **MusicStoreEntities.cs**a přidají obory názvů **System.Data.Entity** a **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="e15ae-305">Nahraďte deklaraci třídy rozšířit **DbContext** – třída: deklarovat veřejné **DBSet** a přepsání **OnModelCreating** metoda.</span><span class="sxs-lookup"><span data-stu-id="e15ae-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="e15ae-306">Po provedení tohoto kroku budete mít domény třídu, která odkaz modelu pomocí rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e15ae-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="e15ae-307">Aby bylo možné provést, nahraďte kód třídy následující:</span><span class="sxs-lookup"><span data-stu-id="e15ae-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="e15ae-308">(Code fragment kódu - *modely a přístup k datům - Ex2 kód první MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="e15ae-309">S platformou Entity Framework **DbContext** a **DBSet** bude moci dotazovat třídu objektů POCO Genre.</span><span class="sxs-lookup"><span data-stu-id="e15ae-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="e15ae-310">Tím, že rozšíří **OnModelCreating** metoda, určíte v **kód** mapovány Genre do databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="e15ae-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="e15ae-311">Další informace o DBContext a DBSet najdete v tomto článku msdn: [odkaz](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="e15ae-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="e15ae-312">Úloha 4 – dotazování databáze</span><span class="sxs-lookup"><span data-stu-id="e15ae-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="e15ae-313">V této úloze bude aktualizace třídy pro StoreController tak, aby místo použití pevně zakódované dat, načte se z databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="e15ae-314">Tato úloha je společné s cvičení 1.</span><span class="sxs-lookup"><span data-stu-id="e15ae-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="e15ae-315">Pokud jste dokončili cvičení 1 Všimněte si tyto kroky jsou stejné v obou přístupů (první databáze nebo nejprve Code).</span><span class="sxs-lookup"><span data-stu-id="e15ae-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="e15ae-316">Liší se v tom, jak data jsou spojena s modelem, ale přístup k data entity je ještě transparentní z řadiče.</span><span class="sxs-lookup"><span data-stu-id="e15ae-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="e15ae-317">Otevřete **Controllers\StoreController.cs** a přidejte následující pole do třídy pro uložení instance **MusicStoreEntities** třídy s názvem **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="e15ae-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="e15ae-318">(Code fragment kódu - *modely a přístup k datům - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="e15ae-319">**MusicStoreEntities** třída zpřístupňuje vlastnost kolekce pro každou tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="e15ae-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="e15ae-320">Aktualizace **Procházet** metody akce k načtení Genre se všemi **alb**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="e15ae-321">(Code fragment kódu - *modely a přístup k datům - procházet úložiště Ex2*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="e15ae-322">Používáte funkce .NET názvem **LINQ** (language-integrated query) pro zápis výrazy silného typu dotazů vůči tyto kolekce – které bude spouštění kódu v databázi a vrátit objekty, které můžete naprogramovat proti.</span><span class="sxs-lookup"><span data-stu-id="e15ae-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="e15ae-323">Další informace o LINQ, naleznete [webu msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="e15ae-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="e15ae-324">Aktualizace **Index** metody akce k načtení všech žánry.</span><span class="sxs-lookup"><span data-stu-id="e15ae-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="e15ae-325">(Code fragment kódu - *modely a Data Access – Index úložiště Ex2*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="e15ae-326">Aktualizace **Index** metody akce k načtení všech žánry a transformace kolekce do seznamu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="e15ae-327">(Code fragment kódu - *modely a přístup k datům - Ex2 úložiště GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="e15ae-328">Úloha 5: spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e15ae-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="e15ae-329">V této úloze zkontroluje se, že na stránce Index úložiště se nyní zobrazí žánry uloženy v databázi místo pevně zakódované ty, které jsou.</span><span class="sxs-lookup"><span data-stu-id="e15ae-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="e15ae-330">Není třeba měnit zobrazit šablonu, protože **StoreController** vrací stejné **StoreIndexViewModel** jako předtím, ale tentokrát budou pocházet data z databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="e15ae-331">Znovu sestavte řešení a stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="e15ae-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e15ae-332">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="e15ae-332">The project starts in the Home page.</span></span> <span data-ttu-id="e15ae-333">Ověřte, že v nabídce **žánry** již není seznamu pevně zakódované a přímo načtena data z databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="e15ae-335">*Procházení žánry z databáze*</span><span class="sxs-lookup"><span data-stu-id="e15ae-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="e15ae-336">Nyní přejděte do jakékoli genre a ověřte, zda že alb budou naplněny z databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="e15ae-337">![Procházení alb z databáze](aspnet-mvc-4-models-and-data-access/_static/image23.png "procházení alb z databáze")</span><span class="sxs-lookup"><span data-stu-id="e15ae-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="e15ae-338">*Procházení alb z databáze*</span><span class="sxs-lookup"><span data-stu-id="e15ae-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="e15ae-339">Cvičení 3: Dotaz na databázi s parametry</span><span class="sxs-lookup"><span data-stu-id="e15ae-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="e15ae-340">V tomto cvičení se dozvíte, jak k dotazování databáze pomocí parametrů a jak používat službu Shaping výsledků dotazu, funkce, která snižuje počet databáze přistupuje k načítání dat v efektivnější.</span><span class="sxs-lookup"><span data-stu-id="e15ae-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="e15ae-341">Další informace o Shaping výsledek dotazu naleznete na následujícím [článku na webu msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="e15ae-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="e15ae-342">Úloha 1 – změny StoreController alb načíst z databáze</span><span class="sxs-lookup"><span data-stu-id="e15ae-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="e15ae-343">V této úloze se změní **StoreController** třídy pro přístup k databázi načíst alb z konkrétní genre.</span><span class="sxs-lookup"><span data-stu-id="e15ae-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="e15ae-344">Otevřete **začít** řešení nacházející se v **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** složky, pokud chcete použít první kód nebo **Source\ EX3. QueryingTheDatabaseWithParametersDBFirst\Begin** složky, pokud chcete použít první databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="e15ae-345">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="e15ae-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="e15ae-346">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e15ae-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e15ae-347">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e15ae-348">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="e15ae-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e15ae-349">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e15ae-350">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e15ae-351">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e15ae-352">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e15ae-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e15ae-353">Otevřete **StoreController** třída změnit **Procházet** metody akce.</span><span class="sxs-lookup"><span data-stu-id="e15ae-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="e15ae-354">Chcete-li to provést, v **Průzkumníku řešení**, rozbalte **řadiče** složku a dvojím kliknutím **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="e15ae-355">Změna **Procházet** metody akce k načtení alb pro konkrétní genre.</span><span class="sxs-lookup"><span data-stu-id="e15ae-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="e15ae-356">Chcete-li to provést, nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e15ae-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="e15ae-357">(Code fragment kódu - *modely a přístup k datům - EX3. StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="e15ae-358">K naplnění kolekce entit, budete muset použít **zahrnout** metoda k určení, můžete obnovit alb příliš.</span><span class="sxs-lookup"><span data-stu-id="e15ae-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="e15ae-359">Můžete použít. **Single()** rozšíření v technologii LINQ protože v takovém případě je očekávána pouze jedna genre pro album.</span><span class="sxs-lookup"><span data-stu-id="e15ae-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="e15ae-360">**Single()** metoda přebírá jako parametr, který v tomto případě Určuje jeden objekt Genre tak, aby jeho název odpovídá definovanou hodnotu výrazu Lambda.</span><span class="sxs-lookup"><span data-stu-id="e15ae-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="e15ae-361">Bude využít výhod funkce, která umožňuje určit další, které chcete také načíst, když je načíst objekt Genre entit v relaci.</span><span class="sxs-lookup"><span data-stu-id="e15ae-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="e15ae-362">Tato funkce je volána **Shaping výsledek dotazu**a umožňuje snížit počet pokusů, které jsou potřebné pro přístup k databázi k načtení informací.</span><span class="sxs-lookup"><span data-stu-id="e15ae-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="e15ae-363">V tomto scénáři můžete předem načíst alba pro Genre je načíst.</span><span class="sxs-lookup"><span data-stu-id="e15ae-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="e15ae-364">Dotaz obsahuje **Genres.Include (&quot;alb&quot;)** k označení, že chcete také související alb.</span><span class="sxs-lookup"><span data-stu-id="e15ae-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="e15ae-365">Výsledkem bude efektivnější aplikaci, vzhledem k tomu, že ho načte Genre a Album data v požadavku jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="e15ae-366">Úloha 2 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e15ae-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="e15ae-367">V této úloze se spustit aplikaci a načtení alb konkrétní genre z databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="e15ae-368">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="e15ae-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e15ae-369">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="e15ae-369">The project starts in the Home page.</span></span> <span data-ttu-id="e15ae-370">Změňte adresu URL na **/úložiště/procházet? genre = Pop** k ověření, že výsledky jsou načítány z databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="e15ae-371">![Procházení podle genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "procházení podle genre")</span><span class="sxs-lookup"><span data-stu-id="e15ae-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="e15ae-372">*Procházení/úložiště/procházet? genre = Pop*</span><span class="sxs-lookup"><span data-stu-id="e15ae-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="e15ae-373">Úloha 3 – přístup k alb podle Id</span><span class="sxs-lookup"><span data-stu-id="e15ae-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="e15ae-374">V této úloze bude opakujte předchozí postup k získání alb podle jejich Id.</span><span class="sxs-lookup"><span data-stu-id="e15ae-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="e15ae-375">Zavřete prohlížeč v případě potřeby se vraťte do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e15ae-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="e15ae-376">Otevřete **StoreController** třída změnit **podrobnosti** metody akce.</span><span class="sxs-lookup"><span data-stu-id="e15ae-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="e15ae-377">Chcete-li to provést, v **Průzkumníku řešení**, rozbalte **řadiče** složku a dvojím kliknutím **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="e15ae-378">Změna **podrobnosti** metoda akce se načíst podrobnosti o alb na základě jejich **Id**. Chcete-li to provést, nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e15ae-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="e15ae-379">(Code fragment kódu - *modely a přístup k datům - EX3. StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="e15ae-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="e15ae-380">Úloha 4 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e15ae-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="e15ae-381">V této úloze se spustit aplikaci ve webovém prohlížeči a získat podrobnosti o album podle jejich Id.</span><span class="sxs-lookup"><span data-stu-id="e15ae-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="e15ae-382">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="e15ae-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e15ae-383">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="e15ae-383">The project starts in the Home page.</span></span> <span data-ttu-id="e15ae-384">Změňte adresu URL na **/Store/Details/51** nebo žánry Procházet a vyberte album k ověření, že výsledky jsou načítány z databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="e15ae-385">![Procházení podrobností](aspnet-mvc-4-models-and-data-access/_static/image25.png "procházení podrobností")</span><span class="sxs-lookup"><span data-stu-id="e15ae-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="e15ae-386">*Procházení /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="e15ae-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="e15ae-387">Kromě toho můžete nasadit tuto aplikaci do následující weby systému Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="e15ae-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e15ae-388">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e15ae-388">Summary</span></span>

<span data-ttu-id="e15ae-389">Pomocí dokončení tohoto testovacího prostředí Hands-on jste se naučili základy modely ASP.NET MVC a přístup k datům, pomocí **Database First** přístup a taky **Code First** přístup:</span><span class="sxs-lookup"><span data-stu-id="e15ae-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="e15ae-390">Jak přidat databáze do řešení, aby bylo možné využívat svá data</span><span class="sxs-lookup"><span data-stu-id="e15ae-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="e15ae-391">Postup aktualizace řadičů poskytnout šablon zobrazení dat získaných z databáze místo pevně jeden</span><span class="sxs-lookup"><span data-stu-id="e15ae-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="e15ae-392">Postup dotazování databáze pomocí parametrů</span><span class="sxs-lookup"><span data-stu-id="e15ae-392">How to query the database using parameters</span></span>
- <span data-ttu-id="e15ae-393">Jak používat dotazu výsledek tvarování, funkce, která snižuje počet přístupům do databáze, načítání dat v efektivnější</span><span class="sxs-lookup"><span data-stu-id="e15ae-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="e15ae-394">Jak používat první databáze a Code First přístupy v Microsoft Entity Framework propojení databáze s modelem</span><span class="sxs-lookup"><span data-stu-id="e15ae-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="e15ae-395">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="e15ae-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="e15ae-396">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="e15ae-397">Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="e15ae-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="e15ae-398">Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="e15ae-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="e15ae-399">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="e15ae-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="e15ae-400">Klikněte na **nyní nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-400">Click on **Install Now**.</span></span> <span data-ttu-id="e15ae-401">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="e15ae-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="e15ae-402">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="e15ae-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="e15ae-403">![Nainstalovat Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "nainstalovat Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="e15ae-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="e15ae-404">*Nainstalovat Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="e15ae-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="e15ae-405">Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="e15ae-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Vyjádření souhlasu s podmínkami licence](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="e15ae-407">*Vyjádření souhlasu s podmínkami licence*</span><span class="sxs-lookup"><span data-stu-id="e15ae-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="e15ae-408">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="e15ae-408">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="e15ae-410">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="e15ae-410">*Installation progress*</span></span>
6. <span data-ttu-id="e15ae-411">Po dokončení instalace, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-411">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="e15ae-413">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="e15ae-413">*Installation completed*</span></span>
7. <span data-ttu-id="e15ae-414">Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.</span><span class="sxs-lookup"><span data-stu-id="e15ae-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="e15ae-415">Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="e15ae-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pro Web dlaždice](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="e15ae-417">*VS Express pro Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="e15ae-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="e15ae-418">Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu</span><span class="sxs-lookup"><span data-stu-id="e15ae-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="e15ae-419">Tento dodatek vám ukáže, jak vytvořit nový web z portálu Windows Azure Management Portal a publikovat aplikace, kterou jste získali podle testovacím prostředí, využívat výhod nasazení webu publikování funkce poskytované služby Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="e15ae-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="e15ae-420">Úloha 1 – Vytvoření nového webu ze systému Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e15ae-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="e15ae-421">Přejděte na [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů společnosti Microsoft, které jsou spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="e15ae-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e15ae-422">S Windows Azure můžete bezplatné hostování 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="e15ae-423">Můžete si zaregistrovat [zde](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="e15ae-423">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="e15ae-424">![Přihlaste se k portálu Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Přihlaste se k portálu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="e15ae-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="e15ae-425">*Přihlaste se k Windows Azure Management Portal*</span><span class="sxs-lookup"><span data-stu-id="e15ae-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="e15ae-426">Klikněte na tlačítko **nový** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="e15ae-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="e15ae-427">![Vytvoření nového webu](aspnet-mvc-4-models-and-data-access/_static/image32.png "vytváření nového webu")</span><span class="sxs-lookup"><span data-stu-id="e15ae-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="e15ae-428">*Vytvoření nového webu*</span><span class="sxs-lookup"><span data-stu-id="e15ae-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="e15ae-429">Klikněte na tlačítko **výpočetní** | **webu**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="e15ae-430">Potom vyberte **rychle vytvořit** možnost.</span><span class="sxs-lookup"><span data-stu-id="e15ae-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="e15ae-431">Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvoření webu**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e15ae-432">Web systému Windows Azure je hostitel pro spouštění v cloudu, ve kterém můžete řídit a spravovat webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e15ae-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="e15ae-433">Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál.</span><span class="sxs-lookup"><span data-stu-id="e15ae-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="e15ae-434">Postup pro nastavení databáze neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="e15ae-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="e15ae-435">![Vytvoření nového webu pomocí metody rychlého vytvoření](aspnet-mvc-4-models-and-data-access/_static/image33.png "vytváření nového webu pomocí metody rychlého vytvoření")</span><span class="sxs-lookup"><span data-stu-id="e15ae-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="e15ae-436">*Vytvoření nového webu pomocí metody rychlého vytvoření*</span><span class="sxs-lookup"><span data-stu-id="e15ae-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="e15ae-437">Počkejte, dokud nové **webu** je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="e15ae-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="e15ae-438">Po vytvoření webu klikněte na odkaz v části **URL** sloupce.</span><span class="sxs-lookup"><span data-stu-id="e15ae-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="e15ae-439">Zkontrolujte, zda je funkční nový web.</span><span class="sxs-lookup"><span data-stu-id="e15ae-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="e15ae-440">![Procházení na nový web](aspnet-mvc-4-models-and-data-access/_static/image34.png "procházení na nový web")</span><span class="sxs-lookup"><span data-stu-id="e15ae-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="e15ae-441">*Procházení na nový web*</span><span class="sxs-lookup"><span data-stu-id="e15ae-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="e15ae-442">![Webový server spuštěn](aspnet-mvc-4-models-and-data-access/_static/image35.png "webu systémem")</span><span class="sxs-lookup"><span data-stu-id="e15ae-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="e15ae-443">*Spuštění webu*</span><span class="sxs-lookup"><span data-stu-id="e15ae-443">*Web site running*</span></span>
6. <span data-ttu-id="e15ae-444">Přejděte zpět na portál a klikněte na název webu v části **název** sloupec pro zobrazení stránky pro správu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="e15ae-445">![Otevření stránky Správa webu](aspnet-mvc-4-models-and-data-access/_static/image36.png "otevření stránek správu webového serveru")</span><span class="sxs-lookup"><span data-stu-id="e15ae-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="e15ae-446">*Otevření stránek správu webového serveru*</span><span class="sxs-lookup"><span data-stu-id="e15ae-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="e15ae-447">V **řídicí panel** v části **rychlého přehledu** klikněte na položku **stažení profilu publikování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="e15ae-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e15ae-448">*Profilu publikování* obsahuje všechny informace požadované pro publikování webové aplikace na web služby Windows Azure pro každou metodu povoleno publikace.</span><span class="sxs-lookup"><span data-stu-id="e15ae-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="e15ae-449">Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a řetězců databází, které jsou potřebné k připojení k a ověřování na základě těchto koncových bodů, pro které je metoda publikace povolena.</span><span class="sxs-lookup"><span data-stu-id="e15ae-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="e15ae-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pro Web** a **sadu Microsoft Visual Studio 2012** podporu čtení publikační profily k automatické konfiguraci těchto programů pro publikování webové aplikace na weby služby Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="e15ae-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="e15ae-451">![Na webu stažení profilu publikování](aspnet-mvc-4-models-and-data-access/_static/image37.png "stahování webové stránky profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="e15ae-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="e15ae-452">*Na webu stažení profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="e15ae-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="e15ae-453">Stáhněte si soubor profil publikování do vhodného umístění.</span><span class="sxs-lookup"><span data-stu-id="e15ae-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="e15ae-454">Dále v tomto cvičení uvidíte jak používat tento soubor k publikování webové aplikace na weby systému Windows Azure ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e15ae-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="e15ae-455">![Ukládání souboru profilu publikování](aspnet-mvc-4-models-and-data-access/_static/image38.png "ukládání profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="e15ae-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="e15ae-456">*Ukládání souboru profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="e15ae-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="e15ae-457">Úloha 2 – konfigurování serveru databáze</span><span class="sxs-lookup"><span data-stu-id="e15ae-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="e15ae-458">Pokud vaše aplikace využívá systému SQL Server, databáze, budete muset vytvořit databázi SQL server.</span><span class="sxs-lookup"><span data-stu-id="e15ae-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="e15ae-459">Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá systém SQL Server může tuto úlohu přeskočit.</span><span class="sxs-lookup"><span data-stu-id="e15ae-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="e15ae-460">Budete potřebovat databázi SQL serveru pro ukládání databázi aplikace.</span><span class="sxs-lookup"><span data-stu-id="e15ae-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="e15ae-461">Databáze SQL servery můžete zobrazit ze svého předplatného na portál Windows Azure Management **databází Sql** | **servery** | **serveru Řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="e15ae-462">Pokud nemáte server vytvořeno, můžete vytvořit jeden pomocí **přidat** tlačítka na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="e15ae-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="e15ae-463">Poznamenejte si **název serveru a adresa URL, správce přihlašovací jméno a heslo**, jako je použijete v další úkoly.</span><span class="sxs-lookup"><span data-stu-id="e15ae-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="e15ae-464">Nevytvářejte databáze ještě, jak bude vytvořen v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="e15ae-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="e15ae-465">![Řídicí panel serveru databáze SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "řídicího panelu serveru databáze SQL")</span><span class="sxs-lookup"><span data-stu-id="e15ae-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="e15ae-466">*Řídicí panel serveru databáze SQL*</span><span class="sxs-lookup"><span data-stu-id="e15ae-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="e15ae-467">V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio, proto je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="e15ae-468">To lze provést, klikněte na tlačítko **konfigurace**, vyberte IP adresu z **aktuální IP adresa klienta** a vkládání na **počáteční IP adresa** a **Koncová IP adresa** textová pole a kliknutím ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e15ae-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Přidávání IP adresy klienta](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="e15ae-470">*Přidávání IP adresy klienta*</span><span class="sxs-lookup"><span data-stu-id="e15ae-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="e15ae-471">Jednou **IP adresa klienta** je povolené IP adresy do seznamu, klikněte na **Uložit** potvrďte změny.</span><span class="sxs-lookup"><span data-stu-id="e15ae-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potvrzení změn](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="e15ae-473">*Potvrzení změn*</span><span class="sxs-lookup"><span data-stu-id="e15ae-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="e15ae-474">Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu</span><span class="sxs-lookup"><span data-stu-id="e15ae-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="e15ae-475">Přejděte zpět na ASP.NET MVC 4 řešení.</span><span class="sxs-lookup"><span data-stu-id="e15ae-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="e15ae-476">V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="e15ae-477">![Publikování aplikace](aspnet-mvc-4-models-and-data-access/_static/image43.png "publikování aplikace")</span><span class="sxs-lookup"><span data-stu-id="e15ae-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="e15ae-478">*Publikování webu*</span><span class="sxs-lookup"><span data-stu-id="e15ae-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="e15ae-479">Umožňuje naimportujte profil publikování, který jste uložili v první úloze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="e15ae-480">![Import profilu publikování](aspnet-mvc-4-models-and-data-access/_static/image44.png "import profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="e15ae-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="e15ae-481">*Import profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="e15ae-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="e15ae-482">Klikněte na tlačítko **ověření připojení**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-482">Click **Validate Connection**.</span></span> <span data-ttu-id="e15ae-483">Po dokončení ověření klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e15ae-484">Ověření je hotová, jakmile se zobrazí zelené zaškrtnutí zobrazí vedle tlačítko ověřit připojení.</span><span class="sxs-lookup"><span data-stu-id="e15ae-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="e15ae-485">![Ověření připojení](aspnet-mvc-4-models-and-data-access/_static/image45.png "ověřování připojení")</span><span class="sxs-lookup"><span data-stu-id="e15ae-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="e15ae-486">*Ověření připojení*</span><span class="sxs-lookup"><span data-stu-id="e15ae-486">*Validating connection*</span></span>
4. <span data-ttu-id="e15ae-487">V **nastavení** v části **databáze** části, klikněte na tlačítko vedle připojení databáze textové pole (tj. **objekt DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="e15ae-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="e15ae-488">![Konfigurace nasazení webu](aspnet-mvc-4-models-and-data-access/_static/image46.png "konfigurace nasazení webu")</span><span class="sxs-lookup"><span data-stu-id="e15ae-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="e15ae-489">*Konfigurace nasazení webu*</span><span class="sxs-lookup"><span data-stu-id="e15ae-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="e15ae-490">Připojení k databázi nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e15ae-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="e15ae-491">V **název serveru** zadejte vaše databáze SQL serveru adresu URL pomocí *tcp:* předponu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="e15ae-492">V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="e15ae-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="e15ae-493">V **heslo** zadejte přihlašovací heslo správce serveru.</span><span class="sxs-lookup"><span data-stu-id="e15ae-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="e15ae-494">Zadejte nový název databáze.</span><span class="sxs-lookup"><span data-stu-id="e15ae-494">Type a new database name.</span></span>

     <span data-ttu-id="e15ae-495">![Konfigurace cílový připojovací řetězec](aspnet-mvc-4-models-and-data-access/_static/image47.png "konfigurace cílový připojovací řetězec")</span><span class="sxs-lookup"><span data-stu-id="e15ae-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="e15ae-496">*Konfigurace cílový připojovací řetězec*</span><span class="sxs-lookup"><span data-stu-id="e15ae-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="e15ae-497">Pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-497">Then click **OK**.</span></span> <span data-ttu-id="e15ae-498">Po zobrazení výzvy k vytvoření databáze, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="e15ae-499">![Vytvoření databáze](aspnet-mvc-4-models-and-data-access/_static/image48.png "vytváření řetězec databáze")</span><span class="sxs-lookup"><span data-stu-id="e15ae-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="e15ae-500">*Vytvoření databáze*</span><span class="sxs-lookup"><span data-stu-id="e15ae-500">*Creating the database*</span></span>
7. <span data-ttu-id="e15ae-501">Připojovací řetězec, který budete používat pro připojení k databázi SQL v systému Windows Azure je uvedené v rámci textbox výchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="e15ae-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="e15ae-502">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-502">Then click **Next**.</span></span>

    <span data-ttu-id="e15ae-503">![Připojovací řetězec odkazující na databázi SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "připojovací řetězec odkazující na databázi SQL")</span><span class="sxs-lookup"><span data-stu-id="e15ae-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="e15ae-504">*Připojovací řetězec odkazující na databázi SQL*</span><span class="sxs-lookup"><span data-stu-id="e15ae-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="e15ae-505">V **Preview** klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="e15ae-506">![Publikování webové aplikace](aspnet-mvc-4-models-and-data-access/_static/image50.png "publikování webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="e15ae-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="e15ae-507">*Publikování webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="e15ae-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="e15ae-508">Jakmile proces publikování dokončí, otevře se výchozí prohlížeč publikované webové stránky.</span><span class="sxs-lookup"><span data-stu-id="e15ae-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="e15ae-509">Příloha C: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="e15ae-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="e15ae-510">S fragmenty kódu máte všechny kód, který je nutné na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="e15ae-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="e15ae-511">Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="e15ae-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="e15ae-512">![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-models-and-data-access/_static/image51.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="e15ae-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="e15ae-513">*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="e15ae-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="e15ae-514">***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***</span><span class="sxs-lookup"><span data-stu-id="e15ae-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="e15ae-515">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="e15ae-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="e15ae-516">Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).</span><span class="sxs-lookup"><span data-stu-id="e15ae-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="e15ae-517">Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.</span><span class="sxs-lookup"><span data-stu-id="e15ae-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="e15ae-518">Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).</span><span class="sxs-lookup"><span data-stu-id="e15ae-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="e15ae-519">Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="e15ae-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="e15ae-520">![Začněte psát název fragmentu](aspnet-mvc-4-models-and-data-access/_static/image52.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="e15ae-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="e15ae-521">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="e15ae-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="e15ae-522">![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](aspnet-mvc-4-models-and-data-access/_static/image53.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="e15ae-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="e15ae-523">*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="e15ae-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="e15ae-524">![Stisknutím klávesy Tab znovu a fragmentu rozšíří](aspnet-mvc-4-models-and-data-access/_static/image54.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")</span><span class="sxs-lookup"><span data-stu-id="e15ae-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="e15ae-525">*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*</span><span class="sxs-lookup"><span data-stu-id="e15ae-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="e15ae-526">***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="e15ae-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="e15ae-527">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="e15ae-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="e15ae-528">Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="e15ae-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="e15ae-529">Vyberte relevantní fragment kódu ze seznamu, kliknutím na.</span><span class="sxs-lookup"><span data-stu-id="e15ae-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="e15ae-530">![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-models-and-data-access/_static/image55.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="e15ae-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="e15ae-531">*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="e15ae-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="e15ae-532">![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-models-and-data-access/_static/image56.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="e15ae-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="e15ae-533">*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="e15ae-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
