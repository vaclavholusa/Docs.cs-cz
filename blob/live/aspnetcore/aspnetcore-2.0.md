---
title: "Co je nového v technologii ASP.NET 2.0 jádra"
author: rick-anderson
description: "Co je nového v technologii ASP.NET 2.0 jádra"
keywords: "ASP.NET Core, poznámky k verzi, co je nového"
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: 08c9f457-9c24-40f9-a08b-47dc251e4cec
ms.technology: aspnet
ms.prod: aspnet-core
uid: aspnetcore-2.0
ms.openlocfilehash: f01f047f809e4eaa055a4204611b152c5db87f74
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="whats-new-in-aspnet-core-20"></a><span data-ttu-id="ffe9e-104">Co je nového v technologii ASP.NET 2.0 jádra</span><span class="sxs-lookup"><span data-stu-id="ffe9e-104">What's new in ASP.NET Core 2.0</span></span>

<span data-ttu-id="ffe9e-105">V tomto článku klade důraz nejvýznamnějších změn v aplikaci ASP.NET 2.0 jádra, s odkazy na příslušné dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-105">This article highlights the most significant changes in ASP.NET Core 2.0, with links to relevant documentation.</span></span>

## <a name="razor-pages"></a><span data-ttu-id="ffe9e-106">Stránky Razor</span><span class="sxs-lookup"><span data-stu-id="ffe9e-106">Razor Pages</span></span>

<span data-ttu-id="ffe9e-107">Stránky Razor je nová funkce rozhraní ASP.NET MVC jádra, která vytváří kódování zaměřené na stránce scénáře jednodušší a zvýšit produktivitu.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-107">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="ffe9e-108">Další informace najdete v tématu Úvod a kurzu:</span><span class="sxs-lookup"><span data-stu-id="ffe9e-108">For more information, see the introduction and tutorial:</span></span>

