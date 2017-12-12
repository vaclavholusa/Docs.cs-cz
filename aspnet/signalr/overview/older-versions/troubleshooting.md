---
uid: signalr/overview/older-versions/troubleshooting
title: "Řešení potíží s SignalR (SignalR 1.x) | Microsoft Docs"
author: pfletcher
description: "Tento článek popisuje běžné problémy s vývojem aplikací SignalR."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: eb739f806eb57d09008fa0bf04b5a40721dab73d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="signalr-troubleshooting-signalr-1x"></a><span data-ttu-id="09408-103">Řešení potíží s SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="09408-103">SignalR Troubleshooting (SignalR 1.x)</span></span>
====================
<span data-ttu-id="09408-104">podle [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="09408-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="09408-105">Tento dokument popisuje řešení běžných problémů s SignalR.</span><span class="sxs-lookup"><span data-stu-id="09408-105">This document describes common troubleshooting issues with SignalR.</span></span>


<span data-ttu-id="09408-106">Tento dokument obsahuje následující části.</span><span class="sxs-lookup"><span data-stu-id="09408-106">This document contains the following sections.</span></span>

- [<span data-ttu-id="09408-107">Volání metody mezi klientem a serverem bez upozornění selže</span><span class="sxs-lookup"><span data-stu-id="09408-107">Calling methods between the client and server silently fails</span></span>](#connection)
- [<span data-ttu-id="09408-108">Jiné potíže s připojením</span><span class="sxs-lookup"><span data-stu-id="09408-108">Other connection issues</span></span>](#other)
- [<span data-ttu-id="09408-109">Chyby kompilace a na straně serveru</span><span class="sxs-lookup"><span data-stu-id="09408-109">Compilation and server-side errors</span></span>](#server)
- [<span data-ttu-id="09408-110">Problémy v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09408-110">Visual Studio issues</span></span>](#vs)
- [<span data-ttu-id="09408-111">Internetová informační služba problémy</span><span class="sxs-lookup"><span data-stu-id="09408-111">Internet Information Services issues</span></span>](#iis)
- [<span data-ttu-id="09408-112">Problémy s Azure</span><span class="sxs-lookup"><span data-stu-id="09408-112">Azure issues</span></span>](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a><span data-ttu-id="09408-113">Volání metody mezi klientem a serverem bez upozornění selže</span><span class="sxs-lookup"><span data-stu-id="09408-113">Calling methods between the client and server silently fails</span></span>

<span data-ttu-id="09408-114">Tato část popisuje možné příčiny pro volání metody mezi klientem a serverem smysluplné chybové zprávy v případě selhání.</span><span class="sxs-lookup"><span data-stu-id="09408-114">This section describes possible causes for a method call between client and server to fail without a meaningful error message.</span></span> <span data-ttu-id="09408-115">V aplikaci SignalR server nemá žádné informace o metody, které klient implementuje; Když server volá metodu klienta, metoda název a parametru data se odesílají do klienta a tuto metodu je spustit pouze v případě, že existuje ve formátu, který zadaný server.</span><span class="sxs-lookup"><span data-stu-id="09408-115">In a SignalR application, the server has no information about the methods that the client implements; when the server invokes a client method, the method name and parameter data are sent to the client, and the method is executed only if it exists in the format that the server specified.</span></span> <span data-ttu-id="09408-116">Pokud je nalezena žádná odpovídající metoda na klientovi, nedojde k žádné akci, a žádná chybová zpráva se vyvolá na serveru.</span><span class="sxs-lookup"><span data-stu-id="09408-116">If no matching method is found on the client, nothing happens, and no error message is raised on the server.</span></span>

<span data-ttu-id="09408-117">K hlubšímu prošetření aplikace volá metody klienta, můžete zapnout protokolování před voláním metody start na rozbočovači zobrazíte jaké volání pocházejí ze serveru.</span><span class="sxs-lookup"><span data-stu-id="09408-117">To further investigate client methods not getting called, you can turn on logging before the calling the start method on the hub to see what calls are coming from the server.</span></span> <span data-ttu-id="09408-118">Povolení protokolování v aplikaci JavaScript, najdete v části [jak povolit protokolování na straně klienta (verze jazyka JavaScript klienta)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span><span class="sxs-lookup"><span data-stu-id="09408-118">To enable logging in a JavaScript application, see [How to enable client-side logging (JavaScript client version)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span></span> <span data-ttu-id="09408-119">Povolení protokolování v aplikaci klienta rozhraní .NET, najdete v části [jak povolit protokolování na straně klienta (klient .NET verze)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span><span class="sxs-lookup"><span data-stu-id="09408-119">To enable logging in a .NET client application, see [How to enable client-side logging (.NET Client version)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span></span>

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a><span data-ttu-id="09408-120">Metoda překlepu, metoda nesprávný podpis nebo název nesprávný rozbočovače</span><span class="sxs-lookup"><span data-stu-id="09408-120">Misspelled method, incorrect method signature, or incorrect hub name</span></span>

<span data-ttu-id="09408-121">Pokud název nebo podpis metody volané přesně neodpovídá odpovídající metody na straně klienta, volání selže.</span><span class="sxs-lookup"><span data-stu-id="09408-121">If the name or signature of a called method does not exactly match an appropriate method on the client, the call will fail.</span></span> <span data-ttu-id="09408-122">Ověřte, že název metody, které jsou volány server odpovídá názvu metody na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="09408-122">Verify that the method name called by the server matches the name of the method on the client.</span></span> <span data-ttu-id="09408-123">Také SignalR vytvoří proxy server rozbočovače metodami ve formátu camelCase, jako je vhodné v jazyce JavaScript, takže volána metoda `SendMessage` na serveru by volat `sendMessage` proxy serveru klienta.</span><span class="sxs-lookup"><span data-stu-id="09408-123">Also, SignalR creates the hub proxy using camel-cased methods, as is appropriate in JavaScript, so a method called `SendMessage` on the server would be called `sendMessage` in the client proxy.</span></span> <span data-ttu-id="09408-124">Pokud použijete `HubName` atribut ve vašem kódu na straně serveru, zkontrolujte, zda použít název odpovídá názvu použít k vytvoření rozbočovače na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="09408-124">If you use the `HubName` attribute in your server-side code, verify that the name used matches the name used to create the hub on the client.</span></span> <span data-ttu-id="09408-125">Pokud použijete `HubName` atribut, ověřte, zda je název rozbočovače v klientovi JavaScript ve formátu camelCase-, jako je například chatHub místo ChatHub.</span><span class="sxs-lookup"><span data-stu-id="09408-125">If you do not use the `HubName` attribute, verify that the name of the hub in a JavaScript client is camel-cased, such as chatHub instead of ChatHub.</span></span>

### <a name="duplicate-method-name-on-client"></a><span data-ttu-id="09408-126">Duplicitní název metody na klientovi</span><span class="sxs-lookup"><span data-stu-id="09408-126">Duplicate method name on client</span></span>

<span data-ttu-id="09408-127">Zkontrolujte duplicitní metodu nemají na klientovi, který se liší pouze v případě.</span><span class="sxs-lookup"><span data-stu-id="09408-127">Verify that you do not have a duplicate method on the client that differs only by case.</span></span> <span data-ttu-id="09408-128">Pokud klientské aplikace má metodu s názvem `sendMessage`, ověřte, že není k dispozici také metodu s názvem `SendMessage` také.</span><span class="sxs-lookup"><span data-stu-id="09408-128">If your client application has a method called `sendMessage`, verify that there isn't also a method called `SendMessage` as well.</span></span>

### <a name="missing-json-parser-on-the-client"></a><span data-ttu-id="09408-129">Chybějící analyzátor JSON na straně klienta</span><span class="sxs-lookup"><span data-stu-id="09408-129">Missing JSON parser on the client</span></span>

<span data-ttu-id="09408-130">SignalR vyžaduje analyzátor JSON pro serializaci volání mezi serverem a klientem.</span><span class="sxs-lookup"><span data-stu-id="09408-130">SignalR requires a JSON parser to be present to serialize calls between the server and the client.</span></span> <span data-ttu-id="09408-131">Pokud váš klient nemá předdefinované analyzátor JSON (například aplikace Internet Explorer 7), musíte použít jeden ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="09408-131">If your client doesn't have a built-in JSON parser (such as Internet Explorer 7), you'll need to include one in your application.</span></span> <span data-ttu-id="09408-132">Můžete si stáhnout analyzátor JSON [zde](http://nuget.org/packages/json2).</span><span class="sxs-lookup"><span data-stu-id="09408-132">You can download the JSON parser [here](http://nuget.org/packages/json2).</span></span>

### <a name="mixing-hub-and-persistentconnection-syntax"></a><span data-ttu-id="09408-133">Kombinací rozbočovače a připojení PersistentConnection syntaxe</span><span class="sxs-lookup"><span data-stu-id="09408-133">Mixing Hub and PersistentConnection syntax</span></span>

<span data-ttu-id="09408-134">SignalR používá dva modely komunikace: rozbočovače a PersistentConnections.</span><span class="sxs-lookup"><span data-stu-id="09408-134">SignalR uses two communication models: Hubs and PersistentConnections.</span></span> <span data-ttu-id="09408-135">Syntaxe volání tyto modely dva komunikace se liší v kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="09408-135">The syntax for calling these two communication models is different in the client code.</span></span> <span data-ttu-id="09408-136">Pokud jste přidali rozbočovač v serverovém kódu, ověřte, že veškerý kód klienta se používá syntaxe správné rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="09408-136">If you have added a hub in your server code, verify that all of your client code uses the proper hub syntax.</span></span>

<span data-ttu-id="09408-137">**Kód JavaScript klienta, který vytvoří připojení PersistentConnection v klientovi JavaScript**</span><span class="sxs-lookup"><span data-stu-id="09408-137">**JavaScript client code that creates a PersistentConnection in a JavaScript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

<span data-ttu-id="09408-138">**Kód JavaScript klienta, který vytvoří proxy server rozbočovače v klientovi Javascript**</span><span class="sxs-lookup"><span data-stu-id="09408-138">**JavaScript client code that creates a Hub Proxy in a Javascript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

<span data-ttu-id="09408-139">**C# serveru kód, který mapuje trasu připojení PersistentConnection**</span><span class="sxs-lookup"><span data-stu-id="09408-139">**C# server code that maps a route to a PersistentConnection**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

<span data-ttu-id="09408-140">**C# serveru kód, který mapuje trasu k rozbočovači, nebo více centra, pokud máte několik aplikací**</span><span class="sxs-lookup"><span data-stu-id="09408-140">**C# server code that maps a route to a Hub, or to mulitple hubs if you have multiple applications**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a><span data-ttu-id="09408-141">Připojení před předplatná se přidají spustit</span><span class="sxs-lookup"><span data-stu-id="09408-141">Connection started before subscriptions are added</span></span>

<span data-ttu-id="09408-142">Pokud připojení rozbočovače na spuštění před metody, které lze volat ze serveru se přidají k proxy serveru, nebude přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="09408-142">If the Hub's connection is started before methods that can be called from the server are added to the proxy, messages will not be received.</span></span> <span data-ttu-id="09408-143">Následující kód v JavaScriptu nespustí rozbočovače správně:</span><span class="sxs-lookup"><span data-stu-id="09408-143">The following JavaScript code will not start the hub properly:</span></span>

<span data-ttu-id="09408-144">**Nesprávný kód klienta JavaScript, který nebude povolit příjem zpráv rozbočovače**</span><span class="sxs-lookup"><span data-stu-id="09408-144">**Incorrect JavaScript client code that will not allow Hubs messages to be received**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

<span data-ttu-id="09408-145">Místo toho přidejte odběry metoda před voláním metody Start:</span><span class="sxs-lookup"><span data-stu-id="09408-145">Instead, add the method subscriptions before calling Start:</span></span>

<span data-ttu-id="09408-146">**Kód JavaScript klienta, který správně přidá odběry k rozbočovači**</span><span class="sxs-lookup"><span data-stu-id="09408-146">**JavaScript client code that correctly adds subscriptions to a hub**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a><span data-ttu-id="09408-147">Chybí název metody na proxy server rozbočovače</span><span class="sxs-lookup"><span data-stu-id="09408-147">Missing method name on the hub proxy</span></span>

<span data-ttu-id="09408-148">Ověřte, že metoda definované na serveru je přihlášen k na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="09408-148">Verify that the method defined on the server is subscribed to on the client.</span></span> <span data-ttu-id="09408-149">I když server definuje metodu, musí pořád přidané k proxy serveru klienta.</span><span class="sxs-lookup"><span data-stu-id="09408-149">Even though the server defines the method, it must still be added to the client proxy.</span></span> <span data-ttu-id="09408-150">Metody lze přidat k proxy serveru klienta následujícími způsoby (Všimněte si, že metoda je přidán do `client` člen rozbočovače, rozbočovače není přímo):</span><span class="sxs-lookup"><span data-stu-id="09408-150">Methods can be added to the client proxy in the following ways (Note that the method is added to the `client` member of the hub, not the hub directly):</span></span>

<span data-ttu-id="09408-151">**Kód jazyka JavaScript klienta, který přidá metody proxy server rozbočovače**</span><span class="sxs-lookup"><span data-stu-id="09408-151">**JavaScript client code that adds methods to a hub proxy**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a><span data-ttu-id="09408-152">Centrum nebo není deklarován jako veřejné metody rozbočovače</span><span class="sxs-lookup"><span data-stu-id="09408-152">Hub or hub methods not declared as Public</span></span>

<span data-ttu-id="09408-153">Aby byl viditelný na straně klienta, musí být deklarován implementace rozbočovače a metody jako `public`.</span><span class="sxs-lookup"><span data-stu-id="09408-153">To be visible on the client, the hub implementation and methods must be declared as `public`.</span></span>

### <a name="accessing-hub-from-a-different-application"></a><span data-ttu-id="09408-154">Přístup k centru z jiné aplikace</span><span class="sxs-lookup"><span data-stu-id="09408-154">Accessing hub from a different application</span></span>

<span data-ttu-id="09408-155">Rozbočovače SignalR je přístupné pouze prostřednictvím aplikace, které implementují klienti SignalR.</span><span class="sxs-lookup"><span data-stu-id="09408-155">SignalR Hubs can only be accessed through applications that implement SignalR clients.</span></span> <span data-ttu-id="09408-156">SignalR nemůže spolupracovat s další komunikace knihovny (například SOAP nebo WCF webových služeb.) Pokud je k dispozici pro svou cílovou platformu žádné klienta SignalR, nemají přímý přístup serveru koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="09408-156">SignalR cannot interoperate with other communication libraries (like SOAP or WCF web services.) If there is no SignalR client available for your target platform, you cannot access the server's endpoint directly.</span></span>

### <a name="manually-serializing-data"></a><span data-ttu-id="09408-157">Ručně serializaci dat</span><span class="sxs-lookup"><span data-stu-id="09408-157">Manually serializing data</span></span>

<span data-ttu-id="09408-158">SignalR automaticky použije JSON k serializaci metodu parametry zde není nutné provést sami.</span><span class="sxs-lookup"><span data-stu-id="09408-158">SignalR will automatically use JSON to serialize your method parameters- there's no need to do it yourself.</span></span>

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a><span data-ttu-id="09408-159">Vzdálené metody rozbočovače nebyl proveden v klientovi ve funkci ondisconnected rozbočovače</span><span class="sxs-lookup"><span data-stu-id="09408-159">Remote Hub method not executed on client in OnDisconnected function</span></span>

<span data-ttu-id="09408-160">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="09408-160">This behavior is by design.</span></span> <span data-ttu-id="09408-161">Když `OnDisconnected` je volána, rozbočovače již zadány `Disconnected` stav, který neumožňuje další metody rozbočovače k volání.</span><span class="sxs-lookup"><span data-stu-id="09408-161">When `OnDisconnected` is called, the hub has already entered the `Disconnected` state, which does not allow further hub methods to be called.</span></span>

<span data-ttu-id="09408-162">**C# serveru kód, který správně spustí kód v události ondisconnected rozbočovače**</span><span class="sxs-lookup"><span data-stu-id="09408-162">**C# server code that correctly executes code in the OnDisconnected event**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a><span data-ttu-id="09408-163">Bylo dosaženo limitu připojení</span><span class="sxs-lookup"><span data-stu-id="09408-163">Connection limit reached</span></span>

<span data-ttu-id="09408-164">Pokud používáte plnou verzi služby IIS na klientský operační systém jako Windows 7, omezení 10 připojení není nastaveno.</span><span class="sxs-lookup"><span data-stu-id="09408-164">When using the full version of IIS on a client operating system like Windows 7, a 10-connection limit is imposed.</span></span> <span data-ttu-id="09408-165">Při použití klientského operačního systému, používejte IIS Express, místo aby se zabránilo toto omezení.</span><span class="sxs-lookup"><span data-stu-id="09408-165">When using a client OS, use IIS Express instead to avoid this limit.</span></span>

### <a name="cross-domain-connection-not-set-up-properly"></a><span data-ttu-id="09408-166">Není nastaveno správně připojení mezi doménami</span><span class="sxs-lookup"><span data-stu-id="09408-166">Cross-domain connection not set up properly</span></span>

<span data-ttu-id="09408-167">Pokud připojení mezi doménami není správně nastaven (připojení, pro kterou SignalR adresa URL není ve stejné doméně jako hostování stránka), připojení se pravděpodobně nezdaří bez chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="09408-167">If a cross-domain connection (a connection for which the SignalR URL is not in the same domain as the hosting page) is not set up correctly, the connection may fail without an error message.</span></span> <span data-ttu-id="09408-168">Informace o tom, jak povolit komunikaci mezi doménami, najdete v článku [postup připojení mezi doménami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="09408-168">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a><span data-ttu-id="09408-169">Připojení pomocí protokolu NTLM (Active Directory) nefunguje v klientovi rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="09408-169">Connection using NTLM (Active Directory) not working in .NET client</span></span>

<span data-ttu-id="09408-170">Připojení v aplikaci klienta rozhraní .NET, který používá zabezpečení domény může selhat, pokud připojení není správně nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="09408-170">A connection in a .NET client application that uses Domain security may fail if the connection is not configured properly.</span></span> <span data-ttu-id="09408-171">Pokud chcete používat funkce SignalR v prostředí domény, nastavte vlastnost požadavků připojení takto:</span><span class="sxs-lookup"><span data-stu-id="09408-171">To use SignalR in a domain environment, set the requisite connection property as follows:</span></span>

<span data-ttu-id="09408-172">**C# klienta kód, který implementuje přihlašovací údaje pro připojení**</span><span class="sxs-lookup"><span data-stu-id="09408-172">**C# client code that implements connection credentials**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a><span data-ttu-id="09408-173">Jiné potíže s připojením</span><span class="sxs-lookup"><span data-stu-id="09408-173">Other connection issues</span></span>

<span data-ttu-id="09408-174">Tato část popisuje příčiny a řešení pro konkrétní příznaky nebo chybové zprávy, které se vyskytnou při připojení.</span><span class="sxs-lookup"><span data-stu-id="09408-174">This section describes the causes and solutions for specific symptoms or error messages that occur during a connection.</span></span>

### <a name="start-must-be-called-before-data-can-be-sent-error"></a><span data-ttu-id="09408-175">"Spuštění musí být voláno před odesláním dat" Chyba</span><span class="sxs-lookup"><span data-stu-id="09408-175">"Start must be called before data can be sent" error</span></span>

<span data-ttu-id="09408-176">Tato chyba obvykle nastává, pokud kód odkazuje SignalR objekty před zahájením připojení.</span><span class="sxs-lookup"><span data-stu-id="09408-176">This error is commonly seen if code references SignalR objects before the connection is started.</span></span> <span data-ttu-id="09408-177">Wireup pro obslužné rutiny a podobně, budou volání metody definované na serveru musí přidat po dokončení připojení.</span><span class="sxs-lookup"><span data-stu-id="09408-177">The wireup for handlers and the like that will call methods defined on the server must be added after the connection completes.</span></span> <span data-ttu-id="09408-178">Všimněte si, že volání `Start` je asynchronní, takže kód po volání může provést dříve, než se dokončí.</span><span class="sxs-lookup"><span data-stu-id="09408-178">Note that the call to `Start` is asynchronous, so code after the call may be executed before it completes.</span></span> <span data-ttu-id="09408-179">Nejlepší způsob, jak přidat obslužné rutiny po spuštění připojení úplně je umístí je do funkce zpětného volání, která je jako parametr předaný metodě počáteční:</span><span class="sxs-lookup"><span data-stu-id="09408-179">The best way to add handlers after a connection starts completely is to put them into a callback function that is passed as a parameter to the start method:</span></span>

<span data-ttu-id="09408-180">**Kód jazyka JavaScript klienta, který správně přidá obslužné rutiny událostí, které odkazují na objekty SignalR**</span><span class="sxs-lookup"><span data-stu-id="09408-180">**JavaScript client code that correctly adds event handlers that reference SignalR objects**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

<span data-ttu-id="09408-181">Tato chyba se zobrazí také v případě připojení zastaví při SignalR objekty jsou stále se na ně odkazovat.</span><span class="sxs-lookup"><span data-stu-id="09408-181">This error will also be seen if a connection stops while SignalR objects are still being referenced.</span></span>

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a><span data-ttu-id="09408-182">"Trvale přesunut 301" nebo "dočasně přesunout 302" Chyba</span><span class="sxs-lookup"><span data-stu-id="09408-182">"301 Moved Permanently" or "302 Moved Temporarily" error</span></span>

<span data-ttu-id="09408-183">Tato chyba může být zobrazena, pokud projekt obsahuje složku s názvem SignalR, který koliduje s proxy serverem, automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="09408-183">This error may be seen if the project contains a folder called SignalR, which will interfere with the automatically-created proxy.</span></span> <span data-ttu-id="09408-184">Chcete-li se vyhnout této chybě, nepoužívejte složku s názvem `SignalR` v aplikaci, nebo vypněte vytváření automatické proxy vypnout.</span><span class="sxs-lookup"><span data-stu-id="09408-184">To avoid this error, do not use a folder called `SignalR` in your application, or turn automatic proxy generation off.</span></span> <span data-ttu-id="09408-185">V tématu [The generované Proxy a jakým způsobem vám](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="09408-185">See [The Generated Proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) for more details.</span></span>

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a><span data-ttu-id="09408-186">"403 zakázán" došlo k chybě v rozhraní .NET nebo Silverlight klienta</span><span class="sxs-lookup"><span data-stu-id="09408-186">"403 Forbidden" error in .NET or Silverlight client</span></span>

<span data-ttu-id="09408-187">K této chybě může dojít v prostředí napříč doménami, kde není správně povolit komunikaci mezi doménami.</span><span class="sxs-lookup"><span data-stu-id="09408-187">This error may occur in cross-domain environments where cross-domain communication is not properly enabled.</span></span> <span data-ttu-id="09408-188">Informace o tom, jak povolit komunikaci mezi doménami, najdete v článku [postup připojení mezi doménami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="09408-188">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span> <span data-ttu-id="09408-189">Vytvoření připojení mezi doménami v klienta Silverlight naleznete v části [napříč doménami připojení z klientů Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span><span class="sxs-lookup"><span data-stu-id="09408-189">To establish a cross-domain connection in a Silverlight client, see [Cross-domain connections from Silverlight clients](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span></span>

### <a name="404-not-found-error"></a><span data-ttu-id="09408-190">Chyba "404 nebyl nalezen."</span><span class="sxs-lookup"><span data-stu-id="09408-190">"404 Not Found" error</span></span>

<span data-ttu-id="09408-191">Existuje několik příčin tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="09408-191">There are several causes for this issue.</span></span> <span data-ttu-id="09408-192">Ověřte všechny z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="09408-192">Verify all of the following:</span></span>

- <span data-ttu-id="09408-193">**Odkaz adresu proxy server rozbočovače není správně naformátovaný:** tato chyba obvykle nastává Pokud odkaz na adresu proxy serveru generovaného centra není správně naformátovaná.</span><span class="sxs-lookup"><span data-stu-id="09408-193">**Hub proxy address reference not formatted correctly:** This error is commonly seen if the reference to the generated hub proxy address is not formatted correctly.</span></span> <span data-ttu-id="09408-194">Ověřte, že je odkaz na adresu rozbočovače provedeny správně.</span><span class="sxs-lookup"><span data-stu-id="09408-194">Verify that the reference to the hub address is made properly.</span></span> <span data-ttu-id="09408-195">V tématu [jak odkazovat dynamicky generovaném proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="09408-195">See [How to reference the dynamically generated proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) for details.</span></span>
- <span data-ttu-id="09408-196">**Přidání tras do aplikace před přidáním trasu rozbočovače:** Pokud vaše aplikace používá další postupy, ověřte, zda je první trasy přidat volání `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="09408-196">**Adding routes to application before adding the hub route:** If your application uses other routes, verify that the first route added is the call to `MapHubs`.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="09408-197">"Error 500 interního serveru.</span><span class="sxs-lookup"><span data-stu-id="09408-197">"500 Internal Server Error"</span></span>

<span data-ttu-id="09408-198">Toto je velmi obecná chyba, která může mít celou řadu příčin.</span><span class="sxs-lookup"><span data-stu-id="09408-198">This is a very generic error that could have a wide variety of causes.</span></span> <span data-ttu-id="09408-199">Informace o chybě by se měla objevit v protokolu událostí serveru nebo naleznete prostřednictvím ladění serveru.</span><span class="sxs-lookup"><span data-stu-id="09408-199">The details of the error should appear in the server's event log, or can be found through debugging the server.</span></span> <span data-ttu-id="09408-200">Když zapnete podrobné chyby na serveru může získat podrobnější informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="09408-200">More detailed error information may be obtained by turning on detailed errors on the server.</span></span> <span data-ttu-id="09408-201">Další informace najdete v tématu [způsob zpracování chyb ve třídě rozbočovače](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span><span class="sxs-lookup"><span data-stu-id="09408-201">For more information, see [How to handle errors in the Hub class](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span></span>

### <a name="typeerror-lthubtypegt-is-undefined-error"></a><span data-ttu-id="09408-202">"TypeError: &lt;hubType&gt; není definován" Chyba</span><span class="sxs-lookup"><span data-stu-id="09408-202">"TypeError: &lt;hubType&gt; is undefined" error</span></span>

<span data-ttu-id="09408-203">K této chybě dojde, pokud volání `MapHubs` není provedené správně.</span><span class="sxs-lookup"><span data-stu-id="09408-203">This error will result if the call to `MapHubs` is not made properly.</span></span> <span data-ttu-id="09408-204">V tématu [způsob registrace SignalR trasy a konfigurovat možnosti SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) Další informace.</span><span class="sxs-lookup"><span data-stu-id="09408-204">See [How to register the SignalR route and configure SignalR options](../guide-to-the-api/hubs-api-guide-server.md#route) for more information.</span></span>

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a><span data-ttu-id="09408-205">JsonSerializationException byl neošetřená pomocí uživatelského kódu</span><span class="sxs-lookup"><span data-stu-id="09408-205">JsonSerializationException was unhandled by user code</span></span>

<span data-ttu-id="09408-206">Ověřte, že parametry, které poslat vaše metody nezahrnují-Serializovatelné typy (například obslužných rutin souborů nebo připojení k databázi).</span><span class="sxs-lookup"><span data-stu-id="09408-206">Verify that the parameters you send to your methods do not include non-serializable types (like file handles or database connections).</span></span> <span data-ttu-id="09408-207">Pokud budete muset použít členy na straně serveru objekt, který nechcete, aby k odeslání do klienta (buď pro zabezpečení nebo z důvodů serializace), použijte `JSONIgnore` atribut.</span><span class="sxs-lookup"><span data-stu-id="09408-207">If you need to use members on a server-side object that you don't want to be sent to the client (either for security or for reasons of serialization), use the `JSONIgnore` attribute.</span></span>

### <a name="protocol-error-unknown-transport-error"></a><span data-ttu-id="09408-208">"Chyba protokolu: Neznámý přenos" Chyba</span><span class="sxs-lookup"><span data-stu-id="09408-208">"Protocol error: Unknown transport" error</span></span>

<span data-ttu-id="09408-209">Této chybě může dojít, pokud klient nepodporuje přenosy, které používá SignalR.</span><span class="sxs-lookup"><span data-stu-id="09408-209">This error may occur if the client does not support the transports that SignalR uses.</span></span> <span data-ttu-id="09408-210">V tématu [přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports) informace, na kterém prohlížečů lze použít s SignalR.</span><span class="sxs-lookup"><span data-stu-id="09408-210">See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for information on which browsers can be used with SignalR.</span></span>

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a><span data-ttu-id="09408-211">"Vytváření proxy JavaScript Hub bylo zakázáno."</span><span class="sxs-lookup"><span data-stu-id="09408-211">"JavaScript Hub proxy generation has been disabled."</span></span>

<span data-ttu-id="09408-212">K této chybě dojde, pokud `DisableJavaScriptProxies` je nastaven při také včetně odkazu na dynamicky vygenerovaný proxy server na `signalr/hubs`.</span><span class="sxs-lookup"><span data-stu-id="09408-212">This error will occur if `DisableJavaScriptProxies` is set while also including a reference to the dynamically generated proxy at `signalr/hubs`.</span></span> <span data-ttu-id="09408-213">Další informace o vytvoření proxy serveru ručně, najdete v části [vygenerovaný proxy server a jakým způsobem vám](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="09408-213">For more information on creating the proxy manually, see [The generated proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span></span>

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a><span data-ttu-id="09408-214">"ID připojení je v nesprávném formátu" nebo "identity uživatele se nemůže změnit během aktivního připojení SignalR" Chyba</span><span class="sxs-lookup"><span data-stu-id="09408-214">"The connection ID is in the incorrect format" or "The user identity cannot change during an active SignalR connection" error</span></span>

<span data-ttu-id="09408-215">Tato chyba může být zobrazena, pokud se používá ověřování a klient se zaznamená před ukončením připojení.</span><span class="sxs-lookup"><span data-stu-id="09408-215">This error may be seen if authentication is being used, and the client is logged out before the connection is stopped.</span></span> <span data-ttu-id="09408-216">Řešení je k zastavení před protokolování klienta připojení SignalR.</span><span class="sxs-lookup"><span data-stu-id="09408-216">The solution is to stop the SignalR connection before logging the client out.</span></span>

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a><span data-ttu-id="09408-217">"Nezachycená Chyba: SignalR: jQuery nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="09408-217">"Uncaught Error: SignalR: jQuery not found.</span></span> <span data-ttu-id="09408-218">Zkontrolujte, že jQuery odkazuje před soubor SignalR.js"Chyba</span><span class="sxs-lookup"><span data-stu-id="09408-218">Please ensure jQuery is referenced before the SignalR.js file" error</span></span>

<span data-ttu-id="09408-219">Klient SignalR JavaScript vyžaduje jQuery ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="09408-219">The SignalR JavaScript client requires jQuery to run.</span></span> <span data-ttu-id="09408-220">Ověřte, zda je správný, zda cesty použité platná a že je odkaz na jQuery před odkaz na SignalR vaši informaci, abyste jQuery.</span><span class="sxs-lookup"><span data-stu-id="09408-220">Verify that your reference to jQuery is correct, that the path used is valid, and that the reference to jQuery is before the reference to SignalR.</span></span>

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a><span data-ttu-id="09408-221">"Nezachycená TypeError: Nelze načíst vlastnost '&lt;vlastnost&gt;' undefined" Chyba</span><span class="sxs-lookup"><span data-stu-id="09408-221">"Uncaught TypeError: Cannot read property '&lt;property&gt;' of undefined" error</span></span>

<span data-ttu-id="09408-222">Tato chyba se výsledkem nemá jQuery nebo proxy server rozbočovače odkazovaný správně.</span><span class="sxs-lookup"><span data-stu-id="09408-222">This error results from not having jQuery or the hubs proxy referenced properly.</span></span> <span data-ttu-id="09408-223">Ověřte, zda je správný, zda cesta používaná platná a že je odkaz na jQuery před odkaz na proxy server rozbočovače vaši informaci, abyste jQuery a proxy server rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="09408-223">Verify that your reference to jQuery and the hubs proxy is correct, that the path used is valid, and that the reference to jQuery is before the reference to the hubs proxy.</span></span> <span data-ttu-id="09408-224">Výchozí odkaz na proxy server rozbočovače by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="09408-224">The default reference to the hubs proxy should look like the following:</span></span>

<span data-ttu-id="09408-225">**Kód na straně klienta HTML, která správně odkazuje na proxy server rozbočovače**</span><span class="sxs-lookup"><span data-stu-id="09408-225">**HTML client-side code that correctly references the Hubs proxy**</span></span>

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a><span data-ttu-id="09408-226">Chyba "RuntimeBinderException byl neošetřená pomocí uživatelského kódu."</span><span class="sxs-lookup"><span data-stu-id="09408-226">"RuntimeBinderException was unhandled by user code" error</span></span>

<span data-ttu-id="09408-227">Této chybě může dojít při přetížení nesprávné `Hub.On` se používá.</span><span class="sxs-lookup"><span data-stu-id="09408-227">This error may occur when the incorrect overload of `Hub.On` is used.</span></span> <span data-ttu-id="09408-228">Pokud metoda má návratovou hodnotu, návratový typ musí být zadány jako parametr obecného typu:</span><span class="sxs-lookup"><span data-stu-id="09408-228">If the method has a return value, the return type must be specified as a generic type parameter:</span></span>

<span data-ttu-id="09408-229">**Metody definované v klientovi (bez vygenerovaný proxy server)**</span><span class="sxs-lookup"><span data-stu-id="09408-229">**Method defined on the client (without generated proxy)**</span></span>

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a><span data-ttu-id="09408-230">ID připojení je nekonzistentní nebo připojení dělí mezi načtení stránky</span><span class="sxs-lookup"><span data-stu-id="09408-230">Connection ID is inconsistent or connection breaks between page loads</span></span>

<span data-ttu-id="09408-231">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="09408-231">This behavior is by design.</span></span> <span data-ttu-id="09408-232">Vzhledem k tomu, že objekt rozbočovače hostovaný v objektu page, rozbočovače zničena při aktualizaci stránky.</span><span class="sxs-lookup"><span data-stu-id="09408-232">Since the hub object is hosted in the page object, the hub is destroyed when the page refreshes.</span></span> <span data-ttu-id="09408-233">Aplikace s více stránkami musí udržovat přidružení mezi uživateli a ID připojení tak, aby byla konzistentní mezi načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="09408-233">A multi-page application needs to maintain the association between users and connection IDs so that they will be consistent between page loads.</span></span> <span data-ttu-id="09408-234">ID připojení mohou být uloženy na serveru buď `ConcurrentDictionary` objekt nebo databáze.</span><span class="sxs-lookup"><span data-stu-id="09408-234">The connection IDs can be stored on the server in either a `ConcurrentDictionary` object or a database.</span></span>

### <a name="value-cannot-be-null-error"></a><span data-ttu-id="09408-235">Chyba "Hodnota nemůže být null."</span><span class="sxs-lookup"><span data-stu-id="09408-235">"Value cannot be null" error</span></span>

<span data-ttu-id="09408-236">Metody na straně serveru s volitelné parametry nejsou aktuálně podporovány; Pokud je volitelný parametr vynechán, metoda selže.</span><span class="sxs-lookup"><span data-stu-id="09408-236">Server-side methods with optional parameters are not currently supported; if the optional parameter is omitted, the method will fail.</span></span> <span data-ttu-id="09408-237">Další informace najdete v tématu [volitelné parametry](https://github.com/SignalR/SignalR/issues/324).</span><span class="sxs-lookup"><span data-stu-id="09408-237">For more information, see [Optional Parameters](https://github.com/SignalR/SignalR/issues/324).</span></span>

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a><span data-ttu-id="09408-238">"Firefox nemůže navázat připojení k serveru na &lt;adresu&gt;" došlo k chybě v Firebug</span><span class="sxs-lookup"><span data-stu-id="09408-238">"Firefox can't establish a connection to the server at &lt;address&gt;" error in Firebug</span></span>

<span data-ttu-id="09408-239">Tato chybová zpráva se zobrazí v Firebug Pokud selže vyjednávání protokolu WebSocket přenosu a místo toho používá jiný přenos.</span><span class="sxs-lookup"><span data-stu-id="09408-239">This error message can be seen in Firebug if negotiation of the WebSocket transport fails and another transport is used instead.</span></span> <span data-ttu-id="09408-240">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="09408-240">This behavior is by design.</span></span>

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a><span data-ttu-id="09408-241">Chyba "Vzdálený certifikát není platný podle procedury ověřování" v aplikaci klienta rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="09408-241">"The remote certificate is invalid according to the validation procedure" error in .NET client application</span></span>

<span data-ttu-id="09408-242">Pokud server vyžaduje vlastní klientské certifikáty, potom můžete přidat certifikátu x 509 na připojení před požadavku.</span><span class="sxs-lookup"><span data-stu-id="09408-242">If your server requires custom client certificates, then you can add an x509certificate to the connection before the request is made.</span></span> <span data-ttu-id="09408-243">Přidání certifikátu pro připojení s využitím `Connection.AddClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="09408-243">Add the certificate to the connection using `Connection.AddClientCertificate`.</span></span>

### <a name="connection-drops-after-authentication-times-out"></a><span data-ttu-id="09408-244">Zrušení připojení vyčkat před po vypršení časového limitu ověřování</span><span class="sxs-lookup"><span data-stu-id="09408-244">Connection drops after authentication times out</span></span>

<span data-ttu-id="09408-245">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="09408-245">This behavior is by design.</span></span> <span data-ttu-id="09408-246">Pověření ověřování nejde změnit, když je připojení aktivní; Aktualizujte přihlašovací údaje, musí být připojení zastavena a restartována.</span><span class="sxs-lookup"><span data-stu-id="09408-246">Authentication credentials cannot be modified while a connection is active; to refresh credentials, the connection must be stopped and restarted.</span></span>

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a><span data-ttu-id="09408-247">Onconnected rozbočovače volala dvakrát, při použití technologie jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="09408-247">OnConnected gets called twice when using jQuery Mobile</span></span>

<span data-ttu-id="09408-248">jQuery Mobile je `initializePage` funkce vynutí skripty v každé stránce znovu spouštění, proto vytváření druhé připojení.</span><span class="sxs-lookup"><span data-stu-id="09408-248">jQuery Mobile's `initializePage` function forces the scripts in each page to be re-executed, thus creating a second connection.</span></span> <span data-ttu-id="09408-249">Řešení tohoto problému patří:</span><span class="sxs-lookup"><span data-stu-id="09408-249">Solutions for this issue include:</span></span>

- <span data-ttu-id="09408-250">Zahrnout odkaz na architekturu jQuery Mobile před souboru JavaScript.</span><span class="sxs-lookup"><span data-stu-id="09408-250">Include the reference to jQuery Mobile before your JavaScript file.</span></span>
- <span data-ttu-id="09408-251">Zakažte `initializePage` funkce nastavením `$.mobile.autoInitializePage = false`.</span><span class="sxs-lookup"><span data-stu-id="09408-251">Disable the `initializePage` function by setting `$.mobile.autoInitializePage = false`.</span></span>
- <span data-ttu-id="09408-252">Počkejte, než pro stránku k dokončení inicializace před spuštěním připojení.</span><span class="sxs-lookup"><span data-stu-id="09408-252">Wait for the page to finish initializing before starting the connection.</span></span>

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a><span data-ttu-id="09408-253">Zprávy jsou odložené v aplikace Silverlight pomocí události odeslané serverem</span><span class="sxs-lookup"><span data-stu-id="09408-253">Messages are delayed in Silverlight applications using Server Sent Events</span></span>

<span data-ttu-id="09408-254">Zprávy jsou zpoždění při používání serveru odeslání události na Silverlight.</span><span class="sxs-lookup"><span data-stu-id="09408-254">Messages are delayed when using server sent events on Silverlight.</span></span> <span data-ttu-id="09408-255">Chcete-li vynutit dlouhé dotazování místo toho použít, použijte následující postupy při spouštění připojení:</span><span class="sxs-lookup"><span data-stu-id="09408-255">To force long polling to be used instead, use the following when starting the connection:</span></span>

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a><span data-ttu-id="09408-256">Použití "Oprávnění byl odepřen" navždy rámce protokolu</span><span class="sxs-lookup"><span data-stu-id="09408-256">"Permission Denied" using Forever Frame protocol</span></span>

<span data-ttu-id="09408-257">Jedná se o známý problém popsaný [zde](https://github.com/SignalR/SignalR/issues/1963).</span><span class="sxs-lookup"><span data-stu-id="09408-257">This is a known issue, described [here](https://github.com/SignalR/SignalR/issues/1963).</span></span> <span data-ttu-id="09408-258">Tento příznak může zobrazit pomocí nejnovější knihovny JQuery; Přejít na starší verzi aplikace JQuery 1.8.2 je alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="09408-258">This symptom may be seen using the latest JQuery library; the workaround is to downgrade your application to JQuery 1.8.2.</span></span>

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a><span data-ttu-id="09408-259">Chyby kompilace a na straně serveru</span><span class="sxs-lookup"><span data-stu-id="09408-259">Compilation and server-side errors</span></span>

 <span data-ttu-id="09408-260">Následující oddíl obsahuje možná řešení kompilátoru a chyby za běhu na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="09408-260">The following section contains possible solutions to compiler and server-side runtime errors.</span></span> 

### <a name="reference-to-hub-instance-is-null"></a><span data-ttu-id="09408-261">Odkaz na instanci rozbočovače má hodnotu null</span><span class="sxs-lookup"><span data-stu-id="09408-261">Reference to Hub instance is null</span></span>

<span data-ttu-id="09408-262">Vzhledem k tomu, že je pro každé připojení vytvořená hub instance, nelze vytvořit instanci rozbočovače ve vašem kódu sami.</span><span class="sxs-lookup"><span data-stu-id="09408-262">Since a hub instance is created for each connection, you can't create an instance of a hub in your code yourself.</span></span> <span data-ttu-id="09408-263">Volání metody v rozbočovači z mimo rozbočovače sám sebe, najdete v tématu [jak volat metody klienta a spravovat skupiny z mimo třídy rozbočovače](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) pro získání odkazu na kontext rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="09408-263">To call methods on a hub from outside the hub itself, see [How to call client methods and manage groups from outside the Hub class](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) for how to obtain a reference to the hub context.</span></span>

### <a name="httpcontextcurrentsession-is-null"></a><span data-ttu-id="09408-264">HTTPContext.Current.Session má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="09408-264">HTTPContext.Current.Session is null</span></span>

<span data-ttu-id="09408-265">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="09408-265">This behavior is by design.</span></span> <span data-ttu-id="09408-266">SignalR nepodporuje stavu relace ASP.NET, vzhledem k tomu, že povolení stavu relace by rozdělit duplexní zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="09408-266">SignalR does not support the ASP.NET session state, since enabling the session state would break duplex messaging.</span></span>

### <a name="no-suitable-method-to-override"></a><span data-ttu-id="09408-267">Žádná vhodná metoda. k přepsání</span><span class="sxs-lookup"><span data-stu-id="09408-267">No suitable method to override</span></span>

<span data-ttu-id="09408-268">Tato chyba nastane, pokud používáte kód z starší dokumentaci a blogů.</span><span class="sxs-lookup"><span data-stu-id="09408-268">You may see this error if you are using code from older documentation or blogs.</span></span> <span data-ttu-id="09408-269">Ověřte, že nejsou odkazující na názvy metod, které byly změněny nebo zastaralé (jako je `OnConnectedAsync`).</span><span class="sxs-lookup"><span data-stu-id="09408-269">Verify that you are not referencing names of methods that have been changed or deprecated (like `OnConnectedAsync`).</span></span>

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a><span data-ttu-id="09408-270">HostContextExtensions.WebSocketServerUrl má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="09408-270">HostContextExtensions.WebSocketServerUrl is null</span></span>

<span data-ttu-id="09408-271">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="09408-271">This behavior is by design.</span></span> <span data-ttu-id="09408-272">Tento člen je zastaralá a by se neměla používat.</span><span class="sxs-lookup"><span data-stu-id="09408-272">This member is deprecated and should not be used.</span></span>

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a><span data-ttu-id="09408-273">Chyba "trasu s názvem 'signalr.hubs' je již v kolekci trasy"</span><span class="sxs-lookup"><span data-stu-id="09408-273">"A route named 'signalr.hubs' is already in the route collection" error</span></span>

<span data-ttu-id="09408-274">Tato chyba se zobrazí v případě `MapHubs` volá dvakrát vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="09408-274">This error will be seen if `MapHubs` is called twice by your application.</span></span> <span data-ttu-id="09408-275">Některé aplikace volání příklad `MapHubs` přímo v souboru globální aplikace; ostatní použije volání v obálkovou třídu.</span><span class="sxs-lookup"><span data-stu-id="09408-275">Some example applications call `MapHubs` directly in the global application file; others make the call in a wrapper class.</span></span> <span data-ttu-id="09408-276">Ujistěte se, že vaše aplikace není proveďte obojí.</span><span class="sxs-lookup"><span data-stu-id="09408-276">Ensure that your application does not do both.</span></span>

<a id="vs"></a>

## <a name="visual-studio-issues"></a><span data-ttu-id="09408-277">Problémy v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09408-277">Visual Studio issues</span></span>

<span data-ttu-id="09408-278">Tato část popisuje problémy v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="09408-278">This section describes issues encountered in Visual Studio.</span></span>

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a><span data-ttu-id="09408-279">Skript dokumenty uzel neobjeví v Průzkumníku řešení</span><span class="sxs-lookup"><span data-stu-id="09408-279">Script Documents node does not appear in Solution Explorer</span></span>

<span data-ttu-id="09408-280">Některé z našich kurzů vás nasměrovat k uzlu "Dokumentů skriptu" v Průzkumníku řešení při ladění.</span><span class="sxs-lookup"><span data-stu-id="09408-280">Some of our tutorials direct you to the "Script Documents" node in Solution Explorer while debugging.</span></span> <span data-ttu-id="09408-281">Tento uzel je produkovaný ladicího programu JavaScript a zobrazí se pouze při ladění klienty prohlížeče v Internet Exploreru; uzel se nezobrazí, pokud se používají Chrome nebo Firefox.</span><span class="sxs-lookup"><span data-stu-id="09408-281">This node is produced by the JavaScript debugger, and will only appear while debugging browser clients in Internet Explorer; the node will not appear if Chrome or Firefox are used.</span></span> <span data-ttu-id="09408-282">Ladicí program JavaScript se nespustí, i pokud jiný klient ladicí program běží, jako je například ladicí program Silverlight.</span><span class="sxs-lookup"><span data-stu-id="09408-282">The JavaScript debugger will also not run if another client debugger is running, such as the Silverlight debugger.</span></span>

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a><span data-ttu-id="09408-283">SignalR nefunguje v sadě Visual Studio 2008 nebo dřívější</span><span class="sxs-lookup"><span data-stu-id="09408-283">SignalR does not work on Visual Studio 2008 or earlier</span></span>

<span data-ttu-id="09408-284">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="09408-284">This behavior is by design.</span></span> <span data-ttu-id="09408-285">SignalR vyžaduje rozhraní .NET Framework 4 nebo novější; To vyžaduje, aby aplikací SignalR vývoje v sadě Visual Studio 2010 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="09408-285">SignalR requires .NET Framework 4 or later; this requires that SignalR applications be developed in Visual Studio 2010 or later.</span></span>

<a id="iis"></a>

## <a name="iis-issues"></a><span data-ttu-id="09408-286">Problémy služby IIS</span><span class="sxs-lookup"><span data-stu-id="09408-286">IIS issues</span></span>

<span data-ttu-id="09408-287">Tato část obsahuje problémy se službou IIS.</span><span class="sxs-lookup"><span data-stu-id="09408-287">This section contains issues with Internet Information Services.</span></span>

### <a name="web-site-crashes-after-maphubs-call"></a><span data-ttu-id="09408-288">Po volání MapHubs dojde k chybě webu</span><span class="sxs-lookup"><span data-stu-id="09408-288">Web site crashes after MapHubs call</span></span>

<span data-ttu-id="09408-289">Tento problém byl opraven v nejnovější verzi systému SignalR.</span><span class="sxs-lookup"><span data-stu-id="09408-289">This issue has been fixed in the latest version of SignalR.</span></span> <span data-ttu-id="09408-290">Ověřte, že používáte nejnovější prodejní verze nástroje SignalR aktualizací instalace pomocí nástroje NuGet.</span><span class="sxs-lookup"><span data-stu-id="09408-290">Verify that you are using the latest released version of SignalR by updating your installation using NuGet.</span></span>

<a id="azure"></a>

## <a name="azure-issues"></a><span data-ttu-id="09408-291">Problémy s Azure</span><span class="sxs-lookup"><span data-stu-id="09408-291">Azure issues</span></span>

<span data-ttu-id="09408-292">Tato část obsahuje problémy s Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="09408-292">This section contains issues with Microsoft Azure.</span></span>

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a><span data-ttu-id="09408-293">Prostřednictvím Azure propojovacího rozhraní po Změna tématu názvy nejsou přijaty zprávy.</span><span class="sxs-lookup"><span data-stu-id="09408-293">Messages are not received through the Azure backplane after altering topic names</span></span>

<span data-ttu-id="09408-294">V tématech, která používá Azure propojovacího rozhraní nejsou určeny k možné konfigurovat uživatele.</span><span class="sxs-lookup"><span data-stu-id="09408-294">The topics used by the Azure backplane are not intended to be user-configurable.</span></span>
