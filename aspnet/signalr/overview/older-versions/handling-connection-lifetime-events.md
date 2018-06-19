---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: Princip a zpracování události životnost připojení v systému SignalR 1.x | Microsoft Docs
author: pfletcher
description: Tento článek popisuje, jak používat zveřejněný prostřednictvím rozhraní API centra událostí.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 4fe77769c27dd46967da2e1d68791d7142021d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036723"
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>Princip a zpracování události životnost připojení v systému SignalR 1.x
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní Dykstra](https://github.com/tdykstra)

> Tento článek obsahuje přehled SignalR připojení, opětovné připojení a odpojení události, které může zpracovat a nastavení časového limitu a keepalive, která se dají konfigurovat.
> 
> Článek předpokládá, že již máte některé informace o události životního cyklu SignalR a připojení. Úvod do SignalR, najdete v části [SignalR – přehled – Začínáme](index.md). Seznam událostí životnost připojení najdete v následujících zdrojích informací:
> 
> - [Postupy: zpracování události životnost připojení v třídy rozbočovače](index.md)
> - [Postupy: zpracování události životnost připojení v klientech JavaScript](index.md)
> - [Postupy: zpracování události životnost připojení v klientů .NET](index.md)


## <a name="overview"></a>Přehled

Tento článek obsahuje následující části:

- [Připojení životnosti terminologie a scénáře](#terminology)

    - [Připojení SignalR, připojení přenosu a fyzické připojení](#signalrvstransport)
    - [Scénáře odpojení přenosu](#transportdisconnect)
    - [Scénáře odpojení klienta](#clientdisconnect)
    - [Scénáře odpojení serveru](#serverdisconnect)
- [Nastavení časového limitu a funkce keepalive](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Postup změny nastavení časového limitu a funkce keepalive](#changetimeout)
- [Jak upozornit uživatele o odpojení](#notifydisconnect)
- [Opakované nepřetržitě připojení](#continuousreconnect)
- [Jak odpojení klienta v serverovém kódu](#disconnectclientfromserver)

Odkazy na témata referenční dokumentace rozhraní API jsou na rozhraní .NET 4.5 verzi rozhraní API. Pokud používáte rozhraní .NET 4, přečtěte si téma [verze .NET 4 témat rozhraní API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Připojení životnosti terminologie a scénáře

`OnReconnected` Obslužné rutiny událostí v rozbočovači SignalR může spustit přímo po `OnConnected` , ale není po `OnDisconnected` pro daného klienta. Z důvodu, že máte opětovné připojení bez že odpojení si vyžádá je, že existuje několik způsobů, ve kterých se používá slovo "připojení" v systému SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Připojení SignalR, připojení přenosu a fyzické připojení

Tento článek se rozlišit mezi *připojení SignalR*, *přenosu připojení*, a *fyzické připojení*:

- **Připojení SignalR** odkazuje na logický vztah mezi klientem a adresu URL serveru, spravuje pomocí rozhraní API SignalR a jednoznačně identifikuje ID připojení. Data o tento vztah je možný díky SignalR a slouží k navázání připojení pro přenos. Relace je ukončená a SignalR uvolní dat, když klient zavolá `Stop` metoda nebo časový limit je dosaženo při SignalR se pokouší o obnovení připojení ke ztrátě přenosu.
- **Připojení přenosu** odkazuje na logický vztah mezi sebou klient a server udržuje jeden čtyři přenosu rozhraní API: objekty WebSockets, události odeslané serverem navždy rámce nebo dlouhé dotazování. SignalR používá k vytvoření připojení přenosu přenosu rozhraní API a rozhraní API přenosu závisí na existenci fyzické síťové připojení k vytvoření připojení pro přenos. Připojení přenosu ukončí při SignalR ukončí ho nebo při přenosu rozhraní API zjistí, že fyzické připojení je přerušené.
- **Fyzické připojení** odkazuje na fyzických síťových odkazy – vodičům, bezdrátové signály, směrovače, atd.--, které usnadňují komunikace mezi klientským počítačem a počítači se serverem. Fyzické připojení musí být přítomen, aby bylo možné navázat připojení přenosu a aby bylo možné navázat připojení SignalR musí navázat připojení přenosu. Ale nejnovější fyzické připojení není vždy okamžitě ukončí připojení pro přenos nebo připojení SignalR, jak se vysvětluje dále v tomto tématu.

V následujícím diagramu připojení SignalR je reprezentována rozhraní API pro rozbočovače a připojení PersistentConnection API SignalR vrstvy, připojení přenosu je reprezentována vrstvě přenosy a fyzické připojení je reprezentována řádky mezi serverem a klienty.

![Diagram architektury SignalR](handling-connection-lifetime-events/_static/image1.png)

Při volání `Start` metoda v klientovi SignalR, že zadáváte kód klienta SignalR všechny informace, které je nutné za účelem vytvoření fyzické připojení k serveru. Kód klienta SignalR používá tuto informaci k vytvoření žádosti o HTTP a vytvořit fyzické připojení, který používá jednu z metod čtyři přenosu. Pokud selže připojení pro přenos nebo server selže, připojení SignalR není zmizí okamžitě vzhledem k tomu, že klient má stále informace, které potřebuje automaticky znovu vytvořit nové připojení přenosu pro stejnou adresu URL SignalR V tomto scénáři je zahrnuta bez zásahu uživatele aplikace a když kód klienta SignalR vytvoří nové připojení přenosu, se nespustí nové připojení SignalR. Kontinuita připojení SignalR se odrazí v fakt, na který ID připojení, která je vytvořena při volání `Start` metody se nemění.

`OnReconnected` Obslužné rutiny události v centru provede, když je poté, co bylo ztraceno automaticky znovu navázané připojení přenosu. `OnDisconnected` Obslužné rutiny události provádí na konci připojení SignalR. Připojení SignalR můžete ukončit v některém z následujících způsobů:

- Pokud klient zavolá `Stop` metoda, je odeslána zpráva stop na server a klient i server připojení SignalR okamžitě ukončí.
- Po ztrátě připojení mezi klientem a serverem, klient se pokusí znovu připojit a server čeká, klient se znovu připojit. Pokud neúspěšných pokusů o připojení a odpojení časový limit ukončení, klient i server ukončení připojení SignalR. Klient zastaví pokus o opětovné připojení, a server nakládá s jeho reprezentace připojení SignalR.
- Pokud klient zastaví bez nutnosti příležitosti k volání `Stop` metoda, server čeká, klient se znovu připojit a pak se ukončí připojení SignalR po uplynutí časového limitu odpojení.
- Pokud serveru ukončí, klient pokusí znovu připojit (znovu vytvořit připojení přenosu) a pak se ukončí připojení SignalR po uplynutí časového limitu odpojení.

Pokud neexistují žádné problémy s připojením a uživatelská aplikace se ukončí připojení SignalR voláním `Stop` metodu, připojení SignalR a připojení přenosu začínat a končit na přibližně ve stejnou dobu. Následující části popisují podrobněji jiné scénáře.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Scénáře odpojení přenosu

Fyzické připojení může být pomalé nebo může být přerušení připojení. V závislosti na faktorech, jako je délka přerušení může dojít ke ztrátě připojení přenosu. SignalR se potom pokusí znovu navázat připojení přenosu. Někdy připojení přenosu rozhraní API zjistí narušení chodu a zahodí připojení přenosu a SignalR vyhledá okamžitě, že dojde ke ztrátě připojení. V jiných scénářích ani připojení přenosu rozhraní API ani SignalR zjistí okamžitě, že připojení bylo ztraceno. Pro všechny přenosy s výjimkou dlouhým dotazováním ke klientovi SignalR používá funkci s názvem *keepalive* zkontrolujte ztrátě připojení, které se nepodařilo zjistit přenosu rozhraní API. Informace o dlouhé dotazování připojení najdete v tématu [nastavení časového limitu a keepalive](#timeoutkeepalive) dál v tomto tématu.

Při připojení je neaktivní, pravidelně server odešle klientovi paket keepalive. Výchozí frekvence od data, kdy probíhá zápis v tomto článku, se každých 10 sekund. Prostřednictvím naslouchání pro tyto pakety, klienti poznáte, pokud dojde k problému připojení. Pokud paket keepalive neobdrží dle očekávání, po krátkou dobu klient předpokládá, že jsou problémy s připojením například pomalost nebo přerušení. Není-li keepalive stále přijata po delší dobu, klient předpokládá, že byla vyřazena připojení, a začne pokus o opětovné připojení.

Následující diagram znázorňuje klientských a serverových události, které se vyvolá v Typický scénář, když dochází k problémům s fyzické připojení, které nejsou rozpoznány okamžitě pomocí přenosu rozhraní API. Diagram platí v těchto případech:

- Přenos je Websocket, navždy rámečkem nebo události odeslané serverem.
- Zde jsou různých období přerušení v fyzické síťové připojení.
- Přenos rozhraní API není zaregistrují přerušení, takže SignalR spoléhá na funkce keepalive vyhledáním.

![Odpojení přenosu](handling-connection-lifetime-events/_static/image2.png)

Pokud klient přejde do režimu připojení, ale nemůže navázat připojení přenosu v časovém limitu odpojení, server ukončuje připojení SignalR. Pokud k tomu dojde, server zpracuje rozbočovače na `OnDisconnected` metoda a fronty až zprávu o odpojení k odeslání do klienta v případě, že klient spravuje připojit později. Pokud klient znovu připojit, obdrží příkaz odpojení a volání `Stop` metoda. V tomto scénáři `OnReconnected` není spuštěn, když se klient znovu připojí, a `OnDisconnected` není spuštěn, když klient zavolá `Stop`. Následující diagram znázorňuje tento scénář.

![Přerušení přenosu - časový limit serveru](handling-connection-lifetime-events/_static/image3.png)

Události životního cyklu připojení SignalR, které může být zvýšena na straně klienta jsou následující:

- `ConnectionSlow`událost klienta.

    Vyvolá, když přednastavené podíl keepalive časový limit uplynul od poslední zprávy nebo byl přijat příkaz ping keepalive. Keepalive výchozí časový limit upozornění je 2/3 keepalive časového limitu. Časový limit keepalive je 20 sekund, takže dojde k upozornění na sekundám 13.

    Ve výchozím nastavení server odešle příkazy ping keepalive každých 10 sekund a klient vyhledává příkazy ping keepalive o každé 2 sekundy (jednu třetinu rozdíl mezi keepalive hodnota časového limitu a hodnota keepalive časový limit upozornění).

    Pokud přenos rozhraní API bude vědět, že odpojení si vyžádá, SignalR může být informováni o odpojení před předá keepalive časový limit upozornění. V takovém případě `ConnectionSlow` nebude vyvolána událost a SignalR přejde přímo na `Reconnecting` událostí.
- `Reconnecting`událost klienta.

    Vyvolá, když (a) přenosu rozhraní API zjistí, že připojení dojde ke ztrátě nebo nebo (b) keepalive časový limit uplynul od poslední zprávy nebo byl přijat příkaz ping keepalive. Kód klienta SignalR začne, pokus o opětovné připojení. Tato událost může zpracovat, pokud má vaše aplikace určitou akci při ztrátě připojení přenosu. Keepalive výchozího časového limitu je aktuálně 20 sekund.

    Pokud váš klientský kód se pokusí volání metody rozbočovače SignalR je v režimu připojení, se pokusí odeslat příkaz SignalR. Ve většině případů, budou tyto pokusy o neúspěšné, ale v některých případech může být úspěšné. Funkce SignalR pro události odeslané serverem, navždy rámečku a dlouhé dotazování přenosy, používá dva komunikační kanály, ten, který klient používá k odesílání zpráv a jeden, který používá pro příjem zpráv. Kanál, který slouží pro příjem je trvale otevřené a, je ten, který je uzavřen, když dojde k přerušení fyzické připojení. Kanál používá k odeslání zůstanou k dispozici, tak pokud obnovení fyzického připojení volání metody od klienta k serveru může být úspěšné předtím, než je obnoveno kanál receive. Návratová hodnota nebude přijat, dokud SignalR znovu otevře kanál, který slouží pro přijetí.
- `Reconnected`událost klienta.

    Vyvolá, když je znovu navázat připojení přenosu. `OnReconnected` Provede obslužné rutiny událostí v rozbočovači.
- `Closed`událost klienta (`disconnected` událostí v jazyce JavaScript).

    Vyvolá, když vyprší časový limit odpojení při kód klienta SignalR se pokouší znovu připojit po ztrátě připojení přenosu. Odpojit výchozí časový limit je 30 sekund. (Tato událost je aktivována i po ukončení připojení, protože `Stop` metoda je volána.)

Události životního cyklu má být aktivována nemusí jakékoli připojení, způsobit přerušení připojení přenosu, které nejsou zjištěny nástrojem přenosu rozhraní API a není zpoždění příjem keepalive příkazy ping ze serveru po dobu delší než časový limit upozornění keepalive.

Některé sítě v prostředích úmyslně zavřete nečinné připojení a další funkce keepalive paketů je pomáhá to zabránit umožňují tyto sítě vědět, že připojení SignalR se používá. Ve výjimečných případech nemusí být výchozím intervalu příkazy ping keepalive dostatek čímž znemožní připojení uzavřené. V takovém případě můžete nakonfigurovat příkazy ping keepalive zasílat častěji. Další informace najdete v tématu [nastavení časového limitu a keepalive](#timeoutkeepalive) dál v tomto tématu.

> [!NOTE] 
> 
> [!IMPORTANT]
> Posloupnost událostí, které jsou zde popsané není zaručena. SignalR díky všechny pokusy o vyvolávání událostí životnost připojení předvídatelný způsobem podle toto schéma, ale existuje mnoho variant události sítě a mnoha způsoby, ve kterých je zpracovávat základní architektury komunikace například přenosu rozhraní API. Například `Reconnected` událost nemusí vyvolá, když klient znovu připojí, nebo `OnConnected` obslužná rutina na serveru může spustit po neúspěšném pokusu o navázání připojení. Toto téma popisuje pouze důsledky, které by obvykle vytvořeny některé typické okolností.


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Scénáře odpojení klienta

V prohlížeči klienta kód klienta SignalR, která udržuje připojení SignalR běží v kontextu JavaScript na webové stránce. Má proto připojení SignalR má koncovou při navigaci z jedné stránky do jiného a že je proč máte více připojení s ID více připojení Pokud připojíte více okna prohlížeče nebo karty. Když uživatel zavře okno prohlížeče nebo na kartě nebo přejde na novou stránku nebo aktualizuje stránku, připojení SignalR okamžitě skončí kvůli tomu, kód klienta SignalR zpracovává tohoto prohlížeče událostí a počet volání `Stop` metoda. V těchto scénářích, nebo všechny klientské platformy, kdy aplikace volá `Stop` metody `OnDisconnected` obslužné rutiny události okamžitě spustí na serveru a klienta vyvolá `Closed` událostí (událost jmenuje `disconnected` v JavaScript).

Pokud klientská aplikace nebo počítač, který je spuštěn na dojde k chybě nebo přejde do režimu spánku (například když uživatel nezavře přenosný počítač), server není informována o co se stalo. Jde o server zná, ztrátě klienta může být způsobeno přerušení připojení a klient může pokus o opětovné připojení. Proto v těchto scénářích server čeká, umožnit klienta aby mohli znovu připojit a `OnDisconnected` nespustí, dokud nevyprší časový limit odpojení (přibližně 30 sekund ve výchozím nastavení). Následující diagram znázorňuje tento scénář.

![Klientské počítače se nezdařilo](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Scénáře odpojení serveru

Pokud server přejde do režimu offline--restartování, selže, doména aplikace recykluje atd – výsledkem mohou být podobná ke ztrátě připojení, nebo přenosu rozhraní API a SignalR možné, že znáte okamžitě, že je server pryč, a SignalR může začít pokus o opětovné připojení bez vyvolání `ConnectionSlow` událostí. Pokud klient přejde do režimu připojení a pokud server obnoví nebo restartování nebo nový server je uvést do režimu online před vypršením doby vypršení časového limitu odpojení, klient bude znovu obnovené nebo nový server. V takovém případě se připojení SignalR pokračuje v klientovi a `Reconnected` událost se vyvolá. Na prvním serveru `OnDisconnected` nikdy proveden a na novém serveru, `OnReconnected` je provést, i když `OnConnected` byla pro tohoto klienta na tomto serveru před nikdy spuštěna. (Efekt je, že stejné, když se klient znovu připojí ke stejnému serveru po recyklaci domény restartování nebo aplikace, protože při jeho restartování serveru nemá žádné paměti připojení předchozí aktivity.) Následující diagram předpokládá, že přenosu rozhraní API se dozví o ztrátě připojení okamžitě, proto `ConnectionSlow` událost není aktivována.

![Selhání serveru a opětovného připojení](handling-connection-lifetime-events/_static/image5.png)

Pokud server není k dispozici v rámci časového limitu odpojení, ukončí připojení SignalR. V tomto scénáři `Closed` událostí (`disconnected` v klientech JavaScript) se vyvolá na straně klienta, ale `OnDisconnected` nikdy nazývá na serveru. Následující diagram předpokládá, že přenosu rozhraní API není zaregistrují ke ztrátě připojení, tak se zjišťují pomocí funkce keepalive SignalR a `ConnectionSlow` událost se vyvolá.

![Selhání serveru a vypršení časového limitu](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Nastavení časového limitu a funkce keepalive

Výchozí hodnota `ConnectionTimeout`, `DisconnectTimeout`, a `KeepAlive` hodnoty jsou vhodné pro většinu scénářů, ale můžete změnit, pokud vaše prostředí disponuje zvláštními potřebami. Například pokud vaše síťové prostředí dojde k ukončení připojení, které jsou v nečinnosti, po dobu 5 sekund, budete možná muset snížit hodnota keepalive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Toto nastavení představuje dobu, kterou má připojení přenosu zůstat otevřené a čeká na odpověď před jeho zavřením a otevřením nového připojení. Výchozí hodnota je 110 sekund.

Toto nastavení platí, pouze když funkce keepalive je zakázáno, které se obvykle vztahují pouze na na dlouhém dotazování přenosu. Následující diagram znázorňuje účinek tohoto nastavení na typ long dotazování připojení přenosu.

![Dlouhé dotazování připojení přenosu](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>Hodnota DisconnectTimeout

Toto nastavení představuje dobu čekání po připojení přenosu dojde ke ztrátě, než se vyvolá `Disconnected` událostí. Výchozí hodnota je 30 sekund. Když nastavíte `DisconnectTimeout`, `KeepAlive` automaticky nastavena na hodnotu 1/3 `DisconnectTimeout` hodnotu.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Toto nastavení představuje dobu čekání před odesláním paketu keepalive přes nečinné připojení. Výchozí hodnota je 10 sekund. Tato hodnota nesmí být víc než 1/3 `DisconnectTimeout` hodnotu.

Pokud chcete nastavit obě `DisconnectTimeout` a `KeepAlive`, nastavte `KeepAlive` po `DisconnectTimeout`. V opačném případě vaše `KeepAlive` nastavení budou přepsána při `DisconnectTimeout` automaticky nastaví `KeepAlive` na 1/3 hodnota časového limitu.

Pokud chcete zakázat funkci keepalive, nastavte `KeepAlive` na hodnotu null. Funkce Keepalive je automaticky zakázán pro long dotazování přenosu.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Postup změny nastavení časového limitu a funkce keepalive

Chcete-li změnit výchozí hodnoty pro tato nastavení, nastavte je v `Application_Start` ve vaší *Global.asax* souboru, jak je znázorněno v následujícím příkladu. Hodnoty zobrazené v ukázkovém kódu jsou stejné jako výchozí hodnoty.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Jak upozornit uživatele o odpojení

V některých aplikacích můžete zobrazit zprávu pro uživatele, pokud nejsou potíže s připojením k. Máte několik možností, jak a kdy k tomu. Následující ukázky kódu jsou pro JavaScript klienta pomocí vygenerovaný proxy server.

- Zpracování `connectionSlow` událost se má zobrazit zprávu co nejrychleji SignalR si je vědoma potíže s připojením, než se přejde do režimu připojení.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Zpracování `reconnecting` událost se má zobrazit zprávu, když SignalR je vědět, že odpojení si vyžádá a přechází do režimu připojení.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Zpracování `disconnected` událostí k zobrazení zprávy při pokusu o připojení vypršel. V tomto scénáři je jediný způsob, jak znovu navázat spojení se serverem znovu restartujte připojení SignalR voláním `Start` metoda, která vytvoří nové ID připojení. Následující příklad kódu používá příznak a ujistěte se, že vydáte oznámení pouze po opětovně se připojujícího vypršení časového limitu, není po normální mohli připojení SignalR způsobené volání `Stop` metoda.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Opakované nepřetržitě připojení

V některých aplikacích můžete chtít automaticky znovu navázat připojení poté, co byl ztracen a zopakovat pokus o připojení vypršel. Pokud chcete to udělat, můžete zavolat `Start` metoda z vaší `Closed` obslužné rutiny události (`disconnected` obslužné rutiny události na klientech JavaScript). Můžete chtít čekání dobu před voláním `Start` předejdete tím příliš často při serveru nebo fyzické připojení jsou k dispozici. Následující ukázka kódu je pro JavaScript klienta pomocí vygenerovaný proxy server.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Na potenciální problém znát v mobilních klientů je, že průběžné opětovné připojení pokusy, kdy server nebo fyzické připojení není k dispozici může způsobit zbytečné baterie vyprazdňování.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Jak odpojení klienta v serverovém kódu

SignalR verze 1.1.1 nemá integrovaného serveru rozhraní API pro odpojení klientů. Existují [plány pro přidání této funkce v budoucnu](https://github.com/SignalR/SignalR/issues/2101). V aktuální verzi SignalR je nejjednodušší způsob, jak odpojení klienta od serveru implementovat metodu odpojení na straně klienta a volejte tuto metodu ze serveru. Následující příklad kódu ukazuje metodu odpojení pro JavaScript klienta pomocí vygenerovaný proxy server.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Zabezpečení - ani tuto metodu pro odpojení klienti ani navrhované integrované rozhraní API bude zabývat scénář napadené klientů, kteří jsou spuštění škodlivého kódu, protože klienti může znovu připojit nebo může odstranit kód napadené `stopClient` metoda nebo změňte Jak funguje. Na příslušné místo k implementaci stavová odmítnutí služby (DOS) ochrany není v rámci nebo vrstvě serveru, ale v front-end infrastruktury.
