---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Databázi EF nejprve s architekturou ASP.NET MVC: vytvoření webové aplikace a datové modely | Microsoft Docs'
author: tfitzmac
description: Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 04ccc00fa48702608fdc7b5b00d73778985852f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878773"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a><span data-ttu-id="2aa63-104">Databázi EF nejprve s architekturou ASP.NET MVC: vytvoření webové aplikace a datové modely</span><span class="sxs-lookup"><span data-stu-id="2aa63-104">EF Database First with ASP.NET MVC: Creating the Web Application and Data Models</span></span>
====================
<span data-ttu-id="2aa63-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2aa63-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2aa63-106">Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="2aa63-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="2aa63-107">Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="2aa63-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="2aa63-108">Generovaný kód odpovídá sloupců v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="2aa63-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="2aa63-109">Tato část řady se zaměřuje na vytvoření webové aplikace a generování modelů dat podle databázových tabulek.</span><span class="sxs-lookup"><span data-stu-id="2aa63-109">This part of the series focuses on creating the web application, and generating the data models based on your database tables.</span></span>


## <a name="create-a-new-aspnet-web-application"></a><span data-ttu-id="2aa63-110">Vytvoření nové webové aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2aa63-110">Create a new ASP.NET Web Application</span></span>

<span data-ttu-id="2aa63-111">V nové řešení nebo do stejného řešení jako projekt databáze, vytvořte nový projekt v sadě Visual Studio a vyberte **webové aplikace ASP.NET** šablony.</span><span class="sxs-lookup"><span data-stu-id="2aa63-111">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="2aa63-112">Název projektu **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="2aa63-112">Name the project **ContosoSite**.</span></span>

![Vytvoření projektu](creating-the-web-application/_static/image1.png)

<span data-ttu-id="2aa63-114">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="2aa63-114">Click **OK**.</span></span>

<span data-ttu-id="2aa63-115">V okně Nový projekt ASP.NET, vyberte **MVC** šablony.</span><span class="sxs-lookup"><span data-stu-id="2aa63-115">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="2aa63-116">Můžete vymazat **hostitel v cloudu** možnost prozatím, protože se nasazení aplikace do cloudu později.</span><span class="sxs-lookup"><span data-stu-id="2aa63-116">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="2aa63-117">Klikněte na tlačítko **OK** k vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="2aa63-117">Click **OK** to create the application.</span></span>

![Vyberte šablonu mvc](creating-the-web-application/_static/image2.png)

<span data-ttu-id="2aa63-119">Vytvoření projektu s výchozí soubory a složky.</span><span class="sxs-lookup"><span data-stu-id="2aa63-119">The project is created with the default files and folders.</span></span>

<span data-ttu-id="2aa63-120">V tomto kurzu použijete Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2aa63-120">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="2aa63-121">Ve vašem projektu prostřednictvím okno Spravovat balíčky NuGet můžete zkontrolujte verzi rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2aa63-121">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="2aa63-122">V případě potřeby aktualizujte svou verzi rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2aa63-122">If necessary, update your version of Entity Framework.</span></span>

