---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Vytvořit rozhraní ASP.NET Web Forms aplikace pomocí SMS dvoufaktorové ověřování (C#) | Microsoft Docs
author: Erikre
description: V tomto kurzu se dozvíte, jak vytvářet aplikace webových formulářů ASP.NET pomocí Dvojúrovňového ověřování. Tento kurz byl navržen jako doplněk kurzu s názvem Cr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 6c040fd3e0592b8cfd230dcd85ed3293f0a22ba7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887422"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Vytvořit rozhraní ASP.NET Web Forms aplikace pomocí SMS dvoufaktorové ověřování (C#)
====================
Podle [Erik Reitan](https://github.com/Erikre)

[Stáhněte si aplikaci ASP.NET Web Forms s e-mailu a SMS dvoufaktorové ověřování](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> V tomto kurzu se dozvíte, jak vytvářet aplikace webových formulářů ASP.NET pomocí Dvojúrovňového ověřování. Tento kurz byl navržený tak, aby doplňovala kurzu s názvem [vytvořit zabezpečený aplikací webových formulářů ASP.NET s registrací uživatele, e-mailem potvrzení a heslo resetovat](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Kromě toho se v tomto kurzu podle Rick Anderson [kurz MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Úvod

Tento kurz vás provede kroky potřebné pro vytvoření aplikace webových formulářů ASP.NET, která podporuje dvoufaktorové ověřování pomocí sady Visual Studio. Dvoufaktorové ověřování je krok ověřování navíc uživatele. Tento krok navíc generuje jedinečné osobní identifikační číslo (PIN) během přihlášení. PIN kód se většinou posílají uživateli jako e-mail nebo SMS zprávy. Při přihlášení uživatele vaší aplikace pak nezadá PIN jako opatření další ověřování.

### <a name="tutorial-tasks-and-information"></a>Kurz úkoly a informace:

- [Vytvoření aplikace ASP.NET – webové formuláře](#createWebForms)
- [Instalační program služby SMS a dvoufaktorové ověřování](#SMS)
- [Povolení dvoufaktorového ověřování pro registrovaný uživatel](#use2FA)
- [Další zdroje informací](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Vytvoření aplikace ASP.NET – webové formuláře

Začněte tím, že instalace a spuštění [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Nainstalujte [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší také. Navíc budete muset vytvořit [Twilio](https://www.twilio.com/try-twilio) účet, jak je popsáno níže.

> [!NOTE]
> Důležité: Je nutné nainstalovat [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší k dokončení tohoto kurzu.


1. Vytvoření nového projektu (**soubor**  - &gt; **nový projekt**) a vyberte **webové aplikace ASP.NET** šablony společně s rozhraní .NET Framework verze 4.5.2 z **nový projekt** dialogové okno.
2. Z **nový projekt ASP.NET** dialogové okno, vyberte **webových formulářů** šablony. Ponechte výchozí ověřování jako **jednotlivé uživatelské účty**. Potom klikněte na **OK** k vytvoření nového projektu.  
    ![Dialogové okno Nový projekt ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Povolte Secure Sockets Layer (SSL) pro projekt. Postupujte podle kroků v k dispozici **povolit šifrování SSL pro projekt** části [Začínáme s webovými formuláři kurz řadou](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. V sadě Visual Studio, otevřete **Konzola správce balíčků** (**nástroje**  - &gt; **Manager balíček NuGet**  - &gt; **Konzola správce balíčků**) a zadejte následující příkaz:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Instalační program služby SMS a dvoufaktorové ověřování

Tento kurz používá Twilio, ale mohou využívat kteréhokoli zprostředkovatele serveru SMS.

1. Vytvoření [Twilio](https://www.twilio.com/try-twilio) účtu.
2. Z **řídicí panel** kartě svého účtu Twilio kopie **SID účtu** a **ověření tokenu.** Přidáte je do vaší aplikace později.
3. Z **čísla** kartě, zkopírujte vaše Twilio **telefonní číslo** také.
4. Ujistěte se, Twilio **SID účtu**, **Auth Token** a **telefonní číslo** k dispozici pro aplikaci. Pro zjednodušení se uloží v tyto hodnoty *web.config* souboru. Když nasadíte do Azure, můžete ukládat bezpečně v hodnoty **appSettings** karta Konfigurace části na webu. Navíc při přidávání telefonní číslo, používejte pouze čísla.   
   Všimněte si, že můžete také přidat přihlašovací údaje sendgrid vám umožňuje. Sendgrid vám umožňuje je služba oznámení e-mailu. Podrobnosti o tom, jak povolit Sendgridu, naleznete v části "háku až Sendgridu, kurzu s názvem [vytvoření zabezpečeného ASP.NET webové formuláře aplikace s registrací uživatele, e-mailem potvrzení a heslo resetovat.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Zabezpečení – nikdy úložiště citlivá data ve zdrojovém kódu. V tomto příkladu účet a přihlašovací údaje uložené v **appSettings** části *Web.config* souboru. V Azure, můžete bezpečně uložit tyto hodnoty na **[konfigurace](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** na portálu Azure. Další informace naleznete v tématu Rick Anderson s názvem [osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a do Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Konfigurace `SmsService` třídy v *aplikace\_Start\IdentityConfig.cs* zvýrazněných v žlutý změně souboru tím, že provedete následující: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Přidejte následující `using` příkazy na začátek *IdentityConfig.cs* souboru: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Aktualizace *Account/Manage.aspx* souboru odebráním zvýrazněných v žlutý řádky:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. V `Page_Load` obslužnou rutinu *Manage.aspx.cs* kódu, zrušte komentář u následuje řádek kódu zvýrazněných v žlutý, takže se objeví jako: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. V codebehind z *účet*/*TwoFactorAuthenticationSignIn.aspx.cs*, aktualizovat `Page_Load` obslužná rutina přidáním zvýrazněných v žlutý následující kód: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Tím, že ve výše uvedeném kódu změňte, nebude "Zprostředkovatele" rozevírací seznam, který obsahuje možnosti ověřování nastavena na první hodnota. To vám umožní uživateli úspěšně vybrat všechny možnosti pro použití při ověřování, nikoli jen na prvním.
10. V **Průzkumníku řešení**, klikněte pravým tlačítkem na *Default.aspx* a vyberte **nastavit jako úvodní stránku**.
11. Testování vaší aplikace, nejprve sestavení aplikace (**Ctrl**+**Shift**+**B**) a pak spusťte aplikaci (**F5**) a Vyberte buď **zaregistrovat** vytvořit nový uživatelský účet nebo vybrat **přihlásit** Pokud uživatelský účet je již zaregistrován.
12. Jakmile přihlásili vám (jako uživatel), klikněte na ID uživatele (e-mailovou adresu) na navigačním panelu zobrazíte **spravovat účet** stránky (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Klikněte na tlačítko **přidat** vedle **telefonní číslo** na **spravovat účet** stránky.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Telefonní číslo, kde (jako uživatel) chcete přijímat zprávy SMS (textových zpráv) a klikněte na tlačítko Přidat **odeslání** tlačítko.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    V tomto okamžiku aplikace bude používat přihlašovací údaje z *Web.config* kontaktovat Twilio. SMS zpráva (SMS) odešle do telefonu přidružené k účtu uživatele. Můžete ověřit, že Twilio zpráva byla odeslána pomocí zobrazení řídicího panelu Twilio.
15. Během pár sekund získají phone přidružené k účtu uživatele textovou zprávu obsahující ověřovací kód. Zadejte ověřovací kód a stiskněte klávesu **odeslání**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Povolit dvoufaktorové ověřování registrovaný uživatel

V tomto okamžiku jste povolili dvoufaktorového ověřování pro vaši aplikaci. Uživatel použít dvojúrovňové ověřování můžou jednoduše změnit jejich nastavení pomocí uživatelského rozhraní. 

1. Jako uživatel aplikace můžete povolit dvoufaktorové ověřování pro vaše konkrétní účet kliknutím na ID uživatele (e-mailový alias) na navigačním panelu zobrazíte **spravovat účet** stránky. Potom klikněte na **povolit** odkaz na povolení dvoufaktorového ověřování pro účet.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Odhlásit a potom znovu přihlásit. Pokud jste povolili e-mailu, můžete vybrat SMS nebo e-mailu pro dvoufaktorové ověřování. Pokud jste nepovolili e-mailu, najdete v části kurzu s názvem [vytvoření zabezpečeného ASP.NET webové formuláře aplikace s registrací uživatele, potvrzení e-mailu a resetování hesla](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Zobrazí se stránka dvoufaktorové ověřování, kde můžete zadat kód (z SMS nebo e-mailu).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Kliknutím na **mějte na paměti, tento prohlížeč** zaškrtávací políčko je vyloučit z museli používat k přihlášení při používání prohlížeče a zařízení, kde zaškrtnuté políčko dvoufaktorové ověřování. Tak dlouho, dokud uživatelé se zlými úmysly nemůže získat přístup k zařízení, povolení dvoufaktorového ověřování a kliknutím na **mějte na paměti, tento prohlížeč** vám poskytne přístup heslo pohodlný jeden krok, a zároveň zachovat silné Ochrana dvoufaktorového ověřování pro veškerý přístup z nedůvěryhodné zařízení. Můžete provést na jakémkoli privátní zařízení, kterou používáte pravidelně.

<a id="addRes"></a>
## <a name="additional-resources"></a>Další prostředky

- [Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Odkazy na identitě ASP.NET Identity doporučené prostředky](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Nasazení aplikace zabezpečené rozhraní ASP.NET Web Forms s členství, OAuth a SQL Database na web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Kurz řady webových formulářů ASP.NET – přidání poskytovatele OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET řady kurz webové formuláře – povolit šifrování SSL pro projekt](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Potvrzení účtu a heslo pro obnovení s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Vytvoření aplikace v síti Facebook a připojení aplikace k projektu](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
