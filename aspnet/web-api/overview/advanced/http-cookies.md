---
uid: web-api/overview/advanced/http-cookies
title: Soubory cookie protokolu HTTP v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 363ca975cf75b635b766a53eeda87cf957eed60c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="a51f1-102">Soubory cookie protokolu HTTP v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="a51f1-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="a51f1-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a51f1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a51f1-104">Toto téma popisuje, jak odesílat a přijímat soubory cookie protokolu HTTP v rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="a51f1-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="a51f1-105">Pozadí na souborech cookie HTTP</span><span class="sxs-lookup"><span data-stu-id="a51f1-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="a51f1-106">Tato část poskytuje stručný přehled o tom, jak jsou implementované soubory cookie na úrovni protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a51f1-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="a51f1-107">Podrobné informace, [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="a51f1-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="a51f1-108">Cookie je část dat, který server odešle v odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="a51f1-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="a51f1-109">Klient (volitelně) uloží soubor cookie a vrátí ji u subsequet požadavků.</span><span class="sxs-lookup"><span data-stu-id="a51f1-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="a51f1-110">To umožňuje klientovi i serveru sdílet stavu.</span><span class="sxs-lookup"><span data-stu-id="a51f1-110">This allows the client and server to share state.</span></span> <span data-ttu-id="a51f1-111">Pokud chcete nastavit soubor cookie, server obsahuje hlavičku Set-Cookie v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a51f1-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="a51f1-112">Formát souboru cookie je pár název hodnota s volitelných atributů.</span><span class="sxs-lookup"><span data-stu-id="a51f1-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="a51f1-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a51f1-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="a51f1-114">Tady je příklad s atributy:</span><span class="sxs-lookup"><span data-stu-id="a51f1-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="a51f1-115">Pokud chcete vrátit do souboru cookie na serveru, klienta zahrnuje hlavička Cookie v novější požadavky.</span><span class="sxs-lookup"><span data-stu-id="a51f1-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="a51f1-116">Odpověď HTTP může obsahovat více hlavičky Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="a51f1-117">Klient vrátí více souborů cookie pomocí jednoho hlavička Cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="a51f1-118">Rozsah a platnost souboru cookie jsou řízeny následující atributy v hlavičce Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="a51f1-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="a51f1-119">**Domény**: řekne klientovi, které domény by měly dostávat souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="a51f1-120">Pokud doména je "example.com", klient odešle soubor cookie pro každou poddoménu example.com. Pokud není zadaný, doména je zdrojový server.</span><span class="sxs-lookup"><span data-stu-id="a51f1-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com. If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="a51f1-121">**Cesta**: omezuje soubor cookie na zadané cestě v rámci domény.</span><span class="sxs-lookup"><span data-stu-id="a51f1-121">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="a51f1-122">Pokud není zadaný, použije se cestu identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="a51f1-122">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="a51f1-123">**Vypršení platnosti**: nastaví datum vypršení platnosti souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-123">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="a51f1-124">Klient odstraní soubor cookie, když vyprší její platnost.</span><span class="sxs-lookup"><span data-stu-id="a51f1-124">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="a51f1-125">**Max-Age**: Nastaví maximální stáří souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-125">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="a51f1-126">Klient odstraní soubor cookie při dosažení maximální stáří.</span><span class="sxs-lookup"><span data-stu-id="a51f1-126">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="a51f1-127">Pokud oba `Expires` a `Max-Age` jsou nastavené, `Max-Age` přednost.</span><span class="sxs-lookup"><span data-stu-id="a51f1-127">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="a51f1-128">Pokud je nastavena ani jeden z nich, odstraní klienta při ukončení relace. aktuální soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-128">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="a51f1-129">(Přesný význam "relace" je určen podle uživatelského agenta).</span><span class="sxs-lookup"><span data-stu-id="a51f1-129">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="a51f1-130">Ale pozor, aby klienti ignorovat soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-130">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="a51f1-131">Uživatel může například zakázat soubory cookie z důvodů ochrany osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="a51f1-131">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="a51f1-132">Klienti lze soubory cookie odstranit, než vyprší jejich platnost, nebo omezit počet uložené soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-132">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="a51f1-133">Z důvodu ochrany osobních údajů klientů často odmítnout soubory cookie "třetích stran", kde domény neodpovídá na zdrojový server.</span><span class="sxs-lookup"><span data-stu-id="a51f1-133">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="a51f1-134">Stručně řečeno server neměli spoléhat na získávání soubory cookie, které se nastaví zpět.</span><span class="sxs-lookup"><span data-stu-id="a51f1-134">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="a51f1-135">Soubory cookie v rozhraní Web API</span><span class="sxs-lookup"><span data-stu-id="a51f1-135">Cookies in Web API</span></span>

