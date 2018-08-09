---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio
author: rick-anderson
description: Vytvoření webového rozhraní API pomocí ASP.NET Core MVC a sady Visual Studio ve Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 2694388324cdbd246aad6c88d8439171704dfe89
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722513"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a>Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Mike Wasson](https://github.com/mikewasson)

V tomto kurzu sestavení webového rozhraní API pro správu seznam "úkolů". Uživatelské rozhraní (UI) není vytvořena.

Existují tři verze tohoto kurzu:

* Windows: Webové rozhraní API pomocí sady Visual Studio na Windows (Tento kurz)
* macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)
* macOS, Linux, Windows: [webového rozhraní API pomocí Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Požadavky

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>Vytvoření projektu

Postupujte podle těchto kroků v sadě Visual Studio:

* Z **souboru** nabídce vyberte možnost **nový** > **projektu**.
* Vyberte **webové aplikace ASP.NET Core** šablony. Pojmenujte projekt *TodoApi* a klikněte na tlačítko **OK**.
* V **nové webové aplikace ASP.NET Core – TodoApi** dialogového okna, vyberte verzi technologie ASP.NET Core. Vyberte **API** šablonu a klikněte na tlačítko **OK**. Proveďte **není** vyberte **povolit podporu Dockeru**.

### <a name="launch-the-app"></a>Spuštění aplikace

V sadě Visual Studio stiskněte CTRL + F5 spusťte aplikaci. Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>/api/values`, kde `<port>` je číslo portu náhodně vybrané. Chrome, Microsoft Edge a Firefox se zobrazí následující výstup:

```json
["value1","value2"]
```

Pokud používáte Internet Explorer, budete vyzváni k uložení *values.json* souboru.

### <a name="add-a-model-class"></a>Přidejte třídu modelu

Model je objekt reprezentující data v aplikaci. V tomto případě jediný model je položky úkolu.

V Průzkumníku řešení klikněte pravým tlačítkem na projekt. Vyberte **přidat** > **novou složku**. Název složky *modely*.

> [!NOTE]
> Třídy modelu můžete kamkoliv v projektu. *Modely* složky používají konvence pro třídy modelu.

V Průzkumníku řešení klikněte pravým tlačítkem myši *modely* a pak zvolte položku **přidat** > **třídy**. Název třídy *TodoItem* a klikněte na tlačítko **přidat**.

Aktualizace `TodoItem` třídy následujícím kódem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Vytvoří databázi `Id` při `TodoItem` se vytvoří.

### <a name="create-the-database-context"></a>Vytvořte kontext databáze

*Kontext databáze* je hlavní třída, která koordinuje funkce Entity Framework pro daný datový model. Tato třída se vytvoří odvozením z `Microsoft.EntityFrameworkCore.DbContext` třídy.

V Průzkumníku řešení klikněte pravým tlačítkem myši *modely* a pak zvolte položku **přidat** > **třídy**. Název třídy *TodoContext* a klikněte na tlačítko **přidat**.

Třída nahraďte následujícím kódem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Přidání kontroleru

V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složky. Vyberte **přidat** > **nová položka**. V **přidat novou položku** dialogového okna, vyberte **třída Kontroleru rozhraní API** šablony. Název třídy *TodoController*a klikněte na tlačítko **přidat**.

![Přidání nové položky dialogové okno s kontrolerem v vyhledávací pole a webové rozhraní API kontroleru vybrané](first-web-api/_static/new_controller.png)

Třída nahraďte následujícím kódem:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Spuštění aplikace

V sadě Visual Studio stiskněte CTRL + F5 spusťte aplikaci. Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>/api/values`, kde `<port>` je číslo portu náhodně vybrané. Přejděte `Todo` ovladač na `http://localhost:<port>/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
