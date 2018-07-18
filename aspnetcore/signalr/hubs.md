---
title: Použití rozbočovače signalr technologie ASP.NET Core
author: tdykstra
description: Další informace o použití rozbočovače signalr technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: be39666373e2b099054bb71f4a7fcf17aeb9a01c
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095278"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Použití rozbočovače signalr pro ASP.NET Core

Podle [Rachel Appel](https://twitter.com/rachelappel) a [Kevin Griffin](https://twitter.com/1kevgriff)

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(jak stáhnout)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Co je rozbočovače SignalR

Rozhraní API pro rozbočovače SignalR můžete volat metody pro připojené klienty ze serveru. V serverovém kódu definovat metody, které jsou volány klientem. V kódu klienta můžete definovat metody, které jsou volány ze serveru. Funkce SignalR se postará o všechno, co na pozadí, které umožňuje komunikaci klienta se serverem a server klient v reálném čase.

## <a name="configure-signalr-hubs"></a>Konfigurace rozbočovače SignalR

SignalR middleware vyžaduje některé služby, které jsou nakonfigurované pomocí volání `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Při přidávání funkce SignalR pro aplikace ASP.NET Core, nastavit trasy SignalR voláním `app.UseSignalR` v `Startup.Configure` metody.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Vytvoření a použití rozbočovače

Vytvoření centra deklarováním třídy, která dědí z `Hub`a přidejte do ní veřejné metody. Klienti mohou volat metody, které jsou definovány jako `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Můžete určit návratový typ a parametry, včetně komplexní typy a pole, stejně jako v jakékoli metodě jazyka C#. Funkce SignalR zpracovává serializace a deserializace komplexních objektů a polí v parametry a návratové hodnoty.

## <a name="the-clients-object"></a>Objekt klientů

Každá instance `Hub` třída nemá vlastnost s názvem `Clients` , který obsahuje následující členy pro komunikaci mezi serverem a klientem:

| Vlastnost | Popis |
| ------ | ----------- |
| `All` | Volá metodu na všechny připojené klienty |
| `Caller` | Volá metodu na straně klienta, který volal metodu rozbočovače na |
| `Others` | Volá metodu na všechny připojené klienty kromě klienta, který volal metodu |


Kromě toho `Hub.Clients` obsahuje následující dvě metody:

| Metoda | Popis |
| ------ | ----------- |
| `AllExcept` | Volá metodu na všechny připojené klienty s výjimkou zadaných připojení |
| `Client` | Volá metodu pro konkrétní připojení klienta |
| `Clients` | Volá metodu pro konkrétní připojených klientů |
| `Group` | Volá metodu pro všechna připojení v určené skupině  |
| `GroupExcept` | Volá metodu pro všechna připojení v určené skupině s výjimkou zadaných připojení |
| `Groups` | Volá metodu k několika skupinám připojení  |
| `OthersInGroup` | Volá metodu pro připojení s výjimkou klienta, který volal metodu rozbočovače na skupinu  |
| `User` | Volá metodu pro všechna připojení, které jsou spojené s konkrétním uživatelem |
| `Users` | Volá metodu pro všechna připojení přidružená k určení uživatelé |

Každé vlastnosti nebo metody v předchozích tabulkách vrátí objekt s `SendAsync` metoda. `SendAsync` Metoda vám umožňuje zadat název a parametry metody volání.

## <a name="send-messages-to-clients"></a>Odesílání zpráv do klientů

Chcete-li provést volání do konkrétních klientů, použijte vlastnosti `Clients` objektu. V následujícím příkladu `SendMessageToCaller` metoda ukazuje odesílání zprávy připojení, které volal metodu rozbočovače. `SendMessageToGroups` Metoda odešle zprávu do skupin uložených v `List` s názvem `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Zpracování událostí pro připojení

Poskytuje rozhraní API pro rozbočovače SignalR `OnConnectedAsync` a `OnDisconnectedAsync` virtuální metody pro správu a sledování připojení. Přepsat `OnConnectedAsync` virtuální metody pro provádění akcí po připojení klienta k rozbočovači, jako je například přidávání do skupiny.

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Zpracování chyb

Výjimky vzniklé v metodách vašeho centra se posílají klientovi, který volal metodu. Na klientovi JavaScript `invoke` metoda vrátí hodnotu [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Když klient obdrží chybu s obslužnou rutinou k němu připojit pomocí promise `catch`, ji má a předají jako JavaScript `Error` objektu.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>Související prostředky

* [Úvod do ASP.NET Core SignalR](xref:signalr/introduction)
* [Klient JavaScriptu](xref:signalr/javascript-client)
* [Publikování do Azure](xref:signalr/publish-to-azure-web-app)
