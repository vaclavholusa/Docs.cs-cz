---
title: Úvod do základní ASP.NET SignalR
author: rachelappel
description: Zjistěte, jak knihovny ASP.NET Core SignalR zjednodušuje přidání funkcí v reálném čase do aplikací.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 0c833acea139d22883a69a02c2357a71f3ac8db8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277901"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="1696e-103">Úvod do základní ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="1696e-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="1696e-104">Podle [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="1696e-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="1696e-105">Co je SignalR?</span><span class="sxs-lookup"><span data-stu-id="1696e-105">What is SignalR?</span></span>

<span data-ttu-id="1696e-106">Jádro ASP.NET SignalR je do knihovny, která zjednodušuje přidávání funkce webu v reálném čase do aplikací.</span><span class="sxs-lookup"><span data-stu-id="1696e-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="1696e-107">Funkce webu v reálném čase umožňuje kódu na straně serveru k obsahu nabízených klientům okamžitě.</span><span class="sxs-lookup"><span data-stu-id="1696e-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="1696e-108">Vhodnými kandidáty pro SignalR:</span><span class="sxs-lookup"><span data-stu-id="1696e-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="1696e-109">Aplikace, které vyžadují vysoká frekvence aktualizace ze serveru.</span><span class="sxs-lookup"><span data-stu-id="1696e-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="1696e-110">Příklady jsou herní, sociálních sítí, hlasování, aukce, mapy a GPS aplikace.</span><span class="sxs-lookup"><span data-stu-id="1696e-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="1696e-111">Řídicí panely a monitorování aplikací.</span><span class="sxs-lookup"><span data-stu-id="1696e-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="1696e-112">Mezi příklady patří společnosti řídicí panely, rychlých prodeje aktualizace, nebo cestují výstrahy.</span><span class="sxs-lookup"><span data-stu-id="1696e-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="1696e-113">Spolupráce aplikace.</span><span class="sxs-lookup"><span data-stu-id="1696e-113">Collaborative apps.</span></span> <span data-ttu-id="1696e-114">Aplikace tabulí a tým splňuje softwaru jsou příklady spolupráci aplikací.</span><span class="sxs-lookup"><span data-stu-id="1696e-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="1696e-115">Aplikace, které vyžadují oznámení.</span><span class="sxs-lookup"><span data-stu-id="1696e-115">Apps that require notifications.</span></span> <span data-ttu-id="1696e-116">Sociálních sítí, e-mailu, konverzace, hry, cesta výstrahy a mnoho dalších aplikací používat oznámení.</span><span class="sxs-lookup"><span data-stu-id="1696e-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="1696e-117">Funkce SignalR poskytuje rozhraní API pro vytvoření klienta a serveru [vzdálených volání procedur (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="1696e-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="1696e-118">Vzdálených volání procedur volají funkce JavaScript na klientských počítačích z kódu .NET Core straně serveru.</span><span class="sxs-lookup"><span data-stu-id="1696e-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="1696e-119">Funkce SignalR pro ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="1696e-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="1696e-120">Provádí správu připojení automaticky.</span><span class="sxs-lookup"><span data-stu-id="1696e-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="1696e-121">Umožňuje všesměrové vysílání zprávy pro všechny připojené klienty současně.</span><span class="sxs-lookup"><span data-stu-id="1696e-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="1696e-122">Například chatovací místnosti.</span><span class="sxs-lookup"><span data-stu-id="1696e-122">For example, a chat room.</span></span>
* <span data-ttu-id="1696e-123">Umožňuje odesílání zpráv do konkrétní klienti nebo skupiny klientů.</span><span class="sxs-lookup"><span data-stu-id="1696e-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="1696e-124">Je open source v [Githubu](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="1696e-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="1696e-125">Škálovatelná.</span><span class="sxs-lookup"><span data-stu-id="1696e-125">Scalable.</span></span>

<span data-ttu-id="1696e-126">Připojení mezi klientem a serverem je trvalé, na rozdíl od připojení HTTP.</span><span class="sxs-lookup"><span data-stu-id="1696e-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="1696e-127">Přenosy</span><span class="sxs-lookup"><span data-stu-id="1696e-127">Transports</span></span>

<span data-ttu-id="1696e-128">SignalR přehledů v rámci počtu techniky pro vytváření aplikací webu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="1696e-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="1696e-129">[Technologie WebSockets](https://tools.ietf.org/html/rfc7118) je optimální přenos, ale jinými technikami, jako je Server-Sent události a dlouhé dotazování lze použít při těch, které nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="1696e-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="1696e-130">SignalR automaticky rozpozná a inicializaci odpovídající přenos podle funkce podporovány na serveru a klienta.</span><span class="sxs-lookup"><span data-stu-id="1696e-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="1696e-131">Rozbočovače</span><span class="sxs-lookup"><span data-stu-id="1696e-131">Hubs</span></span>

<span data-ttu-id="1696e-132">SignalR centra používá ke komunikaci mezi klienty a servery.</span><span class="sxs-lookup"><span data-stu-id="1696e-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="1696e-133">Rozbočovač je nejdůležitější kanál, který umožňuje klient a server pro volání metody na sobě navzájem.</span><span class="sxs-lookup"><span data-stu-id="1696e-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="1696e-134">SignalR zpracovává odeslání mezi různými počítači automaticky, které klientům umožňuje volat metody na serveru jako snadno jako místní metody a naopak.</span><span class="sxs-lookup"><span data-stu-id="1696e-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="1696e-135">Centra povolit předání silného typu parametry metody, která umožňuje vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="1696e-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="1696e-136">Funkce SignalR poskytuje dva předdefinované rozbočovače protokoly: protokol text na základě JSON a binární protokol založený na [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="1696e-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="1696e-137">MessagePack obvykle vytvoří zpráv menší než při použití formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="1696e-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="1696e-138">Starší prohlížeče musí podporovat [XHR úroveň 2](https://caniuse.com/#feat=xhr2) poskytovat podporu protokolu MessagePack.</span><span class="sxs-lookup"><span data-stu-id="1696e-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="1696e-139">Odesílání zpráv pomocí aktivní přenos rozbočovače pro volání kódu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="1696e-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="1696e-140">Zprávy obsahují název a parametry metody na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="1696e-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="1696e-141">Objekty odeslán jako parametry metody jsou deserializovat pomocí nakonfigurované protokolu.</span><span class="sxs-lookup"><span data-stu-id="1696e-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="1696e-142">Klient se pokusí shodovat s názvem na metodu v kódu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="1696e-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="1696e-143">Pokud je shoda se stane, metodu klienta běží, pomocí dat deserializovat parametru.</span><span class="sxs-lookup"><span data-stu-id="1696e-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1696e-144">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1696e-144">Additional resources</span></span>

* [<span data-ttu-id="1696e-145">Začínáme s SignalR pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1696e-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="1696e-146">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="1696e-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="1696e-147">Centra</span><span class="sxs-lookup"><span data-stu-id="1696e-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="1696e-148">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="1696e-148">JavaScript client</span></span>](xref:signalr/javascript-client)
