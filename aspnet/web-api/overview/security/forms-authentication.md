---
uid: web-api/overview/security/forms-authentication
title: "Ověřování pomocí formulářů v rozhraní ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: "Popisuje použití ověřování pomocí formulářů v rozhraní ASP.NET Web API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9027d76bcf8854fc85f11906d3651511f350cd32
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="forms-authentication-in-aspnet-web-api"></a>Ověřování pomocí formulářů v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Ověřování pomocí formulářů formuláře HTML na odeslání přihlašovacích údajů uživatele k serveru. Není internetovým standardem. Ověřování pomocí formulářů je vhodné pro webové rozhraní API, které se nazývají z webové aplikace pouze tak, aby uživatel mohl pracovat s formuláře HTML.

| Výhody | Nevýhody |
| --- | --- |
| -Snadno implementovat: integrovaný do technologie ASP.NET. -Používá poskytovatele členství prostředí ASP.NET, který umožňuje snadno spravovat uživatelské účty. | -Není standardní HTTP ověřování mechanismus; místo standardní hlavičku autorizace používá soubory cookie protokolu HTTP. -Vyžaduje klientský prohlížeč. -Přihlašovací údaje se odesílají jako prostý text. -Citlivé na padělání požadavku posílaného mezi weby (proti útokům CSRF); vyžaduje opatření proti proti útokům CSRF. -Obtížně použitelný od nonbrowser klientů. Přihlášení vyžaduje prohlížeč. -Uživatelské přihlašovací údaje se odesílají v požadavku. -Někteří uživatelé zakažte soubory cookie. |

Stručně řečeno ověřování pomocí formulářů technologie ASP.NET funguje takto:

1. Klient požádá o prostředek, který vyžaduje ověření.
2. Pokud není uživatel ověřen, server vrátí HTTP 302 (nalezeno) a přesměruje na přihlašovací stránku.
3. Uživatel zadá přihlašovací údaje a odeslání formuláře.
4. Server vrátí jiné HTTP 302, který přesměruje zpět na původní identifikátor URI. Tato odpověď obsahuje soubor cookie ověřování.
5. Klient požádá o prostředek znovu. Požadavek obsahuje ověřovacího souboru cookie, tak server udělí žádosti.

![](forms-authentication/_static/image1.png)

Další informace najdete v tématu [Přehled ověřování založené na formulářích.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Ověřování pomocí formulářů pomocí webového rozhraní API

Pokud chcete vytvořit aplikaci, která používá ověřování pomocí formulářů, vyberte šablonu "Internet aplikace" v Průvodci projekt MVC 4. Tato šablona vytvoří MVC řadiče pro správu účtu. Můžete taky šabloně "Jedné stránky aplikace", dostupné v aktualizaci 2012 patří technologie ASP.NET.

V řadičích webového rozhraní API můžete omezit přístup pomocí `[Authorize]` atributu, jak je popsáno v [pomocí atributu [Authorize]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Ověřování založené na formulářích používá k ověřování žádostí o souboru cookie relace. Všechny relevantní soubory cookie prohlížeče automaticky odesílat do cílového webu. Tato funkce je zranitelný ověřování pomocí formulářů potenciálně webů požadavek padělání (proti útokům CSRF) před útoky najdete [útoky brání webů požadavek padělání (proti útokům CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

Ověřování pomocí formulářů nešifruje přihlašovací údaje uživatele. Ověřování pomocí formulářů je proto není zabezpečená, není-li použít s protokolem SSL. V tématu [práce s protokolem SSL v rozhraní Web API](working-with-ssl-in-web-api.md).
