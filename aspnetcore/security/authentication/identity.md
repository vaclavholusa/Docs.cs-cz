---
title: "Úvod do Identity na jádro ASP.NET"
author: rick-anderson
description: "Pomocí Identity aplikace ASP.NET Core. Obsahuje požadavky na heslo nastavení (RequireDigit, RequiredLength, RequiredUniqueChars a další)."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity
ms.openlocfilehash: 8cbf002a9280650a08ae8d49b5b6d23bafb8be18
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Úvod do Identity na jádro ASP.NET

Podle [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [tní Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), a [Steve Smith](https://ardalis.com/)

Identita ASP.NET Core je systém členství, který umožňuje přidat funkce přihlášení do aplikace. Uživatelé mohou vytvářet účet a přihlášení s uživatelským jménem a heslem nebo se můžete použít externího zprostředkovatele přihlášení jako je Facebook, Google, Microsoft Account, služby Twitter nebo jiné.

Můžete nakonfigurovat ASP.NET Identity Core ukládat uživatelská jména, hesla a data profilu do databáze serveru SQL Server. Alternativně můžete použít vlastní trvalého úložiště, například Azure Table Storage. Tento dokument obsahuje pokyny pro sadu Visual Studio a pro používání rozhraní příkazového řádku.

[Zobrazení nebo stažení ukázkového kódu.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Postup stažení)](https://docs.microsoft.com/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Přehled identity

V tomto tématu budete Naučte se používat ASP.NET Core Identity k přidání funkcí registrace, přihlášení a odhlášení uživatele. Podrobnější pokyny k vytváření aplikací pomocí ASP.NET Core Identity najdete v části Další kroky na konci tohoto článku.

1.  Vytvoření projektu webové aplikace ASP.NET Core s jednotlivé uživatelské účty.

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    V sadě Visual Studio, vyberte **soubor** > **nový** > **projektu**. Vyberte **webové aplikace ASP.NET Core** a klikněte na tlačítko **OK**.

    ![Dialogové okno Nový projekt](identity/_static/01-new-project.png)

    Vyberte ASP.NET Core **webové aplikace (Model-View-Controller)** pro technologii ASP.NET základní 2.x a pak vyberte **změna ověřování**.

    ![Dialogové okno Nový projekt](identity/_static/02-new-project.png)

    Zobrazí se dialogové okno se zobrazí nabízení možnosti ověřování. Vyberte **jednotlivé uživatelské účty** a klikněte na tlačítko **OK** se vrátíte do předchozího dialogového okna.

    ![Dialogové okno Nový projekt](identity/_static/03-new-project-auth.png)

    Výběr **jednotlivé uživatelské účty** přesměruje Chcete-li vytvořit modely, ViewModels, zobrazení, řadiče a další prostředky požadované pro ověřování v rámci šablony projektů Visual Studio.

    # <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

    Pokud používáte rozhraní příkazového řádku .NET Core, vytvořte nový projekt pomocí ``dotnet new mvc --auth Individual``. Tento příkaz vytvoří nový projekt se stejným kódem šablony Identity, které sada Visual Studio vytvoří.

    Vytvořený projekt obsahuje `Microsoft.AspNetCore.Identity.EntityFrameworkCore` balíček, která ukládá data identit a schématu na SQL Server pomocí [Entity Framework Core](https://docs.microsoft.com/ef/).

    ---

2.  Nakonfigurujte identitu služby a přidejte middleware v `Startup`.

    Identita služby jsou přidány do aplikace `ConfigureServices` metoda v `Startup` – třída:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    Tyto služby jsou k dispozici pro aplikace prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection).
    
    Identity je pro tuto aplikaci povolena voláním `UseAuthentication` v `Configure` metoda. `UseAuthentication` Přidá ověřování [middleware](xref:fundamentals/middleware/index) požadavku kanálu.
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    Tyto služby jsou k dispozici pro aplikace prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection).
    
    Identity je pro tuto aplikaci povolena voláním `UseIdentity` v `Configure` metoda. `UseIdentity` Přidá ověřování na základě souborů cookie [middleware](xref:fundamentals/middleware/index) požadavku kanálu.
        
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    Další informace o spuštění aplikace se proces najdete v tématu [spuštění aplikace](xref:fundamentals/startup).

3.  Vytvořte uživatele.

    Spuštění aplikace a potom klikněte na **zaregistrovat** odkaz.

    Pokud je při prvním provedení této akce, může být potřeba spustit migrace. Aplikace vás vyzve, abyste **použít migrace**. V případě potřeby aktualizujte stránku.
    
    ![Použití migrace webové stránky](identity/_static/apply-migrations.png)
    
    Alternativně můžete otestovat pomocí ASP.NET Core Identity s vaší aplikací bez trvalé databáze pomocí databáze v paměti. Chcete-li používat databázi v paměti, přidejte ``Microsoft.EntityFrameworkCore.InMemory`` balíčku do vaší aplikace a upravit volání vaší aplikace ``AddDbContext`` v ``ConfigureServices`` následujícím způsobem:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    Když uživatel klikne **zaregistrovat** odkaz, ``Register`` akce je volána v ``AccountController``. ``Register`` Akce vytvoří uživatele voláním `CreateAsync` na `_userManager` objektu (poskytované ``AccountController`` pomocí vkládání závislostí):
 
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    Pokud byl uživatel vytvořen úspěšně, uživatel je přihlášen pomocí volání ``_signInManager.SignInAsync``.

    **Poznámka:** najdete v části [účet potvrzení](xref:security/authentication/accconfirm#prevent-login-at-registration) kroky, aby se zabránilo okamžitou přihlášení při registraci.
 
4.  Přihlásit se.
 
    Uživatelé se mohou přihlásit kliknutím **přihlásit** odkaz v horní části webu, nebo se může přesměrováni na stránku přihlášení Pokud pokus o přístup k součástí lokality, která vyžaduje ověření. Když uživatel odešle formulář na přihlašovací stránku, ``AccountController`` ``Login`` akce je volána.

    ``Login`` Volání akce ``PasswordSignInAsync`` na ``_signInManager`` objektu (poskytované ``AccountController`` pomocí vkládání závislostí).

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    Základní ``Controller`` třídy zpřístupňuje ``User`` vlastnost, která je přístupné z metody kontroleru. Například můžete vytvořit výčet `User.Claims` a autorizační rozhodnutí. Další informace najdete v tématu [autorizace](xref:security/authorization/index).
 
5.  Odhlaste.
 
    Kliknutím **odhlášení** odkaz volání `LogOut` akce.
 
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    Předchozí kód výše volání `_signInManager.SignOutAsync` metoda. `SignOutAsync` Metoda vymaže deklaracích identity uživatele v souborech cookie.
 
<a name="pw"></a>
6.  Konfigurace.

    Identita má některé výchozí chování, které mohou být přepsána nastaveními v třída při spuštění aplikace. `IdentityOptions` není nutné nakonfigurovat při použití výchozí chování. Následující kód nastaví několik možností síly hesla:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    Další informace o tom, jak nakonfigurovat Identity najdete v tématu [konfigurace Identity](xref:security/authentication/identity-configuration).
    
    Můžete také konfigurovat datový typ primární klíč, najdete v části [konfigurace Identity primárních klíčů datový typ](xref:security/authentication/identity-primary-key-configuration).
 
7.  Zobrazte databázi.

    Pokud vaše aplikace používá databázi systému SQL Server (výchozí možnost v systému Windows a pro uživatele v sadě Visual Studio), můžete zobrazit databázi aplikace vytvořená. Můžete použít **SQL Server Management Studio**. Případně ze sady Visual Studio, vyberte možnost **zobrazení** > **Průzkumník objektů systému SQL Server**. Připojení k **\MSSQLLocalDB (localdb)**. Databáze s odpovídajícím názvem **aspnet - <*název projektu*>-<*datum řetězec* >**  se zobrazí.

    ![Kontextové nabídky na AspNetUsers databázové tabulky](identity/_static/04-db.png)
    
    Rozbalte databázi a její **tabulky**, klikněte pravým tlačítkem **dbo. AspNetUsers** tabulky a vyberte **Data zobrazení**.

8. Ověření Identity funguje

    Výchozí hodnota *webové aplikace ASP.NET Core* šablona projektu umožňuje uživatelům přístup k veškeré akce v aplikaci bez nutnosti k přihlášení. Chcete-li ověřit, že ASP.NET Identity funguje, přidejte`[Authorize]` atribut `About` akce `Home` řadiče.
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Spusťte projekt pomocí **Ctrl** + **F5** a přejděte do **o** stránky. Může používat pouze ověření uživatelé **o** stránka nyní, takže ASP.NET vás přesměruje na přihlašovací stránku pro přihlášení nebo registraci.

    # <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

    Otevřete okno příkazového řádku a přejděte do projektu kořenový adresář obsahující `.csproj` souboru. Spustit [dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz ke spuštění aplikace:

    ```cs
    dotnet run 
    ```

    Vyhledejte ve výstupu zadanou adresu URL [dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz. Adresa URL by měla odkazovat na `localhost` s číslem generovaného portu. Přejděte na **o** stránky. Může používat pouze ověření uživatelé **o** stránka nyní, takže ASP.NET vás přesměruje na přihlašovací stránku pro přihlášení nebo registraci.

    ---

## <a name="identity-components"></a>Komponent identity

Je primární referenční sestavení pro systém identit `Microsoft.AspNetCore.Identity`. Tento balíček obsahuje sadu základní rozhraní pro ASP.NET Core Identity a je začleněn sám v `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Tyto závislosti jsou potřeba k použití v aplikacích ASP.NET Core systém identit:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Obsahuje požadované typy, které mají pomocí Identity Entity Framework Core.

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core je technologie přístup doporučené dat společnosti Microsoft pro relační databáze jako SQL Server. Pro testování, můžete použít `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies` -Middleware, který umožňuje aplikaci používat ověřování na základě souboru cookie.

## <a name="migrating-to-aspnet-core-identity"></a>Migrace na jádro ASP.NET Identity

Pro další informace a pokyny k migraci stávající identitu úložiště najdete v tématu [migrace ověřování a identita](xref:migration/identity).

## <a name="setting-password-strength"></a>Nastavení síly hesla

V tématu [konfigurace](#pw) pro ukázku, která nastaví požadavky na minimální hesla.

## <a name="next-steps"></a>Další kroky

* [Migrace ověřování a identita](xref:migration/identity)
* [Potvrzení účtu a obnovení hesla](xref:security/authentication/accconfirm)
* [Dvoufaktorové ověřování přes SMS](xref:security/authentication/2fa)
* [Facebook, Google a externí zprostředkovatel ověřování](xref:security/authentication/social/index)
