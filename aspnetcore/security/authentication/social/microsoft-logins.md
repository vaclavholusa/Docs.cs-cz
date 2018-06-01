---
title: Nastavení Microsoft Account externí přihlášení pomocí ASP.NET Core
author: rick-anderson
description: Tento kurz představuje integraci ověřování uživatele účtu Microsoft do existující aplikace ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/microsoft-logins
ms.openlocfilehash: a9bf7b49b1cfdfff65c639eed1e14c94c5432350
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/31/2018
ms.locfileid: "34689019"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Nastavení Microsoft Account externí přihlášení pomocí ASP.NET Core

Podle [Valeriy Novytskyy](https://github.com/01binary) a [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se dozvíte, jak povolit uživatelům přihlásit se pomocí svého účtu Microsoft pomocí projektu ASP.NET 2.0 základní ukázka na vytvořit [předchozí stránce](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Vytvoření aplikace v portálu pro vývojáře společnosti Microsoft

* Přejděte na [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) a vytvořte nebo se přihlaste účtem Microsoft:

![Přihlaste se dialogové okno](index/_static/MicrosoftDevLogin.png)

Pokud nemáte účet Microsoft, klepněte na  **[vytvořit!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** Po přihlášení budete přesměrováni na **Moje aplikace** stránky:

![Otevřete v Microsoft Edge portál pro vývojáře společnosti Microsoft](index/_static/MicrosoftDev.png)

* Klepněte na **přidat aplikaci** v pravém horním rohu a zadejte vaše **název aplikace** a **e-mailu kontaktujte**:

![Dialogové okno Nový registraci aplikace](index/_static/MicrosoftDevAppCreate.png)

* Pro účely tohoto kurzu, zrušte **instalace na základě** zaškrtávací políčko.

* Klepněte na **vytvořit** nadále **registrace** stránky. Zadejte **název** a poznamenejte si hodnotu **Id aplikace**, který použijete jako `ClientId` dál v tomto kurzu:

![Registrační stránce](index/_static/MicrosoftDevAppReg.png)

* Klepněte na **přidejte platformu** v **platformy** a vyberte **webové** platformy:

![Přidejte platformu dialogové okno](index/_static/MicrosoftDevAppPlatform.png)

* V novém **webové** platformy zadejte URL vývoj s */signin-microsoft* připojí do **adres URL pro přesměrování** pole (například: `https://localhost:44320/signin-microsoft`). Schéma ověřování Microsoft nakonfigurované později v tomto kurzu bude automaticky zpracovávat požadavky na */signin-microsoft* trasy k implementaci toku OAuth:

![Webové části platforma](index/_static/MicrosoftRedirectUri.png)

* Klepněte na **přidat adresu URL** aby adresa URL byla přidána.

* V případě potřeby zadejte další nastavení aplikace a klepněte na **Uložit** v dolní části stránky se uložit změny do konfigurace aplikací.

* Při nasazování webu budete potřebovat k pokroku **registrace** stránky a nastavte novou veřejnou adresu URL.

## <a name="store-microsoft-application-id-and-password"></a>Uložit aplikaci Microsoft Id a heslo

* Poznámka: `Application Id` zobrazí na **registrace** stránky.

* Klepněte na **generovat nové heslo** v **tajné klíče aplikace** části. Zobrazí se v poli, kde můžete kopírovat heslo aplikace:

![Dialogové okno Nový hesla generovaného](index/_static/MicrosoftDevPassword.png)

Odkaz citlivá nastavení, jako je Microsoft `Application ID` a `Password` do konfigurace vaší aplikace pomocí [tajný klíč správce](xref:security/app-secrets). Pro účely tohoto kurzu, název tokeny `Authentication:Microsoft:ApplicationId` a `Authentication:Microsoft:Password`.

## <a name="configure-microsoft-account-authentication"></a>Konfigurovat ověřování účet Microsoft

Šablona projektu použili v tomto kurzu zajistí, že [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) balíček je již nainstalován.

* Chcete-li nainstalovat tento balíček s Visual Studio 2017, klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.
* Chcete-li nainstalovat s .NET Core rozhraní příkazového řádku, spusťte následující v adresáři projektu:

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

Přidání služby Account Microsoft `ConfigureServices` metoda v *Startup.cs* souboru:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Přidat middlewaru Microsoft Account v `Configure` metoda v *Startup.cs* souboru:

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

I když tyto tokeny názvy technologiím použitým na portál pro vývojáře společnosti Microsoft `ApplicationId` a `Password`, že zveřejněné jako `ClientId` a `ClientSecret` v konfiguraci rozhraní API.

Najdete v článku [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) referenční dokumentace rozhraní API pro další informace o možností konfigurace podporovanou Account Microsoft ověřování. To slouží k požadavku na jiné informace o uživateli.

## <a name="sign-in-with-microsoft-account"></a>Přihlaste se pomocí účtu Microsoft

Spusťte aplikaci a klikněte na tlačítko **přihlásit**. Zobrazí se možnost přihlásit se pomocí Microsoft:

![Webovou aplikaci protokolu na stránce: uživatel není ověřen.](index/_static/DoneMicrosoft.png)

Když kliknete na Microsoft, budete přesměrováni do společnosti Microsoft pro ověřování. Po přihlášení pomocí Account Microsoft (pokud ještě není přihlášení) se zobrazí výzva k umožňuje aplikaci přístup k informacím:

![Dialogové okno Microsoft ověřování](index/_static/MicrosoftLogin.png)

Klepněte na **Ano** a budete přesměrováni zpět na webovou stránku, kde můžete nastavit e-mailu.

Nyní jste se přihlásili pomocí přihlašovacích údajů společnosti Microsoft:

![Webové aplikace: uživatel ověřený](index/_static/Done.png)

## <a name="troubleshooting"></a>Poradce při potížích

* Pokud zprostředkovatel Account Microsoft vás přesměruje na přihlašovací stránce chyba, vezměte na vědomí chyba název a popis parametrů řetězce dotazu přímo následující `#` (hashtag) v identifikátoru Uri.

  I když se chybová zpráva se zdá, že znamenat problém související s ověřováním Microsoft, nejčastější příčinou je identifikátor Uri neodpovídá žádné z vaší aplikace **identifikátory URI přesměrování** zadaná pro **webové** platformy .
* **ASP.NET Core 2.x pouze:** pokud identita není nastavena voláním `services.AddIdentity` v `ConfigureServices`, pokusu o ověření bude mít za následek *ArgumentException –: musí být použita volba 'SignInScheme'*. Šablona projektu použili v tomto kurzu zajistí, že to probíhá.
* Pokud databázi lokality s použitím počáteční migrace vytvořena nebyla, zobrazí se *databázová operace se nezdařila při zpracování požadavku* chyby. Klepněte na **použít migrace** vytvořit databázi a aktualizujte pokračujte dále chyba.

## <a name="next-steps"></a>Další kroky

* Tento článek vám ukázal, jak můžete ověřovat se společností Microsoft. Můžete postupovat podle podobný postup k ověření pomocí jiných poskytovatelů uvedené na [předchozí stránce](xref:security/authentication/social/index).

* Jakmile budete publikovat web vaší webové aplikace Azure, měli byste vytvořit nový `Password` v portálu pro vývojáře společnosti Microsoft.

* Nastavte `Authentication:Microsoft:ApplicationId` a `Authentication:Microsoft:Password` jako nastavení aplikace v portálu Azure. Konfigurace systému je nastavený čtení klíčů z proměnných prostředí.
