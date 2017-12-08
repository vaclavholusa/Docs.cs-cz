---
uid: signalr/overview/getting-started/supported-platforms
title: "Podporované platformy | Microsoft Docs"
author: pfletcher
description: "Tento článek popisuje, jaké klienty a servery jsou podporované systémem SignalR."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 7f41017a2a8c058c01fe6f89a2503eb5fa77048e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="supported-platforms"></a>Podporované platformy
====================
podle [Patrik Fletcher](https://github.com/pfletcher)

> Tento článek popisuje, jaké klienty a servery jsou podporované systémem SignalR. 
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


SignalR je podporován v rámci různých konfigurací klienta a serveru. Kromě toho každá možnost přenosu má sadu požadavků své vlastní; Pokud požadavky na systém pro přenos nejsou k dispozici, bude převzetí služeb při selhání další přenosy řádně SignalR. Další informace o přenosy, které podporuje funkci SignalR naleznete v tématu [přenosy a případech Přejít](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Požadavky na systém Server

Součást serveru SignalR může být hostovaný na celou řadu konfigurací serveru. Tato část popisuje podporované verze operačních systémů, rozhraní .NET framework, Internet Information Server a ostatní součásti.

### <a name="supported-server-operating-systems"></a>Podporované serverové operační systémy

Součást serveru SignalR může být hostovaný v následujících operačních systémů serveru nebo klienta. Všimněte si, že pro SignalR Websocket používat Windows Server 2012 nebo Windows 8 požadované (protokolu WebSocket lze použít na weby systému Windows Azure, dokud v lokalitě verze rozhraní .NET framework je nastavená na 4.5 a webové sokety je povolená na stránce konfigurace lokality).

- Windows Server 2012
- Windows Server 2008 r2
- Windows 8
- Windows 7
- Microsoft Azure

### <a name="supported-server-net-framework-version"></a>Verze rozhraní .NET Framework podporovaných serveru

SignalR 2 je podporován pouze v rozhraní .NET Framework 4.5. Najdete v článku [doporučené aktualizace](#updates) části aktualizací, které zlepšují spolehlivost, kompatibility, stability a výkonu.

### <a name="supported-server-iis-versions"></a>Verze služby IIS podporovanou serveru

Pokud SignalR hostovaný ve službě IIS, jsou podporovány následující verze. Poznamenat, že pokud se používá operační systém klienta, například pro vývoj (Windows 8 nebo Windows 7), úplná verze služby IIS nebo Cassini nesmí použít, vzhledem k tomu, že budou existovat omezení 10 souběžných připojení, která jsou uložena, a které bude velmi rychle dostupný od připojení nejsou přechodná, často se obnovil a jsou odstraněny okamžitě po už používá. Služba IIS Express je třeba použít v klientských operačních systémech.

Všimněte si také, že pro SignalR používat protokol WebSocket, IIS 8 nebo IIS 8 Express se musí použít, server musí používat systém Windows 8, Windows Server 2012 nebo novější, a protokolu WebSocket musí být povolena ve službě IIS. Informace o tom, jak povolit protokolu WebSocket ve službě IIS najdete v tématu [podpory protokolu WebSocket IIS 8.0](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- Služba IIS 8 nebo IIS 8 Express.
- Služba IIS 7 a 7.5. Podpora pro [adresy URL bez přípony](https://support.microsoft.com/kb/980368) je vyžadován.
- Služba IIS musí být spuštěna v integrovaném režimu; klasický režim není podporován. Zpráva zpoždění až 30 sekund může došlo, pokud služba IIS běží v klasickém režimu použití přenosu Server-Sent události.
- Hostování aplikace musí být spuštěn v režimu úplný vztah důvěryhodnosti.

## <a name="client-system-requirements"></a>Požadavky na systém klienta

SignalR lze použít v různých klientských platforem. Tato část popisuje požadavky na systém pro použití SignalR ve webových prohlížečů, aplikací klasické pracovní plochy Windows, aplikace Silverlight a mobilní zařízení.

### <a name="web-browsers"></a>webové prohlížeče

SignalR lze použít v různých webové prohlížeče, ale obvykle jsou podporovány pouze nejnovější dvě verze.

Aplikace, které používají SignalR v prohlížečích musí používat verzi jQuery 1.6.4 nebo hlavní novější verze (například 1.7.2, 1.8.2 nebo 1.9.1).

SignalR lze použít v následujících prohlížečů:

- Verze aplikace Microsoft Internet Explorer 8, 9, 10 a 11. Moderní, počítače a mobilní verze jsou podporovány.
- Mozilla Firefox: aktuální verze - 1, verze systému Windows a Mac.
- Google Chrome: aktuální verze - 1, verze systému Windows a Mac.
- Safari: aktuální verze - 1, verze Mac a iOS.
- Opera: aktuální verze - 1, pouze v systému Windows.
- Prohlížeč systému Android

Kromě nutnosti některé prohlížeče, různé přenosy, které používá SignalR mít vlastní požadavky. Podporovány jsou následující přenosy pod následující konfigurace:

<a id="browser"></a>

**Požadavky na webový prohlížeč přenosu**

| Přenos | Internet Explorer | Chrome (Windows nebo iOS) | Firefox | Safari (OSX nebo iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| Technologie WebSockets | 10+ | aktuální - 1 | aktuální - 1 | aktuální - 1 | Není k dispozici |
| Události odeslané serverem | Není k dispozici | aktuální - 1 | aktuální - 1 | aktuální - 1 | Není k dispozici |
| ForeverFrame | 8+ | Není k dispozici | Není k dispozici | Není k dispozici | 4.1 |
| Dlouhým dotazováním | 8+ | aktuální - 1 | aktuální - 1 | aktuální - 1 | 4.1 |

\*: 6 + je nutná pro plnou funkčnost.

#### <a name="unsupported-browsers"></a>Nepodporované prohlížeče

Při SignalR *může* spustit bez významných problémech ve starší verze prohlížeče, jsme neprovádějte testování aktivně SignalR v nich a obecně nelze vyřešit chyby, které se mohou objevit v nich.

### <a name="windows-desktop-and-silverlight-applications"></a>Windows Desktop a aplikacím Silverlight

Kromě spuštění ve webovém prohlížeči, je možné hostovat SignalR v klientovi Windows samostatné nebo aplikacím Silverlight. Aplikace Windows Desktop a Silverlight SignalR mají následující požadavky.

- Aplikace, které používají .NET 4 jsou podporovány v systému Windows XP SP3 nebo vyšší.
- Aplikace, které používají rozhraní .NET Framework 4.5 jsou podporovány v systému Windows Vista nebo novější.

Kromě operačního systému a požadavky na rozhraní .NET framework přenosy, které jsou k dispozici pro SignalR mít vlastní požadavky. Podporovány jsou následující přenosy pod následující konfigurace:

**Windows Desktop a požadavky na přenos Silverlight**

| Přenos | Aplikace .NET | Silverlight |
| --- | --- | --- |
| Webové sokety | Windows 8 + a rozhraní .NET 4.5 + | Není k dispozici |
| Navždy rámce | Není k dispozici | Není k dispozici |
| Události odeslané serverem | ROZHRANÍ .NET 4 + | 5+ |
| Dlouhým dotazováním | ROZHRANÍ .NET 4 + | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows Store a Windows Phone aplikace

Aplikace pro Windows Store a v aplikacích Windows Phone 8 lze SignalR. Podporovány jsou následující přenosy pod následující konfigurace:

**Windows Store a Windows Phone přenosu požadavky**

| Přenos | Windows Store nebo rozhraní .NET | Windows Store / JavaScript | Windows Phone / IE | Windows Phone nebo rozhraní .NET |
| --- | --- | --- | --- | --- |
| Technologie WebSockets | Není k dispozici | Win8 + | 8+ | Není k dispozici |
| Navždy rámce | Není k dispozici | Win8 + | 7.5+ | Není k dispozici |
| Události odeslané serverem | Win8 + | Není k dispozici | Není k dispozici | 8+ |
| Dlouhým dotazováním | Win8 + | Win8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Doporučené aktualizace

SignalR serverů doporučujeme následující aktualizace:

- Aktualizace pro rozhraní .NET Framework 4.5 je k dispozici [zde](https://support.microsoft.com/kb/2750149).
- Společnost Microsoft vydá pravidelně opravy QFE pro technologii ASP.NET. Toto bude použito jako dostupné.
