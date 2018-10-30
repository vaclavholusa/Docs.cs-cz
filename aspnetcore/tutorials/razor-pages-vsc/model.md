---
title: Přidání modelu pro aplikace ASP.NET Core Razor Pages s Visual Studio Code
author: rick-anderson
description: Zjistěte, jak přidat modelu do mobilní aplikace Razor Pages v ASP.NET Core s využitím Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244720"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>Přidání modelu pro aplikace ASP.NET Core Razor Pages s Visual Studio Code

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Přidání datového modelu

* Přidat složku s názvem *modely*.
* Přidat třídu *modely* složku s názvem *Movie.cs*.
* Přidejte následující kód, který *Models/Movie.cs* souboru:

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a>Entity Framework Core NuGet balíček pro SQLite

Z příkazového řádku spusťte následující příkaz rozhraní příkazového řádku .NET Core:

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a>Zaregistrujte kontext databáze

Zaregistrovat kontext databáze s [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru *Startup.cs* souboru.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

Přidejte následující `using` příkazů v horní části *Startup.cs*:

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Sestavte projekt a ověřte, že nemáte žádné chyby.

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a>Vygenerované uživatelské rozhraní Video modelu

* Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).
* **Pro Windows**: spusťte následující příkaz:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **Pro macOS a Linux**: spusťte následující příkaz:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

V dalším kurzu vysvětluje souborů vytvořených databázovým generování uživatelského rozhraní.

> [!div class="step-by-step"]
> [Předchozí: Začínáme](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Další: generované uživatelské rozhraní pro stránky Razor](xref:tutorials/razor-pages-vsc/page)
