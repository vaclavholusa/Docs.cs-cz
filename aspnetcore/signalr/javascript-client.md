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
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="7c387-103">ASP.NET Core SignalR JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="7c387-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="7c387-104">Podle [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="7c387-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="7c387-105">Klientská knihovna ASP.NET Core SignalR JavaScript umožňuje vývojářům volat kód rozbočovače na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="7c387-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="7c387-106">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7c387-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="7c387-107">Instalace balíčku pro klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="7c387-107">Install the SignalR client package</span></span>

<span data-ttu-id="7c387-108">Klientské knihovny SignalR JavaScript je dodávána jako [npm](https://www.npmjs.com/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="7c387-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="7c387-109">Pokud používáte Visual Studio, spusťte `npm install` z **Konzola správce balíčků** během činnosti v kořenové složce.</span><span class="sxs-lookup"><span data-stu-id="7c387-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="7c387-110">Visual Studio Code, spusťte příkaz z **integrovaný terminál**.</span><span class="sxs-lookup"><span data-stu-id="7c387-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="7c387-111">Npm nainstaluje balíček obsahu *node_modules\\ @aspnet\signalr\dist\browser*  složky.</span><span class="sxs-lookup"><span data-stu-id="7c387-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="7c387-112">Vytvořte novou složku s názvem *signalr* pod *wwwroot\\lib* složky.</span><span class="sxs-lookup"><span data-stu-id="7c387-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="7c387-113">Kopírovat *signalr.js* do souboru *wwwroot\lib\signalr* složky.</span><span class="sxs-lookup"><span data-stu-id="7c387-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="7c387-114">Použití klienta SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="7c387-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="7c387-115">Odkazovat na klientovi SignalR JavaScript v `<script>` elementu.</span><span class="sxs-lookup"><span data-stu-id="7c387-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="7c387-116">Připojení k rozbočovači</span><span class="sxs-lookup"><span data-stu-id="7c387-116">Connect to a hub</span></span>

<span data-ttu-id="7c387-117">Následující kód vytvoří a spustí připojení.</span><span class="sxs-lookup"><span data-stu-id="7c387-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="7c387-118">Název centra se nerozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="7c387-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="7c387-119">Nepůvodního zdroje připojení</span><span class="sxs-lookup"><span data-stu-id="7c387-119">Cross-origin connections</span></span>

<span data-ttu-id="7c387-120">Obvykle prohlížeče načíst připojení ve stejné doméně jako požadovanou stránku.</span><span class="sxs-lookup"><span data-stu-id="7c387-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="7c387-121">Existují však situace, kdy je vyžadováno připojení k jiné doméně.</span><span class="sxs-lookup"><span data-stu-id="7c387-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="7c387-122">Aby se zabránilo škodlivým webům ve čtení citlivých dat z jiné lokality [nepůvodního zdroje připojení](xref:security/cors) jsou ve výchozím nastavení zakázané.</span><span class="sxs-lookup"><span data-stu-id="7c387-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="7c387-123">Žádosti nepůvodního zdroje, povolit jej `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="7c387-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="7c387-124">Volání metod rozbočovače na z klienta</span><span class="sxs-lookup"><span data-stu-id="7c387-124">Call hub methods from client</span></span>

<span data-ttu-id="7c387-125">Klientům JavaScript volat veřejné metody v centrech podle pomocí `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="7c387-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="7c387-126">`invoke` Metoda přijímá dva argumenty:</span><span class="sxs-lookup"><span data-stu-id="7c387-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="7c387-127">Název metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="7c387-127">The name of the hub method.</span></span> <span data-ttu-id="7c387-128">V následujícím příkladu je název centra `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="7c387-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="7c387-129">Všechny argumenty podle metody rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="7c387-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="7c387-130">V následujícím příkladu je název argumentu `message`.</span><span class="sxs-lookup"><span data-stu-id="7c387-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="7c387-131">Volání metody klienta od rozbočovače</span><span class="sxs-lookup"><span data-stu-id="7c387-131">Call client methods from hub</span></span>

<span data-ttu-id="7c387-132">Chcete-li přijímat zprávy z centra, definovat metodu pomocí `connection.on` metody.</span><span class="sxs-lookup"><span data-stu-id="7c387-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="7c387-133">Název metody jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c387-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="7c387-134">V následujícím příkladu je název metody `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="7c387-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="7c387-135">Argumenty, které se předá metodě rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="7c387-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="7c387-136">V následujícím příkladu je hodnota argumentu `message`.</span><span class="sxs-lookup"><span data-stu-id="7c387-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="7c387-137">Předchozí kód v `connection.on` spustí, když kód na straně serveru pomocí volání `SendAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="7c387-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="7c387-138">SignalR Určuje, jakou metodu klienta volat to provede spárováním odpovídajících názvu metody a argumenty definovaných v `SendAsync` a `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="7c387-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="7c387-139">Jako osvědčený postup volání `connection.start` po `connection.on` tak vaší obslužné rutiny jsou registrovány předtím, než jsou přijaty žádné zprávy.</span><span class="sxs-lookup"><span data-stu-id="7c387-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="7c387-140">Protokolování a zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="7c387-140">Error handling and logging</span></span>

<span data-ttu-id="7c387-141">Řetězce `catch` metoda na konec objektu `connection.start` metodu ke zpracování chyby na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="7c387-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="7c387-142">Použití `console.error` chyby výstup do konzoly prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7c387-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="7c387-143">Nastavení na straně klienta protokolu trasování předáním protokolovací nástroj a typ události do protokolu, když se připojení.</span><span class="sxs-lookup"><span data-stu-id="7c387-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="7c387-144">Zprávy jsou zaznamenány na úrovni zadaný protokol a vyšší.</span><span class="sxs-lookup"><span data-stu-id="7c387-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="7c387-145">Dostupné úrovně jsou následující:</span><span class="sxs-lookup"><span data-stu-id="7c387-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="7c387-146">`signalR.LogLevel.Error` : Chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="7c387-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="7c387-147">Protokoly `Error` pouze zprávy.</span><span class="sxs-lookup"><span data-stu-id="7c387-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="7c387-148">`signalR.LogLevel.Warning` : Zprávy s upozorněním potenciální chyby.</span><span class="sxs-lookup"><span data-stu-id="7c387-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="7c387-149">Protokoly `Warning`, a `Error` zprávy.</span><span class="sxs-lookup"><span data-stu-id="7c387-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="7c387-150">`signalR.LogLevel.Information` : Stavových zpráv bez chyb.</span><span class="sxs-lookup"><span data-stu-id="7c387-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="7c387-151">Protokoly `Information`, `Warning`, a `Error` zprávy.</span><span class="sxs-lookup"><span data-stu-id="7c387-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="7c387-152">`signalR.LogLevel.Trace` : Trasovací zprávy.</span><span class="sxs-lookup"><span data-stu-id="7c387-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="7c387-153">Protokoly vše, včetně dat přenášených mezi centrem a klienta.</span><span class="sxs-lookup"><span data-stu-id="7c387-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="7c387-154">Použití `configureLogging` metodu na `HubConnectionBuilder` nakonfigurovat úroveň protokolu.</span><span class="sxs-lookup"><span data-stu-id="7c387-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="7c387-155">Zprávy jsou protokolovány do konzoly prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7c387-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="7c387-156">Související prostředky</span><span class="sxs-lookup"><span data-stu-id="7c387-156">Related resources</span></span>

* [<span data-ttu-id="7c387-157">Centra</span><span class="sxs-lookup"><span data-stu-id="7c387-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="7c387-158">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="7c387-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="7c387-159">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="7c387-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="7c387-160">Povolení žádostí napříč zdroji (CORS) v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c387-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
