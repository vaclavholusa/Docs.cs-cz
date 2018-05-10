---
title: Úvod do základní ASP.NET SignalR
author: rachelappel
description: Zjistěte, jak knihovny ASP.NET Core SignalR zjednodušuje přidání funkcí v reálném čase do aplikací.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: f05b7cbf05372dc5d5cdadaf5a534d7a9d9bfecc
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="introduction-to-aspnet-core-signalr"></a>Úvod do základní ASP.NET SignalR

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
* Škálovatelná.

Připojení mezi klientem a serverem je trvalé, na rozdíl od připojení HTTP.

## <a name="transports"></a>Přenosy

SignalR přehledů v rámci počtu techniky pro vytváření aplikací webu v reálném čase. [Technologie WebSockets](https://tools.ietf.org/html/rfc7118) je optimální přenos, ale jinými technikami, jako je Server-Sent události a dlouhé dotazování lze použít při těch, které nejsou k dispozici. SignalR automaticky rozpozná a inicializaci odpovídající přenos podle funkce podporovány na serveru a klienta.

## <a name="hubs"></a>Rozbočovače

SignalR centra používá ke komunikaci mezi klienty a servery.

Rozbočovač je nejdůležitější kanál, který umožňuje klient a server pro volání metody na sobě navzájem. SignalR zpracovává odeslání mezi různými počítači automaticky, které klientům umožňuje volat metody na serveru jako snadno jako místní metody a naopak. Centra povolit předání silného typu parametry metody, která umožňuje vazby modelu. Funkce SignalR poskytuje dva předdefinované rozbočovače protokoly: protokol text na základě JSON a binární protokol založený na [MessagePack](https://msgpack.org/).  MessagePack obvykle vytvoří zpráv menší než při použití formátu JSON. Starší prohlížeče musí podporovat [XHR úroveň 2](https://caniuse.com/#feat=xhr2) poskytovat podporu protokolu MessagePack.

Odesílání zpráv pomocí aktivní přenos rozbočovače pro volání kódu na straně klienta. Zprávy obsahují název a parametry metody na straně klienta. Objekty odeslán jako parametry metody jsou deserializovat pomocí nakonfigurované protokolu. Klient se pokusí shodovat s názvem na metodu v kódu na straně klienta. Pokud je shoda se stane, metodu klienta běží, pomocí dat deserializovat parametru.

## <a name="additional-resources"></a>Další zdroje

* [Začínáme s SignalR pro ASP.NET Core](xref:signalr/get-started)
* [Podporované platformy](xref:signalr/supported-platforms)
* [Centra](xref:signalr/hubs)
* [Klient JavaScriptu](xref:signalr/javascript-client)
