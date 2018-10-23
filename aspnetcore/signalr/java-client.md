---
title: Klientskou sadou Java základní funkce SignalR technologie ASP.NET
author: mikaelm12
description: Zjistěte, jak používat klientskou sadou Java funkce SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 10/18/2018
uid: signalr/java-client
ms.openlocfilehash: 77ea338f08b1986e69ba8ef1578c4cfe01a310de
ms.sourcegitcommit: ce6b6792c650708e92cdea051a5d166c0708c7c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2018
ms.locfileid: "49652303"
---
# <a name="aspnet-core-signalr-java-client"></a>Klientskou sadou Java základní funkce SignalR technologie ASP.NET

Podle [Mikael Mengistu](https://twitter.com/MikaelM_12)

Klientskou sadou Java umožňuje připojení k serveru funkce SignalR technologie ASP.NET Core z kódu Java, včetně aplikací pro Android. Podobně jako [javascriptový klient](xref:signalr/javascript-client) a [klienta .NET](xref:signalr/dotnet-client), klientskou sadou Java umožňuje příjem a odesílání zpráv do centra v reálném čase. Klientskou sadou Java je k dispozici v ASP.NET Core 2.2 a novější.

Konzolovou aplikaci Java vzorku, který odkazuje tento článek používá klientskou sadou SignalR Java.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Instalace balíčku pro klienta SignalR Java

*Signalr 1.0.0 preview3 35501* soubor JAR umožňuje klientům připojení k rozbočovačům SignalR. Číslo verze nejnovější soubor JAR, najdete v tématu [výsledky hledání Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Pokud používáte Gradle, přidejte následující řádek, který `dependencies` část vaší *build.gradle* souboru:

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

Pokud pomocí nástroje Maven, přidejte následující řádky uvnitř `<dependencies>` prvek vaše *pom.xml* souboru:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Připojení k rozbočovači

K navázání `HubConnection`, `HubConnectionBuilder` by měla sloužit. Při vytváření připojení se dá nakonfigurovat úrovně rozbočovače adresy URL a protokolu. Nakonfigurujte veškeré požadované možnosti voláním některé z `HubConnectionBuilder` metody před `build`. Zahájit připojení s `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Volání metod rozbočovače na z klienta

Volání `send` volá metodu rozbočovače. Předat název metody rozbočovače a argumentů podle metody rozbočovače na `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a>Volání metody klienta od rozbočovače

Použití `hubConnection.on` definovat metody na straně klienta, která může volat rozbočovače. Definujte metody po sestavení, ale před spuštěním připojení.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Přidání protokolování

Používá klientskou sadou SignalR Java [SLF4J](https://www.slf4j.org/) knihovny pro protokolování. Jde o rozhraní API vysoké úrovně protokolování, který umožňuje uživatelům knihovny zvolili vlastní implementace protokolování konkrétní díky možnostem protokolování konkrétní závislost. Následující fragment kódu ukazuje, jak používat `java.util.logging` s klientskou sadou SignalR Java.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Pokud nenakonfigurujete přihlášení závislostí, načte SLF4J protokolovacího nástroje výchozí žádné operace s tímto upozorněním:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

To můžete bezpečně ignorovat.

## <a name="known-limitations"></a>Známá omezení

Toto je verze preview služby klientskou sadou Java. Některé funkce nejsou podporovány:

* Pouze protokol JSON je podporován.
* Je podporován pouze přenosu objekty Websocket.
* Streamování se ještě nepodporuje.

## <a name="additional-resources"></a>Další zdroje

* [Reference k rozhraní Java API](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