<span data-ttu-id="a51f1-136">Chcete-li přidat do souboru cookie do odpovědi HTTP, vytvořte **CookieHeaderValue** instanci, která představuje soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-136">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="a51f1-137">Potom zavolejte **AddCookies** metoda rozšíření, která je definována v **System.Net.Http. HttpResponseHeadersExtensions** třídy, přidejte soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-137">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="a51f1-138">Následující kód například přidá soubor cookie v rámci akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="a51f1-138">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="a51f1-139">Všimněte si, že **AddCookies** přijímá pole **CookieHeaderValue** instance.</span><span class="sxs-lookup"><span data-stu-id="a51f1-139">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="a51f1-140">Pokud chcete extrahovat soubory cookie z požadavku klienta, volání **GetCookies** metoda:</span><span class="sxs-lookup"><span data-stu-id="a51f1-140">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="a51f1-141">A **CookieHeaderValue** obsahuje kolekci **CookieState** instance.</span><span class="sxs-lookup"><span data-stu-id="a51f1-141">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="a51f1-142">Každý **CookieState** představuje jeden soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-142">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="a51f1-143">Pomocí této metody indexer získat **CookieState** podle názvu, jak je vidět.</span><span class="sxs-lookup"><span data-stu-id="a51f1-143">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="a51f1-144">Soubor Cookie strukturovaných dat</span><span class="sxs-lookup"><span data-stu-id="a51f1-144">Structured Cookie Data</span></span>

<span data-ttu-id="a51f1-145">Mnoho prohlížečů omezení počtu souborů cookie, které se budou ukládat&#8212;celkový počet i číslo v každé doméně.</span><span class="sxs-lookup"><span data-stu-id="a51f1-145">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="a51f1-146">Proto může být užitečné uvést strukturovaných dat do jednoho souboru cookie, namísto nastavování více souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-146">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="a51f1-147">RFC 6265 nedefinuje strukturu dat souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-147">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="a51f1-148">Pomocí **CookieHeaderValue** třídu, můžete předat seznam dvojic název hodnota pro data souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-148">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="a51f1-149">Tyto páry název hodnota jsou zakódovány jako data kódovaná adresou URL formuláře v hlavičce Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="a51f1-149">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="a51f1-150">Předchozí kód vytvoří následující hlavičku Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="a51f1-150">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="a51f1-151">**CookieState** třída poskytuje metody indexer číst ze souboru cookie ve zprávě požadavku dílčí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="a51f1-151">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="a51f1-152">Příklad: Nastavení a načtení souborů cookie v obslužné rutiny zpráv</span><span class="sxs-lookup"><span data-stu-id="a51f1-152">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="a51f1-153">V předchozích příkladech vám ukázal, jak používat soubory cookie v rámci kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a51f1-153">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="a51f1-154">Další možností je použít [obslužné rutiny zpráv](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="a51f1-154">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="a51f1-155">Obslužné rutiny zpráv jsou vyvolány dříve v kanálu než řadičů.</span><span class="sxs-lookup"><span data-stu-id="a51f1-155">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="a51f1-156">Obslužné rutiny zpráv můžou číst soubory cookie z požadavku, než požadavek dosáhne řadičem nebo po řadičem generuje odpovědi přidat do odpovědi soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-156">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="a51f1-157">Následující kód ukazuje obslužné rutiny zpráv pro vytvoření ID relace.</span><span class="sxs-lookup"><span data-stu-id="a51f1-157">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="a51f1-158">ID relace je uložený v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="a51f1-158">The session ID is stored in a cookie.</span></span> <span data-ttu-id="a51f1-159">Obslužná rutina ověří žádost o souboru cookie relace.</span><span class="sxs-lookup"><span data-stu-id="a51f1-159">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="a51f1-160">Pokud žádost neobsahuje soubor cookie, obslužná rutina generuje ID nové relace.</span><span class="sxs-lookup"><span data-stu-id="a51f1-160">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="a51f1-161">V obou případech obslužná rutina ukládá ID relace v **HttpRequestMessage.Properties** kontejneru objektů a dat.</span><span class="sxs-lookup"><span data-stu-id="a51f1-161">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="a51f1-162">Soubor cookie relace také přidá do odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="a51f1-162">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="a51f1-163">Tato implementace neověřuje, zda Identifikátor relace z klienta bylo vydáno ve skutečnosti serveru.</span><span class="sxs-lookup"><span data-stu-id="a51f1-163">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="a51f1-164">Nepoužívejte ho jako formu ověřování!</span><span class="sxs-lookup"><span data-stu-id="a51f1-164">Don't use it as a form of authentication!</span></span> <span data-ttu-id="a51f1-165">Bod v příkladu se má zobrazit správy souboru cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="a51f1-165">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="a51f1-166">Řadič můžete získat z ID relace **HttpRequestMessage.Properties** kontejneru objektů a dat.</span><span class="sxs-lookup"><span data-stu-id="a51f1-166">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
