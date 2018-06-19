---
uid: web-api/overview/security/authentication-filters
title: Filtry ověřování v rozhraní ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Filtr ověřování je komponenta, která ověřuje požadavek HTTP. Rozhraní Web API 2 i MVC 5 podporovaly filtry ověřování, ale se mírně liší...
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
ms.locfileid: "29153515"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a><span data-ttu-id="3b4ef-104">Filtry ověřování v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="3b4ef-104">Authentication Filters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="3b4ef-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3b4ef-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="3b4ef-106">Filtr ověřování je komponenta, která ověřuje požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-106">An authentication filter is a component that authenticates an HTTP request.</span></span> <span data-ttu-id="3b4ef-107">Rozhraní Web API 2 i MVC 5 podporovaly filtry ověřování, ale liší mírně, většinou se zásady vytváření názvů pro rozhraní filtru.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-107">Web API 2 and MVC 5 both support authentication filters, but they differ slightly, mostly in the naming conventions for the filter interface.</span></span> <span data-ttu-id="3b4ef-108">Toto téma popisuje filtry ověřování webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-108">This topic describes Web API authentication filters.</span></span>


<span data-ttu-id="3b4ef-109">Filtry ověřování umožňují nastavit příslušné schéma ověřování pro jednotlivé řadiče nebo akce.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-109">Authentication filters let you set an authentication scheme for individual controllers or actions.</span></span> <span data-ttu-id="3b4ef-110">Tímto způsobem, vaše aplikace může podporovat různé ověřovací mechanismy, které pro různé prostředky HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-110">That way, your app can support different authentication mechanisms for different HTTP resources.</span></span>

