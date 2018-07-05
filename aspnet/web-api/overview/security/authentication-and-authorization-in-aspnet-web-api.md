---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Ověřování a autorizace v rozhraní ASP.NET Web API | Dokumentace Microsoftu
author: MikeWasson
description: Poskytuje obecný přehled o ověřování a autorizace v rozhraní ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 981eebeaa1daaf85cb90a52f073c88cb71099edb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365617"
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Ověřování a autorizace v rozhraní ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Vytvoření webového rozhraní API, ale nyní chcete řídit přístup k němu. V této sérii článků se podíváme na některé možnosti pro zabezpečení webového rozhraní API před neoprávněnými uživateli. Tato série probereme ověřování a autorizace.

- *Ověřování* je znalost identitu uživatele. Například Alice přihlásí pomocí svého uživatelského jména a hesla, a server používá heslo k ověření Alice.
- *Autorizace* rozhoduje, jestli uživatel může provádět akce. Alice má například oprávnění získat prostředek, ale ne vytvořit prostředek.

První článek v sérii poskytuje obecný přehled o ověřování a autorizace v rozhraní ASP.NET Web API. Další témata popisují běžné scénáře ověřování pro webové rozhraní API.

> [!NOTE]
> Díky ti, kdo zkontrolovat tuto řadu a poskytuje cenné názory: Rick Anderson, Levi Broderick, Jiří Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.


## <a name="authentication"></a>Ověřování

Webové rozhraní API předpokládá, že v hostiteli se stane, že ověřování. Hostitel je pro hostování webů služby IIS, která používá z modulů HTTP pro ověřování. Můžete nakonfigurovat projektu pro použití žádné z modulů ověření integrované do služby IIS nebo v technologii ASP.NET nebo napsat vlastní modul HTTP pro provedení vlastního ověření.

