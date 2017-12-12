---
title: "Úvod do základní ASP.NET MVC na Mac, Linux nebo Windows"
author: rick-anderson
description: "Začínáme s ASP.NET MVC jádra a Visual Studio Code na Mac, Linux a Windows"
keywords: "Jádro ASP.NET, MVC, VS kódu, Visual Studio Code, Mac, Linux, Windows"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 87ce5dca011a7bc37d34799db159d933c158cba1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a>Začínáme s ASP.NET MVC jádra na Mac, Linux nebo Windows

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu naučit, základní informace o vytváření ASP.NET MVC základní webové aplikace pomocí [Visual Studio Code](https://code.visualstudio.com) (VS Code). Kurz předpokládá familarity kódem VS. V tématu [Začínáme s VS Code](https://code.visualstudio.com/docs) a [Visual Studio Code nápovědy](#visual-studio-code-help) Další informace. [!INCLUDE[consider RP](../../includes/razor.md)]

Existují 3 verze tohoto kurzu:

* systému macOS: [vytvořit aplikaci ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [vytvořit základní ASP.NET MVC aplikace pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* systému macOS, Linux a Windows: [vytvořit aplikaci ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="install-vs-code-and-net-core"></a>Instalace VS Code a .NET Core

Tento kurz vyžaduje [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější. V tématu [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) pro verzi ASP.NET Core 1.1.

Nainstalujte následující:

* [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější.
* [Visual Studio Code](https://code.visualstudio.com)
* VS Code [rozšíření C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-web-app-with-dotnet"></a>Vytvoření webové aplikace s dotnet.

Z terminálu spusťte následující příkazy:

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

Otevřete *MvcMovie* složky v kódu pro Visual Studio (VS kódu) a vyberte *Startup.cs* souboru.

- Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'MvcMovie'. Přidat jejich?"
- Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".

!['MvcMovie' chybí VS Code s varování požadované prostředky pro sestavení a ladění. Je přidat? Nezobrazovat dotaz dalších, teď ne Ano a také informace o – neexistují nevyřešené závislosti - Restore - zavřít](../web-api-vsc/_static/vsc_restore.png)

Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu.

![spuštění aplikace](../first-mvc-app/start-mvc/_static/1.png)

VS kód spustí [Kestrel](xref:fundamentals/servers/kestrel) webový server a spustí aplikace. Všimněte si, že se zobrazí na panelu Adresa `localhost:5000` a není něco jako `example.com`. Je to způsobeno `localhost` je standardní název hostitele místního počítače.

Výchozí šablony vám práce **domácí o** a **kontaktujte** odkazy. Na obrázku prohlížeče výše nezobrazí tyto odkazy. V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.

![v pravé horní části ikonu Navigace](../first-mvc-app/start-mvc/_static/2.png)

V další části tohoto kurzu jsme vám další informace o MVC a zahájit zápis nějaký kód.

## <a name="visual-studio-code-help"></a>Visual Studio Code nápovědy

- [Začínáme](https://code.visualstudio.com/docs)
- [Ladění](https://code.visualstudio.com/docs/editor/debugging)
- [Integrované terminálu](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Klávesové zkratky](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Mac klávesové zkratky](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Linux klávesové zkratky](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Klávesové zkratky systému Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[Další – Přidání kontroleru](adding-controller.md)
