---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: "Hustotu připojení SignalR testování pomocí Crank | Microsoft Docs"
author: tfitzmac
description: "Hustotu připojení SignalR testování pomocí Crank"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/22/2015
ms.topic: article
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: a3e8fdb47bd80d819358f9c48cd82fd51d6c0400
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/21/2017
---
<a name="signalr-connection-density-testing-with-crank"></a>Hustotu připojení SignalR testování pomocí Crank
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek popisuje, jak používat nástroj Crank k testování aplikace s více simulované klientů.


Jakmile vaše aplikace běží v jeho hostitelské prostředí (buď Azure webové role, služby IIS, nebo automatických hostované pomocí Owin), můžete otestovat aplikace odpověď na vysokou hustotou připojení pomocí nástroje Crank. Hostitelské prostředí může být serveru Internetové informační služby (IIS), hostitele Owin nebo webové role Azure. (Poznámka: čítače výkonu nejsou k dispozici na Azure App Service Web Apps, takže nebude moct získat údaje o výkonu z testovací hustotu připojení.)

Připojení hustotu odkazuje na počet souběžných připojení TCP, které by bylo možné navázat na serveru. Každé připojení TCP vlastní náročnější a otevírání velký počet nečinné připojení nakonec vytvoří kritický bod paměti.

[Funkce SignalR codebase](https://github.com/signalr/signalr) obsahuje nástroj zátěžové testování nazvaný **klikové**. Nejnovější verzi Crank naleznete v [větev Dev](https://github.com/SignalR/signalr/tree/dev) na Githubu. Můžete si stáhnout Zip codebase archivu větve Dev funkce signalr [zde](https://github.com/SignalR/SignalR/archive/dev.zip).

Crank může sloužit k plně saturate paměti serveru pro výpočet celkový počet možných na serverový hardware nečinné připojení. Alternativně můžete také používat Crank pro zátěžový test serveru v části určité množství přetížení paměti, podle ramping připojení, dokud nebude dosaženo konkrétní počet nebo konkrétní paměti prahovou hodnotu.

Při testování, je důležité použít mailového vzdáleného klienta předejdete žádné soutěž o zdroje (tj. připojení TCP a paměť). Monitorování mailového klienta zajistit, že jejich nejsou stiskne kritické body, které může server zabránit v dosažení jeho kapacita (paměti nebo procesoru). Potřebujete zvýšit počet klientů, aby bylo možné plně zatížení serveru.

### <a name="running-a-connection-density-test"></a>Spuštění testu hustotu připojení

Tato část popisuje kroky potřebné ke spuštění testu připojení hustotu v aplikaci SignalR.

1. Stáhněte si a sestavte [základu kódu Dev větve funkce signalr](https://github.com/SignalR/SignalR/archive/dev.zip). V příkazovém řádku přejděte do &lt;adresáři projektu&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Nasazení aplikace na jeho určený hostitelské prostředí. Poznamenejte si koncového bodu, který vaše aplikace používá; například v vytvořené v aplikaci [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md), je koncový bod `http://<yourhost>:8080/signalr`.
3. Nainstalujte [čítače výkonu SignalR](signalr-performance.md#perfcounters) na serveru. Pokud vaše aplikace běží v Azure, najdete v části [pomocí čítače výkonu SignalR webové role Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Jakmile jste stáhli a vytvořené základu kódu a v hostiteli nainstalován čítače výkonu, nástroje příkazového řádku Crank lze nalézt v `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` složky.

Mezi dostupné možnosti pro nástroj Crank patří:

- **/?** : Zobrazuje obrazovka nápovědy. Dostupné možnosti se zobrazí také v případě **Url** je parametr vynechán.
- **Nebo adresa Url**: adresa URL pro připojení SignalR. Tento parametr je povinný. SignalR aplikaci pomocí výchozí mapování, bude končit cestu "/ signalr".
- **/ Přenosu**: název přenosu použít. Výchozí hodnota je `auto`, který vybere nejlepší dostupný protokol. Mezi možnosti patří `WebSockets`, `ServerSentEvents`, a `LongPolling` (`ForeverFrame` není možnost pro Crank, od klientů .NET místo slouží v aplikaci Internet Explorer). Další informace o tom, jak SignalR vybere přenosy najdete v tématu [přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports).
- **/ BatchSize**: počet klientů přidaných v každé dávce. Výchozí hodnota je 50.
- **/ ConnectInterval**: interval v milisekundách mezi přidáním připojení. Výchozí hodnota je 500.
- **Nebo připojení**: počet připojení používá k testování zatížení aplikace. Výchozí hodnota je 100 000.
- **/ ConnectTimeout**: časový limit v sekundách, než přerušení test. Výchozí hodnota je 300.
- **MinServerMBytes**: V megabajtech minimální serveru připojit. Výchozí hodnota je 500.
- **SendBytes**: velikost datová část odeslaná na server v bajtech. Výchozí hodnota je 0.
- **SendInterval**: zpoždění v milisekundách mezi zprávy na server. Výchozí hodnota je 500.
- **SendTimeout**: vypršení časového limitu v milisekundách pro zprávy na server. Výchozí hodnota je 300.
- **ControllerUrl**: adresa Url, kde bude jeden klientský hostitelem řadiče rozbočovače. Výchozí hodnota je null (žádná centra řadič). Rozbočovač řadiče je spuštěna při spuštění relace Crank; žádné další mezi Rozbočovač řadiče a Crank styku.
- **NumClients**: počet simulované klientům připojit se k aplikaci. Výchozí hodnota je jedna.
- **Soubor protokolu**: název souboru pro soubor protokolu pro test spustit. Výchozí hodnota je `crank.csv`.
- **SampleInterval**: čas v milisekundách mezi vzorků. Výchozí hodnota je 1 000.
- **SignalRInstance**: název instance pro čítače výkonu na serveru. Výchozí hodnota je pro použití stavu připojení klienta.

### <a name="example"></a>Příklad

Následující příkaz testovat web s názvem `pfsignalr` v Azure, který je hostitelem aplikace na portu 8080 s rozbočovačem s názvem "ControllerHub" a pomocí připojení 100.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
