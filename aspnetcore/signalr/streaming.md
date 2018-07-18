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
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="e3468-102">Použití datových proudů v knihovně SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3468-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="e3468-103">Podle [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="e3468-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="e3468-104">Funkce SignalR technologie ASP.NET Core podporuje streamování návratové hodnoty metod serveru.</span><span class="sxs-lookup"><span data-stu-id="e3468-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="e3468-105">To je užitečné pro scénáře, kde budou přicházet fragmenty dat průběhu času.</span><span class="sxs-lookup"><span data-stu-id="e3468-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="e3468-106">Pokud vrácená hodnota se streamuje klientovi, každý fragment je odeslat klientovi, jakmile bude k dispozici, nikoli čeká všechna data k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e3468-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="e3468-107">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e3468-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="e3468-108">Nastavení centra</span><span class="sxs-lookup"><span data-stu-id="e3468-108">Set up the hub</span></span>

<span data-ttu-id="e3468-109">Metodu rozbočovače na automaticky stane streamování metody rozbočovače, je po návratu `ChannelReader<T>` nebo `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="e3468-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="e3468-110">Níže je příklad, který ukazuje základy streamovaných dat na klientovi.</span><span class="sxs-lookup"><span data-stu-id="e3468-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="e3468-111">Vždy, když je objekt zapsána do `ChannelReader` tento objekt se okamžitě se odešlou do klienta.</span><span class="sxs-lookup"><span data-stu-id="e3468-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="e3468-112">Na konci `ChannelReader` je dokončit, aby se dali pokyn klientovi, datový proud je uzavřen.</span><span class="sxs-lookup"><span data-stu-id="e3468-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="e3468-113">Zápis do `ChannelReader` na vlákně na pozadí a vraťte se `ChannelReader` co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="e3468-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="e3468-114">Další volání rozbočovače se zablokuje, dokud `ChannelReader` je vrácena.</span><span class="sxs-lookup"><span data-stu-id="e3468-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="e3468-115">.NET client</span><span class="sxs-lookup"><span data-stu-id="e3468-115">.NET client</span></span>

<span data-ttu-id="e3468-116">`StreamAsChannelAsync` Metodu na `HubConnection` se používá k volání metody streaming.</span><span class="sxs-lookup"><span data-stu-id="e3468-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="e3468-117">Předat název metody rozbočovače a argumenty, které jsou definovány v metody rozbočovače na `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="e3468-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="e3468-118">Obecný parametr na `StreamAsChannelAsync<T>` Určuje typ objektu vrácený metodou streamování.</span><span class="sxs-lookup"><span data-stu-id="e3468-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="e3468-119">A `ChannelReader<T>` vrácená z volání služby stream a představuje datový proud na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e3468-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="e3468-120">Čtení dat, běžně používá k vytvoření smyčky přes `WaitToReadAsync` a volat `TryRead` kdy data jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e3468-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="e3468-121">Smyčky se ukončí, pokud datový proud bylo ukončeno serverem nebo předat token zrušení `StreamAsChannelAsync` se zruší.</span><span class="sxs-lookup"><span data-stu-id="e3468-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="e3468-122">Javascriptový klient</span><span class="sxs-lookup"><span data-stu-id="e3468-122">JavaScript client</span></span>

<span data-ttu-id="e3468-123">Klientů JavaScript pomocí volání metody streaming v centrech `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="e3468-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="e3468-124">`stream` Metoda přijímá dva argumenty:</span><span class="sxs-lookup"><span data-stu-id="e3468-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="e3468-125">Název metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="e3468-125">The name of the hub method.</span></span> <span data-ttu-id="e3468-126">V následujícím příkladu je název metody rozbočovače `Counter`.</span><span class="sxs-lookup"><span data-stu-id="e3468-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="e3468-127">Argumenty podle metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="e3468-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="e3468-128">V následujícím příkladu jsou argumenty: počet pro počet položek datového proudu pro příjem a zpoždění mezi položkami datového proudu.</span><span class="sxs-lookup"><span data-stu-id="e3468-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="e3468-129">`connection.stream` Vrátí `IStreamResult` obsahující `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="e3468-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="e3468-130">Předejte `IStreamSubscriber` k `subscribe` a nastavit `next`, `error`, a `complete` zpětná volání, které chcete dostávat oznámení `stream` vyvolání.</span><span class="sxs-lookup"><span data-stu-id="e3468-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="e3468-131">Do konce datového proudu z volání klienta `dispose` metodu na `ISubscription` , který je vrácen z `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="e3468-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="e3468-132">Související prostředky</span><span class="sxs-lookup"><span data-stu-id="e3468-132">Related resources</span></span>

* [<span data-ttu-id="e3468-133">Centra</span><span class="sxs-lookup"><span data-stu-id="e3468-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e3468-134">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="e3468-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e3468-135">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="e3468-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="e3468-136">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="e3468-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
