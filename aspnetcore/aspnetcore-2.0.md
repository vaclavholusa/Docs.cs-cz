---
title: Co je nového v technologii ASP.NET 2.0 jádra
author: rick-anderson
description: Další informace o nových funkcích v technologii ASP.NET 2.0 jádra.
manager: wpickett
ms.author: riande
ms.date: 07/10/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: aspnetcore-2.0
ms.openlocfilehash: b4ac500888ce134e8f4f0d4bf16efa4e95f24c15
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="whats-new-in-aspnet-core-20"></a><span data-ttu-id="34672-103">Co je nového v technologii ASP.NET 2.0 jádra</span><span class="sxs-lookup"><span data-stu-id="34672-103">What's new in ASP.NET Core 2.0</span></span>

<span data-ttu-id="34672-104">V tomto článku klade důraz nejvýznamnějších změn v aplikaci ASP.NET 2.0 jádra, s odkazy na příslušné dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="34672-104">This article highlights the most significant changes in ASP.NET Core 2.0, with links to relevant documentation.</span></span>

## <a name="razor-pages"></a><span data-ttu-id="34672-105">Stránky Razor</span><span class="sxs-lookup"><span data-stu-id="34672-105">Razor Pages</span></span>

<span data-ttu-id="34672-106">Stránky Razor je nová funkce rozhraní ASP.NET MVC jádra, která vytváří kódování zaměřené na stránce scénáře jednodušší a zvýšit produktivitu.</span><span class="sxs-lookup"><span data-stu-id="34672-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="34672-107">Další informace najdete v tématu Úvod a kurzu:</span><span class="sxs-lookup"><span data-stu-id="34672-107">For more information, see the introduction and tutorial:</span></span>

