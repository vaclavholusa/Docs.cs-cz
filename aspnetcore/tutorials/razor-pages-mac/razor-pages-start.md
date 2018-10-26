---
title: Začínáme se stránkami Razor v ASP.NET Core v systému macOS pomocí sady Visual Studio pro Mac
author: rick-anderson
description: Objevte, jak začít pracovat se stránkami Razor v ASP.NET Core pomocí sady Visual Studio pro Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 8a4cc6c52684558ce9176f98205e439096e88cbf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089648"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a>Začínáme se stránkami Razor v ASP.NET Core v systému macOS pomocí sady Visual Studio pro Mac

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se naučíte se základy vytváření webové aplikace ASP.NET Core Razor Pages. Doporučujeme vám přečíst si téma [Úvod do stránky Razor](xref:razor-pages/index) před zahájením tohoto kurzu. Stránky Razor je doporučený způsob sestavení uživatelského rozhraní pro webové aplikace v ASP.NET Core.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a>Vytvoření webové aplikace Razor

Z terminálu spusťte následující příkazy:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Předchozí příkazy použití [rozhraní příkazového řádku .NET Core](/dotnet/core/tools/dotnet) k vytvoření a spuštění projektu pro stránky Razor. Otevřete prohlížeč na http://localhost:5000 zobrazení aplikace.

![Index nebo Domovská stránka](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Otevřete projekt

Stisknutím kláves Ctrl + C vypnout aplikaci.

Ze sady Visual Studio, vyberte **soubor > Otevřít**a pak vyberte *RazorPagesMovie.csproj* souboru.

### <a name="launch-the-app"></a>Spuštění aplikace

V sadě Visual Studio, vyberte **spuštění > Spustit bez ladění** aplikaci spustit. Visual Studio spustí [Kestrel](xref:fundamentals/servers/kestrel), spustí se prohlížeč a přejde na `http://localhost:5000`.

V dalším kurzu přidáme modelu do projektu.

> [!div class="step-by-step"]
> [Další krok: Přidání modelu](xref:tutorials/razor-pages-mac/model)
