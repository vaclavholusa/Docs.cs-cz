---
title: Přidání, stáhněte si a odstranit vlastní uživatelská data na identitu v projektu aplikace ASP.NET Core
author: rick-anderson
description: Zjistěte, jak přidat vlastní uživatelská data na identitu v projektu aplikace ASP.NET Core. Odstraníte data podle nařízení GDPR.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
uid: security/authentication/add-user-data
ms.openlocfilehash: 6f583d65460803c816bf1ccd314216952710cd55
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/31/2018
ms.locfileid: "39378612"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="a8b72-104">Přidání, stáhněte si a odstranit vlastní uživatelská data na identitu v projektu aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8b72-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="a8b72-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a8b72-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a8b72-106">Tento článek popisuje, jak:</span><span class="sxs-lookup"><span data-stu-id="a8b72-106">This article shows how to:</span></span>

* <span data-ttu-id="a8b72-107">Přidáte vlastní uživatelská data do webové aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8b72-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="a8b72-108">Uspořádání vlastního uživatele datového modelu o [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atribut tak, aby byl automaticky k dispozici ke stažení a odstranění.</span><span class="sxs-lookup"><span data-stu-id="a8b72-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="a8b72-109">Nastavení může ke stažení a odstranění dat pomáhá plnit [GDPR](xref:security/gdpr) požadavky.</span><span class="sxs-lookup"><span data-stu-id="a8b72-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="a8b72-110">Ukázkový projekt je vytvořený z webová aplikace Razor Pages, ale pokyny jsou podobné pro webovou aplikaci ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="a8b72-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="a8b72-111">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8b72-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8b72-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a8b72-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="a8b72-113">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="a8b72-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8b72-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8b72-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8b72-115">Ze sady Visual Studio **souboru** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="a8b72-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="a8b72-116">Pojmenujte projekt **WebApp1** Pokud budete chtít odpovídat oboru názvů [stáhněte si ukázky](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) kódu.</span><span class="sxs-lookup"><span data-stu-id="a8b72-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="a8b72-117">Vyberte **webová aplikace ASP.NET Core** > **OK**</span><span class="sxs-lookup"><span data-stu-id="a8b72-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="a8b72-118">Vyberte **ASP.NET Core 2.1** v rozevírací nabídce</span><span class="sxs-lookup"><span data-stu-id="a8b72-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="a8b72-119">Vyberte **webovou aplikaci**  > **OK**</span><span class="sxs-lookup"><span data-stu-id="a8b72-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="a8b72-120">Sestavte a spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="a8b72-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a8b72-121">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a8b72-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="a8b72-122">Spustit generátor Identity</span><span class="sxs-lookup"><span data-stu-id="a8b72-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8b72-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8b72-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8b72-124">Z **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt > **přidat** > **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="a8b72-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a8b72-125">V levém podokně **přidat vygenerované uživatelské rozhraní** dialogového okna, vyberte **Identity** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a8b72-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="a8b72-126">V **ADD Identity** dialogové okno, následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="a8b72-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="a8b72-127">Vyberte existující soubor rozložení *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a8b72-127">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="a8b72-128">Vyberte následující soubory, které chcete přepsat:</span><span class="sxs-lookup"><span data-stu-id="a8b72-128">Select the following files to override:</span></span>
    * <span data-ttu-id="a8b72-129">**Účtu/registrace**</span><span class="sxs-lookup"><span data-stu-id="a8b72-129">**Account/Register**</span></span>
    * <span data-ttu-id="a8b72-130">**Účet a správa/indexu**</span><span class="sxs-lookup"><span data-stu-id="a8b72-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="a8b72-131">Vyberte **+** tlačítko pro vytvoření nového **třída kontextu dat**.</span><span class="sxs-lookup"><span data-stu-id="a8b72-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="a8b72-132">Přijmout typ (**WebApp1.Models.WebApp1Context** Pokud je název projektu **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="a8b72-132">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="a8b72-133">Vyberte **+** tlačítko pro vytvoření nového **třídu uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="a8b72-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="a8b72-134">Přijmout typ (**WebApp1User** Pokud je název projektu **WebApp1**) > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a8b72-134">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="a8b72-135">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a8b72-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a8b72-136">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a8b72-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a8b72-137">Pokud jste nenainstalovali dříve generátor technologie ASP.NET, nainstalujte ho:</span><span class="sxs-lookup"><span data-stu-id="a8b72-137">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="a8b72-138">Přidat odkaz na balíček pro [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do souboru projektu (.csproj).</span><span class="sxs-lookup"><span data-stu-id="a8b72-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="a8b72-139">Spusťte následující příkaz v adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="a8b72-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="a8b72-140">Spusťte následující příkaz k výpisu možností generátor Identity:</span><span class="sxs-lookup"><span data-stu-id="a8b72-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="a8b72-141">Ve složce projektu spustit generátor Identity:</span><span class="sxs-lookup"><span data-stu-id="a8b72-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="a8b72-142">Postupujte podle pokynů v [migrace, UseAuthentication a rozložení](xref:security/authentication/scaffold-identity#efm) provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a8b72-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="a8b72-143">Vytvoření migrace a aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="a8b72-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="a8b72-144">Add `UseAuthentication` to `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a8b72-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="a8b72-145">Přidat `<partial name="_LoginPartial" />` do rozložení souboru.</span><span class="sxs-lookup"><span data-stu-id="a8b72-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="a8b72-146">Testování aplikace:</span><span class="sxs-lookup"><span data-stu-id="a8b72-146">Test the app:</span></span>
  * <span data-ttu-id="a8b72-147">Registrace uživatele</span><span class="sxs-lookup"><span data-stu-id="a8b72-147">Register a user</span></span>
  * <span data-ttu-id="a8b72-148">Vyberte nové uživatelské jméno (vedle **odhlášení** odkaz).</span><span class="sxs-lookup"><span data-stu-id="a8b72-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="a8b72-149">Můžete potřebovat rozbalte okno nebo vyberte ikonu navigačního panelu a zobrazí se uživatelské jméno a dalších odkazů.</span><span class="sxs-lookup"><span data-stu-id="a8b72-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="a8b72-150">Vyberte **osobní údaje** kartu.</span><span class="sxs-lookup"><span data-stu-id="a8b72-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="a8b72-151">Vyberte **Stáhnout** tlačítko a prověřit *PersonalData.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="a8b72-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="a8b72-152">Test **odstranit** tlačítko, které odstraní přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a8b72-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="a8b72-153">Přidat vlastní uživatelská data do databáze Identity</span><span class="sxs-lookup"><span data-stu-id="a8b72-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="a8b72-154">Aktualizace `IdentityUser` odvozené třídy s vlastními vlastnostmi.</span><span class="sxs-lookup"><span data-stu-id="a8b72-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="a8b72-155">Pokud jste projekt WebApp1, má název souboru *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="a8b72-155">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="a8b72-156">Aktualizace souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="a8b72-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="a8b72-157">Upravené vlastnosti pomocí [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atributu:</span><span class="sxs-lookup"><span data-stu-id="a8b72-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="a8b72-158">Odstranit, když *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* volá stránky Razor `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="a8b72-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="a8b72-159">Součástí staženého data podle *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="a8b72-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="a8b72-160">Aktualizovat stránku Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="a8b72-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="a8b72-161">Aktualizace `InputModel` v *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="a8b72-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

<span data-ttu-id="a8b72-162">Aktualizace *Areas/Identity/Pages/Account/Manage/Index.cshtml* s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="a8b72-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="a8b72-163">Aktualizovat stránku Account/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="a8b72-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="a8b72-164">Aktualizace `InputModel` v *Areas/Identity/Pages/Account/Register.cshtml.cs* s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="a8b72-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="a8b72-165">Aktualizace *Areas/Identity/Pages/Account/Register.cshtml* s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="a8b72-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="a8b72-166">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="a8b72-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="a8b72-167">Přidejte migraci pro vlastní uživatelská data</span><span class="sxs-lookup"><span data-stu-id="a8b72-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8b72-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8b72-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a8b72-169">V sadě Visual Studio **Konzola správce balíčků**:</span><span class="sxs-lookup"><span data-stu-id="a8b72-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a8b72-170">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a8b72-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="a8b72-171">Test vytvořit, zobrazit, stáhnout, odstranit vlastní uživatelská data</span><span class="sxs-lookup"><span data-stu-id="a8b72-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="a8b72-172">Testování aplikace:</span><span class="sxs-lookup"><span data-stu-id="a8b72-172">Test the app:</span></span>

* <span data-ttu-id="a8b72-173">Registrace nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="a8b72-173">Register a new user.</span></span>
* <span data-ttu-id="a8b72-174">Zobrazit vlastní uživatelské údaje o `/Identity/Account/Manage` stránky.</span><span class="sxs-lookup"><span data-stu-id="a8b72-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="a8b72-175">Stáhnout a zobrazit osobní data uživatelů z `/Identity/Account/Manage/PersonalData` stránky.</span><span class="sxs-lookup"><span data-stu-id="a8b72-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
