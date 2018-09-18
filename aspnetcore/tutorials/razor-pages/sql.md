---
title: Práce s SQL Server LocalDB a ASP.NET Core
author: rick-anderson
description: Vysvětluje, práci s SQL Server LocalDB a ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 20e2353eb2e453235c2fb04c696a7e3d27bed5bf
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011271"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="53e09-103">Práce s SQL Server LocalDB a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53e09-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="53e09-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="53e09-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="53e09-105">`MovieContext` Objekt zpracovává úlohu s připojením k databázi a mapování `Movie` objekty se záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="53e09-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="53e09-106">Kontext databáze je zaregistrován [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru v `ConfigureServices` metoda ve *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="53e09-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="53e09-107">Další informace o metodách `ConfigureServices`, naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="53e09-107">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="53e09-108">[Podpora EU obecného Regulation (GDPR) v ASP.NET Core](xref:security/gdpr) pro `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="53e09-108">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="53e09-109">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="53e09-109">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

::: moniker-end

<span data-ttu-id="53e09-110">ASP.NET Core [konfigurace](xref:fundamentals/configuration/index) systému čtení `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="53e09-110">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="53e09-111">Pro místní vývoj, získá připojovací řetězec z *appsettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="53e09-111">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="53e09-112">Hodnota názvu databáze (`Database={Database name}`) se bude lišit pro vygenerovaný kód.</span><span class="sxs-lookup"><span data-stu-id="53e09-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="53e09-113">Hodnota názvu je volitelný.</span><span class="sxs-lookup"><span data-stu-id="53e09-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="53e09-114">Když nasadíte aplikaci na testovacím nebo produkčním serveru, můžete použít proměnné prostředí nebo jiné přístup se nastavit připojovací řetězec na skutečný SQL Server.</span><span class="sxs-lookup"><span data-stu-id="53e09-114">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="53e09-115">Zobrazit [konfigurace](xref:fundamentals/configuration/index) Další informace.</span><span class="sxs-lookup"><span data-stu-id="53e09-115">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="53e09-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="53e09-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="53e09-117">LocalDB je Odlehčená verze SQL serveru Express databázového stroje, která je určená pro vývoj v programu.</span><span class="sxs-lookup"><span data-stu-id="53e09-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="53e09-118">LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není bez složité konfigurace.</span><span class="sxs-lookup"><span data-stu-id="53e09-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="53e09-119">Ve výchozím nastavení, vytvoří databázi LocalDB "\*.mdf" soubory *C:/uživatele/\<uživatele\>*  adresáře.</span><span class="sxs-lookup"><span data-stu-id="53e09-119">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="53e09-120">Z **zobrazení** nabídce otevřete **Průzkumník objektů systému SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="53e09-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Nabídka Zobrazit](sql/_static/ssox.png)

* <span data-ttu-id="53e09-122">Klikněte pravým tlačítkem na `Movie` tabulce a vybrat **Návrhář zobrazení**:</span><span class="sxs-lookup"><span data-stu-id="53e09-122">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Kontextová nabídka otevřít na tabulky Movie](sql/_static/design.png)

  ![Otevřít v Návrháři tabulky Movie](sql/_static/dv.png)

<span data-ttu-id="53e09-125">Poznámka: na ikonu klíče vedle `ID`.</span><span class="sxs-lookup"><span data-stu-id="53e09-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="53e09-126">Ve výchozím nastavení, EF vytvoří vlastnost s názvem `ID` pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="53e09-126">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="53e09-127">Klikněte pravým tlačítkem na `Movie` tabulce a vybrat **Data zobrazení**:</span><span class="sxs-lookup"><span data-stu-id="53e09-127">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Otevřít zobrazení tabulky dat tabulky Movie](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="53e09-129">Přidání dat do databáze</span><span class="sxs-lookup"><span data-stu-id="53e09-129">Seed the database</span></span>

<span data-ttu-id="53e09-130">Vytvořte novou třídu s názvem `SeedData` v *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="53e09-130">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="53e09-131">Generovaného kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="53e09-131">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

<span data-ttu-id="53e09-132">Pokud jsou všechny filmy v databázi, vrátí inicializátoru pro dosazení hodnot a jsou přidány žádné video.</span><span class="sxs-lookup"><span data-stu-id="53e09-132">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="53e09-133">Přidat inicializační výraz počáteční hodnoty</span><span class="sxs-lookup"><span data-stu-id="53e09-133">Add the seed initializer</span></span>

<span data-ttu-id="53e09-134">V *Program.cs*, změnit `Main` metoda můžete provádět následující:</span><span class="sxs-lookup"><span data-stu-id="53e09-134">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="53e09-135">Instance kontextu databáze získáte z kontejneru pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="53e09-135">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="53e09-136">Volejte metodu počáteční hodnoty předání kontextu.</span><span class="sxs-lookup"><span data-stu-id="53e09-136">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="53e09-137">Kontext Dispose po dokončení počáteční hodnoty metody.</span><span class="sxs-lookup"><span data-stu-id="53e09-137">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="53e09-138">Následující kód ukazuje aktualizovaný *Program.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="53e09-138">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

<span data-ttu-id="53e09-139">Produkční aplikace by volat `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="53e09-139">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="53e09-140">Přidá se do předchozí kód, aby se zabránilo následující výjimku při `Update-Database` nebyl spuštěn:</span><span class="sxs-lookup"><span data-stu-id="53e09-140">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="53e09-141">SqlException: Databázi "RazorPagesMovieContext 21" požadovaný v přihlášení nelze otevřít.</span><span class="sxs-lookup"><span data-stu-id="53e09-141">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="53e09-142">Přihlášení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="53e09-142">The login failed.</span></span>
<span data-ttu-id="53e09-143">Přihlašovací jméno uživatele 'jméno uživatele' se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="53e09-143">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="53e09-144">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="53e09-144">Test the app</span></span>

* <span data-ttu-id="53e09-145">Odstraníte všechny záznamy z databáze.</span><span class="sxs-lookup"><span data-stu-id="53e09-145">Delete all the records in the DB.</span></span> <span data-ttu-id="53e09-146">To lze provést pomocí odstranit odkazy v prohlížeči nebo z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="53e09-146">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="53e09-147">Se aplikace inicializuje (volat metody ve `Startup` třídy), spustí seed – metoda.</span><span class="sxs-lookup"><span data-stu-id="53e09-147">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="53e09-148">Pokud chcete vynutit inicializace, služba IIS Express musí zastavit, restartovat.</span><span class="sxs-lookup"><span data-stu-id="53e09-148">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="53e09-149">Provést s některou z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="53e09-149">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="53e09-150">Klikněte pravým tlačítkem na službu IIS Express systému na hlavním panelu ikonu v oznamovací oblasti a klepněte na **ukončovací** nebo **zastavení webu**:</span><span class="sxs-lookup"><span data-stu-id="53e09-150">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Služba IIS Express ikonu na hlavním panelu](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Kontextové nabídky](sql/_static/stopIIS.png)

    * <span data-ttu-id="53e09-153">Pokud VS byly spuštěny v režimu bez ladění, stiskněte klávesu F5 ke spuštění v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="53e09-153">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="53e09-154">Pokud jste používali zastavení ladicího programu VS v režimu ladění a stisknutím klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="53e09-154">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="53e09-155">Aplikace bude zobrazovat dosazená data:</span><span class="sxs-lookup"><span data-stu-id="53e09-155">The app shows the seeded data:</span></span>

![Otevřít v prohlížeči Chrome ukazující data o filmech aplikace Movie](sql/_static/m55.png)

<span data-ttu-id="53e09-157">V dalším kurzu se vyčistit prezentace data.</span><span class="sxs-lookup"><span data-stu-id="53e09-157">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="53e09-158">[Předchozí: Generované uživatelské rozhraní pro stránky Razor](xref:tutorials/razor-pages/page)
> [Další: aktualizace stránek](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="53e09-158">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
