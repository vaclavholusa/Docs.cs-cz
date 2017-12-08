---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: "Ověřování a autorizace v rozhraní ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: "Poskytuje obecný přehled ověřování a autorizace v rozhraní ASP.NET Web API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 137ac45166be03ae3c4864f41666d2acd1a37dc2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a><span data-ttu-id="05ea2-103">Ověřování a autorizace v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="05ea2-103">Authentication and Authorization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="05ea2-104">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="05ea2-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="05ea2-105">Vytvoření webové rozhraní API, ale teď chcete řídit přístup k němu.</span><span class="sxs-lookup"><span data-stu-id="05ea2-105">You've created a web API, but now you want to control access to it.</span></span> <span data-ttu-id="05ea2-106">Z této série články podíváme některé možnosti pro zabezpečení webového rozhraní API před neoprávněnými uživateli.</span><span class="sxs-lookup"><span data-stu-id="05ea2-106">In this series of articles, we'll look at some options for securing a web API from unauthorized users.</span></span> <span data-ttu-id="05ea2-107">Tato řada se bude zabývat ověřování a autorizace.</span><span class="sxs-lookup"><span data-stu-id="05ea2-107">This series will cover both authentication and authorization.</span></span>

- <span data-ttu-id="05ea2-108">*Ověřování* je znalost identitu uživatele.</span><span class="sxs-lookup"><span data-stu-id="05ea2-108">*Authentication* is knowing the identity of the user.</span></span> <span data-ttu-id="05ea2-109">Například Alice přihlásí pomocí svého uživatelského jména a hesla a server používá heslo k ověření Alice.</span><span class="sxs-lookup"><span data-stu-id="05ea2-109">For example, Alice logs in with her username and password, and the server uses the password to authenticate Alice.</span></span>
- <span data-ttu-id="05ea2-110">*Autorizace* je rozhodování, zda má uživatel k provedení akce.</span><span class="sxs-lookup"><span data-stu-id="05ea2-110">*Authorization* is deciding whether a user is allowed to perform an action.</span></span> <span data-ttu-id="05ea2-111">Například Alice má oprávnění k získání prostředek, ale není vytvoření prostředku.</span><span class="sxs-lookup"><span data-stu-id="05ea2-111">For example, Alice has permission to get a resource but not create a resource.</span></span>

<span data-ttu-id="05ea2-112">První článek v řadě poskytuje obecný přehled ověřování a autorizace v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="05ea2-112">The first article in the series gives a general overview of authentication and authorization in ASP.NET Web API.</span></span> <span data-ttu-id="05ea2-113">Další témata popisují běžné scénáře ověřování pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="05ea2-113">Other topics describe common authentication scenarios for Web API.</span></span>

> [!NOTE]
> <span data-ttu-id="05ea2-114">Díky osob, které tato řada zkontrolovány a poskytuje cenné zpětné vazby: Rick Anderson, Levi Broderick, Jiří Dorrans, tní Dykstra, Hongmei Ge, David Matson, ADAM Roth, Tim Teebken.</span><span class="sxs-lookup"><span data-stu-id="05ea2-114">Thanks to the people who reviewed this series and provided valuable feedback: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.</span></span>


## <a name="authentication"></a><span data-ttu-id="05ea2-115">Ověřování</span><span class="sxs-lookup"><span data-stu-id="05ea2-115">Authentication</span></span>

<span data-ttu-id="05ea2-116">Webové rozhraní API předpokládá, že se stane, že ověřování na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="05ea2-116">Web API assumes that authentication happens in the host.</span></span> <span data-ttu-id="05ea2-117">Pro hostování webů, je hostitele služby IIS, která používá vytváření modulů HTTP pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="05ea2-117">For web-hosting, the host is IIS, which uses HTTP modules for authentication.</span></span> <span data-ttu-id="05ea2-118">Můžete nakonfigurovat projektu pro použití žádné z modulů ověřování, které jsou součástí služby IIS nebo v technologii ASP.NET, nebo napsat vlastní modul protokolu HTTP k provedení vlastní ověřování.</span><span class="sxs-lookup"><span data-stu-id="05ea2-118">You can configure your project to use any of the authentication modules built in to IIS or ASP.NET, or write your own HTTP module to perform custom authentication.</span></span>

