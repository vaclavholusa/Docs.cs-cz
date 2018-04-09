---
title: Práce s SQL Server LocalDB a ASP.NET Core
author: rick-anderson
description: Vysvětluje práci s SQL Server LocalDB a ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/sql
ms.openlocfilehash: e5fa552f80f7ee0876fe31dd87a1ddbd5fe567dd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="594fd-103">Práce s SQL Server LocalDB a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="594fd-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="594fd-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="594fd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="594fd-105">`MovieContext` Objekt zpracovává úlohu s připojením k databázi a mapování `Movie` objekty záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="594fd-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="594fd-106">Kontext databáze není zaregistrována [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru v `ConfigureServices` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="594fd-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

<span data-ttu-id="594fd-107">ASP.NET Core [konfigurace](xref:fundamentals/configuration/index) systému čtení `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="594fd-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="594fd-108">Pro místní vývoj, získá připojovací řetězec z *appSettings.JSON určený* souboru:</span><span class="sxs-lookup"><span data-stu-id="594fd-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="594fd-109">Když nasadíte aplikaci k testu nebo produkčním serveru, můžete použít proměnné prostředí nebo jiný přístup k nastavení připojovacího řetězce k skutečné systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="594fd-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="594fd-110">V tématu [konfigurace](xref:fundamentals/configuration/index) Další informace.</span><span class="sxs-lookup"><span data-stu-id="594fd-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="594fd-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="594fd-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="594fd-112">LocalDB je Odlehčená verze SQL serveru Express databázového stroje je cílová pro vývoj programu.</span><span class="sxs-lookup"><span data-stu-id="594fd-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="594fd-113">LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není žádná komplexní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="594fd-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="594fd-114">Ve výchozím nastavení, vytvoří databáze LocalDB "\*.mdf" soubory *C: či uživatelů nebo\<uživatele\>*  adresáře.</span><span class="sxs-lookup"><span data-stu-id="594fd-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="594fd-115">Z **zobrazení** nabídce otevřete **Průzkumník objektů systému SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="594fd-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Nabídka Zobrazit](sql/_static/ssox.png)

* <span data-ttu-id="594fd-117">Klikněte pravým tlačítkem na `Movie` tabulky a vyberte **Návrhář zobrazení**:</span><span class="sxs-lookup"><span data-stu-id="594fd-117">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Otevřete v tabulce film kontextové nabídky](sql/_static/design.png)

  ![Tabulka film otevřít v Návrháři](sql/_static/dv.png)

<span data-ttu-id="594fd-120">Poznámka: na ikonu klíče do `ID`.</span><span class="sxs-lookup"><span data-stu-id="594fd-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="594fd-121">Ve výchozím nastavení vytvoří EF vlastnost s názvem `ID` pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="594fd-121">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="594fd-122">Klikněte pravým tlačítkem na `Movie` tabulky a vyberte **Data zobrazení**:</span><span class="sxs-lookup"><span data-stu-id="594fd-122">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tabulka film otevřete zobrazení dat v tabulce](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="594fd-124">Počáteční hodnoty databáze</span><span class="sxs-lookup"><span data-stu-id="594fd-124">Seed the database</span></span>

<span data-ttu-id="594fd-125">Vytvořte novou třídu s názvem `SeedData` v *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="594fd-125">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="594fd-126">Generovaného kódu nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="594fd-126">Replace the generated code with the following:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="594fd-127">Pokud jsou všechny filmy v databázi, vrátí inicializátoru počáteční hodnoty a jsou přidány žádné filmy.</span><span class="sxs-lookup"><span data-stu-id="594fd-127">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="594fd-128">Přidat inicializátoru počáteční hodnoty</span><span class="sxs-lookup"><span data-stu-id="594fd-128">Add the seed initializer</span></span>

<span data-ttu-id="594fd-129">Přidejte na konec inicializátoru počáteční hodnoty `Main` metoda v *Program.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="594fd-129">Add the seed initializer to the end of the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

<span data-ttu-id="594fd-130">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="594fd-130">Test the app</span></span>

* <span data-ttu-id="594fd-131">Odstraňte všechny záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="594fd-131">Delete all the records in the DB.</span></span> <span data-ttu-id="594fd-132">To lze provést pomocí odstranění odkazy v prohlížeči nebo z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="594fd-132">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="594fd-133">Vynutit aplikace k chybě při inicializaci (volání metody v `Startup` třída), spustí metodu počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="594fd-133">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="594fd-134">Pokud chcete vynutit inicializace, IIS Express musí být zastavena a restartována.</span><span class="sxs-lookup"><span data-stu-id="594fd-134">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="594fd-135">Můžete to provést pomocí některé z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="594fd-135">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="594fd-136">Klikněte pravým tlačítkem na hlavním panelu ikonu IIS Express systému v oznamovací oblasti a klepněte na **ukončení** nebo **zastavit lokality**:</span><span class="sxs-lookup"><span data-stu-id="594fd-136">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Služba IIS Express ikonu na hlavním panelu](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Kontextové nabídky](sql/_static/stopIIS.png)

    * <span data-ttu-id="594fd-139">Pokud VS byly spuštěny v režimu bez ladění, stisknutím klávesy F5 spusťte v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="594fd-139">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="594fd-140">Pokud VS byly spuštěny v režimu ladění, zastavení ladicího programu a stisknutím klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="594fd-140">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="594fd-141">Aplikace zobrazuje dosazená data:</span><span class="sxs-lookup"><span data-stu-id="594fd-141">The app shows the seeded data:</span></span>

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](sql/_static/m55.png)

<span data-ttu-id="594fd-143">V dalším kurzu se vyčistit prezentaci dat.</span><span class="sxs-lookup"><span data-stu-id="594fd-143">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="594fd-144">[Předchozí: Vygeneroval stránky Razor](xref:tutorials/razor-pages/page)
> [Další: aktualizace stránky](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="594fd-144">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
