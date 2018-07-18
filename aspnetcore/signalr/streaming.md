---
title: Použití datových proudů v knihovně SignalR technologie ASP.NET Core
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 0001eed830249ac46ba35331759187bb4e7e8fd3
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095259"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Použití datových proudů v knihovně SignalR technologie ASP.NET Core

Podle [Brennan Conroy](https://github.com/BrennanConroy)

Funkce SignalR technologie ASP.NET Core podporuje streamování návratové hodnoty metod serveru. To je užitečné pro scénáře, kde budou přicházet fragmenty dat průběhu času. Pokud vrácená hodnota se streamuje klientovi, každý fragment je odeslat klientovi, jakmile bude k dispozici, nikoli čeká všechna data k dispozici.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Nastavení centra

Metodu rozbočovače na automaticky stane streamování metody rozbočovače, je po návratu `ChannelReader<T>` nebo `Task<ChannelReader<T>>`. Níže je příklad, který ukazuje základy streamovaných dat na klientovi. Vždy, když je objekt zapsána do `ChannelReader` tento objekt se okamžitě se odešlou do klienta. Na konci `ChannelReader` je dokončit, aby se dali pokyn klientovi, datový proud je uzavřen.

> [!NOTE]
> Zápis do `ChannelReader` na vlákně na pozadí a vraťte se `ChannelReader` co nejdříve. Další volání rozbočovače se zablokuje, dokud `ChannelReader` je vrácena.

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>.NET client

`StreamAsChannelAsync` Metodu na `HubConnection` se používá k volání metody streaming. Předat název metody rozbočovače a argumenty, které jsou definovány v metody rozbočovače na `StreamAsChannelAsync`. Obecný parametr na `StreamAsChannelAsync<T>` Určuje typ objektu vrácený metodou streamování. A `ChannelReader<T>` vrácená z volání služby stream a představuje datový proud na straně klienta. Čtení dat, běžně používá k vytvoření smyčky přes `WaitToReadAsync` a volat `TryRead` kdy data jsou k dispozici. Smyčky se ukončí, pokud datový proud bylo ukončeno serverem nebo předat token zrušení `StreamAsChannelAsync` se zruší.

```csharp
var channel = await hubConnection.StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

## <a name="javascript-client"></a>Javascriptový klient

Klientů JavaScript pomocí volání metody streaming v centrech `connection.stream`. `stream` Metoda přijímá dva argumenty:

* Název metody rozbočovače. V následujícím příkladu je název metody rozbočovače `Counter`.
* Argumenty podle metody rozbočovače. V následujícím příkladu jsou argumenty: počet pro počet položek datového proudu pro příjem a zpoždění mezi položkami datového proudu.

`connection.stream` Vrátí `IStreamResult` obsahující `subscribe` metody. Předejte `IStreamSubscriber` k `subscribe` a nastavit `next`, `error`, a `complete` zpětná volání, které chcete dostávat oznámení `stream` vyvolání.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Do konce datového proudu z volání klienta `dispose` metodu na `ISubscription` , který je vrácen z `subscribe` metody.

## <a name="related-resources"></a>Související prostředky

* [Centra](xref:signalr/hubs)
* [Klient .NET](xref:signalr/dotnet-client)
* [Klient JavaScriptu](xref:signalr/javascript-client)
* [Publikování do Azure](xref:signalr/publish-to-azure-web-app)
