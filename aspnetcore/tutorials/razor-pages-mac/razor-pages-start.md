---
title: "Začínáme s stránky Razor v ASP.NET Core v systému Mac"
author: rick-anderson
description: "Zjistit, jak začít pracovat s stránky Razor v ASP.NET Core pomocí sady Visual Studio for Mac."
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: e56799d321cc84c60188a72def9448a0b339d568
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a>Začínáme s stránky Razor v ASP.NET Core pomocí sady Visual Studio pro Mac

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky. Doporučujeme vám přečíst si téma [Úvod do stránky Razor](xref:mvc/razor-pages/index) před zahájením tohoto kurzu. Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.

## <a name="prerequisites"></a>Požadavky

Nainstalujte následující:

* [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější
* [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

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

Ze sady Visual Studio, vyberte **soubor > Otevřít**a pak vyberte *RazorPagesMovie.csproj* souboru.

### <a name="launch-the-app"></a>Spusťte aplikaci

V sadě Visual Studio, vyberte **spustit > Spustit bez ladění** spusťte aplikaci. Visual Studio spustí [Kestrel](xref:fundamentals/servers/kestrel), spustí se prohlížeč a přejde na `http://localhost:5000`.

V dalším kurzu přidáme modelu do projektu.

>[!div class="step-by-step"]
[Další krok: Přidání modelu](xref:tutorials/razor-pages-mac/model)
