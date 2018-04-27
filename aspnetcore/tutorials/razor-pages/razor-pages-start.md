---
title: Začínáme s stránky Razor v ASP.NET Core
author: rick-anderson
description: Zjistit základní informace o vytváření webové aplikace ASP.NET Core Razor stránky. Doporučuje se stránky Razor pro webové úlohy v ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: cf9877a4fd3ef0e24348aa68a0a2bd401203c2e5
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>Začínáme s stránky Razor v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky. Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.

Existují tři verze tohoto kurzu:

* Windows: V tomto kurzu
* Systému MacOS: [začít pracovat s stránky Razor pomocí sady Visual Studio pro Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* systému macOS, Linux a Windows: [Začínáme s ASP.NET Core Razor stránky v kódu Visual Studio](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Vytvoření webové aplikace Razor

* Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.
* Vytvořte novou webovou aplikaci ASP.NET Core. Název projektu **RazorPagesMovie**. Je třeba název projektu *RazorPagesMovie* , obory názvů bude případy, kdy je zkopírujte a vložte kód.
  ![nové webové aplikace ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)
* Vyberte **technologii ASP.NET 2.0 základní** v rozevírací nabídce a potom vyberte **webové aplikace**.

  [!INCLUDE [install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

Šablony sady Visual Studio vytvoří projekt starter:

![Průzkumník řešení](razor-pages-start/_static/se.png)

Stiskněte klávesu **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** běžet bez připojení ladicího programu

![Index nebo Domovská stránka](razor-pages-start/_static/home.png)

* Visual Studio spustí [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace. Zobrazí panel Adresa `localhost:port#` a není něco jako `example.com`. Je to způsobeno `localhost` je standardní název hostitele místního počítače. Localhost slouží pouze webových požadavků z místního počítače. Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server. Na předchozím obrázku číslo portu je 5 000. Při spuštění aplikace se zobrazí jiné číslo portu.
* Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu. Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

> [!div class="step-by-step"]
> [Další krok: Přidání modelu](xref:tutorials/razor-pages/model)
