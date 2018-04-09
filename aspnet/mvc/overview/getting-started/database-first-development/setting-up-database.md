---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Začínáme s Entity Framework 6 Database First pomocí MVC 5 | Microsoft Docs
author: tfitzmac
description: Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: ae60b5c808d2522c66dc17ccf7d16fefdc65d552
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a><span data-ttu-id="5dc57-104">Začínáme s Entity Framework 6 Database First pomocí MVC 5</span><span class="sxs-lookup"><span data-stu-id="5dc57-104">Getting Started with Entity Framework 6 Database First using MVC 5</span></span>
====================
<span data-ttu-id="5dc57-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5dc57-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5dc57-106">Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="5dc57-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="5dc57-107">Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="5dc57-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="5dc57-108">Generovaný kód odpovídá sloupců v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="5dc57-108">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="5dc57-109">V poslední části řady nasadíte lokalitu a databázi do Azure.</span><span class="sxs-lookup"><span data-stu-id="5dc57-109">In the last part of the series, you will deploy the site and database to Azure.</span></span>
> 
> <span data-ttu-id="5dc57-110">Tato část řady se zaměřuje na vytvoření databáze a vyplnění daty.</span><span class="sxs-lookup"><span data-stu-id="5dc57-110">This part of the series focuses on creating a database and populating it with data.</span></span>
> 
> <span data-ttu-id="5dc57-111">Tato řada byla zapsána s příspěvky z tní Dykstra a Rick Anderson.</span><span class="sxs-lookup"><span data-stu-id="5dc57-111">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="5dc57-112">Byl vylepšen na základě na zpětnou vazbu od uživatelů v sekci komentáře.</span><span class="sxs-lookup"><span data-stu-id="5dc57-112">It was improved based on feedback from users in the comments section.</span></span>


## <a name="introduction"></a><span data-ttu-id="5dc57-113">Úvod</span><span class="sxs-lookup"><span data-stu-id="5dc57-113">Introduction</span></span>

<span data-ttu-id="5dc57-114">Toto téma ukazuje, jak spustit s existující databáze a rychle vytvořit webovou aplikaci, která umožňuje uživatelům interakci s daty.</span><span class="sxs-lookup"><span data-stu-id="5dc57-114">This topic shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="5dc57-115">Entity Framework 6 a MVC 5 používá k vytvoření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5dc57-115">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="5dc57-116">Funkce ASP.NET generování uživatelského rozhraní umožňuje automaticky generovat kód pro zobrazení, aktualizaci, vytváření a odstraňování dat.</span><span class="sxs-lookup"><span data-stu-id="5dc57-116">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="5dc57-117">Pomocí nástroje pro publikování v sadě Visual Studio, můžete snadno nasadit lokalitu a databázi do Azure.</span><span class="sxs-lookup"><span data-stu-id="5dc57-117">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="5dc57-118">Toto téma nabízí situaci, kdy máte databázi a chcete pro generování kódu pro webovou aplikaci na základě polí této databáze.</span><span class="sxs-lookup"><span data-stu-id="5dc57-118">This topic addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="5dc57-119">Tento postup se nazývá Database First vývoj.</span><span class="sxs-lookup"><span data-stu-id="5dc57-119">This approach is called Database First development.</span></span> <span data-ttu-id="5dc57-120">Pokud již nemáte existující databázi, můžete místo toho volat Code First vývoj, který zahrnuje definování datových tříd a generování databáze z vlastnosti třídy přístup.</span><span class="sxs-lookup"><span data-stu-id="5dc57-120">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

