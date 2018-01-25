---
title: "Visual Studio publikační profily pro nasazení aplikace ASP.NET Core"
author: rick-anderson
description: "Zjistit, jak vytvořit publikační profily pro aplikace ASP.NET Core v sadě Visual Studio."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 1f403447c39db4ebfe3dafda591602f0dc9db8c3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="5d487-103">Visual Studio publikační profily pro nasazení aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5d487-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="5d487-104">Podle [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5d487-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5d487-105">Tento článek se týká použití Visual Studio 2017 k vytvoření publikační profily.</span><span class="sxs-lookup"><span data-stu-id="5d487-105">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="5d487-106">Profily publikování, vytvořené pomocí sady Visual Studio můžete spustit z nástroje MSBuild a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="5d487-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="5d487-107">Článek obsahuje podrobné informace o procesu publikování.</span><span class="sxs-lookup"><span data-stu-id="5d487-107">The article provides details of the publishing process.</span></span> <span data-ttu-id="5d487-108">V tématu [publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny k publikování do Azure.</span><span class="sxs-lookup"><span data-stu-id="5d487-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="5d487-109">Následující *.csproj* soubor byl vytvořen pomocí příkazu `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="5d487-109">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5d487-110">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="5d487-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5d487-111">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="5d487-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="5d487-112">`Sdk` Atribut `<Project>` elementu (v první řádek) výše uvedený kód provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="5d487-112">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="5d487-113">Importuje soubor vlastnosti z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* na začátku.</span><span class="sxs-lookup"><span data-stu-id="5d487-113">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="5d487-114">Importuje soubor cíle z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* na konci.</span><span class="sxs-lookup"><span data-stu-id="5d487-114">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="5d487-115">Výchozím umístěním pro `MSBuildSDKsPath` (s Visual Studio 2017 Enterprise) je *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* složky.</span><span class="sxs-lookup"><span data-stu-id="5d487-115">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="5d487-116">`Microsoft.NET.Sdk.Web`závisí na:</span><span class="sxs-lookup"><span data-stu-id="5d487-116">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="5d487-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="5d487-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="5d487-118">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="5d487-118">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="5d487-119">Což způsobí, že následující vlastnosti a cílů určených k importu:</span><span class="sxs-lookup"><span data-stu-id="5d487-119">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="5d487-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="5d487-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="5d487-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="5d487-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="5d487-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="5d487-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="5d487-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="5d487-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="5d487-124">Publikujte cíle import právo sada cíle založené na metodě publikování použít.</span><span class="sxs-lookup"><span data-stu-id="5d487-124">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="5d487-125">Při načtení projektu nástroje MSBuild nebo Visual Studio jsou prováděny následující akce vysoké úrovně:</span><span class="sxs-lookup"><span data-stu-id="5d487-125">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="5d487-126">Sestavení projektu</span><span class="sxs-lookup"><span data-stu-id="5d487-126">Build project</span></span>
* <span data-ttu-id="5d487-127">Výpočetní soubory k publikování</span><span class="sxs-lookup"><span data-stu-id="5d487-127">Compute files to publish</span></span>
* <span data-ttu-id="5d487-128">Publikovat soubory do cílového umístění</span><span class="sxs-lookup"><span data-stu-id="5d487-128">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="5d487-129">Výpočetní položky projektu</span><span class="sxs-lookup"><span data-stu-id="5d487-129">Compute project items</span></span>

<span data-ttu-id="5d487-130">Při načtení projektu, se vypočítávají položky projektu (soubory).</span><span class="sxs-lookup"><span data-stu-id="5d487-130">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="5d487-131">`item type` Atribut určuje způsob zpracování souboru.</span><span class="sxs-lookup"><span data-stu-id="5d487-131">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="5d487-132">Ve výchozím nastavení *.cs* souborů jsou součástí `Compile` seznamu položek.</span><span class="sxs-lookup"><span data-stu-id="5d487-132">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="5d487-133">Soubory `Compile` kompilované seznamu položek.</span><span class="sxs-lookup"><span data-stu-id="5d487-133">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="5d487-134">`Content` Seznamu položek obsahuje soubory, které jsou publikovány kromě výstupy sestavení.</span><span class="sxs-lookup"><span data-stu-id="5d487-134">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="5d487-135">Ve výchozím nastavení, soubory odpovídající vzoru `wwwroot/**` jsou součástí `Content` položky.</span><span class="sxs-lookup"><span data-stu-id="5d487-135">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="5d487-136">[Wwwroot /\* \* je vzor expanze názvů](https://gruntjs.com/configuring-tasks#globbing-patterns) určující všechny soubory v *wwwroot* složky **a** podsložky.</span><span class="sxs-lookup"><span data-stu-id="5d487-136">[wwwroot/\*\* is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="5d487-137">Pokud chcete explicitně přidat do seznamu publikovat soubor, přidejte přímo v souboru *.csproj* souboru, jak je znázorněno v [soubory včetně](#including-files).</span><span class="sxs-lookup"><span data-stu-id="5d487-137">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="5d487-138">Při výběru **publikovat** tlačítko v sadě Visual Studio nebo při publikování z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="5d487-138">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="5d487-139">Nebude vlastnosti se vypočítávají (soubory, které jsou potřebné k vytvoření).</span><span class="sxs-lookup"><span data-stu-id="5d487-139">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="5d487-140">Visual Studio pouze: obnovení balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="5d487-140">Visual Studio only: NuGet packages are restored.</span></span> <span data-ttu-id="5d487-141">(Obnovit, musí být explicitní uživatelem v rozhraní příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="5d487-141">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="5d487-142">Sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="5d487-142">The project builds.</span></span>
* <span data-ttu-id="5d487-143">Publikování položky se vypočítávají (soubory, které jsou potřebné k publikování).</span><span class="sxs-lookup"><span data-stu-id="5d487-143">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="5d487-144">Po publikování projektu.</span><span class="sxs-lookup"><span data-stu-id="5d487-144">The project is published.</span></span> <span data-ttu-id="5d487-145">(Počítaný soubory se zkopírují do cílového umístění publikovat).</span><span class="sxs-lookup"><span data-stu-id="5d487-145">(The computed files are copied to the publish destination.)</span></span>

<span data-ttu-id="5d487-146">Když projekt ASP.NET Core odkazuje `Microsoft.NET.Sdk.Web` v souboru projektu, *app_offline.htm* soubor je umístěn v kořenovém adresáři webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5d487-146">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="5d487-147">Když se soubor nachází, modul ASP.NET Core řádně ukončí aplikaci a slouží *app_offline.htm* souboru během nasazení.</span><span class="sxs-lookup"><span data-stu-id="5d487-147">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="5d487-148">Další informace najdete v tématu [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="5d487-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="5d487-149">Základní publikování příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5d487-149">Basic command-line publishing</span></span>

<span data-ttu-id="5d487-150">Příkazového řádku publikování nevyžaduje Visual Studio a funguje na všechny platformy .NET Core podporována.</span><span class="sxs-lookup"><span data-stu-id="5d487-150">Command-line publishing works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="5d487-151">V níže, ukázky `dotnet publish` příkaz spuštěn z adresáře projektu (obsahující *.csproj* souboru).</span><span class="sxs-lookup"><span data-stu-id="5d487-151">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="5d487-152">Pokud není ve složce projektu explicitně předat v cestě k souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="5d487-152">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="5d487-153">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5d487-153">For example:</span></span>

```console
dotnet publish c:/webs/web1
```

<span data-ttu-id="5d487-154">Spusťte následující příkazy k vytvoření a publikování webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="5d487-154">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5d487-155">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="5d487-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5d487-156">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="5d487-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="5d487-157">`dotnet publish` Vytváří výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="5d487-157">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="5d487-158">Výchozí nastavení publikování složky je `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="5d487-158">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="5d487-159">Výchozí hodnota pro `$(Configuration)` je ladění.</span><span class="sxs-lookup"><span data-stu-id="5d487-159">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="5d487-160">V ukázce výše `<TargetFramework>` je `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="5d487-160">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="5d487-161">`dotnet publish -h`Zobrazí nápovědu informace pro publikování.</span><span class="sxs-lookup"><span data-stu-id="5d487-161">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="5d487-162">Určuje následující příkaz `Release` sestavení a publikování adresáře:</span><span class="sxs-lookup"><span data-stu-id="5d487-162">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="5d487-163">`dotnet publish` Příkaz volá MSBuild, které vyvolá `Publish` cíl.</span><span class="sxs-lookup"><span data-stu-id="5d487-163">The `dotnet publish` command calls MSBuild which invokes the `Publish` target.</span></span> <span data-ttu-id="5d487-164">Žádné parametry předaný `dotnet publish` jsou předávány MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5d487-164">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="5d487-165">`-c` Parametr se mapuje `Configuration` vlastnosti MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5d487-165">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="5d487-166">`-o` Parametr mapuje `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="5d487-166">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="5d487-167">Vlastnosti nástroje MSBuild lze předat některou z následujících formátů:</span><span class="sxs-lookup"><span data-stu-id="5d487-167">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="5d487-168">Publikuje následující příkaz `Release` sestavení do sdílené síťové složky:</span><span class="sxs-lookup"><span data-stu-id="5d487-168">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="5d487-169">Sdílené síťové složce je zadán s lomítkem (*//r8/*) a funguje na všech platformách .NET Core podporována.</span><span class="sxs-lookup"><span data-stu-id="5d487-169">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="5d487-170">Potvrďte, že není spuštěn publikované aplikace pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="5d487-170">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="5d487-171">Soubory *publikování* složky jsou zamčené, když aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="5d487-171">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="5d487-172">Nasazení nemůže proběhnout, protože uzamčen, že soubory nelze kopírovat.</span><span class="sxs-lookup"><span data-stu-id="5d487-172">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="5d487-173">Publikační profily</span><span class="sxs-lookup"><span data-stu-id="5d487-173">Publish profiles</span></span>

<span data-ttu-id="5d487-174">Tato část používá k vytvoření profilů publikování Visual Studio 2017 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="5d487-174">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="5d487-175">Po vytvoření publikování ze sady Visual Studio nebo pomocí příkazového řádku je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5d487-175">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="5d487-176">Publikování profily může zjednodušit proces publikování.</span><span class="sxs-lookup"><span data-stu-id="5d487-176">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="5d487-177">Několik publikačních profilů může existovat.</span><span class="sxs-lookup"><span data-stu-id="5d487-177">Multiple publish profiles can exist.</span></span> <span data-ttu-id="5d487-178">Pokud chcete vytvořit profil publikování v sadě Visual Studio, klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="5d487-178">To create a publish profile in Visual Studio, right-click on the project in Solution Explorer and select **Publish**.</span></span> <span data-ttu-id="5d487-179">Případně vyberte možnost **publikovat \<název projektu >** nabídce sestavení.</span><span class="sxs-lookup"><span data-stu-id="5d487-179">Alternatively, select **Publish \<project name>** from the build menu.</span></span> <span data-ttu-id="5d487-180">**Publikovat** kapacity stránky aplikace se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="5d487-180">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="5d487-181">Pokud projekt neobsahuje profil publikování, zobrazí se následující stránka:</span><span class="sxs-lookup"><span data-stu-id="5d487-181">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Vybrat kartu publikovat aplikace kapacity stránky zobrazující Azure, služby IIS, FTB, složku s Azure.](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="5d487-184">Při **složky** je vybrána, **publikovat** tlačítko vytvoří složku profil publikování a publikuje.</span><span class="sxs-lookup"><span data-stu-id="5d487-184">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![** Karta publikovat ** aplikace kapacity stránky zobrazující Azure, služby IIS, FTB, složky](visual-studio-publish-profiles/_static/pub1.png)

<span data-ttu-id="5d487-186">Po vytvoření profilu publikování **publikovat** změny a vyberte **vytvořit nový profil** k vytvoření nového profilu.</span><span class="sxs-lookup"><span data-stu-id="5d487-186">Once a publish profile is created, the **Publish** tab changes, and select **Create new profile** to create a new profile.</span></span>

![** Karta publikovat ** aplikace kapacity stránky zobrazující FolderProfile-vytvořili v posledním kroku a vytvořit nový profil odkaz](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="5d487-188">Průvodce Publikovat podporuje následující cíle publikování:</span><span class="sxs-lookup"><span data-stu-id="5d487-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="5d487-189">Microsoft Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5d487-189">Microsoft Azure App Service</span></span>
* <span data-ttu-id="5d487-190">Služba IIS, FTP atd., (pro všechny webové servery)</span><span class="sxs-lookup"><span data-stu-id="5d487-190">IIS, FTP, etc (for any web server)</span></span>
* <span data-ttu-id="5d487-191">Folder</span><span class="sxs-lookup"><span data-stu-id="5d487-191">Folder</span></span>
* <span data-ttu-id="5d487-192">Importujte profil (umožňuje import profilu).</span><span class="sxs-lookup"><span data-stu-id="5d487-192">Import profile (allows profile import).</span></span>
* <span data-ttu-id="5d487-193">Virtuální počítače Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="5d487-193">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="5d487-194">V tématu [jaké možnosti publikování je pro mě nejlepší?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) Další informace.</span><span class="sxs-lookup"><span data-stu-id="5d487-194">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="5d487-195">Při vytváření profilu publikování pomocí sady Visual Studio *vlastnosti/PublishProfiles/\<název publikovat > .pubxml* k vytvoření souboru MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5d487-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="5d487-196">To *.pubxml* soubor je soubor MSBuild a obsahuje konfigurační nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="5d487-196">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="5d487-197">Tento soubor můžete změnit na přizpůsobení sestavení a publikování procesu.</span><span class="sxs-lookup"><span data-stu-id="5d487-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="5d487-198">Proces publikování je pro čtení tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="5d487-198">This file is read by the publishing process.</span></span> <span data-ttu-id="5d487-199">`<LastUsedBuildConfiguration>`je speciální, protože je globální vlastnost a by neměl být ve všech souborů, které je v sestavení naimportována.</span><span class="sxs-lookup"><span data-stu-id="5d487-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="5d487-200">V tématu [MSBuild: jak nastavit vlastnost konfigurace](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="5d487-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="5d487-201">*.Pubxml* souboru by neměla být zaškrtnuta do správy zdrojového kódu, protože závisí na *.uživatel* souboru.</span><span class="sxs-lookup"><span data-stu-id="5d487-201">The *.pubxml* file shouldn't be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="5d487-202">*.Uživatel* soubor by měl zkontrolovat nikdy do správy zdrojového kódu, protože ho mohou obsahovat citlivé údaje a jeho je platná pouze pro jednoho uživatele a počítače.</span><span class="sxs-lookup"><span data-stu-id="5d487-202">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="5d487-203">Se šifrují citlivých informací (jako je publikování heslo) na uživatele nebo počítač úrovně a uložené v *vlastnosti/PublishProfiles/\<název publikovat >. pubxml.user* souboru.</span><span class="sxs-lookup"><span data-stu-id="5d487-203">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="5d487-204">Protože tento soubor může obsahovat citlivé informace, by měl **není** se změnami do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="5d487-204">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="5d487-205">Přehled o tom, jak publikovat webovou aplikaci na ASP.NET Core najdete [hostitele a nasazení](index.md).</span><span class="sxs-lookup"><span data-stu-id="5d487-205">For an overview of how to publish a web app on ASP.NET Core see [Host and deploy](index.md).</span></span> <span data-ttu-id="5d487-206">[Hostování a nasadit](index.md) je opensourcový projekt v https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="5d487-206">[Host and deploy](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="5d487-207">`dotnet publish`můžete použít složku, MSDeploy, a [KUDU](https://github.com/projectkudu/kudu/wiki) publikační profily:</span><span class="sxs-lookup"><span data-stu-id="5d487-207">`dotnet publish` can use folder, MSDeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="5d487-208">Složka (funguje napříč platformami):`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="5d487-208">Folder (works cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="5d487-209">MSDeploy (aktuálně toto funguje pouze v systému windows, protože není MSDeploy napříč platformami):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="5d487-209">MSDeploy (currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="5d487-210">Balíček MSDeploy (aktuálně toto funguje pouze v systému windows, protože není MSDeploy napříč platformami):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="5d487-210">MSDeploy package(currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="5d487-211">V předcházející ukázky **nemáte** předat `deployonbuild` k `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="5d487-211">In the preceeding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="5d487-212">Další informace najdete v tématu [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="5d487-212">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="5d487-213">`dotnet publish`podporuje rozhraní API modulu KUDU pro publikování do služby Azure z libovolné platformě.</span><span class="sxs-lookup"><span data-stu-id="5d487-213">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="5d487-214">Visual Studio publikovat nepodporuje podporu rozhraní API modulu KUDU, ale podporuje websdk pro křížové plat publikování v Azure.</span><span class="sxs-lookup"><span data-stu-id="5d487-214">Visual Studio publish does support the KUDU APIs but it's supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="5d487-215">Přidat profil publikování do *vlastnosti nebo PublishProfiles* složku s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="5d487-215">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="5d487-216">Spuštěním následujícího příkazu Zpráva prolétne si obsah publikovat a publikujete ho v Azure pomocí rozhraní API modulu KUDU:</span><span class="sxs-lookup"><span data-stu-id="5d487-216">Running the following command zips up the publish contents and publish it to Azure using the KUDU APIs:</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="5d487-217">Při použití profil publikování, nastavte následující vlastnosti nástroje MSBuild:</span><span class="sxs-lookup"><span data-stu-id="5d487-217">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="5d487-218">Při publikování k profilu s názvem *FolderProfile*, můžete provést některou z níže uvedených příkazů:</span><span class="sxs-lookup"><span data-stu-id="5d487-218">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="5d487-219">Při vyvolání `dotnet build`, zavolá `msbuild` spustit sestavení a publikování procesu.</span><span class="sxs-lookup"><span data-stu-id="5d487-219">When invoking `dotnet build`, it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="5d487-220">Volání metody `dotnet build` nebo `msbuild` je v podstatě ekvivalentní při předávání ve složce profilu.</span><span class="sxs-lookup"><span data-stu-id="5d487-220">Calling `dotnet build` or `msbuild` is essentially equivalent when passing in a folder profile.</span></span> <span data-ttu-id="5d487-221">Při volání metody MSBuild přímo v systému Windows, je použít rozhraní .NET Framework verze nástroje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5d487-221">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="5d487-222">MSDeploy je aktuálně omezená na počítačích s Windows pro publikování.</span><span class="sxs-lookup"><span data-stu-id="5d487-222">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="5d487-223">Volání metody `dotnet build` na do jiné složky profil vyvolá MSBuild a MSBuild používá MSDeploy v jiné složce profily.</span><span class="sxs-lookup"><span data-stu-id="5d487-223">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="5d487-224">Volání metody `dotnet build` na jiný složky profilu vyvolá MSBuild (pomocí MSDeploy) a má za následek selhání (i když běžících na platformě Windows).</span><span class="sxs-lookup"><span data-stu-id="5d487-224">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="5d487-225">Pokud chcete publikovat pomocí profilu pro jiné složky, přímo voláním funkce MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5d487-225">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="5d487-226">Profil publikování se následující složce byl vytvořen pomocí sady Visual Studio a publikuje do sdílené síťové složky:</span><span class="sxs-lookup"><span data-stu-id="5d487-226">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="5d487-227">Poznámka: `<LastUsedBuildConfiguration>` je nastaven na `Release`.</span><span class="sxs-lookup"><span data-stu-id="5d487-227">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="5d487-228">Při publikování ze sady Visual Studio `<LastUsedBuildConfiguration>` je nastavena hodnota vlastnosti konfigurace, pomocí hodnoty, když se spustí proces publikování.</span><span class="sxs-lookup"><span data-stu-id="5d487-228">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="5d487-229">`<LastUsedBuildConfiguration>` Vlastnosti konfigurace je speciální a by nemělo být přepsáno v importovaného souboru MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5d487-229">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="5d487-230">Tato vlastnost může být přepsána z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5d487-230">This property can be overridden from the command line.</span></span> <span data-ttu-id="5d487-231">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5d487-231">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="5d487-232">Pomocí nástroje MSBuild:</span><span class="sxs-lookup"><span data-stu-id="5d487-232">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="5d487-233">Publikovat do koncového bodu MSDeploy z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5d487-233">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="5d487-234">Jak už jsme zmínili, publikování můžete udělat pomocí `dotnet publish` nebo `msbuild` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5d487-234">As previously mentioned, publishing can be accomplished using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="5d487-235">`dotnet publish`běží v kontextu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d487-235">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="5d487-236">`msbuild` Příkaz vyžaduje rozhraní .NET framework a proto je omezený na prostředí Windows.</span><span class="sxs-lookup"><span data-stu-id="5d487-236">The `msbuild` command requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="5d487-237">Nejjednodušší způsob, jak publikovat s MSDeploy je nejdřív vytvořte profil publikování ve Visual Studio 2017 a použít profil z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5d487-237">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="5d487-238">V následující ukázce je vytvoření webové aplikace ASP.NET Core (pomocí `dotnet new mvc`), a přidají se profilem publikování Azure pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5d487-238">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="5d487-239">Spustit `msbuild` z **příkazový řádek vývojáře pro VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="5d487-239">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="5d487-240">Příkazový řádek vývojáře má správný *msbuild.exe* v cestě sadou některé nástroje MSBuild proměnné.</span><span class="sxs-lookup"><span data-stu-id="5d487-240">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="5d487-241">MSBuild používá následující syntaxe:</span><span class="sxs-lookup"><span data-stu-id="5d487-241">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="5d487-242">Získat `Password` z  *\<publikovat name >. PublishSettings* souboru.</span><span class="sxs-lookup"><span data-stu-id="5d487-242">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="5d487-243">Stažení *. PublishSettings* soubor z buď:</span><span class="sxs-lookup"><span data-stu-id="5d487-243">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="5d487-244">Průzkumník řešení: Klikněte pravým tlačítkem na webovou aplikaci a vyberte **stažení profilu publikování**.</span><span class="sxs-lookup"><span data-stu-id="5d487-244">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="5d487-245">Azure Management Portal: Vyberte **profilu publikování Get** v okně webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5d487-245">The Azure Management Portal: Select **Get publish profile** from the Web App blade.</span></span>

<span data-ttu-id="5d487-246">`Username`můžete najít v profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="5d487-246">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="5d487-247">Profil publikování se následující ukázka používá "Web11112 – nasazení webu":</span><span class="sxs-lookup"><span data-stu-id="5d487-247">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="5d487-248">Vyloučení souborů</span><span class="sxs-lookup"><span data-stu-id="5d487-248">Excluding files</span></span>

<span data-ttu-id="5d487-249">Při publikování webové aplikace ASP.NET Core, sestavení artefaktů a obsah *wwwroot* složky jsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="5d487-249">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="5d487-250">`msbuild`podporuje [expanze názvů vzory](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="5d487-250">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="5d487-251">Například následující `<Content>` element značek vyloučí všechny text (*.txt*) souborů z *wwwroot a obsah* složce a jejích podsložkách.</span><span class="sxs-lookup"><span data-stu-id="5d487-251">For example, the following `<Content>` element markup excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="5d487-252">Výše uvedený kód lze přidat do profilu publikování nebo *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="5d487-252">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="5d487-253">Když je přidán do *.csproj* , pravidlo se přidá soubor všechny profily publikování v projektu.</span><span class="sxs-lookup"><span data-stu-id="5d487-253">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="5d487-254">Následující `<MsDeploySkipRules>` element značek vyloučeny všechny soubory z *wwwroot a obsah* složky:</span><span class="sxs-lookup"><span data-stu-id="5d487-254">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="5d487-255">`<MsDeploySkipRules>`neodstraní *přeskočit* cíle z webu nasazení.</span><span class="sxs-lookup"><span data-stu-id="5d487-255">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="5d487-256">`<Content>`cílové soubory a složky, se odstraní z webu nasazení.</span><span class="sxs-lookup"><span data-stu-id="5d487-256">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="5d487-257">Předpokládejme například, že nasazené webové aplikace měla následující soubory:</span><span class="sxs-lookup"><span data-stu-id="5d487-257">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="5d487-258">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5d487-258">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="5d487-259">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5d487-259">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="5d487-260">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5d487-260">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="5d487-261">Pokud následující `<MsDeploySkipRules>` těchto souborů nemohli odstranit na webu nasazení, přidání značka.</span><span class="sxs-lookup"><span data-stu-id="5d487-261">If the following `<MsDeploySkipRules>` markup is added, those files wouldn't be deleted on the deployment site.</span></span>

``` xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="5d487-262">`<MsDeploySkipRules>` Brání značek výše uvedeném *přeskočen* soubory nebudou depoyed ale neodstraní těchto souborů se poté, co máte nasazené.</span><span class="sxs-lookup"><span data-stu-id="5d487-262">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed but won't delete those files once they're deployed.</span></span>

<span data-ttu-id="5d487-263">Následující `<Content>` značek odstraní cílové soubory v lokalitě nasazení:</span><span class="sxs-lookup"><span data-stu-id="5d487-263">The following `<Content>` markup deletes the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="5d487-264">Pomocí příkazového řádku nasazení s `<Content>` značek výše má za následek výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="5d487-264">Using command-line deployment with the `<Content>` markup above results in output similar to the following:</span></span>

``` console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="including-files"></a><span data-ttu-id="5d487-265">Zahrnutí souborů</span><span class="sxs-lookup"><span data-stu-id="5d487-265">Including files</span></span>

<span data-ttu-id="5d487-266">Zahrnuje následující kód *bitové kopie* složky mimo adresář projektu, který má *wwwroot nebo obrázky* složku publikování webu:</span><span class="sxs-lookup"><span data-stu-id="5d487-266">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="5d487-267">Značky lze přidat do *.csproj* soubor nebo profil publikování.</span><span class="sxs-lookup"><span data-stu-id="5d487-267">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="5d487-268">Pokud je přidán do *.csproj* souboru, má součástí každý profil publikování v projektu.</span><span class="sxs-lookup"><span data-stu-id="5d487-268">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="5d487-269">Následující zvýrazněnou kód ukazuje, jak na:</span><span class="sxs-lookup"><span data-stu-id="5d487-269">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="5d487-270">Kopírování souboru z mimo projekt do *wwwroot* složky.</span><span class="sxs-lookup"><span data-stu-id="5d487-270">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="5d487-271">Vyloučit *wwwroot\Content* složky.</span><span class="sxs-lookup"><span data-stu-id="5d487-271">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="5d487-272">Exclude *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5d487-272">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="5d487-273">Najdete v článku [WebSDK Readme](https://github.com/aspnet/websdk) pro další ukázky nasazení.</span><span class="sxs-lookup"><span data-stu-id="5d487-273">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="5d487-274">Spustit cíl před nebo po publikování</span><span class="sxs-lookup"><span data-stu-id="5d487-274">Run a target before or after publishing</span></span>

<span data-ttu-id="5d487-275">Integrované `BeforePublish` a `AfterPublish` cíle můžete použít ke spuštění cíl před nebo po cíl publikování.</span><span class="sxs-lookup"><span data-stu-id="5d487-275">The built-in `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="5d487-276">Následující kód můžete přidat do profilu publikování k protokolování zpráv bude výstup konzoly, před a po publikování:</span><span class="sxs-lookup"><span data-stu-id="5d487-276">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="5d487-277">Služby modulu Kudu</span><span class="sxs-lookup"><span data-stu-id="5d487-277">The Kudu service</span></span>

<span data-ttu-id="5d487-278">Chcete-li zobrazit soubory v Azure aplikace Service nasazení aplikace web, použijte [Kudu služby](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="5d487-278">To view the files in the an Azure Apps Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="5d487-279">Připojení `scm` tokenu k názvu webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5d487-279">Append the `scm` token to the name of the web app.</span></span> <span data-ttu-id="5d487-280">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5d487-280">For example:</span></span>

| <span data-ttu-id="5d487-281">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="5d487-281">URL</span></span>                                    | <span data-ttu-id="5d487-282">Výsledek</span><span class="sxs-lookup"><span data-stu-id="5d487-282">Result</span></span>      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="5d487-283">Web App</span><span class="sxs-lookup"><span data-stu-id="5d487-283">Web App</span></span>     |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="5d487-284">Adresářové kudu</span><span class="sxs-lookup"><span data-stu-id="5d487-284">Kudu sevice</span></span> |

<span data-ttu-id="5d487-285">Vyberte [ladění konzoly](https://github.com/projectkudu/kudu/wiki/Kudu-console) položku nabídky Zobrazit nebo upravit nebo odstranit nebo přidat soubory.</span><span class="sxs-lookup"><span data-stu-id="5d487-285">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5d487-286">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5d487-286">Additional resources</span></span>

* <span data-ttu-id="5d487-287">[Nasazení webové](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) zjednodušuje nasazení webové aplikace a weby pro servery služby IIS.</span><span class="sxs-lookup"><span data-stu-id="5d487-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="5d487-288">[https://github.com/ASPNET/websdk](https://github.com/aspnet/websdk/issues): soubor problémy a žádosti o funkce pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="5d487-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
