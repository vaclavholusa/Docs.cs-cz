---
title: Modul ASP.NET Core
author: tdykstra
description: "Představuje ASP.NET Core modulu (ANCM), modul služby IIS, která umožní Kestrel webový server použít jako reverzní proxy server služby IIS nebo IIS Express."
keywords: "Základní modul ASP.NET Core, IIS, IIS Express,ASP.NET, UseIISIntegration"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: 4661af33-34c5-4d71-93a0-8c7632f43580
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5eef9405c0c3d219755d7cffa5d45c3df45ddb5c
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="introduction-to-aspnet-core-module"></a>Úvod do modulu ASP.NET Core

Podle [tní Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), a [Ross Jan](https://github.com/Tratcher) 

ASP.NET Core modulu (ANCM) umožňuje spouštění aplikací za služby IIS, ASP.NET Core pomocí služby IIS pro co je vhodný v (zabezpečení, správy a mnoha Další) a pomocí [Kestrel](kestrel.md) pro co je vhodný v (se skutečně rychlé) a získávání výhody z obě technologie současně. **ANCM pracuje pouze s Kestrel; není kompatibilní s WebListener (v ASP.NET Core 1.x) nebo ovladač HTTP.sys (v 2.x).** 

Podporované verze systému Windows:

* Windows 7 a Windows Server 2008 R2 a novější

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>Jaké jsou modulu jádro ASP.NET

ANCM je nativní modul služby IIS, který zachytí do kanálu služby IIS a přesměrování provozu do back-end aplikace ASP.NET Core. Většině ostatních modulů, jako je například ověřování systému windows, získat stále možné spustit. ANCM pouze přebírá řízení, pokud obslužná rutina je vybrán pro požadavek a mapování obslužné rutiny je definována v aplikaci *web.config* souboru.

Protože se spouští v procesu aplikace ASP.NET Core samostatné z pracovní proces služby IIS, ANCM také zpracovat správy. ANCM spustí proces pro aplikace ASP.NET Core, když první požadavek odeslán a ho restartuje, když ho dojde k chybě. Toto je v podstatě stejné chování jako classic aplikace ASP.NET, spusťte v procesu ve službě IIS a spravovaná službou WAS (Windows Activation Service).

Zde je diagram, který ukazuje vztah mezi aplikací služby IIS, ANCM a ASP.NET Core.

![Modul ASP.NET Core](aspnet-core-module/_static/ancm.png)

Požadavky přichází z webu a stiskněte tlačítko ovladač Http.Sys režimu jádra, který směruje je do služby IIS na primární port (80) nebo SSL port (443). ANCM předává požadavky na aplikace ASP.NET Core na port HTTP, který je nakonfigurovaný pro aplikaci, které není portu 80/443.

Kestrel naslouchá pro provoz přicházející ze ANCM.  ANCM Určuje port, přes proměnné prostředí při spuštění a [UseIISIntegration](#call-useiisintegration) metoda nakonfiguruje server tak, aby naslouchala na `http://localhost:{port}`. Existují další kontroly, aby zamítal požadavky, nikoli z ANCM. (ANCM nepodporuje předávání protokolu HTTPS, takže jsou předávány požadavky prostřednictvím protokolu HTTP i v případě, že přijatých službou IIS prostřednictvím protokolu HTTPS.)

Kestrel převezme požadavky z ANCM a nabízených oznámení je do kanálu ASP.NET Core middlewaru, který poté je zpracovává a předává je jako `HttpContext` instance, které chcete aplikaci logiky. Odpovědi aplikací jsou pak předán zpět do služby IIS, které nabízených oznámení je zpět do klienta protokolu HTTP, který spustil žádosti.

ANCM má několik dalších funkcí také:

* Nastaví proměnné prostředí.
* Protokoly `stdout` výstup k úložišti souborů.
* Předává tokeny ověřování systému Windows.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Jak používat ANCM v aplikacích ASP.NET Core

Tato část obsahuje přehled procesu pro nastavení serveru služby IIS a ASP.NET Core aplikace. Podrobné pokyny najdete v tématu [hostitele v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index).

### <a name="install-ancm"></a>Nainstalujte ANCM


Základní modul ASP.NET musí být nainstalovaná ve službě IIS na serverech a ve službě IIS Express na vašich počítačích vývoj. Pro servery, je součástí ANCM [.NET jádra Windows serveru, který hostuje sady](https://aka.ms/dotnetcore-2-windowshosting). Pro počítače, vývoj Visual Studio automaticky nainstaluje ANCM ve službě IIS Express a ve službě IIS Pokud je již nainstalován na počítači.

### <a name="install-the-iisintegration-nuget-package"></a>Nainstalujte balíček IISIntegration NuGet

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) balíček je součástí metapackages ASP.NET Core ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) a [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ). Pokud nepoužíváte jeden z metapackages, nainstalujte `Microsoft.AspNetCore.Server.IISIntegration` samostatně. `IISIntegration` Balíčku je balík vzájemná funkční spolupráce, který čte proměnné prostředí všesměrového vysílání pomocí ANCM k nastavení aplikace. Proměnné prostředí poskytují informace o konfiguraci, jako je port pro naslouchání. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

V aplikaci nainstalovat [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/). `IISIntegration` Balíčku je balík vzájemná funkční spolupráce, který čte proměnné prostředí všesměrového vysílání pomocí ANCM k nastavení aplikace. Proměnné prostředí poskytují informace o konfiguraci, jako je port pro naslouchání. 

---

### <a name="call-useiisintegration"></a>Volání UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

`UseIISIntegration` Rozšiřující metody na [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) je zavolána automaticky při spuštění pomocí služby IIS.

Pokud nepoužíváte jeden metapackages ASP.NET Core a nenainstalovali `Microsoft.AspNetCore.Server.IISIntegration` balíčku, dojde k chybě běhového prostředí. Když zavoláte `UseIISIntegration` explicitně, zobrazí chybu v době kompilace Pokud balíček není nainstalovaný.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Ve vaší aplikaci `Main` metoda, volání `UseIISIntegration` rozšiřující metody na [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

`UseIISIntegration` Metoda vyhledá proměnné prostředí, které nastaví ANCM ale ne ops, pokud nejsou nalezeny. Toto chování usnadňuje scénáře jako vývoj a testování v systému macOS nebo Linux a nasazení na server, který spouští IIS. Při spouštění v systému macOS nebo Linux, Kestrel funguje jako webový server; ale když se aplikace nasadí prostředí služby IIS, automaticky používá ANCM a služby IIS.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>Vazbou portu ANCM přepíše ostatní port vazby

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

ANCM generuje dynamický port přiřadit pro proces back-end. `UseIISIntegration` Metoda převezme tento dynamický port a nakonfiguruje Kestrel tak, aby naslouchala na `http://locahost:{dynamicPort}/`. Přepíše ostatní konfigurace adresy URL, například volání `UseUrls` nebo [API naslouchat na Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Proto nemusíte volání `UseUrls` nebo na Kestrel `Listen` rozhraní API při použití ANCM. Když zavoláte `UseUrls` nebo `Listen`, Kestrel naslouchá na portu zadejte při spuštění aplikace bez služby IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

ANCM generuje dynamický port přiřadit pro proces back-end. `UseIISIntegration` Metoda převezme tento dynamický port a nakonfiguruje Kestrel tak, aby naslouchala na `http://locahost:{dynamicPort}/`. Přepíše ostatní konfigurace adresy URL, například volání `UseUrls`. Proto nemusíte volání `UseUrls` při použití ANCM. Když zavoláte `UseUrls`, Kestrel naslouchá na portu zadejte při spuštění aplikace bez služby IIS.

V technologii ASP.NET Core 1.0, když zavoláte `UseUrls`, volání **před** zavoláte `UseIISIntegration` tak, aby port nakonfigurovaný ANCM není přepsán. Toto pořadí volání nevyžaduje v technologii ASP.NET Core 1.1, protože přepsání nastavení ANCM `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>Konfigurace možností ANCM v souboru Web.config.

Konfigurace modulu jádra ASP.NET je uložená v *web.config* soubor, který se nachází v kořenové složce aplikace. Nastavení v tomto souboru, přejděte na spuštění příkaz i jeho argumenty, které spustí vaše aplikace ASP.NET Core. Pro ukázku *web.config* kód a pokyny k možnosti konfigurace, najdete v části [odkazu na modul Konfigurace ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

### <a name="run-with-iis-express-in-development"></a>Spustit v vývoj službou IIS Express

Služba IIS Express, může být spuštěn Visual Studio pomocí výchozí profil definované šablony ASP.NET Core.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfigurace proxy serveru používá protokol HTTP a párovací token

Proxy server mezi ANCM a Kestrel vytvořen používá protokol HTTP. Pomocí protokolu HTTP je optimalizace výkonu, kde provoz mezi ANCM a Kestrel probíhá na adresu zpětné smyčky z síťové rozhraní. Neexistuje žádné riziko odposlouchávání provoz mezi ANCM a Kestrel z umístění od serveru.

Token párovací se používá k zaručit, že požadavků přijatých službou Kestrel byly směrovány přes proxy server službou IIS a nebyla pochází z jiného zdroje. Párovací token se vytvoří a nastavení do proměnné prostředí (`ASPNETCORE_TOKEN`) pomocí ANCM. Také nastavení párovací tokenu do záhlaví (`MSAspNetCoreToken`) u každého požadavku směrovány přes proxy server. Služba IIS Middleware kontroly požadavku že obdrží potvrďte, že párovací hodnota tokenu hlavičky odpovídá hodnotu proměnné prostředí. Pokud hodnoty tokenu se neshodují, žádost je zaznamenána a odmítnut. Proměnnou párovací tokenu prostředí a přenosy dat mezi ANCM a Kestrel nejsou dostupné z umístění od serveru. Bez znalosti párovací hodnota tokenu, útočník nemohou odesílat požadavky, které obejít kontrolu v IIS Middleware.

## <a name="next-steps"></a>Další kroky

Další informace naleznete v následujících materiálech:

* [Ukázková aplikace pro tohoto článku](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [ASP.NET Core modulu zdrojového kódu](https://github.com/aspnet/AspNetCoreModule)
* [Referenční dokumentace k modulu ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Hostování ve Windows se službou IIS](xref:host-and-deploy/iis/index)
