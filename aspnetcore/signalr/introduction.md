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
# <a name="introduction-to-aspnet-core-signalr"></a>Úvod do ASP.NET Core SignalR

Podle [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>Co je SignalR?

Funkce SignalR technologie ASP.NET Core je knihovna, která zjednodušuje přidávání funkce webu v reálném čase do aplikací. Funkce webu v reálném čase umožňuje okamžitě kódu na straně serveru předávaný obsah do klientů.

Vhodnými kandidáty pro funkci SignalR:

* Aplikace, které vyžadují vysoká frekvence aktualizace ze serveru. Příkladem jsou hry, sociálních sítích, hlasování, aukce, mapy a GPS aplikace.
* Řídicí panely a monitorování aplikace. Příklady zahrnují společnosti řídicích panelů, rychlých aktualizací prodeje, nebo cestují výstrahy.
* Aplikace pro spolupráci. Aplikace tabule a týmu splnění softwaru jsou příklady aplikací pro spolupráci.
* Aplikace, které vyžadují oznámení. Sociálních sítí, e-mailu, konverzace, využívají hry, cestování výstrahy a mnoha jiných aplikací oznámení.

Funkce SignalR poskytuje rozhraní API pro vytváření server klient [vzdálených volání procedur (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Vzdálených volání procedur volají funkce JavaScript na klientských počítačích z kódu .NET Core na straně serveru.

Funkce SignalR technologie ASP.NET Core:

* Správa připojení automaticky zpracovává.
* Umožňuje všesměrové vysílání zpráv do všech připojených klientů současně. Například chatovací místnosti.
* Umožňuje odesílání zpráv do konkrétních klientů nebo skupiny klientů.
* Open source v [Githubu](https://github.com/aspnet/signalr).
* Škálovatelné.

Připojení mezi klientem a serverem je trvalé, na rozdíl od připojení HTTP.

## <a name="transports"></a>Přenosy

Funkce SignalR přehledů přes několik technik pro vytváření aplikací webu v reálném čase. [Protokoly Websocket](https://tools.ietf.org/html/rfc7118) je optimální přenosu, ale jiné postupů, jako jsou události Server-Sent a dlouhý interval dotazování se dá použít při ty nejsou k dispozici. Bude automaticky rozpoznávat a inicializovat příslušné přenosu na základě funkcí podporován na serveru a klienta SignalR.

## <a name="hubs"></a>Rozbočovače

Rozbočovače SignalR používá ke komunikaci mezi klienty a servery.

Centrum je základní kanál, který umožňuje klientem a serverem pro volání metod na sobě navzájem. Funkce SignalR zpracovává odeslání přes hranice počítač automaticky, umožňuje klientům volání metod na serveru jako snadno jako místní metody a naopak. Rozbočovače povolit předávání silného typu parametrů k metodám, což umožňuje vazby modelu. Funkce SignalR poskytuje dva protokoly integrované centra: textového protokolu na základě JSON a binární protokol založený na [MessagePack](https://msgpack.org/).  MessagePack obvykle vytvoří zpráv menší než při použití formátu JSON. Starší prohlížeče musí podporovat [XHR úrovně 2](https://caniuse.com/#feat=xhr2) k zajištění podpory protokolu MessagePack.

Odesílání zpráv pomocí aktivní přenos volání rozbočovače kód na straně klienta. Zprávy obsahují název a parametry metody na straně klienta. Objekty, které jako parametry metody jsou deserializaci pomocí nakonfigurovaný protokol. Klient se pokusí shodovat s názvem metody v kódu na straně klienta. Když nastane shoda, metoda klienta spouští pomocí deserializovat parametr data.

## <a name="additional-resources"></a>Další zdroje

* [Začínáme s knihovnou SignalR pro ASP.NET Core](xref:tutorials/signalr)
* [Podporované platformy](xref:signalr/supported-platforms)
* [Centra](xref:signalr/hubs)
* [Klient JavaScriptu](xref:signalr/javascript-client)
