---
uid: signalr/overview/performance/signalr-performance
title: "SignalR výkonu | Microsoft Docs"
author: pfletcher
description: "SignalR výkonu"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 4468ee8031afccca847db67bd4b5b263f0a2c5ac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="signalr-performance"></a>SignalR výkonu
====================
podle [Patrik Fletcher](https://github.com/pfletcher)

> Toto téma popisuje postup návrhu pro měření a zlepšit výkon v aplikaci SignalR.
> 
> ## <a name="software-versions-used-in-this-topic"></a>Verze softwaru použitým v tomto tématu
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR verze 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Předchozí verze tohoto tématu
> 
> Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


Poslední prezentace na SignalR výkonu a škálování, najdete v části [škálování webu v reálném čase pomocí funkce SignalR technologie ASP.NET](https://channel9.msdn.com/Events/Build/2013/3-502).

Toto téma obsahuje následující oddíly:

- [Aspekty návrhu](#design)
- [Optimalizace výkonu serveru SignalR](#tuning)
- [Řešení potíží s výkonem](#troubleshooting)
- [Pomocí čítače výkonu SignalR](#perfcounters)
- [Pomocí dalších čítačů výkonu](#othercounters)
- [Další prostředky](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Aspekty návrhu

Tato část popisuje vzorů, které lze provádět při návrhu aplikace SignalR zajistit, že není právě výkonu omezován generování nepotřebné síťový provoz.

### <a name="throttling-message-frequency"></a>Frekvence omezení zpráv

I v aplikaci, která odesílá zprávy s vysoká frekvence (jako například herní aplikací v reálném čase) není nutné odesílat zprávy o několik a sekundu většinu aplikací. Snížit objem provozu, který generuje každého klienta, smyčku zpráv se dají implementovat, fronty a odesílá zprávy nejvýše často pevné sazby (tedy až počet zpráv odešle za sekundu, pokud jsou v té době v zprávy terval k odeslání). Ukázkové aplikace, která omezí generovaný zprávy do určité míry (z klient i server), najdete v části [vysoká frekvence v reálném čase s SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Snižuje velikost zprávy

Snížením velikosti serializovaných objektů můžete snížit velikost zprávy SignalR. V serverovém kódu, pokud odesíláte objekt, který obsahuje vlastnosti, které není nutné přenášet, zabránit tyto vlastnosti serializována pomocí `JsonIgnore` atribut. Názvy vlastností jsou také uloženy ve zprávě; názvy vlastností lze zkrátit pomocí `JsonProperty` atribut. Následující ukázka kódu ukazuje, jak být vyloučena určitá vlastnost odeslání do klienta a jak tak, aby zkrátil názvy vlastností:

**Kód serveru .NET, která popisuje atribut JsonIgnore vyloučení dat z odesílány do klienta a atribut JsonProperty snížit velikost zprávy**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Chcete-li zachovat čitelnost / udržovatelnosti v kódu klienta, názvy zkrácené vlastností, které může být změnu mapování k lidské friendly názvy, po doručení zprávy. Následující příklad kódu ukazuje jeden možný způsob přemapování zkrácení názvy chcete delší, tím, že definujete kontrakt zprávy (mapování) a pomocí `reMap` funkce, která se týkají kontrakt třídu optimalizované zpráv:

**Kód jazyka JavaScript na straně klienta, který změní mapování zkrátit názvy vlastností, abyste čitelná pro člověka názvy**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Názvy lze zkrátit ve zprávách z klienta na server taky stejným způsobem.

Snižuje nároky na paměť (který je množství paměti pro zprávu) zprávy mohou objekt rovněž zlepšit výkon. Například pokud plný rozsah `int` není potřeba, `short` nebo `byte` můžete místo toho použít.

Vzhledem k tomu, že zprávy jsou uloženy ve sběrnici zpráv v paměti serveru, zmenšení velikosti této zprávy můžete také řeší problémy paměti serveru.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Optimalizace výkonu serveru SignalR

Následující nastavení konfigurace slouží k vyladění vašeho serveru pro lepší výkon v aplikaci SignalR. Obecné informace o tom, jak zlepšit výkon v aplikaci ASP.NET najdete v tématu [zlepšení výkonu technologie ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).

**Nastavení konfigurace SignalR**

- **DefaultMessageBufferSize**: ve výchozím nastavení, SignalR uchovává zprávy 1000 v paměti za rozbočovač za připojení. Pokud se používají velké zprávy, to může způsobit problémy s pamětí, které můžete zmírnit po nastavení této hodnoty. Toto nastavení může být nastavena v `Application_Start` obslužné rutiny události v aplikaci ASP.NET nebo v `Configuration` metoda třídy pro spuštění OWIN ve vlastním hostováním aplikaci. Následující příklad ukazuje, jak snížit tato hodnota za účelem snížení nároky na paměť pro vaše aplikace za účelem snížení množství paměti serveru:

    **Kód serveru .NET v souboru Startup.cs pro snížení výchozí velikost vyrovnávací paměti zpráv**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Nastavení konfigurace služby IIS**

- **Maximální počet souběžných požadavků na aplikaci**: zvýšení počtu souběžných IIS požadavky zvýší prostředky serveru k dispozici pro obsluhovat požadavky. Výchozí hodnota je 5 000; Pokud chcete zvýšit toto nastavení, spusťte následující příkazy v příkazovém řádku se zvýšenými oprávněními:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength**: Toto je maximální počet požadavků, ovladač Http.sys zařadí do fronty pro fond aplikací. Při zaplnění fronty, nové požadavky obdrží odpověď 503 "Služba není k dispozici". Výchozí hodnota je 1 000.

    Zkrátit délku fronty pracovního procesu ve fondu aplikací, který je hostitelem vaší aplikace bude šetřit prostředky paměti. Další informace najdete v tématu [pro správu, optimalizaci a konfigurace fondů aplikací](https://technet.microsoft.com/library/cc745955.aspx).

**Nastavení konfigurace ASP.NET**

Tato část obsahuje nastavení konfigurace, které lze nastavit v `aspnet.config` souboru. Tento soubor se nachází v jednom ze dvou umístěních, v závislosti na platformě:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Nastavení ASP.NET, které může zvýšit výkon SignalR zahrnují následující:

- **Maximální souběžných požadavků na jeden procesor**: zvýšením tohoto nastavení může zmírnit kritické body. Chcete-li zvýšit toto nastavení, přidejte následující nastavení konfigurace na `aspnet.config` souboru:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Limit fronty požadavků**: Pokud celkový počet připojení překročí `maxConcurrentRequestsPerCPU` nastavení, ASP.NET spustí omezování požadavků pomocí fronty. Pokud chcete zvýšit velikost fronty, můžete zvýšit `requestQueueLimit` nastavení. K tomu, přidejte následující nastavení konfigurace pro `processModel` uzlu v `config/machine.config` (místo `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Řešení potíží s výkonem

Tato část popisuje způsoby jak najít kritické body ve vaší aplikaci.

### <a name="verifying-that-websocket-is-being-used"></a>Ověření, že je použit protokol WebSocket

Zatímco SignalR používat celou řadu přenosů pro komunikaci mezi klientem a serverem, protokolu WebSocket nabízí využít významně zvýšit výkon a by měl použít, pokud ji podporuje klient a server. Pokud chcete zjistit, pokud klient a server splňovat požadavky protokolu WebSocket, najdete v části [přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports). Pokud chcete určit, jaký přenos se používá v aplikaci, můžete použít nástroje pro vývojáře prohlížeče a v protokolech zobrazíte, jaký přenos se používá pro připojení. Informace o použití nástrojů pro vývoj prohlížeče v Internet Exploreru a Chrome, najdete v části [přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Pomocí čítače výkonu SignalR

Tato část popisuje, jak povolit a používat čítače výkonu SignalR v nalezen `Microsoft.AspNet.SignalR.Utils` balíčku.

### <a name="installing-signalrexe"></a>Instalace signalr.exe

Čítače výkonu lze přidat na server názvem SignalR.exe nástroj. Chcete-li nainstalovat tento nástroj, postupujte takto:

1. V aplikaci Visual Studio, vyberte **nástroje**, **Správce balíčků knihoven**, **spravovat balíčky NuGet pro řešení...**
2. Vyhledejte **signalr.utils**a vyberte možnost instalace.

    ![](signalr-performance/_static/image1.png)
3. Přijměte licenční smlouvu k instalaci balíčku.
4. SignalR.exe se nainstaluje pro `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Instalace s SignalR.exe čítače výkonu

Pokud chcete nainstalovat čítače výkonu SignalR, spusťte SignalR.exe v příkazovém řádku se zvýšenými s parametrem následující:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Chcete-li odebrat čítače výkonu SignalR, spusťte SignalR.exe v příkazovém řádku se zvýšenými s následující parametr:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Čítače výkonu SignalR

Balíček nástroje nainstaluje následující čítače výkonu. "Celkovou" čítače měření počtu událostí od poslední fond aplikací nebo server restartovat.

**Metriky připojení**

Následující metriky měření události životnost připojení, ke kterým dochází. Další informace najdete v tématu [pochopení a zpracování události životnost připojení](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Připojení připojení**
- **Připojení znovu**
- **Připojení, odpojení**
- **Aktuální připojení**

**Zpráva metriky**

Následující metriky měření provoz zprávy generované SignalR.

- **Celkový počet přijatých zprávy připojení**
- **Celkový počet odesílají zprávy připojení**
- **Přijaté zprávy připojení za sekundu**
- **Připojení odeslaných zpráv za sekundu**

**Metriky sběrnice zpráv**

Následující metriky měření provoz prostřednictvím interní sběrnice zpráv SignalR, fronty, ve kterém jsou umístěny všechny příchozí a odchozí zprávy SignalR. Zpráva je **publikováno** při se odesílá nebo všesměrového vysílání. A **odběratele** v tomto kontextu je předplatné na sběrnici zpráv; to musí být roven počtu klientů a samotný server. **Přidělené pracovní** je komponenta, která odesílá data do active připojení, **zaneprázdněn pracovní** je ten, který je aktivně odesílání zprávy.

- **Celkový počet přijatých zpráv sběrnice zpráv**
- **Sběrnice zpráv zprávy přijaté za sekundu**
- **Celkový počet publikovaných zprávy sběrnice zpráv**
- **Sběrnice zpráv zprávy publikované za sekundu**
- **Aktuální odběratele sběrnice zpráv**
- **Celkový počet odběratele sběrnice zpráv**
- **Odběratelé sběrnice zpráv za sekundu**
- **Sběrnice zpráv přidělené pracovní procesy**
- **Sběrnice zpráv vytížených pracovních procesů**
- **Aktuální témata sběrnice zpráv**

**Chyba metriky**

Následující metriky měření chybách vygenerovaných provoz zpráv SignalR. **Centrum řešení** dojít k chybám, když nelze přeložit rozbočovače nebo metody rozbočovače. **Volání rozbočovače** chyby jsou výjimky vydané při vyvolání metody hub. **Přenos** chyby jsou chyby připojení během požadavku nebo odpovědi HTTP.

- **Chyb: Celkový počet všech**
- **Chyby: Všechna za sekundu**
- **Chyby: Celkový počet řešení rozbočovače**
- **Chyby: Centrum řešení za sekundu**
- **Chyby: Celkový počet volání rozbočovače**
- **Chyby: Rozbočovače volání za sekundu**
- **Chyb: Celkový počet přenosu**
- **Chyby: Přenos/s**

<a id="scaleout_metrics"></a>

**Metriky škálování.**

Následující metriky měření provoz a chyby vygenerované zprostředkovatelem škálování. A **datového proudu** v tomto kontextu je jednotka škálování používá zprostředkovatel škálování; jde tabulky, pokud se používá SQL Server, téma, pokud se používá Service Bus a předplatné, pokud se používá Redis. Každý datový proud zajišťuje uspořádaný operací čtení a zápisu; jeden datový proud je potenciální problémová místa škálování, takže snížit této kritický bod je možné zvýšit počet datových proudů. Pokud používáte víc datových proudů, SignalR automaticky distribuovat zprávy (horizontálních) v těchto datových proudů způsobem, který zajistí, že zpráv odeslaných z daného připojení jsou v pořadí.

[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) nastavení ovládacích prvků délka fronty pro odesílání dat o škálování udržované SignalR. Jeho nastavení na hodnotu větší než 0 umístí všechny zprávy ve frontě odeslání k odeslání po jednom nakonfigurovaných propojovací rozhraní systému zasílání zpráv. Pokud velikost fronty překročí nakonfigurovaná délka, následných výzev k odeslání bude okamžitě nezdaří s [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) dokud počet zpráv ve frontě je menší než nastavení znovu. Služby Řízení front je ve výchozím nastavení zakázáno, protože implementované backplanes obvykle mají své vlastní služby Řízení front nebo řízení toku na místě. V případě systému SQL Server sdružování připojení efektivně omezí počet zasílá děje v daném okamžiku.

Ve výchozím nastavení, je použít pouze jeden datový proud pro SQL Server a Redis, pět datových proudů se používají pro Service Bus a služby Řízení front je zakázaná, ale tato nastavení lze změnit prostřednictvím konfigurace na serveru SQL a sběrnice:

**Rozhraní .NET serverový kód pro konfiguraci tabulky počet a fronty délku propojovací rozhraní systému SQL Server**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**Rozhraní .NET serverový kód pro konfiguraci tématu počet a fronty délku propojovací rozhraní systému Service Bus**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

A **ukládání do vyrovnávací paměti** datový proud je ten, který má zadaný chybný stav; když je datový proud v chybovém stavu, všech zpráv odeslaných do propojovacího rozhraní se nezdaří, až do datového proudu už selhal. **Délka fronty pro odeslání** je počet zpráv, které byly odeslány, ale ještě nebyla odeslána.

- **Sběrnice zpráv o škálování zprávy přijaté za sekundu**
- **Celkový počet datových proudů škálování.**
- **Škálování datové proudy Open**
- **Datové proudy škálování ukládání do vyrovnávací paměti**
- **Celkový počet chyb škálování.**
- **Škálování chyby/s**
- **Délka fronty pro odesílání škálování.**

Další informace o co jsou tyto čítače měření najdete v tématu [SignalR škálování s Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Pomocí dalších čítačů výkonu

Následující čítače výkonu může být taky užitečný v monitorování výkonu aplikace.

**Paměť**

- Využívání paměti rozhraním .NET CLR\\# bajtů ve všech haldách (pro w3wp)

**ASP.NET**

- Aktuální ASP.NET\Requests
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- Information\Processor času procesoru

**TCP/IP**

- TCPv6 nebo připojení
- TCPv4 nebo připojení

**Web Service**

- Webová Service\Current připojení
- Webová Service\Maximum připojení

**Dělení na vlákna**

- Rozhraní .NET CLR zamkne a vlákna\\# aktuální logické vláken
- Rozhraní .NET CLR zamkne a vlákna\\# aktuální fyzické vláken

<a id="otherresources"></a>

## <a name="other-resources"></a>Další zdroje

Další informace o výkonu technologie ASP.NET, sledování a ladění najdete v následujících tématech:

- [Přehled výkonnostní ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [Využití vlákno ASP.NET na IIS 7.5, IIS 7.0 a IIS 6.0.](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt; – Element (webové nastavení)](https://msdn.microsoft.com/library/dd560842.aspx)
