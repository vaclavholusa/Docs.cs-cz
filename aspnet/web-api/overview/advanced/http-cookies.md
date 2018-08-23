---
uid: web-api/overview/advanced/http-cookies
title: Soubory cookie protokolu HTTP v rozhraní ASP.NET Web API | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/17/2012
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 61e0c47efdd92a3a0b329930aeec757b446eb9b8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756413"
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="7b8e6-102">Soubory cookie protokolu HTTP v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="7b8e6-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="7b8e6-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7b8e6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7b8e6-104">Toto téma popisuje, jak odesílat a přijímat soubory cookie protokolu HTTP v rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="7b8e6-105">Na pozadí na soubory cookie protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="7b8e6-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="7b8e6-106">Tato část poskytuje stručný přehled o tom, jak jsou implementované soubory cookie na úrovni protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="7b8e6-107">Podrobné informace, [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="7b8e6-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="7b8e6-108">Soubor cookie je část dat, která odesílá na server v odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="7b8e6-109">Klient (volitelně) uloží soubor cookie a vrátí na subsequet požadavky.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="7b8e6-110">Díky tomu klienta a serveru, sdílení stavu.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-110">This allows the client and server to share state.</span></span> <span data-ttu-id="7b8e6-111">Nastavení souboru cookie, server obsahuje hlavičku Set-Cookie v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="7b8e6-112">Formát souboru cookie je pár název hodnota pomocí volitelných atributů.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="7b8e6-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7b8e6-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="7b8e6-114">Tady je příklad s atributy:</span><span class="sxs-lookup"><span data-stu-id="7b8e6-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="7b8e6-115">Pokud chcete vrátit do souboru cookie na serveru, klienta obsahuje hlavičku souboru Cookie novějším žádostem.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="7b8e6-116">Odpověď HTTP může obsahovat více záhlaví Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="7b8e6-117">Klient odešle několik souborů cookie pomocí jednoho hlavičky souboru Cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="7b8e6-118">Rozsahu a doby trvání soubor cookie se řídí následujícími atributy v hlavičce Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="7b8e6-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="7b8e6-119">**Domény**: instruuje klienta, které domény souboru cookie přijímat.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="7b8e6-120">Pokud tato doména "example.com", klient vrátí soubor cookie pro každou poddoménu example.com.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="7b8e6-121">Pokud není zadán, doména je zdrojový server.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-121">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="7b8e6-122">**Cesta**: omezuje na zadané cestě v rámci domény souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-122">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="7b8e6-123">Pokud není zadán, je použít cesty identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-123">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="7b8e6-124">**Vypršení platnosti**: nastaví datum vypršení platnosti souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-124">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="7b8e6-125">Klient odstraní soubor cookie, když jeho platnost vyprší.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-125">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="7b8e6-126">**Max-Age**: Nastaví maximální stáří souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-126">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="7b8e6-127">Klient odstraní soubor cookie, když dosáhl maximální stáří.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-127">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="7b8e6-128">Pokud mají oba `Expires` a `Max-Age` jsou nastaveny, `Max-Age` přednost.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-128">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="7b8e6-129">Pokud je nastavena ani jedno, klient odstraní soubor cookie po skončení aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-129">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="7b8e6-130">(Přesné význam "relace" se určuje podle uživatelského agenta).</span><span class="sxs-lookup"><span data-stu-id="7b8e6-130">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="7b8e6-131">Nezapomínejte, že klienti mohou ignorovat soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-131">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="7b8e6-132">Uživatel může například zakažte soubory cookie z důvodů ochrany osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-132">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="7b8e6-133">Klienti soubory cookie odstranit předtím, než vyprší jejich platnost, nebo omezit počet uložených souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-133">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="7b8e6-134">Z důvodu ochrany osobních údajů klientů často odmítnout "třetích stran" soubory cookie, kde doména se neshoduje s původním serveru.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-134">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="7b8e6-135">Stručně řečeno server neměli spoléhat na návrat, které nastaví soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-135">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="7b8e6-136">Soubory cookie ve webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7b8e6-136">Cookies in Web API</span></span>

<span data-ttu-id="7b8e6-137">Pokud chcete přidat do souboru cookie do odpovědi HTTP, vytvořte **CookieHeaderValue** instanci, která představuje soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-137">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="7b8e6-138">Zavolejte **AddCookies** rozšiřující metoda, která je definována v **System.Net.Http. HttpResponseHeadersExtensions** třídy, chcete-li přidat soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-138">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="7b8e6-139">Například následující kód přidá soubor cookie v rámci akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="7b8e6-139">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="7b8e6-140">Všimněte si, že **AddCookies** přijímá pole **CookieHeaderValue** instancí.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-140">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="7b8e6-141">Chcete-li extrahovat soubory cookie z požadavku klienta, zavolejte **GetCookies** metody:</span><span class="sxs-lookup"><span data-stu-id="7b8e6-141">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="7b8e6-142">A **CookieHeaderValue** obsahuje kolekci **CookieState** instancí.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-142">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="7b8e6-143">Každý **CookieState** představuje jeden soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-143">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="7b8e6-144">Získat pomocí metody indexeru **CookieState** podle názvu, jak je znázorněno.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-144">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="7b8e6-145">Soubor Cookie strukturovaná Data</span><span class="sxs-lookup"><span data-stu-id="7b8e6-145">Structured Cookie Data</span></span>

<span data-ttu-id="7b8e6-146">Většina prohlížečů omezení počtu souborů cookie, které se budou ukládat&#8212;celkový počet i číslo na doménu.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-146">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="7b8e6-147">Proto může být užitečné zavést strukturovaná data do jednoho souboru cookie, namísto nastavení více souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-147">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="7b8e6-148">RFC 6265 nedefinuje strukturu dat souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-148">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="7b8e6-149">Použití **CookieHeaderValue** třídy, můžete předat seznam dvojic název hodnota pro data souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-149">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="7b8e6-150">Tyto páry název hodnota jsou zakódovány jako data kódovaná adresou URL formuláře v hlavičce Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="7b8e6-150">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="7b8e6-151">Předchozí kód vytvoří následující hlavičkou Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="7b8e6-151">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="7b8e6-152">**CookieState** třída poskytuje metodu indexer načíst dílčí hodnoty ze souboru cookie ve zprávě požadavku:</span><span class="sxs-lookup"><span data-stu-id="7b8e6-152">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="7b8e6-153">Příklad: Nastavení a načtení souborů cookie v obslužné rutiny zpráv</span><span class="sxs-lookup"><span data-stu-id="7b8e6-153">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="7b8e6-154">Předchozí příklady nám ukázaly, jak používat soubory cookie z v rámci kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-154">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="7b8e6-155">Další možností je použít [obslužné rutiny zpráv](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="7b8e6-155">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="7b8e6-156">Obslužné rutiny zpráv jsou vyvolány dříve v kanálu než řadiče.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-156">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="7b8e6-157">Obslužné rutiny zpráv můžete číst soubory cookie z požadavku, než požadavek dosáhne kontroleru nebo po kontroleru generuje odpovědi přidat soubory cookie v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-157">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="7b8e6-158">Následující kód ukazuje obslužné rutiny zpráv pro vytvoření ID relace.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-158">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="7b8e6-159">ID relace se ukládají do souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-159">The session ID is stored in a cookie.</span></span> <span data-ttu-id="7b8e6-160">Obslužná rutina ověří požadavek pro soubor cookie relace.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-160">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="7b8e6-161">Pokud žádost neobsahuje soubor cookie, obslužná rutina generuje ID nové relace.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-161">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="7b8e6-162">V obou případech se obslužná rutina uloží ID relace v **HttpRequestMessage.Properties** kontejner objektů a dat.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-162">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="7b8e6-163">Také přidá soubor cookie relace do odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-163">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="7b8e6-164">Tato implementace nelze ověřit, že ID relace, od klienta byl ve skutečnosti vydán serverem.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-164">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="7b8e6-165">Nepoužívejte ho jako formu ověřování!</span><span class="sxs-lookup"><span data-stu-id="7b8e6-165">Don't use it as a form of authentication!</span></span> <span data-ttu-id="7b8e6-166">Bod v příkladu je zobrazení správy souboru cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-166">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="7b8e6-167">Kontroler můžete získat ID relace, od **HttpRequestMessage.Properties** kontejner objektů a dat.</span><span class="sxs-lookup"><span data-stu-id="7b8e6-167">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
