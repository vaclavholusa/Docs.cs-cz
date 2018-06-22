---
title: Migrace z ClaimsPrincipal.Current
author: mjrousos
description: Zjistěte, jak migrovat mimo ClaimsPrincipal.Current načíst aktuální uživatel ověřený identity a deklarací identity v ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
uid: migration/claimsprincipal-current
ms.openlocfilehash: 477f9fe8f0249bdfc528e1b401f072851f2f0d23
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277183"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migrace z ClaimsPrincipal.Current

Projekty ASP.NET byl společné pro použití [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) načíst aktuální ověření identity a deklarací identity uživatele. V ASP.NET Core, tato vlastnost je již nastavena. Kód, který byl v závislosti na něm je třeba aktualizovat získat aktuální uživatel ověřený identity prostřednictvím různých prostředků.

## <a name="context-specific-data-instead-of-static-data"></a>Konkrétní data místo statických dat

Při použití ASP.NET Core, hodnoty obou `ClaimsPrincipal.Current` a `Thread.CurrentPrincipal` nejsou nastaveny. Tyto vlastnosti obě představují statické stav, který obecně zabraňuje ASP.NET Core. Místo toho je architektura ASP.NET Core načíst závislosti (jako je aktuální uživatel identita) ze služby konkrétní kolekcí (pomocí jeho [vkládání závislostí](xref:fundamentals/dependency-injection) modelu (DI)). Co je více `Thread.CurrentPrincipal` je statické, vlákno, takže se nemusí zachování změn v některých scénářích asynchronní (a `ClaimsPrincipal.Current` právě volá `Thread.CurrentPrincipal` ve výchozím nastavení).

Pochopit druhy problémů vlákno může způsobit statické členy v asynchronní scénáře, zvažte následující fragment kódu:

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

Předchozí sady ukázka kódu `Thread.CurrentPrincipal` a ověří jeho hodnotu před a po čeká na asynchronní volání. `Thread.CurrentPrincipal` je specifická pro *vlákno* na kterém je nastavená a metoda je pravděpodobně pokračovat v provádění v jiném podprocesu po await. V důsledku toho `Thread.CurrentPrincipal` je přítomen, když je nejdřív zaškrtnuta možnost, ale má hodnotu null po volání `await Task.Yield()`.

Získávání aktuálního uživatele identity z kolekce služby DI aplikace je více možností intenzivního testování, příliš, vzhledem k tomu, že testovací identity můžete snadno vložit.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Načtení aktuální uživatel v aplikaci ASP.NET Core

Existuje několik možností pro načítání aktuální uživatel ověřený `ClaimsPrincipal` v ASP.NET Core místě `ClaimsPrincipal.Current`:

* **ControllerBase.User**. Kontrolery MVC můžete přístup k aktuální ověřeného uživatele s jejich [uživatele](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) vlastnost.
* **HttpContext.User**. Komponenty s přístupem k aktuální `HttpContext` (například middleware) můžete získat aktuální uživatel `ClaimsPrincipal` z [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Předané z volající**. Knihovny bez přístupu k aktuální `HttpContext` se často nazývají z řadiče nebo komponenty middlewaru a může mít aktuální uživatel identity předat jako argument.
* **IHttpContextAccessor**. Projekt ASP.NET se migruje ASP.NET Core může být příliš velký pro snadno předat všechny nezbytné umístění aktuální identita uživatele. V takových případech [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) lze použít jako alternativní řešení. `IHttpContextAccessor` dokáže získat přístup k aktuální `HttpContext` (pokud existuje). Krátkodobé řešení pro získávání aktuálního uživatele identity v kódu, který ještě nebyl aktualizován pro práci s architekturou ASP.NET Core řízené DI by byl:

  * Ujistěte se, `IHttpContextAccessor` k dispozici v kontejneru DI voláním [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) v `Startup.ConfigureServices`.
  * Získat instanci `IHttpContextAccessor` během spouštění a ukládání do statických proměnné. Instance je k dispozici na kód, který byl dříve načítání aktuálního uživatele z pomocí statické vlastnosti.
  * Načtení aktuální uživatel `ClaimsPrincipal` pomocí `HttpContextAccessor.HttpContext?.User`. Pokud se tento kód používá mimo kontext požadavku HTTP, `HttpContext` má hodnotu null.

Konečné možnost, pomocí `IHttpContextAccessor`, bylo v rozporu s principy ASP.NET Core (upřednostňují vloženého závislosti na statické závislosti). Naplánujte nakonec odstranit závislost na statické `IHttpContextAccessor` pomocné rutiny. Může být užitečné mostu, ale při migraci velké existující aplikace ASP.NET, které byly dříve pomocí `ClaimsPrincipal.Current`.
