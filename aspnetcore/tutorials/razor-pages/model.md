---
title: Přidat model do aplikace pro stránky Razor v ASP.NET Core
author: rick-anderson
description: Zjistit, jak přidat třídy pro správu filmy v databázi pomocí Entity Framework Core (EF jader).
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 80b3aae661342ccde257805c370780cd6f5b4aa4
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/20/2018
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Přidat model do aplikace pro stránky Razor v ASP.NET Core

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Přidat datový model

V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**. Název složky *modely*.

Klikněte pravým tlačítkem *modely* složky. Vyberte **přidat** > **třída**. Název třídy **film** a přidejte následující vlastnosti:

[!INCLUDE [model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Přidat připojovací řetězec databáze

Přidat připojovací řetězec k *appSettings.JSON určený* souboru.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Zaregistrovat kontext databáze

Zaregistrovat kontext databáze s [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru [ConfigureServices metodu třída při spuštění](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Sestavte projekt a ověřte, že nemáte žádné chyby.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Přidat vygenerované uživatelské rozhraní nástroje a provedení počáteční migrace

V této části použijte konzoly Správce balíčků (PMC) na:

* Přidejte balíček generování kódu webové sady Visual Studio. Tento balíček je potřeba spustit modul generování uživatelského rozhraní.
* Přidáte počáteční migrace.
* Aktualizace databáze s počáteční migrace.

Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.

  ![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

Pomocí PMC zadejte následující příkazy:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

Alternativně můžete použít následující příkazy .NET Core rozhraní příkazového řádku:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

`Install-Package` Příkaz nainstaluje nástroje potřebné ke spuštění modulu generování uživatelského rozhraní.

`Add-Migration` Příkaz generuje kód pro vytvoření schématu počáteční databáze. Schéma je založena na zadaný ve model `DbContext` (v *Models/MovieContext.cs* souboru). `Initial` Argument se používá k pojmenování byla migrace. Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci. V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.

`Update-Database` Příkaz spustí `Up` metoda v *migrace nebo\<časové razítko > _InitialCreate.cs* souboru, který vytvoří databázi.

[!INCLUDE [model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE [model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a>Testování aplikace

* Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/movies`).
* Testovací **vytvořit** odkaz.

  ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Testovací **upravit**, **podrobnosti**, a **odstranit** odkazy.

Pokud dojde k výjimce SQL, ověřte, jestli mají spuštění migrace a aktualizovat databázi:

Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.

> [!div class="step-by-step"]
> [Předchozí: Začínáme](xref:tutorials/razor-pages/razor-pages-start)
> [Další: vygeneroval stránky Razor](xref:tutorials/razor-pages/page)    
