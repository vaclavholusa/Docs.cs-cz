---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Řešení potíží s HTTP 405 chyby po publikování webové rozhraní API 2 aplikace | Microsoft Docs
author: rmcmurray
description: Tento kurz popisuje, jak k řešení chyb HTTP 405 po publikování aplikace webového rozhraní API na produkční webový server.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2014
ms.topic: article
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: b87ae7420e1295030e90c30e97b1e331413ce263
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
ms.locfileid: "26743270"
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>Řešení potíží s HTTP 405 chyby po publikování webové rozhraní API 2 aplikace
====================
podle [Roberta Mcmurrayho](https://github.com/rmcmurray)

> Tento kurz popisuje, jak k řešení chyb HTTP 405 po publikování aplikace webového rozhraní API na produkční webový server.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Internetové informační služby (IIS)](https://www.iis.net/) (verze 7 nebo novější)
> - [Webové rozhraní API](../../index.md) (verze 1 nebo 2)


Webové rozhraní API aplikace obvykle používat několik běžných operací protokolu HTTP: GET, POST, PUT, DELETE a někdy opravy. Který výše uvedeného, vývojáři mohou spustit v situacích, kde jsou tyto operace implementované jiný modul služby IIS na provozním serveru, což vede k situaci, kdy vrátí kontroleru webového rozhraní API, která funguje správně v sadě Visual Studio nebo v rámci vývojového serveru HTTP 405 při nasazení na provozním serveru došlo k chybě. Naštěstí je snadno vyřešit tento problém, ale řešení zaručuje vysvětlením, proč k problému dochází.

## <a name="what-causes-http-405-errors"></a>Jaké příčiny HTTP 405 chyby

První krok naučit se chyby protokolu HTTP 405 pro řešení problémů je pochopit, co se chybu HTTP 405 ve skutečnosti rozumí. Primární řídících dokumentů pro HTTP je [dokumentu RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), která definuje stavový kód protokolu HTTP 405 jako ***metoda není povoleno***a dále popisuje tento kód stavu jako stav kde &quot;metodu zadaný v řádek požadavku není povolená pro v prostředku označeném identifikátorem URI požadavku.&quot; Jinými slovy příkaz HTTP není povolena pro konkrétní adresy URL, která vyžaduje klientem HTTP.

Jako stručný přehled Zde jsou některé z nejčastěji používané metody HTTP jak jsou definovány v dokumentu RFC 2616 RFC 4918 a RFC 5789:

| Metoda HTTP | Popis |
| --- | --- |
| **GET** | Tato metoda se používá k načtení dat z identifikátoru URI ale pravděpodobně metodu nejpoužívanějším protokolu HTTP. |
| **HEAD** | Tato metoda je podobné jako metodu GET s tím rozdílem, že ve skutečnosti není načíst z identifikátoru URI požadavku – jednoduše načte stav protokolu HTTP. |
| **POST** | Tato metoda se obvykle používá k odesílání nová data k identifikátoru URI; POST se často používá k odesílání dat formuláře. |
| **PUT** | Tato metoda se obvykle používá k odesílání dat ve formátu raw k identifikátoru URI; PUT se často používá k odesílání dat XML nebo JSON do aplikace webového rozhraní API. |
| **ODSTRANIT** | Tato metoda se používá k odebrání dat z identifikátoru URI. |
| **MOŽNOSTI** | Tato metoda se obvykle používá k načtení seznamu metod HTTP, které jsou podporovány pro identifikátor URI. |
| **KOPÍROVÁNÍ PŘESUNUTÍ** | Tyto dvě metody se používají s WebDAV a jejich účelem je není potřeba vysvětlovat. |
| **MKCOL** | Tato metoda se používá s WebDAV a slouží k vytvoření kolekce (např. adresář) na zadaný identifikátor URI. |
| **PROPFIND, PROPPATCH** | Tyto dvě metody se používají s WebDAV a používají se k dotazování nebo nastavení vlastností pro identifikátor URI. |
| **ODEMKNOUT UZAMČENÍ** | Tyto dvě metody se používají s WebDAV a slouží k uzamčení v prostředku označeném identifikátorem URI požadavku při vytváření. |
| **OPRAVA** | Tato metoda se používá k úpravě existující prostředek HTTP. |

Když jednu z těchto metod HTTP je nakonfigurován pro použití na serveru, odpoví server stav protokolu HTTP a další data, která je vhodná pro požadavek. (Například metoda GET obdržet HTTP 200 ***OK*** odpověď a metodu PUT HTTP 201 obdržet ***vytvořená*** odpověď.)

Pokud metoda HTTP není nakonfigurovaná pro použití na serveru, server odpoví na HTTP 501 ***není implementováno*** chyby.

Ale pokud metoda HTTP je nakonfigurován pro použití na serveru, ale byla zakázána pro daný identifikátor URI, server odpoví pomocí protokolu HTTP 405 ***metoda není povoleno*** chyby.

## <a name="example-http-405-error"></a>Příklad protokolu HTTP 405 chyby

Následující příklad HTTP požadavku a odpovědi ilustruje situaci, kdy při pokusu o VLOŽTE hodnotu do webového rozhraní API aplikace na webovém serveru je v klientovi HTTP, a server vrátí chybu HTTP, které stavy, které metodu PUT není povolená:


Požadavek HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


Odpověď HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


V tomto příkladu klienta HTTP poslali žádost platná JSON na adresu URL webového rozhraní API aplikace na webovém serveru, ale server vrátil HTTP 405 chybovou zprávu, která označuje, že metodu PUT nebyla povolena pro adresu URL. Pokud se identifikátoru URI požadavku neodpovídá trasu pro aplikace webového rozhraní API, naproti tomu by server vrátit HTTP 404 ***nebyl nalezen*** chyby.

## <a name="resolving-http-405-errors"></a>Řešení HTTP 405 chyby

Tady je několik důvodů, proč nemusí být konkrétní akce HTTP povolena, ale je jeden primární scénář, který je přední příčinu této chyby ve službě IIS: více obslužných rutin, které jsou definovány pro stejnou operaci nebo metodu a jeden z obslužných rutin blokuje očekávané obslužná rutina z zpracování žádosti. Prostřednictvím vysvětlení služba IIS zpracovává obslužné rutiny z první, poslední na základě pořadí obslužná rutina položek v souboru applicationHost.config a web.config soubory, kde první odpovídající kombinace cestu, operace, prostředků, atd., se použije pro zpracování požadavku.

Následující příklad je výňatek ze souboru applicationHost.config pro server služby IIS, který byl vrátila chybu HTTP 405 při použití metody PUT odesílání dat do aplikace webového rozhraní API. V této výňatek ze jsou definovány několik obslužné rutiny HTTP a každou obslužnou rutinu je jiná sada metod HTTP, pro které je nakonfigurován – poslední položky v seznamu je statický obsah obslužná rutina, což je výchozí obslužná rutina, která se používá po ostatních obslužných rutin má chanc e zkontrolujte žádost:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

V předchozím příkladu jsou obslužná rutina protokolu WebDAV a obslužná rutina adresy URL bez přípony pro technologii ASP.NET (které se používají pro webového rozhraní API) jasně definované pro samostatné seznamy metod HTTP. Všimněte si, že obslužnou rutinu ISAPI DLL je nakonfigurován pro všechny metody HTTP, i když tato konfigurace nebude nutně dojít k chybě. Nastavení konfigurace, ale jako tento musí zohlednit při řešení potíží s chybami HTTP 405.

V předchozím příkladu obslužnou rutinu ISAPI DLL nebyl problém; ve skutečnosti problém nebyl definován v souboru applicationHost.config pro server služby IIS – tento problém byl způsobený tím položku, která byla vytvořená v souboru web.config, když aplikace webového rozhraní API byla vytvořena v sadě Visual Studio. Následující výňatek ze souboru web.config aplikace zobrazuje umístění problému:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

V této výňatek ze je obslužná rutina adresy URL bez přípony pro technologii ASP.NET předefinovat zahrnout další metody HTTP, které budou použity s aplikace webového rozhraní API. Ale vzhledem k tomu, že podobnou sadu metod HTTP je definována pro obslužná rutina protokolu WebDAV, dojde ke konfliktu. V tomto případě konkrétní obslužná rutina protokolu WebDAV je definovaný a načíst službou IIS, i když protokol WebDAV je zakázán pro web, který obsahuje aplikace webového rozhraní API. Během zpracování požadavku HTTP PUT, služba IIS vyvolá modul WebDAV vzhledem k tomu, že je definována pro operaci PUT. Při volání modulu protokolu WebDAV, zkontroluje konfiguraci a uvidí, že je zakázané, tak vrátí protokolu HTTP 405 ***metoda není povoleno*** chyb pro jakékoli požadavky, které se podobá žádost protokolu WebDAV. Chcete-li vyřešit tento problém, byste měli odebrat ze seznamu modulů HTTP pro web, kde je definován aplikace webového rozhraní API protokolu WebDAV. Následující příklad ukazuje, jaké, může vypadá jako:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Tento scénář je často došlo po publikování aplikace z vývojového prostředí do provozního prostředí a k tomu dochází, protože seznam obslužné rutiny nebo moduly se liší mezi vaše vývojová a produkční prostředí. Například pokud používáte Visual Studio 2012 nebo 2013 pro vývoj aplikací webového rozhraní API, IIS Express 8 je výchozí webový server pro testování. Tento webový server vývoj je škálovat nižší verzi úplné funkce služby IIS, který se dodává v serveru produktu a tohoto webového serveru vývoj obsahuje několik změn, které byly přidány pro vývojové scénáře. Například se modulu protokolu WebDAV často nainstaluje na produkční webový server, který běží plnou verzi služby IIS, i když nemusí být při skutečném použití. Vývoj verze služby IIS, (služba IIS Express), nainstaluje modul protokolu WebDAV, ale položky pro modul protokolu WebDAV jsou záměrně komentované, takže modul WebDAV je nikdy načtena ve službě IIS Express, pokud výslovně upravíte konfiguraci služby IIS Express nastavení můžete doplnit funkci protokolu WebDAV do instalace služby IIS Express. V důsledku toho může webová aplikace ve svém vývojovém počítači fungovat správně, ale mohou nastat chyby protokolu HTTP 405 při publikování aplikace webového rozhraní API na produkční webový server.

## <a name="summary"></a>Souhrn

HTTP 405 způsobuje chyby při metoda HTTP není povolena webového serveru pro požadovanou adresu URL. Tato podmínka je často zobrazí, pokud byla definována konkrétní obslužná rutina pro konkrétní operaci a přepisují této obslužné rutiny obslužná rutina, která byste měli zpracovat žádost.

Pokud nastat situace, kdy se zobrazí chybová zpráva HTTP 501, což znamená, že určité funkce nebyla implementována na serveru, často znamená to, že není žádná obslužná rutina definované v nastaveních služby IIS, která odpovídá požadavku HTTP, který pravděpodobně označuje, že něco nebyl správně nainstalován v počítači, nebo něco změnil nastavení služby IIS tak, aby nejsou že definované žádné obslužné rutiny dané metody podporu konkrétní HTTP. Chcete-li tento problém vyřešit, musíte znovu nainstalovat všechny aplikace, který se pokouší použít metodu HTTP, pro kterou má žádné odpovídající modul nebo obslužná rutina definice.
