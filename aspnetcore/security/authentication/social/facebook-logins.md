---
title: Instalační program externí přihlášení sítě Facebook v ASP.NET Core
author: rick-anderson
description: Tento kurz představuje integraci ověřování uživatele účet Facebook do existující aplikace ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/facebook-logins
ms.openlocfilehash: cabb5acc6e593c02c20b3403b39c601ce26a4d99
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688980"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Instalační program externí přihlášení sítě Facebook v ASP.NET Core

Podle [Valeriy Novytskyy](https://github.com/01binary) a [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se dozvíte, jak povolit uživatelům se přihlásit pomocí účtu sítě Facebook pomocí projekt ASP.NET 2.0 základní ukázka vytvořený na [předchozí stránce](xref:security/authentication/social/index). Vyžaduje ověřování Facebook [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) balíček NuGet. Začneme vytvořením ID aplikace pro Facebook podle [oficiální kroky](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Vytvoření aplikace ve službě Facebook.

* Přejděte na [Facebook vývojáři aplikace](https://developers.facebook.com/apps/) stránce a přihlaste se. Pokud nemáte účet Facebook, použijte **zaregistrovat pro Facebook** odkaz na přihlašovací stránku k jeho vytvoření.

* Klepněte **přidejte novou aplikaci** tlačítko v pravém horním rohu k vytvoření nového ID aplikace.

   ![Facebook pro portál pro vývojáře otevřít v Microsoft Edge](index/_static/FBMyApps.png)

* Vyplňte formulář a klepněte **vytvoření ID aplikace** tlačítko.

   ![Vytvoření nové aplikace ID formuláře](index/_static/FBNewAppId.png)

* Na **vybrat produkt** klikněte na tlačítko **nastavit až** na **Facebook přihlášení** karty.

   ![Stránka instalační program produktu](index/_static/FBProductSetup.png)

* **Rychlý Start** průvodce se spustí s **vybrat platformu** jako první stránka. Vynechat Průvodce prozatím kliknutím **nastavení** odkaz v nabídce na levé straně:

   ![Přeskočit rychlý Start](index/_static/FBSkipQuickStart.png)

* Zobrazí se **nastavení klienta OAuth** stránky:

![Stránka OAuth nastavení klienta](index/_static/FBOAuthSetup.png)

* Zadejte váš vývojový identifikátor URI s */signin-facebook* připojí do **identifikátory URI pro přesměrování platný OAuth** pole (například: `https://localhost:44320/signin-facebook`). Ověřování Facebook nakonfigurované později v tomto kurzu bude automaticky zpracovávat požadavky na */signin-facebook* trasy k implementaci toku OAuth.

* Klikněte na tlačítko **uložit změny**.

* Klikněte **řídicí panel** odkaz v levém navigačním panelu. 

    Na této stránce, poznamenejte si vaše `App ID` a `App Secret`. Přidá do vaší aplikace ASP.NET Core v další části:

   ![Řídicí panel vývojáře Facebook](index/_static/FBDashboard.png)

* Při nasazování webu potřebujete k pokroku **Facebook přihlášení** stránce instalace a registrace nový veřejný identifikátor URI.

## <a name="store-facebook-app-id-and-app-secret"></a>Ukládání ID aplikace pro Facebook a tajný klíč aplikace

Odkaz citlivá nastavení, jako je Facebook `App ID` a `App Secret` do konfigurace vaší aplikace pomocí [tajný klíč správce](xref:security/app-secrets). Pro účely tohoto kurzu, název tokeny `Authentication:Facebook:AppId` a `Authentication:Facebook:AppSecret`.

Spuštěním následujících příkazů bezpečně uložit `App ID` a `App Secret` pomocí tajný klíč správce:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Konfigurace ověřování Facebook

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

Přidání služby Facebook `ConfigureServices` metoda v *Startup.cs* souboru:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Nainstalujte [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) balíčku.

* Chcete-li nainstalovat tento balíček s Visual Studio 2017, klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.
* Chcete-li nainstalovat s .NET Core rozhraní příkazového řádku, spusťte následující v adresáři projektu:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Přidat middlewaru Facebook v `Configure` metoda v *Startup.cs* souboru:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

Najdete v článku [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) referenční dokumentace rozhraní API pro další informace o možnostech konfigurace nepodporuje ověřování Facebook. Možnosti konfigurace umožňuje:

* Žádost různé informace o uživateli.
* Přidejte argumenty řetězce dotazu přizpůsobit přihlašovací prostředí.

## <a name="sign-in-with-facebook"></a>Přihlaste se pomocí sítě Facebook

Spusťte aplikaci a klikněte na tlačítko **přihlásit**. Zobrazí se možnost přihlásit se přes Facebook.

![Webové aplikace: uživatel není ověřen.](index/_static/DoneFacebook.png)

Po kliknutí na **Facebook**, budete přesměrováni na Facebook pro ověření:

![Stránka ověřování Facebook](index/_static/FBLogin.png)

Ověřování Facebook požadavky veřejného profilu a e-mailovou adresu ve výchozím nastavení:

![Stránka ověřování Facebook](index/_static/FBLoginDone.png)

Po zadání přihlašovacích údajů Facebook budete přesměrováni zpět na váš web, kde můžete nastavit e-mailu.

Nyní jste se přihlásili pomocí přihlašovacích údajů Facebook:

![Webové aplikace: uživatel ověřený](index/_static/Done.png)

## <a name="troubleshooting"></a>Poradce při potížích

* **ASP.NET Core 2.x pouze:** pokud identita není nastavena voláním `services.AddIdentity` v `ConfigureServices`, pokusu o ověření bude mít za následek *ArgumentException –: musí být použita volba 'SignInScheme'*. Šablona projektu použili v tomto kurzu zajistí, že to probíhá.
* Jestliže databáze lokality nebyl vytvořen použitím počáteční migrace, můžete získat *databázová operace se nezdařila při zpracování požadavku* chyby. Klepněte na **použít migrace** vytvořit databázi a aktualizujte pokračujte dále chyba.

## <a name="next-steps"></a>Další kroky

* Tento článek vám ukázal, jak můžete ověřit pomocí služby Facebook. Můžete postupovat podle podobný postup k ověření pomocí jiných poskytovatelů uvedené na [předchozí stránce](xref:security/authentication/social/index).

* Jakmile budete publikovat web vaší webové aplikace Azure, byste měli obnovit `AppSecret` v portálu pro vývojáře Facebook.

* Nastavte `Authentication:Facebook:AppId` a `Authentication:Facebook:AppSecret` jako nastavení aplikace v portálu Azure. Konfigurace systému je nastavený čtení klíčů z proměnných prostředí.
