---
title: Konfigurace ověřování Windows v ASP.NET Core
author: ardalis
description: Tento článek popisuje, jak nakonfigurovat ověřování Windows v ASP.NET Core pomocí služby IIS Express, IIS, ovladač HTTP.sys a WebListener.
ms.author: riande
ms.date: 08/18/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 93b1a1de74ef6554d48709b04870f7e23738846b
ms.sourcegitcommit: 15d7bd0b2c4e6fe9ac335d658bab71a45ca5bc72
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/20/2018
ms.locfileid: "41753135"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="70230-103">Konfigurace ověřování Windows v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70230-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="70230-104">Podle [Steve Smith](https://ardalis.com) a [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="70230-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="70230-105">Ověřování Windows se dá nakonfigurovat pro aplikace ASP.NET Core hostované službou IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), nebo [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="70230-105">Windows Authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="windows-authentication"></a><span data-ttu-id="70230-106">Ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="70230-106">Windows Authentication</span></span>

<span data-ttu-id="70230-107">Ověřování Windows závisí na operačním systému k ověření uživatelů z aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70230-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="70230-108">Ověřování Windows můžete použít, pokud váš server běží v podnikové síti pomocí identit domény služby Active Directory nebo jiné účty Windows k identifikaci uživatelů.</span><span class="sxs-lookup"><span data-stu-id="70230-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="70230-109">Ověřování Windows je nejvhodnější pro prostředí intranetu, ve kterých uživatelé, klientské aplikace a webové servery patří do stejné domény Windows.</span><span class="sxs-lookup"><span data-stu-id="70230-109">Windows Authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="70230-110">[Další informace o ověřování Windows a nainstalovat ho pro službu IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="70230-110">[Learn more about Windows Authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="70230-111">Povolení ověřování Windows v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70230-111">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="70230-112">Šablony Visual Studio webové aplikace mohou být nakonfigurované pro podporu ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="70230-112">The Visual Studio Web Application template can be configured to support Windows Authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="70230-113">Použití šablony aplikace ověřování Windows</span><span class="sxs-lookup"><span data-stu-id="70230-113">Use the Windows Authentication app template</span></span>

<span data-ttu-id="70230-114">V sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="70230-114">In Visual Studio:</span></span>

1. <span data-ttu-id="70230-115">Vytvořte novou webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70230-115">Create a new ASP.NET Core Web Application.</span></span>
1. <span data-ttu-id="70230-116">Vyberte webovou aplikaci ze seznamu šablon.</span><span class="sxs-lookup"><span data-stu-id="70230-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="70230-117">Vyberte **změna ověřování** tlačítko a vyberte **ověřování Windows**.</span><span class="sxs-lookup"><span data-stu-id="70230-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="70230-118">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70230-118">Run the app.</span></span> <span data-ttu-id="70230-119">Uživatelské jméno se zobrazí v horní části přímo z aplikace.</span><span class="sxs-lookup"><span data-stu-id="70230-119">The username appears in the top right of the app.</span></span>

![Snímek obrazovky prohlížeče ověřování Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="70230-121">Při vývojových pracích pomocí služby IIS Express Šablona nabízí veškeré konfigurace nezbytné pro použití ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="70230-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows Authentication.</span></span> <span data-ttu-id="70230-122">Následující část popisuje postup při ruční konfiguraci aplikace ASP.NET Core pro ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="70230-122">The following section shows how to manually configure an ASP.NET Core app for Windows Authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="70230-123">Nastavení sady Visual Studio pro Windows a anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="70230-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="70230-124">Projekt aplikace Visual Studio **vlastnosti** stránky **ladění** karta obsahuje zaškrtávací políčka pro ověřování Windows a anonymní ověřování.</span><span class="sxs-lookup"><span data-stu-id="70230-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows Authentication and anonymous authentication.</span></span>

![Snímek obrazovky prohlížeče ověřování Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="70230-126">Alternativně se dá nakonfigurovat tyto dvě vlastnosti v *launchSettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="70230-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="70230-127">Povolení ověřování Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="70230-127">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="70230-128">Služba IIS použije [modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) pro hostování aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70230-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="70230-129">Ověřování Windows konfigurován ve službě IIS, ne aplikace.</span><span class="sxs-lookup"><span data-stu-id="70230-129">Windows Authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="70230-130">Následující části vysvětlují, jak pomocí Správce služby IIS ke konfiguraci aplikace ASP.NET Core používat ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="70230-130">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows Authentication.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="70230-131">Konfigurace služby IIS</span><span class="sxs-lookup"><span data-stu-id="70230-131">IIS configuration</span></span>

<span data-ttu-id="70230-132">Povolte službu Role služby IIS pro ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="70230-132">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="70230-133">Další informace najdete v tématu [povolit ověřování Windows služby Role služby IIS (viz krok 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="70230-133">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="70230-134">Middleware pro integraci služby IIS je ve výchozím nastavení nakonfigurované k automatickému ověření žádosti.</span><span class="sxs-lookup"><span data-stu-id="70230-134">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="70230-135">Další informace najdete v tématu [hostitele ASP.NET Core ve Windows se službou IIS: IIS možnosti (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="70230-135">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="70230-136">Modul ASP.NET Core je ve výchozím nastavení nakonfigurovaný pro předávání Windows ověřovací token do aplikace.</span><span class="sxs-lookup"><span data-stu-id="70230-136">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="70230-137">Další informace najdete v tématu [odkaz Konfigurace modul ASP.NET Core: atributy elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="70230-137">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="70230-138">Vytvořit nový web služby IIS</span><span class="sxs-lookup"><span data-stu-id="70230-138">Create a new IIS site</span></span>

<span data-ttu-id="70230-139">Zadejte název a složku a mohla vytvořit nový fond aplikací.</span><span class="sxs-lookup"><span data-stu-id="70230-139">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="70230-140">Přizpůsobení ověřování</span><span class="sxs-lookup"><span data-stu-id="70230-140">Customize authentication</span></span>

<span data-ttu-id="70230-141">Otevřete funkcích ověřování pro lokalitu.</span><span class="sxs-lookup"><span data-stu-id="70230-141">Open the Authentication features for the site.</span></span>

![Nabídka ověřování služby IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="70230-143">Zakázat anonymní ověřování a povolit ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="70230-143">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Nastavení ověřování služby IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="70230-145">Publikovat projekt do složky webu služby IIS</span><span class="sxs-lookup"><span data-stu-id="70230-145">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="70230-146">Pomocí sady Visual Studio nebo rozhraní příkazového řádku .NET Core, publikujte aplikaci do cílové složky.</span><span class="sxs-lookup"><span data-stu-id="70230-146">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Dialogové okno pro publikování aplikace Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="70230-148">Další informace o [publikování do služby IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="70230-148">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="70230-149">Spuštění aplikace a zkontrolujte, že funguje ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="70230-149">Launch the app to verify Windows Authentication is working.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="70230-150">Povolení ověřování Windows pomocí HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="70230-150">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="70230-151">Přestože Kestrel nepodporuje ověřování Windows, můžete použít [HTTP.sys](xref:fundamentals/servers/httpsys) pro zajištění podpory scénářů v místním prostředí ve Windows.</span><span class="sxs-lookup"><span data-stu-id="70230-151">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="70230-152">Následující příklad nastaví hostitel webové aplikace pro použití HTTP.sys s ověřováním Windows:</span><span class="sxs-lookup"><span data-stu-id="70230-152">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="70230-153">Ovladač HTTP.sys delegáty pro ověřování v režimu jádra ověřování protokolem Kerberos.</span><span class="sxs-lookup"><span data-stu-id="70230-153">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="70230-154">Režim ověřování uživatele nepodporuje protokolů Kerberos a HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="70230-154">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="70230-155">Účet počítače musí být použité k dešifrování token/lístek služby Kerberos, která se získá z Active Directory a předá klienta na serveru k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="70230-155">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="70230-156">Zaregistrujte hlavní název služby (SPN) příslušného hostitele není uživatel aplikace.</span><span class="sxs-lookup"><span data-stu-id="70230-156">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a><span data-ttu-id="70230-157">Povolení ověřování Windows pomocí WebListener</span><span class="sxs-lookup"><span data-stu-id="70230-157">Enable Windows Authentication with WebListener</span></span>

<span data-ttu-id="70230-158">Přestože Kestrel nepodporuje ověřování Windows, můžete použít [WebListener](xref:fundamentals/servers/weblistener) pro zajištění podpory scénářů v místním prostředí ve Windows.</span><span class="sxs-lookup"><span data-stu-id="70230-158">Although Kestrel doesn't support Windows Authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="70230-159">Následující příklad nastaví hostitel webové aplikace pro použití WebListener s ověřováním Windows:</span><span class="sxs-lookup"><span data-stu-id="70230-159">The following example configures the app's web host to use WebListener with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> <span data-ttu-id="70230-160">WebListener delegáty pro ověřování v režimu jádra ověřování protokolem Kerberos.</span><span class="sxs-lookup"><span data-stu-id="70230-160">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="70230-161">Režim ověřování uživatele nepodporuje protokolů Kerberos a WebListener.</span><span class="sxs-lookup"><span data-stu-id="70230-161">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="70230-162">Účet počítače musí být použité k dešifrování token/lístek služby Kerberos, která se získá z Active Directory a předá klienta na serveru k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="70230-162">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="70230-163">Zaregistrujte hlavní název služby (SPN) příslušného hostitele není uživatel aplikace.</span><span class="sxs-lookup"><span data-stu-id="70230-163">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

## <a name="work-with-windows-authentication"></a><span data-ttu-id="70230-164">Práce s ověřováním Windows</span><span class="sxs-lookup"><span data-stu-id="70230-164">Work with Windows Authentication</span></span>

<span data-ttu-id="70230-165">Stav konfigurace anonymního přístupu určuje způsob, jakým `[Authorize]` a `[AllowAnonymous]` atributy se používají v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70230-165">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="70230-166">Následující dvě části popisují, jak zpracovat stavy zakázaných a povolených Konfigurace anonymního přístupu.</span><span class="sxs-lookup"><span data-stu-id="70230-166">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="70230-167">Zakázat anonymní přístup</span><span class="sxs-lookup"><span data-stu-id="70230-167">Disallow anonymous access</span></span>

<span data-ttu-id="70230-168">Když je povolené ověřování Windows a je zakázán anonymní přístup, `[Authorize]` a `[AllowAnonymous]` atributy nemají žádný účinek.</span><span class="sxs-lookup"><span data-stu-id="70230-168">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="70230-169">Pokud web služby IIS (nebo HTTP.sys nebo WebListener server) je konfigurována tak anonymní přístup, požadavek dosáhne nikdy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="70230-169">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="70230-170">Z tohoto důvodu `[AllowAnonymous]` atributu nelze použít.</span><span class="sxs-lookup"><span data-stu-id="70230-170">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="70230-171">Povolení anonymního přístupu</span><span class="sxs-lookup"><span data-stu-id="70230-171">Allow anonymous access</span></span>

<span data-ttu-id="70230-172">Pokud jsou povolené ověřování Windows a anonymní přístup, použijte `[Authorize]` a `[AllowAnonymous]` atributy.</span><span class="sxs-lookup"><span data-stu-id="70230-172">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="70230-173">`[Authorize]` Atribut umožňuje zabezpečit části aplikace, které skutečně vyžadují ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="70230-173">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="70230-174">`[AllowAnonymous]` Atribut přepsání `[Authorize]` atribut využití v aplikacích, které umožňují anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="70230-174">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="70230-175">Zobrazit [jednoduchá autorizace](xref:security/authorization/simple) podrobnosti o použití atributu.</span><span class="sxs-lookup"><span data-stu-id="70230-175">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="70230-176">V ASP.NET Core 2.x, `[Authorize]` atribut vyžaduje další konfiguraci ve *Startup.cs* vybízí anonymních požadavků pro ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="70230-176">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="70230-177">Doporučená konfigurace mírně závisí na webovém serveru, který je používán.</span><span class="sxs-lookup"><span data-stu-id="70230-177">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="70230-178">Ve výchozím nastavení uživatelé, kteří nemají oprávnění k získání přístupu ke stránce se zobrazí prázdnou odpověď HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="70230-178">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="70230-179">[StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) umožňují uživatelům poskytovat lepší prostředí "Přístup byl odepřen".</span><span class="sxs-lookup"><span data-stu-id="70230-179">The [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="70230-180">IIS</span><span class="sxs-lookup"><span data-stu-id="70230-180">IIS</span></span>

<span data-ttu-id="70230-181">Pokud používáte službu IIS, přidejte následující text do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="70230-181">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="70230-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="70230-182">HTTP.sys</span></span>

<span data-ttu-id="70230-183">Pokud používáte HTTP.sys, přidejte následující text do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="70230-183">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="70230-184">Zosobnění</span><span class="sxs-lookup"><span data-stu-id="70230-184">Impersonation</span></span>

<span data-ttu-id="70230-185">ASP.NET Core neimplementuje zosobnění.</span><span class="sxs-lookup"><span data-stu-id="70230-185">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="70230-186">Aplikace běží s identitou aplikace pro všechny požadavky pomocí aplikace identity fondu nebo procesu.</span><span class="sxs-lookup"><span data-stu-id="70230-186">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="70230-187">Pokud je třeba explicitně provést akce jménem uživatele, použijte `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="70230-187">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="70230-188">V tomto kontextu spuštění jedné akce a potom zavřete kontextu.</span><span class="sxs-lookup"><span data-stu-id="70230-188">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="70230-189">Všimněte si, že `RunImpersonated` nepodporuje asynchronní operace by se neměl používat pro komplexní scénáře.</span><span class="sxs-lookup"><span data-stu-id="70230-189">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="70230-190">Například obtékání celý požadavky nebo middleware zřetězen není podporován nebo doporučené.</span><span class="sxs-lookup"><span data-stu-id="70230-190">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
