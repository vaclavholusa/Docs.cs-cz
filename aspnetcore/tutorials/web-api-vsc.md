---
title: "Vytvoření webové rozhraní API pomocí ASP.NET Core a VS Code"
description: "Sestavení webového rozhraní API v systému macOS, Linux nebo Windows s ASP.NET MVC jádra a Visual Studio Code"
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, webové rozhraní API, REST, Mac, Linux, HTTP, služby, služba HTTP, VS Code"
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: caf40ee1c2d45d2fbf33b07d707fa4f1be98d31c
ms.sourcegitcommit: 8b5733f1cd5d2c2b6d432bf82fcd4be2d2d6b2a3
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a>Vytvoření webového rozhraní API s ASP.NET MVC jádra a Visual Studio Code v systému Windows, Linux a systému macOS

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)

V tomto kurzu vytvoříte webového rozhraní API pro správu "úkolů" položek seznamu. Nebude sestavení uživatelského rozhraní.

Existují 3 verze tohoto kurzu:

* systému macOS, Linux, Windows: webového rozhraní API s Visual Studio Code (Tento kurz)
* systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)
* Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a>Nastavení vývojového prostředí

Stáhněte a nainstalujte:
- [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější.
- [Visual Studio Code](https://code.visualstudio.com)
- Visual Studio Code [rozšíření C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

## <a name="create-the-project"></a>Vytvoření projektu

Z konzoly spusťte následující příkazy:

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

Otevřete *TodoApi* složky v kódu pro Visual Studio (VS kódu) a vyberte *Startup.cs* souboru.

- Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'TodoApi'. Přidat jejich?"
- Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

!['TodoApi' chybí VS Code s varování požadované prostředky pro sestavení a ladění. Je přidat? Nezobrazovat dotaz dalších, teď ne Ano a také informace o – neexistují nevyřešené závislosti - Restore - zavřít](web-api-vsc/_static/vsc_restore.png)

Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu. V prohlížeči přejděte na http://localhost: 5000/api/hodnoty. Zobrazí se:

`["value1","value2"]`

V tématu [Visual Studio Code nápovědy](#visual-studio-code-help) tipy pro používání VS Code.

## <a name="add-support-for-entity-framework-core"></a>Přidání podpory pro Entity Framework Core

Upravit *TodoApi.csproj* k instalaci [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) zprostředkovatel databáze. Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

Spustit `dotnet restore` ke stažení a instalaci zprostředkovatele EF DB InMemory jádra. Můžete spustit `dotnet restore` z terminálu nebo zadejte `⌘⇧P` (macOS) nebo `Ctrl+Shift+P` (Linux) v VS Code a pak zadejte **.NET**. Vyberte **.NET: obnovení balíčků**.

## <a name="add-a-model-class"></a>Přidejte třídu modelu

Model je objekt, který představuje data ve vaší aplikaci. V takovém případě je pouze model položku seznamu úkolů.

Přidat složku s názvem *modely*. Třídy modelu můžete umístit kdekoli v projektu, ale *modely* složky se používá podle konvence.

Přidat `TodoItem` třídy následujícím kódem:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Generuje databázi `Id` při `TodoItem` je vytvořena.

## <a name="create-the-database-context"></a>Vytvoření kontextu databáze

*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model. Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.

Přidat `TodoContext` třídy v *modely* složky:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Přidání kontroleru

V *řadiče* složky, vytvořte třídu s názvem `TodoController`. Přidejte následující kód:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Spusťte aplikaci

V produktu VS Code stisknutím klávesy F5 spusťte aplikaci. Přejděte na http://localhost: 5000/api/todo ( `Todo` řadiče jsme právě vytvořili).

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Visual Studio Code nápovědy

- [Začínáme](https://code.visualstudio.com/docs)
- [Ladění](https://code.visualstudio.com/docs/editor/debugging)
- [Integrované terminálu](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Klávesové zkratky](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Mac klávesové zkratky](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Linux klávesové zkratky](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Klávesové zkratky systému Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]

