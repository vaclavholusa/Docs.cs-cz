---
title: Klient .NET funkce SignalR technologie ASP.NET Core
author: tdykstra
description: Informace o .NET klienta SignalR technologie ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373316"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="ea5c1-103">Klient .NET funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea5c1-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="ea5c1-104">Podle [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="ea5c1-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="ea5c1-105">Klientská knihovna .NET funkce SignalR technologie ASP.NET Core umožňuje komunikovat s rozbočovače SignalR z aplikací .NET.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-105">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="ea5c1-106">Xamarin nabízí zvláštní požadavky na verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-106">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="ea5c1-107">Další informace najdete v tématu [klienta SignalR 2.1.1 v Xamarinu](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="ea5c1-107">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="ea5c1-108">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ea5c1-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ea5c1-109">Vzorový kód v tomto článku je aplikace WPF, která používá klienta .NET funkce SignalR technologie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-109">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="ea5c1-110">Instalace balíčku pro klienta SignalR .NET</span><span class="sxs-lookup"><span data-stu-id="ea5c1-110">Install the SignalR .NET client package</span></span>

<span data-ttu-id="ea5c1-111">`Microsoft.AspNetCore.SignalR.Client` Balíčku je potřeba pro klienty .NET pro připojení k rozbočovačům SignalR.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-111">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="ea5c1-112">Pokud chcete nainstalovat klientské knihovny, spusťte následující příkaz **Konzola správce balíčků** okno:</span><span class="sxs-lookup"><span data-stu-id="ea5c1-112">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="ea5c1-113">Připojení k rozbočovači</span><span class="sxs-lookup"><span data-stu-id="ea5c1-113">Connect to a hub</span></span>

<span data-ttu-id="ea5c1-114">K navázání připojení, vytvoření `HubConnectionBuilder` a volat `Build`.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="ea5c1-115">Adresa URL rozbočovače, protokol, typ přenosu, úroveň protokolu, záhlaví a další možnosti je možné nakonfigurovat při vytváření připojení.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="ea5c1-116">Nakonfigurujte veškeré požadované možnosti vložíte-li některý z `HubConnectionBuilder` metod do `Build`.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="ea5c1-117">Zahájit připojení s `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="ea5c1-118">Volání metod rozbočovače na z klienta</span><span class="sxs-lookup"><span data-stu-id="ea5c1-118">Call hub methods from client</span></span>

<span data-ttu-id="ea5c1-119">`InvokeAsync` volání metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-119">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="ea5c1-120">Předat název metody rozbočovače a argumentů podle metody rozbočovače na `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-120">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="ea5c1-121">SignalR je asynchronní, proto `async` a `await` při volání.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-121">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="ea5c1-122">Volání metody klienta od rozbočovače</span><span class="sxs-lookup"><span data-stu-id="ea5c1-122">Call client methods from hub</span></span>

<span data-ttu-id="ea5c1-123">Definovat metody rozbočovače volá pomocí `connection.On` po sestavení, ale před spuštěním připojení.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-123">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="ea5c1-124">Předchozí kód v `connection.On` spustí, když kód na straně serveru pomocí volání `SendAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-124">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="ea5c1-125">Protokolování a zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="ea5c1-125">Error handling and logging</span></span>

<span data-ttu-id="ea5c1-126">Zpracování chyb pomocí příkazu try-catch.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-126">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="ea5c1-127">Zkontrolujte `Exception` objektu určit správnou akce má být provedena, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="ea5c1-127">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="ea5c1-128">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ea5c1-128">Additional resources</span></span>

* [<span data-ttu-id="ea5c1-129">Centra</span><span class="sxs-lookup"><span data-stu-id="ea5c1-129">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ea5c1-130">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="ea5c1-130">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ea5c1-131">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="ea5c1-131">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
