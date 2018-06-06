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
ms.openlocfilehash: 4370fdaf7a5c066cec7b341a6012a100f8aed3ea
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734546"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="33b46-103">Vytvořte opakovaně použitelné uživatelské rozhraní v projektu knihovny tříd Razor ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="33b46-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="33b46-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="33b46-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="33b46-105">Zobrazení syntaxe Razor, stránky, řadiče, stránka modely [zobrazení součásti](xref:mvc/views/view-components), a datové modely jde integrovat přímo do knihovny tříd Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="33b46-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="33b46-106">RCL můžete zabalené a znovu použít.</span><span class="sxs-lookup"><span data-stu-id="33b46-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="33b46-107">Aplikace může obsahovat RCL a přepsání, zobrazení a stránky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="33b46-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="33b46-108">Když najde zobrazení, částečná zobrazení nebo stránky Razor ve webové aplikaci a RCL, kód Razor (*.cshtml* souboru) ve webové aplikaci, dostane přednost.</span><span class="sxs-lookup"><span data-stu-id="33b46-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="33b46-109">Tato funkce vyžaduje [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="33b46-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="33b46-110">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="33b46-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="33b46-111">Vytvoření knihovny tříd obsahující Razor uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="33b46-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="33b46-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33b46-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="33b46-113">Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="33b46-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="33b46-114">Vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="33b46-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="33b46-115">Název knihovny (například "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="33b46-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="33b46-116">Abyste se vyhnuli kolize názvů souboru s knihovnou generované zobrazení, ujistěte se, není název knihovny končit `.Views`.</span><span class="sxs-lookup"><span data-stu-id="33b46-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="33b46-117">Ověřte **ASP.NET Core 2.1** nebo novější je vybrána.</span><span class="sxs-lookup"><span data-stu-id="33b46-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="33b46-118">Vyberte **knihovny tříd Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="33b46-118">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="33b46-119">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="33b46-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="33b46-120">Z příkazového řádku, spusťte `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="33b46-120">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="33b46-121">Příklad:</span><span class="sxs-lookup"><span data-stu-id="33b46-121">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="33b46-122">Další informace najdete v tématu [dotnet nové](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="33b46-122">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="33b46-123">Abyste se vyhnuli kolize názvů souboru s knihovnou generované zobrazení, ujistěte se, není název knihovny končit `.Views`.</span><span class="sxs-lookup"><span data-stu-id="33b46-123">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

------
<span data-ttu-id="33b46-124">Přidáte soubory Razor RCL.</span><span class="sxs-lookup"><span data-stu-id="33b46-124">Add Razor files to the RCL.</span></span>

<span data-ttu-id="33b46-125">Doporučujeme, abyste RCL obsahu přejděte v *oblasti* složky.</span><span class="sxs-lookup"><span data-stu-id="33b46-125">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="33b46-126">Odkazování na obsah knihovny Razor – třída</span><span class="sxs-lookup"><span data-stu-id="33b46-126">Referencing Razor Class Library content</span></span>

<span data-ttu-id="33b46-127">RCL lze odkazovat pomocí:</span><span class="sxs-lookup"><span data-stu-id="33b46-127">The RCL can be referenced by:</span></span>

* <span data-ttu-id="33b46-128">Balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="33b46-128">NuGet package.</span></span> <span data-ttu-id="33b46-129">V tématu [balíčky NuGet vytváření](/nuget/create-packages/creating-a-package) a [dotnet. Přidejte balíček](/dotnet/core/tools/dotnet-add-package) a [vytvoření a publikování balíčku NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="33b46-129">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="33b46-130">*{Název projektu} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="33b46-130">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="33b46-131">V tématu [dotnet – přidat odkaz](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="33b46-131">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

### <a name="partial-files-access-in-the-rcl"></a><span data-ttu-id="33b46-132">Částečné soubory přístup v RCL</span><span class="sxs-lookup"><span data-stu-id="33b46-132">Partial files access in the RCL</span></span>

<span data-ttu-id="33b46-133">Pro obsah mimo RCL runtime ASP.NET Core nehledá částečné soubory v RCL.</span><span class="sxs-lookup"><span data-stu-id="33b46-133">For content outside the RCL, the ASP.NET Core runtime does not search for partial files in the RCL.</span></span>

<span data-ttu-id="33b46-134">Například v ukázkové stahování *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* částečné zobrazení může **není** bude odkazovat ve *WebApp1\Pages\About.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="33b46-134">For example, in the sample download, the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view can **not** be referenced in *WebApp1\Pages\About.cshtml*.</span></span> <span data-ttu-id="33b46-135">Nicméně stránky v RCL ( *RazorUIClassLib /* **můžete** přístup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="33b46-135">However, pages in the RCL ( *RazorUIClassLib/* **can** access *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="33b46-136">Návod: Vytvoření projektu knihovny tříd Razor a použít z projektu stránky Razor</span><span class="sxs-lookup"><span data-stu-id="33b46-136">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="33b46-137">Si můžete stáhnout [dokončený projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) a otestovat ji spíš než její vytvoření.</span><span class="sxs-lookup"><span data-stu-id="33b46-137">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="33b46-138">Stažení ukázkové obsahuje další kód a odkazy, které usnadňují testování projektu.</span><span class="sxs-lookup"><span data-stu-id="33b46-138">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="33b46-139">Můžete ponechat zpětné vazby v [potíže Githubu](https://github.com/aspnet/Docs/issues/6098) s komentáře na stažení ukázky a podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="33b46-139">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="33b46-140">Testování stažení aplikace</span><span class="sxs-lookup"><span data-stu-id="33b46-140">Test the download app</span></span>

<span data-ttu-id="33b46-141">Pokud nebyly staženy dokončené aplikace a by místo vytvoření projektu návod, pokračujte [další části](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="33b46-141">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="33b46-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33b46-142">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="33b46-143">Otevřete *.sln* souborů v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33b46-143">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="33b46-144">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="33b46-144">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="33b46-145">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="33b46-145">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="33b46-146">Z příkazového řádku v *rozhraní příkazového řádku* directory sestavení RCL a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="33b46-146">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="33b46-147">Přesunout do *WebApp1* adresáře a spusťte aplikaci:</span><span class="sxs-lookup"><span data-stu-id="33b46-147">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="33b46-148">Postupujte podle pokynů v [WebApp1 testu](#test)</span><span class="sxs-lookup"><span data-stu-id="33b46-148">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="33b46-149">Vytvoření knihovny tříd Razor</span><span class="sxs-lookup"><span data-stu-id="33b46-149">Create a Razor Class Library</span></span>

<span data-ttu-id="33b46-150">V této části se vytvoří Razor třídu knihovny (RCL).</span><span class="sxs-lookup"><span data-stu-id="33b46-150">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="33b46-151">RCL jsou přidány soubory Razor.</span><span class="sxs-lookup"><span data-stu-id="33b46-151">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="33b46-152">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33b46-152">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="33b46-153">Vytvoření projektu RCL:</span><span class="sxs-lookup"><span data-stu-id="33b46-153">Create the RCL project:</span></span>

* <span data-ttu-id="33b46-154">Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="33b46-154">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="33b46-155">Vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="33b46-155">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="33b46-156">Název aplikace **RazorUIClassLib**.</span><span class="sxs-lookup"><span data-stu-id="33b46-156">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="33b46-157">Ověřte **ASP.NET Core 2.1** nebo novější je vybrána.</span><span class="sxs-lookup"><span data-stu-id="33b46-157">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="33b46-158">Vyberte **knihovny tříd Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="33b46-158">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="33b46-159">Vytvoření stránky Razor webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="33b46-159">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="33b46-160">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení > **přidat** >  **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="33b46-160">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="33b46-161">Vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="33b46-161">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="33b46-162">Název aplikace **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="33b46-162">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="33b46-163">Ověřte **ASP.NET Core 2.1** nebo novější je vybrána.</span><span class="sxs-lookup"><span data-stu-id="33b46-163">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="33b46-164">Vyberte **webovou aplikaci** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="33b46-164">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="33b46-165">Přidejte Razor soubory a složky, do projektu.</span><span class="sxs-lookup"><span data-stu-id="33b46-165">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="33b46-166">Přidání souboru nástroje Razor částečné zobrazení s názvem *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="33b46-166">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="33b46-167">Nahraďte kód v *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="33b46-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="33b46-168">Kopírování *soubor _ViewStart.cshtml* soubor z projekt WebApp1 *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="33b46-168">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="33b46-169">[Viewstart](xref:mvc/views/layout#running-code-before-each-view) soubor je vyžadován pro použití rozložení stránky Razor projektu.</span><span class="sxs-lookup"><span data-stu-id="33b46-169">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="33b46-170">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="33b46-170">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="33b46-171">Z příkazového řádku spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="33b46-171">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="33b46-172">Předchozí příkazy:</span><span class="sxs-lookup"><span data-stu-id="33b46-172">The preceding commands:</span></span>

* <span data-ttu-id="33b46-173">Vytvoří `RazorUIClassLib` knihovny tříd Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="33b46-173">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="33b46-174">Vytvoří stránku _Message Razor a přidává ji k RCL.</span><span class="sxs-lookup"><span data-stu-id="33b46-174">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="33b46-175">`-np` Parametr vytvoří stránku bez `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="33b46-175">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="33b46-176">Vytvoří [viewstart](xref:mvc/views/layout#running-code-before-each-view) souboru a přidá ho RCL.</span><span class="sxs-lookup"><span data-stu-id="33b46-176">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="33b46-177">Soubor viewstart je potřeba použít rozložení stránky Razor projektu (která je přidána v další části).</span><span class="sxs-lookup"><span data-stu-id="33b46-177">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="33b46-178">Aktualizace stránky Razor:</span><span class="sxs-lookup"><span data-stu-id="33b46-178">Update the Razor Pages:</span></span>

* <span data-ttu-id="33b46-179">Nahraďte kód v *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="33b46-179">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="33b46-180">Nahraďte kód v *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="33b46-180">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="33b46-181">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` je potřeba použít částečné zobrazení (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="33b46-181">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="33b46-182">Místo včetně `@addTagHelper` direktivy, můžete přidat *_ViewImports.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="33b46-182">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="33b46-183">Příklad:</span><span class="sxs-lookup"><span data-stu-id="33b46-183">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="33b46-184">Další informace o viewimports najdete v tématu [import sdílených direktivy](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="33b46-184">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="33b46-185">Sestavení knihovny tříd se ověřit, zda že nejsou žádné chyby kompilátoru:</span><span class="sxs-lookup"><span data-stu-id="33b46-185">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="33b46-186">Výstup sestavení obsahuje *RazorUIClassLib.dll* a *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="33b46-186">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="33b46-187">*RazorUIClassLib.Views.dll* obsahuje kompilovaný obsah Razor.</span><span class="sxs-lookup"><span data-stu-id="33b46-187">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="33b46-188">Použití knihovny uživatelského rozhraní Razor z projektu stránky Razor</span><span class="sxs-lookup"><span data-stu-id="33b46-188">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="33b46-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33b46-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="33b46-190">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na **WebApp1** a vyberte **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="33b46-190">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="33b46-191">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na **WebApp1** a vyberte **sestavení závislosti** > **závislosti projektu**.</span><span class="sxs-lookup"><span data-stu-id="33b46-191">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="33b46-192">Zkontrolujte **RazorUIClassLib** jako závislost **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="33b46-192">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="33b46-193">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na **WebApp1** a vyberte **přidat** > **odkaz**.</span><span class="sxs-lookup"><span data-stu-id="33b46-193">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="33b46-194">V **správce odkazů** dialogové okno, zkontrolujte **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="33b46-194">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="33b46-195">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="33b46-195">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="33b46-196">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="33b46-196">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="33b46-197">Vytvoření stránky Razor webové aplikace a řešení obsahující stránky Razor aplikace a knihovny tříd Razor:</span><span class="sxs-lookup"><span data-stu-id="33b46-197">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="33b46-198">Sestavení a spuštění webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="33b46-198">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="33b46-199">Test WebApp1</span><span class="sxs-lookup"><span data-stu-id="33b46-199">Test WebApp1</span></span>

<span data-ttu-id="33b46-200">Ověřte, zda že je používán knihovny tříd Razor uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="33b46-200">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="33b46-201">Přejděte do `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="33b46-201">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="33b46-202">Přepsání, zobrazení, částečná zobrazení a stránky</span><span class="sxs-lookup"><span data-stu-id="33b46-202">Override views, partial views, and pages</span></span>

<span data-ttu-id="33b46-203">Když najde zobrazení, částečná zobrazení nebo stránky Razor ve webové aplikace a knihovny tříd Razor, kód Razor (*.cshtml* souboru) ve webové aplikaci, dostane přednost.</span><span class="sxs-lookup"><span data-stu-id="33b46-203">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="33b46-204">Například přidejte *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* k WebApp1, a Page1 v WebApp1 má přednost Page1in knihovny tříd Razor.</span><span class="sxs-lookup"><span data-stu-id="33b46-204">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="33b46-205">Při stahování ukázka přejmenovat *WebApp1, oblasti nebo MyFeature2* k *WebApp1, oblasti nebo MyFeature* k testování přednost před.</span><span class="sxs-lookup"><span data-stu-id="33b46-205">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="33b46-206">Kopírování *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* částečné zobrazení k *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="33b46-206">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="33b46-207">Aktualizujte kód označující nové umístění.</span><span class="sxs-lookup"><span data-stu-id="33b46-207">Update the markup to indicate the new location.</span></span> <span data-ttu-id="33b46-208">Sestavení a spuštění aplikace k ověření, že verze aplikace s částečným je používán.</span><span class="sxs-lookup"><span data-stu-id="33b46-208">Build and run the app to verify the app's version of the partial is being used.</span></span>
