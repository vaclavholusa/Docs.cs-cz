---
title: Co je nového v technologii ASP.NET Core 2.1
author: isaac2004
description: Další informace o nových funkcích v ASP.NET Core 2.1.
manager: wpickett
monikerRange: = aspnetcore-2.1
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: aspnetcore-2.1
ms.openlocfilehash: 97c9f3d82264715358591a656dc4c1c5975b6113
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/02/2018
ms.locfileid: "34728944"
---
# <a name="whats-new-in-aspnet-core-21"></a><span data-ttu-id="73fbe-103">Co je nového v technologii ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="73fbe-103">What's new in ASP.NET Core 2.1</span></span>

<span data-ttu-id="73fbe-104">V tomto článku klade důraz nejvýznamnějších změn v ASP.NET Core 2.1, s odkazy na příslušné dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="73fbe-104">This article highlights the most significant changes in ASP.NET Core 2.1, with links to relevant documentation.</span></span>

## <a name="signalr"></a><span data-ttu-id="73fbe-105">SignalR</span><span class="sxs-lookup"><span data-stu-id="73fbe-105">SignalR</span></span>

<span data-ttu-id="73fbe-106">SignalR jsou přepsána pro ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="73fbe-106">SignalR has been rewritten for ASP.NET Core 2.1.</span></span> <span data-ttu-id="73fbe-107">Jádro ASP.NET SignalR zahrnuje množství vylepšení:</span><span class="sxs-lookup"><span data-stu-id="73fbe-107">ASP.NET Core SignalR includes a number of improvements:</span></span>

* <span data-ttu-id="73fbe-108">Zjednodušený model Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="73fbe-108">A simplified scale-out model.</span></span>
* <span data-ttu-id="73fbe-109">Nový klient JavaScript se žádné jQuery závislostí.</span><span class="sxs-lookup"><span data-stu-id="73fbe-109">A new JavaScript client with no jQuery dependency.</span></span>
* <span data-ttu-id="73fbe-110">Nový kompaktní binární protokol podle MessagePack.</span><span class="sxs-lookup"><span data-stu-id="73fbe-110">A new compact binary protocol based on MessagePack.</span></span>
* <span data-ttu-id="73fbe-111">Podpora pro vlastní protokoly.</span><span class="sxs-lookup"><span data-stu-id="73fbe-111">Support for custom protocols.</span></span>
* <span data-ttu-id="73fbe-112">Nový model datových proudů odpovědi.</span><span class="sxs-lookup"><span data-stu-id="73fbe-112">A new streaming response model.</span></span>
* <span data-ttu-id="73fbe-113">Podpora pro klienty založené na úplné objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="73fbe-113">Support for clients based on bare WebSockets.</span></span>

<span data-ttu-id="73fbe-114">Další informace najdete v tématu [ASP.NET Core SignalR](xref:signalr/index).</span><span class="sxs-lookup"><span data-stu-id="73fbe-114">For more information, see [ASP.NET Core SignalR](xref:signalr/index).</span></span>

## <a name="razor-class-libraries"></a><span data-ttu-id="73fbe-115">Knihovny tříd Razor</span><span class="sxs-lookup"><span data-stu-id="73fbe-115">Razor class libraries</span></span>

<span data-ttu-id="73fbe-116">ASP.NET Core 2.1 usnadňuje sestavení a zahrnout Razor na základě uživatelského rozhraní v knihovně a sdílet mezi více projektů.</span><span class="sxs-lookup"><span data-stu-id="73fbe-116">ASP.NET Core 2.1 makes it easier to build and include Razor-based UI in a library and share it across multiple projects.</span></span> <span data-ttu-id="73fbe-117">Nové Razor SDK umožňuje vytvářet soubory Razor do projektu knihovny tříd, které můžete zabalené do balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="73fbe-117">The new Razor SDK enables building Razor files into a class library project that can be packaged into a NuGet package.</span></span> <span data-ttu-id="73fbe-118">Zobrazení a stránky v knihovnách se automaticky zjistí a aplikace se dá přepsat.</span><span class="sxs-lookup"><span data-stu-id="73fbe-118">Views and pages in libraries are automatically discovered and can be overridden by the app.</span></span> <span data-ttu-id="73fbe-119">Díky integraci Razor kompilace do sestavení:</span><span class="sxs-lookup"><span data-stu-id="73fbe-119">By integrating Razor compilation into the build:</span></span>

