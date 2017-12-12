---
title: "Sdílení souborů cookie mezi aplikacemi"
author: rick-anderson
description: "Tento dokument popisuje, jak se zásobníkem ochrany dat podporuje sdílení souborů cookie ověřování mezi ASP.NET 4.x a ASP.NET Core aplikace."
keywords: "Sdílení Core,ASP.NET,cookies,Interop,cookie ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: e92cc81f9362787b7b4bfb44ba26b82182826a59
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="sharing-cookies-between-applications"></a><span data-ttu-id="39baf-104">Sdílení souborů cookie mezi aplikacemi</span><span class="sxs-lookup"><span data-stu-id="39baf-104">Sharing cookies between applications</span></span>

<span data-ttu-id="39baf-105">Webové servery obvykle obsahovat mnoho jednotlivých webových aplikací, všech pracovních společně obchodu.</span><span class="sxs-lookup"><span data-stu-id="39baf-105">Web sites commonly consist of many individual web applications, all working together harmoniously.</span></span> <span data-ttu-id="39baf-106">Pokud vývojář aplikace chce zajistit funkční prostředí jednotného přihlašování, potřebují budete často všechny jiné webové aplikace v rámci lokality lístků pro ověřování mezi sebou sdílet.</span><span class="sxs-lookup"><span data-stu-id="39baf-106">If an application developer wants to provide a good single-sign-on experience, they'll often need all of the different web applications within the site to share authentication tickets between each other.</span></span>

<span data-ttu-id="39baf-107">Pro podporu tohoto scénáře, zásobník ochrany dat umožňuje sdílení Katana ověřování souborů cookie a ASP.NET Core lístků pro ověřování pomocí souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="39baf-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

## <a name="sharing-authentication-cookies-between-applications"></a><span data-ttu-id="39baf-108">Sdílení souborů cookie ověřování mezi aplikacemi</span><span class="sxs-lookup"><span data-stu-id="39baf-108">Sharing authentication cookies between applications</span></span>

<span data-ttu-id="39baf-109">Pokud chcete sdílet soubory cookie pro ověřování mezi dvěma různými aplikacemi ASP.NET Core, nakonfigurujte všech aplikací, které by měly sdílet soubory cookie následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="39baf-109">To share authentication cookies between two different ASP.NET Core applications, configure each application that should share cookies as follows.</span></span>

<span data-ttu-id="39baf-110">Ve vaší konfiguraci metody, použijte CookieAuthenticationOptions nastavit služba ochrany dat pro soubory cookie a AuthenticationScheme tak, aby odpovídaly ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="39baf-110">In your configure method, use the CookieAuthenticationOptions to set up the data protection service for cookies and the AuthenticationScheme to match ASP.NET 4.x.</span></span>

<span data-ttu-id="39baf-111">Pokud používáte identity:</span><span class="sxs-lookup"><span data-stu-id="39baf-111">If you're using identity:</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

<span data-ttu-id="39baf-112">Pokud používáte soubory cookie přímo:</span><span class="sxs-lookup"><span data-stu-id="39baf-112">If you're using cookies directly:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
<span data-ttu-id="39baf-113">`DataProtectionProvider` Vyžaduje `Microsoft.AspNetCore.DataProtection.Extensions` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="39baf-113">The `DataProtectionProvider` requires the `Microsoft.AspNetCore.DataProtection.Extensions` NuGet package.</span></span>

