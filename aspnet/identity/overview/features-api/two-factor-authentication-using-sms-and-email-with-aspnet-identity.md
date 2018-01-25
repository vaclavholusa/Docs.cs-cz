---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: "Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity | Microsoft Docs"
author: HaoK
description: "Tento kurz vám ukáže, jak nastavit dvoufaktorové ověřování (2FA) pomocí serveru SMS a e-mailu. Tento článek napsal Rick Anderson ( @RickAndMSFT ), za..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0f9ff7cf74048a008b150da1e843ff15333269ab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity
====================
podle [Haovi společnosti ani](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Tento kurz vám ukáže, jak nastavit dvoufaktorové ověřování (2FA) pomocí serveru SMS a e-mailu.
> 
> Tento článek napsal Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), ani Haovi společnosti a Suhas Joshi. Ukázka NuGet napsal především ani Haovi společnosti.


Toto téma obsahuje následující:

- [Vytváření ukázka Identity](#build)
- [Nastavení služby SMS pro dvoufaktorové ověřování](#SMS)
- [Povolit dvoufaktorové ověřování](#enable2)
- [Postup registrace zprostředkovatele dvoufaktorového ověřování](#reg)
- [Kombinování sociálních a místní přihlášení účtů](#combine)
- [Uzamčení účtu před útoky hrubou silou](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Vytváření ukázka Identity

V této části použijete NuGet Stáhnout ukázku, kterou budeme pracovat s. Začněte tím, že instalace a spuštění [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalaci sady Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší.

> [!NOTE]
> Upozornění: Je nutné nainstalovat Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) k dokončení tohoto kurzu.


1. Vytvořte novou ***prázdný*** webový projekt ASP.NET.
2. V konzole Správce balíčků, zadejte následující příkazy:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
 V tomto kurzu použijeme [sendgrid vám umožňuje](http://sendgrid.com/) k odesílání e-mailu a [Twilio](https://www.twilio.com/) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) pro odesílání textových zpráv sms. `Identity.Samples` Balíček nainstaluje kód Pracujeme s.
3. Nastavte [projektu pro použití protokolu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Volitelné*: postupujte podle pokynů v mé [kurzu potvrzení e-mailu](account-confirmation-and-password-recovery-with-aspnet-identity.md) spojit sendgrid vám umožňuje a pak spusťte aplikaci a zaregistrovat e-mailový účet.
5. * Volitelné: * kód Ukázkový odkaz e-mailu potvrzení odebrání vzorku ( `ViewBag.Link` kódu v kontroleru účtu. Najdete v článku `DisplayEmail` a `ForgotPasswordConfirmation` metody akce a zobrazení syntaxe razor).
6. * Volitelné: * Odebrat `ViewBag.Status` kód z řadičů spravovat a účet a *Views\Account\VerifyCode.cshtml* a *Views\Manage\VerifyPhoneNumber.cshtml* zobrazení syntaxe razor. Alternativně můžete ponechat `ViewBag.Status` displej tak, aby testovat, jak se tato aplikace funguje místně bez nutnosti spojit a odeslat e-mailu a zpráv SMS.

> [!NOTE]
> Upozornění: Pokud změníte některá nastavení zabezpečení v této ukázce, výroby, které aplikace bude muset podstoupit auditu zabezpečení, která explicitně volá se změny provedené.


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Nastavení služby SMS pro dvoufaktorové ověřování

Tento kurz obsahuje pokyny pro použití Twilio nebo ASPSMS ale můžete použít další poskytovatele serveru SMS.

1. **Vytvoření uživatelského účtu s poskytovatelem služby SMS**  
  
 Vytvoření [Twilio](https://www.twilio.com/try-twilio) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) účtu.
2. **Instalace dalších balíčků nebo přidání odkazů na služby**  
  
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
 Z **řídicí panel** kartě svého účtu Twilio kopie **SID účtu** a **Auth token**.  
  
 ASPSMS:  
 V nastavení svého účtu, přejděte na **Userkey** a zkopírujte jej spolu s samoobslužné definované **heslo**.  
  
 Tyto hodnoty jsme později budou ukládat do proměnné `SMSAccountIdentification` a `SMSAccountPassword` .
4. **Zadání ID odesílatele nebo původce**  
  
 Twilio:  
 Z **čísla** kartě, zkopírujte své telefonní číslo Twilio.  
  
 ASPSMS:  
 V rámci **odemknutí autoru** nabídce odemknutí jeden nebo více autoru nebo vyberte alfanumerické původce (nepodporuje všechny sítě).  
  
 Tato hodnota jsme později se uloží v proměnné `SMSAccountFrom` .
5. **Přenos přihlašovací údaje poskytovatele služby SMS do aplikace**  
  
 Zkontrolujte přihlašovací údaje a telefonní číslo odesílatele, k dispozici pro aplikaci:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Zabezpečení – nikdy úložiště citlivá data ve zdrojovém kódu. Účet a přihlašovací údaje se přidají do výše pro zjednodušení v ukázkovém kódu. V tématu Jon Atten [rozhraní ASP.NET MVC: zachovat mimo privátní nastavení správy zdrojového kódu](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementace přenos dat do poskytovatele služby SMS**  
  
 Konfigurace `SmsService` třídy v *aplikace\_Start\IdentityConfig.cs* souboru.  
  
 V závislosti na použité poskytovatele služby SMS aktivovat buď **Twilio** nebo **ASPSMS** části: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Spusťte aplikaci a přihlaste se pomocí účtu, který jste dříve registrován.
8. Klikněte na vaše ID uživatele, který aktivuje `Index` metodu akce v `Manage` řadiče.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Klikněte na tlačítko Přidat.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. Během pár sekund obdržíte textovou zprávu s ověřovacím kódem. Zadejte ho a stiskněte klávesu **odeslání**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. Správa zobrazení ukazuje, že vaše telefonní číslo bylo přidáno.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Zkontrolujte kód

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index` Metodu akce v `Manage` řadiče nastaví stavové zprávy založené na předchozí akce a poskytuje odkazy na místní heslo změnit nebo přidat místní účet. `Index` Metoda také zobrazuje stav nebo vaše 2FA telefonní číslo, externích přihlášení, 2FA povolena a nezapomeňte 2FA metodu pro tento prohlížeč (vysvětlení později). Kliknutím na vaše ID uživatele (e-mailu) v záhlaví neprojde zprávu. Kliknutím na **telefonní číslo: odeberte** odkaz předává `Message=RemovePhoneSuccess` jako řetězec dotazu.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber` Metoda akce zobrazí dialogové okno zadat telefonní číslo, které může přijímat zprávy SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Kliknutím na **Odeslat ověřovací kód** tlačítko účtuje telefonní číslo na HTTP POST `AddPhoneNumber` metody akce.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync` Metoda generuje tokeny zabezpečení, které budou nastaveny ve zprávě SMS. Pokud je nakonfigurovaná služba SMS, token se budou odesílat jako řetězec &quot;váš kód zabezpečení: &lt;tokenu&gt;&quot;. `SmsService.SendAsync` Metodu nazývá asynchronně a potom aplikaci je přesměrován na `VerifyPhoneNumber` metoda akce (který se zobrazí následující dialogové okno), kde můžete zadat ověřovací kód.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Poté zadejte kód a klikněte na odeslání, kód je odeslána do HTTP POST `VerifyPhoneNumber` metody akce.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync` Metoda zkontroluje odeslaných bezpečnostní kód. Pokud kód je správná, telefonní číslo je přidat do `PhoneNumber` pole z `AspNetUsers` tabulky. Pokud toto volání úspěšné, `SignInAsync` metoda je volána:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent` Parametr nastaví, zda je relace ověřování zachová pro víc požadavků.

Když změníte profil zabezpečení, nové razítko zabezpečení se generuje a uloží do `SecurityStamp` pole z *AspNetUsers* tabulky. Poznámka: `SecurityStamp` pole se liší od zabezpečení souboru cookie. Soubor cookie zabezpečení nejsou uloženy v `AspNetUsers` tabulky (nebo kdekoli jinde v databázi Identity). Token zabezpečení souboru cookie je podepsaný pomocí [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) a pomocí `UserId, SecurityStamp` a informace o čas vypršení platnosti.

Middlewaru souboru cookie zkontroluje souborů cookie u každého požadavku. `SecurityStampValidator` Metoda v `Startup` třída přístupů do databáze a zkontroluje razítko zabezpečení pravidelně jako zadaný `validateInterval`. Tato situace nastane pouze každých 30 minut (v našem ukázce) není-li změnit váš profil zabezpečení. Chcete-li minimalizovat služebních cest do databáze byla zvolena intervalu 30 minut.

`SignInAsync` Metoda musí být volána při jakékoli změně profil zabezpečení. Při změně profil zabezpečení databáze je aktualizace `SecurityStamp` pole a bez volání `SignInAsync` metoda by zůstat přihlášení *pouze* do příštího kanálu OWIN přístupů do databáze ( `validateInterval`). Toto můžete otestovat změnou `SignInAsync` metoda vrátí okamžitě a nastavení souboru cookie `validateInterval` vlastnost z 30 minut na 5 sekund:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Výše uvedený kód změn, můžete změnit váš profil zabezpečení (například změnou stavu **dvě povolené Multi-Factor**) a můžete se odhlásit, v 5 sekund při `SecurityStampValidator.OnValidateIdentity` metoda selže. Odebrat návratový řádek v `SignInAsync` metoda, nastavit jiný profil zabezpečení změnit a jste se nebudou protokolovat. `SignInAsync` Metoda generuje nový soubor cookie zabezpečení.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Povolit dvoufaktorové ověřování

V ukázkové aplikace budete muset povolit dvoufaktorové ověřování (2FA) pomocí uživatelského rozhraní. Chcete-li povolit 2FA, klikněte na vaše ID uživatele (e-mailový alias) na navigačním panelu.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Klikněte na povolit 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Odhlaste se a potom znovu přihlásit. Pokud jste povolili e-mailu (najdete v části Moje [předchozí kurzu](account-confirmation-and-password-recovery-with-aspnet-identity.md)), můžete vybrat SMS nebo e-mailu pro 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Zobrazí se stránka ověření kódu, kde můžete zadat kód (z SMS nebo e-mailu).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Kliknutím na **mějte na paměti, tento prohlížeč** zaškrtávací políčko je vyloučit z nutnosti použít 2FA přihlášení pomocí tohoto počítače a prohlížeče. Povolení 2FA a kliknutím na **mějte na paměti, tento prohlížeč** vám poskytne silné 2FA ochrany z uživatelé se zlými úmysly pokusu o přístup k účtu, tak dlouho, dokud nemají přístup k vašemu počítači. Můžete provést z jakéhokoli privátní počítače, kterou používáte pravidelně. Nastavením **mějte na paměti, tento prohlížeč**, můžete získat z počítače, které nepoužívají pravidelně zvýšené zabezpečení 2FA a získat užitečný na nebudou muset projít 2FA na vlastní počítače. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Postup registrace zprostředkovatele dvoufaktorového ověřování

Když vytvoříte nový projekt MVC, *IdentityConfig.cs* soubor obsahuje následující kód k registraci zprostředkovatele dvoufaktorového ověřování:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Přidat telefonní číslo pro 2FA

`AddPhoneNumber` Metodu akce v `Manage` zadané jeho telefonní číslo, které se odesílají a řadič generuje token zabezpečení.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Po odeslání token, je přesměrován na `VerifyPhoneNumber` metody akce, kde můžete zadat kód k registraci serveru SMS pro 2FA. SMS 2FA se nepoužije, dokud si neověříte telefonní číslo.

## <a name="enabling-2fa"></a>Povolení 2FA

`EnableTFA` Metoda akce umožňuje 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Poznámka: `SignInAsync` musí volat, protože je povolit 2FA ke změně profil zabezpečení. Pokud je povoleno 2FA, uživatel bude muset použít 2FA k přihlášení přístupy 2FA registrace (SMS a e-mailu v ukázce).

Se dají přidávat další zprostředkovatelé 2FA například generátory kódu QR nebo můžete napsat vlastníte (viz [pomocí Google Authenticator s ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Kódy 2FA jsou generovány pomocí [založené na čase jednorázové heslo algoritmus](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) a kódy jsou platné po dobu šesti minut. Pokud pořídíte víc než šest minut zadejte kód, získáte neplatný kód chybovou zprávu.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Kombinování sociálních a místní přihlášení účtů

Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociálních účty. V tomto pořadí &quot; RickAndMSFT@gmail.com &quot; je poprvé vytvořen jako místní přihlášení, ale můžete vytvořit účet jako sociálních protokolu v prvním a pak přidejte místní přihlášení.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Klikněte na **spravovat** odkaz. Poznámka: externí 0 (sociální přihlášení) spojené s tímto účtem.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Klikněte na odkaz na jiný protokol ve službě service a přijímat žádosti o aplikaci. Dva účty jsou spojena, nebudete moci přihlásit pomocí buď účtu. Můžete chtít uživatelům v případě, že jejich sociální přihlášení ověřovací služby, jsou vypnuty nebo jejich více pravděpodobně ztratili přístup k účtu mají sociálních přidat místní účty.

Na následujícím obrázku je tní sociálních přihlášení (což je vidět na **externích přihlášení: 1** zobrazený na stránce).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Kliknutím na **vyberte heslo** umožňuje přidat místního protokolu na související se stejným účtem.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Uzamčení účtu před útoky hrubou silou

Účty ve vaší aplikaci z slovníkovým útokům můžete chránit povolením uzamčení uživatele. Následující kód na `ApplicationUserManager Create` metoda nakonfiguruje uzamčení:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Výše uvedený kód umožňuje uzamčení pro jenom dvoufaktorové ověřování. Když povolíte uzamčení pro přihlášení změnou `shouldLockout` na hodnotu true v `Login` metoda řadičem účet, doporučujeme uzamčení pro přihlášení není povolit, protože účet je ohrožena útoky založenými na [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) přihlášení útoky. V ukázkovém kódu je zakázána uzamčení pro účet správce vytvořený v `ApplicationDbInitializer Seed` metoda:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>A uživatel tak, aby měl ověřené e-mailový účet

Následující kód, musí uživatel tak, aby měl ověřené e-mailový účet, než se můžete přihlásit:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Jak SignInManager vyhledává 2FA požadavek

Jak místní přihlášení a sociálních protokolu v zkontrolujte, zda je povoleno 2FA. Pokud je povoleno 2FA, `SignInManager` přihlášení metoda vrátí `SignInStatus.RequiresVerification`, a uživatel bude přesměrován na `SendCode` metody akce, které se mají zadat kód pro dokončení protokol v pořadí. Pokud má uživatel obdobou je nastavený na místní soubor cookie uživatele, `SignInManager` vrátí `SignInStatus.Success` a nebude muset projít 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Následující kód ukazuje `SendCode` metody akce. A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) je vytvořena s všechny metody 2FA pro daného uživatele povoleno. [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) je předán [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) pomocné rutiny, která umožňuje uživateli vybrat metodu 2FA (obvykle e-mailu a SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Jakmile uživatel odešle 2FA přístup, `HTTP POST SendCode` akce metoda je volána, `SignInManager` zasílá kód 2FA a uživatel se přesměruje na `VerifyCode` metody akce, kde můžete zadat kód pro dokončení přihlášení.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA uzamčení

I když selhání pokusů o přihlášení hesel můžete nastavit uzamčení účtu, tento přístup umožňuje vaše přihlášení náchylné k [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) uzamčení. Doporučujeme, abyste že je použít pouze s 2FA uzamčení účtu. Když `ApplicationUserManager` je vytvořen, ukázkový kód nastaví 2FA uzamčení a `MaxFailedAccessAttemptsBeforeLockout` na pět. Jakmile se uživatel přihlásí (prostřednictvím místního účtu nebo sociální účtu), je uložen v 2FA každý neúspěšný pokus a pokud bude dosažen maximální počet pokusů, uživatel je uzamčen pět minut (můžete nastavit uzamčení časem `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Další prostředky

- [ASP.NET Identity doporučené prostředky](../getting-started/aspnet-identity-recommended-resources.md) úplný seznam Identity blogy, videa, kurzy a dobře tak odkazy.
- [Aplikace MVC 5 se službou Facebook, Twitter, LinkedIn a přihlašování Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) také ukazuje, jak přidat informace o profilu do tabulky uživatelů.
- [ASP.NET MVC a Identity 2.0: Principy Základy](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) podle Atten Jan.
- [Potvrzení účtu a heslo pro obnovení s ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Úvod do ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Uvedení RTM technologie ASP.NET identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) podle Pranav Rastogi.
- [Identitu ASP.NET 2.0: Nastavení ověření účtu a dvoufaktorové ověřování](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) podle Atten Jan.
