---
title: Přidání modelu pro aplikace ASP.NET Core Razor Pages pomocí sady Visual Studio pro Mac
author: rick-anderson
description: Zjistěte, jak přidat modelu do mobilní aplikace Razor Pages v ASP.NET Core pomocí sady Visual Studio pro Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 3ca6c9b9988b8335116b7248c6c4a89997d02b14
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186755"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a>Přidání modelu pro aplikace ASP.NET Core Razor Pages pomocí sady Visual Studio pro Mac

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Přidání datového modelu

* V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** projektu a pak vyberte **přidat** > **novou složku**. Název složky *modely*.
* Klikněte pravým tlačítkem myši *modely* složku a pak vyberte **přidat** > **nový soubor**.
* V **nový soubor** dialogové okno:

  * Vyberte **Obecné** v levém podokně.
  * Vyberte **prázdnou třídu** v center usnadnit práci.
  * Název třídy **film** a vyberte **nový**.

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Klikněte pravým tlačítkem myši klikněte na červenou vlnovkou čáru, například `MovieContext` v řádku `services.AddDbContext<MovieContext>(options =>`. Vyberte **Quick Fix > pomocí RazorPagesMovie.Models;**. Visual studio přidá nástroje pomocí příkazu.

Sestavte projekt a ověřte, že nemáte žádné chyby.

![Vytvoření stránky](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Entity Framework Core NuGet balíčky pro migrace

EF nástroje pro rozhraní příkazového řádku (CLI) jsou k dispozici v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Klikněte na [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) odkaz na číslo verze používat. K instalaci tohoto balíčku, přidejte ji tak `DotNetCliToolReference` kolekce v *.csproj* souboru. **Poznámka:** je potřeba nainstalovat tento balíček úpravou *.csproj* soubor; nelze použít `install-package` příkaz nebo grafické uživatelské rozhraní Správce balíčků.

Chcete-li upravit *.csproj* souboru:

* Vyberte **souboru** > **otevřít**a pak vyberte *.csproj* souboru.
* Vyberte **možnosti**.
* Změna **otevřít v programu** k **Editor zdrojového kódu**.

![Upravte soubor csproj](model/csproj.png)

Přidat `Microsoft.EntityFrameworkCore.Tools.DotNet` nástroj odkaz na druhý  **\<ItemGroup >**.:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

V době psaní byly správné číslo verze je znázorněno v následujícím kódu.

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>Přidání stránky/filmy souborů do projektu

* V sadě Visual Studio, klikněte pravým tlačítkem myši *stránky* a pak zvolte položku **Přidat > Přidat existující složku**.
* Vyberte *filmy* složky.
* V *vybrat soubory k zahrnutí do projektu* dialogového okna, vyberte **zahrnout všechny**.

V dalším kurzu vysvětluje souborů vytvořených databázovým generování uživatelského rozhraní.

> [!div class="step-by-step"]
> [Předchozí: Začínáme](xref:tutorials/razor-pages-mac/razor-pages-start)
> [Další: generované uživatelské rozhraní pro stránky Razor](xref:tutorials/razor-pages-mac/page)
