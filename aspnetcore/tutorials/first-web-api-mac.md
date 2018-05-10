---
title: Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac
author: rick-anderson
description: Vytvoření webové rozhraní API s jádro ASP.NET MVC a Visual Studio pro Mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 699fbbf54abf1dc5c4156c559761110cdb375558
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)

V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu. Rozhraní není vytvořená.

Existují tři verze tohoto kurzu:

* systému macOS: webové rozhraní API pomocí sady Visual Studio pro Mac (Tento kurz)
* Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)
* systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

V tématu [Úvod do rozhraní ASP.NET MVC jádra v systému macOS nebo Linux](xref:tutorials/first-mvc-app-xplat/index) pro příklad, který používá trvalé databáze.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>Vytvoření projektu

Ze sady Visual Studio, vyberte **soubor** > **nové řešení**.

![systému macOS nové řešení](first-web-api-mac/_static/sln.png)

Vyberte **aplikace .NET Core** > **ASP.NET Core webového rozhraní API** > **Další**.

![systému macOS dialogové okno Nový projekt](first-web-api-mac/_static/1.png)

Zadejte *TodoApi* pro **název projektu**a potom klikněte na **vytvořit**.

![Dialogové okno Konfigurace](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Spusťte aplikaci

V sadě Visual Studio, vyberte **spustit** > **spustit s ladění** spusťte aplikaci. Visual Studio spustí prohlížeč a přejde na `http://localhost:5000`. Zobrazí chyba HTTP 404 (není nalezena). Změňte adresu URL na `http://localhost:<port>/api/values`. `ValuesController` Se zobrazí data:

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Přidání podpory pro Entity Framework Core

Nainstalujte [Entity Framework Core InMemory](/ef/core/providers/in-memory/) zprostředkovatel databáze. Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.

* Z **projektu** nabídce vyberte možnost **přidání balíčků NuGet**.

  * Alternativně můžete můžete kliknout pravým tlačítkem **závislosti**a potom vyberte **přidat balíčky**.

* Zadejte `EntityFrameworkCore.InMemory` do vyhledávacího pole.
* Vyberte `Microsoft.EntityFrameworkCore.InMemory`a potom vyberte **přidat balíček**.

### <a name="add-a-model-class"></a>Přidejte třídu modelu

Model je objekt reprezentující data v aplikaci. V takovém případě je pouze model položku seznamu úkolů.

V Průzkumníku řešení klikněte pravým tlačítkem na projekt. Vyberte **přidat** > **novou složku**. Název složky *modely*.

![Nová složka](first-web-api-mac/_static/folder.png)

> [!NOTE]
> Třídy modelu můžete umístit kdekoli v projektu, ale *modely* složky se používá podle konvence.

Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **nový soubor** > **Obecné**  >  **Prázdná třída**. Název třídy *TodoItem*a potom klikněte na **nový**.

Nahraďte generovaný kód:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Generuje databázi `Id` při `TodoItem` je vytvořena.

### <a name="create-the-database-context"></a>Vytvoření kontextu databáze

*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model. Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.

Přidat `TodoContext` třídy k *modely* složky.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Přidání kontroleru

V Průzkumníku řešení v *řadiče* složky, přidejte třídu `TodoController`.

Generovaného kódu nahraďte následujícím textem:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Spusťte aplikaci

V sadě Visual Studio, vyberte **spustit** > **spustit s ladění** spusťte aplikaci. Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>`, kde `<port>` je náhodně vybraný port číslo. Zobrazí chyba HTTP 404 (není nalezena). Změňte adresu URL na `http://localhost:<port>/api/values`. `ValuesController` Se zobrazí data:

```json
["value1","value2"]
```

Přejděte na `Todo` ovladač na `http://localhost:<port>/api/todo`. Se vrátí následujícím kódu JSON:

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implementace dalších operací CRUD

Přidáme `Create`, `Update`, a `Delete` metody pro kontroler. Tyto metody jsou varianty motiv, tak I budete jenom zobrazit kód a zvýrazněte hlavní rozdíly. Sestavení projektu po přidání nebo změna kódu.

### <a name="create"></a>Create

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Předchozí postup odpoví na HTTP POST, jak [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut. [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atribut informuje MVC k získání hodnoty položky úkolů z textu požadavku HTTP.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Předchozí postup odpoví na HTTP POST, jak [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut. MVC získá hodnotu položky úkolů z textu požadavku HTTP.
::: moniker-end

`CreatedAtRoute` Metoda vrátí 201 odpověď. Je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru. `CreatedAtRoute` také přidá do odpovědi hlavičku umístění. Hlavička umístění Určuje identifikátor URI položky nově vytvořený úkolů. V tématu [10.2.2 201 vytvořit](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Použití Postman k odeslání požadavku na vytvořit

* Spusťte aplikaci (**spustit** > **začínat ladění**).
* Otevřete Postman.

![Konzole postman](first-web-api/_static/pmc.png)

* Aktualizujte číslo portu v adrese URL místního hostitele.
* Nastavte jako metodu HTTP *POST*.
* Klikněte **textu** kartě.
* Vyberte **nezpracovaná** přepínač.
* Nastavte typ na *JSON (application/json)*.
* Zadejte obsah žádosti s úkol podobné následujícím kódu JSON:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Klikněte **odeslat** tlačítko.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Pokud žádná odpověď zobrazí po kliknutí na **odeslat**, zakažte **SSL certifikační ověření** možnost. To je v části **soubor** > **nastavení**. Klikněte **odeslat** tlačítko znovu po zakázání nastavení.
::: moniker-end

Klikněte na tlačítko **hlavičky** ve **odpovědi** podokně a zkopírujte **umístění** hodnota hlavičky:

![Karta hlavičky konzoly Postman](first-web-api/_static/pmc2.png)

Hlavička umístění URI můžete použít pro přístup k prostředku, kterou jste vytvořili. `Create` Metoda vrátí [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_). První parametr předaný `CreatedAtRoute` představuje pojmenovanou trasu sloužící ke generování adresy URL. Odvolat, který `GetById` vytvořili `"GetTodo"` s názvem trasy:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a>Aktualizace

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` je podobná `Create`, ale používá HTTP PUT. Odpověď [204 (ne obsahu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Podle specifikace protokolu HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovanou entitu, nikoli pouze rozdíly. Chcete-li podporovat částečné aktualizace, použijte HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Odstranit

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Odpověď [204 (ne obsahu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