* [<span data-ttu-id="34672-108">Úvod do stránky Razor</span><span class="sxs-lookup"><span data-stu-id="34672-108">Introduction to Razor Pages</span></span>](xref:mvc/razor-pages/index)
* [<span data-ttu-id="34672-109">Začínáme se stránkami Razor</span><span class="sxs-lookup"><span data-stu-id="34672-109">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a><span data-ttu-id="34672-110">Metapackage ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34672-110">ASP.NET Core metapackage</span></span>

<span data-ttu-id="34672-111">Nové metapackage ASP.NET Core zahrnuje všechny balíčky provedené a nepodporuje týmy ASP.NET Core a Entity Framework Core spolu s jejich interní a 3. stran závislosti.</span><span class="sxs-lookup"><span data-stu-id="34672-111">A new ASP.NET Core metapackage includes all of the packages made and supported by the ASP.NET Core and Entity Framework Core teams, along with their internal and 3rd-party dependencies.</span></span> <span data-ttu-id="34672-112">Již nepotřebujete vyberte jednotlivé ASP.NET Core funkce balíčkem.</span><span class="sxs-lookup"><span data-stu-id="34672-112">You no longer need to choose individual ASP.NET Core features by package.</span></span> <span data-ttu-id="34672-113">Všechny funkce jsou součástí [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) balíčku.</span><span class="sxs-lookup"><span data-stu-id="34672-113">All features are included in the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package.</span></span> <span data-ttu-id="34672-114">Tento balíček použít výchozí šablony.</span><span class="sxs-lookup"><span data-stu-id="34672-114">The default templates use this package.</span></span>

<span data-ttu-id="34672-115">Další informace najdete v tématu [Microsoft.AspNetCore.All metapackage pro technologii ASP.NET 2.0 základní](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="34672-115">For more information, see [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0](xref:fundamentals/metapackage).</span></span>

## <a name="runtime-store"></a><span data-ttu-id="34672-116">Modul runtime úložiště</span><span class="sxs-lookup"><span data-stu-id="34672-116">Runtime Store</span></span>

<span data-ttu-id="34672-117">Aplikace, které používají `Microsoft.AspNetCore.All` metapackage automaticky využít výhod nové úložiště .NET Core Runtime.</span><span class="sxs-lookup"><span data-stu-id="34672-117">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the new .NET Core Runtime Store.</span></span> <span data-ttu-id="34672-118">Úložiště obsahuje všechny prostředky runtime potřebné ke spuštění aplikace ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="34672-118">The Store contains all the runtime assets needed to run ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="34672-119">Při použití `Microsoft.AspNetCore.All` metapackage žádné prostředky z odkazované balíčky ASP.NET Core NuGet nasazených s aplikací, protože se již nacházejí v cílovém systému.</span><span class="sxs-lookup"><span data-stu-id="34672-119">When you use the `Microsoft.AspNetCore.All` metapackage, no assets from the referenced ASP.NET Core NuGet packages are deployed with the application because they already reside on the target system.</span></span> <span data-ttu-id="34672-120">Prostředky v úložišti Runtime jsou také předkompilovaných ke zlepšení času spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="34672-120">The assets in the Runtime Store are also precompiled to improve application startup time.</span></span>

<span data-ttu-id="34672-121">Další informace najdete v tématu [Runtime úložiště](/dotnet/core/deploying/runtime-store)</span><span class="sxs-lookup"><span data-stu-id="34672-121">For more information, see [Runtime store](/dotnet/core/deploying/runtime-store)</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="34672-122">Rozhraní .NET standard 2.0</span><span class="sxs-lookup"><span data-stu-id="34672-122">.NET Standard 2.0</span></span>

<span data-ttu-id="34672-123">Balíčky technologii ASP.NET 2.0 základní cíle standardní rozhraní .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="34672-123">The ASP.NET Core 2.0 packages target .NET Standard 2.0.</span></span> <span data-ttu-id="34672-124">Balíčky mohou odkazovat další rozhraní .NET 2.0 standardní knihovny a jejich spuštěním na .NET Standard 2.0 kompatibilní implementace rozhraní .NET, včetně .NET Core 2.0 a .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="34672-124">The packages can be referenced by other .NET Standard 2.0 libraries, and they can run on .NET Standard 2.0-compliant implementations of .NET, including .NET Core 2.0 and .NET Framework 4.6.1.</span></span> 

<span data-ttu-id="34672-125">`Microsoft.AspNetCore.All` Metapackage cílem .NET Core 2.0, protože je určen pro použití s úložištěm modulu Runtime .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="34672-125">The `Microsoft.AspNetCore.All` metapackage targets .NET Core 2.0 only, because it's intended to be used with the .NET Core 2.0 Runtime Store.</span></span>

## <a name="configuration-update"></a><span data-ttu-id="34672-126">Aktualizace konfigurace</span><span class="sxs-lookup"><span data-stu-id="34672-126">Configuration update</span></span>

<span data-ttu-id="34672-127">`IConfiguration` Instance je přidat do kontejneru služby ve výchozím nastavení v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="34672-127">An `IConfiguration` instance is added to the services container by default in ASP.NET Core 2.0.</span></span> <span data-ttu-id="34672-128">`IConfiguration` v služby kontejneru usnadňuje aplikace k načtení hodnoty konfigurace z kontejneru.</span><span class="sxs-lookup"><span data-stu-id="34672-128">`IConfiguration` in the services container makes it easier for applications to retrieve configuration values from the container.</span></span>

<span data-ttu-id="34672-129">Informace o stavu plánované dokumentaci najdete v tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/3387).</span><span class="sxs-lookup"><span data-stu-id="34672-129">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3387).</span></span>

## <a name="logging-update"></a><span data-ttu-id="34672-130">Protokolování aktualizace</span><span class="sxs-lookup"><span data-stu-id="34672-130">Logging update</span></span>

<span data-ttu-id="34672-131">V aplikaci ASP.NET 2.0 jádra je ve výchozím nastavení protokolování součástí systému (DI) vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="34672-131">In ASP.NET Core 2.0, logging is incorporated into the dependency injection (DI) system by default.</span></span> <span data-ttu-id="34672-132">Můžete přidat zprostředkovatele a nakonfigurovat filtrování v *Program.cs* místo v souboru *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="34672-132">You add providers and configure filtering in the *Program.cs* file instead of in the *Startup.cs* file.</span></span> <span data-ttu-id="34672-133">A ve výchozím nastavení `ILoggerFactory` podporuje filtrování způsobem, který vám umožní používat jeden flexibilní přístup pro filtrování mezi zprostředkovatele i filtrování specifické pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="34672-133">And the default `ILoggerFactory` supports filtering in a way that lets you use one flexible approach for both cross-provider filtering and specific-provider filtering.</span></span>

