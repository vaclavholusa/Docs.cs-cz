---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Databázi EF nejprve s architekturou ASP.NET MVC: generování zobrazení | Microsoft Docs'
author: tfitzmac
description: Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: b60e89a187a879255eb051dc87241714cef6fa63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="8a975-104">Databázi EF nejprve s architekturou ASP.NET MVC: generování zobrazení</span><span class="sxs-lookup"><span data-stu-id="8a975-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="8a975-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8a975-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8a975-106">Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="8a975-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="8a975-107">Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="8a975-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="8a975-108">Generovaný kód odpovídá sloupců v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="8a975-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="8a975-109">Tato část řady se zaměřuje na generování kontrolery a zobrazení pomocí generování uživatelského rozhraní ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8a975-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="8a975-110">Přidat vygenerované uživatelské rozhraní</span><span class="sxs-lookup"><span data-stu-id="8a975-110">Add scaffold</span></span>

<span data-ttu-id="8a975-111">Jste připraveni ke generování kódu, který bude poskytovat standard datové operace pro třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="8a975-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="8a975-112">Přidejte kód tak, že přidáte položku vygenerované uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8a975-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="8a975-113">Existuje mnoho možností pro typ generování uživatelského rozhraní, které můžete přidat; v tomto kurzu bude obsahovat vygenerované uživatelské rozhraní, řadič a zobrazení, které odpovídají Student a registrace modely, kterou jste vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="8a975-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="8a975-114">K zajištění konzistence v projektu, přidáte nový řadič ke stávající **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="8a975-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="8a975-115">Klikněte pravým tlačítkem myši **řadiče** složky a vyberte **přidat** – **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="8a975-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![Přidat vygenerované uživatelské rozhraní](generating-views/_static/image1.png)

<span data-ttu-id="8a975-117">Vyberte **kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework** možnost.</span><span class="sxs-lookup"><span data-stu-id="8a975-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="8a975-118">Tato možnost způsobí vygenerování řadiče a zobrazení pro aktualizaci, odstranění, vytváření a zobrazování dat v modelu.</span><span class="sxs-lookup"><span data-stu-id="8a975-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![přidat řadič mvc](generating-views/_static/image2.png)

<span data-ttu-id="8a975-120">Vyberte **Student** pro třídu modelu, vyberte **ContosoUniversityEntities** pro třídy kontextu.</span><span class="sxs-lookup"><span data-stu-id="8a975-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="8a975-121">Zachovat názvu řadiče jako **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="8a975-121">Keep the controller name as **StudentsController**,</span></span>

![Zadejte řadič](generating-views/_static/image3.png)

<span data-ttu-id="8a975-123">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8a975-123">Click **Add**.</span></span>

<span data-ttu-id="8a975-124">Pokud se zobrazí chyba, pravděpodobně není sestavení projektu v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="8a975-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="8a975-125">Pokud ano, zkuste jej sestavit projekt a poté znovu přidejte vygenerované položky.</span><span class="sxs-lookup"><span data-stu-id="8a975-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="8a975-126">Po dokončení procesu generování kódu se zobrazí nový řadič a zobrazení ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="8a975-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![Zobrazit zobrazení](generating-views/_static/image4.png)

