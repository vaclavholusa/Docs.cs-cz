---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Vytvoření ASP.NET Web Forms aplikace pomocí Dvojúrovňového ověřování pomocí SMS (C#) | Dokumentace Microsoftu
author: Erikre
description: V tomto kurzu se dozvíte, jak vytvořit aplikaci webových formulářů ASP.NET s dvoufaktorovým ověřováním. V tomto kurzu je navržená k doplnění kurz s názvem Cr...
ms.author: aspnetcontent
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 16045b116ca5c797e7840f2ee5944e5f2c6282eb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803546"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Vytvoření ASP.NET Web Forms aplikace pomocí Dvojúrovňového ověřování pomocí SMS (C#)
====================
podle [Erik Reitan](https://github.com/Erikre)

[Stáhněte si aplikaci rozhraní ASP.NET Web Forms s e-mailu a Dvojúrovňového ověřování pomocí SMS](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> V tomto kurzu se dozvíte, jak vytvořit aplikaci webových formulářů ASP.NET s dvoufaktorovým ověřováním. V tomto kurzu je navržená k doplnění kurz s názvem [vytvoření zabezpečené aplikace webových formulářů ASP.NET s registrací uživatele, e-mailové potvrzení a resetováním hesla](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Kromě toho tento kurz je založený na Rick Anderson [kurz ASP.NET MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Úvod

Tento kurz vás provede kroky potřebné k vytvoření aplikace webových formulářů ASP.NET, která podporuje Dvojúrovňového ověřování pomocí sady Visual Studio. Dvoufaktorové ověřování je krok ověřování navíc uživatele. Tento další krok vygeneruje jedinečné osobní identifikační číslo (PIN) během přihlašování. PIN kód se obvykle odesílají uživateli jako e-mailu nebo SMS zprávy. Při přihlášení uživatele vaší aplikace potom zadá PIN kód jako opatření další ověřování.

### <a name="tutorial-tasks-and-information"></a>Kurz úkoly a informace:

- [Vytvoření aplikace ASP.NET Web Forms](#createWebForms)
- [Instalace služby SMS a dvoufaktorového ověřování](#SMS)
- [Povolení dvoufaktorového ověřování pro registrovaný uživatel](#use2FA)
- [Další zdroje informací](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Vytvoření aplikace ASP.NET Web Forms

Začněte tím, že instalaci a používání [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Nainstalujte [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší také. Kromě toho budete muset vytvořit [Twilio](https://www.twilio.com/try-twilio) účtu, jak je popsáno níže.

> [!NOTE]
> Důležité: Je nutné nainstalovat [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší, k dokončení tohoto kurzu.


1. Vytvoření nového projektu (**souboru**  - &gt; **nový projekt**) a vyberte **webová aplikace ASP.NET** šablony spolu s rozhraní .NET Framework od verze 4.5.2 **nový projekt** dialogové okno.
2. Z **nový projekt ASP.NET** dialogové okno, vyberte **webových formulářů** šablony. Ponechte výchozí ověřování jako **jednotlivé uživatelské účty**. Potom klikněte na **OK** k vytvoření nového projektu.  
    ![Dialogové okno Nový projekt ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Povolte vrstvy SSL (Secure Sockets) pro projekt. Postupujte podle kroků uvedených v **povolit šifrování SSL pro projekt** část [Začínáme s webovými formuláři sérii](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. V sadě Visual Studio, otevřete **Konzola správce balíčků** (**nástroje**  - &gt; **Správce balíčků NuGet**  - &gt; **Konzola správce balíčků**) a zadejte následující příkaz:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Instalace služby SMS a dvoufaktorového ověřování

Tento kurz používá Twilio, ale můžete použít jakýkoli poskytovatel serveru SMS.

1. Vytvoření [Twilio](https://www.twilio.com/try-twilio) účtu.
2. Z **řídicí panel** kartu svého účtu Twilio, kopie **SID účtu** a **ověřovací Token.** Přidáte je do své aplikace později.
3. Z **čísla** kartu, zkopírujte váš Twilio **telefonní číslo** také.
4. Ujistěte se, Twilio **SID účtu**, **ověřovací Token** a **telefonní číslo** k dispozici pro aplikaci. Pro zjednodušení budete ukládat tyto hodnoty *web.config* souboru. Při nasazování do Azure můžete ukládat bezpečně v hodnot **appSettings** karta Konfigurace části na webové stránce. Navíc když přidáte telefonní číslo, používejte pouze čísla.   
   Všimněte si, že můžete také přidat přihlašovací údaje služby SendGrid. SendGrid je služba e-mailové oznámení. Podrobnosti o tom, jak povolit SendGrid, naleznete v části "Hook nahoru SendGrid" kurzu s názvem [vytvořit zabezpečené webové formuláře aplikace v ASP.NET s registrací uživatele, e-mailové potvrzení a resetováním hesla.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Zabezpečení – nikdy ukládání citlivých dat ve zdrojovém kódu. V tomto příkladu účtu a přihlašovací údaje jsou uloženy v **appSettings** část *Web.config* souboru. V Azure, můžete bezpečně uložit tyto hodnoty na **[konfigurovat](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** karta na portálu Azure portal. Související informace naleznete v tématu Rick Anderson s názvem [osvědčené postupy pro nasazení hesel a dalších citlivých dat do ASP.NET a Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Konfigurace `SmsService` třídy v *aplikace\_Start\IdentityConfig.cs* změně žlutě zvýrazněné v souboru tak, že následující: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Přidejte následující `using` příkazy na začátek *IdentityConfig.cs* souboru: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Aktualizace *Account/Manage.aspx* souboru odebráním zvýrazněné žlutou barvou řádky:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. V `Page_Load` obslužná rutina *Manage.aspx.cs* použití modelu code-behind, Odkomentujte řádek kódu zvýrazněn žlutou barvou, tak že se objeví jako následující: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. V codebehind z *účet*/*TwoFactorAuthenticationSignIn.aspx.cs*, aktualizujte `Page_Load` obslužná rutina přidáním následujícího kódu zvýrazněné žlutou barvou: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Tím, že ve výše uvedeném kódu změňte, nebude "Zprostředkovatele" DropDownList obsahující možnosti ověřování nastavena na první hodnota. To vám umožní uživateli vybrat úspěšně všechny možnosti pro použití při ověřování, nikoli jen na prvním.
10. V **Průzkumníka řešení**, klikněte pravým tlačítkem na *Default.aspx* a vyberte **nastavit jako úvodní stránku**.
11. Při testování vaší aplikace, nejprve vytvořte aplikaci (**Ctrl**+**Shift**+**B**) a pak spusťte aplikaci (**F5**) a Vyberte buď **zaregistrovat** a vytvořte nový uživatelský účet nebo vyberte **přihlášení** Pokud uživatelský účet už je zaregistrovaný.
12. Jakmile (jako uživatel) se přihlásíte, klikněte na uživatelské ID (e-mailovou adresu) v navigačním panelu se zobrazí **spravovat účet** stránky (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Klikněte na tlačítko **přidat** vedle **telefonní číslo** na **spravovat účet** stránky.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Přidat telefonní číslo, kde (jako uživatel) chcete přijímat zprávy SMS (text zprávy) a klikněte na tlačítko **odeslat** tlačítko.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    V tomto okamžiku bude aplikace používat přihlašovací údaje z *Web.config* kontaktovat Twilio. Zpráva SMS (textová zpráva) se odešlou do telefonu přiřazeném k uživatelskému účtu. Můžete ověřit, že Twilio zpráva byla odeslána tím, že zobrazíte řídicí panel Twilio.
15. Během několika sekund telefonu přiřazeném k uživatelský účet získá textovou zprávu obsahující ověřovací kód. Zadejte ověřovací kód a stiskněte klávesu **odeslat**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Povolení dvoufaktorového ověřování pro registrovaný uživatel

V tomto okamžiku jste povolili dvoufaktorového ověřování pro vaši aplikaci. Uživatel chce použít dvojúrovňové ověřování mohou jednoduše změnit jejich nastavení pomocí uživatelského rozhraní. 

1. Jako uživatel aplikace můžete povolit dvoufaktorové ověřování pro konkrétní účet kliknutím na ID uživatele (e-mailový alias) v navigačním panelu se zobrazí **spravovat účet** stránky. Potom klikněte na **povolit** odkaz pro povolení dvoufaktorového ověřování pro účet.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Odhlásit a znovu přihlásit. Pokud jste povolili e-mailu, můžete vybrat, SMS nebo e-mailu pro dvoufaktorové ověřování. Pokud jste ještě nepovolili e-mailu, najdete v kurzu s názvem [vytvořit zabezpečené webové formuláře aplikace v ASP.NET s registrací uživatele, e-mailové potvrzení a resetování hesla](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Zobrazí se stránka dvojúrovňové ověřování, ve kterém můžete zadat kód (z SMS nebo e-mailu).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Kliknutím na **chcete Zapamatovat tento prohlížeč** zaškrtávacího políčka můžete před nutností přihlášení při používání prohlížeče a zařízení, kde zaškrtnuté políčko pomocí dvojúrovňového ověřování vyloučit. Za předpokladu, uživateli se zlými úmysly nemůže získat přístup k zařízení, povolení dvoufaktorového ověřování a kliknete na **chcete Zapamatovat tento prohlížeč** vám poskytne pohodlný přístup heslo jeden krok, při zachování strong dvoufaktorové ověřování ochrana pro veškerý přístup z jiných důvěryhodných zařízení. Můžete to provést na libovolném privátní zařízení, kterou používáte pravidelně.

<a id="addRes"></a>
## <a name="additional-resources"></a>Další prostředky

- [Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Odkazy na ASP.NET Identity doporučené zdroje informací](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Nasaďte aplikaci zabezpečení rozhraní ASP.NET Web Forms s nástroji Membership, OAuth a SQL Database na web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Série kurzů webových formulářů ASP.NET – přidání poskytovatele OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Technologie ASP.NET webové formuláře sérii - povolit šifrování SSL pro projekt](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Potvrzení účtu a obnovení hesla s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Vytvoření aplikace na Facebooku a připojení aplikace k projektu](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
