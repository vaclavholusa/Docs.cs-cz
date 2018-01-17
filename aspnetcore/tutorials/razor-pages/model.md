---
title: "Přidání modelu do aplikace stránky Razor ve ASP.NET Core"
author: rick-anderson
description: "Přidání modelu do aplikace stránky Razor ve ASP.NET Core"
keywords: "ASP.NET Core, stránky Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: d72c01971b1dbf26b96e438543fea037eda3a879
ms.sourcegitcommit: bc723b483182fbcbf8c4c7098f70443662076905
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="b9d68-104">Přidání modelu do aplikace pro stránky Razor</span><span class="sxs-lookup"><span data-stu-id="b9d68-104">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="b9d68-105">Přidat datový model</span><span class="sxs-lookup"><span data-stu-id="b9d68-105">Add a data model</span></span>

<span data-ttu-id="b9d68-106">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="b9d68-106">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="b9d68-107">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="b9d68-107">Name the folder *Models*.</span></span>

<span data-ttu-id="b9d68-108">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="b9d68-108">Right click the *Models* folder.</span></span> <span data-ttu-id="b9d68-109">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="b9d68-109">Select **Add** > **Class**.</span></span> <span data-ttu-id="b9d68-110">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="b9d68-110">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="b9d68-111">Přidat připojovací řetězec databáze</span><span class="sxs-lookup"><span data-stu-id="b9d68-111">Add a database connection string</span></span>

<span data-ttu-id="b9d68-112">Přidat připojovací řetězec k *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="b9d68-112">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="b9d68-113">Zaregistrovat kontext databáze</span><span class="sxs-lookup"><span data-stu-id="b9d68-113">Register the database context</span></span>

<span data-ttu-id="b9d68-114">Zaregistrovat kontext databáze s [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="b9d68-114">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="b9d68-115">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="b9d68-115">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="b9d68-116">Přidat vygenerované uživatelské rozhraní nástroje a provedení počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="b9d68-116">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="b9d68-117">V této části použijte konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="b9d68-117">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="b9d68-118">Přidejte balíček generování kódu webové sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b9d68-118">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="b9d68-119">Tento balíček je potřeba spustit modul generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b9d68-119">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="b9d68-120">Přidáte počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="b9d68-120">Add an initial migration.</span></span>
* <span data-ttu-id="b9d68-121">Aktualizace databáze s počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="b9d68-121">Update the database with the initial migration.</span></span>

<span data-ttu-id="b9d68-122">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="b9d68-122">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="b9d68-124">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="b9d68-124">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

<span data-ttu-id="b9d68-125">`Install-Package` Příkaz nainstaluje nástroje potřebné ke spuštění modulu generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b9d68-125">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="b9d68-126">`Add-Migration` Příkaz generuje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="b9d68-126">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="b9d68-127">Schéma je založena na zadaný ve model `DbContext` (v *Models/MovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="b9d68-127">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="b9d68-128">`Initial` Argument se používá k pojmenování byla migrace.</span><span class="sxs-lookup"><span data-stu-id="b9d68-128">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="b9d68-129">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="b9d68-129">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="b9d68-130">V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="b9d68-130">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="b9d68-131">`Update-Database` Příkaz spustí `Up` metoda v *migrace nebo\<časové razítko > _InitialCreate.cs* souboru, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="b9d68-131">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="b9d68-132">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="b9d68-132">Test the app</span></span>

* <span data-ttu-id="b9d68-133">Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="b9d68-133">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="b9d68-134">Testovací **vytvořit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="b9d68-134">Test the **Create** link.</span></span>

 ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="b9d68-136">Testovací **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="b9d68-136">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="b9d68-137">Pokud dojde k výjimce SQL, ověřte, jestli mají spuštění migrace a aktualizovat databázi:</span><span class="sxs-lookup"><span data-stu-id="b9d68-137">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="b9d68-138">Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b9d68-138">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b9d68-139">[Předchozí: Začínáme](xref:tutorials/razor-pages/razor-pages-start)
[Další: vygeneroval stránky Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="b9d68-139">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
