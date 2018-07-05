---
uid: web-api/overview/security/authentication-filters
title: Filtry ověřování v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: Filtr ověřování je komponenta, která ověřuje požadavek HTTP. Rozhraní Web API 2 a MVC 5 obě podporují filtry ověřování, ale mírně se liší...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: be2dcb246597f90ed7f00b2cf647b92e44aa254c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385674"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>Filtry ověřování v rozhraní ASP.NET Web API 2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

> Filtr ověřování je komponenta, která ověřuje požadavek HTTP. Rozhraní Web API 2 a MVC 5 podporovaly filtry ověřování, ale mírně se liší především v zásady vytváření názvů pro rozhraní filtru. Toto téma popisuje filtry ověřování webové rozhraní API.


Filtry ověřování můžete tak nastavit schéma ověřování pro individuální řadiče nebo akce. Tímto způsobem, vaše aplikace může podporovat různých ověřovacích mechanismů pro různé zdroje HTTP.

V tomto článku ukážeme si kód z [základní ověřování](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) ukázku [ http://aspnet.codeplex.com ](http://aspnet.codeplex.com). Tato ukázka vysvětluje filtr ověřování, která implementuje schéma HTTP Basic Authentication přístup (RFC 2617). Tento filtr je implementována ve třídě s názvem `IdentityBasicAuthenticationAttribute`. Můžu se nezobrazí veškerý kód ze vzorku, pouze ty části, které ukazují, jak napsat filtr ověřování.

## <a name="setting-an-authentication-filter"></a>Nastavení filtr ověřování

Stejně jako ostatní filtry může být filtry ověřování použité na řadič, na akci nebo globálně pro všechny kontrolery rozhraní Web API.

Použít filtr ověřování pro kontroler, uspořádání třídy kontroleru pomocí atributu filtru. Následující kód nastaví `[IdentityBasicAuthentication]` filtr na třídu kontroleru, který umožňuje základní ověřování pro všechny akce kontroleru.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Použít filtr pro jednu akci, uspořádání s filtrem akce. Následující kód nastaví `[IdentityBasicAuthentication]` filtr na kontroleru `Post` metody.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Chcete-li použít filtr na všechny řadiče webové rozhraní API, přidejte ho do **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementace filtru ověřování webové rozhraní API

V rozhraní Web API implementovat filtry ověřování [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) rozhraní. Také by měla dědit z **System.Attribute**, aby bylo možné použít jako atributy.

**IAuthenticationFilter** rozhraní má dvě metody:

- **AuthenticateAsync** ověří se tato žádost ověřením pověření v požadavku, pokud jsou k dispozici.
- **ChallengeAsync** v případě potřeby přidá výzvu ověřování odpovědi HTTP.

Tyto metody, které odpovídají na tok ověřování definované v [RFC 2612](http://tools.ietf.org/html/rfc2616) a [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. Klient odešle přihlašovací údaje v autorizační hlavičce. To obvykle proběhne, když klient obdrží odpověď 401 (Neautorizováno) ze serveru. Klient však můžete odesílat přihlašovací údaje s žádnou žádostí nejen po získání zobrazuje chyba 401.
2. Pokud server nepřijímá přihlašovací údaje, vrátí odpověď 401 (Neautorizováno). Odpověď obsahuje hlavičku Www-Authenticate, který obsahuje jeden nebo více výzev. Každé výzvy Určuje schéma ověřování rozpoznána serverem.

Server můžete také vrátit 401 z anonymní žádosti. Ve skutečnosti je to obvykle jak zahájit proces ověřování:

1. Klient odešle anonymní žádosti.
2. Server vrací 401.
3. Klienti odešle požadavek s přihlašovacími údaji znovu.

Tento tok obsahuje *ověřování* a *autorizace* kroky.

- Ověřování prokáže identity klienta.
- Autorizace následně určí, zda má klient přístup k určitému prostředku.

Filtry ověřování v rozhraní Web API zpracovávat ověřování, ale ne autorizaci. Autorizace se má počítat filtr autorizace nebo uvnitř akce kontroleru.

Tady je tok v kanálu Web API 2:

1. Webové rozhraní API před vyvoláním akci, vytvoří seznam filtrů ověřování pro danou akci. To zahrnuje filtry s rozsahem akci, kontroler oboru a globální obor.
2. Webové rozhraní API volá **AuthenticateAsync** na každý filtr v seznamu. Každého filtru lze ověřit přihlašovací údaje v požadavku. Pokud libovolný filtr úspěšně ověří přihlašovací údaje, filtr vytvoří **IPrincipal** a připojí ho k požadavku. Filtr můžete také aktivovat chybu v tomto okamžiku. Pokud ano, zbývající kanál se nespustí.
3. Za předpokladu, že se nezobrazí žádná chyba, požadavek prochází přes rest z kanálu.
4. Nakonec webového rozhraní API volá každý filtr ověřování **ChallengeAsync** metody. Filtry tuto metodu použijte v případě potřeby přidat výzvu v odpovědi. Obvykle (ale ne vždy), který bude možné v reakci na chybu 401.

Následující diagramy znázorňují dva možné případy. V první filtr ověřování úspěšně ověří žádost, filtr autorizace ověří požadavek a akce kontroleru vrátí hodnotu 200 (OK).

![](authentication-filters/_static/image1.png)

V druhém příkladu požadavek ověří filtr ověřování, ale filtr autorizace vrátí 401 (Neautorizováno). V takovém případě není vyvolána akce kontroleru. Ověření filtru přidá do odpovědi hlavičku Www-Authenticate.

![](authentication-filters/_static/image2.png)

Jiné kombinace jsou možné&mdash;například, pokud akce kontroleru umožňuje anonymní požadavky, bude pravděpodobně filtr ověřování, ale bez autorizace.

## <a name="implementing-the-authenticateasync-method"></a>Implementace metody AuthenticateAsync

**AuthenticateAsync** metoda se pokusí ověřit žádost. Následuje podpis metody:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

**AuthenticateAsync** metody musíte udělat jednu z následujících akcí:

1. Nothing (no-op).
2. Vytvoření **IPrincipal** a nastavte ho na požadavek.
3. Nastavte chybného výsledku.

Možnost (1) znamená, že požadavek neobsahoval žádné přihlašovací údaje, které rozumí filtr. Možnost (2) znamená, že filtr úspěšně ověřit žádost. Možnost (3) znamená, že žádost má neplatné přihlašovací údaje (například chybné heslo), aktivuje se reakce na chybu.

Tady je obecný postup pro implementaci **AuthenticateAsync**.

1. Vyhledejte přihlašovací údaje v požadavku.
2. Pokud neexistují žádné přihlašovací údaje, neprovádějte žádnou akci a vrátí (no-op).
3. Pokud jsou přihlašovací údaje, ale filtr nerozpozná schéma ověřování, neprovádějte žádnou akci a vrátí (no-op). Schéma může pochopit jiný filtr v kanálu.
4. Pokud jsou přihlašovací údaje, které rozumí filtr, pokusí se jejich ověření.
5. Pokud přihlašovací údaje jsou chybná, vrátí 401 nastavením `context.ErrorResult`.
6. Pokud jsou pověření platná, vytvořte **IPrincipal** a nastavte `context.Principal`.

Ukazuje následující kód **AuthenticateAsync** metodu z [základní ověřování](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) vzorku. Komentáře označte každý krok. Kód ukazuje několik typů Chyba: autorizační hlavičky bez přihlašovacích údajů, poškozený přihlašovací údaje a chybné uživatelské jméno a heslo.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Nastavení chybného výsledku

Pokud přihlašovací údaje jsou neplatné, musíte nastavit filtr `context.ErrorResult` do **IHttpActionResult** , který vytváří reakce na chybu. Další informace o **IHttpActionResult**, naleznete v tématu [výsledky akcí ve webovém rozhraní API 2](../getting-started-with-aspnet-web-api/action-results.md).

Ukázka základní ověřování zahrnuje `AuthenticationFailureResult` třídu, která je vhodná pro tento účel.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementace ChallengeAsync

Účelem **ChallengeAsync** metodou je přidat výzev ověřování pro odpověď, a to v případě potřeby. Následuje podpis metody:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Metoda je volána v každé ověřování filtr v kanálu požadavku.

Je důležité si uvědomit, že **ChallengeAsync** nazývá *před* odpověď HTTP, která je vytvořená a případně i před spuštěním akce kontroleru. Když **ChallengeAsync** se nazývá `context.Result` obsahuje **IHttpActionResult**, které se později používá k vytvoření odpovědi HTTP. Takže když **ChallengeAsync** je volána, nic o odpovědi HTTP ještě neznáte. **ChallengeAsync** metody by měly nahradit původní hodnoty `context.Result` s novou **IHttpActionResult**. To **IHttpActionResult** musíte zabalit původní `context.Result`.

![](authentication-filters/_static/image3.png)

Já ho nazvu třeba původní **IHttpActionResult** *vnitřní výsledek*a nové **IHttpActionResult** *vnější výsledek*. Vnější výsledek musíte udělat toto:

1. Vyvolá vnitřní výsledek pro vytvoření odpovědi HTTP.
2. Zkontrolujte odpovědi.
3. V případě potřeby přidáte výzvu ověřování k odpovědi.

V následujícím příkladu je převzat z ukázky základní ověřování. Definuje **IHttpActionResult** pro vnější výsledek.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

`InnerResult` Vlastnost obsahuje vnitřní **IHttpActionResult**. `Challenge` Vlastnost představuje hlavičky Www-ověřování. Všimněte si, že **ExecuteAsync** nejprve volá `InnerResult.ExecuteAsync` k vytvoření odpovědi HTTP a pak přidá před obrovskou výzvou – v případě potřeby.

Před přidáním před obrovskou výzvou – zkontrolujte kód odpovědi. Většina schémat ověřování pouze přidat výzvu Pokud odpověď 401, jak je znázorněno zde. Ale určitá schémata ověřování přidat výzvu úspěšná odpověď. Viz například [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Zadaný `AddChallengeOnUnauthorizedResult` třídy, skutečný kód **ChallengeAsync** je jednoduché. Stačí vytvořit výsledek a připojte ji k `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Poznámka: Ukázkové základní ověřování abstrahuje tuto logiku trochu, tak, že v metodě rozšíření.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Kombinování filtry ověřování s ověřováním na úrovni hostitele

"Ověřování na úrovni hostitele" je ověřování prováděné tímto hostitelem (například služby IIS), než rozhraní požadavek dosáhne webového rozhraní API.

Často můžete povolit ověření na úrovni hostitele pro ostatní aplikace, ale zakázat pro kontrolerům rozhraní Web API. Typický scénář je třeba povolit ověřování pomocí formulářů na úrovni hostitele, ale používat ověřování založené na tokenech pro webové rozhraní API.

Chcete-li zakázat ověřování na úrovni hostitele do kanálu webové rozhraní API, zavolejte `config.SuppressHostPrincipal()` ve vaší konfiguraci. To způsobí, že webové rozhraní API k odebrání **IPrincipal** z všechny požadavky, které přejde do kanálu webové rozhraní API. Efektivně, ho &quot;zrušení-ověřuje&quot; požadavku.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Další prostředky

[Filtry zabezpečení rozhraní ASP.NET Web API](https://msdn.microsoft.com/magazine/dn781361.aspx) (Zpravodaj MSDN Magazine)
