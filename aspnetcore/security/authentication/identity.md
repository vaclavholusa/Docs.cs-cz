---
title: "Úvod do Identity na jádro ASP.NET"
author: rick-anderson
description: "Pomocí Identity aplikace ASP.NET Core"
keywords: "ASP.NET Core, Identity, autorizaci zabezpečení"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 0679663b3b3b66f9935d0fb24360be2954fcdee1
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Úvod do Identity na jádro ASP.NET

Podle [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [tní Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), a [Steve Smith](https://ardalis.com/)

Identita ASP.NET Core je systém členství, který umožňuje přidat funkce přihlášení do aplikace. Uživatelé mohou vytvářet účet a přihlášení s uživatelským jménem a heslem nebo se můžete použít externího zprostředkovatele přihlášení jako je Facebook, Google, Microsoft Account, služby Twitter nebo jiné.

Můžete nakonfigurovat ASP.NET Identity Core ukládat uživatelská jména, hesla a data profilu do databáze serveru SQL Server. Alternativně můžete použít vlastní trvalého úložiště, například Azure Table Storage. Tento dokument obsahuje pokyny pro sadu Visual Studio a pro používání rozhraní příkazového řádku.

## <a name="overview-of-identity"></a>Přehled identity

V tomto tématu budete Naučte se používat ASP.NET Core Identity k přidání funkcí registrace, přihlášení a odhlášení uživatele. Podrobnější pokyny k vytváření aplikací pomocí ASP.NET Core Identity najdete v části Další kroky na konci tohoto článku.

1.  Vytvoření projektu webové aplikace ASP.NET Core s jednotlivé uživatelské účty.

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)
    V sadě Visual Studio, vyberte **soubor** -> **nový** -> **projektu**. Vyberte **webové aplikace ASP.NET** z **nový projekt** dialogové okno. Výběr ASP.NET Core **webové Application(Model-View-Controller)** pro ASP.NET Core 2.x s **jednotlivé uživatelské účty** jako metodu ověřování.

    Poznámka: Je nutné vybrat **jednotlivé uživatelské účty**.
 
    ![Dialogové okno Nový projekt](identity/_static/01-mvc_2.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[.NET core rozhraní příkazového řádku](#tab/netcore-cli)
    Pokud používáte rozhraní příkazového řádku .NET Core, vytvořte nový projekt pomocí ``dotnet new mvc --auth Individual``. Tím se vytvoří nový projekt se stejným kódem šablony Identity, které sada Visual Studio vytvoří.
 
    Vytvořený projekt obsahuje `Microsoft.AspNetCore.Identity.EntityFrameworkCore` balíček, který se uchová Identity dat a schématu na SQL Server pomocí [Entity Framework Core](https://docs.microsoft.com/ef/).
    
    ---
 
2.  Nakonfigurujte identitu služby a přidejte middleware v `Startup`.

    Identita služby jsou přidány do aplikace `ConfigureServices` metoda v `Startup` – třída:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    Tyto služby jsou k dispozici pro aplikace prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection).
    
    Identity je pro tuto aplikaci povolena voláním `UseAuthentication` v `Configure` metoda. `UseAuthentication`Přidá ověřování [middleware](xref:fundamentals/middleware) požadavku kanálu.
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    Tyto služby jsou k dispozici pro aplikace prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection).
    
    Identity je pro tuto aplikaci povolena voláním `UseIdentity` v `Configure` metoda. `UseIdentity`Přidá ověřování na základě souborů cookie [middleware](xref:fundamentals/middleware) požadavku kanálu.
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    Další informace o spuštění aplikace se proces najdete v tématu [spuštění aplikace](xref:fundamentals/startup).

3.  Vytvořte uživatele.

    Spuštění aplikace a potom klikněte na **zaregistrovat** odkaz.

    Pokud je při prvním provedení této akce, může být potřeba spustit migrace. Aplikace vás vyzve, abyste **použít migrace**:
    
    ![Použití migrace webové stránky](identity/_static/apply-migrations.png)
    
    Alternativně můžete otestovat pomocí ASP.NET Core Identity s vaší aplikací bez trvalé databáze pomocí databáze v paměti. Chcete-li používat databázi v paměti, přidejte ``Microsoft.EntityFrameworkCore.InMemory`` balíčku do vaší aplikace a upravit volání vaší aplikace ``AddDbContext`` v ``ConfigureServices`` následujícím způsobem:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    Když uživatel klikne **zaregistrovat** odkaz, ``Register`` akce je volána v ``AccountController``. ``Register`` Akce vytvoří uživatele voláním `CreateAsync` na `_userManager` objektu (poskytované ``AccountController`` pomocí vkládání závislostí):
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    Pokud byl uživatel vytvořen úspěšně, uživatel je přihlášen pomocí volání ``_signInManager.SignInAsync``.

    **Poznámka:** najdete v části [účet potvrzení](xref:security/authentication/accconfirm#prevent-login-at-registration) kroky, aby se zabránilo okamžitou přihlášení při registraci.
 
4.  Přihlásit se.
 
    Uživatelé se mohou přihlásit kliknutím **přihlásit** odkaz v horní části webu, nebo se může přesměrováni na stránku přihlášení Pokud pokus o přístup k součástí lokality, která vyžaduje ověření. Když uživatel odešle formulář na přihlašovací stránku, ``AccountController`` ``Login`` akce je volána.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    ``Login`` Volání akce ``PasswordSignInAsync`` na ``_signInManager`` objektu (poskytované ``AccountController`` pomocí vkládání závislostí).
 
    Základní ``Controller`` třídy zpřístupňuje ``User`` vlastnost, která je přístupné z metody kontroleru. Například můžete vytvořit výčet `User.Claims` a autorizační rozhodnutí. Další informace najdete v tématu [autorizace](xref:security/authorization/index).
 
5.  Odhlaste.
 
    Kliknutím **odhlášení** odkaz volání `LogOut` akce.
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    Předchozí kód výše volání `_signInManager.SignOutAsync` metoda. `SignOutAsync` Metoda vymaže deklaracích identity uživatele v souborech cookie.
 
6.  Konfigurace.

    Identita má některé výchozí chování lze přepsat v třídě spuštění vaší aplikace. Není potřeba konfigurovat ``IdentityOptions`` Pokud používáte výchozí chování.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    Další informace o tom, jak nakonfigurovat Identity najdete v tématu [konfigurace Identity](xref:security/authentication/identity-configuration).
    
    Můžete také konfigurovat datový typ primární klíč, najdete v části [konfigurace Identity primárních klíčů datový typ](xref:security/authentication/identity-primary-key-configuration).
 
7.  Zobrazte databázi.

    Pokud vaše aplikace používá databázi systému SQL Server (výchozí možnost v systému Windows a pro uživatele v sadě Visual Studio), můžete zobrazit databázi aplikace vytvořená. Můžete použít **SQL Server Management Studio**. Případně ze sady Visual Studio, vyberte možnost **zobrazení** -> **Průzkumník objektů systému SQL Server**. Připojení k **\MSSQLLocalDB (localdb)**. Databáze s odpovídajícím názvem  **aspnet - <*název projektu*>-<*datum řetězec*> ** se zobrazí.

    ![Kontextové nabídky na AspNetUsers databázové tabulky](identity/_static/04-db.png)
    
    Rozbalte databázi a její **tabulky**, klikněte pravým tlačítkem **dbo. AspNetUsers** tabulky a vyberte **Data zobrazení**.

## <a name="identity-components"></a>Komponent identity

Je primární referenční sestavení pro systém identit `Microsoft.AspNetCore.Identity`. Tento balíček obsahuje sadu základní rozhraní pro ASP.NET Core Identity a je začleněn sám v `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Tyto závislosti jsou potřeba k použití v aplikacích ASP.NET Core systém identit:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Obsahuje požadované typy, které mají pomocí Identity Entity Framework Core.

* `Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core je technologie přístup doporučené dat společnosti Microsoft pro relační databáze jako SQL Server. Pro testování, můžete použít `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies`-Middleware, který umožňuje aplikaci používat ověřování na základě souboru cookie.

## <a name="migrating-to-aspnet-core-identity"></a>Migrace na jádro ASP.NET Identity

Pro další informace a pokyny k migraci stávající identitu úložiště najdete v tématu [migrace ověřování a identita](xref:migration/identity).

## <a name="next-steps"></a>Další kroky

* [Migrace ověřování a identita](xref:migration/identity)
* [Potvrzení účtu a heslo pro obnovení](xref:security/authentication/accconfirm)
* [Dvoufaktorové ověřování pomocí serveru SMS](xref:security/authentication/2fa)
* [Povolení ověřování pomocí služby Facebook, Google a dalších externích zprostředkovatelů](xref:security/authentication/social/index)
