---
title: "Vynucování SSL v aplikaci ASP.NET Core"
author: rick-anderson
description: "Ukazuje, jak chcete vyžadovat protokol SSL v ASP.NET Core webové aplikace"
manager: wpickett
ms.author: riande
ms.date: 07/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 2e0a2f4732e574c80ceef8fd21a530a11aef254c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
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
