---
title: Přidat, stahování a odstraňování dat vlastní uživatele k identitě v projektu ASP.NET Core
author: rick-anderson
description: Zjistěte, jak přidat vlastní uživatelská data do Identity v projektu ASP.NET Core. Odstraníte data za GDPR.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
uid: security/authentication/add-user-data
ms.openlocfilehash: ecd0e6d1c71b24309fab70fbb06af7731463bb0e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271954"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="07f5d-104">Přidat, stahování a odstraňování dat vlastní uživatele k identitě v projektu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07f5d-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="07f5d-105">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="07f5d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="07f5d-106">Tento článek ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="07f5d-106">This article shows how to:</span></span>

* <span data-ttu-id="07f5d-107">Přidáte vlastní uživatelská data do webové aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07f5d-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="07f5d-108">Uspořádání vlastního uživatelského datového modelu s [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atributu, je automaticky dostupný pro stažení a odstranění.</span><span class="sxs-lookup"><span data-stu-id="07f5d-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="07f5d-109">Nastavení dat na moct stáhnout a odstranit pomáhá splňují [GDPR](xref:security/gdpr) požadavky.</span><span class="sxs-lookup"><span data-stu-id="07f5d-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="07f5d-110">Ukázkový projekt je vytvořený z stránky Razor webové aplikace, ale tyto pokyny jsou podobné pro webovou aplikaci ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="07f5d-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="07f5d-111">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="07f5d-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07f5d-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="07f5d-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="07f5d-113">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="07f5d-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="07f5d-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="07f5d-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="07f5d-115">Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="07f5d-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="07f5d-116">Název projektu **WebApp1** Pokud budete chtít odpovídat obor názvů [stažení ukázky](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) kódu.</span><span class="sxs-lookup"><span data-stu-id="07f5d-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="07f5d-117">Vyberte **webové aplikace ASP.NET Core** > **OK**</span><span class="sxs-lookup"><span data-stu-id="07f5d-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="07f5d-118">Vyberte **ASP.NET Core 2.1** v rozevírací nabídce</span><span class="sxs-lookup"><span data-stu-id="07f5d-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="07f5d-119">Vyberte **webovou aplikaci**  > **OK**</span><span class="sxs-lookup"><span data-stu-id="07f5d-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="07f5d-120">Sestavte a spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="07f5d-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="07f5d-121">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="07f5d-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="07f5d-122">Spustit Identity scaffolder</span><span class="sxs-lookup"><span data-stu-id="07f5d-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="07f5d-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="07f5d-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="07f5d-124">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt > **přidat** > **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="07f5d-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="07f5d-125">V levém podokně nástroje **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **Identity** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="07f5d-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="07f5d-126">V **přidat Identity** dialogové okno, následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="07f5d-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="07f5d-127">Vyberte existující soubor rozložení *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="07f5d-127">Select your existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="07f5d-128">Vyberte následující soubory k přepsání:</span><span class="sxs-lookup"><span data-stu-id="07f5d-128">Select the following files to override:</span></span>
    * <span data-ttu-id="07f5d-129">**Účet nebo registrace**</span><span class="sxs-lookup"><span data-stu-id="07f5d-129">**Account/Register**</span></span>
    * <span data-ttu-id="07f5d-130">**Účet nebo spravovat/indexu**</span><span class="sxs-lookup"><span data-stu-id="07f5d-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="07f5d-131">Vyberte **+** tlačítko pro vytvoření nového **třída kontextu dat**.</span><span class="sxs-lookup"><span data-stu-id="07f5d-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="07f5d-132">Přijměte typ (**WebApp1.Models.WebApp1Context** Pokud název projektu **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="07f5d-132">Accept the type (**WebApp1.Models.WebApp1Context** if you named the project **WebApp1**).</span></span>
  * <span data-ttu-id="07f5d-133">Vyberte **+** tlačítko pro vytvoření nového **třídu uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="07f5d-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="07f5d-134">Přijměte typ (**WebApp1User** Pokud název projektu **WebApp1**) > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="07f5d-134">Accept the type (**WebApp1User** if you named the project **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="07f5d-135">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="07f5d-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="07f5d-136">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="07f5d-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="07f5d-137">Pokud jste nenainstalovali dříve ASP.NET scaffolder, nainstalujte ji:</span><span class="sxs-lookup"><span data-stu-id="07f5d-137">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="07f5d-138">Přidat odkaz na balíček [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) k souboru projektu (.csproj).</span><span class="sxs-lookup"><span data-stu-id="07f5d-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="07f5d-139">Spusťte následující příkaz v adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="07f5d-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="07f5d-140">Spusťte následující příkaz, který zobrazí seznam možností scaffolder Identity:</span><span class="sxs-lookup"><span data-stu-id="07f5d-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="07f5d-141">Ve složce projektu spusťte Identity scaffolder:</span><span class="sxs-lookup"><span data-stu-id="07f5d-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="07f5d-142">Postupujte podle pokynů v [UseAuthentication, migrace a rozložení](xref:security/authentication/scaffold-identity#efm) provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="07f5d-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="07f5d-143">Vytvořte migrace a aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="07f5d-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="07f5d-144">Add `UseAuthentication` to `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="07f5d-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="07f5d-145">Přidat `<partial name="_LoginPartial" />` rozložení souboru.</span><span class="sxs-lookup"><span data-stu-id="07f5d-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="07f5d-146">Testovací aplikace:</span><span class="sxs-lookup"><span data-stu-id="07f5d-146">Test the app:</span></span>
  * <span data-ttu-id="07f5d-147">Registrace uživatele</span><span class="sxs-lookup"><span data-stu-id="07f5d-147">Register a user</span></span>
  * <span data-ttu-id="07f5d-148">Vyberte nové uživatelské jméno (vedle **odhlášení** odkaz).</span><span class="sxs-lookup"><span data-stu-id="07f5d-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="07f5d-149">Možná budete muset rozbalte okno nebo vyberte ikony navigačního panelu zobrazit uživatelské jméno a další odkazy.</span><span class="sxs-lookup"><span data-stu-id="07f5d-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="07f5d-150">Vyberte **osobní Data** kartě.</span><span class="sxs-lookup"><span data-stu-id="07f5d-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="07f5d-151">Vyberte **Stáhnout** tlačítko a vyšetřit *PersonalData.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="07f5d-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="07f5d-152">Testovací **odstranit** tlačítko, které odstraní přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="07f5d-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="07f5d-153">Přidat vlastní uživatelská data do databáze Identity</span><span class="sxs-lookup"><span data-stu-id="07f5d-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="07f5d-154">Aktualizace `IdentityUser` odvozené třídy s vlastní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="07f5d-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="07f5d-155">Pokud jste projekt WebApp1, je soubor s názvem *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="07f5d-155">If you named your project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="07f5d-156">Aktualizujte soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="07f5d-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="07f5d-157">Vlastnosti označených pomocí [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atribut jsou:</span><span class="sxs-lookup"><span data-stu-id="07f5d-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="07f5d-158">Odstranit, když *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* stránky Razor volá `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="07f5d-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="07f5d-159">Součástí stažená data pomocí *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="07f5d-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="07f5d-160">Account/Manage/Index.cshtml stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="07f5d-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="07f5d-161">Aktualizace `InputModel` v *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* s následujícími službami zvýrazněná kódu:</span><span class="sxs-lookup"><span data-stu-id="07f5d-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

<span data-ttu-id="07f5d-162">Aktualizace *Areas/Identity/Pages/Account/Manage/Index.cshtml* s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="07f5d-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="07f5d-163">Account/Register.cshtml stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="07f5d-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="07f5d-164">Aktualizace `InputModel` v *Areas/Identity/Pages/Account/Register.cshtml.cs* s následujícími službami zvýrazněná kódu:</span><span class="sxs-lookup"><span data-stu-id="07f5d-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="07f5d-165">Aktualizace *Areas/Identity/Pages/Account/Register.cshtml* s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="07f5d-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="07f5d-166">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="07f5d-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="07f5d-167">Přidat migrace pro vlastní data</span><span class="sxs-lookup"><span data-stu-id="07f5d-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="07f5d-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="07f5d-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="07f5d-169">V sadě Visual Studio **Konzola správce balíčků**:</span><span class="sxs-lookup"><span data-stu-id="07f5d-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="07f5d-170">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="07f5d-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="07f5d-171">Test vytvořit, zobrazit, stáhnout, odstranit vlastní uživatelská data</span><span class="sxs-lookup"><span data-stu-id="07f5d-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="07f5d-172">Testovací aplikace:</span><span class="sxs-lookup"><span data-stu-id="07f5d-172">Test the app:</span></span>

* <span data-ttu-id="07f5d-173">Registrace nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="07f5d-173">Register a new user.</span></span>
* <span data-ttu-id="07f5d-174">Zobrazení dat vlastní uživatele na `/Identity/Account/Manage` stránky.</span><span class="sxs-lookup"><span data-stu-id="07f5d-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="07f5d-175">Stáhnout a zobrazit osobní údaje uživatelů z `/Identity/Account/Manage/PersonalData` stránky.</span><span class="sxs-lookup"><span data-stu-id="07f5d-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
