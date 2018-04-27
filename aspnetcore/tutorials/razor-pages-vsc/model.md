---
title: Přidat model do aplikace ASP.NET Core Razor stránky pomocí sady Visual Studio pro Mac
author: rick-anderson
description: Zjistěte, jak přidat model do aplikace pro stránky Razor v ASP.NET Core pomocí sady Visual Studio for Mac.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: b00b9741a3e94a43755b024a7aa42c4eed274ccc
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a>Přidat model do aplikace ASP.NET Core Razor stránky pomocí sady Visual Studio pro Mac

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Přidat datový model

* Přidat složku s názvem *modely*.
* Přidání třídy k *modely* složku s názvem *Movie.cs*.
* Přidejte následující kód, který *Models/Movie.cs* souboru:

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Sestavte projekt a ověřte, že nemáte žádné chyby.

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Entity Framework Core NuGet balíčky pro migrace

Nástroje EF pro rozhraní příkazového řádku (CLI) jsou uvedeny v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). K instalaci tohoto balíčku, přidejte ho do `DotNetCliToolReference` kolekce *.csproj* souboru. **Poznámka:** je nutné nainstalovat tento balíček úpravou *.csproj* souboru; nelze použít `install-package` příkaz nebo grafického uživatelského rozhraní Správce balíčků.

Upravit *RazorPagesMovie.csproj* souboru:

* Vyberte **soubor** > **otevření souboru**a pak vyberte *RazorPagesMovie.csproj* souboru.
* Přidat odkaz na nástroj pro `Microsoft.EntityFrameworkCore.Tools.DotNet` druhým  **\<ItemGroup >**:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Vygenerované uživatelské rozhraní film modelu

* Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).
* Spusťte následující příkaz:

**Poznámka: Spusťte následující příkaz v systému Windows. Systému MacOS a Linux najdete v části další příkaz**

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* V systému MacOS a Linux spusťte následující příkaz:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Pokud dojde k chybě:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Visual Studio ukončete a spusťte příkaz znovu.

[!INCLUDE [model 4](../../includes/RP/model4.md)]

Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.

> [!div class="step-by-step"]
> [Předchozí: Začínáme](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Další: vygeneroval stránky Razor](xref:tutorials/razor-pages-vsc/page)
