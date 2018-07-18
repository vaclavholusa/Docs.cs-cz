---
title: Úvod do ASP.NET Core SignalR
author: tdykstra
description: Zjistěte, jak funkce SignalR technologie ASP.NET Core library usnadňuje přidávání funkcí v reálném čase do aplikací.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095386"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="ed053-103">Úvod do ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ed053-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="ed053-104">Podle [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="ed053-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="ed053-105">Co je SignalR?</span><span class="sxs-lookup"><span data-stu-id="ed053-105">What is SignalR?</span></span>

<span data-ttu-id="ed053-106">Funkce SignalR technologie ASP.NET Core je knihovna, která zjednodušuje přidávání funkce webu v reálném čase do aplikací.</span><span class="sxs-lookup"><span data-stu-id="ed053-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="ed053-107">Funkce webu v reálném čase umožňuje okamžitě kódu na straně serveru předávaný obsah do klientů.</span><span class="sxs-lookup"><span data-stu-id="ed053-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="ed053-108">Vhodnými kandidáty pro funkci SignalR:</span><span class="sxs-lookup"><span data-stu-id="ed053-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="ed053-109">Aplikace, které vyžadují vysoká frekvence aktualizace ze serveru.</span><span class="sxs-lookup"><span data-stu-id="ed053-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="ed053-110">Příkladem jsou hry, sociálních sítích, hlasování, aukce, mapy a GPS aplikace.</span><span class="sxs-lookup"><span data-stu-id="ed053-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="ed053-111">Řídicí panely a monitorování aplikace.</span><span class="sxs-lookup"><span data-stu-id="ed053-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="ed053-112">Příklady zahrnují společnosti řídicích panelů, rychlých aktualizací prodeje, nebo cestují výstrahy.</span><span class="sxs-lookup"><span data-stu-id="ed053-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="ed053-113">Aplikace pro spolupráci.</span><span class="sxs-lookup"><span data-stu-id="ed053-113">Collaborative apps.</span></span> <span data-ttu-id="ed053-114">Aplikace tabule a týmu splnění softwaru jsou příklady aplikací pro spolupráci.</span><span class="sxs-lookup"><span data-stu-id="ed053-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="ed053-115">Aplikace, které vyžadují oznámení.</span><span class="sxs-lookup"><span data-stu-id="ed053-115">Apps that require notifications.</span></span> <span data-ttu-id="ed053-116">Sociálních sítí, e-mailu, konverzace, využívají hry, cestování výstrahy a mnoha jiných aplikací oznámení.</span><span class="sxs-lookup"><span data-stu-id="ed053-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="ed053-117">Funkce SignalR poskytuje rozhraní API pro vytváření server klient [vzdálených volání procedur (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="ed053-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="ed053-118">Vzdálených volání procedur volají funkce JavaScript na klientských počítačích z kódu .NET Core na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="ed053-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="ed053-119">Funkce SignalR technologie ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="ed053-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="ed053-120">Správa připojení automaticky zpracovává.</span><span class="sxs-lookup"><span data-stu-id="ed053-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="ed053-121">Umožňuje všesměrové vysílání zpráv do všech připojených klientů současně.</span><span class="sxs-lookup"><span data-stu-id="ed053-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="ed053-122">Například chatovací místnosti.</span><span class="sxs-lookup"><span data-stu-id="ed053-122">For example, a chat room.</span></span>
* <span data-ttu-id="ed053-123">Umožňuje odesílání zpráv do konkrétních klientů nebo skupiny klientů.</span><span class="sxs-lookup"><span data-stu-id="ed053-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="ed053-124">Open source v [Githubu](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="ed053-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="ed053-125">Škálovatelné.</span><span class="sxs-lookup"><span data-stu-id="ed053-125">Scalable.</span></span>

<span data-ttu-id="ed053-126">Připojení mezi klientem a serverem je trvalé, na rozdíl od připojení HTTP.</span><span class="sxs-lookup"><span data-stu-id="ed053-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="ed053-127">Přenosy</span><span class="sxs-lookup"><span data-stu-id="ed053-127">Transports</span></span>

<span data-ttu-id="ed053-128">Funkce SignalR přehledů přes několik technik pro vytváření aplikací webu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="ed053-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="ed053-129">[Protokoly Websocket](https://tools.ietf.org/html/rfc7118) je optimální přenosu, ale jiné postupů, jako jsou události Server-Sent a dlouhý interval dotazování se dá použít při ty nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ed053-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="ed053-130">Bude automaticky rozpoznávat a inicializovat příslušné přenosu na základě funkcí podporován na serveru a klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="ed053-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="ed053-131">Rozbočovače</span><span class="sxs-lookup"><span data-stu-id="ed053-131">Hubs</span></span>

<span data-ttu-id="ed053-132">Rozbočovače SignalR používá ke komunikaci mezi klienty a servery.</span><span class="sxs-lookup"><span data-stu-id="ed053-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="ed053-133">Centrum je základní kanál, který umožňuje klientem a serverem pro volání metod na sobě navzájem.</span><span class="sxs-lookup"><span data-stu-id="ed053-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="ed053-134">Funkce SignalR zpracovává odeslání přes hranice počítač automaticky, umožňuje klientům volání metod na serveru jako snadno jako místní metody a naopak.</span><span class="sxs-lookup"><span data-stu-id="ed053-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="ed053-135">Rozbočovače povolit předávání silného typu parametrů k metodám, což umožňuje vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="ed053-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="ed053-136">Funkce SignalR poskytuje dva protokoly integrované centra: textového protokolu na základě JSON a binární protokol založený na [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="ed053-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="ed053-137">MessagePack obvykle vytvoří zpráv menší než při použití formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ed053-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="ed053-138">Starší prohlížeče musí podporovat [XHR úrovně 2](https://caniuse.com/#feat=xhr2) k zajištění podpory protokolu MessagePack.</span><span class="sxs-lookup"><span data-stu-id="ed053-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="ed053-139">Odesílání zpráv pomocí aktivní přenos volání rozbočovače kód na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ed053-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="ed053-140">Zprávy obsahují název a parametry metody na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ed053-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="ed053-141">Objekty, které jako parametry metody jsou deserializaci pomocí nakonfigurovaný protokol.</span><span class="sxs-lookup"><span data-stu-id="ed053-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="ed053-142">Klient se pokusí shodovat s názvem metody v kódu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ed053-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="ed053-143">Když nastane shoda, metoda klienta spouští pomocí deserializovat parametr data.</span><span class="sxs-lookup"><span data-stu-id="ed053-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed053-144">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ed053-144">Additional resources</span></span>

* [<span data-ttu-id="ed053-145">Začínáme s knihovnou SignalR pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed053-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="ed053-146">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="ed053-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="ed053-147">Centra</span><span class="sxs-lookup"><span data-stu-id="ed053-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ed053-148">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="ed053-148">JavaScript client</span></span>](xref:signalr/javascript-client)
