---
title: Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows
author: rick-anderson
description: Sestavení webového rozhraní API s ASP.NET MVC jádra a Visual Studio pro Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 1680d5e0be0f4844c904d923af30634c53bd1b81
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/15/2018
ms.locfileid: "34306670"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)

V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu. Uživatelské rozhraní (UI) se nevytvoří.

Existují tři verze tohoto kurzu:

* Windows: Webové rozhraní API s Visual Studio pro Windows (Tento kurz)
* systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)
* systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Požadavky

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>Vytvoření projektu

Pomocí těchto kroků v sadě Visual Studio:

* Z **soubor** nabídce vyberte možnost **nový** > **projektu**.
* Vyberte **webové aplikace ASP.NET Core** šablony. Název projektu *TodoApi* a klikněte na tlačítko **OK**.
* V **nové základní webové aplikace ASP.NET - TodoApi** dialogovém okně, vyberte verzi ASP.NET Core. Vyberte **rozhraní API** šablonu a klikněte na tlačítko **OK**. Proveďte **není** vyberte **povolení podpory Docker**.

### <a name="launch-the-app"></a>Spusťte aplikaci

V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci. Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>/api/values`, kde `<port>` je náhodně vybraný port číslo. Chrome, Microsoft Edge a Firefox zobrazí následující výstup:

```json
["value1","value2"]
```

Pokud používáte Internet Explorer, budete vyzváni k uložení *values.json* souboru.

### <a name="add-a-model-class"></a>Přidejte třídu modelu

Model je objekt reprezentující data v aplikaci. V takovém případě je pouze model položku seznamu úkolů.

V Průzkumníku řešení klikněte pravým tlačítkem na projekt. Vyberte **přidat** > **novou složku**. Název složky *modely*.

> [!NOTE]
> Třídy modelu můžete přejít kdekoli v projektu. *Modely* složky používají konvence pro třídy modelu.

V Průzkumníku řešení klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**. Název třídy *TodoItem* a klikněte na tlačítko **přidat**.

Aktualizace `TodoItem` třídy následujícím kódem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Generuje databázi `Id` při `TodoItem` je vytvořena.

### <a name="create-the-database-context"></a>Vytvoření kontextu databáze

*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model. Tato třída se vytvoří odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.

V Průzkumníku řešení klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**. Název třídy *TodoContext* a klikněte na tlačítko **přidat**.

Třída nahraďte následujícím kódem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Přidání kontroleru

V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složky. Vyberte **přidat** > **novou položku**. V **přidat novou položku** dialogovém okně, vyberte **třída Kontroleru rozhraní API** šablony. Název třídy *TodoController*a klikněte na tlačítko **přidat**.

![Přidání nové položky dialogové okno s řadiče v vyhledávací pole a webové rozhraní API řadič vybrané](first-web-api/_static/new_controller.png)

Třída nahraďte následujícím kódem:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Spusťte aplikaci

V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci. Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>/api/values`, kde `<port>` je náhodně vybraný port číslo. Přejděte na `Todo` ovladač na `http://localhost:<port>/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
