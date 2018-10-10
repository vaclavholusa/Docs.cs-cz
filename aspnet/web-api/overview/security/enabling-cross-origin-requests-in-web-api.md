---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Povolení žádostí nepůvodního zdroje v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: Ukazuje, jak k podpoře sdílení prostředků mezi zdroji (CORS) v rozhraní ASP.NET Web API.
ms.author: riande
ms.date: 07/15/2014
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: dc95c39af0821c2f456f5a312de5532c5aeb3c10
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912199"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="0a4cb-103">Povolení žádostí nepůvodního zdroje v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="0a4cb-103">Enabling Cross-Origin Requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="0a4cb-104">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0a4cb-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="0a4cb-105">Zabezpečení prohlížečů brání zasílání požadavků AJAX na jinou doménu na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="0a4cb-106">Toto omezení je volána *zásada stejného zdroje*a brání škodlivým webům ve čtení citlivých dat z jiné lokality.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="0a4cb-107">Ale v některých případech můžete chtít nechat ostatních lokalit volání webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-107">However, sometimes you might want to let other sites call your web API.</span></span>
> 
> <span data-ttu-id="0a4cb-108">[Mezi sdílení zdrojů původu](http://www.w3.org/TR/cors/) (CORS) je standard W3C, která umožňuje server zmírnit zásadu stejného zdroje.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="0a4cb-109">Pomocí CORS, server explicitně můžou některé požadavky cross-origin zatímco jiné odmítnout.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="0a4cb-110">CORS je bezpečnější a flexibilnější, než starší techniky, jako [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="0a4cb-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="0a4cb-111">Tento kurz ukazuje postupy při povolení CORS v aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0a4cb-112">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="0a4cb-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="0a4cb-113">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="0a4cb-113">Visual Studio 2013 Update 2</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="0a4cb-114">Webové rozhraní API 2.2</span><span class="sxs-lookup"><span data-stu-id="0a4cb-114">Web API 2.2</span></span>


<a id="intro"></a>
## <a name="introduction"></a><span data-ttu-id="0a4cb-115">Úvod</span><span class="sxs-lookup"><span data-stu-id="0a4cb-115">Introduction</span></span>

<span data-ttu-id="0a4cb-116">Tento kurz ukazuje, že podpora CORS v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="0a4cb-117">Začneme tím, že vytvoříte dva projekty ASP.NET – jeden volané "Webová služba", který je hostitelem kontroler Web API, a ostatní volané "WebClient", která volá webové služby.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="0a4cb-118">Protože jsou dvě aplikace hostované v různých doménách, je požadavek AJAX z webový klient webové služby žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="0a4cb-119">Co je "Stejné zdroj"?</span><span class="sxs-lookup"><span data-stu-id="0a4cb-119">What is "Same Origin"?</span></span>

<span data-ttu-id="0a4cb-120">Dvě adresy URL mají stejný původ, pokud mají stejné schémata, hostitele a porty.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="0a4cb-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="0a4cb-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="0a4cb-122">Tyto dvě adresy URL mají stejného původu:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="0a4cb-123">Tyto adresy URL mají různé zdroje než ta předchozí dvě:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="0a4cb-124">`http://example.net` -Jinou doménu</span><span class="sxs-lookup"><span data-stu-id="0a4cb-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="0a4cb-125">`http://example.com:9000/foo.html` -Jiný port</span><span class="sxs-lookup"><span data-stu-id="0a4cb-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="0a4cb-126">`https://example.com/foo.html` -Jiné schéma</span><span class="sxs-lookup"><span data-stu-id="0a4cb-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="0a4cb-127">`http://www.example.com/foo.html` – Různé subdomény</span><span class="sxs-lookup"><span data-stu-id="0a4cb-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="0a4cb-128">Aplikace Internet Explorer nebere v úvahu port při porovnání zdrojů.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-128">Internet Explorer does not consider the port when comparing origins.</span></span>


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a><span data-ttu-id="0a4cb-129">Vytvoření projektu webové služby</span><span class="sxs-lookup"><span data-stu-id="0a4cb-129">Create the WebService Project</span></span>

> [!NOTE]
> <span data-ttu-id="0a4cb-130">Této části se předpokládá, že už víte, jak vytvořit projekty webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="0a4cb-131">Pokud ne, přečtěte si téma [Začínáme s rozhraním ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="0a4cb-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>


<span data-ttu-id="0a4cb-132">Spusťte sadu Visual Studio a vytvořte nový **webová aplikace ASP.NET** projektu.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-132">Start Visual Studio and create a new **ASP.NET Web Application** project.</span></span> <span data-ttu-id="0a4cb-133">Vyberte **prázdný** šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-133">Select the **Empty** project template.</span></span> <span data-ttu-id="0a4cb-134">V části "Přidat složky a základní odkazy pro" vyberte **webového rozhraní API** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-134">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="0a4cb-135">Volitelně vyberte možnost "Hostitel v cloudu" k nasazení aplikace do Azure výrobci.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-135">Optionally, select the "Host in Cloud" option to deploy the app to Mircosoft Azure.</span></span> <span data-ttu-id="0a4cb-136">Společnost Microsoft nabízí bezplatné webových hostitelských služeb pro až 10 služeb websites ve [Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="0a4cb-136">Microsoft offers free web hosting for up to 10 websites in a [free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

<span data-ttu-id="0a4cb-137">Přidat kontroler Web API s názvem `TestController` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-137">Add a Web API controller named `TestController` with the following code:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

<span data-ttu-id="0a4cb-138">Můžete spustit aplikaci místně nebo nasazení do Azure.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-138">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="0a4cb-139">(Pro snímky obrazovky v tomto kurzu, můžu nasadit do Azure App Service Web Apps.) Pokud chcete ověřit, že je pracovní webové rozhraní API, přejděte na `http://hostname/api/test/`, kde *název hostitele* je doména, kam jste nasadili aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-139">(For the screenshots in this tutorial, I deployed to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="0a4cb-140">Měli byste vidět text odpovědi &quot;získat: testovací zpráva&quot;.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-140">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a><span data-ttu-id="0a4cb-141">Vytvoření projektu WebClient</span><span class="sxs-lookup"><span data-stu-id="0a4cb-141">Create the WebClient Project</span></span>

<span data-ttu-id="0a4cb-142">Vytvořte nový projekt webové aplikace ASP.NET a vyberte **MVC** šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-142">Create another ASP.NET Web Application project and select the **MVC** project template.</span></span> <span data-ttu-id="0a4cb-143">Volitelně vyberte **změna ověřování** > **bez ověřování**.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="0a4cb-144">Pro účely tohoto kurzu nepotřebujete ověřování.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-144">You don't need authentication for this tutorial.</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

<span data-ttu-id="0a4cb-145">V Průzkumníku řešení otevřete soubor Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-145">In Solution Explorer, open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="0a4cb-146">Nahraďte kód v tomto souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-146">Replace the code in this file with the following:</span></span>

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

<span data-ttu-id="0a4cb-147">Pro *serviceUrl* proměnné, použijte identifikátor URI aplikace webové služby.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-147">For the *serviceUrl* variable, use the URI of the WebService app.</span></span> <span data-ttu-id="0a4cb-148">Nyní místním spouštění aplikace webový klient nebo ji publikovat na jiný web.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-148">Now run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="0a4cb-149">Kliknutím na tlačítko "Vyzkoušet" odešle požadavek AJAX do aplikace webové služby, pomocí protokolu HTTP metody uvedené v rozevíracím seznamu (GET, POST a PUT).</span><span class="sxs-lookup"><span data-stu-id="0a4cb-149">Clicking the "Try It" button submits an AJAX request to the WebService app, using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="0a4cb-150">To umožňuje nám prozkoumat různé požadavky cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-150">This lets us examine different cross-origin requests.</span></span> <span data-ttu-id="0a4cb-151">V tuto chvíli, aplikace webové služby nepodporuje CORS, takže pokud kliknete na tlačítko, obdržíte chybu.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-151">Right now, the WebService app does not support CORS, so if you click the button, you will get an error.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="0a4cb-152">Pokud sledujete přenos pomocí protokolu HTTP v nástroji, jako jsou [Fiddler](http://www.telerik.com/fiddler), zobrazí se, že do prohlížeče odeslat požadavek na získání a úspěšného vykonání požadavku, ale volání jazyka AJAX vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-152">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you will see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="0a4cb-153">Je důležité pochopit, že zásada stejného zdroje nezabraňuje prohlížeče z *odesílání* požadavku.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-153">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="0a4cb-154">Místo toho brání aplikaci v zobrazení *odpovědi*.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-154">Instead, it prevents the application from seeing the *response*.</span></span>


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a><span data-ttu-id="0a4cb-155">Povolení CORS</span><span class="sxs-lookup"><span data-stu-id="0a4cb-155">Enable CORS</span></span>

<span data-ttu-id="0a4cb-156">Nyní Pojďme povolení CORS v app webové služby.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-156">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="0a4cb-157">Nejprve přidejte balíček CORS NuGet.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-157">First, add the CORS NuGet package.</span></span> <span data-ttu-id="0a4cb-158">V aplikaci Visual Studio z **nástroje** příkaz **Správce balíčků NuGet**, vyberte **konzoly Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-158">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="0a4cb-159">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-159">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="0a4cb-160">Tento příkaz nainstaluje nejnovější balíček a aktualizuje všechny závislosti, včetně základní webové rozhraní API knihovny.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-160">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="0a4cb-161">Uživatel příznak-Version pro cílení na konkrétní verzi.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-161">User the -Version flag to target a specific version.</span></span> <span data-ttu-id="0a4cb-162">Vyžaduje balíček CORS webového rozhraní API 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-162">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="0a4cb-163">Otevřete soubor aplikace\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-163">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="0a4cb-164">Přidejte následující kód, který **WebApiConfig.Register** metody.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-164">Add the following code to the **WebApiConfig.Register** method.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="0a4cb-165">Dále přidejte **[EnableCors]** atribut `TestController` třídy:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-165">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="0a4cb-166">Pro *zdroje* parametrů, použijte identifikátor URI, kam jste nasadili webový klient aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-166">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="0a4cb-167">Díky tomu požadavky cross-origin z WebClient, při stále Probíhá zakazování všech ostatních požadavků mezi doménami.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-167">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="0a4cb-168">Později budete I popisují parametry pro **[EnableCors]** podrobněji.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-168">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="0a4cb-169">Nezahrnují dopředné lomítko na konci *zdroje* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-169">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="0a4cb-170">Znovu nasaďte aktualizovanou aplikaci webové služby.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-170">Redeploy the updated WebService application.</span></span> <span data-ttu-id="0a4cb-171">Není nutné aktualizovat WebClient.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-171">You don't need to update WebClient.</span></span> <span data-ttu-id="0a4cb-172">Nyní požadavek AJAX z WebClient uspěli.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-172">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="0a4cb-173">Metody GET a PUT, POST všechny povolené.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-173">The GET, PUT, and POST methods are all allowed.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a><span data-ttu-id="0a4cb-174">Jak funguje CORS</span><span class="sxs-lookup"><span data-stu-id="0a4cb-174">How CORS Works</span></span>

<span data-ttu-id="0a4cb-175">Tato část popisuje, co se stane, že v požadavku CORS, na úrovni zprávy HTTP.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-175">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="0a4cb-176">Je důležité pochopit, jak funguje CORS, můžete nakonfigurovat tak, aby **[EnableCors]** atributu správně a vyřešit případné věci nefungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-176">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly, and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="0a4cb-177">Specifikace CORS zavádí několik nové hlavičky protokolu HTTP, které umožňují požadavky cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-177">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="0a4cb-178">Pokud je prohlížeč podporuje CORS, nastaví tyto hlavičky automaticky pro požadavky cross-origin; nemusíte dělat nic zvláštního v kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-178">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="0a4cb-179">Tady je příklad žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-179">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="0a4cb-180">Hlavička "Origin" dává domény, lokality, která odeslala žádost.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-180">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="0a4cb-181">Pokud server umožňuje, aby žádosti, nastaví hlavičky Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-181">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="0a4cb-182">Hodnotu této hlavičky odpovídá hlavička počátečního nebo je hodnota zástupného znaku "\*", což znamená, že jakýkoli původ je povolen.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-182">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="0a4cb-183">Pokud odpověď neobsahuje hlavičky Access-Control-Allow-Origin, požadavek AJAX selže.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-183">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="0a4cb-184">Konkrétně v prohlížeči zakazuje požadavku.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-184">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="0a4cb-185">I v případě, že server vrátí úspěšné odpovědi, prohlížeč není zpřístupnit odpověď do klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-185">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="0a4cb-186">**Předběžných požadavků**</span><span class="sxs-lookup"><span data-stu-id="0a4cb-186">**Preflight Requests**</span></span>

<span data-ttu-id="0a4cb-187">U některých požadavků CORS prohlížeč odešle požadavek další, nazývá "předběžný požadavek", předtím, než odešle skutečnou žádost pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-187">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="0a4cb-188">Prohlížeči můžete přeskočit předběžný požadavek, pokud jsou splněny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-188">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="0a4cb-189">Metoda žádosti je GET, HEAD nebo POST, *a*</span><span class="sxs-lookup"><span data-stu-id="0a4cb-189">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="0a4cb-190">Aplikace nemá nastaven záhlaví požadavku než přijmout, Accept-Language, jazyka obsahu Content-Type nebo poslední-Event-ID, *a*</span><span class="sxs-lookup"><span data-stu-id="0a4cb-190">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="0a4cb-191">Hlavička Content-Type (Pokud nastavit) je jedním z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-191">The Content-Type header (if set) is one of the following:</span></span> 

    - <span data-ttu-id="0a4cb-192">Application/x--www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="0a4cb-192">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="0a4cb-193">multipart/formuláře data</span><span class="sxs-lookup"><span data-stu-id="0a4cb-193">multipart/form-data</span></span>
    - <span data-ttu-id="0a4cb-194">text/plain</span><span class="sxs-lookup"><span data-stu-id="0a4cb-194">text/plain</span></span>

<span data-ttu-id="0a4cb-195">Pravidlo o hlavičky požadavku se vztahuje na hlavičky, které aplikace nastaví voláním **setRequestHeader** na **XMLHttpRequest** objektu.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-195">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="0a4cb-196">(Specifikaci CORS volá tyto "Autor hlavičky žádosti".) Toto pravidlo se nevztahuje na záhlaví *prohlížeče* můžete nastavit, jako je například uživatelského agenta, hostitele nebo Content-Length.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-196">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="0a4cb-197">Tady je příklad předběžný požadavek:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-197">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="0a4cb-198">Přípravné požadavek používá metodu HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-198">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="0a4cb-199">Obsahuje dva speciálními záhlavími:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-199">It includes two special headers:</span></span>

- <span data-ttu-id="0a4cb-200">Access-Control-Request-Method: Metoda HTTP, který se použije pro aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-200">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="0a4cb-201">Access-Control-Request-Headers: Seznam hlaviček požadavků, který *aplikace* nastavit u aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-201">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="0a4cb-202">(Znovu, nezahrnuje hlavičky, které nastaví prohlížeče.)</span><span class="sxs-lookup"><span data-stu-id="0a4cb-202">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="0a4cb-203">Tady je příklad odpovědi, za předpokladu, že server umožňuje žádosti:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-203">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="0a4cb-204">Odpověď obsahuje hlavičku přístup – ovládací prvek-Allow-Methods, který obsahuje seznam povolených metod a volitelně hlavičky Access-Control-povolit-Headers, která zobrazuje povolené hlavičky.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-204">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="0a4cb-205">Pokud je předběžný požadavek úspěšné, prohlížeč odesílá skutečnou žádost, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-205">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="0a4cb-206">Obor pravidla pro [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="0a4cb-206">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="0a4cb-207">CORS můžete povolit každou akci, na kontroler nebo globálně pro všechny kontrolery rozhraní Web API ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-207">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="0a4cb-208">**Za akci**</span><span class="sxs-lookup"><span data-stu-id="0a4cb-208">**Per Action**</span></span>

<span data-ttu-id="0a4cb-209">Povolení CORS pro jednu akci, nastavte **[EnableCors]** atributu na metodu akce.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-209">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="0a4cb-210">Následující příklad umožňuje použití CORS pro `GetItem` pouze metody.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-210">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="0a4cb-211">**Na kontroler**</span><span class="sxs-lookup"><span data-stu-id="0a4cb-211">**Per Controller**</span></span>

<span data-ttu-id="0a4cb-212">Nastavíte-li **[EnableCors]** na třídy řadiče, vztahuje se na všechny akce v řadiči.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-212">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="0a4cb-213">Zakázání CORS pro akci, přidat **[DisableCors]** atribut k akci.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-213">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="0a4cb-214">Následující příklad umožňuje použití CORS pro každou metodu s výjimkou `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-214">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="0a4cb-215">**Globálně**</span><span class="sxs-lookup"><span data-stu-id="0a4cb-215">**Globally**</span></span>

<span data-ttu-id="0a4cb-216">Povolení CORS pro všechny řadiče webové rozhraní API ve vaší aplikaci, předejte **EnableCorsAttribute** instance na **EnableCors** metody:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-216">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="0a4cb-217">Pokud jste nastavili atribut na více než jednoho oboru, je pořadí podle priority:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-217">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="0a4cb-218">Akce</span><span class="sxs-lookup"><span data-stu-id="0a4cb-218">Action</span></span>
2. <span data-ttu-id="0a4cb-219">Kontroler</span><span class="sxs-lookup"><span data-stu-id="0a4cb-219">Controller</span></span>
3. <span data-ttu-id="0a4cb-220">Globální</span><span class="sxs-lookup"><span data-stu-id="0a4cb-220">Global</span></span>

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a><span data-ttu-id="0a4cb-221">Nastavte povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="0a4cb-221">Set the Allowed Origins</span></span>

<span data-ttu-id="0a4cb-222">*Původu* parametr **[EnableCors]** atribut určuje původu, které jsou povoleny pro přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-222">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="0a4cb-223">Hodnota je čárkou oddělený seznam Povolené zdroje.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-223">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="0a4cb-224">Můžete také použít zástupný znak hodnotu "\*" požadavky všechny původy.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-224">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="0a4cb-225">Pečlivě zvažte předtím, než žádosti z původu.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-225">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="0a4cb-226">To znamená, že doslova všechny weby mohl provádět volání AJAX do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-226">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="0a4cb-227">Nastavení HTTP povolené metody</span><span class="sxs-lookup"><span data-stu-id="0a4cb-227">Set the Allowed HTTP Methods</span></span>

<span data-ttu-id="0a4cb-228">*Metody* parametr **[EnableCors]** atribut určuje, jaké metody HTTP jsou povoleny pro přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-228">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="0a4cb-229">Pokud chcete povolit všechny metody, použijte hodnotu zástupný znak "\*".</span><span class="sxs-lookup"><span data-stu-id="0a4cb-229">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="0a4cb-230">Následující příklad umožňuje pouze požadavky GET a POST.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-230">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="0a4cb-231">Nastavit hlavičku povolené žádosti</span><span class="sxs-lookup"><span data-stu-id="0a4cb-231">Set the Allowed Request Headers</span></span>

<span data-ttu-id="0a4cb-232">Dříve jsem popisuje jak předběžný požadavek může obsahovat hlavičku Access-Control-Request-Headers výpis hlavičky protokolu HTTP, nastavte aplikací (takzvaný ", vytvářet hlavičky žádosti").</span><span class="sxs-lookup"><span data-stu-id="0a4cb-232">Earlier I described how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="0a4cb-233">*Záhlaví* parametr **[EnableCors]** atribut určuje, které autor hlavičky požadavku jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-233">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="0a4cb-234">Chcete-li povolit všechny hlavičky, nastavte *záhlaví* na "\*".</span><span class="sxs-lookup"><span data-stu-id="0a4cb-234">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="0a4cb-235">Na seznamu povolených IP adres konkrétní záhlaví, nastavte *záhlaví* do seznamu Povolené hlavičky oddělené čárkami:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-235">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="0a4cb-236">Prohlížeče však nejsou zcela konzistentní v tom, jak nastavují Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-236">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="0a4cb-237">Například Chrome aktuálně obsahuje "zdroj"; zatímco FireFox nezahrnuje hlavičky standardních, jako je například "Přijmout", i v případě, že aplikace nastaví ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-237">For example, Chrome currently includes "origin"; while FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="0a4cb-238">Pokud nastavíte *záhlaví* pro nic jiného než "\*", měli byste zahrnout alespoň "přijímat", "content-type" a "zdroj" a navíc jakékoli vlastní hlavičky, které chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-238">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="0a4cb-239">Nastavit hlavičky odpovědi povolené</span><span class="sxs-lookup"><span data-stu-id="0a4cb-239">Set the Allowed Response Headers</span></span>

<span data-ttu-id="0a4cb-240">Ve výchozím prohlížeči nezveřejňuje všechny hlavičky odpovědí do aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-240">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="0a4cb-241">Hlavičky odpovědi, které jsou k dispozici ve výchozím nastavení jsou:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-241">The response headers that are available by default are:</span></span>

- <span data-ttu-id="0a4cb-242">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="0a4cb-242">Cache-Control</span></span>
- <span data-ttu-id="0a4cb-243">Jazyk obsahu</span><span class="sxs-lookup"><span data-stu-id="0a4cb-243">Content-Language</span></span>
- <span data-ttu-id="0a4cb-244">Typ obsahu</span><span class="sxs-lookup"><span data-stu-id="0a4cb-244">Content-Type</span></span>
- <span data-ttu-id="0a4cb-245">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="0a4cb-245">Expires</span></span>
- <span data-ttu-id="0a4cb-246">Datum poslední změny</span><span class="sxs-lookup"><span data-stu-id="0a4cb-246">Last-Modified</span></span>
- <span data-ttu-id="0a4cb-247">Direktiva pragma</span><span class="sxs-lookup"><span data-stu-id="0a4cb-247">Pragma</span></span>

<span data-ttu-id="0a4cb-248">Specifikace CORS volá tyto [hlavičky odpovědi jednoduché](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="0a4cb-248">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="0a4cb-249">Chcete-li zpřístupnit další záhlaví aplikace, nastavte *exposedHeaders* parametr **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-249">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="0a4cb-250">V následujícím příkladu, kontroler společnosti `Get` metoda nastaví vlastní hlavičku s názvem "X-Custom-Header".</span><span class="sxs-lookup"><span data-stu-id="0a4cb-250">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="0a4cb-251">Ve výchozím nastavení nebude prohlížeč vystavit toto záhlaví v žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-251">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="0a4cb-252">Pokud chcete zpřístupnit záhlaví, patří "X-Custom-Header" v *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-252">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a><span data-ttu-id="0a4cb-253">Přihlašovací údaje předávání žádostí nepůvodního zdroje</span><span class="sxs-lookup"><span data-stu-id="0a4cb-253">Passing Credentials in Cross-Origin Requests</span></span>

<span data-ttu-id="0a4cb-254">Přihlašovací údaje vyžadují speciální zacházení v požadavku CORS.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-254">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="0a4cb-255">Ve výchozím prohlížeči neodešle žádné přihlašovací údaje se žádostí nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-255">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="0a4cb-256">Přihlašovací údaje zahrnují soubory cookie, jakož i schémat ověřování protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-256">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="0a4cb-257">Odesílá pověření s žádostí nepůvodního zdroje, musíte nastavit klienta **XMLHttpRequest.withCredentials** na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-257">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="0a4cb-258">Pomocí **XMLHttpRequest** přímo:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-258">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="0a4cb-259">V jQuery:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-259">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="0a4cb-260">Na serveru navíc musíte povolit přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-260">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="0a4cb-261">Nastavení umožňující pověření původu mezi webové rozhraní API **SupportsCredentials** vlastnost na hodnotu true na **[EnableCors]** atribut:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-261">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="0a4cb-262">Pokud je tato vlastnost hodnotu true, odpověď HTTP bude obsahovat hlavičku přístup – ovládací prvek-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-262">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="0a4cb-263">Tato hlavička sděluje prohlížeči, že server umožňuje přihlašovací údaje pro žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-263">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="0a4cb-264">Pokud prohlížeč odesílá pověření, ale odpověď neobsahuje platnou hlavičku přístup – ovládací prvek-Allow-Credentials, prohlížeči nebude vystavení odpovědi do aplikace a požadavek AJAX selže.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-264">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="0a4cb-265">Buďte velmi opatrní při nastavení **SupportsCredentials** na hodnotu true, protože to znamená, že web v jiné doméně můžete poslat pověření přihlášeného uživatele webové rozhraní API jménem uživatele, aniž by uživatel znal.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-265">Be very careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="0a4cb-266">Specifikace CORS také uvádí nastavení *zdroje* k &quot; \* &quot; je neplatný Pokud **SupportsCredentials** má hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-266">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a><span data-ttu-id="0a4cb-267">Zásady poskytovatele vlastní CORS</span><span class="sxs-lookup"><span data-stu-id="0a4cb-267">Custom CORS Policy Providers</span></span>

<span data-ttu-id="0a4cb-268">**[EnableCors]** implementuje atribut **ICorsPolicyProvider** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-268">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="0a4cb-269">Můžete zadat vlastní implementaci tak, že vytvoříte třídu, která je odvozena z **atribut** a implementuje **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-269">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="0a4cb-270">Nyní můžete použít atribut jakékoli místo, že by měla být umístěna **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-270">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="0a4cb-271">Vlastní zprostředkovatel zásad CORS například může číst nastavení z konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-271">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="0a4cb-272">Jako alternativu k použití atributů, můžete zaregistrovat **ICorsPolicyProviderFactory** objekt, který vytvoří **ICorsPolicyProvider** objekty.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-272">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="0a4cb-273">Chcete-li nastavit **ICorsPolicyProviderFactory**, volání **SetCorsPolicyProviderFactory** rozšiřující metoda při spuštění, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0a4cb-273">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a><span data-ttu-id="0a4cb-274">Podpora prohlížeče</span><span class="sxs-lookup"><span data-stu-id="0a4cb-274">Browser Support</span></span>

<span data-ttu-id="0a4cb-275">Balíček CORS webového rozhraní API je technologie na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-275">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="0a4cb-276">Také musí podporovat CORS webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-276">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="0a4cb-277">Naštěstí aktuální verze všech nejpoužívanějších prohlížečích obsahují [podporu CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="0a4cb-277">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

<span data-ttu-id="0a4cb-278">Aplikace Internet Explorer 8 a Internet Explorer 9 mít částečně se podporuje CORS, místo starší verze XDomainRequest objekt XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="0a4cb-278">Internet Explorer 8 and Internet Explorer 9 have partial support for CORS, using the legacy XDomainRequest object instead of XMLHttpRequest.</span></span> <span data-ttu-id="0a4cb-279">Další informace najdete v tématu [XDomainRequest – omezení, omezení a řešení](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a4cb-279">For more information, see [XDomainRequest - Restrictions, Limitations and Workarounds](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span></span>
