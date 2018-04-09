---
title: Začínáme s ASP.NET Core Razor stránky v kódu Visual Studio
author: rick-anderson
description: Přečtěte si základní informace o vytváření webové aplikace ASP.NET Core Razor stránky s kódem jazyka Visual Studio.
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 11164fda108e7cc87791e777a66e58f33fe558ae
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a>Začínáme s ASP.NET Core Razor stránky v kódu Visual Studio

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky. Doporučujeme, abyste dokončení [Úvod do stránky Razor](xref:mvc/razor-pages/index) před zahájením tohoto kurzu. Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a>Vytvoření webové aplikace Razor

Z terminálu spusťte následující příkazy:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Předchozí příkazy použijte [.NET Core rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/dotnet) k vytvoření a spuštění projektu stránky Razor. Otevřete prohlížeč na http://localhost:5000 zobrazíte aplikace.

![Index nebo Domovská stránka](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Otevřete projekt

Stiskněte kombinaci kláves Ctrl + C vypnutí aplikace.

Visual Studio Code (kód VS), vyberte **soubor > Otevřít složku**a pak vyberte *RazorPagesMovie* složky.

- Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'RazorPagesMovie'. Přidat jejich?"
- Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".

### <a name="launch-the-app"></a>Spusťte aplikaci

Stisknutím kombinace kláves Ctrl + F5 a spusťte aplikaci bez ladění. Můžete také z **ladění** nabídce vyberte možnost **spustit bez ladění**.

V dalším kurzu přidáme modelu do projektu. 

> [!div class="step-by-step"]
> [Další krok: Přidání modelu](xref:tutorials/razor-pages-vsc/model)  
