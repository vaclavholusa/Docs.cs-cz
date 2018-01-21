---
title: "Potvrzení účtu a obnovení hesla v ASP.NET Core"
author: rick-anderson
description: "Ukazuje, jak vytvořit aplikaci ASP.NET Core pomocí e-mailu potvrzení a heslo resetovat."
ms.author: riande
manager: wpickett
ms.date: 12/1/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: b004a8e7680b203416552e5a7a2809799e657759
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Potvrzení účtu a obnovení hesla v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Audette Jan](https://twitter.com/joeaudette) 

Tento kurz ukazuje, jak vytvořit aplikaci ASP.NET Core pomocí e-mailu potvrzení a heslo resetovat.

## <a name="create-a-new-aspnet-core-project"></a>Vytvořte nový projekt ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Tento krok platí pro Visual Studio v systému Windows. Najdete v části Další pokyny rozhraní příkazového řádku.

Tento kurz vyžaduje Visual Studio 2017 Preview 2 nebo novější.

* V sadě Visual Studio vytvořte nový projekt webové aplikace.
* Vyberte **jádro ASP.NET 2.0**. Následující obrázek zobrazit **.NET Core** vybrána, ale můžete vybrat **rozhraní .NET Framework**.
* Vyberte **změna ověřování** a nastavte na **jednotlivé uživatelské účty**.
* Ponechte výchozí **úložiště uživatelských účtů v aplikaci**.

![Dialogové okno Nový projekt zobrazující "Jednotlivých uživatelských účtů radio" vybrali](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Tento kurz vyžaduje Visual Studio 2017 nebo novější.

* V sadě Visual Studio vytvořte nový projekt webové aplikace.
* Vyberte **změna ověřování** a nastavte na **jednotlivé uživatelské účty**.

![Dialogové okno Nový projekt zobrazující "Jednotlivých uživatelských účtů radio" vybrali](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a>Vytvoření projektu .NET core rozhraní příkazového řádku pro systému macOS a Linux

Pokud používáte rozhraní příkazového řádku nebo SQLite, spusťte následující příkazy v příkazovém okně:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual`Určuje šablonu na jednotlivé uživatelské účty.
* V systému Windows, přidejte `-uld` možnost. `-uld` Možnost vytvoří připojovací řetězec databáze LocalDB, nikoli databáze SQLite.
* Spustit `new mvc --help` získání nápovědy na tento příkaz.

## <a name="test-new-user-registration"></a>Otestovat novou registraci uživatele

Spuštění aplikace, vyberte **zaregistrovat** propojit a zaregistrovat uživatele. Postupujte podle pokynů ke spuštění migrace Entity Framework Core. V tomto okamžiku je pouze ověření na e-mailu [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atribut. Po odeslání registrace jste přihlášení do aplikace. Později v tomto kurzu Změníme to, které noví uživatelé nelze přihlásit, dokud ověřena e-mailu.

## <a name="view-the-identity-database"></a>Zobrazení Identity databáze

# <a name="sql-servertabsql-server"></a>[SQL Server](#tab/sql-server)

* Z **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server** (SSOX). 
* Přejděte na **(localdb) MSSQLLocalDB (SQL Server 13)**. Klikněte pravým tlačítkem na **dbo. AspNetUsers** > **zobrazení dat**:

![Kontextové nabídky pro tabulku AspNetUsers v Průzkumníku objektů systému SQL Server](accconfirm/_static/ssox.png)

Poznámka: `EmailConfirmed` pole je `False`.

Můžete chtít tento e-mail znovu použít v dalším kroku při aplikace odešle e-mail s potvrzením. Klikněte pravým tlačítkem myši na řádek a vyberte **odstranit**. Odstranění e-mailu alias teď bude jednodušší v následujících krocích.

# <a name="sqlitetabsqlite"></a>[SQLite](#tab/sqlite)

V tématu [práce s SQLite v projektu ASP.NET MVC základní](xref:tutorials/first-mvc-app-xplat/working-with-sql) pokyny o tom, jak zobrazit databáze SQLite. 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a>Požadovat protokol SSL a nastavení služby IIS Express pro protokol SSL

V tématu [vynucování SSL](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Požadovat potvrzení e-mailu

Je osvědčeným postupem potvrďte e-mailu nové registrace uživatele ověřit, že nejsou zosobnění někdo jiný (to znamená, že nebyly zaregistrována někoho jiného e-mailu). Předpokládejme, že jste měli diskusní fórum, a chcete zabránit "yli@example.com"od registrace jako"nolivetto@contoso.com." Bez potvrzení e-mailu "nolivetto@contoso.com" může získat nežádoucí e-mailu z vaší aplikace. Předpokládejme, že uživatel omylem zaregistrován jako "ylo@example.com" a kdyby zaznamenali chyby v pravopisu systému "yli", se nebudou moci používat obnovení hesla, protože aplikace nemá správnou e-mailovou. Potvrzení e-mailu poskytuje jen omezenou ochranu z robotů a neposkytuje ochranu proti určené odesílatelům nevyžádané pošty, kteří mají mnoho aliasy pracovní e-mailu, které můžete použít k registraci.

Obvykle budete chtít zabránit noví uživatelé publikování všechna data na webové stránky, než budou mít potvrzené e-mailu. 

Aktualizace `ConfigureServices` tak, aby vyžadovala potvrzen e-mailu:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
Předchozí řádek zabrání registrovaní uživatelé protokolována, dokud je potvrzen e-mailu. Daného řádku však nezabrání noví uživatelé se přihlášení po jejich registraci. Ve výchozím kódu přihlásí uživatel po jejich registraci. Po odhlášení se nebudou moct přihlásit znovu dokud jejich registraci. Později v tomto kurzu Změníme kódu, takže nově zaregistrovaný uživatele jsou **není** přihlášení.

### <a name="configure-email-provider"></a>Nakonfigurujte poskytovatele tak e-mailu

V tomto kurzu se sendgrid vám umožňuje používá k odesílání e-mailu. Musíte sendgrid vám umožňuje účtu a klíč k odeslání e-mailu. Můžete vytvořit další poskytovatele e-mailu. ASP.NET Core 2.x zahrnuje `System.Net.Mail`, který umožňuje odeslat e-mailu z vaší aplikace. Doporučujeme, aby že použití sendgrid vám umožňuje nebo jinou e-mailovou službu pro odeslání e-mailu.

[Možnosti vzor](xref:fundamentals/configuration/options) se používá pro přístup k účtu a klíč nastavení uživatele. Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).

Vytvořte třídu načíst klíč zabezpečení e-mailu. Tato ukázka `AuthMessageSenderOptions` je v vytvořit třídu *Services/AuthMessageSenderOptions.cs* souboru.

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Nastavte `SendGridUser` a `SendGridKey` s [nástroj tajný klíč správce](../app-secrets.md). Příklad:

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

V systému Windows, tajný klíč správce ukládá vaše páry klíčů a hodnoty v *secrets.json* soubor v adresáři %APPDATA%/Microsoft/UserSecrets/ < WebAppName-userSecretsId >.

Obsah *secrets.json* soubor není zašifrován. *Secrets.json* souboru je uveden níže ( `SendGridKey` hodnota byla odebrána.)

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Konfigurace spuštění používat AuthMessageSenderOptions

Přidat `AuthMessageSenderOptions` ke kontejneru služby na konci `ConfigureServices` metoda v *Startup.cs* souboru:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Konfigurovat třídu AuthMessageSender

Tento kurz ukazuje, jak přidat e-mailová oznámení prostřednictvím [sendgrid vám umožňuje](https://sendgrid.com/), ale můžete odesílat e-mailu pomocí protokolu SMTP a další mechanismy.

* Nainstalujte `SendGrid` balíček NuGet. Z konzoly Správce balíčků, zadejte následující příkaz:

  `Install-Package SendGrid`

* V tématu [začněte sendgridu zadarmo](https://sendgrid.com/free/) zaregistrovat bezplatný účet sendgrid vám umožňuje.

#### <a name="configure-sendgrid"></a>Konfigurace sendgrid vám umožňuje

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

* Přidejte kód v *Services/EmailSender.cs* podobný následujícímu konfigurace sendgrid vám umožňuje:

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)
* Přidejte kód v *Services/MessageServices.cs* podobný následujícímu konfigurace sendgrid vám umožňuje:

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Povolit obnovení potvrzení a heslo účtu

Šablona má kód pro obnovení potvrzení a heslo účtu. Najít `[HttpPost] Register` metoda v *AccountController.cs* souboru.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Nově zaregistrovaný uživatelům zabránit v automaticky přihlášený při psaní komentářů následující řádek:

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

Kompletní metoda je zobrazena změněné řádek zvýrazněna:

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

Poznámka: Předchozí kód se nezdaří, pokud budete implementovat `IEmailSender` a odesílat emaily s prostým textem. V tématu [tento problém](https://github.com/aspnet/Home/issues/2152) pro další informace a řešení.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Zrušte komentář kódu, chcete-li povolit potvrzení účtu.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

Poznámka: Jsme se také brání nově zaregistrovaný uživatele automaticky přihlášený při psaní komentářů následující řádek:

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Povolit obnovení hesla uncommenting kód `ForgotPassword` akce v *Controllers/AccountController.cs* souboru.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Zrušením komentáře u prvku formuláře v *Views/Account/ForgotPassword.cshtml*. Můžete chtít odebrat `<p> For more information on how to enable reset password ... </p>` element, který obsahuje odkaz na tohoto článku.

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

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


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Budeme mluvit o tuto stránku později v tomto kurzu.
![Stránka Správa](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Test resetování hesla

* Pokud jste přihlášeni, vyberte **odhlášení**.  
* Vyberte **přihlásit** propojení a vyberte **zapomněli jste heslo?** odkaz.
* Zadejte e-mailu, které jste použili k registraci účtu.
* Odešle e-mail s odkazem pro resetování hesla. Zkontrolujte e-mailu a klikněte na odkaz k resetování hesla.  Po vaše heslo bylo resetováno úspěšně, můžete se přihlásit s e-mailu a nové heslo.

<a name="debug"></a>

### <a name="debug-email"></a>Ladění e-mailu

Pokud nelze získat pracovní e-mailu:

* Zkontrolujte [e-mailu aktivity](https://sendgrid.com/docs/User_Guide/email_activity.html) stránky.
* Zkontrolujte složky nevyžádané pošty.
* Zkuste jinou e-mailový alias na jinou e-mailovou zprostředkovatele (Microsoft, Yahoo, z Gmailu atd.)
* Vytvoření [konzolovou aplikaci k odesílání e-mailu](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Pokus o odeslání na jiné e-mailové účty.

**Poznámka:** nejlepším způsobem zabezpečení nechcete použít produkční tajných klíčů v testovacích a vývojových. Pokud publikujete aplikaci do Azure, můžete tajné klíče sendgrid vám umožňuje nastavit jako nastavení aplikace na portálu Azure webové aplikace. Konfigurace systému je instalační program načíst klíče z proměnných prostředí.

## <a name="prevent-login-at-registration"></a>Zakázat přihlášení při registraci

Aktuální šablonami po dokončení registrace formuláře, uživatel se přihlásí (ověřený). Chcete obecně potvrzení e-mailu před jejich protokolování. V následující části jsme se změnit kód tak, aby vyžadovala noví uživatelé mít potvrzené e-mailu předtím, než se přihlásí. Aktualizace `[HttpPost] Login` akce v *AccountController.cs* soubor s následující zvýrazněný změny.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

**Poznámka:** nejlepším způsobem zabezpečení nechcete použít produkční tajných klíčů v testovacích a vývojových. Pokud publikujete aplikaci do Azure, můžete tajné klíče sendgrid vám umožňuje nastavit jako nastavení aplikace na portálu Azure webové aplikace. Konfigurace systému je instalační program načíst klíče z proměnných prostředí.


## <a name="combine-social-and-local-login-accounts"></a>Kombinování sociálních a místní přihlášení účtů

Poznámka: Tato část se týká pouze ASP.NET Core 1.x. Pro technologii ASP.NET základní 2.x najdete v tématu [to](https://github.com/aspnet/Docs/issues/3753) problém.

K dokončení této části, musíte nejdřív povolit externí zprostředkovatel ověřování. V tématu [povolení ověřování pomocí služby Facebook, Google a ostatní externího poskytovatele](social/index.md).

Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociálních účty. V tomto pořadí "RickAndMSFT@gmail.com" je poprvé vytvořen jako místní přihlášení; však můžete nejprve vytvořte účet jako přihlášení prostřednictvím sociální sítě a pak přidejte místní přihlášení.

![Webové aplikace: RickAndMSFT@gmail.com uživatel ověřený](accconfirm/_static/rick.png)

Klikněte na **spravovat** odkaz. Poznámka: externí 0 (sociální přihlášení) spojené s tímto účtem.

![Správa zobrazení](accconfirm/_static/manage.png)

Klikněte na odkaz na jinou službu přihlášení a přijímat žádosti o aplikaci. Na následujícím obrázku je Facebook zprostředkovatele externího ověřování:

![Spravovat seznam Facebook zobrazení externích přihlášení.](accconfirm/_static/fb.png)

Dva účty jsou spojena. Bude moct přihlásit pomocí buď účtu. Můžete chtít uživatelům v případě, že jejich sociální přihlášení ověřovací služby, jsou vypnuty nebo jejich více pravděpodobně ztratili přístup k účtu mají sociálních přidat místní účty.
