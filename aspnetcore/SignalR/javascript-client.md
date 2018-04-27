---
title: ASP.NET SignalR JavaScript jádra klienta
author: rachelappel
description: Přehled technologie ASP.NET Core SignalR JavaScript klienta.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/06/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: cca1a523bd536f4365e54c196f6c9024779d5d76
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="0ef1a-103">ASP.NET SignalR JavaScript jádra klienta</span><span class="sxs-lookup"><span data-stu-id="0ef1a-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="0ef1a-104">Podle [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="0ef1a-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="0ef1a-105">Knihovny ASP.NET Core SignalR JavaScript klienta umožňuje vývojářům volat kód rozbočovače na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="0ef1a-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0ef1a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="0ef1a-107">Nainstalovat balíček klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="0ef1a-107">Install the SignalR client package</span></span>

<span data-ttu-id="0ef1a-108">Klientská knihovna SignalR JavaScript je dodávána jako [npm](https://www.npmjs.com/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="0ef1a-109">Pokud používáte Visual Studio, spusťte `npm install` z **Konzola správce balíčků** při v kořenové složce.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="0ef1a-110">Pro Visual Studio Code, spusťte příkaz z **integrované Terminálové**.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="0ef1a-111">Nainstaluje obsah balíčku Npm *node_modules\\ @aspnet\signalr\dist\browser*  složky.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="0ef1a-112">Vytvořte novou složku s názvem *signalr* pod *wwwroot\\lib* složky.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="0ef1a-113">Kopírování *signalr.js* do souboru *wwwroot\lib\signalr* složky.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="0ef1a-114">Používání klienta SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="0ef1a-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="0ef1a-115">Referenční klienta SignalR JavaScript v `<script>` elementu.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="0ef1a-116">Připojení k rozbočovači</span><span class="sxs-lookup"><span data-stu-id="0ef1a-116">Connect to a hub</span></span>

<span data-ttu-id="0ef1a-117">Následující kód vytvoří a spustí připojení.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="0ef1a-118">Název rozbočovače na je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=1-2,18)]

### <a name="cross-origin-connections"></a><span data-ttu-id="0ef1a-119">Připojení mezi zdroji</span><span class="sxs-lookup"><span data-stu-id="0ef1a-119">Cross-origin connections</span></span>

<span data-ttu-id="0ef1a-120">Obvykle prohlížeče načíst připojení ze stejné domény jako k požadované stránce.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="0ef1a-121">Existují však situace, když je nutné připojení do jiné domény.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="0ef1a-122">Čtení citlivá data z jiné lokality, aby škodlivé weby [připojení mezi zdroji](xref:security/cors) jsou ve výchozím nastavení zakázané.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="0ef1a-123">Žádost o cross-origin, povolit jej do `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-34,55)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="0ef1a-124">Volání metody rozbočovače z klienta</span><span class="sxs-lookup"><span data-stu-id="0ef1a-124">Call hub methods from client</span></span>

<span data-ttu-id="0ef1a-125">Klientům JavaScript volat veřejné metody pro centra pomocí pomocí `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="0ef1a-126">`invoke` Metoda přijímá dva argumenty:</span><span class="sxs-lookup"><span data-stu-id="0ef1a-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="0ef1a-127">Název metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-127">The name of the hub method.</span></span> <span data-ttu-id="0ef1a-128">V následujícím příkladu je název centra `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="0ef1a-129">Všechny argumenty definované v metodě rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="0ef1a-130">V následujícím příkladu je argument název `message`.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=14)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="0ef1a-131">Volání metody klienta od rozbočovače</span><span class="sxs-lookup"><span data-stu-id="0ef1a-131">Call client methods from hub</span></span>

<span data-ttu-id="0ef1a-132">Chcete-li přijímat zprávy z centra, definovat metoda pomocí `connection.on` metoda.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="0ef1a-133">Název metody JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="0ef1a-134">V následujícím příkladu je název metody `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="0ef1a-135">Argumenty rozbočovače předá metodě.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="0ef1a-136">V následujícím příkladu je hodnota argumentu `message`.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=4-9)]

<span data-ttu-id="0ef1a-137">Předchozí kód v `connection.on` se spustí v případě, že volá kódu na straně serveru pomocí `SendAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-javascript[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="0ef1a-138">SignalR Určuje, jaké metody odpovídající název metody pro volání a argumenty definované v `SendAsync` a `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="0ef1a-139">Jako osvědčený postup volání `connection.start` po `connection.on` tak vaší obslužné rutiny jsou zaregistrované předtím, než jsou přijaty žádné zprávy.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="0ef1a-140">Protokolování a zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="0ef1a-140">Error handling and logging</span></span>

<span data-ttu-id="0ef1a-141">Řetězec `catch` metoda na konec `connection.start` metoda se budou zpracovávat chyby na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="0ef1a-142">Použití `console.error` chyby výstup do konzoly prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=18)]

<span data-ttu-id="0ef1a-143">Instalační program klienta protokolu trasování předáním protokolovacího nástroje a typ události do protokolu po navázání připojení.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="0ef1a-144">Zprávy jsou protokolovány s úrovní zadaného protokolu a vyšší.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="0ef1a-145">Úrovně protokolu k dispozici jsou následující:</span><span class="sxs-lookup"><span data-stu-id="0ef1a-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="0ef1a-146">`signalR.LogLevel.Error` : Chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="0ef1a-147">Protokoly `Error` pouze zprávy.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="0ef1a-148">`signalR.LogLevel.Warning` : Zprávy s upozorněním potenciální chyby.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="0ef1a-149">Protokoly `Warning`, a `Error` zprávy.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="0ef1a-150">`signalR.LogLevel.Information` : Stavové zprávy bez chyb.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="0ef1a-151">Protokoly `Information`, `Warning`, a `Error` zprávy.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="0ef1a-152">`signalR.LogLevel.Trace` : Trasování zpráv.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="0ef1a-153">Zaznamená vše, včetně dat přenášených mezi rozbočovače a klienta.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="0ef1a-154">Připojení k protokolování předejte protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-154">Pass the logger to the connection to start logging.</span></span> <span data-ttu-id="0ef1a-155">Nástroje pro vývojáře prohlížeče obvykle obsahují konzoly, která zobrazuje zprávy.</span><span class="sxs-lookup"><span data-stu-id="0ef1a-155">Browser developer tools typically contain a console that displays the messages.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=1-2)]

## <a name="related-resources"></a><span data-ttu-id="0ef1a-156">Související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="0ef1a-156">Related resources</span></span>

* [<span data-ttu-id="0ef1a-157">Rozbočovače SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ef1a-157">ASP.NET Core SignalR Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="0ef1a-158">Povolení žádostí napříč zdroji (CORS) v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ef1a-158">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)