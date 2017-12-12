---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: "Povolení trasování SignalR | Microsoft Docs"
author: tfitzmac
description: "Tento dokument popisuje, jak povolit a konfigurovat trasování pro SignalR servery a klienty. Trasování umožňuje zobrazit diagnostické informace o události..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 2f01ab5d66e44cd82634f1b3df1ca6c78b7fd9d5
ms.sourcegitcommit: c07fb5cb5df0a12f9fe6735fcbc90964608fa687
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
<a name="enabling-signalr-tracing"></a>Povolení trasování SignalR
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento dokument popisuje, jak povolit a konfigurovat trasování pro SignalR servery a klienty. Trasování umožňuje zobrazit diagnostické informace o událostech v aplikaci SignalR.
> 
> Toto téma byl původně zapsán pomocí Patrik Fletcher.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET Framework 4.5
> - SignalR verze 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


Pokud je povoleno sledování, vytvoří aplikace SignalR položky protokolu událostí. Z klienta a server může protokolovat události. Trasování na připojení k serveru protokoly, zprostředkovatele o škálování a události sběrnice zpráv. Trasování událostí připojení protokolů klienta. V systému SignalR 2.1 nebo novější protokolů trasování v klientovi celý obsah zprávy volání rozbočovače.

## <a name="contents"></a>Obsah

