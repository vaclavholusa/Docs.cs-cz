---
title: Visual Studio publikačních profilů pro nasazení aplikace ASP.NET Core
author: rick-anderson
description: Zjistěte, jak vytvořit profily publikování v sadě Visual Studio a jejich použití pro správu nasazení aplikací ASP.NET Core do různých cílů.
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 751f25f74a0e24eb9ce4f2bd6b2fa462ccb03ecb
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538398"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="9961f-103">Visual Studio publikačních profilů pro nasazení aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9961f-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="9961f-104">Podle [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9961f-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9961f-105">Tento dokument je zaměřený na vytváření a používání pomocí sady Visual Studio 2017 se publikační profily.</span><span class="sxs-lookup"><span data-stu-id="9961f-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="9961f-106">Profily publikování vytvořené pomocí sady Visual Studio můžete spustit z nástroje MSBuild a sadě Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="9961f-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="9961f-107">Zobrazit [publikovat webovou aplikaci ASP.NET Core do služby Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny k publikování do Azure.</span><span class="sxs-lookup"><span data-stu-id="9961f-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="9961f-108">Následující soubor projektu byl vytvořen pomocí příkazu `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="9961f-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9961f-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9961f-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.1.4" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9961f-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9961f-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="9961f-111">`<Project>` Elementu `Sdk` atribut provádí následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="9961f-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="9961f-112">Importuje soubor vlastnosti z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* na začátku.</span><span class="sxs-lookup"><span data-stu-id="9961f-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="9961f-113">Importuje soubor cílů z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* na konci.</span><span class="sxs-lookup"><span data-stu-id="9961f-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="9961f-114">Výchozím umístěním pro `MSBuildSDKsPath` (pomocí sady Visual Studio 2017 Enterprise) je *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* složky.</span><span class="sxs-lookup"><span data-stu-id="9961f-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="9961f-115">`Microsoft.NET.Sdk.Web` SDK závisí na:</span><span class="sxs-lookup"><span data-stu-id="9961f-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="9961f-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="9961f-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="9961f-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="9961f-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="9961f-118">Což způsobí, že následující vlastnosti a cíle, které se mají naimportovat:</span><span class="sxs-lookup"><span data-stu-id="9961f-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="9961f-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="9961f-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="9961f-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="9961f-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="9961f-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="9961f-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="9961f-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="9961f-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="9961f-123">Publikování import cílů vpravo sadu cílů založené na metodě publikování použít.</span><span class="sxs-lookup"><span data-stu-id="9961f-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="9961f-124">Při načtení projektu nástroje MSBuild nebo Visual Studio provedou tyto akce vysoké úrovně:</span><span class="sxs-lookup"><span data-stu-id="9961f-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="9961f-125">Sestavit projekt</span><span class="sxs-lookup"><span data-stu-id="9961f-125">Build project</span></span>
* <span data-ttu-id="9961f-126">COMPUTE souborů k publikování</span><span class="sxs-lookup"><span data-stu-id="9961f-126">Compute files to publish</span></span>
* <span data-ttu-id="9961f-127">Publikování souborů do cíle</span><span class="sxs-lookup"><span data-stu-id="9961f-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="9961f-128">COMPUTE položky projektu</span><span class="sxs-lookup"><span data-stu-id="9961f-128">Compute project items</span></span>

<span data-ttu-id="9961f-129">Při načtení projektu jsou vypočítány položky projektu (soubory).</span><span class="sxs-lookup"><span data-stu-id="9961f-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="9961f-130">`item type` Atribut určuje, jak se zpracovávají souboru.</span><span class="sxs-lookup"><span data-stu-id="9961f-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="9961f-131">Ve výchozím nastavení *.cs* soubory jsou zahrnuty v `Compile` seznam položek.</span><span class="sxs-lookup"><span data-stu-id="9961f-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="9961f-132">Soubory `Compile` jsou zkompilovány seznam položek.</span><span class="sxs-lookup"><span data-stu-id="9961f-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="9961f-133">`Content` Seznamu položek obsahuje soubory, které jsou publikovány kromě výstupy sestavení.</span><span class="sxs-lookup"><span data-stu-id="9961f-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="9961f-134">Ve výchozím nastavení, soubory odpovídající vzoru `wwwroot/**` jsou součástí `Content` položky.</span><span class="sxs-lookup"><span data-stu-id="9961f-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="9961f-135">`wwwroot/\*\*` [Model podpory zástupných znaků](https://gruntjs.com/configuring-tasks#globbing-patterns) vyhledá všechny soubory v *wwwroot* složky **a** podsložky.</span><span class="sxs-lookup"><span data-stu-id="9961f-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="9961f-136">Pokud chcete explicitně přidat soubor do seznamu publikovat, přidejte přímo v souboru *.csproj* sdílené, jak je znázorněno v [vložených souborů](#include-files).</span><span class="sxs-lookup"><span data-stu-id="9961f-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="9961f-137">Při výběru **publikovat** tlačítko v sadě Visual Studio nebo při publikování z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="9961f-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="9961f-138">Jsou vypočítané vlastnosti/položky (soubory, které jsou potřebné k sestavení).</span><span class="sxs-lookup"><span data-stu-id="9961f-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="9961f-139">**Visual Studio pouze**: obnovení balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="9961f-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="9961f-140">(Obnovit musí být explicitní uživatelem na rozhraní příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="9961f-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="9961f-141">Sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="9961f-141">The project builds.</span></span>
* <span data-ttu-id="9961f-142">Publikovat položky se zpracovávají (soubory, které jsou potřebné k publikování).</span><span class="sxs-lookup"><span data-stu-id="9961f-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="9961f-143">Publikování projektu (vypočítané soubory se zkopírují do cílového umístění publikování).</span><span class="sxs-lookup"><span data-stu-id="9961f-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="9961f-144">Když se odkazuje projekt ASP.NET Core `Microsoft.NET.Sdk.Web` v souboru projektu *app_offline.htm* soubor umístěn v kořenovém adresáři webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9961f-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="9961f-145">Pokud soubor existuje, modul ASP.NET Core řádně ukončí aplikaci, slouží *app_offline.htm* souboru během nasazení.</span><span class="sxs-lookup"><span data-stu-id="9961f-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="9961f-146">Další informace najdete v tématu [odkaz Konfigurace modul ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="9961f-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="9961f-147">Publikování základních příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9961f-147">Basic command-line publishing</span></span>

<span data-ttu-id="9961f-148">Příkazový řádek publikování funguje na všech platformách .NET Core podporovány a nevyžaduje, aby Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9961f-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="9961f-149">Níže [dotnet publikovat](/dotnet/core/tools/dotnet-publish) příkaz spustí z adresáře projektu (která obsahuje *.csproj* souboru).</span><span class="sxs-lookup"><span data-stu-id="9961f-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="9961f-150">Pokud není ve složce projektu explicitně předávat v cestě k souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="9961f-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="9961f-151">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9961f-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="9961f-152">Spusťte následující příkazy k vytvoření a publikování webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="9961f-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9961f-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9961f-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9961f-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9961f-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="9961f-155">[Dotnet publikovat](/dotnet/core/tools/dotnet-publish) příkaz vytváří výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="9961f-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="9961f-156">Výchozí nastavení složky pro publikování je `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="9961f-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="9961f-157">Výchozí nastavení pro `$(Configuration)` je *ladění*.</span><span class="sxs-lookup"><span data-stu-id="9961f-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="9961f-158">V předchozím příkladu `<TargetFramework>` je `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="9961f-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="9961f-159">`dotnet publish -h` Zobrazí nápovědu pro publikování.</span><span class="sxs-lookup"><span data-stu-id="9961f-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="9961f-160">Následující příkaz určuje `Release` sestavení a publikování adresáře:</span><span class="sxs-lookup"><span data-stu-id="9961f-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="9961f-161">[Dotnet publikovat](/dotnet/core/tools/dotnet-publish) volání MSBuild, který vyvolá příkaz `Publish` cíl.</span><span class="sxs-lookup"><span data-stu-id="9961f-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="9961f-162">Žádné parametry předány `dotnet publish` jsou předávaným do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9961f-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="9961f-163">`-c` Parametr se mapuje `Configuration` vlastnosti Msbuildu.</span><span class="sxs-lookup"><span data-stu-id="9961f-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="9961f-164">`-o` Parametr mapuje na `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="9961f-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="9961f-165">Vlastnosti nástroje MSBuild mohou být předány některou z následujících formátů:</span><span class="sxs-lookup"><span data-stu-id="9961f-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="9961f-166">Následující příkaz publikuje `Release` od sestavení k síťové sdílené složky:</span><span class="sxs-lookup"><span data-stu-id="9961f-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="9961f-167">Sdílené síťové složky se zadaným lomítka (*//r8/*) a funguje na všech platformách .NET Core podporovány.</span><span class="sxs-lookup"><span data-stu-id="9961f-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="9961f-168">Potvrďte, že publikované aplikace pro nasazení neběží.</span><span class="sxs-lookup"><span data-stu-id="9961f-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="9961f-169">Soubory *publikovat* složky jsou zamčené, když je aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="9961f-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="9961f-170">Nasazení nebyla vytvořena, protože uzamčena, nelze zkopírovat soubory.</span><span class="sxs-lookup"><span data-stu-id="9961f-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="9961f-171">Profily publikování</span><span class="sxs-lookup"><span data-stu-id="9961f-171">Publish profiles</span></span>

<span data-ttu-id="9961f-172">Tato část používá Visual Studio 2017 k vytvoření profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="9961f-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="9961f-173">Po vytvoření publikování ze sady Visual Studio nebo příkazového řádku je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9961f-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="9961f-174">Publikování profilů může zjednodušit proces publikování, a může existovat libovolný počet profilů.</span><span class="sxs-lookup"><span data-stu-id="9961f-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="9961f-175">Vytvořte profil publikování v sadě Visual Studio výběrem jedné z následujících cest:</span><span class="sxs-lookup"><span data-stu-id="9961f-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="9961f-176">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="9961f-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="9961f-177">Vyberte **publikovat &lt;project_name&gt;**  z **sestavení** nabídky.</span><span class="sxs-lookup"><span data-stu-id="9961f-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="9961f-178">**Publikovat** zobrazuje kartu na stránce kapacity aplikace.</span><span class="sxs-lookup"><span data-stu-id="9961f-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="9961f-179">Pokud projekt nemá profil publikování, zobrazí se následující stránka:</span><span class="sxs-lookup"><span data-stu-id="9961f-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![Na kartě Publikovat na stránce kapacity aplikace](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="9961f-181">Když **složky** je vybrána, zadejte cestu ke složce pro ukládání publikovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="9961f-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="9961f-182">Výchozí složkou je *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="9961f-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="9961f-183">Klikněte na tlačítko **vytvořit profil** tlačítko Dokončit.</span><span class="sxs-lookup"><span data-stu-id="9961f-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="9961f-184">Po vytvoření profilu publikování **publikovat** kartě změny.</span><span class="sxs-lookup"><span data-stu-id="9961f-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="9961f-185">V rozevíracím seznamu se zobrazí nově vytvořený profil.</span><span class="sxs-lookup"><span data-stu-id="9961f-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="9961f-186">Klikněte na tlačítko **vytvořit nový profil** vytvořte další nový profil.</span><span class="sxs-lookup"><span data-stu-id="9961f-186">Click **Create new profile** to create another new profile.</span></span>

![Na kartě Publikovat na stránce kapacity aplikace zobrazuje FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="9961f-188">Průvodce publikováním podporuje následující cíle publikování:</span><span class="sxs-lookup"><span data-stu-id="9961f-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="9961f-189">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9961f-189">Azure App Service</span></span>
* <span data-ttu-id="9961f-190">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="9961f-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="9961f-191">Služba IIS, FTP atd., (pro všechny webové servery)</span><span class="sxs-lookup"><span data-stu-id="9961f-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="9961f-192">Folder</span><span class="sxs-lookup"><span data-stu-id="9961f-192">Folder</span></span>
* <span data-ttu-id="9961f-193">Importovat profil</span><span class="sxs-lookup"><span data-stu-id="9961f-193">Import Profile</span></span>

<span data-ttu-id="9961f-194">Další informace najdete v tématu [jaké možnosti publikování jsou pro mě nejlepší](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="9961f-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="9961f-195">Při vytváření profilu publikování pomocí sady Visual Studio *vlastnosti/PublishProfiles/&lt;Název_profilu&gt;.pubxml* se vytvoří soubor MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9961f-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="9961f-196">*.Pubxml* soubor je soubor MSBuild a obsahuje konfigurační nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="9961f-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="9961f-197">Tento soubor můžete změnit na vlastní nastavení sestavení a publikování procesu.</span><span class="sxs-lookup"><span data-stu-id="9961f-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="9961f-198">Tento soubor je pro čtení procesem publikování.</span><span class="sxs-lookup"><span data-stu-id="9961f-198">This file is read by the publishing process.</span></span> <span data-ttu-id="9961f-199">`<LastUsedBuildConfiguration>` je speciální, protože globální vlastností a nemohou být zapsány všech souborů, které se importují v sestavení.</span><span class="sxs-lookup"><span data-stu-id="9961f-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="9961f-200">Zobrazit [MSBuild: jak nastavit vlastnost konfigurace](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="9961f-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="9961f-201">Při publikování do Azure cíl, *.pubxml* soubor obsahuje identifikátor vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="9961f-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="9961f-202">S cílovým typem přidává se tento soubor do správy zdrojového kódu se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="9961f-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="9961f-203">Při publikování na cíl mimo Azure, je k vrácení se změnami *.pubxml* souboru.</span><span class="sxs-lookup"><span data-stu-id="9961f-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="9961f-204">Se šifrují citlivé údaje (třeba publikovat heslo) na úrovni uživatele nebo počítače.</span><span class="sxs-lookup"><span data-stu-id="9961f-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="9961f-205">Je uložený v *vlastnosti/PublishProfiles/&lt;Název_profilu&gt;. pubxml.user* souboru.</span><span class="sxs-lookup"><span data-stu-id="9961f-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="9961f-206">Protože tento soubor můžete uložit citlivé informace, by neměla být zaškrtnuta do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="9961f-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="9961f-207">Přehled o tom, jak publikovat webovou aplikaci v ASP.NET Core, najdete v části [hostitele a nasadit](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="9961f-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="9961f-208">Úlohy MSBuild a cíle plánování pro publikování aplikace ASP.NET Core je open source na https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="9961f-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="9961f-209">`dotnet publish` můžete použít složku, MSDeploy, a [Kudu](https://github.com/projectkudu/kudu/wiki) publikační profily:</span><span class="sxs-lookup"><span data-stu-id="9961f-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="9961f-210">Složka (funguje napříč platformami):</span><span class="sxs-lookup"><span data-stu-id="9961f-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="9961f-211">MSDeploy (aktuálně toto funguje pouze ve Windows, protože není MSDeploy napříč platformami):</span><span class="sxs-lookup"><span data-stu-id="9961f-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="9961f-212">Balíček MSDeploy (aktuálně toto funguje pouze ve Windows, protože není MSDeploy napříč platformami):</span><span class="sxs-lookup"><span data-stu-id="9961f-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="9961f-213">V předchozích ukázkách **není** předat `deployonbuild` k `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="9961f-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="9961f-214">Další informace najdete v tématu [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="9961f-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="9961f-215">`dotnet publish` podporuje rozhraní API Kudu k publikování do Azure z jakékoliv platformy.</span><span class="sxs-lookup"><span data-stu-id="9961f-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="9961f-216">Visual Studio publikování podporuje Kudu rozhraní API, ale je podporovaný modulem WebSDK pro různé platformy publikovat do Azure.</span><span class="sxs-lookup"><span data-stu-id="9961f-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="9961f-217">Přidat profil publikování *vlastnosti/PublishProfiles* složka s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="9961f-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="9961f-218">Spusťte následující příkaz, který zip obsah publikovat a publikujete ji do Azure pomocí rozhraní API Kudu:</span><span class="sxs-lookup"><span data-stu-id="9961f-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="9961f-219">Při použití profil publikování, nastavte následující vlastnosti MSBuild:</span><span class="sxs-lookup"><span data-stu-id="9961f-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="9961f-220">Při publikování tak, že profil s názvem *FolderProfile*, lze provést jednu z následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="9961f-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="9961f-221">Při vyvolání [dotnet sestavení](/dotnet/core/tools/dotnet-build), volá `msbuild` ke spuštění sestavení a publikování procesu.</span><span class="sxs-lookup"><span data-stu-id="9961f-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="9961f-222">Volání na buď `dotnet build` nebo `msbuild` je ekvivalentní při předávání ve složce profilu.</span><span class="sxs-lookup"><span data-stu-id="9961f-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="9961f-223">Při volání nástroje MSBuild přímo na Windows, se používá rozhraní .NET Framework verze nástroje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9961f-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="9961f-224">Je aktuálně omezená na počítače s Windows pro publikování MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="9961f-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="9961f-225">Volání `dotnet build` do jiné složky profil vyvolá MSBuild a nástroj MSBuild používá MSDeploy v jiné složce profily.</span><span class="sxs-lookup"><span data-stu-id="9961f-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="9961f-226">Volání `dotnet build` v jiné složce profilu vyvolá MSBuild (pomocí MSDeploy) a vede k chybě (i když spuštěn na platformě Windows).</span><span class="sxs-lookup"><span data-stu-id="9961f-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="9961f-227">Pokud chcete publikovat pomocí profilu bez složky, přímo voláním funkce MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9961f-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="9961f-228">Profil publikování následující složka byla vytvořena pomocí sady Visual Studio a publikuje do sdílené síťové složky:</span><span class="sxs-lookup"><span data-stu-id="9961f-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="9961f-229">Poznámka: `<LastUsedBuildConfiguration>` je nastavena na `Release`.</span><span class="sxs-lookup"><span data-stu-id="9961f-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="9961f-230">Při publikování ze sady Visual Studio `<LastUsedBuildConfiguration>` hodnota vlastnosti konfigurace je nastavena pomocí hodnoty, když se spustí proces publikování.</span><span class="sxs-lookup"><span data-stu-id="9961f-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="9961f-231">`<LastUsedBuildConfiguration>` Vlastnost konfigurace je speciální a nesmí být přepsána nastaveními v importovaný soubor MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9961f-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="9961f-232">Tato vlastnost může být přepsána z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9961f-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="9961f-233">Použití .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="9961f-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="9961f-234">Použití nástroje MSBuild:</span><span class="sxs-lookup"><span data-stu-id="9961f-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="9961f-235">Publikování do koncového bodu MSDeploy z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9961f-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="9961f-236">Publikování můžete udělat pomocí rozhraní příkazového řádku .NET Core nebo MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9961f-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="9961f-237">`dotnet publish` spuštění v rámci .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9961f-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="9961f-238">`msbuild` Příkaz vyžaduje rozhraní .NET Framework, který omezuje do prostředí Windows.</span><span class="sxs-lookup"><span data-stu-id="9961f-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="9961f-239">Nejdřív vytvořte profil publikování v sadě Visual Studio 2017 a použít profil z příkazového řádku je nejjednodušší způsob, jak publikovat pomocí nástroje MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="9961f-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="9961f-240">V následujícím příkladu se vytvoří webová aplikace ASP.NET Core (pomocí `dotnet new mvc`), a přidá se profil publikování Azure s Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9961f-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="9961f-241">Spustit `msbuild` z **Developer Command Prompt for VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="9961f-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="9961f-242">Developer Command Prompt má správný *msbuild.exe* v cestě některé sadou proměnných MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9961f-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="9961f-243">Nástroj MSBuild používá následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="9961f-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="9961f-244">Získejte `Password` z  *\<publikovat jméno >. PublishSettings* souboru.</span><span class="sxs-lookup"><span data-stu-id="9961f-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="9961f-245">Stáhněte si *. PublishSettings* soubor z aplikace:</span><span class="sxs-lookup"><span data-stu-id="9961f-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="9961f-246">Průzkumník řešení: Klikněte pravým tlačítkem na webovou aplikaci a vyberte **stáhnout profil publikování**.</span><span class="sxs-lookup"><span data-stu-id="9961f-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="9961f-247">Azure portal: klikněte na tlačítko **získat profil publikování** ve webové aplikaci **přehled** panelu.</span><span class="sxs-lookup"><span data-stu-id="9961f-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="9961f-248">`Username` nenašlo se v profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="9961f-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="9961f-249">Následující ukázkový používá *Web11112 – nasazení webu* profil publikování:</span><span class="sxs-lookup"><span data-stu-id="9961f-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="9961f-250">Vyloučení souborů</span><span class="sxs-lookup"><span data-stu-id="9961f-250">Exclude files</span></span>

<span data-ttu-id="9961f-251">Při publikování webové aplikace ASP.NET Core, artefakty sestavení a obsah *wwwroot* složky jsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="9961f-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="9961f-252">`msbuild` podporuje [vzorů podpory zástupných znaků](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="9961f-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="9961f-253">Například následující `<Content>` element vyloučí veškerý text (*.txt*) souborů z doručené pošty *wwwroot/obsah* složce a jejích podsložkách.</span><span class="sxs-lookup"><span data-stu-id="9961f-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="9961f-254">Předchozí kód lze přidat do profilu publikování nebo *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="9961f-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="9961f-255">Když se přidá *.csproj* , pravidlo se přidá soubor všechny profily publikování v projektu.</span><span class="sxs-lookup"><span data-stu-id="9961f-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="9961f-256">Následující `<MsDeploySkipRules>` element vyloučí všechny soubory *wwwroot/obsah* složky:</span><span class="sxs-lookup"><span data-stu-id="9961f-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="9961f-257">`<MsDeploySkipRules>` nedojde k odstranění *přeskočit* cílů nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="9961f-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="9961f-258">`<Content>` cílové soubory a složky, jsou odstraněny z nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="9961f-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="9961f-259">Předpokládejme například, že nasazené webové aplikace má následující soubory:</span><span class="sxs-lookup"><span data-stu-id="9961f-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="9961f-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9961f-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="9961f-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9961f-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="9961f-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9961f-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="9961f-263">Pokud následující `<MsDeploySkipRules>` prvky jsou přidány, by dojít k odstranění těchto souborů na web pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="9961f-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
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

<span data-ttu-id="9961f-264">Předchozí `<MsDeploySkipRules>` zabránit prvky *přeskočeno* soubory z nasazení.</span><span class="sxs-lookup"><span data-stu-id="9961f-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="9961f-265">Tyto soubory neodstraní po nasazení.</span><span class="sxs-lookup"><span data-stu-id="9961f-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="9961f-266">Následující `<Content>` element odstraní cílové soubory na web pro nasazení:</span><span class="sxs-lookup"><span data-stu-id="9961f-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="9961f-267">Pomocí příkazového řádku nasazení s předchozím `<Content>` element provede následující výstup:</span><span class="sxs-lookup"><span data-stu-id="9961f-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

```console
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

## <a name="include-files"></a><span data-ttu-id="9961f-268">Soubory k zahrnutí</span><span class="sxs-lookup"><span data-stu-id="9961f-268">Include files</span></span>

<span data-ttu-id="9961f-269">Obsahuje následující kód *image* mimo adresář projektu do složky *wwwroot/imagí* složku publikování webu:</span><span class="sxs-lookup"><span data-stu-id="9961f-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="9961f-270">Značky mohou být přidány do *.csproj* souboru nebo profil publikování.</span><span class="sxs-lookup"><span data-stu-id="9961f-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="9961f-271">Pokud je přidána do *.csproj* souboru, je zahrnutý v každé profilu publikování v projektu.</span><span class="sxs-lookup"><span data-stu-id="9961f-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="9961f-272">Následující zvýrazněný kód ukazuje jak na:</span><span class="sxs-lookup"><span data-stu-id="9961f-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="9961f-273">Zkopírovat soubor z mimo projekt sketchflow *wwwroot* složky.</span><span class="sxs-lookup"><span data-stu-id="9961f-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="9961f-274">Vyloučit *wwwroot\Content* složky.</span><span class="sxs-lookup"><span data-stu-id="9961f-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="9961f-275">Vyloučit *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9961f-275">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="9961f-276">Zobrazit [WebSDK Readme](https://github.com/aspnet/websdk) pro další nasazení ukázky.</span><span class="sxs-lookup"><span data-stu-id="9961f-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="9961f-277">Spustit cíl před nebo po publikování</span><span class="sxs-lookup"><span data-stu-id="9961f-277">Run a target before or after publishing</span></span>

<span data-ttu-id="9961f-278">Předdefinované `BeforePublish` a `AfterPublish` cíle spustit cíl před nebo za cíl publikování.</span><span class="sxs-lookup"><span data-stu-id="9961f-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="9961f-279">Přidáte do profilu publikování k protokolování zpráv konzoly před i po publikování následující prvky:</span><span class="sxs-lookup"><span data-stu-id="9961f-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="9961f-280">Publikovat na server pomocí nedůvěryhodného certifikátu</span><span class="sxs-lookup"><span data-stu-id="9961f-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="9961f-281">Přidat `<AllowUntrustedCertificate>` vlastnost s hodnotou `True` do profilu publikování:</span><span class="sxs-lookup"><span data-stu-id="9961f-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="9961f-282">Kudu service</span><span class="sxs-lookup"><span data-stu-id="9961f-282">The Kudu service</span></span>

<span data-ttu-id="9961f-283">Chcete-li zobrazit soubory v nasazení služby Azure App Service webovou aplikaci, použijte [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="9961f-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="9961f-284">Připojit `scm` tokenu název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9961f-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="9961f-285">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9961f-285">For example:</span></span>

| <span data-ttu-id="9961f-286">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="9961f-286">URL</span></span>                                    | <span data-ttu-id="9961f-287">Výsledek</span><span class="sxs-lookup"><span data-stu-id="9961f-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="9961f-288">Webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9961f-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="9961f-289">Kudu service</span><span class="sxs-lookup"><span data-stu-id="9961f-289">Kudu service</span></span> |

<span data-ttu-id="9961f-290">Vyberte [ladění konzoly](https://github.com/projectkudu/kudu/wiki/Kudu-console) položka nabídky zobrazit, upravit, odstranit nebo přidat soubory.</span><span class="sxs-lookup"><span data-stu-id="9961f-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9961f-291">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9961f-291">Additional resources</span></span>

* <span data-ttu-id="9961f-292">[Webu nasadit](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) zjednodušuje nasazení webové aplikace a weby pro servery služby IIS.</span><span class="sxs-lookup"><span data-stu-id="9961f-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="9961f-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Soubor problémů a požádat o funkce pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="9961f-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="9961f-294">Publikování webové aplikace v ASP.NET do virtuálního počítače Azure ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9961f-294">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
