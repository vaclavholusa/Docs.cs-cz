---
title: Modul ASP.NET Core
author: tdykstra
description: Zjistěte, jak modul základní technologie ASP.NET umožňuje Kestrel webového serveru použít jako reverzní proxy server služby IIS nebo IIS Express.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: d99a4b446b53c431b11bfe083b1bb6133f8e0078
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="aspnet-core-module"></a>Modul ASP.NET Core

Podle [tní Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), a [Ross Jan](https://github.com/Tratcher) 

Základní modul ASP.NET umožňuje ASP.NET Core aplikace pro spouštění za služby IIS v konfiguraci reverzní proxy server. Služba IIS poskytuje pokročilé webové funkce zabezpečení a možnosti správy aplikací.

Podporované verze systému Windows:

* Windows 7 nebo novější
* Windows Server 2008 R2 nebo novější&#8224;

&#8224;Koncepčně používání modulu jádra ASP.NET se službou IIS, které jsou popsané v tomto dokumentu platí také pro hostování aplikací ASP.NET Core na Nano Server IIS. Specifické pro Nano Server pokyny najdete v tématu [ASP.NET Core pomocí služby IIS na serveru Nano](xref:tutorials/nano-server) kurzu.

Základní modul ASP.NET pracuje pouze s Kestrel. Modul není kompatibilní s [HTTP.sys](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener)).

## <a name="aspnet-core-module-description"></a>Popis modulu jádro ASP.NET

Základní modul ASP.NET je nativní modul služby IIS, která po zapojení do kanálu služby IIS pro přesměrování požadavků na webu na back-end aplikace ASP.NET Core. Množství nativních modulů, jako je například ověřování systému Windows, zůstávají aktivní. Další informace o moduly služby IIS active s modulem najdete v tématu [moduly služby IIS](xref:host-and-deploy/iis/modules).

Protože samostatné aplikace ASP.NET Core spustit v procesu z pracovní proces služby IIS, modul také zpracovává proces správy. Modul zahájí proces pro aplikace ASP.NET Core při prvním požadavku dorazí a restartuje aplikace, pokud ji dojde k chybě. Toto je v podstatě stejné chování jako aplikace ASP.NET 4.x, které běží v procesu v IIS, které jsou spravovány [služby Aktivace procesů systému Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Následující diagram znázorňuje vztah mezi službou IIS, modul ASP.NET Core a ASP.NET Core aplikace:

![Modul ASP.NET Core](aspnet-core-module/_static/ancm.png)

Požadavky přicházejí z webu pro ovladač HTTP.sys režimu jádra. Ovladač směrovat požadavky na služby IIS na konfigurovaném portu webu, obvykle 80 (HTTP) nebo 443 (HTTPS). Modul předává požadavky Kestrel na náhodných portu pro aplikaci, která není portu 80/443.

Modul určuje port, přes proměnné prostředí při spuštění a Middleware integrační služby IIS nakonfiguruje server tak, aby naslouchala na `http://localhost:{port}`. Jsou-li provést další kontroly a odmítne požadavky, které nejsou pocházejí z modulu. Modul nepodporuje předávání protokolu HTTPS, takže jsou předávány požadavky prostřednictvím protokolu HTTP i v případě, že přijatých službou IIS prostřednictvím protokolu HTTPS.

Po Kestrel převezme žádost z modulu, požadavek se posune do ASP.NET Core middlewaru v řadě. Middlewaru v řadě zpracuje požadavek a předává je jako `HttpContext` instance aplikace logiky. Odpověď aplikace je předán zpět do služby IIS, které nabízených oznámení zpátky klienta HTTP, který žádost iniciovala.

Základní modul ASP.NET obsahuje několik dalších funkcí. Modul může:

* Nastavení proměnných prostředí pro pracovní proces.
* Protokol `stdout` výstup do souboru úložiště pro řešení potíží při spuštění.
* Předat dál tokeny ověřování systému Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Jak nainstalovat a použít modul jádro ASP.NET

Podrobné pokyny o tom, jak nainstalovat a používat modul jádro ASP.NET najdete v tématu [hostitele v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index). Informace o konfiguraci modulu najdete v tématu [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module).

## <a name="additional-resources"></a>Další zdroje

* [Hostování ve Windows se službou IIS](xref:host-and-deploy/iis/index)
* [Referenční dokumentace k modulu ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Úložiště GitHub modulu jádra ASP.NET (zdrojový kód)](https://github.com/aspnet/AspNetCoreModule)
