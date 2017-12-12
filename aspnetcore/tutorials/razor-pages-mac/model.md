---
title: "Přidání modelu do aplikace pro stránky Razor pomocí sady Visual Studio pro Mac"
author: rick-anderson
description: "Přidání modelu do aplikace stránky Razor ve ASP.NET Core pomocí sady Visual Studio pro Mac"
keywords: "ASP.NET Core, stránky Razor, Razor, MVC, modelu"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 648ecd3a782fa489b727982ce5f7a2087539bf38
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a>Přidání modelu do aplikace stránky Razor ve ASP.NET Core pomocí sady Visual Studio pro Mac

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Přidat datový model

* V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** projektu a potom vyberte **přidat** > **novou složku**. Název složky *modely*.
* Klikněte pravým tlačítkem myši *modely* složku a potom vyberte **přidat** > **nový soubor**.
* V **nový soubor** dialogové okno:

  * Vyberte **Obecné** v levém podokně.
  * Vyberte **prázdné třídy** v problémové center.
  * Název třídy **film** a vyberte **nový**.

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Klikněte pravým tlačítkem na červenou vlnovkou řádek, například `MovieContext` v řádku `services.AddDbContext<MovieContext>(options =>`. Vyberte **Quick Fix > pomocí RazorPagesMovie.Models;**. Visual studio. přidá na pomocí příkazu.

Sestavte projekt a ověřte, že nemáte žádné chyby.

![Vytvoření stránky](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Entity Framework Core NuGet balíčky pro migrace

Nástroje EF pro rozhraní příkazového řádku (CLI) jsou uvedeny v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). K instalaci tohoto balíčku, přidejte ho do `DotNetCliToolReference` kolekce *.csproj* souboru. **Poznámka:** je nutné nainstalovat tento balíček úpravou *.csproj* souboru; nelze použít `install-package` příkaz nebo grafického uživatelského rozhraní Správce balíčků.

Chcete-li upravit *.csproj* souboru:

* Vyberte **soubor** > **otevřete**a pak vyberte *.csproj* souboru.
* Vyberte **možnosti**.
* Změna **Otevřít protokolem** k **editoru zdrojového kódu**.

![Upravte soubor csproj](model/csproj.png)

Přidat `Microsoft.EntityFrameworkCore.Tools.DotNet` nástroj odkaz na druhý  **\<ItemGroup >**:

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>Do projektu přidejte tyto stránek nebo filmy soubory

* V sadě Visual Studio, klikněte pravým tlačítkem myši *stránky* složky a vyberte **Přidat > Přidat existující složku**.
* Vyberte *filmy* složky.
* V *Chosse soubory pro zahrnutí do projektu* dialogovém okně, vyberte **zahrnují všechny**.

Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.

>[!div class="step-by-step"]
[Předchozí: Začínáme](xref:tutorials/razor-pages-mac/razor-pages-start)
[Další: vygeneroval stránky Razor](xref:tutorials/razor-pages/page)
