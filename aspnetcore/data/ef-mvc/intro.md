---
title: "Jádro ASP.NET MVC s Entity Framework Core - kurz 1 10"
author: tdykstra
description: 
keywords: Kurz ASP.NET Core Entity Framework Core,
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: b67c3d4a-f2bf-4132-a48b-4b0d599d7981
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/intro
ms.openlocfilehash: 2b21c7fb35c65d9374723faac5b812289023a0f6
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-entity-framework-core-using-visual-studio-1-of-10"></a><span data-ttu-id="0bda3-103">Začínáme s ASP.NET MVC jádra a Entity Framework Core pomocí sady Visual Studio (1 10)</span><span class="sxs-lookup"><span data-stu-id="0bda3-103">Getting started with ASP.NET Core MVC and Entity Framework Core using Visual Studio (1 of 10)</span></span>

<span data-ttu-id="0bda3-104">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0bda3-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0bda3-105">Stránky Razor verze tohoto kurzu je k dispozici [zde](xref:data/ef-rp/intro).</span><span class="sxs-lookup"><span data-stu-id="0bda3-105">A Razor Pages version of this tutorial is available [here](xref:data/ef-rp/intro).</span></span> <span data-ttu-id="0bda3-106">Verze stránky Razor je snazší postupujte podle a popisuje další funkce EF.</span><span class="sxs-lookup"><span data-stu-id="0bda3-106">The Razor Pages version is easier to follow and covers more EF features.</span></span> <span data-ttu-id="0bda3-107">Doporučujeme vám, že budete postupovat podle [stránky Razor verze tohoto kurzu](xref:data/ef-rp/intro).</span><span class="sxs-lookup"><span data-stu-id="0bda3-107">We recommend you follow the [Razor Pages version of this tutorial](xref:data/ef-rp/intro).</span></span>

<span data-ttu-id="0bda3-108">Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC 2.0 základní pomocí základní Entity Framework (EF) 2.0 a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="0bda3-108">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="0bda3-109">Vzorová aplikace je web pro fiktivní vysoké školy Contoso.</span><span class="sxs-lookup"><span data-stu-id="0bda3-109">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="0bda3-110">Obsahuje funkce, jako je jejich příchodu student, postupu vytvoření a přiřazení lektorem.</span><span class="sxs-lookup"><span data-stu-id="0bda3-110">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="0bda3-111">Toto je první ze série kurzů, které vysvětlují, jak k vytvoření ukázkové aplikace Contoso univerzity od začátku.</span><span class="sxs-lookup"><span data-stu-id="0bda3-111">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

