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
ms.openlocfilehash: c229537e7f533eea2173dbc51b8d0d0e097d434a
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2018
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a>Konfigurovat ověřování systému Windows v aplikaci ASP.NET Core

Podle [Steve Smith](https://ardalis.com) a [Scott Addie](https://twitter.com/Scott_Addie)

Ověřování systému Windows lze konfigurovat pro aplikace ASP.NET Core hostované službou IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), nebo [WebListener](xref:fundamentals/servers/weblistener).

## <a name="what-is-windows-authentication"></a>Co je ověřování systému Windows?

Ověřování systému Windows závisí na operačním systému k ověřování uživatelů aplikace ASP.NET Core. Pokud váš server běží v podnikové síti pomocí identity domény služby Active Directory nebo jiné účty systému Windows k identifikaci uživatelů, můžete použít ověřování systému Windows. Ověřování systému Windows je nejvhodnější pro prostředí intranetu, ve kterých uživatelů, klientské aplikace a webové servery patří do stejné domény systému Windows.

[Další informace o ověřování systému Windows a instalaci pro službu IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Povolit ověřování systému Windows v aplikaci ASP.NET Core

Šablony sady Visual Studio webové aplikace mohou být nakonfigurované pro podporu ověřování systému Windows.

### <a name="use-the-windows-authentication-app-template"></a>Použití šablony aplikace ověřování systému Windows

In Visual Studio:
1. Vytvořte novou webovou aplikaci ASP.NET Core. 
1. Vyberte webovou aplikaci ze seznamu šablon.
1. Vyberte **změna ověřování** tlačítko a vyberte **ověřování systému Windows**. 

Spusťte aplikaci. Uživatelské jméno se zobrazí v horním rohu aplikace.

![Snímek obrazovky prohlížeče ověřování systému Windows](windowsauth/_static/browser-screenshot.png)

Šablona pro vývojové práci pomocí služby IIS Express, poskytuje veškeré konfigurace nezbytné použít ověřování systému Windows. V následující části ukazuje, jak ručně nakonfigurovat aplikace ASP.NET Core pro ověřování systému Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Nastavení sady Visual Studio pro systém Windows a anonymní ověřování

Projekt Visual Studio **vlastnosti** stránky **ladění** karta obsahuje zaškrtávací políčka pro ověřování systému Windows a anonymní ověřování.

![Snímek obrazovky prohlížeče ověřování systému Windows](windowsauth/_static/vs-auth-property-menu.png)

Můžete taky tyto dvě vlastnosti se dá nakonfigurovat v *launchSettings.json* souboru:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>Povolit ověřování systému Windows pomocí služby IIS

Služba IIS použije [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) (ANCM) pro hostování aplikací ASP.NET Core. ANCM toky ověřování systému Windows do služby IIS ve výchozím nastavení. Konfigurace ověřování systému Windows se provádí v rámci služby IIS, ne projekt aplikace. Následující části vysvětlují, jak pomocí Správce služby IIS ke konfiguraci aplikace ASP.NET Core pomocí ověřování systému Windows.

### <a name="create-a-new-iis-site"></a>Vytvoří nový web služby IIS

Zadejte název a složky a povolit ji vytvořit nový fond aplikací.

### <a name="customize-authentication"></a>Přizpůsobení ověřování

Otevřete nabídku ověřování pro danou lokalitu.

![Nabídky ověřování služby IIS](windowsauth/_static/iis-authentication-menu.png)

Zakázáním anonymního ověřování a povolit ověřování systému Windows.

![Nastavení ověřování služby IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Publikování projektu do složky webového serveru IIS

Pomocí sady Visual Studio nebo rozhraní příkazového řádku .NET Core, publikujte aplikaci do cílové složky.

![Dialogové okno publikování sady Visual Studio](windowsauth/_static/vs-publish-app.png)

Další informace o [publikování do služby IIS](xref:host-and-deploy/iis/index).

Spusťte aplikaci a ověří, zda je funkční ověřování systému Windows.

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a>Povolení ověřování systému Windows pomocí ovladače HTTP.sys nebo WebListener

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

I když Kestrel nepodporuje ověřování systému Windows, můžete použít [HTTP.sys](xref:fundamentals/servers/httpsys) podporu vlastním hostováním scénářů v systému Windows. Následující příklad konfiguruje hostitel webové aplikace používat ovladač HTTP.sys pomocí ověřování systému Windows:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

I když Kestrel nepodporuje ověřování systému Windows, můžete použít [WebListener](xref:fundamentals/servers/weblistener) podporu vlastním hostováním scénářů v systému Windows. Následující příklad konfiguruje hostitel webové aplikace používat WebListener pomocí ověřování systému Windows:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a>Práce s ověřováním systému Windows

Stav konfigurace anonymního přístupu určuje, jakým způsobem `[Authorize]` a `[AllowAnonymous]` atributy se používají v aplikaci. Následující dvě části popisují způsob zpracování konfigurace zakázaných a povolených stavy anonymní přístup.

### <a name="disallow-anonymous-access"></a>Zakáže anonymní přístup

Pokud je povolené ověřování systému Windows a je zakázán anonymní přístup, `[Authorize]` a `[AllowAnonymous]` atributy mít žádný vliv. Pokud webu služby IIS (nebo ovladač HTTP.sys nebo WebListener server) je nakonfigurován tak, aby zakázala anonymní přístup, požadavek dosáhne nikdy vaší aplikace. Z tohoto důvodu `[AllowAnonymous]` atribut neplatí.

### <a name="allow-anonymous-access"></a>Povolení anonymního přístupu

Pokud jsou povolené ověřování systému Windows a anonymní přístup, použijte `[Authorize]` a `[AllowAnonymous]` atributy. `[Authorize]` Atribut umožňuje zabezpečit součásti aplikace, které skutečně vyžadují ověřování systému Windows. `[AllowAnonymous]` Atribut přepsání `[Authorize]` atribut využití v rámci aplikace, které povolí anonymní přístup. V tématu [jednoduché autorizace](xref:security/authorization/simple) podrobnosti o použití atributu.

V ASP.NET Core 2.x, `[Authorize]` atribut vyžaduje, aby další konfigurace v *Startup.cs* vyzve anonymních požadavků pro ověřování systému Windows. Doporučená konfigurace mírně závisí na webovém serveru, který je používán.

> [!NOTE]
> Ve výchozím nastavení uživatelé, kteří nemají oprávnění pro přístup k stránky zobrazí se prázdný dokument. [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) lze nakonfigurovat, aby uživatelům lepší "Přístup byl odepřen".

#### <a name="iis"></a>IIS

Pokud používáte službu IIS, přidejte následující `ConfigureServices` metoda: 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Pokud používáte HTTP.sys, přidejte následující `ConfigureServices` metoda:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Zosobnění

ASP.NET Core neimplementuje zosobnění. Aplikace spustí s identity aplikací pro všechny požadavky a pomocí identity aplikace fondu nebo proces. Pokud potřebujete explicitně provedení akce jménem uživatele, použijte `WindowsIdentity.RunImpersonated`. Spustit jednou akcí v tomto kontextu a pak zavřete kontextu.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Všimněte si, že `RunImpersonated` nepodporuje asynchronní operace a by nemělo být použito pro komplexní scénáře. Například zabalení celý požadavky nebo middleware zřetězen není podporován nebo doporučené.

---
