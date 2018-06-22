---
title: Začínáme s stránky Razor v ASP.NET Core
author: rick-anderson
description: Zjistit základní informace o vytváření webové aplikace ASP.NET Core Razor stránky. Doporučuje se stránky Razor pro webové úlohy v ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e317b49f2ad33c392de33bc32a87f67bb8cb72a0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278041"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>Začínáme s stránky Razor v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-2.0"

Doporučujeme, abyste že podle verze 2.1 jádro ASP.NET v tomto kurzu. Má **mnohem** snáze sledovat a popisuje další funkce. Vyberte **ASP.NET Core 2.1** v modulu pro výběr verze.

![Selektor verze obsahu](razor-pages-start/_static/v21.png)

::: moniker-end

V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky. Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.

Existují tři verze tohoto kurzu:

* Windows: V tomto kurzu
* Systému MacOS: [začít pracovat s stránky Razor pomocí sady Visual Studio pro Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* systému macOS, Linux a Windows: [Začínáme s ASP.NET Core Razor stránky v kódu Visual Studio](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a>Požadavky

[! Zahrnout [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Vytvoření webové aplikace Razor

* Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.
* Vytvořte novou webovou aplikaci ASP.NET Core. Název projektu **RazorPagesMovie**. Je třeba název projektu *RazorPagesMovie* , obory názvů bude případy, kdy je zkopírujte a vložte kód.
 ![nové webové aplikace ASP.NET Core](razor-pages-start/_static/np_2.1.png)
* Vyberte **ASP.NET Core 2.1** v rozevírací nabídce a potom vyberte **webové aplikace**.

 ![nové webové aplikace ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

Šablony sady Visual Studio vytvoří projekt starter:

![Průzkumník řešení](razor-pages-start/_static/se2.1.png)

Stiskněte klávesu **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** běžet bez připojení ladicího programu. Vyberte **přijmout** o souhlas pro sledování. Tato aplikace nesleduje osobní údaje. Kód šablony generuje obsahuje prostředky ke splnění [obecné Data Protection nařízení (GDPR)](xref:security/gdpr).

![Index nebo Domovská stránka](razor-pages-start/_static/homeGDPR.png)

Následující obrázek znázorňuje aplikaci po přijetí sledování:

![Index nebo Domovská stránka](razor-pages-start/_static/home2.1.png)

* Visual Studio spustí [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace. Zobrazí panel Adresa `localhost:port#` a není něco jako `example.com`. Je to způsobeno `localhost` je standardní název hostitele místního počítače. Localhost slouží pouze webových požadavků z místního počítače. Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server. Na předchozím obrázku číslo portu je 5 000. Při spuštění aplikace se zobrazí jiné číslo portu.
* Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu. Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a>Požadavky

[! Zahrnout [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Vytvoření webové aplikace Razor

* Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.
* Vytvořte novou webovou aplikaci ASP.NET Core. Název projektu **RazorPagesMovie**. Je třeba název projektu *RazorPagesMovie* , obory názvů bude případy, kdy je zkopírujte a vložte kód.
  ![nové webové aplikace ASP.NET Core](../../razor-pages/index/_static/np.png)
* Vyberte **technologii ASP.NET 2.0 základní** v rozevírací nabídce a potom vyberte **webové aplikace**.

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

Šablony sady Visual Studio vytvoří projekt starter:

![Průzkumník řešení](razor-pages-start/_static/se.png)

Stiskněte klávesu **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** běžet bez připojení ladicího programu

![Index nebo Domovská stránka](razor-pages-start/_static/home.png)

* Visual Studio spustí [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace. Zobrazí panel Adresa `localhost:port#` a není něco jako `example.com`. Je to způsobeno `localhost` je standardní název hostitele místního počítače. Localhost slouží pouze webových požadavků z místního počítače. Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server. Na předchozím obrázku číslo portu je 5 000. Při spuštění aplikace se zobrazí jiné číslo portu.
* Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu. Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [Další krok: Přidání modelu](xref:tutorials/razor-pages/model)
