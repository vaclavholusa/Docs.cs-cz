---
title: ASP.NET Core SignalR JavaScript klienta
author: tdykstra
description: Přehled ASP.NET Core SignalR JavaScript klienta.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: 10958c414aa4a285c8a2810bb99e278f719c5b7f
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2018
ms.locfileid: "46483046"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="9349c-103">ASP.NET Core SignalR JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="9349c-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="9349c-104">Podle [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="9349c-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="9349c-105">Klientská knihovna ASP.NET Core SignalR JavaScript umožňuje vývojářům volat kód rozbočovače na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="9349c-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="9349c-106">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9349c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="9349c-107">Instalace balíčku pro klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="9349c-107">Install the SignalR client package</span></span>

<span data-ttu-id="9349c-108">Klientské knihovny SignalR JavaScript je dodávána jako [npm](https://www.npmjs.com/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="9349c-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="9349c-109">Pokud používáte Visual Studio, spusťte `npm install` z **Konzola správce balíčků** během činnosti v kořenové složce.</span><span class="sxs-lookup"><span data-stu-id="9349c-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="9349c-110">Visual Studio Code, spusťte příkaz z **integrovaný terminál**.</span><span class="sxs-lookup"><span data-stu-id="9349c-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="9349c-111">npm nainstaluje balíček obsahu *node_modules\\@aspnet\signalr\dist\browser* složky.</span><span class="sxs-lookup"><span data-stu-id="9349c-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="9349c-112">Vytvořte novou složku s názvem *signalr* pod *wwwroot\\lib* složky.</span><span class="sxs-lookup"><span data-stu-id="9349c-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="9349c-113">Kopírovat *signalr.js* do souboru *wwwroot\lib\signalr* složky.</span><span class="sxs-lookup"><span data-stu-id="9349c-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="9349c-114">Použití klienta SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="9349c-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="9349c-115">Odkazovat na klientovi SignalR JavaScript v `<script>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9349c-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="9349c-116">Připojení k rozbočovači</span><span class="sxs-lookup"><span data-stu-id="9349c-116">Connect to a hub</span></span>

<span data-ttu-id="9349c-117">Následující kód vytvoří a spustí připojení.</span><span class="sxs-lookup"><span data-stu-id="9349c-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="9349c-118">Název centra se nerozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="9349c-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="9349c-119">Nepůvodního zdroje připojení</span><span class="sxs-lookup"><span data-stu-id="9349c-119">Cross-origin connections</span></span>

<span data-ttu-id="9349c-120">Obvykle prohlížeče načíst připojení ve stejné doméně jako požadovanou stránku.</span><span class="sxs-lookup"><span data-stu-id="9349c-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="9349c-121">Existují však situace, kdy je vyžadováno připojení k jiné doméně.</span><span class="sxs-lookup"><span data-stu-id="9349c-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="9349c-122">Aby se zabránilo škodlivým webům ve čtení citlivých dat z jiné lokality [nepůvodního zdroje připojení](xref:security/cors) jsou ve výchozím nastavení zakázané.</span><span class="sxs-lookup"><span data-stu-id="9349c-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="9349c-123">Žádosti nepůvodního zdroje, povolit jej `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="9349c-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="9349c-124">Volání metod rozbočovače na z klienta</span><span class="sxs-lookup"><span data-stu-id="9349c-124">Call hub methods from client</span></span>

<span data-ttu-id="9349c-125">Klientům JavaScript volat veřejné metody rozbočovače prostřednictvím [vyvolat](/javascript/api/%40aspnet/signalr/hubconnection#invoke) metodu [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="9349c-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="9349c-126">`invoke` Metoda přijímá dva argumenty:</span><span class="sxs-lookup"><span data-stu-id="9349c-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="9349c-127">Název metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9349c-127">The name of the hub method.</span></span> <span data-ttu-id="9349c-128">V následujícím příkladu je název metody rozbočovače `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="9349c-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="9349c-129">Všechny argumenty podle metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9349c-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="9349c-130">V následujícím příkladu je název argumentu `message`.</span><span class="sxs-lookup"><span data-stu-id="9349c-130">In the following example, the argument name is `message`.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="9349c-131">Volání metody klienta od rozbočovače</span><span class="sxs-lookup"><span data-stu-id="9349c-131">Call client methods from hub</span></span>

<span data-ttu-id="9349c-132">Chcete-li přijímat zprávy z centra, definovat metodu pomocí [na](/javascript/api/%40aspnet/signalr/hubconnection#on) metodu `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="9349c-132">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="9349c-133">Název metody jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9349c-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="9349c-134">V následujícím příkladu je název metody `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="9349c-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="9349c-135">Argumenty, které se předá metodě rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9349c-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="9349c-136">V následujícím příkladu je hodnota argumentu `message`.</span><span class="sxs-lookup"><span data-stu-id="9349c-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="9349c-137">Předchozí kód v `connection.on` spustí, když kód na straně serveru pomocí volání [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metoda.</span><span class="sxs-lookup"><span data-stu-id="9349c-137">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="9349c-138">SignalR Určuje, jakou metodu klienta volat to provede spárováním odpovídajících názvu metody a argumenty definovaných v `SendAsync` a `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="9349c-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="9349c-139">Jako osvědčený postup volání [start](/javascript/api/%40aspnet/signalr/hubconnection#start) metodu `HubConnection` po `on`.</span><span class="sxs-lookup"><span data-stu-id="9349c-139">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="9349c-140">Tím se zajistí, že vaše obslužné rutiny jsou registrovány předtím, než jsou přijaty žádné zprávy.</span><span class="sxs-lookup"><span data-stu-id="9349c-140">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="9349c-141">Protokolování a zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="9349c-141">Error handling and logging</span></span>

<span data-ttu-id="9349c-142">Řetězce `catch` metoda na konec objektu `start` metodu ke zpracování chyby na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="9349c-142">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="9349c-143">Použití `console.error` chyby výstup do konzoly prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9349c-143">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="9349c-144">Nastavení na straně klienta protokolu trasování předáním protokolovací nástroj a typ události do protokolu, když se připojení.</span><span class="sxs-lookup"><span data-stu-id="9349c-144">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="9349c-145">Zprávy jsou zaznamenány na úrovni zadaný protokol a vyšší.</span><span class="sxs-lookup"><span data-stu-id="9349c-145">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="9349c-146">Dostupné úrovně jsou následující:</span><span class="sxs-lookup"><span data-stu-id="9349c-146">Available log levels are as follows:</span></span>

* <span data-ttu-id="9349c-147">`signalR.LogLevel.Error` &ndash; Chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="9349c-147">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="9349c-148">Protokoly `Error` pouze zprávy.</span><span class="sxs-lookup"><span data-stu-id="9349c-148">Logs `Error` messages only.</span></span>
* <span data-ttu-id="9349c-149">`signalR.LogLevel.Warning` &ndash; Zprávy s upozorněním potenciální chyby.</span><span class="sxs-lookup"><span data-stu-id="9349c-149">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="9349c-150">Protokoly `Warning`, a `Error` zprávy.</span><span class="sxs-lookup"><span data-stu-id="9349c-150">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="9349c-151">`signalR.LogLevel.Information` &ndash; Stavové zprávy bez chyb.</span><span class="sxs-lookup"><span data-stu-id="9349c-151">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="9349c-152">Protokoly `Information`, `Warning`, a `Error` zprávy.</span><span class="sxs-lookup"><span data-stu-id="9349c-152">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="9349c-153">`signalR.LogLevel.Trace` &ndash; Trasovací zprávy.</span><span class="sxs-lookup"><span data-stu-id="9349c-153">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="9349c-154">Protokoly vše, včetně dat přenášených mezi centrem a klienta.</span><span class="sxs-lookup"><span data-stu-id="9349c-154">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="9349c-155">Použití [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metoda [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) nakonfigurovat úroveň protokolu.</span><span class="sxs-lookup"><span data-stu-id="9349c-155">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="9349c-156">Zprávy jsou protokolovány do konzoly prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9349c-156">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="additional-resources"></a><span data-ttu-id="9349c-157">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9349c-157">Additional resources</span></span>

* [<span data-ttu-id="9349c-158">Reference k rozhraní API jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="9349c-158">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="9349c-159">Centra</span><span class="sxs-lookup"><span data-stu-id="9349c-159">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="9349c-160">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="9349c-160">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="9349c-161">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="9349c-161">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="9349c-162">Povolení žádostí napříč zdroji (CORS) v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9349c-162">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
