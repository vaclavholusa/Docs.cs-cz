---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplikace ASP.NET MVC 5 s SMS a e-mailu Dvojúrovňového ověřování | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5 s dvoufaktorovým ověřováním. Vytvoření zabezpečené ASP.NET MVC 5 webové aplikace s by se měla dokončit...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: a422988f681273b153725d32bb8337deb25b12f1
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912589"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Aplikace ASP.NET MVC 5 s SMS a e-mailu dvoufaktorového ověřování
====================
Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))

> V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5 s dvoufaktorovým ověřováním. By se měla Dokončit [vytvořit zabezpečenou webovou aplikaci ASP.NET MVC 5 s přihlášením, resetovat heslo a potvrzení e-mailu](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) než budete pokračovat. Můžete stáhnout hotovou aplikaci [tady](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Položka ke stažení obsahuje ladění pomocné rutiny, které umožňují otestovat bez nastavení e-mailem nebo poskytovatele služby SMS serveru SMS a e-mailové potvrzení.
> 
> V tomto kurzu zapsal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Vytvoření aplikace ASP.NET MVC](#createMvc)
- [Nastavení služby SMS pro dvoufaktorového ověřování](#SMS)
- [Povolení dvoufaktorového ověřování](#enable2)
- [Další prostředky](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Vytvoření aplikace ASP.NET MVC

Začněte tím, že instalaci a používání [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Nainstalujte [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší.

> [!NOTE]
> Upozornění: By se měla Dokončit [vytvořit zabezpečenou webovou aplikaci ASP.NET MVC 5 s přihlášením, resetovat heslo a potvrzení e-mailu](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) než budete pokračovat. Je nutné nainstalovat [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší, k dokončení tohoto kurzu.


1. Vytvoření nového projektu ASP.NET Web a vyberte šablonu MVC. Webové formuláře také podporuje ASP.NET Identity, takže může podle podobných kroků ve webové aplikaci formulářů.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Ponechte výchozí ověřování jako **jednotlivé uživatelské účty**. Pokud chcete hostovat aplikace ve službě Azure, ponechte políčko zaškrtnuté. Později v tomto kurzu nasadíme do Azure. Je možné [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Nastavte [projektu pro použití protokolu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Adresa:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Obor názvů:  
    `ASPSMSX2`
3. **Zjištění přihlašovací údaje uživatele poskytovatele serveru SMS**  
  
   Twilio:  
   Z **řídicí panel** kartu svého účtu Twilio, kopie **SID účtu** a **ověřovací token**.  
  
   ASPSMS:  
   V nastavení účtu, přejděte na **vlastnosti userkey jedná** a zkopírujte ho spolu s místním definované **heslo**.  
  
   Později jsme uloží tyto hodnoty *web.config* souboru v rámci klíče `"SMSAccountIdentification"` a `"SMSAccountPassword"` .
4. **Určení SenderID / původce**  
  
   Twilio:  
   Z **čísla** kartu, zkopírujte své telefonní číslo Twilio.  
  
   ASPSMS:  
   V rámci **odemknout autoru** nabídce odemknout jeden nebo více autoru nebo zvolte příkazce alfanumerické znaky (není podporováno ve všech sítích).  
  
   Dále jsme uloží tuto hodnotu v *web.config* soubor daný klíč `"SMSAccountFrom"` .
5. **Přenos přihlašovací údaje poskytovatele služby SMS do aplikace**  
  
   Přihlašovací údaje a zpřístupníte odesílatele telefonní číslo do aplikace. Pro zjednodušení jsme uloží tyto hodnoty *web.config* souboru. Pokud nasadíme do Azure, abychom mohli uložit hodnoty bezpečně v **nastavení aplikace** karta Konfigurace části na webové stránce. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Zabezpečení – nikdy ukládání citlivých dat ve zdrojovém kódu. Účet a přihlašovací údaje se přidají do výše uvedený pro zjednodušení ukázkový kód. Zobrazit [osvědčené postupy pro nasazení hesel a dalších citlivých dat do ASP.NET a Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Provádění přenos dat do poskytovatele služby SMS**  
  
   Konfigurace `SmsService` třídy v *aplikace\_Start\IdentityConfig.cs* souboru.  
  
   V závislosti na poskytovateli serveru SMS používá aktivaci buď **Twilio** nebo **ASPSMS** části: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Aktualizace *Views\Manage\Index.cshtml* zobrazení Razor: (Poznámka: není právě komentářů ve stávající kód, použijte následující kód.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Ověření `EnableTwoFactorAuthentication` a `DisableTwoFactorAuthentication` metody akce v `ManageController` mít[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atribut:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Spusťte aplikaci a přihlaste se pomocí účtu, který jste dříve zaregistrovali.
10. Klikněte na své ID uživatele, která se aktivuje `Index` metodu akce v `Manage` kontroleru.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Klikněte na tlačítko Přidat.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber` Metoda akce se zobrazí dialogové okno zadat telefonní číslo, které můžou přijímat zprávy SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. Během několika sekund obdržíte textovou zprávu s ověřovacím kódem. Zadejte ji a stiskněte klávesu **odeslat**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. Správa zobrazení ukazuje že vaše telefonní číslo bylo přidáno.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Povolení dvoufaktorového ověřování

V aplikaci vygenerovanou šablonu budete muset použít uživatelské rozhraní pro povolení dvoufaktorového ověřování (2FA). Pokud chcete povolit 2FA, klikněte na své ID uživatele (e-mailový alias) na navigačním panelu.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Klikněte na povolit 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Odhlášení, potom znovu přihlásit v. Pokud jste povolili e-mailu (naleznete v tématu Moje [předchozí kurz o službě](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), můžete vybrat, SMS nebo e-mailu pro 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Zobrazí se stránka Ověřte kód, ve kterém můžete zadat kód (z SMS nebo e-mailu).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Kliknutím na **chcete Zapamatovat tento prohlížeč** zaškrtávacího políčka můžete vyloučit z by bylo potřeba při používání prohlížeče a zařízení, kde zaškrtnuté políčko přihlášení pomocí 2FA. Za předpokladu, uživateli se zlými úmysly nemůže získat přístup k zařízení, povolení 2FA a kliknete na **chcete Zapamatovat tento prohlížeč** vám poskytne pohodlný přístup heslo jeden krok, při zachování ochrany silné 2FA pro veškerý přístup z nedůvěryhodné zařízení. Můžete to provést na libovolném privátní zařízení, kterou používáte pravidelně.

Tento kurz obsahuje rychlý úvod k povolení 2FA na novou aplikaci ASP.NET MVC. Tento kurz [dvoufaktorovým ověřováním pomocí SMS a e-mailu s ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) obsahuje podrobnosti na pozadí ukázky kódu.

<a id="addRes"></a>
## <a name="additional-resources"></a>Další prostředky

- [Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) obsahuje podrobnosti o dvoufaktorového ověřování
- [Odkazy na ASP.NET Identity doporučené zdroje informací](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Potvrzení účtu a obnovení hesla s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) obsahuje větší podrobnosti o potvrzení hesla obnovení a účtu.
- [Aplikace MVC 5 s Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) v tomto kurzu se dozvíte, jak psát aplikace ASP.NET MVC 5 s autorizací Facebook nebo Google OAuth 2. Také ukazuje, jak přidat další data do databáze nástroje Identity.
- [Nasazení zabezpečenou aplikaci ASP.NET MVC pomocí členství, OAuth a SQL Database do Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). V tomto kurzu přidá nasazení v Azure, jak zabezpečit vaši aplikaci s rolemi, jak můžete členské rozhraní API přidávat uživatele a role a další funkce zabezpečení.
- [Vytvoření aplikace služby Google OAuth 2 a připojení aplikace k projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Vytvoření aplikace na Facebooku a připojení aplikace k projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Nastavení protokolu SSL v projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [Jak nastavit vývojové prostředí jazyka C# a architektura ASP.NET MVC](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
