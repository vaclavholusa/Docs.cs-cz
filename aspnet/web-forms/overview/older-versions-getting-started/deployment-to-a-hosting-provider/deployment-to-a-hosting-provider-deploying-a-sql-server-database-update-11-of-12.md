---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace databáze SQL serveru – 11 12 | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: c744bc54c8ce12820d1e1388ac7ab2db814ff031
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816625"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="1c152-103">Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace databáze SQL serveru – 11, 12</span><span class="sxs-lookup"><span data-stu-id="1c152-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="1c152-104">podle [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1c152-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="1c152-105">Stáhnout počáteční projekt</span><span class="sxs-lookup"><span data-stu-id="1c152-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="1c152-106">Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web.</span><span class="sxs-lookup"><span data-stu-id="1c152-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="1c152-107">Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web.</span><span class="sxs-lookup"><span data-stu-id="1c152-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="1c152-108">Úvod do řady, naleznete v tématu [z prvního kurzu této série](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="1c152-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="1c152-109">Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do modelu weby Windows Azure, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1c152-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="1c152-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="1c152-110">Overview</span></span>

<span data-ttu-id="1c152-111">V tomto kurzu se dozvíte, jak nasadit aktualizace databáze na úplné databáze serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1c152-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="1c152-112">Vzhledem k tomu, že migrace Code First vykonává všechnu práci aktualizace databáze, je téměř shodné s co jste to udělali pro SQL Server Compact v procesu [nasazení aktualizace databáze](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="1c152-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="1c152-113">Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="1c152-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="1c152-114">Přidá sloupec do tabulky</span><span class="sxs-lookup"><span data-stu-id="1c152-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="1c152-115">V této části kurzu provede změnit databázi a odpovídající změny kódu, otestujete je v sadě Visual Studio při přípravě na nasazení do testovacího a produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c152-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="1c152-116">Tato změna vyžaduje přidání `OfficeHours` sloupec, který se `Instructor` entity a zobrazení nových informací v **Instruktoři** webové stránky.</span><span class="sxs-lookup"><span data-stu-id="1c152-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="1c152-117">Otevřete v projektu ContosoUniversity.DAL *Instructor.cs* a přidejte následující vlastnost mezi `HireDate` a `Courses` vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="1c152-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="1c152-118">Aktualizace inicializátor třídy tak, aby se nasazení nasazuje nový sloupec s daty testu.</span><span class="sxs-lookup"><span data-stu-id="1c152-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="1c152-119">Otevřít *Migrations\Configuration.cs* a nahraďte blok kódu, který začíná `var instructors = new List<Instructor>` s následující blok kódu, který obsahuje nový sloupec:</span><span class="sxs-lookup"><span data-stu-id="1c152-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="1c152-120">Otevřete v projektu ContosoUniversity *Instructors.aspx* a přidat nové pole šablony pro office hodin těsně před uzavírací `</Columns>` značky v prvním `GridView` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="1c152-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="1c152-121">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="1c152-121">Build the solution.</span></span>

<span data-ttu-id="1c152-122">Otevřít **Konzola správce balíčků** okna a vyberte ContosoUniversity.DAL jako **výchozí projekt**.</span><span class="sxs-lookup"><span data-stu-id="1c152-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="1c152-123">Zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1c152-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="1c152-124">Spusťte aplikaci a vyberte **Instruktoři** stránky.</span><span class="sxs-lookup"><span data-stu-id="1c152-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="1c152-125">Na stránce trvá o něco déle než obvykle. Chcete načíst, protože rozhraní Entity Framework znovu vytvoří databázi a nasazení se nasazuje s testovací data.</span><span class="sxs-lookup"><span data-stu-id="1c152-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="1c152-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1c152-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="1c152-127">Nasazení aktualizace databáze do testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="1c152-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="1c152-128">Při použití migrace Code First metodu pro nasazení změn databáze na SQL serveru je stejná jako SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="1c152-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="1c152-129">Nicméně je nutné změnit testovací profil publikování, protože stále byla nastavená pro migraci ze systému SQL Server Compact do systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1c152-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="1c152-130">Prvním krokem je odebrat transformace řetězec připojení, které jste vytvořili v předchozím kurzu.</span><span class="sxs-lookup"><span data-stu-id="1c152-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="1c152-131">Tyto jsou už je nepotřebujete vzhledem k tomu, že zadáte transformace připojovací řetězec do profilu publikování, jako jste to udělali dříve, než jste nakonfigurovali **balení/publikování kódu SQL** kartu pro migraci na SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1c152-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="1c152-132">Otevřít *Web.Test.config* souboru a odebrat `connectionStrings` elementu.</span><span class="sxs-lookup"><span data-stu-id="1c152-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="1c152-133">Jediný zbývající transformace v *Web.Test.config* je soubor `Environment` hodnota v `appSettings` elementu.</span><span class="sxs-lookup"><span data-stu-id="1c152-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="1c152-134">Nyní můžete aktualizovat profil publikování a publikovat do testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c152-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="1c152-135">Otevřít **Publikovat Web** průvodce a pak přepnout do **profilu** kartu.</span><span class="sxs-lookup"><span data-stu-id="1c152-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="1c152-136">Vyberte **Test** profil publikování.</span><span class="sxs-lookup"><span data-stu-id="1c152-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="1c152-137">Vyberte **nastavení** kartu.</span><span class="sxs-lookup"><span data-stu-id="1c152-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="1c152-138">Klikněte na tlačítko **povolit nová vylepšení publikování databáze**.</span><span class="sxs-lookup"><span data-stu-id="1c152-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="1c152-139">V poli připojovací řetězec pro **SchoolContext**, zadejte stejnou hodnotu, která jste použili v *Web.Test.config* transformační soubor v předchozím kurzu:</span><span class="sxs-lookup"><span data-stu-id="1c152-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="1c152-140">Vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**.</span><span class="sxs-lookup"><span data-stu-id="1c152-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="1c152-141">(Ve vaší verzi sady Visual Studio může být označené jako zaškrtávací políčko **použít migrace Code First**.)</span><span class="sxs-lookup"><span data-stu-id="1c152-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="1c152-142">V poli připojovací řetězec pro **objekt DefaultConnection**, zadejte stejnou hodnotu, která jste použili v *Web.Test.config* transformační soubor v předchozím kurzu:</span><span class="sxs-lookup"><span data-stu-id="1c152-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="1c152-143">Ponechte **aktualizace databáze** vymazána.</span><span class="sxs-lookup"><span data-stu-id="1c152-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="1c152-144">Klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="1c152-144">Click **Publish**.</span></span>

<span data-ttu-id="1c152-145">Visual Studio nasadí změny kódu do testovacího prostředí a otevře prohlížeč na domovskou stránku společnosti Contoso University.</span><span class="sxs-lookup"><span data-stu-id="1c152-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="1c152-146">Vyberte stránku Instruktoři.</span><span class="sxs-lookup"><span data-stu-id="1c152-146">Select the Instructors page.</span></span>

<span data-ttu-id="1c152-147">Při spuštění aplikace na této stránce, pokusí se o přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="1c152-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="1c152-148">Migrace Code First zkontroluje, jestli je aktuální databáze a zjistí, že zatím nebyla použita nejnovější migrace.</span><span class="sxs-lookup"><span data-stu-id="1c152-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="1c152-149">Migrace Code First použije nejnovější migrace, spustí `Seed` metoda a pak na stránce běží normálně.</span><span class="sxs-lookup"><span data-stu-id="1c152-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="1c152-150">Zobrazí se nový sloupec Office Hours s dosazená data.</span><span class="sxs-lookup"><span data-stu-id="1c152-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="1c152-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1c152-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="1c152-152">Nasazení aktualizace databáze do produkčního prostředí</span><span class="sxs-lookup"><span data-stu-id="1c152-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="1c152-153">Budete muset změnit také profil publikování pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c152-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="1c152-154">V tomto případě budete odeberte existující profil a vytvořit nové importováním souboru .publishsettings aktualizované.</span><span class="sxs-lookup"><span data-stu-id="1c152-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="1c152-155">Aktualizovaný soubor bude obsahovat připojovací řetězec pro databázi serveru SQL Server na Cytanium.</span><span class="sxs-lookup"><span data-stu-id="1c152-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="1c152-156">Jak jste viděli, kdy jste nasadili do testovacího prostředí, které už nepotřebujete transformace řetězec připojení v *Web.Production.config* transformačního souboru.</span><span class="sxs-lookup"><span data-stu-id="1c152-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="1c152-157">Otevřete soubor a odebrat `connectionStrings` elementu.</span><span class="sxs-lookup"><span data-stu-id="1c152-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="1c152-158">Zbývající transformace jsou pro `Environment` hodnotu `appSettings` element a `location` element, který omezuje přístup k Elmah zprávy o chybách.</span><span class="sxs-lookup"><span data-stu-id="1c152-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="1c152-159">Než vytvoříte nový profil publikování pro produkční prostředí, stažení souboru .publishsettings aktualizované stejně jako jste to udělali dříve v [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="1c152-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="1c152-160">(V Ovládacích panelech Cytanium, klikněte na tlačítko **weby**a potom klikněte na tlačítko **contosouniversity.com** webu.</span><span class="sxs-lookup"><span data-stu-id="1c152-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="1c152-161">Vyberte **publikování na webu** kartu a potom klikněte na tlačítko **stáhnout profil publikování pro tento web**.) Z důvodů, proč to děláte je ke sbírání připojovací řetězec databáze v souboru .publishsettings.</span><span class="sxs-lookup"><span data-stu-id="1c152-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="1c152-162">Připojovací řetězec nebyl k dispozici při prvním stažený soubor, protože stále používali SQL Server Compact a kdyby databáze serveru SQL Server na Cytanium zatím nevytvořili.</span><span class="sxs-lookup"><span data-stu-id="1c152-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="1c152-163">Nyní můžete aktualizovat profil publikování a publikovat do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c152-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="1c152-164">Otevřít **Publikovat Web** průvodce a pak přepnout do **profilu** kartu.</span><span class="sxs-lookup"><span data-stu-id="1c152-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="1c152-165">Klikněte na tlačítko **spravovat profily**a pak odstranit profil produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c152-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="1c152-166">Zavřít **publikování webu** průvodce a uložte tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="1c152-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="1c152-167">Otevřít **Publikovat Web** průvodce znovu a pak klikněte na tlačítko **Import**.</span><span class="sxs-lookup"><span data-stu-id="1c152-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="1c152-168">Na **připojení** kartu, změňte **cílovou adresu URL** na odpovídající hodnotu, pokud používáte dočasné adresy URL.</span><span class="sxs-lookup"><span data-stu-id="1c152-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="1c152-169">Klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1c152-169">Click **Next**.</span></span>

<span data-ttu-id="1c152-170">Na **nastavení** klikněte na tlačítko **povolit nová vylepšení publikování databáze**.</span><span class="sxs-lookup"><span data-stu-id="1c152-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="1c152-171">V rozevíracím seznamu připojovací řetězec pro **SchoolContext**, vyberte Cytanium připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="1c152-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="1c152-173">Vyberte **provést Code First (spuštěno při spuštění aplikace)**.</span><span class="sxs-lookup"><span data-stu-id="1c152-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="1c152-174">V rozevíracím seznamu připojovací řetězec pro **objekt DefaultConnection**, vyberte Cytanium připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="1c152-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="1c152-175">Vyberte **profilu** klikněte na tlačítko **spravovat profily**a přejmenování profilu "Produkční" z "contosouniversity.com – nástroj nasazení webu".</span><span class="sxs-lookup"><span data-stu-id="1c152-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="1c152-176">Profil publikování se uložit změnu zavřít a znovu ji spusťte.</span><span class="sxs-lookup"><span data-stu-id="1c152-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="1c152-177">Klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="1c152-177">Click **Publish**.</span></span> <span data-ttu-id="1c152-178">(Pro skutečné produkční webovou stránku, zkopírujte *aplikace\_offline.htm* do produkčních a vložit ho do složky projektu před publikováním, odeberte ho po dokončení nasazení.)</span><span class="sxs-lookup"><span data-stu-id="1c152-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="1c152-179">Visual Studio nasadí změny kódu do testovacího prostředí a otevře prohlížeč na domovskou stránku společnosti Contoso University.</span><span class="sxs-lookup"><span data-stu-id="1c152-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="1c152-180">Vyberte stránku Instruktoři.</span><span class="sxs-lookup"><span data-stu-id="1c152-180">Select the Instructors page.</span></span>

<span data-ttu-id="1c152-181">Migrace Code First aktualizuje databázi stejným způsobem jako fungovala v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c152-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="1c152-182">Zobrazí se nový sloupec Office Hours s dosazená data.</span><span class="sxs-lookup"><span data-stu-id="1c152-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="1c152-184">Teď úspěšně jste nasadili aktualizaci aplikace, která zahrnuté změny databáze, použití databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1c152-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="1c152-185">Další informace</span><span class="sxs-lookup"><span data-stu-id="1c152-185">More Information</span></span>

<span data-ttu-id="1c152-186">Dokončení tohoto postupu Tato série kurzů o nasazení webové aplikace ASP.NET do poskytovatele hostitelských služeb třetích stran.</span><span class="sxs-lookup"><span data-stu-id="1c152-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="1c152-187">Další informace o některém z témat uvedených v následujících kurzech najdete v článku [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) na webové stránce MSDN.</span><span class="sxs-lookup"><span data-stu-id="1c152-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="1c152-188">Potvrzení</span><span class="sxs-lookup"><span data-stu-id="1c152-188">Acknowledgements</span></span>

<span data-ttu-id="1c152-189">Chci vám chceme poděkovat následující uživatelů, kteří provedli významné příspěvky k obsahu v této sérii kurzů:</span><span class="sxs-lookup"><span data-stu-id="1c152-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="1c152-190">Alberto Poblacion, MVP &amp; MCT, Španělsko</span><span class="sxs-lookup"><span data-stu-id="1c152-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="1c152-191">Jarod Ferguson, vývoj pro platformu dat MVP, Spojené státy</span><span class="sxs-lookup"><span data-stu-id="1c152-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="1c152-192">Nepříznivých Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="1c152-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="1c152-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="1c152-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="1c152-194">Mike Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="1c152-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="1c152-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="1c152-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="1c152-196">Raffaele Rialdi, Itálie</span><span class="sxs-lookup"><span data-stu-id="1c152-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="1c152-197">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="1c152-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="1c152-198">[SAYED Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="1c152-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="1c152-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="1c152-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="1c152-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="1c152-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="1c152-201">Srđan Božović, Srbsko</span><span class="sxs-lookup"><span data-stu-id="1c152-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="1c152-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="1c152-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1c152-203">[Předchozí](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [další](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="1c152-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
