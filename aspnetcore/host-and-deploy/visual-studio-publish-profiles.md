---
title: Visual Studio publikační profily pro nasazení aplikace ASP.NET Core
author: rick-anderson
description: Naučte se vytvářet profily publikování v sadě Visual Studio a použít je pro správu nasazení aplikací ASP.NET Core do různých cíle.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483367"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="46eb1-103">Visual Studio publikační profily pro nasazení aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="46eb1-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="46eb1-104">Podle [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="46eb1-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="46eb1-105">Tento dokument se zaměřuje na použití Visual Studio 2017 k vytvoření a používání publikační profily.</span><span class="sxs-lookup"><span data-stu-id="46eb1-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="46eb1-106">Profily publikování, vytvořené pomocí sady Visual Studio můžete spustit z nástroje MSBuild a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="46eb1-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="46eb1-107">V tématu [publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny k publikování do Azure.</span><span class="sxs-lookup"><span data-stu-id="46eb1-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="46eb1-108">Následující soubor projektu byl vytvořen pomocí příkazu `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="46eb1-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46eb1-109">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="46eb1-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46eb1-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46eb1-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="46eb1-111">`<Project>` Elementu `Sdk` atribut provádí následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="46eb1-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="46eb1-112">Importuje soubor vlastnosti z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* na začátku.</span><span class="sxs-lookup"><span data-stu-id="46eb1-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="46eb1-113">Importuje soubor cíle z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* na konci.</span><span class="sxs-lookup"><span data-stu-id="46eb1-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="46eb1-114">Výchozím umístěním pro `MSBuildSDKsPath` (s Visual Studio 2017 Enterprise) je *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* složky.</span><span class="sxs-lookup"><span data-stu-id="46eb1-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="46eb1-115">`Microsoft.NET.Sdk.Web` SDK závisí na:</span><span class="sxs-lookup"><span data-stu-id="46eb1-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="46eb1-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="46eb1-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="46eb1-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="46eb1-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="46eb1-118">Což způsobí, že následující vlastnosti a cílů určených k importu:</span><span class="sxs-lookup"><span data-stu-id="46eb1-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="46eb1-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="46eb1-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="46eb1-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="46eb1-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="46eb1-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="46eb1-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="46eb1-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="46eb1-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="46eb1-123">Publikujte cíle import právo sada cíle založené na metodě publikování použít.</span><span class="sxs-lookup"><span data-stu-id="46eb1-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="46eb1-124">Při načtení projektu nástroje MSBuild nebo Visual Studio dojde k vysoké úrovně takto:</span><span class="sxs-lookup"><span data-stu-id="46eb1-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="46eb1-125">Sestavení projektu</span><span class="sxs-lookup"><span data-stu-id="46eb1-125">Build project</span></span>
* <span data-ttu-id="46eb1-126">Výpočetní soubory k publikování</span><span class="sxs-lookup"><span data-stu-id="46eb1-126">Compute files to publish</span></span>
* <span data-ttu-id="46eb1-127">Publikovat soubory do cílového umístění</span><span class="sxs-lookup"><span data-stu-id="46eb1-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="46eb1-128">Výpočetní položky projektu</span><span class="sxs-lookup"><span data-stu-id="46eb1-128">Compute project items</span></span>

<span data-ttu-id="46eb1-129">Při načtení projektu, se vypočítávají položky projektu (soubory).</span><span class="sxs-lookup"><span data-stu-id="46eb1-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="46eb1-130">`item type` Atribut určuje způsob zpracování souboru.</span><span class="sxs-lookup"><span data-stu-id="46eb1-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="46eb1-131">Ve výchozím nastavení *.cs* souborů jsou součástí `Compile` seznamu položek.</span><span class="sxs-lookup"><span data-stu-id="46eb1-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="46eb1-132">Soubory `Compile` kompilované seznamu položek.</span><span class="sxs-lookup"><span data-stu-id="46eb1-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="46eb1-133">`Content` Seznamu položek obsahuje soubory, které jsou publikovány kromě výstupy sestavení.</span><span class="sxs-lookup"><span data-stu-id="46eb1-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="46eb1-134">Ve výchozím nastavení, soubory odpovídající vzoru `wwwroot/**` jsou součástí `Content` položky.</span><span class="sxs-lookup"><span data-stu-id="46eb1-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="46eb1-135">`wwwroot/\*\*` [Expanze názvů vzor](https://gruntjs.com/configuring-tasks#globbing-patterns) odpovídá všechny soubory v *wwwroot* složky **a** podsložky.</span><span class="sxs-lookup"><span data-stu-id="46eb1-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="46eb1-136">Pokud chcete explicitně přidat do seznamu publikovat soubor, přidejte přímo v souboru *.csproj* souboru, jak je znázorněno v [souborů](#include-files).</span><span class="sxs-lookup"><span data-stu-id="46eb1-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="46eb1-137">Při výběru **publikovat** tlačítko v sadě Visual Studio nebo při publikování z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="46eb1-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="46eb1-138">Nebude vlastnosti se vypočítávají (soubory, které jsou potřebné k vytvoření).</span><span class="sxs-lookup"><span data-stu-id="46eb1-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="46eb1-139">**Visual Studio pouze**: obnovení balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="46eb1-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="46eb1-140">(Obnovit, musí být explicitní uživatelem v rozhraní příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="46eb1-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="46eb1-141">Sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="46eb1-141">The project builds.</span></span>
* <span data-ttu-id="46eb1-142">Publikování položky se vypočítávají (soubory, které jsou potřebné k publikování).</span><span class="sxs-lookup"><span data-stu-id="46eb1-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="46eb1-143">Po publikování projektu (počítaný soubory se zkopírují do cílového umístění publikovat).</span><span class="sxs-lookup"><span data-stu-id="46eb1-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="46eb1-144">Když projekt ASP.NET Core odkazuje `Microsoft.NET.Sdk.Web` v souboru projektu, *app_offline.htm* soubor je umístěn v kořenovém adresáři webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="46eb1-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="46eb1-145">Když se soubor nachází, modul ASP.NET Core řádně ukončí aplikaci a slouží *app_offline.htm* souboru během nasazení.</span><span class="sxs-lookup"><span data-stu-id="46eb1-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="46eb1-146">Další informace najdete v tématu [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="46eb1-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="46eb1-147">Základní publikování příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="46eb1-147">Basic command-line publishing</span></span>

<span data-ttu-id="46eb1-148">Příkazového řádku publikování funguje na všech platformách podporovaných .NET Core a nevyžaduje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="46eb1-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="46eb1-149">V níže, ukázky [dotnet publikování](/dotnet/core/tools/dotnet-publish) příkaz spuštěn z adresáře projektu (obsahující *.csproj* souboru).</span><span class="sxs-lookup"><span data-stu-id="46eb1-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="46eb1-150">Pokud není ve složce projektu explicitně předat v cestě k souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="46eb1-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="46eb1-151">Příklad:</span><span class="sxs-lookup"><span data-stu-id="46eb1-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="46eb1-152">Spusťte následující příkazy k vytvoření a publikování webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="46eb1-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46eb1-153">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="46eb1-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46eb1-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46eb1-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="46eb1-155">[Dotnet publikování](/dotnet/core/tools/dotnet-publish) příkaz vytváří výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="46eb1-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="46eb1-156">Výchozí nastavení publikování složky je `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="46eb1-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="46eb1-157">Výchozí hodnota pro `$(Configuration)` je *ladění*.</span><span class="sxs-lookup"><span data-stu-id="46eb1-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="46eb1-158">V předchozím příkladu `<TargetFramework>` je `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="46eb1-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="46eb1-159">`dotnet publish -h` Zobrazí nápovědu informace pro publikování.</span><span class="sxs-lookup"><span data-stu-id="46eb1-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="46eb1-160">Určuje následující příkaz `Release` sestavení a publikování adresáře:</span><span class="sxs-lookup"><span data-stu-id="46eb1-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="46eb1-161">[Dotnet publikování](/dotnet/core/tools/dotnet-publish) příkaz volání MSBuild, které vyvolá `Publish` cíl.</span><span class="sxs-lookup"><span data-stu-id="46eb1-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="46eb1-162">Žádné parametry předaný `dotnet publish` jsou předávány MSBuild.</span><span class="sxs-lookup"><span data-stu-id="46eb1-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="46eb1-163">`-c` Parametr se mapuje `Configuration` vlastnosti MSBuild.</span><span class="sxs-lookup"><span data-stu-id="46eb1-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="46eb1-164">`-o` Parametr mapuje `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="46eb1-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="46eb1-165">Vlastnosti nástroje MSBuild lze předat některou z následujících formátů:</span><span class="sxs-lookup"><span data-stu-id="46eb1-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="46eb1-166">Publikuje následující příkaz `Release` sestavení do sdílené síťové složky:</span><span class="sxs-lookup"><span data-stu-id="46eb1-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="46eb1-167">Sdílené síťové složce je zadán s lomítkem (*//r8/*) a funguje na všech platformách .NET Core podporována.</span><span class="sxs-lookup"><span data-stu-id="46eb1-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="46eb1-168">Potvrďte, že není spuštěn publikované aplikace pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="46eb1-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="46eb1-169">Soubory *publikování* složky jsou zamčené, když aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="46eb1-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="46eb1-170">Nasazení nemůže proběhnout, protože uzamčen, že soubory nelze kopírovat.</span><span class="sxs-lookup"><span data-stu-id="46eb1-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="46eb1-171">Publikační profily</span><span class="sxs-lookup"><span data-stu-id="46eb1-171">Publish profiles</span></span>

<span data-ttu-id="46eb1-172">Tato část používá Visual Studio 2017 k vytvoření profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="46eb1-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="46eb1-173">Po vytvoření publikování ze sady Visual Studio nebo pomocí příkazového řádku je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="46eb1-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="46eb1-174">Publikování profily může zjednodušit proces publikování a může existovat libovolný počet profilů.</span><span class="sxs-lookup"><span data-stu-id="46eb1-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="46eb1-175">Vytvořte profil publikování v sadě Visual Studio volbou jedné z následujících cestách:</span><span class="sxs-lookup"><span data-stu-id="46eb1-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="46eb1-176">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="46eb1-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="46eb1-177">Vyberte **publikovat &lt;název_projektu&gt;**  z **sestavení** nabídky.</span><span class="sxs-lookup"><span data-stu-id="46eb1-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="46eb1-178">**Publikovat** kapacity stránky aplikace se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="46eb1-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="46eb1-179">Pokud projekt chybí profil publikování, zobrazí se následující stránka:</span><span class="sxs-lookup"><span data-stu-id="46eb1-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![Na kartě Publikovat kapacity stránky aplikace](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="46eb1-181">Když **složky** je vybrána, zadejte cestu ke složce pro uložení publikovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="46eb1-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="46eb1-182">Výchozí složkou je *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="46eb1-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="46eb1-183">Klikněte **vytvořit profil** tlačítko Dokončit.</span><span class="sxs-lookup"><span data-stu-id="46eb1-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="46eb1-184">Po vytvoření profilu publikování **publikovat** kartě změny.</span><span class="sxs-lookup"><span data-stu-id="46eb1-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="46eb1-185">Nově vytvořený profil se objeví v rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="46eb1-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="46eb1-186">Klikněte na tlačítko **vytvořit nový profil** vytvořit jinou nový profil.</span><span class="sxs-lookup"><span data-stu-id="46eb1-186">Click **Create new profile** to create another new profile.</span></span>

![Na kartě publikovat aplikace kapacity stránky zobrazující FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="46eb1-188">Průvodce Publikovat podporuje následující cíle publikování:</span><span class="sxs-lookup"><span data-stu-id="46eb1-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="46eb1-189">Aplikační služba Azure</span><span class="sxs-lookup"><span data-stu-id="46eb1-189">Azure App Service</span></span>
* <span data-ttu-id="46eb1-190">Virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="46eb1-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="46eb1-191">Služba IIS, FTP, atd. (pro všechny webové servery)</span><span class="sxs-lookup"><span data-stu-id="46eb1-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="46eb1-192">Folder</span><span class="sxs-lookup"><span data-stu-id="46eb1-192">Folder</span></span>
* <span data-ttu-id="46eb1-193">Importovat profil</span><span class="sxs-lookup"><span data-stu-id="46eb1-193">Import Profile</span></span>

<span data-ttu-id="46eb1-194">Další informace najdete v tématu [jaké možnosti publikování je pro mě nejlepší](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="46eb1-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="46eb1-195">Při vytváření profilu publikování pomocí sady Visual Studio *vlastnosti/PublishProfiles/&lt;Název_profilu&gt;.pubxml* k vytvoření souboru MSBuild.</span><span class="sxs-lookup"><span data-stu-id="46eb1-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="46eb1-196">*.Pubxml* soubor je soubor MSBuild a obsahuje konfigurační nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="46eb1-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="46eb1-197">Tento soubor můžete změnit na přizpůsobení sestavení a publikování procesu.</span><span class="sxs-lookup"><span data-stu-id="46eb1-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="46eb1-198">Proces publikování je pro čtení tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="46eb1-198">This file is read by the publishing process.</span></span> <span data-ttu-id="46eb1-199">`<LastUsedBuildConfiguration>` je speciální, protože je globální vlastnost a by neměl být ve všech souborů, které je v sestavení naimportována.</span><span class="sxs-lookup"><span data-stu-id="46eb1-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="46eb1-200">V tématu [MSBuild: jak nastavit vlastnost konfigurace](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="46eb1-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="46eb1-201">Při publikování Azure cíl, *.pubxml* soubor obsahuje ID vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="46eb1-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="46eb1-202">Přidání tohoto souboru do správy zdrojového kódu se nedoporučuje se tento typ cíle.</span><span class="sxs-lookup"><span data-stu-id="46eb1-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="46eb1-203">Při publikování na cíl, mimo Azure, je možné vrátit se změnami *.pubxml* souboru.</span><span class="sxs-lookup"><span data-stu-id="46eb1-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="46eb1-204">Citlivých informací (jako je heslo publikování) se šifrují úrovni uživatele nebo počítače.</span><span class="sxs-lookup"><span data-stu-id="46eb1-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="46eb1-205">Je uložený v *vlastnosti/PublishProfiles/&lt;Název_profilu&gt;. pubxml.user* souboru.</span><span class="sxs-lookup"><span data-stu-id="46eb1-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="46eb1-206">Protože tento soubor můžete ukládat citlivé informace, by neměla být zaškrtnuta do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="46eb1-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="46eb1-207">Přehled o tom, jak publikovat webovou aplikaci na ASP.NET Core najdete v tématu [hostitele a nasazení](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="46eb1-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="46eb1-208">Úlohy nástroje MSBuild a cíle, které jsou nezbytné pro publikování aplikace ASP.NET Core jsou open source v https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="46eb1-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="46eb1-209">`dotnet publish` můžete použít složku, MSDeploy, a [Kudu](https://github.com/projectkudu/kudu/wiki) publikační profily:</span><span class="sxs-lookup"><span data-stu-id="46eb1-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="46eb1-210">Složka (funguje napříč platformami):</span><span class="sxs-lookup"><span data-stu-id="46eb1-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="46eb1-211">MSDeploy (aktuálně toto funguje pouze v systému Windows, protože není MSDeploy napříč platformami):</span><span class="sxs-lookup"><span data-stu-id="46eb1-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="46eb1-212">Balíček MSDeploy (aktuálně toto funguje pouze v systému Windows, protože není MSDeploy napříč platformami):</span><span class="sxs-lookup"><span data-stu-id="46eb1-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="46eb1-213">V předchozích ukázkách **nemáte** předat `deployonbuild` k `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="46eb1-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="46eb1-214">Další informace najdete v tématu [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="46eb1-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="46eb1-215">`dotnet publish` podporuje rozhraní API modulu Kudu pro publikování do služby Azure z libovolné platformě.</span><span class="sxs-lookup"><span data-stu-id="46eb1-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="46eb1-216">Visual Studio publikovat podporuje rozhraní API modulu Kudu, ale podporuje WebSDK pro různé platformy publikování v Azure.</span><span class="sxs-lookup"><span data-stu-id="46eb1-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="46eb1-217">Přidat profil publikování, který *vlastnosti nebo PublishProfiles* složku s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="46eb1-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="46eb1-218">Spusťte následující příkaz, který zip si obsah publikovat a publikujete ho v Azure pomocí rozhraní API modulu Kudu:</span><span class="sxs-lookup"><span data-stu-id="46eb1-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="46eb1-219">Při použití profil publikování, nastavte následující vlastnosti nástroje MSBuild:</span><span class="sxs-lookup"><span data-stu-id="46eb1-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="46eb1-220">Při publikování k profilu s názvem *FolderProfile*, můžete provést některou z níže uvedených příkazů:</span><span class="sxs-lookup"><span data-stu-id="46eb1-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="46eb1-221">Při vyvolání [dotnet sestavení](/dotnet/core/tools/dotnet-build), zavolá `msbuild` spustit sestavení a publikování procesu.</span><span class="sxs-lookup"><span data-stu-id="46eb1-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="46eb1-222">Volání metody buď `dotnet build` nebo `msbuild` je ekvivalentní při předávání ve složce profilu.</span><span class="sxs-lookup"><span data-stu-id="46eb1-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="46eb1-223">Při volání metody MSBuild přímo v systému Windows, je použít rozhraní .NET Framework verze nástroje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="46eb1-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="46eb1-224">MSDeploy je aktuálně omezená na počítačích s Windows pro publikování.</span><span class="sxs-lookup"><span data-stu-id="46eb1-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="46eb1-225">Volání metody `dotnet build` na do jiné složky profil vyvolá MSBuild a MSBuild používá MSDeploy v jiné složce profily.</span><span class="sxs-lookup"><span data-stu-id="46eb1-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="46eb1-226">Volání metody `dotnet build` na jiný složky profilu vyvolá MSBuild (pomocí MSDeploy) a má za následek selhání (i když běžících na platformě Windows).</span><span class="sxs-lookup"><span data-stu-id="46eb1-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="46eb1-227">Pokud chcete publikovat pomocí profilu pro jiné složky, přímo voláním funkce MSBuild.</span><span class="sxs-lookup"><span data-stu-id="46eb1-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="46eb1-228">Profil publikování se následující složce byl vytvořen pomocí sady Visual Studio a publikuje do sdílené síťové složky:</span><span class="sxs-lookup"><span data-stu-id="46eb1-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="46eb1-229">Poznámka: `<LastUsedBuildConfiguration>` je nastaven na `Release`.</span><span class="sxs-lookup"><span data-stu-id="46eb1-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="46eb1-230">Při publikování ze sady Visual Studio `<LastUsedBuildConfiguration>` je nastavena hodnota vlastnosti konfigurace, pomocí hodnoty, když se spustí proces publikování.</span><span class="sxs-lookup"><span data-stu-id="46eb1-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="46eb1-231">`<LastUsedBuildConfiguration>` Vlastnosti konfigurace je speciální a by nemělo být přepsáno v importovaného souboru MSBuild.</span><span class="sxs-lookup"><span data-stu-id="46eb1-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="46eb1-232">Tato vlastnost může být přepsána z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="46eb1-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="46eb1-233">Pomocí .NET Core rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="46eb1-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="46eb1-234">Pomocí nástroje MSBuild:</span><span class="sxs-lookup"><span data-stu-id="46eb1-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="46eb1-235">Publikovat do koncového bodu MSDeploy z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="46eb1-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="46eb1-236">Publikování můžete provést pomocí rozhraní .NET Core CLI nebo MSBuild.</span><span class="sxs-lookup"><span data-stu-id="46eb1-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="46eb1-237">`dotnet publish` běží v kontextu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="46eb1-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="46eb1-238">`msbuild` Příkaz vyžaduje rozhraní .NET Framework, který omezuje na prostředí Windows.</span><span class="sxs-lookup"><span data-stu-id="46eb1-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="46eb1-239">Nejjednodušší způsob, jak publikovat s MSDeploy je nejdřív vytvořte profil publikování ve Visual Studio 2017 a použít profil z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="46eb1-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="46eb1-240">V následující ukázce je vytvoření webové aplikace ASP.NET Core (pomocí `dotnet new mvc`), a přidají se profilem publikování Azure pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="46eb1-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="46eb1-241">Spustit `msbuild` z **příkazový řádek vývojáře pro VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="46eb1-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="46eb1-242">Příkazový řádek vývojáře má správný *msbuild.exe* v cestě sadou některé nástroje MSBuild proměnné.</span><span class="sxs-lookup"><span data-stu-id="46eb1-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="46eb1-243">MSBuild používá následující syntaxe:</span><span class="sxs-lookup"><span data-stu-id="46eb1-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="46eb1-244">Získat `Password` z  *\<publikovat name >. PublishSettings* souboru.</span><span class="sxs-lookup"><span data-stu-id="46eb1-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="46eb1-245">Stažení *. PublishSettings* soubor z buď:</span><span class="sxs-lookup"><span data-stu-id="46eb1-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="46eb1-246">Průzkumník řešení: Klikněte pravým tlačítkem na webovou aplikaci a vyberte **stažení profilu publikování**.</span><span class="sxs-lookup"><span data-stu-id="46eb1-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="46eb1-247">Portál Azure: klikněte na tlačítko **profilu publikování Get** ve webové aplikaci **přehled** panelu.</span><span class="sxs-lookup"><span data-stu-id="46eb1-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="46eb1-248">`Username` můžete najít v profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="46eb1-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="46eb1-249">Následující ukázkové používá *Web11112 – Web Deploy* profilu publikování:</span><span class="sxs-lookup"><span data-stu-id="46eb1-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="46eb1-250">Vyloučit soubory</span><span class="sxs-lookup"><span data-stu-id="46eb1-250">Exclude files</span></span>

<span data-ttu-id="46eb1-251">Při publikování webové aplikace ASP.NET Core, sestavení artefaktů a obsah *wwwroot* složky jsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="46eb1-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="46eb1-252">`msbuild` podporuje [expanze názvů vzory](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="46eb1-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="46eb1-253">Například následující `<Content>` element vyloučí všechny text (*.txt*) souborů z *wwwroot a obsah* složce a jejích podsložkách.</span><span class="sxs-lookup"><span data-stu-id="46eb1-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="46eb1-254">Předchozí kód lze přidat do profilu publikování nebo *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="46eb1-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="46eb1-255">Když je přidán do *.csproj* , pravidlo se přidá soubor všechny profily publikování v projektu.</span><span class="sxs-lookup"><span data-stu-id="46eb1-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="46eb1-256">Následující `<MsDeploySkipRules>` element vyloučí všechny soubory z *wwwroot a obsah* složky:</span><span class="sxs-lookup"><span data-stu-id="46eb1-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="46eb1-257">`<MsDeploySkipRules>` neodstraní *přeskočit* cíle z webu nasazení.</span><span class="sxs-lookup"><span data-stu-id="46eb1-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="46eb1-258">`<Content>` cílové soubory a složky, se odstraní z webu nasazení.</span><span class="sxs-lookup"><span data-stu-id="46eb1-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="46eb1-259">Předpokládejme například, že nasazené webové aplikace měla následující soubory:</span><span class="sxs-lookup"><span data-stu-id="46eb1-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="46eb1-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="46eb1-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="46eb1-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="46eb1-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="46eb1-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="46eb1-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="46eb1-263">Pokud následující `<MsDeploySkipRules>` těchto souborů nemohli odstranit na webu nasazení, jsou přidány elementy.</span><span class="sxs-lookup"><span data-stu-id="46eb1-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="46eb1-264">Podle předchozích `<MsDeploySkipRules>` elementy zabránit *přeskočen* soubory z nasazení.</span><span class="sxs-lookup"><span data-stu-id="46eb1-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="46eb1-265">Tyto soubory neodstraní poté, co máte nasazené.</span><span class="sxs-lookup"><span data-stu-id="46eb1-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="46eb1-266">Následující `<Content>` element odstraní cílové soubory v lokalitě nasazení:</span><span class="sxs-lookup"><span data-stu-id="46eb1-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="46eb1-267">Pomocí příkazového řádku nasazení s předchozím `<Content>` element vypočítá následující výstup:</span><span class="sxs-lookup"><span data-stu-id="46eb1-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="46eb1-268">Vložené soubory</span><span class="sxs-lookup"><span data-stu-id="46eb1-268">Include files</span></span>

<span data-ttu-id="46eb1-269">Zahrnuje následující kód *bitové kopie* složky mimo adresář projektu, který má *wwwroot nebo obrázky* složku publikování webu:</span><span class="sxs-lookup"><span data-stu-id="46eb1-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="46eb1-270">Značky lze přidat do *.csproj* soubor nebo profil publikování.</span><span class="sxs-lookup"><span data-stu-id="46eb1-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="46eb1-271">Pokud je přidán do *.csproj* souboru, má součástí každý profil publikování v projektu.</span><span class="sxs-lookup"><span data-stu-id="46eb1-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="46eb1-272">Následující zvýrazněnou kód ukazuje, jak na:</span><span class="sxs-lookup"><span data-stu-id="46eb1-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="46eb1-273">Kopírování souboru z mimo projekt do *wwwroot* složky.</span><span class="sxs-lookup"><span data-stu-id="46eb1-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="46eb1-274">Vyloučit *wwwroot\Content* složky.</span><span class="sxs-lookup"><span data-stu-id="46eb1-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="46eb1-275">Vyloučit *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="46eb1-275">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="46eb1-276">Najdete v článku [WebSDK Readme](https://github.com/aspnet/websdk) pro další ukázky nasazení.</span><span class="sxs-lookup"><span data-stu-id="46eb1-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="46eb1-277">Spustit cíl před nebo po publikování</span><span class="sxs-lookup"><span data-stu-id="46eb1-277">Run a target before or after publishing</span></span>

<span data-ttu-id="46eb1-278">Integrované `BeforePublish` a `AfterPublish` cíle provést cíl před nebo po cíl publikování.</span><span class="sxs-lookup"><span data-stu-id="46eb1-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="46eb1-279">Do profilu publikování k protokolování zpráv konzoly před i po publikování přidejte následující prvky:</span><span class="sxs-lookup"><span data-stu-id="46eb1-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="46eb1-280">Publikování na serveru, který používá nedůvěryhodný certifikát</span><span class="sxs-lookup"><span data-stu-id="46eb1-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="46eb1-281">Přidat `<AllowUntrustedCertificate>` vlastnosti s hodnotou `True` k profilu publikování:</span><span class="sxs-lookup"><span data-stu-id="46eb1-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="46eb1-282">Služby modulu Kudu</span><span class="sxs-lookup"><span data-stu-id="46eb1-282">The Kudu service</span></span>

<span data-ttu-id="46eb1-283">Chcete-li zobrazit soubory v nasazení aplikace webové služby Azure App Service, použijte [Kudu služby](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="46eb1-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="46eb1-284">Připojení `scm` tokenu pro název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="46eb1-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="46eb1-285">Příklad:</span><span class="sxs-lookup"><span data-stu-id="46eb1-285">For example:</span></span>

| <span data-ttu-id="46eb1-286">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="46eb1-286">URL</span></span>                                    | <span data-ttu-id="46eb1-287">Výsledek</span><span class="sxs-lookup"><span data-stu-id="46eb1-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="46eb1-288">Webové aplikace</span><span class="sxs-lookup"><span data-stu-id="46eb1-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="46eb1-289">Služby modulu kudu</span><span class="sxs-lookup"><span data-stu-id="46eb1-289">Kudu service</span></span> |

<span data-ttu-id="46eb1-290">Vyberte [ladění konzoly](https://github.com/projectkudu/kudu/wiki/Kudu-console) položky nabídky, které chcete zobrazit, upravit, odstranit nebo přidat soubory.</span><span class="sxs-lookup"><span data-stu-id="46eb1-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="46eb1-291">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="46eb1-291">Additional resources</span></span>

* <span data-ttu-id="46eb1-292">[Nasazení webové](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) zjednodušuje nasazení webové aplikace a weby pro servery služby IIS.</span><span class="sxs-lookup"><span data-stu-id="46eb1-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="46eb1-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Soubor problémy a žádosti o funkce pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="46eb1-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
