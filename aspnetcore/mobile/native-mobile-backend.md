---
title: "Vytváření služeb back-end pro nativní mobilní aplikace s ASP.NET Core"
author: ardalis
description: "Naučte se vytvářet back-end služby pomocí ASP.NET MVC jádra pro podporu nativních mobilní aplikace."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mobile/native-mobile-backend
ms.openlocfilehash: 3902cf728ab6ba776674382361ebb1b28e765711
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="creating-backend-services-for-native-mobile-applications-with-aspnet-core"></a>Vytváření služeb back-end pro nativní mobilní aplikace s ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

Mobilní aplikace můžete snadno komunikovat se službami ASP.NET Core back-end.

[Zobrazit nebo stáhnout ukázkový kód služby back-end](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>Ukázka nativní mobilní aplikace

Tento kurz ukazuje, jak vytvořit back-end služby pomocí ASP.NET MVC jádra pro podporu nativních mobilní aplikace. Použije [aplikace Xamarin Forms ToDoRest](/xamarin/xamarin-forms/data-cloud/consuming/rest) jako nativní klient, který zahrnuje samostatné nativní klientů pro zařízení Android, iOS, univerzální pro Windows a Windows Phone. Vám může postupovat v kurzu propojené vytvoření nativní aplikaci (a instalace nástroje Xamarin nezbytné volné), stejně jako stažení ukázkové řešení Xamarin. Ukázka Xamarin zahrnuje služby projektu ASP.NET Web API 2, které tento článek aplikace ASP.NET Core nahradí (bez nutnosti klientem změn).

![Pro aplikaci Rest se systémem Android smartphone](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>Funkce

Aplikace ToDoRest podporuje výpis, přidávání, odstraňování a aktualizace položkami seznamu úkolů. Každá položka má ID, název, poznámky a vlastnost, která určuje, zda byla dokončí ještě.

Hlavní zobrazení položek, jako v příkladu nahoře, uvádí název každé položky a označuje, pokud se provádí se zaškrtnutím.

Klepnutím `+` ikona se otevře dialogové okno Přidat položky:

![Přidat položku – dialogové okno](native-mobile-backend/_static/todo-android-new-item.png)

Klepnutím na obrazovce hlavní seznamu položku otevře upravit dialogové okno, kde můžete upravit název, poznámky a Done nastavení položky, položka lze nebo:

![Položka dialogové okno Upravit](native-mobile-backend/_static/todo-android-edit-item.png)

Tato ukázka je nakonfigurované ve výchozím nastavení použít back-end službu hostovanou na developer.xamarin.com, umožňujících operacím jen pro čtení. K testování si sami proti aplikace ASP.NET Core vytvořená v další části běžícího na vašem počítači, budete muset aktualizovat aplikace `RestUrl` konstantní. Přejděte na `ToDoREST` projektu a otevřete *Constants.cs* souboru. Nahraďte `RestUrl` s adresou URL, která obsahuje váš počítač IP adres (ne localhost nebo 127.0.0.1, protože tato adresa se používá z zařízení emulátoru, není z vašeho počítače). Zahrnout také číslo portu (5000). Chcete-li otestovat, že vaše služby fungovat s zařízení, ujistěte se, že nemáte aktivní brána firewall blokuje přístup na tento port.

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>Vytvoření projektu ASP.NET Core

Vytvořte novou webovou aplikaci ASP.NET Core v sadě Visual Studio. Zvolte šablonu webového rozhraní API a bez ověřování. Název projektu *ToDoApi*.

![Dialogové okno nové webové aplikace ASP.NET s vybraná šablona projektu webového rozhraní API](native-mobile-backend/_static/web-api-template.png)

Aplikace má odpovědět na všechny požadavky na port 5000. Aktualizace *Program.cs* zahrnout `.UseUrls("http://*:5000")` k dosažení tohoto cíle:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> Zajistěte, aby že aplikaci spouštíte přímo, nikoli za služby IIS Express, které nejsou místní požadavky ve výchozím nastavení ignoruje. Spustit [dotnet spustit](/dotnet/core/tools/dotnet-run) z příkazového řádku, nebo vyberte název profilu aplikace z rozevíracího seznamu cíl ladění na panelu nástrojů Visual Studio.

Přidejte třídu modelu představují položkami seznamu úkolů. Označit požadovaná pole pomocí `[Required]` atribut:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

Metody rozhraní API vyžadují některé způsob, jak pracovat s daty. Použijte stejný `IToDoRepository` rozhraní původní Ukázka používá Xamarin:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

Tato ukázka implementace právě používá privátní kolekce položek:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

Konfigurace v implementaci v *Startup.cs*:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

V tomto okamžiku jste připraveni vytvořit *ToDoItemsController*.

> [!TIP]
> Další informace o vytváření webovým rozhraním API v [vytváření vaše první rozhraní Web API s ASP.NET MVC jádra a sady Visual Studio](../tutorials/first-web-api.md).

## <a name="creating-the-controller"></a>Vytvoření Kontroleru

Přidejte nový řadič do projektu, *ToDoItemsController*. Z Microsoft.AspNetCore.Mvc.Controller musí dědit. Přidat `Route` atribut k označení, že řadičem bude zpracovávat požadavky na cesty počínaje `api/todoitems`. `[controller]` Token v trasy, která je nahrazena název kontroleru (vynechání `Controller` příponu) a je obzvláště užitečné pro globální trasy. Další informace o [směrování](../fundamentals/routing.md).

Vyžaduje kontroleru `IToDoRepository` do funkce; požadavku instance tohoto typu pomocí konstruktoru kontroleru. Za běhu, tato instance bude poskytnuta pomocí rozhraní framework podporu pro [vkládání závislostí](../fundamentals/dependency-injection.md).

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

Toto rozhraní API podporuje čtyři jiné příkazy HTTP k provádění operací CRUD (vytvořit, číst, Update, Delete) na datovém zdroji. Nejjednodušší z nich je operace čtení, který odpovídá na požadavek HTTP GET.

### <a name="reading-items"></a>Čtení položek

Požadavku na seznam položek provádí pomocí požadavek GET na `List` metoda. `[HttpGet]` Atributu u `List` metoda určuje, že tato akce pouze zpracování požadavků GET. Trasy pro tuto akci je trasy zadaný na řadiči. Nepotřebujete nutně použití název akce v rámci trasy. Potřebujete zajistěte, aby měla každá akce jedinečný a jednoznačné trasy. Atributy směrování lze použít v kontroleru a úrovně metody vybudovat konkrétní trasy.

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

`List` Metoda vrátí kód odpovědi 200 OK a všechny položky ToDo serializovanou jako JSON.

Můžete otestovat způsob nové rozhraní API pomocí různých nástrojů, jako třeba [Postman](https://www.getpostman.com/docs/), znázorněno zde:

![Postman konzola znázorňující požadavek GET pro todoitems a text odpovědi zobrazující JSON pro vráceny tři položky.](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>Vytváření položek

Podle konvence vytváření nových položek dat je namapována na příkazu HTTP POST. `Create` Metoda má `[HttpPost]` atribut použije na ni a přijímá `ToDoItem` instance. Vzhledem k tomu, `item` argument budou předány v textu v příspěvku, tento parametr je upraven pomocí `[FromBody]` atribut.

Uvnitř metody položka je kontrola platnosti a předchozí existence v úložišti dat a pokud dojde k žádné problémy, se přidá pomocí úložiště. Kontrola `ModelState.IsValid` provede [modelu ověření](../mvc/models/validation.md)a by mělo být provedeno v každé metody rozhraní API, která podporuje vstup uživatele.

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

Ukázka používá výčet obsahující kódy chyb, které se předávají do mobilního klienta:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

Otestovat, přidávání nových položek pomocí Postman tak, že zvolíte příkaz POST poskytuje nový objekt ve formátu JSON v textu požadavku. Měli byste také přidat zadání hlavičky požadavku `Content-Type` z `application/json`.

![Postman konzola znázorňující POST a odpovědi](native-mobile-backend/_static/postman-post.png)

Metoda vrátí nově vytvořenou položku v odpovědi.

### <a name="updating-items"></a>Aktualizace položek

Úpravy záznamů se provádí pomocí požadavků HTTP PUT. Než tuto změnu `Edit` metoda je téměř shodné s `Create`. Všimněte si, že pokud se nenajde záznamu, `Edit` akce vrátí `NotFound` odpovědi (404).

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

Testování s Postman, změňte příkaz na PUT. Určete data, aktualizovaného objektu v textu požadavku.

![Postman konzola znázorňující PUT a odpovědi](native-mobile-backend/_static/postman-put.png)

Tato metoda vrátí hodnotu `NoContent` (204) odpovědi, při úspěšné, z důvodu konzistence s existující rozhraní API.

### <a name="deleting-items"></a>Odstraňování položek

Odstraňování záznamů dosahuje tím, že žádosti o odstranění ke službě a předávání ID položky pro odstranění. Jako s aktualizacemi, se zobrazí požadavky pro položky, které neexistují `NotFound` odpovědi. Jinak bude úspěšné žádosti získat `NoContent` (204) odpovědi.

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

Při testování funkci odstranění, nic je vyžadována v textu požadavku.

![Postman konzola znázorňující DELETE a odpovědi](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>Běžné konvence webového rozhraní API

Když budete vyvíjet služby back-end pro vaši aplikaci, můžete spolu s konzistentní sadu názvů nebo zásady pro nakládání s mezi vyjímání otázky. Například ve výše uvedeném službě požadavky pro konkrétní záznamů, které nebyly nalezeny přijatých `NotFound` odpověď, a ne `BadRequest` odpovědi. Podobně příkazy provedené v této služby, která předaná vázána k modelu typy vždy zaškrtnuto `ModelState.IsValid` a vrátí `BadRequest` pro typy neplatný model.

Jakmile jste zformulovali běžné zásady pro vaše rozhraní API, můžete obvykle zapouzdřit v [filtru](../mvc/controllers/filters.md). Další informace o [jak zapouzdřit běžné zásady rozhraní API do aplikací ASP.NET MVC základní](https://msdn.microsoft.com/magazine/mt767699.aspx).
