---
uid: web-api/overview/security/basic-authentication
title: "Základní ověřování v rozhraní ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: "Popisuje použití základního ověřování v rozhraní ASP.NET Web API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="basic-authentication-in-aspnet-web-api"></a>Základní ověřování v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Základní ověřování je definována v [RFC 2617, ověřování pomocí protokolu HTTP: Basic a ověřování algoritmem Digest přístup](http://www.ietf.org/rfc/rfc2617.txt).

Nevýhody

- Přihlašovací údaje uživatele jsou zasílány v požadavku.
- Přihlašovací údaje se odesílají jako prostý text.
- Přihlašovací údaje se odesílají s každou žádostí.
- Žádný způsob, jak odhlášení, s výjimkou ukončení relace prohlížeče.
- Bude zranitelný vůči webů padělání žádosti (proti útokům CSRF); vyžaduje opatření proti proti útokům CSRF.

Výhody

- Internetovým standardem.
- Podporuje všechny hlavní prohlížeče.
- Relativně jednoduché protokol.

Základní ověřování pracuje takto:

1. Pokud požadavek vyžaduje ověření, server vrátí 401 (Neautorizováno). Odpověď obsahuje hlavičku WWW-Authenticate, což značí, že server podporuje základní ověřování.
2. Klient odešle jinou žádost, pomocí přihlašovacích údajů klienta v hlavičce autorizace. Přihlašovací údaje jsou formátovány jako řetězec "název: heslo", kódování base64. Přihlašovací údaje nejsou šifrovány.

Základní ověřování se provádí v kontextu "sféru." Server obsahuje název sféry v záhlaví WWW-Authenticate. Přihlašovací údaje uživatele jsou platné v rámci této sféry. Server je definována přesný oboru sféru. Například můžete třeba definovat několik sfér, aby oddíl prostředky.

![](basic-authentication/_static/image1.png)

Protože přihlašovací údaje se odesílají nezašifrované, základní ověřování je pouze zabezpečené pomocí protokolu HTTPS. V tématu [práce s protokolem SSL v rozhraní Web API](working-with-ssl-in-web-api.md).

Základní ověřování je také bude zranitelný vůči útoku proti útokům CSRF. Poté, co uživatel zadá přihlašovací údaje, prohlížeč automaticky odesílá je na následné žádosti do stejné domény, po dobu trvání relace. To zahrnuje požadavky AJAX. V tématu [prevence útoků (proti útokům CSRF) padělání požadavku posílaného mezi weby](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Základní ověřování pomocí služby IIS

Služba IIS podporuje základní ověřování, ale je přímý přístup paměti: ověření uživatele před jejich přihlašovací údaje systému Windows. To znamená, že uživatel musí mít účet v doméně serveru. Pro veřejné webový server budete chtít obvykle ověřují se zprostředkovatelem členství prostředí ASP.NET.

Pokud chcete povolit základní ověřování pomocí služby IIS, nastavte režim ověřování "Systém Windows" v souboru Web.config vašeho projektu ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

V tomto režimu služba IIS používá přihlašovací údaje Windows k ověření. Kromě toho je třeba povolit základní ověřování ve službě IIS. Ve Správci služby IIS, přejděte do zobrazení funkce, vyberte možnost ověřování a povolit základní ověřování.

![](basic-authentication/_static/image2.png)

V projektu webového rozhraní API, přidejte `[Authorize]` atribut pro všechny akce kontroleru, které je třeba ověřování.

Klient se ověří nastavením autorizační hlavičky v požadavku. Tento krok proveďte klienty prohlížeče automaticky. Klienti nonbrowser muset nastavit hlavičku.

## <a name="basic-authentication-with-custom-membership"></a>Základní ověřování s vlastní členství

Jak je uvedeno, základní ověřování, které jsou integrovány do služby IIS používá přihlašovací údaje systému Windows. To znamená, že je potřeba vytvořit uživatelské účty na hostitelském serveru. Ale pro internetovou aplikaci, uživatelské účty jsou obvykle uložena v externí databáze.

Následující kód, jak modul HTTP, která provádí základní ověřování. Můžete snadno zařadit zprostředkovatelem členství prostředí ASP.NET tak, že nahradíte `CheckPassword` metodu, která je fiktivní metoda v tomto příkladu.

Ve webovém rozhraní API 2, byste měli zvážit zápis [ověřování filtru](authentication-filters.md) nebo [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), místo modulu HTTP.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Chcete-li povolit modul HTTP, přidejte následující v souboru web.config **system.webServer** části:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

"YourAssemblyName" nahraďte název sestavení (včetně není rozšíření "dll").

Měli byste zakázat další metody ověřování, jako jsou formuláře nebo systému Windows umožňuje
