---
title: Klientskou sadou Java základní funkce SignalR technologie ASP.NET
author: mikaelm12
description: Zjistěte, jak používat klientskou sadou Java funkce SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: 0eba59a05ea6fd3fed46fcab86ac20caf40ebb65
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482915"
---
# <a name="aspnet-core-signalr-java-client"></a>Klientskou sadou Java funkce SignalR technologie ASP.NET Core

Podle [Mikael Mengistu](https://twitter.com/MikaelM_12)

Klientskou sadou Java umožňuje připojení k serveru funkce SignalR technologie ASP.NET Core z kódu Java, včetně aplikací pro Android. Podobně jako [javascriptový klient](xref:signalr/javascript-client) a [klienta .NET](xref:signalr/dotnet-client), klientskou sadou Java umožňuje příjem a odesílání zpráv do centra v reálném čase. Klientskou sadou Java je k dispozici v ASP.NET Core 2.2 a novější.

Konzolovou aplikaci Java vzorku, který odkazuje tento článek používá klientskou sadou SignalR Java.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Instalace balíčku pro klienta SignalR Java

*Signalr 0.1.0 preview2 35174* soubor JAR umožňuje klientům připojení k rozbočovačům SignalR. Číslo verze nejnovější soubor JAR, najdete v tématu [výsledky hledání Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).

Pokud používáte Gradle, přidejte následující řádek, který `dependencies` část vaší *build.gradle* souboru:

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

Pokud pomocí nástroje Maven, přidejte následující řádky uvnitř `<dependencies>` prvek vaše *pom.xml* souboru:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Připojení k rozbočovači

K navázání `HubConnection`, `HubConnectionBuilder` by měla sloužit. Při vytváření připojení se dá nakonfigurovat úrovně rozbočovače adresy URL a protokolu. Nakonfigurujte veškeré požadované možnosti voláním některé z `HubConnectionBuilder` metody před `build`. Zahájit připojení s `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a>Volání metod rozbočovače na z klienta

Volání `send` volá metodu rozbočovače. Předat název metody rozbočovače a argumentů podle metody rozbočovače na `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a>Volání metody klienta od rozbočovače

Použití `hubConnection.on` definovat metody na straně klienta, která může volat rozbočovače. Definujte metody po sestavení, ale před spuštěním připojení.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a>Známá omezení

Jde o předběžnou verzi preview klienta Java. Existuje mnoho funkcí, které se zatím nepodporují. Následující mezery se pracuje v budoucích verzích:

* Pouze primitivní typy mohou být přijímány jako parametry a návratové typy.
* Rozhraní API jsou synchronní.
* V tuto chvíli je podporován pouze typ "Odeslat" volání. "Vyvolat" a vysílání datového proudu návratové hodnoty nejsou podporovány.
* Pouze protokol JSON je podporován.
* Je podporován pouze přenosu objekty Websocket.

## <a name="additional-resources"></a>Další zdroje

* [Reference k rozhraní Java API](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
