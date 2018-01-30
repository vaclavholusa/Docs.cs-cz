---
title: "Konfigurovat ověřování systému Windows v ASP.NET Core"
author: ardalis
description: "Tento článek popisuje postup konfigurace ověřování systému Windows v ASP.NET Core, pomocí služby IIS Express, IIS, ovladač HTTP.sys a WebListener."
manager: wpickett
ms.author: riande
ms.date: 10/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/windowsauth
ms.openlocfilehash: aaa14e2f2704a7cfa836c5524642d2138a3ae7c8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="9b4f8-103">Konfigurovat ověřování systému Windows v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b4f8-103">Configure Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="9b4f8-104">Podle [Steve Smith](https://ardalis.com) a [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="9b4f8-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="9b4f8-105">Ověřování systému Windows lze konfigurovat pro aplikace ASP.NET Core hostované službou IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), nebo [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="9b4f8-105">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="9b4f8-106">Co je ověřování systému Windows?</span><span class="sxs-lookup"><span data-stu-id="9b4f8-106">What is Windows authentication?</span></span>

<span data-ttu-id="9b4f8-107">Ověřování systému Windows závisí na operačním systému k ověřování uživatelů aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-107">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="9b4f8-108">Pokud váš server běží v podnikové síti pomocí identity domény služby Active Directory nebo jiné účty systému Windows k identifikaci uživatelů, můžete použít ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-108">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="9b4f8-109">Ověřování systému Windows je nejvhodnější pro prostředí intranetu, ve kterých uživatelů, klientské aplikace a webové servery patří do stejné domény systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-109">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="9b4f8-110">[Další informace o ověřování systému Windows a instalaci pro službu IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="9b4f8-110">[Learn more about Windows authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="9b4f8-111">Povolit ověřování systému Windows v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b4f8-111">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="9b4f8-112">Šablony sady Visual Studio webové aplikace mohou být nakonfigurované pro podporu ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-112">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="9b4f8-113">Použití šablony aplikace ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="9b4f8-113">Use the Windows authentication app template</span></span>

<span data-ttu-id="9b4f8-114">In Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9b4f8-114">In Visual Studio:</span></span>
1. <span data-ttu-id="9b4f8-115">Vytvořte novou webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-115">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="9b4f8-116">Vyberte webovou aplikaci ze seznamu šablon.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="9b4f8-117">Vyberte **změna ověřování** tlačítko a vyberte **ověřování systému Windows**.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="9b4f8-118">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-118">Run the app.</span></span> <span data-ttu-id="9b4f8-119">Uživatelské jméno se zobrazí v horním rohu aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-119">The username appears in the top right of the app.</span></span>

![Snímek obrazovky prohlížeče ověřování systému Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="9b4f8-121">Šablona pro vývojové práci pomocí služby IIS Express, poskytuje veškeré konfigurace nezbytné použít ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="9b4f8-122">V následující části ukazuje, jak ručně nakonfigurovat aplikace ASP.NET Core pro ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-122">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="9b4f8-123">Nastavení sady Visual Studio pro systém Windows a anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="9b4f8-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="9b4f8-124">Projekt Visual Studio **vlastnosti** stránky **ladění** karta obsahuje zaškrtávací políčka pro ověřování systému Windows a anonymní ověřování.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Snímek obrazovky prohlížeče ověřování systému Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="9b4f8-126">Můžete taky tyto dvě vlastnosti se dá nakonfigurovat v *launchSettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="9b4f8-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="9b4f8-127">Povolit ověřování systému Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="9b4f8-127">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="9b4f8-128">Služba IIS použije [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) (ANCM) pro hostování aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="9b4f8-129">ANCM toky ověřování systému Windows do služby IIS ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-129">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="9b4f8-130">Konfigurace ověřování systému Windows se provádí v rámci služby IIS, ne projekt aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-130">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="9b4f8-131">Následující části vysvětlují, jak pomocí Správce služby IIS ke konfiguraci aplikace ASP.NET Core pomocí ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-131">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="9b4f8-132">Vytvoří nový web služby IIS</span><span class="sxs-lookup"><span data-stu-id="9b4f8-132">Create a new IIS site</span></span>

<span data-ttu-id="9b4f8-133">Zadejte název a složky a povolit ji vytvořit nový fond aplikací.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-133">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="9b4f8-134">Přizpůsobení ověřování</span><span class="sxs-lookup"><span data-stu-id="9b4f8-134">Customize authentication</span></span>

<span data-ttu-id="9b4f8-135">Otevřete nabídku ověřování pro danou lokalitu.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-135">Open the Authentication menu for the site.</span></span>

![Nabídky ověřování služby IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="9b4f8-137">Zakázáním anonymního ověřování a povolit ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-137">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Nastavení ověřování služby IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="9b4f8-139">Publikování projektu do složky webového serveru IIS</span><span class="sxs-lookup"><span data-stu-id="9b4f8-139">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="9b4f8-140">Pomocí sady Visual Studio nebo rozhraní příkazového řádku .NET Core, publikujte aplikaci do cílové složky.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-140">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Dialogové okno publikování sady Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="9b4f8-142">Další informace o [publikování do služby IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="9b4f8-142">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="9b4f8-143">Spusťte aplikaci a ověří, zda je funkční ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-143">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="9b4f8-144">Povolení ověřování systému Windows pomocí ovladače HTTP.sys nebo WebListener</span><span class="sxs-lookup"><span data-stu-id="9b4f8-144">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9b4f8-145">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="9b4f8-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9b4f8-146">I když Kestrel nepodporuje ověřování systému Windows, můžete použít [HTTP.sys](xref:fundamentals/servers/httpsys) podporu vlastním hostováním scénářů v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-146">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="9b4f8-147">Následující příklad konfiguruje hostitel webové aplikace používat ovladač HTTP.sys pomocí ověřování systému Windows:</span><span class="sxs-lookup"><span data-stu-id="9b4f8-147">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9b4f8-148">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="9b4f8-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9b4f8-149">I když Kestrel nepodporuje ověřování systému Windows, můžete použít [WebListener](xref:fundamentals/servers/weblistener) podporu vlastním hostováním scénářů v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-149">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="9b4f8-150">Následující příklad konfiguruje hostitel webové aplikace používat WebListener pomocí ověřování systému Windows:</span><span class="sxs-lookup"><span data-stu-id="9b4f8-150">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="9b4f8-151">Práce s ověřováním systému Windows</span><span class="sxs-lookup"><span data-stu-id="9b4f8-151">Work with Windows authentication</span></span>

<span data-ttu-id="9b4f8-152">Stav konfigurace anonymního přístupu určuje, jakým způsobem `[Authorize]` a `[AllowAnonymous]` atributy se používají v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-152">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="9b4f8-153">Následující dvě části popisují způsob zpracování konfigurace zakázaných a povolených stavy anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-153">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="9b4f8-154">Zakáže anonymní přístup</span><span class="sxs-lookup"><span data-stu-id="9b4f8-154">Disallow anonymous access</span></span>

<span data-ttu-id="9b4f8-155">Pokud je povolené ověřování systému Windows a je zakázán anonymní přístup, `[Authorize]` a `[AllowAnonymous]` atributy mít žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-155">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="9b4f8-156">Pokud webu služby IIS (nebo ovladač HTTP.sys nebo WebListener server) je nakonfigurován tak, aby zakázala anonymní přístup, požadavek dosáhne nikdy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-156">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="9b4f8-157">Z tohoto důvodu `[AllowAnonymous]` atribut neplatí.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-157">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="9b4f8-158">Povolení anonymního přístupu</span><span class="sxs-lookup"><span data-stu-id="9b4f8-158">Allow anonymous access</span></span>

<span data-ttu-id="9b4f8-159">Pokud jsou povolené ověřování systému Windows a anonymní přístup, použijte `[Authorize]` a `[AllowAnonymous]` atributy.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-159">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="9b4f8-160">`[Authorize]` Atribut umožňuje zabezpečit součásti aplikace, které skutečně vyžadují ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-160">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="9b4f8-161">`[AllowAnonymous]` Atribut přepsání `[Authorize]` atribut využití v rámci aplikace, které povolí anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-161">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="9b4f8-162">V tématu [jednoduché autorizace](xref:security/authorization/simple) podrobnosti o použití atributu.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-162">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="9b4f8-163">V ASP.NET Core 2.x, `[Authorize]` atribut vyžaduje, aby další konfigurace v *Startup.cs* vyzve anonymních požadavků pro ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-163">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="9b4f8-164">Doporučená konfigurace mírně závisí na webovém serveru, který je používán.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-164">The recommended configuration varies slightly based on the web server being used.</span></span>

#### <a name="iis"></a><span data-ttu-id="9b4f8-165">IIS</span><span class="sxs-lookup"><span data-stu-id="9b4f8-165">IIS</span></span>

<span data-ttu-id="9b4f8-166">Pokud používáte službu IIS, přidejte následující `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="9b4f8-166">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="9b4f8-167">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="9b4f8-167">HTTP.sys</span></span>

<span data-ttu-id="9b4f8-168">Pokud používáte HTTP.sys, přidejte následující `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="9b4f8-168">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="9b4f8-169">Zosobnění</span><span class="sxs-lookup"><span data-stu-id="9b4f8-169">Impersonation</span></span>

<span data-ttu-id="9b4f8-170">ASP.NET Core neimplementuje zosobnění.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-170">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="9b4f8-171">Aplikace spustí s identity aplikací pro všechny požadavky a pomocí identity aplikace fondu nebo proces.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-171">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="9b4f8-172">Pokud potřebujete explicitně provedení akce jménem uživatele, použijte `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-172">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="9b4f8-173">Spustit jednou akcí v tomto kontextu a pak zavřete kontextu.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-173">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="9b4f8-174">Všimněte si, že `RunImpersonated` nepodporuje asynchronní operace a by nemělo být použito pro komplexní scénáře.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-174">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="9b4f8-175">Například zabalení celý požadavky nebo middleware zřetězen není podporován nebo doporučené.</span><span class="sxs-lookup"><span data-stu-id="9b4f8-175">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---
