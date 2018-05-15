---
title: Opakovaně použitelné uživatelské rozhraní Razor v knihovny tříd ASP.NET Core
author: Rick-Anderson
description: Vysvětluje, jak vytvářet opakovaně použitelné uživatelské rozhraní Razor v knihovně tříd.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 44091ab8ab5d69a5975e191fd00fca1d1d957238
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/08/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="2d931-103">Vytvořte opakovaně použitelné uživatelské rozhraní v projektu knihovny tříd Razor ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2d931-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="2d931-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2d931-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2d931-105">Zobrazení syntaxe Razor, stránky, řadiče, modely stránky a datové modely se dají vytvářet do knihovny tříd Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="2d931-105">Razor views, pages, controllers, page models, and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="2d931-106">RCL můžete zabalené a znovu použít.</span><span class="sxs-lookup"><span data-stu-id="2d931-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="2d931-107">Aplikace může obsahovat RCL a přepsání, zobrazení a stránky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="2d931-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="2d931-108">Když najde zobrazení, částečná zobrazení nebo stránky Razor ve webové aplikaci a RCL, kód Razor (*.cshtml* souboru) ve webové aplikaci, dostane přednost.</span><span class="sxs-lookup"><span data-stu-id="2d931-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="2d931-109">Tato funkce vyžaduje [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="2d931-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="2d931-110">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2d931-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="2d931-111">Vytvoření knihovny tříd obsahující Razor uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="2d931-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d931-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d931-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2d931-113">Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="2d931-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="2d931-114">Vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2d931-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="2d931-115">Ověřte **ASP.NET Core 2.1** nebo novější je vybrána.</span><span class="sxs-lookup"><span data-stu-id="2d931-115">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="2d931-116">Vyberte **knihovny tříd Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d931-116">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2d931-117">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="2d931-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2d931-118">Z příkazového řádku, spusťte `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="2d931-118">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="2d931-119">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2d931-119">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="2d931-120">Další informace najdete v tématu [dotnet nové](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="2d931-120">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span>

------
<span data-ttu-id="2d931-121">Přidáte soubory Razor RCL.</span><span class="sxs-lookup"><span data-stu-id="2d931-121">Add Razor files to the RCL.</span></span>

<span data-ttu-id="2d931-122">Doporučujeme, abyste RCL obsahu přejděte v *oblasti* složky.</span><span class="sxs-lookup"><span data-stu-id="2d931-122">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="2d931-123">Odkazování na obsah knihovny Razor – třída</span><span class="sxs-lookup"><span data-stu-id="2d931-123">Referencing Razor Class Library content</span></span>

<span data-ttu-id="2d931-124">RCL lze odkazovat pomocí:</span><span class="sxs-lookup"><span data-stu-id="2d931-124">The RCL can be referenced by:</span></span>

* <span data-ttu-id="2d931-125">Balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="2d931-125">NuGet package.</span></span> <span data-ttu-id="2d931-126">V tématu [balíčky NuGet vytváření](/nuget/create-packages/creating-a-package) a [dotnet. Přidejte balíček](/dotnet/core/tools/dotnet-add-package) a [vytvoření a publikování balíčku NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="2d931-126">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="2d931-127">*{Název projektu} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="2d931-127">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="2d931-128">V tématu [dotnet – přidat odkaz](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="2d931-128">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

### <a name="partial-files-access-in-the-rcl"></a><span data-ttu-id="2d931-129">Částečné soubory přístup v RCL</span><span class="sxs-lookup"><span data-stu-id="2d931-129">Partial files access in the RCL</span></span>

<span data-ttu-id="2d931-130">Pro obsah mimo RCL runtime ASP.NET Core nehledá částečné soubory v RCL.</span><span class="sxs-lookup"><span data-stu-id="2d931-130">For content outside the RCL, the ASP.NET Core runtime does not search for partial files in the RCL.</span></span>

<span data-ttu-id="2d931-131">Například v ukázkové stahování *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* částečné zobrazení může **není** bude odkazovat ve *WebApp1\Pages\About.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="2d931-131">For example, in the sample download, the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view can **not** be referenced in *WebApp1\Pages\About.cshtml*.</span></span> <span data-ttu-id="2d931-132">Nicméně stránky v RCL ( *RazorUIClassLib /* **můžete** přístup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2d931-132">However, pages in the RCL ( *RazorUIClassLib/* **can** access *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="2d931-133">Návod: Vytvoření projektu knihovny tříd Razor a použít z projektu stránky Razor</span><span class="sxs-lookup"><span data-stu-id="2d931-133">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="2d931-134">Si můžete stáhnout [dokončený projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) a otestovat ji spíš než její vytvoření.</span><span class="sxs-lookup"><span data-stu-id="2d931-134">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="2d931-135">Stažení ukázkové obsahuje další kód a odkazy, které usnadňují testování projektu.</span><span class="sxs-lookup"><span data-stu-id="2d931-135">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="2d931-136">Můžete ponechat zpětné vazby v [potíže Githubu](https://github.com/aspnet/Docs/issues/6098) s komentáře na stažení ukázky a podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="2d931-136">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="2d931-137">Testování stažení aplikace</span><span class="sxs-lookup"><span data-stu-id="2d931-137">Test the download app</span></span>

<span data-ttu-id="2d931-138">Pokud nebyly staženy dokončené aplikace a by místo vytvoření projektu návod, pokračujte [další části](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="2d931-138">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d931-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d931-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2d931-140">Otevřete *.sln* souborů v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d931-140">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="2d931-141">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2d931-141">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2d931-142">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="2d931-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2d931-143">Z příkazového řádku v *rozhraní příkazového řádku* directory sestavení RCL a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2d931-143">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="2d931-144">Přesunout do *WebApp1* adresáře a spusťte aplikaci:</span><span class="sxs-lookup"><span data-stu-id="2d931-144">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="2d931-145">Postupujte podle pokynů v [WebApp1 testu](#test)</span><span class="sxs-lookup"><span data-stu-id="2d931-145">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="2d931-146">Vytvoření knihovny tříd Razor</span><span class="sxs-lookup"><span data-stu-id="2d931-146">Create a Razor Class Library</span></span>

<span data-ttu-id="2d931-147">V této části se vytvoří Razor třídu knihovny (RCL).</span><span class="sxs-lookup"><span data-stu-id="2d931-147">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="2d931-148">RCL jsou přidány soubory Razor.</span><span class="sxs-lookup"><span data-stu-id="2d931-148">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d931-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d931-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2d931-150">Vytvoření projektu RCL:</span><span class="sxs-lookup"><span data-stu-id="2d931-150">Create the RCL project:</span></span>

* <span data-ttu-id="2d931-151">Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="2d931-151">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="2d931-152">Vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2d931-152">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="2d931-153">Název aplikace **RazorUIClassLib**.</span><span class="sxs-lookup"><span data-stu-id="2d931-153">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="2d931-154">Ověřte **ASP.NET Core 2.1** nebo novější je vybrána.</span><span class="sxs-lookup"><span data-stu-id="2d931-154">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="2d931-155">Vyberte **knihovny tříd Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d931-155">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="2d931-156">Vytvoření stránky Razor webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="2d931-156">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="2d931-157">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení > **přidat** >  **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="2d931-157">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="2d931-158">Vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2d931-158">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="2d931-159">Název aplikace **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="2d931-159">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="2d931-160">Ověřte **ASP.NET Core 2.1** nebo novější je vybrána.</span><span class="sxs-lookup"><span data-stu-id="2d931-160">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="2d931-161">Vyberte **webovou aplikaci** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d931-161">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="2d931-162">Přidejte Razor soubory a složky, do projektu.</span><span class="sxs-lookup"><span data-stu-id="2d931-162">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="2d931-163">Přidání souboru nástroje Razor částečné zobrazení s názvem *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2d931-163">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="2d931-164">Nahraďte kód v *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2d931-164">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="2d931-165">Kopírování *soubor _ViewStart.cshtml* soubor z projekt WebApp1 *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2d931-165">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="2d931-166">[Viewstart](xref:mvc/views/layout#running-code-before-each-view) soubor je vyžadován pro použití rozložení stránky Razor projektu.</span><span class="sxs-lookup"><span data-stu-id="2d931-166">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2d931-167">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="2d931-167">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2d931-168">Z příkazového řádku spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2d931-168">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="2d931-169">Předchozí příkazy:</span><span class="sxs-lookup"><span data-stu-id="2d931-169">The preceding commands:</span></span>

* <span data-ttu-id="2d931-170">Vytvoří `RazorUIClassLib` knihovny tříd Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="2d931-170">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="2d931-171">Vytvoří stránku _Message Razor a přidává ji k RCL.</span><span class="sxs-lookup"><span data-stu-id="2d931-171">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="2d931-172">`-np` Parametr vytvoří stránku bez `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="2d931-172">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="2d931-173">Vytvoří [viewstart](xref:mvc/views/layout#running-code-before-each-view) souboru a přidá ho RCL.</span><span class="sxs-lookup"><span data-stu-id="2d931-173">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="2d931-174">Soubor viewstart je potřeba použít rozložení stránky Razor projektu (která je přidána v další části).</span><span class="sxs-lookup"><span data-stu-id="2d931-174">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="2d931-175">Aktualizace stránky Razor:</span><span class="sxs-lookup"><span data-stu-id="2d931-175">Update the Razor Pages:</span></span>

* <span data-ttu-id="2d931-176">Nahraďte kód v *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2d931-176">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="2d931-177">Nahraďte kód v *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2d931-177">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="2d931-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` je potřeba použít částečné zobrazení (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="2d931-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="2d931-179">Místo včetně `@addTagHelper` direktivy, můžete přidat *_ViewImports.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="2d931-179">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="2d931-180">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2d931-180">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="2d931-181">Další informace o viewimports najdete v tématu [import sdílených direktivy](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="2d931-181">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="2d931-182">Sestavení knihovny tříd se ověřit, zda že nejsou žádné chyby kompilátoru:</span><span class="sxs-lookup"><span data-stu-id="2d931-182">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="2d931-183">Výstup sestavení obsahuje *RazorUIClassLib.dll* a *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="2d931-183">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="2d931-184">*RazorUIClassLib.Views.dll* obsahuje kompilovaný obsah Razor.</span><span class="sxs-lookup"><span data-stu-id="2d931-184">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="2d931-185">Použití knihovny uživatelského rozhraní Razor z projektu stránky Razor</span><span class="sxs-lookup"><span data-stu-id="2d931-185">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d931-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d931-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2d931-187">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na **WebApp1** a vyberte **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="2d931-187">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="2d931-188">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na **WebApp1** a vyberte **sestavení závislosti** > **závislosti projektu**.</span><span class="sxs-lookup"><span data-stu-id="2d931-188">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="2d931-189">Zkontrolujte **RazorUIClassLib** jako závislost **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="2d931-189">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="2d931-190">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na **WebApp1** a vyberte **přidat** > **odkaz**.</span><span class="sxs-lookup"><span data-stu-id="2d931-190">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="2d931-191">V **správce odkazů** dialogové okno, zkontrolujte **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d931-191">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="2d931-192">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2d931-192">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2d931-193">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="2d931-193">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2d931-194">Vytvoření stránky Razor webové aplikace a řešení obsahující stránky Razor aplikace a knihovny tříd Razor:</span><span class="sxs-lookup"><span data-stu-id="2d931-194">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="2d931-195">Sestavení a spuštění webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="2d931-195">Build and run the web app:</span></span>

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="2d931-196">Test WebApp1</span><span class="sxs-lookup"><span data-stu-id="2d931-196">Test WebApp1</span></span>

<span data-ttu-id="2d931-197">Ověřte, zda že je používán knihovny tříd Razor uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2d931-197">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="2d931-198">Přejděte do `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="2d931-198">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="2d931-199">Přepsání, zobrazení, částečná zobrazení a stránky</span><span class="sxs-lookup"><span data-stu-id="2d931-199">Override views, partial views, and pages</span></span>

<span data-ttu-id="2d931-200">Když najde zobrazení, částečná zobrazení nebo stránky Razor ve webové aplikace a knihovny tříd Razor, kód Razor (*.cshtml* souboru) ve webové aplikaci, dostane přednost.</span><span class="sxs-lookup"><span data-stu-id="2d931-200">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="2d931-201">Například přidejte *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* k WebApp1, a Page1 v WebApp1 má přednost Page1in knihovny tříd Razor.</span><span class="sxs-lookup"><span data-stu-id="2d931-201">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="2d931-202">Při stahování ukázka přejmenovat *WebApp1, oblasti nebo MyFeature2* k *WebApp1, oblasti nebo MyFeature* k testování přednost před.</span><span class="sxs-lookup"><span data-stu-id="2d931-202">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="2d931-203">Kopírování *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* částečné zobrazení k *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2d931-203">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="2d931-204">Aktualizujte kód označující nové umístění.</span><span class="sxs-lookup"><span data-stu-id="2d931-204">Update the markup to indicate the new location.</span></span> <span data-ttu-id="2d931-205">Sestavení a spuštění aplikace k ověření, že verze aplikace s částečným je používán.</span><span class="sxs-lookup"><span data-stu-id="2d931-205">Build and run the app to verify the app's version of the partial is being used.</span></span>
