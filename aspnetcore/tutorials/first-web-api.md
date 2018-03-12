---
title: "Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows"
author: rick-anderson
description: "Sestavení webového rozhraní API s ASP.NET MVC jádra a Visual Studio pro Windows"
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 6ce18cfd488d89a633bca6c02f9391c1dcca9c08
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)

V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu. Uživatelské rozhraní (UI) se nevytvoří.

Existují 3 verze tohoto kurzu:

* Windows: Webové rozhraní API s Visual Studio pro Windows (Tento kurz)
* systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)
* systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Požadavky

[!INCLUDE[install 2.0](../includes/install2.0.md)]

V tématu [tento PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) pro verzi ASP.NET Core 1.1.

## <a name="create-the-project"></a>Vytvoření projektu

Ze sady Visual Studio, vyberte **soubor** nabídce > **nový** > **projektu**.

Vyberte **.NET Core** >  **webové aplikace ASP.NET Core** šablona projektu. Název projektu `TodoApi` a vyberte **OK**.

![Dialogové okno Nový projekt](first-web-api/_static/new-project.png)

V **nové základní webové aplikace ASP.NET - TodoApi** dialogovém okně, vyberte **rozhraní API** šablony. Vyberte **OK**. Proveďte **není** vyberte **povolení podpory Docker**.

![Dialogové okno nové webové aplikace ASP.NET pomocí šablony projektu webového rozhraní API vybrané ze šablon jádro ASP.NET](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a>Spusťte aplikaci

V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci. Visual Studio spustí prohlížeč a přejde na `http://localhost:port/api/values`, kde *portu* je náhodně vybraný port číslo. Chrome, Microsoft Edge a Firefox zobrazí následující výstup:

```
["value1","value2"]
```

### <a name="add-a-model-class"></a>Přidejte třídu modelu

Model je objekt, který představuje data v aplikaci. V takovém případě je pouze model položku seznamu úkolů.

Přidáte složku s názvem "Modely". V Průzkumníku řešení klikněte pravým tlačítkem na projekt. Vyberte **přidat** > **novou složku**. Název složky *modely*.

Poznámka: Třídy modelu přejděte kdekoli v projektu. *Modely* složky používají konvence pro třídy modelu.

Přidat `TodoItem` třídy. Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**. Název třídy `TodoItem` a vyberte **přidat**.

Aktualizace `TodoItem` třídy následujícím kódem:

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Generuje databázi `Id` při `TodoItem` je vytvořena.

### <a name="create-the-database-context"></a>Vytvoření kontextu databáze

*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model. Tato třída se vytvoří odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.

Přidat `TodoContext` třídy. Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**. Název třídy `TodoContext` a vyberte **přidat**.

Třída nahraďte následujícím kódem:

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Přidání kontroleru

V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složky. Vyberte **přidat** > **novou položku**. V **přidat novou položku** dialogovém okně, vyberte **třída Kontroleru rozhraní Web API** šablony. Název třídy `TodoController`.

![Přidání nové položky dialogové okno s řadiče v vyhledávací pole a webové rozhraní API řadič vybrané](first-web-api/_static/new_controller.png)

Třída nahraďte následujícím kódem:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Spusťte aplikaci

V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci. Visual Studio spustí prohlížeč a přejde na `http://localhost:port/api/values`, kde *portu* je náhodně vybraný port číslo. Přejděte na `Todo` ovladač na `http://localhost:port/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

