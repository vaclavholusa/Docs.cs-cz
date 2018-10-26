---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio Code
author: rick-anderson
description: Vytvoření webového rozhraní API v systému macOS, Linux nebo Windows pomocí ASP.NET Core MVC a Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 740110908358a382f20bc1e54e98056296278acf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089661"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio Code

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Mike Wasson](https://github.com/mikewasson)

V tomto kurzu se vytvoření webového rozhraní API pro správu seznam "úkolů". Není vytvořen uživatelského rozhraní.

Existují tři verze tohoto kurzu:

* macOS, Linux, Windows: webového rozhraní API pomocí Visual Studio Code (Tento kurz)
* macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)
* Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Požadavky

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

Zobrazit [nápovědy Visual Studio Code](#visual-studio-code-help) tipy pro používání VS Code.

## <a name="create-the-project"></a>Vytvoření projektu

Z konzoly spusťte následující příkazy:

```console
dotnet new webapi -o TodoApi
code TodoApi
```

*TodoApi* otevře složka ve Visual Studio Code (VS Code). Vyberte *Startup.cs* souboru.

* Vyberte **Ano** k **upozornit** zpráva "požadované prostředky pro vytvoření a odladění chybí"TodoApi". Přidáním?"
* Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code s upozornit požadované prostředky pro sestavování a ladění 'TodoApi' chybí. Přidat? Neptat se znovu, nyní ne, Ano](web-api-vsc/_static/vsc_restore.png)

Stisknutím klávesy **ladění** (F5) a sestavte a spusťte program. V prohlížeči přejděte na http://localhost:5000/api/values. Zobrazí se následující výstup:

```json
["value1","value2"]
```



## <a name="add-support-for-entity-framework-core"></a>Přidání podpory pro Entity Framework Core

:::moniker range=">= aspnetcore-2.1"

Vytvoření nového projektu ASP.NET Core 2.1 nebo novější přidá [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) do souboru projektu:

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

Vytvoření nového projektu v aplikaci ASP.NET Core 2.0 přidá [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage) do souboru projektu:

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

Není nutné k instalaci [Entity Framework Core InMemory](/ef/core/providers/in-memory/) databázi zprostředkovatele samostatně. Tento poskytovatel databáze umožňuje Entity Framework Core, který se má použít s databází v paměti.

## <a name="add-a-model-class"></a>Přidejte třídu modelu

Model je objekt, který reprezentuje data ve vaší aplikaci. V tomto případě jediný model je položky úkolu.

Přidat složku s názvem *modely*. Třídy modelu můžete umístit kdekoli ve vašem projektu, ale *modely* složky používají konvence.

Přidat `TodoItem` třídy následujícím kódem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Vytvoří databázi `Id` při `TodoItem` se vytvoří.

## <a name="create-the-database-context"></a>Vytvořte kontext databáze

*Kontext databáze* je hlavní třída, která koordinuje funkce Entity Framework pro daný datový model. Vytvoření této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.

Přidat `TodoContext` třídy v *modely* složky:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Přidání kontroleru

V *řadiče* složku, vytvořte třídu s názvem `TodoController`. Nahraďte jeho obsah následujícím kódem:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Spuštění aplikace

V nástroji VS Code stisknutím klávesy F5 spusťte aplikaci. Přejděte do http://localhost:5000/api/todo ( `Todo` řadič jsme vytvořili).

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Visual Studio Code – Nápověda

* [Začínáme](https://code.visualstudio.com/docs)
* [Ladění](https://code.visualstudio.com/docs/editor/debugging)
* [Integrovaný terminál](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [Klávesové zkratky](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [macOS klávesové zkratky](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Linux klávesové zkratky](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Windows klávesové zkratky](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
