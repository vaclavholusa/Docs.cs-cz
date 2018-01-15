---
title: "Začínáme s ASP.NET MVC jádra a sady Visual Studio"
author: rick-anderson
description: "Začínáme s ASP.NET MVC jádra a sady Visual Studio"
keywords: "Jádro ASP.NET, MVC"
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 72852678a296107e6a2a146ed2324335a6e870ee
ms.sourcegitcommit: 77b8025c30ec2fd46d85ee2a2b497c44435d3009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a>Začínáme s ASP.NET MVC jádra a sady Visual Studio

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[consider RP](../../includes/razor.md)]

Existují 3 verze tohoto kurzu:

* systému macOS: [vytvořit aplikaci ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [vytvořit základní ASP.NET MVC aplikace pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* systému macOS, Linux a Windows: [vytvořit aplikaci ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="install-visual-studio-and-net-core"></a>Instalace sady Visual Studio a .NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Nainstalujte Visual Studio Community 2017. Vyberte stahování komunity. Tento krok přeskočte, pokud máte Visual Studio 2017 nainstalována.

* [Instalační program Visual Studio 2017 domovské stránky](https://www.visualstudio.com/)

Spusťte instalační program a vyberte následující úlohy:

* **Vývoj pro ASP.NET a webové** (v části **Web a Cloud**)
* **Vývoj pro různé platformy .NET core** (v části **ostatní modulové**)

![**ASP.NET a webové vývoj ** (v části ** Web a Cloud **)](start-mvc/_static/web_workload.png)

![* *.NET základní cross-cross-platfrom vývoj ** (v části ** ostatní modulové **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

Ze sady Visual Studio, vyberte **soubor > Nový > projekt**.

![Soubor > Nový > Projekt](start-mvc/_static/alt_new_project.png)

Dokončení **nový projekt** dialogové okno:

* V levém podokně, klepněte na **.NET Core**
* V prostředním podokně, klepněte na **webové aplikace ASP.NET Core (.NET Core)**
* Název projektu "MvcMovie" (je třeba název projektu "MvcMovie", při kopírování kód se bude shodovat s oboru názvů.)
* Klepněte na **OK**

![Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:

* V poli verze selektor rozevíracího seznamu vyberte **ASP.NET Core 2.-**
* Vyberte **webové Application(Model-View-Controller)**
* Klepněte na **OK**.

![Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:

* V klepnutí rozevíracího seznamu pro výběr verze **ASP.NET Core 1.1**
* Klepněte na **webové aplikace**
* Ponechte výchozí **bez ověřování**
* Klepněte na **OK**.

![Nové webové aplikace ASP.NET Core](start-mvc/_static/p3.png)

---

Visual Studio použít výchozí šablonu pro projekt MVC, který jste právě vytvořili. Zadáním název projektu a výběrem možnosti několik Teď máte aplikaci práci. Toto je jednoduchá starter projektu a je dobrým místem, kde začít,

Klepněte na **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** v režimu bez ladění.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![spuštění aplikace](start-mvc/_static/1.png)

* Visual Studio spustí [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace. Všimněte si, že se zobrazí na panelu Adresa `localhost:port#` a není něco jako `example.com`. Je to způsobeno `localhost` je standardní název hostitele místního počítače. Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server. Na obrázku výše je číslo portu 5 000. Adresu URL v prohlížeči zobrazí `localhost:5000`. Při spuštění aplikace se zobrazí jiné číslo portu.
* Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu. Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.
* Můžete spustit aplikaci v ladění nebo režim bez ladění z **ladění** položky nabídky:

![Ladění nabídky](start-mvc/_static/debug_menu.png)

* Aplikace můžete ladit, klepnutím **IIS Express** tlačítko

![Služby IIS Express](start-mvc/_static/iis_express.png)

Výchozí šablony vám práce **domácí o** a **kontaktujte** odkazy. Na obrázku prohlížeče výše nezobrazí tyto odkazy. V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.

![v pravé horní části ikonu Navigace](start-mvc/_static/2.png)

Pokud byla spuštěna v režimu ladění, klepněte na **Shift + F5** Zastavit ladění.

V další části tohoto kurzu jsme vám další informace o MVC a zahájit zápis nějaký kód.

>[!div class="step-by-step"]
[Next](adding-controller.md)  
