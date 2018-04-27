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
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 97bc9f14b8d6da958a7f587e54a37d2d0e0aabd4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a>Přidat model do aplikace ASP.NET Core Razor stránky pomocí sady Visual Studio pro Mac

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Přidat datový model

* V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** projektu a potom vyberte **přidat** > **novou složku**. Název složky *modely*.
* Klikněte pravým tlačítkem myši *modely* složku a potom vyberte **přidat** > **nový soubor**.
* V **nový soubor** dialogové okno:

  * Vyberte **Obecné** v levém podokně.
  * Vyberte **prázdné třídy** v problémové center.
  * Název třídy **film** a vyberte **nový**.

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Klikněte pravým tlačítkem na červenou vlnovkou řádek, například `MovieContext` v řádku `services.AddDbContext<MovieContext>(options =>`. Vyberte **Quick Fix > pomocí RazorPagesMovie.Models;**. Visual studio. přidá na pomocí příkazu.

Sestavte projekt a ověřte, že nemáte žádné chyby.

![Vytvoření stránky](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Entity Framework Core NuGet balíčky pro migrace

Nástroje EF pro rozhraní příkazového řádku (CLI) jsou uvedeny v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Klikněte na [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) odkaz získat číslo verze pro. K instalaci tohoto balíčku, přidejte ho do `DotNetCliToolReference` kolekce *.csproj* souboru. **Poznámka:** je nutné nainstalovat tento balíček úpravou *.csproj* souboru; nelze použít `install-package` příkaz nebo grafického uživatelského rozhraní Správce balíčků.

Chcete-li upravit *.csproj* souboru:

* Vyberte **soubor** > **otevřete**a pak vyberte *.csproj* souboru.
* Vyberte **možnosti**.
* Změna **Otevřít protokolem** k **editoru zdrojového kódu**.

![Upravte soubor csproj](model/csproj.png)

Přidat `Microsoft.EntityFrameworkCore.Tools.DotNet` nástroj odkaz na druhý  **\<ItemGroup >**.:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

V době psaní byly správné číslo verze vidět v následujícím kódu.

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>Do projektu přidejte tyto stránek nebo filmy soubory

* V sadě Visual Studio, klikněte pravým tlačítkem myši *stránky* složky a vyberte **Přidat > Přidat existující složku**.
* Vyberte *filmy* složky.
* V *vyberte soubory, které chcete zahrnout do projektu* dialogovém okně, vyberte **zahrnují všechny**.

Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.

> [!div class="step-by-step"]
> [Předchozí: Začínáme](xref:tutorials/razor-pages-mac/razor-pages-start)
> [Další: vygeneroval stránky Razor](xref:tutorials/razor-pages-mac/page)
