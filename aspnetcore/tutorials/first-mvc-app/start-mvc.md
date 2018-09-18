---
title: Začínáme s ASP.NET Core MVC a sady Visual Studio
author: rick-anderson
description: Zjistěte, jak začít pracovat s ASP.NET Core MVC a sady Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 41f986a06ec46dc025c4e8218745b4a513e8ee2a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011696"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a>Začínáme s ASP.NET Core MVC a sady Visual Studio

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Existují 3 verze tohoto kurzu:

* macOS: [vytvoření aplikace ASP.NET Core MVC se sadou Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [vytvořit v aplikaci MVC ASP.NET Core pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux a Windows: [vytvoření aplikace ASP.NET Core MVC pomocí Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="install-visual-studio-and-net-core"></a>Instalace sady Visual Studio a .NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

Ze sady Visual Studio, vyberte **soubor > Nový > projekt**.

![Soubor > Nový > Projekt](start-mvc/_static/alt_new_project.png)

Dokončení **nový projekt** dialogové okno:

* V levém podokně, klepněte na **.NET Core**
* V prostředním podokně, klepněte na **webová aplikace ASP.NET Core (.NET Core)**
* Pojmenujte projekt "MvcMovie" (je třeba název projektu "MvcMovie", takže při kopírování kódu bude odpovídat oboru názvů.)
* Klepněte na **OK**

![Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core ](start-mvc/_static/new_project2-21.png)

Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:

* V poli verze selektoru rozevíracího seznamu vyberte **ASP.NET Core 2.1**
* Vyberte **webové Application(Model-View-Controller)**
* Klepněte na **OK**.

![Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core ](start-mvc/_static/new_project22-21.png)

Visual Studio používá výchozí šablonu pro projekt MVC, který jste právě vytvořili. Zadáním názvu projektu a výběr několika možností Teď máte funkční aplikaci. Toto je základní počáteční projekt, a je vhodné oddělení na zahájení,

Klepněte na **F5** ke spuštění aplikace v režimu ladění nebo **Ctrl-F5** v režimu bez ladění.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![spuštění aplikace](start-mvc/_static/1.png)

* Visual Studio spustí [služby IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spouští vaše aplikace. Všimněte si, že do adresního řádku ukazuje `localhost:port#` a nemít něco podobného `example.com`. Důvodem je, že `localhost` je standardní název hostitele místního počítače. Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server. Na obrázku výše je číslo portu 5000. Adresa URL v prohlížeči zobrazí `localhost:5000`. Při spuštění aplikace se zobrazí jiné číslo portu.
* Spuštění aplikace s **Ctrl + F5** (bez ladění režim) umožňuje provádět změny kódu, uložte soubor, aktualizujte prohlížeč a zobrazení změn kódu. Mnoho vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a podívejte se na změny.
* Spustíte ji v ladění nebo v režimu bez ladění z **ladění** položky nabídky:

![Ladění nabídky](start-mvc/_static/debug_menu.png)

* Můžete ladit aplikaci klepnutím **služby IIS Express** tlačítko

![Služba IIS Express](start-mvc/_static/iis_express.png)

Výchozí šablony vám práci **domácí o** a **kontakt** odkazy. Výše uvedené prohlížeče obraz se nezobrazí těchto odkazů. V závislosti na velikosti vašeho prohlížeče mohou muset kliknout na ikonu navigace se budou zobrazovat.

![Ikona navigace v vpravo nahoře](start-mvc/_static/2.png)

Pokud jsou spuštěné v režimu ladění, klepněte na **Shift + F5** chcete zastavit ladění.

V další části tohoto kurzu přidáme další informace o MVC a začít psát kód.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Instalace sady Visual Studio Community 2017. Vyberte stáhnout Community. Tento krok přeskočte, pokud máte nainstalovanou sadu Visual Studio 2017.

* [Instalační program sady Visual Studio 2017 domovské stránky](https://www.visualstudio.com/)

Spusťte instalační program a vybrat následující úlohy:

* **Vývoj pro ASP.NET a web** (v části **Web a Cloud**)
* **Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)

![**ASP.NET a webového vývoje ** (v části ** Web a Cloud **)](start-mvc/_static/web_workload.png)

![* *.NET core napříč. mezi platfrom vývoj ** (v části ** Další sady nástrojů **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

Ze sady Visual Studio, vyberte **soubor > Nový > projekt**.

![Soubor > Nový > Projekt](start-mvc/_static/alt_new_project.png)

Dokončení **nový projekt** dialogové okno:

* V levém podokně, klepněte na **.NET Core**
* V prostředním podokně, klepněte na **webová aplikace ASP.NET Core (.NET Core)**
* Pojmenujte projekt "MvcMovie" (je třeba název projektu "MvcMovie", takže při kopírování kódu bude odpovídat oboru názvů.)
* Klepněte na **OK**

![Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:

* V poli verze selektoru rozevíracího seznamu vyberte **ASP.NET Core 2.-**
* Vyberte **webové Application(Model-View-Controller)**
* Klepněte na **OK**.

![Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:

* V tap verze selektoru rozevíracího seznamu **ASP.NET Core 1.1**
* Klepněte na **webové aplikace**
* Ponechte výchozí **bez ověřování**
* Klepněte na **OK**.

![Nová webová aplikace ASP.NET Core](start-mvc/_static/p3.png)

---

Visual Studio používá výchozí šablonu pro projekt MVC, který jste právě vytvořili. Zadáním názvu projektu a výběr několika možností Teď máte funkční aplikaci. Toto je základní počáteční projekt, a je vhodné oddělení na zahájení,

Klepněte na **F5** ke spuštění aplikace v režimu ladění nebo **Ctrl-F5** v režimu bez ladění.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![spuštění aplikace](start-mvc/_static/1.png)

* Visual Studio spustí [služby IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spouští vaše aplikace. Všimněte si, že do adresního řádku ukazuje `localhost:port#` a nemít něco podobného `example.com`. Důvodem je, že `localhost` je standardní název hostitele místního počítače. Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server. Na obrázku výše je číslo portu 5000. Adresa URL v prohlížeči zobrazí `localhost:5000`. Při spuštění aplikace se zobrazí jiné číslo portu.
* Spuštění aplikace s **Ctrl + F5** (bez ladění režim) umožňuje provádět změny kódu, uložte soubor, aktualizujte prohlížeč a zobrazení změn kódu. Mnoho vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a podívejte se na změny.
* Spustíte ji v ladění nebo v režimu bez ladění z **ladění** položky nabídky:

![Ladění nabídky](start-mvc/_static/debug_menu.png)

* Můžete ladit aplikaci klepnutím **služby IIS Express** tlačítko

![Služba IIS Express](start-mvc/_static/iis_express.png)

Výchozí šablony vám práci **domácí o** a **kontakt** odkazy. Výše uvedené prohlížeče obraz se nezobrazí těchto odkazů. V závislosti na velikosti vašeho prohlížeče mohou muset kliknout na ikonu navigace se budou zobrazovat.

![Ikona navigace v vpravo nahoře](start-mvc/_static/2.png)

Pokud jsou spuštěné v režimu ladění, klepněte na **Shift + F5** chcete zastavit ladění.

V další části tohoto kurzu přidáme další informace o MVC a začít psát kód.

::: moniker-end

> [!div class="step-by-step"]
> [Next](adding-controller.md)  
