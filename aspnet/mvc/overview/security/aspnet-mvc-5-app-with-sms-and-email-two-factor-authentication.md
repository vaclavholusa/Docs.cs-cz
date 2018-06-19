---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplikace ASP.NET MVC 5 s SMS a e-mailu dvoufaktorové ověřování | Microsoft Docs
author: Rick-Anderson
description: Tento kurz ukazuje, jak sestavit webové aplikace ASP.NET MVC 5 s dvoufaktorové ověřování. Vytvoření zabezpečeného ASP.NET MVC 5 webové aplikace s by se měla dokončit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 5e1c54b3901f2c8c85134445c1fa91ee9f2e0d59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873609"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Aplikace ASP.NET MVC 5 s SMS a e-mailu dvoufaktorové ověřování
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> Tento kurz ukazuje, jak sestavit webové aplikace ASP.NET MVC 5 s dvoufaktorové ověřování. By se měla Dokončit [vytvořit zabezpečený webovou aplikaci ASP.NET MVC 5 s protokolu v e-mailu potvrzení a heslo resetovat](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) než budete pokračovat. Hotová aplikace si můžete stáhnout [zde](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Stahování obsahuje ladění pomocné rutiny, které umožňují testování potvrzení e-mailu a SMS bez nastavení e-mailem nebo poskytovatele služby SMS.
> 
> V tomto kurzu napsal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Vytvoření aplikace ASP.NET MVC](#createMvc)
- [Nastavení služby SMS pro dvoufaktorové ověřování](#SMS)
- [Povolit dvoufaktorové ověřování](#enable2)
- [Další zdroje informací](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Vytvoření aplikace ASP.NET MVC

Začněte tím, že instalace a spuštění [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Nainstalujte [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší.

> [!NOTE]
> Upozornění: By se měla Dokončit [vytvořit zabezpečený webovou aplikaci ASP.NET MVC 5 s protokolu v e-mailu potvrzení a heslo resetovat](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) než budete pokračovat. Je nutné nainstalovat [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší k dokončení tohoto kurzu.


1. Vytvořte nový projekt ASP.NET Web a vyberte šablonu MVC. Webové formuláře také podporuje ASP.NET Identity, takže můžete sledovat v aplikaci web forms podobným způsobem.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Ponechte výchozí ověřování jako **jednotlivé uživatelské účty**. Pokud chcete hostovat aplikaci v Azure, nechte políčko zaškrtnuto. Později v tomto kurzu jsme nasadit do Azure. Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Nastavte [projektu pro použití protokolu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Adresa:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Obor názvů:  
    `ASPSMSX2`
3. **Zjištění přihlašovací údaje uživatele poskytovatele serveru SMS**  
  
   Twilio:  
   Z **řídicí panel** kartě svého účtu Twilio kopie **SID účtu** a **Auth token**.  
  
   ASPSMS:  
   V nastavení svého účtu, přejděte na **Userkey** a zkopírujte jej spolu s samoobslužné definované **heslo**.  
  
   Později jsme se uloží v tyto hodnoty *web.config* souboru v rámci klíče `"SMSAccountIdentification"` a `"SMSAccountPassword"` .
4. **Zadání ID odesílatele nebo původce**  
  
   Twilio:  
   Z **čísla** kartě, zkopírujte své telefonní číslo Twilio.  
  
   ASPSMS:  
   V rámci **odemknutí autoru** nabídce odemknutí jeden nebo více autoru nebo vyberte alfanumerické původce (nepodporuje všechny sítě).  
  
   Později jsme uloží tuto hodnotu v *web.config* souboru daný klíč `"SMSAccountFrom"` .
5. **Přenos přihlašovací údaje poskytovatele služby SMS do aplikace**  
  
   Přihlašovací údaje a zpřístupníte odesílatele telefonní číslo do aplikace. Pro zjednodušení jsme se uloží v tyto hodnoty *web.config* souboru. Když jsme nasadit do Azure, uložíme můžete bezpečně v hodnoty **nastavení aplikace** karta Konfigurace části na webu. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Zabezpečení – nikdy úložiště citlivá data ve zdrojovém kódu. Účet a přihlašovací údaje se přidají do výše pro zjednodušení v ukázkovém kódu. V tématu [osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a do Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementace přenos dat do poskytovatele služby SMS**  
  
   Konfigurace `SmsService` třídy v *aplikace\_Start\IdentityConfig.cs* souboru.  
  
   V závislosti na použité poskytovatele služby SMS aktivovat buď **Twilio** nebo **ASPSMS** části: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Aktualizace *Views\Manage\Index.cshtml* zobrazení syntaxe Razor: (Poznámka: není právě Odebere komentáře v existujícímu kódu, použijte následující kód.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Ověřte `EnableTwoFactorAuthentication` a `DisableTwoFactorAuthentication` metody akce v `ManageController` mít[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atribut:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Spusťte aplikaci a přihlaste se pomocí účtu, který jste dříve registrován.
10. Klikněte na vaše ID uživatele, který aktivuje `Index` metodu akce v `Manage` řadiče.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Klikněte na tlačítko Přidat.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber` Metoda akce zobrazí dialogové okno zadat telefonní číslo, které může přijímat zprávy SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. Během pár sekund obdržíte textovou zprávu s ověřovacím kódem. Zadejte ho a stiskněte klávesu **odeslání**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. Správa zobrazení ukazuje, že vaše telefonní číslo bylo přidáno.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Povolit dvoufaktorové ověřování

V aplikaci vygenerované šablony budete muset povolit dvoufaktorové ověřování (2FA) pomocí uživatelského rozhraní. Chcete-li povolit 2FA, klikněte na vaše ID uživatele (e-mailový alias) na navigačním panelu.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Klikněte na povolit 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Odhlášení, potom znovu přihlásit v. Pokud jste povolili e-mailu (najdete v části Moje [předchozí kurzu](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), můžete vybrat SMS nebo e-mailu pro 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Zobrazí se stránka ověření kódu, kde můžete zadat kód (z SMS nebo e-mailu).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Kliknutím na **mějte na paměti, tento prohlížeč** zaškrtávací políčko je vyloučit z museli používat 2FA k přihlášení při používání prohlížeče a zařízení, kde zaškrtnuté políčko. Tak dlouho, dokud uživatelé se zlými úmysly nemůže získat přístup k zařízení, povolení 2FA a kliknutím na **mějte na paměti, tento prohlížeč** vám poskytne přístup heslo pohodlný jeden krok, a zároveň zachovat ochranu silným 2FA pro veškerý přístup. z nedůvěryhodné zařízení. Můžete provést na jakémkoli privátní zařízení, kterou používáte pravidelně.

V tomto kurzu poskytuje rychlý úvod do povolení 2FA na novou aplikaci ASP.NET MVC. Moje kurzu [dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) přejde do podrobností na ukázku kódu.

<a id="addRes"></a>
## <a name="additional-resources"></a>Další prostředky

- [Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) přejde do podrobnosti o dvoufaktorové ověřování
- [Odkazy na identitě ASP.NET Identity doporučené prostředky](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Účet potvrzení a obnovení hesla s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) přejde do další podrobnosti o potvrzení hesla obnovení a účet.
- [Aplikace MVC 5 se službou Facebook, Twitter, LinkedIn a přihlašování Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) tohoto kurzu se dozvíte, jak psát aplikace ASP.NET MVC 5 s ověřování sítě Facebook a Google OAuth 2. Také ukazuje, jak přidat další data do databáze Identity.
- [Nasazení aplikace zabezpečené rozhraní ASP.NET MVC s členství, OAuth a databáze SQL Azure web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). V tomto kurzu přidá nasazení Azure, jak zabezpečit vaše aplikace s rolemi, jak používat členské rozhraní API k přidání uživatelů a rolí a další funkce zabezpečení.
- [Vytvoření aplikace na Google OAuth 2 a připojení aplikace k projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Vytvoření aplikace v síti Facebook a připojení aplikace k projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Nastavení protokolu SSL v projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
