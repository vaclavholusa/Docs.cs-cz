---
title: Instalační program externí přihlášení Google v ASP.NET Core
author: rick-anderson
description: Tento kurz představuje integraci ověřování uživatele účet Google do existující aplikace ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/google-logins
ms.openlocfilehash: 878c0b16e24f48a0ee84f93393af67af1728e284
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725962"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Instalační program externí přihlášení Google v ASP.NET Core

Podle [Valeriy Novytskyy](https://github.com/01binary) a [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto kurzu se dozvíte, jak povolit uživatelům přihlásit se pomocí účtu své Google + pomocí projektu ASP.NET 2.0 základní ukázka na vytvořit [předchozí stránce](xref:security/authentication/social/index). Začneme podle [oficiální kroky](https://developers.google.com/identity/sign-in/web/devconsole-project) k vytvoření nové aplikace v konzole rozhraní API Google.

## <a name="create-the-app-in-google-api-console"></a>Vytvořit aplikaci v konzole rozhraní API Google

* Přejděte na [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) a přihlaste se. Pokud nemáte účet Google, použijte **další možnosti** > **[vytvořit účet](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  odkaz k jeho vytvoření:

![Konzoly rozhraní Google API](index/_static/GoogleConsoleLogin.png)

* Budete přesměrováni na **knihovna rozhraní API Správce** stránky:

![Stránka rozhraní API Správce knihovny](index/_static/GoogleConsoleSwitchboard.png)

* Klepněte na **vytvořit** a zadejte vaše **název projektu**:

![Dialogové okno Nový projekt](index/_static/GoogleConsoleNewProj.png)

* Po přijetí dialogové okno, budete přesměrováni zpět na stránku knihovny si vybrat funkce pro novou aplikaci. Najít **rozhraní API Google +** v seznamu a klikněte na jeho odkazu přidat funkci rozhraní API:

![Stránka rozhraní API Správce knihovny](index/_static/GoogleConsoleChooseApi.png)

* Zobrazí se stránka pro nově přidaného rozhraní API. Klepněte na **povolit** pro přidání Google + se funkce do vaší aplikace:

![Správce rozhraní API Google + API stránky](index/_static/GoogleConsoleEnableApi.png)

* Po povolení rozhraní API, klepněte na **vytvořit přihlašovací údaje** ke konfiguraci těchto tajných klíčů:

![Správce rozhraní API Google + API stránky](index/_static/GoogleConsoleGoCredentials.png)

* Zvolte:
   * **Google + rozhraní API**
   * **Webový server (například node.js, Tomcat)**, a
   * **Uživatelská data**:

![Stránka přihlašovací údaje rozhraní API Správce: Podívejte se, co typ pověření budete potřebovat panely](index/_static/GoogleConsoleChooseCred.png)

* Klepněte na **jaké přihlašovací údaje potřebuji?** kterého přejdete na druhý krok konfigurace aplikací **vytvoření ID klienta OAuth 2.0**:

![Stránka přihlašovací údaje rozhraní API Správce: vytvoření ID klienta OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* Protože vytváříme projektu Google + s jedním funkcí (přihlášení), můžeme zadat stejný **název** pro ID klienta OAuth 2.0, jako jsme použili pro projekt.

* Zadejte váš vývojový identifikátor URI s `/signin-google` připojí do **autorizováno přesměrování identifikátory URI** pole (například: `https://localhost:44320/signin-google`). Ověřování Google nakonfigurované později v tomto kurzu bude automaticky zpracovávat požadavky na `/signin-google` trasy k implementaci toku OAuth.

> [!NOTE]
> Segment identifikátoru URI `/signin-google` je nastaven jako výchozí zpětného volání zprostředkovatele ověřování Google. Můžete změnit výchozí identifikátor URI zpětného volání při konfiguraci middleware ověřování Google prostřednictvím zděděnou [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) vlastnost [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) třídy.

* Stisknutím klávesy TAB přidat **autorizováno přesměrování identifikátory URI** položku.

* Klepněte na **vytvořit ID klienta**, která přejde třetí krok **nastavení na obrazovce souhlas OAuth 2.0**:

![Stránka přihlašovací údaje rozhraní API Správce: nastavení na obrazovce souhlas OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* Zadejte vaše veřejné přístupem **e-mailová adresa** a **název produktu** zobrazené pro vaši aplikaci při Google + vyzývá uživatele k přihlášení. Další možnosti jsou k dispozici v části **další možnosti přizpůsobení**.

* Klepněte na **pokračovat** k přejděte k poslednímu kroku **stáhnout přihlašovací údaje**:

![Stránka přihlašovací údaje rozhraní API Správce: Stáhněte si přihlašovací údaje](index/_static/GoogleConsoleFinish.png)

* Klepněte na **Stáhnout** uložit soubor JSON s tajné klíče aplikace, a **provádí** k dokončení vytvoření nové aplikace.

* Při nasazování webu budete potřebovat k pokroku **konzoly Google** a zaregistrujte novou veřejnou adresu url.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID a ClientSecret

Odkaz citlivá nastavení, jako je Google `Client ID` a `Client Secret` do konfigurace vaší aplikace pomocí [tajný klíč správce](xref:security/app-secrets). Pro účely tohoto kurzu, název tokeny `Authentication:Google:ClientId` a `Authentication:Google:ClientSecret`.

Hodnoty pro tyto tokeny naleznete v souboru JSON stáhli v předchozím kroku v části `web.client_id` a `web.client_secret`.

## <a name="configure-google-authentication"></a>Konfigurace ověřování Google

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

Přidání služby Google `ConfigureServices` metoda v *Startup.cs* souboru:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Šablona projektu použili v tomto kurzu zajistí, že [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) balíček nainstalován.

* Chcete-li nainstalovat tento balíček s Visual Studio 2017, klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.
* Chcete-li nainstalovat s .NET Core rozhraní příkazového řádku, spusťte následující v adresáři projektu:

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

Přidat v middlewaru Google `Configure` metoda v *Startup.cs* souboru:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

Najdete v článku [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) referenční dokumentace rozhraní API pro další informace o možnostech konfigurace nepodporuje ověřování Google. To slouží k požadavku na jiné informace o uživateli.

## <a name="sign-in-with-google"></a>Přihlaste se pomocí služby Google

Spusťte aplikaci a klikněte na tlačítko **přihlásit**. Zobrazí se možnost přihlásit se přes Google:

![Webová aplikace spuštěna v Microsoft Edge: uživatel není ověřen.](index/_static/DoneGoogle.png)

Když kliknete na Google, budete přesměrováni na Google pro ověřování:

![Dialogové okno ověřování Google](index/_static/GoogleLogin.png)

Po zadání přihlašovacích údajů Google, pak budete přesměrováni zpět na webovou stránku, kde můžete nastavit e-mailu.

Nyní jste se přihlásili pomocí svých přihlašovacích údajů Google:

![Webová aplikace spuštěna v Microsoft Edge: uživatel ověřený](index/_static/Done.png)

## <a name="troubleshooting"></a>Poradce při potížích

* Pokud se zobrazí `403 (Forbidden)` chybová stránka ze své vlastní aplikace při spuštění v režimu pro vývoj (nebo přerušení ladicího s ke stejné chybě), ujistěte se, že **rozhraní API Google +** povolen v **knihovna rozhraní API Správce** podle následujících kroků uvedených v tomto [starší na této stránce](#create-the-app-in-google-api-console). Pokud nefunguje přihlášení a se nezobrazují žádné chyby, přepněte do režimu vývoj usnadnění ladění problém.
* **ASP.NET Core 2.x pouze:** pokud identita není nastavena voláním `services.AddIdentity` v `ConfigureServices`, pokusu o ověření bude mít za následek *ArgumentException –: musí být použita volba 'SignInScheme'*. Šablona projektu použili v tomto kurzu zajistí, že to probíhá.
* Pokud databázi lokality s použitím počáteční migrace vytvořena nebyla, zobrazí se *databázová operace se nezdařila při zpracování požadavku* chyby. Klepněte na **použít migrace** vytvořit databázi a aktualizujte pokračujte dále chyba.

## <a name="next-steps"></a>Další kroky

* Tento článek vám ukázal, jak můžete ověřit pomocí služby Google. Můžete postupovat podle podobný postup k ověření pomocí jiných poskytovatelů uvedené na [předchozí stránce](xref:security/authentication/social/index).

* Jakmile budete publikovat web vaší webové aplikace Azure, byste měli obnovit `ClientSecret` v konzole rozhraní API Google.

* Nastavte `Authentication:Google:ClientId` a `Authentication:Google:ClientSecret` jako nastavení aplikace v portálu Azure. Konfigurace systému je nastavený čtení klíčů z proměnných prostředí.
