---
title: Začínáme s stránky Razor v ASP.NET Core v systému macOS pomocí sady Visual Studio pro Mac
author: rick-anderson
description: Zjistit, jak začít pracovat s stránky Razor v ASP.NET Core pomocí sady Visual Studio for Mac.
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 6d9233c952c632022c0b37ef1ea6a5320e4eeb6d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a>Začínáme s stránky Razor v ASP.NET Core v systému macOS pomocí sady Visual Studio pro Mac

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky. Doporučujeme vám přečíst si téma [Úvod do stránky Razor](xref:mvc/razor-pages/index) před zahájením tohoto kurzu. Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

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

Ze sady Visual Studio, vyberte **soubor > Otevřít**a pak vyberte *RazorPagesMovie.csproj* souboru.

### <a name="launch-the-app"></a>Spusťte aplikaci

V sadě Visual Studio, vyberte **spustit > Spustit bez ladění** spusťte aplikaci. Visual Studio spustí [Kestrel](xref:fundamentals/servers/kestrel), spustí se prohlížeč a přejde na `http://localhost:5000`.

V dalším kurzu přidáme modelu do projektu.

> [!div class="step-by-step"]
> [Další krok: Přidání modelu](xref:tutorials/razor-pages-mac/model)
