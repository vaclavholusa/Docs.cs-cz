---
title: "Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac"
author: rick-anderson
description: "Vytvoření webové rozhraní API s jádro ASP.NET MVC a Visual Studio pro Mac"
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: c7825530146e5e4a879bf44db5a92bc7700de73b
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Vytvoření webové rozhraní API s jádro ASP.NET MVC a Visual Studio pro Mac

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)

V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu. Rozhraní není vytvořená.

Existují 3 verze tohoto kurzu:

* systému macOS: webové rozhraní API pomocí sady Visual Studio pro Mac (Tento kurz)
* Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)
* systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* V tématu [Úvod do architektury ASP.NET MVC jádra na Mac nebo Linux](xref:tutorials/first-mvc-app-xplat/index) pro příklad, který používá trvalé databáze.

## <a name="prerequisites"></a>Požadavky

Nainstalujte následující:

- [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější
- [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a>Vytvoření projektu

Ze sady Visual Studio, vyberte **soubor > Nový řešení**.

![systému macOS nové řešení](first-web-api-mac/_static/sln.png)

Vyberte **aplikace .NET Core > ASP.NET Core webového rozhraní API > Další**.

![systému macOS dialogové okno Nový projekt](first-web-api-mac/_static/1.png)

Zadejte **TodoApi** pro **název projektu**a vyberte možnost vytvořit.

![Dialogové okno Konfigurace](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Spusťte aplikaci

V sadě Visual Studio, vyberte **spustit > spustit s ladění** spusťte aplikaci. Visual Studio spustí prohlížeč a přejde na `http://localhost:5000`. Zobrazí chyba HTTP 404 (není nalezena).  Změňte adresu URL na `http://localhost:port/api/values`. `ValuesController` Data se zobrazí:

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Přidání podpory pro Entity Framework Core

Nainstalujte [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) zprostředkovatel databáze. Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.

* Z **projektu** nabídce vyberte možnost **přidání balíčků NuGet**. 

  *  Alternativně můžete můžete kliknout pravým tlačítkem **závislosti**a potom vyberte **přidat balíčky**.

* Zadejte `EntityFrameworkCore.InMemory` do vyhledávacího pole.
* Vyberte `Microsoft.EntityFrameworkCore.InMemory`a potom vyberte **přidat balíček**.

### <a name="add-a-model-class"></a>Přidejte třídu modelu

Model je objekt, který představuje data ve vaší aplikaci. V takovém případě je pouze model položku seznamu úkolů.

Přidat složku s názvem *modely*. V Průzkumníku řešení klikněte pravým tlačítkem na projekt. Vyberte **přidat** > **novou složku**. Název složky *modely*.

![Nová složka](first-web-api-mac/_static/folder.png)

Poznámka: Můžete umístit třídy modelu kdekoli v projektu, ale *modely* složky se používá podle konvence.

Přidat `TodoItem` třídy. Klikněte pravým tlačítkem myši *modely* složky a vyberte **Přidat > Nový soubor > Obecné > prázdné třídy**. Název třídy `TodoItem`a potom vyberte **nový**.

Nahraďte generovaný kód:

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Generuje databázi `Id` při `TodoItem` je vytvořena.

### <a name="create-the-database-context"></a>Vytvoření kontextu databáze

*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model. Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.

Přidat `TodoContext` třídy k *modely* složky.

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Přidání kontroleru

V Průzkumníku řešení v *řadiče* složky, přidejte třídu `TodoController`.

Nahradit následující generovaný kód (a přidejte uzavírací závorky):

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Spusťte aplikaci

V sadě Visual Studio, vyberte **spustit > spustit s ladění** spusťte aplikaci. Visual Studio spustí prohlížeč a přejde na `http://localhost:port`, kde *portu* je náhodně vybraný port číslo. Zobrazí chyba HTTP 404 (není nalezena).  Změňte adresu URL na `http://localhost:port/api/values`. `ValuesController` Data se zobrazí:

```
["value1","value2"]
```

Přejděte na `Todo` ovladač na`http://localhost:port/api/todo`:

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implementace dalších operací CRUD

Přidáme `Create`, `Update`, a `Delete` metody pro kontroler. Tyto jsou varianty motiv, tak I budete jenom zobrazit kód a zvýrazněte hlavní rozdíly. Sestavení projektu po přidání nebo změna kódu.

### <a name="create"></a>Create

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Toto je metoda HTTP POST, uvedené [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atribut. [ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atribut informuje MVC k získání hodnoty položky úkolů z textu požadavku HTTP.

`CreatedAtRoute` Metoda vrátí odpověď 201, což je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru. `CreatedAtRoute` také přidá do odpovědi hlavičku umístění. Hlavička umístění Určuje identifikátor URI položky nově vytvořený úkolů. V tématu [10.2.2 201 vytvořit](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Použití Postman k odeslání požadavku na vytvořit

* Spusťte aplikaci (**spustit > spustit s laděním**).
* Spusťte Postman.

![Konzole postman](first-web-api/_static/pmc.png)

* Nastavte jako metodu HTTP `POST`
* Vyberte **textu** přepínač
* Vyberte **nezpracovaná** přepínač
* Nastavte typ do formátu JSON
* V editoru klíč hodnota zadejte položky Todo

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Vyberte **odeslat**

* Vyberte kartu hlavičky v dolním podokně a zkopírujte **umístění** hlavičky:

![Karta hlavičky konzoly Postman](first-web-api/_static/pmget.png)

Hlavička umístění URI můžete použít pro přístup k prostředku, který jste právě vytvořili. Odvolat `GetById` vytvořili `"GetTodo"` s názvem trasy:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a>Aktualizace

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` je podobná `Create`, ale používá HTTP PUT. Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Podle specifikace protokolu HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovanou entitu, nikoli pouze rozdíly. Chcete-li podporovat částečné aktualizace, použijte HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Odstranit

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a>Další kroky

* [Směrování do akce Kontroleru](xref:mvc/controllers/routing)
* Informace o nasazení rozhraní API najdete v tématu [hostitele a nasazení](xref:host-and-deploy/index).
* [Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))
* [Postman](https://www.getpostman.com/)
* [Fiddler](https://www.telerik.com/download/fiddler)