<span data-ttu-id="34672-134">Další informace najdete v tématu [Úvod k protokolování](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="34672-134">For more information, see [Introduction to Logging](xref:fundamentals/logging/index).</span></span>

## <a name="authentication-update"></a><span data-ttu-id="34672-135">Aktualizace ověřování</span><span class="sxs-lookup"><span data-stu-id="34672-135">Authentication update</span></span>

<span data-ttu-id="34672-136">Nový model ověřování usnadňuje nakonfigurovat ověřování pro aplikaci pomocí DI.</span><span class="sxs-lookup"><span data-stu-id="34672-136">A new authentication model makes it easier to configure authentication for an application using DI.</span></span>

<span data-ttu-id="34672-137">Nové šablony jsou dostupné pro konfiguraci ověřování pro webové aplikace a webové rozhraní API pomocí [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="34672-137">New templates are available for configuring authentication for web apps and web APIs using [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).</span></span>

<span data-ttu-id="34672-138">Informace o stavu plánované dokumentaci najdete v tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/3054).</span><span class="sxs-lookup"><span data-stu-id="34672-138">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3054).</span></span>

## <a name="identity-update"></a><span data-ttu-id="34672-139">Aktualizace identity</span><span class="sxs-lookup"><span data-stu-id="34672-139">Identity update</span></span>

<span data-ttu-id="34672-140">Provedli jsme je snazší sestavovat zabezpečené webové rozhraní API pomocí Identity v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="34672-140">We've made it easier to build secure web APIs using Identity in ASP.NET Core 2.0.</span></span> <span data-ttu-id="34672-141">Můžete získat přístupové tokeny pro přístup k pomocí webového rozhraní API [Microsoft ověřování knihovny (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span><span class="sxs-lookup"><span data-stu-id="34672-141">You can acquire access tokens for accessing your web APIs using the [Microsoft Authentication Library (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span></span>

<span data-ttu-id="34672-142">Další informace o ověřování změny v 2.0 naleznete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="34672-142">For more information on authentication changes in 2.0, see the following resources:</span></span>

