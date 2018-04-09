---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace databáze | Microsoft Docs'
author: tdykstra
description: Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 3020cfa19bf12f21c6d42a77ed257595431b4e86
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="add4e-103">Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace databáze</span><span class="sxs-lookup"><span data-stu-id="add4e-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="add4e-104">Podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="add4e-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="add4e-105">Stáhněte si úvodní projekt</span><span class="sxs-lookup"><span data-stu-id="add4e-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="add4e-106">Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="add4e-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="add4e-107">Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="add4e-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="add4e-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="add4e-108">Overview</span></span>

<span data-ttu-id="add4e-109">V tomto kurzu provedete změnu databáze a změny související kódu, testovat změny v sadě Visual Studio a pak nasadit aktualizace do prostředí testovací, přípravné nebo produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="add4e-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="add4e-110">Kurz nejprve ukazuje, jak aktualizovat databázi, která spravuje migrace Code First, a pak později ukazuje, jak aktualizace databáze pomocí zprostředkovatele dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="add4e-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="add4e-111">Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="add4e-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="add4e-112">Nasazení aktualizace databáze pomocí migrace Code First</span><span class="sxs-lookup"><span data-stu-id="add4e-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="add4e-113">V této části můžete přidat sloupec Datum narození `Person` základní třídu pro `Student` a `Instructor` entity.</span><span class="sxs-lookup"><span data-stu-id="add4e-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="add4e-114">Potom je aktualizovat stránky, která zobrazuje data lektorem tak, aby se zobrazí nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="add4e-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="add4e-115">Nakonec můžete nasadit změny pro testovací, přípravné nebo produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="add4e-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="add4e-116">Přidat sloupce do tabulky v databázi aplikace</span><span class="sxs-lookup"><span data-stu-id="add4e-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="add4e-117">V *ContosoUniversity.DAL* projekt, otevřete *Person.cs* a přidejte následující vlastnost na konci `Person` – třída (měla by existovat dvě zavřením složené závorky následující ji):</span><span class="sxs-lookup"><span data-stu-id="add4e-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="add4e-118">Potom aktualizujte `Seed` metoda, kterou it přináší hodnotu pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="add4e-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="add4e-119">Otevřete *Migrations\Configuration.cs* a nahraďte bloku kódu, který začíná `var instructors = new List<Instructor>` s následující blok kódu, který obsahuje informace o datu narození:</span><span class="sxs-lookup"><span data-stu-id="add4e-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="add4e-120">Sestavte řešení a pak otevřete **Konzola správce balíčků** okno.</span><span class="sxs-lookup"><span data-stu-id="add4e-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="add4e-121">Ujistěte se, že je stále ContosoUniversity.DAL vybrán jako **výchozí projekt**.</span><span class="sxs-lookup"><span data-stu-id="add4e-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="add4e-122">V **Konzola správce balíčků** vyberte **ContosoUniversity.DAL** jako **výchozí projekt**a potom zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="add4e-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="add4e-123">Po dokončení tohoto příkazu Visual Studio otevře soubor třídy, který definuje nový `DbMIgration` třída a v `Up` metoda vidíte kód, který vytvoří nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="add4e-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="add4e-124">`Up` Metoda vytvoří sloupec při implementaci změny a `Down` metoda odstraní sloupec při vrácení zpět změny.</span><span class="sxs-lookup"><span data-stu-id="add4e-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="add4e-126">Sestavte řešení a potom zadejte následující příkaz v **Konzola správce balíčků** okno (musí být vybrána projektu ContosoUniversity.DAL stále):</span><span class="sxs-lookup"><span data-stu-id="add4e-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="add4e-127">Rozhraní Entity Framework běží `Up` metoda a poté spustí `Seed` metoda.</span><span class="sxs-lookup"><span data-stu-id="add4e-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="add4e-128">Zobrazit nového sloupce na stránce vyučující</span><span class="sxs-lookup"><span data-stu-id="add4e-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="add4e-129">Otevřete v projektu ContosoUniversity *Instructors.aspx* a přidejte nové šablony pole, které chcete zobrazit datum narození.</span><span class="sxs-lookup"><span data-stu-id="add4e-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="add4e-130">Přidáte mezi těm, které jsou pro přiřazení přijetím datum a office:</span><span class="sxs-lookup"><span data-stu-id="add4e-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="add4e-131">(Pokud odsazení kód získá synchronizován, můžete stisknout CTRL-K a pak CTRL-D automaticky přeformátuje souboru.)</span><span class="sxs-lookup"><span data-stu-id="add4e-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="add4e-132">Spusťte aplikaci a klikněte na **vyučující** odkaz.</span><span class="sxs-lookup"><span data-stu-id="add4e-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="add4e-133">Při načtení stránky uvidíte, že má nové pole pro datum narození.</span><span class="sxs-lookup"><span data-stu-id="add4e-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Stránka vyučující s datum narození](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="add4e-135">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="add4e-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="add4e-136">Nasaďte aktualizaci databáze</span><span class="sxs-lookup"><span data-stu-id="add4e-136">Deploy the database update</span></span>

1. <span data-ttu-id="add4e-137">V **Průzkumníku řešení** vyberte ContosoUniversity projekt.</span><span class="sxs-lookup"><span data-stu-id="add4e-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="add4e-138">V **publikování webu jedním kliknutím** nástrojů, klikněte na tlačítko **Test** profil publikování a potom klikněte na **Publikovat Web**.</span><span class="sxs-lookup"><span data-stu-id="add4e-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="add4e-139">(Pokud je zakázána panelu nástrojů, vyberte projekt ContosoUniversity v **Průzkumníku řešení**.)</span><span class="sxs-lookup"><span data-stu-id="add4e-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="add4e-140">Visual Studio nasadí aktualizovanou aplikaci a prohlížeči se otevře na domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="add4e-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="add4e-141">Spustit **vyučující** a ověřte, že aktualizace byla úspěšně nasazena.</span><span class="sxs-lookup"><span data-stu-id="add4e-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="add4e-142">Když se aplikace pokusí o přístup k databázi pro tuto stránku, Code First aktualizace schématu databáze a spustí `Seed` metoda.</span><span class="sxs-lookup"><span data-stu-id="add4e-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="add4e-143">Při zobrazení stránky, uvidíte očekávané **datum narození** sloupce s daty v ní.</span><span class="sxs-lookup"><span data-stu-id="add4e-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="add4e-144">V **publikování webu jedním kliknutím** nástrojů, klikněte na tlačítko **pracovní** profil publikování a potom klikněte na **Publikovat Web**.</span><span class="sxs-lookup"><span data-stu-id="add4e-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="add4e-145">Spustit **vyučující** stránky v pracovní k ověření, že aktualizace byla úspěšně nasazena.</span><span class="sxs-lookup"><span data-stu-id="add4e-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="add4e-146">V **publikování webu jedním kliknutím** nástrojů, klikněte na tlačítko **produkční** profil publikování a potom klikněte na **Publikovat Web**.</span><span class="sxs-lookup"><span data-stu-id="add4e-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="add4e-147">Spustit **vyučující** stránky v produkčním prostředí, chcete-li ověřit, že aktualizace byla úspěšně nasazena.</span><span class="sxs-lookup"><span data-stu-id="add4e-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="add4e-148">Pro aktualizaci skutečné produkční aplikace, která zahrnuje změnu databáze by také běžně využít aplikace do režimu offline během nasazení pomocí *aplikace\_offline.htm*, jak jste viděli v předchozí kurzu.</span><span class="sxs-lookup"><span data-stu-id="add4e-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="add4e-149">Nasazení aktualizace databáze pomocí poskytovatele dbDacFx</span><span class="sxs-lookup"><span data-stu-id="add4e-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="add4e-150">V této části přidáte *komentáře* sloupec, který se *uživatele* tabulky v databázi členství a vytvořte stránku, která vám umožní zobrazit a upravit komentáře pro každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="add4e-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="add4e-151">Pak můžete nasadit změny pro testovací, přípravné nebo produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="add4e-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="add4e-152">Přidat sloupce do tabulky v databázi členství</span><span class="sxs-lookup"><span data-stu-id="add4e-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="add4e-153">V sadě Visual Studio otevřete **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="add4e-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="add4e-154">Rozbalte položku **(localdb) \v11.0**, rozbalte položku **databáze**, rozbalte položku **aspnet ContosoUniversity** (ne **aspnet. ContosoUniversity produkčnímu**) a potom rozbalte **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="add4e-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="add4e-155">Pokud nevidíte **(localdb) \v11.0** v části **systému SQL Server** uzel, klikněte pravým tlačítkem myši **systému SQL Server** uzel a klikněte na tlačítko **přidat SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="add4e-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="add4e-156">V **připojit k serveru** dialogové okno zadejte *(localdb) \v11.0* jako **název serveru**a potom klikněte na **Connect**.</span><span class="sxs-lookup"><span data-stu-id="add4e-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="add4e-157">Pokud nevidíte **aspnet ContosoUniversity**, spusťte projekt a přihlaste se pomocí *správce* přihlašovací údaje (heslo je *devpwd*) a potom aktualizujte  **Průzkumník objektů systému SQL Server** okno.</span><span class="sxs-lookup"><span data-stu-id="add4e-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="add4e-158">Klikněte pravým tlačítkem myši **uživatelé** tabulky a potom klikněte na **Návrhář zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="add4e-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Návrhář SSOX zobrazení](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="add4e-160">V návrháři, přidejte *komentáře* sloupce a nastavit jej jako *nvarchar(128)* a s povolenou hodnotou Null a potom klikněte na **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="add4e-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Přidáním sloupce komentáře](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="add4e-162">V **aktualizace databáze Preview** pole, klikněte na tlačítko **aktualizace databáze**.</span><span class="sxs-lookup"><span data-stu-id="add4e-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Aktualizace databáze Preview](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="add4e-164">Vytvořte stránku k zobrazení a úpravě nového sloupce</span><span class="sxs-lookup"><span data-stu-id="add4e-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="add4e-165">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **účet** složky ContosoUniversity projektu, klikněte na tlačítko **přidat**a potom klikněte na **nová položka** .</span><span class="sxs-lookup"><span data-stu-id="add4e-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="add4e-166">Vytvořte novou **webové formuláře pomocí stránka předlohy** a pojmenujte ji *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="add4e-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="add4e-167">Přijměte výchozí nastavení *Site.Master* souboru stránky předlohy.</span><span class="sxs-lookup"><span data-stu-id="add4e-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="add4e-168">Zkopírujte následující kód do `MainContent` `Content` – element (posledních 3 `Content` prvky):</span><span class="sxs-lookup"><span data-stu-id="add4e-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="add4e-169">Klikněte pravým tlačítkem myši *UserInfo.aspx* a klikněte na tlačítko **zobrazit v prohlížeči**.</span><span class="sxs-lookup"><span data-stu-id="add4e-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="add4e-170">Přihlaste se pomocí vaší *správce* přihlašovací údaje uživatele (heslo je *devpwd*) a přidejte některé komentáře pro uživatele k ověření, že stránka funguje správně.</span><span class="sxs-lookup"><span data-stu-id="add4e-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Stránka informací o uživateli](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="add4e-172">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="add4e-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="add4e-173">Nasaďte aktualizaci databáze</span><span class="sxs-lookup"><span data-stu-id="add4e-173">Deploy the database update</span></span>

<span data-ttu-id="add4e-174">Nasazení pomocí poskytovatele dbDacFx, stačí vybrat **aktualizace databáze** možnost v profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="add4e-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="add4e-175">Však pro počáteční nasazení při použití této možnosti můžete také nakonfigurovat spuštění některých dalších skriptů SQL: těch, které jsou stále v profilu a budete se muset zabránit znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="add4e-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="add4e-176">Otevřete **Publikovat Web** Průvodce pravým tlačítkem na projekt ContosoUniversity a kliknutím na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="add4e-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="add4e-177">Vyberte **Test** profilu.</span><span class="sxs-lookup"><span data-stu-id="add4e-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="add4e-178">Klikněte **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="add4e-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="add4e-179">V části **objekt DefaultConnection**, vyberte **aktualizace databáze**.</span><span class="sxs-lookup"><span data-stu-id="add4e-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="add4e-180">Zakažte další skripty, které jste nakonfigurovali pro počáteční nasazení:</span><span class="sxs-lookup"><span data-stu-id="add4e-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="add4e-181">Klikněte na tlačítko **konfigurovat aktualizace databáze**.</span><span class="sxs-lookup"><span data-stu-id="add4e-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="add4e-182">V **konfigurovat aktualizace databáze** dialogové okno, zrušte políček vedle *Grant.sql* a *aspnet. data dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="add4e-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="add4e-183">Klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="add4e-183">Click **Close**.</span></span>
6. <span data-ttu-id="add4e-184">Klikněte **Preview** kartě.</span><span class="sxs-lookup"><span data-stu-id="add4e-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="add4e-185">V části **databáze** a napravo od **objekt DefaultConnection**, klikněte na tlačítko **Preview databáze** odkaz.</span><span class="sxs-lookup"><span data-stu-id="add4e-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![K náhledu databáze](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="add4e-187">Okno náhledu zobrazí skript, který se spustí v cílové databázi, aby schéma této databáze odpovídat schématu zdrojové databáze.</span><span class="sxs-lookup"><span data-stu-id="add4e-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="add4e-188">Tento skript obsahuje příkaz ALTER TABLE, který přidá nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="add4e-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="add4e-189">Zavřít **náhledu databáze** dialogové okno a pak klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="add4e-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="add4e-190">Visual Studio nasadí aktualizovanou aplikaci a prohlížeči se otevře na domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="add4e-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="add4e-191">Spuštění stránky informací o uživateli (Přidat *Account/UserInfo.aspx* na adresu URL domovské stránce) Chcete-li ověřit, že aktualizace byla úspěšně nasazena.</span><span class="sxs-lookup"><span data-stu-id="add4e-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="add4e-192">Budete se muset přihlásit zadáním *správce* a *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="add4e-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="add4e-193">Data v tabulkách není nasazený ve výchozím nastavení a nenakonfigurovali skript nasazení do dat spustit, takže nebude možné najít komentář, který jste přidali v vývoj.</span><span class="sxs-lookup"><span data-stu-id="add4e-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="add4e-194">Teď můžete přidat komentář, nové v pracovním pro ověření, že tato změna byla nasazena do databáze a stránka funguje správně.</span><span class="sxs-lookup"><span data-stu-id="add4e-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="add4e-195">Postupujte stejným způsobem, k nasazení pro pracovní a provozní.</span><span class="sxs-lookup"><span data-stu-id="add4e-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="add4e-196">Nezapomeňte si zakázat další skripty.</span><span class="sxs-lookup"><span data-stu-id="add4e-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="add4e-197">Ve srovnání s profilem testovací jediným rozdílem je, že zakážete pouze jeden skript staging a produkční profily, protože byly nakonfigurované ke spuštění pouze *aspnet. produkčnímu data.sql*.</span><span class="sxs-lookup"><span data-stu-id="add4e-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="add4e-198">Správce a prodpwd jsou pověření pro pracovní a provozní.</span><span class="sxs-lookup"><span data-stu-id="add4e-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="add4e-199">Pro aktualizaci skutečné produkční aplikace, která zahrnuje změnu databáze by také běžně trvat aplikace do režimu offline během nasazení tím, že nahrajete *aplikace\_offline.htm* před publikováním a odstranění následně, jako jste viděli v [předchozí kurzu](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="add4e-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="add4e-200">Souhrn</span><span class="sxs-lookup"><span data-stu-id="add4e-200">Summary</span></span>

<span data-ttu-id="add4e-201">Nyní jste nasadili aktualizaci aplikace, která zahrnuté změnu databáze pomocí migrace Code First a poskytovatele dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="add4e-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Stránka vyučující s datum narození](deploying-a-database-update/_static/image8.png)

![Stránka informací o uživateli](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="add4e-204">V dalším kurzu se dozvíte, jak provést nasazení pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="add4e-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="add4e-205">[Předchozí](deploying-a-code-update.md)
> [další](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="add4e-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
