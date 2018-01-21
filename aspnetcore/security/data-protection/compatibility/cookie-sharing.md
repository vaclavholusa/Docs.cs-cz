---
title: "Sdílení souborů cookie mezi aplikacemi"
author: rick-anderson
description: "Tento dokument popisuje, jak se zásobníkem ochrany dat podporuje sdílení souborů cookie ověřování mezi ASP.NET 4.x a ASP.NET Core aplikace."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: 0cbf5a3e9dfe8f99433800ac5c10ed36b4de6527
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="sharing-cookies-between-applications"></a>Sdílení souborů cookie mezi aplikacemi

Webové servery obvykle obsahovat mnoho jednotlivých webových aplikací, všech pracovních společně obchodu. Pokud vývojář aplikace chce zajistit funkční prostředí jednotného přihlašování, potřebují budete často všechny jiné webové aplikace v rámci lokality lístků pro ověřování mezi sebou sdílet.

Pro podporu tohoto scénáře, zásobník ochrany dat umožňuje sdílení Katana ověřování souborů cookie a ASP.NET Core lístků pro ověřování pomocí souboru cookie.

## <a name="sharing-authentication-cookies-between-applications"></a>Sdílení souborů cookie ověřování mezi aplikacemi

Pokud chcete sdílet soubory cookie pro ověřování mezi dvěma různými aplikacemi ASP.NET Core, nakonfigurujte všech aplikací, které by měly sdílet soubory cookie následujícím způsobem.

Ve vaší konfiguraci metody, použijte CookieAuthenticationOptions nastavit služba ochrany dat pro soubory cookie a AuthenticationScheme tak, aby odpovídaly ASP.NET 4.x.

Pokud používáte identity:

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

Pokud používáte soubory cookie přímo:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
`DataProtectionProvider` Vyžaduje `Microsoft.AspNetCore.DataProtection.Extensions` balíček NuGet.

Pokud se použije tímto způsobem, DirectoryInfo by měla odkazovat na umístění úložiště klíčů konkrétně vyčleněné pro soubory cookie pro ověřování. Middleware ověřování souborů cookie použije výslovně stanovenou implementace DataProtectionProvider, který je nyní izolované ze systému ochrany dat používá dalších částí aplikace. Název aplikace je ignorována (záměrně Ano, protože se snažíte získat více aplikacím sdílet datové části).

>[!WARNING]
>Měli byste zvážit, tak, aby klíče jsou zašifrovaná přinejmenším, jako v konfiguraci DataProtectionProvider následujícím příkladu.
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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a>Sdílení souborů cookie ověřování mezi ASP.NET 4.x a ASP.NET Core aplikace

Aplikace ASP.NET 4.x, které využívají middleware ověřování souborů cookie Katana může být nakonfigurováno pro generování ověřovací soubory cookie, které jsou slučitelné s middleware ověřování souborů cookie ASP.NET Core. To umožňuje upgrade velké lokality jednotlivých aplikací piecemeal současně stále zajišťuje bezproblémovou jednotné přihlašování v prostředí celém webu.

>[!TIP]
> Můžete zadat, pokud aplikace využívá middleware ověřování souborů cookie Katana existencí volání UseCookieAuthentication v Startup.Auth.cs vašeho projektu. Projekty webových aplikací ASP.NET 4.x vytvořené pomocí sady Visual Studio 2013 a později middleware ověřování souborů cookie Katana ve výchozím nastavení.

> [!NOTE]
> Aplikace ASP.NET 4.x musí mít jako cíl rozhraní .NET Framework 4.5.1 nebo novější, jinak potřebné balíčky NuGet se nepodaří nainstalovat.

Sdílet soubory cookie pro ověřování mezi vaší aplikace ASP.NET 4.x a vaše aplikace ASP.NET Core, konfiguraci aplikace ASP.NET Core, jak je uvedeno výše a potom podle následujících kroků konfigurace vaše aplikace ASP.NET 4.x.

1.  Nainstalujte balíček Microsoft.Owin.Security.Interop do každé z vašich aplikací ASP.NET 4.x.

2.   V Startup.Auth.cs vyhledejte volání UseCookieAuthentication, který bude obvykle vypadat podobně jako tento.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  Následujícím způsobem změnit volání UseCookieAuthentication, změna CookieName tak, aby odpovídaly název používaný middleware ověřování souborů cookie ASP.NET Core poskytnutím instance DataProtectionProvider, který byl inicializován do umístění úložiště klíčů, a Nastavte CookieManager spolupráce komponenta ChunkingCookieManager tak Formát bloku dat je kompatibilní.

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
    DirectoryInfo má tak, aby odkazovaly do stejného umístění úložiště, který odkazoval aplikace ASP.NET Core a by měl být nakonfigurovaný pomocí stejné nastavení.

Technologie ASP.NET 4.x a ASP.NET Core aplikace je nyní nakonfigurováno pro sdílet soubory cookie pro ověřování.

> [!NOTE]
> Budete potřebovat, abyste měli jistotu, že je na stejné uživatele databáze odkazoval systém identit pro každou aplikaci. Systém identit jinak způsobí chyby za běhu, když se pokouší vyhledat informace v souboru cookie pro ověřování proti informací ve své databázi.
