---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Databázi EF nejprve s architekturou ASP.NET MVC: Změna databáze | Microsoft Docs'
author: tfitzmac
description: Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 63ee8768a43dbdac80922e3adbedd3378c10da73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="a7ce7-104">Databázi EF nejprve s architekturou ASP.NET MVC: Změna databáze</span><span class="sxs-lookup"><span data-stu-id="a7ce7-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="a7ce7-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a7ce7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a7ce7-106">Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="a7ce7-107">Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="a7ce7-108">Generovaný kód odpovídá sloupců v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="a7ce7-109">Tato část řady se zaměřuje na provedení aktualizace na strukturu databáze a šíření této změny v rámci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="a7ce7-110">Přidá sloupec</span><span class="sxs-lookup"><span data-stu-id="a7ce7-110">Add a column</span></span>

<span data-ttu-id="a7ce7-111">Pokud aktualizujete struktura tabulky v databázi, je třeba zajistit, že změny rozšířena do datový model, zobrazení a kontroler.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="a7ce7-112">V tomto kurzu přidáte nový sloupec do tabulky Student k zaznamenání křestní jméno student.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="a7ce7-113">Pokud chcete přidat tento sloupec, otevřete projekt databáze a otevřete soubor Student.sql.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="a7ce7-114">Prostřednictvím návrháře nebo kód T-SQL, přidat sloupec s názvem **MiddleName** , je NVARCHAR(50) a umožňuje hodnoty NULL.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![Přidat křestní jméno](changing-the-database/_static/image1.png)

<span data-ttu-id="a7ce7-116">Nasaďte tuto změnu do místní databáze spuštěním projekt databáze (nebo F5).</span><span class="sxs-lookup"><span data-stu-id="a7ce7-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="a7ce7-117">Nové pole se přidá do tabulky.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-117">The new field is added to the table.</span></span> <span data-ttu-id="a7ce7-118">Pokud se nezobrazí se v Průzkumníku objektů SQL serveru, klikněte na tlačítko Aktualizovat v podokně.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![zobrazení nového sloupce](changing-the-database/_static/image2.png)

<span data-ttu-id="a7ce7-120">Nový sloupec v tabulce databáze existuje, ale neexistuje aktuálně v třídy datového modelu.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="a7ce7-121">Je třeba aktualizovat model, který má obsahovat nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-121">You must update the model to include your new column.</span></span> <span data-ttu-id="a7ce7-122">V **modely** složku, otevřete **ContosoModel.edmx** soubor k zobrazení diagramu modelu.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="a7ce7-123">Všimněte si, že Student model neobsahuje vlastnost MiddleName.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="a7ce7-124">Klepněte pravým tlačítkem myši na návrhovou plochu a vyberte **aktualizovat Model z databáze**.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![Aktualizace modelu](changing-the-database/_static/image3.png)

<span data-ttu-id="a7ce7-126">V Průvodci aktualizací, vyberte **aktualizovat** kartě a **Student** tabulky.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![Průvodce aktualizací](changing-the-database/_static/image4.png)

<span data-ttu-id="a7ce7-128">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-128">Click **Finish**.</span></span>

<span data-ttu-id="a7ce7-129">Po dokončení procesu aktualizace zahrnuje nový diagram databáze **MiddleName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="a7ce7-130">Uložit **ContosoModel.edmx** souboru.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="a7ce7-131">Je nutné uložit tento soubor nové vlastnosti mají být předány **Student.cs** třídy.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="a7ce7-132">Nyní jste aktualizovali databáze a modelu.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="a7ce7-133">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-133">Build the solution.</span></span>

<span data-ttu-id="a7ce7-134">Bohužel zobrazení stále neobsahuje novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="a7ce7-135">K aktualizaci zobrazení máte dvě možnosti – můžete znovu vygenerovat zobrazení generování uživatelského rozhraní pro třídu Student přidáním znovu, nebo můžete ručně přidat nové vlastnosti pro stávající zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="a7ce7-136">V tomto kurzu přidáte generování uživatelského rozhraní znovu vzhledem k tomu, že jste neprovedli změny přizpůsobené zobrazení automaticky generovány.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="a7ce7-137">Můžete uvažovat o ručně přidejte vlastnost, pokud byly provedeny změny zobrazení a nechcete tyto změny budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="a7ce7-138">Aby se znovu vytvořit zobrazení, odstranit **studenty** ve složce **zobrazení**a odstranit **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="a7ce7-139">Klikněte pravým tlačítkem **řadiče** složky a přidat generování uživatelského rozhraní pro **Student** modelu.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="a7ce7-140">Znovu, názvu kontroleru **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="a7ce7-141">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-141">Select **OK**.</span></span>

<span data-ttu-id="a7ce7-142">Zobrazení nyní obsahují vlastnost MiddleName.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-142">The views now contain the MiddleName property.</span></span>

![Zobrazit křestní jméno](changing-the-database/_static/image5.png)

<span data-ttu-id="a7ce7-144">V další části přidáte kód pro přizpůsobení zobrazení pro zobrazení podrobností o student záznamu.</span><span class="sxs-lookup"><span data-stu-id="a7ce7-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a7ce7-145">[Předchozí](generating-views.md)
> [další](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="a7ce7-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
