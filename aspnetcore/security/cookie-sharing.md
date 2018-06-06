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
ms.openlocfilehash: f6d62d5f6e446e3e2001ed6bde72a6c409aa2833
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734676"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="145c3-103">Sdílet soubory cookie mezi aplikace s ASP.NET a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="145c3-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="145c3-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="145c3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="145c3-105">Weby často obsahovat jednotlivé webové aplikace, které pracují společně.</span><span class="sxs-lookup"><span data-stu-id="145c3-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="145c3-106">A poskytuje prostředí jednotné přihlašování (SSO), musí webové aplikace v rámci lokality sdílet soubory cookie pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="145c3-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="145c3-107">Pro podporu tohoto scénáře, zásobník ochrany dat umožňuje sdílení Katana ověřování souborů cookie a ASP.NET Core lístků pro ověřování pomocí souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="145c3-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="145c3-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="145c3-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="145c3-109">Ukázka ukazuje soubor cookie sdílení napříč tří aplikací, které používala ověřování souborů cookie:</span><span class="sxs-lookup"><span data-stu-id="145c3-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="145c3-110">Aplikace ASP.NET Core stránky Razor 2.0 bez použití [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="145c3-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="145c3-111">Jádro ASP.NET 2.0 aplikace MVC se službou ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="145c3-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="145c3-112">Aplikace MVC rozhraní ASP.NET 4.6.1 s ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="145c3-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="145c3-113">V příkladech, které následují:</span><span class="sxs-lookup"><span data-stu-id="145c3-113">In the examples that follow:</span></span>

* <span data-ttu-id="145c3-114">Název souboru cookie ověřování je nastaven na hodnotu běžné `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="145c3-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="145c3-115">`AuthenticationType` Je nastaven na `Identity.Application` explicitně nebo ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="145c3-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="145c3-116">Běžný název aplikace se používá k povolení systému ochrany dat a sdílet klíče ochrany dat (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="145c3-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="145c3-117">`Identity.Application` slouží jako schéma ověřování.</span><span class="sxs-lookup"><span data-stu-id="145c3-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="145c3-118">Ať schéma se používá, musí být konzistentně použity *v rámci a napříč* sdílený soubor cookie aplikace jako výchozí schéma nebo explicitně jeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="145c3-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="145c3-119">Schéma se používá při šifrování a dešifrování souborů cookie, takže různých aplikacích se musí použít konzistentní schéma.</span><span class="sxs-lookup"><span data-stu-id="145c3-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="145c3-120">Společné [klíč ochrany dat](xref:security/data-protection/implementation/key-management) umístění úložiště se používá.</span><span class="sxs-lookup"><span data-stu-id="145c3-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="145c3-121">Ukázková aplikace používá složku s názvem *Správce klíčů* v kořenovém adresáři řešení pro ochranu dat klíče.</span><span class="sxs-lookup"><span data-stu-id="145c3-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="145c3-122">V aplikacích ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) se používá k nastavení umístění úložiště klíčů.</span><span class="sxs-lookup"><span data-stu-id="145c3-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="145c3-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) slouží ke konfiguraci běžný název sdílené aplikace.</span><span class="sxs-lookup"><span data-stu-id="145c3-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="145c3-124">V rozhraní .NET Framework aplikaci middleware ověřování souborů cookie používá provádění [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="145c3-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="145c3-125">`DataProtectionProvider` poskytuje služby ochrany dat pro šifrování a dešifrování dat datové části souboru cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="145c3-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="145c3-126">`DataProtectionProvider` Instance je izolovaná od systému ochrany dat používá dalších částí aplikace.</span><span class="sxs-lookup"><span data-stu-id="145c3-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="145c3-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo, akce\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) přijme [DirectoryInfo](/dotnet/api/system.io.directoryinfo) zadat umístění pro úložiště dat ochrany klíčů.</span><span class="sxs-lookup"><span data-stu-id="145c3-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="145c3-128">Ukázková aplikace obsahuje cestu *Správce klíčů* složku pro `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="145c3-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="145c3-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) nastaví běžný název aplikace.</span><span class="sxs-lookup"><span data-stu-id="145c3-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="145c3-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) vyžaduje [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="145c3-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="145c3-131">Pokud chcete získat tento balíček pro ASP.NET Core 2.1 a novější aplikace, odkazovat [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="145c3-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="145c3-132">Při cílení na rozhraní .NET Framework, přidat odkaz na balíček `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="145c3-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="145c3-133">Sdílet soubory cookie pro ověřování mezi aplikací ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="145c3-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="145c3-134">Při použití ASP.NET Core Identity:</span><span class="sxs-lookup"><span data-stu-id="145c3-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="145c3-135">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="145c3-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="145c3-136">V `ConfigureServices` metoda, použijte [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) metody rozšíření pro nastavení služby ochrany dat pro soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="145c3-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="145c3-137">Klíče ochrany dat a název aplikace musí být sdílená mezi aplikací.</span><span class="sxs-lookup"><span data-stu-id="145c3-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="145c3-138">V ukázkových aplikací `GetKeyRingDirInfo` vrátí společné umístění úložiště klíčů pro [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metoda.</span><span class="sxs-lookup"><span data-stu-id="145c3-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="145c3-139">Použití [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) konfigurace běžný název sdílené aplikace (`SharedCookieApp` v ukázce).</span><span class="sxs-lookup"><span data-stu-id="145c3-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="145c3-140">Další informace najdete v tématu [konfiguraci ochrany dat](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="145c3-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="145c3-141">Najdete v článku *CookieAuthWithIdentity.Core* v projektu [ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="145c3-141">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="145c3-142">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="145c3-142">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="145c3-143">V `Configure` metoda, použijte [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) nastavit:</span><span class="sxs-lookup"><span data-stu-id="145c3-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="145c3-144">Služba ochrany dat pro soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="145c3-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="145c3-145">`AuthenticationScheme` Tak, aby odpovídaly ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="145c3-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="145c3-146">Při používání souborů cookie přímo:</span><span class="sxs-lookup"><span data-stu-id="145c3-146">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="145c3-147">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="145c3-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="145c3-148">Klíče ochrany dat a název aplikace musí být sdílená mezi aplikací.</span><span class="sxs-lookup"><span data-stu-id="145c3-148">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="145c3-149">V ukázkových aplikací `GetKeyRingDirInfo` vrátí společné umístění úložiště klíčů pro [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metoda.</span><span class="sxs-lookup"><span data-stu-id="145c3-149">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="145c3-150">Použití [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) konfigurace běžný název sdílené aplikace (`SharedCookieApp` v ukázce).</span><span class="sxs-lookup"><span data-stu-id="145c3-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="145c3-151">Další informace najdete v tématu [konfiguraci ochrany dat](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="145c3-151">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span> 

<span data-ttu-id="145c3-152">Najdete v článku *CookieAuth.Core* v projektu [ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="145c3-152">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="145c3-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="145c3-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="145c3-154">Šifrování klíče ochrany dat v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="145c3-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="145c3-155">Pro nasazení v produkčním prostředí, nakonfigurovat `DataProtectionProvider` k šifrování klíče v klidovém stavu pomocí rozhraní DPAPI nebo certifikátu x 509.</span><span class="sxs-lookup"><span data-stu-id="145c3-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="145c3-156">V tématu [klíč šifrování na Rest](xref:security/data-protection/implementation/key-encryption-at-rest) Další informace.</span><span class="sxs-lookup"><span data-stu-id="145c3-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="145c3-157">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="145c3-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="145c3-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="145c3-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="145c3-159">Sdílení souborů cookie ověřování mezi ASP.NET 4.x a ASP.NET Core aplikace</span><span class="sxs-lookup"><span data-stu-id="145c3-159">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="145c3-160">Aplikace ASP.NET 4.x, která používá Katana middleware ověřování souborů cookie může být nakonfigurováno pro generování ověřovací soubory cookie, které jsou kompatibilní s middleware ověřování souborů cookie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="145c3-160">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="145c3-161">To umožňuje piecemeal upgrade velké lokality jednotlivými aplikacemi při současném poskytování smooth přihlašováním celém webu.</span><span class="sxs-lookup"><span data-stu-id="145c3-161">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="145c3-162">Pokud aplikace používá Katana middleware ověřování souborů cookie, zavolá `UseCookieAuthentication` v projektu *Startup.Auth.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="145c3-162">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="145c3-163">Projekty ASP.NET 4.x webové aplikace vytvořené pomocí sady Visual Studio 2013 a později middleware ověřování souborů cookie Katana ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="145c3-163">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="145c3-164">Aplikace ASP.NET 4.x musí mít jako cíl rozhraní .NET Framework 4.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="145c3-164">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="145c3-165">Potřebné balíčky NuGet, jinak hodnota nepodaří nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="145c3-165">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="145c3-166">Sdílet soubory cookie pro ověřování mezi ASP.NET 4.x aplikace a aplikace ASP.NET Core, konfiguraci aplikace ASP.NET Core, jak je uvedeno výše a potom nakonfigurujte aplikace ASP.NET 4.x podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="145c3-166">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="145c3-167">Nainstalujte balíček [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) do každé aplikace ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="145c3-167">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="145c3-168">V *Startup.Auth.cs*, vyhledejte volání `UseCookieAuthentication` a upravit ho následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="145c3-168">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="145c3-169">Změňte název souboru cookie, aby odpovídal názvu používané middleware ověřování souborů cookie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="145c3-169">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="145c3-170">Zadejte instanci `DataProtectionProvider` inicializována tak, aby společné umístění dat ochrany úložiště klíčů.</span><span class="sxs-lookup"><span data-stu-id="145c3-170">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="145c3-171">Ujistěte se, že název aplikace je nastavena na běžný název aplikace, které jsou používané všechny aplikace, které sdílejí soubory cookie, `SharedCookieApp` v ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="145c3-171">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="145c3-172">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="145c3-172">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="145c3-173">Najdete v článku *CookieAuthWithIdentity.NETFramework* v projektu [ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="145c3-173">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="145c3-174">Při generování identitu uživatele, typ ověřování se musí shodovat s typem definovaným v `AuthenticationType` s `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="145c3-174">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="145c3-175">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="145c3-175">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="145c3-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="145c3-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="145c3-177">Nastavte `CookieManager` zprostředkovatele komunikace s objekty `ChunkingCookieManager` tak Formát bloku dat je kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="145c3-177">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="145c3-178">Použít společnou databázi uživatele</span><span class="sxs-lookup"><span data-stu-id="145c3-178">Use a common user database</span></span>

<span data-ttu-id="145c3-179">Potvrďte, že je na stejné uživatele databáze odkazoval systém identit pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="145c3-179">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="145c3-180">Systém identit, jinak hodnota vytvoří chyby za běhu, když pokusí se porovnat informace v souboru cookie pro ověřování proti informací ve své databázi.</span><span class="sxs-lookup"><span data-stu-id="145c3-180">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