* <span data-ttu-id="73fbe-120">Doba spuštění aplikace je výrazně rychlejší.</span><span class="sxs-lookup"><span data-stu-id="73fbe-120">The app startup time is significantly faster.</span></span>
* <span data-ttu-id="73fbe-121">Rychlé aktualizace zobrazení syntaxe Razor a stránky v době běhu jsou stále k dispozici jako součást pracovního postupu iterativní vývoj.</span><span class="sxs-lookup"><span data-stu-id="73fbe-121">Fast updates to Razor views and pages at runtime are still available as part of an iterative development workflow.</span></span>

<span data-ttu-id="73fbe-122">Další informace najdete v tématu [vytvářet opakovaně použitelné uživatelské rozhraní pomocí projektu knihovny tříd Razor](xref:mvc/razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="73fbe-122">For more information, see [Create reusable UI using the Razor Class Library project](xref:mvc/razor-pages/ui-class).</span></span>

## <a name="identity-ui-library--scaffolding"></a><span data-ttu-id="73fbe-123">Knihovna uživatelského rozhraní identity & generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="73fbe-123">Identity UI library & scaffolding</span></span>

<span data-ttu-id="73fbe-124">Poskytuje ASP.NET Core 2.1 [ASP.NET Core Identity](xref:security/authentication/identity) jako [knihovny tříd Razor](xref:mvc/razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="73fbe-124">ASP.NET Core 2.1 provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:mvc/razor-pages/ui-class).</span></span> <span data-ttu-id="73fbe-125">Aplikace, které zahrnují Identity můžete použít nové Identity scaffolder selektivně přidat zdrojový kód obsažené v Identity Razor třídu knihovny (RCL).</span><span class="sxs-lookup"><span data-stu-id="73fbe-125">Apps that include Identity can apply the new Identity scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="73fbe-126">Můžete chtít generovat zdrojový kód, abyste mohli upravit kód a změnit chování.</span><span class="sxs-lookup"><span data-stu-id="73fbe-126">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="73fbe-127">Například může vyzvat scaffolder ke generování kódu použít v registraci.</span><span class="sxs-lookup"><span data-stu-id="73fbe-127">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="73fbe-128">Generovaného kódu mají přednost před stejný kód v Identity RCL.</span><span class="sxs-lookup"><span data-stu-id="73fbe-128">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="73fbe-129">Aplikace, které provádějí **není** zahrnují ověřování můžete použít scaffolder Identity k přidání balíčku RCL Identity.</span><span class="sxs-lookup"><span data-stu-id="73fbe-129">Apps that do **not** include authentication can apply the Identity scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="73fbe-130">Máte možnost výběru Identity kódu má být vygenerován.</span><span class="sxs-lookup"><span data-stu-id="73fbe-130">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="73fbe-131">Další informace najdete v tématu [Identity vygenerované uživatelské rozhraní v projektů ASP.NET Core](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="73fbe-131">For more information, see [Scaffold Identity in ASP.NET Core projects](xref:security/authentication/scaffold-identity).</span></span>

## <a name="https"></a><span data-ttu-id="73fbe-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="73fbe-132">HTTPS</span></span>

<span data-ttu-id="73fbe-133">S vyšší zaměřit na zabezpečení a ochrana osobních údajů je důležité povolit HTTPS pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="73fbe-133">With the increased focus on security and privacy, enabling HTTPS for web apps is important.</span></span> <span data-ttu-id="73fbe-134">Vynucení HTTPS se stává stále striktní na webu.</span><span class="sxs-lookup"><span data-stu-id="73fbe-134">HTTPS enforcement is becoming increasingly strict on the web.</span></span> <span data-ttu-id="73fbe-135">Servery, které nepoužívají HTTPS, jsou považovány za nezabezpečené.</span><span class="sxs-lookup"><span data-stu-id="73fbe-135">Sites that don’t use HTTPS are considered insecure.</span></span> <span data-ttu-id="73fbe-136">K vynucení, že webových funkcí produktu musí používat z kontextu zabezpečení jsou spouštění prohlížeče (chromu, Mozilla).</span><span class="sxs-lookup"><span data-stu-id="73fbe-136">Browsers (Chromium, Mozilla) are starting to enforce that web features must be used from a secure context.</span></span> <span data-ttu-id="73fbe-137">[GDPR](xref:security/gdpr) vyžaduje použití protokolu HTTPS k ochrany osobních údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="73fbe-137">[GDPR](xref:security/gdpr) requires the use of HTTPS to protect user privacy.</span></span> <span data-ttu-id="73fbe-138">Při použití protokolu HTTPS v produkčním prostředí je rozhodující, pomocí protokolu HTTPS v vývoj pomáhají zabránit problémům v nasazení (například nezabezpečené odkazy).</span><span class="sxs-lookup"><span data-stu-id="73fbe-138">While using HTTPS in production is critical, using HTTPS in development can help prevent issues in deployment (for example, insecure links).</span></span> <span data-ttu-id="73fbe-139">ASP.NET Core 2.1 zahrnuje množství vylepšení, které usnadňují používání protokolu HTTPS v vývoj a konfigurace protokolu HTTPS v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="73fbe-139">ASP.NET Core 2.1 includes a number of improvements that make it easier to use HTTPS in development and to configure HTTPS in production.</span></span> <span data-ttu-id="73fbe-140">Další informace najdete v tématu [vynutit HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="73fbe-140">For more information, see [Enforce HTTPS](xref:security/enforcing-ssl).</span></span>

### <a name="on-by-default"></a><span data-ttu-id="73fbe-141">Na ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="73fbe-141">On by default</span></span>

<span data-ttu-id="73fbe-142">Usnadňuje vývoj zabezpečeného webu HTTPS nyní ve výchozím nastavení zapnutá.</span><span class="sxs-lookup"><span data-stu-id="73fbe-142">To facilitate secure website development, HTTPS  is now enabled by default.</span></span> <span data-ttu-id="73fbe-143">Od verze 2.1, Kestrel naslouchá na `https://localhost:5001` při místní vývojový certifikát nachází.</span><span class="sxs-lookup"><span data-stu-id="73fbe-143">Starting in 2.1, Kestrel listens on `https://localhost:5001` when a local development certificate is present.</span></span> <span data-ttu-id="73fbe-144">Vývojový certifikát se vytvoří:</span><span class="sxs-lookup"><span data-stu-id="73fbe-144">A development certificate is created:</span></span>

* <span data-ttu-id="73fbe-145">V rámci prostředí prvního spuštění .NET Core SDK, při použití sady SDK pro první.</span><span class="sxs-lookup"><span data-stu-id="73fbe-145">As part of the .NET Core SDK first-run experience, when you use the SDK for the first time.</span></span>
* <span data-ttu-id="73fbe-146">Ručně pomocí nové `dev-certs` nástroj.</span><span class="sxs-lookup"><span data-stu-id="73fbe-146">Manually using the new `dev-certs` tool.</span></span>

<span data-ttu-id="73fbe-147">Spustit `dotnet dev-certs https --trust` certifikátu důvěřovat.</span><span class="sxs-lookup"><span data-stu-id="73fbe-147">Run `dotnet dev-certs https --trust` to trust the certificate.</span></span>

### <a name="https-redirection-and-enforcement"></a><span data-ttu-id="73fbe-148">Přesměrování protokolu HTTPS a vynucení</span><span class="sxs-lookup"><span data-stu-id="73fbe-148">HTTPS redirection and enforcement</span></span>

<span data-ttu-id="73fbe-149">Webové aplikace obvykle nutné naslouchání protokolu HTTP a HTTPS, ale pak přesměruje veškerý provoz protokolu HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="73fbe-149">Web apps typically need to listen on both HTTP and HTTPS, but then redirect all HTTP traffic to HTTPS.</span></span> <span data-ttu-id="73fbe-150">V 2.1 specializované middleware přesměrování protokolu HTTPS, který inteligentně přesměruje na základě přítomnosti konfigurace nebo je zavedený porty vázané serveru.</span><span class="sxs-lookup"><span data-stu-id="73fbe-150">In 2.1, specialized HTTPS redirection middleware that intelligently redirects based on the presence of configuration or bound server ports has been introduced.</span></span>

<span data-ttu-id="73fbe-151">Použití protokolu HTTPS lze dále vynutit pomocí [HTTP striktní přenosu zabezpečení protokolu (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="73fbe-151">Use of HTTPS can be further enforced using [HTTP Strict Transport Security Protocol (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="73fbe-152">HSTS dá pokyn prohlížeče vždy přístup k webu přes HTTPS.</span><span class="sxs-lookup"><span data-stu-id="73fbe-152">HSTS instructs browsers to always access the site via HTTPS.</span></span> <span data-ttu-id="73fbe-153">ASP.NET Core 2.1 přidá HSTS middleware, který podporuje možnosti pro maximální stáří, subdomény, a HSTS přednačtení seznamu.</span><span class="sxs-lookup"><span data-stu-id="73fbe-153">ASP.NET Core 2.1 adds HSTS middleware that supports options for max age, subdomains, and the HSTS preload list.</span></span>

### <a name="configuration-for-production"></a><span data-ttu-id="73fbe-154">Konfigurace pro produkční prostředí</span><span class="sxs-lookup"><span data-stu-id="73fbe-154">Configuration for production</span></span>

<span data-ttu-id="73fbe-155">V produkčním prostředí musí být explicitně nakonfigurovaný protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="73fbe-155">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="73fbe-156">V 2.1 výchozí schéma konfigurace pro konfiguraci protokolu HTTPS pro Kestrel přidala.</span><span class="sxs-lookup"><span data-stu-id="73fbe-156">In 2.1, default configuration schema for configuring HTTPS for Kestrel has been added.</span></span> <span data-ttu-id="73fbe-157">Aplikace může být nakonfigurován k používání:</span><span class="sxs-lookup"><span data-stu-id="73fbe-157">Apps can be configured to use:</span></span>

* <span data-ttu-id="73fbe-158">Víc koncových bodů včetně adres URL.</span><span class="sxs-lookup"><span data-stu-id="73fbe-158">Multiple endpoints including the URLs.</span></span> <span data-ttu-id="73fbe-159">Další informace najdete v tématu [Kestrel webového serveru implementace: konfigurace koncového bodu](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="73fbe-159">For more information, see [Kestrel web server implementation: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
* <span data-ttu-id="73fbe-160">Certifikát, který se má použít pro protokol HTTPS ze souboru na disku nebo z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="73fbe-160">The certificate to use for HTTPS either from a file on disk or from a certificate store.</span></span>

## <a name="gdpr"></a><span data-ttu-id="73fbe-161">GDPR</span><span class="sxs-lookup"><span data-stu-id="73fbe-161">GDPR</span></span>

<span data-ttu-id="73fbe-162">Základní technologie ASP.NET poskytuje rozhraní API a šablony, které vám pomůžou splňovat některé [Evropa obecné Data Protection nařízení (GDPR)](https://www.eugdpr.org/) požadavky.</span><span class="sxs-lookup"><span data-stu-id="73fbe-162">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements.</span></span> <span data-ttu-id="73fbe-163">Další informace najdete v tématu [GDPR podporovat v ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="73fbe-163">For more information, see [GDPR support in ASP.NET Core](xref:security/gdpr).</span></span> <span data-ttu-id="73fbe-164">A [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ukazuje způsob použití a umožňuje testovat většinu GDPR rozšíření bodů a přidat do šablony ASP.NET Core 2.1 rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="73fbe-164">A [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) shows how to use and lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span>

## <a name="integration-tests"></a><span data-ttu-id="73fbe-165">Integrace testů</span><span class="sxs-lookup"><span data-stu-id="73fbe-165">Integration tests</span></span>

<span data-ttu-id="73fbe-166">Nový balíček uvádíme, že test zjednodušuje vytváření a spouštění.</span><span class="sxs-lookup"><span data-stu-id="73fbe-166">A new package is introduced that streamlines test creation and execution.</span></span> <span data-ttu-id="73fbe-167">[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) balíček zpracovává následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="73fbe-167">The [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) package handles the following tasks:</span></span>

* <span data-ttu-id="73fbe-168">Zkopíruje soubor závislosti (*\*.deps*) z otestované aplikace do testovacího projektu *bin* složky.</span><span class="sxs-lookup"><span data-stu-id="73fbe-168">Copies the dependency file (*\*.deps*) from the tested app into the test project's *bin* folder.</span></span>
* <span data-ttu-id="73fbe-169">Nastaví kořenu obsahu na kořenového projektu otestované aplikace, aby statické soubory a stránek/zobrazení se našly při provádění testů.</span><span class="sxs-lookup"><span data-stu-id="73fbe-169">Sets the content root to the tested app's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="73fbe-170">Poskytuje [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) třída pro zjednodušení zavádění otestované aplikace s [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span><span class="sxs-lookup"><span data-stu-id="73fbe-170">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the tested app with [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span>

<span data-ttu-id="73fbe-171">Tento test používá [xUnit](https://xunit.github.io/) zkontrolujte, že indexovou stránku načte se stavovým kódem úspěch a s správnou hlavičku Content-Type:</span><span class="sxs-lookup"><span data-stu-id="73fbe-171">The following test uses [xUnit](https://xunit.github.io/) to check that the Index page loads with a success status code and with the correct Content-Type header:</span></span>

```csharp
public class BasicTests
    : IClassFixture<WebApplicationFactory<RazorPagesProject.Startup>>
{
    private readonly HttpClient _client;

    public BasicTests(WebApplicationFactory<RazorPagesProject.Startup> factory)
    {
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task GetHomePage()
    {
        // Act
        var response = await _client.GetAsync("/");

        // Assert
        response.EnsureSuccessStatusCode(); // Status Code 200-299
        Assert.Equal("text/html; charset=utf-8",
            response.Content.Headers.ContentType.ToString());
    }
}
```

<span data-ttu-id="73fbe-172">Další informace najdete v tématu [integrace testy](xref:test/integration-tests) tématu.</span><span class="sxs-lookup"><span data-stu-id="73fbe-172">For more information, see the [Integration tests](xref:test/integration-tests) topic.</span></span>

## <a name="apicontroller-actionresult"></a><span data-ttu-id="73fbe-173">[Objektu ApiController], ActionResult</span><span class="sxs-lookup"><span data-stu-id="73fbe-173">[ApiController], ActionResult</span></span>

<span data-ttu-id="73fbe-174">ASP.NET Core 2.1 přidá nový programovací konvence, které usnadňují sestavení vyčištění a popisný webových rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="73fbe-174">ASP.NET Core 2.1 adds new programming conventions that make it easier to build clean and descriptive web APIs.</span></span> <span data-ttu-id="73fbe-175">`ActionResult<T>` je k přidání nového typu umožňuje aplikacím vracet typ odpovědi nebo jiné akce výsledek (podobně jako IActionResult), při stále označující typ odpovědi.</span><span class="sxs-lookup"><span data-stu-id="73fbe-175">`ActionResult<T>` is a new type added to allow an app to return either a response type or any other action result (similar to IActionResult), while still indicating the response type.</span></span> <span data-ttu-id="73fbe-176">`[ApiController]` Atribut přidala také jako způsob, jak můžete vyjádřit výslovný souhlas konvence specifické pro webové rozhraní API a chování.</span><span class="sxs-lookup"><span data-stu-id="73fbe-176">The `[ApiController]` attribute has also been added as the way to opt in to Web API-specific conventions and behaviors.</span></span>

<span data-ttu-id="73fbe-177">Další informace najdete v tématu [sestavení webového rozhraní API pomocí ASP.NET Core](xref:web-api/index).</span><span class="sxs-lookup"><span data-stu-id="73fbe-177">For more information, see [Build Web APIs with ASP.NET Core](xref:web-api/index).</span></span>

## <a name="ihttpclientfactory"></a><span data-ttu-id="73fbe-178">IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="73fbe-178">IHttpClientFactory</span></span>

<span data-ttu-id="73fbe-179">ASP.NET Core 2.1 obsahuje novou `IHttpClientFactory` služba, která usnadňuje konfigurace a využívat instancí `HttpClient` v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="73fbe-179">ASP.NET Core 2.1 includes a new `IHttpClientFactory` service that makes it easier to configure and consume instances of `HttpClient` in apps.</span></span> <span data-ttu-id="73fbe-180">`HttpClient` již obsahuje koncept delegování obslužných rutin, které může být propojena pro odchozí požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="73fbe-180">`HttpClient` already has the concept of delegating handlers that could be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="73fbe-181">Objekt factory:</span><span class="sxs-lookup"><span data-stu-id="73fbe-181">The factory:</span></span>

* <span data-ttu-id="73fbe-182">Díky registrace instancí `HttpClient` za intuitivnější pojmenované klienta.</span><span class="sxs-lookup"><span data-stu-id="73fbe-182">Makes registering of instances of `HttpClient` per named client more intuitive.</span></span>
* <span data-ttu-id="73fbe-183">Implementuje Polly obslužná rutina, která umožňuje Polly zásady, které se používají pro opakování, CircuitBreakers atd.</span><span class="sxs-lookup"><span data-stu-id="73fbe-183">Implements a Polly handler that allows Polly policies to be used for Retry, CircuitBreakers, etc.</span></span>

<span data-ttu-id="73fbe-184">Další informace najdete v tématu [zahájit požadavků HTTP](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="73fbe-184">For more information, see [Initiate HTTP Requests](xref:fundamentals/http-requests).</span></span>

## <a name="kestrel-transport-configuration"></a><span data-ttu-id="73fbe-185">Konfigurace kestrel přenosu</span><span class="sxs-lookup"><span data-stu-id="73fbe-185">Kestrel transport configuration</span></span>

<span data-ttu-id="73fbe-186">Ve verzi ASP.NET Core 2.1 Kestrel na výchozí přenos je už podle Libuv ale na spravované sokety.</span><span class="sxs-lookup"><span data-stu-id="73fbe-186">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="73fbe-187">Další informace najdete v tématu [Kestrel webového serveru implementace: Konfigurace přenosu](xref:fundamentals/servers/kestrel#transport-configuration).</span><span class="sxs-lookup"><span data-stu-id="73fbe-187">For more information, see [Kestrel web server implementation: Transport configuration](xref:fundamentals/servers/kestrel#transport-configuration).</span></span>

## <a name="generic-host-builder"></a><span data-ttu-id="73fbe-188">Tvůrce obecné hostitele</span><span class="sxs-lookup"><span data-stu-id="73fbe-188">Generic host builder</span></span>

<span data-ttu-id="73fbe-189">Obecný Tvůrce hostitele (`HostBuilder`) byla zavedena.</span><span class="sxs-lookup"><span data-stu-id="73fbe-189">The Generic Host Builder (`HostBuilder`) has been introduced.</span></span> <span data-ttu-id="73fbe-190">Tohoto Tvůrce lze použít pro aplikace, které nejsou zpracování požadavků HTTP (zasílání zpráv, úlohy na pozadí, atd.).</span><span class="sxs-lookup"><span data-stu-id="73fbe-190">This builder can be used for apps that don't process HTTP requests (Messaging, background tasks, etc.).</span></span>

<span data-ttu-id="73fbe-191">Další informace najdete v tématu [obecné hostitele .NET](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="73fbe-191">For more information, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

## <a name="updated-spa-templates"></a><span data-ttu-id="73fbe-192">Aktualizované šablony SPA</span><span class="sxs-lookup"><span data-stu-id="73fbe-192">Updated SPA templates</span></span>

<span data-ttu-id="73fbe-193">Šablony jedné stránky aplikace pro úhlová reagují, a reagují s Redux se aktualizují používat struktury standardní projektu a sestavení systémy pro každou platformu.</span><span class="sxs-lookup"><span data-stu-id="73fbe-193">The Single Page Application templates for Angular, React, and React with Redux are updated to use the standard project structures and build systems for each framework.</span></span>

<span data-ttu-id="73fbe-194">Úhlová šablona je založená na úhlová rozhraní příkazového řádku a reagují šablony jsou založeny na vytvořit aplikaci reagují.</span><span class="sxs-lookup"><span data-stu-id="73fbe-194">The Angular template is based on the Angular CLI, and the React templates are based on create-react-app.</span></span>
<span data-ttu-id="73fbe-195">Další informace najdete v tématu [šablony jedné stránky aplikace pomocí ASP.NET Core](xref:spa/index).</span><span class="sxs-lookup"><span data-stu-id="73fbe-195">For more information, see [Use the Single Page Application templates with ASP.NET Core](xref:spa/index).</span></span>

## <a name="razor-pages-search-for-razor-assets"></a><span data-ttu-id="73fbe-196">Stránky Razor vyhledejte prostředky Razor</span><span class="sxs-lookup"><span data-stu-id="73fbe-196">Razor Pages search for Razor assets</span></span>

<span data-ttu-id="73fbe-197">V 2.1 vyhledejte stránky Razor Razor prostředky (například rozložení a částečné.) v následujících adresářích v uvedeném pořadí:</span><span class="sxs-lookup"><span data-stu-id="73fbe-197">In 2.1, Razor Pages search for Razor assets (such as layouts and partials) in the following directories in the listed order:</span></span>

1. <span data-ttu-id="73fbe-198">Aktuální stránky složce.</span><span class="sxs-lookup"><span data-stu-id="73fbe-198">Current Pages folder.</span></span>
1. <span data-ttu-id="73fbe-199">*Řetězec/Pages/sdílené /*</span><span class="sxs-lookup"><span data-stu-id="73fbe-199">*/Pages/Shared/*</span></span>
1. <span data-ttu-id="73fbe-200">*/Views/sdílené /*</span><span class="sxs-lookup"><span data-stu-id="73fbe-200">*/Views/Shared/*</span></span>

## <a name="razor-pages-in-an-area"></a><span data-ttu-id="73fbe-201">Stránky Razor v oblasti.</span><span class="sxs-lookup"><span data-stu-id="73fbe-201">Razor Pages in an area</span></span>

<span data-ttu-id="73fbe-202">Teď podporují řešení stránky Razor [oblasti](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="73fbe-202">Razor Pages now support [areas](xref:mvc/controllers/areas).</span></span> <span data-ttu-id="73fbe-203">Pokud chcete zobrazit příklad oblastech, vytvořte novou webovou aplikaci stránky Razor s jednotlivých uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="73fbe-203">To see an example of areas, create a new Razor Pages web app with individual user accounts.</span></span> <span data-ttu-id="73fbe-204">Stránky Razor webové aplikace s jednotlivých uživatelských účtů zahrnuje */Areas/Identity/Pages*.</span><span class="sxs-lookup"><span data-stu-id="73fbe-204">A Razor Pages web app with individual user accounts includes */Areas/Identity/Pages*.</span></span>

## <a name="migrate-from-20-to-21"></a><span data-ttu-id="73fbe-205">Migrace z 2.0 na 2.1</span><span class="sxs-lookup"><span data-stu-id="73fbe-205">Migrate from 2.0 to 2.1</span></span>

<span data-ttu-id="73fbe-206">V tématu [migraci ze základní technologie ASP.NET 2.0 na 2.1](xref:migration/20_21).</span><span class="sxs-lookup"><span data-stu-id="73fbe-206">See [Migrate from ASP.NET Core 2.0 to 2.1](xref:migration/20_21).</span></span>

## <a name="additional-information"></a><span data-ttu-id="73fbe-207">Další informace</span><span class="sxs-lookup"><span data-stu-id="73fbe-207">Additional information</span></span>

<span data-ttu-id="73fbe-208">Úplný seznam změn, najdete v článku [poznámky k verzi ASP.NET Core 2.1](https://github.com/aspnet/Home/releases/tag/2.1.0).</span><span class="sxs-lookup"><span data-stu-id="73fbe-208">For the complete list of changes, see the [ASP.NET Core 2.1 Release Notes](https://github.com/aspnet/Home/releases/tag/2.1.0).</span></span>
