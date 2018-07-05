---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity | Dokumentace Microsoftu
author: HaoK
description: Tento kurzu se dozvíte, jak nastavit dvojúrovňového ověřování (2FA) pomocí SMS a e-mailu. Tento článek zapsal Rick Anderson ( @RickAndMSFT ), za...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0519ae69b3d2ef129d206a936b199f781fef5bf6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399676"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity
====================
podle [Haovi společnosti ani](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Tento kurzu se dozvíte, jak nastavit dvojúrovňového ověřování (2FA) pomocí SMS a e-mailu.
> 
> Tento článek zapsal Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), ani Haovi společnosti a Suhas Joshi. Ukázka NuGet zapsal primárně ani Haovi společnosti.


Toto téma obsahuje následující:

- [Ukázka Identity sestavení](#build)
- [Nastavení služby SMS pro dvoufaktorového ověřování](#SMS)
- [Povolení dvoufaktorového ověřování](#enable2)
- [Postup při registraci zprostředkovatele dvoufaktorového ověřování](#reg)
- [Sloučit účty sociálních sítí a místní přihlášení](#combine)
- [Uzamčení účtu před útoky hrubou silou](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Ukázka Identity sestavení

V této části použijete Stáhnout ukázku, kterou pak ve spolupráci s NuGet. Začněte tím, že instalaci a používání [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalace sady Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší.

> [!NOTE]
> Upozornění: Je nutné nainstalovat Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) k dokončení tohoto kurzu.


1. Vytvořte nový ***prázdný*** webový projekt ASP.NET.
2. V konzole Správce balíčků zadejte následující příkazy:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   V tomto kurzu použijeme [SendGrid](http://sendgrid.com/) k odesílání e-mailu a [Twilio](https://www.twilio.com/) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) pro odesílání textových zpráv sms. `Identity.Samples` Balíček nainstaluje budeme pracovat s kódem.
3. Nastavte [projektu pro použití protokolu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Volitelné*: postupujte podle pokynů v mé [kurzu potvrzení e-mailu](account-confirmation-and-password-recovery-with-aspnet-identity.md) připojení SendGrid a pak spusťte aplikaci a zaregistrování e-mailový účet.
5. * Volitelné: * odebrat je ukázka e-mailu odkaz potvrzovací kód z ukázkového ( `ViewBag.Link` kód v účet kontroleru. Zobrazit `DisplayEmail` a `ForgotPasswordConfirmation` metody akce a zobrazeními razor).
6. <em>Volitelné: * Odebrat `ViewBag.Status` kódu z řadiče spravovat a účet a *Views\Account\VerifyCode.cshtml</em> a <em>Views\Manage\VerifyPhoneNumber.cshtml</em> zobrazení syntaxe razor. Alternativně můžete zachovat `ViewBag.Status` displej tak, aby test, jak tato aplikace funguje místně bez nutnosti připojení a odeslat e-mailu a zpráv SMS.

> [!NOTE]
> Upozornění: Pokud změníte některá nastavení zabezpečení v této ukázce, výroby aplikací bude chtít projít auditu zabezpečení, který explicitně vám sdělí, provedené změny.


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Nastavení služby SMS pro dvoufaktorového ověřování

Tento kurz obsahuje pokyny k používání Twilio nebo ASPSMS ale můžete použít jakýkoli jiný poskytovatel serveru SMS.

1. **Vytvoření uživatelského účtu s poskytovatelem služby SMS**  
  
   Vytvoření [Twilio](https://www.twilio.com/try-twilio) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) účtu.
2. **Instalace dalších balíčků nebo přidávání odkazů na služby**  
  
   Twilio:  
   V konzole Správce balíčků zadejte následující příkaz:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Následující odkaz na službu musí být přidán:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Adresa:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Obor názvů:  
    `ASPSMSX2`
3. **Zjištění přihlašovací údaje uživatele poskytovatele serveru SMS**  
  
   Twilio:  
   Z **řídicí panel** kartu svého účtu Twilio, kopie **SID účtu** a **ověřovací token**.  
  
   ASPSMS:  
   V nastavení účtu, přejděte na **vlastnosti userkey jedná** a zkopírujte ho spolu s místním definované **heslo**.  
  
   Uložíme tyto hodnoty se později v proměnné `SMSAccountIdentification` a `SMSAccountPassword` .
4. **Určení SenderID / původce**  
  
   Twilio:  
   Z **čísla** kartu, zkopírujte své telefonní číslo Twilio.  
  
   ASPSMS:  
   V rámci **odemknout autoru** nabídce odemknout jeden nebo více autoru nebo zvolte příkazce alfanumerické znaky (není podporováno ve všech sítích).  
  
   Jsme později uloží tuto hodnotu do proměnné `SMSAccountFrom` .
5. **Přenos přihlašovací údaje poskytovatele služby SMS do aplikace**  
  
   Přihlašovací údaje a zpřístupníte odesílatele telefonní číslo do aplikace:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Zabezpečení – nikdy ukládání citlivých dat ve zdrojovém kódu. Účet a přihlašovací údaje se přidají do výše uvedený pro zjednodušení ukázkový kód. Zobrazit Jon Atten [ASP.NET MVC: zachovat privátní nastavení vzdálené správy zdrojového kódu](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Provádění přenos dat do poskytovatele služby SMS**  
  
   Konfigurace `SmsService` třídy v *aplikace\_Start\IdentityConfig.cs* souboru.  
  
   V závislosti na poskytovateli serveru SMS používá aktivaci buď **Twilio** nebo **ASPSMS** části: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Spusťte aplikaci a přihlaste se pomocí účtu, který jste dříve zaregistrovali.
8. Klikněte na své ID uživatele, která se aktivuje `Index` metodu akce v `Manage` kontroleru.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Klikněte na tlačítko Přidat.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. Během několika sekund obdržíte textovou zprávu s ověřovacím kódem. Zadejte ji a stiskněte klávesu **odeslat**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. Správa zobrazení ukazuje že vaše telefonní číslo bylo přidáno.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Prozkoumejte kód

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index` Metodu akce v `Manage` kontroleru nastaví ve zprávě o stavu na základě předchozí akce a poskytuje odkazy na vaše místní heslo změnit nebo přidat místní účet. `Index` Metoda také zobrazí stav nebo váš 2FA telefonní číslo, externí přihlášení, 2FA povolená a nezapomeňte 2FA metoda tohoto prohlížeče (vysvětleno dále). Kliknutím na své ID uživatele (e-mail) v záhlaví není úspěšný zprávu. Kliknutím na **telefonní číslo: odeberte** propojit předá `Message=RemovePhoneSuccess` jako řetězec dotazu.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber` Metoda akce se zobrazí dialogové okno zadat telefonní číslo, které můžou přijímat zprávy SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Kliknutím na **poslat ověřovací kód** tlačítko odešle telefonní číslo do HTTP POST `AddPhoneNumber` metody akce.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync` Metoda generuje token zabezpečení, který bude nastavený ve zprávě SMS. Pokud služba SMS byla nakonfigurovaná, token, který se odešle jako řetězec &quot;je váš bezpečnostní kód &lt;token&gt;&quot;. `SmsService.SendAsync` Metoda je volána asynchronně a potom aplikaci je přesměrován na `VerifyPhoneNumber` metody akce (který se zobrazí následující dialogové okno), ve kterém můžete zadat ověřovací kód.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Zadejte kód a klikněte na Odeslat kód je odeslána do HTTP POST `VerifyPhoneNumber` metody akce.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync` Metoda zkontroluje odeslaných bezpečnostní kód. Pokud kód je správný, telefonní číslo je přidána do `PhoneNumber` pole `AspNetUsers` tabulky. Pokud toto volání je úspěšné, `SignInAsync` volání metody:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent` Parametr nastaví, zda je relace ověřování zachová pro víc požadavků.

Pokud změníte profil zabezpečení, je generována a uložené v nové razítko zabezpečení `SecurityStamp` pole *AspNetUsers* tabulky. Všimněte si, `SecurityStamp` pole se liší od soubor cookie zabezpečení. Soubor cookie zabezpečení není uložený v `AspNetUsers` tabulky (nebo kdekoli jinde v Identity DB). Token souboru cookie zabezpečení je podepsaný držitelem, pomocí [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) a je vytvořen s `UserId, SecurityStamp` a informace doby vypršení platnosti.

Middlewaru souboru cookie. zkontroluje soubor cookie s každým požadavkem. `SecurityStampValidator` Metodu `Startup` třídy volání databáze a zkontroluje razítko zabezpečení pravidelně uvedené s `validateInterval`. Tato situace nastane pouze každých 30 minut (v našem příkladu) nezměníte svůj profil zabezpečení. Chcete-li minimalizovat cest k databázi bylo zvoleno intervalu 30 minut.

`SignInAsync` Metoda musí být volána při jakékoli změně na profil zabezpečení. Když se změní na profil zabezpečení databáze je aktualizace `SecurityStamp` pole a bez volání `SignInAsync` metody by zůstat přihlášen(a) *pouze* až při příští kanálu OWIN narazí na databázi ( `validateInterval`). Můžete ho otestovat tak, že změníte `SignInAsync` metoda výsledky okamžitě a nastavení souboru cookie `validateInterval` vlastnost z 30 minut na 5 sekund:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Výše uvedené změny kódu, můžete změnit svůj profil zabezpečení (například tak, že změníte stav **dvě povolené faktor**) a můžete se odhlásíte během 5 sekund při `SecurityStampValidator.OnValidateIdentity` metoda selže. Odebrání řádku vrácené v `SignInAsync` metoda, nastavit jiný profil zabezpečení změnit a můžete se nebudou protokolovat navýšení kapacity. `SignInAsync` Metoda vytvoří nový soubor cookie zabezpečení.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Povolení dvoufaktorového ověřování

V ukázkové aplikaci musíte použít uživatelské rozhraní pro povolení dvoufaktorového ověřování (2FA). Pokud chcete povolit 2FA, klikněte na své ID uživatele (e-mailový alias) na navigačním panelu.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Klikněte na povolit 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Odhlaste se a potom znovu přihlásit. Pokud jste povolili e-mailu (naleznete v tématu Moje [předchozí kurz o službě](account-confirmation-and-password-recovery-with-aspnet-identity.md)), můžete vybrat, SMS nebo e-mailu pro 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Zobrazí se stránka Ověřte kód, ve kterém můžete zadat kód (z SMS nebo e-mailu).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Kliknutím na **chcete Zapamatovat tento prohlížeč** zaškrtávacího políčka můžete vyloučit před nutností pomocí 2FA Přihlaste se pomocí tohoto počítače a prohlížeče. Povolení 2FA a kliknete na **chcete Zapamatovat tento prohlížeč** vám poskytne silné 2FA ochrany z uživateli se zlými úmysly pokusu o přístup ke svému účtu, za předpokladu, že nemají přístup k vašemu počítači. Můžete to provést na privátní počítač, který pravidelně používáte. Nastavením **chcete Zapamatovat tento prohlížeč**, získáte z počítačů, které nepoužíváte pravidelně zvýšení zabezpečení 2FA, a získáte usnadnění práce na nebudete muset absolvovat 2FA na vlastním počítači. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Postup při registraci zprostředkovatele dvoufaktorového ověřování

Když vytvoříte nový projekt MVC *IdentityConfig.cs* soubor obsahuje následující kód k registraci zprostředkovatele dvoufaktorového ověřování:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Přidat telefonní číslo pro 2FA

`AddPhoneNumber` Metodu akce v `Manage` řadič generuje token zabezpečení a odešle na telefonní číslo, které se mají k dispozici.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Po odeslání tokenu, přesměruje na `VerifyPhoneNumber` metodě akce, ve kterém můžete zadat kód a zaregistrujte 2FA SMS. SMS 2FA se nepoužije, dokud si neověříte telefonní číslo.

## <a name="enabling-2fa"></a>Povolení 2FA

`EnableTFA` Metody akce umožňuje 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Poznámka: `SignInAsync` musí volat, protože povolit 2FA se mění na profil zabezpečení. Při zapnutém 2FA, uživatel bude potřebovat k přihlášení pomocí 2FA pomocí 2FA přístupů registrace (SMS a e-mailu v ukázce).

Můžete přidat další poskytovatelé 2FA, jako je například generátory kódu QR nebo můžete napsat, které vlastníte (viz [pomocí Google Authenticator s ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> 2FA kódy jsou generovány pomocí [podle času jednorázové heslo algoritmus](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) a kódy jsou platné po dobu 6 minut. Pokud budete postupovat víc než šest minut zadejte kód, získáte neplatný kód chybovou zprávu.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Sloučit účty sociálních sítí a místní přihlášení

Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociální účty. V následujícím pořadí &quot; RickAndMSFT@gmail.com &quot; se nejprve vytvoří jako místní přihlášení, ale můžete vytvořit účet jako sociální protokolu v prvním a pak přidat místní přihlašovací údaje.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Klikněte na **spravovat** odkaz. Poznámka: externí 0 (přihlašování přes sociální sítě) spojená s tímto účtem.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Klikněte na odkaz na jiný protokol služby a přijímání požadavků aplikace. Byli sloučeni dva účty, budou moct přihlásit pomocí obou. Můžete chtít uživatelům přidat místní účty v případě jejich sociálních sítí protokolu ve službě ověřování je mimo provoz nebo spíše se ztratili přístup k jejich účtu na sociální síti.

Na následujícím obrázku je Petr sociální přihlášení (které si můžete všimnout **externí přihlášení: 1** zobrazený na stránce).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Kliknutím na **výběru hesla** umožňuje přidat místního protokolu na spojené se stejným účtem.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Uzamčení účtu před útoky hrubou silou

Dokáže chránit účty v aplikaci na slovníkové útoky tím, že uzamčení uživatelů. Následující kód na `ApplicationUserManager Create` metoda nakonfiguruje uzamčení:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Výše uvedený kód povoluje uzamčení k dvojúrovňovému ověřování pouze. Zatímco uzamčení pro přihlášení můžete povolit tak, že změníte `shouldLockout` na hodnotu true `Login` metody kontroleru účet, doporučujeme uzamčení pro přihlášení není povolit, protože účet je náchylný k [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) přihlášení útoky. Ve vzorovém kódu uzamčení zakázána pro účet správce vytvořený `ApplicationDbInitializer Seed` metody:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>By po uživateli, aby ověřený e-mailový účet

Následující kód, musí uživatel má ověřená e-mailový účet před můžou přihlásit:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Jak SignInManager vyhledává 2FA požadavek

Místní přihlášení i sociální přihlášení zkontrolujte, jestli je povolené 2FA. Pokud je povolené 2FA, `SignInManager` přihlašovací metoda vrátí `SignInStatus.RequiresVerification`, a uživatel bude přesměrován na `SendCode` metodě akce, ve kterém bude nutné zadat kód k provedení protokolu v pořadí. Pokud má uživatel RememberMe je nastavena na místní soubor cookie uživatele, `SignInManager` vrátí `SignInStatus.Success` a nebude muset projít 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Následující kód ukazuje `SendCode` metody akce. A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) se vytvoří s všechny metody 2FA povolen pro daného uživatele. [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) je předán [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) pomocné rutiny, která umožňuje uživateli vybrat přístup 2FA (obvykle e-mail nebo SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Jakmile uživatel odešle přístup 2FA `HTTP POST SendCode` volání metody `SignInManager` odešle kód 2FA a uživatel se přesměruje na `VerifyCode` metody akce, ve kterém můžete zadat kód pro dokončení přihlášení.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA uzamčení

I když uzamčení účtu lze nastavit na selhání pokusů o přihlášení hesel, tento přístup umožňuje vaše přihlašovací údaje náchylné k [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) uzamčení. Doporučujeme že použít uzamčení účtu jenom pomocí 2FA. Když `ApplicationUserManager` je vytvořen, ukázkový kód nastaví uzamčení 2FA a `MaxFailedAccessAttemptsBeforeLockout` na pět. Jakmile se uživatel přihlásí (prostřednictvím místního účtu nebo účtu na sociální síti), ukládají jednotlivé neúspěšné pokusy o na 2FA a při dosažení maximální počet pokusů o je uživatel uzamčen po dobu pěti minut (můžete nastavit zámek čas `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Další prostředky

- [ASP.NET Identity – doporučené prostředky](../getting-started/aspnet-identity-recommended-resources.md) úplný seznam identit blogy, videa, kurzy a velké tak odkazy.
- [Aplikace MVC 5 s Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) také ukazuje, jak přidat informace o profilu do tabulky uživatelů.
- [ASP.NET MVC a Identity 2.0: Základy práce s](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) podle Atten Jan.
- [Potvrzení účtu a obnovení hesla s ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Úvod do ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Oznamujeme vydání verze RTM technologie ASP.NET identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) napsal Pranav Rastogi.
- [Identita ASP.NET 2.0: Nastavení ověření účtu a povolení dvoufaktorového](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) podle Atten Jan.