- [Povolení trasování na serveru](#server)

    - [Protokolování událostí serveru do textových souborů](#server_text)
    - [Protokolování serveru události do protokolu událostí](#server_eventlog)
- [Povolení trasování v rozhraní .NET klienta (aplikace Windows Desktop)](#net_client)

    - [Protokolování událostí klienta pro stolní počítače do konzoly nástroje](#desktop_console)
    - [Protokolování událostí plochy klienta do textového souboru](#desktop_text)
- [Povolení trasování v klientech Windows Phone 8](#phone)

    - [Protokolování událostí pro Windows Phone klienta do uživatelského rozhraní](#phone_ui)
    - [Protokolování událostí pro Windows Phone klienta ke konzole ladění](#phone_debug)
- [Povolení trasování v klientovi JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Povolení trasování na serveru

Povolení trasování na serveru v rámci konfiguračního souboru aplikace (App.config nebo Web.config v závislosti na typu projektu.) Zadáte, které kategorie události do protokolu. V konfiguračním souboru, můžete také určit, zda chcete protokolovat události do textového souboru, protokol událostí systému Windows nebo vlastního protokolu pomocí implementace [TraceListener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Kategorie události serveru zahrnují následující řazení zpráv:

| Zdroj | Zprávy |
| --- | --- |
| SignalR.SqlMessageBus | Instalace zprostředkovatele služby SQL sběrnice zpráv o škálování, operace databáze, chyby a události vypršení časového limitu |
| SignalR.ServiceBusMessageBus | Vytváření tématu zprostředkovatele škálování sběrnice služby a předplatného, chyb a zasílání zpráv události |
| SignalR.RedisMessageBus | Události zprostředkovatele připojení, odpojení a chybové škálování redis |
| SignalR.ScaleoutMessageBus | Události zasílání zpráv o škálování |
| SignalR.Transports.WebSocketTransport | Přenos protokolu WebSocket připojení, odpojení, zasílání zpráv a chybové události |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents přenosu připojení, odpojení, zasílání zpráv a chybové události |
| SignalR.Transports.ForeverFrameTransport | ForeverFrame přenos připojení, odpojení, zasílání zpráv a chybové události |
| SignalR.Transports.LongPollingTransport | LongPolling přenos připojení, odpojení, zasílání zpráv a chybové události |
| SignalR.Transports.TransportHeartBeat | Události připojení, odpojení a keepalive přenosu |
| SignalR.ReflectedHubDescriptorProvider | Události zjišťování rozbočovače |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Protokolování událostí serveru do textových souborů

Následující kód ukazuje, jak povolit trasování pro každou kategorii událostí. Tato ukázka konfiguruje aplikaci, do protokolu událostí textových souborů.

**Kód XML serveru pro povolení trasování**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

Ve výše, kódu `SignalRSwitch` položka určuje [TraceLevel](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelevel(v=vs.110).aspx) používá pro událostí odeslaných do zadaného protokolu. V takovém případě je nastavený na `Verbose` to znamená, všechny ladění a trasování zprávy se zaznamenávají.

Následující výstup zobrazuje záznamy z `transports.log.txt` soubor pro aplikaci pomocí konfiguračního souboru výše. Zobrazuje nové připojení, odebrané připojení a přenosu prezenční signál události.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Protokolování serveru události do protokolu událostí

Chcete-li protokolovat události do protokolu událostí, nikoli textového souboru, změňte hodnoty pro položky v `sharedListeners` uzlu. Následující kód ukazuje, jak se přihlásit do protokolu událostí události serveru:

**Serverový kód XML pro protokolování události do protokolu událostí**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Události se zaznamenávají v protokolu aplikace a jsou k dispozici prostřednictvím v prohlížeči událostí, jak je uvedeno níže:

![Prohlížeč událostí zobrazuje protokoly SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Při použití protokolu událostí, **TraceLevel** k **chyba** zachovat spravovat počet zpráv.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Povolení trasování v rozhraní .NET klienta (aplikace Windows Desktop)

Klient .NET může protokolovat události konzole, s textovým souborem, nebo vlastní protokol pomocí implementace [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx).

Povolení protokolování v rozhraní .NET klienta, nastavte jiné připojení `TraceLevel` vlastnosti [TraceLevels](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) hodnotu a `TraceWriter` vlastnost, která má platnou [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx) instance.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Protokolování událostí klienta pro stolní počítače do konzoly nástroje

Následující kód C# ukazuje, jak do protokolu událostí v rozhraní .NET klienta do konzoly:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Protokolování událostí plochy klienta do textového souboru

Následující kód C# ukazuje, jak do protokolu událostí v rozhraní .NET klienta do textového souboru:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

Následující výstup zobrazuje záznamy z `ClientLog.txt` soubor pro aplikaci pomocí konfiguračního souboru výše. Zobrazuje klient pro připojování k serveru a rozbočovače volání metody klienta s názvem `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Povolení trasování v klientech Windows Phone 8

SignalR aplikací pro aplikace Windows Phone pomocí stejného klienta rozhraní .NET jako desktopové aplikace, ale [Console.Out](https://msdn.microsoft.com/en-us/library/system.console.out(v=vs.110).aspx) a zápis do souboru s [StreamWriter](https://msdn.microsoft.com/en-us/library/system.io.streamwriter(v=vs.110).aspx) nejsou k dispozici. Místo toho je potřeba vytvořit vlastní provádění [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) pro trasování. 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Protokolování událostí pro Windows Phone klienta do uživatelského rozhraní

[Základu kódu SignalR](https://github.com/SignalR/SignalR/archive/master.zip) zahrnuje Windows Phone vzorku, který zapíše výstup trasování do [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) pomocí vlastní [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) implementace volá `TextBlockWriter`. Tato třída lze nalézt v **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** projektu. Při vytváření instance `TextBlockWriter`, předejte aktuální [SynchronizationContext](https://msdn.microsoft.com/en-us/library/system.threading.synchronizationcontext(v=vs.110).aspx)a [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) kde se vytvoří [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) pro trasování výstup:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

Výstup trasování budou zapsány do nového [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) vytvořené v [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) předán v:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Protokolování událostí pro Windows Phone klienta ke konzole ladění

K odeslání výstupu konzoly ladění, místo uživatelského rozhraní, vytvořte implementaci [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) , zapíše se do okna ladění a přiřaďte ji k připojení k [TraceWriter](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) vlastnost:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Informace o trasování budou zapsány do okna ladění v sadě Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Povolení trasování v klientovi JavaScript

Chcete-li povolit protokolování na straně klienta na připojení, nastavte `logging` vlastnost u objektu připojení před voláním `start` metodu pro vytvoření připojení.

**Kód JavaScript klienta pro povolení trasování do konzoly prohlížeče (s vygenerovaný proxy server)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Kód JavaScript klienta pro povolení trasování do konzoly prohlížeče (bez vygenerovaný proxy server)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Pokud je povoleno sledování, JavaScript klienta protokoluje události do konzoly prohlížeče. Pro přístup do konzoly prohlížeče, najdete v části [monitorování přenosy](../getting-started/introduction-to-signalr.md#MonitoringTransports).

Následující snímek obrazovky ukazuje SignalR JavaScript klienta s povolené trasování. V konzole prohlížeče zobrazuje připojení a rozbočovače vyvolání události:

![Události trasování SignalR v konzole prohlížeče](enabling-signalr-tracing/_static/image4.png)
