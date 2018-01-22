---
title: "Přidání modelu do aplikace stránky Razor ve ASP.NET Core"
author: rick-anderson
description: "Přidání modelu do aplikace stránky Razor ve ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: 84e5ec27904b564fa6ee29843ceae0bb70754ea7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="5bb54-103">Přidání modelu do aplikace pro stránky Razor</span><span class="sxs-lookup"><span data-stu-id="5bb54-103">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="5bb54-104">Přidat datový model</span><span class="sxs-lookup"><span data-stu-id="5bb54-104">Add a data model</span></span>

<span data-ttu-id="5bb54-105">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="5bb54-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="5bb54-106">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="5bb54-106">Name the folder *Models*.</span></span>

<span data-ttu-id="5bb54-107">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="5bb54-107">Right click the *Models* folder.</span></span> <span data-ttu-id="5bb54-108">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="5bb54-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="5bb54-109">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="5bb54-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="5bb54-110">Přidat připojovací řetězec databáze</span><span class="sxs-lookup"><span data-stu-id="5bb54-110">Add a database connection string</span></span>

<span data-ttu-id="5bb54-111">Přidat připojovací řetězec k *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="5bb54-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="5bb54-112">Zaregistrovat kontext databáze</span><span class="sxs-lookup"><span data-stu-id="5bb54-112">Register the database context</span></span>

<span data-ttu-id="5bb54-113">Zaregistrovat kontext databáze s [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="5bb54-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="5bb54-114">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="5bb54-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="5bb54-115">Přidat vygenerované uživatelské rozhraní nástroje a provedení počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="5bb54-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="5bb54-116">V této části použijte konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="5bb54-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="5bb54-117">Přidejte balíček generování kódu webové sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5bb54-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="5bb54-118">Tento balíček je potřeba spustit modul generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5bb54-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="5bb54-119">Přidáte počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="5bb54-119">Add an initial migration.</span></span>
* <span data-ttu-id="5bb54-120">Aktualizace databáze s počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="5bb54-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="5bb54-121">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="5bb54-121">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="5bb54-123">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5bb54-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

<span data-ttu-id="5bb54-124">`Install-Package` Příkaz nainstaluje nástroje potřebné ke spuštění modulu generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5bb54-124">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="5bb54-125">`Add-Migration` Příkaz generuje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="5bb54-125">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="5bb54-126">Schéma je založena na zadaný ve model `DbContext` (v *Models/MovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="5bb54-126">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="5bb54-127">`Initial` Argument se používá k pojmenování byla migrace.</span><span class="sxs-lookup"><span data-stu-id="5bb54-127">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="5bb54-128">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="5bb54-128">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="5bb54-129">V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="5bb54-129">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="5bb54-130">`Update-Database` Příkaz spustí `Up` metoda v *migrace nebo\<časové razítko > _InitialCreate.cs* souboru, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="5bb54-130">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="5bb54-131">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="5bb54-131">Test the app</span></span>

* <span data-ttu-id="5bb54-132">Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="5bb54-132">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="5bb54-133">Testovací **vytvořit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="5bb54-133">Test the **Create** link.</span></span>

 ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="5bb54-135">Testovací **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="5bb54-135">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="5bb54-136">Pokud dojde k výjimce SQL, ověřte, jestli mají spuštění migrace a aktualizovat databázi:</span><span class="sxs-lookup"><span data-stu-id="5bb54-136">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="5bb54-137">Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5bb54-137">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5bb54-138">[Předchozí: Začínáme](xref:tutorials/razor-pages/razor-pages-start)
[Další: vygeneroval stránky Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="5bb54-138">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
