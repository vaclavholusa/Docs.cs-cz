---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: "Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace databáze - 9 12 | Microsoft Docs"
author: tdykstra
description: "Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 44faca6ac2cdb9193f5c7f6b1d7f5a26e6e22248
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a><span data-ttu-id="a6508-103">Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace databáze - 9 12</span><span class="sxs-lookup"><span data-stu-id="a6508-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Database Update - 9 of 12</span></span>
====================
<span data-ttu-id="a6508-104">podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a6508-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="a6508-105">Stáhněte si úvodní projekt</span><span class="sxs-lookup"><span data-stu-id="a6508-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="a6508-106">Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web.</span><span class="sxs-lookup"><span data-stu-id="a6508-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="a6508-107">Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web.</span><span class="sxs-lookup"><span data-stu-id="a6508-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="a6508-108">Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="a6508-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="a6508-109">Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, naleznete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a6508-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="a6508-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="a6508-110">Overview</span></span>

<span data-ttu-id="a6508-111">V tomto kurzu provedete změnu databáze a změny související kódu, testovat změny v sadě Visual Studio a pak nasadit aktualizace do testovací i produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a6508-111">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to both the test and production environments.</span></span>

<span data-ttu-id="a6508-112">Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="a6508-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="a6508-113">Přidá sloupec do tabulky</span><span class="sxs-lookup"><span data-stu-id="a6508-113">Adding a New Column to a Table</span></span>

<span data-ttu-id="a6508-114">V této části můžete přidat sloupec Datum narození `Person` základní třídu pro `Student` a `Instructor` entity.</span><span class="sxs-lookup"><span data-stu-id="a6508-114">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="a6508-115">Potom je aktualizovat stránky, která zobrazuje data lektorem tak, aby se zobrazí nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="a6508-115">Then you update the page that displays instructor data so that it displays the new column.</span></span>

<span data-ttu-id="a6508-116">V *ContosoUniversity.DAL* projekt, otevřete *Person.cs* a přidejte následující vlastnost na konci `Person` – třída (měla by existovat dvě zavřením složené závorky následující ji):</span><span class="sxs-lookup"><span data-stu-id="a6508-116">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

<span data-ttu-id="a6508-117">Potom aktualizujte metodu počáteční hodnoty tak, aby poskytuje hodnotu pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="a6508-117">Next, update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="a6508-118">Otevřete *Migrations\Configuration.cs* a nahraďte bloku kódu, který začíná `var instructors = new List<Instructor>` s následující blok kódu, který obsahuje informace o datu narození:</span><span class="sxs-lookup"><span data-stu-id="a6508-118">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

<span data-ttu-id="a6508-119">Otevřete v projektu ContosoUniversity *Instructors.aspx* a přidejte nové šablony pole, které chcete zobrazit datum narození.</span><span class="sxs-lookup"><span data-stu-id="a6508-119">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="a6508-120">Přidáte mezi těm, které jsou pro přiřazení přijetím datum a office:</span><span class="sxs-lookup"><span data-stu-id="a6508-120">Add it between the ones for hire date and office assignment:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

<span data-ttu-id="a6508-121">(Pokud odsazení kód získá synchronizován, můžete stisknout CTRL-K a pak CTRL-D automaticky přeformátuje souboru.)</span><span class="sxs-lookup"><span data-stu-id="a6508-121">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>

<span data-ttu-id="a6508-122">Sestavte řešení a pak otevřete **Konzola správce balíčků** okno.</span><span class="sxs-lookup"><span data-stu-id="a6508-122">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="a6508-123">Ujistěte se, že je stále ContosoUniversity.DAL vybrán jako **výchozí projekt**.</span><span class="sxs-lookup"><span data-stu-id="a6508-123">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>

<span data-ttu-id="a6508-124">V **Konzola správce balíčků** vyberte **ContosoUniversity.DAL** jako **výchozí projekt**a potom zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a6508-124">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

<span data-ttu-id="a6508-125">Po dokončení tohoto příkazu Visual Studio otevře soubor třídy, který definuje nový `DbMIgration` třída a v `Up` metoda vidíte kód, který vytvoří nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="a6508-125">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span>

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

<span data-ttu-id="a6508-127">Sestavte řešení a potom zadejte následující příkaz v **Konzola správce balíčků** okno (musí být vybrána projektu ContosoUniversity.DAL stále):</span><span class="sxs-lookup"><span data-stu-id="a6508-127">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

<span data-ttu-id="a6508-128">Až se příkaz dokončí, spusťte aplikaci a vyberte stránku vyučující.</span><span class="sxs-lookup"><span data-stu-id="a6508-128">When the command finishes, run the application and select the Instructors page.</span></span> <span data-ttu-id="a6508-129">Při načtení stránky uvidíte, že má nové pole pro datum narození.</span><span class="sxs-lookup"><span data-stu-id="a6508-129">When the page loads, you see that it has the new birth date field.</span></span>

<span data-ttu-id="a6508-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="a6508-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="a6508-131">Nasazení aktualizace databáze do testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="a6508-131">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="a6508-132">V **Průzkumníku řešení** vyberte ContosoUniversity projekt.</span><span class="sxs-lookup"><span data-stu-id="a6508-132">In **Solution Explorer** select the ContosoUniversity project.</span></span>

<span data-ttu-id="a6508-133">V **publikování webu jedním kliknutím** nástrojů, vyberte **Test** profil publikování a potom klikněte na **Publikovat Web**.</span><span class="sxs-lookup"><span data-stu-id="a6508-133">In the **Web One Click Publish** toolbar, select the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="a6508-134">(Pokud je zakázána panelu nástrojů, vyberte projekt ContosoUniversity v **Průzkumníku řešení**.)</span><span class="sxs-lookup"><span data-stu-id="a6508-134">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

