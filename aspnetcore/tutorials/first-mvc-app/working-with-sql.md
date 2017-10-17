---
title: "Práce s LocalDB serveru SQL"
author: rick-anderson
description: "Použití SQL serveru LocalDB s jednoduchou aplikaci MVC"
keywords: ASP.NET Core, LocalDB LocalDB, SQL Server, SQL Server
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: ff8fd9b8-7c98-424d-8641-7524e23bf541
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: e44b6de13540d93337bf9a128d287808cffbfb46
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/22/2017
---
# <a name="working-with-sql-server-localdb"></a><span data-ttu-id="60068-104">Práce s LocalDB serveru SQL</span><span class="sxs-lookup"><span data-stu-id="60068-104">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="60068-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="60068-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="60068-106">`MvcMovieContext` Objekt zpracovává úlohu s připojením k databázi a mapování `Movie` objekty záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="60068-106">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="60068-107">Kontext databáze není zaregistrována [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru v `ConfigureServices` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="60068-107">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

<span data-ttu-id="60068-108">ASP.NET Core [konfigurace](xref:fundamentals/configuration) systému čtení `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="60068-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration) system reads the `ConnectionString`.</span></span> <span data-ttu-id="60068-109">Pro místní vývoj, získá připojovací řetězec z *appSettings.JSON určený* souboru:</span><span class="sxs-lookup"><span data-stu-id="60068-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-javascript[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="60068-110">Když nasadíte aplikaci k testu nebo produkčním serveru, můžete použít proměnné prostředí nebo jiný přístup k nastavení připojovacího řetězce k skutečné systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="60068-110">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="60068-111">V tématu [konfigurace](xref:fundamentals/configuration) Další informace.</span><span class="sxs-lookup"><span data-stu-id="60068-111">See [Configuration](xref:fundamentals/configuration) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="60068-112">Databáze SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="60068-112">SQL Server Express LocalDB</span></span>

<span data-ttu-id="60068-113">LocalDB je Odlehčená verze SQL serveru Express databázového stroje je cílová pro vývoj programu.</span><span class="sxs-lookup"><span data-stu-id="60068-113">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="60068-114">LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není žádná komplexní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="60068-114">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="60068-115">Ve výchozím nastavení, vytvoří databáze LocalDB "\*.mdf" soubory *C: či uživatelů nebo\<uživatele\>*  adresáře.</span><span class="sxs-lookup"><span data-stu-id="60068-115">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="60068-116">Z **zobrazení** nabídce otevřete **Průzkumník objektů systému SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="60068-116">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Nabídka Zobrazit](working-with-sql/_static/ssox.png)

* <span data-ttu-id="60068-118">Klikněte pravým tlačítkem na `Movie` tabulky **> Návrhář zobrazení**</span><span class="sxs-lookup"><span data-stu-id="60068-118">Right click on the `Movie` table **> View Designer**</span></span>

  ![Otevřete v tabulce film kontextové nabídky](working-with-sql/_static/design.png)

  ![Tabulka film otevřít v Návrháři](working-with-sql/_static/dv.png)

<span data-ttu-id="60068-121">Poznámka: na ikonu klíče do `ID`.</span><span class="sxs-lookup"><span data-stu-id="60068-121">Note the key icon next to `ID`.</span></span> <span data-ttu-id="60068-122">Ve výchozím nastavení, bude EF vlastnost s názvem `ID` primární klíč.</span><span class="sxs-lookup"><span data-stu-id="60068-122">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="60068-123">Klikněte pravým tlačítkem na `Movie` tabulky **> zobrazení dat**</span><span class="sxs-lookup"><span data-stu-id="60068-123">Right click on the `Movie` table **> View Data**</span></span>

  ![Otevřete v tabulce film kontextové nabídky](working-with-sql/_static/ssox2.png)

  ![Tabulka film otevřete zobrazení dat v tabulce](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="60068-126">Počáteční hodnoty databáze</span><span class="sxs-lookup"><span data-stu-id="60068-126">Seed the database</span></span>

<span data-ttu-id="60068-127">Vytvořte novou třídu s názvem `SeedData` v *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="60068-127">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="60068-128">Generovaného kódu nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="60068-128">Replace the generated code with the following:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="60068-129">Pokud jsou všechny filmy v databázi, vrátí inicializátoru počáteční hodnoty a jsou přidány žádné filmy.</span><span class="sxs-lookup"><span data-stu-id="60068-129">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="60068-130">Přidat inicializátoru počáteční hodnoty</span><span class="sxs-lookup"><span data-stu-id="60068-130">Add the seed initializer</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="60068-131">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="60068-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="60068-132">Přidat inicializátoru počáteční hodnoty do `Main` metoda v *Program.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="60068-132">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="60068-133">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="60068-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="60068-134">Přidejte na konec inicializátoru počáteční hodnoty `Configure` metoda v *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="60068-134">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

<span data-ttu-id="60068-135">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="60068-135">Test the app</span></span>

* <span data-ttu-id="60068-136">Odstraňte všechny záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="60068-136">Delete all the records in the DB.</span></span> <span data-ttu-id="60068-137">Můžete provést s odstranit odkazy v prohlížeči nebo z SSOX.</span><span class="sxs-lookup"><span data-stu-id="60068-137">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="60068-138">Vynutit aplikace k chybě při inicializaci (volání metody v `Startup` třída), spustí metodu počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="60068-138">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="60068-139">Pokud chcete vynutit inicializace, IIS Express musí být zastavena a restartována.</span><span class="sxs-lookup"><span data-stu-id="60068-139">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="60068-140">Můžete to provést pomocí některé z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="60068-140">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="60068-141">Klikněte pravým tlačítkem na hlavním panelu ikonu IIS Express systému v oznamovací oblasti a klepněte na **ukončení** nebo **zastavit lokality**</span><span class="sxs-lookup"><span data-stu-id="60068-141">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Služba IIS Express ikonu na hlavním panelu](working-with-sql/_static/iisExIcon.png)

    ![Kontextové nabídky](working-with-sql/_static/stopIIS.png)

   * <span data-ttu-id="60068-144">Pokud VS byly spuštěny v režimu bez ladění, stisknutím klávesy F5 spusťte v režimu ladění</span><span class="sxs-lookup"><span data-stu-id="60068-144">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
   * <span data-ttu-id="60068-145">Pokud VS byly spuštěny v režimu ladění, zastavení ladicího programu a stisknutím klávesy F5</span><span class="sxs-lookup"><span data-stu-id="60068-145">If you were running VS in debug mode, stop the debugger and press F5</span></span>
   
<span data-ttu-id="60068-146">Aplikace zobrazuje dosazená data.</span><span class="sxs-lookup"><span data-stu-id="60068-146">The app shows the seeded data.</span></span>

![Aplikace MVC film otevřete v Microsoft Edge znázorňující film data](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
<span data-ttu-id="60068-148">[Předchozí](adding-model.md)
[další](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="60068-148">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
