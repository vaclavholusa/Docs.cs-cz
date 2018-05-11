---
title: ASP.NET SignalR JavaScript jádra klienta
author: rachelappel
description: Přehled technologie ASP.NET Core SignalR JavaScript klienta.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/09/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: 1701d9ac5222bf64f9690c1cecdf54ef95fe4a49
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET SignalR JavaScript jádra klienta

Podle [Rachel Appel](http://twitter.com/rachelappel)

Knihovny ASP.NET Core SignalR JavaScript klienta umožňuje vývojářům volat kód rozbočovače na straně serveru.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Nainstalovat balíček klienta SignalR

Klientská knihovna SignalR JavaScript je dodávána jako [npm](https://www.npmjs.com/) balíčku. Pokud používáte Visual Studio, spusťte `npm install` z **Konzola správce balíčků** při v kořenové složce. Pro Visual Studio Code, spusťte příkaz z **integrované Terminálové**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

Nainstaluje obsah balíčku Npm *node_modules\\ @aspnet\signalr\dist\browser*  složky. Vytvořte novou složku s názvem *signalr* pod *wwwroot\\lib* složky. Kopírování *signalr.js* do souboru *wwwroot\lib\signalr* složky.

## <a name="use-the-signalr-javascript-client"></a>Používání klienta SignalR JavaScript

Referenční klienta SignalR JavaScript v `<script>` elementu.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Připojení k rozbočovači

Následující kód vytvoří a spustí připojení. Název rozbočovače na je malá a velká písmena.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>Připojení mezi zdroji

Obvykle prohlížeče načíst připojení ze stejné domény jako k požadované stránce. Existují však situace, když je nutné připojení do jiné domény.

Čtení citlivá data z jiné lokality, aby škodlivé weby [připojení mezi zdroji](xref:security/cors) jsou ve výchozím nastavení zakázané. Žádost o cross-origin, povolit jej do `Startup` třídy.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Volání metody rozbočovače z klienta

Klientům JavaScript volat veřejné metody pro centra pomocí pomocí `connection.invoke`. `invoke` Metoda přijímá dva argumenty:

* Název metody rozbočovače. V následujícím příkladu je název centra `SendMessage`.
* Všechny argumenty definované v metodě rozbočovače. V následujícím příkladu je argument název `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Volání metody klienta od rozbočovače

Chcete-li přijímat zprávy z centra, definovat metoda pomocí `connection.on` metoda.

* Název metody JavaScript klienta. V následujícím příkladu je název metody `ReceiveMessage`.
* Argumenty rozbočovače předá metodě. V následujícím příkladu je hodnota argumentu `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Předchozí kód v `connection.on` se spustí v případě, že volá kódu na straně serveru pomocí `SendAsync` metoda.

[!code-javascript[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR Určuje, jaké metody odpovídající název metody pro volání a argumenty definované v `SendAsync` a `connection.on`.

> [!NOTE]
> Jako osvědčený postup volání `connection.start` po `connection.on` tak vaší obslužné rutiny jsou zaregistrované předtím, než jsou přijaty žádné zprávy.

## <a name="error-handling-and-logging"></a>Protokolování a zpracování chyb

Řetězec `catch` metoda na konec `connection.start` metoda se budou zpracovávat chyby na straně klienta. Použití `console.error` chyby výstup do konzoly prohlížeče.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Instalační program klienta protokolu trasování předáním protokolovacího nástroje a typ události do protokolu po navázání připojení. Zprávy jsou protokolovány s úrovní zadaného protokolu a vyšší. Úrovně protokolu k dispozici jsou následující:

* `signalR.LogLevel.Error` : Chybové zprávy. Protokoly `Error` pouze zprávy.
* `signalR.LogLevel.Warning` : Zprávy s upozorněním potenciální chyby. Protokoly `Warning`, a `Error` zprávy.
* `signalR.LogLevel.Information` : Stavové zprávy bez chyb. Protokoly `Information`, `Warning`, a `Error` zprávy.
* `signalR.LogLevel.Trace` : Trasování zpráv. Zaznamená vše, včetně dat přenášených mezi rozbočovače a klienta.

Použití `configureLogging` metodu `HubConnectionBuilder` nakonfigurovat úroveň protokolu. Zprávy jsou protokolovány v konzoli prohlížeče.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>Související informační zdroje

* [Rozbočovače SignalR technologie ASP.NET Core](xref:signalr/hubs)
* [Povolení žádostí napříč zdroji (CORS) v ASP.NET Core](xref:security/cors)