* [<span data-ttu-id="ffe9e-109">Úvod do stránky Razor</span><span class="sxs-lookup"><span data-stu-id="ffe9e-109">Introduction to Razor Pages</span></span>](xref:mvc/razor-pages/index)
* [<span data-ttu-id="ffe9e-110">Začínáme se stránkami Razor</span><span class="sxs-lookup"><span data-stu-id="ffe9e-110">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a><span data-ttu-id="ffe9e-111">Metapackage ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ffe9e-111">ASP.NET Core metapackage</span></span>

<span data-ttu-id="ffe9e-112">Nové metapackage ASP.NET Core zahrnuje všechny balíčky provedené a nepodporuje týmy ASP.NET Core a Entity Framework Core spolu s jejich interní a 3. stran závislosti.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-112">A new ASP.NET Core metapackage includes all of the packages made and supported by the ASP.NET Core and Entity Framework Core teams, along with their internal and 3rd-party dependencies.</span></span> <span data-ttu-id="ffe9e-113">Již nepotřebujete vyberte jednotlivé ASP.NET Core funkce balíčkem.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-113">You no longer need to choose individual ASP.NET Core features by package.</span></span> <span data-ttu-id="ffe9e-114">Všechny funkce jsou součástí [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) balíčku.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-114">All features are included in the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package.</span></span> <span data-ttu-id="ffe9e-115">Tento balíček použít výchozí šablony.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-115">The default templates use this package.</span></span>

<span data-ttu-id="ffe9e-116">Další informace najdete v tématu [Microsoft.AspNetCore.All metapackage pro technologii ASP.NET 2.0 základní](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-116">For more information, see [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0](xref:fundamentals/metapackage).</span></span>

## <a name="runtime-store"></a><span data-ttu-id="ffe9e-117">Modul runtime úložiště</span><span class="sxs-lookup"><span data-stu-id="ffe9e-117">Runtime Store</span></span>

<span data-ttu-id="ffe9e-118">Aplikace, které používají `Microsoft.AspNetCore.All` metapackage automaticky využít výhod nové úložiště .NET Core Runtime.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-118">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the new .NET Core Runtime Store.</span></span> <span data-ttu-id="ffe9e-119">Úložiště obsahuje všechny prostředky runtime potřebné ke spuštění aplikace ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-119">The Store contains all the runtime assets needed to run ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="ffe9e-120">Při použití `Microsoft.AspNetCore.All` metapackage žádné prostředky z odkazované balíčky ASP.NET Core NuGet nasazených s aplikací, protože se již nacházejí v cílovém systému.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-120">When you use the `Microsoft.AspNetCore.All` metapackage, no assets from the referenced ASP.NET Core NuGet packages are deployed with the application because they already reside on the target system.</span></span> <span data-ttu-id="ffe9e-121">Prostředky v úložišti Runtime jsou také předkompilovaných ke zlepšení času spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-121">The assets in the Runtime Store are also precompiled to improve application startup time.</span></span>

<span data-ttu-id="ffe9e-122">Další informace najdete v tématu [Runtime úložiště](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)</span><span class="sxs-lookup"><span data-stu-id="ffe9e-122">For more information, see [Runtime store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="ffe9e-123">Rozhraní .NET standard 2.0</span><span class="sxs-lookup"><span data-stu-id="ffe9e-123">.NET Standard 2.0</span></span>

<span data-ttu-id="ffe9e-124">Balíčky technologii ASP.NET 2.0 základní cíle standardní rozhraní .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-124">The ASP.NET Core 2.0 packages target .NET Standard 2.0.</span></span> <span data-ttu-id="ffe9e-125">Balíčky mohou odkazovat další rozhraní .NET 2.0 standardní knihovny a jejich spuštěním na .NET Standard 2.0 kompatibilní implementace rozhraní .NET, včetně .NET Core 2.0 a .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-125">The packages can be referenced by other .NET Standard 2.0 libraries, and they can run on .NET Standard 2.0-compliant implementations of .NET, including .NET Core 2.0 and .NET Framework 4.6.1.</span></span> 

<span data-ttu-id="ffe9e-126">`Microsoft.AspNetCore.All` Metapackage cílem .NET Core 2.0, protože je určen pro použití s úložištěm modulu Runtime .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-126">The `Microsoft.AspNetCore.All` metapackage targets .NET Core 2.0 only, because it is intended to be used with the .NET Core 2.0 Runtime Store.</span></span>

## <a name="configuration-update"></a><span data-ttu-id="ffe9e-127">Aktualizace konfigurace</span><span class="sxs-lookup"><span data-stu-id="ffe9e-127">Configuration update</span></span>

<span data-ttu-id="ffe9e-128">`IConfiguration` Instance je přidat do kontejneru služby ve výchozím nastavení v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-128">An `IConfiguration` instance is added to the services container by default in ASP.NET Core 2.0.</span></span> <span data-ttu-id="ffe9e-129">`IConfiguration`v služby kontejneru usnadňuje aplikace k načtení hodnoty konfigurace z kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-129">`IConfiguration` in the services container makes it easier for applications to retrieve configuration values from the container.</span></span>

<span data-ttu-id="ffe9e-130">Informace o stavu plánované dokumentaci najdete v tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/3387).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-130">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3387).</span></span>

## <a name="logging-update"></a><span data-ttu-id="ffe9e-131">Protokolování aktualizace</span><span class="sxs-lookup"><span data-stu-id="ffe9e-131">Logging update</span></span>

<span data-ttu-id="ffe9e-132">V aplikaci ASP.NET 2.0 jádra je ve výchozím nastavení protokolování součástí systému (DI) vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-132">In ASP.NET Core 2.0, logging is incorporated into the dependency injection (DI) system by default.</span></span> <span data-ttu-id="ffe9e-133">Můžete přidat zprostředkovatele a nakonfigurovat filtrování v *Program.cs* místo v souboru *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-133">You add providers and configure filtering in the *Program.cs* file instead of in the *Startup.cs* file.</span></span> <span data-ttu-id="ffe9e-134">A ve výchozím nastavení `ILoggerFactory` podporuje filtrování způsobem, který vám umožní používat jeden flexibilní přístup pro filtrování mezi zprostředkovatele i filtrování specifické pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-134">And the default `ILoggerFactory` supports filtering in a way that lets you use one flexible approach for both cross-provider filtering and specific-provider filtering.</span></span>

<span data-ttu-id="ffe9e-135">Další informace najdete v tématu [Úvod k protokolování](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-135">For more information, see [Introduction to Logging](xref:fundamentals/logging/index).</span></span>

## <a name="authentication-update"></a><span data-ttu-id="ffe9e-136">Aktualizace ověřování</span><span class="sxs-lookup"><span data-stu-id="ffe9e-136">Authentication update</span></span>

<span data-ttu-id="ffe9e-137">Nový model ověřování usnadňuje nakonfigurovat ověřování pro aplikaci pomocí DI.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-137">A new authentication model makes it easier to configure authentication for an application using DI.</span></span>

<span data-ttu-id="ffe9e-138">Nové šablony jsou dostupné pro konfiguraci ověřování pro webové aplikace a webové rozhraní API pomocí [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-138">New templates are available for configuring authentication for web apps and web APIs using [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).</span></span>

<span data-ttu-id="ffe9e-139">Informace o stavu plánované dokumentaci najdete v tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/3054).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-139">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3054).</span></span>

## <a name="identity-update"></a><span data-ttu-id="ffe9e-140">Aktualizace identity</span><span class="sxs-lookup"><span data-stu-id="ffe9e-140">Identity update</span></span>

<span data-ttu-id="ffe9e-141">Provedli jsme je snazší sestavovat zabezpečené webové rozhraní API pomocí Identity v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-141">We've made it easier to build secure web APIs using Identity in ASP.NET Core 2.0.</span></span> <span data-ttu-id="ffe9e-142">Můžete získat přístupové tokeny pro přístup k pomocí webového rozhraní API [Microsoft ověřování knihovny (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-142">You can acquire access tokens for accessing your web APIs using the [Microsoft Authentication Library (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span></span>

<span data-ttu-id="ffe9e-143">Další informace o ověřování změny v 2.0 naleznete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="ffe9e-143">For more information on authentication changes in 2.0, see the following resources:</span></span>

* [<span data-ttu-id="ffe9e-144">Potvrzení účtu a obnovení hesla v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ffe9e-144">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="ffe9e-145">Povolení generování kód QR pro aplikace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ffe9e-145">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>](xref:security/authentication/identity-enable-qrcodes)
* [<span data-ttu-id="ffe9e-146">Migrace ověřování a identita na jádro ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="ffe9e-146">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a><span data-ttu-id="ffe9e-147">SPA šablony</span><span class="sxs-lookup"><span data-stu-id="ffe9e-147">SPA templates</span></span>

<span data-ttu-id="ffe9e-148">Jeden šablony projektů stránky aplikace (SPA) pro úhlová, Aurelia, Knockout.js, React.js a React.js s Redux jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-148">Single Page Application (SPA) project templates for Angular, Aurelia, Knockout.js, React.js, and React.js with Redux are available.</span></span> <span data-ttu-id="ffe9e-149">Úhlová šablony je aktualizovaná tak, aby úhlová 4.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-149">The Angular template has been updated to Angular 4.</span></span> <span data-ttu-id="ffe9e-150">Úhlová a reagují šablony jsou dostupné ve výchozím nastavení; informace o tom, jak získat další šablony najdete v tématu [vytvoření nového projektu SPA](xref:client-side/spa-services#creating-a-new-project).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-150">The Angular and React templates are available by default; for information about how to get the other templates, see [Creating a new SPA project](xref:client-side/spa-services#creating-a-new-project).</span></span> <span data-ttu-id="ffe9e-151">Informace o tom, jak sestavit SPA v ASP.NET Core najdete v tématu [pomocí JavaScriptServices pro vytváření jednostránkové aplikace](xref:client-side/spa-services).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-151">For information about how to build a SPA in ASP.NET Core, see [Using JavaScriptServices for Creating Single Page Applications](xref:client-side/spa-services).</span></span>

## <a name="kestrel-improvements"></a><span data-ttu-id="ffe9e-152">Vylepšení kestrel</span><span class="sxs-lookup"><span data-stu-id="ffe9e-152">Kestrel improvements</span></span>

<span data-ttu-id="ffe9e-153">Webový server Kestrel obsahuje nové funkce, které lépe vyhovovalo jako internetový server.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-153">The Kestrel web server has new features that make it more suitable as an Internet-facing server.</span></span> <span data-ttu-id="ffe9e-154">Přidali jsme řadu možnosti konfigurace serveru omezení v `KestrelServerOptions` třída je nové `Limits` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-154">We’ve added a number of server constraint configuration options in the `KestrelServerOptions` class’s new `Limits` property.</span></span> <span data-ttu-id="ffe9e-155">Nyní můžete přidat omezení pro následující:</span><span class="sxs-lookup"><span data-stu-id="ffe9e-155">You can now add limits for the following:</span></span>

- <span data-ttu-id="ffe9e-156">Maximální počet klientských připojení</span><span class="sxs-lookup"><span data-stu-id="ffe9e-156">Maximum client connections</span></span>
- <span data-ttu-id="ffe9e-157">Velikost textu maximální požadavku</span><span class="sxs-lookup"><span data-stu-id="ffe9e-157">Maximum request body size</span></span>
- <span data-ttu-id="ffe9e-158">Minimální požadavek textu přenosová rychlost</span><span class="sxs-lookup"><span data-stu-id="ffe9e-158">Minimum request body data rate</span></span>

<span data-ttu-id="ffe9e-159">Další informace najdete v tématu [Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-159">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel).</span></span>

## <a name="weblistener-renamed-to-httpsys"></a><span data-ttu-id="ffe9e-160">WebListener přejmenovat ovladače HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ffe9e-160">WebListener renamed to HTTP.sys</span></span>

<span data-ttu-id="ffe9e-161">Balíčky `Microsoft.AspNetCore.Server.WebListener` a `Microsoft.Net.Http.Server` byly sloučeny do nového balíčku `Microsoft.AspNetCore.Server.HttpSys`.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-161">The packages `Microsoft.AspNetCore.Server.WebListener` and `Microsoft.Net.Http.Server` have been merged into a new package `Microsoft.AspNetCore.Server.HttpSys`.</span></span> <span data-ttu-id="ffe9e-162">Obory názvů byly aktualizovány tak, aby odpovídaly.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-162">The namespaces have been updated to match.</span></span>

<span data-ttu-id="ffe9e-163">Další informace najdete v tématu [HTTP.sys webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-163">For more information, see [HTTP.sys web server implementation in ASP.NET Core](xref:fundamentals/servers/httpsys).</span></span>

## <a name="enhanced-http-header-support"></a><span data-ttu-id="ffe9e-164">Rozšířená podpora hlavičky protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="ffe9e-164">Enhanced HTTP header support</span></span>

<span data-ttu-id="ffe9e-165">Při použití MVC přenést `FileStreamResult` nebo `FileContentResult`, máte nyní možnost nastavit `ETag` nebo `LastModified` datum obsah přenosu.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-165">When using MVC to transmit a `FileStreamResult` or a `FileContentResult`, you now have the option to set an `ETag` or a `LastModified` date on the content you transmit.</span></span> <span data-ttu-id="ffe9e-166">Můžete nastavit tyto hodnoty na vrácený obsah s kódem podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="ffe9e-166">You can set these values on the returned content with code similar to the following:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

<span data-ttu-id="ffe9e-167">Soubor vrátil návštěvnících bude označených pomocí příslušné hlavičky protokolu HTTP pro `ETag` a `LastModified` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-167">The file returned to your visitors will be decorated with the appropriate HTTP headers for the `ETag` and `LastModified` values.</span></span>

<span data-ttu-id="ffe9e-168">Pokud aplikaci návštěvníka vyžádá obsah s hlavičku požadavku rozsahu, ASP.NET, rozpozná a zpracování této hlavičky.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-168">If an application visitor requests content with a Range Request header, ASP.NET will recognize that and handle that header.</span></span> <span data-ttu-id="ffe9e-169">Pokud je požadovaný obsah mohou být zajišťovány částečně, ASP.NET správně přeskočit a vrátit pouze požadovanou sadu bajtů.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-169">If the requested content can be partially delivered, ASP.NET will appropriately skip and return just the requested set of bytes.</span></span>  <span data-ttu-id="ffe9e-170">Není nutné k zápisu do vaší metody přizpůsobit nebo zpracování této funkce; všechny speciální obslužné rutiny prováděno automaticky za vás.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-170">You do not need to write any special handlers into your methods to adapt or handle this feature; it is automatically handled for you.</span></span>

## <a name="hosting-startup-and-application-insights"></a><span data-ttu-id="ffe9e-171">Hostování spuštění a Application Insights</span><span class="sxs-lookup"><span data-stu-id="ffe9e-171">Hosting startup and Application Insights</span></span>

<span data-ttu-id="ffe9e-172">Teď hostitelská prostředí můžete vložit balíčku Další závislosti a spuštění kódu při spuštění aplikace, bez nutnosti explicitně trvat závislost nebo volat všechny metody aplikace.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-172">Hosting environments can now inject extra package dependencies and execute code during application startup, without the application needing to explicitly take a dependency or call any methods.</span></span> <span data-ttu-id="ffe9e-173">Tato funkce slouží k povolení určitých prostředích "light-up" funkcí, které jsou jedinečné pro prostředí bez aplikace nepotřebuje vědět předem.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-173">This feature can be used to enable certain environments to "light-up" features unique to that environment without the application needing to know ahead of time.</span></span> 

<span data-ttu-id="ffe9e-174">V aplikaci ASP.NET 2.0 jádra, tato funkce slouží automaticky povolí se Diagnostika Application Insights při ladění v sadě Visual Studio a (po výslovném souhlasu) při spuštění v Azure App Services.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-174">In ASP.NET Core 2.0, this feature is used to automatically enable Application Insights diagnostics when debugging in Visual Studio and (after opting in) when running in Azure App Services.</span></span> <span data-ttu-id="ffe9e-175">V důsledku toho šablony projektů už přidejte Application Insights balíčky a kódu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-175">As a result, the project templates no longer add Application Insights packages and code by default.</span></span>

<span data-ttu-id="ffe9e-176">Informace o stavu plánované dokumentaci najdete v tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/3389).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-176">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3389).</span></span>

## <a name="automatic-use-of-anti-forgery-tokens"></a><span data-ttu-id="ffe9e-177">Automatické použití tokeny proti zfalšování</span><span class="sxs-lookup"><span data-stu-id="ffe9e-177">Automatic use of anti-forgery tokens</span></span>

<span data-ttu-id="ffe9e-178">ASP.NET Core vždy pomohlo kódování HTML obsah ve výchozím nastavení, ale s novou verzí přesměrováváme krok navíc pomáhá zabránit útokům (XSRF) padělání požadavku posílaného mezi weby.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-178">ASP.NET Core has always helped HTML-encode your content by default, but with the new version we’re taking an extra step to help prevent cross-site request forgery (XSRF) attacks.</span></span> <span data-ttu-id="ffe9e-179">ASP.NET Core bude nyní emitování tokeny proti zfalšování ve výchozím nastavení a ověření je na akce POST formuláře a stránkách bez další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-179">ASP.NET Core will now emit anti-forgery tokens by default and validate them on form POST actions and pages without extra configuration.</span></span>

<span data-ttu-id="ffe9e-180">Další informace najdete v tématu [útoky brání webů požadavku padělání (XSRF/proti útokům CSRF) v ASP.NET Core](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-180">For more information, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](xref:security/anti-request-forgery).</span></span>

## <a name="automatic-precompilation"></a><span data-ttu-id="ffe9e-181">Automatická předkompilace</span><span class="sxs-lookup"><span data-stu-id="ffe9e-181">Automatic precompilation</span></span>

<span data-ttu-id="ffe9e-182">Předběžné kompilace zobrazení syntaxe Razor je standardně během publikování, snižuje velikost výstup publikování a aplikace čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-182">Razor view pre-compilation is enabled during publish by default, reducing the publish output size and application startup time.</span></span>

## <a name="razor-support-for-c-71"></a><span data-ttu-id="ffe9e-183">Podpora syntaxe Razor pro C# 7.1</span><span class="sxs-lookup"><span data-stu-id="ffe9e-183">Razor support for C# 7.1</span></span>

<span data-ttu-id="ffe9e-184">Pro práci s novou kompilátoru Roslyn byla aktualizována zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-184">The Razor view engine has been updated to work with the new Roslyn compiler.</span></span> <span data-ttu-id="ffe9e-185">Který zahrnuje podporu pro funkce C# 7.1 jako výchozí výrazy, odvodit názvy řazené kolekce členů a porovnávání s obecnými typy.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-185">That includes support for C# 7.1 features like Default Expressions, Inferred Tuple Names, and Pattern-Matching with Generics.</span></span> <span data-ttu-id="ffe9e-186">Použití jazyka C# 7.1 v projektu, přidejte následující vlastnost v souboru projektu a potom ho znovu načtěte řešení:</span><span class="sxs-lookup"><span data-stu-id="ffe9e-186">To use C# 7.1 in your project, add the following property in your project file and then reload the solution:</span></span>

```xml
<LangVersion>latest</LangVersion>
```

<span data-ttu-id="ffe9e-187">Informace o stavu funkcí jazyka C# 7.1 najdete v tématu [úložiště Roslyn GitHub](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-187">For information about the status of C# 7.1 features, see [the Roslyn GitHub repository](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span></span>

## <a name="other-documentation-updates-for-20"></a><span data-ttu-id="ffe9e-188">Další aktualizace dokumentace pro 2.0</span><span class="sxs-lookup"><span data-stu-id="ffe9e-188">Other documentation updates for 2.0</span></span>

* [<span data-ttu-id="ffe9e-189">Visual Studio publikační profily pro nasazení aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ffe9e-189">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>](xref:host-and-deploy/visual-studio-publish-profiles)
* [<span data-ttu-id="ffe9e-190">Správa klíčů</span><span class="sxs-lookup"><span data-stu-id="ffe9e-190">Key Management</span></span>](xref:security/data-protection/implementation/key-management)
* [<span data-ttu-id="ffe9e-191">Konfigurace ověřování Facebook</span><span class="sxs-lookup"><span data-stu-id="ffe9e-191">Configuring Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="ffe9e-192">Konfigurace ověřování služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="ffe9e-192">Configuring Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="ffe9e-193">Konfigurace ověřování Google</span><span class="sxs-lookup"><span data-stu-id="ffe9e-193">Configuring Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="ffe9e-194">Konfigurace ověřování Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="ffe9e-194">Configuring Microsoft Account authentication</span></span>](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a><span data-ttu-id="ffe9e-195">Pokyny pro migraci</span><span class="sxs-lookup"><span data-stu-id="ffe9e-195">Migration guidance</span></span>

<span data-ttu-id="ffe9e-196">Pokyny k migraci aplikace ASP.NET Core 1.x na technologii ASP.NET 2.0 jádra najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="ffe9e-196">For guidance on how to migrate ASP.NET Core 1.x applications to ASP.NET Core 2.0, see the following resources:</span></span>

* [<span data-ttu-id="ffe9e-197">Migrace z ASP.NET Core 1.x na jádro ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="ffe9e-197">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/index)
* [<span data-ttu-id="ffe9e-198">Migrace ověřování a identita na jádro ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="ffe9e-198">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a><span data-ttu-id="ffe9e-199">Další informace</span><span class="sxs-lookup"><span data-stu-id="ffe9e-199">Additional Information</span></span>

<span data-ttu-id="ffe9e-200">Úplný seznam změn, najdete v článku [poznámky k verzi ASP.NET Core 2.0](https://github.com/aspnet/Home/releases/tag/2.0.0).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-200">For the complete list of changes, see the [ASP.NET Core 2.0 Release Notes](https://github.com/aspnet/Home/releases/tag/2.0.0).</span></span>

<span data-ttu-id="ffe9e-201">Pokud chcete propojit s ASP.NET Core vývojový tým průběh a plány, naladit týdenní [ASP.NET komunity Standup](https://live.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="ffe9e-201">If you’d like to connect with the ASP.NET Core development team’s progress and plans, tune in to the weekly [ASP.NET Community Standup](https://live.asp.net/).</span></span>
