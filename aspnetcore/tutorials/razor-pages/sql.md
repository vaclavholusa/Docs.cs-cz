---
title: Práce s SQL Server LocalDB a ASP.NET Core
author: rick-anderson
description: Vysvětluje práci s SQL Server LocalDB a ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 1fd86fb61b7f5ddb760992ac10f9bee43ab831cf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272140"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="7d091-103">Práce s SQL Server LocalDB a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d091-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="7d091-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="7d091-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="7d091-105">`MovieContext` Objekt zpracovává úlohu s připojením k databázi a mapování `Movie` objekty záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="7d091-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="7d091-106">Kontext databáze není zaregistrována [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru v `ConfigureServices` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="7d091-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="7d091-107">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]</span><span class="sxs-lookup"><span data-stu-id="7d091-107">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="7d091-108">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="7d091-108">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]</span></span>

<span data-ttu-id="7d091-109">Další informace o metodách `ConfigureServices`, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="7d091-109">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="7d091-110">[Podpora Evropa obecné Data Protection nařízení (GDPR) v ASP.NET Core](xref:security/gdpr) pro `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="7d091-110">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="7d091-111">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="7d091-111">SetCompatibilityVersion</span></span>](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

<span data-ttu-id="7d091-112">ASP.NET Core [konfigurace](xref:fundamentals/configuration/index) systému čtení `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="7d091-112">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="7d091-113">Pro místní vývoj, získá připojovací řetězec z *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="7d091-113">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="7d091-114">Hodnota názvu pro databázi (`Database={Database name}`) se budou lišit pro vygenerovaný kód.</span><span class="sxs-lookup"><span data-stu-id="7d091-114">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="7d091-115">Hodnota názvu je libovolný.</span><span class="sxs-lookup"><span data-stu-id="7d091-115">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="7d091-116">Když nasadíte aplikaci k testu nebo produkčním serveru, můžete použít proměnné prostředí nebo jiný přístup k nastavení připojovacího řetězce k skutečné systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7d091-116">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="7d091-117">V tématu [konfigurace](xref:fundamentals/configuration/index) Další informace.</span><span class="sxs-lookup"><span data-stu-id="7d091-117">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="7d091-118">Databáze SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="7d091-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="7d091-119">LocalDB je Odlehčená verze SQL serveru Express databázového stroje je cílová pro vývoj programu.</span><span class="sxs-lookup"><span data-stu-id="7d091-119">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="7d091-120">LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není žádná komplexní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7d091-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="7d091-121">Ve výchozím nastavení, vytvoří databáze LocalDB "\*.mdf" soubory *C: či uživatelů nebo\<uživatele\>*  adresáře.</span><span class="sxs-lookup"><span data-stu-id="7d091-121">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="7d091-122">Z **zobrazení** nabídce otevřete **Průzkumník objektů systému SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="7d091-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Nabídka Zobrazit](sql/_static/ssox.png)

* <span data-ttu-id="7d091-124">Klikněte pravým tlačítkem na `Movie` tabulky a vyberte **Návrhář zobrazení**:</span><span class="sxs-lookup"><span data-stu-id="7d091-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Otevřete v tabulce film kontextové nabídky](sql/_static/design.png)

  ![Tabulka film otevřít v Návrháři](sql/_static/dv.png)

<span data-ttu-id="7d091-127">Poznámka: na ikonu klíče do `ID`.</span><span class="sxs-lookup"><span data-stu-id="7d091-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="7d091-128">Ve výchozím nastavení vytvoří EF vlastnost s názvem `ID` pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="7d091-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="7d091-129">Klikněte pravým tlačítkem na `Movie` tabulky a vyberte **Data zobrazení**:</span><span class="sxs-lookup"><span data-stu-id="7d091-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tabulka film otevřete zobrazení dat v tabulce](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="7d091-131">Počáteční hodnoty databáze</span><span class="sxs-lookup"><span data-stu-id="7d091-131">Seed the database</span></span>

<span data-ttu-id="7d091-132">Vytvořte novou třídu s názvem `SeedData` v *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="7d091-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="7d091-133">Generovaného kódu nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="7d091-133">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="7d091-134">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="7d091-134">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7d091-135">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="7d091-135">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]</span></span>

