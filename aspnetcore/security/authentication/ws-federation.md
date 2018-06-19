---
title: Ověřuje uživatele pomocí protokolu WS-Federation v ASP.NET Core
author: chlowell
description: Tento kurz ukazuje, jak používat v aplikaci ASP.NET Core WS-Federation.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: d4621c7b97678903b9f2562e353da3883334b599
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898801"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Ověřuje uživatele pomocí protokolu WS-Federation v ASP.NET Core

Tento kurz ukazuje, jak povolit uživatelům přihlásit se přes zprostředkovatele ověřování WS-Federation jako je Active Directory Federation Services (ADFS) nebo [Azure Active Directory](/azure/active-directory/) (AAD). Používá technologii ASP.NET 2.0 základní ukázková aplikace popsané v [Facebook, Google a externí zprostředkovatel ověřování](xref:security/authentication/social/index).

Pro aplikace ASP.NET Core 2.0 WS-Federation podpora je k dispozici ve [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Tato součást je přenášet mezi [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) a celá řada mechanismy tuto součást. Komponenty se však lišit v několika důležitých způsoby.

Ve výchozím nastavení nové middleware:

* Neumožňuje nevyžádané přihlášení. Tato funkce protokolu WS-Federation je ohrožena útoky XSRF. Ale lze je aktivovat pomocí `AllowUnsolicitedLogins` možnost.
* Neohlásí každých post formuláře pro přihlašování zprávy. Jenom žádosti `CallbackPath` zkontrolují sign in `CallbackPath` výchozí `/signin-wsfed` lze ji však změnit. Tato cesta je možné sdílet s dalších zprostředkovatelů ověřování povolením `SkipUnrecognizedRequests` možnost.

## <a name="register-the-app-with-active-directory"></a>Zaregistrovat aplikaci služby Active Directory

### <a name="active-directory-federation-services"></a>Služba Active Directory Federation Services

* Otevřete serveru **předávající strany Průvodce přidáním vztahu důvěryhodnosti** z konzoly pro správu služby AD FS:

![Přidat vztah důvěryhodnosti předávající strany Průvodce: úvodní](ws-federation/_static/AdfsAddTrust.png)

* Vyberte ručně zadat data:

![Přidání průvodce předávající strany vztahu důvěryhodnosti: Vyberte zdroj dat](ws-federation/_static/AdfsSelectDataSource.png)

* Zadejte zobrazovaný název předávající strany. Název není důležité, abyste aplikaci ASP.NET Core.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) chybí podporu pro šifrování tokenu, takže nemusíte nakonfigurovat certifikát šifrování tokenů:

![Přidání průvodce předávající strany vztahu důvěryhodnosti: Konfigurace certifikátu](ws-federation/_static/AdfsConfigureCert.png)

* Povolte podporu pasivního protokolu WS-Federation protokolu, pomocí adresy URL aplikace. Ověřte, že port je správný pro aplikaci:

![Přidání průvodce předávající strany vztahu důvěryhodnosti: Konfigurace adresy URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Toto musí být adresu URL HTTPS. Služba IIS Express můžete zadat certifikát podepsaný svým držitelem při hostování aplikace během vývoje. Kestrel vyžaduje konfiguraci certifikáty ručně. Najdete v článku [Kestrel dokumentace](xref:fundamentals/servers/kestrel) další podrobnosti.

* Klikněte na tlačítko **Další** procházení zbývající části v průvodci a **Zavřít** na konci.

* ASP.NET Core Identity vyžaduje **ID názvu** deklarací identity. Přidejte jedno z **upravit pravidla deklarací identity** dialogové okno:

![Upravit pravidla deklarace identity](ws-federation/_static/EditClaimRules.png)

* V **průvodci Přidat transformované deklarace identity pravidlo**, ponechte výchozí nastavení **odesílat atributy LDAP jako deklarace identity** vybraná šablona a klikněte na **Další**. Přidat pravidlo mapování **název účtu SAM** atributu LDAP **ID názvu** odchozí deklarace identity:

![Přidání průvodce pravidla transformace deklarací identity: Konfigurace pravidla deklarace identity](ws-federation/_static/AddTransformClaimRule.png)

* Klikněte na tlačítko **Dokončit** > **OK** v **upravit pravidla deklarací identity** okno.

### <a name="azure-active-directory"></a>Azure Active Directory

* Přejděte do okna registrace klienta AAD aplikace. Klikněte na tlačítko **nové registrace aplikace**:

![Azure Active Directory: Registrace aplikace](ws-federation/_static/AadNewAppRegistration.png)

* Zadejte název pro registraci aplikace. Tato akce není důležité, abyste aplikaci ASP.NET Core.
* Zadejte adresu URL aplikace naslouchá na jako **přihlašovací adresa URL**:

![Azure Active Directory: Registrace aplikací vytvořit](ws-federation/_static/AadCreateAppRegistration.png)

* Klikněte na tlačítko **koncové body** a poznamenejte si **dokument federačních metadat** adresy URL. Toto je WS-Federation middleware `MetadataAddress`:

![Azure Active Directory: Endpoints](ws-federation/_static/AadFederationMetadataDocument.png)

* Přejděte do nové aplikace registrace. Klikněte na tlačítko **nastavení** > **vlastnosti** a poznamenejte si **identifikátor ID URI aplikace**. Toto je WS-Federation middleware `Wtrealm`:

![Azure Active Directory: Vlastnosti registrace aplikace](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Přidat jako poskytovatel externí přihlášení pro ASP.NET Core Identity WS-Federation

* Přidat závislost na [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) do projektu.
* Přidat WS-Federation k `Configure` metoda v *Startup.cs*:

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

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>Přihlaste se pomocí protokolu WS-Federation

Přejděte na aplikace a klikněte na tlačítko **přihlásit** odkaz v hlavičce navigaci. Je k dispozici možnost se přihlásit WsFederation: ![s přihlašovací stránkou](ws-federation/_static/WsFederationButton.png)

Se službou AD FS jako zprostředkovatel tlačítko přesměruje na přihlašovací stránku služby AD FS: ![přihlašovací stránku služby AD FS](ws-federation/_static/AdfsLoginPage.png)

S Azure Active Directory jako zprostředkovatel, tlačítko přesměruje na přihlašovací stránku služby AAD: ![AAD přihlašovací stránky](ws-federation/_static/AadSignIn.png)

U úspěšné přihlášení pro nového uživatele přesměruje na stránku registrace uživatele aplikace: ![stránku registrace](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Pomocí protokolu WS-Federation bez ASP.NET Core Identity

Middleware WS-Federation můžete použít bez Identity. Příklad:

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
