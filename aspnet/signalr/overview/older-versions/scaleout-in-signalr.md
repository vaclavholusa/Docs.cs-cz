---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: "Úvod do škálování v systému SignalR 1.x | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/29/2013
ms.topic: article
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: ee3384046bf8a0f363aa6801d7a46f68b2bf125a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-scaleout-in-signalr-1x"></a>Úvod do škálování v systému SignalR 1.x
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Patrik Fletcher](https://github.com/pfletcher)

Obecně platí, existují dva způsoby škálování webové aplikace: *škálovat* a *škálovat*.

- Škálování znamená větší serveru (nebo na větší virtuální počítač) pomocí více paměti RAM, procesory atd.
- Horizontální navýšení kapacity znamená přidávat další servery pro zpracování zátěže.

Problém s vertikálním navýšení kapacity je rychle dosáhl limitu velikost počítače. Kromě toho budete muset škálovat. Však při škálovat, klienti mohou získat směrovány na jiné servery. Klient, který je připojený k jednomu serveru nebude příjem zpráv odeslaných z jiného serveru.

![](scaleout-in-signalr/_static/image1.png)

Jedním z řešení je k předávání zpráv mezi servery pomocí komponenty s názvem *propojovacího rozhraní*. S propojovacího rozhraní povoleno každá instance aplikace odesílá zprávy do propojovacího rozhraní a předává je propojovacího rozhraní dalších instancí aplikace. (V elektronické součástky, propojovací rozhraní je skupina paralelní konektorů. Obdobně propojovací rozhraní SignalR připojí více serverů.)

![](scaleout-in-signalr/_static/image2.png)

Funkce SignalR aktuálně poskytuje tři backplanes:

- **Azure Service Bus**. Service Bus je zasílání zpráv infrastrukturu, která umožňuje součásti odesílat zprávy volně párované způsobem.
- **Redis**. Redis je úložišti klíč hodnota v paměti. Redis podporuje vzor publikovat/odebírat ("pub nebo sub") pro odesílání zpráv.
- **SQL Server**. Propojovací rozhraní systému SQL Server zapíše zprávy do tabulky SQL. Propojovacího rozhraní používá službu Service Broker pro efektivní zasílání zpráv. Ale spolupracuje také pokud Service Broker není povolena.

Pokud nasazujete aplikaci na platformě Azure, zvažte použití propojovacího rozhraní Azure Service Bus. Pokud nasazujete do serverové farmy, zvažte systému SQL Server nebo Redis backplanes.

Následující témata obsahují podrobné kurzy pro každý propojovacího rozhraní:

- [Škálování aplikace SignalR službou Azure Service Bus](scaleout-with-windows-azure-service-bus.md)
- [Šklálování aplikace SignalR službou Redis](scaleout-with-redis.md)
- [Šklálování aplikace SignalR SQL Serverem](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementace

V systému SignalR každá zpráva se budou odesílat prostřednictvím sběrnice zpráv. Implementuje sběrnice zpráv [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) rozhraní, která poskytuje abstrakci se publikování a přihlášení k odběru. Backplanes fungovat tak, že nahradíte výchozí **IMessageBus** se sběrnicí určené pro tento propojovacího rozhraní. Je například sběrnici zpráv pro Redis [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), a používá Redis [pub nebo sub](http://redis.io/topics/pubsub) mechanismus odesílat a přijímat zprávy.

Každá instance serveru se připojí k propojovacího rozhraní přes sběrnici. Pokud je odeslána zpráva, přejdete do propojovacího rozhraní a propojovacího rozhraní odešle ji do každého serveru. Když server získá zprávu z propojovacího rozhraní, uloží zprávu v místní mezipaměti. Server pak přináší zprávy pro klienty z místní mezipaměti.

Pro každé připojení klienta je sledovat průběh klienta při čtení datový proud zpráv pomocí kurzoru. (Kurzoru představuje pozici v datovém proudu zpráv.) Pokud se klient odpojí a potom se znovu připojí, zobrazí dotaz, sběrnici pro všechny zprávy, které byly přijaty po hodnotu kurzoru klienta. Samé se stane, když připojení používá [dlouhé dotazování](../getting-started/introduction-to-signalr.md#transports). Po dokončení žádosti dlouhé dotazování klienta otevře nové připojení a požádá o zprávy, které byly přijaty po kurzor.

Funguje mechanismus kurzoru i v případě, že klient se směruje na jiný server na znovu. Propojovacího rozhraní si je vědoma ve všech serverech a nezávisle na tom, který server pro připojení klienta.

## <a name="limitations"></a>Omezení

Pomocí propojovací rozhraní, maximální propustnost je nižší, než je v případě, že klienti komunikují přímo s uzlem jeden server. Je to způsobeno propojovacího rozhraní předává každou zprávu pro každý uzel, takže propojovacího rozhraní se může stát úzkým místem. Zda je toto omezení problém závisí na aplikaci. Tady jsou například některé typické scénáře SignalR:

- [Všesměrové vysílání server](tutorial-server-broadcast-with-aspnet-signalr.md) (například běžícími): Backplanes fungovat i pro tento scénář, protože serveru ovládá rychlost, jakou jsou odesílány zprávy.
- [Klient klient](tutorial-getting-started-with-signalr.md) (například chat): V tomto scénáři můžou propojovacího rozhraní problémové místo, pokud počet zpráv škáluje s počtem klientů; to znamená, pokud počet zpráv zvětšování úměrně Další klienti se připojují.
- [Vysoká frekvence v reálném čase](tutorial-high-frequency-realtime-with-signalr.md) (např. v reálném čase hry): propojovací rozhraní se nedoporučuje pro tento scénář.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Povolení trasování pro škálování SignalR

Pokud chcete povolit trasování pro backplanes, přidat následující části v souboru web.config, v kořenové **konfigurace** element:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
