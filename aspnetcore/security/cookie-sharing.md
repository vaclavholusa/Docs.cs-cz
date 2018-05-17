---
title: Sdílet soubory cookie mezi aplikace s ASP.NET a ASP.NET Core
author: rick-anderson
description: Naučte se sdílet soubory cookie pro ověřování mezi ASP.NET 4.x a ASP.NET Core aplikace.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cookie-sharing
ms.openlocfilehash: 5f77377f168993d48686217adac54a75313766ec
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>Sdílet soubory cookie mezi aplikace s ASP.NET a ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Luke Latham](https://github.com/guardrex)

Weby často obsahovat jednotlivé webové aplikace, které pracují společně. A poskytuje prostředí jednotné přihlašování (SSO), musí webové aplikace v rámci lokality sdílet soubory cookie pro ověřování. Pro podporu tohoto scénáře, zásobník ochrany dat umožňuje sdílení Katana ověřování souborů cookie a ASP.NET Core lístků pro ověřování pomocí souboru cookie.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Ukázka ukazuje soubor cookie sdílení napříč tří aplikací, které používala ověřování souborů cookie:

* Aplikace ASP.NET Core stránky Razor 2.0 bez použití [ASP.NET Core Identity](xref:security/authentication/identity)
* Jádro ASP.NET 2.0 aplikace MVC se službou ASP.NET Core Identity
* Aplikace MVC rozhraní ASP.NET 4.6.1 s ASP.NET Identity

V příkladech, které následují:

* Název souboru cookie ověřování je nastaven na hodnotu běžné `.AspNet.SharedCookie`.
* `AuthenticationType` Je nastaven na `Identity.Application` explicitně nebo ve výchozím nastavení.
* Běžný název aplikace se používá k povolení systému ochrany dat a sdílet klíče ochrany dat (`SharedCookieApp`).
* `Identity.Application` slouží jako schéma ověřování. Ať schéma se používá, musí být konzistentně použity *v rámci a napříč* sdílený soubor cookie aplikace jako výchozí schéma nebo explicitně jeho nastavení. Schéma se používá při šifrování a dešifrování souborů cookie, takže různých aplikacích se musí použít konzistentní schéma.
* Společné [klíč ochrany dat](xref:security/data-protection/implementation/key-management) umístění úložiště se používá. Ukázková aplikace používá složku s názvem *Správce klíčů* v kořenovém adresáři řešení pro ochranu dat klíče.
* V aplikacích ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) se používá k nastavení umístění úložiště klíčů. [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) slouží ke konfiguraci běžný název sdílené aplikace.
* V rozhraní .NET Framework aplikaci middleware ověřování souborů cookie používá provádění [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` poskytuje služby ochrany dat pro šifrování a dešifrování dat datové části souboru cookie ověřování. `DataProtectionProvider` Instance je izolovaná od systému ochrany dat používá dalších částí aplikace.
  * [DataProtectionProvider.Create (System.IO.DirectoryInfo, akce\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) přijme [DirectoryInfo](/dotnet/api/system.io.directoryinfo) zadat umístění pro úložiště dat ochrany klíčů. Ukázková aplikace obsahuje cestu *Správce klíčů* složku pro `DirectoryInfo`. [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) nastaví běžný název aplikace.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) vyžaduje [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) balíček NuGet. Pokud chcete získat tento balíček pro technologii ASP.NET 2.0 jádra a novější aplikace, odkazovat [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. Při cílení na rozhraní .NET Framework, přidat odkaz na balíček `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Sdílet soubory cookie pro ověřování mezi aplikací ASP.NET Core

Při použití ASP.NET Core Identity:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

V `ConfigureServices` metoda, použijte [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) metody rozšíření pro nastavení služby ochrany dat pro soubory cookie.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Klíče ochrany dat a název aplikace musí být sdílená mezi aplikací. V ukázkových aplikací `GetKeyRingDirInfo` vrátí společné umístění úložiště klíčů pro [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metoda. Použití [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) konfigurace běžný název sdílené aplikace (`SharedCookieApp` v ukázce). Další informace najdete v tématu [konfiguraci ochrany dat](xref:security/data-protection/configuration/overview).

Najdete v článku *CookieAuthWithIdentity.Core* v projektu [ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

V `Configure` metoda, použijte [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) nastavit:

* Služba ochrany dat pro soubory cookie.
* `AuthenticationScheme` Tak, aby odpovídaly ASP.NET 4.x.

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = 
        "ApplicationCookie";

    var protectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"));

    options.Cookies.ApplicationCookie.DataProtectionProvider = 
        protectionProvider;

    options.Cookies.ApplicationCookie.TicketDataFormat = 
        new TicketDataFormat(protectionProvider.CreateProtector(
            "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", 
            "Cookies", 
            "v2"));
});
```

---

Při používání souborů cookie přímo:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

Klíče ochrany dat a název aplikace musí být sdílená mezi aplikací. V ukázkových aplikací `GetKeyRingDirInfo` vrátí společné umístění úložiště klíčů pro [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metoda. Použití [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) konfigurace běžný název sdílené aplikace (`SharedCookieApp` v ukázce). Další informace najdete v tématu [konfiguraci ochrany dat](xref:security/data-protection/configuration/overview). 

Najdete v článku *CookieAuth.Core* v projektu [ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a>Šifrování klíče ochrany dat v klidovém stavu

Pro nasazení v produkčním prostředí, nakonfigurovat `DataProtectionProvider` k šifrování klíče v klidovém stavu pomocí rozhraní DPAPI nebo certifikátu x 509. V tématu [klíč šifrování na Rest](xref:security/data-protection/implementation/key-encryption-at-rest) Další informace.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(
        new DirectoryInfo(@"PATH_TO_KEY_RING"),
        configure =>
        {
            configure.ProtectKeysWithCertificate("thumbprint");
        })
});
```

---

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Sdílení souborů cookie ověřování mezi ASP.NET 4.x a ASP.NET Core aplikace

Aplikace ASP.NET 4.x, která používá Katana middleware ověřování souborů cookie může být nakonfigurováno pro generování ověřovací soubory cookie, které jsou kompatibilní s middleware ověřování souborů cookie ASP.NET Core. To umožňuje piecemeal upgrade velké lokality jednotlivými aplikacemi při současném poskytování smooth přihlašováním celém webu.

> [!TIP]
> Pokud aplikace používá Katana middleware ověřování souborů cookie, zavolá `UseCookieAuthentication` v projektu *Startup.Auth.cs* souboru. Projekty ASP.NET 4.x webové aplikace vytvořené pomocí sady Visual Studio 2013 a později middleware ověřování souborů cookie Katana ve výchozím nastavení.

> [!NOTE]
> Aplikace ASP.NET 4.x musí mít jako cíl rozhraní .NET Framework 4.5.1 nebo novější. Potřebné balíčky NuGet, jinak hodnota nepodaří nainstalovat.

Sdílet soubory cookie pro ověřování mezi ASP.NET 4.x aplikace a aplikace ASP.NET Core, konfiguraci aplikace ASP.NET Core, jak je uvedeno výše a potom nakonfigurujte aplikace ASP.NET 4.x podle následujících kroků.

1. Nainstalujte balíček [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) do každé aplikace ASP.NET 4.x.

2. V *Startup.Auth.cs*, vyhledejte volání `UseCookieAuthentication` a upravit ho následujícím způsobem. Změňte název souboru cookie, aby odpovídal názvu používané middleware ověřování souborů cookie ASP.NET Core. Zadejte instanci `DataProtectionProvider` inicializována tak, aby společné umístění dat ochrany úložiště klíčů. Ujistěte se, že název aplikace je nastavena na běžný název aplikace, které jsou používané všechny aplikace, které sdílejí soubory cookie, `SharedCookieApp` v ukázkové aplikace.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

Najdete v článku *CookieAuthWithIdentity.NETFramework* v projektu [ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample)).

Při generování identitu uživatele, typ ověřování se musí shodovat s typem definovaným v `AuthenticationType` s `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Nastavte `CookieManager` zprostředkovatele komunikace s objekty `ChunkingCookieManager` tak Formát bloku dat je kompatibilní.

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
    CookieName = ".AspNetCore.Cookies",
    // CookieName = ".AspNetCore.ApplicationCookie", (if using ASP.NET Identity)
    // CookiePath = "...", (if necessary)
    // ...
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create(
                new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", 
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});
```

---

## <a name="use-a-common-user-database"></a>Použít společnou databázi uživatele

Potvrďte, že je na stejné uživatele databáze odkazoval systém identit pro každou aplikaci. Systém identit, jinak hodnota vytvoří chyby za běhu, když pokusí se porovnat informace v souboru cookie pro ověřování proti informací ve své databázi.
