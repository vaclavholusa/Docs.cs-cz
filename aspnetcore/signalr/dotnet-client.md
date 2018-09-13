---
title: Klient .NET funkce SignalR technologie ASP.NET Core
author: tdykstra
description: Informace o .NET klienta SignalR technologie ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: ef84ede2ed45ddc3b64d4ce8f5bd0018a681faf6
ms.sourcegitcommit: 4db337bd47d70c06fff91000c58bc048a491ccec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2018
ms.locfileid: "44749318"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="f0dfb-103">Klient .NET funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0dfb-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="f0dfb-104">Klientská knihovna .NET funkce SignalR technologie ASP.NET Core umožňuje komunikovat s rozbočovače SignalR z aplikací .NET.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="f0dfb-105">Xamarin nabízí zvláštní požadavky na verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="f0dfb-106">Další informace najdete v tématu [klienta SignalR 2.1.1 v Xamarinu](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="f0dfb-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="f0dfb-107">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f0dfb-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f0dfb-108">Vzorový kód v tomto článku je aplikace WPF, která používá klienta .NET funkce SignalR technologie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="f0dfb-109">Instalace balíčku pro klienta SignalR .NET</span><span class="sxs-lookup"><span data-stu-id="f0dfb-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="f0dfb-110">`Microsoft.AspNetCore.SignalR.Client` Balíčku je potřeba pro klienty .NET pro připojení k rozbočovačům SignalR.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="f0dfb-111">Pokud chcete nainstalovat klientské knihovny, spusťte následující příkaz **Konzola správce balíčků** okno:</span><span class="sxs-lookup"><span data-stu-id="f0dfb-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="f0dfb-112">Připojení k rozbočovači</span><span class="sxs-lookup"><span data-stu-id="f0dfb-112">Connect to a hub</span></span>

<span data-ttu-id="f0dfb-113">K navázání připojení, vytvoření `HubConnectionBuilder` a volat `Build`.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="f0dfb-114">Adresa URL rozbočovače, protokol, typ přenosu, úroveň protokolu, záhlaví a další možnosti je možné nakonfigurovat při vytváření připojení.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="f0dfb-115">Nakonfigurujte veškeré požadované možnosti vložíte-li některý z `HubConnectionBuilder` metod do `Build`.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="f0dfb-116">Zahájit připojení s `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="f0dfb-117">Zpracování došlo ke ztrátě připojení</span><span class="sxs-lookup"><span data-stu-id="f0dfb-117">Handle lost connection</span></span>

<span data-ttu-id="f0dfb-118">Použití <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> události a reagovat na došlo ke ztrátě připojení.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="f0dfb-119">Můžete například chtít automatizovat opětovné připojení.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="f0dfb-120">`Closed` Událost vyžaduje delegáta, který vrátí `Task`, což umožňuje asynchronní kód ke spuštění bez použití `async void`.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="f0dfb-121">Tím se uspokojí delegáta v `Closed` obslužná rutina události, která spustí synchronně, vracet `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="f0dfb-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="f0dfb-122">Hlavním důvodem pro asynchronní podporu je tak můžete restartovat připojení.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="f0dfb-123">Spouští se připojení je asynchronní akce.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="f0dfb-124">V `Closed` obslužná rutina, která restartuje připojení, vezměte v úvahu časový limit na některé náhodné zpoždění, aby se zabránilo přetížení serveru, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="f0dfb-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="f0dfb-125">Volání metod rozbočovače na z klienta</span><span class="sxs-lookup"><span data-stu-id="f0dfb-125">Call hub methods from client</span></span>

<span data-ttu-id="f0dfb-126">`InvokeAsync` volání metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="f0dfb-127">Předat název metody rozbočovače a argumentů podle metody rozbočovače na `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="f0dfb-128">SignalR je asynchronní, proto `async` a `await` při volání.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="f0dfb-129">Volání metody klienta od rozbočovače</span><span class="sxs-lookup"><span data-stu-id="f0dfb-129">Call client methods from hub</span></span>

<span data-ttu-id="f0dfb-130">Definovat metody rozbočovače volá pomocí `connection.On` po sestavení, ale před spuštěním připojení.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-130">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="f0dfb-131">Předchozí kód v `connection.On` spustí, když kód na straně serveru pomocí volání `SendAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-131">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="f0dfb-132">Protokolování a zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="f0dfb-132">Error handling and logging</span></span>

<span data-ttu-id="f0dfb-133">Zpracování chyb pomocí příkazu try-catch.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-133">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="f0dfb-134">Zkontrolujte `Exception` objektu určit správnou akce má být provedena, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="f0dfb-134">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="f0dfb-135">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f0dfb-135">Additional resources</span></span>

* [<span data-ttu-id="f0dfb-136">Centra</span><span class="sxs-lookup"><span data-stu-id="f0dfb-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="f0dfb-137">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="f0dfb-137">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="f0dfb-138">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="f0dfb-138">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