::: moniker-end

<span data-ttu-id="7d091-136">Pokud jsou všechny filmy v databázi, vrátí inicializátoru počáteční hodnoty a jsou přidány žádné filmy.</span><span class="sxs-lookup"><span data-stu-id="7d091-136">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="7d091-137">Přidat inicializátoru počáteční hodnoty</span><span class="sxs-lookup"><span data-stu-id="7d091-137">Add the seed initializer</span></span>

<span data-ttu-id="7d091-138">V *Program.cs*, změnit `Main` metoda proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="7d091-138">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="7d091-139">Zjištění instance kontextu DB z kontejneru pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="7d091-139">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="7d091-140">Volejte metodu počáteční hodnoty, předání kontextu.</span><span class="sxs-lookup"><span data-stu-id="7d091-140">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="7d091-141">Kontext Dispose po dokončení metodu počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7d091-141">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="7d091-142">Následující kód ukazuje aktualizovaný *Program.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="7d091-142">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="7d091-143">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="7d091-143">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7d091-144">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="7d091-144">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]</span></span>

::: moniker-end

<span data-ttu-id="7d091-145">Produkční aplikace nebude volání `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="7d091-145">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="7d091-146">Přidá do kód který předchází aby následující výjimky při `Update-Database` nebyl spuštěn:</span><span class="sxs-lookup"><span data-stu-id="7d091-146">It's added to the preceeding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="7d091-147">SqlException: Nelze otevřít databázi "RazorPagesMovieContext-21" požadovaný v přihlášení.</span><span class="sxs-lookup"><span data-stu-id="7d091-147">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="7d091-148">Přihlášení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="7d091-148">The login failed.</span></span>
<span data-ttu-id="7d091-149">Přihlášení uživatele, uživatelské jméno, se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="7d091-149">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="7d091-150">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="7d091-150">Test the app</span></span>

* <span data-ttu-id="7d091-151">Odstraňte všechny záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="7d091-151">Delete all the records in the DB.</span></span> <span data-ttu-id="7d091-152">To lze provést pomocí odstranění odkazy v prohlížeči nebo z [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="7d091-152">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="7d091-153">Vynutit aplikace k chybě při inicializaci (volání metody v `Startup` třída), spustí metodu počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7d091-153">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="7d091-154">Pokud chcete vynutit inicializace, IIS Express musí být zastavena a restartována.</span><span class="sxs-lookup"><span data-stu-id="7d091-154">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="7d091-155">Můžete to provést pomocí některé z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="7d091-155">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="7d091-156">Klikněte pravým tlačítkem na hlavním panelu ikonu IIS Express systému v oznamovací oblasti a klepněte na **ukončení** nebo **zastavit lokality**:</span><span class="sxs-lookup"><span data-stu-id="7d091-156">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Služba IIS Express ikonu na hlavním panelu](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Kontextové nabídky](sql/_static/stopIIS.png)

    * <span data-ttu-id="7d091-159">Pokud VS byly spuštěny v režimu bez ladění, stisknutím klávesy F5 spusťte v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="7d091-159">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="7d091-160">Pokud VS byly spuštěny v režimu ladění, zastavení ladicího programu a stisknutím klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="7d091-160">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="7d091-161">Aplikace zobrazuje dosazená data:</span><span class="sxs-lookup"><span data-stu-id="7d091-161">The app shows the seeded data:</span></span>

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](sql/_static/m55.png)

<span data-ttu-id="7d091-163">V dalším kurzu se vyčistit prezentaci dat.</span><span class="sxs-lookup"><span data-stu-id="7d091-163">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7d091-164">[Předchozí: Vygeneroval stránky Razor](xref:tutorials/razor-pages/page)
> [Další: aktualizace stránky](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="7d091-164">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