* [<span data-ttu-id="34672-143">Potvrzení účtu a obnovení hesla v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34672-143">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="34672-144">Povolit generování kód QR pro aplikace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34672-144">Enable QR Code generation for authenticator apps in ASP.NET Core</span></span>](xref:security/authentication/identity-enable-qrcodes)
* [<span data-ttu-id="34672-145">Migrace ověřování a identita na jádro ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="34672-145">Migrate Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a><span data-ttu-id="34672-146">SPA šablony</span><span class="sxs-lookup"><span data-stu-id="34672-146">SPA templates</span></span>

<span data-ttu-id="34672-147">Jeden šablony projektů stránky aplikace (SPA) pro úhlová, Aurelia, Knockout.js, React.js a React.js s Redux jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="34672-147">Single Page Application (SPA) project templates for Angular, Aurelia, Knockout.js, React.js, and React.js with Redux are available.</span></span> <span data-ttu-id="34672-148">Úhlová šablony je aktualizovaná tak, aby úhlová 4.</span><span class="sxs-lookup"><span data-stu-id="34672-148">The Angular template has been updated to Angular 4.</span></span> <span data-ttu-id="34672-149">Úhlová a reagují šablony jsou dostupné ve výchozím nastavení; informace o tom, jak získat další šablony najdete v tématu [vytvořte nový projekt SPA](xref:client-side/spa-services#creating-a-new-project).</span><span class="sxs-lookup"><span data-stu-id="34672-149">The Angular and React templates are available by default; for information about how to get the other templates, see [Create a new SPA project](xref:client-side/spa-services#creating-a-new-project).</span></span> <span data-ttu-id="34672-150">Informace o tom, jak sestavit SPA v ASP.NET Core najdete v tématu [JavaScriptServices použijte pro vytvoření jednostránkové aplikace](xref:client-side/spa-services).</span><span class="sxs-lookup"><span data-stu-id="34672-150">For information about how to build a SPA in ASP.NET Core, see [Use JavaScriptServices for Creating Single Page Applications](xref:client-side/spa-services).</span></span>

## <a name="kestrel-improvements"></a><span data-ttu-id="34672-151">Vylepšení kestrel</span><span class="sxs-lookup"><span data-stu-id="34672-151">Kestrel improvements</span></span>

<span data-ttu-id="34672-152">Webový server Kestrel obsahuje nové funkce, které lépe vyhovovalo jako internetový server.</span><span class="sxs-lookup"><span data-stu-id="34672-152">The Kestrel web server has new features that make it more suitable as an Internet-facing server.</span></span> <span data-ttu-id="34672-153">Počet možnosti konfigurace serveru omezení přidán v `KestrelServerOptions` třída je nové `Limits` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="34672-153">A number of server constraint configuration options are added in the `KestrelServerOptions` class's new `Limits` property.</span></span> <span data-ttu-id="34672-154">Přidejte následující omezení:</span><span class="sxs-lookup"><span data-stu-id="34672-154">Add limits for the following:</span></span>

- <span data-ttu-id="34672-155">Maximální počet klientských připojení</span><span class="sxs-lookup"><span data-stu-id="34672-155">Maximum client connections</span></span>
- <span data-ttu-id="34672-156">Velikost textu maximální požadavku</span><span class="sxs-lookup"><span data-stu-id="34672-156">Maximum request body size</span></span>
- <span data-ttu-id="34672-157">Minimální požadavek textu přenosová rychlost</span><span class="sxs-lookup"><span data-stu-id="34672-157">Minimum request body data rate</span></span>

<span data-ttu-id="34672-158">Další informace najdete v tématu [Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="34672-158">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel).</span></span>

## <a name="weblistener-renamed-to-httpsys"></a><span data-ttu-id="34672-159">WebListener přejmenovat ovladače HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="34672-159">WebListener renamed to HTTP.sys</span></span>

<span data-ttu-id="34672-160">Balíčky `Microsoft.AspNetCore.Server.WebListener` a `Microsoft.Net.Http.Server` byly sloučeny do nového balíčku `Microsoft.AspNetCore.Server.HttpSys`.</span><span class="sxs-lookup"><span data-stu-id="34672-160">The packages `Microsoft.AspNetCore.Server.WebListener` and `Microsoft.Net.Http.Server` have been merged into a new package `Microsoft.AspNetCore.Server.HttpSys`.</span></span> <span data-ttu-id="34672-161">Obory názvů byly aktualizovány tak, aby odpovídaly.</span><span class="sxs-lookup"><span data-stu-id="34672-161">The namespaces have been updated to match.</span></span>

<span data-ttu-id="34672-162">Další informace najdete v tématu [HTTP.sys webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="34672-162">For more information, see [HTTP.sys web server implementation in ASP.NET Core](xref:fundamentals/servers/httpsys).</span></span>

## <a name="enhanced-http-header-support"></a><span data-ttu-id="34672-163">Rozšířená podpora hlavičky protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="34672-163">Enhanced HTTP header support</span></span>

<span data-ttu-id="34672-164">Při použití MVC přenést `FileStreamResult` nebo `FileContentResult`, máte nyní možnost nastavit `ETag` nebo `LastModified` datum obsah přenosu.</span><span class="sxs-lookup"><span data-stu-id="34672-164">When using MVC to transmit a `FileStreamResult` or a `FileContentResult`, you now have the option to set an `ETag` or a `LastModified` date on the content you transmit.</span></span> <span data-ttu-id="34672-165">Můžete nastavit tyto hodnoty na vrácený obsah s kódem podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="34672-165">You can set these values on the returned content with code similar to the following:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

<span data-ttu-id="34672-166">Soubor vrátil návštěvnících bude označených pomocí příslušné hlavičky protokolu HTTP pro `ETag` a `LastModified` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="34672-166">The file returned to your visitors will be decorated with the appropriate HTTP headers for the `ETag` and `LastModified` values.</span></span>

<span data-ttu-id="34672-167">Pokud aplikaci návštěvníka vyžádá obsah s hlavičku požadavku rozsahu, ASP.NET, rozpozná a zpracování této hlavičky.</span><span class="sxs-lookup"><span data-stu-id="34672-167">If an application visitor requests content with a Range Request header, ASP.NET will recognize that and handle that header.</span></span> <span data-ttu-id="34672-168">Pokud je požadovaný obsah mohou být zajišťovány částečně, ASP.NET správně přeskočit a vrátit pouze požadovanou sadu bajtů.</span><span class="sxs-lookup"><span data-stu-id="34672-168">If the requested content can be partially delivered, ASP.NET will appropriately skip and return just the requested set of bytes.</span></span>  <span data-ttu-id="34672-169">Nepotřebujete k zápisu do vaší metody přizpůsobit nebo zpracování této funkce; všechny speciální obslužné rutiny prováděno automaticky za vás.</span><span class="sxs-lookup"><span data-stu-id="34672-169">You don't need to write any special handlers into your methods to adapt or handle this feature; it's automatically handled for you.</span></span>

## <a name="hosting-startup-and-application-insights"></a><span data-ttu-id="34672-170">Hostování spuštění a Application Insights</span><span class="sxs-lookup"><span data-stu-id="34672-170">Hosting startup and Application Insights</span></span>

<span data-ttu-id="34672-171">Teď hostitelská prostředí můžete vložit balíčku Další závislosti a spuštění kódu při spuštění aplikace, bez nutnosti explicitně trvat závislost nebo volat všechny metody aplikace.</span><span class="sxs-lookup"><span data-stu-id="34672-171">Hosting environments can now inject extra package dependencies and execute code during application startup, without the application needing to explicitly take a dependency or call any methods.</span></span> <span data-ttu-id="34672-172">Tato funkce slouží k povolení určitých prostředích "light-up" funkcí, které jsou jedinečné pro prostředí bez aplikace nepotřebuje vědět předem.</span><span class="sxs-lookup"><span data-stu-id="34672-172">This feature can be used to enable certain environments to "light-up" features unique to that environment without the application needing to know ahead of time.</span></span> 

<span data-ttu-id="34672-173">V aplikaci ASP.NET 2.0 jádra, tato funkce slouží automaticky povolí se Diagnostika Application Insights při ladění v sadě Visual Studio a (po výslovném souhlasu) při spuštění v Azure App Services.</span><span class="sxs-lookup"><span data-stu-id="34672-173">In ASP.NET Core 2.0, this feature is used to automatically enable Application Insights diagnostics when debugging in Visual Studio and (after opting in) when running in Azure App Services.</span></span> <span data-ttu-id="34672-174">V důsledku toho šablony projektů už přidejte Application Insights balíčky a kódu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="34672-174">As a result, the project templates no longer add Application Insights packages and code by default.</span></span>

<span data-ttu-id="34672-175">Informace o stavu plánované dokumentaci najdete v tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/3389).</span><span class="sxs-lookup"><span data-stu-id="34672-175">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3389).</span></span>

## <a name="automatic-use-of-anti-forgery-tokens"></a><span data-ttu-id="34672-176">Automatické použití tokeny proti zfalšování</span><span class="sxs-lookup"><span data-stu-id="34672-176">Automatic use of anti-forgery tokens</span></span>

<span data-ttu-id="34672-177">ASP.NET Core vždy pomohlo kódování HTML obsah ve výchozím nastavení, ale s novou verzí pořízení krok navíc pomohou zabránit útokům (XSRF) padělání požadavku posílaného mezi weby.</span><span class="sxs-lookup"><span data-stu-id="34672-177">ASP.NET Core has always helped HTML-encode content by default, but with the new version an extra step is taken to help prevent cross-site request forgery (XSRF) attacks.</span></span> <span data-ttu-id="34672-178">ASP.NET Core bude nyní emitování tokeny proti zfalšování ve výchozím nastavení a ověření je na akce POST formuláře a stránkách bez další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="34672-178">ASP.NET Core will now emit anti-forgery tokens by default and validate them on form POST actions and pages without extra configuration.</span></span>

<span data-ttu-id="34672-179">Další informace najdete v tématu [zabránit webů požadavku padělání (XSRF/proti útokům CSRF) před útoky](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="34672-179">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="automatic-precompilation"></a><span data-ttu-id="34672-180">Automatická předkompilace</span><span class="sxs-lookup"><span data-stu-id="34672-180">Automatic precompilation</span></span>

<span data-ttu-id="34672-181">Předběžné kompilace zobrazení syntaxe Razor je standardně během publikování, snižuje velikost výstup publikování a aplikace čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="34672-181">Razor view pre-compilation is enabled during publish by default, reducing the publish output size and application startup time.</span></span>

<span data-ttu-id="34672-182">Další informace najdete v tématu [kompilace zobrazení syntaxe Razor a předkompilaci v ASP.NET Core](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="34672-182">For more information, see [Razor view compilation and precompilation in ASP.NET Core](xref:mvc/views/view-compilation).</span></span>

## <a name="razor-support-for-c-71"></a><span data-ttu-id="34672-183">Podpora syntaxe Razor pro C# 7.1</span><span class="sxs-lookup"><span data-stu-id="34672-183">Razor support for C# 7.1</span></span>

<span data-ttu-id="34672-184">Pro práci s novou kompilátoru Roslyn byla aktualizována zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="34672-184">The Razor view engine has been updated to work with the new Roslyn compiler.</span></span> <span data-ttu-id="34672-185">Který zahrnuje podporu pro funkce C# 7.1 jako výchozí výrazy, odvodit názvy řazené kolekce členů a porovnávání s obecnými typy.</span><span class="sxs-lookup"><span data-stu-id="34672-185">That includes support for C# 7.1 features like Default Expressions, Inferred Tuple Names, and Pattern-Matching with Generics.</span></span> <span data-ttu-id="34672-186">Použití jazyka C# 7.1 v projektu, přidejte následující vlastnost v souboru projektu a potom ho znovu načtěte řešení:</span><span class="sxs-lookup"><span data-stu-id="34672-186">To use C# 7.1 in your project, add the following property in your project file and then reload the solution:</span></span>

```xml
<LangVersion>latest</LangVersion>
```

<span data-ttu-id="34672-187">Informace o stavu funkcí jazyka C# 7.1 najdete v tématu [úložiště Roslyn GitHub](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span><span class="sxs-lookup"><span data-stu-id="34672-187">For information about the status of C# 7.1 features, see [the Roslyn GitHub repository](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span></span>

## <a name="other-documentation-updates-for-20"></a><span data-ttu-id="34672-188">Další aktualizace dokumentace pro 2.0</span><span class="sxs-lookup"><span data-stu-id="34672-188">Other documentation updates for 2.0</span></span>

* [<span data-ttu-id="34672-189">Visual Studio publikační profily pro nasazení aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34672-189">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>](xref:host-and-deploy/visual-studio-publish-profiles)
* [<span data-ttu-id="34672-190">Správa klíčů</span><span class="sxs-lookup"><span data-stu-id="34672-190">Key Management</span></span>](xref:security/data-protection/implementation/key-management)
* [<span data-ttu-id="34672-191">Konfigurace ověřování Facebook</span><span class="sxs-lookup"><span data-stu-id="34672-191">Configure Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="34672-192">Nakonfigurujte ověřování služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="34672-192">Configure Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="34672-193">Konfigurace ověřování Google</span><span class="sxs-lookup"><span data-stu-id="34672-193">Configure Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="34672-194">Konfigurace ověřování Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="34672-194">Configure Microsoft Account authentication</span></span>](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a><span data-ttu-id="34672-195">Pokyny pro migraci</span><span class="sxs-lookup"><span data-stu-id="34672-195">Migration guidance</span></span>

<span data-ttu-id="34672-196">Pokyny k migraci aplikace ASP.NET Core 1.x na technologii ASP.NET 2.0 jádra najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="34672-196">For guidance on how to migrate ASP.NET Core 1.x applications to ASP.NET Core 2.0, see the following resources:</span></span>

* [<span data-ttu-id="34672-197">Migrace z ASP.NET Core 1.x na jádro ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="34672-197">Migrate from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/index)
* [<span data-ttu-id="34672-198">Migrace ověřování a identita na jádro ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="34672-198">Migrate Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a><span data-ttu-id="34672-199">Další informace</span><span class="sxs-lookup"><span data-stu-id="34672-199">Additional Information</span></span>

<span data-ttu-id="34672-200">Úplný seznam změn, najdete v článku [poznámky k verzi ASP.NET Core 2.0](https://github.com/aspnet/Home/releases/tag/2.0.0).</span><span class="sxs-lookup"><span data-stu-id="34672-200">For the complete list of changes, see the [ASP.NET Core 2.0 Release Notes](https://github.com/aspnet/Home/releases/tag/2.0.0).</span></span>

<span data-ttu-id="34672-201">Pro připojení k vývojový tým ASP.NET Core průběh a plány, naladit [ASP.NET komunity Standup](https://live.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="34672-201">To connect with the ASP.NET Core development team's progress and plans, tune in to the [ASP.NET Community Standup](https://live.asp.net/).</span></span>