Když hostitel ověří uživatele, vytvoří *instančního objektu*, což je [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) objekt, který reprezentuje kontext zabezpečení, pod kterým běží kód. Hostitel připojí objekt zabezpečení pro aktuální vlákno nastavením **vlastnost Thread.CurrentPrincipal**. Objekt zabezpečení obsahuje přiřazený **Identity** objekt, který obsahuje informace o uživateli. Pokud uživatel je ověřen, **Identity.IsAuthenticated** vrátí vlastnost **true**. U anonymních požadavků **ověření identity** vrátí **false**. Další informace o objektech najdete v tématu [zabezpečení na základě](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Obslužné rutiny zpráv HTTP pro ověřování

Místo tohoto hostitele používat pro ověřování, můžete umístit logika ověřování do [obslužná rutina zprávy HTTP](../advanced/http-message-handlers.md). V takovém případě obslužná rutina zprávy prozkoumá požadavek HTTP a nastaví objekt zabezpečení.

Kdy byste měli použít obslužné rutiny zpráv pro ověřování? Tady jsou některé nevýhody:

- Modulu HTTP se zobrazí všechny požadavky, které procházejí kanálu ASP.NET. Obslužná rutina zprávy se zobrazují pouze požadavky, které jsou směrovány do webového rozhraní API.
- Můžete nastavit obslužné rutiny zpráv na trasy, která vám umožní použít schéma ověřování na konkrétní postup.
- Z modulů HTTP jsou specifické pro službu IIS. Obslužné rutiny zpráv jsou nezávislá na hostitele, takže je možné s hostování webů a samoobslužné hostování.
- Z modulů HTTP účasti v protokolování služby IIS, auditování a tak dále.
- Z modulů HTTP spouštět dříve v kanálu. Pokud budete zpracovávat ověřování v obslužné rutiny zpráv, získat objekt zabezpečení není nastavený, dokud se obslužná rutina se spustí. Kromě toho se objekt zabezpečení vrátí zpět na předchozí objekt zabezpečení při odpovědi opustí obslužná rutina zprávy.

Obecně platí Pokud už nebudete potřebovat pro podporu s vlastním hostováním, modul HTTP je lepší volbou. Pokud budete potřebovat pro podporu s vlastním hostováním, vezměte v úvahu obslužné rutiny zpráv.

### <a name="setting-the-principal"></a>Nastavení objektu zabezpečení

Pokud vaše aplikace provádí všechny vlastní ověřovací logiku, je nutné nastavit objekt zabezpečení na dvou místech:

- **Vlastnost Thread.CurrentPrincipal**. Tato vlastnost je standardní způsob, jak nastavit objekt zabezpečení vlákna v rozhraní .NET.
- **HttpContext.Current.User**. Tato vlastnost je specifické pro technologii ASP.NET.

Následující kód ukazuje, jak nastavit zabezpečení:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Pro hostování webů, je nutné nastavit objekt zabezpečení na obou místech; v opačném případě kontextu zabezpečení může být nekonzistentní. Hostování na vlastním však **HttpContext.Current** má hodnotu null. Aby váš kód je nezávislá na hostitele, proto se zajišťuje hodnota null před přiřazením **HttpContext.Current**, jak je znázorněno.

## <a name="authorization"></a>Autorizace

Později v kanálu, se stane autorizace blíž ke kontroleru. Který vám umožňuje provést podrobnější volby, pokud udělíte přístup k prostředkům.

- *Filtry autorizace* spuštěn akce kontroleru. Pokud požadavek není autorizovaný, filtr vrátí odpověď a není vyvolána akce.
- V rámci akce kontroleru, můžete získat aktuální objekt zabezpečení z **ApiController.User** vlastnost. Například můžete filtrovat seznam prostředků podle uživatelské jméno, vracet jenom prostředky, které patří do daného uživatele.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Použití [Povolit] atributu

Webové rozhraní API poskytuje integrované autorizační filtr, [třídy AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Tento filtr kontroluje, zda je uživatel ověřený. V opačném případě vrátí stavový kód HTTP 401 (Neautorizováno), bez vyvolání akce.

Můžete použít filtr na úrovni kontroleru globálně, nebo na úrovni jednotlivých akcí.

**Globálně**: Pokud chcete omezit přístup pro každý kontroler Web API, přidejte **třídy AuthorizeAttribute** filtr na seznam globálních filtrů:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Kontroler**: Pokud chcete omezit přístup k určitému kontroleru, přidejte filtr jako atribut ke kontroleru:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Akce**: Pokud chcete omezit přístup pro určité akce, přidejte atribut do metody akce:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Alternativně můžete omezit kontroler a pak umožnit anonymní přístup k určitým akcím s použitím `[AllowAnonymous]` atribut. V následujícím příkladu `Post` metoda je omezená, ale `Get` metoda umožňuje anonymní přístup.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

V předchozích příkladech filtr umožňuje všem ověřeným uživatelům přístup k metodám s omezeným přístupem; navýšení kapacity jsou uchovány pouze anonymní uživatelé. Můžete také omezit přístup pro určité uživatele nebo pro uživatele v určitých rolích:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> **Třídy AuthorizeAttribute** filtru pro kontrolery rozhraní Web API se nachází v **System.Web.Http** oboru názvů. Je podobné filtr pro kontrolery MVC v **System.Web.Mvc** obor názvů, který není kompatibilní s kontrolerů rozhraní Web API.


### <a name="custom-authorization-filters"></a>Filtry autorizace

Zápis filtru autorizace, odvození od některého z těchto typů:

- **Třídy AuthorizeAttribute**. Rozšíření této třídy provádět logiku autorizace na základě aktuálního uživatele a role uživatele.
- **AuthorizationFilterAttribute**. Rozšíření této třídy pro provádění synchronní autorizace logiky, která není nutně založená na aktuální uživatel nebo role.
- **IAuthorizationFilter**. Implementace tohoto rozhraní pro provádění asynchronní autorizace logiky; například, pokud svoji logiku autorizace volá asynchronní vstupně-výstupních operací nebo sítě. (Pokud svoji logiku autorizace je vázané na procesor, je jednodušší odvodit z **AuthorizationFilterAttribute**, protože pak není nutné psát asynchronní metody.)

Následující diagram znázorňuje hierarchii tříd pro **třídy AuthorizeAttribute** třídy.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Povolení uvnitř akce Kontroleru

V některých případech může umožnit požadavek na pokračovat, ale změnit chování podle objektu zabezpečení. Například informace, které vrátíte se může změnit v závislosti na roli uživatele. V rámci metody kontroleru, můžete získat z aktuální zásady **ApiController.User** vlastnost.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