![Zobrazit verze](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="2aa63-124">Generování modelů</span><span class="sxs-lookup"><span data-stu-id="2aa63-124">Generate the models</span></span>

<span data-ttu-id="2aa63-125">Rozhraní Entity Framework modely nyní vytvoří z tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="2aa63-125">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="2aa63-126">Tyto modely jsou třídy, které budete používat pro práci s daty.</span><span class="sxs-lookup"><span data-stu-id="2aa63-126">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="2aa63-127">Každý model zrcadlí tabulky v databázi a obsahuje vlastnosti, které odpovídají sloupců v tabulce.</span><span class="sxs-lookup"><span data-stu-id="2aa63-127">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="2aa63-128">Klikněte pravým tlačítkem myši **modely** složky a vyberte **přidat** a **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="2aa63-128">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

![Přidat novou položku](creating-the-web-application/_static/image4.png)

<span data-ttu-id="2aa63-130">V okně Přidat novou položku, vyberte **Data** v levém podokně a **ADO.NET Entity Data Model** z možností v prostředním podokně.</span><span class="sxs-lookup"><span data-stu-id="2aa63-130">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="2aa63-131">Název nového souboru modelu **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="2aa63-131">Name the new model file **ContosoModel**.</span></span>

![Vytvoření modelu](creating-the-web-application/_static/image5.png)

<span data-ttu-id="2aa63-133">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="2aa63-133">Click **Add**.</span></span>

<span data-ttu-id="2aa63-134">V průvodci Entity Data Model vyberte **EF Designer z databáze**.</span><span class="sxs-lookup"><span data-stu-id="2aa63-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

![generování z databáze](creating-the-web-application/_static/image6.png)

<span data-ttu-id="2aa63-136">Klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2aa63-136">Click **Next**.</span></span>

<span data-ttu-id="2aa63-137">Pokud máte připojení databáze, které jsou definované v rámci vývojového prostředí, může najdete v jednom z těchto připojení předem vybraná.</span><span class="sxs-lookup"><span data-stu-id="2aa63-137">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="2aa63-138">Chcete vytvořit nové připojení k databázi, kterou jste vytvořili v první části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2aa63-138">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="2aa63-139">Klikněte **nové připojení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2aa63-139">Click the **New Connection** button.</span></span>

![připojení k databázi](creating-the-web-application/_static/image7.png)

<span data-ttu-id="2aa63-141">V okně Vlastnosti připojení zadat název místního serveru, kde byla vytvořena databáze (v tomto případě **(localdb) \ProjectsV12**).</span><span class="sxs-lookup"><span data-stu-id="2aa63-141">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV12**).</span></span> <span data-ttu-id="2aa63-142">Po zadání názvu serveru, vyberte z dostupných databází ContosoUniversityData.</span><span class="sxs-lookup"><span data-stu-id="2aa63-142">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![Nastavte vlastnosti připojení](creating-the-web-application/_static/image8.png)

<span data-ttu-id="2aa63-144">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="2aa63-144">Click **OK**.</span></span>

<span data-ttu-id="2aa63-145">Nyní se zobrazí vlastnosti správné připojení.</span><span class="sxs-lookup"><span data-stu-id="2aa63-145">The correct connection properties are now displayed.</span></span> <span data-ttu-id="2aa63-146">Můžete použít výchozí název pro připojení v souboru Web.Config</span><span class="sxs-lookup"><span data-stu-id="2aa63-146">You can use the default name for connection in the Web.Config file</span></span>

![nastavení připojení](creating-the-web-application/_static/image9.png)

<span data-ttu-id="2aa63-148">Klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2aa63-148">Click **Next**.</span></span>

<span data-ttu-id="2aa63-149">Vyberte **tabulky** ke generování modely pro všechny tři tabulky.</span><span class="sxs-lookup"><span data-stu-id="2aa63-149">Select **Tables** to generate models for all three tables.</span></span>

![Vyberte tabulky](creating-the-web-application/_static/image10.png)

<span data-ttu-id="2aa63-151">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="2aa63-151">Click **Finish**.</span></span>

<span data-ttu-id="2aa63-152">Pokud se zobrazí upozornění zabezpečení, vyberte **OK** pokračovat v provozu šablony.</span><span class="sxs-lookup"><span data-stu-id="2aa63-152">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="2aa63-153">Z tabulky databáze, které se generují modely a zobrazená diagram znázorňuje vlastnosti a vztahy mezi tabulkami.</span><span class="sxs-lookup"><span data-stu-id="2aa63-153">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![Diagram modelu](creating-the-web-application/_static/image11.png)

<span data-ttu-id="2aa63-155">Složku modely nyní zahrnuje mnoho nové soubory související s modely, které byly vygenerovány z databáze.</span><span class="sxs-lookup"><span data-stu-id="2aa63-155">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

![Zobrazit nové soubory modelu](creating-the-web-application/_static/image12.png)

<span data-ttu-id="2aa63-157">**ContosoModel.Context.cs** soubor obsahuje třídu odvozenou od **DbContext** třídy a poskytuje vlastnosti pro každou třídu modelu, která odpovídá do databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="2aa63-157">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="2aa63-158">**Course.cs**, **Enrollment.cs**, a **Student.cs** soubory obsahují třídy modelu, které představují tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="2aa63-158">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="2aa63-159">Třídy kontextu a třídy modelu použije při práci s generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2aa63-159">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="2aa63-160">Než budete pokračovat v tomto kurzu, sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="2aa63-160">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="2aa63-161">V další části bude generovat kód podle datové modely, ale tento oddíl nebude fungovat, pokud nebyl sestaven projektu.</span><span class="sxs-lookup"><span data-stu-id="2aa63-161">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2aa63-162">[Předchozí](setting-up-database.md)
> [další](generating-views.md)</span><span class="sxs-lookup"><span data-stu-id="2aa63-162">[Previous](setting-up-database.md)
[Next](generating-views.md)</span></span>
