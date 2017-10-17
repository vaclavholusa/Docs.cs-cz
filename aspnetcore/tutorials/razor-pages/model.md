---
title: "Přidání modelu do aplikace stránky Razor ve ASP.NET Core"
author: rick-anderson
description: "Přidání modelu do aplikace stránky Razor ve ASP.NET Core"
keywords: "ASP.NET Core, stránky Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: 195128f670223f15e1679c51fb6354c1aa527c01
ms.sourcegitcommit: 3ba32b2b6425ed94604cb0f681db0d5bb5f8ad58
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a>Přidání modelu do aplikace pro stránky Razor

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Přidat datový model

V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** Projekt > **přidat** > **novou složku**. Název složky *modely*.

Klikněte pravým tlačítkem *modely* složky. Vyberte **přidat** > **třída**. Název třídy **film** a přidejte následující vlastnosti:

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Přidat připojovací řetězec databáze

Přidat připojovací řetězec k *appSettings.JSON určený* souboru.

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Zaregistrovat kontext databáze

Zaregistrovat kontext databáze s [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru *Startup.cs* souboru.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]

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
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

`Install-Package` Příkaz nainstaluje nástroje potřebné ke spuštění modulu generování uživatelského rozhraní.

`Add-Migration` Příkaz generuje kód pro vytvoření schématu počáteční databáze. Schéma je založena na zadaný ve model `DbContext` (v *Models/MovieContext.cs* souboru). `Initial` Argument se používá k pojmenování byla migrace. Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci. V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.

`Update-Database` Příkaz spustí `Up` metoda v *migrace nebo\<časové razítko > _InitialCreate.cs* souboru, který vytvoří databázi.

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.

>[!div class="step-by-step"]
[Předchozí: Začínáme](xref:tutorials/razor-pages/razor-pages-start)
[Další: vygeneroval stránky Razor](xref:tutorials/razor-pages/page)    
