---
title: Použití rozhraní příkazového řádku (CLI) LibMan s ASP.NET Core
author: scottaddie
description: Další informace o použití rozhraní příkazového řádku (CLI) LibMan v projektu aplikace ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: 5667f79648a60b8fd9496f8041ef08891ab766af
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402082"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a><span data-ttu-id="5c2cf-103">Použití rozhraní příkazového řádku (CLI) LibMan s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c2cf-103">Use the LibMan command-line interface (CLI) with ASP.NET Core</span></span>

<span data-ttu-id="5c2cf-104">Podle [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="5c2cf-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="5c2cf-105">[LibMan](xref:client-side/libman/index) CLI je nástroj napříč platformami, která je podporována everywhere .NET Core je podporována.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c2cf-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5c2cf-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="5c2cf-107">Instalace</span><span class="sxs-lookup"><span data-stu-id="5c2cf-107">Installation</span></span>

<span data-ttu-id="5c2cf-108">Instalace rozhraní příkazového řádku LibMan:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-108">To install the LibMan CLI:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="5c2cf-109">A [globální nástroje .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) se instaluje z [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="5c2cf-110">Instalace rozhraní příkazového řádku LibMan z konkrétního zdroje balíčku NuGet:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="5c2cf-111">V předchozím příkladu je nainstalován nástroj globální .NET Core z místního počítače Windows *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* souboru.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="5c2cf-112">Použití</span><span class="sxs-lookup"><span data-stu-id="5c2cf-112">Usage</span></span>

<span data-ttu-id="5c2cf-113">Po úspěšné instalaci rozhraní příkazového řádku můžete pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="5c2cf-114">Pokud chcete zobrazit nainstalovanou verzi rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="5c2cf-115">Chcete-li zobrazit dostupné příkazy rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="5c2cf-116">Ve výstupu předchozího příkazu se zobrazí výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-116">The preceding command displays output similar to the following:</span></span>

```console
 1.0.163+g45474d37ed

Usage: libman [options] [command]

Options:
  --help|-h  Show help information
  --version  Show version information

Commands:
  cache      List or clean libman cache contents
  clean      Deletes all library files defined in libman.json from the project
  init       Create a new libman.json
  install    Add a library definition to the libman.json file, and download the 
             library to the specified location
  restore    Downloads all files from provider and saves them to specified 
             destination
  uninstall  Deletes all files for the specified library from their specified 
             destination, then removes the specified library definition from 
             libman.json
  update     Updates the specified library

Use "libman [command] --help" for more information about a command.
```

<span data-ttu-id="5c2cf-117">Následující oddíly popisují dostupné příkazy rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="5c2cf-118">Inicializovat LibMan v projektu</span><span class="sxs-lookup"><span data-stu-id="5c2cf-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="5c2cf-119">`libman init` Příkaz vytvoří *libman.json* souboru, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="5c2cf-120">Soubor se vytvoří s výchozí obsah šablony položky.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5c2cf-121">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5c2cf-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="5c2cf-122">Možnosti</span><span class="sxs-lookup"><span data-stu-id="5c2cf-122">Options</span></span>

<span data-ttu-id="5c2cf-123">Jsou k dispozici pro následující možnosti `libman init` příkaz:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="5c2cf-124">Cestu relativní vzhledem k aktuální složky.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-124">A path relative to the current folder.</span></span> <span data-ttu-id="5c2cf-125">Soubory knihovny nainstalují v tomto umístění, pokud žádné `destination` vlastnost je definována pro knihovny *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="5c2cf-126">`<PATH>` Hodnotu zapíšete do `defaultDestination` vlastnost *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="5c2cf-127">Zprostředkovatele, který se použijte, pokud není definován žádný poskytovatel pro danou knihovnu.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="5c2cf-128">`<PROVIDER>` Hodnotu zapíšete do `defaultProvider` vlastnost *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="5c2cf-129">Nahraďte `<PROVIDER>` s jedním z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5c2cf-130">Příklady</span><span class="sxs-lookup"><span data-stu-id="5c2cf-130">Examples</span></span>

<span data-ttu-id="5c2cf-131">Chcete-li vytvořit *libman.json* soubor v projektu aplikace ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="5c2cf-132">Přejděte do kořenového adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-132">Navigate to the project root.</span></span>
* <span data-ttu-id="5c2cf-133">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="5c2cf-134">Zadejte název výchozího poskytovatele nebo stisknutím klávesy `Enter` poskytovatel CDNJS výchozí se použije.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="5c2cf-135">Platné hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![příkaz init libman – výchozího zprostředkovatele](_static/libman-init-provider.png)

<span data-ttu-id="5c2cf-137">A *libman.json* přidá soubor do kořenového adresáře projektu s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="5c2cf-138">Přidejte soubory knihovny</span><span class="sxs-lookup"><span data-stu-id="5c2cf-138">Add library files</span></span>

<span data-ttu-id="5c2cf-139">`libman install` Příkaz stáhne a nainstaluje soubory knihovny do projektu.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="5c2cf-140">A *libman.json* se přidá soubor, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="5c2cf-141">*Libman.json* změně souboru k ukládání konfiguračních souborů knihoven.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5c2cf-142">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5c2cf-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="5c2cf-143">Arguments</span><span class="sxs-lookup"><span data-stu-id="5c2cf-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="5c2cf-144">Název knihovny určené k instalaci.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-144">The name of the library to install.</span></span> <span data-ttu-id="5c2cf-145">Tento název může obsahovat notation číslo verze (například `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="5c2cf-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="5c2cf-146">Možnosti</span><span class="sxs-lookup"><span data-stu-id="5c2cf-146">Options</span></span>

<span data-ttu-id="5c2cf-147">Jsou k dispozici pro následující možnosti `libman install` příkaz:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="5c2cf-148">Umístění pro instalaci knihovny.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-148">The location to install the library.</span></span> <span data-ttu-id="5c2cf-149">Pokud není zadán, použije se výchozí umístění.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-149">If not specified, the default location is used.</span></span> <span data-ttu-id="5c2cf-150">Pokud ne `defaultDestination` je zadána vlastnost v *libman.json*, tato možnost je vyžadována.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="5c2cf-151">Zadejte název souboru k instalaci z knihovny.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="5c2cf-152">Pokud není zadán, nainstalují se všechny soubory z knihovny.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="5c2cf-153">Zadejte jeden `--files` možnost na soubor k instalaci.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="5c2cf-154">Jsou podporovány příliš relativní cesty.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-154">Relative paths are supported too.</span></span> <span data-ttu-id="5c2cf-155">Příklad: `--files dist/browser/signalr.js`.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="5c2cf-156">Název zprostředkovatele, který má být použit pro získání knihovny.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="5c2cf-157">Nahraďte `<PROVIDER>` s jedním z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="5c2cf-158">Pokud není zadán, `defaultProvider` vlastnost *libman.json* se používá.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="5c2cf-159">Pokud ne `defaultProvider` je zadána vlastnost v *libman.json*, tato možnost je vyžadována.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5c2cf-160">Příklady</span><span class="sxs-lookup"><span data-stu-id="5c2cf-160">Examples</span></span>

<span data-ttu-id="5c2cf-161">Vezměte v úvahu následující *libman.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="5c2cf-162">K instalaci verze jQuery 3.2.1 *jquery.min.js* do souboru *wwwroot/skripty/jquery* pomocí zprostředkovatele CDNJS složky:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="5c2cf-163">*Libman.json* soubor má následující podobu:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-163">The *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

<span data-ttu-id="5c2cf-164">Chcete-li nainstalovat *calendar.js* a *calendar.css* souborů z *C:\\temp\\contosoCalendar\\*  pomocí systému souborů poskytovatel:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="5c2cf-165">Se zobrazí následující výzva pro dva důvody:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="5c2cf-166">*Libman.json* soubor neobsahuje `defaultDestination` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="5c2cf-167">`libman install` Neobsahuje příkaz `-d|--destination` možnost.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![libman nainstalovat příkaz – cíl](_static/libman-install-destination.png)

<span data-ttu-id="5c2cf-169">Po přijetí výchozího místa určení, *libman.json* soubor má následující podobu:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot/lib/contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a><span data-ttu-id="5c2cf-170">Obnovit soubory knihoven</span><span class="sxs-lookup"><span data-stu-id="5c2cf-170">Restore library files</span></span>

<span data-ttu-id="5c2cf-171">`libman restore` Nainstaluje soubory knihoven, které jsou definovány v *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="5c2cf-172">Platí následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-172">The following rules apply:</span></span>

* <span data-ttu-id="5c2cf-173">Pokud ne *libman.json* soubor v kořenovém adresáři projektu existuje, vrátí se chyba.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="5c2cf-174">Pokud knihovny určuje zprostředkovatele, `defaultProvider` vlastnost v *libman.json* se ignoruje.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="5c2cf-175">Pokud knihovny určuje cíl, `defaultDestination` vlastnost *libman.json* se ignoruje.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5c2cf-176">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5c2cf-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="5c2cf-177">Možnosti</span><span class="sxs-lookup"><span data-stu-id="5c2cf-177">Options</span></span>

<span data-ttu-id="5c2cf-178">Jsou k dispozici pro následující možnosti `libman restore` příkaz:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5c2cf-179">Příklady</span><span class="sxs-lookup"><span data-stu-id="5c2cf-179">Examples</span></span>

<span data-ttu-id="5c2cf-180">Pokud chcete obnovit soubory knihovny definované v *libman.json*:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="5c2cf-181">Odstranit soubory knihoven</span><span class="sxs-lookup"><span data-stu-id="5c2cf-181">Delete library files</span></span>

<span data-ttu-id="5c2cf-182">`libman clean` Příkaz odstraní soubory knihovny přes LibMan byly obnoveny.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="5c2cf-183">Složky, které se stanou prázdný po provedení této operace jsou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="5c2cf-184">Konfigurace v přidružené soubory knihovny `libraries` vlastnost *libman.json* se neodeberou.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5c2cf-185">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5c2cf-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="5c2cf-186">Možnosti</span><span class="sxs-lookup"><span data-stu-id="5c2cf-186">Options</span></span>

<span data-ttu-id="5c2cf-187">Jsou k dispozici pro následující možnosti `libman clean` příkaz:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5c2cf-188">Příklady</span><span class="sxs-lookup"><span data-stu-id="5c2cf-188">Examples</span></span>

<span data-ttu-id="5c2cf-189">Chcete-li odstranit soubory knihovny k instalaci použili LibMan:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="5c2cf-190">Odinstalace soubory knihoven</span><span class="sxs-lookup"><span data-stu-id="5c2cf-190">Uninstall library files</span></span>

<span data-ttu-id="5c2cf-191">`libman uninstall` Příkaz:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="5c2cf-192">Odstraní všechny soubory přidružené k zadanou knihovnu z cílového umístění v *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="5c2cf-193">Odstraní konfiguraci přidruženou knihovnu z *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="5c2cf-194">Dojde k chybě při:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-194">An error occurs when:</span></span>

* <span data-ttu-id="5c2cf-195">Ne *libman.json* existuje soubor v kořenové složce projektu.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="5c2cf-196">Zadanou knihovnu neexistuje.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="5c2cf-197">Pokud je nainstalovaná více než jedna knihovna se stejným názvem, budete vyzváni, zvolte jeden.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5c2cf-198">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5c2cf-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="5c2cf-199">Arguments</span><span class="sxs-lookup"><span data-stu-id="5c2cf-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="5c2cf-200">Název knihovny určené k odinstalaci.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-200">The name of the library to uninstall.</span></span> <span data-ttu-id="5c2cf-201">Tento název může obsahovat notation číslo verze (například `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="5c2cf-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="5c2cf-202">Možnosti</span><span class="sxs-lookup"><span data-stu-id="5c2cf-202">Options</span></span>

<span data-ttu-id="5c2cf-203">Jsou k dispozici pro následující možnosti `libman uninstall` příkaz:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5c2cf-204">Příklady</span><span class="sxs-lookup"><span data-stu-id="5c2cf-204">Examples</span></span>

<span data-ttu-id="5c2cf-205">Vezměte v úvahu následující *libman.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="5c2cf-206">Pokud chcete odinstalovat jQuery, některý z následujících příkazů úspěšné:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="5c2cf-207">K odinstalaci soubory Lodash instalovaných pomocí instalace `filesystem` zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="5c2cf-208">Aktualizujte verzi knihovny</span><span class="sxs-lookup"><span data-stu-id="5c2cf-208">Update library version</span></span>

<span data-ttu-id="5c2cf-209">`libman update` Příkaz aktualizuje instalovaných pomocí instalace LibMan zadanou verzi knihovny.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="5c2cf-210">Dojde k chybě při:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-210">An error occurs when:</span></span>

* <span data-ttu-id="5c2cf-211">Ne *libman.json* existuje soubor v kořenové složce projektu.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="5c2cf-212">Zadanou knihovnu neexistuje.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="5c2cf-213">Pokud je nainstalovaná více než jedna knihovna se stejným názvem, budete vyzváni, zvolte jeden.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5c2cf-214">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5c2cf-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="5c2cf-215">Arguments</span><span class="sxs-lookup"><span data-stu-id="5c2cf-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="5c2cf-216">Název knihovny určené k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="5c2cf-217">Možnosti</span><span class="sxs-lookup"><span data-stu-id="5c2cf-217">Options</span></span>

<span data-ttu-id="5c2cf-218">Jsou k dispozici pro následující možnosti `libman update` příkaz:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="5c2cf-219">Získáte nejnovější předběžnou verzi knihovny.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="5c2cf-220">Získáte konkrétní verzi knihovny.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5c2cf-221">Příklady</span><span class="sxs-lookup"><span data-stu-id="5c2cf-221">Examples</span></span>

* <span data-ttu-id="5c2cf-222">JQuery aktualizovat na nejnovější verzi:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="5c2cf-223">Chcete aktualizovat jQuery verze 3.3.1:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="5c2cf-224">Chcete-li aktualizovat jQuery nejnovější předběžnou verzi:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="5c2cf-225">Správa mezipaměti knihovny</span><span class="sxs-lookup"><span data-stu-id="5c2cf-225">Manage library cache</span></span>

<span data-ttu-id="5c2cf-226">`libman cache` Příkaz spravuje mezipaměti LibMan knihovny.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="5c2cf-227">`filesystem` Poskytovatele nepoužívá mezipaměť knihovny.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5c2cf-228">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5c2cf-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="5c2cf-229">Arguments</span><span class="sxs-lookup"><span data-stu-id="5c2cf-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="5c2cf-230">Použít pouze s `clean` příkazu.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-230">Only used with the `clean` command.</span></span> <span data-ttu-id="5c2cf-231">Určuje mezipaměť zprostředkovatele k vyčištění.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="5c2cf-232">Platné hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="5c2cf-233">Možnosti</span><span class="sxs-lookup"><span data-stu-id="5c2cf-233">Options</span></span>

<span data-ttu-id="5c2cf-234">Jsou k dispozici pro následující možnosti `libman cache` příkaz:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="5c2cf-235">Seznam názvů souborů, které jsou uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="5c2cf-236">Seznam názvů knihoven, které jsou uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5c2cf-237">Příklady</span><span class="sxs-lookup"><span data-stu-id="5c2cf-237">Examples</span></span>

* <span data-ttu-id="5c2cf-238">Chcete-li zobrazit názvy knihoven v mezipaměti na poskytovatele, použijte jednu z následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="5c2cf-239">Zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-239">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      font-awesome
      jquery
      knockout
      lodash.js
      react
  ```

* <span data-ttu-id="5c2cf-240">Chcete-li zobrazit názvy souborů v mezipaměti knihovny na zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="5c2cf-241">Zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-241">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout:
          <list omitted for brevity>
      react:
          <list omitted for brevity>
      vue:
          <list omitted for brevity>
  cdnjs:
      font-awesome
          metadata.json
      jquery
          metadata.json
          3.2.1\core.js
          3.2.1\jquery.js
          3.2.1\jquery.min.js
          3.2.1\jquery.min.map
          3.2.1\jquery.slim.js
          3.2.1\jquery.slim.min.js
          3.2.1\jquery.slim.min.map
          3.3.1\core.js
          3.3.1\jquery.js
          3.3.1\jquery.min.js
          3.3.1\jquery.min.map
          3.3.1\jquery.slim.js
          3.3.1\jquery.slim.min.js
          3.3.1\jquery.slim.min.map
      knockout
          metadata.json
          3.4.2\knockout-debug.js
          3.4.2\knockout-min.js
      lodash.js
          metadata.json
          4.17.10\lodash.js
          4.17.10\lodash.min.js
      react
          metadata.json
  ```

  <span data-ttu-id="5c2cf-242">Všimněte si, že tento výstup ukazuje, že jQuery verze 3.2.1 a 3.3.1 jsou uložené v mezipaměti v rámci zprostředkovatele CDNJS.</span><span class="sxs-lookup"><span data-stu-id="5c2cf-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="5c2cf-243">Vyprázdnit mezipaměť knihovny pro poskytovatele CDNJS:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="5c2cf-244">Po vyprazdňují se mezipaměť zprostředkovatele CDNJS, `libman cache list` příkazu se zobrazí následující:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      (empty)
  ```

* <span data-ttu-id="5c2cf-245">Vyprázdnění mezipaměti pro všechny podporované zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="5c2cf-246">Po vyprazdňují se všechny mezipaměti zprostředkovatele `libman cache list` příkazu se zobrazí následující:</span><span class="sxs-lookup"><span data-stu-id="5c2cf-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="5c2cf-247">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5c2cf-247">Additional resources</span></span>

* [<span data-ttu-id="5c2cf-248">Nainstalujte nástroj Global</span><span class="sxs-lookup"><span data-stu-id="5c2cf-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="5c2cf-249">Úložiště LibMan GitHub</span><span class="sxs-lookup"><span data-stu-id="5c2cf-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
