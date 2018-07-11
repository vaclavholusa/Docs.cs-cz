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
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="278da-103">Klient .NET funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="278da-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="278da-104">Podle [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="278da-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="278da-105">Klient .NET funkce SignalR technologie ASP.NET Core můžete využívat aplikace pro Xamarin, WPF, Windows Forms, konzoly a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="278da-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="278da-106">Podobně jako [javascriptový klient](xref:signalr/javascript-client), klient .NET umožňuje příjem a odesílání a příjem zpráv do centra v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="278da-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

<span data-ttu-id="278da-107">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="278da-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="278da-108">Vzorový kód v tomto článku je aplikace WPF, která používá klienta .NET funkce SignalR technologie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="278da-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="278da-109">Instalace balíčku pro klienta SignalR .NET</span><span class="sxs-lookup"><span data-stu-id="278da-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="278da-110">`Microsoft.AspNetCore.SignalR.Client` Balíčku je potřeba pro klienty .NET pro připojení k rozbočovačům SignalR.</span><span class="sxs-lookup"><span data-stu-id="278da-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="278da-111">Pokud chcete nainstalovat klientské knihovny, spusťte následující příkaz **Konzola správce balíčků** okno:</span><span class="sxs-lookup"><span data-stu-id="278da-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="278da-112">Připojení k rozbočovači</span><span class="sxs-lookup"><span data-stu-id="278da-112">Connect to a hub</span></span>

<span data-ttu-id="278da-113">K navázání připojení, vytvoření `HubConnectionBuilder` a volat `Build`.</span><span class="sxs-lookup"><span data-stu-id="278da-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="278da-114">Adresa URL rozbočovače, protokol, typ přenosu, úroveň protokolu, záhlaví a další možnosti je možné nakonfigurovat při vytváření připojení.</span><span class="sxs-lookup"><span data-stu-id="278da-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="278da-115">Nakonfigurujte veškeré požadované možnosti vložíte-li některý z `HubConnectionBuilder` metod do `Build`.</span><span class="sxs-lookup"><span data-stu-id="278da-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="278da-116">Zahájit připojení s `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="278da-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="278da-117">Volání metod rozbočovače na z klienta</span><span class="sxs-lookup"><span data-stu-id="278da-117">Call hub methods from client</span></span>

<span data-ttu-id="278da-118">`InvokeAsync` volání metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="278da-118">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="278da-119">Předat název metody rozbočovače a argumentů podle metody rozbočovače na `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="278da-119">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="278da-120">SignalR je asynchronní, proto `async` a `await` při volání.</span><span class="sxs-lookup"><span data-stu-id="278da-120">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="278da-121">Volání metody klienta od rozbočovače</span><span class="sxs-lookup"><span data-stu-id="278da-121">Call client methods from hub</span></span>

<span data-ttu-id="278da-122">Definovat metody rozbočovače volá pomocí `connection.On` po sestavení, ale před spuštěním připojení.</span><span class="sxs-lookup"><span data-stu-id="278da-122">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="278da-123">Předchozí kód v `connection.On` spustí, když kód na straně serveru pomocí volání `SendAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="278da-123">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="278da-124">Protokolování a zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="278da-124">Error handling and logging</span></span>

<span data-ttu-id="278da-125">Zpracování chyb pomocí příkazu try-catch.</span><span class="sxs-lookup"><span data-stu-id="278da-125">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="278da-126">Zkontrolujte `Exception` objektu určit správnou akce má být provedena, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="278da-126">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="278da-127">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="278da-127">Additional resources</span></span>

* [<span data-ttu-id="278da-128">Centra</span><span class="sxs-lookup"><span data-stu-id="278da-128">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="278da-129">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="278da-129">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="278da-130">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="278da-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
