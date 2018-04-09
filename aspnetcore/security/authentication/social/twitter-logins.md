---
title: Instalační program služby Twitter externí přihlášení pomocí ASP.NET Core
author: rick-anderson
description: Tento kurz představuje integrační služby Twitter účet uživatele ověřování do existující aplikace ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/twitter-logins
ms.openlocfilehash: 3f0eb9abce067108b82cf8b639cea3b120ca4b5a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>Instalační program služby Twitter externí přihlášení pomocí ASP.NET Core

Podle [Valeriy Novytskyy](https://github.com/01binary) a [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se dozvíte, jak povolit uživatelům [přihlásit pomocí svého účtu služby Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) pomocí projektu ASP.NET 2.0 základní ukázka na vytvořit [předchozí stránce](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Vytvoření aplikace v Twitter

* Přejděte na [ https://apps.twitter.com/ ](https://apps.twitter.com/) a přihlaste se. Pokud nemáte účet služby Twitter, pomocí **[nyní](https://twitter.com/signup)** odkaz k jeho vytvoření. Po přihlášení, **Správa aplikací** stránky se zobrazí:

![Otevřete správu aplikací v Microsoft Edge služby Twitter](index/_static/TwitterAppManage.png)

* Klepněte na **vytvořit novou aplikaci** a vyplňte aplikace **název**, **popis** a veřejné **webu** identifikátor URI (může to být dočasný dokud Zaregistrujte název domény):

![Vytvoření stránky aplikace](index/_static/TwitterCreate.png)

* Zadejte váš vývojový identifikátor URI s */signin-twitter* připojí do **identifikátory URI pro přesměrování platný OAuth** pole (například: `https://localhost:44320/signin-twitter`). Schéma ověřování služby Twitter nakonfigurované později v tomto kurzu bude automaticky zpracovávat požadavky na */signin-twitter* trasy k implementaci toku OAuth.

* Vyplňte zbytek formuláře a klepněte na **vytvořit aplikaci služby Twitter**. Podrobnosti o nové aplikace se zobrazí:

![Podrobnosti karty na stránce aplikace](index/_static/TwitterAppDetails.png)

* Při nasazování webu budete potřebovat k pokroku **Správa aplikací** stránky a zaregistrujte nový veřejný identifikátor URI.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Ukládání ConsumerKey služby Twitter a ConsumerSecret

Odkaz citlivá nastavení, jako je Twitter `Consumer Key` a `Consumer Secret` do konfigurace vaší aplikace pomocí [tajný klíč správce](xref:security/app-secrets). Pro účely tohoto kurzu, název tokeny `Authentication:Twitter:ConsumerKey` a `Authentication:Twitter:ConsumerSecret`.

Tyto tokeny můžete najít na **klíče a přístupové tokeny** karta po vytvoření nové aplikace služby Twitter:

![Karta klíče a přístupové tokeny](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Nakonfigurujte ověřování služby Twitter.

Šablona projektu použili v tomto kurzu zajistí, že [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) balíček je již nainstalován.

* Chcete-li nainstalovat tento balíček s Visual Studio 2017, klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.
* Chcete-li nainstalovat s .NET Core rozhraní příkazového řádku, spusťte následující v adresáři projektu:

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)
Přidání služby Twitter `ConfigureServices` metoda v *Startup.cs* souboru:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x/)
Přidat v middlewaru Twitter `Configure` metoda v *Startup.cs* souboru:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

* * *
Najdete v článku [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) referenční dokumentace rozhraní API pro další informace o možností konfigurace podporovanou ověřováním služby Twitter. To slouží k požadavku na jiné informace o uživateli.

## <a name="sign-in-with-twitter"></a>Přihlaste se pomocí služby Twitter.

Spusťte aplikaci a klikněte na tlačítko **přihlásit**. Zobrazí se možnost přihlásit se přes Twitter:

![Webové aplikace: uživatel není ověřen.](index/_static/DoneTwitter.png)

Kliknutím na **Twitter** přesměruje na Twitteru pro ověřování:

![Stránka ověřování služby Twitter.](index/_static/TwitterLogin.png)

Po zadání přihlašovacích údajů služby Twitter, budete přesměrováni zpět na webovou stránku, kde můžete nastavit e-mailu.

Nyní jste se přihlásili pomocí přihlašovacích údajů služby Twitter:

![Webové aplikace: uživatel ověřený](index/_static/Done.png)

## <a name="troubleshooting"></a>Poradce při potížích

* **ASP.NET Core 2.x pouze:** pokud identita není nastavena voláním `services.AddIdentity` v `ConfigureServices`, pokusu o ověření bude mít za následek *ArgumentException –: musí být použita volba 'SignInScheme'*. Šablona projektu použili v tomto kurzu zajistí, že to probíhá.
* Pokud databázi lokality s použitím počáteční migrace vytvořena nebyla, zobrazí se *databázová operace se nezdařila při zpracování požadavku* chyby. Klepněte na **použít migrace** vytvořit databázi a aktualizujte pokračujte dále chyba.

## <a name="next-steps"></a>Další kroky

* Tento článek vám ukázal, jak můžete ověřit pomocí služby Twitter. Můžete postupovat podle podobný postup k ověření pomocí jiných poskytovatelů uvedené na [předchozí stránce](xref:security/authentication/social/index).

* Jakmile budete publikovat web vaší webové aplikace Azure, byste měli obnovit `ConsumerSecret` v portálu pro vývojáře služby Twitter.

* Nastavte `Authentication:Twitter:ConsumerKey` a `Authentication:Twitter:ConsumerSecret` jako nastavení aplikace v portálu Azure. Konfigurace systému je nastavený čtení klíčů z proměnných prostředí.
