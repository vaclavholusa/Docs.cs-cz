---
title: Klient .NET funkce SignalR technologie ASP.NET Core
author: tdykstra
description: Informace o .NET klienta SignalR technologie ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/07/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 970888a410b2486a20f98ce77a8674f8ec357f50
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655249"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="dc78d-103">Klient .NET funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc78d-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="dc78d-104">Podle [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="dc78d-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="dc78d-105">Klient .NET funkce SignalR technologie ASP.NET Core můžete využívat aplikace pro Xamarin, WPF, Windows Forms, konzoly a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc78d-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="dc78d-106">Podobně jako [javascriptový klient](xref:signalr/javascript-client), klient .NET umožňuje příjem a odesílání a příjem zpráv do centra v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="dc78d-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

> [!NOTE]
> <span data-ttu-id="dc78d-107">Xamarin nabízí zvláštní požadavky na verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc78d-107">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="dc78d-108">Další informace najdete v tématu [klienta SignalR 2.1.1 v Xamarinu](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="dc78d-108">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="dc78d-109">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dc78d-109">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="dc78d-110">Vzorový kód v tomto článku je aplikace WPF, která používá klienta .NET funkce SignalR technologie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc78d-110">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="dc78d-111">Instalace balíčku pro klienta SignalR .NET</span><span class="sxs-lookup"><span data-stu-id="dc78d-111">Install the SignalR .NET client package</span></span>

<span data-ttu-id="dc78d-112">`Microsoft.AspNetCore.SignalR.Client` Balíčku je potřeba pro klienty .NET pro připojení k rozbočovačům SignalR.</span><span class="sxs-lookup"><span data-stu-id="dc78d-112">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="dc78d-113">Pokud chcete nainstalovat klientské knihovny, spusťte následující příkaz **Konzola správce balíčků** okno:</span><span class="sxs-lookup"><span data-stu-id="dc78d-113">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="dc78d-114">Připojení k rozbočovači</span><span class="sxs-lookup"><span data-stu-id="dc78d-114">Connect to a hub</span></span>

<span data-ttu-id="dc78d-115">K navázání připojení, vytvoření `HubConnectionBuilder` a volat `Build`.</span><span class="sxs-lookup"><span data-stu-id="dc78d-115">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="dc78d-116">Adresa URL rozbočovače, protokol, typ přenosu, úroveň protokolu, záhlaví a další možnosti je možné nakonfigurovat při vytváření připojení.</span><span class="sxs-lookup"><span data-stu-id="dc78d-116">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="dc78d-117">Nakonfigurujte veškeré požadované možnosti vložíte-li některý z `HubConnectionBuilder` metod do `Build`.</span><span class="sxs-lookup"><span data-stu-id="dc78d-117">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="dc78d-118">Zahájit připojení s `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc78d-118">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="dc78d-119">Volání metod rozbočovače na z klienta</span><span class="sxs-lookup"><span data-stu-id="dc78d-119">Call hub methods from client</span></span>

<span data-ttu-id="dc78d-120">`InvokeAsync` volání metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="dc78d-120">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="dc78d-121">Předat název metody rozbočovače a argumentů podle metody rozbočovače na `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc78d-121">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="dc78d-122">SignalR je asynchronní, proto `async` a `await` při volání.</span><span class="sxs-lookup"><span data-stu-id="dc78d-122">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="dc78d-123">Volání metody klienta od rozbočovače</span><span class="sxs-lookup"><span data-stu-id="dc78d-123">Call client methods from hub</span></span>

<span data-ttu-id="dc78d-124">Definovat metody rozbočovače volá pomocí `connection.On` po sestavení, ale před spuštěním připojení.</span><span class="sxs-lookup"><span data-stu-id="dc78d-124">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="dc78d-125">Předchozí kód v `connection.On` spustí, když kód na straně serveru pomocí volání `SendAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="dc78d-125">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="dc78d-126">Protokolování a zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="dc78d-126">Error handling and logging</span></span>

<span data-ttu-id="dc78d-127">Zpracování chyb pomocí příkazu try-catch.</span><span class="sxs-lookup"><span data-stu-id="dc78d-127">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="dc78d-128">Zkontrolujte `Exception` objektu určit správnou akce má být provedena, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="dc78d-128">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="dc78d-129">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="dc78d-129">Additional resources</span></span>

* [<span data-ttu-id="dc78d-130">Centra</span><span class="sxs-lookup"><span data-stu-id="dc78d-130">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="dc78d-131">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="dc78d-131">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="dc78d-132">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="dc78d-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
