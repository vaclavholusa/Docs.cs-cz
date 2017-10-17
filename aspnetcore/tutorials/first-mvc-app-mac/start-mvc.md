---
title: "Začínáme s ASP.NET MVC jádra a sady Visual Studio pro Mac"
author: rick-anderson
description: "Začínáme s ASP.NET MVC jádra a sady Visual Studio"
keywords: "Jádro ASP.NET, MVC, Visual Studio pro Mac, rozhraní Entity Framework"
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 028d6d3246908a9cd44a6834449d2fdbc9cae0b8
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Začínáme s ASP.NET MVC jádra a sady Visual Studio pro Mac

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se dozvíte, jaké základní informace o vytváření ASP.NET MVC základní webové aplikace pomocí [Visual Studio pro Mac](https://www.visualstudio.com/vs/visual-studio-mac/). [!INCLUDE[consider RP](../../includes/razor.md)]

Existují 3 verze tohoto kurzu:

* systému macOS: [sestavení aplikace ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [sestavení ASP.NET Core aplikace MVC pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* Systému macOS, Linux a Windows: [sestavení aplikace ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Požadavky

Tento kurz vyžaduje [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější. V tématu [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) pro verzi ASP.NET Core 1.1.

Nainstalujte následující:

- [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější
- [Visual Studio pro Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

Ze sady Visual Studio, vyberte **soubor > Nový řešení**.

![systému macOS nové řešení](../first-web-api-mac/_static/sln.png)

Vyberte **aplikace .NET Core > ASP.NET Core > Webová aplikace > Další**.

![systému macOS dialogové okno Nový projekt](start-mvc/1.png)

Název projektu **MvcMovie**a potom vyberte **vytvořit**.

![systému macOS dialogové okno Nový projekt](start-mvc/2.png)

### <a name="launch-the-app"></a>Spusťte aplikaci

V sadě Visual Studio, vyberte **spustit > Spustit bez ladění** spusťte aplikaci. Visual Studio spustí [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), spustí se prohlížeč a přejde na `http://localhost:port`, kde *portu* je náhodně vybraný port číslo.

![Prohlížeč s nový projekt](start-mvc/b1.png)

* Zobrazí panel Adresa `localhost:port#` a není něco jako `example.com`. Je to způsobeno `localhost` je standardní název hostitele místního počítače. Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server. Při spuštění aplikace se zobrazí jiné číslo portu.
* Můžete spustit aplikaci v ladění nebo režim bez ladění z **spustit** nabídky.

Výchozí šablony vám **domácí o** a **kontaktujte** odkazy. Na obrázku prohlížeče výše nezobrazí tyto odkazy. V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.

![Prohlížeč s nový projekt](start-mvc/b2.png)

V další části tohoto kurzu Další informace o MVC a zahájit zápis nějaký kód.

>[!div class="step-by-step"]
[Další](adding-controller.md)  
