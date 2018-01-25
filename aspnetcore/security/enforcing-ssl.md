---
title: "Vynucování SSL v aplikaci ASP.NET Core"
author: rick-anderson
description: "Ukazuje, jak chcete vyžadovat protokol SSL v ASP.NET Core webové aplikace"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: f248e9c0463cf4a46a447a9c896b3276a50f5f08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Vynucování SSL v aplikaci ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Tento dokument ukazuje, jak:

- Vyžadovat protokol SSL pro všechny požadavky (pouze požadavky HTTPS).
- Přesměrujte všechny požadavky HTTP do HTTPS.

## <a name="require-ssl"></a>Požadovat protokol SSL

[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) slouží k vyžadování protokolu SSL. Můžete uspořádání řadiče nebo metody s tímto atributem nebo ho můžete použít globálně, jak je uvedeno níže:

Přidejte následující kód, který `ConfigureServices` v `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

Zvýrazněný výše vyžaduje všechny požadavky používat `HTTPS`, proto jsou ignorovány požadavky HTTP. Následující zvýrazněný kód přesměruje všech požadavků HTTP do HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

V tématu [URL přepisování Middleware](xref:fundamentals/url-rewriting) Další informace.

Globálně vyžadujících protokol HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je nejlepším postupem zabezpečení. Použití `[RequireHttps]` atribut, aby všechny řadiče se nepovažuje za bezpečnou jako jako globální vyžadujících protokol HTTPS. Nebudete moct zaručit, nových řadičů přidán do vaší aplikace bude nezapomeňte použít `[RequireHttps]` atribut.
