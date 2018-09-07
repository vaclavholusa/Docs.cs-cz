---
title: Klientskou sadou Java základní funkce SignalR technologie ASP.NET
author: mikaelm12
description: Zjistěte, jak používat klientskou sadou Java funkce SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: f110f5391ac34f5cb4a72f64c16d86c8a37369a2
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "43995427"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="89854-103">Klientskou sadou Java funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89854-103">ASP.NET Core SignalR Java Client</span></span>

<span data-ttu-id="89854-104">Podle [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="89854-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="89854-105">Klientskou sadou Java umožňuje připojení k serveru funkce SignalR technologie ASP.NET Core z kódu Java, včetně aplikací pro Android.</span><span class="sxs-lookup"><span data-stu-id="89854-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="89854-106">Podobně jako [javascriptový klient](xref:signalr/javascript-client) a [klienta .NET](xref:signalr/dotnet-client), klientskou sadou Java umožňuje příjem a odesílání zpráv do centra v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="89854-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="89854-107">Klientskou sadou Java je k dispozici v ASP.NET Core 2.2 a novější.</span><span class="sxs-lookup"><span data-stu-id="89854-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="89854-108">Konzolovou aplikaci Java vzorku, který odkazuje tento článek používá klientskou sadou SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="89854-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="89854-109">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="89854-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="89854-110">Instalace balíčku pro klienta SignalR Java</span><span class="sxs-lookup"><span data-stu-id="89854-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="89854-111">*Signalr 0.1.0 preview1 35029* soubor JAR umožňuje klientům připojení k rozbočovačům SignalR.</span><span class="sxs-lookup"><span data-stu-id="89854-111">The *signalr-0.1.0-preview1-35029* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="89854-112">Číslo verze nejnovější soubor JAR, najdete v tématu [výsledky hledání Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span><span class="sxs-lookup"><span data-stu-id="89854-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span></span>

<span data-ttu-id="89854-113">Pokud používáte Gradle, přidejte následující řádek, který `dependencies` část vaší *build.gradle* souboru:</span><span class="sxs-lookup"><span data-stu-id="89854-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview1-35029'
```

<span data-ttu-id="89854-114">Pokud pomocí nástroje Maven, přidejte následující řádky uvnitř `<dependencies>` prvek vaše *pom.xml* souboru:</span><span class="sxs-lookup"><span data-stu-id="89854-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="89854-115">Připojení k rozbočovači</span><span class="sxs-lookup"><span data-stu-id="89854-115">Connect to a hub</span></span>

<span data-ttu-id="89854-116">K navázání `HubConnection`, `HubConnectionBuilder` by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="89854-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="89854-117">Při vytváření připojení se dá nakonfigurovat úrovně rozbočovače adresy URL a protokolu.</span><span class="sxs-lookup"><span data-stu-id="89854-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="89854-118">Nakonfigurujte veškeré požadované možnosti voláním některé z `HubConnectionBuilder` metody před `build`.</span><span class="sxs-lookup"><span data-stu-id="89854-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="89854-119">Zahájit připojení s `start`.</span><span class="sxs-lookup"><span data-stu-id="89854-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="89854-120">Volání metod rozbočovače na z klienta</span><span class="sxs-lookup"><span data-stu-id="89854-120">Call hub methods from client</span></span>

<span data-ttu-id="89854-121">Volání `send` volá metodu rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="89854-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="89854-122">Předat název metody rozbočovače a argumentů podle metody rozbočovače na `send`.</span><span class="sxs-lookup"><span data-stu-id="89854-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="89854-123">Volání metody klienta od rozbočovače</span><span class="sxs-lookup"><span data-stu-id="89854-123">Call client methods from hub</span></span>

<span data-ttu-id="89854-124">Použití `hubConnection.on` definovat metody na straně klienta, která může volat rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="89854-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="89854-125">Definujte metody po sestavení, ale před spuštěním připojení.</span><span class="sxs-lookup"><span data-stu-id="89854-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a><span data-ttu-id="89854-126">Známá omezení</span><span class="sxs-lookup"><span data-stu-id="89854-126">Known limitations</span></span>

<span data-ttu-id="89854-127">Jde o předběžnou verzi preview klienta Java.</span><span class="sxs-lookup"><span data-stu-id="89854-127">This is an early preview release of the Java client.</span></span> <span data-ttu-id="89854-128">Existuje mnoho funkcí, které se zatím nepodporují.</span><span class="sxs-lookup"><span data-stu-id="89854-128">There are many features that aren't supported yet.</span></span> <span data-ttu-id="89854-129">Následující mezery se pracuje v budoucích verzích:</span><span class="sxs-lookup"><span data-stu-id="89854-129">The following gaps are being worked on for future releases:</span></span>

* <span data-ttu-id="89854-130">Pouze primitivní typy mohou být přijímány jako parametry a návratové typy.</span><span class="sxs-lookup"><span data-stu-id="89854-130">Only primitive types can be accepted as parameters and return types.</span></span>
* <span data-ttu-id="89854-131">Rozhraní API jsou synchronní.</span><span class="sxs-lookup"><span data-stu-id="89854-131">The APIs are synchronous.</span></span>
* <span data-ttu-id="89854-132">V tuto chvíli je podporován pouze typ "Odeslat" volání.</span><span class="sxs-lookup"><span data-stu-id="89854-132">Only the "Send" call type is supported at this time.</span></span> <span data-ttu-id="89854-133">"Vyvolat" a vysílání datového proudu návratové hodnoty nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="89854-133">"Invoke" and streaming of return values aren't supported.</span></span>
* <span data-ttu-id="89854-134">Klient v současné době nepodporuje [služby Azure SignalR](/azure/azure-signalr/).</span><span class="sxs-lookup"><span data-stu-id="89854-134">The client doesn't currently support the [Azure SignalR Service](/azure/azure-signalr/).</span></span>
* <span data-ttu-id="89854-135">Pouze protokol JSON je podporován.</span><span class="sxs-lookup"><span data-stu-id="89854-135">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="89854-136">Je podporován pouze přenosu objekty Websocket.</span><span class="sxs-lookup"><span data-stu-id="89854-136">Only the WebSockets transport is supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="89854-137">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="89854-137">Additional resources</span></span>

* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
