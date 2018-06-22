---
title: ASP.NET SignalR JavaScript jádra klienta
author: rachelappel
description: Přehled technologie ASP.NET Core SignalR JavaScript klienta.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: 9ea12dc21abfef6d2e6f3fcc455d8aa58ecaa011
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271941"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="7ff21-103">ASP.NET SignalR JavaScript jádra klienta</span><span class="sxs-lookup"><span data-stu-id="7ff21-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="7ff21-104">Podle [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="7ff21-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="7ff21-105">Knihovny ASP.NET Core SignalR JavaScript klienta umožňuje vývojářům volat kód rozbočovače na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="7ff21-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="7ff21-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7ff21-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="7ff21-107">Nainstalovat balíček klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="7ff21-107">Install the SignalR client package</span></span>

<span data-ttu-id="7ff21-108">Klientská knihovna SignalR JavaScript je dodávána jako [npm](https://www.npmjs.com/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="7ff21-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="7ff21-109">Pokud používáte Visual Studio, spusťte `npm install` z **Konzola správce balíčků** při v kořenové složce.</span><span class="sxs-lookup"><span data-stu-id="7ff21-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="7ff21-110">Pro Visual Studio Code, spusťte příkaz z **integrované Terminálové**.</span><span class="sxs-lookup"><span data-stu-id="7ff21-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="7ff21-111">Nainstaluje obsah balíčku Npm *node_modules\\ @aspnet\signalr\dist\browser*  složky.</span><span class="sxs-lookup"><span data-stu-id="7ff21-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="7ff21-112">Vytvořte novou složku s názvem *signalr* pod *wwwroot\\lib* složky.</span><span class="sxs-lookup"><span data-stu-id="7ff21-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="7ff21-113">Kopírování *signalr.js* do souboru *wwwroot\lib\signalr* složky.</span><span class="sxs-lookup"><span data-stu-id="7ff21-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="7ff21-114">Používání klienta SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="7ff21-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="7ff21-115">Referenční klienta SignalR JavaScript v `<script>` elementu.</span><span class="sxs-lookup"><span data-stu-id="7ff21-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="7ff21-116">Připojení k rozbočovači</span><span class="sxs-lookup"><span data-stu-id="7ff21-116">Connect to a hub</span></span>

<span data-ttu-id="7ff21-117">Následující kód vytvoří a spustí připojení.</span><span class="sxs-lookup"><span data-stu-id="7ff21-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="7ff21-118">Název rozbočovače na je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="7ff21-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="7ff21-119">Připojení mezi zdroji</span><span class="sxs-lookup"><span data-stu-id="7ff21-119">Cross-origin connections</span></span>

<span data-ttu-id="7ff21-120">Obvykle prohlížeče načíst připojení ze stejné domény jako k požadované stránce.</span><span class="sxs-lookup"><span data-stu-id="7ff21-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="7ff21-121">Existují však situace, když je nutné připojení do jiné domény.</span><span class="sxs-lookup"><span data-stu-id="7ff21-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="7ff21-122">Čtení citlivá data z jiné lokality, aby škodlivé weby [připojení mezi zdroji](xref:security/cors) jsou ve výchozím nastavení zakázané.</span><span class="sxs-lookup"><span data-stu-id="7ff21-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="7ff21-123">Žádost o cross-origin, povolit jej do `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="7ff21-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="7ff21-124">Volání metody rozbočovače z klienta</span><span class="sxs-lookup"><span data-stu-id="7ff21-124">Call hub methods from client</span></span>

<span data-ttu-id="7ff21-125">Klientům JavaScript volat veřejné metody pro centra pomocí pomocí `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="7ff21-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="7ff21-126">`invoke` Metoda přijímá dva argumenty:</span><span class="sxs-lookup"><span data-stu-id="7ff21-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="7ff21-127">Název metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="7ff21-127">The name of the hub method.</span></span> <span data-ttu-id="7ff21-128">V následujícím příkladu je název centra `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="7ff21-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="7ff21-129">Všechny argumenty definované v metodě rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="7ff21-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="7ff21-130">V následujícím příkladu je argument název `message`.</span><span class="sxs-lookup"><span data-stu-id="7ff21-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="7ff21-131">Volání metody klienta od rozbočovače</span><span class="sxs-lookup"><span data-stu-id="7ff21-131">Call client methods from hub</span></span>

<span data-ttu-id="7ff21-132">Chcete-li přijímat zprávy z centra, definovat metoda pomocí `connection.on` metoda.</span><span class="sxs-lookup"><span data-stu-id="7ff21-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="7ff21-133">Název metody JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="7ff21-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="7ff21-134">V následujícím příkladu je název metody `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="7ff21-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="7ff21-135">Argumenty rozbočovače předá metodě.</span><span class="sxs-lookup"><span data-stu-id="7ff21-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="7ff21-136">V následujícím příkladu je hodnota argumentu `message`.</span><span class="sxs-lookup"><span data-stu-id="7ff21-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="7ff21-137">Předchozí kód v `connection.on` se spustí v případě, že volá kódu na straně serveru pomocí `SendAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="7ff21-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="7ff21-138">SignalR Určuje, jaké metody odpovídající název metody pro volání a argumenty definované v `SendAsync` a `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="7ff21-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="7ff21-139">Jako osvědčený postup volání `connection.start` po `connection.on` tak vaší obslužné rutiny jsou zaregistrované předtím, než jsou přijaty žádné zprávy.</span><span class="sxs-lookup"><span data-stu-id="7ff21-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="7ff21-140">Protokolování a zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="7ff21-140">Error handling and logging</span></span>

<span data-ttu-id="7ff21-141">Řetězec `catch` metoda na konec `connection.start` metoda se budou zpracovávat chyby na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="7ff21-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="7ff21-142">Použití `console.error` chyby výstup do konzoly prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7ff21-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="7ff21-143">Instalační program klienta protokolu trasování předáním protokolovacího nástroje a typ události do protokolu po navázání připojení.</span><span class="sxs-lookup"><span data-stu-id="7ff21-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="7ff21-144">Zprávy jsou protokolovány s úrovní zadaného protokolu a vyšší.</span><span class="sxs-lookup"><span data-stu-id="7ff21-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="7ff21-145">Úrovně protokolu k dispozici jsou následující:</span><span class="sxs-lookup"><span data-stu-id="7ff21-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="7ff21-146">`signalR.LogLevel.Error` : Chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="7ff21-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="7ff21-147">Protokoly `Error` pouze zprávy.</span><span class="sxs-lookup"><span data-stu-id="7ff21-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="7ff21-148">`signalR.LogLevel.Warning` : Zprávy s upozorněním potenciální chyby.</span><span class="sxs-lookup"><span data-stu-id="7ff21-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="7ff21-149">Protokoly `Warning`, a `Error` zprávy.</span><span class="sxs-lookup"><span data-stu-id="7ff21-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="7ff21-150">`signalR.LogLevel.Information` : Stavové zprávy bez chyb.</span><span class="sxs-lookup"><span data-stu-id="7ff21-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="7ff21-151">Protokoly `Information`, `Warning`, a `Error` zprávy.</span><span class="sxs-lookup"><span data-stu-id="7ff21-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="7ff21-152">`signalR.LogLevel.Trace` : Trasování zpráv.</span><span class="sxs-lookup"><span data-stu-id="7ff21-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="7ff21-153">Zaznamená vše, včetně dat přenášených mezi rozbočovače a klienta.</span><span class="sxs-lookup"><span data-stu-id="7ff21-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="7ff21-154">Použití `configureLogging` metodu `HubConnectionBuilder` nakonfigurovat úroveň protokolu.</span><span class="sxs-lookup"><span data-stu-id="7ff21-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="7ff21-155">Zprávy jsou protokolovány v konzoli prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7ff21-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="7ff21-156">Související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="7ff21-156">Related resources</span></span>

* [<span data-ttu-id="7ff21-157">Centra</span><span class="sxs-lookup"><span data-stu-id="7ff21-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="7ff21-158">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="7ff21-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="7ff21-159">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="7ff21-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="7ff21-160">Povolení žádostí napříč zdroji (CORS) v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ff21-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)