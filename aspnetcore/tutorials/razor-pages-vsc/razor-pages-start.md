---
title: Začínáme s ASP.NET Core Razor Pages ve Visual Studio Code
author: rick-anderson
description: Naučte se základy vytváření webovou aplikaci ASP.NET Core Razor Pages s Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: b1f1dcc1401d707cff79f3269ab70b900e9fc21c
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38123380"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a>Začínáme s ASP.NET Core Razor Pages ve Visual Studio Code

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se naučíte se základy vytváření webové aplikace ASP.NET Core Razor Pages. Doporučujeme je provést [Úvod do stránky Razor](xref:razor-pages/index) před zahájením tohoto kurzu. Stránky Razor je doporučený způsob sestavení uživatelského rozhraní pro webové aplikace v ASP.NET Core.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a>Vytvoření webové aplikace Razor

Z terminálu spusťte následující příkazy:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Předchozí příkazy použití [rozhraní příkazového řádku .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) k vytvoření a spuštění projektu pro stránky Razor. Otevřete prohlížeč na http://localhost:5000 zobrazení aplikace.

![Index nebo Domovská stránka](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Otevřete projekt

Stisknutím kláves Ctrl + C vypnout aplikaci.

Visual Studio Code (VS Code), vyberte **soubor > Otevřít složku**a pak vyberte *RazorPagesMovie* složky.

- Vyberte **Ano** k **upozornit** zpráva "požadované prostředky pro vytvoření a odladění chybí"RazorPagesMovie". Přidáním?"
- Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".

### <a name="launch-the-app"></a>Spuštění aplikace

Stisknutím kláves Ctrl + F5 spusťte aplikaci bez ladění. Alternativně z **ladění** nabídce vyberte možnost **spustit bez ladění**.

V dalším kurzu přidáme modelu do projektu. 

> [!div class="step-by-step"]
> [Další krok: Přidání modelu](xref:tutorials/razor-pages-vsc/model)  
