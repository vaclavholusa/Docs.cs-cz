---
title: "Vynucování HTTPS v aplikaci ASP.NET Core"
author: rick-anderson
description: "Ukazuje, jak chcete vyžadovat protokol HTTPS/TLS v ASP.NET Core webové aplikace."
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 636077ea21581716308384ebf8d47c1e417a256a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a>Vynucování HTTPS v aplikaci ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Tento dokument ukazuje, jak:

- Vyžadovat protokol HTTPS pro všechny požadavky.
- Přesměrujte všechny požadavky HTTP do HTTPS.

> [!WARNING]
> Proveďte **není** použít `RequireHttpsAttribute` na webovým rozhraním API, která zobrazit citlivé informace. `RequireHttpsAttribute`stavové kódy HTTP se používá k přesměrování prohlížeče z HTTP do HTTPS. Rozhraní API klienti nemusí pochopit a orientují přesměrování z HTTP do HTTPS. Tito klienti mohou odesílat informace přes protokol HTTP. Webová rozhraní API se musí buď:
>
>* Nelze naslouchat na protokolu HTTP.
>* Ukončení připojení se stavovým kódem 400 (Chybný požadavek) a není požadavek vyřídit.

## <a name="require-https"></a>Vyžadovat protokol HTTPS

[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) se používá k vyžadují protokol HTTPS. `[RequireHttpsAttribute]`můžete uspořádání řadiče nebo metod, nebo mohou být použity globálně. Použít atribut globálně, přidejte následující kód do `ConfigureServices` v `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Předchozí zvýrazněný vyžaduje všechny požadavky používat `HTTPS`; proto požadavky HTTP jsou ignorovány. Následující zvýrazněný kód přesměruje všech požadavků HTTP do HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Další informace najdete v tématu [URL přepisování Middleware](xref:fundamentals/url-rewriting).

Globálně vyžadujících protokol HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je nejlepším postupem zabezpečení. Použití `[RequireHttps]` atribut na všechny řadiče nebo Razor stránky se nepovažuje za bezpečnou jako jako globální vyžadujících protokol HTTPS. Nebudete moct zaručit `[RequireHttps]` atribut se používá, když se přidají nové řadiče a stránky Razor.