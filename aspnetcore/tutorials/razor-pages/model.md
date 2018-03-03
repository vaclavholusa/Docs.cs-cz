---
title: "Přidání modelu do aplikace stránky Razor ve ASP.NET Core"
author: rick-anderson
description: "Přidání modelu do aplikace stránky Razor ve ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 234af466979c8d8b14990da4d4a07e6e54649bbe
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="88a98-103">Přidání modelu do aplikace pro stránky Razor</span><span class="sxs-lookup"><span data-stu-id="88a98-103">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="88a98-104">Přidat datový model</span><span class="sxs-lookup"><span data-stu-id="88a98-104">Add a data model</span></span>

<span data-ttu-id="88a98-105">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="88a98-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="88a98-106">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="88a98-106">Name the folder *Models*.</span></span>

<span data-ttu-id="88a98-107">Klikněte pravým tlačítkem *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="88a98-107">Right click the *Models* folder.</span></span> <span data-ttu-id="88a98-108">Vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="88a98-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="88a98-109">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="88a98-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="88a98-110">Přidat připojovací řetězec databáze</span><span class="sxs-lookup"><span data-stu-id="88a98-110">Add a database connection string</span></span>

<span data-ttu-id="88a98-111">Přidat připojovací řetězec k *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="88a98-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="88a98-112">Zaregistrovat kontext databáze</span><span class="sxs-lookup"><span data-stu-id="88a98-112">Register the database context</span></span>

<span data-ttu-id="88a98-113">Zaregistrovat kontext databáze s [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="88a98-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="88a98-114">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="88a98-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="88a98-115">Přidat vygenerované uživatelské rozhraní nástroje a provedení počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="88a98-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="88a98-116">V této části použijte konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="88a98-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="88a98-117">Přidejte balíček generování kódu webové sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="88a98-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="88a98-118">Tento balíček je potřeba spustit modul generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="88a98-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="88a98-119">Přidáte počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="88a98-119">Add an initial migration.</span></span>
* <span data-ttu-id="88a98-120">Aktualizace databáze s počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="88a98-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="88a98-121">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="88a98-121">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="88a98-123">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="88a98-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

<span data-ttu-id="88a98-124">`Install-Package` Příkaz nainstaluje nástroje potřebné ke spuštění modulu generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="88a98-124">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="88a98-125">`Add-Migration` Příkaz generuje kód pro vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="88a98-125">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="88a98-126">Schéma je založena na zadaný ve model `DbContext` (v *Models/MovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="88a98-126">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="88a98-127">`Initial` Argument se používá k pojmenování byla migrace.</span><span class="sxs-lookup"><span data-stu-id="88a98-127">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="88a98-128">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="88a98-128">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="88a98-129">V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="88a98-129">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="88a98-130">`Update-Database` Příkaz spustí `Up` metoda v *migrace nebo\<časové razítko > _InitialCreate.cs* souboru, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="88a98-130">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="88a98-131">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="88a98-131">Test the app</span></span>

* <span data-ttu-id="88a98-132">Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="88a98-132">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="88a98-133">Testovací **vytvořit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="88a98-133">Test the **Create** link.</span></span>

 ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="88a98-135">Testovací **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="88a98-135">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="88a98-136">Pokud dojde k výjimce SQL, ověřte, jestli mají spuštění migrace a aktualizovat databázi:</span><span class="sxs-lookup"><span data-stu-id="88a98-136">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="88a98-137">Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="88a98-137">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="88a98-138">[Předchozí: Začínáme](xref:tutorials/razor-pages/razor-pages-start)
[Další: vygeneroval stránky Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="88a98-138">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
