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
# <a name="aspnet-core-signalr-supported-platforms"></a>Funkce SignalR technologie ASP.NET Core podporované platformy

## <a name="server-system-requirements"></a>Požadavky na systém Server

Funkce SignalR technologie ASP.NET Core podporuje libovolnou platformu serveru, který podporuje ASP.NET Core.

## <a name="javascript-client"></a>Javascriptový klient

[Javascriptový klient](https://www.npmjs.com/package/@aspnet/signalr) běží v prostředí NodeJS 8 a novějších verzí a následujících prohlížečů:

| Prohlížeč | Version |
| ------- | ------- |
| Microsoft Edge | aktuální |
| Mozilla Firefox | aktuální |
| Google Chrome; zahrnuje Android | aktuální |
| Safari; zahrnuje iOS | aktuální |
| Microsoft Internet Explorer | 11 |
 
## <a name="net-client"></a>.NET client

[Klienta .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) běží na libovolné platformě server podporuje ASP.NET Core.

Když na serveru běží služby IIS, přenosové protokoly Websocket vyžaduje službu IIS 8.0 nebo novější, na Windows Server 2012 nebo novějším. Jiné přenosy jsou podporované na všech platformách.

## <a name="java-client"></a>Klientskou sadou Java

[Klientskou sadou Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) podporuje Javou 8 a novější verze.

## <a name="unsupported-clients"></a>Nepodporovaná klientů

Následující klienti jsou k dispozici, ale jsou experimentální nebo neoficiální. Tyto nejsou nyní podporovány a někdy nemusí být podporované.

* [Kódu klienta jazyka C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [SWIFT klienta](https://github.com/moozzyk/SignalR-Client-Swift)
