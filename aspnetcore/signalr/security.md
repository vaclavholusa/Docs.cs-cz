---
title: Informace o zabezpečení ve funkci SignalR technologie ASP.NET Core
author: rachelappel
description: Další informace o použití ověřování a autorizace v knihovně SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: eff4542b88f24dd6c1c0675f56874e368d441fdd
ms.sourcegitcommit: 32626efaa7316c9b283c96be6516e637d548c5e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/13/2018
ms.locfileid: "39028524"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Informace o zabezpečení ve funkci SignalR technologie ASP.NET Core

Podle [Andrew Stanton sestry](https://twitter.com/anurse)

## <a name="overview"></a>Přehled

Funkce SignalR poskytuje řadu ochranu zabezpečení ve výchozím nastavení. Je důležité pochopit, jak nakonfigurovat tyto ochrany.

### <a name="cross-origin-resource-sharing"></a>Sdílení prostředků různého původu

[Prostředků mezi zdroji (CORS) pro sdílení obsahu](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) slouží k povolení připojení SignalR nepůvodního zdroje v prohlížeči. Pokud váš kód jazyka JavaScript je hostovaný na jiný název domény z vaší aplikace SignalR, budete muset povolit [middlewarem ASP.NET Core CORS](xref:security/cors) umožní připojení. Obecně platí povolení žádostí nepůvodního pouze z domén, které spravujete. Například, pokud je hostitelem vašeho webu `http://www.example.com` a je hostitelem vaší aplikace SignalR `http://signalr.example.com`, byste měli nakonfigurovat CORS ve vaší aplikaci SignalR a Povolit jenom původ `www.example.com`.

Další informace o konfiguraci CORS, najdete v části [dokumentaci k ASP.NET Core CORS](xref:security/cors). Funkce SignalR správnou funkci vyžaduje následující zásady CORS:

* Zásada musí povolit konkrétní zdroje očekávat, nebo povolíte původu (nedoporučuje se).
* Metody HTTP `GET` a `POST` musí být povoleno.
* Přihlašovací údaje musí být povolena, i když nepoužíváte ověřování.

Například následující zásadu CORS umožňuje klientovi SignalR prohlížeče hostované na `http://example.com` pro přístup k vaší aplikace SignalR:

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> SignalR je nekompatibilní s integrovaná funkce CORS v Azure App Service.

### <a name="access-token-logging"></a>Protokolování token přístupu

Při použití Server-Sent události nebo protokoly Websocket, klientský prohlížeč odesílá přístupový token v řetězci dotazu. Toto je obecně stejně bezpečné jako použití standardní `Authorization` záhlaví, ale mnohé webové servery protokolu pro každý požadavek adresu URL, včetně řetězec dotazu. To znamená, že přístupový token může být součástí protokolů. Vezměte v úvahu zkontrolujete nastavení protokolování webového serveru, aby tyto informace protokolování.

### <a name="exceptions"></a>Výjimky

Zprávy o výjimkách jsou obvykle považovány za citlivá data, která by neměla být odhalena do klienta. Ve výchozím nastavení nebude funkce SignalR Odeslat podrobnosti výjimky vyvolané metodou rozbočovače klientovi. Místo toho klient obdrží obecná zpráva oznamující, že došlo k chybě. Toto chování můžete přepsat tak, že nastavíte [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) nastavení.

### <a name="buffer-management"></a>Správa vyrovnávací paměti

Funkce SignalR používá vyrovnávací paměti za připojení za účelem správy příchozí a odchozí zprávy. Ve výchozím nastavení omezuje SignalR vyrovnávací paměti na 32KB. To znamená, že největší možné zprávu, kterou můžete poslat klient nebo server je 32 KB. Zároveň to znamená, že maximální množství paměti používané připojení pro zprávy je 32 KB. Pokud víte, že vaše zprávy jsou vždy menší než toto omezení, můžete snížit velikost zabránit odeslání větší zprávy a vynutit serveru přidělení paměti přijmout klienta. Podobně pokud víte, že vaše zprávy jsou větší než toto omezení, můžete zvýšit ji. Nezapomínejte, že tento limit zvýšit znamená, že klient může způsobit, že server další paměť přidělit a může snížit počet souběžných připojení, které vaše aplikace dokáže zpracovat.

Existují zvláštní omezení pro příchozí a odchozí zprávy, jak lze nakonfigurovat podle [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) objekt gurovaný `MapHub`:

* `ApplicationMaxBufferSize` představuje maximální počet bajtů z klienta, který vyrovnávací paměti serveru. Pokud se klient pokusí odeslat zprávu větší než tento limit, může připojení ukončeno.
* `TransportMaxBufferSize` představuje maximální počet bajtů, které může server odeslat. Pokud se server pokusí odeslat zprávu (zahrnuje návratové hodnoty metod rozbočovače na) větší než tento limit, bude vyvolána výjimka.

Nastavení limitu `0` zcela zakazuje limit. Ale to by mělo být provedeno s nejvyšší opatrností. Odebrání limitu umožňuje klientovi umožní odeslat zprávu libovolné velikosti. To lze použít škodlivého klienta způsobit nadbytek paměti, kterou je potřeba přidělit, což může výrazně snížit počet souběžných připojení, které vaše aplikace může podporovat.
