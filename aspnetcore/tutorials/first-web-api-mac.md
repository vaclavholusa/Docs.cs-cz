---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac
author: rick-anderson
description: Vytvoření webového rozhraní API s ASP.NET Core MVC a sady Visual Studio pro Mac
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 4caa6d9057de8d0e821c4abefe22985f43ff95ad
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38156137"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Mike Wasson](https://github.com/mikewasson)

V tomto kurzu se vytvoření webového rozhraní API pro správu seznam "úkolů". Není vytvořen uživatelského rozhraní.

Existují tři verze tohoto kurzu:

* macOS: webové rozhraní API pomocí sady Visual Studio pro Mac (Tento kurz)
* Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)
* macOS, Linux, Windows: [webového rozhraní API pomocí Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

Zobrazit [Úvod do ASP.NET Core MVC v systému macOS nebo Linux](xref:tutorials/first-mvc-app-xplat/index) příklad, který používá trvalé databáze.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>Vytvoření projektu

Ze sady Visual Studio, vyberte **souboru** > **nové řešení**.

![macOS nové řešení](first-web-api-mac/_static/sln.png)

Vyberte **aplikace .NET Core** > **webového rozhraní API ASP.NET Core** > **Další**.

![macOS dialogové okno nového projektu](first-web-api-mac/_static/1.png)

Zadejte *TodoApi* pro **název projektu**a potom klikněte na tlačítko **vytvořit**.

![Dialogové okno Konfigurace](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Spuštění aplikace

V sadě Visual Studio, vyberte **spustit** > **spustit s ladění** aplikaci spustit. Visual Studio spustí prohlížeč a přejde na `http://localhost:5000`. Obdržíte chybu HTTP 404 (Nenalezeno). Změňte adresu URL na `http://localhost:<port>/api/values`. `ValuesController` Data se zobrazí:

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Přidání podpory pro Entity Framework Core

Nainstalujte [Entity Framework Core InMemory](/ef/core/providers/in-memory/) poskytovatele databáze. Tento poskytovatel databáze umožňuje Entity Framework Core, který se má použít s databází v paměti.

* Z **projektu** nabídce vyberte možnost **přidat balíčky NuGet**.

  * Alternativně je můžete kliknout pravým tlačítkem **závislosti**a pak vyberte **přidat balíčky**.

* Zadejte `EntityFrameworkCore.InMemory` do vyhledávacího pole.
* Vyberte `Microsoft.EntityFrameworkCore.InMemory`a pak vyberte **přidat balíček**.

### <a name="add-a-model-class"></a>Přidejte třídu modelu

Model je objekt, který reprezentuje data ve vaší aplikaci. V tomto případě jediný model je položky úkolu.

V Průzkumníku řešení klikněte pravým tlačítkem na projekt. Vyberte **přidat** > **novou složku**. Název složky *modely*.

![Nová složka](first-web-api-mac/_static/folder.png)

> [!NOTE]
> Třídy modelu můžete umístit kdekoli ve vašem projektu, ale *modely* složky používají konvence.

Klikněte pravým tlačítkem myši *modely* a pak zvolte položku **přidat** > **nový soubor** > **Obecné**  >  **Prázdná třída**. Název třídy *TodoItem*a potom klikněte na tlačítko **nový**.

Nahraďte kód vygenerovaný pomocí:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Vytvoří databázi `Id` při `TodoItem` se vytvoří.

### <a name="create-the-database-context"></a>Vytvořte kontext databáze

*Kontext databáze* je hlavní třída, která koordinuje funkce Entity Framework pro daný datový model. Vytvoření této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.

Přidat `TodoContext` třídu *modely* složky.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Přidání kontroleru

V Průzkumníku řešení v *řadiče* složky, přidejte třídu `TodoController`.

Generovaného kódu nahraďte následujícím kódem:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Spuštění aplikace

V sadě Visual Studio, vyberte **spustit** > **spustit s ladění** aplikaci spustit. Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>`, kde `<port>` je číslo portu náhodně vybrané. Obdržíte chybu HTTP 404 (Nenalezeno). Změňte adresu URL na `http://localhost:<port>/api/values`. `ValuesController` Data se zobrazí:

```json
["value1","value2"]
```

Přejděte `Todo` ovladač na `http://localhost:<port>/api/todo`. Vrátí následující JSON:

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Provádění jiných operací CRUD

Přidáme `Create`, `Update`, a `Delete` metody pro kontroler. Tyto metody jsou varianty motiv, tak I budete jenom zobrazit kód a zvýraznit hlavní rozdíly. Po přidání nebo změna kódu sestavte projekt.

### <a name="create"></a>Create

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Předchozí metoda odpovídá HTTP POST, je určeno [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut. [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atribut oznamuje MVC k získání hodnoty položky seznamu úkolů z textu požadavku HTTP.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Předchozí metoda odpovídá HTTP POST, je určeno [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut. MVC získá hodnotu položky úkolů z textu požadavku HTTP.
::: moniker-end

`CreatedAtRoute` Metoda vrátí odezvě 201. Je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru. `CreatedAtRoute` také přidá do odpovědi hlavičku umístění. Hlavička umístění Určuje identifikátor URI nově vytvořeného úkolu položky. Zobrazit [10.2.2 201 vytvořili](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Odeslat požadavek na vytvoření pomocí nástroje Postman

* Spusťte aplikaci (**spustit** > **spustit s laděním**).
* Otevřete nástroj Postman.

![Konzola nástroje postman](first-web-api/_static/pmc.png)

* Aktualizujte číslo portu adresy URL místního hostitele.
* Nastavte jako metodu HTTP *příspěvek*.
* Klikněte na tlačítko **tělo** kartu.
* Vyberte **nezpracovaná** přepínač.
* Nastavte typ *JSON (application/json)*.
* Zadejte text požadavku s položku úkolu připomínající následující JSON:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Klikněte na tlačítko **odeslat** tlačítko.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Pokud žádná odpověď zobrazí po kliknutí na tlačítko **odeslat**, zakažte **ověření certifikace SSL** možnost. Se nachází v části **souboru** > **nastavení**. Klikněte na tlačítko **odeslat** tlačítko znovu po zakázání nastavení.
::: moniker-end

Klikněte na tlačítko **záhlaví** kartu **odpovědi** podokně a zkopírujte **umístění** hodnota hlavičky:

![Karta hlavičky z konzoly nástroje Postman](first-web-api/_static/pmc2.png)

Hlavička umístění URI můžete použít pro přístup k prostředku, který jste vytvořili. `Create` Vrátí metoda [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_). První parametr předána `CreatedAtRoute` představuje pojmenovanou trasu sloužící ke generování adresy URL. Vzpomeňte si, že `GetById` metoda vytvořené `"GetTodo"` trasy s názvem:

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

`Update` je podobný `Create`, ale používá HTTP PUT. Odpověď je [204 (žádný obsah)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Podle specifikace HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovaná entita, nikoli pouze rozdíly. Pro podporu částečné aktualizace, pomocí HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman konzola znázorňující 204 (žádný obsah) odpovědi](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Odstranit

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Odpověď je [204 (žádný obsah)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Postman konzola znázorňující 204 (žádný obsah) odpovědi](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
