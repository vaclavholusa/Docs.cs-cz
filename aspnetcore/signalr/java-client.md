---
title: Klientskou sadou Java základní funkce SignalR technologie ASP.NET
author: mikaelm12
description: Zjistěte, jak používat klientskou sadou Java funkce SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 10/18/2018
uid: signalr/java-client
ms.openlocfilehash: 3d2cfe5f58cc41741512c68cebbbc90e8f714cba
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148782"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="6e1d5-103">Klientskou sadou Java základní funkce SignalR technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6e1d5-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="6e1d5-104">Podle [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="6e1d5-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="6e1d5-105">Klientskou sadou Java umožňuje připojení k serveru funkce SignalR technologie ASP.NET Core z kódu Java, včetně aplikací pro Android.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="6e1d5-106">Podobně jako [javascriptový klient](xref:signalr/javascript-client) a [klienta .NET](xref:signalr/dotnet-client), klientskou sadou Java umožňuje příjem a odesílání zpráv do centra v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="6e1d5-107">Klientskou sadou Java je k dispozici v ASP.NET Core 2.2 a novější.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="6e1d5-108">Konzolovou aplikaci Java vzorku, který odkazuje tento článek používá klientskou sadou SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="6e1d5-109">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6e1d5-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="6e1d5-110">Instalace balíčku pro klienta SignalR Java</span><span class="sxs-lookup"><span data-stu-id="6e1d5-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="6e1d5-111">*Signalr 1.0.0 preview3 35501* soubor JAR umožňuje klientům připojení k rozbočovačům SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-111">The *signalr-1.0.0-preview3-35501* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="6e1d5-112">Číslo verze nejnovější soubor JAR, najdete v tématu [výsledky hledání Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="6e1d5-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="6e1d5-113">Pokud používáte Gradle, přidejte následující řádek, který `dependencies` část vaší *build.gradle* souboru:</span><span class="sxs-lookup"><span data-stu-id="6e1d5-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

<span data-ttu-id="6e1d5-114">Pokud pomocí nástroje Maven, přidejte následující řádky uvnitř `<dependencies>` prvek vaše *pom.xml* souboru:</span><span class="sxs-lookup"><span data-stu-id="6e1d5-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="6e1d5-115">Připojení k rozbočovači</span><span class="sxs-lookup"><span data-stu-id="6e1d5-115">Connect to a hub</span></span>

<span data-ttu-id="6e1d5-116">K navázání `HubConnection`, `HubConnectionBuilder` by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="6e1d5-117">Při vytváření připojení se dá nakonfigurovat úrovně rozbočovače adresy URL a protokolu.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="6e1d5-118">Nakonfigurujte veškeré požadované možnosti voláním některé z `HubConnectionBuilder` metody před `build`.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="6e1d5-119">Zahájit připojení s `start`.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="6e1d5-120">Volání metod rozbočovače na z klienta</span><span class="sxs-lookup"><span data-stu-id="6e1d5-120">Call hub methods from client</span></span>

<span data-ttu-id="6e1d5-121">Volání `send` volá metodu rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="6e1d5-122">Předat název metody rozbočovače a argumentů podle metody rozbočovače na `send`.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="6e1d5-123">Volání metody klienta od rozbočovače</span><span class="sxs-lookup"><span data-stu-id="6e1d5-123">Call client methods from hub</span></span>

<span data-ttu-id="6e1d5-124">Použití `hubConnection.on` definovat metody na straně klienta, která může volat rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="6e1d5-125">Definujte metody po sestavení, ale před spuštěním připojení.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="6e1d5-126">Přidání protokolování</span><span class="sxs-lookup"><span data-stu-id="6e1d5-126">Add logging</span></span>

<span data-ttu-id="6e1d5-127">Používá klientskou sadou SignalR Java [SLF4J](https://www.slf4j.org/) knihovny pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-127">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="6e1d5-128">Jde o rozhraní API vysoké úrovně protokolování, který umožňuje uživatelům knihovny zvolili vlastní implementace protokolování konkrétní díky možnostem protokolování konkrétní závislost.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-128">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="6e1d5-129">Následující fragment kódu ukazuje, jak používat `java.util.logging` s klientskou sadou SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-129">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="6e1d5-130">Pokud nenakonfigurujete přihlášení závislostí, načte SLF4J protokolovacího nástroje výchozí žádné operace s tímto upozorněním:</span><span class="sxs-lookup"><span data-stu-id="6e1d5-130">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="6e1d5-131">To můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-131">This can safely be ignored.</span></span>

## <a name="known-limitations"></a><span data-ttu-id="6e1d5-132">Známá omezení</span><span class="sxs-lookup"><span data-stu-id="6e1d5-132">Known limitations</span></span>

<span data-ttu-id="6e1d5-133">Toto je verze preview služby klientskou sadou Java.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-133">This is a preview release of the Java client.</span></span> <span data-ttu-id="6e1d5-134">Některé funkce nejsou podporovány:</span><span class="sxs-lookup"><span data-stu-id="6e1d5-134">Some features aren't supported:</span></span>

* <span data-ttu-id="6e1d5-135">Pouze protokol JSON je podporován.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-135">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="6e1d5-136">Je podporován pouze přenosu objekty Websocket.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-136">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="6e1d5-137">Streamování se ještě nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="6e1d5-137">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6e1d5-138">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6e1d5-138">Additional resources</span></span>

* [<span data-ttu-id="6e1d5-139">Referenční dokumentace k rozhraní API v Javě</span><span class="sxs-lookup"><span data-stu-id="6e1d5-139">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
