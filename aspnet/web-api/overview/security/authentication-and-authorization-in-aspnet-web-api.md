---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Ověřování a autorizace v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Poskytuje obecný přehled ověřování a autorizace v rozhraní ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d7cbb9505afb6461ba4c2087d57e9ea0da38ede
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
ms.locfileid: "29726755"
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Ověřování a autorizace v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Vytvoření webové rozhraní API, ale teď chcete řídit přístup k němu. Z této série články podíváme některé možnosti pro zabezpečení webového rozhraní API před neoprávněnými uživateli. Tato řada se bude zabývat ověřování a autorizace.

- *Ověřování* je znalost identitu uživatele. Například Alice přihlásí pomocí svého uživatelského jména a hesla a server používá heslo k ověření Alice.
- *Autorizace* je rozhodování, zda má uživatel k provedení akce. Například Alice má oprávnění k získání prostředek, ale není vytvoření prostředku.

První článek v řadě poskytuje obecný přehled ověřování a autorizace v rozhraní ASP.NET Web API. Další témata popisují běžné scénáře ověřování pro webové rozhraní API.

> [!NOTE]
> Díky osob, které tato řada zkontrolovány a poskytuje cenné zpětné vazby: Rick Anderson, Levi Broderick, Jiří Dorrans, tní Dykstra, Hongmei Ge, David Matson, ADAM Roth, Tim Teebken.


## <a name="authentication"></a>Ověřování

Webové rozhraní API předpokládá, že se stane, že ověřování na hostiteli. Pro hostování webů, je hostitele služby IIS, která používá vytváření modulů HTTP pro ověřování. Můžete nakonfigurovat projektu pro použití žádné z modulů ověřování, které jsou součástí služby IIS nebo v technologii ASP.NET, nebo napsat vlastní modul protokolu HTTP k provedení vlastní ověřování.

