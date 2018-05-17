---
title: Podpora pro objekty WebSockets v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak začít pracovat s objekty WebSockets v ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: ede8064b5e77024b843357d4715869b3495b9147
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/14/2018
---
# <a name="websockets-support-in-aspnet-core"></a>Podpora pro objekty WebSockets v ASP.NET Core

Podle [tní Dykstra](https://github.com/tdykstra) a [Andrew Stanton-Nurse](https://github.com/anurse)

Tento článek vysvětluje, jak začít pracovat s objekty WebSockets v ASP.NET Core. [Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP. Používá se v aplikacích, které těžit z rychlé, v reálném čase komunikaci, jako je například konverzace a řídicí panel a herní aplikace.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)). Najdete v článku [další kroky](#next-steps) části Další informace.

## <a name="prerequisites"></a>Požadavky

* ASP.NET Core 1.1 nebo novější
* Všechny operační systém, který podporuje ASP.NET Core:
  
  * Windows 7 nebo Windows Server 2008 nebo novější
  * Linux
  * macOS
  
* Pokud aplikace běží v systému Windows pomocí služby IIS:

  * Windows 8 nebo Windows Server 2012 nebo novější
  * Služba IIS 8 / Express IIS 8
  * Technologie WebSockets musí být povolena ve službě IIS (najdete v článku [podporu služby IIS/IIS Express](#iisiis-express-support) části.)
  
* Pokud aplikace běží [HTTP.sys](xref:fundamentals/servers/httpsys):

  * Windows 8 nebo Windows Server 2012 nebo novější

* Podporované prohlížeče, najdete v části https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>Kdy použít objekty WebSockets

Pomocí technologie WebSockets pracovat přímo s připojení soketu. Například použijte Websocket pro nejlepší možný výkon s hry s v reálném čase.

[Funkce SignalR technologie ASP.NET](/aspnet/signalr/overview/getting-started/introduction-to-signalr) poskytuje bohatší model aplikace pro funkce v reálném čase, ale lze spustit pouze na ASP.NET 4.x, ne jádro ASP.NET. Ve verzi ASP.NET Core funkce signalr se plánuje vydání s ASP.NET Core 2.1. V tématu [plánování vysoké úrovně ASP.NET Core 2.1](https://github.com/aspnet/Announcements/issues/288).

Dokud SignalR Core vydání, můžete použít objekty WebSockets. Funkce, které funkce SignalR poskytuje však musí zadat a vývojář podporována. Příklad:

* Podpora pro širší škálu verze prohlížeče pomocí automatického přechodu na metody alternativní přenosu.
* Automatické obnovení připojení při připojení zahodí.
* Podpora pro klienty, volání metod na serveru a naopak.
* Podpora pro škálování na více serverů.

## <a name="how-to-use-it"></a>Způsobu jeho použití

* Nainstalujte [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) balíčku.
* Nakonfigurujte middleware.
* Přijímal požadavky protokolu WebSocket.
* Odesílat a přijímat zprávy.

### <a name="configure-the-middleware"></a>Konfiguraci middlewaru

Přidat objekty WebSockets middleware v `Configure` metodu `Startup` třídy:

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Můžete nakonfigurovat následující nastavení:

* `KeepAliveInterval` – Jak často se bude odesílat rámce "ping" klientovi zajistit proxy nechat otevřené připojení.
* `ReceiveBufferSize` -Velikost vyrovnávací paměti používané přijímat data. Pokročilí uživatelé možná muset změnit na optimalizace na základě velikosti dat výkonu.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Přijímání požadavků protokolu WebSocket

Někde dole v životním cyklu žádosti (dále v `Configure` metoda nebo v MVC akce, např.) zkontrolujte, zda se jedná o žádost protokolu WebSocket a přijměte žádost protokolu WebSocket.

V následujícím příkladu je z později v `Configure` metoda:

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Žádost protokolu WebSocket může dojít na libovolnou adresu URL, ale tento ukázkový kód přijímá pouze požadavky na `/ws`.

### <a name="send-and-receive-messages"></a>Odesílat a přijímat zprávy

`AcceptWebSocketAsync` Metoda upgradu připojení TCP na připojení protokolu WebSocket a poskytuje [WebSocket](/dotnet/core/api/system.net.websockets.websocket) objektu. Použití `WebSocket` objekt, který chcete odesílat a přijímat zprávy.

Kód zobrazí dříve, který přijme žádost protokolu WebSocket předá `WebSocket` do objektu `Echo` metoda. Kód přijme nějakou zprávu a ihned odešle zpět stejnou zprávu. Zpráv odesílaných i přijímaných ve smyčce, dokud klient uzavře připojení:

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Když přijímá připojení protokolu WebSocket před zahájením smyčky, skončí middlewaru v řadě. Po zavření soket, unwinds kanálu. To znamená požadavek se zastaví postoupíte v kanálu, když je přijímán objektu WebSocket. Po dokončení smyčky a zavření soket, žádost pokračuje zálohování kanálu.

## <a name="iisiis-express-support"></a>Podpora služby IIS/IIS Express

Windows Server 2012 nebo novější a Windows 8 nebo novější s služby IIS/IIS Express 8 nebo novější má podporu protokolu WebSocket.

Povolení podpory pro protokol WebSocket v systému Windows Server 2012 nebo novějším:

1. Použití **přidat role a funkce** průvodce z **spravovat** nabídky nebo na odkaz v **správce serveru**.
1. Vyberte **instalace na základě rolí nebo na základě funkcí**. Vyberte **Další**.
1. Vyberte příslušný server (ve výchozím nastavení se vybere místní server). Vyberte **Další**.
1. Rozbalte položku **webového serveru (IIS)** v **role** stromu, rozbalte položku **Webový Server**a potom rozbalte **vývoj aplikací**.
1. Vyberte **protokol WebSocket**. Vyberte **Další**.
1. Pokud nejsou potřeba další funkce, vyberte **Další**.
1. Vyberte **nainstalovat**.
1. Po dokončení instalace, vyberte **Zavřít** ukončíte průvodce.

Povolení podpory pro protokol WebSocket v systému Windows 8 nebo novější:

1. Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky).
1. Otevřete následující uzly: **Internetová informační služba** > **webové služby** > **funkce pro vývoj aplikací**.
1. Vyberte **protokol WebSocket** funkce. Vyberte **OK**.

**Zakázat protokol WebSocket, když pomocí socket.io na node.js**

Pokud používáte podporu protokolu WebSocket v [socket.io](https://socket.io/) na [Node.js](https://nodejs.org/), zakažte pomocí modulu IIS WebSocket výchozí `webSocket` element v *web.config* nebo *applicationHost.config*. Pokud není tento krok provést, pokusí se modulu protokolu WebSocket IIS zpracovávat komunikaci protokolu WebSocket místo Node.js a aplikace.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Další kroky

[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) Tento doprovodný článek je aplikace odezvu. Má na webové stránce, který slouží k propojení protokolu WebSocket a server znovu odešle všechny zprávy, které obdrží zpět do klienta. Spuštění aplikace z příkazového řádku (není zřídila ke spuštění ze sady Visual Studio službou IIS Express) a přejděte do http://localhost:5000. Webová stránka zobrazuje stav připojení v levé horní části:

![Počáteční stav webové stránky](websockets/_static/start.png)

Vyberte **Connect** na adresu URL v poslat žádost protokolu WebSocket. Zadejte testovací zprávu a vyberte **odeslat**. Až budete hotoví, vyberte **zavřít soketu**. **Komunikaci protokolu** části sestavy každou otevřená, odeslání a dochází k němu zavřít akci, jako je.

![Počáteční stav webové stránky](websockets/_static/end.png)
