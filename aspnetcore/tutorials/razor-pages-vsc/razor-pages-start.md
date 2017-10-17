---
title: "Začínáme s stránky Razor v ASP.NET Core s kódem jazyka Visual Studio"
author: rick-anderson
description: "Začínáme s stránky Razor v ASP.NET Core pomocí Visual Studio Code"
keywords: "Jádro ASP.NET Razor stránky generování uživatelského rozhraní, Entity Framework Core, EF, EF Core, databáze, mac, systému macOS, Visual Studio Code, kód"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 1b9dff14fa98314601fa44aa229aef6b73bb79d0
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/19/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a>Začínáme s stránky Razor v ASP.NET Core s kódem jazyka Visual Studio

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky. Doporučujeme, abyste dokončení [Úvod do stránky Razor](xref:mvc/razor-pages/index) před zahájením tohoto kurzu. Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.

## <a name="prerequisites"></a>Požadavky

Nainstalujte následující:

* [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější
* [Visual Studio Code](https://code.visualstudio.com)
* VS Code [rozšíření C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-razor-web-app"></a>Vytvoření webové aplikace Razor

Z terminálu spusťte následující příkazy:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Předchozí příkazy použijte [.NET Core rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/dotnet) k vytvoření a spuštění projektu stránky Razor. Otevřete v prohlížeči na http://localhost: 5000 zobrazení aplikace.

![Index nebo Domovská stránka](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Otevřete projekt

Stiskněte kombinaci kláves Ctrl + C vypnutí aplikace.

Visual Studio Code (kód VS), vyberte **soubor > Otevřít složku**a pak vyberte *RazorPagesMovie* složky.

- Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'RazorPagesMovie'. Přidat jejich?"
- Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".

### <a name="launch-the-app"></a>Spusťte aplikaci

Stisknutím kombinace kláves Ctrl + F5 a spusťte aplikaci bez ladění. Můžete také z **ladění** nabídce vyberte možnost **spustit bez ladění**.

V dalším kurzu přidáme modelu do projektu. 

>[!div class="step-by-step"]
[Další krok: Přidání modelu](xref:tutorials/razor-pages-vsc/model)  
