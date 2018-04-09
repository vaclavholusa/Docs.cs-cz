---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Úvod do SignalR | Microsoft Docs
author: pfletcher
description: Tento článek popisuje, co je SignalR a některé řešení, které je určen k vytvoření.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0ceca3edc26d35b1155946e60863a84da0bbe592
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-signalr"></a>Úvod do SignalR
====================
podle [Patrik Fletcher](https://github.com/pfletcher)

> Tento článek popisuje, co je SignalR a některé řešení, které je určen k vytvoření. 
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).


## <a name="what-is-signalr"></a>Co je SignalR?

Funkce SignalR technologie ASP.NET je knihovna pro vývojáře využívající technologii ASP.NET, který zjednodušuje proces přidávání funkce webu v reálném čase do aplikací. Funkce webu v reálném čase je schopnost serveru kód nabízené obsah připojeným klientům okamžitě, jakmile je k dispozici, místo aby se server čekat na klienta k žádosti o nová data.

SignalR slouží k přidání žádné řazení funkce "v reálném čase" webu do aplikace ASP.NET. Při chat se často používá jako příklad, můžete provést mnoho více. Kdykoli uživatel aktualizuje na webové stránce zobrazíte nová data nebo stránce implementuje [dlouhé dotazování](http://en.wikipedia.org/wiki/Push_technology#Long_polling) načte nová data, je kandidátem pro použití funkce SignalR. Mezi příklady patří řídicí panely a monitorování aplikací, spolupráce aplikace (např. souběžných úpravy dokumentů), úlohy, Probíhá aktualizace a v reálném čase formuláře.

Funkce SignalR také umožňuje zcela nové typy webových aplikací, které vyžadují vysoká frekvence aktualizace ze serveru, například v reálném čase herní.

Funkce SignalR poskytuje jednoduché rozhraní API pro vytvoření klienta a serveru vzdálených volání procedur (RPC) volají funkce JavaScript v klientovi prohlížeče (a jiné platformy klienta) z kódu .NET na straně serveru. Funkce SignalR také zahrnuje rozhraní API pro správu připojení (například připojit a odpojit událostí) a seskupování připojení.

![Volání metody s SignalR](introduction-to-signalr/_static/image1.png)

SignalR zpracovává správu připojení automaticky a umožňuje vám zprávy všesměrového vysílání pro všechny připojené klienty současně, jako je chatovací místnosti. Také mohou zasílat zprávy do konkrétní klientů. Připojení mezi klientem a serverem je trvalé, na rozdíl od classic připojení protokolu HTTP, které je obnoveno pro každé komunikace směrem.

SignalR podporuje funkce "serveru push", ve kterém můžete volat serverový kód kód klienta v prohlížeči Dnes pomocí vzdáleného volání procedur (RPC), nikoli běžné modelu požadavků a odpovědí na webu.

Můžete škálování aplikací SignalR k tisícům klientů pomocí služby Service Bus, SQL Server nebo [Redis](http://redis.io).

SignalR je open source, přístupný prostřednictvím [Githubu](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR a protokolu WebSocket

SignalR používá nový přenos protokolu WebSocket, kde je k dispozici a spadne zpět na starší přenosy potřeby. Zatímco můžete napsat určitě vaší aplikace pomocí protokolu WebSocket přímo, pomocí funkce SignalR znamená, že mnoho další funkce, které by bylo nutné implementovat bude mít neudělali za vás. Co je nejdůležitější – to znamená, že může kód aplikace využívat výhod protokolu WebSocket bez nutnosti starat o vytváření samostatných kódové cestě pro starší klienty. Funkce SignalR také chrání před starosti o aktualizace protokolu WebSocket, protože SignalR budou nadále být aktualizované kvůli podpoře změny v základní přenos poskytuje jednotné rozhraní aplikace mezi verzemi protokolu WebSocket.

Určitě může vytvořit řešení pomocí protokolu WebSocket samostatně, funkce SignalR poskytuje všechny funkce, budete muset napsat sami, jako je například přechod na další přenosy a úprava aplikace aktualizací implementace protokolu WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Přenosy a případech přejít

SignalR je abstrakci přes některé přenosů, které jsou požadovány pro práci v reálném čase mezi klientem a serverem. Připojení SignalR se spustí jako HTTP a se poté vyzval připojení protokolu WebSocket, pokud je k dispozici. Protokol WebSocket je ideální přenos pro funkci SignalR, protože umožňuje využívat s maximální efektivitou paměti serveru, má nejnižší latenci a má nejvíce základní funkce (například plně duplexní komunikace mezi klientem a serverem), ale má také nejpřísnější požadavky: protokolu WebSocket vyžaduje server musí používat Windows Server 2012 nebo Windows 8 a rozhraní .NET Framework 4.5. Pokud tyto požadavky nejsou splněny, se pokusí používat ostatní přenosy k vytvoření připojení SignalR.

### <a name="html-5-transports"></a>Přenosy, HTML 5

Tyto přenosy závisí na podporu pro [standardu HTML 5](http://en.wikipedia.org/wiki/HTML5). Pokud prohlížeč klienta nepodporuje standardu HTML 5, použije se starší přenosy.

- **Protokol WebSocket** (Pokud server i prohlížeč znamenat mohou podporovat protokolu Websocket). Protokol WebSocket je pouze přenos, který stanoví true trvalého obousměrné připojení mezi klientem a serverem. Má-však také protokolu WebSocket nejpřísnější požadavky; je plně podporovaný jenom v nejnovější verzi aplikace Microsoft Internet Explorer a Google Chrome, Mozilla Firefox a má jenom částečnou implementaci v dalších prohlížečích například Opera a Safari.
- **Události odeslané serverem**, označované také jako EventSource (Pokud je prohlížeč podporuje odeslané události serveru, který je v podstatě všechny prohlížeče s výjimkou aplikace Internet Explorer).

### <a name="comet-transports"></a>Přenosy Comet

Následující přenosy jsou založené na [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) model webové aplikace, ve kterém prohlížeče nebo jiné klienta udržuje uchovávat dlouho požadavek HTTP, který server můžete použít tak, aby nabízel data konkrétně klientovi bez klienta nástroje o to požádá.

- **Navždy rámce** (pro Internet Explorer pouze). Navždy rámce vytvoří skrytá IFrame, který vytváří požadavek na koncový bod na serveru, který není dokončena. Server pak průběžně odešle skriptu klientovi, který se spustí okamžitě, poskytuje jednosměrný v reálném čase připojení ze serveru do klienta. Připojení z klienta na server používá samostatné připojení ze serveru pro připojení klienta, a jako standardní požadavku HTTP, se vytvoří nové připojení pro jednotlivá data, která potřebují k odeslání.
- **AJAX dlouhé dotazování**. Dlouhé dotazování nevytvoří trvalé připojení, ale místo toho dotazuje server s požadavek, který zůstane otevřená, dokud server odpoví, okamžiku zavře připojení a okamžitě vyžádání nového připojení. To může způsobit určité zpoždění při resetuje připojení.

Další informace o jaké přenosy jsou podporovány v rámci kterých konfiguracích najdete v tématu [podporované platformy](supported-platforms.md).

### <a name="transport-selection-process"></a>Výběr proces přenosu

Následující seznam obsahuje kroky, které používá SignalR rozhodnout, který typ přenosu.

1. Pokud v prohlížeči Internet Explorer 8 nebo dřívější verzí, dlouhé dotazování se používá.
2. Pokud je nakonfigurovaný JSONP (tedy `jsonp` parametr je nastaven na `true` při spuštění připojení), dlouhé dotazování se používá.
3. Pokud mezi doménami právě připojení (Pokud SignalR koncový bod není ve stejné doméně jako hostování stránka), bude WebSocket použit, pokud se splní následující kritéria:

   - Klient podporuje CORS (sdílení prostředků různého původu). Informace, na kterých klienti podporují CORS najdete v tématu [CORS v caniuse.com](http://www.caniuse.com/CORS).
   - Klient podporuje protokol WebSocket
   - Server podporuje protokol WebSocket

     Pokud nejsou splněny některé z těchto kritérií, dlouhé dotazování se použije. Další informace o připojení mezi doménami, najdete v části [postup připojení mezi doménami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Pokud není nakonfigurovaný JSONP a připojení není mezi doménami, WebSocket se použijí, pokud klient i server podporovat.
5. Pokud klient nebo server nepodporují protokolu WebSocket, odeslané události serveru se používá, pokud je k dispozici.
6. Pokud události odeslané serveru není k dispozici, dojde k pokusu o navždy rámce.
7. Dlouhé dotazování se používá v případě selhání navždy rámce.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Monitorování přenosů

Můžete určit, jaký přenos vaše aplikace používá povolením protokolování na rozbočovače a otevřete v okně konzoly v prohlížeči.

Povolení protokolování pro vaše Centrum událostí v prohlížeči, přidejte do klientské aplikace následující příkaz:

`$.connection.hub.logging = true;`

- V aplikaci Internet Explorer otevřete stisknutím klávesy F12 nástrojů pro vývojáře a klikněte na kartu konzoly.

    ![V Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- V prohlížeči Chrome otevřete stisknutím kláves Ctrl + Shift + J konzolu.

    ![Konzoly Google Chrome](introduction-to-signalr/_static/image3.png)

Otevřete konzolu a protokolování budete moci zobrazit, které přenosu je stále používán SignalR.

![Konzole v Internet Exploreru zobrazující přenos protokolu WebSocket](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Určení přenos

Vyjednávání přenos trvá prostředky určité množství času a klient server. Pokud se ví, že možnosti klienta, může přenos zadán, při spuštění připojení klienta. Následující fragment kódu ukazuje spuštění pomocí přenosu Ajax dlouhé dotazování, jak se použije, pokud byl označuje, že klient nepodporoval žádný jiný protokol pro připojení:

`connection.start({ transport: 'longPolling' });`

Pokud chcete, aby klient pokusit určité přenosy v pořadí, můžete zadat záložní pořadí. Následující fragment kódu ukazuje snažíme WebSocket a pokud v něm, přejdete přímo na dlouhé dotazování.

`connection.start({ transport: ['webSockets','longPolling'] });`

Řetězcové konstanty pro zadání přenosy jsou definovány takto:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Připojeními a rozbočovači

Rozhraní API SignalR obsahuje dva modely pro komunikaci mezi klienty a servery: trvalé připojeními a rozbočovači.

Připojení představuje jednoduchý koncový bod pro odesílání zpráv jednoho příjemce, seskupené nebo všesměrového vysílání. Trvalé připojení API (v třídě připojení PersistentConnection znázorněná kód .NET) poskytuje, vývojář přímý přístup k nízké úrovně komunikační protokol, který zveřejňuje funkce SignalR. Pomocí připojení komunikační model bude pro vývojáře, kteří použili založeného na připojení rozhraní API jako je Windows Communication Foundation.

Rozbočovač je založena na rozhraní API připojení, která umožňuje klientovi i serveru volat metody na sobě navzájem přímo více vysoké úrovně kanálu. SignalR zpracovává odeslání mezi různými počítači jako podle magic, které klientům umožňuje volat metody na serveru jako snadno jako místní metody a naopak. Pomocí centra komunikační model bude pro vývojáře, kteří použili vzdáleného volání rozhraní API, jako je například .NET Remoting. Použití rozbočovače také umožňuje předat silného typu parametry metody, povolení vazby modelu.

### <a name="architecture-diagram"></a>diagram architektury

Následující diagram znázorňuje vztah mezi rozbočovačů, trvalé připojení a základní technologií používaných pro přenosy.

![Diagram architektury SignalR znázorňující rozhraní API, přenosy a klienty](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Jak fungují rozbočovače

Když kódu na straně serveru volá metody na straně klienta, paket se odesílají přes aktivní přenos, který obsahuje název a parametry metody, která se má volat (když je objekt jako parametru metody, je serializován pomocí JSON). Klient pak odpovídá názvu metoda pro metody definované v kódu na straně klienta. Pokud je nalezen, metoda klienta provést pomocí dat deserializovat parametru.

Volání metody, které je možné monitorovat pomocí nástroje, například [aplikaci Fiddler.](http://fiddler2.com/) Následující obrázek ukazuje volání metody odeslaných ze serveru SignalR do webového prohlížeče klienta v podokně protokoly aplikaci Fiddler. Volání metody, které je odeslané z rozbočovače názvem `MoveShapeHub`, a ke kterému volaná metoda je volána `updateShape`.

![Zobrazení protokolu Fiddler zobrazující provoz SignalR](introduction-to-signalr/_static/image6.png)

V tomto příkladu je název centra označeny `H` parametr; metodu přiřazen název `M` parametr a data odesílána metodu je identifikován s `A` parametr. Vytvoření aplikace, která vygenerovala tuto zprávu v [v reálném čase vysoká frekvence](tutorial-high-frequency-realtime-with-signalr.md) kurzu.

### <a name="choosing-a-communication-model"></a>Výběr modelu komunikace

Většina aplikací by měl použít rozhraní API rozbočovače. Rozhraní API připojení může v následujících případech:

- Formát je nutné zadat skutečné zpráva byla odeslána.
- Vývojář upřednostní pro práci s modelem zasílání zpráv a odesílající spíše než model vzdálené volání.
- Existující aplikace, která používá model zasílání zpráv je právě přesně do použít SignalR.
