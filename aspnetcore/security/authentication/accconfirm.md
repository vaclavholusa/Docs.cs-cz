---
title: Potvrzení účtu a obnovení hesla v ASP.NET Core
author: rick-anderson
description: Naučte se vytvářet aplikace ASP.NET Core pomocí e-mailu potvrzení a heslo resetovat.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: d7c1aea2b533fc614eb25c537b72bea773e76077
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252279"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Potvrzení účtu a obnovení hesla v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Audette Jan](https://twitter.com/joeaudette)

Tento kurz ukazuje, jak vytvořit aplikaci ASP.NET Core pomocí e-mailu potvrzení a heslo resetovat. Tento kurz je určen **není** začátku tématu. Měli byste se seznámit s:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Ověřování](xref:security/authentication/index)
* [Potvrzení účtu a obnovení hesla](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

V tématu [tento PDF soubor](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) pro verze ASP.NET Core MVC 1.1 a 2.x.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Vytvořte nový projekt ASP.NET Core pomocí rozhraní příkazového řádku .NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* `--auth Individual` Určuje šablonu projektu na jednotlivé uživatelské účty.
* V systému Windows, přidejte `-uld` možnost. Určuje, že místo SQLite by použít LocalDB.
* Spustit `new mvc --help` získání nápovědy na tento příkaz.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Pokud používáte rozhraní příkazového řádku nebo SQLite, spusťte následující příkazy v příkazovém okně:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Určuje šablonu projektu na jednotlivé uživatelské účty.
* V systému Windows, přidejte `-uld` možnost. Určuje, že místo SQLite by použít LocalDB.
* Spustit `new mvc --help` získání nápovědy na tento příkaz.

---

Alternativně můžete vytvořit nový projekt ASP.NET Core pomocí sady Visual Studio:

* V sadě Visual Studio vytvořte novou **webové aplikace** projektu.
* Vyberte **jádro ASP.NET 2.0**. **.NET core** je vybrána na následujícím obrázku, ale můžete vybrat **rozhraní .NET Framework**.
* Vyberte **změna ověřování** a nastavte na **jednotlivé uživatelské účty**.
* Ponechte výchozí **úložiště uživatelských účtů v aplikaci**.

![Dialogové okno Nový projekt zobrazující "Jednotlivých uživatelských účtů radio" vybrali](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Otestovat novou registraci uživatele

Spuštění aplikace, vyberte **zaregistrovat** propojit a zaregistrovat uživatele. Postupujte podle pokynů ke spuštění migrace Entity Framework Core. V tomto okamžiku je pouze ověření na e-mailu [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atribut. Po odeslání registrace, jste přihlášení do aplikace. Později v tomto kurzu se kód aktualizuje, nelze noví uživatelé přihlásit, dokud ověřena e-mailu.

## <a name="view-the-identity-database"></a>Zobrazení Identity databáze

V tématu [pracovat s SQLite v projektu ASP.NET MVC základní](xref:tutorials/first-mvc-app-xplat/working-with-sql) pokyny o tom, jak zobrazit databáze SQLite.

Pro sadu Visual Studio:

* Z **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server** (SSOX).
* Přejděte na **(localdb) MSSQLLocalDB (SQL Server 13)**. Klikněte pravým tlačítkem na **dbo. AspNetUsers** > **zobrazení dat**:

![Kontextové nabídky pro tabulku AspNetUsers v Průzkumníku objektů systému SQL Server](accconfirm/_static/ssox.png)

Poznámka: v tabulce `EmailConfirmed` pole je `False`.

Můžete chtít tento e-mail znovu použít v dalším kroku při aplikace odešle e-mail s potvrzením. Klikněte pravým tlačítkem myši na řádek a vyberte **odstranit**. Odstranění e-mailový alias usnadní v následujících krocích.

---

## <a name="require-https"></a>Vyžadovat protokol HTTPS

V tématu [vyžadují protokol HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Požadovat potvrzení e-mailu

Je osvědčeným postupem potvrďte e-mailu nové registrace uživatele. E-mailem potvrzení pomáhá ověřte, že nejsou zosobnění někdo jiný (to znamená, že nebyly zaregistrována někoho jiného e-mailu). Předpokládejme, že jste měli diskusní fórum, a chcete zabránit "yli@example.com"od registrace jako"nolivetto@contoso.com". Bez potvrzení e-mailu "nolivetto@contoso.com" může přijímat nežádoucí e-mailu z vaší aplikace. Předpokládejme, že uživatel omylem zaregistrován jako "ylo@example.com" a kdyby zaznamenali chyby v pravopisu systému "yli". Se nebudou moci používat obnovení hesla, protože aplikace nemá správnou e-mailovou. Potvrzení e-mailu poskytuje jen omezenou ochrany ze robotů. Potvrzení e-mailu neposkytuje ochranu z uživatelé se zlými úmysly s mnoha e-mailové účty.

Obvykle budete chtít zabránit noví uživatelé publikování všechna data na webové stránky, než budou mít potvrzené e-mailu.

Aktualizace `ConfigureServices` tak, aby vyžadovala potvrzen e-mailu:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` zabraňuje registrovaní uživatelé přihlásit, dokud je potvrzen e-mailu.

### <a name="configure-email-provider"></a>Nakonfigurujte poskytovatele tak e-mailu

V tomto kurzu se sendgrid vám umožňuje používá k odesílání e-mailu. Musíte sendgrid vám umožňuje účtu a klíč k odeslání e-mailu. Můžete vytvořit další poskytovatele e-mailu. ASP.NET Core 2.x zahrnuje `System.Net.Mail`, který umožňuje odeslat e-mailu z vaší aplikace. Doporučujeme, aby že použití sendgrid vám umožňuje nebo jinou e-mailovou službu pro odeslání e-mailu. SMTP je obtížné zabezpečení a nastavit správně.

[Možnosti vzor](xref:fundamentals/configuration/options) se používá pro přístup k účtu a klíč nastavení uživatele. Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).

Vytvořte třídu načíst klíč zabezpečení e-mailu. Tato ukázka `AuthMessageSenderOptions` je v vytvořit třídu *Services/AuthMessageSenderOptions.cs* souboru:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Nastavte `SendGridUser` a `SendGridKey` s [nástroj tajný klíč správce](xref:security/app-secrets). Příklad:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

V systému Windows, tajný klíč správce ukládá páry klíčů/hodnota v *secrets.json* v soubor `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` adresáře.

Obsah *secrets.json* soubor není zašifrován. *Secrets.json* souboru je uveden níže ( `SendGridKey` hodnota byla odebrána.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Konfigurace spuštění používat AuthMessageSenderOptions

Přidat `AuthMessageSenderOptions` ke kontejneru služby na konci `ConfigureServices` metoda v *Startup.cs* souboru:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Konfigurovat třídu AuthMessageSender

Tento kurz ukazuje, jak přidat e-mailová oznámení prostřednictvím [sendgrid vám umožňuje](https://sendgrid.com/), ale můžete odesílat e-mailu pomocí protokolu SMTP a další mechanismy.

Nainstalujte `SendGrid` balíček NuGet:

* Z příkazového řádku:

    `dotnet add package SendGrid`

* Z konzoly Správce balíčků zadejte následující příkaz:

  `Install-Package SendGrid`

V tématu [začněte sendgridu zadarmo](https://sendgrid.com/free/) zaregistrovat bezplatný účet sendgrid vám umožňuje.

#### <a name="configure-sendgrid"></a>Konfigurace sendgrid vám umožňuje

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

Ke konfiguraci Sendgridu, přidejte kód podobný následujícímu v *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Přidejte kód v *Services/MessageServices.cs* podobný následujícímu konfigurace sendgrid vám umožňuje:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Povolit obnovení potvrzení a heslo účtu

Šablona má kód pro obnovení potvrzení a heslo účtu. Najít `OnPostAsync` metoda v *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

Nově zaregistrovaný uživatelům zabránit v automaticky přihlášený při psaní komentářů následující řádek:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Kompletní metoda je zobrazena změněné řádek zvýrazněna:

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Chcete-li povolit potvrzení účtu, zrušte komentář u následující kód:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Poznámka:** kód znemožňuje nově zaregistrovaný uživatele automaticky přihlášený při psaní komentářů následující řádek:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Povolit obnovení hesla uncommenting kód `ForgotPassword` akce *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Zrušením komentáře u prvku formuláře v *Views/Account/ForgotPassword.cshtml*. Můžete chtít odebrat `<p> For more information on how to enable reset password ... </p>` element, který obsahuje odkaz na tohoto článku.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Zaregistrovat, potvrďte e-mailu a resetování hesla

Spusťte webovou aplikaci a testování potvrzení účtu a heslo pro obnovení toku.

* Spusťte aplikaci a zaregistrovat nového uživatele

  ![Zobrazení zaregistrovat účet webové aplikace](accconfirm/_static/loginaccconfirm1.png)

* Zkontrolujte e-mailu pro potvrzení propojení účtu. V tématu [ladění e-mailu](#debug) Pokud neobdržíte e-mail.
* Kliknutím na odkaz k potvrzení e-mailu.
* Přihlaste se pomocí vaší e-mailu a heslo.
* Odhlaste se.

### <a name="view-the-manage-page"></a>Zobrazení stránky Správa

Vyberte jméno uživatele v prohlížeči: ![okno prohlížeče s uživatelským jménem](accconfirm/_static/un.png)

Možná budete muset Rozbalit navigační panel zobrazíte uživatelské jméno.

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Zobrazí se stránka Správa s **profil** vybrána karta. **E-mailu** zobrazí zaškrtávací políčko označující e-mailu, bylo potvrzeno.

![Stránka Správa](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

To je uvedeno dále v tomto kurzu.
![Stránka Správa](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Test resetování hesla

* Pokud jste přihlášeni, vyberte **odhlášení**.
* Vyberte **přihlásit** propojení a vyberte **zapomněli jste heslo?** odkaz.
* Zadejte e-mailu, které jste použili k registraci účtu.
* Se odeslal e-mail s odkazem pro resetování hesla. Zkontrolujte e-mailu a klikněte na odkaz k resetování hesla. Po vaše heslo bylo resetováno úspěšně, můžete přihlásit e-mailu a nové heslo.

<a name="debug"></a>

### <a name="debug-email"></a>Ladění e-mailu

Pokud nelze získat pracovní e-mailu:

* Vytvoření [konzolovou aplikaci k odesílání e-mailu](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Zkontrolujte [e-mailu aktivity](https://sendgrid.com/docs/User_Guide/email_activity.html) stránky.
* Zkontrolujte složky nevyžádané pošty.
* Zkuste jinou e-mailový alias na jinou e-mailovou zprostředkovatele (Microsoft, Yahoo, z Gmailu atd.)
* Pokus o odeslání na jiné e-mailové účty.

**Nejlepším postupem zabezpečení** je **není** použít produkční tajných klíčů v testovacích a vývojových. Pokud publikujete aplikaci do Azure, můžete tajné klíče sendgrid vám umožňuje nastavit jako nastavení aplikace na portálu Azure webové aplikace. Konfigurace systému je nastavený čtení klíčů z proměnných prostředí.

## <a name="combine-social-and-local-login-accounts"></a>Kombinování sociálních a místní přihlášení účtů

K dokončení této části, musíte nejdřív povolit externí zprostředkovatel ověřování. V tématu [Facebook, Google a externí zprostředkovatel ověřování](xref:security/authentication/social/index).

Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociálních účty. V tomto pořadí "RickAndMSFT@gmail.com" je poprvé vytvořen jako místní přihlášení; však můžete nejprve vytvořte účet jako přihlášení prostřednictvím sociální sítě a pak přidejte místní přihlášení.

![Webové aplikace: RickAndMSFT@gmail.com uživatel ověřený](accconfirm/_static/rick.png)

Klikněte na **spravovat** odkaz. Poznámka: externí 0 (sociální přihlášení) spojené s tímto účtem.

![Správa zobrazení](accconfirm/_static/manage.png)

Klikněte na odkaz na jinou službu přihlášení a přijímat žádosti o aplikaci. Na následujícím obrázku je Facebook zprostředkovatele externího ověřování:

![Spravovat seznam Facebook zobrazení externích přihlášení.](accconfirm/_static/fb.png)

Dva účty jsou spojena. Budete moci přihlásit pomocí obou. Můžete chtít uživatelům v případě, že své služby ověřování přihlášení prostřednictvím sociální sítě, jsou vypnuty nebo více pravděpodobně ztrátu přístupu k účtu mají sociálních přidat místní účty.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Povolení potvrzení účtu po lokalitu má uživatelé

Povolení potvrzení účtu v lokalitě s uživateli zamezí všichni stávající uživatelé. Stávající uživatelé jsou uzamčen, protože nejsou potvrzen své účty. Existující uzamčení uživatelů obejít, použijte jednu z následujících postupů:

* Aktualizace databáze k označení všichni stávající uživatelé, jako je potvrzen.
* Zkontrolujte stávající uživatele. Například batch odesílání e-mailů s odkazy na potvrzení.
