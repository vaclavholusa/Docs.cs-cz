---
uid: web-api/overview/security/basic-authentication
title: Základní ověřování v rozhraní ASP.NET Web API | Dokumentace Microsoftu
author: MikeWasson
description: Popisuje použití základního ověřování v rozhraní ASP.NET Web API.
ms.author: aspnetcontent
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 035baec7c56c0bf6eaacd26ea5192faf2ed6e932
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829584"
---
<a name="basic-authentication-in-aspnet-web-api"></a>Základní ověřování v rozhraní ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Základní ověřování je definována v [RFC 2617, ověřování pomocí protokolu HTTP: Basic a ověřování algoritmem Digest přístup](http://www.ietf.org/rfc/rfc2617.txt).

Universal Windows Platform – používá  rozhraní API.

- Přihlašovací údaje uživatele jsou odeslány v požadavku.
- Přihlašovací údaje jsou odeslány jako prostý text.
- Přihlašovací údaje jsou odeslány při každé žádosti.
- Žádný způsob, jak se odhlásit, s výjimkou ukončením relace prohlížeče.
- Snadno napadnutelný mezi weby (CSRF); proti padělání požadavků vyžaduje Antimalware CSRF opatření.

Android – systém cesta vrácená procedurou  je přijatelné umístění pro uložení souboru databáze.

- Internet standard.
- Podporuje všechny hlavní prohlížeče.
- Poměrně jednoduchá protokol.

Základní ověřování pracuje následujícím způsobem:

1. Pokud požadavek vyžaduje ověření, server vrátí 401 (Neautorizováno). Odpověď obsahuje hlavičku WWW-Authenticate, oznamující, že server podporuje základní ověřování.
2. Klient odešle požadavek na jiný, pomocí přihlašovacích údajů klienta v hlavičce autorizace. Přihlašovací údaje jsou formátovány jako řetězec "název: heslo", s kódováním base64. Přihlašovací údaje nejsou šifrovány.

Základní ověřování je prováděno v kontextu "sféru." Na serveru obsahuje název sféry hlavičky WWW-Authenticate. Přihlašovací údaje uživatele jsou platné v rámci této sféru. Přesné oboru sféru je definována na serveru. Například můžete třeba definovat několik sféry v pořadí oddílu prostředků.

![](basic-authentication/_static/image1.png)

Vzhledem k tomu, že přihlašovací údaje se odesílají v nešifrované, základní ověřování je pouze zabezpečené přes protokol HTTPS. Zobrazit [práce s protokolem SSL ve webovém rozhraní API](working-with-ssl-in-web-api.md).

Základní ověřování je také zranitelný vůči útokům CSRF. Poté, co uživatel zadá přihlašovací údaje, prohlížeč automaticky odesílá je na následné žádosti do stejné domény po dobu trvání relace. To zahrnuje odesílání požadavků AJAX. Zobrazit [prevence útoků proti padělání (CSRF) podvržení žádosti](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Základní ověřování se službou IIS

Služba IIS podporuje základní ověřování, ale je výstrahou: ověření uživatele vůči přihlašovacích údajů Windows. To znamená, že uživatel musí mít účet v doméně serveru. Pro webový server veřejnou obvykle chcete k ověřování na základě poskytovatele členství ASP.NET.

Pokud chcete povolit základní ověřování pomocí služby IIS, nastavte "Windows" v souboru Web.config vašeho projektu ASP.NET režim ověřování:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

V tomto režimu služba IIS používá přihlašovací údaje Windows k ověření. Kromě toho musíte povolit základní ověřování ve službě IIS. Ve Správci služby IIS, přejděte na zobrazení funkcí, vyberte metodu ověřování a povolit základní ověřování.

![](basic-authentication/_static/image2.png)

V projektu webového rozhraní API, přidejte `[Authorize]` atribut pro všechny akce kontroleru, které vyžadují ověřování.

Klient se ověří tak, že nastavíte hlavičku autorizace v požadavku. Klienty prohlížeče proveďte tento krok automaticky. Klienti nonbrowser muset nastavit hlavičku.

## <a name="basic-authentication-with-custom-membership"></a>Základní ověřování s vlastní členství

Jak už bylo zmíněno, základní ověření integrované do služby IIS používá přihlašovací údaje Windows. To znamená, že je potřeba vytvořit účty pro uživatele na hostitelském serveru. Ale pro internetovou aplikaci, uživatelské účty jsou obvykle uložena v externí databázi.

Následující kód způsob, jakým modul HTTP, který provádí základní ověřování. Můžete snadno připojit poskytovatele členství ASP.NET tak, že nahradíte `CheckPassword` metodu, která je fiktivní metoda v tomto příkladu.

Ve webovém rozhraní API 2, měli byste zvážit zápis [filtr ověřování](authentication-filters.md) nebo [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), místo modulu HTTP.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Pokud chcete povolit modul HTTP, přidejte následující v souboru web.config **system.webServer** části:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Nahraďte název sestavení (nezahrnuje příponu "dll") "YourAssemblyName".

Měli byste zakázat další metody ověřování, jako jsou tyto formuláře nebo Windows
