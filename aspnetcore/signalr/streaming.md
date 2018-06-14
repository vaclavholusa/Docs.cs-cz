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
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="3f708-102">Použití datových proudů v ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="3f708-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="3f708-103">Podle [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="3f708-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="3f708-104">Jádro ASP.NET SignalR podporuje streamování návratové hodnoty metody server.</span><span class="sxs-lookup"><span data-stu-id="3f708-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="3f708-105">To je užitečné pro scénáře, kde bude pocházet fragmenty dat průběhu času.</span><span class="sxs-lookup"><span data-stu-id="3f708-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="3f708-106">Když je návratovou hodnotu datového proudu, do klienta, každý fragment je odeslat klientovi, jakmile bude k dispozici, místo čekání všechna data k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3f708-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="3f708-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3f708-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="3f708-108">Nastavit rozbočovače</span><span class="sxs-lookup"><span data-stu-id="3f708-108">Set up the hub</span></span>

<span data-ttu-id="3f708-109">Metody rozbočovače automaticky stane streamování metody rozbočovače, až se obnoví `ChannelReader<T>` nebo `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="3f708-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="3f708-110">Níže je ukázka, která ukazuje základy streamování dat klientovi.</span><span class="sxs-lookup"><span data-stu-id="3f708-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="3f708-111">Vždy, když je objekt zapsán do `ChannelReader` tento objekt je okamžitě se odešlou do klienta.</span><span class="sxs-lookup"><span data-stu-id="3f708-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="3f708-112">Na konci `ChannelReader` dokončení říct klienta datový proud je uzavřen.</span><span class="sxs-lookup"><span data-stu-id="3f708-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="3f708-113">Zápis do `ChannelReader` na pozadí přístup z více vláken a vraťte se `ChannelReader` co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="3f708-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="3f708-114">Další volání rozbočovače se zablokuje, dokud `ChannelReader` je vrácen.</span><span class="sxs-lookup"><span data-stu-id="3f708-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="3f708-115">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="3f708-115">.NET client</span></span>

<span data-ttu-id="3f708-116">`StreamAsChannelAsync` Metodu `HubConnection` se používá k volání metody streamování.</span><span class="sxs-lookup"><span data-stu-id="3f708-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="3f708-117">Předat název metody rozbočovače a argumenty, které jsou definované v metodě rozbočovače k `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="3f708-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="3f708-118">Obecný parametr na `StreamAsChannelAsync<T>` Určuje typy objektů vrácené metodou streamování.</span><span class="sxs-lookup"><span data-stu-id="3f708-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="3f708-119">A `ChannelReader<T>` je vrácená z volání datového proudu a představuje datový proud na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="3f708-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="3f708-120">Načíst data, je běžné vzor smyčku `WaitToReadAsync` a volání `TryRead` při data nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3f708-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="3f708-121">Smyčky skončí při uzavření datový proud serverem, nebo token zrušení předaný `StreamAsChannelAsync` je zrušená.</span><span class="sxs-lookup"><span data-stu-id="3f708-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="3f708-122">JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="3f708-122">JavaScript client</span></span>

<span data-ttu-id="3f708-123">Klientům JavaScript volat metody streamování pro centra pomocí `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="3f708-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="3f708-124">`stream` Metoda přijímá dva argumenty:</span><span class="sxs-lookup"><span data-stu-id="3f708-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="3f708-125">Název metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="3f708-125">The name of the hub method.</span></span> <span data-ttu-id="3f708-126">V následujícím příkladu je název metody rozbočovače `Counter`.</span><span class="sxs-lookup"><span data-stu-id="3f708-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="3f708-127">Argumenty definované v metodě rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="3f708-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="3f708-128">V následujícím příkladu argumenty jsou: počet pro počet položek datového proudu pro příjem a zpoždění mezi položkami datového proudu.</span><span class="sxs-lookup"><span data-stu-id="3f708-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="3f708-129">`connection.stream` Vrátí `IStreamResult` obsahující `subscribe` metoda.</span><span class="sxs-lookup"><span data-stu-id="3f708-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="3f708-130">Předat `IStreamSubscriber` k `subscribe` a nastavte `next`, `error`, a `complete` zpětná volání a dostávat oznámení od `stream` volání.</span><span class="sxs-lookup"><span data-stu-id="3f708-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="3f708-131">Na konec datového proudu z volání klienta `dispose` metodu `ISubscription` , je vrácena z `subscribe` metoda.</span><span class="sxs-lookup"><span data-stu-id="3f708-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="3f708-132">Související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="3f708-132">Related resources</span></span>

* [<span data-ttu-id="3f708-133">Centra</span><span class="sxs-lookup"><span data-stu-id="3f708-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="3f708-134">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="3f708-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="3f708-135">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="3f708-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="3f708-136">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="3f708-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)