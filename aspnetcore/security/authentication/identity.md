---
title: Úvod do Identity v ASP.NET Core
author: rick-anderson
description: Pomocí Identity aplikace v ASP.NET Core. Obsahuje požadavky na heslo nastavení (RequireDigit, RequiredLength, RequiredUniqueChars a další).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: c231a7619a4433ce004342ce68564e4c3892e702
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829299"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Úvod do Identity v ASP.NET Core

Podle [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Petr Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), a [Steve Smith](https://ardalis.com/)

ASP.NET Core Identity je systém členství, který slouží k přidání funkcí přihlášení do vaší aplikace. Uživatelé můžou vytvářet účet a přihlášení s uživatelským jménem a heslem nebo že můžete použít externího zprostředkovatele přihlášení jako je Facebook, Google, Microsoft Account, Twitteru nebo jiné.

ASP.NET Core Identity pro použití databáze SQL serveru k ukládání uživatelská jména, hesla a data profilu můžete nakonfigurovat. Alternativně můžete použít vlastní trvalého úložiště, například Azure Table Storage. Tento dokument obsahuje pokyny pro Visual Studio a pomocí rozhraní příkazového řádku.

[Zobrazení nebo stažení ukázkového kódu.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Jak stáhnout)](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Přehled Identity

V tomto tématu budete zjistěte, jak přidat funkci do zaregistrovat, přihlaste se pomocí ASP.NET Core Identity a odhlášení uživatele. Podrobnější pokyny týkající se vytváření aplikací pomocí ASP.NET Core Identity najdete v části Další kroky na konci tohoto článku.

1. Vytvoření projektu webové aplikace ASP.NET Core s jednotlivými uživatelskými účty.

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   V sadě Visual Studio, vyberte **souboru** > **nový** > **projektu**. Vyberte **webové aplikace ASP.NET Core** a klikněte na tlačítko **OK**.

   ![Dialogové okno nového projektu](identity/_static/01-new-project.png)

   Vyberte v ASP.NET Core **webové aplikace (Model-View-Controller)** pro ASP.NET Core 2.x, pak vyberte **změna ověřování**.

   ![Dialogové okno nového projektu](identity/_static/02-new-project.png)

   Dialogové okno se zobrazí nabídka možnosti ověřování. Vyberte **jednotlivé uživatelské účty** a klikněte na tlačítko **OK** vrátit do předchozího dialogového okna.

   ![Dialogové okno nového projektu](identity/_static/03-new-project-auth.png)

   Výběr **jednotlivé uživatelské účty** instruuje Visual Studio a vytvářet modely, modely ViewModels, zobrazení, Kontrolerů a další prostředky požadované pro ověřování jako součást šablony projektu.

   # <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

   Pokud používáte rozhraní příkazového řádku .NET Core, vytvořte nový projekt pomocí `dotnet new mvc --auth Individual`. Tento příkaz vytvoří nový projekt se stejným kódem šablony Identity, které sada Visual Studio vytvoří.

   Vytvořený projekt obsahuje `Microsoft.AspNetCore.Identity.EntityFrameworkCore` balíček, který ukládá data identit a schémat na SQL Server pomocí [Entity Framework Core](https://docs.microsoft.com/ef/).

   ---

2. Nakonfigurujte identitu služby a přidejte middlewaru v `Startup`.

   Služby identit jsou přidané do aplikace v `ConfigureServices` metodu `Startup` třídy:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Tyto služby jsou k dispozici pro aplikace prostřednictvím [injektáž závislostí](xref:fundamentals/dependency-injection).

   Povolení identity pro aplikace po zavolání `UseAuthentication` v `Configure` metody. `UseAuthentication` Přidá ověřování [middleware](xref:fundamentals/middleware/index) požadavku kanálu.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Tyto služby jsou k dispozici pro aplikace prostřednictvím [injektáž závislostí](xref:fundamentals/dependency-injection).

   Povolení identity pro aplikace po zavolání `UseIdentity` v `Configure` metody. `UseIdentity` Přidá na základě souboru cookie ověřování [middleware](xref:fundamentals/middleware/index) požadavku kanálu.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   Další informace o spuštění aplikace procesu naleznete v tématu [spuštění aplikace](xref:fundamentals/startup).

3. Vytvoření uživatele.

   Spusťte aplikaci a potom klikněte na **zaregistrovat** odkaz.

   Pokud je to poprvé při provádění této akce, může být potřeba ke spouštění migrace. Aplikace vás vyzve, abyste **migrace použít**. V případě potřeby aktualizujte stránku.

   ![Použití migrace webové stránky](identity/_static/apply-migrations.png)

   Alternativně můžete otestovat pomocí ASP.NET Core Identity s vaší aplikací bez trvalé databáze pomocí databáze v paměti. Chcete-li používat databázi v paměti, přidejte `Microsoft.EntityFrameworkCore.InMemory` balíček do vaší aplikace a upravte volání vaší aplikace `AddDbContext` v `ConfigureServices` následujícím způsobem:

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   Pokud uživatel klikne **zaregistrovat** odkaz, `Register` akce je volána na `AccountController`. `Register` Akce vytvoří uživatele voláním `CreateAsync` na `_userManager` objektu (poskytnuté `AccountController` pomocí vkládání závislostí):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   Pokud byl uživatel vytvořen úspěšně, uživatel je přihlášen voláním `_signInManager.SignInAsync`.

   **Poznámka:** naleznete v tématu [účtu potvrzení](xref:security/authentication/accconfirm#prevent-login-at-registration) kroky, jak zabránit okamžité přihlášení při registraci.

4. Přihlásit se.

   Uživatelé můžou přihlásit po klepnutí na **přihlášení** odkazu v horní části webu, nebo na přihlašovací stránku, může přejde v případě podobného pokusu získat přístup k součásti lokality, která vyžaduje ověření. Když uživatel odešle formulář na přihlašovací stránku, `AccountController` `Login` akce je volána.

   `Login` Volání akce `PasswordSignInAsync` na `_signInManager` objektu (poskytnuté `AccountController` pomocí vkládání závislostí).

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   Základní `Controller` třídy zpřístupňuje `User` vlastnost, která se dá dostat z metody kontroleru. Například můžete zobrazit výčet `User.Claims` a rozhodnutí o autorizaci. Další informace najdete v tématu [autorizace](xref:security/authorization/index).

5. Odhlaste.

   Kliknutím **Odhlásit** propojit volání `LogOut` akce.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Předchozí kód výše volání `_signInManager.SignOutAsync` metody. `SignOutAsync` Metoda vymaže deklarací identity uživatele uložené v souboru cookie.

<a name="pw"></a>
6. Konfigurace.

   Identita má některé výchozí chování, které mohou být přepsána nastaveními v třída při spuštění aplikace. `IdentityOptions` není potřeba konfigurovat při použití výchozí chování. Následující kód nastaví několik možností, jak síly hesla:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   Další informace o tom, jak nakonfigurovat Identity, najdete v části [konfigurace Identity](xref:security/authentication/identity-configuration).

   Můžete také konfigurovat datový typ primárního klíče, naleznete v tématu [konfigurace Identity primárních klíčů datový typ](xref:security/authentication/identity-primary-key-configuration).

7. Zobrazí se databáze.

   Pokud vaše aplikace používá databázi serveru SQL Server (výchozí nastavení na Windows a pro uživatele sady Visual Studio), můžete zobrazit aplikace vytvořená databáze. Můžete použít **SQL Server Management Studio**. Můžete také z aplikace Visual Studio vyberte **zobrazení** > **Průzkumník objektů systému SQL Server**. Připojte se k **(localdb) \MSSQLLocalDB**. Databáze s názvem odpovídajícím `aspnet-<name of your project>-<guid>` se zobrazí.

   ![Kontextové nabídky v tabulce databáze AspNetUsers](identity/_static/04-db.png)

   Rozbalte databázi a její **tabulky**, klepněte pravým tlačítkem myši **dbo. AspNetUsers** tabulce a vybrat **Data zobrazení**.

8. Zkontrolujte, jestli funguje Identity

    Výchozí hodnota *webové aplikace ASP.NET Core* šablona projektu umožňuje uživatelům přístup k žádné akci v aplikaci bez nutnosti přihlášení. Chcete-li ověřit, jestli ASP.NET Identity funguje, přidejte`[Authorize]` atribut `About` akce `Home` Kontroleru.

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Spustit projekt pomocí **Ctrl** + **F5** a přejděte **o** stránky. Může používat jenom ověření uživatelé **o** stránky teď ASP.NET vás přesměruje na přihlašovací stránku a přihlásit nebo zaregistrovat.

    # <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

    Otevřete okno příkazového řádku a přejděte do kořenového adresáře projektu adresáře, který obsahuje `.csproj` souboru. Spustit [dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz ke spuštění aplikace:

    ```csharp
    dotnet run 
    ```

    Adresa URL zadaná ve výstupu vyhledejte [dotnet spustit](/dotnet/core/tools/dotnet-run) příkazu. Adresa URL by měly odkazovat na `localhost` s vygenerované číslo portu. Přejděte **o** stránky. Může používat jenom ověření uživatelé **o** stránky teď ASP.NET vás přesměruje na přihlašovací stránku a přihlásit nebo zaregistrovat.

    ---

## <a name="identity-components"></a>Komponenty identit

Primární referenční sestavení pro systém identit je `Microsoft.AspNetCore.Identity`. Tento balíček obsahuje základní sadu rozhraní pro ASP.NET Core Identity a je součástí `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Tyto závislosti jsou potřeba pro použití systému identit v aplikacích ASP.NET Core:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Obsahuje požadované typy pro použití s Entity Framework Core Identity.

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core je technologie přístup doporučené dat společnosti Microsoft pro relační databáze jako SQL Server. Pro účely testování můžete použít `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies` -Middlewaru, který umožňuje aplikaci používat na základě souboru cookie ověřování.

## <a name="migrating-to-aspnet-core-identity"></a>Migrace do ASP.NET Core Identity

Pro další informace a pokyny k migraci svou existující identitu úložiště najdete v tématu [migrovat ověřování a identita](xref:migration/identity).

## <a name="setting-password-strength"></a>Nastavení síly hesla

Zobrazit [konfigurace](#pw) ukázku, která nastavuje minimální požadavky.

## <a name="next-steps"></a>Další kroky

* [Migrace ověřování a identita](xref:migration/identity)
* [Potvrzení účtu a obnovení hesla](xref:security/authentication/accconfirm)
* [Dvoufaktorové ověřování přes SMS](xref:security/authentication/2fa)
* [Facebook, Google a externí zprostředkovatel ověřování](xref:security/authentication/social/index)
