---
uid: web-api/overview/security/authentication-filters
title: "Filtry ověřování v rozhraní ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: "Filtr ověřování je komponenta, která ověřuje požadavek HTTP. Rozhraní Web API 2 i MVC 5 podporovaly filtry ověřování, ale se mírně liší..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 16e451f52799625983368bc938091eff47019b52
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>Filtry ověřování v rozhraní ASP.NET Web API 2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> Filtr ověřování je komponenta, která ověřuje požadavek HTTP. Rozhraní Web API 2 i MVC 5 podporovaly filtry ověřování, ale liší mírně, většinou se zásady vytváření názvů pro rozhraní filtru. Toto téma popisuje filtry ověřování webového rozhraní API.


Filtry ověřování umožňují nastavit příslušné schéma ověřování pro jednotlivé řadiče nebo akce. Tímto způsobem, vaše aplikace může podporovat různé ověřovací mechanismy, které pro různé prostředky HTTP.

V tomto článku budete zobrazit kód z [základní ověřování](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) ukázku na [http://aspnet.codeplex.com](http://aspnet.codeplex.com). Ukázka zobrazuje filtr ověřování, který implementuje schéma základní ověřování přístupu k protokolu HTTP (RFC 2617). Filtr je implementována do třídy s názvem `IdentityBasicAuthenticationAttribute`. Nezobrazí I všechny kód z ukázce právě části, které ukazují, jak zapsat filtr ověřování.

## <a name="setting-an-authentication-filter"></a>Nastavení filtr ověřování

Podobně jako ostatní filtry může být filtry ověřování použité na řadič, -action nebo globálně na všechny řadiče webového rozhraní API.

Pokud chcete použít filtr ověřování na řadič, uspořádání třídy kontroleru s atributem filtru. Následující kód nastaví `[IdentityBasicAuthentication]` filtru na řadič třídy, která umožňuje základní ověřování pro všechny akce kontroleru.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Pokud chcete použít filtr jednu akci, uspořádání s filtrem akce. Následující kód nastaví `[IdentityBasicAuthentication]` filtru na kontroleru `Post` metoda.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Chcete-li filtr použít na všechny řadiče webového rozhraní API, přidejte ho do **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementace filtru webové rozhraní API ověřování

V rozhraní Web API implementovat filtry ověřování [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) rozhraní. Také musí dědit z **System.Attribute**, aby bylo možné použít jako atributy.

**IAuthenticationFilter** rozhraní má dvě metody:

- **AuthenticateAsync** ověří žádost tím, že ověří přihlašovací údaje v požadavku, pokud je k dispozici.
- **ChallengeAsync** přidá výzvu ověřování odpovědi HTTP, v případě potřeby.

Tyto metody odpovídají tok ověřování definované v [RFC 2612](http://tools.ietf.org/html/rfc2616) a [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. Klient odešle přihlašovací údaje v hlavičce autorizace. K tomu obvykle dochází, když klient obdrží odpověď na 401 (Neautorizováno) ze serveru. Klient však může odesílat přihlašovací údaje s žádnou žádostí, ne jenom po získání 401.
2. Pokud server nepřijímá přihlašovací údaje, vrátí odpověď na 401 (Neautorizováno). Odpověď obsahuje hlavičku Www-Authenticate, která obsahuje jeden nebo více výzev. Každý výzvy určuje příslušné schéma ověřování rozpoznáno serverem.

Server může také vracet 401 z anonymní žádosti. Ve skutečnosti, který je obvykle jak spustil proces ověřování:

1. Klient odešle žádost o anonymní.
2. Server vrátí 401.
3. Klienti znovu odešle žádost s přihlašovacími údaji.

Tento tok zahrnuje obě *ověřování* a *autorizace* kroky.

- Ověřování prokáže identitu klienta.
- Autorizace určuje, zda má klient přístup k určitému prostředku.

Filtry ověřování v rozhraní Web API zpracovávat ověřování, ale není autorizace. Autorizace je potřeba filtr autorizace nebo uvnitř akce kontroleru.

Tady je postup v kanálu webovém rozhraní API 2:

1. Před vyvoláním akce, webového rozhraní API vytvoří seznam filtrů ověřování pro tuto akci. To zahrnuje filtry se akce obor, řadiče oboru a globální obor.
2. Volání rozhraní API webové **AuthenticateAsync** na každý filtru v seznamu. Každého filtru lze ověřit přihlašovací údaje v požadavku. Pokud se libovolný filtr úspěšně ověří přihlašovací údaje, filtr vytvoří **IPrincipal** a připojí k požadavku. Filtr lze také spustit chybu v tomto okamžiku. Pokud ano, zbytek kanálu nelze spustit.
3. Za předpokladu, že se nezobrazí žádná chyba, žádost toky prostřednictvím rest kanálu.
4. Nakonec webového rozhraní API volá každé ověřování filtru **ChallengeAsync** metoda. Filtry tuto metodu použijte v případě potřeby přidat výzvu do odpovědi. Obvykle (ale ne vždy), by dojít v odpovědi na 401 chyba.

Následující diagramy znázorňují dva možné případy. V první filtr ověřování úspěšně ověří žádost, filtr autorizace ověří požadavek a akce kontroleru vrátí hodnotu 200 (OK).

![](authentication-filters/_static/image1.png)

V druhém příkladu filtr ověřování ověří žádost, ale filtr autorizace vrátí 401 (Neautorizováno). V takovém případě není vyvolána akce kontroleru. Ověřování filtr přidá do odpovědi hlavičku Www-Authenticate.

![](authentication-filters/_static/image2.png)

Je možné, jiné kombinace&mdash;například pokud akce kontroleru umožňuje anonymních požadavků, může být filtr ověřování, ale žádné oprávnění.

## <a name="implementing-the-authenticateasync-method"></a>Implementace metody AuthenticateAsync

**AuthenticateAsync** metoda se pokusí o ověření požadavku. Tady je podpis metody:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

**AuthenticateAsync** metoda musíte udělat jednu z následujících akcí:

1. Nothing (no-op).
2. Vytvoření **IPrincipal** a nastavte ji na žádosti.
3. Nastavte chybného výsledku.

Možnost (1) znamená požadavek neměl žádné přihlašovací údaje, které rozpozná filtru. Možnost (2) znamená filtr úspěšně ověřit žádost. Možnost (3) znamená žádost měla neplatné přihlašovací údaje (například chybné heslo), která aktivuje chybnou odpověď.

Tady je obecný postup pro implementaci **AuthenticateAsync**.

1. Podívejte se na přihlašovací údaje v požadavku.
2. Pokud neexistují žádné přihlašovací údaje, Neprovádět žádnou akci a vrátí (no-op).
3. Pokud jsou přihlašovací údaje, ale filtru nebyl rozpoznán schéma ověřování, Neprovádět žádnou akci a vrátí (no-op). Jiné filtru v kanálu může pochopit schéma.
4. Pokud jsou přihlašovací údaje, které rozumí filtr, opakujte pokus o ověření je.
5. Pokud přihlašovací údaje jsou chybná, vrátí 401 nastavením `context.ErrorResult`.
6. Pokud jsou pověření platná, vytvořte **IPrincipal** a nastavte `context.Principal`.

Zobrazí kód postupujte podle kroků **AuthenticateAsync** metoda z [základní ověřování](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) ukázka. Komentáře znamenat každý krok. Kód ukazuje několik typů Chyba: autorizační hlavičky bez přihlašovacích údajů, poškozený přihlašovací údaje a chybné uživatelské jméno a heslo.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Nastavení chybného výsledku

Pokud přihlašovací údaje jsou neplatné, musíte nastavit filtr `context.ErrorResult` k **IHttpActionResult** vytvářející chybnou odpověď. Další informace o **IHttpActionResult**, najdete v části [výsledky akce ve webovém rozhraní API 2](../getting-started-with-aspnet-web-api/action-results.md).

Ukázka základní ověřování zahrnuje `AuthenticationFailureResult` třídu, která je vhodná pro tento účel.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementace ChallengeAsync

Účelem **ChallengeAsync** metodou je přidání výzev ověřování pro odpověď, a to v případě potřeby. Tady je podpis metody:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Na každé ověřování filtru v kanálu požadavku je volání metody.

Je důležité pochopit, že **ChallengeAsync** nazývá *před* odpověď HTTP je vytvořený a případně i před spuštěním akce kontroleru. Když **ChallengeAsync** je volána, `context.Result` obsahuje **IHttpActionResult**, které se později používá k vytvoření odpovědi HTTP. Takže když **ChallengeAsync** je volána, bez znalosti o odpověď HTTP ještě. **ChallengeAsync** metoda by měla nahradit původní hodnotu `context.Result` s novou **IHttpActionResult**. To **IHttpActionResult** musíte zabalit původní `context.Result`.

![](authentication-filters/_static/image3.png)

Můžu budete volat původní **IHttpActionResult** *vnitřní výsledek*a nové **IHttpActionResult** *vnější výsledek*. Vnější výsledek postupujte takto:

1. Vyvolání vnitřní výsledek, který má vytvořit odpověď HTTP.
2. Zkontrolujte odpovědi.
3. Přidáte výzvu ověřování odpovědi, v případě potřeby.

Následující příklad je převzat ze ukázka základní ověřování. Definuje, **IHttpActionResult** pro vnější výsledek.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

`InnerResult` Vlastnost obsahuje vnitřní **IHttpActionResult**. `Challenge` Vlastnost představuje hlavičku Www-ověřování. Všimněte si, že **ExecuteAsync** nejprve volá `InnerResult.ExecuteAsync` vytvořit odpověď HTTP a potom přidá výzvy v případě potřeby.

Před přidáním výzvy ověřil kód odpovědi. Většina schémat ověřování přidat výzvu pouze pokud je odpověď na 401, jak je vidět tady. Ale některé schémat ověřování přidat výzvu úspěšná odpověď. Například v tématu [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Zadané `AddChallengeOnUnauthorizedResult` třídy, skutečný kód v **ChallengeAsync** je jednoduchá. Právě vytvoření výsledku a připojte ji k `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Poznámka: Ukázkové základní ověřování abstrahuje této logiky trochu, umístění v metody rozšíření.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Kombinování filtry ověřování s ověřováním na úrovni hostitele

"Ověřování na úrovni hostitele" ověřování prováděné tímto hostitelem (například IIS), je před framework dosáhnou webového rozhraní API žádosti.

Často můžete chtít povolit ověřování na úrovni hostitele pro zbytek vaší aplikace, ale zakázat pro řadičů webového rozhraní API. Typický scénář je třeba povolit ověřování pomocí formulářů na úrovni hostitele, ale používat ověřování na základě tokenu pro rozhraní Web API.

Chcete-li zakázat ověřování na úrovni hostitele uvnitř kanál rozhraní Web API, volejte `config.SuppressHostPrincipal()` ve vaší konfiguraci. To způsobí, že webového rozhraní API k odebrání **IPrincipal** z jakékoli požadavky, které zadá kanál rozhraní Web API. Prakticky ho &quot;zrušení-ověřuje&quot; žádosti.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Další prostředky

[Filtry zabezpečení rozhraní ASP.NET Web API](https://msdn.microsoft.com/magazine/dn781361.aspx) (Časopis MSDN)
