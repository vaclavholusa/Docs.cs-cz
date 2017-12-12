---
title: "Konfigurovat ověřování systému Windows v ASP.NET Core"
author: ardalis
description: "Tento článek popisuje postup konfigurace ověřování systému Windows v ASP.NET Core, pomocí služby IIS Express, IIS, ovladač HTTP.sys a WebListener."
keywords: "ASP.NET Core, ověřování systému Windows, atribut autorizovat, AllowAnonymous atribut"
ms.author: riande
manager: wpickett
ms.date: 10/24/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: 703f924d049a267cb8bee22e63e6669b13099c53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="5135d-104">Konfigurovat ověřování systému Windows v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5135d-104">Configure Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="5135d-105">Podle [Steve Smith](https://ardalis.com) a [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="5135d-105">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="5135d-106">Ověřování systému Windows lze konfigurovat pro aplikace ASP.NET Core hostované službou IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), nebo [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="5135d-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="5135d-107">Co je ověřování systému Windows?</span><span class="sxs-lookup"><span data-stu-id="5135d-107">What is Windows authentication?</span></span>

<span data-ttu-id="5135d-108">Ověřování systému Windows závisí na operačním systému k ověřování uživatelů aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5135d-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="5135d-109">Pokud váš server běží v podnikové síti pomocí identity domény služby Active Directory nebo jiné účty systému Windows k identifikaci uživatelů, můžete použít ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5135d-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="5135d-110">Ověřování systému Windows je nejvhodnější pro prostředí intranetu, ve kterých uživatelů, klientské aplikace a webové servery patří do stejné domény systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5135d-110">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="5135d-111">[Další informace o ověřování systému Windows a instalaci pro službu IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="5135d-111">[Learn more about Windows authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="5135d-112">Povolit ověřování systému Windows v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5135d-112">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="5135d-113">Šablony sady Visual Studio webové aplikace mohou být nakonfigurované pro podporu ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5135d-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="5135d-114">Použití šablony aplikace ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="5135d-114">Use the Windows authentication app template</span></span>

<span data-ttu-id="5135d-115">V sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="5135d-115">In Visual Studio:</span></span>
1. <span data-ttu-id="5135d-116">Vytvořte novou webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5135d-116">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="5135d-117">Vyberte webovou aplikaci ze seznamu šablon.</span><span class="sxs-lookup"><span data-stu-id="5135d-117">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="5135d-118">Vyberte **změna ověřování** tlačítko a vyberte **ověřování systému Windows**.</span><span class="sxs-lookup"><span data-stu-id="5135d-118">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="5135d-119">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5135d-119">Run the app.</span></span> <span data-ttu-id="5135d-120">Uživatelské jméno se zobrazí v horním rohu aplikace.</span><span class="sxs-lookup"><span data-stu-id="5135d-120">The username appears in the top right of the app.</span></span>

