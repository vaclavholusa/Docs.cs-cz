---
uid: web-api/overview/security/forms-authentication
title: Ověřování pomocí formulářů v rozhraní ASP.NET Web API | Dokumentace Microsoftu
author: MikeWasson
description: Popisuje použití ověřování pomocí formulářů v rozhraní ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7642a30816a04a88a25ef8bf4f01e766fda362ce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365068"
---
<a name="forms-authentication-in-aspnet-web-api"></a>Ověřování pomocí formulářů v rozhraní ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Ověřování pomocí formulářů formuláře HTML k odeslání přihlašovacích údajů uživatele k serveru. Není internetovým standardem. Ověřování pomocí formulářů je vhodné pro webové rozhraní API, která se volají z webové aplikace pouze tak, aby uživatelé můžou komunikovat s formuláře HTML.

| Android – systém cesta vrácená procedurou  je přijatelné umístění pro uložení souboru databáze. | Universal Windows Platform – používá  rozhraní API. |
| --- | --- |
| – Snadno se implementuje: integrovaný do technologie ASP.NET. -Používá poskytovatele členství prostředí ASP.NET, které umožňuje snadno spravovat uživatelské účty. | -Není standardní HTTP ověřování mechanismus; používá soubory cookie protokolu HTTP místo standardní hlavička autorizace. -Vyžaduje klientský prohlížeč. -Přihlašovací údaje jsou odeslány jako prostý text. -Citlivé na padělání žádosti mezi weby (CSRF); vyžaduje Antimalware CSRF opatření. -Obtížně používala, od nonbrowser klientů. Přihlášení se vyžaduje prohlížeč. -Uživatelských přihlašovacích údajů se odesílají v požadavku. – Někteří si uživatel cookies vypne. |

Stručně řečeno ověřování pomocí formulářů v ASP.NET funguje takto:

1. Klient požádá o prostředek, který vyžaduje ověření.
2. Pokud uživatel není ověřen, server vrátí HTTP 302 (Found) a přesměruje na přihlašovací stránku.
3. Uživatel zadá přihlašovací údaje a formulář odešle.
4. Server vrátí jiný HTTP 302, který přesměruje zpátky na původní identifikátor URI. Tato odpověď obsahuje soubor cookie ověřování.
5. Klient požádá o prostředek znovu. Požadavek obsahuje ověřovacího souboru cookie, takže server udělí žádost.

![](forms-authentication/_static/image1.png)

Další informace najdete v tématu [Přehled ověřování založené na formulářích.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Ověřování pomocí formulářů pomocí webového rozhraní API

Chcete-li vytvořit aplikaci, která používá ověřování pomocí formulářů, vyberte šablonu "Internet aplikace" v Průvodci vytvořením projektu MVC 4. Tato šablona vytvoří MVC řadiče pro správu účtu. Můžete také použít šablony "Jednostránkové aplikace" k dispozici v ASP.NET Fall 2012 Update.

Ve vašich kontrolerech webového rozhraní API můžete omezit přístup s použitím `[Authorize]` atributu, jak je popsáno v [pomocí atributu [Authorize]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Ověřování založené na formulářích použije k ověření požadavků souboru cookie relace. Prohlížeče automaticky odesílat všechny relevantní soubory cookie k cílovému webu. Tato funkce usnadňuje ověřování pomocí formulářů do webů viz útoků proti padělání (CSRF) žádost o potenciálně ohrožená [brání webů požadavek ÚTOKŮ Csrf útoky](preventing-cross-site-request-forgery-csrf-attacks.md).

Ověřování pomocí formulářů, nešifruje přihlašovací údaje uživatele. Ověřování pomocí formulářů proto není zabezpečený, není-li použít s protokolem SSL. Zobrazit [práce s protokolem SSL ve webovém rozhraní API](working-with-ssl-in-web-api.md).
