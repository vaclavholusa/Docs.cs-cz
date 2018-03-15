---
title: "Dvoufaktorové ověřování pomocí SMS v ASP.NET Core"
author: rick-anderson
description: "Zjistěte, jak nastavit dvoufaktorové ověřování (2FA) s aplikací ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: c328c6f4b674695dd1f2db8145a7ac1b8f12d36d
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>Dvoufaktorové ověřování pomocí SMS v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [mezi Devs](https://github.com/Swiss-Devs)

V tomto kurzu platí pro ASP.NET Core pouze 1.x. V tématu [vygenerovat kód QR povolení pro aplikace v ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) pro technologii ASP.NET 2.0 jádra a novější.

Tento kurz ukazuje, jak nastavit dvoufaktorové ověřování (2FA) pomocí serveru SMS. Jsou uvedeny pokyny pro [twilio](https://www.twilio.com/) a [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale můžete použít další poskytovatele serveru SMS. Doporučujeme, abyste dokončení [potvrzení účtu a heslo pro obnovení](accconfirm.md) před zahájením tohoto kurzu.

Zobrazení [hotová ukázka](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Stažení](xref:tutorials/index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Vytvořte nový projekt ASP.NET Core

Vytvořit novou webovou aplikaci ASP.NET Core s názvem `Web2FA` s jednotlivých uživatelských účtů. Postupujte podle pokynů v [vynucování SSL v aplikaci ASP.NET Core](xref:security/enforcing-ssl) nastavit a vyžadovat protokol SSL.

### <a name="create-an-sms-account"></a>Vytvoření účtu služby SMS

Vytvoření účtu služby SMS, například z [twilio](https://www.twilio.com/) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Zaznamenejte ověřovací pověření (pro twilio: accountSid a ověřovacího tokenu pro ASPSMS: Userkey a heslo).

#### <a name="figuring-out-sms-provider-credentials"></a>Zjištění přihlašovací údaje poskytovatele služby SMS

**Twilio:**  
Na kartě řídicí panel svého účtu Twilio zkopírovat **SID účtu** a **Auth token**.

**ASPSMS:**  
V nastavení svého účtu, přejděte na **Userkey** a zkopírujte jej společně s vaší **heslo**.

Později jsme uloží tyto hodnoty pomocí nástroje Správce tajný klíč v rámci klíče `SMSAccountIdentification` a `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Zadání ID odesílatele nebo původce

**Twilio:**  
Na kartě čísla zkopírujte vaše Twilio **telefonní číslo**. 

**ASPSMS:**  
V nabídce autoru odemknutí odemknutí jeden nebo více autoru nebo zvolte alfanumerické původce (nepodporuje všechny sítě). 

Později jsme uloží tuto hodnotu pomocí nástroje Správce tajný klíč v rámci klíč `SMSAccountFrom`.


### <a name="provide-credentials-for-the-sms-service"></a>Zadejte přihlašovací údaje pro službu SMS

Použijeme [možnosti vzor](xref:fundamentals/configuration/options) pro přístup k účtu a klíč nastavení uživatele. 

   * Vytvořte třídu načíst zabezpečený klíč serveru SMS. Tato ukázka `SMSoptions` je v vytvořit třídu *Services/SMSoptions.cs* souboru.

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Nastavte `SMSAccountIdentification`, `SMSAccountPassword` a `SMSAccountFrom` s [nástroj tajný klíč správce](xref:security/app-secrets). Příklad:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* Přidejte balíček NuGet pro poskytovatele služby SMS. Z balíček správce konzoly (pomocí PMC) spusťte:

**Twilio:**  
`Install-Package Twilio`

**ASPSMS:**  
`Install-Package ASPSMS`


* Přidejte kód v *Services/MessageServices.cs* souboru SMS. Použijte Twilio nebo ASPSMS části:


**Twilio:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Konfigurace spuštění používat `SMSoptions`

Přidat `SMSoptions` ke kontejneru služby v `ConfigureServices` metoda v *Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Povolit dvoufaktorové ověřování

Otevřete *Views/Manage/Index.cshtml* souboru nástroje Razor zobrazení a odebrání komentář znaků (takže žádný kód je odkomentovaný).

## <a name="log-in-with-two-factor-authentication"></a>Přihlaste se pomocí dvoufaktorové ověřování

* Spusťte aplikaci a zaregistrovat nového uživatele

![Webovou aplikaci zaregistrovat zobrazení otevřít v Microsoft Edge](2fa/_static/login2fa1.png)

* Klepněte na jméno uživatele, který aktivuje `Index` metodu akce v kontroleru spravovat. Potom klepněte na telefonní číslo **přidat** odkaz.

![Správa zobrazení](2fa/_static/login2fa2.png)

* Telefonní číslo, který bude přijímat ověřovací kód a klepněte na Přidat **Odeslat ověřovací kód**.

![Přidat stránku telefonní číslo](2fa/_static/login2fa3.png)

* Zobrazí se zpráva SMS s ověřovacím kódem. Zadejte ji a klepněte na **odeslání**

![Zkontrolujte telefonní číslo stránky](2fa/_static/login2fa4.png)

Pokud neobdržíte textovou zprávu, najdete v protokolu stránky twilio.

* Správa zobrazení ukazuje, že váš telefon byl úspěšně přidán.

![Správa zobrazení](2fa/_static/login2fa5.png)

* Klepněte na **povolit** povolit dvoufaktorové ověřování.

![Správa zobrazení](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Test dvoufaktorové ověřování

* Odhlaste se.

* Přihlásit se.

* Uživatelský účet má povoleno dvoufaktorové ověřování, je nutné zadat druhý faktor ověřování. V tomto kurzu jste povolili ověření telefonu. Integrované šablony lze také nastavit e-mail. jako druhý faktor. Můžete nastavit další druhý faktory ověřování, jako je kódy QR. Klepněte na **odeslání**.

![Odeslat ověřovací kód zobrazení](2fa/_static/login2fa7.png)

* Zadejte kód, který se zobrazí ve zprávě SMS.

* Kliknutím na **mějte na paměti, tento prohlížeč** zaškrtávací políčko je vyloučit z nutnosti použít 2FA přihlášení při použití stejné zařízení a prohlížeče. Povolení 2FA a kliknete na **mějte na paměti, tento prohlížeč** vám poskytne silné 2FA ochrany z uživatelé se zlými úmysly pokusu o přístup k účtu, tak dlouho, dokud nemají přístup k zařízení. Můžete provést na jakémkoli privátní zařízení, kterou používáte pravidelně. Nastavením **mějte na paměti, tento prohlížeč**, získáte další zabezpečení 2FA ze zařízení, nepoužívejte pravidelně a získat užitečný na nebudou muset projít 2FA na vlastní zařízení.

![Ověření zobrazení](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Uzamčení účtu pro ochranu před útoky hrubou silou

Uzamčení účtu se doporučuje s 2FA. Jakmile se uživatel přihlásí pomocí místního účtu nebo sociální účtu, je uložený v 2FA každý neúspěšný pokus. Pokud je dostupný maximální neúspěšných pokusů o přístup, je uživatel uzamčen (výchozí: 5 minut uzamčení po 5 neúspěšných pokusů o přístup). Úspěšné ověření resetuje počet pokusů o neúspěšných přístupů a obnoví hodiny. Maximální počet neúspěšných pokusů o přístup a doby uzamčení lze nastavit s [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) a [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). Následující konfiguruje uzamčení účtu po dobu 10 minut po 10 neúspěšných pokusů o přístup:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Potvrďte, že [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) nastaví `lockoutOnFailure` k `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
