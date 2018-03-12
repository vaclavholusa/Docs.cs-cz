---
title: "Úvod do funkce SignalR technologie ASP.NET Core"
author: rachelappel
description: "Zjistěte, jak knihovny ASP.NET Core SignalR zjednodušuje přidávání funkce webu v reálném čase do aplikací."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction-signalr-core
ms.openlocfilehash: d4ad9bb1910a3339ac8d0d8ff740417f4e7262b7
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="introduction-to-signalr"></a>Úvod do SignalR

Podle [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>Co je SignalR?

Jádro ASP.NET SignalR je do knihovny, která zjednodušuje přidávání funkce webu v reálném čase do aplikací. Funkce webu v reálném čase umožňuje kódu na straně serveru k obsahu nabízených klientům okamžitě.

Vhodnými kandidáty pro SignalR:

* Aplikace, které vyžadují vysoká frekvence aktualizace ze serveru. Příklady jsou herní, sociálních sítí, hlasování, aukce, mapy a GPS aplikace.
* Řídicí panely a monitorování aplikací. Mezi příklady patří společnosti řídicí panely, rychlých prodeje aktualizace, nebo cestují výstrahy.
* Spolupráce aplikace. Aplikace tabulí a tým splňuje softwaru jsou příklady spolupráci aplikací.
* Aplikace, které vyžadují oznámení. Sociálních sítí, e-mailu, konverzace, hry, cesta výstrahy a mnoho dalších aplikací používat oznámení.

Funkce SignalR poskytuje rozhraní API pro vytvoření klienta a serveru [vzdálených volání procedur (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Vzdálených volání procedur volají funkce JavaScript na klientských počítačích z kódu .NET Core straně serveru.

Funkce SignalR pro ASP.NET Core:

* Provádí správu připojení automaticky.
* Umožňuje všesměrové vysílání zprávy pro všechny připojené klienty současně. Například chatovací místnosti.
* Umožňuje odesílání zpráv do konkrétní klienti nebo skupiny klientů.
* Je open source v [Githubu](https://github.com/aspnet/signalr).
* Výborně škáluje.

Připojení mezi klientem a serverem je trvalé, na rozdíl od připojení HTTP.

## <a name="transports"></a>Přenosy

SignalR přehledů v rámci počtu techniky pro vytváření aplikací webu v reálném čase. [Technologie WebSockets](https://tools.ietf.org/html/rfc7118) je optimální přenos, ale jinými technikami, jako je Server-Sent události a dlouhé dotazování lze použít při těch, které nejsou k dispozici. SignalR automaticky rozpozná a inicializaci odpovídající přenos podle funkce podporovány na serveru a klienta.

## <a name="hubs-and-endpoints"></a>Koncové body a rozbočovače

SignalR používá koncové body centra a ke komunikaci mezi klienty a servery. Rozhraní API centra pokrývá většinu scénářů.

Rozbočovač je založena na koncový bod rozhraní API umožňující klient a server pro volání metody na sobě navzájem vysoké úrovně kanálu. SignalR zpracovává odeslání mezi různými počítači automaticky, které klientům umožňuje volat metody na serveru jako snadno jako místní metody a naopak. Centra povolit předání silného typu parametry metody, která umožňuje vazby modelu. Funkce SignalR poskytuje dva předdefinované rozbočovače protokoly: protokol text na základě JSON a binární protokol založený na [MessagePack](https://msgpack.org/).  MessagePack obvykle vytvoří zpráv menší než při použití formátu JSON. Starší prohlížeče musí podporovat [XHR úroveň 2](https://caniuse.com/#feat=xhr2) poskytovat podporu protokolu MessagePack.

Odesílání zpráv pomocí aktivní přenos rozbočovače pro volání kódu na straně klienta. Zprávy obsahují název a parametry metody na straně klienta. Objekty odeslán jako parametry metody jsou deserializovat pomocí nakonfigurované protokolu. Klient se pokusí shodovat s názvem na metodu v kódu na straně klienta. Pokud je shoda se stane, metodu klienta běží, pomocí dat deserializovat parametru.

Koncové body zadejte nezpracovaná rozhraní API soketu jako povolením číst a zapisovat z klienta. Je to na vývojáře pro zpracování seskupení, všesměrové vysílání a další funkce. Rozhraní API centra je postavená na vrstvě koncové body.

Následující diagram znázorňuje vztah mezi rozbočovače, koncových bodů a klienty.

![Mapa SignalR](introduction-signalr-core/_static/signalr-core-architecture.png)

## <a name="related-resources"></a>Související informační zdroje

[Začínáme s SignalR pro ASP.NET Core](xref:signalr/get-started-signalr-core)
