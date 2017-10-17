---
title: "Vytvoření profilů publikování pro Visual Studio a nástroje MSBuild"
author: rick-anderson
description: "Popisuje publikování na webu v sadě Visual Studio."
keywords: "Web ASP.NET Core, publikování, publikování, msbuild, nasazení webu, publikovat dotnet, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: f33fb9d648a611bb7b2b96291dd2176b230a9a43
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/29/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a><span data-ttu-id="86d13-104">Vytvoření publikační profily pro Visual Studio a nástroje MSBuild, jak nasadit aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86d13-104">Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps</span></span>

<span data-ttu-id="86d13-105">Podle [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="86d13-105">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="86d13-106">Tento článek se týká použití Visual Studio 2017 k vytvoření publikační profily.</span><span class="sxs-lookup"><span data-stu-id="86d13-106">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="86d13-107">Profily publikování, vytvořené pomocí sady Visual Studio můžete spustit z nástroje MSBuild a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="86d13-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="86d13-108">Článek obsahuje podrobné informace o procesu publikování.</span><span class="sxs-lookup"><span data-stu-id="86d13-108">The article provides details of the publishing process.</span></span> <span data-ttu-id="86d13-109">V tématu [publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny k publikování do Azure.</span><span class="sxs-lookup"><span data-stu-id="86d13-109">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="86d13-110">Následující *.csproj* soubor byl vytvořen pomocí příkazu `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="86d13-110">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="86d13-111">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="86d13-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="86d13-112">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="86d13-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="86d13-113">`Sdk` Atribut `<Project>` elementu (v první řádek) výše uvedený kód provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="86d13-113">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="86d13-114">Importy `props` souboru z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* na začátku.</span><span class="sxs-lookup"><span data-stu-id="86d13-114">Imports the `props` file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="86d13-115">Importuje soubor cíle z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* na konci.</span><span class="sxs-lookup"><span data-stu-id="86d13-115">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="86d13-116">Výchozím umístěním pro `MSBuildSDKsPath` (s Visual Studio 2017 Enterprise) je *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* složky.</span><span class="sxs-lookup"><span data-stu-id="86d13-116">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="86d13-117">`Microsoft.NET.Sdk.Web`závisí na:</span><span class="sxs-lookup"><span data-stu-id="86d13-117">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="86d13-118">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="86d13-118">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="86d13-119">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="86d13-119">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="86d13-120">Což způsobí, že následující `props` a `targets` určených k importu:</span><span class="sxs-lookup"><span data-stu-id="86d13-120">Which causes the following `props` and `targets` to be imported:</span></span>

* <span data-ttu-id="86d13-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="86d13-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="86d13-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="86d13-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="86d13-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="86d13-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="86d13-124">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="86d13-124">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="86d13-125">Publikujte cíle bude znovu importovat správnou sadu cílů založené na metodě publikování použít.</span><span class="sxs-lookup"><span data-stu-id="86d13-125">Publish targets will again import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="86d13-126">Při načtení projektu nástroje MSBuild nebo Visual Studio jsou prováděny následující akce vysoké úrovně:</span><span class="sxs-lookup"><span data-stu-id="86d13-126">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="86d13-127">Sestavení projektu</span><span class="sxs-lookup"><span data-stu-id="86d13-127">Build project</span></span>
* <span data-ttu-id="86d13-128">Výpočetní soubory k publikování</span><span class="sxs-lookup"><span data-stu-id="86d13-128">Compute files to publish</span></span>
* <span data-ttu-id="86d13-129">Publikovat soubory do cílového umístění</span><span class="sxs-lookup"><span data-stu-id="86d13-129">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="86d13-130">Výpočetní položky projektu</span><span class="sxs-lookup"><span data-stu-id="86d13-130">Compute project items</span></span>

<span data-ttu-id="86d13-131">Při načtení projektu, se vypočítávají položky projektu (soubory).</span><span class="sxs-lookup"><span data-stu-id="86d13-131">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="86d13-132">`item type` Atribut určuje způsob zpracování souboru.</span><span class="sxs-lookup"><span data-stu-id="86d13-132">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="86d13-133">Ve výchozím nastavení *.cs* souborů jsou součástí `Compile` seznamu položek.</span><span class="sxs-lookup"><span data-stu-id="86d13-133">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="86d13-134">Soubory `Compile` kompilované seznamu položek.</span><span class="sxs-lookup"><span data-stu-id="86d13-134">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="86d13-135">`Content` Seznamu položek obsahuje soubory, které budou publikovány kromě výstupy sestavení.</span><span class="sxs-lookup"><span data-stu-id="86d13-135">The `Content` item list contains files that will be published in addition to the build outputs.</span></span> <span data-ttu-id="86d13-136">Ve výchozím nastavení, soubory odpovídající vzor wwwroot / ** budou zahrnuty do `Content` položky.</span><span class="sxs-lookup"><span data-stu-id="86d13-136">By default, files matching the pattern wwwroot/** will be included in the `Content` item.</span></span> <span data-ttu-id="86d13-137">[Wwwroot / ** je vzor expanze názvů](https://gruntjs.com/configuring-tasks#globbing-patterns) určující všechny soubory v *wwwroot* složky **a** podsložky.</span><span class="sxs-lookup"><span data-stu-id="86d13-137">[wwwroot/** is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="86d13-138">Pokud chcete explicitně přidat do seznamu publikovat soubor, přidejte přímo v souboru *.csproj* souboru, jak je znázorněno v [soubory včetně](#including-files).</span><span class="sxs-lookup"><span data-stu-id="86d13-138">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="86d13-139">Když vyberete **publikovat** tlačítko v sadě Visual Studio nebo při publikování z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="86d13-139">When you select the **Publish** button in Visual Studio or when you publish from command line:</span></span>

- <span data-ttu-id="86d13-140">Nebude vlastnosti se vypočítávají (soubory, které jsou potřebné k vytvoření).</span><span class="sxs-lookup"><span data-stu-id="86d13-140">The properties/items are computed (the files that are needed to build).</span></span>
- <span data-ttu-id="86d13-141">Visual Studio pouze: obnovení balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="86d13-141">Visual Studio only: NuGet packages are restored.</span></span>  <span data-ttu-id="86d13-142">(Obnovit, musí být explicitní uživatelem v rozhraní příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="86d13-142">(Restore needs to be explicit by the user on the CLI.)</span></span>
- <span data-ttu-id="86d13-143">Sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="86d13-143">The project builds.</span></span>
- <span data-ttu-id="86d13-144">Publikování položky se vypočítávají (soubory, které jsou potřebné k publikování).</span><span class="sxs-lookup"><span data-stu-id="86d13-144">The publish items are computed (the files that are needed to publish).</span></span>
- <span data-ttu-id="86d13-145">Po publikování projektu.</span><span class="sxs-lookup"><span data-stu-id="86d13-145">The project is published.</span></span> <span data-ttu-id="86d13-146">(Počítaný soubory se zkopírují do cílového umístění publikovat).</span><span class="sxs-lookup"><span data-stu-id="86d13-146">(The computed files are copied to the publish destination.)</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="86d13-147">Publikování základní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="86d13-147">Basic command line publishing</span></span>

<span data-ttu-id="86d13-148">Tato část funguje na všech platformách podporované .NET Core a nevyžaduje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86d13-148">This section works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="86d13-149">V níže, ukázky `dotnet publish` příkaz spuštěn z adresáře projektu (obsahující *.csproj* souboru).</span><span class="sxs-lookup"><span data-stu-id="86d13-149">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="86d13-150">Pokud si nejste ve složce projektu, které lze předat explicitně v cestě k souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="86d13-150">If you're not in the project folder, you can explicitly pass in the project file path.</span></span> <span data-ttu-id="86d13-151">Příklad:</span><span class="sxs-lookup"><span data-stu-id="86d13-151">For example:</span></span>

```console
dotnet publish  c:/webs/web1
```

<span data-ttu-id="86d13-152">Spusťte následující příkazy k vytvoření a publikování webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="86d13-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="86d13-153">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="86d13-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="86d13-154">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="86d13-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

--------------

<span data-ttu-id="86d13-155">`dotnet publish` Vytváří výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="86d13-155">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="86d13-156">Výchozí nastavení publikování složky je `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="86d13-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="86d13-157">Výchozí hodnota pro `$(Configuration)` je ladění.</span><span class="sxs-lookup"><span data-stu-id="86d13-157">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="86d13-158">V ukázce výše `<TargetFramework>` je `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="86d13-158">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="86d13-159">`dotnet publish -h`Zobrazí nápovědu informace pro publikování.</span><span class="sxs-lookup"><span data-stu-id="86d13-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="86d13-160">Určuje následující příkaz `Release` sestavení a publikování adresáře:</span><span class="sxs-lookup"><span data-stu-id="86d13-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="86d13-161">`dotnet publish` Příkaz volání `MSBuild` který vyvolá `Publish` cíl.</span><span class="sxs-lookup"><span data-stu-id="86d13-161">The `dotnet publish` command calls `MSBuild` which invokes the `Publish` target.</span></span> <span data-ttu-id="86d13-162">Žádné parametry předaný `dotnet publish` jsou předávány `MSBuild`.</span><span class="sxs-lookup"><span data-stu-id="86d13-162">Any parameters passed to `dotnet publish` are passed to `MSBuild`.</span></span> <span data-ttu-id="86d13-163">`-c` Parametr se mapuje `Configuration` vlastnosti MSBuild.</span><span class="sxs-lookup"><span data-stu-id="86d13-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="86d13-164">`-o` Parametr mapuje `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="86d13-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="86d13-165">Můžete předat vlastnosti nástroje MSBuild některou z následujících formátů:</span><span class="sxs-lookup"><span data-stu-id="86d13-165">You can pass MSBuild properties using either of the following formats:</span></span>

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

<span data-ttu-id="86d13-166">Publikuje následující příkaz `Release` sestavení do sdílené síťové složky:</span><span class="sxs-lookup"><span data-stu-id="86d13-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="86d13-167">Sdílené síťové složce je zadán s lomítkem (*//r8/*) a funguje na všech platformách .NET Core podporována.</span><span class="sxs-lookup"><span data-stu-id="86d13-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="86d13-168">Potvrďte, že není spuštěn publikované aplikace pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="86d13-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="86d13-169">Soubory *publikování* složky jsou zamčené, když aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="86d13-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="86d13-170">Nasazení nemůže proběhnout, protože uzamčen, že soubory nelze kopírovat.</span><span class="sxs-lookup"><span data-stu-id="86d13-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="86d13-171">Publikační profily</span><span class="sxs-lookup"><span data-stu-id="86d13-171">Publish profiles</span></span>

<span data-ttu-id="86d13-172">Tato část používá k vytvoření profilů publikování Visual Studio 2017 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="86d13-172">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="86d13-173">Po vytvoření můžete publikovat ze sady Visual Studio nebo pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="86d13-173">Once created, you can publish from Visual Studio or the command line.</span></span>

<span data-ttu-id="86d13-174">Publikování profily může zjednodušit proces publikování.</span><span class="sxs-lookup"><span data-stu-id="86d13-174">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="86d13-175">Můžete mít více publikační profily.</span><span class="sxs-lookup"><span data-stu-id="86d13-175">You can have multiple publish profiles.</span></span> <span data-ttu-id="86d13-176">Pokud chcete vytvořit profil publikování v sadě Visual Studio, klikněte pravým tlačítkem na projekt v prozkoumat řešení a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="86d13-176">To create a publish profile in Visual Studio, right click on the project in Solution Explore and select **Publish**.</span></span> <span data-ttu-id="86d13-177">Alternativně můžete vybrat **publikovat \<název projektu >** nabídce sestavení.</span><span class="sxs-lookup"><span data-stu-id="86d13-177">Alternatively, you can select **Publish     \<project name>** from the build menu.</span></span> <span data-ttu-id="86d13-178">**Publikovat** kapacity stránky aplikace se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="86d13-178">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="86d13-179">Pokud projekt neobsahuje profil publikování, zobrazí se následující stránka:</span><span class="sxs-lookup"><span data-stu-id="86d13-179">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![** Publikovat ** kartu aplikace kapacity stránky zobrazující Azure, služby IIS, FTB, vybrána složka s Azure.](web-publishing-vs/_static/az.png)

<span data-ttu-id="86d13-182">Při **složky** je vybrána, **publikovat** tlačítko vytvoří složku profil publikování a publikuje.</span><span class="sxs-lookup"><span data-stu-id="86d13-182">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![** Karta publikovat ** aplikace kapacity stránky zobrazující Azure, služby IIS, FTB, složky](web-publishing-vs/_static/pub1.png)

<span data-ttu-id="86d13-184">Po vytvoření profilu publikování **publikovat** kartě změny a vyberte **vytvořit nový profil** k vytvoření nového profilu.</span><span class="sxs-lookup"><span data-stu-id="86d13-184">Once a publish profile is created, the **Publish** tab changes, and you select **Create new profile** to create a new profile.</span></span>

![** Karta publikovat ** aplikace kapacity stránky zobrazující FolderProfile-vytvořili v posledním kroku a vytvořit nový profil odkaz](web-publishing-vs/_static/create_new.png)

<span data-ttu-id="86d13-186">Průvodce Publikovat podporuje následující cíle publikování:</span><span class="sxs-lookup"><span data-stu-id="86d13-186">The Publish wizard supports the following publish targets:</span></span>

- <span data-ttu-id="86d13-187">Microsoft Azure App Service</span><span class="sxs-lookup"><span data-stu-id="86d13-187">Microsoft Azure App Service</span></span>
- <span data-ttu-id="86d13-188">Služba IIS, FTP atd., (pro všechny webové servery)</span><span class="sxs-lookup"><span data-stu-id="86d13-188">IIS, FTP, etc (for any web server)</span></span>
- <span data-ttu-id="86d13-189">Folder</span><span class="sxs-lookup"><span data-stu-id="86d13-189">Folder</span></span>
- <span data-ttu-id="86d13-190">Import profilu (umožňuje importovat profil).</span><span class="sxs-lookup"><span data-stu-id="86d13-190">Import profile (allows you to import a profile).</span></span>
- <span data-ttu-id="86d13-191">Virtuální počítače Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="86d13-191">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="86d13-192">V tématu [jaké možnosti publikování je pro mě nejlepší?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) Další informace.</span><span class="sxs-lookup"><span data-stu-id="86d13-192">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="86d13-193">Když vytvoříte profil publikování pomocí sady Visual Studio *vlastnosti/PublishProfiles/\<název publikovat > .pubxml* k vytvoření souboru MSBuild.</span><span class="sxs-lookup"><span data-stu-id="86d13-193">When you create a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="86d13-194">To *.pubxml* soubor je soubor MSBuild a obsahuje konfigurační nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="86d13-194">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="86d13-195">Tento soubor pro přizpůsobení sestavení a publikování procesu, můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="86d13-195">You can change this file to customize the build and publish process.</span></span> <span data-ttu-id="86d13-196">Proces publikování je pro čtení tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="86d13-196">This file is read by the publishing process.</span></span> <span data-ttu-id="86d13-197">`<LastUsedBuildConfiguration>`je speciální, protože je globální vlastnost a by neměl být ve všech souborů, které je v sestavení naimportována.</span><span class="sxs-lookup"><span data-stu-id="86d13-197">`<LastUsedBuildConfiguration>` is special because it’s a global property and shouldn’t be in any file that’s imported in the build.</span></span> <span data-ttu-id="86d13-198">V tématu [MSBuild: jak nastavit vlastnost konfigurace](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="86d13-198">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="86d13-199">*.Pubxml* souboru by neměla být zaškrtnuta do správy zdrojového kódu, protože závisí na *.uživatel* souboru.</span><span class="sxs-lookup"><span data-stu-id="86d13-199">The *.pubxml* file should not be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="86d13-200">*.Uživatel* soubor by měl zkontrolovat nikdy do správy zdrojového kódu, protože ho mohou obsahovat citlivé údaje a jeho je platná pouze pro jednoho uživatele a počítače.</span><span class="sxs-lookup"><span data-stu-id="86d13-200">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="86d13-201">Se šifrují citlivých informací (jako je publikování heslo) na uživatele nebo počítač úrovně a uložené v *vlastnosti/PublishProfiles/\<název publikovat >. pubxml.user* souboru.</span><span class="sxs-lookup"><span data-stu-id="86d13-201">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="86d13-202">Protože tento soubor může obsahovat citlivé informace, by měl **není** se změnami do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="86d13-202">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="86d13-203">Přehled o tom, jak publikovat webovou aplikaci na ASP.NET Core najdete [publikování a nasazení](index.md).</span><span class="sxs-lookup"><span data-stu-id="86d13-203">For an overview of how to publish a web app on ASP.NET Core see [Publishing and Deployment](index.md).</span></span> <span data-ttu-id="86d13-204">[Publikování a nasazení](index.md) je opensourcový projekt v https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="86d13-204">[Publishing and Deployment](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

 <span data-ttu-id="86d13-205">`dotnet publish`můžete použít složku, Msdeploy, a [KUDU](https://github.com/projectkudu/kudu/wiki) publikační profily:</span><span class="sxs-lookup"><span data-stu-id="86d13-205">`dotnet publish` can use folder, Msdeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="86d13-206">Složku (funguje napříč platformami)`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="86d13-206">Folder (works cross-platform) `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="86d13-207">MSDeploy (aktuálně toto funguje pouze v systému windows, protože msdeploy není napříč platformami):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="86d13-207">Msdeploy (currently this only works in windows since msdeploy is not cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="86d13-208">Balíček MSDeploy (aktuálně toto funguje pouze v systému windows, protože msdeploy není napříč platformami):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="86d13-208">Msdeploy package(currently this only works in windows since msdeploy is not cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="86d13-209">V předcházející ukázky **nemáte** předat `deployonbuild` k `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="86d13-209">In the preceeding samples, **don’t** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="86d13-210">Další informace najdete v tématu [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)</span><span class="sxs-lookup"><span data-stu-id="86d13-210">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)</span></span>

<span data-ttu-id="86d13-211">`dotnet publish`podporuje rozhraní API modulu KUDU pro publikování do služby Azure z libovolné platformě.</span><span class="sxs-lookup"><span data-stu-id="86d13-211">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="86d13-212">Visual Studio publikovat nepodporuje podporu rozhraní API modulu KUDU, ale podporuje websdk pro křížové plat publikování v Azure.</span><span class="sxs-lookup"><span data-stu-id="86d13-212">Visual Studio publish does support the KUDU APIs but it is supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="86d13-213">Přidat profil publikování do *vlastnosti nebo PublishProfiles* složku s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="86d13-213">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="86d13-214">Spuštěním následujícího příkazu zip si obsah publikovat a publikujete ho v Azure pomocí rozhraní API modulu KUDU.</span><span class="sxs-lookup"><span data-stu-id="86d13-214">Running the following command will zip up the publish contents and publish it to Azure using the KUDU APIs.</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="86d13-215">Při použití profil publikování, nastavte následující vlastnosti nástroje MSBuild:</span><span class="sxs-lookup"><span data-stu-id="86d13-215">Set the following MSBuild properties when using a publish profile:</span></span>

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

<span data-ttu-id="86d13-216">Například při publikování k profilu s názvem *FolderProfile* můžete provést některou z níže uvedených příkazů.</span><span class="sxs-lookup"><span data-stu-id="86d13-216">For example, when publishing with a profile named *FolderProfile* you can execute either of the commands below.</span></span>

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="86d13-217">Při vyvolání `dotnet build` bude volat `msbuild` spustit sestavení a publikování procesu.</span><span class="sxs-lookup"><span data-stu-id="86d13-217">When you invoke `dotnet build` it will call `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="86d13-218">Volání metody `dotnet build` nebo `msbuild` je v podstatě ekvivalentní při předání ve složce profilu.</span><span class="sxs-lookup"><span data-stu-id="86d13-218">Calling `dotnet build` or `msbuild` is essentially equivalent when you pass in a folder profile.</span></span> <span data-ttu-id="86d13-219">Při volání metody MSBuild přímo v systému Windows můžete získat verzi rozhraní .NET Framework nástroje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="86d13-219">When calling MSBuild directly on Windows you get the .NET Framework version of MSBuild.</span></span>  <span data-ttu-id="86d13-220">MSDeploy je aktuálně omezená na počítačích s Windows pro publikování.</span><span class="sxs-lookup"><span data-stu-id="86d13-220">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="86d13-221">Volání metody `dotnet build` na do jiné složky profil vyvolá MSBuild a MSBuild používá MSDeploy v jiné složce profily.</span><span class="sxs-lookup"><span data-stu-id="86d13-221">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="86d13-222">Volání metody `dotnet build` na jiný složky profilu vyvolá MSBuild (pomocí MSDeploy) a má za následek selhání (i když běžících na platformě Windows).</span><span class="sxs-lookup"><span data-stu-id="86d13-222">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="86d13-223">Pokud chcete publikovat pomocí profilu pro jiné složky, přímo voláním funkce MSBuild.</span><span class="sxs-lookup"><span data-stu-id="86d13-223">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="86d13-224">Profil publikování se následující složce byl vytvořen pomocí sady Visual Studio a publikuje do sdílené síťové složky:</span><span class="sxs-lookup"><span data-stu-id="86d13-224">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

<span data-ttu-id="86d13-225">Poznámka: `<LastUsedBuildConfiguration>` je nastaven na `Release`.</span><span class="sxs-lookup"><span data-stu-id="86d13-225">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span>  <span data-ttu-id="86d13-226">Při publikování ze sady Visual Studio `<LastUsedBuildConfiguration>` je nastavena hodnota vlastnosti konfigurace, pomocí hodnoty, když se spustí proces publikování.</span><span class="sxs-lookup"><span data-stu-id="86d13-226">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="86d13-227">`<LastUsedBuildConfiguration>` Vlastnosti konfigurace je speciální a by nemělo být přepsáno v importovaného souboru MSBuild.</span><span class="sxs-lookup"><span data-stu-id="86d13-227">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn’t be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="86d13-228">Tato vlastnost z příkazového řádku můžete přepsat.</span><span class="sxs-lookup"><span data-stu-id="86d13-228">You can override this property from the command line.</span></span> <span data-ttu-id="86d13-229">Příklad:</span><span class="sxs-lookup"><span data-stu-id="86d13-229">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="86d13-230">Pomocí nástroje MSBuild:</span><span class="sxs-lookup"><span data-stu-id="86d13-230">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="86d13-231">Publikovat do koncového bodu MSDeploy z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="86d13-231">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="86d13-232">Jak už jsme zmínili, které můžete publikovat pomocí `dotnet publish` nebo `msbuild` příkaz.</span><span class="sxs-lookup"><span data-stu-id="86d13-232">As previously mentioned, you can publish using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="86d13-233">`dotnet publish`běží v kontextu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="86d13-233">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="86d13-234">`msbuild`vyžaduje rozhraní .NET framework a proto je omezený na prostředí Windows.</span><span class="sxs-lookup"><span data-stu-id="86d13-234">`msbuild` requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="86d13-235">Nejjednodušší způsob, jak publikovat s MSDeploy je nejdřív vytvořte profil publikování ve Visual Studio 2017 a použít profil z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="86d13-235">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="86d13-236">V následující ukázce je vytvoření webové aplikace ASP.NET Core (pomocí `dotnet new mvc`) a přidají se profilem publikování Azure pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86d13-236">In the following sample, an ASP.NET Core web app is created ( using `dotnet new mvc`) and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="86d13-237">Při spuštění `msbuild` z **příkazový řádek vývojáře pro VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="86d13-237">You run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="86d13-238">Příkazový řádek vývojáře budou mít správný *msbuild.exe* v jeho cestu a nastavte některé nástroje MSBuild proměnné.</span><span class="sxs-lookup"><span data-stu-id="86d13-238">The Developer Command Prompt will have the correct *msbuild.exe* in its path and set some MSBuild variables.</span></span>

<span data-ttu-id="86d13-239">MSBuild používá následující syntaxe:</span><span class="sxs-lookup"><span data-stu-id="86d13-239">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="86d13-240">Můžete získat `Password` z  *\<publikovat name >. PublishSettings* souboru.</span><span class="sxs-lookup"><span data-stu-id="86d13-240">You can get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="86d13-241">Si můžete stáhnout *. PublishSettings* ze souboru:</span><span class="sxs-lookup"><span data-stu-id="86d13-241">You can download the *.PublishSettings* file from:</span></span>

- <span data-ttu-id="86d13-242">Průzkumník řešení: klikněte pravým tlačítkem na webovou aplikaci a vyberte **stažení profilu publikování**.</span><span class="sxs-lookup"><span data-stu-id="86d13-242">Solution Explorer: Right click on the Web App and select **Download Publish Profile**.</span></span>
- <span data-ttu-id="86d13-243">Azure Management Portal: Vyberte **profilu publikování Get** v okně webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="86d13-243">The Azure Management Portal: Select **Get publish profile** from the  Web App blade.</span></span>

<span data-ttu-id="86d13-244">`Username`můžete najít v profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="86d13-244">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="86d13-245">Profil publikování se následující ukázka používá "Web11112 – nasazení webu":</span><span class="sxs-lookup"><span data-stu-id="86d13-245">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="86d13-246">Vyloučení souborů</span><span class="sxs-lookup"><span data-stu-id="86d13-246">Excluding files</span></span>

<span data-ttu-id="86d13-247">Při publikování webové aplikace ASP.NET Core, sestavení artefaktů a obsah *wwwroot* složky jsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="86d13-247">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="86d13-248">`msbuild`podporuje [expanze názvů vzory](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="86d13-248">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="86d13-249">Například následující `<Content>` element značek vyloučí všechny text (*.txt*) souborů z *wwwroot a obsah* složce a jejích podsložkách.</span><span class="sxs-lookup"><span data-stu-id="86d13-249">For example, the following `<Content>` element markup will exclude all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="86d13-250">Výše uvedený kód lze přidat do profilu publikování nebo *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="86d13-250">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="86d13-251">Když je přidán do *.csproj* , pravidlo se přidá soubor všechny profily publikování v projektu.</span><span class="sxs-lookup"><span data-stu-id="86d13-251">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="86d13-252">Následující `<MsDeploySkipRules>` element značek vyloučeny všechny soubory z *wwwroot a obsah* složky:</span><span class="sxs-lookup"><span data-stu-id="86d13-252">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="86d13-253">`<MsDeploySkipRules>`neodstraní *přeskočit* cíle z webu nasazení.</span><span class="sxs-lookup"><span data-stu-id="86d13-253">`<MsDeploySkipRules>`  will not delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="86d13-254">`<Content>`cílové soubory a složky, se odstraní z webu nasazení.</span><span class="sxs-lookup"><span data-stu-id="86d13-254">`<Content>` targeted files and folders will be deleted from the deployment site.</span></span> <span data-ttu-id="86d13-255">Předpokládejme například, že měl nasadíte webovou aplikaci s následující soubory:</span><span class="sxs-lookup"><span data-stu-id="86d13-255">For example, suppose you had deployed a web app with the following files:</span></span>

- <span data-ttu-id="86d13-256">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="86d13-256">*Views/Home/About1.cshtml*</span></span>
- <span data-ttu-id="86d13-257">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="86d13-257">*Views/Home/About2.cshtml*</span></span>
- <span data-ttu-id="86d13-258">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="86d13-258">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="86d13-259">Pokud jste přidali následující `<MsDeploySkipRules>` značek, těchto souborů nemohli odstranit na webu nasazení.</span><span class="sxs-lookup"><span data-stu-id="86d13-259">If you added the following `<MsDeploySkipRules>` markup, those files would not be deleted on the deployment site.</span></span>

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

<span data-ttu-id="86d13-260">`<MsDeploySkipRules>` Brání značek výše uvedeném *přeskočen* soubory nebudou depoyed, ale bude není odstranit ty soubory, jakmile jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="86d13-260">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed, but will not delete those files once they are deployed.</span></span>

<span data-ttu-id="86d13-261">Následující `<Content>` značek by odstranit cílové soubory v lokalitě nasazení:</span><span class="sxs-lookup"><span data-stu-id="86d13-261">The following `<Content>` markup would delete the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="86d13-262">Pomocí příkazového řádku nasazení s `<Content>` výše uvedený kód by způsobilo výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="86d13-262">Using command line deployment with the `<Content>` markup above would result in output similar to the following:</span></span>

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

## <a name="including-files"></a><span data-ttu-id="86d13-263">Zahrnutí souborů</span><span class="sxs-lookup"><span data-stu-id="86d13-263">Including files</span></span>

<span data-ttu-id="86d13-264">Následující kód slouží k patří *bitové kopie* složky mimo adresář projektu, který má *wwwroot nebo obrázky* složku publikování webu.</span><span class="sxs-lookup"><span data-stu-id="86d13-264">The following markup can be used to include an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="86d13-265">Značky lze přidat do *.csproj* soubor nebo profil publikování.</span><span class="sxs-lookup"><span data-stu-id="86d13-265">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="86d13-266">Pokud je přidán do *.csproj* souboru, bude součástí každý profil publikování v projektu.</span><span class="sxs-lookup"><span data-stu-id="86d13-266">If it's added to the *.csproj* file, it will be included in each publish profile in the project.</span></span>

<span data-ttu-id="86d13-267">Následující zvýrazněnou kód ukazuje, jak na:</span><span class="sxs-lookup"><span data-stu-id="86d13-267">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="86d13-268">Kopírování souboru z mimo projekt do *wwwroot* složky.</span><span class="sxs-lookup"><span data-stu-id="86d13-268">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="86d13-269">Vyloučit *wwwroot\Content* složky.</span><span class="sxs-lookup"><span data-stu-id="86d13-269">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="86d13-270">Vyloučit *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="86d13-270">Exclude *Views\Home\About2.cshtml*.</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

<span data-ttu-id="86d13-271">Najdete v článku [WebSDK Readme](https://github.com/aspnet/websdk) pro další ukázky nasazení.</span><span class="sxs-lookup"><span data-stu-id="86d13-271">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="86d13-272">Spustit cíl před nebo po publikování</span><span class="sxs-lookup"><span data-stu-id="86d13-272">Run a target before or after publishing</span></span>

<span data-ttu-id="86d13-273">Builtin `BeforePublish` a `AfterPublish` cíle můžete použít ke spuštění cíl před nebo po cíl publikování.</span><span class="sxs-lookup"><span data-stu-id="86d13-273">The builtin `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="86d13-274">Následující kód můžete přidat do profilu publikování k protokolování zpráv bude výstup konzoly, před a po publikování:</span><span class="sxs-lookup"><span data-stu-id="86d13-274">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="86d13-275">Služby modulu Kudu</span><span class="sxs-lookup"><span data-stu-id="86d13-275">The Kudu service</span></span>

<span data-ttu-id="86d13-276">Chcete-li zobrazit soubory na webové aplikace Azure, použijte [kudu služby](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="86d13-276">To view the files on your Azure Web App, use the [kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="86d13-277">Připojení `scm` tokenu název nebo webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="86d13-277">Append the `scm` token to the name or your Web App.</span></span> <span data-ttu-id="86d13-278">Příklad:</span><span class="sxs-lookup"><span data-stu-id="86d13-278">For example:</span></span>

| <span data-ttu-id="86d13-279">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="86d13-279">URL</span></span>               | <span data-ttu-id="86d13-280">Výsledek</span><span class="sxs-lookup"><span data-stu-id="86d13-280">Result</span></span>|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | <span data-ttu-id="86d13-281">Webové aplikace</span><span class="sxs-lookup"><span data-stu-id="86d13-281">Web App</span></span> |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="86d13-282">Adresářové kudu</span><span class="sxs-lookup"><span data-stu-id="86d13-282">Kudu sevice</span></span> |

<span data-ttu-id="86d13-283">Vyberte [ladění konzoly](https://github.com/projectkudu/kudu/wiki/Kudu-console) položku nabídky Zobrazit nebo upravit nebo odstranit nebo přidat soubory.</span><span class="sxs-lookup"><span data-stu-id="86d13-283">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="86d13-284">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="86d13-284">Additional resources</span></span>

- <span data-ttu-id="86d13-285">[Nasazení webové](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) zjednodušuje nasazení webové aplikace a weby pro servery služby IIS.</span><span class="sxs-lookup"><span data-stu-id="86d13-285">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy)  (msdeploy) simplifies deployment of Web applications and Web sites to IIS servers.</span></span>

- <span data-ttu-id="86d13-286">[https://github.com/ASPNET/websdk](https://github.com/aspnet/websdk/issues): soubor problémy a žádosti o funkce pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="86d13-286">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
