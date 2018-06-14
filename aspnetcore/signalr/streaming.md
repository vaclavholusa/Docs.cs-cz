---
title: Použití datových proudů v ASP.NET Core SignalR
author: rachelappel
description: ''
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/streaming
ms.openlocfilehash: e8b32d5f25ae3bf8555fd2375c0fbb99a6f8bd53
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358456"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Použití datových proudů v ASP.NET Core SignalR

Podle [Brennan Conroy](https://github.com/BrennanConroy)

Jádro ASP.NET SignalR podporuje streamování návratové hodnoty metody server. To je užitečné pro scénáře, kde bude pocházet fragmenty dat průběhu času. Když je návratovou hodnotu datového proudu, do klienta, každý fragment je odeslat klientovi, jakmile bude k dispozici, místo čekání všechna data k dispozici.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Nastavit rozbočovače

Metody rozbočovače automaticky stane streamování metody rozbočovače, až se obnoví `ChannelReader<T>` nebo `Task<ChannelReader<T>>`. Níže je ukázka, která ukazuje základy streamování dat klientovi. Vždy, když je objekt zapsán do `ChannelReader` tento objekt je okamžitě se odešlou do klienta. Na konci `ChannelReader` dokončení říct klienta datový proud je uzavřen.

> [!NOTE]
> Zápis do `ChannelReader` na pozadí přístup z více vláken a vraťte se `ChannelReader` co nejdříve. Další volání rozbočovače se zablokuje, dokud `ChannelReader` je vrácen.

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a>Klient .NET

`StreamAsChannelAsync` Metodu `HubConnection` se používá k volání metody streamování. Předat název metody rozbočovače a argumenty, které jsou definované v metodě rozbočovače k `StreamAsChannelAsync`. Obecný parametr na `StreamAsChannelAsync<T>` Určuje typy objektů vrácené metodou streamování. A `ChannelReader<T>` je vrácená z volání datového proudu a představuje datový proud na straně klienta. Načíst data, je běžné vzor smyčku `WaitToReadAsync` a volání `TryRead` při data nejsou k dispozici. Smyčky skončí při uzavření datový proud serverem, nebo token zrušení předaný `StreamAsChannelAsync` je zrušená.

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

## <a name="javascript-client"></a>JavaScript klienta

Klientům JavaScript volat metody streamování pro centra pomocí `connection.stream`. `stream` Metoda přijímá dva argumenty:

* Název metody rozbočovače. V následujícím příkladu je název metody rozbočovače `Counter`.
* Argumenty definované v metodě rozbočovače. V následujícím příkladu argumenty jsou: počet pro počet položek datového proudu pro příjem a zpoždění mezi položkami datového proudu.

`connection.stream` Vrátí `IStreamResult` obsahující `subscribe` metoda. Předat `IStreamSubscriber` k `subscribe` a nastavte `next`, `error`, a `complete` zpětná volání a dostávat oznámení od `stream` volání.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Na konec datového proudu z volání klienta `dispose` metodu `ISubscription` , je vrácena z `subscribe` metoda.

## <a name="related-resources"></a>Související informační zdroje

* [Centra](xref:signalr/hubs)
* [Klient .NET](xref:signalr/dotnet-client)
* [Klient JavaScriptu](xref:signalr/javascript-client)
* [Publikování do Azure](xref:signalr/publish-to-azure-web-app)