---
title: Klient .NET ASP.NET Core SignalR
author: rachelappel
description: Informace o klientovi jádro ASP.NET SignalR rozhraní .NET
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: faa4368988971a3e7fcdcd1b044971e16d70f19a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273292"
---
# <a name="aspnet-core-signalr-net-client"></a>Klient .NET ASP.NET Core SignalR

Podle [Rachel Appel](http://twitter.com/rachelappel)

Klient .NET SignalR technologie ASP.NET Core lze pomocí aplikace Xamarin, WPF, Windows Forms, konzoly a .NET Core. Podobně jako [JavaScript klienta](xref:signalr/javascript-client), klient .NET umožňuje přijímat a odesílat a přijímat zprávy k rozbočovači v reálném čase.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Ukázka kódu v tomto článku je WPF aplikaci, která používá klient .NET SignalR technologie ASP.NET Core.

## <a name="install-the-signalr-net-client-package"></a>Nainstalovat balíček klienta SignalR .NET

`Microsoft.AspNetCore.SignalR.Client` Balíčku je potřeba pro klienty .NET pro připojení k rozbočovačům SignalR. Pokud chcete nainstalovat klientské knihovny, spusťte následující příkaz **Konzola správce balíčků** okno:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Připojení k rozbočovači

Má být navázáno připojení, vytvoření `HubConnectionBuilder` a volání `Build`. Adresa URL rozbočovače, protokol, typ přenosu, úroveň protokolu, hlavičky a další možnosti lze nakonfigurovat při vytváření připojení. Nakonfigurujte veškeré požadované možnosti vložením jakýchkoli `HubConnectionBuilder` metody do `Build`. Zahájení připojení s `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>Volání metody rozbočovače z klienta

`InvokeAsync` volání metody rozbočovače. Předat název metody rozbočovače a všechny argumenty definované v metodě rozbočovače k `InvokeAsync`. SignalR je asynchronní, takže použijte `async` a `await` při volání.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>Volání metody klienta od rozbočovače

Definování metody rozbočovače volá pomocí `connection.On` po sestavení, ale před zahájením připojení.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

Předchozí kód v `connection.On` se spustí v případě, že volá kódu na straně serveru pomocí `SendAsync` metoda.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>Protokolování a zpracování chyb

Zpracování chyb s příkazem try-catch. Zkontrolujte `Exception` objektem pro určení správné akci lze provést po dojde k chybě.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>Další zdroje

* [Centra](xref:signalr/hubs)
* [Klient JavaScriptu](xref:signalr/javascript-client)
* [Publikování do Azure](xref:signalr/publish-to-azure-web-app)