<span data-ttu-id="8a975-128">Stejný postup proveďte znovu, ale přidat vygenerované uživatelské rozhraní pro třídu registrace.</span><span class="sxs-lookup"><span data-stu-id="8a975-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="8a975-129">Po dokončení byste měli mít **EnrollmentsController.cs** souborů a složku ve složce **zobrazení** s názvem **registrace** vytvořit, odstranit, podrobnosti, úpravy a Index zobrazení.</span><span class="sxs-lookup"><span data-stu-id="8a975-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![Zobrazit zobrazení](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="8a975-131">Přidání odkazů na nové zobrazení</span><span class="sxs-lookup"><span data-stu-id="8a975-131">Add links to new views</span></span>

<span data-ttu-id="8a975-132">K usnadnění přejděte na nová zobrazení, můžete přidat několik hypertextové odkazy na Index zobrazení pro studenty a registrace.</span><span class="sxs-lookup"><span data-stu-id="8a975-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="8a975-133">Otevřete soubor v **Views/Home/Index.cshtml**, což je domovská stránka pro svůj web.</span><span class="sxs-lookup"><span data-stu-id="8a975-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="8a975-134">Přidejte následující kód pod jumbotron.</span><span class="sxs-lookup"><span data-stu-id="8a975-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="8a975-135">Pro metodu ActionLink první parametr je text, který se zobrazí v odkazu.</span><span class="sxs-lookup"><span data-stu-id="8a975-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="8a975-136">Druhý parametr je akce a třetí parametr je název řadiče.</span><span class="sxs-lookup"><span data-stu-id="8a975-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="8a975-137">Například odkazuje na první odkaz na akce indexu v StudentsController.</span><span class="sxs-lookup"><span data-stu-id="8a975-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="8a975-138">Skutečný hypertextový odkaz je sestavit z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="8a975-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="8a975-139">Na první odkaz nakonec trvá uživatelům **Index.cshtml** souboru v rámci **zobrazení/studenty** složky.</span><span class="sxs-lookup"><span data-stu-id="8a975-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="8a975-140">Zobrazit student zobrazení</span><span class="sxs-lookup"><span data-stu-id="8a975-140">Display student views</span></span>

<span data-ttu-id="8a975-141">Ověříte, že kód přidán do projektu správně zobrazí seznam na studentů a umožňuje uživatelům upravit, vytvořit nebo odstranit záznamy studentů v databázi.</span><span class="sxs-lookup"><span data-stu-id="8a975-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="8a975-142">Klikněte pravým tlačítkem myši **Views/Home/Index.cshtml** soubor a vyberte **zobrazit v prohlížeči**.</span><span class="sxs-lookup"><span data-stu-id="8a975-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="8a975-143">Na této stránce klikněte na odkaz na seznam studenty.</span><span class="sxs-lookup"><span data-stu-id="8a975-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="8a975-144">Na této stránce Všimněte si seznamu studenty a odkazy, které chcete upravit tato data.</span><span class="sxs-lookup"><span data-stu-id="8a975-144">On this page, notice the list of the students and links to modify this data.</span></span>

![seznam studenty](generating-views/_static/image7.png)

<span data-ttu-id="8a975-146">Klikněte **vytvořit nový** propojení a zadejte hodnoty pro nové student.</span><span class="sxs-lookup"><span data-stu-id="8a975-146">Click the **Create New** link and provide some values for a new student.</span></span>

![Vytvořit nový student](generating-views/_static/image8.png)

<span data-ttu-id="8a975-148">Klikněte na tlačítko **vytvořit**a Všimněte si nový student je přidán do seznamu.</span><span class="sxs-lookup"><span data-stu-id="8a975-148">Click **Create**, and notice the new student is added to your list.</span></span>

![seznam s novou studenty](generating-views/_static/image9.png)

<span data-ttu-id="8a975-150">Vyberte **upravit** propojit a změnit některé hodnoty pro student.</span><span class="sxs-lookup"><span data-stu-id="8a975-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![Upravit studenty](generating-views/_static/image10.png)

<span data-ttu-id="8a975-152">Klikněte na tlačítko **Uložit**a Všimněte si student záznam byl změněn.</span><span class="sxs-lookup"><span data-stu-id="8a975-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="8a975-153">Nakonec vyberte **odstranit** propojit a potvrďte, že chcete odstranit záznam kliknutím **odstranit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a975-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![Odstranit studenty](generating-views/_static/image11.png)

<span data-ttu-id="8a975-155">Bez psaní kódu, jste přidali zobrazení, které provádějí běžné operace s daty v tabulce Student.</span><span class="sxs-lookup"><span data-stu-id="8a975-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="8a975-156">Jste si všimli, že je vlastnost databáze podle textový popisek pro pole (například **LastName**) který není nutně co chcete zobrazit na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="8a975-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="8a975-157">Například můžete chtít štítek, který chcete být **příjmení**.</span><span class="sxs-lookup"><span data-stu-id="8a975-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="8a975-158">Tento problém se zobrazením opraví později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="8a975-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="8a975-159">Zobrazit registrace zobrazení</span><span class="sxs-lookup"><span data-stu-id="8a975-159">Display enrollment views</span></span>

<span data-ttu-id="8a975-160">Vaše databáze zahrnuje na více relaci mezi tabulkami Student a registrace a jeden mnoho relace mezi tabulkami průběh a zápisu.</span><span class="sxs-lookup"><span data-stu-id="8a975-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="8a975-161">Zobrazení pro registraci správně zpracovat tyto relace.</span><span class="sxs-lookup"><span data-stu-id="8a975-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="8a975-162">Přejít na domovskou stránku pro web a vyberte **seznam registrace** odkazu a potom **vytvořit nový** odkaz.</span><span class="sxs-lookup"><span data-stu-id="8a975-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="8a975-163">Zobrazení zobrazí formulář pro vytvoření nového záznamu registrace.</span><span class="sxs-lookup"><span data-stu-id="8a975-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="8a975-164">Zejména Všimněte si, že tento formulář obsahuje dvě rozevírací seznamy, které se naplní hodnoty ze souvisejících tabulek.</span><span class="sxs-lookup"><span data-stu-id="8a975-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![Vytvoření registrace](generating-views/_static/image12.png)

<span data-ttu-id="8a975-166">Kromě toho ověření zadaných hodnot se automaticky použity na základě datový typ pole.</span><span class="sxs-lookup"><span data-stu-id="8a975-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="8a975-167">Úrovni vyžaduje číslo, takže pokud se pokusíte zadat hodnotu není kompatibilní, zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="8a975-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![ověřování zpráv](generating-views/_static/image13.png)

<span data-ttu-id="8a975-169">Jste ověřili, že zobrazení automaticky generované umožnit uživatelům pracovat s daty v databázi.</span><span class="sxs-lookup"><span data-stu-id="8a975-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="8a975-170">V dalším kurzu této série se aktualizovat databázi a provést odpovídající změny ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8a975-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8a975-171">[Předchozí](creating-the-web-application.md)
> [další](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="8a975-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