<span data-ttu-id="a6508-135">Visual Studio nasadí aktualizovanou aplikaci a prohlížeči se otevře na domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="a6508-135">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span> <span data-ttu-id="a6508-136">Spusťte stránku vyučující a ověřte, že aktualizace byla úspěšně nasazena.</span><span class="sxs-lookup"><span data-stu-id="a6508-136">Run the Instructors page to verify that the update was successfully deployed.</span></span> <span data-ttu-id="a6508-137">Když se aplikace pokusí o přístup k databázi pro tuto stránku, Code First aktualizace schématu databáze a spustí `Seed` metoda.</span><span class="sxs-lookup"><span data-stu-id="a6508-137">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="a6508-138">Při zobrazení stránky, uvidíte očekávané **datum narození** sloupce s daty v ní.</span><span class="sxs-lookup"><span data-stu-id="a6508-138">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>

<span data-ttu-id="a6508-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="a6508-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="a6508-140">Nasazení aktualizace databáze do produkčního prostředí</span><span class="sxs-lookup"><span data-stu-id="a6508-140">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="a6508-141">Teď můžete nasadit do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="a6508-141">You can now deploy to production.</span></span> <span data-ttu-id="a6508-142">Jediným rozdílem je, že budete používat *aplikace\_offline.htm* lze zabránit uživatelům v přístupu na web a proto aktualizace databáze, zatímco nasazujete změny.</span><span class="sxs-lookup"><span data-stu-id="a6508-142">The only difference is that you'll use *app\_offline.htm* to prevent users from accessing the site and thus updating the database while you're deploying changes.</span></span> <span data-ttu-id="a6508-143">Pro produkční nasazení proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a6508-143">For production deployment perform the following steps:</span></span>

- <span data-ttu-id="a6508-144">Nahrát *aplikace\_offline.htm* soubor pracoviště.</span><span class="sxs-lookup"><span data-stu-id="a6508-144">Upload the *app\_offline.htm* file to the production site.</span></span>
- <span data-ttu-id="a6508-145">V sadě Visual Studio, vyberte profil produkční v **publikování webu jedním kliknutím** panelu nástrojů a klikněte na tlačítko **Publikovat Web**.</span><span class="sxs-lookup"><span data-stu-id="a6508-145">In Visual Studio, choose the Production profile in the **Web One Click Publish** toolbar and click **Publish Web**.</span></span>
- <span data-ttu-id="a6508-146">Odstranit *aplikace\_offline.htm* soubor z pracoviště.</span><span class="sxs-lookup"><span data-stu-id="a6508-146">Delete the *app\_offline.htm* file from the production site.</span></span>

> [!NOTE]
> <span data-ttu-id="a6508-147">Když aplikaci právě používá v produkčním prostředí by měl být implementace plánu zálohování.</span><span class="sxs-lookup"><span data-stu-id="a6508-147">While your application is in use in the production environment you should be implementing a backup plan.</span></span> <span data-ttu-id="a6508-148">To znamená, které by měl být pravidelně kopírování *školní-Prod.sdf* a *aspnet Prod.sdf* soubory z produkční lokality do umístění zabezpečeného úložiště a měli udržuje několik generací například zálohování.</span><span class="sxs-lookup"><span data-stu-id="a6508-148">That is, you should be periodically copying the *School-Prod.sdf* and *aspnet-Prod.sdf* files from the production site to a secure storage location, and you should be keeping several generations of such backups.</span></span> <span data-ttu-id="a6508-149">Při aktualizaci databáze byste měli vytvořit záložní kopii z bezprostředně před provedením změny.</span><span class="sxs-lookup"><span data-stu-id="a6508-149">When you update the database, you should make a backup copy from immediately before the change.</span></span> <span data-ttu-id="a6508-150">Poté Pokud došlo k chybě a nebudete zjistit až po jeho nasazení do produkčního prostředí, bude stále budete moci obnovit databázi do stavu, ve kterém se nacházel před jeho poškozením.</span><span class="sxs-lookup"><span data-stu-id="a6508-150">Then, if you make a mistake and don't discover it until after you have deployed it to production, you will still be able to recover the database to the state it was in before it became corrupted.</span></span>


<span data-ttu-id="a6508-151">Když Visual Studio otevře adresu URL domovskou stránku v prohlížeči *aplikace\_offline.htm* zobrazí se stránka.</span><span class="sxs-lookup"><span data-stu-id="a6508-151">When Visual Studio opens the home page URL in the browser, the *app\_offline.htm* page is displayed.</span></span> <span data-ttu-id="a6508-152">Po odstranění *aplikace\_offline.htm* souboru, můžete přejít na domovskou stránku znovu a ověřte, že aktualizace byla úspěšně nasazena.</span><span class="sxs-lookup"><span data-stu-id="a6508-152">After you delete the *app\_offline.htm* file, you can browse to your home page again to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="a6508-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="a6508-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span></span>

<span data-ttu-id="a6508-154">Nyní jste nasadili aktualizaci aplikace, která zahrnuté ke změně databáze pro testovací a produkční.</span><span class="sxs-lookup"><span data-stu-id="a6508-154">You've now deployed an application update that included a database change to both test and production.</span></span> <span data-ttu-id="a6508-155">V dalším kurzu se dozvíte, jak migrovat databázi z systém SQL Server Compact do systému SQL Server Express a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a6508-155">The next tutorial shows you how to migrate your database from SQL Server Compact to SQL Server Express and SQL Server.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a6508-156">[Předchozí](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[další](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="a6508-156">[Previous](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[Next](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span></span>
