---
title: ASP.NET Core SignalR JavaScript klienta
author: tdykstra
description: Přehled ASP.NET Core SignalR JavaScript klienta.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: c13c41b0344b0c880e842f2799d6ee97bd7fff7e
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095421"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript klienta

Podle [Rachel Appel](http://twitter.com/rachelappel)

Klientská knihovna ASP.NET Core SignalR JavaScript umožňuje vývojářům volat kód rozbočovače na straně serveru.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Instalace balíčku pro klienta SignalR

Klientské knihovny SignalR JavaScript je dodávána jako [npm](https://www.npmjs.com/) balíčku. Pokud používáte Visual Studio, spusťte `npm install` z **Konzola správce balíčků** během činnosti v kořenové složce. Visual Studio Code, spusťte příkaz z **integrovaný terminál**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

Npm nainstaluje balíček obsahu *node_modules\\ @aspnet\signalr\dist\browser*  složky. Vytvořte novou složku s názvem *signalr* pod *wwwroot\\lib* složky. Kopírovat *signalr.js* do souboru *wwwroot\lib\signalr* složky.

## <a name="use-the-signalr-javascript-client"></a>Použití klienta SignalR JavaScript

Odkazovat na klientovi SignalR JavaScript v `<script>` elementu.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Připojení k rozbočovači

Následující kód vytvoří a spustí připojení. Název centra se nerozlišují malá a velká písmena.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>Nepůvodního zdroje připojení

Obvykle prohlížeče načíst připojení ve stejné doméně jako požadovanou stránku. Existují však situace, kdy je vyžadováno připojení k jiné doméně.

Aby se zabránilo škodlivým webům ve čtení citlivých dat z jiné lokality [nepůvodního zdroje připojení](xref:security/cors) jsou ve výchozím nastavení zakázané. Žádosti nepůvodního zdroje, povolit jej `Startup` třídy.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Volání metod rozbočovače na z klienta

Klientům JavaScript volat veřejné metody v centrech podle pomocí `connection.invoke`. `invoke` Metoda přijímá dva argumenty:

* Název metody rozbočovače. V následujícím příkladu je název centra `SendMessage`.
* Všechny argumenty podle metody rozbočovače. V následujícím příkladu je název argumentu `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Volání metody klienta od rozbočovače

Chcete-li přijímat zprávy z centra, definovat metodu pomocí `connection.on` metody.

* Název metody jazyka JavaScript. V následujícím příkladu je název metody `ReceiveMessage`.
* Argumenty, které se předá metodě rozbočovače. V následujícím příkladu je hodnota argumentu `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Předchozí kód v `connection.on` spustí, když kód na straně serveru pomocí volání `SendAsync` metoda.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR Určuje, jakou metodu klienta volat to provede spárováním odpovídajících názvu metody a argumenty definovaných v `SendAsync` a `connection.on`.

> [!NOTE]
> Jako osvědčený postup volání `connection.start` po `connection.on` tak vaší obslužné rutiny jsou registrovány předtím, než jsou přijaty žádné zprávy.

## <a name="error-handling-and-logging"></a>Protokolování a zpracování chyb

Řetězce `catch` metoda na konec objektu `connection.start` metodu ke zpracování chyby na straně klienta. Použití `console.error` chyby výstup do konzoly prohlížeče.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Nastavení na straně klienta protokolu trasování předáním protokolovací nástroj a typ události do protokolu, když se připojení. Zprávy jsou zaznamenány na úrovni zadaný protokol a vyšší. Dostupné úrovně jsou následující:

* `signalR.LogLevel.Error` : Chybové zprávy. Protokoly `Error` pouze zprávy.
* `signalR.LogLevel.Warning` : Zprávy s upozorněním potenciální chyby. Protokoly `Warning`, a `Error` zprávy.
* `signalR.LogLevel.Information` : Stavových zpráv bez chyb. Protokoly `Information`, `Warning`, a `Error` zprávy.
* `signalR.LogLevel.Trace` : Trasovací zprávy. Protokoly vše, včetně dat přenášených mezi centrem a klienta.

Použití `configureLogging` metodu na `HubConnectionBuilder` nakonfigurovat úroveň protokolu. Zprávy jsou protokolovány do konzoly prohlížeče.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>Související prostředky

* [Centra](xref:signalr/hubs)
* [Klient .NET](xref:signalr/dotnet-client)
* [Publikování do Azure](xref:signalr/publish-to-azure-web-app)
* [Povolení žádostí napříč zdroji (CORS) v ASP.NET Core](xref:security/cors)