<span data-ttu-id="3b4ef-111">V tomto článku budete zobrazit kód z [základní ověřování](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) ukázku na [http://aspnet.codeplex.com](http://aspnet.codeplex.com). Ukázka zobrazuje filtr ověřování, který implementuje schéma základní ověřování přístupu k protokolu HTTP (RFC 2617).</span><span class="sxs-lookup"><span data-stu-id="3b4ef-111">In this article, I'll show code from the [Basic Authentication](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) sample on [http://aspnet.codeplex.com](http://aspnet.codeplex.com). The sample shows an authentication filter that implements the HTTP Basic Access Authentication scheme (RFC 2617).</span></span> <span data-ttu-id="3b4ef-112">Filtr je implementována do třídy s názvem `IdentityBasicAuthenticationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-112">The filter is implemented in a class named `IdentityBasicAuthenticationAttribute`.</span></span> <span data-ttu-id="3b4ef-113">Nezobrazí I všechny kód z ukázce právě části, které ukazují, jak zapsat filtr ověřování.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-113">I won't show all of the code from the sample, just the parts that illustrate how to write an authentication filter.</span></span>

## <a name="setting-an-authentication-filter"></a><span data-ttu-id="3b4ef-114">Nastavení filtr ověřování</span><span class="sxs-lookup"><span data-stu-id="3b4ef-114">Setting an Authentication Filter</span></span>

<span data-ttu-id="3b4ef-115">Podobně jako ostatní filtry může být filtry ověřování použité na řadič, -action nebo globálně na všechny řadiče webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-115">Like other filters, authentication filters can be applied per-controller, per-action, or globally to all Web API controllers.</span></span>

<span data-ttu-id="3b4ef-116">Pokud chcete použít filtr ověřování na řadič, uspořádání třídy kontroleru s atributem filtru.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-116">To apply an authentication filter to a controller, decorate the controller class with the filter attribute.</span></span> <span data-ttu-id="3b4ef-117">Následující kód nastaví `[IdentityBasicAuthentication]` filtru na řadič třídy, která umožňuje základní ověřování pro všechny akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-117">The following code sets the `[IdentityBasicAuthentication]` filter on a controller class, which enables Basic Authentication for all of the controller's actions.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

<span data-ttu-id="3b4ef-118">Pokud chcete použít filtr jednu akci, uspořádání s filtrem akce.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-118">To apply the filter to one action, decorate the action with the filter.</span></span> <span data-ttu-id="3b4ef-119">Následující kód nastaví `[IdentityBasicAuthentication]` filtru na kontroleru `Post` metoda.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-119">The following code sets the `[IdentityBasicAuthentication]` filter on the controller's `Post` method.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

<span data-ttu-id="3b4ef-120">Chcete-li filtr použít na všechny řadiče webového rozhraní API, přidejte ho do **GlobalConfiguration.Filters**.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-120">To apply the filter to all Web API controllers, add it to **GlobalConfiguration.Filters**.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a><span data-ttu-id="3b4ef-121">Implementace filtru webové rozhraní API ověřování</span><span class="sxs-lookup"><span data-stu-id="3b4ef-121">Implementing a Web API Authentication Filter</span></span>

<span data-ttu-id="3b4ef-122">V rozhraní Web API implementovat filtry ověřování [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-122">In Web API, authentication filters implement the [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interface.</span></span> <span data-ttu-id="3b4ef-123">Také musí dědit z **System.Attribute**, aby bylo možné použít jako atributy.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-123">They should also inherit from **System.Attribute**, in order to be applied as attributes.</span></span>

<span data-ttu-id="3b4ef-124">**IAuthenticationFilter** rozhraní má dvě metody:</span><span class="sxs-lookup"><span data-stu-id="3b4ef-124">The **IAuthenticationFilter** interface has two methods:</span></span>

- <span data-ttu-id="3b4ef-125">**AuthenticateAsync** ověří žádost tím, že ověří přihlašovací údaje v požadavku, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-125">**AuthenticateAsync** authenticates the request by validating credentials in the request, if present.</span></span>
- <span data-ttu-id="3b4ef-126">**ChallengeAsync** přidá výzvu ověřování odpovědi HTTP, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-126">**ChallengeAsync** adds an authentication challenge to the HTTP response, if needed.</span></span>

<span data-ttu-id="3b4ef-127">Tyto metody odpovídají tok ověřování definované v [RFC 2612](http://tools.ietf.org/html/rfc2616) a [RFC 2617](http://tools.ietf.org/html/rfc2617):</span><span class="sxs-lookup"><span data-stu-id="3b4ef-127">These methods correspond to the authentication flow defined in [RFC 2612](http://tools.ietf.org/html/rfc2616) and [RFC 2617](http://tools.ietf.org/html/rfc2617):</span></span>

1. <span data-ttu-id="3b4ef-128">Klient odešle přihlašovací údaje v hlavičce autorizace.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-128">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="3b4ef-129">K tomu obvykle dochází, když klient obdrží odpověď na 401 (Neautorizováno) ze serveru.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-129">This typically happens after the client receives a 401 (Unauthorized) response from the server.</span></span> <span data-ttu-id="3b4ef-130">Klient však může odesílat přihlašovací údaje s žádnou žádostí, ne jenom po získání 401.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-130">However, a client can send credentials with any request, not just after getting a 401.</span></span>
2. <span data-ttu-id="3b4ef-131">Pokud server nepřijímá přihlašovací údaje, vrátí odpověď na 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="3b4ef-131">If the server does not accept the credentials, it returns a 401 (Unauthorized) response.</span></span> <span data-ttu-id="3b4ef-132">Odpověď obsahuje hlavičku Www-Authenticate, která obsahuje jeden nebo více výzev.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-132">The response includes a Www-Authenticate header that contains one or more challenges.</span></span> <span data-ttu-id="3b4ef-133">Každý výzvy určuje příslušné schéma ověřování rozpoznáno serverem.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-133">Each challenge specifies an authentication scheme recognized by the server.</span></span>

<span data-ttu-id="3b4ef-134">Server může také vracet 401 z anonymní žádosti.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-134">The server can also return 401 from an anonymous request.</span></span> <span data-ttu-id="3b4ef-135">Ve skutečnosti, který je obvykle jak spustil proces ověřování:</span><span class="sxs-lookup"><span data-stu-id="3b4ef-135">In fact, that's typically how the authentication process is initiated:</span></span>

1. <span data-ttu-id="3b4ef-136">Klient odešle žádost o anonymní.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-136">The client sends an anonymous request.</span></span>
2. <span data-ttu-id="3b4ef-137">Server vrátí 401.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-137">The server returns 401.</span></span>
3. <span data-ttu-id="3b4ef-138">Klienti znovu odešle žádost s přihlašovacími údaji.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-138">The clients resends the request with credentials.</span></span>

<span data-ttu-id="3b4ef-139">Tento tok zahrnuje obě *ověřování* a *autorizace* kroky.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-139">This flow includes both *authentication* and *authorization* steps.</span></span>

- <span data-ttu-id="3b4ef-140">Ověřování prokáže identitu klienta.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-140">Authentication proves the identity of the client.</span></span>
- <span data-ttu-id="3b4ef-141">Autorizace určuje, zda má klient přístup k určitému prostředku.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-141">Authorization determines whether the client can access a particular resource.</span></span>

<span data-ttu-id="3b4ef-142">Filtry ověřování v rozhraní Web API zpracovávat ověřování, ale není autorizace.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-142">In Web API, authentication filters handle authentication, but not authorization.</span></span> <span data-ttu-id="3b4ef-143">Autorizace je potřeba filtr autorizace nebo uvnitř akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-143">Authorization should be done by an authorization filter or inside the controller action.</span></span>

<span data-ttu-id="3b4ef-144">Tady je postup v kanálu webovém rozhraní API 2:</span><span class="sxs-lookup"><span data-stu-id="3b4ef-144">Here is the flow in the Web API 2 pipeline:</span></span>

1. <span data-ttu-id="3b4ef-145">Před vyvoláním akce, webového rozhraní API vytvoří seznam filtrů ověřování pro tuto akci.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-145">Before invoking an action, Web API creates a list of the authentication filters for that action.</span></span> <span data-ttu-id="3b4ef-146">To zahrnuje filtry se akce obor, řadiče oboru a globální obor.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-146">This includes filters with action scope, controller scope, and global scope.</span></span>
2. <span data-ttu-id="3b4ef-147">Volání rozhraní API webové **AuthenticateAsync** na každý filtru v seznamu.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-147">Web API calls **AuthenticateAsync** on every filter in the list.</span></span> <span data-ttu-id="3b4ef-148">Každého filtru lze ověřit přihlašovací údaje v požadavku.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-148">Each filter can validate credentials in the request.</span></span> <span data-ttu-id="3b4ef-149">Pokud se libovolný filtr úspěšně ověří přihlašovací údaje, filtr vytvoří **IPrincipal** a připojí k požadavku.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-149">If any filter successfully validates credentials, the filter creates an **IPrincipal** and attaches it to the request.</span></span> <span data-ttu-id="3b4ef-150">Filtr lze také spustit chybu v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-150">A filter can also trigger an error at this point.</span></span> <span data-ttu-id="3b4ef-151">Pokud ano, zbytek kanálu nelze spustit.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-151">If so, the rest of the pipeline does not run.</span></span>
3. <span data-ttu-id="3b4ef-152">Za předpokladu, že se nezobrazí žádná chyba, žádost toky prostřednictvím rest kanálu.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-152">Assuming there is no error, the request flows through the rest of the pipeline.</span></span>
4. <span data-ttu-id="3b4ef-153">Nakonec webového rozhraní API volá každé ověřování filtru **ChallengeAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-153">Finally, Web API calls every authentication filter's **ChallengeAsync** method.</span></span> <span data-ttu-id="3b4ef-154">Filtry tuto metodu použijte v případě potřeby přidat výzvu do odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-154">Filters use this method to add a challenge to the response, if needed.</span></span> <span data-ttu-id="3b4ef-155">Obvykle (ale ne vždy), by dojít v odpovědi na 401 chyba.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-155">Typically (but not always) that would happen in response to a 401 error.</span></span>

<span data-ttu-id="3b4ef-156">Následující diagramy znázorňují dva možné případy.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-156">The following diagrams show two possible cases.</span></span> <span data-ttu-id="3b4ef-157">V první filtr ověřování úspěšně ověří žádost, filtr autorizace ověří požadavek a akce kontroleru vrátí hodnotu 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="3b4ef-157">In the first, the authentication filter successfully authenticates the request, an authorization filter authorizes the request, and the controller action returns 200 (OK).</span></span>

![](authentication-filters/_static/image1.png)

<span data-ttu-id="3b4ef-158">V druhém příkladu filtr ověřování ověří žádost, ale filtr autorizace vrátí 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="3b4ef-158">In the second example, the authentication filter authenticates the request, but the authorization filter returns 401 (Unauthorized).</span></span> <span data-ttu-id="3b4ef-159">V takovém případě není vyvolána akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-159">In this case, the controller action is not invoked.</span></span> <span data-ttu-id="3b4ef-160">Ověřování filtr přidá do odpovědi hlavičku Www-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-160">The authentication filter adds a Www-Authenticate header to the response.</span></span>

![](authentication-filters/_static/image2.png)

<span data-ttu-id="3b4ef-161">Je možné, jiné kombinace&mdash;například pokud akce kontroleru umožňuje anonymních požadavků, může být filtr ověřování, ale žádné oprávnění.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-161">Other combinations are possible&mdash;for example, if the controller action allows anonymous requests, you might have an authentication filter but no authorization.</span></span>

## <a name="implementing-the-authenticateasync-method"></a><span data-ttu-id="3b4ef-162">Implementace metody AuthenticateAsync</span><span class="sxs-lookup"><span data-stu-id="3b4ef-162">Implementing the AuthenticateAsync Method</span></span>

<span data-ttu-id="3b4ef-163">**AuthenticateAsync** metoda se pokusí o ověření požadavku.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-163">The **AuthenticateAsync** method tries to authenticate the request.</span></span> <span data-ttu-id="3b4ef-164">Tady je podpis metody:</span><span class="sxs-lookup"><span data-stu-id="3b4ef-164">Here is the method signature:</span></span>

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

<span data-ttu-id="3b4ef-165">**AuthenticateAsync** metoda musíte udělat jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="3b4ef-165">The **AuthenticateAsync** method must do one of the following:</span></span>

1. <span data-ttu-id="3b4ef-166">Nothing (no-op).</span><span class="sxs-lookup"><span data-stu-id="3b4ef-166">Nothing (no-op).</span></span>
2. <span data-ttu-id="3b4ef-167">Vytvoření **IPrincipal** a nastavte ji na žádosti.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-167">Create an **IPrincipal** and set it on the request.</span></span>
3. <span data-ttu-id="3b4ef-168">Nastavte chybného výsledku.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-168">Set an error result.</span></span>

<span data-ttu-id="3b4ef-169">Možnost (1) znamená požadavek neměl žádné přihlašovací údaje, které rozpozná filtru.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-169">Option (1) means the request did not have any credentials that the filter understands.</span></span> <span data-ttu-id="3b4ef-170">Možnost (2) znamená filtr úspěšně ověřit žádost.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-170">Option (2) means the filter successfully authenticated the request.</span></span> <span data-ttu-id="3b4ef-171">Možnost (3) znamená žádost měla neplatné přihlašovací údaje (například chybné heslo), která aktivuje chybnou odpověď.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-171">Option (3) means the request had invalid credentials (like the wrong password), which triggers an error response.</span></span>

<span data-ttu-id="3b4ef-172">Tady je obecný postup pro implementaci **AuthenticateAsync**.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-172">Here is a general outline for implementing **AuthenticateAsync**.</span></span>

1. <span data-ttu-id="3b4ef-173">Podívejte se na přihlašovací údaje v požadavku.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-173">Look for credentials in the request.</span></span>
2. <span data-ttu-id="3b4ef-174">Pokud neexistují žádné přihlašovací údaje, Neprovádět žádnou akci a vrátí (no-op).</span><span class="sxs-lookup"><span data-stu-id="3b4ef-174">If there are no credentials, do nothing and return (no-op).</span></span>
3. <span data-ttu-id="3b4ef-175">Pokud jsou přihlašovací údaje, ale filtru nebyl rozpoznán schéma ověřování, Neprovádět žádnou akci a vrátí (no-op).</span><span class="sxs-lookup"><span data-stu-id="3b4ef-175">If there are credentials but the filter does not recognize the authentication scheme, do nothing and return (no-op).</span></span> <span data-ttu-id="3b4ef-176">Jiné filtru v kanálu může pochopit schéma.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-176">Another filter in the pipeline might understand the scheme.</span></span>
4. <span data-ttu-id="3b4ef-177">Pokud jsou přihlašovací údaje, které rozumí filtr, opakujte pokus o ověření je.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-177">If there are credentials that the filter understands, try to authenticate them.</span></span>
5. <span data-ttu-id="3b4ef-178">Pokud přihlašovací údaje jsou chybná, vrátí 401 nastavením `context.ErrorResult`.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-178">If the credentials are bad, return 401 by setting `context.ErrorResult`.</span></span>
6. <span data-ttu-id="3b4ef-179">Pokud jsou pověření platná, vytvořte **IPrincipal** a nastavte `context.Principal`.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-179">If the credentials are valid, create an **IPrincipal** and set `context.Principal`.</span></span>

<span data-ttu-id="3b4ef-180">Zobrazí kód postupujte podle kroků **AuthenticateAsync** metoda z [základní ověřování](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) ukázka.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-180">The follow code shows the **AuthenticateAsync** method from the [Basic Authentication](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) sample.</span></span> <span data-ttu-id="3b4ef-181">Komentáře znamenat každý krok.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-181">The comments indicate each step.</span></span> <span data-ttu-id="3b4ef-182">Kód ukazuje několik typů Chyba: autorizační hlavičky bez přihlašovacích údajů, poškozený přihlašovací údaje a chybné uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-182">The code shows several types of error: An Authorization header with no credentials, malformed credentials, and bad username/password.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a><span data-ttu-id="3b4ef-183">Nastavení chybného výsledku</span><span class="sxs-lookup"><span data-stu-id="3b4ef-183">Setting an Error Result</span></span>

<span data-ttu-id="3b4ef-184">Pokud přihlašovací údaje jsou neplatné, musíte nastavit filtr `context.ErrorResult` k **IHttpActionResult** vytvářející chybnou odpověď.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-184">If the credentials are invalid, the filter must set `context.ErrorResult` to an **IHttpActionResult** that creates an error response.</span></span> <span data-ttu-id="3b4ef-185">Další informace o **IHttpActionResult**, najdete v části [výsledky akce ve webovém rozhraní API 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="3b4ef-185">For more information about **IHttpActionResult**, see [Action Results in Web API 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

<span data-ttu-id="3b4ef-186">Ukázka základní ověřování zahrnuje `AuthenticationFailureResult` třídu, která je vhodná pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-186">The Basic Authentication sample includes an `AuthenticationFailureResult` class that is suitable for this purpose.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a><span data-ttu-id="3b4ef-187">Implementace ChallengeAsync</span><span class="sxs-lookup"><span data-stu-id="3b4ef-187">Implementing ChallengeAsync</span></span>

<span data-ttu-id="3b4ef-188">Účelem **ChallengeAsync** metodou je přidání výzev ověřování pro odpověď, a to v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-188">The purpose of the **ChallengeAsync** method is to add authentication challenges to the response, if needed.</span></span> <span data-ttu-id="3b4ef-189">Tady je podpis metody:</span><span class="sxs-lookup"><span data-stu-id="3b4ef-189">Here is the method signature:</span></span>

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

<span data-ttu-id="3b4ef-190">Na každé ověřování filtru v kanálu požadavku je volání metody.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-190">The method is called on every authentication filter in the request pipeline.</span></span>

<span data-ttu-id="3b4ef-191">Je důležité pochopit, že **ChallengeAsync** nazývá *před* odpověď HTTP je vytvořený a případně i před spuštěním akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-191">It's important to understand that **ChallengeAsync** is called *before* the HTTP response is created, and possibly even before the controller action runs.</span></span> <span data-ttu-id="3b4ef-192">Když **ChallengeAsync** je volána, `context.Result` obsahuje **IHttpActionResult**, které se později používá k vytvoření odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-192">When **ChallengeAsync** is called, `context.Result` contains an **IHttpActionResult**, which is used later to create the HTTP response.</span></span> <span data-ttu-id="3b4ef-193">Takže když **ChallengeAsync** je volána, bez znalosti o odpověď HTTP ještě.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-193">So when **ChallengeAsync** is called, you don't know anything about the HTTP response yet.</span></span> <span data-ttu-id="3b4ef-194">**ChallengeAsync** metoda by měla nahradit původní hodnotu `context.Result` s novou **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-194">The **ChallengeAsync** method should replace the original value of `context.Result` with a new **IHttpActionResult**.</span></span> <span data-ttu-id="3b4ef-195">To **IHttpActionResult** musíte zabalit původní `context.Result`.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-195">This **IHttpActionResult** must wrap the original `context.Result`.</span></span>

![](authentication-filters/_static/image3.png)

<span data-ttu-id="3b4ef-196">Můžu budete volat původní **IHttpActionResult** *vnitřní výsledek*a nové **IHttpActionResult** *vnější výsledek*.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-196">I'll call the original **IHttpActionResult** the *inner result*, and the new **IHttpActionResult** the *outer result*.</span></span> <span data-ttu-id="3b4ef-197">Vnější výsledek postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="3b4ef-197">The outer result must do the following:</span></span>

1. <span data-ttu-id="3b4ef-198">Vyvolání vnitřní výsledek, který má vytvořit odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-198">Invoke the inner result to create the HTTP response.</span></span>
2. <span data-ttu-id="3b4ef-199">Zkontrolujte odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-199">Examine the response.</span></span>
3. <span data-ttu-id="3b4ef-200">Přidáte výzvu ověřování odpovědi, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-200">Add an authentication challenge to the response, if needed.</span></span>

<span data-ttu-id="3b4ef-201">Následující příklad je převzat ze ukázka základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-201">The following example is taken from the Basic Authentication sample.</span></span> <span data-ttu-id="3b4ef-202">Definuje, **IHttpActionResult** pro vnější výsledek.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-202">It defines an **IHttpActionResult** for the outer result.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

<span data-ttu-id="3b4ef-203">`InnerResult` Vlastnost obsahuje vnitřní **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-203">The `InnerResult` property holds the inner **IHttpActionResult**.</span></span> <span data-ttu-id="3b4ef-204">`Challenge` Vlastnost představuje hlavičku Www-ověřování.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-204">The `Challenge` property represents a Www-Authentication header.</span></span> <span data-ttu-id="3b4ef-205">Všimněte si, že **ExecuteAsync** nejprve volá `InnerResult.ExecuteAsync` vytvořit odpověď HTTP a potom přidá výzvy v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-205">Notice that **ExecuteAsync** first calls `InnerResult.ExecuteAsync` to create the HTTP response, and then adds the challenge if needed.</span></span>

<span data-ttu-id="3b4ef-206">Před přidáním výzvy ověřil kód odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-206">Check the response code before adding the challenge.</span></span> <span data-ttu-id="3b4ef-207">Většina schémat ověřování přidat výzvu pouze pokud je odpověď na 401, jak je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-207">Most authentication schemes only add a challenge if the response is 401, as shown here.</span></span> <span data-ttu-id="3b4ef-208">Ale některé schémat ověřování přidat výzvu úspěšná odpověď.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-208">However, some authentication schemes do add a challenge to a success response.</span></span> <span data-ttu-id="3b4ef-209">Například v tématu [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).</span><span class="sxs-lookup"><span data-stu-id="3b4ef-209">For example, see [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).</span></span>

<span data-ttu-id="3b4ef-210">Zadané `AddChallengeOnUnauthorizedResult` třídy, skutečný kód v **ChallengeAsync** je jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-210">Given the `AddChallengeOnUnauthorizedResult` class, the actual code in **ChallengeAsync** is simple.</span></span> <span data-ttu-id="3b4ef-211">Právě vytvoření výsledku a připojte ji k `context.Result`.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-211">You just create the result and attach it to `context.Result`.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

<span data-ttu-id="3b4ef-212">Poznámka: Ukázkové základní ověřování abstrahuje této logiky trochu, umístění v metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-212">Note: The Basic Authentication sample abstracts this logic a bit, by placing it in an extension method.</span></span>

## <a name="combining-authentication-filters-with-host-level-authentication"></a><span data-ttu-id="3b4ef-213">Kombinování filtry ověřování s ověřováním na úrovni hostitele</span><span class="sxs-lookup"><span data-stu-id="3b4ef-213">Combining Authentication Filters with Host-Level Authentication</span></span>

<span data-ttu-id="3b4ef-214">"Ověřování na úrovni hostitele" ověřování prováděné tímto hostitelem (například IIS), je před framework dosáhnou webového rozhraní API žádosti.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-214">"Host-level authentication" is authentication performed by the host (such as IIS), before the request reaches the Web API framework.</span></span>

<span data-ttu-id="3b4ef-215">Často můžete chtít povolit ověřování na úrovni hostitele pro zbytek vaší aplikace, ale zakázat pro řadičů webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-215">Often, you may want to enable host-level authentication for the rest of your application, but disable it for your Web API controllers.</span></span> <span data-ttu-id="3b4ef-216">Typický scénář je třeba povolit ověřování pomocí formulářů na úrovni hostitele, ale používat ověřování na základě tokenu pro rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-216">For example, a typical scenario is to enable Forms Authentication at the host level, but use token-based authentication for Web API.</span></span>

<span data-ttu-id="3b4ef-217">Chcete-li zakázat ověřování na úrovni hostitele uvnitř kanál rozhraní Web API, volejte `config.SuppressHostPrincipal()` ve vaší konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-217">To disable host-level authentication inside the Web API pipeline, call `config.SuppressHostPrincipal()` in your configuration.</span></span> <span data-ttu-id="3b4ef-218">To způsobí, že webového rozhraní API k odebrání **IPrincipal** z jakékoli požadavky, které zadá kanál rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-218">This causes Web API to remove the **IPrincipal** from any request that enters the Web API pipeline.</span></span> <span data-ttu-id="3b4ef-219">Prakticky ho &quot;zrušení-ověřuje&quot; žádosti.</span><span class="sxs-lookup"><span data-stu-id="3b4ef-219">Effectively, it &quot;un-authenticates&quot; the request.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="3b4ef-220">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="3b4ef-220">Additional Resources</span></span>

<span data-ttu-id="3b4ef-221">[Filtry zabezpečení rozhraní ASP.NET Web API](https://msdn.microsoft.com/magazine/dn781361.aspx) (Časopis MSDN)</span><span class="sxs-lookup"><span data-stu-id="3b4ef-221">[ASP.NET Web API Security Filters](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)</span></span>
