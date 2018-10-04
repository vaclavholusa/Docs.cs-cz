---
title: Funkce SignalR technologie ASP.NET Core podporované platformy
author: tdykstra
description: Podporované platformy pro funkci SignalR technologie ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/26/2018
uid: signalr/supported-platforms
ms.openlocfilehash: d6d74a55d35ddb34a6f66a171bfe3f343dd61b63
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577623"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="60962-103">Funkce SignalR technologie ASP.NET Core podporované platformy</span><span class="sxs-lookup"><span data-stu-id="60962-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="60962-104">Požadavky na systém Server</span><span class="sxs-lookup"><span data-stu-id="60962-104">Server system requirements</span></span>

<span data-ttu-id="60962-105">Funkce SignalR technologie ASP.NET Core podporuje libovolnou platformu serveru, který podporuje ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60962-105">SignalR for ASP.NET Core supports any server platform ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="60962-106">Javascriptový klient</span><span class="sxs-lookup"><span data-stu-id="60962-106">JavaScript client</span></span>

<span data-ttu-id="60962-107">[Javascriptový klient](https://www.npmjs.com/package/@aspnet/signalr) běží v prostředí NodeJS 8 a novějších verzí a následujících prohlížečů:</span><span class="sxs-lookup"><span data-stu-id="60962-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="60962-108">Prohlížeč</span><span class="sxs-lookup"><span data-stu-id="60962-108">Browser</span></span> | <span data-ttu-id="60962-109">Version</span><span class="sxs-lookup"><span data-stu-id="60962-109">Version</span></span> |
| ------- | ------- |
| <span data-ttu-id="60962-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="60962-110">Microsoft Edge</span></span> | <span data-ttu-id="60962-111">aktuální</span><span class="sxs-lookup"><span data-stu-id="60962-111">current</span></span> |
| <span data-ttu-id="60962-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="60962-112">Mozilla Firefox</span></span> | <span data-ttu-id="60962-113">aktuální</span><span class="sxs-lookup"><span data-stu-id="60962-113">current</span></span> |
| <span data-ttu-id="60962-114">Google Chrome; zahrnuje Android</span><span class="sxs-lookup"><span data-stu-id="60962-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="60962-115">aktuální</span><span class="sxs-lookup"><span data-stu-id="60962-115">current</span></span> |
| <span data-ttu-id="60962-116">Safari; zahrnuje iOS</span><span class="sxs-lookup"><span data-stu-id="60962-116">Safari; includes iOS</span></span> | <span data-ttu-id="60962-117">aktuální</span><span class="sxs-lookup"><span data-stu-id="60962-117">current</span></span> |
| <span data-ttu-id="60962-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="60962-118">Microsoft Internet Explorer</span></span> | <span data-ttu-id="60962-119">11</span><span class="sxs-lookup"><span data-stu-id="60962-119">11</span></span> |
 
## <a name="net-client"></a><span data-ttu-id="60962-120">.NET client</span><span class="sxs-lookup"><span data-stu-id="60962-120">.NET client</span></span>

<span data-ttu-id="60962-121">[Klienta .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) běží na libovolné platformě server podporuje ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60962-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any server platform supported by ASP.NET Core.</span></span>

<span data-ttu-id="60962-122">Když na serveru běží služby IIS, přenosové protokoly Websocket vyžaduje službu IIS 8.0 nebo novější, na Windows Server 2012 nebo novějším.</span><span class="sxs-lookup"><span data-stu-id="60962-122">When the server runs IIS, the WebSockets transport requires IIS 8.0 or higher, on Windows Server 2012 or higher.</span></span> <span data-ttu-id="60962-123">Jiné přenosy jsou podporované na všech platformách.</span><span class="sxs-lookup"><span data-stu-id="60962-123">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="60962-124">Klientskou sadou Java</span><span class="sxs-lookup"><span data-stu-id="60962-124">Java client</span></span>

<span data-ttu-id="60962-125">[Klientskou sadou Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) podporuje Javou 8 a novější verze.</span><span class="sxs-lookup"><span data-stu-id="60962-125">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="60962-126">Nepodporovaná klientů</span><span class="sxs-lookup"><span data-stu-id="60962-126">Unsupported clients</span></span>

<span data-ttu-id="60962-127">Následující klienti jsou k dispozici, ale jsou experimentální nebo neoficiální.</span><span class="sxs-lookup"><span data-stu-id="60962-127">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="60962-128">Tyto nejsou nyní podporovány a někdy nemusí být podporované.</span><span class="sxs-lookup"><span data-stu-id="60962-128">They are not supported now and may not ever be supported.</span></span>

* [<span data-ttu-id="60962-129">Kódu klienta jazyka C++</span><span class="sxs-lookup"><span data-stu-id="60962-129">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="60962-130">SWIFT klienta</span><span class="sxs-lookup"><span data-stu-id="60962-130">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
