---
title: "Začínáme s ASP.NET MVC jádra a sady Visual Studio pro Mac"
author: rick-anderson
description: "Začínáme s ASP.NET MVC jádra a sady Visual Studio"
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 05a2323851c58c95667066a74c11f1d015405e6f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Začínáme s ASP.NET MVC jádra a sady Visual Studio pro Mac

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se dozvíte, jaké základní informace o vytváření ASP.NET MVC základní webové aplikace pomocí [Visual Studio pro Mac](https://www.visualstudio.com/vs/visual-studio-mac/). 

[!INCLUDE[consider RP](../../includes/razor.md)]

Existují 3 verze tohoto kurzu:

* systému macOS: [sestavení aplikace ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [sestavení ASP.NET Core aplikace MVC pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* Systému macOS, Linux a Windows: [sestavení aplikace ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Požadavky

Tento kurz vyžaduje [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější.

Nainstalujte následující:

- [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější
- [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

Ze sady Visual Studio, vyberte **soubor > Nový řešení**.

![systému macOS nové řešení](../first-web-api-mac/_static/sln.png)

Vyberte **aplikace .NET Core > ASP.NET Core > Webová aplikace > Další**.

![systému macOS dialogové okno Nový projekt](start-mvc/1.png)

Název projektu **MvcMovie**a potom vyberte **vytvořit**.

![systému macOS dialogové okno Nový projekt](start-mvc/2.png)

### <a name="launch-the-app"></a>Spusťte aplikaci

V sadě Visual Studio, vyberte **spustit > Spustit bez ladění** spusťte aplikaci. Visual Studio spustí [Kestrel](xref:fundamentals/servers/index#kestrel), spustí se prohlížeč a přejde na `http://localhost:port`, kde *portu* je náhodně vybraný port číslo.

![Prohlížeč s nový projekt](start-mvc/b1.png)

* Zobrazí panel Adresa `localhost:port#` a není něco jako `example.com`. Je to způsobeno `localhost` je standardní název hostitele místního počítače. Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server. Při spuštění aplikace se zobrazí jiné číslo portu.
* Můžete spustit aplikaci v ladění nebo režim bez ladění z **spustit** nabídky.

Výchozí šablony vám **domácí o** a **kontaktujte** odkazy. Na obrázku prohlížeče výše nezobrazí tyto odkazy. V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.

![Prohlížeč s nový projekt](start-mvc/b2.png)

V další části tohoto kurzu Další informace o MVC a zahájit zápis nějaký kód.

>[!div class="step-by-step"]
[Next](adding-controller.md)  
