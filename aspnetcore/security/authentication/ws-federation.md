---
title: "Ověřuje uživatele pomocí protokolu WS-Federation v ASP.NET Core"
author: chlowell
description: "Tento kurz ukazuje, jak používat v aplikaci ASP.NET Core WS-Federation."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: 0532f866e9c58b2e45623f522f62438e15017e54
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="6c228-103">Ověřuje uživatele pomocí protokolu WS-Federation v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c228-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="6c228-104">Tento kurz ukazuje, jak povolit uživatelům přihlásit se přes zprostředkovatele ověřování WS-Federation jako je Active Directory Federation Services (ADFS) nebo [Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="6c228-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="6c228-105">Používá technologii ASP.NET 2.0 základní ukázková aplikace popsané v [Facebook, Google a externí zprostředkovatel ověřování](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="6c228-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="6c228-106">Pro aplikace ASP.NET Core 2.0 WS-Federation podpora je k dispozici ve [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="6c228-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="6c228-107">Tato součást je přenášet mezi [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) a celá řada mechanismy tuto součást.</span><span class="sxs-lookup"><span data-stu-id="6c228-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="6c228-108">Komponenty se však lišit v několika důležitých způsoby.</span><span class="sxs-lookup"><span data-stu-id="6c228-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="6c228-109">Ve výchozím nastavení nové middleware:</span><span class="sxs-lookup"><span data-stu-id="6c228-109">By default, the new middleware:</span></span>

* <span data-ttu-id="6c228-110">Neumožňuje nevyžádané přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6c228-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="6c228-111">Tato funkce protokolu WS-Federation je ohrožena útoky XSRF.</span><span class="sxs-lookup"><span data-stu-id="6c228-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="6c228-112">Ale lze je aktivovat pomocí `AllowUnsolicitedLogins` možnost.</span><span class="sxs-lookup"><span data-stu-id="6c228-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="6c228-113">Neohlásí každých post formuláře pro přihlašování zprávy.</span><span class="sxs-lookup"><span data-stu-id="6c228-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="6c228-114">Jenom žádosti `CallbackPath` zkontrolují sign in `CallbackPath` výchozí `/signin-wsfed` lze ji však změnit.</span><span class="sxs-lookup"><span data-stu-id="6c228-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed.</span></span> <span data-ttu-id="6c228-115">Tato cesta je možné sdílet s dalších zprostředkovatelů ověřování povolením `SkipUnrecognizedRequests` možnost.</span><span class="sxs-lookup"><span data-stu-id="6c228-115">This path can be shared with other authentication providers by enabling the `SkipUnrecognizedRequests` option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="6c228-116">Zaregistrovat aplikaci služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="6c228-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="6c228-117">Služba Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="6c228-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="6c228-118">Otevřete serveru **předávající strany Průvodce přidáním vztahu důvěryhodnosti** z konzoly pro správu služby AD FS:</span><span class="sxs-lookup"><span data-stu-id="6c228-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Přidat vztah důvěryhodnosti předávající strany Průvodce: úvodní](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="6c228-120">Vyberte ručně zadat data:</span><span class="sxs-lookup"><span data-stu-id="6c228-120">Choose to enter data manually:</span></span>

![Přidání průvodce předávající strany vztahu důvěryhodnosti: Vyberte zdroj dat](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="6c228-122">Zadejte zobrazovaný název předávající strany.</span><span class="sxs-lookup"><span data-stu-id="6c228-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="6c228-123">Název není důležité, abyste aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6c228-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="6c228-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) chybí podporu pro šifrování tokenu, takže nemusíte nakonfigurovat certifikát šifrování tokenů:</span><span class="sxs-lookup"><span data-stu-id="6c228-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Přidání průvodce předávající strany vztahu důvěryhodnosti: Konfigurace certifikátu](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="6c228-126">Povolte podporu pasivního protokolu WS-Federation protokolu, pomocí adresy URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c228-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="6c228-127">Ověřte, že port je správný pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="6c228-127">Verify the port is correct for the app:</span></span>

![Přidání průvodce předávající strany vztahu důvěryhodnosti: Konfigurace adresy URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="6c228-129">Toto musí být adresu URL HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6c228-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="6c228-130">Služba IIS Express můžete zadat certifikát podepsaný svým držitelem při hostování aplikace během vývoje.</span><span class="sxs-lookup"><span data-stu-id="6c228-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="6c228-131">Kestrel vyžaduje konfiguraci certifikáty ručně.</span><span class="sxs-lookup"><span data-stu-id="6c228-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="6c228-132">Najdete v článku [Kestrel dokumentace](xref:fundamentals/servers/kestrel) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6c228-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="6c228-133">Klikněte na tlačítko **Další** procházení zbývající části v průvodci a **Zavřít** na konci.</span><span class="sxs-lookup"><span data-stu-id="6c228-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="6c228-134">ASP.NET Core Identity vyžaduje **ID názvu** deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="6c228-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="6c228-135">Přidejte jedno z **upravit pravidla deklarací identity** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="6c228-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Upravit pravidla deklarace identity](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="6c228-137">V **průvodci Přidat transformované deklarace identity pravidlo**, ponechte výchozí nastavení **odesílat atributy LDAP jako deklarace identity** vybraná šablona a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="6c228-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="6c228-138">Přidat pravidlo mapování **název účtu SAM** atributu LDAP **ID názvu** odchozí deklarace identity:</span><span class="sxs-lookup"><span data-stu-id="6c228-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Přidání průvodce pravidla transformace deklarací identity: Konfigurace pravidla deklarace identity](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="6c228-140">Klikněte na tlačítko **Dokončit** > **OK** v **upravit pravidla deklarací identity** okno.</span><span class="sxs-lookup"><span data-stu-id="6c228-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="6c228-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6c228-141">Azure Active Directory</span></span>

* <span data-ttu-id="6c228-142">Přejděte do okna registrace klienta AAD aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c228-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="6c228-143">Klikněte na tlačítko **nové registrace aplikace**:</span><span class="sxs-lookup"><span data-stu-id="6c228-143">Click **New application registration**:</span></span>

![Azure Active Directory: Registrace aplikace](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="6c228-145">Zadejte název pro registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c228-145">Enter a name for the app registration.</span></span> <span data-ttu-id="6c228-146">Tato akce není důležité, abyste aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6c228-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="6c228-147">Zadejte adresu URL aplikace naslouchá na jako **přihlašovací adresa URL**:</span><span class="sxs-lookup"><span data-stu-id="6c228-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory: Registrace aplikací vytvořit](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="6c228-149">Klikněte na tlačítko **koncové body** a poznamenejte si **dokument federačních metadat** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6c228-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="6c228-150">Toto je WS-Federation middleware `MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="6c228-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory: Endpoints](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="6c228-152">Přejděte do nové aplikace registrace.</span><span class="sxs-lookup"><span data-stu-id="6c228-152">Navigate to the new app registration.</span></span> <span data-ttu-id="6c228-153">Klikněte na tlačítko **nastavení** > **vlastnosti** a poznamenejte si **identifikátor ID URI aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6c228-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="6c228-154">Toto je WS-Federation middleware `Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="6c228-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory: Vlastnosti registrace aplikace](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="6c228-156">Přidat jako poskytovatel externí přihlášení pro ASP.NET Core Identity WS-Federation</span><span class="sxs-lookup"><span data-stu-id="6c228-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="6c228-157">Přidat závislost na [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) do projektu.</span><span class="sxs-lookup"><span data-stu-id="6c228-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="6c228-158">Přidat WS-Federation k `Configure` metoda v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6c228-158">Add WS-Federation to the `Configure` method in *Startup.cs*:</span></span>

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE[default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="6c228-159">Přihlaste se pomocí protokolu WS-Federation</span><span class="sxs-lookup"><span data-stu-id="6c228-159">Log in with WS-Federation</span></span>

<span data-ttu-id="6c228-160">Přejděte na aplikace a klikněte na tlačítko **přihlásit** odkaz v hlavičce navigaci.</span><span class="sxs-lookup"><span data-stu-id="6c228-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="6c228-161">Je k dispozici možnost se přihlásit WsFederation: ![s přihlašovací stránkou](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="6c228-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="6c228-162">Se službou AD FS jako zprostředkovatel tlačítko přesměruje na přihlašovací stránku služby AD FS: ![přihlašovací stránku služby AD FS](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="6c228-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="6c228-163">S Azure Active Directory jako zprostředkovatel, tlačítko přesměruje na přihlašovací stránku služby AAD: ![AAD přihlašovací stránky](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="6c228-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="6c228-164">U úspěšné přihlášení pro nového uživatele přesměruje na stránku registrace uživatele aplikace: ![stránku registrace](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="6c228-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="6c228-165">Pomocí protokolu WS-Federation bez ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="6c228-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="6c228-166">Middleware WS-Federation můžete použít bez Identity.</span><span class="sxs-lookup"><span data-stu-id="6c228-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="6c228-167">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6c228-167">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
