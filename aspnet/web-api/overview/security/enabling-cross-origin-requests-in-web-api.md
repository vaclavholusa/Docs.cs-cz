---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Povolení žádostí nepůvodního zdroje v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: Ukazuje, jak k podpoře sdílení prostředků mezi zdroji (CORS) v rozhraní ASP.NET Web API.
ms.author: riande
ms.date: 10/10/2018
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 118b779c89edb874f7f928315d1094738be5f097
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348517"
---
<a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="e1d93-103">Povolení žádostí nepůvodního v ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="e1d93-103">Enable cross-origin requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="e1d93-104">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e1d93-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e1d93-105">Zabezpečení prohlížečů brání zasílání požadavků AJAX na jinou doménu na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="e1d93-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="e1d93-106">Toto omezení je volána *zásada stejného zdroje*a brání škodlivým webům ve čtení citlivých dat z jiné lokality.</span><span class="sxs-lookup"><span data-stu-id="e1d93-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="e1d93-107">Ale v některých případech můžete chtít nechat ostatních lokalit volání webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e1d93-107">However, sometimes you might want to let other sites call your web API.</span></span>
>
> <span data-ttu-id="e1d93-108">[Mezi sdílení zdrojů původu](http://www.w3.org/TR/cors/) (CORS) je standard W3C, která umožňuje server zmírnit zásadu stejného zdroje.</span><span class="sxs-lookup"><span data-stu-id="e1d93-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="e1d93-109">Pomocí CORS, server explicitně můžou některé požadavky cross-origin zatímco jiné odmítnout.</span><span class="sxs-lookup"><span data-stu-id="e1d93-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="e1d93-110">CORS je bezpečnější a flexibilnější, než starší techniky, jako [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="e1d93-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="e1d93-111">Tento kurz ukazuje postupy při povolení CORS v aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e1d93-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
>
> ## <a name="software-used-in-the-tutorial"></a><span data-ttu-id="e1d93-112">V tomto kurzu použili softwaru</span><span class="sxs-lookup"><span data-stu-id="e1d93-112">Software used in the tutorial</span></span>
>
> - [<span data-ttu-id="e1d93-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1d93-113">Visual Studio</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="e1d93-114">Webové rozhraní API 2.2</span><span class="sxs-lookup"><span data-stu-id="e1d93-114">Web API 2.2</span></span>

## <a name="introduction"></a><span data-ttu-id="e1d93-115">Úvod</span><span class="sxs-lookup"><span data-stu-id="e1d93-115">Introduction</span></span>

<span data-ttu-id="e1d93-116">Tento kurz ukazuje, že podpora CORS v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="e1d93-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="e1d93-117">Začneme tím, že vytvoříte dva projekty ASP.NET – jeden volané "Webová služba", který je hostitelem kontroler Web API, a ostatní volané "WebClient", která volá webové služby.</span><span class="sxs-lookup"><span data-stu-id="e1d93-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="e1d93-118">Protože jsou dvě aplikace hostované v různých doménách, je požadavek AJAX z webový klient webové služby žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="e1d93-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="e1d93-119">Co je "stejné zdroj"?</span><span class="sxs-lookup"><span data-stu-id="e1d93-119">What is "same origin"?</span></span>

<span data-ttu-id="e1d93-120">Dvě adresy URL mají stejný původ, pokud mají stejné schémata, hostitele a porty.</span><span class="sxs-lookup"><span data-stu-id="e1d93-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="e1d93-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="e1d93-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="e1d93-122">Tyto dvě adresy URL mají stejného původu:</span><span class="sxs-lookup"><span data-stu-id="e1d93-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="e1d93-123">Tyto adresy URL mají různé zdroje než ta předchozí dvě:</span><span class="sxs-lookup"><span data-stu-id="e1d93-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="e1d93-124">`http://example.net` -Jinou doménu</span><span class="sxs-lookup"><span data-stu-id="e1d93-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="e1d93-125">`http://example.com:9000/foo.html` -Jiný port</span><span class="sxs-lookup"><span data-stu-id="e1d93-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="e1d93-126">`https://example.com/foo.html` -Jiné schéma</span><span class="sxs-lookup"><span data-stu-id="e1d93-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="e1d93-127">`http://www.example.com/foo.html` – Různé subdomény</span><span class="sxs-lookup"><span data-stu-id="e1d93-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="e1d93-128">Aplikace Internet Explorer nebere v úvahu port při porovnání zdrojů.</span><span class="sxs-lookup"><span data-stu-id="e1d93-128">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="create-the-webservice-project"></a><span data-ttu-id="e1d93-129">Vytvoření projektu webové služby</span><span class="sxs-lookup"><span data-stu-id="e1d93-129">Create the WebService project</span></span>

> [!NOTE]
> <span data-ttu-id="e1d93-130">Této části se předpokládá, že už víte, jak vytvořit projekty webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e1d93-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="e1d93-131">Pokud ne, přečtěte si téma [Začínáme s rozhraním ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e1d93-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>

1. <span data-ttu-id="e1d93-132">Spusťte sadu Visual Studio a vytvořte nový **webová aplikace ASP.NET (.NET Framework)** projektu.</span><span class="sxs-lookup"><span data-stu-id="e1d93-132">Start Visual Studio and create a new **ASP.NET Web Application (.NET Framework)** project.</span></span>
2. <span data-ttu-id="e1d93-133">V **nová webová aplikace ASP.NET** dialogové okno, vyberte **prázdný** šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="e1d93-133">In the **New ASP.NET Web Application** dialog box, select the **Empty** project template.</span></span> <span data-ttu-id="e1d93-134">V části **přidat složky a základní odkazy pro**, vyberte **webového rozhraní API** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="e1d93-134">Under **Add folders and core references for**, select the **Web API** checkbox.</span></span>

   ![ASP.NET dialogové okno nového projektu v sadě Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. <span data-ttu-id="e1d93-136">Přidat kontroler Web API s názvem `TestController` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e1d93-136">Add a Web API controller named `TestController` with the following code:</span></span>

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. <span data-ttu-id="e1d93-137">Můžete spustit aplikaci místně nebo nasazení do Azure.</span><span class="sxs-lookup"><span data-stu-id="e1d93-137">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="e1d93-138">(Pro snímky obrazovky v tomto kurzu, aplikace nasadí do Azure App Service Web Apps.) Pokud chcete ověřit, že je pracovní webové rozhraní API, přejděte na `http://hostname/api/test/`, kde *název hostitele* je doména, kam jste nasadili aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e1d93-138">(For the screenshots in this tutorial, the app deploys to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="e1d93-139">Měli byste vidět text odpovědi &quot;získat: testovací zpráva&quot;.</span><span class="sxs-lookup"><span data-stu-id="e1d93-139">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

   ![Webové prohlížeče zobrazující zkušební zprávy](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a><span data-ttu-id="e1d93-141">Vytvoření projektu WebClient</span><span class="sxs-lookup"><span data-stu-id="e1d93-141">Create the WebClient project</span></span>

1. <span data-ttu-id="e1d93-142">Vytvořte další **webová aplikace ASP.NET (.NET Framework)** projektu a vyberte **MVC** šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="e1d93-142">Create another **ASP.NET Web Application (.NET Framework)** project and select the **MVC** project template.</span></span> <span data-ttu-id="e1d93-143">Volitelně vyberte **změna ověřování** > **bez ověřování**.</span><span class="sxs-lookup"><span data-stu-id="e1d93-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="e1d93-144">Pro účely tohoto kurzu nepotřebujete ověřování.</span><span class="sxs-lookup"><span data-stu-id="e1d93-144">You don't need authentication for this tutorial.</span></span>

   ![Šablona MVC v dialogu Nový projekt ASP.NET v sadě Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. <span data-ttu-id="e1d93-146">V **Průzkumníka řešení**, otevřete soubor *Views/Home/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e1d93-146">In **Solution Explorer**, open the file *Views/Home/Index.cshtml*.</span></span> <span data-ttu-id="e1d93-147">Nahraďte kód v tomto souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e1d93-147">Replace the code in this file with the following:</span></span>

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   <span data-ttu-id="e1d93-148">Pro *serviceUrl* proměnné, použijte identifikátor URI aplikace webové služby.</span><span class="sxs-lookup"><span data-stu-id="e1d93-148">For the *serviceUrl* variable, use the URI of the WebService app.</span></span>

3. <span data-ttu-id="e1d93-149">Místní spuštění aplikace WebClient nebo ji publikovat na jiný web.</span><span class="sxs-lookup"><span data-stu-id="e1d93-149">Run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="e1d93-150">Po kliknutí na tlačítko "Vyzkoušet" požadavek AJAX se odešle do aplikace webové služby pomocí protokolu HTTP metody uvedené v rozevíracím seznamu (GET, POST a PUT).</span><span class="sxs-lookup"><span data-stu-id="e1d93-150">When you click the "Try It" button, an AJAX request is submitted to the WebService app using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="e1d93-151">To umožňuje prozkoumat různé požadavky cross-origin.</span><span class="sxs-lookup"><span data-stu-id="e1d93-151">This lets you examine different cross-origin requests.</span></span> <span data-ttu-id="e1d93-152">Aplikace webové služby v současné době nepodporuje CORS, takže pokud kliknete na tlačítko, zobrazí se vám chyba.</span><span class="sxs-lookup"><span data-stu-id="e1d93-152">Currently, the WebService app does not support CORS, so if you click the button you'll get an error.</span></span>

![Chyba "Zkuste to" v prohlížeči](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="e1d93-154">Pokud sledujete přenos pomocí protokolu HTTP v nástroji, jako jsou [Fiddler](http://www.telerik.com/fiddler), uvidíte, že do prohlížeče odeslat požadavek na získání a úspěšného vykonání požadavku, ale volání jazyka AJAX vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="e1d93-154">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you'll see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="e1d93-155">Je důležité pochopit, že zásada stejného zdroje nezabraňuje prohlížeče z *odesílání* požadavku.</span><span class="sxs-lookup"><span data-stu-id="e1d93-155">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="e1d93-156">Místo toho brání aplikaci v zobrazení *odpovědi*.</span><span class="sxs-lookup"><span data-stu-id="e1d93-156">Instead, it prevents the application from seeing the *response*.</span></span>

![Ladicí program webové aplikaci Fiddler zobrazují webové požadavky](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a><span data-ttu-id="e1d93-158">Povolení CORS</span><span class="sxs-lookup"><span data-stu-id="e1d93-158">Enable CORS</span></span>

<span data-ttu-id="e1d93-159">Nyní Pojďme povolení CORS v app webové služby.</span><span class="sxs-lookup"><span data-stu-id="e1d93-159">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="e1d93-160">Nejprve přidejte balíček CORS NuGet.</span><span class="sxs-lookup"><span data-stu-id="e1d93-160">First, add the CORS NuGet package.</span></span> <span data-ttu-id="e1d93-161">V aplikaci Visual Studio z **nástroje** příkaz **Správce balíčků NuGet**, vyberte **konzoly Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="e1d93-161">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="e1d93-162">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e1d93-162">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="e1d93-163">Tento příkaz nainstaluje nejnovější balíček a aktualizuje všechny závislosti, včetně základní webové rozhraní API knihovny.</span><span class="sxs-lookup"><span data-stu-id="e1d93-163">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="e1d93-164">Použití `-Version` příznak pro cílení na konkrétní verzi.</span><span class="sxs-lookup"><span data-stu-id="e1d93-164">Use the `-Version` flag to target a specific version.</span></span> <span data-ttu-id="e1d93-165">Vyžaduje balíček CORS webového rozhraní API 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e1d93-165">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="e1d93-166">Otevřete soubor *aplikace\_Start/WebApiConfig.cs*.</span><span class="sxs-lookup"><span data-stu-id="e1d93-166">Open the file *App\_Start/WebApiConfig.cs*.</span></span> <span data-ttu-id="e1d93-167">Přidejte následující kód, který **WebApiConfig.Register** metody:</span><span class="sxs-lookup"><span data-stu-id="e1d93-167">Add the following code to the **WebApiConfig.Register** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="e1d93-168">Dále přidejte **[EnableCors]** atribut `TestController` třídy:</span><span class="sxs-lookup"><span data-stu-id="e1d93-168">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="e1d93-169">Pro *zdroje* parametrů, použijte identifikátor URI, kam jste nasadili webový klient aplikace.</span><span class="sxs-lookup"><span data-stu-id="e1d93-169">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="e1d93-170">Díky tomu požadavky cross-origin z WebClient, při stále Probíhá zakazování všech ostatních požadavků mezi doménami.</span><span class="sxs-lookup"><span data-stu-id="e1d93-170">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="e1d93-171">Později budete I popisují parametry pro **[EnableCors]** podrobněji.</span><span class="sxs-lookup"><span data-stu-id="e1d93-171">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="e1d93-172">Nezahrnují dopředné lomítko na konci *zdroje* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="e1d93-172">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="e1d93-173">Znovu nasaďte aktualizovanou aplikaci webové služby.</span><span class="sxs-lookup"><span data-stu-id="e1d93-173">Redeploy the updated WebService application.</span></span> <span data-ttu-id="e1d93-174">Není nutné aktualizovat WebClient.</span><span class="sxs-lookup"><span data-stu-id="e1d93-174">You don't need to update WebClient.</span></span> <span data-ttu-id="e1d93-175">Nyní požadavek AJAX z WebClient uspěli.</span><span class="sxs-lookup"><span data-stu-id="e1d93-175">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="e1d93-176">Metody GET a PUT, POST všechny povolené.</span><span class="sxs-lookup"><span data-stu-id="e1d93-176">The GET, PUT, and POST methods are all allowed.</span></span>

![Webové prohlížeče zobrazující úspěšné testovací zpráva](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a><span data-ttu-id="e1d93-178">Jak funguje CORS</span><span class="sxs-lookup"><span data-stu-id="e1d93-178">How CORS Works</span></span>

<span data-ttu-id="e1d93-179">Tato část popisuje, co se stane, že v požadavku CORS, na úrovni zprávy HTTP.</span><span class="sxs-lookup"><span data-stu-id="e1d93-179">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="e1d93-180">Je důležité pochopit, jak funguje CORS, takže můžete nakonfigurovat **[EnableCors]** atribut správně a odstraňování potíží, pokud věci nefungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="e1d93-180">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="e1d93-181">Specifikace CORS zavádí několik nové hlavičky protokolu HTTP, které umožňují požadavky cross-origin.</span><span class="sxs-lookup"><span data-stu-id="e1d93-181">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="e1d93-182">Pokud je prohlížeč podporuje CORS, nastaví tyto hlavičky automaticky pro požadavky cross-origin; nemusíte dělat nic zvláštního v kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e1d93-182">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="e1d93-183">Tady je příklad žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="e1d93-183">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="e1d93-184">Hlavička "Origin" dává domény, lokality, která odeslala žádost.</span><span class="sxs-lookup"><span data-stu-id="e1d93-184">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="e1d93-185">Pokud server umožňuje, aby žádosti, nastaví hlavičky Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="e1d93-185">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="e1d93-186">Hodnotu této hlavičky odpovídá hlavička počátečního nebo je hodnota zástupného znaku "\*", což znamená, že jakýkoli původ je povolen.</span><span class="sxs-lookup"><span data-stu-id="e1d93-186">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="e1d93-187">Pokud odpověď neobsahuje hlavičky Access-Control-Allow-Origin, požadavek AJAX selže.</span><span class="sxs-lookup"><span data-stu-id="e1d93-187">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="e1d93-188">Konkrétně v prohlížeči zakazuje požadavku.</span><span class="sxs-lookup"><span data-stu-id="e1d93-188">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="e1d93-189">I v případě, že server vrátí úspěšné odpovědi, prohlížeč není zpřístupnit odpověď do klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="e1d93-189">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="e1d93-190">**Předběžných požadavků**</span><span class="sxs-lookup"><span data-stu-id="e1d93-190">**Preflight Requests**</span></span>

<span data-ttu-id="e1d93-191">U některých požadavků CORS prohlížeč odešle požadavek další, nazývá "předběžný požadavek", předtím, než odešle skutečnou žádost pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="e1d93-191">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="e1d93-192">Prohlížeči můžete přeskočit předběžný požadavek, pokud jsou splněny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="e1d93-192">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="e1d93-193">Metoda žádosti je GET, HEAD nebo POST, *a*</span><span class="sxs-lookup"><span data-stu-id="e1d93-193">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="e1d93-194">Aplikace nemá nastaven záhlaví požadavku než přijmout, Accept-Language, jazyka obsahu Content-Type nebo poslední-Event-ID, *a*</span><span class="sxs-lookup"><span data-stu-id="e1d93-194">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="e1d93-195">Hlavička Content-Type (Pokud nastavit) je jedním z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="e1d93-195">The Content-Type header (if set) is one of the following:</span></span>

    - <span data-ttu-id="e1d93-196">Application/x--www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="e1d93-196">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="e1d93-197">multipart/formuláře data</span><span class="sxs-lookup"><span data-stu-id="e1d93-197">multipart/form-data</span></span>
    - <span data-ttu-id="e1d93-198">text/plain</span><span class="sxs-lookup"><span data-stu-id="e1d93-198">text/plain</span></span>

<span data-ttu-id="e1d93-199">Pravidlo o hlavičky požadavku se vztahuje na hlavičky, které aplikace nastaví voláním **setRequestHeader** na **XMLHttpRequest** objektu.</span><span class="sxs-lookup"><span data-stu-id="e1d93-199">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="e1d93-200">(Specifikaci CORS volá tyto "Autor hlavičky žádosti".) Toto pravidlo se nevztahuje na záhlaví *prohlížeče* můžete nastavit, jako je například uživatelského agenta, hostitele nebo Content-Length.</span><span class="sxs-lookup"><span data-stu-id="e1d93-200">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="e1d93-201">Tady je příklad předběžný požadavek:</span><span class="sxs-lookup"><span data-stu-id="e1d93-201">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="e1d93-202">Přípravné požadavek používá metodu HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="e1d93-202">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="e1d93-203">Obsahuje dva speciálními záhlavími:</span><span class="sxs-lookup"><span data-stu-id="e1d93-203">It includes two special headers:</span></span>

- <span data-ttu-id="e1d93-204">Access-Control-Request-Method: Metoda HTTP, který se použije pro aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="e1d93-204">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="e1d93-205">Access-Control-Request-Headers: Seznam hlaviček požadavků, který *aplikace* nastavit u aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="e1d93-205">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="e1d93-206">(Znovu, nezahrnuje hlavičky, které nastaví prohlížeče.)</span><span class="sxs-lookup"><span data-stu-id="e1d93-206">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="e1d93-207">Tady je příklad odpovědi, za předpokladu, že server umožňuje žádosti:</span><span class="sxs-lookup"><span data-stu-id="e1d93-207">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="e1d93-208">Odpověď obsahuje hlavičku přístup – ovládací prvek-Allow-Methods, který obsahuje seznam povolených metod a volitelně hlavičky Access-Control-povolit-Headers, která zobrazuje povolené hlavičky.</span><span class="sxs-lookup"><span data-stu-id="e1d93-208">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="e1d93-209">Pokud je předběžný požadavek úspěšné, prohlížeč odesílá skutečnou žádost, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="e1d93-209">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="e1d93-210">Obor pravidla pro [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="e1d93-210">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="e1d93-211">CORS můžete povolit každou akci, na kontroler nebo globálně pro všechny kontrolery rozhraní Web API ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e1d93-211">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="e1d93-212">**Za akci**</span><span class="sxs-lookup"><span data-stu-id="e1d93-212">**Per Action**</span></span>

<span data-ttu-id="e1d93-213">Povolení CORS pro jednu akci, nastavte **[EnableCors]** atributu na metodu akce.</span><span class="sxs-lookup"><span data-stu-id="e1d93-213">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="e1d93-214">Následující příklad umožňuje použití CORS pro `GetItem` pouze metody.</span><span class="sxs-lookup"><span data-stu-id="e1d93-214">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="e1d93-215">**Na kontroler**</span><span class="sxs-lookup"><span data-stu-id="e1d93-215">**Per Controller**</span></span>

<span data-ttu-id="e1d93-216">Nastavíte-li **[EnableCors]** na třídy řadiče, vztahuje se na všechny akce v řadiči.</span><span class="sxs-lookup"><span data-stu-id="e1d93-216">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="e1d93-217">Zakázání CORS pro akci, přidat **[DisableCors]** atribut k akci.</span><span class="sxs-lookup"><span data-stu-id="e1d93-217">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="e1d93-218">Následující příklad umožňuje použití CORS pro každou metodu s výjimkou `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="e1d93-218">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="e1d93-219">**Globálně**</span><span class="sxs-lookup"><span data-stu-id="e1d93-219">**Globally**</span></span>

<span data-ttu-id="e1d93-220">Povolení CORS pro všechny řadiče webové rozhraní API ve vaší aplikaci, předejte **EnableCorsAttribute** instance na **EnableCors** metody:</span><span class="sxs-lookup"><span data-stu-id="e1d93-220">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="e1d93-221">Pokud jste nastavili atribut na více než jednoho oboru, je pořadí podle priority:</span><span class="sxs-lookup"><span data-stu-id="e1d93-221">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="e1d93-222">Akce</span><span class="sxs-lookup"><span data-stu-id="e1d93-222">Action</span></span>
2. <span data-ttu-id="e1d93-223">Kontroler</span><span class="sxs-lookup"><span data-stu-id="e1d93-223">Controller</span></span>
3. <span data-ttu-id="e1d93-224">Globální</span><span class="sxs-lookup"><span data-stu-id="e1d93-224">Global</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="e1d93-225">Nastavte povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="e1d93-225">Set the allowed origins</span></span>

<span data-ttu-id="e1d93-226">*Původu* parametr **[EnableCors]** atribut určuje původu, které jsou povoleny pro přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="e1d93-226">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="e1d93-227">Hodnota je čárkou oddělený seznam Povolené zdroje.</span><span class="sxs-lookup"><span data-stu-id="e1d93-227">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="e1d93-228">Můžete také použít zástupný znak hodnotu "\*" požadavky všechny původy.</span><span class="sxs-lookup"><span data-stu-id="e1d93-228">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="e1d93-229">Pečlivě zvažte předtím, než žádosti z původu.</span><span class="sxs-lookup"><span data-stu-id="e1d93-229">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="e1d93-230">To znamená, že doslova všechny weby mohl provádět volání AJAX do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e1d93-230">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="e1d93-231">Nastavte povolené metody HTTP</span><span class="sxs-lookup"><span data-stu-id="e1d93-231">Set the allowed HTTP methods</span></span>

<span data-ttu-id="e1d93-232">*Metody* parametr **[EnableCors]** atribut určuje, jaké metody HTTP jsou povoleny pro přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="e1d93-232">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="e1d93-233">Pokud chcete povolit všechny metody, použijte hodnotu zástupný znak "\*".</span><span class="sxs-lookup"><span data-stu-id="e1d93-233">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="e1d93-234">Následující příklad umožňuje pouze požadavky GET a POST.</span><span class="sxs-lookup"><span data-stu-id="e1d93-234">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="e1d93-235">Nastavit hlavičku povolené žádosti</span><span class="sxs-lookup"><span data-stu-id="e1d93-235">Set the allowed request headers</span></span>

<span data-ttu-id="e1d93-236">V tomto článku je popsáno dříve jak předběžný požadavek může obsahovat hlavičku Access-Control-Request-Headers, výpis hlavičky protokolu HTTP, nastavte aplikací (takzvaný ", vytvářet hlavičky žádosti").</span><span class="sxs-lookup"><span data-stu-id="e1d93-236">This article described earlier how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="e1d93-237">*Záhlaví* parametr **[EnableCors]** atribut určuje, které autor hlavičky požadavku jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="e1d93-237">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="e1d93-238">Chcete-li povolit všechny hlavičky, nastavte *záhlaví* na "\*".</span><span class="sxs-lookup"><span data-stu-id="e1d93-238">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="e1d93-239">Na seznamu povolených IP adres konkrétní záhlaví, nastavte *záhlaví* do seznamu Povolené hlavičky oddělené čárkami:</span><span class="sxs-lookup"><span data-stu-id="e1d93-239">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="e1d93-240">Prohlížeče však nejsou zcela konzistentní v tom, jak nastavují Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="e1d93-240">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="e1d93-241">Například Chrome aktuálně obsahuje "zdroj".</span><span class="sxs-lookup"><span data-stu-id="e1d93-241">For example, Chrome currently includes "origin".</span></span> <span data-ttu-id="e1d93-242">FireFox nezahrnuje hlavičky standardních, jako je například "Přijmout", i v případě, že aplikace nastaví ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="e1d93-242">FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="e1d93-243">Pokud nastavíte *záhlaví* pro nic jiného než "\*", měli byste zahrnout alespoň "přijímat", "content-type" a "zdroj" a navíc jakékoli vlastní hlavičky, které chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="e1d93-243">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="e1d93-244">Nastavit hlavičky odpovědi povolené</span><span class="sxs-lookup"><span data-stu-id="e1d93-244">Set the allowed response headers</span></span>

<span data-ttu-id="e1d93-245">Ve výchozím prohlížeči nezveřejňuje všechny hlavičky odpovědí do aplikace.</span><span class="sxs-lookup"><span data-stu-id="e1d93-245">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="e1d93-246">Hlavičky odpovědi, které jsou k dispozici ve výchozím nastavení jsou:</span><span class="sxs-lookup"><span data-stu-id="e1d93-246">The response headers that are available by default are:</span></span>

- <span data-ttu-id="e1d93-247">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="e1d93-247">Cache-Control</span></span>
- <span data-ttu-id="e1d93-248">Jazyk obsahu</span><span class="sxs-lookup"><span data-stu-id="e1d93-248">Content-Language</span></span>
- <span data-ttu-id="e1d93-249">Typ obsahu</span><span class="sxs-lookup"><span data-stu-id="e1d93-249">Content-Type</span></span>
- <span data-ttu-id="e1d93-250">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="e1d93-250">Expires</span></span>
- <span data-ttu-id="e1d93-251">Datum poslední změny</span><span class="sxs-lookup"><span data-stu-id="e1d93-251">Last-Modified</span></span>
- <span data-ttu-id="e1d93-252">Direktiva pragma</span><span class="sxs-lookup"><span data-stu-id="e1d93-252">Pragma</span></span>

<span data-ttu-id="e1d93-253">Specifikace CORS volá tyto [hlavičky odpovědi jednoduché](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="e1d93-253">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="e1d93-254">Chcete-li zpřístupnit další záhlaví aplikace, nastavte *exposedHeaders* parametr **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="e1d93-254">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="e1d93-255">V následujícím příkladu, kontroler společnosti `Get` metoda nastaví vlastní hlavičku s názvem "X-Custom-Header".</span><span class="sxs-lookup"><span data-stu-id="e1d93-255">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="e1d93-256">Ve výchozím nastavení nebude prohlížeč vystavit toto záhlaví v žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="e1d93-256">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="e1d93-257">Pokud chcete zpřístupnit záhlaví, patří "X-Custom-Header" v *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="e1d93-257">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a><span data-ttu-id="e1d93-258">Přihlašovací údaje předávat požadavky cross-origin</span><span class="sxs-lookup"><span data-stu-id="e1d93-258">Pass credentials in cross-origin requests</span></span>

<span data-ttu-id="e1d93-259">Přihlašovací údaje vyžadují speciální zacházení v požadavku CORS.</span><span class="sxs-lookup"><span data-stu-id="e1d93-259">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="e1d93-260">Ve výchozím prohlížeči neodešle žádné přihlašovací údaje se žádostí nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="e1d93-260">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="e1d93-261">Přihlašovací údaje zahrnují soubory cookie, jakož i schémat ověřování protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e1d93-261">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="e1d93-262">Odesílá pověření s žádostí nepůvodního zdroje, musíte nastavit klienta **XMLHttpRequest.withCredentials** na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="e1d93-262">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="e1d93-263">Pomocí **XMLHttpRequest** přímo:</span><span class="sxs-lookup"><span data-stu-id="e1d93-263">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="e1d93-264">V jQuery:</span><span class="sxs-lookup"><span data-stu-id="e1d93-264">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="e1d93-265">Na serveru navíc musíte povolit přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="e1d93-265">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="e1d93-266">Nastavení umožňující pověření původu mezi webové rozhraní API **SupportsCredentials** vlastnost na hodnotu true na **[EnableCors]** atribut:</span><span class="sxs-lookup"><span data-stu-id="e1d93-266">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="e1d93-267">Pokud je tato vlastnost hodnotu true, odpověď HTTP bude obsahovat hlavičku přístup – ovládací prvek-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="e1d93-267">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="e1d93-268">Tato hlavička sděluje prohlížeči, že server umožňuje přihlašovací údaje pro žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="e1d93-268">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="e1d93-269">Pokud prohlížeč odesílá pověření, ale odpověď neobsahuje platnou hlavičku přístup – ovládací prvek-Allow-Credentials, prohlížeči nebude vystavení odpovědi do aplikace a požadavek AJAX selže.</span><span class="sxs-lookup"><span data-stu-id="e1d93-269">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="e1d93-270">Buďte opatrní při nastavení **SupportsCredentials** na hodnotu true, protože to znamená, že web v jiné doméně můžete poslat pověření přihlášeného uživatele webové rozhraní API jménem uživatele, aniž by uživatel znal.</span><span class="sxs-lookup"><span data-stu-id="e1d93-270">Be careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="e1d93-271">Specifikace CORS také uvádí nastavení *zdroje* k &quot; \* &quot; je neplatný Pokud **SupportsCredentials** má hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="e1d93-271">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

## <a name="custom-cors-policy-providers"></a><span data-ttu-id="e1d93-272">Vlastní zprostředkovatelé zásad CORS</span><span class="sxs-lookup"><span data-stu-id="e1d93-272">Custom CORS policy providers</span></span>

<span data-ttu-id="e1d93-273">**[EnableCors]** implementuje atribut **ICorsPolicyProvider** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e1d93-273">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="e1d93-274">Můžete zadat vlastní implementaci tak, že vytvoříte třídu, která je odvozena z **atribut** a implementuje **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="e1d93-274">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="e1d93-275">Nyní můžete použít atribut jakékoli místo, že by měla být umístěna **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="e1d93-275">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="e1d93-276">Vlastní zprostředkovatel zásad CORS například může číst nastavení z konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="e1d93-276">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="e1d93-277">Jako alternativu k použití atributů, můžete zaregistrovat **ICorsPolicyProviderFactory** objekt, který vytvoří **ICorsPolicyProvider** objekty.</span><span class="sxs-lookup"><span data-stu-id="e1d93-277">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="e1d93-278">Chcete-li nastavit **ICorsPolicyProviderFactory**, volání **SetCorsPolicyProviderFactory** rozšiřující metoda při spuštění, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e1d93-278">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a><span data-ttu-id="e1d93-279">Podpora prohlížeče</span><span class="sxs-lookup"><span data-stu-id="e1d93-279">Browser support</span></span>

<span data-ttu-id="e1d93-280">Balíček CORS webového rozhraní API je technologie na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="e1d93-280">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="e1d93-281">Také musí podporovat CORS webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e1d93-281">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="e1d93-282">Naštěstí aktuální verze všech nejpoužívanějších prohlížečích obsahují [podporu CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="e1d93-282">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>
