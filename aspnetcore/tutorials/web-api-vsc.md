---
title: Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio Code
author: rick-anderson
description: Sestavení webového rozhraní API v systému macOS, Linux nebo Windows s ASP.NET MVC jádra a Visual Studio Code
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 303016c7e1d27726faaa0b1546575255355d5275
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/28/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio Code

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)

V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu. Uživatelského rozhraní není vytvořená.

Existují tři verze tohoto kurzu:

* systému macOS, Linux, Windows: webového rozhraní API s Visual Studio Code (Tento kurz)
* systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)
* Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Požadavky

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a>Vytvoření projektu

Z konzoly spusťte následující příkazy:

```console
dotnet new webapi -o TodoApi
code TodoApi
```

*TodoApi* složky otevře ve Visual Studio Code (kód VS). Vyberte *Startup.cs* souboru.

* Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'TodoApi'. Přidat jejich?"
* Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

!['TodoApi' chybí VS Code s varování požadované prostředky pro sestavení a ladění. Je přidat? Nezobrazovat dotaz dalších, teď ne Ano](web-api-vsc/_static/vsc_restore.png)

Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu. V prohlížeči přejděte na http://localhost:5000/api/values. Zobrazí se následující výstup:

```json
["value1","value2"]
```

V tématu [Visual Studio Code nápovědy](#visual-studio-code-help) tipy pro používání VS Code.

## <a name="add-support-for-entity-framework-core"></a>Přidání podpory pro Entity Framework Core

:::moniker range="<= aspnetcore-2.0"
Vytvoření nového projektu v technologii ASP.NET 2.0 základní přidá [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) odkaz na balíček *TodoApi.csproj* souboru:

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
Vytvoření nového projektu ASP.NET Core 2.1 nebo novější přidá [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) odkaz na balíček *TodoApi.csproj* souboru:

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end

Není nutné k instalaci [Entity Framework Core InMemory](/ef/core/providers/in-memory/) databáze zprostředkovatele samostatně. Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.

## <a name="add-a-model-class"></a>Přidejte třídu modelu

Model je objekt reprezentující data v aplikaci. V takovém případě je pouze model položku seznamu úkolů.

Přidat složku s názvem *modely*. Třídy modelu můžete umístit kdekoli v projektu, ale *modely* složky se používá podle konvence.

Přidat `TodoItem` třídy následujícím kódem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Generuje databázi `Id` při `TodoItem` je vytvořena.

## <a name="create-the-database-context"></a>Vytvoření kontextu databáze

*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model. Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.

Přidat `TodoContext` třídy v *modely* složky:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Přidání kontroleru

V *řadiče* složky, vytvořte třídu s názvem `TodoController`. Nahraďte jeho obsah následujícím kódem:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Spusťte aplikaci

V produktu VS Code stisknutím klávesy F5 spusťte aplikaci. Přejděte na http://localhost:5000/api/todo ( `Todo` řadiče jsme vytvořili).

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Visual Studio Code nápovědy

* [Začínáme](https://code.visualstudio.com/docs)
* [Ladění](https://code.visualstudio.com/docs/editor/debugging)
* [Integrované terminálu](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [Klávesové zkratky](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [systému macOS klávesové zkratky](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Linux klávesové zkratky](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Klávesové zkratky systému Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
