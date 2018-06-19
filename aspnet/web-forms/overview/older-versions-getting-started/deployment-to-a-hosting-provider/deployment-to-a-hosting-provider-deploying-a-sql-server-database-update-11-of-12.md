---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizaci databáze SQL Server - 11 12 | Microsoft Docs'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8cc072c631900937f31c8f2637869b53d46cf54
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887327"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="f95b7-103">Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizaci databáze SQL Server - 11 12</span><span class="sxs-lookup"><span data-stu-id="f95b7-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="f95b7-104">Podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f95b7-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="f95b7-105">Stáhněte si úvodní projekt</span><span class="sxs-lookup"><span data-stu-id="f95b7-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="f95b7-106">Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web.</span><span class="sxs-lookup"><span data-stu-id="f95b7-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="f95b7-107">Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web.</span><span class="sxs-lookup"><span data-stu-id="f95b7-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="f95b7-108">Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="f95b7-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="f95b7-109">Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit na weby systému Windows Azure, najdete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f95b7-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="f95b7-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="f95b7-110">Overview</span></span>

<span data-ttu-id="f95b7-111">V tomto kurzu se dozvíte, jak nasadit aktualizace databáze úplnou databázi serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f95b7-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="f95b7-112">Vzhledem k tomu, že migrace Code First vykonává všechnu práci aktualizace databáze, je téměř identický nebyla pro SQL Server Compact v procesu [nasazení aktualizace databáze](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="f95b7-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="f95b7-113">Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="f95b7-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="f95b7-114">Přidá sloupec do tabulky</span><span class="sxs-lookup"><span data-stu-id="f95b7-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="f95b7-115">V této části kurzu budete mít databázi změnit a odpovídající změn kódu v rámci přípravy jejich nasazení do testovací a produkční prostředí je testování v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f95b7-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="f95b7-116">Tato změna vyžaduje přidání `OfficeHours` sloupec, který se `Instructor` entitu a zobrazení nové informace o v **vyučující** webové stránky.</span><span class="sxs-lookup"><span data-stu-id="f95b7-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="f95b7-117">Otevřete v projektu ContosoUniversity.DAL *Instructor.cs* a přidejte následující vlastnost mezi `HireDate` a `Courses` vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="f95b7-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="f95b7-118">Aktualizace třídy pro inicializátoru tak, aby ho doplňuje pro nový sloupec s testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="f95b7-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="f95b7-119">Otevřete *Migrations\Configuration.cs* a nahraďte bloku kódu, který začíná `var instructors = new List<Instructor>` s následující blok kódu, který zahrnuje nový sloupec:</span><span class="sxs-lookup"><span data-stu-id="f95b7-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="f95b7-120">Otevřete v projektu ContosoUniversity *Instructors.aspx* a přidat nové šablony pole pro čas office těsně před uzavírací `</Columns>` značky v prvním `GridView` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="f95b7-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="f95b7-121">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="f95b7-121">Build the solution.</span></span>

<span data-ttu-id="f95b7-122">Otevřete **Konzola správce balíčků** okno a vyberte ContosoUniversity.DAL jako **výchozí projekt**.</span><span class="sxs-lookup"><span data-stu-id="f95b7-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="f95b7-123">Zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f95b7-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="f95b7-124">Spusťte aplikaci a vyberte **vyučující** stránky.</span><span class="sxs-lookup"><span data-stu-id="f95b7-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="f95b7-125">Stránce trvá trochu déle než obvykle načíst, protože rozhraní Entity Framework znovu vytvoří databázi a doplňuje s testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="f95b7-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="f95b7-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f95b7-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="f95b7-127">Nasazení aktualizace databáze do testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="f95b7-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="f95b7-128">Pokud používáte migrace Code First, je stejné jako pro systém SQL Server Compact metodu pro nasazení změn databáze do systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f95b7-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="f95b7-129">Ale budete muset změnit Test profil publikování, protože stále jsou nastaveny na migraci ze systému SQL Server Compact do systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f95b7-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="f95b7-130">Prvním krokem je odebrat Transformace řetězců připojení, které jste vytvořili v předchozí kurzu.</span><span class="sxs-lookup"><span data-stu-id="f95b7-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="f95b7-131">Tyto už nejsou potřeba vzhledem k tomu, že specifikujete Transformace řetězců připojení v profil publikování, jako jste to udělali předtím, než jste nakonfigurovali **balení/publikování kódu SQL** kartě pro migraci do systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f95b7-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="f95b7-132">Otevřete *Web.Test.config* souborů a odebrat `connectionStrings` elementu.</span><span class="sxs-lookup"><span data-stu-id="f95b7-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="f95b7-133">Jenom zbývající transformací v *Web.Test.config* soubor `Environment` hodnotu `appSettings` element.</span><span class="sxs-lookup"><span data-stu-id="f95b7-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="f95b7-134">Nyní můžete aktualizovat profil publikování a publikovat do testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="f95b7-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="f95b7-135">Otevřete **Publikovat Web** průvodce a potom přepnout na **profil** kartě.</span><span class="sxs-lookup"><span data-stu-id="f95b7-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="f95b7-136">Vyberte **Test** profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="f95b7-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="f95b7-137">Vyberte **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="f95b7-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="f95b7-138">Klikněte na tlačítko **povolit novou databázi publikování vylepšení**.</span><span class="sxs-lookup"><span data-stu-id="f95b7-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="f95b7-139">V poli připojovací řetězec pro **SchoolContext**, zadejte stejnou hodnotu, která jste použili v *Web.Test.config* transformace souborů v předchozích kurzu:</span><span class="sxs-lookup"><span data-stu-id="f95b7-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="f95b7-140">Vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**.</span><span class="sxs-lookup"><span data-stu-id="f95b7-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="f95b7-141">(Ve vaší verzi sady Visual Studio může být označené políčko **použít migrace Code First**.)</span><span class="sxs-lookup"><span data-stu-id="f95b7-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="f95b7-142">V poli připojovací řetězec pro **objekt DefaultConnection**, zadejte stejnou hodnotu, která jste použili v *Web.Test.config* transformace souborů v předchozích kurzu:</span><span class="sxs-lookup"><span data-stu-id="f95b7-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="f95b7-143">Nechte **aktualizace databáze** vymazán.</span><span class="sxs-lookup"><span data-stu-id="f95b7-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="f95b7-144">Klikněte na tlačítko **publikování**.</span><span class="sxs-lookup"><span data-stu-id="f95b7-144">Click **Publish**.</span></span>

<span data-ttu-id="f95b7-145">Visual Studio nasadí změny kódu do testovacího prostředí a otevře prohlížeč na domovskou stránku univerzity Contoso.</span><span class="sxs-lookup"><span data-stu-id="f95b7-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="f95b7-146">Vyberte stránku vyučující.</span><span class="sxs-lookup"><span data-stu-id="f95b7-146">Select the Instructors page.</span></span>

<span data-ttu-id="f95b7-147">Při spuštění aplikace na této stránce, pokusí se o přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="f95b7-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="f95b7-148">Migrace Code First zkontroluje, zda je aktuální databáze a zjistí, že ještě nebyla použita nejnovější migrace.</span><span class="sxs-lookup"><span data-stu-id="f95b7-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="f95b7-149">Migrace Code First použije nejnovější migrace, běží `Seed` metoda a stránky spustí normálně.</span><span class="sxs-lookup"><span data-stu-id="f95b7-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="f95b7-150">Zobrazí nový sloupec Office hodin se dosazená data.</span><span class="sxs-lookup"><span data-stu-id="f95b7-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="f95b7-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f95b7-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="f95b7-152">Nasazení aktualizace databáze do produkčního prostředí</span><span class="sxs-lookup"><span data-stu-id="f95b7-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="f95b7-153">Budete muset změnit také profil publikování pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="f95b7-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="f95b7-154">V takovém případě budete odeberte existující profil a vytvořit novou importováním souboru .publishsettings aktualizované.</span><span class="sxs-lookup"><span data-stu-id="f95b7-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="f95b7-155">Aktualizovaný soubor bude obsahovat připojovací řetězec pro databázi systému SQL Server v Cytanium.</span><span class="sxs-lookup"><span data-stu-id="f95b7-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="f95b7-156">Jak už jste viděli při nasazení do testovacího prostředí, již nepotřebujete transformací řetězec připojení v *Web.Production.config* soubor transformace.</span><span class="sxs-lookup"><span data-stu-id="f95b7-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="f95b7-157">Otevřete soubor a odebrat `connectionStrings` elementu.</span><span class="sxs-lookup"><span data-stu-id="f95b7-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="f95b7-158">Zbývající transformace jsou pro `Environment` hodnotu `appSettings` elementu a `location` element, který omezuje přístup Elmah chybových zpráv.</span><span class="sxs-lookup"><span data-stu-id="f95b7-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="f95b7-159">Než vytvoříte nový profil publikování pro produkční prostředí, stažení souboru .publishsettings aktualizované stejně jako jste to udělali v [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="f95b7-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="f95b7-160">(V Ovládacích panelech Cytanium, klikněte na tlačítko **weby**a klikněte **contosouniversity.com** webu.</span><span class="sxs-lookup"><span data-stu-id="f95b7-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="f95b7-161">Vyberte **publikování na webu** a pak klikněte **stáhnout profil publikování pro tento web**.) Důvod, proč byste to je vyzvednutí připojovací řetězec databáze v souboru .publishsettings.</span><span class="sxs-lookup"><span data-stu-id="f95b7-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="f95b7-162">Připojovací řetězec nebyl k dispozici při prvním stažený soubor, protože stále používáte systém SQL Server Compact a kdyby databázi systému SQL Server v Cytanium dosud vytvořena.</span><span class="sxs-lookup"><span data-stu-id="f95b7-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="f95b7-163">Nyní můžete aktualizovat profil publikování a publikovat do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="f95b7-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="f95b7-164">Otevřete **Publikovat Web** průvodce a potom přepnout na **profil** kartě.</span><span class="sxs-lookup"><span data-stu-id="f95b7-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="f95b7-165">Klikněte na tlačítko **spravovat profily**a pak odstraňte produkční profil.</span><span class="sxs-lookup"><span data-stu-id="f95b7-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="f95b7-166">Zavřít **Publikovat Web** průvodce uložte tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="f95b7-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="f95b7-167">Otevřete **Publikovat Web** průvodce znovu a pak klikněte na tlačítko **Import**.</span><span class="sxs-lookup"><span data-stu-id="f95b7-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="f95b7-168">Na **připojení** změňte **cílová adresa URL** na odpovídající hodnotu, pokud používáte dočasnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f95b7-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="f95b7-169">Klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f95b7-169">Click **Next**.</span></span>

<span data-ttu-id="f95b7-170">Na **nastavení** , klikněte na **povolit novou databázi publikování vylepšení**.</span><span class="sxs-lookup"><span data-stu-id="f95b7-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="f95b7-171">V rozevíracím seznamu řetězec připojení pro **SchoolContext**, vyberte Cytanium připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="f95b7-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="f95b7-173">Vyberte **migrace provést Code First (spuštěno při spuštění aplikace)**.</span><span class="sxs-lookup"><span data-stu-id="f95b7-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="f95b7-174">V rozevíracím seznamu řetězec připojení pro **objekt DefaultConnection**, vyberte Cytanium připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="f95b7-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="f95b7-175">Vyberte **profil** , klikněte na **spravovat profily**a přejmenujte jej z "contosouniversity.com - nasazení webu" na "Výroba".</span><span class="sxs-lookup"><span data-stu-id="f95b7-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="f95b7-176">Profil publikování uložte tuto změnu zavřete a pak znovu otevřete.</span><span class="sxs-lookup"><span data-stu-id="f95b7-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="f95b7-177">Klikněte na tlačítko **publikování**.</span><span class="sxs-lookup"><span data-stu-id="f95b7-177">Click **Publish**.</span></span> <span data-ttu-id="f95b7-178">(Pro skutečné produkční web, zkopírujte *aplikace\_offline.htm* do produkčního prostředí a vložit jej do složky projektu před publikováním, pak ji odebrat po dokončení nasazení.)</span><span class="sxs-lookup"><span data-stu-id="f95b7-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="f95b7-179">Visual Studio nasadí změny kódu do testovacího prostředí a otevře prohlížeč na domovskou stránku univerzity Contoso.</span><span class="sxs-lookup"><span data-stu-id="f95b7-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="f95b7-180">Vyberte stránku vyučující.</span><span class="sxs-lookup"><span data-stu-id="f95b7-180">Select the Instructors page.</span></span>

<span data-ttu-id="f95b7-181">Migrace Code First aktualizuje databázi stejným způsobem jako v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f95b7-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="f95b7-182">Zobrazí nový sloupec Office hodin se dosazená data.</span><span class="sxs-lookup"><span data-stu-id="f95b7-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="f95b7-184">Teď úspěšně jste nasadili aktualizaci aplikace, která zahrnuté změně databáze použití databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f95b7-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="f95b7-185">Další informace</span><span class="sxs-lookup"><span data-stu-id="f95b7-185">More Information</span></span>

<span data-ttu-id="f95b7-186">Dokončení této série kurzů k nasazení do hostujícího zprostředkovatele třetí strany webovou aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f95b7-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="f95b7-187">Další informace o všech témat zahrnutých v těchto kurzech najdete v tématu [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="f95b7-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="f95b7-188">Potvrzení</span><span class="sxs-lookup"><span data-stu-id="f95b7-188">Acknowledgements</span></span>

<span data-ttu-id="f95b7-189">Děkujeme, že následující osob, které významně přispěli k obsahu tento kurz řady chci:</span><span class="sxs-lookup"><span data-stu-id="f95b7-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="f95b7-190">Alberto Poblacion, MVP &amp; MCT, Španělsko</span><span class="sxs-lookup"><span data-stu-id="f95b7-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="f95b7-191">Jarod Ferguson, vývoj platformy Data MVP, Spojené státy</span><span class="sxs-lookup"><span data-stu-id="f95b7-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="f95b7-192">Harsh Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="f95b7-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="f95b7-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="f95b7-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="f95b7-194">Jan Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="f95b7-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="f95b7-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="f95b7-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="f95b7-196">Raffaele Rialdi, Itálie</span><span class="sxs-lookup"><span data-stu-id="f95b7-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="f95b7-197">Rick Anderson Microsoft</span><span class="sxs-lookup"><span data-stu-id="f95b7-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="f95b7-198">[SAYED Hashimi společnost Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="f95b7-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="f95b7-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="f95b7-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="f95b7-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="f95b7-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="f95b7-201">Srđan Božović, Serbia</span><span class="sxs-lookup"><span data-stu-id="f95b7-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="f95b7-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="f95b7-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f95b7-203">[Předchozí](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [další](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="f95b7-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
