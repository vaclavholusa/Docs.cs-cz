---
title: Úvod do architektury ASP.NET MVC jádra v systému macOS, Linux nebo Windows
author: rick-anderson
description: Zjistěte, jak začít pracovat s ASP.NET MVC jádra a Visual Studio Code v systému macOS, Linux a Windows
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275272"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a>Úvod do architektury ASP.NET MVC jádra v systému macOS, Linux nebo Windows

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu naučit, základní informace o vytváření ASP.NET MVC základní webové aplikace pomocí [Visual Studio Code](https://code.visualstudio.com) (VS Code). Kurz předpokládá familarity kódem VS. V tématu [Začínáme s VS Code](https://code.visualstudio.com/docs) a [Visual Studio Code nápovědy](#visual-studio-code-help) Další informace. 

[!INCLUDE [consider RP](../../includes/razor.md)]

Existují 3 verze tohoto kurzu:

* systému macOS: [vytvořit aplikaci ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [vytvořit základní ASP.NET MVC aplikace pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* systému macOS, Linux a Windows: [vytvořit aplikaci ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

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

  - [systému macOS klávesové zkratky](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Linux klávesové zkratky](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Klávesové zkratky systému Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [Další – Přidání kontroleru](adding-controller.md)
