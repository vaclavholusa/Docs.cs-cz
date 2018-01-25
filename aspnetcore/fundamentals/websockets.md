---
title: Podpora pro objekty WebSockets v ASP.NET Core
author: tdykstra
description: "Zjistěte, jak začít pracovat s objekty WebSockets v ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 6f335376c72cd0c68f4667cf0e661a25bf448980
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>Úvod do technologie WebSockets v ASP.NET Core

Podle [tní Dykstra](https://github.com/tdykstra) a [Andrew Stanton-Nurse](https://github.com/anurse)

Tento článek vysvětluje, jak začít pracovat s objekty WebSockets v ASP.NET Core. [Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP. Používá se pro aplikace, jako je chat, kurzů akcií, hry, vždy, když chcete v reálném čase funkce ve webové aplikaci.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)). Najdete v článku [další kroky](#next-steps) části Další informace.


## <a name="prerequisites"></a>Požadavky

* ASP.NET Core 1.1 (nefunguje v 1.0)
* Všechny operační systém, který ASP.NET Core běží na:
  
  * Windows 7 nebo Windows Server 2008 a novější
  * Linux
  * macOS

* **Výjimka**: Pokud vaše aplikace běží v systému Windows pomocí služby IIS, nebo s WebListener, je nutné použít:

  * Windows 8 nebo Windows Server 2012 nebo novější
  * Služba IIS 8 / Express IIS 8
  * Protokol WebSocket musí být povolena ve službě IIS

* Podporované prohlížeče najdete v části http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Kdy ji použít

Technologie WebSockets použijte, když potřebujete pracovat přímo s připojení soketu. Můžete například potřebovat, aby nejlepší možný výkon za hru v reálném čase.

[Funkce SignalR technologie ASP.NET](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) poskytuje bohatší model aplikace pro funkce v reálném čase, ale funguje pouze na technologii ASP.NET, není ASP.NET Core. Základní verzi SignalR je ve vývoji; postupujte podle jeho průběh, najdete v článku [úložiště GitHub pro SignalR Core](https://github.com/aspnet/SignalR).

Pokud nechcete čekat SignalR Core, můžete objekty WebSockets přímo teď. Ale možná budete muset vyvíjet funkcí, které by poskytují SignalR, jako například:

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

Přidat objekty WebSockets middleware v `Configure` metodu `Startup` třídy.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Můžete nakonfigurovat následující nastavení:

* `KeepAliveInterval`– Jak často k odesílání rámců "ping" na klientovi, ujistěte se, že proxy nechat otevřené připojení.
* `ReceiveBufferSize`-Velikost vyrovnávací paměti používané přijímat data. Jenom Pokročilí uživatelé by muset změnit to, pro optimalizaci výkonu na základě velikosti svá data.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Přijímání požadavků protokolu WebSocket

Někde dole v životním cyklu žádosti (dále v `Configure` metoda nebo v MVC akce, např.) zkontrolujte, zda se jedná o žádost protokolu WebSocket a přijměte žádost protokolu WebSocket.

V tomto příkladu je z později v `Configure` metoda.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Žádost protokolu WebSocket může dojít na libovolnou adresu URL, ale tento ukázkový kód přijímá pouze požadavky na `/ws`.

### <a name="send-and-receive-messages"></a>Odesílat a přijímat zprávy

`AcceptWebSocketAsync` Metoda upgradu připojení TCP na připojení protokolu WebSocket a poskytuje [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) objektu. Pomocí objektu WebSocket odesílat a přijímat zprávy.

Předá kód uvedena výše, který přijme žádost protokolu WebSocket `WebSocket` do objektu `Echo` metoda; zde uvádíme `Echo` metoda. Kód přijme nějakou zprávu a ihned odešle zpět stejnou zprávu. Zůstane ve smyčce učinit dokud klient uzavře připojení. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Když přijmete protokol WebSocket před zahájením této smyčky, skončí middlewaru v řadě.  Po zavření soket, unwinds kanálu. To znamená zastaví požadavek postoupíte v kanálu, když přijímají protokolu WebSocket stejně, jako by při průchodu akce MVC, třeba.  Ale po dokončení této smyčky a zavřete soket, žádost pokračuje zálohování kanálu.

## <a name="next-steps"></a>Další kroky

[Ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) Tento doprovodný článek je jednoduchý odezvu aplikací. Má na webové stránce, který slouží k propojení protokolu WebSocket a server právě znovu odešle klientovi všechny zprávy, které obdrží. Spuštění z příkazového řádku (není zřídila ke spuštění ze sady Visual Studio službou IIS Express) a přejděte na http://localhost: 5000. Webová stránka zobrazuje stav připojení v levém horním:

![Počáteční stav webové stránky](websockets/_static/start.png)

Vyberte **Connect** na adresu URL v poslat žádost protokolu WebSocket.  Zadejte testovací zprávu a vyberte **odeslat**. Až budete hotoví, vyberte **zavřít soketu**. **Komunikaci protokolu** části sestavy každou otevřená, odeslání a dochází k němu zavřít akci, jako je.

![Počáteční stav webové stránky](websockets/_static/end.png)
