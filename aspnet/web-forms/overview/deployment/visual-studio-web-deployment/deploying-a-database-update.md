---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace databáze | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 37b996452dfa619ba1276a1aba562ed7efc579b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389186"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="35d33-103">Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace databáze</span><span class="sxs-lookup"><span data-stu-id="35d33-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="35d33-104">podle [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="35d33-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="35d33-105">Stáhnout počáteční projekt</span><span class="sxs-lookup"><span data-stu-id="35d33-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="35d33-106">V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="35d33-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="35d33-107">Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="35d33-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="35d33-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="35d33-108">Overview</span></span>

<span data-ttu-id="35d33-109">V tomto kurzu provedete změnu databáze a změny související kód, otestovat změny v sadě Visual Studio a pak Nasaďte aktualizaci do prostředí test, přípravném nebo produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="35d33-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="35d33-110">Tento kurz nejprve ukazuje, jak aktualizovat databázi, kterou spravuje migrace Code First a pak později ukazuje, jak pomocí poskytovatele dbDacFx aktualizace databáze.</span><span class="sxs-lookup"><span data-stu-id="35d33-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="35d33-111">Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="35d33-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="35d33-112">Nasazení aktualizace databáze pomocí migrace Code First</span><span class="sxs-lookup"><span data-stu-id="35d33-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="35d33-113">V této části přidáte sloupec Datum narození `Person` základní třída pro `Student` a `Instructor` entity.</span><span class="sxs-lookup"><span data-stu-id="35d33-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="35d33-114">Potom aktualizujte stránku, která zobrazuje data kurzů vedených tak, aby zobrazil nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="35d33-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="35d33-115">Nakonec nasadíte změny do testu, přípravném nebo produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="35d33-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="35d33-116">Přidání sloupce do tabulky v databázi aplikace</span><span class="sxs-lookup"><span data-stu-id="35d33-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="35d33-117">V *ContosoUniversity.DAL* projekt, otevřete *Person.cs* a přidejte následující vlastnost na konci `Person` třídy (měl by existovat dva pravou složenou závorkou následující):</span><span class="sxs-lookup"><span data-stu-id="35d33-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="35d33-118">Dále, aktualizujte `Seed` metodu tak, že poskytuje hodnoty pro nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="35d33-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="35d33-119">Otevřít *Migrations\Configuration.cs* a nahraďte blok kódu, který začíná `var instructors = new List<Instructor>` s následující blok kódu, který obsahuje informace o datu narození:</span><span class="sxs-lookup"><span data-stu-id="35d33-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="35d33-120">Sestavte řešení a pak otevřete **Konzola správce balíčků** okna.</span><span class="sxs-lookup"><span data-stu-id="35d33-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="35d33-121">Ujistěte se, že ContosoUniversity.DAL je stále vybrán jako **výchozí projekt**.</span><span class="sxs-lookup"><span data-stu-id="35d33-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="35d33-122">V **Konzola správce balíčků** okně **ContosoUniversity.DAL** jako **výchozí projekt**a potom zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="35d33-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="35d33-123">Po dokončení tohoto příkazu, Visual Studio otevře soubor třídy, která definuje nový `DbMIgration` třídy a `Up` metoda se zobrazí kód, který vytvoří nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="35d33-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="35d33-124">`Up` Metoda vytvoří sloupec při implementaci této změny a `Down` metoda odstraní sloupec při vrácení zpět změnu.</span><span class="sxs-lookup"><span data-stu-id="35d33-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="35d33-126">Sestavte řešení a potom zadejte následující příkaz v **Konzola správce balíčků** okno (ujistěte se, že je stále vybrán projekt ContosoUniversity.DAL):</span><span class="sxs-lookup"><span data-stu-id="35d33-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="35d33-127">Entity Framework běží `Up` metody a poté ji spustí `Seed` metoda.</span><span class="sxs-lookup"><span data-stu-id="35d33-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="35d33-128">Na stránce Instruktoři zobrazit nový sloupec</span><span class="sxs-lookup"><span data-stu-id="35d33-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="35d33-129">Otevřete v projektu ContosoUniversity *Instructors.aspx* a přidat nové pole šablony se zobrazí datum narození.</span><span class="sxs-lookup"><span data-stu-id="35d33-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="35d33-130">Přidáte mezi pro zařazení data a office přiřazení:</span><span class="sxs-lookup"><span data-stu-id="35d33-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="35d33-131">(Pokud odsazení kódu získá synchronizovaný, můžete stisknutím CTRL-K a pak na kombinaci kláves CTRL + D automaticky přeformátovat soubor.)</span><span class="sxs-lookup"><span data-stu-id="35d33-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="35d33-132">Spusťte aplikaci a klikněte na tlačítko **Instruktoři** odkaz.</span><span class="sxs-lookup"><span data-stu-id="35d33-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="35d33-133">Když se stránka načte, uvidíte, že na něm nové pole pro datum narození.</span><span class="sxs-lookup"><span data-stu-id="35d33-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Stránka Instruktoři s datum narození](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="35d33-135">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="35d33-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="35d33-136">Nasazení aktualizace databáze</span><span class="sxs-lookup"><span data-stu-id="35d33-136">Deploy the database update</span></span>

1. <span data-ttu-id="35d33-137">V **Průzkumníka řešení** vyberte ContosoUniversity projekt.</span><span class="sxs-lookup"><span data-stu-id="35d33-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="35d33-138">V **publikování webu jedním kliknutím** nástrojů, klikněte na tlačítko **testovací** profil publikování a potom klikněte na tlačítko **Publikovat Web**.</span><span class="sxs-lookup"><span data-stu-id="35d33-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="35d33-139">(Pokud panelu nástrojů je zakázaná, vyberte projekt ContosoUniversity v **Průzkumníka řešení**.)</span><span class="sxs-lookup"><span data-stu-id="35d33-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="35d33-140">Visual Studio nasadí aktualizované aplikace a prohlížeč otevře domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="35d33-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="35d33-141">Spustit **Instruktoři** stránku a ověřte, že se aktualizace úspěšně nasadilo.</span><span class="sxs-lookup"><span data-stu-id="35d33-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="35d33-142">Když se aplikace pokusí o přístup k databázi pro tuto stránku, Code First aktualizuje schéma databáze a spustí `Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="35d33-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="35d33-143">Jakmile se zobrazí na stránce se zobrazí očekávané **datum narození** sloupce s datem, v ní.</span><span class="sxs-lookup"><span data-stu-id="35d33-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="35d33-144">V **publikování webu jedním kliknutím** nástrojů, klikněte na tlačítko **pracovní** profil publikování a potom klikněte na tlačítko **Publikovat Web**.</span><span class="sxs-lookup"><span data-stu-id="35d33-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="35d33-145">Spustit **Instruktoři** stránky v přípravě, chcete-li ověřit, že se aktualizace úspěšně nasadilo.</span><span class="sxs-lookup"><span data-stu-id="35d33-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="35d33-146">V **publikování webu jedním kliknutím** nástrojů, klikněte na tlačítko **produkční** profil publikování a potom klikněte na tlačítko **Publikovat Web**.</span><span class="sxs-lookup"><span data-stu-id="35d33-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="35d33-147">Spustit **Instruktoři** stránky v produkčním prostředí k ověření, že se aktualizace úspěšně nasadilo.</span><span class="sxs-lookup"><span data-stu-id="35d33-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="35d33-148">Pro skutečné produkční aktualizaci aplikace, která zahrnuje změnu databáze by také běžně nastavit aplikaci offline během nasazení s použitím *aplikace\_offline.htm*, jak jste viděli v předchozím kurzu.</span><span class="sxs-lookup"><span data-stu-id="35d33-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="35d33-149">Nasazení aktualizace databáze s použitím dbDacFx poskytovatele</span><span class="sxs-lookup"><span data-stu-id="35d33-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="35d33-150">V této části přidáte *komentáře* sloupec, který se *uživatele* tabulky v databázi členství a vytvořit stránku, která vám umožní zobrazit a upravit komentáře pro každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="35d33-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="35d33-151">Pak můžete nasadit změny pro test, přípravném nebo produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="35d33-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="35d33-152">Přidání sloupce do tabulky v databázi členství</span><span class="sxs-lookup"><span data-stu-id="35d33-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="35d33-153">V sadě Visual Studio, otevřete **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="35d33-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="35d33-154">Rozbalte **(localdb) \v11.0**, rozbalte **databází**, rozbalte **aspnet ContosoUniversity** (ne **aspnet. ContosoUniversity Prod**) a potom rozbalte **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="35d33-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="35d33-155">Pokud nevidíte **(localdb) \v11.0** pod **systému SQL Server** uzel, klikněte pravým tlačítkem na **systému SQL Server** uzel a klikněte na tlačítko **přidat SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="35d33-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="35d33-156">V **připojit k serveru** dialogového okna zadejte *(localdb) \v11.0* jako **název serveru**a potom klikněte na tlačítko **připojit**.</span><span class="sxs-lookup"><span data-stu-id="35d33-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="35d33-157">Pokud nevidíte **aspnet ContosoUniversity**, spusťte projekt a přihlaste se pomocí *správce* přihlašovací údaje (heslo je *devpwd*) a potom aktualizovat  **Průzkumník objektů systému SQL Server** okna.</span><span class="sxs-lookup"><span data-stu-id="35d33-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="35d33-158">Klikněte pravým tlačítkem myši **uživatelé** tabulku a pak klikněte na **Návrhář zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="35d33-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Návrhář zobrazení SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="35d33-160">V návrháři, přidejte *komentáře* sloupec a nastavte ji *nvarchar(128)* a s povolenou hodnotou Null a potom klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="35d33-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Přidání sloupce komentáře](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="35d33-162">V **náhled aktualizací databáze** klikněte **aktualizace databáze**.</span><span class="sxs-lookup"><span data-stu-id="35d33-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Náhled aktualizací databáze](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="35d33-164">Vytvoření stránky můžete zobrazit a upravit nový sloupec</span><span class="sxs-lookup"><span data-stu-id="35d33-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="35d33-165">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **účet** klikněte na tlačítko složky v projektu ContosoUniversity **přidat**a potom klikněte na tlačítko **novou položku** .</span><span class="sxs-lookup"><span data-stu-id="35d33-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="35d33-166">Vytvořte nový **webové formuláře pomocí stránka předlohy** a pojmenujte ho *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="35d33-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="35d33-167">Přijměte výchozí nastavení *Site.Master* souboru stránky předlohy.</span><span class="sxs-lookup"><span data-stu-id="35d33-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="35d33-168">Zkopírujte následující kód do `MainContent` `Content` – element (poslední 3 `Content` prvky):</span><span class="sxs-lookup"><span data-stu-id="35d33-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="35d33-169">Klikněte pravým tlačítkem myši *UserInfo.aspx* stránky a klikněte na tlačítko **zobrazit v prohlížeči**.</span><span class="sxs-lookup"><span data-stu-id="35d33-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="35d33-170">Přihlášení vaše *správce* přihlašovací údaje uživatele (heslo je *devpwd*) a přidat nějaké komentáře k uživateli a ověřte, že na stránce funguje správně.</span><span class="sxs-lookup"><span data-stu-id="35d33-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Stránka informací o uživateli](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="35d33-172">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="35d33-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="35d33-173">Nasazení aktualizace databáze</span><span class="sxs-lookup"><span data-stu-id="35d33-173">Deploy the database update</span></span>

<span data-ttu-id="35d33-174">Pokud chcete nasadit s použitím poskytovatele dbDacFx, stačí vybrat **aktualizace databáze** možnost v profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="35d33-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="35d33-175">Ale pro počáteční nasazení při použití této možnosti můžete také nakonfigurovat některé další skripty SQL ke spuštění: to jsou stále v profilu a budete mít tak tomu, aby znovu.</span><span class="sxs-lookup"><span data-stu-id="35d33-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="35d33-176">Otevřít **Publikovat Web** Průvodce pravým tlačítkem myši projekt ContosoUniversity a kliknutím na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="35d33-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="35d33-177">Vyberte **Test** profilu.</span><span class="sxs-lookup"><span data-stu-id="35d33-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="35d33-178">Klikněte na tlačítko **nastavení** kartu.</span><span class="sxs-lookup"><span data-stu-id="35d33-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="35d33-179">V části **objekt DefaultConnection**vyberte **aktualizace databáze**.</span><span class="sxs-lookup"><span data-stu-id="35d33-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="35d33-180">Zakažte další skripty, které jste nakonfigurovali spuštění pro počáteční nasazení:</span><span class="sxs-lookup"><span data-stu-id="35d33-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="35d33-181">Klikněte na tlačítko **konfigurovat aktualizace databáze**.</span><span class="sxs-lookup"><span data-stu-id="35d33-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="35d33-182">V **konfigurace aktualizací databáze** dialogové okno, zrušte zaškrtnutí políček vedle *Grant.sql* a *aspnet-data-dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="35d33-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="35d33-183">Klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="35d33-183">Click **Close**.</span></span>
6. <span data-ttu-id="35d33-184">Klikněte na tlačítko **ve verzi Preview** kartu.</span><span class="sxs-lookup"><span data-stu-id="35d33-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="35d33-185">V části **databází** a napravo od **objekt DefaultConnection**, klikněte na tlačítko **databáze ve verzi Preview** odkaz.</span><span class="sxs-lookup"><span data-stu-id="35d33-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Database ve verzi Preview](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="35d33-187">V okně verze preview se zobrazí skript, který se spustí v cílové databázi, aby toto schéma databáze podle schématu zdrojové databáze.</span><span class="sxs-lookup"><span data-stu-id="35d33-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="35d33-188">Skript obsahuje pomocí příkazu ALTER TABLE, který přidá nový sloupec.</span><span class="sxs-lookup"><span data-stu-id="35d33-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="35d33-189">Zavřít **Database ve verzi Preview** dialogové okno a potom klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="35d33-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="35d33-190">Visual Studio nasadí aktualizované aplikace a prohlížeč otevře domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="35d33-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="35d33-191">Stránka informací o uživateli spustit (Přidat *Account/UserInfo.aspx* na adresu URL domovské stránky) Chcete-li ověřit, že se aktualizace úspěšně nasadilo.</span><span class="sxs-lookup"><span data-stu-id="35d33-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="35d33-192">Budete se muset přihlásit tak, že zadáte *správce* a *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="35d33-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="35d33-193">Ve výchozím nastavení není nasazený data v tabulkách a nenakonfigurovala skriptu nasazení dat. Pokud chcete spustit, nebude najdou komentář, který jste přidali ve vývoji.</span><span class="sxs-lookup"><span data-stu-id="35d33-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="35d33-194">Nyní můžete přidat nový komentář v přípravě, chcete-li ověřit, že tato změna byla nasazena do databáze a na stránce funguje správně.</span><span class="sxs-lookup"><span data-stu-id="35d33-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="35d33-195">Postupujte stejným způsobem pro nasazení do pracovního a produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="35d33-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="35d33-196">Nezapomeňte vypnout další skripty.</span><span class="sxs-lookup"><span data-stu-id="35d33-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="35d33-197">Ve srovnání s testovacího profilu jediným rozdílem je, že zakážete pouze jeden skript do pracovního a produkčního prostředí profily, protože byly nakonfigurované ke spuštění pouze *aspnet-prod-data.sql*.</span><span class="sxs-lookup"><span data-stu-id="35d33-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="35d33-198">Přihlašovací údaje pro pracovního a produkčního prostředí jsou správce a prodpwd.</span><span class="sxs-lookup"><span data-stu-id="35d33-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="35d33-199">Pro skutečné produkční aktualizaci aplikace, která zahrnuje změnu databáze by také běžně nastavit aplikaci offline během nasazení tak, že nahrajete *aplikace\_offline.htm* před publikováním a jeho odstranění následně, protože jste viděli v [předchozí kurz o službě](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="35d33-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="35d33-200">Souhrn</span><span class="sxs-lookup"><span data-stu-id="35d33-200">Summary</span></span>

<span data-ttu-id="35d33-201">Nasadili jste nyní zahrnující Změna databáze pomocí migrace Code First a poskytovateli dbDacFx aktualizace aplikace.</span><span class="sxs-lookup"><span data-stu-id="35d33-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Stránka Instruktoři s datum narození](deploying-a-database-update/_static/image8.png)

![Stránka informací o uživateli](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="35d33-204">V dalším kurzu se dozvíte, jak spustit nasazení pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="35d33-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="35d33-205">[Předchozí](deploying-a-code-update.md)
> [další](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="35d33-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
