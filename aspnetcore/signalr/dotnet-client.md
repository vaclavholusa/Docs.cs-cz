---
title: Klient .NET funkce SignalR technologie ASP.NET Core
author: rachelappel
description: Informace o .NET klienta SignalR technologie ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 35dc1d3abf0d35e17d1835ec462f8cc4feb728eb
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938251"
---
# <a name="aspnet-core-signalr-net-client"></a>Klient .NET funkce SignalR technologie ASP.NET Core

Podle [Rachel Appel](http://twitter.com/rachelappel)

Klient .NET funkce SignalR technologie ASP.NET Core můžete využívat aplikace pro Xamarin, WPF, Windows Forms, konzoly a .NET Core. Podobně jako [javascriptový klient](xref:signalr/javascript-client), klient .NET umožňuje příjem a odesílání a příjem zpráv do centra v reálném čase.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Vzorový kód v tomto článku je aplikace WPF, která používá klienta .NET funkce SignalR technologie ASP.NET Core.

## <a name="install-the-signalr-net-client-package"></a>Instalace balíčku pro klienta SignalR .NET

`Microsoft.AspNetCore.SignalR.Client` Balíčku je potřeba pro klienty .NET pro připojení k rozbočovačům SignalR. Pokud chcete nainstalovat klientské knihovny, spusťte následující příkaz **Konzola správce balíčků** okno:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Připojení k rozbočovači

K navázání připojení, vytvoření `HubConnectionBuilder` a volat `Build`. Adresa URL rozbočovače, protokol, typ přenosu, úroveň protokolu, záhlaví a další možnosti je možné nakonfigurovat při vytváření připojení. Nakonfigurujte veškeré požadované možnosti vložíte-li některý z `HubConnectionBuilder` metod do `Build`. Zahájit připojení s `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>Volání metod rozbočovače na z klienta

`InvokeAsync` volání metody rozbočovače. Předat název metody rozbočovače a argumentů podle metody rozbočovače na `InvokeAsync`. SignalR je asynchronní, proto `async` a `await` při volání.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>Volání metody klienta od rozbočovače

Definovat metody rozbočovače volá pomocí `connection.On` po sestavení, ale před spuštěním připojení.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

Předchozí kód v `connection.On` spustí, když kód na straně serveru pomocí volání `SendAsync` metoda.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>Protokolování a zpracování chyb

Zpracování chyb pomocí příkazu try-catch. Zkontrolujte `Exception` objektu určit správnou akce má být provedena, když dojde k chybě.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>Další zdroje

* [Centra](xref:signalr/hubs)
* [Klient JavaScriptu](xref:signalr/javascript-client)
* [Publikování do Azure](xref:signalr/publish-to-azure-web-app)