Když hostitel ověřuje uživatele, vytvoří *hlavní*, který je [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) objekt, který reprezentuje kontext zabezpečení, v němž je spuštěn kód. Hostitel připojí k objektu zabezpečení pro aktuální vlákno nastavením **Thread.CurrentPrincipal**. Objekt zabezpečení obsahuje přiřazený **Identity** objekt, který obsahuje informace o uživateli. Pokud je uživatel ověřený, **Identity.IsAuthenticated** vlastnost vrátí **true**. U anonymních požadavků **IsAuthenticated** vrátí **false**. Další informace o objekty zabezpečení najdete v tématu [na základě rolí zabezpečení](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Obslužné rutiny zpráv HTTP pro ověřování

Místo použití hostitele pro ověřování, které můžete vložit logika ověřování do [obslužné rutiny zpráv HTTP](../advanced/http-message-handlers.md). V takovém případě popisovač zpráv prozkoumá požadavek HTTP a nastaví objekt zabezpečení.

Kdy mám použít obslužné rutiny zpráv pro ověřování? Zde jsou některé kompromisy:

- Modul HTTP se zobrazí všechny požadavky, které procházejí kanálu ASP.NET. Obslužné rutiny zpráv se zobrazují pouze požadavků, které jsou směrovány do webového rozhraní API.
- Můžete nastavit obslužné rutiny zpráv za trasy, který vám umožňuje použít příslušné schéma ověřování pro konkrétní trasy.
- Vytváření modulů HTTP jsou specifické pro službu IIS. Obslužné rutiny zpráv jsou vázané na hostitele, takže je můžete použít pro hostování webů a samoobslužné hostování.
- Vytváření modulů HTTP v účasti v protokolování služby IIS, auditování a tak dále.
- Vytváření modulů HTTP v spustit dříve v kanálu. Pokud zpracováváte ověřování v obslužné rutiny zpráv, získat objekt není nastaven, dokud obslužná rutina se spustí. Kromě toho se objekt vrátí zpět na předchozí objekt zabezpečení při odesílání odpovědi popisovač zpráv.

Obecně platí Pokud nemusíte podporovat samoobslužné hostování, modul HTTP je lepší volbou. Pokud potřebujete podporovat samoobslužné hostování, zvažte obslužné rutiny zpráv.

### <a name="setting-the-principal"></a>Nastavení objektu zabezpečení

Pokud aplikace provede všechny vlastní ověřovací logiku, je nutné nastavit objekt zabezpečení na dvou místech:

- **Thread.CurrentPrincipal**. Tato vlastnost je standardní způsob, jak nastavit objekt zabezpečení vlákna v rozhraní .NET.
- **HttpContext.Current.User**. Tato vlastnost je specifická pro technologii ASP.NET.

Následující kód ukazuje, jak nastavit objekt zabezpečení:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Pro hostování webů, je potřeba nastavit objekt zabezpečení v obou místech; v opačném případě se může stát nekonzistentní kontext zabezpečení. Pro vlastní hostování, ale **HttpContext.Current** má hodnotu null. Aby váš kód je bez ohledu na hostitele, proto zkontrolujte null před přiřazením **HttpContext.Current**, jak je vidět.

## <a name="authorization"></a>Autorizace

Autorizace se stane později v kanálu, co nejblíže ke kontroleru. Který vám umožňuje provést podrobnější volby při uděluje přístup k prostředkům.

- *Filtry autorizace* spustit před akce kontroleru. Pokud požadavek není ověřen, filtr vrátí odpověď chyba a akce není vyvolána.
- V rámci akce kontroleru, můžete získat z aktuální objekt zabezpečení **ApiController.User** vlastnost. Může například filtrovat seznam prostředků podle uživatelské jméno, vrácení jenom prostředky, které patří do tohoto uživatele.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Pomocí [Povolit] atributu

Webové rozhraní API poskytuje integrované autorizační filtr, [třídy AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Tento filtr kontroluje, zda je uživatel ověřený. V opačném případě vrátí stavový kód HTTP 401 (Neautorizováno), bez vyvolání akce.

Můžete použít filtr globálně, na úrovni kontroleru, nebo na úrovni jednotlivých akcí.

**Globálně**: Pokud chcete omezit přístup pro každý kontroler Web API, přidejte **třídy AuthorizeAttribute** filtru do seznamu globálních filtrů:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Řadič**: Pokud chcete omezit přístup k určitému kontroleru, přidejte filtr jako atribut řadiče:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Akce**: Pokud chcete omezit přístup pro určité akce, přidejte atribut do metody akce:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Alternativně můžete omezit kontroler a pak umožnit anonymní přístup k určitým akcím, pomocí `[AllowAnonymous]` atribut. V následujícím příkladu `Post` metoda je omezená, ale `Get` metoda umožňuje anonymní přístup.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

V předchozích příkladech filtr povoluje všechny ověřené uživatele pro přístup s omezeným přístupem metody; jsou uchovány pouze anonymní uživatelé. Chcete-li omezit přístup na určité uživatele nebo pro uživatele v určitých rolích:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> **Třídy AuthorizeAttribute** filtru pro řadiče webového rozhraní API se nachází v **System.Web.Http** oboru názvů. Není k dispozici podobné filtr pro kontrolery MVC v **System.Web.Mvc** názvů, který není kompatibilní s řadiči webového rozhraní API.


### <a name="custom-authorization-filters"></a>Filtry autorizace

Zápis filtru autorizace, odvodíte z jedné z těchto typů:

- **Třídy AuthorizeAttribute**. Rozšíření této třídy lze provádět logiku ověřování na základě aktuálního uživatele a role uživatele.
- **AuthorizationFilterAttribute**. Rozšíření této třídy lze provádět logiku synchronní autorizace, který není nutně na základě aktuální uživatel nebo role.
- **IAuthorizationFilter**. Implementace tohoto rozhraní k provedení asynchronní autorizace logiku; například, pokud je logika ověřování volá asynchronní vstupně-výstupních operací nebo sítě. (Pokud je logika ověřování vázané na procesor, je jednodušší odvozena od **AuthorizationFilterAttribute**, protože pak nemusíte zápis asynchronní metody.)

Následující diagram znázorňuje hierarchie tříd pro **třídy AuthorizeAttribute** třídy.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorizace uvnitř akce Kontroleru

V některých případech může povolit požadavek na pokračovat, ale změnit chování založené na objekt zabezpečení. Například informace, které vrátíte může změnit v závislosti na roli uživatele. V rámci metoda kontroleru, můžete získat z aktuální Princip **ApiController.User** vlastnost.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