<span data-ttu-id="05ea2-119">Když hostitel ověřuje uživatele, vytvoří *hlavní*, který je [IPrincipal](https://msdn.microsoft.com/en-us/library/System.Security.Principal.IPrincipal.aspx) objekt, který reprezentuje kontext zabezpečení, v němž je spuštěn kód.</span><span class="sxs-lookup"><span data-stu-id="05ea2-119">When the host authenticates the user, it creates a *principal*, which is an [IPrincipal](https://msdn.microsoft.com/en-us/library/System.Security.Principal.IPrincipal.aspx) object that represents the security context under which code is running.</span></span> <span data-ttu-id="05ea2-120">Hostitel připojí k objektu zabezpečení pro aktuální vlákno nastavením **Thread.CurrentPrincipal**.</span><span class="sxs-lookup"><span data-stu-id="05ea2-120">The host attaches the principal to the current thread by setting **Thread.CurrentPrincipal**.</span></span> <span data-ttu-id="05ea2-121">Objekt zabezpečení obsahuje přiřazený **Identity** objekt, který obsahuje informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="05ea2-121">The principal contains an associated **Identity** object that contains information about the user.</span></span> <span data-ttu-id="05ea2-122">Pokud je uživatel ověřený, **Identity.IsAuthenticated** vlastnost vrátí **true**.</span><span class="sxs-lookup"><span data-stu-id="05ea2-122">If the user is authenticated, the **Identity.IsAuthenticated** property returns **true**.</span></span> <span data-ttu-id="05ea2-123">U anonymních požadavků **IsAuthenticated** vrátí **false**.</span><span class="sxs-lookup"><span data-stu-id="05ea2-123">For anonymous requests, **IsAuthenticated** returns **false**.</span></span> <span data-ttu-id="05ea2-124">Další informace o objekty zabezpečení najdete v tématu [na základě rolí zabezpečení](https://msdn.microsoft.com/en-us/library/shz8h065.aspx).</span><span class="sxs-lookup"><span data-stu-id="05ea2-124">For more information about principals, see [Role-Based Security](https://msdn.microsoft.com/en-us/library/shz8h065.aspx).</span></span>

### <a name="http-message-handlers-for-authentication"></a><span data-ttu-id="05ea2-125">Obslužné rutiny zpráv HTTP pro ověřování</span><span class="sxs-lookup"><span data-stu-id="05ea2-125">HTTP Message Handlers for Authentication</span></span>

<span data-ttu-id="05ea2-126">Místo použití hostitele pro ověřování, které můžete vložit logika ověřování do [obslužné rutiny zpráv HTTP](../advanced/http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="05ea2-126">Instead of using the host for authentication, you can put authentication logic into an [HTTP message handler](../advanced/http-message-handlers.md).</span></span> <span data-ttu-id="05ea2-127">V takovém případě popisovač zpráv prozkoumá požadavek HTTP a nastaví objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="05ea2-127">In that case, the message handler examines the HTTP request and sets the principal.</span></span>

<span data-ttu-id="05ea2-128">Kdy mám použít obslužné rutiny zpráv pro ověřování?</span><span class="sxs-lookup"><span data-stu-id="05ea2-128">When should you use message handlers for authentication?</span></span> <span data-ttu-id="05ea2-129">Zde jsou některé kompromisy:</span><span class="sxs-lookup"><span data-stu-id="05ea2-129">Here are some tradeoffs:</span></span>

- <span data-ttu-id="05ea2-130">Modul HTTP se zobrazí všechny požadavky, které procházejí kanálu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="05ea2-130">An HTTP module sees all requests that go through the ASP.NET pipeline.</span></span> <span data-ttu-id="05ea2-131">Obslužné rutiny zpráv se zobrazují pouze požadavků, které jsou směrovány do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="05ea2-131">A message handler only sees requests that are routed to Web API.</span></span>
- <span data-ttu-id="05ea2-132">Můžete nastavit obslužné rutiny zpráv za trasy, který vám umožňuje použít příslušné schéma ověřování pro konkrétní trasy.</span><span class="sxs-lookup"><span data-stu-id="05ea2-132">You can set per-route message handlers, which lets you apply an authentication scheme to a specific route.</span></span>
- <span data-ttu-id="05ea2-133">Vytváření modulů HTTP jsou specifické pro službu IIS.</span><span class="sxs-lookup"><span data-stu-id="05ea2-133">HTTP modules are specific to IIS.</span></span> <span data-ttu-id="05ea2-134">Obslužné rutiny zpráv jsou vázané na hostitele, takže je můžete použít pro hostování webů a samoobslužné hostování.</span><span class="sxs-lookup"><span data-stu-id="05ea2-134">Message handlers are host-agnostic, so they can be used with both web-hosting and self-hosting.</span></span>
- <span data-ttu-id="05ea2-135">Vytváření modulů HTTP v účasti v protokolování služby IIS, auditování a tak dále.</span><span class="sxs-lookup"><span data-stu-id="05ea2-135">HTTP modules participate in IIS logging, auditing, and so on.</span></span>
- <span data-ttu-id="05ea2-136">Vytváření modulů HTTP v spustit dříve v kanálu.</span><span class="sxs-lookup"><span data-stu-id="05ea2-136">HTTP modules run earlier in the pipeline.</span></span> <span data-ttu-id="05ea2-137">Pokud zpracováváte ověřování v obslužné rutiny zpráv, získat objekt není nastaven, dokud obslužná rutina se spustí.</span><span class="sxs-lookup"><span data-stu-id="05ea2-137">If you handle authentication in a message handler, the principal does not get set until the handler runs.</span></span> <span data-ttu-id="05ea2-138">Kromě toho se objekt vrátí zpět na předchozí objekt zabezpečení při odesílání odpovědi popisovač zpráv.</span><span class="sxs-lookup"><span data-stu-id="05ea2-138">Moreover, the principal reverts back to the previous principal when the response leaves the message handler.</span></span>

<span data-ttu-id="05ea2-139">Obecně platí Pokud nemusíte podporovat samoobslužné hostování, modul HTTP je lepší volbou.</span><span class="sxs-lookup"><span data-stu-id="05ea2-139">Generally, if you don't need to support self-hosting, an HTTP module is a better option.</span></span> <span data-ttu-id="05ea2-140">Pokud potřebujete podporovat samoobslužné hostování, zvažte obslužné rutiny zpráv.</span><span class="sxs-lookup"><span data-stu-id="05ea2-140">If you need to support self-hosting, consider a message handler.</span></span>

### <a name="setting-the-principal"></a><span data-ttu-id="05ea2-141">Nastavení objektu zabezpečení</span><span class="sxs-lookup"><span data-stu-id="05ea2-141">Setting the Principal</span></span>

<span data-ttu-id="05ea2-142">Pokud aplikace provede všechny vlastní ověřovací logiku, je nutné nastavit objekt zabezpečení na dvou místech:</span><span class="sxs-lookup"><span data-stu-id="05ea2-142">If your application performs any custom authentication logic, you must set the principal on two places:</span></span>

- <span data-ttu-id="05ea2-143">**Thread.CurrentPrincipal**.</span><span class="sxs-lookup"><span data-stu-id="05ea2-143">**Thread.CurrentPrincipal**.</span></span> <span data-ttu-id="05ea2-144">Tato vlastnost je standardní způsob, jak nastavit objekt zabezpečení vlákna v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="05ea2-144">This property is the standard way to set the thread's principal in .NET.</span></span>
- <span data-ttu-id="05ea2-145">**HttpContext.Current.User**.</span><span class="sxs-lookup"><span data-stu-id="05ea2-145">**HttpContext.Current.User**.</span></span> <span data-ttu-id="05ea2-146">Tato vlastnost je specifická pro technologii ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="05ea2-146">This property is specific to ASP.NET.</span></span>

<span data-ttu-id="05ea2-147">Následující kód ukazuje, jak nastavit objekt zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="05ea2-147">The following code shows how to set the principal:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="05ea2-148">Pro hostování webů, je potřeba nastavit objekt zabezpečení v obou místech; v opačném případě se může stát nekonzistentní kontext zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="05ea2-148">For web-hosting, you must set the principal in both places; otherwise the security context may become inconsistent.</span></span> <span data-ttu-id="05ea2-149">Pro vlastní hostování, ale **HttpContext.Current** má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="05ea2-149">For self-hosting, however, **HttpContext.Current** is null.</span></span> <span data-ttu-id="05ea2-150">Aby váš kód je bez ohledu na hostitele, proto zkontrolujte null před přiřazením **HttpContext.Current**, jak je vidět.</span><span class="sxs-lookup"><span data-stu-id="05ea2-150">To ensure your code is host-agnostic, therefore, check for null before assigning to **HttpContext.Current**, as shown.</span></span>

## <a name="authorization"></a><span data-ttu-id="05ea2-151">Autorizace</span><span class="sxs-lookup"><span data-stu-id="05ea2-151">Authorization</span></span>

<span data-ttu-id="05ea2-152">Autorizace se stane později v kanálu, co nejblíže ke kontroleru.</span><span class="sxs-lookup"><span data-stu-id="05ea2-152">Authorization happens later in the pipeline, closer to the controller.</span></span> <span data-ttu-id="05ea2-153">Který vám umožňuje provést podrobnější volby při uděluje přístup k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="05ea2-153">That lets you make more granular choices when you grant access to resources.</span></span>

- <span data-ttu-id="05ea2-154">*Filtry autorizace* spustit před akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="05ea2-154">*Authorization filters* run before the controller action.</span></span> <span data-ttu-id="05ea2-155">Pokud požadavek není ověřen, filtr vrátí odpověď chyba a akce není vyvolána.</span><span class="sxs-lookup"><span data-stu-id="05ea2-155">If the request is not authorized, the filter returns an error response, and the action is not invoked.</span></span>
- <span data-ttu-id="05ea2-156">V rámci akce kontroleru, můžete získat z aktuální objekt zabezpečení **ApiController.User** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="05ea2-156">Within a controller action, you can get the current principal from the **ApiController.User** property.</span></span> <span data-ttu-id="05ea2-157">Může například filtrovat seznam prostředků podle uživatelské jméno, vrácení jenom prostředky, které patří do tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="05ea2-157">For example, you might filter a list of resources based on the user name, returning only those resources that belong to that user.</span></span>

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a><span data-ttu-id="05ea2-158">Pomocí [Povolit] atributu</span><span class="sxs-lookup"><span data-stu-id="05ea2-158">Using the [Authorize] Attribute</span></span>

<span data-ttu-id="05ea2-159">Webové rozhraní API poskytuje integrované autorizační filtr, [třídy AuthorizeAttribute](https://msdn.microsoft.com/en-us/library/system.web.http.authorizeattribute.aspx).</span><span class="sxs-lookup"><span data-stu-id="05ea2-159">Web API provides a built-in authorization filter, [AuthorizeAttribute](https://msdn.microsoft.com/en-us/library/system.web.http.authorizeattribute.aspx).</span></span> <span data-ttu-id="05ea2-160">Tento filtr kontroluje, zda je uživatel ověřený.</span><span class="sxs-lookup"><span data-stu-id="05ea2-160">This filter checks whether the user is authenticated.</span></span> <span data-ttu-id="05ea2-161">V opačném případě vrátí stavový kód HTTP 401 (Neautorizováno), bez vyvolání akce.</span><span class="sxs-lookup"><span data-stu-id="05ea2-161">If not, it returns HTTP status code 401 (Unauthorized), without invoking the action.</span></span>

<span data-ttu-id="05ea2-162">Můžete použít filtr globálně, na úrovni kontroleru, nebo na úrovni inidivual akcí.</span><span class="sxs-lookup"><span data-stu-id="05ea2-162">You can apply the filter globally, at the controller level, or at the level of inidivual actions.</span></span>

<span data-ttu-id="05ea2-163">**Globálně**: Pokud chcete omezit přístup pro každý kontroler Web API, přidejte **třídy AuthorizeAttribute** filtru do seznamu globálních filtrů:</span><span class="sxs-lookup"><span data-stu-id="05ea2-163">**Globally**: To restrict access for every Web API controller, add the **AuthorizeAttribute** filter to the global filter list:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="05ea2-164">**Řadič**: Pokud chcete omezit přístup k určitému kontroleru, přidejte filtr jako atribut řadiče:</span><span class="sxs-lookup"><span data-stu-id="05ea2-164">**Controller**: To restrict access for a specific controller, add the filter as an attribute to the controller:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="05ea2-165">**Akce**: Pokud chcete omezit přístup pro určité akce, přidejte atribut do metody akce:</span><span class="sxs-lookup"><span data-stu-id="05ea2-165">**Action**: To restrict access for specific actions, add the attribute to the action method:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="05ea2-166">Alternativně můžete omezit kontroler a pak umožnit anonymní přístup k určitým akcím, pomocí `[AllowAnonymous]` atribut.</span><span class="sxs-lookup"><span data-stu-id="05ea2-166">Alternatively, you can restrict the controller and then allow anonymous access to specific actions, by using the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="05ea2-167">V následujícím příkladu `Post` metoda je omezená, ale `Get` metoda umožňuje anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="05ea2-167">In the following example, the `Post` method is restricted, but the `Get` method allows anonymous access.</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="05ea2-168">V předchozích příkladech filtr povoluje všechny ověřené uživatele pro přístup s omezeným přístupem metody; jsou uchovány pouze anonymní uživatelé. Chcete-li omezit přístup na určité uživatele nebo pro uživatele v určitých rolích:</span><span class="sxs-lookup"><span data-stu-id="05ea2-168">In the previous examples, the filter allows any authenticated user to access the restricted methods; only anonymous users are kept out. You can also limit access to specific users or to users in specific roles:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="05ea2-169">**Třídy AuthorizeAttribute** filtru pro řadiče webového rozhraní API se nachází v **System.Web.Http** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="05ea2-169">The **AuthorizeAttribute** filter for Web API controllers is located in the **System.Web.Http** namespace.</span></span> <span data-ttu-id="05ea2-170">Není k dispozici podobné filtr pro kontrolery MVC v **System.Web.Mvc** názvů, který není kompatibilní s řadiči webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="05ea2-170">There is a similar filter for MVC controllers in the **System.Web.Mvc** namespace, which is not compatible with Web API controllers.</span></span>


### <a name="custom-authorization-filters"></a><span data-ttu-id="05ea2-171">Filtry autorizace</span><span class="sxs-lookup"><span data-stu-id="05ea2-171">Custom Authorization Filters</span></span>

<span data-ttu-id="05ea2-172">Zápis filtru autorizace, odvodíte z jedné z těchto typů:</span><span class="sxs-lookup"><span data-stu-id="05ea2-172">To write a custom authorization filter, derive from one of these types:</span></span>

- <span data-ttu-id="05ea2-173">**Třídy AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="05ea2-173">**AuthorizeAttribute**.</span></span> <span data-ttu-id="05ea2-174">Rozšíření této třídy lze provádět logiku ověřování na základě aktuálního uživatele a role uživatele.</span><span class="sxs-lookup"><span data-stu-id="05ea2-174">Extend this class to perform authorization logic based on the current user and the user's roles.</span></span>
- <span data-ttu-id="05ea2-175">**AuthorizationFilterAttribute**.</span><span class="sxs-lookup"><span data-stu-id="05ea2-175">**AuthorizationFilterAttribute**.</span></span> <span data-ttu-id="05ea2-176">Rozšíření této třídy lze provádět logiku synchronní autorizace, který není nutně na základě aktuální uživatel nebo role.</span><span class="sxs-lookup"><span data-stu-id="05ea2-176">Extend this class to perform synchronous authorization logic that is not necessarily based on the current user or role.</span></span>
- <span data-ttu-id="05ea2-177">**IAuthorizationFilter**.</span><span class="sxs-lookup"><span data-stu-id="05ea2-177">**IAuthorizationFilter**.</span></span> <span data-ttu-id="05ea2-178">Implementace tohoto rozhraní k provedení asynchronní autorizace logiku; například, pokud je logika ověřování volá asynchronní vstupně-výstupních operací nebo sítě.</span><span class="sxs-lookup"><span data-stu-id="05ea2-178">Implement this interface to perform asynchronous authorization logic; for example, if your authorization logic makes asynchronous I/O or network calls.</span></span> <span data-ttu-id="05ea2-179">(Pokud je logika ověřování vázané na procesor, je jednodušší odvozena od **AuthorizationFilterAttribute**, protože pak nemusíte zápis asynchronní metody.)</span><span class="sxs-lookup"><span data-stu-id="05ea2-179">(If your authorization logic is CPU-bound, it is simpler to derive from **AuthorizationFilterAttribute**, because then you don't need to write an asynchronous method.)</span></span>

<span data-ttu-id="05ea2-180">Následující diagram znázorňuje hierarchie tříd pro **třídy AuthorizeAttribute** třídy.</span><span class="sxs-lookup"><span data-stu-id="05ea2-180">The following diagram shows the class hierarchy for the **AuthorizeAttribute** class.</span></span>

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a><span data-ttu-id="05ea2-181">Autorizace uvnitř akce Kontroleru</span><span class="sxs-lookup"><span data-stu-id="05ea2-181">Authorization Inside a Controller Action</span></span>

<span data-ttu-id="05ea2-182">V některých případech může povolit požadavek na pokračovat, ale změnit chování založené na objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="05ea2-182">In some cases, you might allow a request to proceed, but change the behavior based on the principal.</span></span> <span data-ttu-id="05ea2-183">Například informace, které vrátíte může změnit v závislosti na roli uživatele.</span><span class="sxs-lookup"><span data-stu-id="05ea2-183">For example, the information that you return might change depending on the user's role.</span></span> <span data-ttu-id="05ea2-184">V rámci metoda kontroleru, můžete získat z aktuální Princip **ApiController.User** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="05ea2-184">Within a controller method, you can get the current principle from the **ApiController.User** property.</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