<span data-ttu-id="39baf-114">Pokud se použije tímto způsobem, DirectoryInfo by měla odkazovat na umístění úložiště klíčů konkrétně vyčleněné pro soubory cookie pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="39baf-114">When used in this manner, the DirectoryInfo should point to a key storage location specifically set aside for authentication cookies.</span></span> <span data-ttu-id="39baf-115">Middleware ověřování souborů cookie použije výslovně stanovenou implementace DataProtectionProvider, který je nyní izolované ze systému ochrany dat používá dalších částí aplikace.</span><span class="sxs-lookup"><span data-stu-id="39baf-115">The cookie authentication middleware will use the explicitly provided implementation of the DataProtectionProvider, which is now isolated from the data protection system used by other parts of the application.</span></span> <span data-ttu-id="39baf-116">Název aplikace je ignorována (záměrně Ano, protože se snažíte získat více aplikacím sdílet datové části).</span><span class="sxs-lookup"><span data-stu-id="39baf-116">The application name is ignored (intentionally so, since you're trying to get multiple applications to share payloads).</span></span>

>[!WARNING]
><span data-ttu-id="39baf-117">Měli byste zvážit, tak, aby klíče jsou zašifrovaná přinejmenším, jako v konfiguraci DataProtectionProvider následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="39baf-117">You should consider configuring the DataProtectionProvider such that keys are encrypted at rest, as in the below example.</span></span>
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a><span data-ttu-id="39baf-118">Sdílení souborů cookie ověřování mezi ASP.NET 4.x a ASP.NET Core aplikace</span><span class="sxs-lookup"><span data-stu-id="39baf-118">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core applications</span></span>

<span data-ttu-id="39baf-119">Aplikace ASP.NET 4.x, které využívají middleware ověřování souborů cookie Katana může být nakonfigurováno pro generování ověřovací soubory cookie, které jsou slučitelné s middleware ověřování souborů cookie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="39baf-119">ASP.NET 4.x applications which use Katana cookie authentication middleware can be configured to generate authentication cookies which are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="39baf-120">To umožňuje upgrade velké lokality jednotlivých aplikací piecemeal současně stále zajišťuje bezproblémovou jednotné přihlašování v prostředí celém webu.</span><span class="sxs-lookup"><span data-stu-id="39baf-120">This allows upgrading a large site's individual applications piecemeal while still providing a smooth single sign on experience across the site.</span></span>

>[!TIP]
> <span data-ttu-id="39baf-121">Můžete zadat, pokud aplikace využívá middleware ověřování souborů cookie Katana existencí volání UseCookieAuthentication v Startup.Auth.cs vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="39baf-121">You can tell if your existing application uses Katana cookie authentication middleware by the existence of a call to UseCookieAuthentication in your project's Startup.Auth.cs.</span></span> <span data-ttu-id="39baf-122">Projekty webových aplikací ASP.NET 4.x vytvořené pomocí sady Visual Studio 2013 a později middleware ověřování souborů cookie Katana ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="39baf-122">ASP.NET 4.x web application projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="39baf-123">Aplikace ASP.NET 4.x musí mít jako cíl rozhraní .NET Framework 4.5.1 nebo novější, jinak potřebné balíčky NuGet se nepodaří nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="39baf-123">Your ASP.NET 4.x application must target .NET Framework 4.5.1 or higher, otherwise the necessary NuGet packages will fail to install.</span></span>

<span data-ttu-id="39baf-124">Sdílet soubory cookie pro ověřování mezi vaší aplikace ASP.NET 4.x a vaše aplikace ASP.NET Core, konfiguraci aplikace ASP.NET Core, jak je uvedeno výše a potom podle následujících kroků konfigurace vaše aplikace ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="39baf-124">To share authentication cookies between your ASP.NET 4.x applications and your ASP.NET Core applications, configure the ASP.NET Core application as stated above, then configure your ASP.NET 4.x applications by following the steps below.</span></span>

1.  <span data-ttu-id="39baf-125">Nainstalujte balíček Microsoft.Owin.Security.Interop do každé z vašich aplikací ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="39baf-125">Install the package Microsoft.Owin.Security.Interop into each of your ASP.NET 4.x applications.</span></span>

2.   <span data-ttu-id="39baf-126">V Startup.Auth.cs vyhledejte volání UseCookieAuthentication, který bude obvykle vypadat podobně jako tento.</span><span class="sxs-lookup"><span data-stu-id="39baf-126">In Startup.Auth.cs, locate the call to UseCookieAuthentication, which will generally look like the following.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  <span data-ttu-id="39baf-127">Následujícím způsobem změnit volání UseCookieAuthentication, změna CookieName tak, aby odpovídaly název používaný middleware ověřování souborů cookie ASP.NET Core poskytnutím instance DataProtectionProvider, který byl inicializován do umístění úložiště klíčů, a Nastavte CookieManager spolupráce komponenta ChunkingCookieManager tak Formát bloku dat je kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="39baf-127">Modify the call to UseCookieAuthentication as follows, changing the CookieName to match the name used by the ASP.NET Core cookie authentication middleware, providing an instance of a DataProtectionProvider that has been initialized to a key storage location, and set CookieManager to interop ChunkingCookieManager so the chunking format is compatible.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
        CookieName = ".AspNetCore.Cookies",
        // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
        // CookiePath = "...", (if necessary)
        // ...
        TicketDataFormat = new AspNetTicketDataFormat(
            new DataProtectorShim(
                DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", "v2"))),
        CookieManager = new ChunkingCookieManager()
    });
    ```
    <span data-ttu-id="39baf-128">DirectoryInfo má tak, aby odkazovaly do stejného umístění úložiště, který odkazoval aplikace ASP.NET Core a by měl být nakonfigurovaný pomocí stejné nastavení.</span><span class="sxs-lookup"><span data-stu-id="39baf-128">The DirectoryInfo has to point to the same storage location that you pointed your ASP.NET Core application to and should be configured using the same settings.</span></span>

<span data-ttu-id="39baf-129">Technologie ASP.NET 4.x a ASP.NET Core aplikace je nyní nakonfigurováno pro sdílet soubory cookie pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="39baf-129">The ASP.NET 4.x and ASP.NET Core applications are now configured to share authentication cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="39baf-130">Budete potřebovat, abyste měli jistotu, že je na stejné uživatele databáze odkazoval systém identit pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="39baf-130">You'll need to make sure that the identity system for each application is pointed at the same user database.</span></span> <span data-ttu-id="39baf-131">Systém identit jinak způsobí chyby za běhu, když se pokouší vyhledat informace v souboru cookie pro ověřování proti informací ve své databázi.</span><span class="sxs-lookup"><span data-stu-id="39baf-131">Otherwise the identity system will produce failures at runtime when it tries to match the information in the authentication cookie against the information in its database.</span></span>
