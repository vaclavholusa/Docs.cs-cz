---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Vytvoření zabezpečeného aplikace webových formulářů ASP.NET s registrací uživatele, e-mailem potvrzení a heslo resetovat (C#) | Microsoft Docs
author: Erikre
description: V tomto kurzu se dozvíte, jak vytvářet aplikace webových formulářů ASP.NET pomocí registrace uživatele, potvrzení e-mailu a resetování hesla pomocí ASP.NET Identity člena...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 1dc7ace69473b45432fd942b9cf1ba32332cb707
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Vytvoření zabezpečeného aplikace webových formulářů ASP.NET s registrací uživatele, e-mailem potvrzení a heslo resetovat (C#)
====================
Podle [Erik Reitan](https://github.com/Erikre)

> V tomto kurzu se dozvíte, jak vytvářet aplikace webových formulářů ASP.NET pomocí registrace uživatele, potvrzení e-mailu a resetování hesla pomocí systému členství ASP.NET Identity. V tomto kurzu bylo založeno na Rick Anderson [kurz MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).


## <a name="introduction"></a>Úvod

Tento kurz vás provede kroky potřebné k vytváření aplikací webových formulářů ASP.NET pomocí sady Visual Studio a technologie ASP.NET 4.5 pro vytvoření zabezpečeného aplikace webových formulářů s registrací uživatele, e-mailem potvrzení a heslo resetovat.

### <a name="tutorial-tasks-and-information"></a>Kurz úkoly a informace:

- [Vytvořit rozhraní ASP.NET Web Forms aplikace](#createWebForms)
- [Propojte sendgrid vám umožňuje](#SG)
- [Požadovat potvrzení e-mailu před přihlášení](#require)
- [Obnovení hesla a resetování](#reset)
- [Odkaz pro potvrzení znovu odeslat e-mailu](#rsend)
- [Řešení potíží s aplikací](#dbg)
- [Další zdroje informací](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Vytvoření aplikace ASP.NET – webové formuláře

Začněte tím, že instalace a spuštění [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Nainstalujte [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší také.

> [!NOTE]
> Upozornění: Je nutné nainstalovat [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší k dokončení tohoto kurzu.


1. Vytvoření nového projektu (**soubor**  - &gt; **nový projekt**) a vyberte **webové aplikace ASP.NET** šablony a nejnovější rozhraní .NET Framework verze z **nový projekt** dialogové okno.
2. Z **nový projekt ASP.NET** dialogové okno, vyberte **webových formulářů** šablony. Ponechte výchozí ověřování jako **jednotlivé uživatelské účty**. Pokud chcete hostovat aplikaci v Azure, nechte **hostitel v cloudu** políčko zaškrtnuto.   
 Potom klikněte na **OK** k vytvoření nového projektu.  
    ![Dialogové okno Nový projekt ASP.NET](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Povolte Secure Sockets Layer (SSL) pro projekt. Postupujte podle kroků v k dispozici **povolit šifrování SSL pro projekt** části [Začínáme s webovými formuláři kurz řadou](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Spuštění aplikace, klikněte na tlačítko **zaregistrovat** propojit a registraci nového uživatele. V tomto okamžiku je pouze ověření na e-mailu na základě [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atribut pro zajištění ve správném formátu e-mailovou adresu. Slouží k úpravě kódu k přidání potvrzení e-mailu. Zavřete okno prohlížeče.
5. V **Průzkumníka serveru** sady Visual Studio (**zobrazení**  - &gt; **Průzkumníka serveru**), přejděte na **Connections\ dat DefaultConnection\Tables\AspNetUsers**, klikněte pravým tlačítkem a vyberte **otevřete definici tabulky**. 

    Na následujícím obrázku `AspNetUsers` schématu tabulky:

    ![Schéma tabulky AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. V **Průzkumníka serveru**, klikněte pravým tlačítkem na **AspNetUsers** tabulky a vyberte **zobrazit Data tabulky**.  
  
    ![Data tabulky AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 V tomto okamžiku není potvrzena e-mailu pro uživatele registrovaná.
7. Klikněte na řádek a vyberte možnost odstranit pro odstranění uživatele. Budete znovu přidat tuto e-mailu v dalším kroku a odeslat zprávu potvrzení e-mailovou adresu.

## <a name="email-confirmation"></a>Potvrzení e-mailu

Je osvědčeným postupem potvrďte e-mailu během registrace nového uživatele ověřit, že nejsou zosobnění někdo jiný (to znamená, že nebyly zaregistrována někoho jiného e-mailu). Předpokládejme, že jste měli diskusní fórum, chcete zabránit `"bob@cpandl.com"` registraci jako `"joe@contoso.com"`. Bez potvrzení e-mailu `"joe@contoso.com"` může získat nežádoucí e-mailu z vaší aplikace. Předpokládejme, že Bob, ať už náhodně registrován jako `"bib@cpandl.com"` a, kdyby zaznamenali mu nebude moci používat obnovení hesla, protože aplikace nemá správnou e-mailovou. Potvrzení e-mailu poskytuje jen omezenou ochranu z robotů a neposkytuje ochranu proti určené odesílatelům nevyžádané pošty.

Obvykle budete chtít zabránit noví uživatelé předtím, než byl potvrzen e-mail, textovou zprávu SMS nebo jiný mechanismus publikování všechna data na váš web. V následujících částech se budeme povolí potvrzení e-mailu a upravit kód nově zaregistrovaný uživatelům zabránit protokolování, dokud bylo potvrzeno e-mailu. V tomto kurzu budete používat e-mailovou službu sendgrid vám umožňuje.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Propojte sendgrid vám umožňuje

I když tento kurz ukazuje jenom postup přidání e-mailové oznámení prostřednictvím [sendgrid vám umožňuje](http://sendgrid.com/), můžete odeslat e-mailu pomocí protokolu SMTP a další mechanismy (viz [další prostředky](#addRes)).

1. V sadě Visual Studio, otevřete **Konzola správce balíčků** (**nástroje**  - &gt; **Manager balíček NuGet**  - &gt; **Konzola správce balíčků**) a zadejte následující příkaz:  
    `Install-Package SendGrid`
2. Přejděte na [stránku pro přihlášení sendgrid vám umožňuje Azure](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) a ZDARMA zaregistrovat účet sendgrid vám umožňuje. Můžete také zaregistrovat bezplatný účet sendgrid vám umožňuje přímo na [sendgrid vám umožňuje na webu](http://www.sendgrid.com).
3. Z **Průzkumníku řešení** otevřete *IdentityConfig.cs* v soubor *aplikace\_spustit* složky a přidejte následující kód zvýrazněných v žlutý k `EmailService` třída ke konfiguraci **sendgrid vám umožňuje**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Také, přidejte následující `using` příkazy na začátek *IdentityConfig.cs* souboru: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Pro zjednodušení této ukázku, budete ukládat hodnoty účet služby e-mailu v `appSettings` části *web.config* souboru. Přidejte následující kód XML zvýrazněných v žlutý k *web.config* soubor v kořenovém adresáři projektu:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Zabezpečení – nikdy úložiště citlivá data ve zdrojovém kódu. V tomto příkladu účet a přihlašovací údaje uložené v **appSetting** části *Web.config* souboru. V Azure, můžete bezpečně uložit tyto hodnoty na **[konfigurace](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** na portálu Azure. Související informace najdete v tématu Rick Anderson s názvem [osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a do Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Přidejte hodnoty e-mailové služby, aby odrážela, že hodnoty sendgrid vám umožňuje ověřování (uživatelské jméno a heslo), aby bylo možné úspěšné odeslání e-mailu z vaší aplikace. Nezapomeňte použít název účtu sendgrid vám umožňuje spíše než e-mailovou adresu, které jste zadali sendgrid vám umožňuje.

### <a name="enable-email-confirmation"></a>Povolit potvrzení e-mailu

 Pokud chcete povolit potvrzení e-mailu, změníte kód registrace pomocí následujících kroků.  
 

1. V *účet* složku, otevřete *Register.aspx.cs* kódu a aktualizovat `CreateUser_Click` způsob povolení následující zvýrazněný změny: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem na *Default.aspx* a vyberte **nastavit jako úvodní stránku**.
3. Spusťte aplikaci stisknutím **F5.** Po zobrazení stránky, klikněte na tlačítko **zaregistrovat** odkaz na stránce registrace.
4. Zadejte váš e-mail a heslo a potom klikněte **zaregistrovat** tlačítko Odeslat e-mailové zprávy prostřednictvím sendgrid vám umožňuje.  
   Aktuální stav projektu a kód vám umožní uživatelům přihlášení po jejich dokončení registrace, i když se nepotvrdily svého účtu.
5. Zkontrolujte e-mailový účet a klikněte na odkaz k potvrzení e-mailu.  
   Po odeslání formuláře registrace budete přihlášeni.  
    ![Ukázkový web - přihlášení](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Požadovat potvrzení e-mailu před přihlášení

I když se ujistíte, e-mailový účet, v tomto okamžiku nebude muset klikněte na odkaz obsažený v e-mailu ověření být plně přihlášeného. V následující části upravíte kód nutnosti nové uživatele tak, aby měl potvrzen e-mailu, než se přihlásí (ověřený).

1. V **Průzkumníku řešení** sady Visual Studio, aktualizovat `CreateUser_Click` událost v *Register.aspx.cs* obsažené v kódu *účty* složku s následující zvýrazněný změny: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Aktualizace `LogIn` metoda v *Login.aspx.cs* kódu s následující zvýrazněný změny: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Spuštění aplikace

 Teď, když jste implementovali kód zkontrolujte, zda byl potvrzen e-mailovou adresu uživatele, můžete zkontrolovat funkčnost na obou **zaregistrovat** a **přihlášení** stránky. 

1. Odstranit všechny účty v **AspNetUsers** tabulku, která obsahují e-mailový alias, který chcete otestovat.
2. Spuštění aplikace (**F5**) a ověřte nelze zaregistrovat jako uživatel, dokud se ujistíte, e-mailovou adresu.
3. Před potvrzením váš nový účet prostřednictvím e-mailu, který byl právě zaslali, pokuste se přihlásit pomocí nového účtu.  
 Dozvíte se, že nebudete moci přihlásit a, musíte mít potvrzené e-mailový účet.
4. Jakmile potvrdíte, e-mailovou adresu, přihlaste se k aplikaci.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Obnovení hesla a resetování

1. V sadě Visual Studio, odeberte komentář znaky z `Forgot` metoda v *Forgot.aspx.cs* obsažené v kódu *účtu* složky tak, že metoda zobrazí jako následuje: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Otevřete *Login.aspx* stránky. Nahraďte kód u konce **loginForm** části jak je znázorněno níže: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Otevřete *Login.aspx.cs* kódu a zrušte komentář u následujícího řádku kódu zvýrazněných v žlutý z `Page_Load` obslužné rutiny události: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Spusťte aplikaci stisknutím **F5.** Po zobrazení stránky, klikněte na tlačítko **přihlásit** odkaz.
5. Klikněte na tlačítko **zapomněli jste heslo?** odkaz na zobrazení **zapomněli jste heslo** stránky.
6. Zadejte e-mailovou adresu a klikněte na **odeslání** tlačítko Odeslat e-mail na vaši adresu, která vám umožní vytvořit nové heslo.   
   Zkontrolujte e-mailový účet a klikněte na odkaz zobrazíte **resetovat heslo** stránky.
7. Na **resetovat heslo** zadejte vaše e-mailu, heslo a potvrzení hesla. Potom stiskněte **resetovat** tlačítko.  
   Pokud jste úspěšně resetovali heslo, **změnit heslo** se zobrazí stránka. Nyní můžete přihlásit pomocí nového hesla.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Odkaz pro potvrzení znovu odeslat e-mailu

Jakmile uživatel vytvoří nový místní účet, jsou e-mailem potvrzení propojení, které musí předtím, než se nemohou přihlásit Pokud uživatel, ať už náhodně odstraní e-mail s potvrzením nebo e-mailu nikdy dorazí, potřebují odkaz potvrzení odeslat znovu. Následující změny kódu ukazují, jak povolit tuto funkci.

1. V sadě Visual Studio, otevřete **Login.aspx.cs** kódu a přidejte následující obslužné rutiny události po `LogIn` obslužné rutiny události:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Změnit `LogIn` obslužné rutiny událostí v *Login.aspx.cs* kódu tak, že změníte kód zvýrazněných v žlutý následujícím způsobem: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Aktualizace *Login.aspx* tak, že přidáte kód zvýrazněných v žlutý následujícím způsobem: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Odstranit všechny účty v **AspNetUsers** tabulku, která obsahují e-mailový alias, který chcete otestovat.
5. Spuštění aplikace (**F5**) a zaregistrujte e-mailovou adresu.
6. Před potvrzením váš nový účet prostřednictvím e-mailu, který byl právě zaslali, pokuste se přihlásit pomocí nového účtu.  
   Dozvíte se, že nebudete moci přihlásit a, musíte mít potvrzené e-mailový účet. Kromě toho můžete potvrzovací zpráva nyní znovu odeslat ke svému účtu e-mailu.
7. Zadejte e-mailovou adresu a heslo, stiskněte **znovu odeslat potvrzení** tlačítko.
8. Jakmile potvrdíte, e-mailovou adresu, na základě nově odeslaných e-mailové zprávy, přihlaste se k aplikaci.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Řešení potíží s aplikací

Pokud jste neobdrželi e-mail, který obsahuje odkaz na ověřit vaše přihlašovací údaje:

- Zkontrolujte složku Nevyžádaná pošta nebo nevyžádané pošty.
- Přihlaste se k účtu sendgrid vám umožňuje a klikněte na [odkaz e-mailu aktivity](https://sendgrid.com/logs/index).
- Být jistý, použít název účtu uživatele sendgrid vám umožňuje jako *Web.config* hodnotu namísto e-mailovou adresu účtu sendgrid vám umožňuje.

<a id="addRes"></a>
## <a name="additional-resources"></a>Další prostředky

- [Odkazy na identitě ASP.NET Identity doporučené prostředky](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Potvrzení účtu a heslo pro obnovení s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Kurz řady webových formulářů ASP.NET – přidání poskytovatele OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Nasazení aplikace zabezpečení rozhraní ASP.NET Web Forms členství, OAuth a databáze SQL Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET řady kurz webové formuláře – povolit šifrování SSL pro projekt](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
