---
title: Webové sockety v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak začít pracovat s objekty Websocket v ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: a9fe13ef7895ea3ab43257dbbaf4521f883c0804
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433984"
---
# <a name="websockets-support-in-aspnet-core"></a>Webové sockety v ASP.NET Core

Podle [Petr Dykstra](https://github.com/tdykstra) a [Andrew Stanton sestry](https://github.com/anurse)

Tento článek vysvětluje, jak začít pracovat s objekty Websocket v ASP.NET Core. [Protokol WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) je protokol, který umožňuje obousměrný trvalé komunikačních kanálů přes připojení TCP. Používá se v aplikacích, které využívají samosprávné komunikace rychlé, v reálném čase, jako jsou konverzace, řídicí panel a herních aplikací.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)). Zobrazit [další kroky](#next-steps) části Další informace.

## <a name="prerequisites"></a>Požadavky

* ASP.NET Core 1.1 nebo vyšší
* Libovolný operační systém, který podporuje ASP.NET Core:
  
  * Windows 7 / Windows Server 2008 nebo novější
  * Linux
  * macOS
  
* Pokud aplikace běží na Windows se službou IIS:

  * Windows 8 nebo Windows Server 2012 nebo novější
  * Služba IIS 8 / 8 služby IIS Express
  * Protokoly Websocket, musí být povolené ve službě IIS (najdete v článku [podpora služby IIS/IIS Express](#iisiis-express-support) části.)
  
* Pokud aplikace běží na [HTTP.sys](xref:fundamentals/servers/httpsys):

  * Windows 8 nebo Windows Server 2012 nebo novější

* Podporované prohlížeče, naleznete v tématu https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>Kdy použít objekty Websocket

Použijte pracovat přímo s připojení soketu Websocket. Například můžete použijte objekty Websocket pro zajištění nejlepšího možného výkonu s hry v reálném čase.

[Funkce SignalR technologie ASP.NET Core](xref:signalr/introduction) je knihovna, která zjednodušuje přidávání funkce webu v reálném čase do aplikací. Používá objekty Websocket, kdykoli je to možné.

## <a name="how-to-use-websockets"></a>Jak používat objekty Websocket

* Nainstalujte [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) balíčku.
* Nakonfigurujte middleware.
* Přijímal požadavky protokolu WebSocket.
* Odesílání a příjem zpráv.

### <a name="configure-the-middleware"></a>Nakonfigurujte middleware

Přidat objekty Websocket middlewaru v `Configure` metodu `Startup` třídy:

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Můžete nakonfigurovat následující nastavení:

* `KeepAliveInterval` – Jak často k odesílání rámců "příkaz ping" ke klientovi, aby proxy servery udržení připojení otevřeného.
* `ReceiveBufferSize` – Velikost vyrovnávací paměti používané přijímat data. Pokročilí uživatelé může být nutné změnit to pro optimalizace na základě velikosti dat výkonu.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Přijímal požadavky protokolu WebSocket

Někde dále v cyklu požadavku (dále v `Configure` metoda nebo akce MVC, například) zkontrolujte, jestli je požadavek protokolu WebSocket a přijmout požadavek protokolu WebSocket.

Následující příklad je z později v `Configure` metody:

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Požadavek protokolu WebSocket může dojít na libovolnou adresu URL, ale tento vzorový kód přijímá jenom žádosti o `/ws`.

### <a name="send-and-receive-messages"></a>Odesílání a příjem zpráv

`AcceptWebSocketAsync` Metoda upgraduje na připojení soketu websocket bylo připojení TCP a poskytuje [protokolu WebSocket](/dotnet/core/api/system.net.websockets.websocket) objektu. Použití `WebSocket` objektu pro odesílání a příjem zpráv.

Předá kódu uvedeného výše, který přijme žádost protokolu WebSocket `WebSocket` objektu `Echo` metody. Kód přijme zprávu a okamžitě odešle zpět stejné zprávy. Zprávy jsou odeslané a přijaté ve smyčce, dokud klient ukončí připojení:

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Při přijímání připojení soketu websocket bylo před zahájením smyčky, middleware kanálu se ukončí. Po zavření soketu, unwinds kanálu. To znamená požadavek se zastaví v budoucnu v kanálu při přijetí objekt WebSocket. Když je smyčka dokončena, je uzavřený soket žádost pokračuje zálohování kanálu.

## <a name="iisiis-express-support"></a>Podpora služby IIS/IIS Express

Windows Server 2012 nebo novější a Windows 8 nebo novější s služby IIS/IIS Express 8 nebo novější obsahuje podporu protokolu WebSocket.

Pokud chcete povolit podporu protokolu WebSocket ve Windows serveru 2012 nebo novější:

1. Použití **přidat role a funkce** z průvodce **spravovat** nabídky nebo na odkaz v **správce serveru**.
1. Vyberte **instalace na základě rolí nebo na základě funkcí**. Vyberte **Další**.
1. Vyberte příslušný server (ve výchozím nastavení je vybraný místní server). Vyberte **Další**.
1. Rozbalte **webového serveru (IIS)** v **role** stromu, rozbalte **Webový Server**a potom rozbalte **vývoj aplikací**.
1. Vyberte **protokol WebSocket**. Vyberte **Další**.
1. Pokud nejsou potřebné další funkce, vyberte **Další**.
1. Vyberte **nainstalovat**.
1. Po dokončení instalace, vybrat **Zavřít** ukončíte průvodce.

Pokud chcete povolit podporu protokolu WebSocket v systému Windows 8 nebo novější:

1. Přejděte do **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky).
1. Otevřete následující uzly: **Internetová informační služba** > **webové služby** > **funkce pro vývoj aplikací**.
1. Vyberte **protokol WebSocket** funkce. Vyberte **OK**.

**Zakázat protokol WebSocket, když v node.js pomocí socket.io**

Pokud používáte podpora protokolu WebSocket v [socket.io](https://socket.io/) na [Node.js](https://nodejs.org/), zakázat pomocí modul WebSocket služby IIS výchozí `webSocket` element v *web.config* nebo *applicationHost.config*. Pokud není tento krok provést, modul WebSocket služby IIS se pokusí se zpracovat komunikaci pomocí protokolu WebSocket spíše než Node.js a aplikace.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Další kroky

[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) , který doprovází tento článek je odezvu aplikace. Obsahuje webovou stránku, která umožňuje připojení pomocí protokolu WebSocket, a server odešle všechny zprávy, které obdrží zpět do klienta. Spustit z příkazového řádku (ne zřídila pro spuštění ze sady Visual Studio službou IIS Express) a přejděte do http://localhost:5000. Webové stránky zobrazí v levém horním rohu stav připojení:

![Počáteční stav webové stránky](websockets/_static/start.png)

Vyberte **připojit** na adresu URL odeslat požadavek protokolu WebSocket. Zadejte testovací zprávu a vybrat **odeslat**. Až budete hotovi, vyberte **zavřít soketu**. **Komunikace protokolu** části sestavy každý otevřený, odesílání a akce při zavření jako to se stane.

![Počáteční stav webové stránky](websockets/_static/end.png)
