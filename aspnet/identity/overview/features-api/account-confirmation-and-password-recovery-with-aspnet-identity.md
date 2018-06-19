---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Účet potvrzení a obnovení hesla s ASP.NET Identity (C#) | Microsoft Docs
author: HaoK
description: Před provedením tohoto kurzu, které by se měla dokončit nejprve vytvořte zabezpečení webové aplikace ASP.NET MVC 5 s přihlášení resetovat heslo a potvrzení e-mailu. V tomto kurzu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0167388cf6b488b72ca36f583a7794690dbf9900
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876030"
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Potvrzení účtu a heslo pro obnovení s ASP.NET Identity (C#)
====================
podle [Haovi společnosti ani](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Před provedením tohoto kurzu musíte nejdřív provést [vytvořit zabezpečený webovou aplikaci ASP.NET MVC 5 s protokolu v e-mailu potvrzení a heslo resetovat](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Tento kurz obsahuje další podrobnosti a vám ukáže, jak nastavit e-mailu pro místní účet potvrzení a dovolit uživatelům resetovat jejich zapomenuté heslo v identitě ASP.NET Identity. Tento článek napsal Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), ani Haovi společnosti a Suhas Joshi. Ukázka NuGet napsal především ani Haovi společnosti.


Místní uživatelský účet vyžaduje, aby uživatel vytvořit heslo pro účet a toto heslo (bezpečně) uložen ve webové aplikaci. ASP.NET Identity také podporuje sociálních účty, které nevyžadují uživatelům vytvořit heslo aplikace. [Sociální účty](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) používat třetích stran (například Google, Twitter, Facebook nebo Microsoft) k ověřování uživatelů. Toto téma obsahuje následující:

- [Vytvoření aplikace ASP.NET MVC](#createMvc) a prozkoumat funkce ASP.NET Identity.
- [Vytváření ukázka Identity](#build)
- [Nastavení e-mailu potvrzení](#email)

Noví uživatelé zaregistrovat e-mailový alias, který vytvoří místní účet.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Kliknutím na tlačítko Zaregistrovat se odešle e-mail s potvrzením, který obsahuje token ověření pro e-mailové adresy.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

Uživatel je odeslán e-mail s potvrzovacím tokenem ke svému účtu.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Kliknutím na odkaz potvrdí účet.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Obnovení nebo resetování hesla

Místní uživatelé zapomenou své heslo může mít token zabezpečení odeslat e-mailový účet, tím jim umožníte obnovit své heslo.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Uživatel brzy dostane e-mail s odkazem, což jim umožní obnovit své heslo.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Kliknutím na odkaz přejdou na stránku resetování.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Kliknutím **resetovat** tlačítko bude potvrďte heslo bylo resetováno.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Vytvoření aplikace technologie ASP.NET

Začněte tím, že instalace a spuštění [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalaci sady Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší.

> [!NOTE]
> Upozornění: Je nutné nainstalovat Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) k dokončení tohoto kurzu.


1. Vytvořte nový projekt ASP.NET Web a vyberte šablonu MVC. Webové formuláře také podporuje ASP.NET Identity, takže můžete sledovat v aplikaci web forms podobným způsobem.
2. Ponechte výchozí ověřování jako **jednotlivé uživatelské účty**.
3. Spuštění aplikace, klikněte na tlačítko **zaregistrovat** propojit a zaregistrovat uživatele. V tomto okamžiku je pouze ověření na e-mailu [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atribut.
4. V Průzkumníku serveru, přejděte na **Data Connections\DefaultConnection\Tables\AspNetUsers**, klikněte pravým tlačítkem a vyberte **otevřete definici tabulky**.

    Na následujícím obrázku `AspNetUsers` schématu:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Klikněte pravým tlačítkem na **AspNetUsers** tabulky a vyberte **zobrazit Data tabulky**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   V tomto okamžiku není potvrzena e-mailu.

Je výchozí úložiště dat pro ASP.NET Identity Entity Framework, ale můžete nakonfigurovat ji k používání jiným úložištím dat a přidat další pole. V tématu [další prostředky](#addRes) na konci tohoto kurzu.

[Třídy pro spuštění OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) je volána, když se aplikace spustí a vyvolá `ConfigureAuth` metoda v *aplikace\_Start\Startup.Auth.cs*, která nakonfiguruje kanál OWIN a inicializuje ASP.NET Identity. Zkontrolujte `ConfigureAuth` metoda. Každý `CreatePerOwinContext` volání zaregistruje zpětné volání (uložené v `OwinContext`), bude vyvolána jedenkrát za požadavek na vytvoření instance zadaného typu. Můžete nastavit bod přerušení v konstruktoru a `Create` metoda každého typu (`ApplicationDbContext, ApplicationUserManager`) a ověřte, se nazývají na každý požadavek. Instanci `ApplicationDbContext` a `ApplicationUserManager` je uložená v kontextu OWIN, který je přístupný v celé aplikaci. ASP.NET Identity zachytí do kanálu OWIN prostřednictvím middlewaru souboru cookie. Další informace najdete v tématu [za správu životního cyklu požadavek pro objekt UserManager třídu v identitě ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Když změníte profil zabezpečení, nové razítko zabezpečení se generuje a uloží do `SecurityStamp` pole z *AspNetUsers* tabulky. Poznámka: `SecurityStamp` pole se liší od zabezpečení souboru cookie. Soubor cookie zabezpečení nejsou uloženy v `AspNetUsers` tabulky (nebo kdekoli jinde v databázi Identity). Token zabezpečení souboru cookie je podepsaný pomocí [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) a pomocí `UserId, SecurityStamp` a informace o čas vypršení platnosti.

Middlewaru souboru cookie zkontroluje souborů cookie u každého požadavku. `SecurityStampValidator` Metoda v `Startup` třída přístupů do databáze a zkontroluje razítko zabezpečení pravidelně jako zadaný `validateInterval`. Tato situace nastane pouze každých 30 minut (v našem ukázce) není-li změnit váš profil zabezpečení. Chcete-li minimalizovat služebních cest do databáze byla zvolena intervalu 30 minut. Najdete v části Moje [dvoufaktorové ověřování kurzu](index.md) další podrobnosti.

Za komentáře v kódu `UseCookieAuthentication` metoda podporuje ověřování souborů cookie. `SecurityStamp` Pole a přidružených kódu poskytuje další vrstvu zabezpečení do vaší aplikace, pokud změníte heslo, budete přihlášeni mimo prohlížeče jste přihlášení. `SecurityStampValidator.OnValidateIdentity` Metoda umožní aplikaci ověřit token zabezpečení při přihlášení uživatele, který se používá, pokud změníte heslo nebo použít externí přihlášení. To je nutné zajistit, že jsou zneplatněny všechny tokeny (soubory cookie) vygenerovat pomocí staré heslo. V projektu vzorku, pokud změníte heslo uživatele pak nový token je generován pro uživatele, všechny předchozí tokeny, jež jsou zneplatněny a `SecurityStamp` pole se aktualizuje.

Systém identit umožňují konfigurovat aplikaci tak při změně uživatelé profil zabezpečení (například když uživatel změní své heslo nebo změny přidružené přihlášení (například kvůli Facebook, Google, účet Microsoft, atd.), je uživatel přihlášen mimo všechny instance prohlížeče. Například obrázek níže znázorňuje [jeden odhlášení ukázka](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) aplikaci, která umožňuje uživatelům Odhlásit se ze všech instancí prohlížeče (v tomto případě aplikace Internet Explorer, Firefox a Chrome) kliknutím na tlačítko jeden. Ukázku můžete taky umožňuje pouze odhlášení od konkrétní prohlížeče instance.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

[Jeden odhlášení ukázka](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) aplikace ukazuje, jak ASP.NET Identity umožňuje znovu vygenerovat token zabezpečení. To je nutné zajistit, že jsou zneplatněny všechny tokeny (soubory cookie) vygenerovat pomocí staré heslo. Tato funkce poskytuje další vrstvu zabezpečení, které vaše aplikace; Pokud změníte heslo, můžete se odhlásit, kde jste přihlášení do této aplikace.

*Aplikace\_Start\IdentityConfig.cs* soubor obsahuje `ApplicationUserManager`, `EmailService` a `SmsService` třídy. `EmailService` a `SmsService` třídy každý implementují `IIdentityMessageService` rozhraní, takže máte běžné metody v každé třída ke konfiguraci e-mailu a SMS. I když tento kurz ukazuje jenom postup přidání e-mailové oznámení prostřednictvím [sendgrid vám umožňuje](http://sendgrid.com/), můžete odeslat e-mailu pomocí protokolu SMTP a další mechanismy.

`Startup` Třída také obsahuje desku kotle přidat sociálních přihlášení (Facebook, Twitter, atd.), najdete v části Moje kurzu [aplikace MVC 5 se službou Facebook, Twitter, LinkedIn a přihlašování Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Další informace.

Zkontrolujte `ApplicationUserManager` třídy, která obsahuje informace o identitě uživatele a nakonfiguruje následující funkce:

- Požadavky síly hesla.
- Uživatel uzamčení (pokusů a čas).
- Dvoufaktorové ověřování (2FA). V jiné kurzu budete zahrnovat 2FA a SMS.
- Zapojování e-mailu a služby SMS. (I zaměříme SMS v jiné kurzu).

`ApplicationUserManager` Třída odvozená z obecná `UserManager<ApplicationUser>` třídy. `ApplicationUser` odvozená z [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` odvozená z obecná `IdentityUser` třídy:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Výše uvedené vlastnosti se shodovat s vlastností v `AspNetUsers` tabulce uvedené výše.

Obecné argumenty na `IUser` umožňují odvození třídy pomocí různých typů pro primární klíč. Najdete v článku [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) vzorku, který ukazuje, jak změnit primární klíč z řetězce na int nebo identifikátor GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) je definován v *Models\IdentityModels.cs* jako:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Generuje zvýrazněný výše [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity a ověřování souborů Cookie OWIN jsou založené na deklaracích, proto rozhraní vyžaduje aplikaci generovat `ClaimsIdentity` pro uživatele. `ClaimsIdentity` obsahuje informace o všech deklarací identity pro uživatele, jako je například uživatelské jméno, stáří a jaké role uživatel patří. V této fázi můžete také přidat další deklarace pro uživatele.

OWIN `AuthenticationManager.SignIn` metoda předá `ClaimsIdentity` a přihlášení uživatele:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[Aplikace MVC 5 se službou Facebook, Twitter, LinkedIn a přihlašování Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) ukazuje, jak můžete přidat další vlastnosti `ApplicationUser` třídy.

## <a name="email-confirmation"></a>Potvrzení e-mailu

Je vhodné pro potvrzení nového uživatele zaregistrovat se k ověření, že nejsou zosobnění někdo jiný e-mailu (to znamená, že nebyly zaregistrována někoho jiného e-mailu). Předpokládejme, že jste měli diskusní fórum, chcete zabránit `"bob@example.com"` registraci jako `"joe@contoso.com"`. Bez potvrzení e-mailu `"joe@contoso.com"` může získat nežádoucí e-mailu z vaší aplikace. Předpokládejme, že Bob, ať už náhodně registrován jako `"bib@example.com"` a kdyby si všimli, si nebude moct používat heslo obnovit, protože aplikace nemá správnou e-mailovou. Potvrzení e-mailu poskytuje jen omezenou ochranu z robotů a neposkytuje ochranu proti určené odesílatelům nevyžádané pošty, mají mnoho aliasy pracovní e-mailu, které můžete použít k registraci. V následující ukázce uživatel nebude moct změnit své heslo, dokud jejich účet potvrzen (jimi kliknutím na odkaz potvrzení přijetí na e-mailový účet se zaregistrována.) Tento pracovní postup můžete použít k ostatním scénářům, třeba odesílání odkaz k potvrzení a resetovat heslo v nové účty, které jsou vytvořené správcem odesílání e-mailu uživatel při změnily svůj profil a tak dále. Obvykle budete chtít zabránit noví uživatelé předtím, než byl potvrzen e-mailu, textovou zprávu SMS nebo jiný mechanismus publikování všechna data na webové stránky. <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>Vytváření více ucelenou ukázku

V této části použijete ke stažení, které budeme pracovat s více ucelenou ukázku NuGet.

1. Vytvořte novou ***prázdný*** webový projekt ASP.NET.
2. V konzole Správce balíčků, zadejte následující příkazy: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   V tomto kurzu použijeme [sendgrid vám umožňuje](http://sendgrid.com/) k odesílání e-mailu. `Identity.Samples` Balíček nainstaluje kód Pracujeme s.
3. Nastavte [projektu pro použití protokolu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Otestujte vytvoření místního účtu spuštěním aplikace a kliknutím na **zaregistrovat** propojit a publikování registračním formuláři.
5. Klikněte na odkaz Ukázkový e-mailu, který simuluje potvrzení e-mailu.
6. Odebrání vzorku ukázkový e-mailu odkaz potvrzovací kód ( `ViewBag.Link` kódu v kontroleru účtu. Najdete v článku `DisplayEmail` a `ForgotPasswordConfirmation` metody akce a zobrazení syntaxe razor).

> [!NOTE]
> Upozornění: Pokud změníte některá nastavení zabezpečení v této ukázce, výroby, které aplikace bude muset podstoupit auditu zabezpečení, která explicitně volá změny provedené.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Kontrola kódu v aplikaci\_Start\IdentityConfig.cs

Ukázka ukazuje, jak vytvořit účet a přidat jej do *správce* role. E-mailu v ukázce měli nahradit e-mailu, který budete používat pro účet správce. Nejjednodušší způsob, teď k vytvoření účtu správce je prostřednictvím kódu programu v `Seed` metoda. Věříme, že budete mít v budoucnu nástroj, který vám umožní vytváření a správu uživatelů a rolí. Ukázkový kód vám umožní vytvořit a spravovat uživatele a role, ale nejdřív musí mít účet správce ke spuštění role a uživatele správce stránky. V této ukázce se vytvoří účet správce, když se nasadí databáze.

Změnit heslo a změňte název na účet, kterou budete dostávat e-mailová oznámení.

> [!WARNING]
> Zabezpečení – nikdy úložiště citlivá data ve zdrojovém kódu.

Jak je uvedeno nahoře, `app.CreatePerOwinContext` volání ve třídě spuštění přidá zpětná volání a `Create` metoda aplikace databáze obsahu, správce a role Manager tříd uživatelů. Volání kanál OWIN `Create` metoda na tyto třídy pro každý požadavek a ukládá kontext pro každou třídu. Účet řadiče zpřístupní správce uživatele z kontextu HTTP (který obsahuje kontext OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Když se uživatel zaregistruje místní účet, `HTTP Post Register` metoda je volána:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Výše uvedený kód používá k vytvoření nového uživatelského účtu pomocí e-mailu a heslo zadané data modelu. Pokud e-mailový alias v úložišti dat, vytvoření účtu se nezdaří a formulář se zobrazí znovu. `GenerateEmailConfirmationTokenAsync` Metoda vytvoří zabezpečené potvrzovací token a ukládá je v úložišti dat ASP.NET Identity. [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) metoda vytvoří odkaz obsahující `UserId` a potvrzovací token. Tento odkaz se pak e-mailem uživateli, může uživatel kliknutím na odkaz v jejich e-mailovou aplikaci a potvrďte svůj účet.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Nastavení e-mailu potvrzení

Přejděte na [sendgrid vám umožňuje Azure registrační stránku](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) a zaregistrovat účet zdarma. Přidejte kód podobná následující příkaz pro konfiguraci sendgrid vám umožňuje:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> E-mailoví klienti často přijímají pouze textové zprávy (žádné HTML). Měl by poskytnout zprávy v text nebo HTML. Ve výše uvedené ukázkové sendgrid vám umožňuje to provádí pomocí `myMessage.Text` a `myMessage.Html` výše uvedeném kódu.


Následující kód ukazuje, jak odeslat e-mailu pomocí [poštovní zpráva](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) třídy kde `message.Body` vrátí jenom propojení.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Zabezpečení – nikdy úložiště citlivá data ve zdrojovém kódu. Účet a přihlašovací údaje jsou uloženy v appSetting. V Azure, můžete bezpečně uložit tyto hodnoty na **[konfigurace](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** na portálu Azure. V tématu [osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a do Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


Zadejte přihlašovacích údajů služby SendGrid, spusťte aplikaci, můžete zaregistrovat s e-mailový alias kliknutím na odkaz potvrdit v e-mailu. Chcete-li zjistit, jak to provést pomocí vaší [Outlook.com](http://outlook.com) e-mailový účet, najdete v části Jan Atten [C# konfigurace SMTP pro hostitele SMTP Outlook.Com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) a svůj[technologii ASP.NET 2.0 Identity: nastavení ověření účtu dvoufaktorové ověřování a](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) odešle.

Jakmile uživatel klikne **zaregistrovat** tlačítko potvrzovací e-mail obsahující ověřovací token bude zaslána e-mailové adresy.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

Uživatel je odeslán e-mail s potvrzovacím tokenem ke svému účtu.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Zkontrolujte kód

Následující kód ukazuje `POST ForgotPassword` metoda.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Metoda selže bez upozornění, pokud nebyl potvrzen e-mail uživatele. Jestliže chybu účtuje Neplatná e-mailové adrese, uživatelé se zlými úmysly může pomocí těchto informací najít platné ID uživatele (e-mailu aliasy) vůči útokům.

Následující kód ukazuje `ConfirmEmail` metoda v kontroler účtů, která je volána, když uživatel klikne na potvrzení odkaz v e-mailu na je:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Jakmile jsou využívány token zapomenuté heslo, je neplatná. Následující změny v kódu `Create` – metoda (v *aplikace\_Start\IdentityConfig.cs* souboru) nastaví tokeny vyprší za 3 hodiny.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Pomocí výše uvedený kód zapomenuté heslo a tokenů potvrzení e-mailu vyprší za 3 hodiny. Výchozí hodnota `TokenLifespan` je jeden den.

Následující kód ukazuje metodu potvrzení e-mailu:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Chcete-li aplikace bezpečnější, podporuje ASP.NET Identity dvoufaktorové ověřování (2FA). V tématu [identitu ASP.NET 2.0: nastavení ověření účtu a dvoufaktorové ověřování](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) podle Atten Jan. I když selhání pokusů o přihlášení hesel můžete nastavit uzamčení účtu, tento přístup umožňuje vaše přihlášení náchylné k [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) uzamčení. Doporučujeme, abyste že je použít pouze s 2FA uzamčení účtu.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Další prostředky

- [Přehled poskytovatelů vlastního úložiště pro ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Aplikace MVC 5 se službou Facebook, Twitter, LinkedIn a přihlašování Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) také ukazuje, jak přidat informace o profilu do tabulky uživatelů.
- [ASP.NET MVC a Identity 2.0: Principy Základy](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) podle Atten Jan.
- [Úvod do ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Uvedení RTM technologie ASP.NET identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) podle Pranav Rastogi.
