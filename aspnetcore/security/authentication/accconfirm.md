---
title: Potvrzení účtu a obnovení hesla v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak vytvořit aplikaci ASP.NET Core s e-mailové potvrzení a resetováním hesla.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803269"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Potvrzení účtu a obnovení hesla v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Joe Audette](https://twitter.com/joeaudette)

V tomto kurzu se dozvíte, jak vytvořit aplikaci ASP.NET Core s e-mailové potvrzení a resetováním hesla. Tento kurz je **není** začátku tématu. Měli byste se seznámit s:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Ověřování](xref:security/authentication/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Zobrazit [tento soubor PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) pro verze technologie ASP.NET Core MVC 1.1 a 2.x.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Vytvořte nový projekt ASP.NET Core pomocí rozhraní příkazového řádku .NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

* `--auth Individual` Určuje šablonu projektu jednotlivé uživatelské účty.
* Na Windows, přidejte `-uld` možnost. Určuje, že by měl být LocalDB použít místo SQLite.
* Spustit `new mvc --help` můžete zobrazit nápovědu pro tento příkaz.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Pokud používáte SQLite nebo rozhraní příkazového řádku, spusťte okno příkazového řádku následující:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Určuje šablonu projektu jednotlivé uživatelské účty.
* Na Windows, přidejte `-uld` možnost. Určuje, že by měl být LocalDB použít místo SQLite.
* Spustit `new mvc --help` můžete zobrazit nápovědu pro tento příkaz.

---

Alternativně můžete vytvořit nový projekt ASP.NET Core pomocí sady Visual Studio:

* V sadě Visual Studio vytvořte nový **webovou aplikaci** projektu.
* Vyberte **ASP.NET Core 2.0**. **.NET core** je vybrána na následujícím obrázku, ale můžete vybrat **rozhraní .NET Framework**.
* Vyberte **změna ověřování** a nastavte **jednotlivé uživatelské účty**.
* Ponechte výchozí **Store uživatelské účty v aplikaci**.

![Dialogové okno Nový projekt zobrazuje "Jednotlivých uživatelských účtů přepínač" vybraná](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Otestovat nové registrace uživatele

Spusťte aplikaci, vyberte **zaregistrovat** propojit a zaregistrovat uživatele. Postupujte podle pokynů pro spuštění migrace Entity Framework Core. V tomto okamžiku je pouze ověření na e-mailu [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atribut. Po odeslání registrace, jste přihlášení do aplikace. Později v tomto kurzu se kód aktualizuje tak, že noví uživatelé nemohou přihlásit, dokud e-mailu je potvrzená.

## <a name="view-the-identity-database"></a>Zobrazení databáze identit

Zobrazit [práce s SQLite v projektu aplikace ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) pokyny o tom, jak zobrazit databázi SQLite.

Pro Visual Studio:

* Z **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server** (SSOX).
* Přejděte do **(localdb) MSSQLLocalDB (SQL Server 13)**. Klikněte pravým tlačítkem na **dbo. AspNetUsers** > **zobrazení dat**:

![Místní nabídku pro tabulku AspNetUsers v Průzkumníku objektů SQL serveru](accconfirm/_static/ssox.png)

Poznámka: v tabulce `EmailConfirmed` pole je `False`.

Můžete chtít tento e-mail znovu použít v dalším kroku při ní odešle e-mail s potvrzením. Klikněte pravým tlačítkem na řádek a vyberte **odstranit**. Odstraňuje se e-mailový alias usnadňuje v následujících krocích.

---

## <a name="require-https"></a>Vyžadovat protokol HTTPS

Zobrazit [vyžadovalo protokol HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Vyžádání potvrzení e-mailu

Je osvědčeným postupem je potvrďte e-mailu nové registrace uživatele. E-mailu pomáhá potvrzení ověření někdo jiný, nejsou zosobnění (to znamená, že se ještě nezaregistrovali někoho jiného e-mailu). Předpokládejme, že jste měli diskusní fórum, a chtěli byste zabránit "yli@example.com"registroval jako"nolivetto@contoso.com". Bez potvrzení e-mailu "nolivetto@contoso.com" mohli dostávat nežádoucí e-mailu z vaší aplikace. Předpokládejme, že uživatel zaregistrován nechtěně jako "ylo@example.com" a kdyby si všimli chyba "yli". Se nebude moci použít obnovení hesla, protože aplikace nemá správnou e-mailu. Potvrzení e-mailu zajišťuje pouze omezenou ochranu před roboty. Potvrzení e-mailu neposkytuje ochranu z uživateli se zlými úmysly s mnoha e-mailové účty.

Obvykle chcete novým uživatelům zabránit v účtování všechna data na webový server dříve, než potvrzeno e-mailu.

Aktualizace `ConfigureServices` tak, aby vyžadovala potvrzeno e-mailu:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` zabraňuje registrovaných uživatelů přihlásit, dokud není potvrzené e-mailu.

### <a name="configure-email-provider"></a>Konfigurace poskytovatele e-mailu

V tomto kurzu SendGrid umožňuje odesílání e-mailů. Potřebujete účet SendGrid a klíč k odesílání e-mailu. Můžete použít jiné poskytovateli e-mailu. ASP.NET Core 2.x zahrnuje `System.Net.Mail`, který umožňuje odeslání e-mailu z vaší aplikace. Doporučujeme že použít SendGrid nebo jiné služby e-mailu k odeslání e-mailu. SMTP je obtížné zabezpečení a zařídit správné nastavení.

[Možnosti vzor](xref:fundamentals/configuration/options) slouží k přístupu k účtu a klíč nastavení. Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).

Vytvoření třídy k načtení klíče zabezpečeného e-mailu. V tomto příkladu `AuthMessageSenderOptions` třída se vytvoří v *Services/AuthMessageSenderOptions.cs* souboru:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Nastavte `SendGridUser` a `SendGridKey` s [manažera tajných nástroj](xref:security/app-secrets). Příklad:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Na Windows, manažera tajných ukládá dvojice klíčů/hodnota v *secrets.json* soubor `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` adresáře.

Obsah *secrets.json* souboru nejsou šifrovány. *Secrets.json* souboru je uveden níže ( `SendGridKey` hodnota byla odebrána.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Konfigurace spuštění používat AuthMessageSenderOptions

Přidat `AuthMessageSenderOptions` do služby kontejneru na konci `ConfigureServices` metodu *Startup.cs* souboru:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Konfigurovat třídu AuthMessageSender

Tento kurz ukazuje, jak přidat e-mailová oznámení prostřednictvím [SendGrid](https://sendgrid.com/), ale můžete odesílat e-mailu pomocí protokolu SMTP a další mechanismy.

Nainstalujte `SendGrid` balíček NuGet:

* Z příkazového řádku:

    `dotnet add package SendGrid`

* V konzole Správce balíčků zadejte následující příkaz:

  `Install-Package SendGrid`

Naleznete v tématu [začít pomocí Sendgridu zdarma](https://sendgrid.com/free/) k registraci bezplatného účtu SendGrid.

#### <a name="configure-sendgrid"></a>Konfigurace služby SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Ke konfiguraci služby SendGrid, přidejte kód, podobně jako v následujícím *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Přidejte kód do *Services/MessageServices.cs* podobně jako následující konfigurace SendGrid:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Povolit obnovení hesla a potvrzení účtu

Šablona má kód pro obnovení potvrzení a heslo účtu. Najít `OnPostAsync` metoda *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Nově zaregistrovaný uživatelům zabránit v automaticky přihlášeni tak následující řádek:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Úplná metoda se zobrazí s změněný řádek zvýrazněný:

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Chcete-li povolit potvrzení účtu, Odkomentujte následující kód:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Poznámka:** kód brání nově zaregistrovaný uživatel automaticky přihlášeni tak následující řádek:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Povolit obnovení hesla uncommenting kód `ForgotPassword` akce *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Zrušením komentáře u elementu form v *Views/Account/ForgotPassword.cshtml*. Můžete chtít odebrat `<p> For more information on how to enable reset password ... </p>` element, který obsahuje odkaz na tohoto článku.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Zaregistrovat a potvrďte e-mailu a resetování hesla

Spuštění webové aplikace a testů potvrzení účtu a heslo pro obnovení toku.

* Spusťte aplikaci a zaregistrovat nový uživatel

  ![Zobrazení účtu zaregistrovat webové aplikace](accconfirm/_static/loginaccconfirm1.png)

* Zkontrolujte svého e-mailu na odkaz pro potvrzení účtu. Zobrazit [ladění e-mailu](#debug) Pokud neobdržíte e-mailu.
* Klikněte na odkaz pro potvrzení e-mailu.
* Přihlaste se pomocí e-mailu a hesla.
* Odhlaste se.

### <a name="view-the-manage-page"></a>Zobrazení stránky Správa

Vyberte své uživatelské jméno v prohlížeči: ![okna prohlížeče s uživatelským jménem](accconfirm/_static/un.png)

Potřebujete rozšířit na navigačním panelu zobrazíte uživatelské jméno.

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Zobrazí se stránka Správa s **profilu** vybraná karta. **E-mailu** zobrazí zaškrtávací políčko označující e-mailu byl potvrzen.

![Stránka Správa](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

To je uvedeno v pozdější části kurzu.
![Stránka Správa](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Resetování hesla testu

* Pokud jste přihlášeni, vyberte **odhlášení**.
* Vyberte **přihlášení** spojit a vybrat možnost **zapomněli jste heslo?** odkaz.
* Zadejte e-mail, který jste použili k registraci účtu.
* Odešle e-mail s odkazem k resetování hesla. Zkontrolujte e-mailu a klikněte na odkaz pro resetování hesla. Po úspěšném resetování vašeho hesla můžete přihlásit e-mailu a nové heslo.

<a name="debug"></a>

### <a name="debug-email"></a>Ladění e-mailu

Pokud nelze získat pracovní e-mailu:

* Vytvoření [konzolovou aplikaci k odesílání e-mailu](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Zkontrolujte [e-mailové aktivity](https://sendgrid.com/docs/User_Guide/email_activity.html) stránky.
* Zkontrolujte složku s nevyžádanou poštou.
* Zkuste jinou e-mailový alias na jinou e-mailovou zprostředkovatele (Microsoft, Yahoo, Gmail, atd.)
* Pokuste se odeslat na jiné e-mailové účty.

**Z bezpečnostních důvodů** je **není** použití produkční tajných kódů v vývoj a testování. Pokud publikujete aplikaci do Azure, můžete nastavit SendGrid tajné kódy jako nastavení aplikace ve webové aplikaci Azure portal. Konfigurační systém je nastavený na klíče pro čtení z proměnných prostředí.

## <a name="combine-social-and-local-login-accounts"></a>Sloučit účty sociálních sítí a místní přihlášení

K dokončení této části, je nutné nejprve povolit externí zprostředkovatel ověřování. Zobrazit [Facebook, Google a externí zprostředkovatel ověřování](xref:security/authentication/social/index).

Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociální účty. V tomto pořadí "RickAndMSFT@gmail.com" se nejprve vytvoří jako místní přihlášení; však můžete nejprve vytvořte účet jako přihlášení prostřednictvím sociální sítě a pak přidat místní přihlašovací údaje.

![Webová aplikace: RickAndMSFT@gmail.com uživatel byl ověřen](accconfirm/_static/rick.png)

Klikněte na **spravovat** odkaz. Poznámka: externí 0 (přihlašování přes sociální sítě) spojená s tímto účtem.

![Správa zobrazení](accconfirm/_static/manage.png)

Klikněte na odkaz pro další přihlášení služby a přijímání požadavků aplikace. Na následujícím obrázku je Facebooku zprostředkovatele externího ověřování:

![Spravovat externí přihlášení zobrazení výpisu Facebooku](accconfirm/_static/fb.png)

Byli sloučeni dva účty. Budete moct přihlásit pomocí obou. Můžete chtít uživatelům přidat místní účty v případě nefungující Služba ověřování v jejich přihlášení prostřednictvím sociální sítě nebo spíše se jste ztratili přístup k jejich účtu na sociální síti.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Po lokality má uživatelům povolit potvrzení účtu

Povolení potvrzení účtu na webu s uživateli zamezí všichni stávající uživatelé. Stávající uživatelé jsou uzamčen, protože jejich účty nejsou potvrzeny. Obejít existující uzamčení uživatelů, použijte jednu z následujících postupů:

* Aktualizujte databázi pro označení všichni stávající uživatelé, jako je potvrzen.
* Zkontrolujte stávající uživatele. Třeba dávkové odeslání e-mailů s potvrzovací odkazy.
