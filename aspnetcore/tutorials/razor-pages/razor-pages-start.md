---
title: "Začínáme s stránky Razor v ASP.NET Core"
author: rick-anderson
description: "Začínáme s stránky Razor v ASP.NET Core"
keywords: "ASP.NET Core, stránky Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 12/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 12685d6a391c127d78399408291c26caa0192ee2
ms.sourcegitcommit: 44a62f59d4db39d685c4487a0345a486be18d7c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>Začínáme s stránky Razor v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky. Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.

Existují tři verze tohoto kurzu:

* Windows: V tomto kurzu
* Systému MacOS: [Začínáme s stránky Razor pomocí sady Visual Studio pro Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* systému macOS, Linux a Windows: [Začínáme s stránky Razor v ASP.NET Core s kódem jazyka Visual Studio](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Požadavky

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a>Vytvoření webové aplikace Razor

* Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.
* Vytvořte novou webovou aplikaci ASP.NET Core. Název projektu **RazorPagesMovie**. Je třeba název projektu *RazorPagesMovie* , obory názvů bude případy, kdy je zkopírujte a vložte kód.
  ![nové webové aplikace ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)
* Vyberte **technologii ASP.NET 2.0 základní** v rozevírací nabídce a potom vyberte **webové aplikace**.

> [!NOTE]
> Pokud chcete použít ASP.NET Core na rozhraní .NET Framework, musíte nejdřív vybrat **rozhraní .NET Framework** krajní levé rozevíracího seznamu v dialogovém okně, pak můžete vybrat požadovanou verzi ASP.NET Core.

  ![Webové aplikace (stránky Razor)](razor-pages-start/_static/np2.png)

Šablony sady Visual Studio vytvoří projekt starter:

![Průzkumník řešení](razor-pages-start/_static/se.png)

Stiskněte klávesu **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** běžet bez připojení ladicího programu

![Index nebo Domovská stránka](razor-pages-start/_static/home.png)

* Visual Studio spustí [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace. Zobrazí panel Adresa `localhost:port#` a není něco jako `example.com`. Je to způsobeno `localhost` je standardní název hostitele místního počítače. Localhost slouží pouze webových požadavků z místního počítače. Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server. Na předchozím obrázku číslo portu je 5 000. Při spuštění aplikace se zobrazí jiné číslo portu.
* Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu. Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[Další krok: Přidání modelu](xref:tutorials/razor-pages/model)