[<span data-ttu-id="0bda3-112">Stažení nebo zobrazení hotová aplikace.</span><span class="sxs-lookup"><span data-stu-id="0bda3-112">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

<span data-ttu-id="0bda3-113">Základní EF 2.0 je nejnovější verzi EF, ale ještě nemá všechny funkce EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="0bda3-113">EF Core 2.0 is the latest version of EF but does not yet have all the features of EF 6.x.</span></span> <span data-ttu-id="0bda3-114">Informace o tom, jak zvolit EF 6.x a EF Core, najdete v části [EF základní vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/).</span><span class="sxs-lookup"><span data-stu-id="0bda3-114">For information about how to choose between EF 6.x and EF Core, see [EF Core vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/).</span></span> <span data-ttu-id="0bda3-115">Pokud se rozhodnete EF 6.x, najdete v části [předchozí verzi tento kurz řady](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="0bda3-115">If you choose EF 6.x, see [the previous version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

> [!NOTE]
> * <span data-ttu-id="0bda3-116">Verze 1.1 jádro ASP.NET v tomto kurzu, najdete v článku [VS 2017 Update 2 verzi v tomto kurzu ve formátu PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="0bda3-116">For the ASP.NET Core 1.1 version of this tutorial, see the [VS 2017 Update 2 version of this tutorial in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).</span></span>
> * <span data-ttu-id="0bda3-117">Verze sady Visual Studio 2015 v tomto kurzu, najdete v článku [VS 2015 verzi ASP.NET Core dokumentaci ve formátu PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span><span class="sxs-lookup"><span data-stu-id="0bda3-117">For the Visual Studio 2015 version of this tutorial, see the [VS 2015 version of ASP.NET Core documentation in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0bda3-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0bda3-118">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a><span data-ttu-id="0bda3-119">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="0bda3-119">Troubleshooting</span></span>

<span data-ttu-id="0bda3-120">Pokud narazíte na problém nevyřešíte, obvykle můžete najít řešení tak, že porovnáte svůj kód [dokončený projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="0bda3-120">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="0bda3-121">Seznam běžných chyb a jak je vyřešit, najdete v části [části Poradce při potížích s poslední kurz v této sérii](advanced.md#common-errors).</span><span class="sxs-lookup"><span data-stu-id="0bda3-121">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="0bda3-122">Pokud se nepodařilo najít, co potřebujete existuje, můžete odeslat dotaz na StackOverflow.com pro [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) nebo [EF základní](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="0bda3-122">If you don't find what you need there, you can post a question to StackOverflow.com for  [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP] 
> <span data-ttu-id="0bda3-123">Toto je řadu 10 kurzy, z nichž každý je založený na co se provádí v dřívější kurzy.</span><span class="sxs-lookup"><span data-stu-id="0bda3-123">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span>  <span data-ttu-id="0bda3-124">Zvažte možnost uložení kopie projektu po každém úspěšném dokončení kurzu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-124">Consider saving a copy of the project after each successful tutorial completion.</span></span>  <span data-ttu-id="0bda3-125">Pak pokud narazíte na problémy, můžete spustit přes z předchozí kurzu místo přejdete zpět na začátek celé řady.</span><span class="sxs-lookup"><span data-stu-id="0bda3-125">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="the-contoso-university-web-application"></a><span data-ttu-id="0bda3-126">Contoso univerzity webové aplikace</span><span class="sxs-lookup"><span data-stu-id="0bda3-126">The Contoso University web application</span></span>

<span data-ttu-id="0bda3-127">Aplikace, kterou jste budete vytvářet v těchto kurzech je jednoduchý univerzity webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="0bda3-127">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="0bda3-128">Uživatelé mohou zobrazit a aktualizovat studenty, během a lektorem informace.</span><span class="sxs-lookup"><span data-stu-id="0bda3-128">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="0bda3-129">Zde najdete několik z obrazovky, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="0bda3-129">Here are a few of the screens you'll create.</span></span>

![Studenti, kteří indexovou stránku](intro/_static/students-index.png)

![Stránka upravit studenty](intro/_static/student-edit.png)

<span data-ttu-id="0bda3-132">Styl uživatelského rozhraní tohoto webu byla choval blízko co je generován integrované šablony tak, aby tento kurz můžete zaměřit především na tom, jak používat rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0bda3-132">The UI style of this site has been kept close to what's generated by the built-in templates, so that the tutorial can focus mainly on how to use the Entity Framework.</span></span>

## <a name="create-an-aspnet-core-mvc-web-application"></a><span data-ttu-id="0bda3-133">Vytvořit webovou aplikaci ASP.NET MVC jádra</span><span class="sxs-lookup"><span data-stu-id="0bda3-133">Create an ASP.NET Core MVC web application</span></span>

<span data-ttu-id="0bda3-134">Otevřete Visual Studio a vytvořte nové ASP.NET Core C# webového projektu s názvem "ContosoUniversity".</span><span class="sxs-lookup"><span data-stu-id="0bda3-134">Open Visual Studio and create a new ASP.NET Core C# web project named "ContosoUniversity".</span></span>

* <span data-ttu-id="0bda3-135">Z **soubor** nabídce vyberte možnost **nový > projekt**.</span><span class="sxs-lookup"><span data-stu-id="0bda3-135">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="0bda3-136">V levém podokně vyberte **nainstalovaná > Visual C# > Web**.</span><span class="sxs-lookup"><span data-stu-id="0bda3-136">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="0bda3-137">Vyberte **webové aplikace ASP.NET Core** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-137">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="0bda3-138">Zadejte **ContosoUniversity** jako název a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="0bda3-138">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![Dialogové okno Nový projekt](intro/_static/new-project.png)

* <span data-ttu-id="0bda3-140">Počkejte **nové základní webové aplikace ASP.NET (.NET Core)** zobrazit dialogové okno</span><span class="sxs-lookup"><span data-stu-id="0bda3-140">Wait for the **New ASP.NET Core Web Application (.NET Core)** dialog to appear</span></span>

* <span data-ttu-id="0bda3-141">Vyberte **technologii ASP.NET 2.0 základní** a **webové aplikace (Model-View-Controller)** šablony.</span><span class="sxs-lookup"><span data-stu-id="0bda3-141">Select **ASP.NET Core 2.0** and the **Web Application (Model-View-Controller)** template.</span></span>

  <span data-ttu-id="0bda3-142">**Poznámka:** tento kurz vyžaduje aplikaci ASP.NET 2.0 jádra a základní EF 2.0 nebo novější – Ujistěte se, že **ASP.NET Core 1.1** není vybraná.</span><span class="sxs-lookup"><span data-stu-id="0bda3-142">**Note:** This tutorial requires ASP.NET Core 2.0 and EF Core 2.0 or later -- make sure that **ASP.NET Core 1.1** is not selected.</span></span>

* <span data-ttu-id="0bda3-143">Zajistěte, aby **ověřování** je nastaven na **bez ověřování**.</span><span class="sxs-lookup"><span data-stu-id="0bda3-143">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="0bda3-144">Klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="0bda3-144">Click **OK**</span></span>

  ![Dialogové okno Nový projekt ASP.NET](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="0bda3-146">Nastavení stylu lokality</span><span class="sxs-lookup"><span data-stu-id="0bda3-146">Set up the site style</span></span>

<span data-ttu-id="0bda3-147">Pár jednoduchými změnami nastaví lokality nabídky, rozložení a domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="0bda3-147">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="0bda3-148">Otevřete *Views/Shared/_Layout.cshtml* a proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="0bda3-148">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="0bda3-149">Změňte všechny výskyty "ContosoUniversity" na "Univerzity Contoso".</span><span class="sxs-lookup"><span data-stu-id="0bda3-149">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="0bda3-150">Existují tři události.</span><span class="sxs-lookup"><span data-stu-id="0bda3-150">There are three occurrences.</span></span>

* <span data-ttu-id="0bda3-151">Přidání položek nabídky pro **studenty**, **kurzy**, **vyučující**, a **oddělení**a odstranit **kontaktujte** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="0bda3-151">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="0bda3-152">Změny se zvýrazněnou.</span><span class="sxs-lookup"><span data-stu-id="0bda3-152">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

<span data-ttu-id="0bda3-153">V *Views/Home/Index.cshtml*, nahraďte obsah souboru následující kód, který nahradí text o ASP.NET a MVC o této aplikaci:</span><span class="sxs-lookup"><span data-stu-id="0bda3-153">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="0bda3-154">Stiskněte klávesu CTRL + F5 a spusťte projekt nebo zvolte **ladění > Spustit bez ladění** z nabídky.</span><span class="sxs-lookup"><span data-stu-id="0bda3-154">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="0bda3-155">Zobrazí na domovské stránce pro stránky, které vytvoříte v těchto kurzech karty.</span><span class="sxs-lookup"><span data-stu-id="0bda3-155">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Contoso univerzity domovské stránky](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a><span data-ttu-id="0bda3-157">Entity Framework Core NuGet balíčky</span><span class="sxs-lookup"><span data-stu-id="0bda3-157">Entity Framework Core NuGet packages</span></span>

<span data-ttu-id="0bda3-158">Podpora jádra EF přidat do projektu, nainstalujte zprostředkovatele databáze, kterou chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="0bda3-158">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="0bda3-159">Tento kurz používá systém SQL Server a poskytovatele balíček je [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="0bda3-159">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="0bda3-160">Tento balíček je součástí [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, takže není nutné ji nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="0bda3-160">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="0bda3-161">Tento balíček a jeho závislosti (`Microsoft.EntityFrameworkCore` a `Microsoft.EntityFrameworkCore.Relational`) poskytují podporu runtime pro EF.</span><span class="sxs-lookup"><span data-stu-id="0bda3-161">This package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="0bda3-162">Bude potřeba přidat balíček nástrojů dál v [migrace](migrations.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-162">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span> 

<span data-ttu-id="0bda3-163">Informace o dalších zprostředkovatelů databáze, které jsou k dispozici pro Entity Framework Core najdete v tématu [databáze zprostředkovatelé](https://docs.microsoft.com/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="0bda3-163">For information about other database providers that are available for Entity Framework Core, see [Database providers](https://docs.microsoft.com/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="0bda3-164">Vytvoření datového modelu</span><span class="sxs-lookup"><span data-stu-id="0bda3-164">Create the data model</span></span>

<span data-ttu-id="0bda3-165">Dále vytvoříte tříd entit pro danou aplikaci univerzity Contoso.</span><span class="sxs-lookup"><span data-stu-id="0bda3-165">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="0bda3-166">Budete začínat následující tři entity.</span><span class="sxs-lookup"><span data-stu-id="0bda3-166">You'll start with the following three entities.</span></span>

![Diagram modelu dat během. registrace studenty](intro/_static/data-model-diagram.png)

<span data-ttu-id="0bda3-168">Je vztah jeden mnoho mezi `Student` a `Enrollment` entity, a je mezi vztah jeden mnoho `Course` a `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="0bda3-168">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="0bda3-169">Jinými slovy student může být zaregistrované v libovolný počet kurzy a kurzu může mít libovolný počet studenti, kteří jsou v ní zaregistrované.</span><span class="sxs-lookup"><span data-stu-id="0bda3-169">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="0bda3-170">V následujících částech vytvoříte třídu pro každé z nich o těchto entitách.</span><span class="sxs-lookup"><span data-stu-id="0bda3-170">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="0bda3-171">Student entity</span><span class="sxs-lookup"><span data-stu-id="0bda3-171">The Student entity</span></span>

![Student entity diagram](intro/_static/student-entity.png)

<span data-ttu-id="0bda3-173">V *modely* složky, vytvořte soubor třídy s názvem *Student.cs* a nahraďte kód šablony s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="0bda3-173">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="0bda3-174">`ID` Vlastnost bude sloupec primárního klíče tabulky databáze, která odpovídá této třídy.</span><span class="sxs-lookup"><span data-stu-id="0bda3-174">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="0bda3-175">Ve výchozím nastavení, rozhraní Entity Framework interpretuje vlastnost s názvem `ID` nebo `classnameID` jako primární klíč.</span><span class="sxs-lookup"><span data-stu-id="0bda3-175">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="0bda3-176">`Enrollments` Je navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0bda3-176">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="0bda3-177">Navigační vlastnosti podržte dalšími subjekty, které se vztahují k této entity.</span><span class="sxs-lookup"><span data-stu-id="0bda3-177">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="0bda3-178">V takovém případě `Enrollments` vlastnost `Student entity` bude obsahovat všechny `Enrollment` entit, které se vztahují k které `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="0bda3-178">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="0bda3-179">Jinými slovy, pokud daný řádek Student v databázi existují dvě související registrace řádky (řádky, které obsahují hodnotu primárního klíče tohoto Studentova v jejich StudentID sloupec cizího klíče), který `Student` entity `Enrollments` navigační vlastnost těch, které bude obsahovat dva `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="0bda3-179">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="0bda3-180">Pokud vlastnost navigace mohou být uloženy více entit (jako relace m: n nebo na více), jeho typ musí být seznam, ve kterém položky možné ji přidat, odstranit nebo aktualizovat, například `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="0bda3-180">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span>  <span data-ttu-id="0bda3-181">Můžete zadat `ICollection<T>` , nebo typu, jako `List<T>` nebo `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="0bda3-181">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="0bda3-182">Pokud zadáte `ICollection<T>`, vytvoří EF `HashSet<T>` kolekce ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="0bda3-182">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="0bda3-183">Registrace entity</span><span class="sxs-lookup"><span data-stu-id="0bda3-183">The Enrollment entity</span></span>

![Diagram entity registrace](intro/_static/enrollment-entity.png)

<span data-ttu-id="0bda3-185">V *modely* složku vytvořit *Enrollment.cs* a existujícího kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="0bda3-185">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="0bda3-186">`EnrollmentID` Primární klíč, bude mít vlastnost; používá tuto entitu `classnameID` vzor místo `ID` samostatně jako jste viděli v `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="0bda3-186">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="0bda3-187">By normálně zvolte jeden vzor a použít ho v rámci datového modelu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-187">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="0bda3-188">Zde variaci znázorňuje, které můžete použít buď vzor.</span><span class="sxs-lookup"><span data-stu-id="0bda3-188">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="0bda3-189">V [novější kurzu](inheritance.md), uvidíte, jak pomocí ID bez classname usnadňuje implementaci dědičnosti v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-189">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="0bda3-190">`Grade` Vlastnost je `enum`.</span><span class="sxs-lookup"><span data-stu-id="0bda3-190">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="0bda3-191">Otazník po `Grade` deklaraci typu znamená, že `Grade` vlastnost umožňuje hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="0bda3-191">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="0bda3-192">Třída, která má hodnotu null se liší od nulové úrovni – null znamená úrovni není známý nebo ještě nebyly přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="0bda3-192">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="0bda3-193">`StudentID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Student`.</span><span class="sxs-lookup"><span data-stu-id="0bda3-193">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="0bda3-194">`Enrollment` Entita je spojen s jednou `Student` entit, tak vlastnost mohou obsahovat pouze jeden `Student` entity (na rozdíl od `Student.Enrollments` navigační vlastnost jste viděli dříve, která může pojmout více `Enrollment` entity).</span><span class="sxs-lookup"><span data-stu-id="0bda3-194">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="0bda3-195">`CourseID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Course`.</span><span class="sxs-lookup"><span data-stu-id="0bda3-195">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="0bda3-196">`Enrollment` Entita je spojen s jednou `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="0bda3-196">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="0bda3-197">Rozhraní Entity Framework interpretuje vlastnost jako vlastnost cizího klíče, pokud je název `<navigation property name><primary key property name>` (například `StudentID` pro `Student` navigační vlastnost, protože `Student` je primární klíč entity `ID`).</span><span class="sxs-lookup"><span data-stu-id="0bda3-197">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="0bda3-198">Taky vlastnosti cizího klíče jednoduše název `<primary key property name>` (například `CourseID` vzhledem k tomu `Course` je primární klíč entity `CourseID`).</span><span class="sxs-lookup"><span data-stu-id="0bda3-198">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="0bda3-199">Během entity</span><span class="sxs-lookup"><span data-stu-id="0bda3-199">The Course entity</span></span>

![Diagram postupu entity](intro/_static/course-entity.png)

<span data-ttu-id="0bda3-201">V *modely* složku vytvořit *Course.cs* a existujícího kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="0bda3-201">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="0bda3-202">`Enrollments` Je navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0bda3-202">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="0bda3-203">A `Course` entity může souviset s libovolný počet `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="0bda3-203">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="0bda3-204">Budete říkáme, informace o `DatabaseGenerated` atribut [novější kurzu](complex-data-model.md) této série.</span><span class="sxs-lookup"><span data-stu-id="0bda3-204">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="0bda3-205">V zásadě platí tento atribut slouží k zadání primární klíč pro během, místo aby databázi jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="0bda3-205">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="0bda3-206">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="0bda3-206">Create the Database Context</span></span>

<span data-ttu-id="0bda3-207">Hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model je třídy kontextu databáze.</span><span class="sxs-lookup"><span data-stu-id="0bda3-207">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="0bda3-208">Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="0bda3-208">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="0bda3-209">V kódu zadáte entit, které jsou zahrnuty v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-209">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="0bda3-210">Můžete také upravit chování určité Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0bda3-210">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="0bda3-211">V tomto projektu je třída s názvem `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="0bda3-211">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="0bda3-212">Ve složce projektu vytvořte složku s názvem *Data*.</span><span class="sxs-lookup"><span data-stu-id="0bda3-212">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="0bda3-213">V *Data* složce vytvořte nový soubor třídy s názvem *SchoolContext.cs*a kód šablony nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="0bda3-213">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="0bda3-214">Tento kód vytvoří `DbSet` vlastnosti pro každou sadu entit.</span><span class="sxs-lookup"><span data-stu-id="0bda3-214">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="0bda3-215">V terminologii rozhraní Entity Framework obvykle sadu entit, odpovídá do databázové tabulky a entity odpovídá na řádek v tabulce.</span><span class="sxs-lookup"><span data-stu-id="0bda3-215">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="0bda3-216">Vám může mít tento parametr vynechán `DbSet<Enrollment>` a `DbSet<Course>` příkazy a bude fungovat stejně.</span><span class="sxs-lookup"><span data-stu-id="0bda3-216">You could have omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="0bda3-217">Rozhraní Entity Framework by mělo zahrnovat je implicitně protože `Student` odkazy na entity `Enrollment` entity a `Enrollment` odkazy na entity `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="0bda3-217">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="0bda3-218">Při vytvoření databáze EF vytvoří tabulek, které jsou stejné jako názvy `DbSet` názvy vlastností.</span><span class="sxs-lookup"><span data-stu-id="0bda3-218">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="0bda3-219">Názvy vlastností pro kolekce jsou obvykle množném čísle (studenty spíše než Student), ale vývojáři Nesouhlasím o tom, jestli by měl názvy tabulek pluralized nebo ne.</span><span class="sxs-lookup"><span data-stu-id="0bda3-219">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="0bda3-220">Tyto kurzy přepíšete výchozí chování zadáním názvů singulární tabulek v DbContext.</span><span class="sxs-lookup"><span data-stu-id="0bda3-220">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="0bda3-221">K tomu, přidejte následující zvýrazněný kód po poslední DbSet vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0bda3-221">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="0bda3-222">Zaregistrovat kontext vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="0bda3-222">Register the context with dependency injection</span></span>

<span data-ttu-id="0bda3-223">Implementuje ASP.NET Core [vkládání závislostí](../../fundamentals/dependency-injection.md) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="0bda3-223">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="0bda3-224">Služby (například kontext databáze EF) jsou registrovány vkládání závislostí při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="0bda3-224">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="0bda3-225">Součásti, které vyžadují tyto služby (například řadiče MVC) jsou k dispozici tyto služby prostřednictvím konstruktor parametry.</span><span class="sxs-lookup"><span data-stu-id="0bda3-225">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="0bda3-226">Zobrazí kód řadiče konstruktor, který získá instance kontextu později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-226">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="0bda3-227">K registraci `SchoolContext` jako služby, otevřete *Startup.cs*a přidejte zvýrazněné řádky, které se `ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="0bda3-227">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="0bda3-228">Název připojovacího řetězce, je předaná do kontextu voláním metody na `DbContextOptionsBuilder` objektu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-228">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="0bda3-229">Pro místní vývoj [ASP.NET Core konfigurační systém](xref:fundamentals/configuration/index) čte připojovací řetězec z *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="0bda3-229">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="0bda3-230">Přidat `using` příkazy pro `ContosoUniversity.Data` a `Microsoft.EntityFrameworkCore` obory názvů a potom sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="0bda3-230">Add `using` statements for `ContosoUniversity.Data`  and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="0bda3-231">Otevřete *appSettings.JSON určený* souboru a přidat připojovací řetězec, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-231">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="0bda3-232">Databáze SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="0bda3-232">SQL Server Express LocalDB</span></span>

<span data-ttu-id="0bda3-233">Připojovací řetězec Určuje databázi SQL serveru LocalDB.</span><span class="sxs-lookup"><span data-stu-id="0bda3-233">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="0bda3-234">LocalDB je Odlehčená verze SQL Server Express Database Engine a je určena pro vývoj aplikací, není použití v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0bda3-234">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="0bda3-235">LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není žádná komplexní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0bda3-235">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="0bda3-236">Ve výchozím nastavení, vytvoří instanci LocalDB *.mdf* soubory v databáze `C:/Users/<user>` adresáře.</span><span class="sxs-lookup"><span data-stu-id="0bda3-236">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-database-with-test-data"></a><span data-ttu-id="0bda3-237">Přidat kód pro inicializaci databáze s testovací data</span><span class="sxs-lookup"><span data-stu-id="0bda3-237">Add code to initialize the database with test data</span></span>

<span data-ttu-id="0bda3-238">Rozhraní Entity Framework pro vytvoření prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="0bda3-238">The Entity Framework will create an empty database for you.</span></span>  <span data-ttu-id="0bda3-239">V této části napíšete metodu, která je volána po vytvoření databáze k naplnění testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="0bda3-239">In this section, you write a method that is called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="0bda3-240">Tady budete používat `EnsureCreated` metoda automaticky vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="0bda3-240">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="0bda3-241">V [novější kurzu](migrations.md) uvidíte, jak zpracovat změny modelu s použitím migrace Code First, chcete-li změnit schéma databáze místo vyřadit a znovu vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="0bda3-241">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="0bda3-242">V *Data* složky, vytvořte nový soubor třídy s názvem *DbInitializer.cs* a nahraďte následujícím kódem, což způsobí, že databáze, který se má vytvořit v případě potřeby kód šablony a zatížením testovacích dat do nové databáze.</span><span class="sxs-lookup"><span data-stu-id="0bda3-242">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="0bda3-243">Kód ověří, zda existují jakékoli studenty v databázi, a pokud ne, předpokládá, databáze je nový a musí být vyplněn testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="0bda3-243">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span>  <span data-ttu-id="0bda3-244">Načte testovací data do pole místo `List<T>` kolekce za účelem optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-244">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="0bda3-245">V *Program.cs*, změnit `Main` metodu na spuštění aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="0bda3-245">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="0bda3-246">Instance kontextu databáze získáte z kontejneru pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="0bda3-246">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="0bda3-247">Volejte metodu počáteční hodnoty, předání kontextu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-247">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="0bda3-248">Uvolnění kontextu, pokud se provádí metodu počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0bda3-248">Dispose the context when the seed method is done.</span></span>

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="0bda3-249">Přidat `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="0bda3-249">Add `using` statements:</span></span>

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="0bda3-250">V starší kurzy, se může zobrazit podobné kód `Configure` metoda v *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0bda3-250">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="0bda3-251">Doporučujeme vám, že používáte `Configure` metoda jenom chcete nastavit kanál požadavku.</span><span class="sxs-lookup"><span data-stu-id="0bda3-251">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="0bda3-252">Kód spuštění aplikace patří `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="0bda3-252">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="0bda3-253">Nyní při prvním spuštění aplikace, databáze bude vytvořen a osadit testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="0bda3-253">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="0bda3-254">Při každé změně datového modelu, můžete odstranit databázi, aktualizovat metodu počáteční hodnoty a začít znovu s novou databázi stejným způsobem jako.</span><span class="sxs-lookup"><span data-stu-id="0bda3-254">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="0bda3-255">V dalších kurzech uvidíte, jak k úpravám databáze, když datový model změny, bez odstranit a znovu ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="0bda3-255">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="0bda3-256">Vytvoření kontroleru a zobrazení</span><span class="sxs-lookup"><span data-stu-id="0bda3-256">Create a controller and views</span></span>

<span data-ttu-id="0bda3-257">V dalším kroku použijete modul generování uživatelského rozhraní v sadě Visual Studio přidat řadič MVC a zobrazení, které budou používat EF pro dotazování a uložit data.</span><span class="sxs-lookup"><span data-stu-id="0bda3-257">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="0bda3-258">Automatické vytváření metody akce CRUD a zobrazení se označuje jako generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0bda3-258">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="0bda3-259">Generování uživatelského rozhraní se liší od generování kódu, jsou automaticky generovaný kód počáteční bod, který můžete upravit tak, aby vyhovovala vaším požadavkům, zatímco obvykle Neupravovat generovaného kódu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-259">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="0bda3-260">Pokud budete potřebovat k přizpůsobení generovaného kódu, použít částečné třídy nebo při změně věcí znovu vygenerovat kód.</span><span class="sxs-lookup"><span data-stu-id="0bda3-260">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="0bda3-261">Klikněte pravým tlačítkem myši **řadiče** složky v **Průzkumníku řešení** a vyberte **Přidat > novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="0bda3-261">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

<span data-ttu-id="0bda3-262">Pokud **přidat závislosti MVC** otevře se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="0bda3-262">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="0bda3-263">[Aktualizovat na nejnovější verzi sady Visual Studio](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="0bda3-263">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="0bda3-264">Visual Studio verze starší než 15,5 zobrazit tento dialog.</span><span class="sxs-lookup"><span data-stu-id="0bda3-264">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="0bda3-265">Pokud nelze aktualizovat, vyberte **přidat**a pak postupujte podle kroků řadiče přidat.</span><span class="sxs-lookup"><span data-stu-id="0bda3-265">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

* <span data-ttu-id="0bda3-266">V **přidat vygenerované uživatelské rozhraní** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="0bda3-266">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="0bda3-267">Vyberte **kontroler MVC s zobrazeními, s využitím nástroje Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="0bda3-267">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="0bda3-268">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0bda3-268">Click **Add**.</span></span>

* <span data-ttu-id="0bda3-269">V **přidat kontroler** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="0bda3-269">In the **Add Controller** dialog box:</span></span>

  * <span data-ttu-id="0bda3-270">V **třída modelu** vyberte **Student**.</span><span class="sxs-lookup"><span data-stu-id="0bda3-270">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="0bda3-271">V **třída kontextu dat** vyberte **SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="0bda3-271">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="0bda3-272">Přijměte výchozí nastavení **StudentsController** jako název.</span><span class="sxs-lookup"><span data-stu-id="0bda3-272">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="0bda3-273">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0bda3-273">Click **Add**.</span></span>

  ![Student vygenerované uživatelské rozhraní](intro/_static/scaffold-student.png)

  <span data-ttu-id="0bda3-275">Když kliknete na tlačítko **přidat**, modul generování uživatelského rozhraní sady Visual Studio vytvoří *StudentsController.cs* souboru a sadu zobrazení (*.cshtml* soubory), pracovat s řadičem.</span><span class="sxs-lookup"><span data-stu-id="0bda3-275">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="0bda3-276">(Generování uživatelského rozhraní modulu také můžete vytvořit kontext databáze pro vás Pokud nemáte vytvořte ručně nejprve, jako jste to udělali dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-276">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="0bda3-277">Můžete zadat novou třídu v kontextu **přidat kontroler** kliknutím na symbol plus napravo od pole **třída kontextu dat**.</span><span class="sxs-lookup"><span data-stu-id="0bda3-277">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="0bda3-278">Potom vytvoří sada Visual Studio vaší `DbContext` třídy a také řadiče a zobrazení.)</span><span class="sxs-lookup"><span data-stu-id="0bda3-278">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="0bda3-279">Můžete si všimnout, že trvá kontroleru `SchoolContext` jako parametr konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="0bda3-279">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="0bda3-280">Vkládání závislostí ASP.NET se postará o předání instanci `SchoolContext` do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0bda3-280">ASP.NET dependency injection will take care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="0bda3-281">Jste nakonfigurovali, že *Startup.cs* výše.</span><span class="sxs-lookup"><span data-stu-id="0bda3-281">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="0bda3-282">Obsahuje kontroler `Index` metody akce, která zobrazuje všechny studenty v databázi.</span><span class="sxs-lookup"><span data-stu-id="0bda3-282">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="0bda3-283">Metoda získá seznam studenty ze sady načtením studenty entit `Students` vlastností kontextu instance databáze:</span><span class="sxs-lookup"><span data-stu-id="0bda3-283">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="0bda3-284">Později v tomto kurzu získáte informace o asynchronní elementům programování v tomto kódu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-284">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="0bda3-285">*Views/Students/Index.cshtml* zobrazení zobrazí tento seznam v tabulce:</span><span class="sxs-lookup"><span data-stu-id="0bda3-285">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="0bda3-286">Stiskněte klávesu CTRL + F5 a spusťte projekt nebo zvolte **ladění > Spustit bez ladění** z nabídky.</span><span class="sxs-lookup"><span data-stu-id="0bda3-286">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="0bda3-287">Klikněte na kartu studenty zobrazíte testovací data, `DbInitializer.Initialize` metoda vložit.</span><span class="sxs-lookup"><span data-stu-id="0bda3-287">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="0bda3-288">V závislosti na tom, jak úzké okno prohlížeče se zobrazí `Student` odkaz karty v horní části stránky nebo budete muset klikněte na ikonu Navigace v pravém horním rohu na odkaz zobrazíte.</span><span class="sxs-lookup"><span data-stu-id="0bda3-288">Depending on how narrow your browser window is, you'll see the `Student` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Domovská stránka Contoso univerzity úzké](intro/_static/home-page-narrow.png)

![Studenti, kteří indexovou stránku](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="0bda3-291">Zobrazení databáze</span><span class="sxs-lookup"><span data-stu-id="0bda3-291">View the Database</span></span>

<span data-ttu-id="0bda3-292">Při spuštění aplikace, `DbInitializer.Initialize` volání metod `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="0bda3-292">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="0bda3-293">EF viděli, že se žádná databáze a proto vytvoří jednu pak zbytek `Initialize` metoda kód naplní databázi s daty.</span><span class="sxs-lookup"><span data-stu-id="0bda3-293">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="0bda3-294">Můžete použít **Průzkumník objektů systému SQL Server** (SSOX) Chcete-li zobrazit databázi v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0bda3-294">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="0bda3-295">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="0bda3-295">Close the browser.</span></span>

<span data-ttu-id="0bda3-296">Pokud už není otevřené okno SSOX, vyberte ho v **zobrazení** nabídky v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0bda3-296">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="0bda3-297">V SSOX, klikněte na **\MSSQLLocalDB (localdb) > databáze**a pak klikněte na položku pro název databáze, který je v připojovacím řetězci v vaše *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="0bda3-297">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that is in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="0bda3-298">Rozbalte **tabulky** uzlu zobrazíte tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="0bda3-298">Expand the **Tables** node to see the tables in your database.</span></span>

![Tabulky v SSOX](intro/_static/ssox-tables.png)

<span data-ttu-id="0bda3-300">Klikněte pravým tlačítkem myši **Student** tabulky a klikněte na tlačítko **Data zobrazení** zobrazíte sloupce, které byly vytvořeny a řádky, které byly vloženy do tabulky.</span><span class="sxs-lookup"><span data-stu-id="0bda3-300">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![Tabulka Student v SSOX](intro/_static/ssox-student-table.png)

<span data-ttu-id="0bda3-302">*.Mdf* a *.ldf* databázové soubory jsou v *C:\Users\<uživatelské_jméno >* složky.</span><span class="sxs-lookup"><span data-stu-id="0bda3-302">The *.mdf* and *.ldf* database files are in the *C:\Users\<yourusername>* folder.</span></span>

<span data-ttu-id="0bda3-303">Protože jste volání `EnsureCreated` v metodě inicializátoru, který spouští při spuštění aplikace, můžete dokonce vytvářet teď ke změně `Student` třídy, odstraňte tuto databázi, spusťte aplikaci znovu a databáze bude automaticky znovu vytvořit tak, aby odpovídaly vaší změn.</span><span class="sxs-lookup"><span data-stu-id="0bda3-303">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="0bda3-304">Například, pokud přidáte `EmailAddress` vlastnost, která má `Student` třídy, se zobrazí nový `EmailAddress` sloupec v tabulce znovu vytvořena.</span><span class="sxs-lookup"><span data-stu-id="0bda3-304">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="0bda3-305">Konvence</span><span class="sxs-lookup"><span data-stu-id="0bda3-305">Conventions</span></span>

<span data-ttu-id="0bda3-306">Kvůli použití konvence nebo předpoklady, které umožňuje Entity Framework je minimální množství kód, který jste měli k zápisu, aby rozhraní Entity Framework, abyste mohli vytvořit databázi dokončení pro vás.</span><span class="sxs-lookup"><span data-stu-id="0bda3-306">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="0bda3-307">Názvy `DbSet` vlastnosti jsou použity jako názvy tabulek.</span><span class="sxs-lookup"><span data-stu-id="0bda3-307">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="0bda3-308">Pro entity není odkazuje `DbSet` vlastnost třídy entita názvy jsou použity jako názvy tabulek.</span><span class="sxs-lookup"><span data-stu-id="0bda3-308">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="0bda3-309">Názvy vlastností entity se používají pro názvy sloupců.</span><span class="sxs-lookup"><span data-stu-id="0bda3-309">Entity property names are used for column names.</span></span>

* <span data-ttu-id="0bda3-310">Vlastnosti entity, které jsou s názvem ID nebo classnameID jsou rozpoznat jako vlastnosti primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="0bda3-310">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="0bda3-311">Vlastnost interpretována jako vlastností cizího klíče, pokud je název  *<navigation property name> <primary key property name>*  (například `StudentID` pro `Student` navigační vlastnost, protože `Student` je primární klíč entity `ID`).</span><span class="sxs-lookup"><span data-stu-id="0bda3-311">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="0bda3-312">Taky vlastnosti cizího klíče jednoduše název  *<primary key property name>*  (například `EnrollmentID` vzhledem k tomu `Enrollment` je primární klíč entity `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="0bda3-312">Foreign key properties can also be named simply *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="0bda3-313">Konvenční chování lze přepsat.</span><span class="sxs-lookup"><span data-stu-id="0bda3-313">Conventional behavior can be overridden.</span></span> <span data-ttu-id="0bda3-314">Například můžete explicitně zadáte názvy tabulek, jako jste viděli dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-314">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="0bda3-315">A můžete nastavit názvy sloupců a nastavte libovolnou vlastnost jako primární klíč, cizí klíč, nebo jak uvidíte v [novější kurzu](complex-data-model.md) této série.</span><span class="sxs-lookup"><span data-stu-id="0bda3-315">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="0bda3-316">Asynchronní kódu</span><span class="sxs-lookup"><span data-stu-id="0bda3-316">Asynchronous code</span></span>

<span data-ttu-id="0bda3-317">Asynchronní programování je výchozí režim pro ASP.NET Core a EF jádra.</span><span class="sxs-lookup"><span data-stu-id="0bda3-317">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="0bda3-318">Webový server má omezený počet vláken, které jsou k dispozici, a v situacích, vysokého zatížení všech dostupných vláken může být používán.</span><span class="sxs-lookup"><span data-stu-id="0bda3-318">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="0bda3-319">Pokud k tomu dojde, server nemůže zpracovat nové žádosti o, dokud se uvolní vláken.</span><span class="sxs-lookup"><span data-stu-id="0bda3-319">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="0bda3-320">Synchronní kód může být vázáno mnoho vláken při jejich nejsou to skutečně veškerou práci, protože se čeká vstupně-výstupních operací na dokončení.</span><span class="sxs-lookup"><span data-stu-id="0bda3-320">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="0bda3-321">Asynchronní kód když proces čeká na vstupně-výstupních operací na dokončení, je jeho přístup z více vláken uvolněno pro server, aby používal pro zpracování dalších požadavků.</span><span class="sxs-lookup"><span data-stu-id="0bda3-321">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="0bda3-322">V důsledku toho asynchronní kódu umožňuje prostředky serveru použije efektivněji a serveru zapnutá, abyste zvládli větší provoz bez zpoždění.</span><span class="sxs-lookup"><span data-stu-id="0bda3-322">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="0bda3-323">Asynchronní kódu v době běhu zavést malé množství režijní náklady na, ale pro situace nízkým provozem, stiskněte klávesu výkonu je nepatrné při pro intenzivní provoz situace, je výrazně potenciální zvýšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="0bda3-323">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="0bda3-324">V následujícím kódu `async` – klíčové slovo, `Task<T>` vrátit hodnotu, `await` – klíčové slovo, a `ToListAsync` metoda zkontrolujte kód provést asynchronně.</span><span class="sxs-lookup"><span data-stu-id="0bda3-324">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="0bda3-325">`async` – Klíčové slovo říká kompilátoru generování zpětných volání pro části těla metody a automaticky vytvářet `Task<IActionResult>` objekt, který je vrácen.</span><span class="sxs-lookup"><span data-stu-id="0bda3-325">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that is returned.</span></span>

* <span data-ttu-id="0bda3-326">Návratový typ `Task<IActionResult>` reprezentuje probíhající práce s tím výsledkem typu `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="0bda3-326">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="0bda3-327">`await` – Klíčové slovo způsobí, že kompilátor metodu rozdělit na dvě části.</span><span class="sxs-lookup"><span data-stu-id="0bda3-327">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="0bda3-328">První část končí operace, které se spustí asynchronně.</span><span class="sxs-lookup"><span data-stu-id="0bda3-328">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="0bda3-329">Druhá část je uvést do metody zpětného volání, která je volána po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="0bda3-329">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="0bda3-330">`ToListAsync`je asynchronní verzi `ToList` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="0bda3-330">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="0bda3-331">Některé věci zajímat, když píšete asynchronní kód, který používá rozhraní Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="0bda3-331">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="0bda3-332">Pouze příkazy, které způsobily dotazy nebo příkazy k odeslání do databáze se spustí asynchronně.</span><span class="sxs-lookup"><span data-stu-id="0bda3-332">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="0bda3-333">Například zahrnující `ToListAsync`, `SingleOrDefaultAsync`, a `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="0bda3-333">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span>  <span data-ttu-id="0bda3-334">Neobsahuje, například příkazy, které právě změnit `IQueryable`, jako například `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="0bda3-334">It does not include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="0bda3-335">Kontextu EF není zaručeno bezpečné používání vláken: nemáte pokusí provést více operací paralelně.</span><span class="sxs-lookup"><span data-stu-id="0bda3-335">An EF context is not thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="0bda3-336">Při volání metody EF všechny asynchronní, vždy nutné použít `await` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="0bda3-336">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="0bda3-337">Pokud chcete využít výhod výkonu asynchronní kódu, ujistěte se, že všechny knihovny balíčky, které používáte (například konkrétní stránkování), také použít asynchronní, pokud volají žádné metody rozhraní Entity Framework, které způsobí dotazy k odeslání do databáze.</span><span class="sxs-lookup"><span data-stu-id="0bda3-337">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="0bda3-338">Další informace o asynchronní programování v rozhraní .NET najdete v tématu [přehled asynchronních](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="0bda3-338">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

## <a name="summary"></a><span data-ttu-id="0bda3-339">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0bda3-339">Summary</span></span>

<span data-ttu-id="0bda3-340">Nyní jste vytvořili jednoduchou aplikaci, která se používá k uložení a zobrazení data Entity Framework Core a SQL Server Express LocalDB.</span><span class="sxs-lookup"><span data-stu-id="0bda3-340">You've now created a simple application that uses the Entity Framework Core and SQL Server Express LocalDB to store and display data.</span></span> <span data-ttu-id="0bda3-341">V následujícím kurzu se dozvíte jak provádět základní CRUD (vytvořit, číst, aktualizovat, odstraňovat) operace.</span><span class="sxs-lookup"><span data-stu-id="0bda3-341">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="0bda3-342">Next</span><span class="sxs-lookup"><span data-stu-id="0bda3-342">Next</span></span>](crud.md)  
