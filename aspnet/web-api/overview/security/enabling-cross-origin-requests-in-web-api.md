---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Povolení žádostí napříč zdroji v rozhraní ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Ukazuje, jak pro podporu sdílení prostředků různých původů (CORS) v rozhraní ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566818"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="48515-103">Povolení žádostí napříč zdroji v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="48515-103">Enabling Cross-Origin Requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="48515-104">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="48515-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="48515-105">Zabezpečení prohlížeče brání provedení požadavky AJAX do jiné domény na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="48515-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="48515-106">Toto omezení se nazývá *zásady stejného původu*a zabrání škodlivé weby čtení citlivá data z jiné lokality.</span><span class="sxs-lookup"><span data-stu-id="48515-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="48515-107">Ale v některých případech můžete chtít umožní jiných lokalitách volání webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="48515-107">However, sometimes you might want to let other sites call your web API.</span></span>
> 
> <span data-ttu-id="48515-108">[Mezi sdílení prostředků původu](http://www.w3.org/TR/cors/) (CORS) je standard W3C, který umožňuje serveru zmírnit zásady stejného původu.</span><span class="sxs-lookup"><span data-stu-id="48515-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="48515-109">Pomocí CORS, server explicitně povolit některé žádostí napříč zdroji při odmítnutí ostatní.</span><span class="sxs-lookup"><span data-stu-id="48515-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="48515-110">CORS, jako je bezpečnější a flexibilnější než dřívější techniky [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="48515-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="48515-111">Tento kurz ukazuje postup povolení CORS v aplikaci webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="48515-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="48515-112">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="48515-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="48515-113">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="48515-113">Visual Studio 2013 Update 2</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="48515-114">Webové rozhraní API 2.2</span><span class="sxs-lookup"><span data-stu-id="48515-114">Web API 2.2</span></span>


<a id="intro"></a>
## <a name="introduction"></a><span data-ttu-id="48515-115">Úvod</span><span class="sxs-lookup"><span data-stu-id="48515-115">Introduction</span></span>

<span data-ttu-id="48515-116">Tento kurz představuje podporu CORS v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="48515-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="48515-117">Začneme vytvořením dva projekty ASP.NET – jeden názvem "Webová služba", který hostuje kontroleru webového rozhraní API, a další názvem "webový klient", která volá webové služby.</span><span class="sxs-lookup"><span data-stu-id="48515-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="48515-118">Protože dvě možná využití jsou hostované v různých doménách, je požadavek AJAX z WebClient webovou žádost cross-origin.</span><span class="sxs-lookup"><span data-stu-id="48515-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="48515-119">Co je "Stejné původ"?</span><span class="sxs-lookup"><span data-stu-id="48515-119">What is "Same Origin"?</span></span>

<span data-ttu-id="48515-120">Dvou adres URL mají stejný původ v případě, že mají stejné schémata, hostitele a porty.</span><span class="sxs-lookup"><span data-stu-id="48515-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="48515-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="48515-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="48515-122">Tyto dvě adresy URL mají stejný původ:</span><span class="sxs-lookup"><span data-stu-id="48515-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="48515-123">Tyto adresy URL mít různého původu než předchozí dva:</span><span class="sxs-lookup"><span data-stu-id="48515-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="48515-124">`http://example.net` -Jiné domény</span><span class="sxs-lookup"><span data-stu-id="48515-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="48515-125">`http://example.com:9000/foo.html` -Jiný port</span><span class="sxs-lookup"><span data-stu-id="48515-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="48515-126">`https://example.com/foo.html` -Jiné schéma</span><span class="sxs-lookup"><span data-stu-id="48515-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="48515-127">`http://www.example.com/foo.html` -Různých subdomény</span><span class="sxs-lookup"><span data-stu-id="48515-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="48515-128">Internet Explorer nebere v úvahu port při porovnávání zdroje.</span><span class="sxs-lookup"><span data-stu-id="48515-128">Internet Explorer does not consider the port when comparing origins.</span></span>


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a><span data-ttu-id="48515-129">Vytvoření projektu webové služby</span><span class="sxs-lookup"><span data-stu-id="48515-129">Create the WebService Project</span></span>

> [!NOTE]
> <span data-ttu-id="48515-130">V této části se předpokládá, že již víte, jak vytvořit projekty webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="48515-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="48515-131">Pokud ne, najdete v části [Začínáme s rozhraním ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="48515-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>


<span data-ttu-id="48515-132">Spuštění sady Visual Studio a vytvořte novou **webové aplikace ASP.NET** projektu.</span><span class="sxs-lookup"><span data-stu-id="48515-132">Start Visual Studio and create a new **ASP.NET Web Application** project.</span></span> <span data-ttu-id="48515-133">Vyberte **prázdný** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="48515-133">Select the **Empty** project template.</span></span> <span data-ttu-id="48515-134">V části "Přidat složky a odkazy na základní" vyberte **webového rozhraní API** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="48515-134">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="48515-135">Volitelně vyberte možnost "Hostitel v cloudu" k nasazení aplikace do Azure výrobci.</span><span class="sxs-lookup"><span data-stu-id="48515-135">Optionally, select the "Host in Cloud" option to deploy the app to Mircosoft Azure.</span></span> <span data-ttu-id="48515-136">Společnost Microsoft nabízí bezplatné webových hostitelských služeb pro až 10 webů ve [Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="48515-136">Microsoft offers free web hosting for up to 10 websites in a [free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

<span data-ttu-id="48515-137">Přidání kontroleru webového rozhraní API s názvem `TestController` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="48515-137">Add a Web API controller named `TestController` with the following code:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

<span data-ttu-id="48515-138">Můžete spustit aplikace místně nebo nasazení do Azure.</span><span class="sxs-lookup"><span data-stu-id="48515-138">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="48515-139">(Snímky obrazovky v tomto kurzu, lze nasadit do Azure App Service Web Apps.) Chcete-li ověřit, zda je funkční webové rozhraní API, přejděte na `http://hostname/api/test/`, kde *hostname* je doména, kde jste nasadili aplikace.</span><span class="sxs-lookup"><span data-stu-id="48515-139">(For the screenshots in this tutorial, I deployed to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="48515-140">Měli byste vidět text odpovědi &quot;získat: testovací zpráva&quot;.</span><span class="sxs-lookup"><span data-stu-id="48515-140">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a><span data-ttu-id="48515-141">Vytvoření projektu WebClient</span><span class="sxs-lookup"><span data-stu-id="48515-141">Create the WebClient Project</span></span>

<span data-ttu-id="48515-142">Vytvořte jiný projekt webové aplikace ASP.NET a vyberte **MVC** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="48515-142">Create another ASP.NET Web Application project and select the **MVC** project template.</span></span> <span data-ttu-id="48515-143">Volitelně vyberte **změna ověřování** > **bez ověřování**.</span><span class="sxs-lookup"><span data-stu-id="48515-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="48515-144">Nepotřebujete ověřování pro tento kurz.</span><span class="sxs-lookup"><span data-stu-id="48515-144">You don't need authentication for this tutorial.</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

<span data-ttu-id="48515-145">V Průzkumníku řešení otevřete soubor Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="48515-145">In Solution Explorer, open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="48515-146">Nahraďte kód v tomto souboru s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="48515-146">Replace the code in this file with the following:</span></span>

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

<span data-ttu-id="48515-147">Pro *serviceUrl* proměnnou, použijte identifikátor URI aplikace webové služby.</span><span class="sxs-lookup"><span data-stu-id="48515-147">For the *serviceUrl* variable, use the URI of the WebService app.</span></span> <span data-ttu-id="48515-148">Nyní spusťte aplikaci WebClient místně nebo publikovat na jiný web.</span><span class="sxs-lookup"><span data-stu-id="48515-148">Now run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="48515-149">Kliknutím na tlačítko "Zkuste ho" odešle požadavek AJAX na webovou aplikaci pomocí protokolu HTTP metody uvedené v rozevíracím (GET, POST nebo PUT).</span><span class="sxs-lookup"><span data-stu-id="48515-149">Clicking the "Try It" button submits an AJAX request to the WebService app, using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="48515-150">To umožňuje nám prozkoumat různých žádostí napříč zdroji.</span><span class="sxs-lookup"><span data-stu-id="48515-150">This lets us examine different cross-origin requests.</span></span> <span data-ttu-id="48515-151">Teď, webovou aplikaci nepodporuje CORS, takže pokud kliknete na tlačítko, zobrazí se chyba.</span><span class="sxs-lookup"><span data-stu-id="48515-151">Right now, the WebService app does not support CORS, so if you click the button, you will get an error.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="48515-152">Je-li sledovat provoz protokolu HTTP v nástroji jako [Fiddler](http://www.telerik.com/fiddler), uvidíte, že do prohlížeče odeslat požadavek GET a neproběhne úspěšně, ale volání AJAX vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="48515-152">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you will see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="48515-153">Je důležité si uvědomit, že zásady stejného původu nebrání prohlížeče z *odesílání* žádosti.</span><span class="sxs-lookup"><span data-stu-id="48515-153">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="48515-154">Místo toho ji brání aplikaci v zobrazuje *odpovědi*.</span><span class="sxs-lookup"><span data-stu-id="48515-154">Instead, it prevents the application from seeing the *response*.</span></span>


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a><span data-ttu-id="48515-155">Povolení CORS</span><span class="sxs-lookup"><span data-stu-id="48515-155">Enable CORS</span></span>

<span data-ttu-id="48515-156">Nyní Pojďme povolení CORS v webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="48515-156">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="48515-157">Nejprve přidejte balíček CORS NuGet.</span><span class="sxs-lookup"><span data-stu-id="48515-157">First, add the CORS NuGet package.</span></span> <span data-ttu-id="48515-158">V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="48515-158">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="48515-159">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="48515-159">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="48515-160">Tento příkaz nainstaluje nejnovější balíček a aktualizuje všechny závislosti, včetně základní knihovny webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="48515-160">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="48515-161">Uživatel-příznak verze cílit na konkrétní verzi.</span><span class="sxs-lookup"><span data-stu-id="48515-161">User the -Version flag to target a specific version.</span></span> <span data-ttu-id="48515-162">Vyžaduje balíček CORS webového rozhraní API 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="48515-162">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="48515-163">Otevřete soubor aplikace\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="48515-163">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="48515-164">Přidejte následující kód, který **WebApiConfig.Register** metoda.</span><span class="sxs-lookup"><span data-stu-id="48515-164">Add the following code to the **WebApiConfig.Register** method.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="48515-165">Dál přidejte **[EnableCors]** atribut `TestController` třídy:</span><span class="sxs-lookup"><span data-stu-id="48515-165">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="48515-166">Pro *zdroje* parametr, použijte identifikátor URI, které jste nasadili webový klient aplikace.</span><span class="sxs-lookup"><span data-stu-id="48515-166">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="48515-167">To umožňuje žádostí napříč zdroji z WebClient, při stále zakazuje všech ostatních požadavků napříč doménami.</span><span class="sxs-lookup"><span data-stu-id="48515-167">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="48515-168">Později, I budete popisují parametry **[EnableCors]** podrobněji.</span><span class="sxs-lookup"><span data-stu-id="48515-168">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="48515-169">Nezahrnují lomítka na konci *zdroje* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="48515-169">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="48515-170">Znovu nasaďte aktualizovanou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="48515-170">Redeploy the updated WebService application.</span></span> <span data-ttu-id="48515-171">Nemusíte aktualizovat WebClient.</span><span class="sxs-lookup"><span data-stu-id="48515-171">You don't need to update WebClient.</span></span> <span data-ttu-id="48515-172">Nyní požadavek AJAX WebClient uspěli.</span><span class="sxs-lookup"><span data-stu-id="48515-172">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="48515-173">Metody GET a PUT, POST všechny povolené.</span><span class="sxs-lookup"><span data-stu-id="48515-173">The GET, PUT, and POST methods are all allowed.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a><span data-ttu-id="48515-174">Jak funguje CORS</span><span class="sxs-lookup"><span data-stu-id="48515-174">How CORS Works</span></span>

<span data-ttu-id="48515-175">Tato část popisuje, co se stane, že v žádosti o CORS na úrovni zpráv protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="48515-175">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="48515-176">Je důležité pochopit, jak funguje CORS, takže můžete nakonfigurovat **[EnableCors]** atribut správně a řešení potíží s Pokud věcí nefungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="48515-176">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly, and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="48515-177">Specifikace CORS zavádí několik nových hlavičky HTTP, které povolení žádostí napříč zdroji.</span><span class="sxs-lookup"><span data-stu-id="48515-177">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="48515-178">Pokud prohlížeč podporuje CORS, nastaví tato záhlaví automaticky pro požadavky cross-origin; nemusíte provádět žádné zvláštní v kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48515-178">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="48515-179">Tady je příklad žádost nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="48515-179">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="48515-180">Záhlaví "Původ" dává domény, lokality, která odeslala žádost.</span><span class="sxs-lookup"><span data-stu-id="48515-180">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="48515-181">Pokud server umožňuje žádost, nastaví hlavičky Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="48515-181">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="48515-182">Hodnotu této hlavičky buď odpovídá hlavičky počátku, nebo je hodnota zástupný znak "\*", což znamená, že jakýkoli původ je povolen.</span><span class="sxs-lookup"><span data-stu-id="48515-182">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="48515-183">Pokud odpověď nezahrnuje hlavičky Access-Control-Allow-Origin, požadavek AJAX se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="48515-183">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="48515-184">V prohlížeči přímo, neumožňuje žádosti.</span><span class="sxs-lookup"><span data-stu-id="48515-184">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="48515-185">I když server vrátí úspěšné odpovědi, prohlížeč není zpřístupnit odpověď do klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="48515-185">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="48515-186">**Předběžných požadavků**</span><span class="sxs-lookup"><span data-stu-id="48515-186">**Preflight Requests**</span></span>

<span data-ttu-id="48515-187">U některých požadavků CORS prohlížeč odešle požadavek další, nazývá "předběžný požadavek", než odešle skutečné požadavek na prostředek.</span><span class="sxs-lookup"><span data-stu-id="48515-187">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="48515-188">V prohlížeči můžete přeskočit předběžný požadavek, pokud jsou splněny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="48515-188">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="48515-189">Metoda požadavku je GET, HEAD nebo POST, *a*</span><span class="sxs-lookup"><span data-stu-id="48515-189">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="48515-190">Aplikace není nastavena všechny hlavičky požadavku než přijmout, Accept-Language, jazyk obsahu Content-Type nebo poslední událost ID, *a*</span><span class="sxs-lookup"><span data-stu-id="48515-190">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="48515-191">Hlavička Content-Type (Pokud nastavit) je jedním z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="48515-191">The Content-Type header (if set) is one of the following:</span></span> 

    - <span data-ttu-id="48515-192">Application/x--www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="48515-192">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="48515-193">multipart/formulář data</span><span class="sxs-lookup"><span data-stu-id="48515-193">multipart/form-data</span></span>
    - <span data-ttu-id="48515-194">text/plain</span><span class="sxs-lookup"><span data-stu-id="48515-194">text/plain</span></span>

<span data-ttu-id="48515-195">Pravidlo o hlavičky požadavku se vztahuje na hlavičky, které aplikace nastaví voláním **setRequestHeader** na **XMLHttpRequest** objektu.</span><span class="sxs-lookup"><span data-stu-id="48515-195">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="48515-196">(Specifikace CORS volá tyto "Autor hlavičky žádosti"). Pravidlo se nevztahuje na záhlaví *prohlížeče* můžete nastavit, jako je například uživatelský Agent, hostitele nebo Content-Length.</span><span class="sxs-lookup"><span data-stu-id="48515-196">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="48515-197">Tady je příklad předběžný požadavek:</span><span class="sxs-lookup"><span data-stu-id="48515-197">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="48515-198">Žádost o předběžné letu používá metodu HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="48515-198">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="48515-199">Obsahuje dva speciální hlavičky:</span><span class="sxs-lookup"><span data-stu-id="48515-199">It includes two special headers:</span></span>

- <span data-ttu-id="48515-200">Access-Control-Request-Method: Metodu protokolu HTTP, který se použije pro skutečný požadavek.</span><span class="sxs-lookup"><span data-stu-id="48515-200">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="48515-201">Access-Control-Request-Headers: Seznam hlaviček požadavku který *aplikace* nastavit na skutečnou žádost.</span><span class="sxs-lookup"><span data-stu-id="48515-201">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="48515-202">(Znovu, nezahrnuje hlavičky, které nastaví prohlížeče.)</span><span class="sxs-lookup"><span data-stu-id="48515-202">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="48515-203">Zde je odpověď příklad, za předpokladu, že server umožňuje žádost:</span><span class="sxs-lookup"><span data-stu-id="48515-203">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="48515-204">Odpověď obsahuje hlavičku přístup – ovládací prvek-Allow-Methods, který obsahuje seznam povolených metod a volitelně hlavičky Access-Control-povolit-Headers, kde jsou uvedeny povolené hlavičky.</span><span class="sxs-lookup"><span data-stu-id="48515-204">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="48515-205">Pokud předběžný požadavek úspěšné, prohlížeč odesílá skutečné požadavku, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="48515-205">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="48515-206">Pravidla rozsahu pro [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="48515-206">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="48515-207">CORS můžete povolit na každou akci, na jeden kontroler nebo globálně pro všechny řadiče webového rozhraní API ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="48515-207">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="48515-208">**Na každou akci**</span><span class="sxs-lookup"><span data-stu-id="48515-208">**Per Action**</span></span>

<span data-ttu-id="48515-209">Chcete-li povolit CORS pro jednu akci, nastavte **[EnableCors]** atribut na metodu akce.</span><span class="sxs-lookup"><span data-stu-id="48515-209">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="48515-210">Následující příklad umožňuje použití CORS pro `GetItem` pouze metoda.</span><span class="sxs-lookup"><span data-stu-id="48515-210">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="48515-211">**Na jeden kontroler**</span><span class="sxs-lookup"><span data-stu-id="48515-211">**Per Controller**</span></span>

<span data-ttu-id="48515-212">Pokud nastavíte **[EnableCors]** na třídy kontroleru se vztahuje na všechny akce v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="48515-212">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="48515-213">Chcete-li zakázat CORS pro akce, přidejte **[DisableCors]** atribut akce.</span><span class="sxs-lookup"><span data-stu-id="48515-213">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="48515-214">Následující příklad umožňuje použití CORS pro každou metodu s výjimkou `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="48515-214">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="48515-215">**Globálně**</span><span class="sxs-lookup"><span data-stu-id="48515-215">**Globally**</span></span>

<span data-ttu-id="48515-216">K povolení sdílení CORS pro všechny řadiče webového rozhraní API v aplikaci, předat **EnableCorsAttribute** instance k **EnableCors** metoda:</span><span class="sxs-lookup"><span data-stu-id="48515-216">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="48515-217">Pokud atribut nastavíte na více než jednoho oboru, je pořadí podle priority:</span><span class="sxs-lookup"><span data-stu-id="48515-217">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="48515-218">Akce</span><span class="sxs-lookup"><span data-stu-id="48515-218">Action</span></span>
2. <span data-ttu-id="48515-219">Řadiče</span><span class="sxs-lookup"><span data-stu-id="48515-219">Controller</span></span>
3. <span data-ttu-id="48515-220">Globální</span><span class="sxs-lookup"><span data-stu-id="48515-220">Global</span></span>

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a><span data-ttu-id="48515-221">Nastavit povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="48515-221">Set the Allowed Origins</span></span>

<span data-ttu-id="48515-222">*Zdroje* parametr **[EnableCors]** atribut určuje, jaké zdroje jsou povoleny pro přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="48515-222">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="48515-223">Hodnota je čárkami oddělený seznam Povolené zdroje.</span><span class="sxs-lookup"><span data-stu-id="48515-223">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="48515-224">Můžete také použít zástupný znak hodnotu "\*" Povolit požadavky od žádné zdroje.</span><span class="sxs-lookup"><span data-stu-id="48515-224">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="48515-225">Pečlivě zvažte, před povolením požadavky od jakýkoli původ.</span><span class="sxs-lookup"><span data-stu-id="48515-225">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="48515-226">Znamená to, že názvy všech webu provádět volání AJAX do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="48515-226">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="48515-227">Nastavit metody povolené HTTP</span><span class="sxs-lookup"><span data-stu-id="48515-227">Set the Allowed HTTP Methods</span></span>

<span data-ttu-id="48515-228">*Metody* parametr **[EnableCors]** atribut určuje, jaké metody HTTP jsou povoleny pro přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="48515-228">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="48515-229">Pokud chcete povolit všechny metody, použijte hodnotu zástupný znak "\*".</span><span class="sxs-lookup"><span data-stu-id="48515-229">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="48515-230">Následující příklad umožňuje pouze požadavky GET a POST.</span><span class="sxs-lookup"><span data-stu-id="48515-230">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="48515-231">Nastavit hlavičky povolených požadavků</span><span class="sxs-lookup"><span data-stu-id="48515-231">Set the Allowed Request Headers</span></span>

<span data-ttu-id="48515-232">Dříve I popisuje jak předběžný požadavek může obsahovat hlavičku Access-Control-Request-Headers výpis hlavičky protokolu HTTP, které jsou nastaveny aplikací (takzvaný "vytvářet hlavičky žádosti").</span><span class="sxs-lookup"><span data-stu-id="48515-232">Earlier I described how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="48515-233">*Hlavičky* parametr **[EnableCors]** atribut určuje, které autor hlavičky žádosti jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="48515-233">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="48515-234">Chcete-li povolit všechny hlavičky, nastavte *hlavičky* na "\*".</span><span class="sxs-lookup"><span data-stu-id="48515-234">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="48515-235">Chcete-li konkrétní hlavičky seznamu povolených IP adres, nastavte *hlavičky* na seznam povolených hlaviček oddělených čárkami:</span><span class="sxs-lookup"><span data-stu-id="48515-235">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="48515-236">Však nejsou prohlížeče zcela konzistentní v tom, jak nastavují Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="48515-236">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="48515-237">Například Chrome aktuálně obsahuje "Původ"; Při FireFox nezahrnuje hlavičky standardních, jako je například "Přijmout", i když je aplikace nastaví ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="48515-237">For example, Chrome currently includes "origin"; while FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="48515-238">Pokud nastavíte *hlavičky* k ničemu jiný než "\*", by měla obsahovat alespoň "přijmout", "content-type" a "Původ" a jakékoli vlastní hlavičky, které chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="48515-238">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="48515-239">Nastavit hlavičky povolené odpovědi</span><span class="sxs-lookup"><span data-stu-id="48515-239">Set the Allowed Response Headers</span></span>

<span data-ttu-id="48515-240">Ve výchozím prohlížeči nevystavuje všechny hlavičky odpovědi do aplikace.</span><span class="sxs-lookup"><span data-stu-id="48515-240">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="48515-241">Hlavičky odpovědi, které jsou k dispozici ve výchozím nastavení jsou:</span><span class="sxs-lookup"><span data-stu-id="48515-241">The response headers that are available by default are:</span></span>

- <span data-ttu-id="48515-242">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="48515-242">Cache-Control</span></span>
- <span data-ttu-id="48515-243">Obsah – jazyk</span><span class="sxs-lookup"><span data-stu-id="48515-243">Content-Language</span></span>
- <span data-ttu-id="48515-244">Typ obsahu</span><span class="sxs-lookup"><span data-stu-id="48515-244">Content-Type</span></span>
- <span data-ttu-id="48515-245">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="48515-245">Expires</span></span>
- <span data-ttu-id="48515-246">Poslední úpravy</span><span class="sxs-lookup"><span data-stu-id="48515-246">Last-Modified</span></span>
- <span data-ttu-id="48515-247">Direktiva pragma</span><span class="sxs-lookup"><span data-stu-id="48515-247">Pragma</span></span>

<span data-ttu-id="48515-248">Specifikace CORS volá tyto [hlavičky odpovědi jednoduché](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="48515-248">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="48515-249">Chcete-li zpřístupnit další hlavičky pro aplikace, nastavte *exposedHeaders* parametr **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="48515-249">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="48515-250">V následujícím příkladu, řadičem na `Get` metoda nastaví vlastní hlavičku s názvem 'X-vlastní hlavičku'.</span><span class="sxs-lookup"><span data-stu-id="48515-250">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="48515-251">Ve výchozím nastavení nebude prohlížeč vystavit této hlavičky v požadavku nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="48515-251">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="48515-252">Pokud chcete zpřístupnit záhlaví, zahrnout 'X vlastní hlavičky, *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="48515-252">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a><span data-ttu-id="48515-253">Předávání pověření v žádostí napříč zdroji</span><span class="sxs-lookup"><span data-stu-id="48515-253">Passing Credentials in Cross-Origin Requests</span></span>

<span data-ttu-id="48515-254">Přihlašovací údaje vyžadují zvláštní zpracování v požadavek CORS.</span><span class="sxs-lookup"><span data-stu-id="48515-254">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="48515-255">Ve výchozím prohlížeči neodešle všechny přihlašovací údaje s žádostí napříč zdroji.</span><span class="sxs-lookup"><span data-stu-id="48515-255">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="48515-256">Přihlašovací údaje zahrnují soubory cookie, jakož i schémat ověřování protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="48515-256">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="48515-257">K odeslání pověření s žádost nepůvodního zdroje, musíte nastavit klienta **XMLHttpRequest.withCredentials** na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="48515-257">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="48515-258">Pomocí **XMLHttpRequest** přímo:</span><span class="sxs-lookup"><span data-stu-id="48515-258">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="48515-259">V jQuery:</span><span class="sxs-lookup"><span data-stu-id="48515-259">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="48515-260">Kromě toho serveru musí umožňovat přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="48515-260">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="48515-261">Povolit cross-origin přihlašovací údaje v rozhraní Web API, nastavte **SupportsCredentials** vlastnost na hodnotu true na **[EnableCors]** atribut:</span><span class="sxs-lookup"><span data-stu-id="48515-261">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="48515-262">Pokud je tato vlastnost hodnotu true, odpověď HTTP bude obsahovat hlavičku přístup – ovládací prvek-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="48515-262">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="48515-263">Tuto hlavičku sděluje prohlížeči, že server umožňuje přihlašovací údaje pro žádost o nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="48515-263">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="48515-264">Pokud prohlížeč odesílá přihlašovací údaje, ale odpověď neobsahuje platný přístup – ovládací prvek-Allow-Credentials záhlaví, prohlížeče nebude vystavit odpověď na žádost a vyvolá chybu požadavek AJAX.</span><span class="sxs-lookup"><span data-stu-id="48515-264">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="48515-265">Buďte velmi opatrní o nastavení **SupportsCredentials** na hodnotu true, protože znamená webu v jiné doméně může odesílat pověření přihlášeného uživatele na váš Web API jménem uživatele, aniž by uživatel znal.</span><span class="sxs-lookup"><span data-stu-id="48515-265">Be very careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="48515-266">Specifikace CORS také stavy tohoto nastavení *zdroje* k &quot; \* &quot; je neplatný Pokud **SupportsCredentials** hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="48515-266">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a><span data-ttu-id="48515-267">Zprostředkovatelé zásady vlastní CORS</span><span class="sxs-lookup"><span data-stu-id="48515-267">Custom CORS Policy Providers</span></span>

<span data-ttu-id="48515-268">**[EnableCors]** atribut implementuje **ICorsPolicyProvider** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="48515-268">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="48515-269">Vlastní implementace můžete zadat třídu odvozenou od **atribut** a implementuje **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="48515-269">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="48515-270">Teď můžete použít atribut žádné umístit, že by měla být umístěna **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="48515-270">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="48515-271">Například vlastní zprostředkovatel zásad CORS by mohl číst nastavení z konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="48515-271">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="48515-272">Jako alternativu k použití atributy, můžete zaregistrovat **ICorsPolicyProviderFactory** objekt, který vytvoří **ICorsPolicyProvider** objekty.</span><span class="sxs-lookup"><span data-stu-id="48515-272">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="48515-273">Chcete-li nastavit **ICorsPolicyProviderFactory**, volání **SetCorsPolicyProviderFactory** metoda rozšíření při spuštění, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="48515-273">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a><span data-ttu-id="48515-274">Podpora prohlížeče</span><span class="sxs-lookup"><span data-stu-id="48515-274">Browser Support</span></span>

<span data-ttu-id="48515-275">Balíček CORS webového rozhraní API je technologie straně serveru.</span><span class="sxs-lookup"><span data-stu-id="48515-275">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="48515-276">Také musí podporovat CORS prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="48515-276">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="48515-277">Naštěstí aktuálních verzích všechny hlavní prohlížeče zahrnují [podpora CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="48515-277">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

<span data-ttu-id="48515-278">Internet Explorer 8 a Internet Explorer 9 je částečná podpora CORS, pomocí starší verze objektu XDomainRequest místo XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="48515-278">Internet Explorer 8 and Internet Explorer 9 have partial support for CORS, using the legacy XDomainRequest object instead of XMLHttpRequest.</span></span> <span data-ttu-id="48515-279">Další informace najdete v tématu [XDomainRequest - omezení, omezení a řešení](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span><span class="sxs-lookup"><span data-stu-id="48515-279">For more information, see [XDomainRequest - Restrictions, Limitations and Workarounds](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span></span>