![Snímek obrazovky prohlížeče ověřování systému Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="5135d-122">Šablona pro vývojové práci pomocí služby IIS Express, poskytuje veškeré konfigurace nezbytné použít ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5135d-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="5135d-123">V následující části ukazuje, jak ručně nakonfigurovat aplikace ASP.NET Core pro ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5135d-123">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="5135d-124">Nastavení sady Visual Studio pro systém Windows a anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="5135d-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="5135d-125">Projekt Visual Studio **vlastnosti** stránky **ladění** karta obsahuje zaškrtávací políčka pro ověřování systému Windows a anonymní ověřování.</span><span class="sxs-lookup"><span data-stu-id="5135d-125">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Snímek obrazovky prohlížeče ověřování systému Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="5135d-127">Můžete taky tyto dvě vlastnosti se dá nakonfigurovat v *launchSettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="5135d-127">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="5135d-128">Povolit ověřování systému Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="5135d-128">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="5135d-129">Služba IIS použije [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) (ANCM) pro hostování aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5135d-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="5135d-130">ANCM toky ověřování systému Windows do služby IIS ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="5135d-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="5135d-131">Konfigurace ověřování systému Windows se provádí v rámci služby IIS, ne projekt aplikace.</span><span class="sxs-lookup"><span data-stu-id="5135d-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="5135d-132">Následující části vysvětlují, jak pomocí Správce služby IIS ke konfiguraci aplikace ASP.NET Core pomocí ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5135d-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="5135d-133">Vytvoří nový web služby IIS</span><span class="sxs-lookup"><span data-stu-id="5135d-133">Create a new IIS site</span></span>

<span data-ttu-id="5135d-134">Zadejte název a složky a povolit ji vytvořit nový fond aplikací.</span><span class="sxs-lookup"><span data-stu-id="5135d-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="5135d-135">Přizpůsobení ověřování</span><span class="sxs-lookup"><span data-stu-id="5135d-135">Customize authentication</span></span>

<span data-ttu-id="5135d-136">Otevřete nabídku ověřování pro danou lokalitu.</span><span class="sxs-lookup"><span data-stu-id="5135d-136">Open the Authentication menu for the site.</span></span>

![Nabídky ověřování služby IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="5135d-138">Zakázáním anonymního ověřování a povolit ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5135d-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Nastavení ověřování služby IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="5135d-140">Publikování projektu do složky webového serveru IIS</span><span class="sxs-lookup"><span data-stu-id="5135d-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="5135d-141">Pomocí sady Visual Studio nebo rozhraní příkazového řádku .NET Core, publikujte aplikaci do cílové složky.</span><span class="sxs-lookup"><span data-stu-id="5135d-141">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Dialogové okno publikování sady Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="5135d-143">Další informace o [publikování do služby IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="5135d-143">Learn more about [publishing to IIS](xref:publishing/iis).</span></span>

<span data-ttu-id="5135d-144">Spusťte aplikaci a ověří, zda je funkční ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5135d-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="5135d-145">Povolení ověřování systému Windows pomocí ovladače HTTP.sys nebo WebListener</span><span class="sxs-lookup"><span data-stu-id="5135d-145">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5135d-146">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="5135d-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5135d-147">I když Kestrel nepodporuje ověřování systému Windows, můžete použít [HTTP.sys](xref:fundamentals/servers/httpsys) podporu vlastním hostováním scénářů v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5135d-147">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="5135d-148">Následující příklad konfiguruje hostitel webové aplikace používat ovladač HTTP.sys pomocí ověřování systému Windows:</span><span class="sxs-lookup"><span data-stu-id="5135d-148">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5135d-149">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="5135d-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5135d-150">I když Kestrel nepodporuje ověřování systému Windows, můžete použít [WebListener](xref:fundamentals/servers/weblistener) podporu vlastním hostováním scénářů v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5135d-150">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="5135d-151">Následující příklad konfiguruje hostitel webové aplikace používat WebListener pomocí ověřování systému Windows:</span><span class="sxs-lookup"><span data-stu-id="5135d-151">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="5135d-152">Práce s ověřováním systému Windows</span><span class="sxs-lookup"><span data-stu-id="5135d-152">Work with Windows authentication</span></span>

<span data-ttu-id="5135d-153">Stav konfigurace anonymního přístupu určuje, jakým způsobem `[Authorize]` a `[AllowAnonymous]` atributy se používají v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5135d-153">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="5135d-154">Následující dvě části popisují způsob zpracování konfigurace zakázaných a povolených stavy anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="5135d-154">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="5135d-155">Zakáže anonymní přístup</span><span class="sxs-lookup"><span data-stu-id="5135d-155">Disallow anonymous access</span></span>

<span data-ttu-id="5135d-156">Pokud je povolené ověřování systému Windows a je zakázán anonymní přístup, `[Authorize]` a `[AllowAnonymous]` atributy mít žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="5135d-156">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="5135d-157">Pokud webu služby IIS (nebo ovladač HTTP.sys nebo WebListener server) je nakonfigurován tak, aby zakázala anonymní přístup, požadavek dosáhne nikdy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5135d-157">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="5135d-158">Z tohoto důvodu `[AllowAnonymous]` atribut neplatí.</span><span class="sxs-lookup"><span data-stu-id="5135d-158">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="5135d-159">Povolení anonymního přístupu</span><span class="sxs-lookup"><span data-stu-id="5135d-159">Allow anonymous access</span></span>

<span data-ttu-id="5135d-160">Pokud jsou povolené ověřování systému Windows a anonymní přístup, použijte `[Authorize]` a `[AllowAnonymous]` atributy.</span><span class="sxs-lookup"><span data-stu-id="5135d-160">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="5135d-161">`[Authorize]` Atribut umožňuje zabezpečit součásti aplikace, které skutečně vyžadují ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5135d-161">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="5135d-162">`[AllowAnonymous]` Atribut přepsání `[Authorize]` atribut využití v rámci aplikace, které povolí anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="5135d-162">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="5135d-163">V tématu [jednoduché autorizace](xref:security/authorization/simple) podrobnosti o použití atributu.</span><span class="sxs-lookup"><span data-stu-id="5135d-163">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="5135d-164">V ASP.NET Core 2.x, `[Authorize]` atribut vyžaduje, aby další konfigurace v *Startup.cs* vyzve anonymních požadavků pro ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5135d-164">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="5135d-165">Doporučená konfigurace mírně závisí na webovém serveru, který je používán.</span><span class="sxs-lookup"><span data-stu-id="5135d-165">The recommended configuration varies slightly based on the web server being used.</span></span>

#### <a name="iis"></a><span data-ttu-id="5135d-166">IIS</span><span class="sxs-lookup"><span data-stu-id="5135d-166">IIS</span></span>

<span data-ttu-id="5135d-167">Pokud používáte službu IIS, přidejte následující `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="5135d-167">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="5135d-168">Ovladač HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="5135d-168">HTTP.sys</span></span>

<span data-ttu-id="5135d-169">Pokud používáte HTTP.sys, přidejte následující `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="5135d-169">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="5135d-170">Zosobnění</span><span class="sxs-lookup"><span data-stu-id="5135d-170">Impersonation</span></span>

<span data-ttu-id="5135d-171">ASP.NET Core neimplementuje zosobnění.</span><span class="sxs-lookup"><span data-stu-id="5135d-171">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="5135d-172">Aplikace spustí s identity aplikací pro všechny požadavky a pomocí identity aplikace fondu nebo proces.</span><span class="sxs-lookup"><span data-stu-id="5135d-172">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="5135d-173">Pokud potřebujete explicitně provedení akce jménem uživatele, použijte `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="5135d-173">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="5135d-174">Spustit jednou akcí v tomto kontextu a pak zavřete kontextu.</span><span class="sxs-lookup"><span data-stu-id="5135d-174">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="5135d-175">Všimněte si, že `RunImpersonated` nepodporuje asynchronní operace a by nemělo být použito pro komplexní scénáře.</span><span class="sxs-lookup"><span data-stu-id="5135d-175">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="5135d-176">Například zabalení celý požadavky nebo middleware zřetězen není podporován nebo doporučené.</span><span class="sxs-lookup"><span data-stu-id="5135d-176">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---
