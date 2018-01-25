---
title: "Žádost o funkce ASP.NET Core"
author: ardalis
description: "Další informace o webového serveru implementace podrobnosti týkající se požadavků HTTP a odpovědí, které jsou definovány v rozhraní pro ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: f0e371f5ea6c6688ef32adcacf667a412e4625e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="request-features-in-aspnet-core"></a>Žádost o funkce ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

Související podrobnosti implementace webového serveru na požadavky HTTP a odpovědí jsou definovány v rozhraní. Tato rozhraní jsou používány implementací serveru a middleware, vytvářet a upravovat hostování kanálu aplikace.

## <a name="feature-interfaces"></a>Funkce rozhraní

Definuje počet HTTP funkce rozhraní ASP.NET Core `Microsoft.AspNetCore.Http.Features` servery které se používají k identifikaci funkce podporují. Následující funkce rozhraní zpracování požadavků a odpovědí vrátit:

`IHttpRequestFeature`Definuje strukturu požadavku HTTP, včetně protokolu, cesta, řetězec dotazu, hlavičky a text.

`IHttpResponseFeature`Definuje strukturu odpovědi HTTP, včetně stavový kód, hlavičky a text odpovědi.

`IHttpAuthenticationFeature`Definuje podpory identifikace uživatele na základě `ClaimsPrincipal` a zadání obslužnou rutinu ověřování.

`IHttpUpgradeFeature`Definuje podporu pro [HTTP upgrady](https://tools.ietf.org/html/rfc2616.html#section-14.42), které umožňují klienta k určení, které další protokoly, ho chcete použít, pokud server, které chcete přepnout protokoly.

`IHttpBufferingFeature`Definuje metody pro zakázání ukládání do vyrovnávací paměti požadavků a odpovědí.

`IHttpConnectionFeature`Definuje vlastnosti pro místní a vzdálené adresy a porty.

`IHttpRequestLifetimeFeature`Definuje podporu pro přerušení připojení nebo zjišťování, pokud požadavek byl ukončen předčasně, například jako službou na odpojení klienta.

`IHttpSendFileFeature`Definuje metody pro asynchronní odesílání souborů.

`IHttpWebSocketFeature`Definuje rozhraní API pro podporu websocket.

`IHttpRequestIdentifierFeature`Přidá vlastnost, která může být implementováno k jednoznačné identifikaci požadavků.

`ISessionFeature`Definuje `ISessionFactory` a `ISession` abstrakce pro podporu uživatelských relací.

`ITlsConnectionFeature`Definuje rozhraní API pro načítání klientské certifikáty.

`ITlsTokenBindingFeature`Definuje metody pro práci s parametry token vazbu protokolu TLS.

> [!NOTE]
> `ISessionFeature`není funkce serveru, ale je implementováno modulem `SessionMiddleware` (viz [stav aplikace pro správu](app-state.md)).

## <a name="feature-collections"></a>Funkce kolekce

`Features` Vlastnost `HttpContext` poskytuje rozhraní pro získání a nastavení k dispozici funkce protokolu HTTP pro aktuální požadavek. Vzhledem k tomu, že kolekce funkce je měnitelný i v kontextu požadavku, middleware slouží k úpravě kolekce a přidání podpory pro další funkce.

## <a name="middleware-and-request-features"></a>Funkce middlewaru a požadavku

Servery jsou zodpovědný za vytváření kolekce funkce, middleware můžete přidat do této kolekce i využívat funkce z kolekce. Například `StaticFileMiddleware` přistupuje `IHttpSendFileFeature` funkce. Pokud funkci existuje, se používá k odesílání požadovaný statických souborů z fyzické cesty. Pomalejší alternativní metodu, jinak se používá k odesílání souboru. Pokud je k dispozici, `IHttpSendFileFeature` umožňuje operačního systému, otevřete soubor a provádět kopie režimu jádra přímé síťové karty.

Kromě toho middleware můžete přidat do kolekce funkce navázat serverem. Stávajících funkcí lze nahradit i middleware, což middleware k posílení funkce serveru. Funkce přidané do kolekce jsou k dispozici okamžitě další middleware nebo základní vlastní aplikace později v kanálu požadavku.

Kombinací implementace vlastního serveru a vylepšení určitému middlewaru jde konstruovat přesné sadu funkcí, které aplikace vyžaduje. To umožňuje chybí funkce, který se má přidat bez nutnosti změny na serveru a zajišťuje jsou viditelné pouze minimální množství funkcí, proto omezení útoku surface area a vylepšuje výkon.

## <a name="summary"></a>Souhrn

Funkce rozhraní definovat konkrétní funkce protokolu HTTP, které můžou podporovat daného požadavku. Servery definovat kolekce funkcí a počáteční sadu funkcí, které tento server podporuje, ale ke zvýšení tyto funkce můžete použít middleware.

## <a name="additional-resources"></a>Další prostředky

* [Servery](servers/index.md)

* [Middleware](middleware.md)

* [Otevřené webové rozhraní pro .NET (OWIN)](owin.md)