<span data-ttu-id="5dc57-121">Příklad úvodní Code First vývoj naleznete v části [Začínáme s ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="5dc57-121">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="5dc57-122">Pokročilejší příklad najdete v tématu [vytváření datového modelu Entity Framework pro aplikace ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="5dc57-122">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="5dc57-123">Informace o výběru přístupem Entity Framework, najdete v části [Entity Framework vývoj přístupy](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span><span class="sxs-lookup"><span data-stu-id="5dc57-123">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5dc57-124">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5dc57-124">Prerequisites</span></span>

<span data-ttu-id="5dc57-125">Visual Studio 2013 nebo Visual Studio Express 2013 pro Web</span><span class="sxs-lookup"><span data-stu-id="5dc57-125">Visual Studio 2013 or Visual Studio Express 2013 for Web</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="5dc57-126">Nastavení databáze</span><span class="sxs-lookup"><span data-stu-id="5dc57-126">Set up the database</span></span>

<span data-ttu-id="5dc57-127">Aby napodobovaly prostředí, že máte existující databázi, nejprve vytvořit databázi s některá předem vyplněné data a poté vytvořit webové aplikace, která se připojuje k databázi.</span><span class="sxs-lookup"><span data-stu-id="5dc57-127">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="5dc57-128">V tomto kurzu byla vyvinuta pomocí LocalDB Visual Studio 2013 nebo Visual Studio Express 2013 pro Web.</span><span class="sxs-lookup"><span data-stu-id="5dc57-128">This tutorial was developed using LocalDB with either Visual Studio 2013 or Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="5dc57-129">Můžete použít existující databázový server místo LocalDB, ale v závislosti na vaší verzi sady Visual Studio a váš typ databáze, všechny nástroje data v sadě Visual Studio nemusí být podporován.</span><span class="sxs-lookup"><span data-stu-id="5dc57-129">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="5dc57-130">Pokud nejsou k dispozici pro vaši databázi nástroje, musíte provést některé z kroků specifické pro databáze v sadě management pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="5dc57-130">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="5dc57-131">Pokud máte potíže s databázové nástroje ve vaší verzi sady Visual Studio, ujistěte se, jestli že je nainstalovaná nejnovější verze nástroje databáze.</span><span class="sxs-lookup"><span data-stu-id="5dc57-131">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="5dc57-132">Informace o aktualizaci nebo instalaci databáze nástroje najdete v tématu [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span><span class="sxs-lookup"><span data-stu-id="5dc57-132">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="5dc57-133">Spusťte sadu Visual Studio a vytvořte **projekt databáze serveru SQL**.</span><span class="sxs-lookup"><span data-stu-id="5dc57-133">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="5dc57-134">Název projektu **ContosoUniversityData**.</span><span class="sxs-lookup"><span data-stu-id="5dc57-134">Name the project **ContosoUniversityData**.</span></span>

![Vytvoření databáze projektu](setting-up-database/_static/image1.png)

<span data-ttu-id="5dc57-136">Nyní máte projekt prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="5dc57-136">You now have an empty database project.</span></span> <span data-ttu-id="5dc57-137">Tato databáze do Azure nasadí později v tomto kurzu, takže budete muset nastavit Azure SQL Database jako cílová platforma pro projekt.</span><span class="sxs-lookup"><span data-stu-id="5dc57-137">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="5dc57-138">Nastavení cílové platformy není nasazen ve skutečnosti databázi; pouze znamená, že projekt databáze bude ověřte, zda návrhu databáze je kompatibilní s cílovou platformu.</span><span class="sxs-lookup"><span data-stu-id="5dc57-138">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="5dc57-139">Chcete-li nastavit cílovou platformu, otevřete **vlastnosti** pro projekt a vyberte **Microsoft Azure SQL Database** pro cílovou platformu.</span><span class="sxs-lookup"><span data-stu-id="5dc57-139">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

![Sada cílové platformy](setting-up-database/_static/image2.png)

<span data-ttu-id="5dc57-141">Můžete vytvořit tabulky pro tento kurz potřeba přidáním skripty SQL, které určují tabulky.</span><span class="sxs-lookup"><span data-stu-id="5dc57-141">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="5dc57-142">Klikněte pravým tlačítkem na projekt a přidejte novou položku.</span><span class="sxs-lookup"><span data-stu-id="5dc57-142">Right-click your project and add a new item.</span></span>

![Přidat novou položku](setting-up-database/_static/image3.png)

<span data-ttu-id="5dc57-144">Přidá novou tabulku s názvem Student.</span><span class="sxs-lookup"><span data-stu-id="5dc57-144">Add a new table named Student.</span></span>

![Přidat tabulka student](setting-up-database/_static/image4.png)

<span data-ttu-id="5dc57-146">V tabulce souboru nahraďte příkaz T-SQL následující kód k vytvoření tabulky.</span><span class="sxs-lookup"><span data-stu-id="5dc57-146">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="5dc57-147">Všimněte si, že okno návrh automaticky synchronizuje s kódem.</span><span class="sxs-lookup"><span data-stu-id="5dc57-147">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="5dc57-148">Můžete pracovat s kódem nebo designer.</span><span class="sxs-lookup"><span data-stu-id="5dc57-148">You can work with either the code or designer.</span></span>

![Zobrazit kód a návrh](setting-up-database/_static/image5.png)

<span data-ttu-id="5dc57-150">Přidejte jinou tabulkou.</span><span class="sxs-lookup"><span data-stu-id="5dc57-150">Add another table.</span></span> <span data-ttu-id="5dc57-151">Tento čas název postupu a použijte následující příkaz T-SQL.</span><span class="sxs-lookup"><span data-stu-id="5dc57-151">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="5dc57-152">A opakujte ještě jednou a vytvořte tabulku s názvem registrace.</span><span class="sxs-lookup"><span data-stu-id="5dc57-152">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="5dc57-153">Jej můžete naplnit databázi daty pomocí skriptu, který se spouští po nasazení databáze.</span><span class="sxs-lookup"><span data-stu-id="5dc57-153">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="5dc57-154">Po nasazení skriptu přidáte do projektu.</span><span class="sxs-lookup"><span data-stu-id="5dc57-154">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="5dc57-155">Můžete použít výchozí název.</span><span class="sxs-lookup"><span data-stu-id="5dc57-155">You can use the default name.</span></span>

![Přidejte skript po nasazení](setting-up-database/_static/image6.png)

<span data-ttu-id="5dc57-157">Přidejte následující kód T-SQL skriptu po nasazení.</span><span class="sxs-lookup"><span data-stu-id="5dc57-157">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="5dc57-158">Tento skript jednoduše přidá data do databáze, pokud se nenajde žádný odpovídající záznam.</span><span class="sxs-lookup"><span data-stu-id="5dc57-158">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="5dc57-159">Nemá přepsat nebo odstranit data, která jste zadali do databáze.</span><span class="sxs-lookup"><span data-stu-id="5dc57-159">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="5dc57-160">Je důležité si uvědomit, že po nasazení skript je spuštěn pokaždé, když nasazujete projekt databáze.</span><span class="sxs-lookup"><span data-stu-id="5dc57-160">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="5dc57-161">Proto musíte pečlivě zvažte své požadavky na při zápisu tohoto skriptu.</span><span class="sxs-lookup"><span data-stu-id="5dc57-161">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="5dc57-162">V některých případech můžete chtít začít znovu od známých sadu dat pokaždé, když je projekt nasazen.</span><span class="sxs-lookup"><span data-stu-id="5dc57-162">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="5dc57-163">V ostatních případech nemusíte chtít změnit stávající data žádným způsobem.</span><span class="sxs-lookup"><span data-stu-id="5dc57-163">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="5dc57-164">Podle potřeb, můžete rozhodnout, zda je nutné spuštění skriptu po nasazení nebo co je potřeba zahrnout do skriptu.</span><span class="sxs-lookup"><span data-stu-id="5dc57-164">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="5dc57-165">Další informace o naplnění databáze pomocí skriptu po nasazení najdete v tématu [včetně dat v projektu databáze serveru SQL](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dc57-165">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="5dc57-166">Nyní máte 4 soubory skriptu SQL, ale žádné skutečné tabulky.</span><span class="sxs-lookup"><span data-stu-id="5dc57-166">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="5dc57-167">Jste připravení nasadit projekt databáze na instanci localdb.</span><span class="sxs-lookup"><span data-stu-id="5dc57-167">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="5dc57-168">V sadě Visual Studio klikněte na tlačítko Start (F5) pro sestavení a nasadit projekt databáze.</span><span class="sxs-lookup"><span data-stu-id="5dc57-168">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="5dc57-169">Zkontrolujte kartu výstup a ověřte, zda sestavení a nasazení proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="5dc57-169">Check the Output tab to verify that the build and deployment succeeded.</span></span>

![Zobrazit výstup](setting-up-database/_static/image7.png)

<span data-ttu-id="5dc57-171">Pokud chcete zobrazit, že byla vytvořena nová databáze, otevřete **Průzkumník objektů systému SQL Server** a podívejte se na název projektu na správné místní databázi serveru (v tomto případě **(localdb) \ProjectsV12**).</span><span class="sxs-lookup"><span data-stu-id="5dc57-171">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV12**).</span></span>

![Zobrazit novou databázi](setting-up-database/_static/image8.png)

<span data-ttu-id="5dc57-173">Pokud chcete zobrazit, v tabulkách jsou naplněný daty, klikněte pravým tlačítkem na tabulku a vyberte **Data zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="5dc57-173">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![Zobrazit data tabulky](setting-up-database/_static/image9.png)

<span data-ttu-id="5dc57-175">Zobrazí se upravitelné zobrazení dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="5dc57-175">An editable view of the table data is displayed.</span></span>

![Zobrazit výsledky tabulky dat](setting-up-database/_static/image10.png)

<span data-ttu-id="5dc57-177">Vaše databáze je nyní nastavení a naplněný daty.</span><span class="sxs-lookup"><span data-stu-id="5dc57-177">Your database is now set up and populated with data.</span></span> <span data-ttu-id="5dc57-178">V dalším kurzu vytvoříte webovou aplikaci pro databázi.</span><span class="sxs-lookup"><span data-stu-id="5dc57-178">In the next tutorial, you will create a web application for the database.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5dc57-179">Next</span><span class="sxs-lookup"><span data-stu-id="5dc57-179">Next</span></span>](creating-the-web-application.md)
